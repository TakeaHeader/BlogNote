# TOMCAT ERORR  (Invalid character found in the request target. The valid characters are defined in RFC 7230 and RFC 3986 )错误记录

**描述**
***
在tomcat 7版本之后都对请求URL加强了控制，严格遵守 RFC 7230 and RFC 3986 协议 ，所以当URL中包含协议中所规定的违法字符，tomcat便会返回 404 错误


**解决方法**
***
1. 
![修改文件]()


在/conf/catalina.properties 中删除注释tomcat.util.http.parser.HttpParser.requestTargetAllow=| 并修改为需要忽略的特殊字符

2.
![修改文件]()
在/conf/server.xml  中<Connector  /> 中加入
          ```
       relaxedPathChars="{}&quot;"
       relaxedQueryChars="{}&quot;"
               <Connector port="19999" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="443"
	             relaxedPathChars="{}&quot;"
               relaxedQueryChars="{}&quot;"
                />
            ```
            
            
***
Tomcat 官方建议使用第二种方式 ，当然也可以更换低版本 tomcat
            
