---

typora-copy-images-to: images
---

#如何搭建属于自己的个人博客



##  一、在github上创建一个远程仓库

* **首先要申请一个github账号（注意GitHub是国外，访问需要挂梯子）**

  

* **生成一个ssh公钥**

  

  * 打开Git Bash,输入ssh-keygen -t rsa -C “your_email@example.com”  ，将your_email@example.com替换为你登录github的邮箱，然后一直回车，使用它给的默认值即可

    

  * 继续输入cat ~/.ssh/id_rsa.pub显示公钥，将显示的内容复制下来

    

    ![1657705450004](images\1657705450004.png)

    

    

  *  登录github找到设置->SSH相关设置

    ![1657705666746](images\1657705666746.png)

    

    

  * 新建一个SSH key将复制下来的公钥粘贴到下面区域

    ![1657705773605](images\1657705773605.png)

    

* **新建一个仓库**

  ![1657703709793](images\1657703709793.png)



* **根据图片找到主题设置，点击Chooes a theme，随意设置一个主题，后面我们还会改的**

  ![1657707599820](D:\XCdouya.github.io\docs\BuildPersonalBlog\images\1657707599820.png)





* **设置完后回到代码页面，复制仓库地址，通过git命令克隆到本地**

  ![1657708048836](images\1657708048836.png)

  

  

##  二、初始化docsify

* **在本地仓库中打开cmd然后跟随docsify官方文档初始化**

  ![1657709092343](images\1657709092343.png)

操作完成后，本地仓库目录下会多出一个doxc文件夹

将本地库commit然后push到远端仓库，看到远端仓库也多出来doxc文件夹即可



## 三、将docsify的目录设置为网页根目录

**改变图中选项，然后点Save保存**

![1657709473614](D:\XCdouya.github.io\docs\BuildPersonalBlog\images\1657709473614.png)





##  四、网页的访问

在上图中，绿色区域提供有一个网址，通过该网址即可访问你的个人博客

如果想网址变得简单一点可以将远程仓库重命名为“[username.github.io]()”

其中**username**替换成你的GitHub用户名