title:  MyBatis通用dao和通用service
date: 2015-04-25
categories :
  - mybatis
tags :
  - mybatis
---


使用通用dao和通用service可以减少代码的开发。可以将常用的增删改查放到通用dao中。对不同的or框架，基本上都有自己的实现如SpringJPA的Repository就提供了常用的增删改查方法。而MyBatis借助代码生成工具也可以生成常用方法的映射

这里只针对Mybatis。如果使用代码生成工具，会有一个问题：每个Mapper里面都有一些方法（增晒改查）。维护起来的话还好。只是在写service的时候会有一个问题。


比如UserMapper里面有

```java
insert(User user)
find(Integer id)
delete(Integer id)

```

等方法，则在service中也要有这些方法的实现。假设每个Mapper有5个方法。则service也需要有5个方法的实现。如果有10个实体类。mapper可以省略（由生成工具生成），但是service有50个方法。到后期肯定不好进行维护


-- 更新于2015/01/29，增加了第三种方法

-- 更新于2015/02/09，第三种方法方法改进，service实现上不需要加泛型


## 使用通用Mapper和Service

该通用Mapper使用了Spring-mybatis。所以没有实现类，而是直接调用xml文件中的同名方法。之所以将通用Mapper抽出来主要是为了方便些通用的service

## 第一种：需要三个泛型，当前实体对象，主键，映射对象，

好处：具体Service实现类简单，如果自己没有特殊的方法，继承BaseServiceImpl之后不需要写任何东西

不好之处：

1. 每个通用方法都需要传递映射参数：比如findById，如果是UserService调用，则需要这样写：userService.findById(1,UserMapper.class)，这样在BaseMapperImpl中才可以通过class.getName得到映射的名称

2. 而且在BaseMapperImpl中写得比较死。修改起来比较复杂

## 第二种：
第一：不需要在方法上写各种泛型参数，同时也不需要在各种方法中传递映射参数

第二：通用dao也不需要写实现：mybatis-spring所倡导的的一致

不好之处：

需要在每个Service实现类中写上：


```java

    @Autowired
    public void setBaseMapper(UserMapper userMapper){

        super.setBaseMapper(userMapper);
    }
```

而且关键的是@Autowired不能少。这个在开发过程中不好进行限制

### 具体代码

1. 通用接口，该接口方法的名称与使用代码生成工具的名称完全相同


```java

package cn.liuyiyou.yishop.mapper;

import java.io.Serializable;

public interface BaseMapper<T,ID extends Serializable> {

    int deleteByPrimaryKey(ID id);

    int insert(T record);

    int insertSelective(T record);

    T selectByPrimaryKey(ID id);

    int updateByPrimaryKeySelective(T record);

    int updateByPrimaryKeyWithBLOBs(T record);

    int updateByPrimaryKey(T record);
}



```



2. 通用service接口。为了方便，名字和通用Mapper同名，其实更倾向于命名add。edit，find这类的命名，显得更加面向对象


```java

package cn.liuyiyou.yishop.service;

import java.io.Serializable;

public interface BaseService<T,ID extends Serializable> {

    void setBaseMapper();

    int deleteByPrimaryKey(ID id);

    int insert(T record);

    int insertSelective(T record);

    T selectByPrimaryKey(ID id);

    int updateByPrimaryKeySelective(T record);

    int updateByPrimaryKeyWithBLOBs(T record);

    int updateByPrimaryKey(T record);


}



```


3. 通用service实现。也很简单。就是调用通用mapper里面的方法即可


```java

package cn.liuyiyou.yishop.service.impl;

import java.io.Serializable;

import cn.liuyiyou.yishop.mapper.BaseMapper;
import cn.liuyiyou.yishop.service.BaseService;

public abstract  class AbstractService<T, ID extends Serializable> implements BaseService<T, ID> {

    private BaseMapper<T, ID> baseMapper;

    public void setBaseMapper(BaseMapper<T, ID> baseMapper) {
        this.baseMapper = baseMapper;
    }

    @Override
    public int deleteByPrimaryKey(ID id) {
        return baseMapper.deleteByPrimaryKey(id);
    }

    @Override
    public int insertSelective(T record) {
        return baseMapper.insertSelective(record);
    }

    @Override
    public T selectByPrimaryKey(ID id) {
        return baseMapper.selectByPrimaryKey(id);
    }

    @Override
    public int updateByPrimaryKeySelective(T record) {
        return baseMapper.updateByPrimaryKey(record);
    }

    @Override
    public int updateByPrimaryKeyWithBLOBs(T record) {
        return baseMapper.updateByPrimaryKeyWithBLOBs(record);
    }

    @Override
    public int updateByPrimaryKey(T record) {
        return baseMapper.updateByPrimaryKey(record);
    }


    @Override
    public int insert(T record) {
        return baseMapper.insert(record);
    }
}



```


4. 具体的Mapper写法:


```java

package cn.liuyiyou.yishop.mapper;

import java.util.List;

import org.springframework.stereotype.Repository;

import cn.liuyiyou.yishop.domain.ProductCategory;

@Repository
public interface ProductCategoryMapper extends BaseMapper<ProductCategory, Long>{

    /**
     * 通过父id得到类目列表
     * @param praentId
     * @return
     */
    List<ProductCategory> selectByParent(Long parent);
}

```


5. 具体的Servicd


