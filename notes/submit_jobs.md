# 提交作业
系统允许用户上传自定义的代码程序和数据文件，通过JSON格式提交作业，由调度器分配资源并按调度规则启动作业。系统支持单机多GPU和多机多GPU的训练任务。    
### 提交作业功能
* 提交作业    
使用JSON格式提交作业。   
* 连接服务   
    * （1）使用Jupyter
    * （2）使用Pycharm
    * （3）使用SSH
    * （4）使用Visual studio （TODO）
    * （5）使用Visual Studio Code（TODO）

其中，使用JSON格式提交作业是最基本，最底层的提交作业方式，SCM接收的是JSON格式文件，但是，启动服务后可以以多种方式访问服务。

**选择建议**    
* 使用JSON启动持久性服务，然后使用（1）、（2）、（3）连接服务的这种方式比较适合提交单机单卡/单机多卡作业。  
* 使用JSON直接提交作业比较适合提交分布式作业。
## 第1节、概念介绍
在提交第一个作业之前，先让我们了解些关于系统的基本概念。    
![](../imgs/job_relation.png)    
*图1-1 cluster、node、job、task-role、task之间的关系*     

如上图1-1所示，在硬件层次上，集群（cluster）由节点（node）组成，通过docker技术可将一台node虚拟出多台主机。在提交任务的层次上，作业（job）可以划分为多个task-role，每个task-role可以有多个相同配置的task，每个task是独立的docker容器，运行job分配的任务。    
### 1.单机任务
当我提交个单机单卡/单击多卡的作业时，scm启动个docker容器，并将CPU，内存和一块GPU等资源分配给docker容器。然后，我们就可以使用这台轻量级的虚拟机跑任务。   

如图1-1，在集群中某一台节点上提交作业（job-1），job-1只有一种任务角色（task-role），且此task-role包含两个任务（work），两个work运行在同一台主机的两块显卡上。

### 2.分布式任务
当我们提交个多机多卡的任务时，scm启动多个docker容器（每个容器相当于一个task），这些容器可在同一台主机或在不同主机上，并将CPU，内存和一块GPU等资源分配给每个docker容器。然后，我们就可以使用这些轻量级的虚拟机跑任务。   

如图1-1，在集群中某一台节点上提交作业（job-2），job-2有main、work两种任务角色（task-role），共有三个任务（work），三个work运行在两台主机的三块显卡上。  
## 第2节、提交作业（JSON）
### 1、提交方式
![](../imgs/submit-01.png)   
*图1-1 提交作业*   
如图1-1，单击`1` 即可进入提交作业界面，SCM是通过JSON文件来提交作业的，如图所示，一共有三种方式：   
* 方式一：导入JSON文件
* 方式二：复制JSON文字
* 方式三：填写表单
### 2、提交HelloWorld作业（填写表单方式） 
**（1） 准备数据和代码：**   
代码和数据存放在github上，链接为：https://github.com/sitonholy/lenet  ，下载代码后，上传到服务器即可， 如图：   
![](../imgs/submit-00.png)   
*图1 上传数据和代码*    

**（2） 填写基本信息：**       
![](../imgs/submit-02.png)   
*图1-2 填写表单*    

**（3） 选择集群和显卡等资源：**   
![](../imgs/submit-03.png)   
*图1-3 填写表单*    

**（4） 设置工作环境和额外的参数等信息：**    
![](../imgs/submit-04.png)  
*图1-4 填写表单*   
由于此例子不需要额外的参数，所以这里略过。   

