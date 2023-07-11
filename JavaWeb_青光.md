# JavaWeb_霞光

## 1 Socket技术

要实现Socket通信，我们必须创建一个数据发送者和一个数据接收者，也就是客户端和服务端，我们需要提前启动服务端，来等待客户端的连接，而客户端只需要随时启动去连接服务端即可！

```java
//服务端
package com.ixuanzi.socket;

import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class Server {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(8080);){

            // 服务端保持开启状态
            while(true){
                Socket accept = serverSocket.accept();
                System.out.println("客户端已连接，IP地址为："+accept.getInetAddress().getHostAddress());
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

```java
//客户端
package com.ixuanzi.socket;

import java.io.IOException;
import java.net.Socket;

public class Client {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 8080)){
            System.out.println("已连接到服务端！");
        }catch (IOException e){
            System.out.println("服务端连接失败！");
            e.printStackTrace();
        }
    }
}
```

实际上它就是一个TCP连接的建立过程：

一旦TCP连接建立，服务端和客户端之间就可以相互发送数据，直到客户端主动关闭连接。当然，服务端不仅仅只可以让一个客户端进行连接，我们可以尝试让服务端一直运行来不断接受客户端的连接：

```java
 try (ServerSocket serverSocket = new ServerSocket(8080);){

            // 服务端保持开启状态
            while(true){
                Socket accept = serverSocket.accept();
                System.out.println("客户端已连接，IP地址为："+accept.getInetAddress().getHostAddress());
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
```

现在我们就可以多次去连接此服务端了。

### 1.1使用Socket传输文件

既然Socket为我们提供了IO流便于数据传输，那么我们就可以轻松地实现文件传输了。

```java
// 服务端
package com.ixuanzi.socket;

import java.io.*;
import java.net.InetAddress;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

public class Server {
    public static void main(String[] args) {
        try {
            ServerSocket serverSocket = new ServerSocket(8080);
            Scanner scanner = new Scanner(System.in);
            // 服务端开启
            Socket socket = serverSocket.accept();
            InputStream stream = socket.getInputStream();
            FileOutputStream fileOutputStream = new FileOutputStream("net/data.txt");

            // 创建字节流读取
            byte[] bytes = new byte[1024];
            int i;
            while((i = stream.read(bytes)) !=-1){
                fileOutputStream.write(bytes, 0, i);
            }

            fileOutputStream.flush();
            serverSocket.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

```java
// 客户端
package com.ixuanzi.socket;

import java.io.*;
import java.net.Socket;


public class Client {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 8080)) {
            FileInputStream fileInputStream = new FileInputStream("test.txt");
            OutputStream stream = socket.getOutputStream();

            byte[] bytes = new byte[1024];
            int i;
            while ((i = fileInputStream.read(bytes)) !=-1){
                stream.write(bytes,0,i);
            }
            fileInputStream.close();
            stream.flush();

        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("连接失败！");

        }
    }
}
```

## 2 数据库