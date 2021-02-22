---
title: Idea生成entity实体类
date: 2021-02-20 08:45:30
tags:
	- Idea
	- Java	
	- Entity
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220084833.jpg
---

Java中如果数据表字段多要手写实体类是件非常麻烦的事情，

所以下面将为大家讲解怎么利用Intellij开发工具来自动生成实体类，生成后实体类如下图。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220084905.png)
直接上步骤

# 一、设置MySQL时区

如果已经设置了的可以直接跳过这一步，直接去下一步

> MySQL数据库创建后。默认的时区比东八区少了八个小时。如果sql语句中使用到MySQL的时间的话就会比正常时间少了八个小时。所以需要修改SQL的系统时区。

1、先确定自己MySQL环境变量是否配置好了(如下图)。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220084922.png)
配置完环境变量，就可以继续下面的操作了。

2、进入命令窗口（Win + R），连接数据库，输入 `mysql -hlocalhost -uroot -p` 后回车输入密码 (如下图)。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220084936.png)
3、继续输入命令 `show variables like'%time_zone';` (如下图)。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220084943.png)
显示 SYSTEM 就是没有设置时区。

4、接下来我们来设置时区，输入`set global time_zone = '+8:00';` (如下图)。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220084950.png)
这样就是设置成功了。

如果保险一点我们可以重启命令窗口重复上面的 2 和 3 的步骤(如下图)，便没有一点问题了。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085000.png)

# 二、项目添加数据库视图

1、打开Intellij IDEA开发工具，再打开数据库视图(如下图)。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085006.png)
打开后右侧会弹出一个窗口(如下图)。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085015.png)
如果没有自动弹出，可以点击 Database 弹出。

2、新建一个MySQL(如下图)。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085023.png)
3、配置MySQL，同步MySQL数据驱动。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085030.png)
我本机安装的MySQL版本是5.5.28的，Intellij IDEA要连接MySQL也要匹配下驱动版本。

Driver选择MySQL for 5.1也是可以的。

4、点击 Test Connection 按钮测试(如下图)。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085037.png)
如图就成功啦！点击OK就已经是连接了MySQL。

当然，如果你是第一次点击 Test Connection 是需要下载插件的，插件下载完成后也会如图一样的，

（这里因为笔者是已经下载过了的，所以没有这个步骤的图片）。

**补充**

如果在匹配驱动版本的时候你选的是MySQL![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330123828386.png)

数据库又是其他版本，也没关系。

在左侧驱动列表里找到 MySQL ，右边Driver files 里，选择一下你需要的版本，保存就可以了。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085044.png)

# 三、生成实体类

1、先打开数据库视图，**选择要生成的数据表，右键选择生成。**
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085054.png)
2、弹出窗口选择实体类的存放文件夹位置。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085101.png)

# 四、自定义生成脚本文件

生成实体类命令 Generate POJOs.groovy 是执行了一个脚本。

为什么要自定义一个脚本文件呢？

是因为原本默认的脚本生成的没有注解，以及注释等等我们想要的信息。 所以我们要自定义一个脚本文件。

1、我们先来新建一个脚本文件，选择 Go to Scripts Directory (如下图)：
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085111.png)
2、自动跳转到 Extensions目录上，在schema文件夹下新建一个 .groovy 后缀的文件（当然这里你也可以直接修改Generate POJOs.groovy文件）。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085120.png)
3、新建脚本文件后，打开文件将如下内容拷贝到该文件。

