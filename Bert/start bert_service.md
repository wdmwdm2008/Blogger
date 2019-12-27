## bert service

- start the bert service  
  bert-serving-start -model_dir /chinese_L-12_H-768_A-12/ -num_worker=4 

- Use Client to Get Sentence Encodes
``` Python
from bert_serving.client import BertClient
bc = BertClient()
bc.encode(['First do it', 'then do it right', 'then do it better'])
```

## Reference
https://github.com/hanxiao/bert-as-service (原文及代码)
