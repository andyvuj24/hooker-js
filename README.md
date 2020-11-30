# Hooker JS [![gitee.png](https://palerock.cn/api-provider/files/view?identity=L2FydGljbGUvaW1hZ2UvMjAyMDA2MjkxNTQyMTMwNzVXcWZyU2dTbC5wbmc=&winghooker-js-js/ ) [![github.png](https://palerock.cn/api-provider/files/view?identity=L2FydGljbGUvaW1hZ2UvMjAyMDA2MjkxNjU3NDkzMDkybWNLRXhHMi5wbmc=&w=15)](https://github.com/cang)

> Used for hijacking methods and performing AOP aspect operations
on targets Based on ES5 syntax for rapid development

----------

## Quick start

```javascript
eHook.hookBefore(window,'alert',function(method,args){
args[0] = args[0] +'[parameter hijacked]';
});
alert('hello eHook'); // hello eHook[parameter hijacked]
```

Global objects: `eHook`, `aHook`

 -`eHook`: Contains basic methods of aop hijacking
 -`aHook`: Contains basic methods for Ajax Url hijacking
 
## Introduction method
1. Download the source code of `hooker.js` or `hooker-mini.js` in the directory `/build`, and then import it through the `<script>` tag, which is a global object: `eHook`, `aHook `
2. Introduced by npm / yarn
```shell script
npm i @palerock/hooker-js
# Or
yarn add @palerock/hooker-js
```
Then import through import or require
```javascript
var hookJS = require('@palerock/hooker-js');
var eHook = hookJS.eHook;
var aHook = hookJS.aHook;
// more...
```
----------

## API documentation
### eHook object
#### `hookBefore`(parent, methodName, before, context)

Execute before specifying method

`parent`:
Type: `object`, required, specify the object where the method is located

`methodName`:
Type: `string`, required, specify the name of the method

`before`:
Type: `function`, required, callback method, the method is executed before the specified method runs
Callback parameters:
-`method`: the specified original method
-`args`: the parameters of the original method (change the parameter value here will affect the parameter value of the subsequent specified method)

`context`:
Type: `object`, optional, context of the callback method

return value:  
Type: `number`, hijack id (used to release hijack)


----------
#### `hookCurrent`(parent, methodName, current, context)
The operation of the hijack method, when the specified method is hijacked, the specified method will not be actively executed, and replaced with the current method in the execution parameter

**Note: This method can only hijack the specified method once. If you use this method to hijack again, it will overwrite the previous hijack [can coexist with hookBefore, hookAfter, and hookBefore and hookAfter can hijack the same specified method multiple times]* *

`parent`:
Type: `object`, required, specify the object where the method is located

`methodName`:
Type: `string`, required, specify the name of the method

`current`:
Type: `function`, required, callback method, which is executed when the specified method is called
Callback parameters and return value:
-`parent`: specify the object where the method is located, type: `object`
-`method`: The specified original method, type: `function`
-`args`: the parameters of the original method, type: `array`
-`Return value`: Specifies the return value of the hijacked method, type: `*`

`context`:
Type: `object`, optional, context of the callback method

return value:  
Type: `number`, hijack id (used to release hijack)

example:  
```javascript
eHook.hookCurrent(Math,'max', function (p, m, args) {
     return m.apply(Math, args) + 1;
});
console.log(Math.max(1, 8)); // 9
```

----------

#### `hookAfter`(parent, methodName, after, context)

Execute after specifying the method

`parent`:
Type: `object`, required, specify the object where the method is located

`methodName`:
Type: `string`, required, specify the name of the method

`after`:
Type: `function`, required, callback method, which is executed after the specified method is run
Callback parameters and return value:
-`method`: The specified original method, type: `function`
-`args`: the parameters of the original method, type: `array`
-`result`: return value of the original method, type: `*`
-`Return value`: Specifies the return value of the hijacked method, type: `*`

`context`:
Type: `object`, optional, context of the callback method

return value:  
Type: `number`, hijack id (used to release hijack)

example:
```javascript
eHook.hookAfter(Math,'max', function (m, args, result) {
     return result + 1;
});
console.log(Math.max(1,8)); // 9
```

----------

#### `hookReplace`(parent, methodName, replace, context)

Hijacking and replacing designated methods

**Note: This method will cover all the previous hijacking of the specified hijacking method, and it cannot be reused, and does not coexist with hookAfter, hookCurrent, and hookBefore. In the case of simultaneous use, hookReplace is preferred over other methods* *

`parent`:
Type: `object`, required, specify the object where the method is located

`methodName`:
Type: `string`, required, specify the name of the method

`replace`:
Type: `function`, required, callback method, the return value of this method is the replacement method
Callback parameters and return value:
-`method`: The specified original method, type: `function`
-`Return value`: Specifies the content of the method to be replaced, type: `function`

`context`:
Type: `object`, optional, context of the callback method

return value:  
Type: `number`, hijack id (used to release hijack)

example:  
```javascript
eHook.hookReplace(Math,'max',function (m) {
     return Math.min;
});
console.log(Math.max(1, 8)); // 1
```
### Document continues to be written...
----------

## Related address
-Gitee address: [https://gitee.com/HGJing/hooker-js](https://gitee.com/HGJing/hooker-js)
-Github address (update the source code first): [https://github.com/canguser/hooker-js](https://github.com/canguser/hooker-js)
-Project homepage: [https://palerock.cn/projects/006HDUuOhBj](https://palerock.cn/projects/006HDUuOhBj)
## If you have any questions, please comment or leave a message
