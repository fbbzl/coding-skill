# coding-skill


要使用lombok, 可以参考其他模块的用法

校验用jsr303, message.proerties不要忘记加上对应的异常信息

尽量使用hutool工具类, 如果不满足考虑使用apache, 如果apache也不满足, 再考虑使用google的工具. 如果再没有合适的方法再统一汇报给我

属性映射尽量使用mapstruct

基础校验放在controller中, 业务校验,数据库校验放在service中

要注意数据库事务的使用

业务异常使用Throws工具类和BizException
