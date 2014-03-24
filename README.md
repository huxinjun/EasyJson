EasyJson快速入门，让你的开发难度降低到0
===========================================================

目前为止最好用的JSON解析工具

为什么要用这个工具的三大理由：
          1.解析速度快，经测试20000行json数据耗时1500ms（fastjson10000行600ms）,反序列化+序列化+反序列化+序列化稳定性测试(1500行)完全通过耗时300ms,保证你的数据的准确性
          2.功能完善，此工具支持
                    （1）将JSON字符串反序列化为JAVABEAN对象
                    （2）支持JAVABEAN对象序列化为JSON
                    （3）支持在JSON字符串中搜索符合您提供的JAVABEAN的集合
                    （4）支持获取JSON字符串中任意节点的属性值
                    （5）支持获取JSON字符串中任意对象下的属性集合
                    （6）获取属性使用路径，简单明了，如/easyjson/function[0]/name
                    （8）jar包超小，只有48kb，并且能在JDK1.5以上的任意开发欢迎中正常运行
         3.有配套的Eclipse插件，提供将工程目录下的JSON文本文件一                      键式转换为.java的JAVABEAN源代码，让你不再忍受字段不同无法解析，JAVA编写繁琐


这么方便的工具赶快走起。。。
下面介绍一下上手流程：

1.定义你的JSON协议，例如
{

	"name":"easyjson",
	"age":18,
	"family":{
                "people_count":3,
                "phone_num";"7789247",
		"mother":{
			"name":"妈妈",
			"age":38
		},
		"father":{
			"name":"妈妈",
			"age":40
		},
		"sister":{
			"name":"姐姐",
			"age":26
		}
	},
	"animal":[
		{
			"name":"猫咪"
		},
		{
			"name":"小狗"
		}
	]
}

2.在项目任何目录下新建一个文本文件，再将上面的JSON字符串拷贝进入，保存，然后右击（前提是您已经安装了EasyJson_JSONToJavaBean插件），选择JSONToJavaBean

3.填写生成JAVABean源码的包名，根据需求选择是否生成注释（如果JSON字符串中对属性使用//进行过注释，生成的JAVABEAN也会有注释哦！），是否生成为内部类，最后填写这个JAVABEAN的名字（java类名），点击确定吧！

4.现在万事俱备，只欠东风啦，您可能需要在服务器将一个JAVABEAN对象序列化为JSON字符串，还可能是在客户端对服务端传递过来的JSON字符串进行反序列化为对象，这是我会告诉你，你只需要知道一个入口类再敲一个点，所有的事情我帮你搞定

5.这个类就是EasyJson,是不是有点熟悉呀！哈哈
      下面介绍一下该类的三个主要的方法：
       （1）getJavaBean(String json,IBeanHandler<T>):T
       （2）getJavaField(String json,IFieldHandler<T>):T
       （3）getJson(Object javaObj) :String
--------------------------------------(1)获取单个Bean-------------------------------------------------------------

     如果你要将一个JSON字符串反序列化为对象，代码如下：
加入刚才定义的JSON生成的JAVABEAN类名为Person

Person p=EasyJson.getJavaBean("你获取到的JSON字符串",new BeanHanlder<Person.class>(Person.class));
   OK！你已经拿到对象了，任由你处置！
--------------------------------------(2)搜寻一批Bean-------------------------------------------------------------

来个高级点的：获取这个JSON中的JAVABEAN集合，咱们就演示一下如果获取Animal集合，可以获取到位于任何层次下的JAVABean集合哦

ArrayList<Animal> animals=EasyJson.getJavaBean("你获取到的JSON字符串",new BeanListHanlder<Person.class>(Person.class));
   拿到了么？

--------------------------------------(3)获取节点属性值-----------------------------------------------------------------

获取属性值需要一个String类型的Path，即路径，比如我们要获取妈妈的年龄，那么Path应该是这样的：/family/mother/age
或者获取数组中的属性：/animal/1/name 或者 /animal[1]/age
知道怎么写Path后那就简单了

Integer age=EasyJson.getJavaField("你获取到的JSON字符串",new FieldHandler<Integer>(Integer.class, path));

---------------------------------(4)获取某个对象下的普通属性Map----------------------------------------------------

先写一个Path,我们就获取一下family下的普通属性吧！

HashMap<String, String> family= EasyJson.getJavaField("你获取到的JSON字符串", new FieldListHandler(path));

你拿到的值就是这样的：
       key            value
people_count :         "3"
phone_num :         "7789247"


好啦，到此结束啦，您学会了吗？如果需要插件，在我的另外一个库中有，名字大概是EasyJson_JSONToJavaBean.zip

祝您开发愉快！！！
