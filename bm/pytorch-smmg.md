# Pytorch 单机多卡测试
参考：[🔗](https://github.com/fusimeng/framework_benchmark)
## 一、准备数据和代码
### 1、下载
我们已将数据和代码存储到我公司的FTP服务器上，单击[此处](http://123.151.118.124:13390/sitonholy/scm/pytorch-smsg.tar.gz)下载，并将其上传到服务器上。    
### 2、上传数据
依次单击`系统管理->文件管理->文档上传`将文件（主要，文件应该是解压后的）上传到服务器。   
![](../imgs/bm-01.png)    
## 二、提交作业
JSON文件如下：    
```
{
  "jobName": "pytorch-smmg_16bea75b",
  "image": "sitonholy/ai:16.04-9.0-3.5-pmt",
  "authFile": "",
  "dataDir": "",
  "outputDir": "",
  "codeDir": "",
  "retryCount": 0,
  "taskRoles": [
    {
      "name": "task_1",
      "taskNumber": 1,
      "cpuNumber": 1,
      "memoryMB": 16384,
      "shmMB": 2048,
      "gpuNumber": 2,
      "storageGB": 5,
      "minFailedTaskCount": null,
      "minSucceededTaskCount": null,
      "command": "cd /root/data/framework_benchmark/pytorch && python smmg.py --lr  0.001 --epoch 100 --trainBatchSize 5000 --gpunum 2",
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
![](../imgs/bm-03.png)   
### 2、资源监控
单击图1中`前往工作指标页面`查看资源利用情况：   
![](../imgs/bm-05.png)    
