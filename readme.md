# pomelo 游戏框架的typescript客户端连接库

## Install

    npm install pomelo-client-ts --save

## Usage

    import { pomelo } from "pomelo-client-ts";

    pomelo.create('ws://127.0.0.1:3010',{

        /// 注意 auth 函数是一个 promise 方法
        auth:async(){
            /// 这是一个自动对连接鉴权的函数,
            /// 例如: 
            /// connection 内部封装了一个 getItem setItem 的方法用来获取本地存储
            const sessionId = pomelo.connection.getItem("sessionId");
            if( sessionId ){
                /// 这里的路由方法需要在服务器实现(相当于一个断线重连恢复玩家服务器数据的功能)
                const ok = await pomelo.connection.request("connector.session.auth",{ sessionId });
                if( !ok ){
                    pomelo.connection.setItem('sessionId',undefined);
                }
            }
        }
    }).then((session)=>{
        console.log("连接完成");
    });

    pomelo.connection.on('error',()=>{
        /// 这是socket 错误的事件
    });
    pomelo.connection.on('connected',()=>{
        /// 这是链接初始化完成的事件(注意区别于鉴权 ready )
    });
    pomelo.connection.on('disconnected',()=>{
        /// 这是 socket 断开连接的事件
    });
    pomelo.connection.on('kicked',()=>{
        /// 这是服务器踢出连接的事件
    });
    pomelo.connection.on('ready',()=>{
        /// 这是鉴权完成后的事件
    });
