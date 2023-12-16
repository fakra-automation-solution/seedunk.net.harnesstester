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
  ```c#
   public bool TryTest(out bool testResult, out string testLog)
 {
     try
     {  
         Socket clientSocket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
         //连接目标   
         clientSocket.Connect("通信IP", 通信端口);
          ... 其他设置
         clientSocket.Send(Command Test|电测命令);

         byte[] res = new byte[1024];
         bool wait = true;
         bool result = false;
         int waitCount = 0;
         testResult = false;
         testLog = "";
         while (wait)
         {
             try
             {
                 if (clientSocket.Receive(res) > 0)
                 {
                     testLog = Encoding.ASCII.GetString(res); 
                     if (testLog.IndexOf("#OK,") > -1)
                     {
                         testResult = true;
                     }
                     wait = false;
                     result = true; 
                     break;
                 }
             }
             catch (Exception e)
             { 
              
                 waitCount++; 
                 Thread.Sleep(1000);
             }
             if (waitCount > 10)
             {
                 wait = false;
             }
         }
         clientSocket.Close();
         return result;
     }
     catch (Exception ex)
     {
         testResult = false;
         testLog = ""; 
         return false;
     }
 }
  ```








