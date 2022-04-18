运行时常量池是方法区的一部分，JDK8中使用元空间代理永久代

String::intern()是一个本地方法，如果字符串常量池中已经包含一个等于此String对象的字符串，则返回代表池中这个字符串的对象引用；否则，会将此String对象包含的字符串添加到常量池，返回对象引用



根加载器部署rt.jar时，System的initializeSystemClass中，sun.misc.version会把java字符串加载到常量池

