
- 查询用户
```
GET /activiti-app/app/rest/admin/users?sort=idAsc&start=0
```

- 创建用户
```
POST /activiti-app/app/rest/admin/users
{"id":"leader","email":"leader@leader.leader","firstName":"leader","lastName":"L","password":"123"}
```

- 查询流程模型
```
GET /activiti-app/app/rest/models?filter=myProcesses&modelType=0&sort=modifiedDesc
```

- 查询流程图
```
GET /activiti-app/app/rest/models/c9962e4e-0625-447e-9ff6-c0f52763fe30/model-json?nocaching=1593162145467
```

- 查询流程实例
```
POST /activiti-app/app/rest/process-instances
{"processDefinitionId":"TestProcess:1:5004","name":"Test Process - June 26th 2020"}

POST /activiti-app/app/rest/process-instances
{"sort":"created-desc","page":0,"deploymentKey":"OA2","state":"running"}

```

- 声明流程实例
```
PUT /activiti-app/app/rest/tasks/5011/action/claim
```

- 声明和完成流程实例
```
PUT /activiti-app/app/rest/tasks/5011/action/claim
PUT /activiti-app/app/rest/tasks/5011/action/complete
```

