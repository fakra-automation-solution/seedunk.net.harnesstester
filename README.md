# 线束测试仪上位机软件
[![主页](http://seedunk.com/badge/github-seedunk.net.harnesstester.svg)](https://github.com/fakra-automation-solution/seedunk.net.harnesstester)  ![开发软件](http://seedunk.com/badge/code-framework48%20.net6.ic-dotnet.cbg-green.svg) ![站点统计](http://seedunk.com/badge/gh-sdn-harnesstester.svg)
## 供应商
  [![苏州中测](http://seedunk.com/badge/hctest.svg)](http://seedunk.com/badge/hctest.html) [![益和](http://seedunk.com/badge/microtest.svg)](http://seedunk.com/badge/microtest.html)
## 通信方式
1. RS232通信
   使用[交叉线](#)
3. TCP通信
   一般测试机不提供网口，可[**利用RS232网关实现TCP通信**](#)
   
## 通信接口
  <img src="http://seedunk.com/media/@va35d57f6d5264.w-640.svg">

## 代码示例
### TCP通信
```c#
         public bool TryTest(out bool testResult, out string testResponse)
       {
               testResult = false;
               testResponse = "";
              bool tryStatus=false;
             try
             {  
               Socket clientSocket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
               //连接目标   
               clientSocket.Connect("Device IP|设备TCP地址", Device Port|设备TCP端口);
                ... 其他设置
               clientSocket.Send(Command Test|电测命令);
      
               byte[] res = new byte[1024];
           
               bool result = false;
               int reconnectCount = 0;
              
               while (reconnectCount<10)
               {
                   try
                   {
                       if (clientSocket.Receive(res) > 0)
                       {
                           testResponse += Encoding.ASCII.GetString(res); 
                           if (testResponse.IndexOf("#OK,") > -1)
                           {
                               testResult = true;
                           } 
                           tryStatus = true;  
                       }
                   }
                   catch (Exception e)
                   {   
                       reconnectCount++; 
                       Thread.Sleep(1000);
                   } 
               }
               clientSocket.Close(); 
           }
           catch (Exception ex)
           {
               Debug.WriteLine(ex.Messsage);
           }
      
            return tryStatus;
       }

```








