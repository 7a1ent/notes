# JavaWeb_2



### Ajax(异步的 JS & XML)

#### 作用：

- 数据交换：通过 ***Ajax*** 给服务器发送请求，并获取服务器响应的数据
- 异步交互：可以在**不重新加载整个页面**的情况下，与服务器端**交换数据并更新部分网页**的技术（例如在搜索时，在页面没有刷新的情况下自动更新下拉的搜索索引）
- ***同步和异步***
  - 同步：客户端在访问请求服务器，服务器处理数据时，客户端**需要等待服务器响应**然后再继续访问
  - 异步：服务器端在处理逻辑处理时，客户端**仍然可以执行其他操作**



#### 原生Ajax操作步骤(较为繁琐并存在某些兼容性问题)![image-20240312113326872](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240312113326872.png)



#### Axios（对于原生Ajax进行封装，提高效率）

- **入门操作**![image-20240312150931163](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240312150931163.png)

- **便捷操作**![image-20240312151126852](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240312151126852.png)

- **基于Vue和Axios实现页面数据动态加载**

  **即在Vue中的八个生命周期中（monuted中）编写Axios操作，在其中发送异步请求加载数据，再通过遍历将数据赋值渲染展示**

​	 	
