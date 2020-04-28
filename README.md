# 阅小说App接口文档

**所有人都可以调用相关的接口，接口不做任何身份验证。仅供大家开发交流使用。如有任何疑问可以通过以下方式联系我。**

*Telegram*: [https://t.me/yuenov](https://t.me/yuenov)

*Gmail*: <yuenov@gmail.com>

**阅小说App下载地址：**

 **<http://yuenov.com>**

 <img src="img/qrcode.png" width = "100" height = "100" alt="" align=center />

---

## HTTP接口

HTTP的域名为 **<http://yuenov.com>**

**国内运营商偶尔会屏蔽80端口所以建议不要使用80端口进行访问**

**目前除了80端口还开放了`15555` `16666` `17777` `18888` `19999`这几个端口，强烈建议使用这几个端口进行访问，访问的格式为*域名+端口+路径*。例如**

**<http://yuenov.com:15555>/path**

HTTP接口返回的数据统一的格式为：

``` json
{
  "result":{
    "code":0,
    "msg":"成功"
  },
  "data":{
  }
}
```
返回的字段定义如下：

|名称   | 类型  | 必需  | 说明  |
| ------------ | ------------| ------------ | ------------ |
| `result` | Object  | 是  | HTTP返回的数据状态信息<ul><li>`code` : `Integer类型` HTTP返回的数据状态码（非HTTP状态码）</li><li>`msg` : `String类型` HTTP返回的数据状态说明</li></ul>  |
|  `data` |  Object |  是 |   HTTP返回的数据|

数据的状态码`code`定义如下

|状态码|说明|
| ------------ | ------------ |
|0|返回数据正确|
|101|新用户创建成功|
|102|未查询到数据|
|1001|参数校验出错|
|1002|返回值异常|
|1003|非法请求|
|1005|权限验证异常|
|1007|远程调用服务超时|
|9999|系统出错|

**注：下面接口返回的数据全部在`data`对象内**

### 书架

#### 批量检查书籍是否有更新
下载到本地的书籍在特定时机应该检查书籍是否有更新，使得本地的数据保持最新的状态。一般用于连载书籍检查是否有新章节更新
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/api/v1/book/checkUpdate</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>POST <code>application/json格式</code></td>
  </tr>
  <tr>
    <td rowspan=2>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>books</code></td>
	<td>List</td>
	<td>是</td>
	<td>检查更新的书籍列表<ul><li><code>bookId</code> : <code>Integer类型</code> 需要检查更新的书籍号</li><li><code>chapterId</code> : <code>Long类型</code> 需要检查更新的书籍最后一章的章节号</li></ul> </tr></td>
</table>
返回结果

|名称|类型|说明|
| ------------ | ------------ | ------------ |
|`updateList`| List|需要更新的书籍列表<ul><li><code>bookId</code> : <code>Integer类型</code> 需要检查更新的书籍号</li><li><code>chapterId</code> : <code>Long类型</code> 需要检查更新的书籍最后一章的章节号</li></ul>|









 
 



