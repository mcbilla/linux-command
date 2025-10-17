curl
===

利用URL规则在命令行下工作的文件传输工具

## 补充说明

**curl命令** 是一个利用URL规则在命令行下工作的文件传输工具。它支持文件的上传和下载，所以是综合传输工具，但按传统，习惯称curl为下载工具。作为一款强力工具，curl支持包括HTTP、HTTPS、ftp等众多协议，还支持POST、cookies、认证、从指定偏移处下载部分文件、用户代理字符串、限速、文件大小、进度条等特征。做网页处理流程和数据检索自动化，curl可以助一臂之力。

## 适用的Linux版本

curl 命令在大多数 Linux 发行版中都是默认安装的，你可以用`curl --version`命令来查看你的系统中的curl版本和支持的协议。如果你的系统中没有curl命令，你可以用以下命令来安装：

- 在基于 Debian 的系统中，如 Ubuntu

  ```
  sudo apt install curl
  ```

- 在基于 Red Hat 的系统中，如 CentOS

  ```
  # CentOS7
  sudo yum install curl
  
  # CentOS8
  sudo dnf install curl
  ```

- 在基于 Arch 的系统中，如 Manjaro

  ```
  sudo pacman -S curl
  ```

安装完成后，你可以用`curl --help`命令来查看curl的用法和选项。

## 命令语法

```shell
curl [options] [URL...]
```

其中，options 是可选的参数，用来指定 curl 的行为和输出。URL 是要传输数据的网址，可以是一个或多个。如果没有指定 URL，curl 会从标准输入中读取数据。

## 选项

