## web和req对象

bamboo工程中的每个handler，必须接收两个固定的参数`web`和`req`，`web`是连接对象，里面封装有一些处理请求的方法，`req`是请求对象，内含用户请求的所有数据。每进来一个请求，都会生成一个新的`web`对象和`req`对象，供handler调用。

通常，使用`web:page(html_str)`来返回html内容给客户端，使用`web:json(lua_table)`来返回json表给客户端。

下面列出web object和req object的详细内容。

### Web Object `web`

##### web:page (data, code, status, headers)

	将字符串数据data返回给客户端。
	- data		字符串，在bamboo中，一般由View()渲染生成
	- code		http响应状态编码，数字，可省略
	- status	http响应状态名称，字符串，可省略
	- headers	http响应头信息，lua table, 可省略

##### web:html (html_tmpl, tbl)

	不建议使用。
	渲染模板html_tmpl，返回给客户端。web:page(View('xxxx'){})的快捷形式。
	- html_tmpl  字符串  模板文件的名字
	- tbl        表      待渲染的参数 

##### web:json(data, ctype)
	
	返回json串。
	- data		lua table, 要返回的json值对应的lua表
	- ctype		string, 用于指定返回的具体类型，比如：'text/html'。可省略。

##### web:jsonError(err_code, err_desc)

	`web:json` 的快捷方式，预设 success = false 。
	- err_code		错误代码。数字
	- err_desc		错误描述。字符串

##### web:jsonSuccess(tbl)

	`web:json` 的快捷方式，预设 success = true 。
	- tbl			要返回的其它参数表

##### web:redirect(url)

	重定向到url。

##### web:method()

	取得当前http请求使用的指令方法，一般为GET或POST。

##### web:isAjax()

	检查当前请求是否是ajax请求。

##### web:path()

	取得当前请求的URI。

##### web:ok(msg)

	Used to response OK page quickly.

##### web:notFound()

	Used to response 404 page quickly.

##### web:unauthorized()

	Used to response 401 page quickly.

##### web:forbidden()

	Used to response 403 page quickly.

##### web:badRequest()

	Used to response 400 page quickly.

##### web:close()

	Close this web connection.

##### web:error(data, code, status, headers)

	Used to response an page quickly, and close this connection.

##### web:session()

	Return session id of this connection.

##### web:getCookie()

	Return cookie value of this request.

##### web:setCookie(cookie)

	Set `set-cookie` flag in headers.

##### web:zapSession()

	Make a new cookie code and set it to request headers.


--------------------------

### Request Object `req`

`req` 中通过包含如下数据：

##### req.GET

	使用GET指令的http请求传上来的参数放在这里面。

##### req.POST

	使用POST指令的http请求传上来的参数放在这里面。
	
##### req.PARAMS

	GET或POST请求传上来的参数放在这里面。对于req.GET和req.POST中有相同的key的情况，以req.POST中的值为准；

##### req.conn_id

	由monserver传入到bamboo中的连接id值。

##### req.sender

	发送对象的uuid，由bamboo工程指定。

##### req.path

	URI be visited in this request.

##### req.session_id

	The session id of this request.

##### req.body

	The body string part of this request.

##### req.data

	A table, into which we can put some key-value.

##### req.headers

	The request headers table.

##### req.headers.URI

	URI of this request.

##### req.headers.PATH

	URI of this request.

##### req.headers.QUERY

	query of this request, contains URI and query string, like '/index?title=foo&n=10'

##### req.headers.referer

	The previous page on which you click an anchor to make this request.

##### req.headers.connection

	Usually is 'keep-alive'.

##### req.headers.x-forwarded-for

	Usually is '127.0.0.1'.

##### req.headers.host

	The host name of this website.

##### req.headers.accept-charset

	Something like 'GB2312,utf-8;q=0.7,*;q=0.7'.

##### req.headers.VERSION

	Usually is 'HTTP/1.1'.

##### req.headers.METHOD

	GET, POST, PUT or DELETE.

##### req.headers.accept

	Something like 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8'.

##### req.headers.cookie

	The cookie string of this request.

##### req.headers.cache-control

	Something like 'max-age=0'.

##### req.headers.accept-language

	Something like 'zh-cn,zh;q=0.5'.

##### req.headers.user-agent

	Something like 'Mozilla/5.0 (X11; U; Linux x86_64; zh-CN; rv:1.9.2.8) Gecko/20100723 Ubuntu/10.04 (lucid) Firefox/3.6.8'

##### req.headers.keep-alive

	Something like '115'.

##### req.headers.accept-encoding

	Something like 'gzip,deflate'.

-----------------------------------

PS：由于太常用了，我们把web和req导出成了**全局变量**，在bamboo工程中的每一个地方都可以访问到。

