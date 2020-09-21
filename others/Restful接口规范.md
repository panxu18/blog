#### 域名

将API部署在专用域名之下

```
https://api.example.com
```

#### 版本

将版本号放入URL

```
https://api.example.com/v1/
```

#### 路径

在Restful中每一个网址表示一种资源，所以网址中不能由动词，只能使用名词通常使用复数形式表示资源集合。

```
https://api.example.com/v1/users
```

#### HTTP方法

```
GET /users 获取所有用户
GET /users/id  获取指定用户
POST /users  创建一个新用户
PUT /users/id 修改指定用户的信息，传递用户全部信息，操作幂等
PATCH /users/id 修改指定用户信息，传递部分需要修改的信息
DELETE /users/id 删除指定用户
```

#### 过滤信息

````
?limit=10 指定返回数量
?offset=10 指定返回记录的开始位置
?page=2&per_page=10 分页
?sortby=name&order=asc 排序
?user_role=1 指定筛选条件
````

#### 状态码

```
200 OK [GET] 返回用户的请求，操作幂等
201 CREATED [POST/PUT/PATCH] 用户新建或修改数据成功
202 Accepted [*] 请求进入后台排序（异步任务）
204 NO CONTENT [DELETE] 用户删除数据成功
400 INVALID REQUEST [POST/PUT/PATCH] 请求有错误，没有进行数据新建或修改，操作幂等
401 Unauthorized [*] 未授权
403 Forbidden [*] 得到授权，但是被禁止访问
404 NOT FOUND [*] 请求数据不存在，服务器没有进行操作，操作幂等
406 NOT Acceptable [GET] 请求格式例如请求JSON格式但是只有XML格式
410 Gone [GET] 请求资源被永久删除
422 Unprocesable entity [POST/PUT/PATCH] 创建或修改对象时，发生验证错误
500 Internal Server Error [*] 服务器发生错误
```

#### 返回结果

````
GET 返回单个或则资源列表
POST 返回新生成的对象
PUT 返回修改后完整的资源对象
PATCH 返回修改后完整的资源对象
DELETE 返回空文档
````

#### Hypermedia API

返回的文档中包含链接指向其他API

```json
{
	"href" : "https://api.example.com/v2/users",
	"documentation_url" : "https://help.example.com/v2"
}
```