**（5） 设置任务角色、任务等：**   
![](../imgs/submit-05.png)   
*图1-5 填写表单*    
然后，单击`提交`按钮，将作业提交给后台运行。   
**（6） 查看所提交的作业：**   
![](../imgs/submit-06.png)   
*图1-6 查看作业*    
如图1-6，单击`作业管理`可以查看作业运行状况，如何具体查看作业运行状况，请参考[作业管理](manage_jobs.md).
### 3、提交HelloWorld作业（复制JSON文字方式）
![](../imgs/submit-07.png)   
*图1-6 提交作业*    
如图1-6，单击`JSON`，然后见JSON文件复制到文本框内，然后单击`提交`按钮，即可提交作业。   
JSON文件内容：    
```
{
  "jobName": "helloworld_16be675e",
  "image": "sitonholy/tensorflow:16.04-9.0-3.5-1.8.0",
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
      "memoryMB": 8192,
      "shmMB": 64,
      "gpuNumber": 1,
      "storageGB": 5,
      "minFailedTaskCount": null,
      "minSucceededTaskCount": null,
      "command": "nvidia-smi",
      "portList": []
    }
  ],
  "jobEnvs": {},
  "extras": {},
  "gpuType": "TITANX",
  "tmp": {
    "vgInfo": {
      "名称": "test",
      "正在跑工作数目": "0",
      "使用中內存/全部內存": "0/100000",
      "使用中硬盘/全部硬盘": "0/100",
      "使用中中央处理器/全部中中央处理器": "0/32",
      "显示卡(使用中/全部)": {
        "TITANX": "0/2",
        "P100": "0/2"
      }
    }
  }
}
```
## 第3节、连接服务
### 1、使用Jupyter连接服务
#### （1）JSON提交作业
```
{
  "jobName": "jupyter_16be8b71",
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
      "memoryMB": 8192,
      "shmMB": 64,
      "gpuNumber": 1,
      "storageGB": 5,
      "minFailedTaskCount": null,
      "minSucceededTaskCount": null,
      "command": "jupyter notebook --allow-root --no-browser --ip 0.0.0.0 --port=$PAI_CONTAINER_HOST_jupyter_PORT_LIST --NotebookApp.token=\\\"\\\" --NotebookApp.allow_origin=\\\"*\\\" --NotebookApp.base_url=\\\"/\\\"",
      "portList": [
        {
          "label": "jupyter",
          "beginAt": 0,
          "portNumber": 1
        }
      ]
    }
  ],
  "jobEnvs": {},
  "extras": {},
  "gpuType": "TITANX",
  "tmp": {
    "vgInfo": {
      "名称": "test",
      "正在跑工作数目": "1",
      "使用中內存/全部內存": "8192/100000",
      "使用中硬盘/全部硬盘": "5/100",
      "使用中中央处理器/全部中中央处理器": "1/32",
      "显示卡(使用中/全部)": {
        "TITANX": "1/2",
        "P100": "0/2"
      }
    }
  }
}
```
#### （2）Jupyter连接服务
![](../imgs/submit-08.png)    
如上图，单击`作业管理->相应的作业名（也就是2）`。    
![](../imgs/submit-09.png)    
如上图，找到作业对应的`IP和端口号`。 在浏览器地址栏，输入IP:port,本例中为192.168.30.14:29640,即可使用Jupyter服务。    
![](../imgs/submit-010.png)    
### 2、使用SSH连接服务
#### （1）JSON提交作业
JSON文件参考第3节中的第1段。    
#### （2）连接SSH服务
![](../imgs/submit-11.png)   
如上图，单击`作业管理->相应的作业名（也就是2）`。    
![](../imgs/submit-12.png)   
如上图，单击`1`可以打开web ssh；单击`2`可以获取连接方式，从xshell等软件连接到服务。    
### 3、使用Pycharm连接服务
#### （1）JSON提交作业
JSON文件参考第3节中的第1段。   
#### （2）下载密钥
![](../imgs/submit-13.png)   
如上图，单击`作业管理->相应的作业名（也就是2）`。    
![](../imgs/submit-12.png)   
如上图，单击`2`。   
![](../imgs/submit-14.png)    
`Ctrl+单击2`下载Key 。   
![](../imgs/submit-15.png)    
#### （3）Pycharm连接服务
* 安装Pycharm    
Pycharm需要专业版，如何破解，请自行百度。    
* 配置本地与远程同步目录    
![](../imgs/submit-16.png)    
单击`1->2->3`    
![](../imgs/submit-17.png)    
单击`1->2`     
![](../imgs/submit-18.png)    
填写名称，自定义即可，本例为jupyter。    
![](../imgs/submit-19.png)    
![](../imgs/submit-20.png)     
单击`1->2->3->4->5->6`     
![](../imgs/submit-21.png)    
单击`1->2->3->4->5`，将本地工作目录映射到task容器内。    
![](../imgs/submit-22.png)    
![](../imgs/submit-23.png)   
单击`1->2->3`即可在本地查看远程task容器的文件目录。     
* 配置项目解析器     
![](../imgs/submit-24.png)    
单击`1->2`打开设置页面      
![](../imgs/submit-25.png)    
单击`1->2->3`，然后单击`add`    
![](../imgs/submit-26.png)   
单击`1->2->3`填入相应内容，然后单击`next`     
![](../imgs/submit-27.png)    
单击`1->2->3->4`填入相应内容，然后在单击`next`   
![](../imgs/submit-28.png)    
单击`1->2->3->4->5`填入相应内容，然后单击`OK->OK->Finish`。    
![](../imgs/submit-29.png)   
单击`OK`,即可配置完项目解析器。    
* 同步代码    
![](../imgs/submit-30.png)   
单击`1->2->3`    
![](../imgs/submit-31.png)    
  * 1是从本地到task容器
  * 2是从task容器到本地
  * 3是单步执行
  * 4是多步执行
