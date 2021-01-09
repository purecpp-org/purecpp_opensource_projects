# purecpp社区开源项目列表

## 格式

项目名称：

状态：（已发布/孵化中）

需要的C++版本：

项目简介：

code first：(10行以内的代码展示项目)

# 项目列表

## 已发布

* [rest_rpc](#rest_rpc)
* [cinatra](#cinatra)
* [iguana](#iguana)
* [ormpp](#ormpp)
* [feather](#feather)
* [asio_redis_client](#asio_redis_client)
* [future](#future)
* [NoahGameFrame](#NoahGameFrame)
* [ajson](#ajson)
* [drogon](#drogon)
* [workflow](#workflow)
* [srpc](#srpc)

## 孵化中

* [plugincpp](#plugincpp)
* [raftcpp](#raftcpp)
* [moon](#moon)

## rest_rpc

项目名称：[rest_rpc](https://github.com/qicosmos/rest_rpc) 

状态：已发布

需要的C++版本：C++11

项目简介：

rest_rpc是一个高性能、易用、跨平台、header only的c++11 rpc库，它的目标是让tcp通信变得非常简单易用，**零依赖，下载下来就可以直接使用**。

code first:

```c++
//server
std::string echo(rpc_conn conn, const std::string& src) {
	return src;
}

server.register_handler("echo", echo);

//client
std::string result = client.call<std::string>("echo", "hello"); //sync call

std::future<std::string> future = client->async_call<FUTURE>("echo", "hello"); //future

client.async_call("echo", [](boost::system::error_code ec, string_view data){ //async call
	std::cout << "echo " << as<std::string>(data) << '\n';
});
```

## cinatra

项目名称：[cinatra](https://github.com/qicosmos/cinatra)

状态：已发布

需要的C++版本：C++17

项目简介：

cinatra是一个高性能易用的跨平台http（server和client）框架，它是用modern c++(c++17)开发的，它的目标是提供一个快速开发的c++ http框架。**零依赖，下载下来就可以直接使用**。它的主要特点如下：

统一而简单的接口
header-only
跨平台
高效
支持面向切面编程

cinatra目前支持了http1.1/1.0, ssl、websocket和文件上传下载, 你可以用它轻易地开发一个http服务器。

code first：

```c++
//server
server.set_http_handler<GET, POST>("/", [](request& req, response& res) mutable{
    res.set_status_and_content(status_type::ok, "hello world");
});

//client
std::string uri = "http://www.purecpp.com";
response_data result = client->get(uri); //sync get

client->async_get(uri, [](response_data data) {// async get
            print(data);
        });
```

## iguana

项目名称：[iguana](https://github.com/qicosmos/iguana)

状态：已发布

需要的C++版本：C++17

项目简介：通用的跨平台的序列化引擎，支持c++对象到json、xml等格式的相互转换，**零依赖，下载下来就可以直接使用**。

code first：

```c++
struct person{
  std::string name;
  int         age;
};
REFLECTION(person, name, age) //define meta data

//serialize and deserialize
person p = { "tom", 28 };
iguana::string_stream ss;
iguana::json::to_json(ss, p);
iguana::json::from_json(p, ss.str());
```

## ormpp

项目名称：[ormpp](https://github.com/qicosmos/ormpp)

状态：已发布

需要的C++版本：C++17

项目简介：ormpp是一个跨平台易用的ORM库，最重要的目标就是让c++中的数据库编程变得简单，为用户提供统一的接口，支持多种数据库，提高数据库开发效率。

code first：

```c++
struct person{
	int id;
	std::string name;
	int age;
};
REFLECTION(person, id, name, age)

person p = {1, "test1", 2};
mysql.insert(p);
std::vector<person> result = mysql.query<person>(); //get all
auto result = mysql.query<std::tuple<std::string>>("select name from person"); //get part
```

## feather

项目名称：[feather](https://github.com/qicosmos/feather)

状态：已发布

需要的C++版本：C++17

项目简介：

Feather是一个快速开发的跨平台的modern c++ web框架，Feather的目标是让使用者快速开发一个高性能的web网站。

code first:

```c++
void comment(request& req, response& res) {
    pp_comment comment{};//ommit init of comment.
    int r = dao.add_object(comment);
    if (r < 0) {
        res.set_status_and_content(status_type::internal_server_error);
    }
    else {
        res.redirect("./detail?id=" + post_id);
    }
}

server.set_http_handler<POST>("/comment", comment, check_login{}, check_comment_input{});

//post request
http://purecpp.org/comment
```

## asio_redis_client

项目名称：[asio_redis_client](https://github.com/topcpporg/asio_redis_client)

状态：已发布

需要的C++版本：C++11

项目简介：

an easy to use, thread-safe, cross-platform async redis client implemented in c++11.

The best c++ redis client!

code first:

```c++
  redis_client->set("hello", "world", [](RedisValue value) {
    if(value.isError()){
      std::cout<<"error:"<<value.toString()<<'\n';
      return;
    }

    std::cout << "set: " << value.toString() << '\n';
  });

  redis_client->get("hello", [](RedisValue value) {
    std::cout << "get: " << value.toString() << '\n';
  });
```

## future

项目名称：[future](https://github.com/topcpporg/future)

状态：已发布

需要的C++版本：C++11

项目简介：

a std::future extension implemented in c++11.

C++11标准中提供了std::future和std::promise，但是标准库的future无法实现链式调用，无法满足异步并行编程的场景, 这个库提供了多线程异步并行的解决方法。

code first:

```c++
  auto future = Async([]{
      return 42;
  }).Then([](int i){
    return i + 2;
  }).Then([](int x){
    return std::to_string(x);
  });

  std::string str = future.Get(); //44
```

## NoahGameFrame

项目名称：[NoahGameFrame](https://github.com/ketoo/NoahGameFrame)

状态：已发布

需要的C++版本：C++11

项目简介：

C++跨平台插件式，模块化的游戏服务器架构，面向接口编程，下载即用.

code first:

```c++
bool NFHelloWorld3Module::Init()
{
        m_pKernelModule = pPluginManager->FindModule<NFIKernelModule>();
        m_pKernelModule->AddClassCallBack(NFrame::Player::ThisName(), this, &NFHelloWorld3Module::OnClassCallBackEvent);
}

int NFHelloWorld3Module::OnClassCallBackEvent(const NFGUID& self, const std::string& className, const CLASS_OBJECT_EVENT event, const NFDataList& arg)
{
	m_pEventModule->AddEventCallBack(self, 1, this, &NFHelloWorld3Module::OnEvent);
	m_pKernelModule->SetPropertyInt(self, "Hello", 1);
	m_pKernelModule->SetPropertyString(self, "Hello", "hello world");
	return 0;
}
```

## ajson

项目名称：[ajson](https://github.com/lordoffox/ajson)

状态：已发布

需要的C++版本：C++11

项目简介：

a utility for serialize C++ and json.

code first:

```c++
struct Person{
  std::string  Name;
  int          Age;
};

AJSON(Person , Name , Age)

int main(int argc,char* argv[]){
  Person obj;
  char * json= "{\"Name\" : \"Boo\", \"Age\" : 28}";
  ajson::load_from_buff(obj,json);
  return 0;
}
```


## plugincpp

项目名称：[plugincpp](https://github.com/qicosmos/plugincpp)

状态：孵化中

需要的C++版本：C++11

项目简介：

现代C++跨平台插件框架，零依赖，下载即用。处于开发中状态。

code first:

```c++
TODO
```

## raftcpp

项目名称：[plugincpp](https://github.com/topcpporg/raftcpp)

状态：孵化中

需要的C++版本：C++17

项目简介：

An implementation of Raft consensus algorithm in modern C++.

code first:

```c++
TODO
```

## moon

项目名称：[moon](https://github.com/sniper00/moon)

状态：孵化中

需要的C++版本：C++17

项目简介：

C++编写的跨平台游戏服务器框架，采用(线程绑定)Actor模型，C/CPP编写核心库，Lua编写逻辑代码的开发方式。框架注重游戏服务器开发，仅提供核心功能，不带游戏逻辑代码。

code first:

```lua
---Scene service
function CMD.PlayerMove(targetPos, speed)
	--call navmesh service(write with cpp navmeshlib),return path points
	return moon.co_call("lua", addr_namvesh, "FindPath", startPos, targetPos)
end

---Player service
local path, err = moon.co_call("lua", addr_scene, "PlayerMove", {x=123.0,y = 124.0}, 1.0)
-- do something
```

## drogon

项目名称: [drogon](https://github.com/an-tao/drogon)

状态: 已发布

需要的C++版本: C++14

项目简介:

Drogon是一个基于C++14/17的Http应用高性能跨平台异步框架，使用Drogon可以方便的使用C++构建各种类型的Web应用程序。

code first:

```c++
#include <drogon/drogon.h>
using namespace drogon;
int main()
{
    app().setLogPath("./")
         .setLogLevel(trantor::Logger::kWarn)
         .addListener("0.0.0.0", 80)
         .setThreadNum(16)
         .enableRunAsDaemon()
         .run();
}
```

## workflow

项目名称: [workflow](https://github.com/sogou/workflow)

状态: 已发布

需要的C++版本: C++11

项目简介:

Workflow可以同时用于异步调度和并行计算，自带Http/Redis/MySQL/Kafka协议，除OpenSSL无其他依赖，通过任务流模式为用户提供完备的通信计算融为一体的编程范式，自带服务治理，是一个设计优雅的企业级编程引擎，在搜狗内部支撑搜索服务、云输入法、在线广告的每日数百亿以上的请求。

code first:

```c++
int main()
{
    WFHttpServer server([](WFHttpTask *task) {
        task->get_resp()->append_output_body("<html>Hello World!</html>");
    });
    if (server.start(8888) == 0) {  // start server on port 8888
        getchar(); // press "Enter" to end.
        server.stop();
    }
    return 0;
}
```

## srpc

项目名称: [srpc](https://github.com/sogou/srpc)

状态: 已发布

需要的C++版本: C++11

项目简介:

srpc是基于workflow开发的RPC系统，兼具高性能和低门槛。支持IDL：**Protobuf/Thrift**；支持协议：**SRPC/BRPC/ThriftFramed/ThriftHttp**；支持压缩类型：**snappy/gzip/zlib/lz4**；支持json且可使用Http进行跨语言。自带部分代码生成，其中thrift纯手工解析，并且打通workflow自带的其他功能包括任务流、计算调度和服务治理等。

code first:

```c++
int main()
{
    Example::SRPCClient client("127.0.0.1", 1412);
    EchoRequest req;
    req.set_message("Hello, srpc!");
    client.Echo(&req, [](EchoResponse *response, RPCContext *ctx) {
        printf("%s\n", response->DebugString().c_str());
    });
    pause();
    return 0;
}
```
