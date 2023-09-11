# API暂停对外访问，如果有任何需求可以通过邮件或者Telegram联系
Email: yuenov@gmail.com </br>
Telegram: https://t.me/twokingsl

----
# 阅小说App接口文档

[阅小说iOS客户端源码](https://github.com/yuenov/reader-ios)

[阅小说Android客户端源码](https://github.com/yuenov/reader-android)

**所有人都可以调用相关的接口，接口不做任何身份验证。仅供大家开发交流使用。**

**开放的接口会对针对IP的访问次数和频次有所限制，如果需要无限制使用或有任何疑问可以通过以下方式联系我**

*Telegram*: [https://t.me/yuenov](https://t.me/yuenov)

*Gmail*: <yuenov@gmail.com>

**注意：不要抓包使用阅小说App内的接口，阅小说App内的接口专属阅小说App使用
接口使用客户端内算法动态加密，生成的密钥有时效性限制。**

**阅小说App下载地址：**

 **[http://yuenov.com](http://yuenov.com?ch=g)**

 <img src="img/qrcode.png" width = "100" height = "100" alt="" align=center />

---

- [HTTP接口](#http接口)
	- [书架](#书架)
		- [批量检查书籍是否有更新](#批量检查书籍是否有更新)
		- [搜索书籍](#搜索书籍)
	- [发现](#发现)
		- [发现页](#发现页)
		- [分类](#分类)
		- [榜单](#榜单)
		- [榜单列表](#榜单列表)
		- [完本](#完本)
		- [专题](#专题)
		- [专题列表](#专题列表)
		- [发现页查看全部](#发现页查看全部)
	- [书城](#书城)
		- [分类书籍列表](#分类书籍列表)
		- [书籍详情](#书籍详情)
		- [书籍推荐](#书籍推荐)
		- [书籍目录](#书籍目录)
		- [书籍章节下载](#书籍章节下载)
		- [章节刷新](#章节刷新)
	- [其他](#其他)
		- [配置接口](#配置接口)
- [声明](#声明)

## HTTP接口

HTTP的域名为 **<http://yuenov.com>**

**国内运营商偶尔会屏蔽80端口所以建议不要使用80端口进行访问**

**目前除了80端口还开放了`15555` `16666` `17777` `18888` `19999`这几个端口，强烈建议使用这几个端口进行访问，访问的格式为*域名+端口+路径*。例如**

<span id="domain">**<http://yuenov.com:15555>/path**

**阅小说图片访问的格式示例**

<span>**<http://pt.yuenov.com:15555>/path**

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
|203|书源已经失效|
|1001|参数校验出错|
|1002|返回值异常|
|1003|非法请求|
|1005|权限验证异常|
|1007|远程调用服务超时|
|9999|系统出错|

**注：下面接口返回的数据全部在`data`对象内**

### 书架

#### 批量检查书籍是否有更新
下载到本地的书籍在特定时机应该检查书籍是否有更新，使得本地的数据保持最新的状态。一般用于连载书籍检查是否有新章节更新。

与[/app/open/api/chapter/getByBookId](#书籍目录)配合使用，来更新本地缓存的目录
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/book/checkUpdate</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>POST <code>application/json</code></td>
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
	<td>检查更新的书籍列表，每个对象的信息如下<ul><li><code>bookId</code> : <code>Integer类型</code> 需要检查更新的书籍号</li><li><code>chapterId</code> : <code>Long类型</code> 需要检查更新的书籍最后一章的章节号</li></ul> </td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ | ------------ |
|`updateList`| List|否|需要更新的书籍列表，每个对象的信息如下<ul><li><code>bookId</code> : <code>Integer类型</code> 需要检查更新的书籍号</li><li><code>chapterId</code> : <code>Long类型</code> 需要检查更新的书籍最后一章的章节号</li></ul>


#### 搜索书籍
根据关键词搜索书籍
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/book/search</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=4>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>keyWord</code></td>
	  <td>String</td>
	  <td>是</td>
	  <td>书籍关键词</td>
  </tr>
  <tr>
    <td><code>pageNum</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求第几页的数据，pageNum最小值为1</td>
  </tr>
  <tr>
    <td><code>pageSize</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求每页多少条的数据</td>
  </tr>
</table>

返回的结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否|搜索的结果书籍列表
|`pageNum`|Integer|否|请求第几页数据
|`pageSize`|Integer|否|请求每页多少条的数据
|`total`|Integer|否|实际返回多少条数据

<span id="book">书籍的字段定义如下
|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`author`| String|否|作者
|`bookId`|Integer|是|书籍号
|`categoryName`|String|是|书籍所属分类
|`chapterStatus`|String|否|书籍连载状态<ul><li>`END` : `String类型` 书籍已完结</li><li>`SERIALIZE` : `String类型` 书籍连载中</li></ul>
|`coverImg`| String|否|书籍的封面路径，**返回的是书籍封面的路径并非URL地址，需要手动拼接上*域名+端口*参考[这里](#domain)**
|`desc`|String|否|书籍内容介绍
|`title`|String|否|书籍的名称
|`word`|String|否|书籍的字数

### 发现
#### 发现页
App内发现页面接口
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/category/discovery</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否| 发现页书籍分类列表，列表中每个对象有以下字段<ul><li>`bookList` : `List类型` 书籍列表，每个书籍的定义在[这里](#book)</li><li>`categoryName` : `String类型` 每个分类的名称</li><li>`type` : `String类型` 每个分类的类型<ul><li>`READ_MOST` : `String类型` 大家都在看</li><li>`RECENT_UPDATE` : `String类型` 最近更新</li><li>`CATEGORY` : `String类型` 书籍分类</li></ul></li><li>`categoryId` : `Integer类型` 只有当**type=CATEGORY**时才有值表示书籍分类号</li></ul>

#### 分类

书籍的全部分类

<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/category/getCategoryChannel</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`channels`| List|否| 获取所有的频道分类列表，目前有男生频道和女生频道，列表中每个对象包含以下字段<ul><li>`categories` : `List类型` 分类信息列表</li><li>`channelId` : `Integer类型` 频道号</li><li>`channelName` : `String类型` 频道名称</li></ul>

分类信息的字段定义如下：

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`categoryId`| Integer |是| 分类号
|`categoryName`| String|否| 分类名
|`coverImgs`| List\<String\>|否| 分类的封面列表，包含该分类排名前三书籍的封面路径**并非URL地址，需要手动拼接上*域名+端口*参考[这里](#domain)**

#### 榜单
书籍榜单信息
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/rank/getList</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`channels`| List|否| 获取所有的频道榜单目前有男生频道和女生频道每个对象包含以下字段<ul><li>`ranks` : `List类型` 榜单信息列表</li><li>`channelId` : `Integer类型` 频道号</li><li>`channelName` : `String类型` 频道名称</li></ul>

榜单信息的字段定义如下：

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`rankId`| Integer |是| 榜单号
|`rankName`| String|否| 榜单名
|`coverImgs`| List|否| 榜单的封面列表，包含该榜单排名前三书籍的封面路径**并非URL地址，需要手动拼接上*域名+端口*参考[这里](#domain)**

#### 榜单列表
每个榜单内的书籍列表
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/rank/getPage</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=5>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>channelId</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>频道号</td>
  </tr>
  <tr>
    <td><code>rankId</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>榜单号</td>
  </tr>
  <tr>
    <td><code>pageNum</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求第几页的数据，pageNum最小值为1</td>
  </tr>
  <tr>
    <td><code>pageSize</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求每页多少条的数据</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否| 榜单书籍列表，每个书籍的定义在[这里](#book)
|`pageNum`| Integer|否| 请求第几页的数据，pageNum最小值为1
|`pageSize`| Integer|否| 请求每页多少条的数据
|`total`| Integer|否|总共有多少条数据


#### 完本
所有完本书籍信息
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/category/getCategoryEnd</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=3>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>pageNum</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求第几页的数据，pageNum最小值为1</td>
  </tr>
  <tr>
    <td><code>pageSize</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求每页多少条的数据</td>
  </tr>
</table>

返回数据

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否| 全部完结书籍的分类列表，列表中每个对象有以下字段<ul><li>`bookList` : `List类型` 书籍列表，每个书籍的定义在[这里](#book)</li><li>`categoryName` : `String类型` 每个分类的名称</li><li>`categoryId` : `Integer类型` 书籍分类号</li></ul>
|`pageNum`| Integer|否| 请求第几页的数据，pageNum最小值为1
|`pageSize`| Integer|否| 请求每页多少条的数据
|`total`| Integer|否|总共有多少条数据

#### 专题
书籍专题信息
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/book/getSpecialList</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=3>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>pageNum</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求第几页的数据，pageNum最小值为1</td>
  </tr>
  <tr>
    <td><code>pageSize</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求每页多少条的数据</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`specialList`| List|否| 全部的专题列表，列表中每个对象有以下字段<ul><li>`bookList` : `List类型` 书籍列表，每个书籍的定义在[这里](#book)</li><li>`name` : `String类型` 每个专题的名称</li><li>`id` : `Integer类型` 专题号</li></ul>

#### 专题列表
专题下全部的书籍和换一换列表
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/book/getSpecialPage</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=4>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>id</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>专题号</td>
  </tr>
  <tr>
    <td><code>pageNum</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求第几页的数据，pageNum最小值为1</td>
  </tr>
  <tr>
    <td><code>pageSize</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求每页多少条的数据</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否| 专题的书籍列表，每个书籍的定义在[这里](#book)
|`pageNum`| Integer|否| 请求第几页的数据，pageNum最小值为1
|`pageSize`| Integer|否| 请求每页多少条的数据
|`total`| Integer|否|总共有多少条数据

#### 发现页查看全部

查看发现页分类的全部或部分内容（查看全部，换一换）
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/category/discoveryAll</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=5>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>pageNum</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求第几页的数据，pageNum最小值为1</td>
  </tr>
  <tr>
    <td><code>pageSize</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求每页多少条的数据</td>
  </tr>
  <tr>
    <td><code>type</code></td>
	  <td>String</td>
	  <td>是</td>
	  <td>发现页的分类类型<ul><li><code>READ_MOST</code> : <code>String类型</code> 大家都在看</li><li><code>RECENT_UPDATE</code> : <code>String类型</code> 最近更新</li><li><code>CATEGORY</code> : <code>String类型</code> 书籍分类</li></ul></td>
  </tr>
  <tr>
    <td><code>categoryId</code></td>
	  <td>Integer</td>
	  <td>否</td>
	  <td>书籍分类号，仅当type=CATEGORY有效</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否| 发现页某个分类的书籍列表，每个书籍的定义在[这里](#book)
|`pageNum`| Integer|否| 请求第几页的数据，pageNum最小值为1
|`pageSize`| Integer|否| 请求每页多少条的数据
|`total`| Integer|否|总共有多少条数据

#### 书城

#### 分类书籍列表
某个分类下所有的书籍

<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/book/getCategoryId</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=6>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>pageNum</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求第几页的数据，pageNum最小值为1</td>
  </tr>
  <tr>
    <td><code>pageSize</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求每页多少条的数据</td>
  </tr>
  <tr>
    <td><code>categoryId</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>书籍所属的分类号</td>
  </tr>
  <tr>
    <td><code>channelId</code></td>
	  <td>Integer</td>
	  <td>否</td>
	  <td>某个频道下的分类书籍</td>
  </tr>
  <tr>
    <td><code>orderBy</code></td>
	  <td>String</td>
	  <td>否</td>
	  <td>分类书籍排序与筛选规则，不传返回全部书籍默认排序<ul><li><code>NEWEST</code> : <code>String类型</code> 按照最新的书籍进行排序</li><li><code>HOT</code> : <code>String类型</code> 按照最火爆的书籍进行排序</li><li><code>END</code> : <code>String类型</code> 筛选已完结的书籍</li></ul></td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否| 分类的书籍列表，每个书籍的定义在[这里](#book)
|`pageNum`| Integer|否| 请求第几页的数据，pageNum最小值为1
|`pageSize`| Integer|否| 请求每页多少条的数据
|`total`| Integer|否|总共有多少条数据

#### 书籍详情
每本书的详细信息

<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/book/getDetail</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=2>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>bookId</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>书籍号</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`author`| String |否| 书籍作者
|`bookId`| Integer|是| 书籍号
|`categoryName`| String|是| 书籍所属分类名
|`chapterNum`| Integer|否|总共有多少章节
|`coverImg`| String|否|书籍的封面路径，**返回的是书籍封面的路径并非URL地址，需要手动拼接上*域名+端口*参考[这里](#domain)**
|`desc`| String|否|书籍内容介绍
|`title`| String|否|书籍的名称
|`update`| Object|否|书籍更新信息包含以下字段<ul><li>`chapterId` : `Long类型` 最新的章节号</li><li>`chapterName` : `String类型` 最新的章节名称</li><li>`chapterStatus` : `String类型` 书籍连载状态<ul><li>`END` : `String类型` 书籍已完结</li><li>`SERIALIZE` : `String类型` 书籍连载中</li></ul></li><li>`time` : `Date类型` 书籍最近更新时间</li></ul>
|`word`| String|否|书籍的字数
|`recommend`| List|否|相关书籍推荐列表，每个书籍的定义在[这里](#book)


#### 书籍推荐
在书籍详情中的推荐列表和换一换

<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/book/getRecommend</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=4>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>bookId</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>书籍号</td>
  </tr>
  <tr>
    <td><code>pageNum</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求第几页的数据，pageNum最小值为1</td>
  </tr>
  <tr>
    <td><code>pageSize</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>请求每页多少条的数据</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否| 书籍推荐列表，每个书籍的定义在[这里](#book)
|`pageNum`| Integer|否| 请求第几页的数据，pageNum最小值为1
|`pageSize`| Integer|否| 请求每页多少条的数据
|`total`| Integer|否|当前请求有多少条数据

#### 书籍目录
获取书籍全部或部分目录信息，当书籍有更新时需要调用该接口来更新本地保存的目录信息，传入chapterId获取此章节之后的数据，不传chapterId是获取全部目录。如果本地已经保存过目录信息，建议最好传入最后一章chapterId来更新本地目录。而不是全量获取所有的目录。
<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/chapter/getByBookId</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
  <tr>
    <td rowspan=4>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
  <tr>
    <td><code>bookId</code></td>
	  <td>Integer</td>
	  <td>是</td>
	  <td>书籍号</td>
  </tr>
  <tr>
    <td><code>chapterId</code></td>
	  <td>Long</td>
	  <td>否</td>
	  <td>从第几章开始请求目录信息，如果不传请求全部的目录信息</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`author`| String|否| 作者
|`bookId`| Integer|是| 书籍号
|`coverImg`| String|否| **返回的是书籍封面的路径并非URL地址，需要手动拼接上*域名+端口*参考[这里](#domain)**
|`desc`| String|否|书籍内容介绍
|`title`| String|否|书籍的名称
|`word`| String|否|书籍的字数
|`chapters`|List|书籍的目录列表，每个目录包含以下字段<ul><li>`id` : `Long类型` 章节号 </li><li>`name` : `String类型` 章节名 </li><li>`v` : `Integer类型` **书籍的版本号，非常重要，书籍下载接口`/app/open/api/chapter/get`需要传递这个参数** </li></ul>


#### 书籍章节下载
下载章节内容，**目前开放接口不支持批量下载**

由于书源失效会导致部分书籍不可访问。如果错误码为`203`表示书源已经失效，此时服务器会自动更新书源。<br>
如果书源失效并返回`203`请重新调用书籍目录接口`/app/open/api/chapter/getByBookId`来获取最新的目录信息 <br>
此时当前接口需要传递`v`这个参数，这个参数由`/app/open/api/chapter/getByBookId`接口返回。<br>
`v`表示当前此书籍的版本。默认为`0`，表示此书籍的书源没有失效过。`1`表示为此书籍的书源更新过一次。<br>
`/app/open/api/chapter/getByBookId`返回`v=1`,如果当前接口传`v=0`表示从旧书源获取内容，会导致获取内容失败。<br>

<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/chapter/get</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>POST <code>application/json</code></td>
  </tr>
  <tr>
    <td rowspan=4>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
   <td><code>bookId</code></td>
	<td>Integer</td>
	<td>是</td>
	<td>下载的书籍号</td>
  </tr>
  <tr>
   <td><code>chapterIdList</code></td>
	<td>List&ltLong&gt</td>
	<td>是</td>
	<td>下载的章节号列表</td>
  </tr>
  <tr>
   <td><code>v</code></td>
	<td>Integer</td>
	<td>否</td>
	<td>书籍的版本号</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否| 下载的章节内容，每个章节的字段如下<ul><li>`content` : `String类型` 章节内容</li><li>`id` : `Long类型` 章节号</li><li>`name` : `String类型` 章节名</li></ul>

 #### 章节刷新

 刷新章节内容，获取最新的章节数据。与下载不一样，下载是获取服务器缓存的数据，但不是最新的数据。一般是下载的内容不正确时会调用该接口。该接口响应时间比较长，谨慎调用。

 <table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/chapter/updateForce</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>POST <code>application/json</code></td>
  </tr>
  <tr>
    <td rowspan=3>参数</td>
    <td>名称</td>
    <td>类型</td>
	<td>必需</td>
	<td>说明</td>
  </tr>
   <td><code>bookId</code></td>
	<td>Integer</td>
	<td>是</td>
	<td>更新的书籍号</td>
  </tr>
  <tr>
   <td><code>chapterIdList</code></td>
	<td>List&ltLong&gt</td>
	<td>是</td>
	<td>更新的章节号列表</td>
  </tr>
</table>

返回结果

|名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`list`| List|否| 更新的章节内容，每个章节的字段如下<ul><li>`content` : `String类型` 章节内容</li><li>`id` : `Long类型` 章节号</li><li>`name` : `String类型` 章节名</li></ul>

### 其他

#### 配置接口

获取热门搜索，书籍默认分类等配置信息，通常是在每次开机时启动


<table>
  <tr>
    <th >路径</th>
    <th colspan=4><code>/app/open/api/system/getAppConfig</code></th>
  </tr>
  <tr>
    <td>请求方式</td>
    <td colspan=4>GET</td>
  </tr>
</table>
 
 返回结果

 |名称|类型|必需|说明|
| ------------ | ------------ | ------------ |------------ |
|`categories`| List|否| 书籍默认的分类列表，每个分类的字段如下<ul><li>`categoryId` : `Integer类型` 分类号</li><li>`categoryName` : `String类型` 分类名</li></ul>
|`hotSearch`| List|否| 热搜书籍列表，每个书籍的定义在[这里](#book)


## 声明

**服务器只提供搜索以及在线转码的计算能力，没有存储任何小说内容。如有任何疑问请通过上面提供的联系方式联系我们。**



