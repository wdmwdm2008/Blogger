### spark client方式启动
#!/bin/bash

export HDP_VERSION=3.1.4.0-315
export SPARK_YARN_USER_ENV=PYTHONHASHSEED=0
export PYSPARK_PYTHON=./ANACONDA/bin/python
export PYSPARK_DRIVER_PYTHON=/home/data/fran.yang/env_python/py36/bin/python
#export PYSPARK_DRIVER_PYTHON=/home/data/fran.yang/env_python/py36/bin/python
# export PYTHONHASHSEED=0


#dict_path="/home/data/fran.yang/RecallClassifier/dict/"
#files=$(ls $dict_path);
#files=${files// / };
#file_arr=($files);
#files_str=""
#for ele in ${file_arr[*]}
#do
#  file_str=${file_str}${dict_path}${ele},
#done
#len=`expr ${#file_str} - 1`
#file_str=`expr substr "$file_str" 1 $len`
#echo $file_str
spark-submit --master yarn \
       --queue maps_bi \
       --deploy-mode client\
       --executor-cores 4\
       --num-executors 200 \
       --executor-memory 8g \
       --driver-memory 8g \
       --conf spark.port.maxRetries=100 \
       --conf spark.driver.maxResultSize=10G\
       --conf spark.speculation=true\
       --conf spark.speculation.quantile=0.9 \
       --conf spark.speculation.multiplier=1.5 \
       --conf spark.sql.hive.convertMetastoreParquet=false \
       --archives /home/data/fran.yang/env_python/py36.tar/#ANACONDA \
       --conf spark.yarn.appMasterEnv.PYSPARK_PYTHON=./ANACONDA/bin/python \
       --files /home/data/fran.yang/ods_torrent/no_res_query/无结果线上query_0424.xlsx \
       poi_validity.py


### spark cluster方式启动
#!/bin/bash
spark-submit --master yarn \
       --deploy-mode cluster \
       --queue maps_bi \
       --driver-memory 8g \
       --num-executors 100 \
       --executor-cores 4\
       --executor-memory 12g \
       --conf 'spark.speculation=true' \
       --conf 'spark.speculation.quantile=0.9' \
       --conf 'spark.speculation.multiplier=1.5' \
       --conf spark.sql.hive.convertMetastoreParquet=false \
       --archives hdfs:///user/fran.yang/py3.tar#py3 \
       --conf spark.yarn.appMasterEnv.PYSPARK_PYTHON=./py3/bin/python \
       --conf spark.yarn.appMasterEnv.PYSPARK_DRIVER_PYTHON=./py3/bin/python \
       --conf spark.yarn.appMasterEnv.LTP_MODEL_DIR=./py3/lib/python3.6/site-packages \
       --py-files /home/data/dongming.wang/house_alloc/classifier.py,/home/data/dongming.wang/house_alloc/valid/proprecess.py \
       /home/data/poi_validity_copy_bamboo_order.py
       #--py-files ./recall_attempt/code/CleanQuery.py,./sug_display/order/poi_order_display_torrent.py\
       #search_table_recall_eva.py
       #sample.py
       #--py-files CleanQuery.py \
       #sample.py
       #--files /home/data/dongming.wang/intentRec/data/hll_ethang_dict_edited.txt \
       #sample.py
