这是本人在阅读教程和平时问题的随笔记录
==
1. java数组复制

在java中有[四种](https://blog.csdn.net/tingzhiyi/article/details/52344845)拷贝数组方式
其中速度最快的是System.arraycopy，因为他是使用C语言已经编译好的本地方法，其次是Object.clone方式，这种需要实现Cloneable接口，并且他们的复制都是属于*浅复制*有浅肯定有深拉 他两二者在复制对象属性的问题下有所[区别](https://www.cnblogs.com/nickhan/p/8569329.html)，如A类持有B类的对象做为成员变量，在复制A类的过程中，如果新A类中的B类对象还是指向原A类对象中B类对象。那么便是浅复制，否则便属于深复制，即新A类中的B类成员变量的内存地址与原A类中的B类对象不一致。
速度差距的原因，
1. 是否是C实现的本地方法,System.clone
2. 在复制过程中是否需要强转，Object.clone
3. 是否需要判断复制时的长度。如Arrays.copy



