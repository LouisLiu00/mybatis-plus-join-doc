---
tags:
  - 'sub-table-logic'
  - 'logic-del-type'
  - 'logicDelToOn'
  - 'logicDelToWhere'
---

# 逻辑删除


如果使用MP逻辑删除, MPJ默认也会带上逻辑删除  
可以通过以下方式关闭逻辑删除

配置文件全局关闭副表逻辑删除(默认开启)
  ```yaml
  mybatis-plus-join:
    #关闭副表逻辑删除 默认开启(true)
    sub-table-logic: false
  ```
关闭本次查询副表逻辑删除
  ```java
  wrapper.disableSubLogicDel()
  ```
关闭本次查询主表逻辑删除
  ```java
  wrapper.disableLogicDel()
  ```


## 改变逻辑删除条件位置

全局配置副表逻辑删除条件的位置（1.4.4+）
  ```yaml
    mybatis-plus-join:
      #副表逻辑删除条件的位置 支持 where / on  
      #默认 on （1.4.8+） 
      #注意：1.4.7.2及之前版本默认为where
      logic-del-type: on
  ```
本次查询所有副表逻辑删除条件位置为 on 语句（1.4.4+）
  ```java
    wrapper.logicDelToOn()
  ```
本次查询所有副表逻辑删除条件位置为 where 语句（1.4.4+）
  ```java
    wrapper.logicDelToWhere()
  ```

## 自定义逻辑删除位置

如果既需要where又需要on可以关闭附表逻辑删除，自定义条件实现
```java
List<Map<String, Object>> list = userMapper.selectJoinMaps(JoinWrappers.lambda(UserDO.class)
        .selectAll(UserDO.class)
        //关闭本次查询副表逻辑删除
        .disableSubLogicDel()
        .leftJoin(AddressDO.class, on -> on
                .eq(AddressDO::getUserId, UserDO::getId)
                //手动添加 ON 语句逻辑删除
                .eq(AddressDO::getDel, "未删除标识"))
        //手动添加 where 语句逻辑删除
        .eq(UserDO::getDel, "未删除标识"));
```
