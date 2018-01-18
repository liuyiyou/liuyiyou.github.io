title:  velocity的List/Map/数组
date: 2015-10-01
categories : 
  - velocity
tags : 
  - velocity
---

这个用到得多，但是很少去记住，在这里记录一下，至少要用到的时候，来这里找

## 遍历个map类型

```html

    #foreach($param in ${paramValues.keySet()})  
    <tr>  
        <th>$param</th>  
        <td>${paramValues.get($param)}</td>  
   </tr>  
    #end  


     #foreach($map in $!scoreQuota.scoreQuotaDto.rule_map.entrySet())
           <label>
               <input name="form-field-checkbox" type="checkbox" class="ace">
               <span class="lbl"> $map.key - $map.value</span>
           </label>
    #end


```

## 遍历List类型


```html

    #foreach($sal in ${salerList})  
    $sal.name  
    #end  
```

## 遍历数组

和List一样。关键是取下标: ${line[0]}

```html

  #foreach($value in ${line})
    $value
    ${line[0]}
   #end


```





