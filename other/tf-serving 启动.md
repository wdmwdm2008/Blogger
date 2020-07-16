# Method 1
### Download the TensorFlow Serving Docker image and repo
docker pull tensorflow/serving

git clone https://github.com/tensorflow/serving
### Location of demo models
TESTDATA="$(pwd)/serving/tensorflow_serving/servables/tensorflow/testdata"

### Start TensorFlow Serving container and open the REST API port
docker run -t --rm -p 8501:8501 \
    -v "$TESTDATA/saved_model_half_plus_two_cpu:/models/half_plus_two" \
    -e MODEL_NAME=half_plus_two \
    tensorflow/serving &

### Query the model using the predict API
curl -d '{"instances": [1.0, 2.0, 5.0]}' \
    -X POST http://localhost:8501/v1/models/half_plus_two:predict

### Returns => { "predictions": [2.5, 3.0, 4.5] }

# Method 2
docker run --runtime=nvidia -p 8501:8501 \
--mount type=bind,\
source=/tmp/tfserving/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_gpu,\
target=/models/half_plus_two \
  -e MODEL_NAME=half_plus_two -t tensorflow/serving:latest-gpu &

curl -d '{"instances": [1.0, 2.0, 5.0]}' \
  -X POST http://localhost:8501/v1/models/half_plus_two:predict


# tensorflow tfserving 部署多个模型、使用不同版本的模型
### 在multiModel文件夹下新建一个配置文件model.config，文件内容为：
model_config_list:{
    config:{
      name:"model1",
      base_path:"/models/multiModel/model1",
      model_platform:"tensorflow"
    },
    config:{
      name:"model2",
      base_path:"/models/multiModel/model2",
      model_platform:"tensorflow"
    },
    config:{
      name:"model3",
      base_path:"/models/multiModel/model3",
      model_platform:"tensorflow"
    } 
}

### 配置文件定义了模型的名称和模型在容器内的路径，现在运行tfserving容器 :
docker run -p 8501:8501 --mount type=bind,source=/home/jerry/tmp/multiModel/,target=/models/multiModel \
 -t tensorflow/serving --model_config_file=/models/multiModel/models.config

### 请求预测：import requests 
import numpy as np 
SERVER_URL = 'http://localhost:8501/v1/models/model3:predict'  
#注意SERVER_URL中的‘model3’是config文件中定义的模型name,不是文件夹名称

def prediction(): 
    predict_request='{"instances":%s}' % str([[[10]*7]*7]) 
    print(predict_request) 
    response = requests.post(SERVER_URL, data=predict_request) 
    print(response)
    prediction = response.json()['predictions'][0] 
    print(prediction) 

if __name__ == "__main__": 
    prediction()

#指定模型版本
如果一个模型有多个版本，并在预测的时候希望指定模型的版本，可以通过以下方式实现。
修改model.config文件，增加model_version_policy：
model_config_list:{
    config:{
      name:"model1",
      base_path:"/models/multiModel/model1",
      model_platform:"tensorflow",
      model_version_policy:{
        all:{}
      }
    },
    config:{
      name:"model2",
      base_path:"/models/multiModel/model2",
      model_platform:"tensorflow"
    },
    config:{
      name:"model3",
      base_path:"/models/multiModel/model3",
      model_platform:"tensorflow"
    } 
}

### 请求预测的时候，如果要使用版本为100001的模型，只要修改SERVER_URL为：
SERVER_URL = 'http://localhost:8501/v1/models/model1/versions/100001:predict' 

tfserving支持模型的Hot Plug，上述容器运行起来之后，如果在宿主机的 /home/jerry/tmp/multiModel/model1/ 文件夹下新增模型文件如100003/，tfserving会自动加载新模型；同样如果移除现有模型，tfserving也会自动卸载模型。
# Restful方法调用模型
predict_inputs = []
for feature in web_test_features:
    predict_input = {
        "input_ids": feature.input_ids,
        "input_mask": feature.input_mask,
        "segment_ids": feature.segment_ids,
        "label_ids": feature.label_id
    }
    predict_inputs.append(predict_input)
data = json.dumps({"signature_name": "serving_default", "instances": predict_inputs})
headers = {"content-type": "application/json"}
prediction = requests.post(model_predict_fn + ':predict', data=data, headers=headers)
predContents = json.loads(prediction.text)['predictions']

# gRPC方法调用模型(pip install tensorflow-serving-api)

from tensorflow_serving.apis import predict_pb2
from tensorflow_serving.apis import prediction_service_pb2_grpc
import grpc
tf.app.flags.DEFINE_string('server', '127.0.0.1:8502', 'PredictionService host:port')
FLAGS = tf.app.flags.FLAGS

def prediction():
    images = image.load_img("test.jpg", target_size=(480, 480))
    x = image.img_to_array(images)
    x = np.expand_dims(x, axis=0)
    image_np = xception.preprocess_input(x)

    options = [('grpc.max_send_message_length', 1000 * 1024 * 1024), ('grpc.max_receive_message_length', 1000 * 1024 * 1024)]
    channel = grpc.insecure_channel(FLAGS.server, options = options)
    stub = prediction_service_pb2_grpc.PredictionServiceStub(channel)
    request = predict_pb2.PredictRequest()
    request.model_spec.name = 'xception' #对应上图第一个方框
    request.model_spec.signature_name = 'serving_default' #对应上图第二个方框
    request.inputs['in'].CopyFrom(tf.make_tensor_proto(image_np)) #in对应上图第三个方框，为模型的输入Name

    result_future = stub.Predict.future(request, 10.0)  # 10 secs timeout
    result = result_future.result()
    print result

if __name__ == "__main__":
    prediction()
# Question
1. Error parsing text-format tensorflow.serving.ModelServerConfig: String literals cannot cross line boundaries.
解决方法：重新写一遍，可能是逗号的问题

# Reference
https://blog.csdn.net/JerryZhang__/article/details/86516428   多模型tf-serving  
https://blog.csdn.net/u012433049/article/details/89354361  多模型tf-serving  
https://www.jianshu.com/p/bd67b40e6b85      制作tf-serving镜像  
https://blog.csdn.net/ljp1919/article/details/101100939  模型部署  
https://blog.csdn.net/zong596568821xp/article/details/101421337
https://zhuanlan.zhihu.com/p/110727286
