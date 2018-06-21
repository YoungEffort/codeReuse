### 判断参数是否是其中之一

```javascript
function oneOf (value, validList) {
    for (let i = 0; i < validList.length; i++) {
        if (value === validList[i]) {
            return true;
        }
    }
    return false;
}
```

### 获取最近几天的日期
```javascript
//获取当天日期  
getDay(0);//当天日期  

//获取最近7天日期  
getDay(-7);//7天前日期  
  
//获取最近3天日期  
getDay(-3);//3天前日期  
  
function getDay(day){    
       var today = new Date();    
           
       var targetday_milliseconds=today.getTime() + 1000*60*60*24*day;            
    
       today.setTime(targetday_milliseconds); //注意，这行是关键代码  
           
       var tYear = today.getFullYear();    
       var tMonth = today.getMonth();    
       var tDate = today.getDate();    
       tMonth = doHandleMonth(tMonth + 1);    
       tDate = doHandleMonth(tDate);    
       return tYear+"-"+tMonth+"-"+tDate;    
}    
function doHandleMonth(month){    
       var m = month;    
       if(month.toString().length == 1){    
          m = "0" + month;    
       }    
       return m;    
} 
``` 