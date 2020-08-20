### 写入日志

使用tf.summary.merge_all()定义运算  
merged_summary_op = tf.summary.merge_all()  

#使用tf.summary.FileWriter(path, graph)将日志写入到文件中  
#path:日志文件保存路径， graph:需要保存的图  
summary_writer = tf.summary.FileWriter(logs_path, graph=tf.get_default_graph())  

#每次训练时执行运算与写入函数  
with tf.Session() as sess:  
    summary = sess.run(merged_summary_op, feed_dict)  #执行运算  
    summary_writer.add_summary(summary, curr_step)    #写入文件  
    
### 使用tensorboard 
tensorboard --logdir ./log --port 5000 --host 127.0.0.1
