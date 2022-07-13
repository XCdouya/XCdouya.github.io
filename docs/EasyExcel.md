# 关于EasyExcel的使用



##  EasyExcel的介绍

  EasyExcel是一个基于Java的简单、省内存的读写Excel的开源项目。

  针对不同的数量级它提供有不同的读写方法，具体可以参考《[EasyExcel官方文档](https://easyexcel.opensource.alibaba.com/docs/current/)》



  

##  简单的写

**我们先来做一个简单的写**



### 第一步：导入相关依赖

~~~java
<dependencies>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>easyexcel</artifactId>
            <version>3.1.1</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>RELEASE</version>
            <scope>compile</scope>
        </dependency>
    </dependencies>
~~~

将这段代码复制到pom.xml文件中即可

如果项目中没有pom.xml文件可以参考《[将普通项目转换成Maven项目](https://blog.csdn.net/qq_50598935/article/details/123028620)》



### 第二步：创建需要写入数据的实体类

~~~java
@Setter
@Getter
@EqualsAndHashCode
public class Student {
    @ExcelProperty("学生姓名")
    private String name;
    @ExcelProperty("学生年龄")
    private int age;
    @ExcelProperty("学生性别")
    private String sex;
}
~~~



* 其中@Setter和@Getter是不是有点眼熟，这两个注解就是用来生成Setter和Getter方法的
* @EqualsAndHashCode用于生成equals()方法和hashCode()方法
* 至于为什么要生成equals()方法和hashCode()方法可以参考《[java中==,equals,hashcode](https://www.jianshu.com/p/dc495f9130dc)》
* @ExcelProperty("学生姓名")注解将学生姓名作为我们写入表的表头



### 第三步：写入数据

~~~java
@Ignore
public class WriteTest {

    /**
     * 最简单的写
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link Student}
     * <p>
     * 2. 直接写即可
     *
     */
    @Test
    public void simpleWrite(Student[] students) {
        // 注意 simpleWrite在数据量不大的情况下可以使用（5000以内，具体也要看实际情况）
      
        //在项目的输出路径下创建一个名为：“Student+系统当前毫秒数” 的.xlsx文件，用于存储写入的数据
        String fileName = TestFileUtil.getPath() + "Student" + System.currentTimeMillis() + ".xlsx";

        /**
        *write的两个参数：
        *fileName,就是你要写入的文件名
        *Student.class就是写入的数据是以哪个类做模板的
        */
        EasyExcel.write(fileName, Student.class)
                //设置写入第一个sheet,并且将sheet命名为“模板”
                .sheet("模板")
                //写入数据，具体逻辑我不太清楚
                .doWrite(() -> {
                    // 分页查询数据
                    //return后面写list集合，
                    //由于我开始创建的学生集合存储在数组中
                    //所以我simpleWrite方法的传参设置为Student[]
                    //return时调用了下面的data方法将数组转化为list集合
                    //数据如果储存在list集合里可以用list集合作为simpleWrite方法的参数传进来return
                    return data(students);
                });
        
        //下面是官方提供的两种不同方法

//        // 写法2
//        fileName = TestFileUtil.getPath() + "simpleWrite" + System.currentTimeMillis() + ".xlsx";
//        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
//        // 如果这里想使用03 则 传入excelType参数即可
//        EasyExcel.write(fileName, DemoData.class).sheet("模板").doWrite(data());
//
//        // 写法3
//        fileName = TestFileUtil.getPath() + "simpleWrite" + System.currentTimeMillis() + ".xlsx";
//        // 这里 需要指定写用哪个class去写
//        try (ExcelWriter excelWriter = EasyExcel.write(fileName, DemoData.class).build()) {
//            WriteSheet writeSheet = EasyExcel.writerSheet("模板").build();
//            excelWriter.write(data(), writeSheet);
//        }

    }


//创建一个list集合作为写入的数据
    private List<Student> data() {
        List<Student> list = ListUtils.newArrayList();
        for (int i = 0; i < 10; i++) {
            Student data = new Student();
            data.setName("字符串" + i);
            data.setAge(i);
            data.setSex("male");
            list.add(data);
        }
        return list;
    }

   
    //将数组转化为Arraylist集合
    private List<Student> data(Student[] students){
        List<Student> list = ListUtils.newArrayList();
        for (Student stu:students) {
            list.add(stu);
        }
        return list;
    }

}
~~~



* 这里我用的是《[EasyExcel官方文档](https://easyexcel.opensource.alibaba.com/docs/current/)》中的代码,然后根据自己的代码做了一些相应的改动



到此为止一个简单的写就完了啦，是不是很简单呢，其实官方还提供了对于大数据量的读写法，感兴趣可以去《[EasyExcel的github源码](https://github.com/alibaba/easyexcel)》上查看



##我所遇到的问题

### 问题一：TestFileUtil找不到

这个问题我在网上找了好多资料，开始我以为是依赖中缺少的文件，后面询问老师才知道这是一个工具类，

于是我从github上扒下了它的源码：《[EasyExcel的github源码](https://github.com/alibaba/easyexcel)》

~~~java
import java.io.File;
import java.io.InputStream;

public class TestFileUtil {
    public static InputStream getResourcesFileInputStream(String fileName) {
        return Thread.currentThread().getContextClassLoader().getResourceAsStream("" + fileName);
    }

    public static String getPath() {
        return TestFileUtil.class.getResource("/").getPath();
    }

    public static File createNewFile(String pathName) {
        File file = new File(getPath() + pathName);
        if (file.exists()) {
            file.delete();
        } else {
            if (!file.getParentFile().exists()) {
                file.getParentFile().mkdirs();
            }
        }
        return file;
    }

    public static File readFile(String pathName) {
        return new File(getPath() + pathName);
    }

    public static File readUserHomeFile(String pathName) {
        return new File(System.getProperty("user.home") + File.separator + pathName);
    }
}
~~~



