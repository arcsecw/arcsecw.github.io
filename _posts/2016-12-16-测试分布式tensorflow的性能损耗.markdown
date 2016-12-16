##测试 tensorflow以及tensorflow-gpu 以及分布式tensorflow的性能损耗

> 测试机器1 macbook_pro_2013_中 + i7 + 8g + gt650m （cuda计算能力3.0）

> 测试机器2 windows8.1 + i7 + 8g + gt965m （cuda计算能力5.2）

> 测试机器3 windows_server 2012 xeon_e5_2620 无gpu

> 网络环境 无线局域网

#### 测试程序
 测试程序为tensorlayer官方教程中的[tutorial_mnist.py](https://github.com/zsdonghao/tensorlayer/blob/master/example/tutorial_mnist.py)
下面只列出来改动的2行
```
51  sess = tf.Session() 
    
562 sess = tf.Session() 

106 n_epoch = 20

115 #network.print_params()
116 #network.print_layers()
```


#### 分布式tensorfolw worker端代码

```
import tensorflow as tf

worker_hosts = ["localhost:2222"]

cluster_spec = tf.train.ClusterSpec({ "worker": worker_hosts})

server = tf.train.Server( cluster_spec , job_name="worker", task_index=0)

server.join()

```


#### 附-每台机器详细速度
- 测试机器1 gpu运行

```
Epoch 1 of 20 took 5.936037s
   train loss: 0.429919
   val loss: 0.380297
   val acc: 0.880500
Epoch 5 of 20 took 5.001142s
   train loss: 0.229022
   val loss: 0.208304
   val acc: 0.943000
Epoch 10 of 20 took 4.878017s
   train loss: 0.161666
   val loss: 0.155864
   val acc: 0.958500
Epoch 15 of 20 took 4.982393s
   train loss: 0.119368
   val loss: 0.120819
   val acc: 0.967900
Epoch 20 of 20 took 4.991056s
   train loss: 0.094131
   val loss: 0.104829
   val acc: 0.970700
Evaluation
   test loss: 0.107837
   test acc: 0.970100
```
- 测试机器1 cpu运行

```
Epoch 1 of 20 took 8.432808s
   train loss: 0.434308
   val loss: 0.388408
   val acc: 0.878400
Epoch 5 of 20 took 8.401819s
   train loss: 0.240203
   val loss: 0.220802
   val acc: 0.937100
Epoch 10 of 20 took 8.459795s
   train loss: 0.162507
   val loss: 0.157550
   val acc: 0.956600
Epoch 15 of 20 took 8.430325s
   train loss: 0.120312
   val loss: 0.123675
   val acc: 0.965500
Epoch 20 of 20 took 8.173468s
   train loss: 0.094554
   val loss: 0.105695
   val acc: 0.969800
Evaluation
   test loss: 0.109030
   test acc: 0.969200
```

- 测试机器2 cpu运行

```
Epoch 1 of 20 took 8.147750s
   train loss: 0.437216
   val loss: 0.392240
   val acc: 0.881300
Epoch 5 of 20 took 8.346888s
   train loss: 0.236115
   val loss: 0.218401
   val acc: 0.939100
Epoch 10 of 20 took 8.420944s
   train loss: 0.160565
   val loss: 0.156358
   val acc: 0.957000
Epoch 15 of 20 took 8.470981s
   train loss: 0.120155
   val loss: 0.125624
   val acc: 0.966000
Epoch 20 of 20 took 8.516008s
   train loss: 0.093928
   val loss: 0.106246
   val acc: 0.970500
Evaluation
   test loss: 0.105454
   test acc: 0.970600
```
- 测试机器2 gpu 运行

```
Epoch 1 of 20 took 2.093471s
   train loss: 0.446878
   val loss: 0.403538
   val acc: 0.872200
Epoch 5 of 20 took 1.876324s
   train loss: 0.244723
   val loss: 0.230331
   val acc: 0.935500
Epoch 10 of 20 took 1.886331s
   train loss: 0.168042
   val loss: 0.166904
   val acc: 0.953800
Epoch 15 of 20 took 1.883332s
   train loss: 0.122522
   val loss: 0.131608
   val acc: 0.964500
Epoch 20 of 20 took 1.887332s
   train loss: 0.095834
   val loss: 0.110439
   val acc: 0.969700
Evaluation
   test loss: 0.107753
   test acc: 0.969300
```
- 测试机器3 cpu运行

```
Epoch 1 of 20 took 16.738041s
   train loss: 0.444871
   val loss: 0.394343
   val acc: 0.878000
Epoch 5 of 20 took 14.664197s
   train loss: 0.230461
   val loss: 0.212415
   val acc: 0.937400
Epoch 10 of 20 took 15.127311s
   train loss: 0.159706
   val loss: 0.155066
   val acc: 0.956500
Epoch 15 of 20 took 15.229579s
   train loss: 0.121513
   val loss: 0.127355
   val acc: 0.964100
Epoch 20 of 20 took 15.171452s
   train loss: 0.095451
   val loss: 0.108837
   val acc: 0.969200
Evaluation
   test loss: 0.109069
   test acc: 0.968500
```
