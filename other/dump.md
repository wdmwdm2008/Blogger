### 1. json.load()

import json  
  
emb_filename = ('/home/cqh/faceData/emb_json.json')  
  
jsObj = json.load(open(emb_filename))   
  
print(jsObj) 
print(type(jsObj)) 
  
for key in jsObj.keys(): 
  print('key: %s  value: %s' % (key,jsObj.get(key)))
  
### 2. json.dump()
import json  
   
name_emb = {'a':'1111','b':'2222','c':'3333','d':'4444'}  
       
emb_filename = ('/home/cqh/faceData/emb_json.json')  
  
solution 1 
jsObj = json.dumps(name_emb)   
with open(emb_filename, "w") as f:  
  f.write(jsObj)  
  f.close()  
    
solution 2   
json.dump(name_emb, open(emb_filename, "w"))


### 3. json.loads()

json.loads()用于将str类型的数据转成dict。

### 4. json.dumps()

 json.dumps()用于将dict类型的数据转成str，因为如果直接将dict类型的数据写入json文件中会发生报错，因此在将数据写入时需要用到该函数。
