### 自定义时间日期格式

```javascript
//定义
Date.prototype.format = function(format)
{
    var o =
    {
        "M+" : this.getMonth()+1, //month
        "d+" : this.getDate(),    //day
        "h+" : this.getHours(),   //hour
        "m+" : this.getMinutes(), //minute
        "s+" : this.getSeconds(), //second
        "q+" : Math.floor((this.getMonth()+3)/3),  //quarter
        "S" : this.getMilliseconds() //millisecond
    }
    if(/(y+)/.test(format))
    format=format.replace(RegExp.$1,(this.getFullYear()+"").substr(4 - RegExp.$1.length));
    for(var k in o)
    if(new RegExp("("+ k +")").test(format))
    format = format.replace(RegExp.$1,RegExp.$1.length==1 ? o[k] : ("00"+ o[k]).substr((""+ o[k]).length));
    return format;
}

//使用
var startDate = new Date().format("yyyy/MM/dd");
```

### 金钱的保留两位小数，3位隔开，加“¥”过滤器
```javascript
Vue.filter('myMoney', function (money, point) {
  point = point > 0 && point <= 20 ? point : 2;
  var isNegative = false;
  if (money < 0) {
    money = Math.abs(money);
    isNegative = true;
  }
  money = parseFloat((money + '').replace(/[^\d\.-]/g, '')).toFixed(point) + '';
  var l = money.split('.')[0].split('').reverse();
  var r = money.split('.')[1];
  var result = '';
  for (var i = 0; i < l.length; i++) {
    result += l[i] + ((i + 1) % 3 === 0 && (i + 1) !== l.length ? ',' : '');
  }
  return '¥ ' + ((isNegative ? '-' : '') + result.split('').reverse().join('') + '.' + r);
});
```