```java


package cn.liuyiyou.yishop.service.impl;

import java.util.HashMap;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import cn.liuyiyou.yishop.domain.Ad;
import cn.liuyiyou.yishop.domain.Product;
import cn.liuyiyou.yishop.mapper.AdMapper;
import cn.liuyiyou.yishop.mapper.ProductMapper;
import cn.liuyiyou.yishop.service.ProductService;

@Service
public class ProductServiceImpl extends AbstractService<Product,Long> implements ProductService {


    @Autowired
    private ProductMapper productMapper;

    @Autowired
    private AdMapper adMapper;

    //这句必须要加上。不然会报空指针异常，因为在实际掉用的时候不是BaseMapper调用，而是这个productMapper
    @Autowired
    public void setBaseMapper(){
        super.setBaseMapper(productMapper);
    }

    @Override
    public Map<String,Object> testTwo() {
        Product product = productMapper.selectByPrimaryKey(1l);
        Ad ad = adMapper.selectByPrimaryKey(1l);

        Map<String,Object> map = new HashMap<String,Object>();
        map.put("product", product);
        map.put("ad", ad);
        return map;
    }




}


```


## 第三种：更新于2015/01/31，增加了第三种方法
这一种算是第二种的改进。避免了在ServiceImpl中写@Autowired

去掉，privste BaseMapper<T,ID> baseMapper。


```java


package cn.liuyiyou.yishop.service.impl;

import java.io.Serializable;

import cn.liuyiyou.yishop.mapper.BaseMapper;
import cn.liuyiyou.yishop.service.BaseService;

public abstract class AbstractService<T, ID extends Serializable> implements BaseService<T, ID>{

    @Override
    public abstract BaseMapper<T, ID> getMapper() ;

    @Override
    public int deleteByPrimaryKey(ID id) {
        return getMapper().deleteByPrimaryKey(id);
    }

    @Override
    public int insert(T record) {
        return getMapper().insert(record);
    }

    @Override
    public int insertSelective(T record) {
        return getMapper().insertSelective(record);
    }

    @Override
    public T selectByPrimaryKey(ID id) {
        return getMapper().selectByPrimaryKey(id);
    }

    @Override
    public int updateByPrimaryKeySelective(T record) {
        return getMapper().updateByPrimaryKeySelective(record);
    }

    @Override
    public int updateByPrimaryKeyWithBLOBs(T record) {
        return getMapper().updateByPrimaryKeyWithBLOBs(record);
    }

    @Override
    public int updateByPrimaryKey(T record) {
        return getMapper().updateByPrimaryKey(record);
    }

}




```


具体的Service实现。在这个里面，通过抽象的Service，来强制具体Service实现getMapper方法，而不需要在该方法上加@Autowired。


```java


package cn.liuyiyou.yishop.service.impl;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import cn.liuyiyou.yishop.domain.Ad;
import cn.liuyiyou.yishop.mapper.AdMapper;
import cn.liuyiyou.yishop.mapper.BaseMapper;
import cn.liuyiyou.yishop.service.AdService;
import cn.liuyiyou.yishop.service.BaseService;

@Service
public class AdServiceImpl extends AbstractService<Ad,Long> implements AdService {


    @Autowired
    public AdMapper adMapper;

    @Override
    public BaseMapper<Ad, Long> getMapper() {
        return adMapper;
    }

}

```


## 最近更新于2015/02/09，第三种方法方法改进，service实现上不需要加泛型

之所以记录下来的原因是因为，在service接口上没有使用泛型


```java


package cn.liuyiyou.springmvc.mapper;

import java.io.Serializable;

/**
 * User: liuyiyou
 * Date: 14-12-27
 * Time: 下午11:39
 */
public interface BaseMapper<T,ID extends Serializable> {

    //只需要这个就可以了，并不需要具体的实现，只需要在Service中设置BaseMapper
    T findById(ID id);

    Integer insert(T t);
}



package cn.liuyiyou.springmvc.service;

import java.io.Serializable;

/**
 * User: liuyiyou
 * Date: 14-12-27
 * Time: 下午11:53
 */
public interface BaseService <T,ID extends Serializable>{

    T findById(ID id);

    int insert(T t);
}


package cn.liuyiyou.springmvc.service;

import cn.liuyiyou.springmvc.mapper.BaseMapper;

import java.io.Serializable;

/**
 * User: liuyiyou
 * Date: 14-12-27
 * Time: 下午11:54
 */
public abstract class BaseServiceImpl<T,ID extends Serializable> implements BaseService<T,ID> {

   public abstract  BaseMapper getMapper();

    @Override
    public T findById(ID id) {
        return (T) getMapper().findById(id);
    }

    @Override
    public int insert(T t) {
        return getMapper().insert(t);
    }
}


package cn.liuyiyou.springmvc.service;

import cn.liuyiyou.springmvc.domain.User;

public interface UserService extends BaseService {

    boolean addUser(User user);

}




package cn.liuyiyou.springmvc.service;

import cn.liuyiyou.springmvc.mapper.BaseMapper;
import cn.liuyiyou.springmvc.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import cn.liuyiyou.springmvc.domain.User;

@Service
@Transactional(value="transactionManager")
public class UserServiceImpl extends BaseServiceImpl  implements UserService{

    @Autowired
    private UserMapper userMapper;


    @Override
    public BaseMapper getMapper(){
        return userMapper;
    }

    public boolean addUser(User user) {
        int reuslt = userMapper.insert(user);
        if (reuslt>0)
            return true;
        return    false;
    }


}




```



## 资料
http://liuyiyou.cn/2014/12/28/base-dao-service/  --这个好像有提到好坏

http://git.oschina.net/free/Mapper/blob/master/UseMapperInSpring4.md --这个看了一下，但是没有进行试验，不知道好坏与否