```java
import com.intellij.database.model.DasTable
import com.intellij.database.model.ObjectKind
import com.intellij.database.util.Case
import com.intellij.database.util.DasUtil
import java.io.*
import java.text.SimpleDateFormat


/*
* Available context bindings:
*   SELECTION   Iterable<DasObject>
*   PROJECT     project
*   FILES       files helper
*/
packageName = ""
typeMapping = [
        (~/(?i)tinyint|smallint|mediumint|bigint/)      : "Long",
        (~/(?i)int/)                                    : "Integer",
        (~/(?i)bool|bit/)                               : "Boolean",
        (~/(?i)float|double|decimal|real/)              : "Double",
        (~/(?i)datetime|timestamp|date|time/)           : "Timestamp",
        (~/(?i)blob|binary|bfile|clob|raw|image/)       : "InputStream",
        (~/(?i)/)                                       : "String"
]




FILES.chooseDirectoryAndSave("Choose directory", "Choose where to store generated files") { dir ->
    SELECTION.filter { it instanceof DasTable && it.getKind() == ObjectKind.TABLE }.each { generate(it, dir) }
}


def generate(table, dir) {
    //def className = javaClassName(table.getName(), true)
    def className = javaClassName(table.getName(), true)
    def fields = calcFields(table)
    packageName = getPackageName(dir)
    PrintWriter printWriter = new PrintWriter(new OutputStreamWriter(new FileOutputStream(new File(dir, className + ".java")), "UTF-8"))
    printWriter.withPrintWriter {out -> generate(out, className, fields,table)}


//    new File(dir, className + ".java").withPrintWriter { out -> generate(out, className, fields,table) }
}


// 获取包所在文件夹路径
def getPackageName(dir) {
    return dir.toString().replaceAll("\\\\", ".").replaceAll("/", ".").replaceAll("^.*src(\\.main\\.java\\.)?", "") + ";"
}


def generate(out, className, fields,table) {
    def tableName = table.getName()
    out.println "package $packageName"
    out.println ""
    out.println "import lombok.NoArgsConstructor;"
    out.println "import lombok.AllArgsConstructor;"
    out.println "import lombok.Data;"


    Set types = new HashSet()


    fields.each() {
        types.add(it.type)
    }


    if (types.contains("Date")) {
        out.println "import java.util.Date;"
    }


    if (types.contains("InputStream")) {
        out.println "import java.io.InputStream;"
    }
    out.println ""
    out.println "/**\n" +
            " * @author  azha \n" +
            " * @create "+ new SimpleDateFormat("yyyy-MM-dd HH:mm").format(new Date()) + " \n" +
            " */"
    out.println ""
    out.println "@Data"
    out.println "@AllArgsConstructor"
    out.println "@NoArgsConstructor"
    out.println "public class $className {"


    fields.each() {
        out.println ""
        // 输出注释
        //if (isNotEmpty(it.comment)) {
        out.println "\t/**"
        out.println "\t * "+"table name:"+"${it.olderName}"
        out.println "\t * "+"table type:"+"${it.olderType}"
        out.println "\t * "+"table comment:"+"${it.comment.toString()}"
        out.println "\t */"
        //}else{
        //out.println "\t/**"
        //out.println "\t * "+"table name:"+"${it.olderName}"
        //out.println "\t * "+"table type:"+"${it.olderType}"
        //out.println "\t * "+"table comment: null"
        //out.println "\t */"
        //}

        // 输出成员变量
        out.println "\tprivate ${it.type} ${it.name};"
    }

    out.println ""
    out.println "}"
}


def calcFields(table) {
    DasUtil.getColumns(table).reduce([]) { fields, col ->
        def spec = Case.LOWER.apply(col.getDataType().getSpecification())


        def typeStr = typeMapping.find { p, t -> p.matcher(spec).find() }.value
        def comm =[
                colName : col.getName(),
                name :  javaName(col.getName(), false),
                olderName: col.getName(),
                type : typeStr,
                olderType : col.getDataType().getSpecification(),
                comment: col.getComment()]
        fields += [comm]
    }
}


// 处理类名（这里是因为我的表都是以t_命名的，所以需要处理去掉生成类名时的开头的T，
// 如果你需要那么请查找用到了 javaName 这个方法的地方修改为 javaClassName即可）
def javaClassName(str, capitalize) {
    def s = com.intellij.psi.codeStyle.NameUtil.splitNameIntoWords(str)
            .collect { Case.LOWER.apply(it).capitalize() }
            .join("")
            .replaceAll(/[^\p{javaJavaIdentifierPart}[_]]/, "_")
    // 去除开头的T
    s = s[1..s.size() - 1]
    capitalize || s.length() == 1? s : Case.LOWER.apply(s[0]) + s[1..-1]
}


def javaName(str, capitalize) {
//    def s = str.split(/(?<=[^\p{IsLetter}])/).collect { Case.LOWER.apply(it).capitalize() }
//            .join("").replaceAll(/[^\p{javaJavaIdentifierPart}]/, "_")
//    capitalize || s.length() == 1? s : Case.LOWER.apply(s[0]) + s[1..-1]
    def s = com.intellij.psi.codeStyle.NameUtil.splitNameIntoWords(str)
            .collect { Case.LOWER.apply(it).capitalize() }
            .join("")
            .replaceAll(/[^\p{javaJavaIdentifierPart}[_]]/, "_")
    capitalize || s.length() == 1? s : Case.LOWER.apply(s[0]) + s[1..-1]
}


def isNotEmpty(content) {
    return content != null && content.toString().trim().length() > 0
}


static String changeStyle(String str, boolean toCamel){
    if(!str || str.size() <= 1)
        return str


    if(toCamel){
        String r = str.toLowerCase().split('_').collect{cc -> Case.LOWER.apply(cc).capitalize()}.join('')
        return r[0].toLowerCase() + r[1..-1]
    }else{
        str = str[0].toLowerCase() + str[1..-1]
        return str.collect{cc -> ((char)cc).isUpperCase() ? '_' + cc.toLowerCase() : cc}.join('')
    }
}


static String genSerialID()
{
    return "\tprivate static final long serialVersionUID =  "+Math.abs(new Random().nextLong())+"L;"
}

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183
```

4、保存文件后，再次打开数据库视图选择新建的 Generate MyPOJOs.groovy 脚本文件，生成实体类。
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220085131.png)
5、pom.xml中导入 lombok 依赖。

```xml
	<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.16.22</version>
      <scope>provided</scope>
    </dependency>
1234567
```

技术分享就到这里啦！感谢各位大佬的阅读🥰