```bash
-a   --append                                   # 上传文件时，附加到目标文件 
-A   --user-agent                               # 设置用户代理发送给服务器 
-anyauth                                        # 可以使用“任何”身份验证方法 
-b   --cookie                                   # cookie字符串或文件读取位置 
     --basic                                    # 使用HTTP基本验证 
-B   --use-ascii                                # 使用ASCII /文本传输 
-c   --cookie-jar                               # 操作结束后把cookie写入到这个文件中 
-C   --continue-at                              # 断点续传 
-d   --data                                     # HTTP POST方式传送数据 
     --data-ascii                               # 以ascii的方式post数据 
     --data-binary                              # 以二进制的方式post数据 
     --negotiate                                # 使用HTTP身份验证 
     --digest                                   # 使用数字身份验证 
     --disable-eprt                             # 禁止使用EPRT或LPRT 
     --disable-epsv                             # 禁止使用EPSV 
-D   --dump-header                              # 把header信息写入到该文件中 
     --egd-file                                 # 为随机数据(SSL)设置EGD socket路径 
     --tcp-nodelay                              # 使用TCP\_NODELAY选项 
-e   --referer                                  # 来源网址 
-E   --cert                                     # 客户端证书文件和密码 (SSL)
     --cert-type                                # 证书文件类型 (DER/PEM/ENG) (SSL)
     --key                                      # 私钥文件名 (SSL)
     --key-type                                 # 私钥文件类型 (DER/PEM/ENG) (SSL)
     --pass                                     # 私钥密码 (SSL)
     --engine                                   # 加密引擎使用 (SSL). "--engine list" for list 
     --cacert                                   # CA证书 (SSL)
     --capath                                   # CA目录 (made using c\_rehash) to verify peer against (SSL)
     --ciphers                                  # SSL密码 
     --compressed                               # 要求返回是压缩的形势 (using deflate or gzip)
     --connect-timeout                          # 设置最大请求时间 
     --create-dirs                              # 建立本地目录的目录层次结构 
     --crlf                                     # 上传是把LF转变成CRLF 
-f   --fail                                     # 连接失败时不显示http错误 
     --ftp-create-dirs                          # 如果远程目录不存在，创建远程目录 
     --ftp-method \[multicwd/nocwd/singlecwd]   # 控制CWD的使用 
     --ftp-pasv                                 # 使用 PASV/EPSV 代替端口 
     --ftp-skip-pasv-ip                         # 使用PASV的时候,忽略该IP地址 
     --ftp-ssl                                  # 尝试用 SSL/TLS 来进行ftp数据传输 
     --ftp-ssl-reqd                             # 要求用 SSL/TLS 来进行ftp数据传输 
-F   --form                                     # 模拟http表单提交数据 
     --form-string                              # 模拟http表单提交数据 
-g   --globoff                                  # 禁用网址序列和范围使用{}和\[] 
-G   --get                                      # 以get的方式来发送数据 
-H   --header                                   # 自定义头信息传递给服务器 
     --ignore-content-length                    # 忽略的HTTP头信息的长度 
-i   --include                                  # 输出时包括protocol头信息 
-I   --head                                     # 只显示请求头信息 
-j   --junk-session-cookies                     # 读取文件进忽略session cookie 
     --interface                                # 使用指定网络接口/地址 
     --krb4                                     # 使用指定安全级别的krb4 
-k   --insecure                                 # 允许不使用证书到SSL站点 
-K   --config                                   # 指定的配置文件读取 
-l   --list-only                                # 列出ftp目录下的文件名称 
     --limit-rate                               # 设置传输速度 
     --local-port                               # 强制使用本地端口号 
-m   --max-time                                 # 设置最大传输时间 
     --max-redirs                               # 设置最大读取的目录数 
     --max-filesize                             # 设置最大下载的文件总量 
-M   --manual                                   # 显示全手动 
-n   --netrc                                    # 从netrc文件中读取用户名和密码 
     --netrc-optional                           # 使用 .netrc 或者 URL来覆盖-n 
     --ntlm                                     # 使用 HTTP NTLM 身份验证 
-N   --no-buffer                                # 禁用缓冲输出 
-o   --output                                   # 把输出写到该文件中 
-O   --remote-name                              # 把输出写到该文件中，保留远程文件的文件名 
-p   --proxytunnel                              # 使用HTTP代理 
     --proxy-anyauth                            # 选择任一代理身份验证方法 
     --proxy-basic                              # 在代理上使用基本身份验证 
     --proxy-digest                             # 在代理上使用数字身份验证 
     --proxy-ntlm                               # 在代理上使用ntlm身份验证 
-P   --ftp-port                                 # 使用端口地址，而不是使用PASV 
-q                                              # 作为第一个参数，关闭 .curlrc 
-Q   --quote                                    # 文件传输前，发送命令到服务器 
-r   --range                                    # 检索来自HTTP/1.1或FTP服务器字节范围 
--range-file                                    # 读取（SSL）的随机文件 
-R   --remote-time                              # 在本地生成文件时，保留远程文件时间 
     --retry                                    # 传输出现问题时，重试的次数 
     --retry-delay                              # 传输出现问题时，设置重试间隔时间 
     --retry-max-time                           # 传输出现问题时，设置最大重试时间 
-s   --silent                                   # 静默模式。不输出任何东西 
-S   --show-error                               # 显示错误 
     --socks4                                   # 用socks4代理给定主机和端口 
     --socks5                                   # 用socks5代理给定主机和端口 
     --stderr                                   #   
-t   --telnet-option                            # Telnet选项设置 
     --trace                                    # 对指定文件进行debug 
     --trace-ascii                              # Like --跟踪但没有hex输出 
     --trace-time                               # 跟踪/详细输出时，添加时间戳 
-T   --upload-file                              # 上传文件 
     --url <url>                                # 要使用的 URL
-u   --user                                     # 设置服务器的用户和密码 
-U   --proxy-user                               # 设置代理用户名和密码 
-w   --write-out \[format]                      # 什么输出完成后 
-x   --proxy                                    # 在给定的端口上使用HTTP代理 
-X   --request                                  # 指定什么命令 
-y   --speed-time                               # 放弃限速所要的时间，默认为30 
-Y   --speed-limit                              # 停止传输速度的限制，速度时间 

```

## 示例

#### **实例1：GET 请求**

```shell
curl "http://www.example.com"    # 如果这里的URL指向的是一个文件或者一幅图都可以直接下载到本地
curl -i "http://www.example.com" # 显示全部信息
curl -l "http://www.example.com" # 显示页面内容
curl -v "http://www.example.com" # 显示get请求全过程解析
```

#### **实例2：POST 请求**

**1、application/x-www-form-urlencoded**

最常见的一种 POST 请求，用 curl 发起这种请求也很简单。

```shell
curl  -X POST -d 'name=张三'  http://localhost:2000/api/basic
```

**2、application/json**

跟发起 application/x-www-form-urlencoded 类型的 POST 请求类似，-d 参数值是 JSON 字符串，并且多了一个 Content-Type: application/json 指定发送内容的格式。

```shell
# 设置json请求体
curl -H "Content-Type: application/json" -X POST -d '{"id": "001", "name":"张三", "phone":"13099999999"}'  http://localhost:2000/api/json

# 读取data.txt文件的内容，作为数据体向服务器发送。
curl -H "Content-Type: application/json" -d '@data.txt' https://example.com/upload
```

