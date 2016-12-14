# AIDL
使用AIDL实现进程间通信+handler机制
           
   额外知识点补充：
   
         -1.开启子线程
         
             new Thread(new Runnable(){
                @Override
                public void run(){
                    //处理具体的逻辑
                  
                }
             }).star();
             
        - 2.要想修改主线程的UI,不能在子线程修改，利用handler机制来修改主线程的UI 或 runOnUiThread()
         
         handler机制的原理:
              4个部分组成：Message Handler MessageQueue Looper
              
                 1). Message
                    线程之间传递的消息，用于在不同线程之间交换数据。arg1，arg2携带的是int类型的数据，obj携带的是Object类型。
                    
                 2). Handler
                    处理者的意思。主要是用于发送和处理消息的。发送消息使用Handler的sendMessage()方法，发送的消息最终会传到Handler的handlerMessage()方法中。
                    
                 3).  MessageQueue
                     消息队列的意思。主要用于存放所有通过Handler发送的消息。这些消息会一直存放在MessageQueue中，等待被处理。
                     
                 4).  Looper
                    MessageQueue的管家，调用Looper的loop()方法后，会进入无限循环，每当发现MessageQueue中存在一个消息就会把他取出来，传递到handler中的handlerMessage()中。
   
   
   AIDL实现步骤
   
     服务器端：
     
        - 1.定义AIDL，接口跟方法都不带修饰符
             interface MyAIDL{
                String getMsg(String text);
             }
             
       - 2.创建service，重写onBind方法，获取binder对象
              Binder binder=new MyAIDL.Stub(){
                  //重写接口里面的方法
                // 处理数据
                 
               }
               
         3.清单文件配置
             设置自己的action ===zyx
       
      客户端:
          
          1.复制服务器AIDL到客户端
          
          2.绑定service，用隐式意图
              Intent intent=new Intent();
              intent.setAction("zyx");
              bindservice(intent,myConn,BIND_AUTO_CREATE);
              
          3.在onServiceConnected方法里面获取AIDL对象
              MyAIDL myAIDL=MyAIDL.Stub.asInterface(service);
              
          4.将数据传入到服务，再获取返回的对象
               String result=myAIDL.getMsg(m);
          
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
