# Tensor 单机单卡测试
参考：[🔗](https://github.com/fusimeng/framework_benchmark)
## 一、准备数据和代码
### 1、下载
我们已将数据和代码存储到我公司的FTP服务器上，单击[此处](http://123.151.118.124:13390/sitonholy/scm/lenet.tar.gz)下载，并将其上传到服务器上。    
### 2、上传数据
依次单击`系统管理->文件管理->文档上传`将文件（主要，文件应该是解压后的）上传到服务器。   
![](../imgs/bm-01.png)    
## 二、提交作业
JSON文件如下：    
```
{
  "jobName": "tensorflow-smsg",
  "image": "sitonholy/scm:16.04-10.0-7.4-3.6-4.1.0-pmt",
  "authFile": "",
  "dataDir": "",
  "outputDir": "",
  "codeDir": "",
  "retryCount": 0,
  "taskRoles": [
    {
      "name": "task1",
      "taskNumber": 1,
      "cpuNumber": 4,
      "memoryMB": 8192,
      "shmMB": 2048,
      "gpuNumber": 1,
      "storageGB": 5,
      "minFailedTaskCount": 1,
      "minSucceededTaskCount": null,
      "command": "cd /root/data/lenet/code && python mnist_lenet_tf.py",
      "portList": []
    }
  ],
  "jobEnvs": {},
  "extras": {
    "virtualGroup": "total"
  },
  "gpuType": "TITANX"
}
```

## 三、日志查看
![](../imgs/bm-02.png)  
*图1*    
### 1、查看日志
单击图1中`标准输出`查看相应日志如下：   
![](../imgs/bm-08.png)   
### 2、资源监控
单击图1中`前往工作指标页面`查看资源利用情况：   
![](../imgs/bm-09.png)    