**3、multipart/form-data**

这种请求一般涉及到文件上传。把数据内容先写到文件里，通过 -d @filename 的方式来提交数据。 例如，有一个 JSON 文件 data.json 内容如下:

```json
{
    "id": "001",
    "name":"张三",
    "phone":"13099999999"
}
```

执行下面命令进行上传

```shell
curl -H "Content-Type: application/json" -X POST -d @data.json  http://localhost:2000/api/json 
```

#### **实例3：文件下载**

你可以用curl命令来下载文件，只需要指定文件的URL即可。例如，你可以用以下命令来下载一个图片文件：

```bash
curl -O <https://www.bing.com/th?id=OGL.9c7f3b6e0f3d4f7c9c9a1f8f0f9c0f7c&rf=LaDigue_1920x1080.jpg&pid=hp>
```

这个命令会将图片文件保存到当前目录中，文件名由URL的最后一部分决定。如果你想指定文件名，你可以用-o选项，如下：

```bash
curl -o bing.jpg <https://www.bing.com/th?id=OGL.9c7f3b6e0f3d4f7c9c9a1f8f0f9c0f7c&rf=LaDigue_1920x1080.jpg&pid=hp>
```

#### 实例4：上传文件

你可以用curl命令来上传文件，只需要指定文件的路径和目标的URL即可。例如，你可以用以下命令来上传一个文本文件到一个FTP服务器：

```bash
$ curl -u user:password -T file.txt <ftp://example.com>
```

这个命令会将file.txt文件上传到example.com的根目录中，-u选项用来设置用户名和密码，-T选项用来指定要上传的文件。如果你想指定上传的目录和文件名，你可以在URL中加上，如下：

```bash
$ curl -u user:password -T file.txt <ftp://example.com/upload/new.txt>
```

这个命令会将file.txt文件上传到example.com的upload目录中，并重命名为new.txt。

#### 实例5：获取网页的源代码

如果你只输入一个URL，curl会默认发送GET请求，并显示网页的源代码。例如：

```bash
$ curl <https://www.bing.com>
<!DOCTYPE html>
<html lang="zh" dir="ltr" data-bm="200">
<head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="referrer" content="origin-when-cross-origin" />
    <meta name="robots" content="NOODP" />
    <meta name="msapplication-config" content="/a/sa/sa_config-nine.xml" /><meta name="theme-color" content="#4F4F4F" />
    <meta name="description" content="使用 Microsoft Bing 搜索引擎，即可获得最佳搜索结果和搜索体验。" />
    ...

</html>
```

#### 实例6：测试网站性能

你可以用curl命令来测试网站的性能，只需要指定-w选项，用来显示指定的信息，如下：

```bash
$ curl -w "@curl-format.txt" -o /dev/null -s <https://www.bing.com>
```

这个命令会将网页的输出重定向到/dev/null，即不显示在屏幕上，-s选项用来隐藏错误和进度信息，-w选项用来指定一个文件，该文件中包含了要显示的信息的格式。例如，curl-format.txt文件的内容如下：

```bash
time_namelookup:  %{time_namelookup}\\n
time_connect:  %{time_connect}\\n
time_appconnect:  %{time_appconnect}\\n
time_pretransfer:  %{time_pretransfer}\\n
time_redirect:  %{time_redirect}\\n
time_starttransfer:  %{time_starttransfer}\\n
----------\\n
time_total:  %{time_total}\\n
```

