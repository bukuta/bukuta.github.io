---
layout: default
title: how seajs does
---
1.使用方式/例子
1.1 页面引用jquery,seajs,seajs.config
1.2 页面使用`seajs.use('mod_index',function(){})`启动项目主模块
1.3 假设，mod_index依赖mod_a,mod_b,mod_a依赖mod_b

模块
callback
load
onload
fetch


2.开发模式(未压缩/合并)下加载/运行原理
2.1 `seajs.use('mod_index',function(){});`生成匿名模块，依赖于mod_index,并设置模块的callback,最终load此匿名模块

2.2 module.load过程
2.2.1 module自己只会加载一次
2.2.2 解析所依赖的模块的uris
2.2.3 所依赖子模块是否有未加载的
2.2.3.1 无，执行本模块的onload
2.2.3.2 有，依次加载所依赖的子模块
2.2.3.2.1 生成所有依赖的子模块[初始化](如果此模块已被夹带过来，则load之)
2.2.3.2.2 生成所有依赖的子模块的请求函数体
2.2.3.2.3 执行所有依赖的子模块的请求函数体
2.2.3.2.3.1 请求队列防止重复请求,将模块放入uri的hashmap中(可能多个Mod合并在同一文件下载下来)
2.2.3.2.3.2 请求成功后，请请求队列里的子模块依次弹出触发load,处理队列中相关条目状态
(如，index依赖mod_a,将下载mod_a的源文件，然后执行mod_a的load)
(因为加载的方式为script，mod_a的源文件下载完成即被执行，但是因为define(factory),mod_a的源码被define拦截执行，mod_a处于下载完，但未执行状态，mod_a是否依赖更多模块不得而知,由define完成依赖分析，故需要加载完的子模块执行load,这将依次深入，将所有依赖模块都加载完成,而所有模块都onload完成，才会依次触发上层模块的onload)

2.3 module.onload过程
2.3.1 本模块是否有callback回调(由use生成匿名模块时添加)，则执行此回调
2.3.2 本模块的onload方法执行时，处理依赖于自己的模块.[通知上层]:如,a依赖b,当b触发onload时，将更新a对b的依赖状态，同时检查a是否依赖处理干净，如果处理干净，执行a的onload方法，依次传递至根模块

2.4 seajs.use给匿名模块添加callback
2.4.1 取出本模块(匿名模块)的依赖模块
2.4.2 执行模块,将模块执行结果再传递给上层callback

2.5 模块执行
2.5.1 模块的定义在define`(factory)`中,factory可为函数|js对象|字面量
2.5.2 模块的执行结果为其所导出的接口/数据，被缓存，多次加载/执行模块时，只会真正执行一次
2.5.2.1 如果为函数，以require,exports=module.exports={},module作为参数执行factory,如果有返回值，返回值,返回值为导出接口，如果没有，则以module.exprots为导出接口(模块中兼容return 写法和exports.xx=xxx，或者 define(xxx))
2.5.2.2 如果factory非函数，则将factory作为模块结果返回
2.5.2.2 factory(模块定义)在执行时，内部require引入其他模块的处理:传进去的require函数
(Module.get(id)取得模块并执行，将结果返回给require的调用方)

2.6define干了啥?
2.6.1 拦截模块本身的执行(factory在恰当的时机执行|依赖模块完全加载load完)
2.6.2 检查参数，区分开发环境和合并环境，define(id,deps,factory)|define(id,factory)|define(deps,factory)
2.6.2.1 若无deps,则静态分析factory源码，取出require('xxx/id'),作为本模块的依赖
2.6.3 被define了的模块至少处于saved状态，如果此模块是被'夹带过来'的(请求了a模块，但a的源码文件同时也包含了b的定义，多见于文件合并)


3 发布后

4 Module的状态
 0:INIT 初始化，只有uri，和可能的依赖模块 因为可能先require('xxx'),也可能先define(id,deps,factory)
    new Module(uri,deps)时设置
 1:FETCHING 获取模块源码文件
```javascript
    mod_parent.load(){
      mod_sub.fetch()  
    }
```
 2:SAVED 模块已经过define处理 但依赖模块不确定,叫DEFINED更好？
    define()
    模块可能不经历fetching阶段就直接变为saved
 3:LOADING 让模块可用前提是必须让其依赖可用
    mod.load()
 4:LOADED 所有依赖子模块都已经LOADED，处于可用，可状态
   解决的所有的依赖项后
   分析自己的依赖项，或者子模块LOADED之后通知父模块的检查
 5:EXECUTING 模块被使用时所需进行的执行，模块只执行一次，结果被缓存
   Module.use，置于mod的callback中的mod.exec()
 6:EXECUTED 模块执行过，再次被导入时，将返回缓存的结果
    mod.exec执行之后

5 模块定位
  模块开发时使用的require('mod_a')
  发布后的define('mod_a',['mod_b','mod_c'],factory);
    `Module.get(id)<=Module.resolve(id,parents_uri)<=seajs.resolve(id,uri)=id2Url(id,uri)`
    Module.resolve(id,baseuri)抛出事件，外部可seajs.on('resolve',function(){})拦截修改处理
    `Module.prototype.resolve()=>Module.resolve(id,baseuri)`分析本模块依赖的模块ids=>uris

6 辅助函数
  id2Uri(id,baseuri)
    parseAlias(id)
    parsePatchs(id)
    parseVars(id)
    normalize(id)
    addbase(id,baseuri)
    parseDependencies(factory_code)
      源码正则搜索require('xxx');