* 运行代码      
![](../imgs/submit-32.png)    
## 第4节、参数介绍
#### 1 环境变量
作业（job）中的每个任务（task）都在一个Docker容器中运行。对于多任务作业（multi-task），一个任务（task）可能与其他任务（task）通信。因此，任务（task）需要了解其他任务（task）的运行时信息，如IP，端口等。系统将此类运行时信息作为环境变量公开给每个任务的Docker容器。为了相互通信，用户可以在容器中编写代码来访问那些运行时环境变量。这些环境变量也可以在作业配置文件中使用。

下面将展示Docker容器中可访问的环境变量的完整列表：    

|类别|环境变量名称|描述|
|:-|:-|:-|
|Job level|PAI_JOB_NAME|作业名称，由JSON中`jobName`设定|
||PAI_USER_NAME|提交这个作业的用户名|
||PAI_DEFAULT_FS_URI|默认的文件系统的URI|
|Task role level|PAI_TASK_ROLE_COUNT|任务角色（task-role）的数量|
||PAI_TASK_ROLE_LIST|配置文件中的所有任务角色名称（逗号分割）|
||PAI_TASK_ROLE_TASK_COUNT_$taskRole|任务角色的任务数量|
||PAI_HOST_IP_$taskRole_$taskIndex|指定$taskRole_$taskIndex的主机IP|
||PAI_PORT_LIST_$taskRole_$taskIndex_$portType|$taskRole_$taskIndex指定主机的端口|
||PAI_RESOURCE_$taskRole|配置文件中（类似“gpuNumber，cpuNumber，memMB，shmMB”）任务角色的资源需求|
||PAI_MIN_FAILED_TASK_COUNT_$taskRole|任务角色中的taskRole.minFailedTaskCount|
||PAI_MIN_SUCCEEDED_TASK_COUNT_$taskRole|任务角色中的taskRole.minSucceededTaskCount|
|Current task role|PAI_CURRENT_TASK_ROLE_NAME|当前任务角色的名称|
|Current task|PAI_CURRENT_TASK_ROLE_CURRENT_TASK_INDEX|	当前任务角色中当前任务的索引，从0开始|

