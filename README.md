# coding-skill

0 jdk8到jdk21的所有新特性,尽量激进的使用
1 要使用lombok, 可以参考其他模块的用法. 
2 校验用jsr303, message.proerties不要忘记加上对应的异常信息
3 尽量只使用hutool工具类, 如果hutool中没有合适的工具,请立即汇报给我
4 属性映射尽量使用mapstruct
5 基础校验放在controller中, 业务校验,数据库校验放在service中
6 要注意数据库事务的使用 
7 业务异常使用Throws工具类和BizException
8 @Bean注解尽量使用方法名作为bean的名称
9 禁止使用var来声明变量