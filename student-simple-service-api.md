# 学生信息（单表）的RESTful服务API文档

## 说明

这个文档提供给前端开发和后端开发进行参考。前后端的开发者应该严格按照该文档的要求进行编程和设计。
目前这个API只涉及一个数据库表：student。 后续课程会涉及多个表和更多的功能。

## 表结构

**表名: student**

|列名|类型|备注|
|----|----|----|
|id|INT|自增列|
|stuName|VCHAR|NOT NULL|
|stuNo|INT|NOT NULL|
|stuClass|VCHAR||
|age|INT||
|gpa|DOUBLE||

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
* 无查询参数 (后期可加上fields这种投影参数)

#### HTTP Response
* Status code:  200 (成功)

成功的情况下，返回一个Student数据, 比如：
```
{
    "id": 1,
    "stuNo": 456,
    "stuName": "张三",
    "stuClass": "计203-1",
    "age": 19,
    "gpa": 4.3
}
```
错误的情况下，给出正确的Http status code 和具体的错误信息。





1. 