### 2 Job配置文件（JSON关键字说明）
json文件描述了作业提交所需的详细配置。详细格式如下：
```
{
  "jobName":   String,
  "image":     String,
  "authFile":  String,
  "dataDir":   String,
  "outputDir": String,
  "codeDir":   String,
  "virtualCluster": String,
  "taskRoles": [
    {
      "name":       String,
      "taskNumber": Integer,
      "cpuNumber":  Integer,
      "memoryMB":   Integer,
      "shmMB":      Integer,
      "gpuNumber":  Integer,
      "portList": [
        {
          "label": String,
          "beginAt": Integer,
          "portNumber": Integer
        }
      ],
      "command":    String,
      "minFailedTaskCount": Integer,
      "minSucceededTaskCount": Integer
    }
  ],
  "gpuType": String,
  "retryCount": Integer,
  "jobEnvs": {
    "foo", Integer,
    "key", String,
    ...
  },
  "extras": {
    "foo", Integer,
    "key", String,
    ...
  }
}
```

json文件描述了作业提交所需的详细配置。详细格式如下：

|字段名称|格式|描述|
|:-|:-|:-|
|jobName|String（正则表达式：`^[A-Za-z0-9\-._~]+$`）,必须填写（requred）|作业名称，值唯一|
|image|	String,requred|Docker镜像的URL|
|authFile|	String, optional（选择填写）| HDFS URI	Docker registry authentication file existing on HDFS|
|dataDir|	String, optional| HDFS URI	Data directory existing on HDFS|
|outputDir|	String, optional| HDFS URI	Output directory on HDFS, $PAI_DEFAULT_FS_URI/Output/$jobName will be used if not specified|
|codeDir|	String, optional| HDFS URI	Code directory existing on HDFS, should not contain any data and should be less than 200MB. codeDir will created to your job container local environment and could be accessed inner job container. NOTE: this folder is readonly|
|virtualCluster	|String, optional|	The virtual cluster job runs on. If omitted, the job will run on default virtual cluster|
|taskRoles|	List, required|	任务角色列表，至少有一个|
|taskRole.name|	String（`^[A-Za-z0-9._~]+$ `), required	|任务角色的名称，唯一|
|taskRole.taskNumber|Integer, required|同一任务角色下的任务熟练，大于等于1|
|taskRole.cpuNumber|	Integer, required|每个任务分配的CPU核数大于等于1|
|taskRole.memoryMB|	Integer, required|每个任务所分配的内存，大于等于100MB|
|taskRole.shmMB	|Integer, optional|每个任务所分配的共享内存大小，默认值是64MB|
|taskRole.gpuNumber|	Integer, required|每个任务所分配的GPU数量，大于等于0|
|taskRole.portList|	List, optional|portType使用清单|
|taskRole.portType.label|	String（`^[A-Za-z0-9._~]+$ `), required|端口类型的标签名称|
|taskRole.portType.beginAt|	Integer, required|端口类型中的端口，0表示随机选择|
|taskRole.portType.portNumber|	Integer, required|	特定类型的端口数|
|taskRole.command|	String, required|任务角色中任务的可执行命令，不能为空|
|taskRole.minFailedTaskCount	|Integer, optional	|Number of failed tasks to fail the entire job, null or no less than 1, if set to null means the job will always succeed regardless any task failure. |
|taskRole.minSucceededTaskCount	|Integer, optional|	Number of succeeded tasks to succeed the entire job, null or no less than 1, if set to null means the job will only succeed until all tasks are completed and minFailedTaskCount is not triggered. |
|gpuType|	String, optional|指定要在任务中使用的GPU类型。如果省略，作业将在任何gpu类型上运行	|
|retryCount	|Integer, optional	|作业重试次数不低于0|
|jobEnvs|	Object, optional|作业env参数，键值对，在作业容器中可用且不允许替换|	
|jobEnvs.isDebug|	Boolean, optional	|将此标志设置为true，如果用户的命令以非零值退出，则将保留故障容器以进行作业调试。|
|extras|	Object, optional|	额外参数，键值对，保存作业可能使用的任何信息|