这个文件中的变量都是curl命令提供的，用来表示不同的时间，如域名解析时间，连接时间，传输时间等。你可以在[这里](https://curl.se/docs/manpage.html#-w)查看所有的变量和含义。运行上面的命令后，你会得到类似于以下的输出：

```bash
time_namelookup:  0.004
time_connect:  0.028
time_appconnect:  0.000
time_pretransfer:  0.028
time_redirect:  0.000
time_starttransfer:  0.103
----------
time_total:  0.103
```

这些信息可以帮助你分析网站的性能和瓶颈。

#### 实例7：显示HTTP状态码

你可以用curl命令来显示HTTP状态码，只需要指定-w选项，用来显示指定的信息，如下：

```bash
$ curl -w "%{http_code}" -o /dev/null -s <https://www.bing.com>
```

这个命令会将网页的输出重定向到/dev/null，即不显示在屏幕上，-s选项用来隐藏错误和进度信息，-w选项用来指定要显示的信息，这里是HTTP状态码。运行上面的命令后，你会得到类似于以下的输出：

```bash
200
```

这个输出表示网页的HTTP状态码是200，意味着请求成功。你可以在[这里](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)查看所有的HTTP状态码和含义。

#### 实例8：显示HTTP头部

你可以用curl命令来显示HTTP头部，只需要指定-I选项，用来只显示响应头，不显示响应体，如下：

```bash
$ curl -I <https://www.bing.com>
```

这个命令会显示网页的HTTP头部信息，如下：

```bash
HTTP/1.1 200 OK
Cache-Control: private, max-age=0
Content-Length: 487
Content-Type: text/html; charset=utf-8
Vary: Accept-Encoding
P3P: CP="NON UNI COM NAV STA LOC CURa DEVa PSAa PSDa OUR IND"
Set-Cookie: _EDGE_S=F=1&SID=0F7E3BC9BEC9A1E9B3F1F2C9BFE7F7E9; path=/; httponly; domain=bing.com
Set-Cookie: _EDGE_V=1; path=/; httponly; expires=Wed, 17-Jan-2024 06:28:38 GMT; domain=bing.com
Set-Cookie: MUID=0F7E3BC9BEC9A1E9B3F1F2C9BFE7F7E9; path=/; expires=Wed, 17-Jan-2024 06:28:38 GMT; domain=bing.com
Set-Cookie: MUIDB=0F7E3BC9BEC9A1E9B3F1F2C9BFE7F7E9; path=/; httponly; expires=Wed, 17-Jan-2024 06:28:38 GMT
X-MSEdge-Ref: Ref A: 9C7F3B6E0F3D4F7C9C9A1F8F0F9C0F7C Ref B: HK2EDGE0915 Ref C: 2023-12-18T06:28:38Z
Date: Mon, 18 Dec 2023 06:28:38 GMT
```

这些信息可以帮助你了解网页的元数据，如缓存控制，内容类型，内容长度，cookie，日期等。

#### 实例9：显示HTTP体

你可以用curl命令来显示HTTP体，只需要指定-i选项，用来在输出中包含响应头，如下：

```bash
$ curl -i <https://www.bing.com>
```

这个命令会显示网页的HTTP头部和HTTP体信息，如下：

```bash
HTTP/1.1 200 OK
Cache-Control: private, max-age=0
Content-Length: 487
Content-Type: text/html; charset=utf-8
Vary: Accept-Encoding
P3P: CP="NON UNI COM NAV STA LOC CURa DEVa PSAa PSDa OUR IND"
Set-Cookie: _EDGE_S=F=1&SID=0F7E3BC9BEC9A1E9B3F1F2C9BFE7F7E9; path=/; httponly; domain=bing.com
Set-Cookie: _EDGE_V=1; path=/; httponly; expires=Wed, 17-Jan-2024 06:28:38 GMT; domain=bing.com
Set-Cookie: MUID=0F7E3BC9BEC9A1E9B3F1F2C9BFE7F7E9; path=/; expires=Wed, 17-Jan-2024 06:28:38 GMT; domain=bing.com
Set-Cookie: MUIDB=0F7E3BC9BEC9A1E9B3F1F2C9BFE7F7E9; path=/; httponly; expires=Wed, 17-Jan-2024 06:28:38 GMT
X-MSEdge-Ref: Ref A: 9C7F3B6E0F3D4F7C9C9A1F8F0F9C0F7C Ref B: HK2EDGE0915 Ref C: 2023-12-18T06:28:38Z
Date: Mon, 18 Dec 2023 06:28:38 GMT

<!DOCTYPE html>
<html lang="zh" dir="ltr" data-bm="200">
<head><meta http-equiv="X-UA-Compatible" content="IE=edge" /><meta name="viewport" content="width=device-width, initial-scale=1" /><meta name="referrer" content="origin-when-cross-origin" /><meta name="robots" content="NOODP" /><meta name="msapplication-config" content="/a/sa/sa_config-nine.xml" /><meta name="theme-color" content="#4F4F4F" /><meta name="description" content="使用 Microsoft Bing 搜索引擎，即可获得最佳搜索结果和搜索体验。" /><meta name="title" content="Bing" />
    ···
</html>
```

#### 实例10：模拟用户行为

你可以用curl命令来模拟用户行为，只需要指定-A选项，用来设置用户代理字符串，如下：

```bash
$ curl -A "Mozilla/5.0 (iPhone; CPU iPhone OS 10_3 like Mac OS X) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.0 Mobile/14E5239e Safari/602.1" <https://www.bing.com>
```

这个命令会模拟一个iPhone用户访问Bing网站，-A选项用来设置用户代理字符串，这里是一个iPhone的用户代理字符串。你可以在[这里](https://developers.whatismybrowser.com/useragents/explore/)查看和选择不同的用户代理字符串，用来模拟不同的浏览器或设备。

#### 实例11：发送cookie数据

你可以用curl命令来发送cookie数据，只需要指定-b选项，用来附带指定的cookie数据，如下：

```bash
$ curl -b "name=alice;age=25" <https://bashcommandnotfound.cn>
```

这个命令会发送两个cookie数据，name和age，到bashcommandnotfound.cn网站，-b选项用来附带指定的cookie数据，可以是一个或多个，用分号分隔。你也可以用-c选项，用来保存接收到的cookie数据到指定的文件中，如下：

```bash
$ curl -c cookies.txt <https://bashcommandnotfound.cn>
```

这个命令会将接收到的cookie数据保存到cookies.txt文件中，-c选项用来指定要保存的文件名。你也可以用-b选项，用来读取文件中的cookie数据，如下：

```bash
$ curl -b cookies.txt <https://bashcommandnotfound.cn>
```

这个命令会读取cookies.txt文件中的cookie数据，并发送到bashcommandnotfound.cn网站，-b选项用来指定要读取的文件名。

#### 实例12：发送表单数据

你可以用curl命令来发送表单数据，只需要指定-F选项，用来附带指定的表单数据，如下：

```bash
$ curl -F "name=alice" -F "age=25" <https://example.com/form>
```

这个命令会发送两个表单数据，name和age，到example.com的form接口，-F选项用来附带指定的表单数据，可以是一个或多个。你也可以用@符号，用来指定一个文件作为表单数据的内容，如下：

```bash
$ curl -F "file=@file.txt" <https://example.com/form>
```

这个命令会将file.txt文件的内容作为表单数据，发送到example.com的form接口，-F选项用来指定文件的路径和名称。

#### 实例13：跟随重定向

你可以用curl命令来跟随重定向，只需要指定-L选项，用来自动跟随服务器返回的3xx状态码的URL，如下：

```bash
$ curl -L <https://example.com/redirect>
```

这个命令会访问example.com的redirect接口，如果服务器返回一个3xx状态码，如301，302，307等，表示重定向到另一个URL，curl会自动跟随该URL，直到得到一个非3xx状态码的响应，-L选项用来开启自动跟随重定向的功能。

#### 实例14：设置请求头

你可以用curl命令来设置请求头，只需要指定-H选项，用来设置指定的请求头，如下：

```bash
$ curl -H "Accept: application/json" -H "Authorization: Bearer your_token" <https://example.com/api>
```

这个命令会设置两个请求头，Accept和Authorization，到example.com的api接口，-H选项用来设置指定的请求头，可以是一个或多个。你可以在[这里](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)查看所有的请求头和含义。

#### 实例15：设置身份验证

你可以用curl命令来设置身份验证，只需要指定-u选项，用来设置用户名和密码，如下：

```bash
$ curl -u user:password <https://example.com/secure>
```

这个命令会设置用户名和密码，user和password，到example.com的secure接口，-u选项用来设置用户名和密码，用冒号分隔。这种方式是使用基本认证（Basic Authentication）的方法，你也可以使用其他的认证方法，如摘要认证（Digest Authentication），OAuth认证等，你可以在[这里](https://curl.se/docs/httpscripting.html#Authentication)查看更多的认证方法和选项。

## Linux curl命令的注意事项

以下是一些使用curl命令时需要注意的事项：

- 如果你的URL中包含特殊字符，如空格，引号，问号等，你需要用百分号编码来替换它们，否则curl可能无法正确解析URL。例如，你可以用%20来替换空格，用%22来替换引号，用%3F来替换问号等。你可以在[这里](https://www.urlencoder.org/)查看和转换百分号编码。
- 如果你的URL中包含变量，如$HOME，你需要用双引号来包裹URL，否则curl可能无法正确解析变量。例如，你可以用以下命令来下载一个文件到你的家目录中：

```bash
$ curl -o "$HOME/file.txt" <https://example.com/file.txt>
```

- 如果你的系统中没有curl命令，你可能会看到以下的错误信息：

```bash
$ curl <https://bashcommandnotfound.cn>
bash: curl: command not found
```

这时，你需要根据你的系统类型来安装curl命令，参考上面的Linux curl命令适用的Linux版本一节。
