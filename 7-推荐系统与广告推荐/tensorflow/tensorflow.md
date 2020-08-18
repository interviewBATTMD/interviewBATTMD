# Tensorflow
## tensorflow C++服务开发指南
https://zhuanlan.zhihu.com/p/59174096  
python版本的tensorflow是被应用最多的。但是python的执行效率偏低。有很多公司后台是用C++编写的，为了更好的将深度模型应用到线上，通常需要进行模型在线inference。  

1、tf训练模型  
模型训练逻辑代码依赖具体任务实现，可以直接用python接口实现。  

2、tf保存模型  
TensorFlow保存模型时分为两部分，网络结构和参数是分开保存的。  

保存网络结构  
调用write_graph()接口保存网络结构到graph.pb  
```
tf.train.write_graph(sess.graph.as_graph_def(), FLAGS.model_dir, 'graph.pb', as_text=False)
```

保存模型参数  
调用Saver.save()接口保存模型参数，FLAGS.model_dir目录下保存多个前缀为model.checkpoint的文件。其中，model.checkpoint.meta包含了网络结构和一些其他信息，所以也包含了上面提到的graph.pb；model.checkpoint.data-00000-of-00001保存了模型参数，其他两个文件辅助作用。  
```
saver = tf.train.Saver()
saver.save(sess, FLAGS.model_dir + "/model.checkpoint")
```

合并网络结构和模型参数  
如果分别保存了网络结构和模型参数，在进行C++ api开发时需要分别导入网络结构和模型参数。使用多个文件部署比较麻烦，如果能整个成一个独立文件会方便很多。  
TensorFlow官方提供了freeze_graph.py工具。如果已经安装了TensorFlow，则在安装目录下可以找到，否则可以直接使用源码tensorflow/python/tools路径下freeze_graph.py。运行例子为：
```
python ${TF_HOME}/tensorflow/python/tools/freeze_graph.py \
    --input_graph="graph.pb" \
    --input_checkpoint="your_checkpoint_path/checkpoint_prefix" \
    --output_graph="your_checkpoint_path/freeze_graph.pb" \
    --output_node_names=Softmax
```
input_graph为网络结构pb文件，input_checkpoint为模型参数文件名前缀，output_graph为我们的目标文件，output_node_names为目标网络节点名称，因为网络包括前向和后向网络，在预测时后向网络其实是多余的，指定output_node_names后只保存从输入节点到这个节点的部分网络。  

得到freeze_graph.pb后，只导入网络结构即可，不再需要另外导入模型参数。  

3、tf C++本地化接口开发  
预测代码主要包括以下几个步骤：

* 创建Session
* 导入之前生成的模型
* 将模型设置到创建的Session里
* 设置模型输入输出，调用Session的Run做预测
* 关闭Session

创建Session  
```
Session* session;
Status status = NewSession(SessionOptions(), &session);
if (!status.ok()) {
  std::cout << status.ToString() << std::endl;
} else {
  std::cout << "Session created successfully" << std::endl;
}
```

导入模型  
```
GraphDef graph_def;
//读取Graph, 如果是文本形式的pb,使用ReadTextProto
Status status = ReadBinaryProto(Env::Default(), "../demo/simple_model/graph.pb", &graph_def);
if (!status.ok()) {
  std::cout << status.ToString() << std::endl;
} else {
  std::cout << "Load graph protobuf successfully" << std::endl;
}
```

将模型设置到创建的Session里  
```
Status status = session->Create(graph_def);
if (!status.ok()) {
  std::cout << status.ToString() << std::endl;
} else {
  std::cout << "Add graph to session successfully" << std::endl;
}
```

模型的输入输出都是Tensor或Sparse Tensor。  
```
Tensor a(DT_FLOAT, TensorShape()); // input a
a.scalar<float>()() = 3.0;

Tensor b(DT_FLOAT, TensorShape()); // input b
b.scalar<float>()() = 2.0;
```

预测  
```
std::vector<std::pair<string, tensorflow::Tensor>> inputs = {
    {"a", a},
    {"b", b},
};
std::vector<tensorflow::Tensor> outputs;

std::vector<std::pair<string, tensorflow::Tensor>> inputs = {
  { "a", a },
  { "b", b },
}; // input

std::vector<tensorflow::Tensor> outputs; // output

Statuc status = session->Run(inputs, {"c"}, {}, &outputs);
if (!status.ok()) {
  std::cout << status.ToString() << std::endl;
} else {
  std::cout << "Run session successfully" << std::endl;
}
```

关闭Session  
```
session->Close();
```

4、tf C++本地化编译  
》1-修改protobuf版本  
由于tensorflow的接口需要使用protobuf3.0+，所以编译需要修改下protobuf的版本。  

5、tf运行时  
在导入模型时，ReadBinaryProto() 接口对导入模型的大小有限制，最大不超过1G。 如遇到此问题，可以将保存的模型保存问文本形式，利用ReadTextProto()进行加载模型可解决。  
初次运行会有lazy loading 问题， 可以通过启动后warm up请求，解决。  