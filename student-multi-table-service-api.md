# 学生信息的RESTful服务API文档

## 说明

这个文档提供给前端开发和后端开发进行参考。前后端的开发者应该严格按照该文档的要求进行编程和设计。
该文档用于演示如何用Java Spring 来开发RESTful服务。目前这个API涉及2个数据库表：student，sclass. 

## 表结构

**学生表: student**

|列名|类型|备注|
|----|----|----|
|id|BIGINT|自增列|
|stuName|VCHAR|NOT NULL|
|stuNo|BIGINT|NOT NULL|
|classId|BIGINT||
|age|INT||
|gpa|DOUBLE||

**班级表: sclass**

|列名|类型|备注|
|----|----|----|
|id|BIGINT|自增列|
|className|VARCHAR||
|department|VARCHAR||


## APIs

Base URL:
```
http(s)://hostname:port
```
* 采用http还是https取决于你是否使用了SSL/TLS。
* hostname: 你的web server的IP或是域名，如果是本机的话，就是localhost 或127.0.0.1
* port: Web服务的端口号，比如：8080


### 1. Get a Student data by ID
#### HTTP Request

> GET  /api/v1/students/***studentId***

#### Parameters：
##### Path parameters
* **studentId** :  The ID of the student
##### Query parameters
无  (以后可加上fields这种投影参数)

#### HTTP Response
* Status code:  200 (成功)

成功的情况下，返回一个Student数据, 比如：
```
{
    "id": 12,
    "stuNo": 456,
    "stuName": "张三",
    "stuClass": {
         "id": 1,
         "className": "计203-1",
         "department": "计算机与控制工程学院"
    },
    "age": 19,
    "gpa": 3.3
}
```
错误的情况下，给出正确的Http status code 和具体的错误信息。


### 2. 获取所有学生或根据条件查询学生数据
#### HTTP Request

> GET  /api/v1/students

#### Parameters：
##### Path parameters
 无
##### Query parameters (所有参数都是optional的)
* **sno**    查找学号等于该参数的学生
* **sname**  查找名字包含该参数的学生
* **cid**    查找班级Id等于该参数的学生
* **age**    查找年龄等于该参数的学生
* **gpa**    查找gpa大于该参数的学生
* 未来可以继续改进的地方：加上fields投影和分页参数等

比如,查询gpa大于3.0的名字中包含'张'的所有学生： 
```
GET /api/v1/students?sname=张&gpa=3
```

#### HTTP Response
* Status code:  200 (成功)

成功的情况下，返回Student的数组, 比如：
```
[
    {
        "id": 11,
        "stuNo": 456,
        "stuName": "张三",
        "stuClass": {
            "id": 1,
            "className": "计203-1",
            "department": "计算机与控制工程学院"
         },
        "age": 19,
        "gpa": 3.3
    },
    {
        "id": 12,
        "stuNo": 568,
        "stuName": "李四",
        "stuClass": {
            "id": 2,
            "className": "计203-2",
            "department": "计算机与控制工程学院"
         },
        "age": 18,
        "gpa": 2.222
    }
    ...
]
```
错误的情况下，给出正确的Http status code 和具体的错误信息。


### 3. Add a student
#### HTTP Request

> POST  /api/v1/students

#### Parameters：
##### Path parameters
无
##### Query parameters
无

#### Request body
* **stuNo**   学号
* **stuName**  姓名
* **classId**  班级 （optional）
* **age**    年龄  （optional)
* **gpa**    GPA   (optional)

比如 HTTP body内容如下：
```   
{
    "stuNo": 888,
    "stuName": "Tim",
    "classId": 2,
    "age": 22,
    "gpa": 3.5
}
```

#### HTTP Response
* Status code:  201 (成功)

**注意：** 返回201的状态码，不要返回200！

成功的情况下，返回成功增加的这个Student数据（要有该Student的ID）, 比如：
```
{
    "id": 29,
    "stuNo": 888,
    "stuName": "Tim",
    "stuClass": {
            "id": 2,
            "className": "计203-2",
            "department": "计算机与控制工程学院"
     },
    "age": 22,
    "gpa": 3.5
}
```
错误的情况下，给出正确的Http status code 和具体的错误信息。


### 4. update a student
#### HTTP Request

> PATCH  /api/v1/students/***studentId***

#### Parameters：
##### Path parameters
* studentId:  要修改的学生的ID
##### Query parameters
无

#### Request body
* **stuNo**   学号  （optional)
* **stuName**  姓名  (optional)
* **classId**  班级 （optional）
* **age**    年龄  （optional)
* **gpa**    GPA   (optional)

注意：和PUT方法不同，PATCH方法在body中只需好包含要修改的内容。
比如要修改gpa，则 HTTP body内容如下：
```
{
    "gpa": 3.66
}
```

#### HTTP Response
* Status code:  200 (成功)

成功的情况下，返回修改后的Student数据（要有全部fields）, 比如：
```
{
    "id": 29,
    "stuNo": 888,
    "stuName": "Tim",
     "stuClass": {
            "id": 2,
            "className": "计203-2",
            "department": "计算机与控制工程学院"
     },
    "age": 22,
    "gpa": 3.66
}
```
错误的情况下，给出正确的Http status code 和具体的错误信息。


### 5. Delete a student
#### HTTP Request

> DELETE  /api/v1/students/***studentId***

#### Parameters：
##### Path parameters
* studentId:  要删除的学生的ID
##### Query parameters
无

#### HTTP Response
* Status code:  200 (成功)

Response body 为空



### 6. Get a class data by ID
#### HTTP Request

> GET  /api/v1/classes/***classId***

#### Parameters：
##### Path parameters
* **classId** :  The ID of the class
##### Query parameters
无  (以后可加上fields这种投影参数)

#### HTTP Response
* Status code:  200 (成功)

成功的情况下，返回一个class的数据, 比如：
```
{
    "id": 1,
    "className": "计203-1",
    "department": "计算机与控制工程学院"
}
```
错误的情况下，给出正确的Http status code 和具体的错误信息。


### 7. 获取所有班级数据
#### HTTP Request

> GET  /api/v1/classes

#### Parameters：
##### Path parameters
 无
##### Query parameters
无

#### HTTP Response
* Status code:  200 (成功)

成功的情况下，返回class数组, 比如：
```
[
    {
        "id": 1,
        "className": "计203-1",
        "department": "计算机与控制工程学院"
    },
    {
        "id": 2,
        "className": "计203-2",
        "department": "计算机与控制工程学院"
    },
    ...
]
```
错误的情况下，给出正确的Http status code 和具体的错误信息。

