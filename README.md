#流程状态机

###这是什么?
这是一个通过状态来控制流程的模型

###有什么用?
举一个简单的例子:
>我点击一个按钮，点击第一次显示1，点击第二次显示'hello world'，点击第3次显示'你好';
一个简单的程序如下:
    
    var sign = 0;
    $(btn).click(function(){
      if ( sign === 0 ) {
        alert(1);
      } else if ( sign === 1 ) {
        alert('hello world');
      } else if ( sign === 2 ) {
        alert('你好');
      }
      if ( ++sign > 2 ) {
        sign = 0;
      }
    });
    
虽然例子很简单，但是却反映出日常开发中很多时候我们需要用`信号量`来判断状态，这在状态很多的时候会很繁琐，所以...流程状态机就产生了。

###用法
是的，这个模型还只是实现了基本的功能，那么我们可以通过一个例子来看看它的用法
    
    //用户会议状态模型
    var Say = new StateMachie({
      trigger: $(btn)
    });

    //获得信息状态
    Say.add('say-one', function sayOne() {
      alert(1);
      this.active('say-helloworld');
    });

    //显示信息状态
    Say.add('say-helloworld', function sayHelloWorld() {
      alert('hello world');
      this.active('say-hello');
    });

    //隐藏信息状态
    Say.add('say-hello', function sayHello() {
      alert('你好');
      this.active('say-one');
    });

    //设置初始状态
    Say.active('say-one');

    //触发状态
    Say.trigger.on('click', $.proxy(function(){
      this.exec();
    }, Say));
这就是通过状态来控制流程的状态机，在更复杂的应用中它会很好的完成任务，具体更多的用法参见手册

###手册
`new StateMachine({})`:构造函数可把参数对象属性复制到实例中;

`.empty()`:清空所有状态;

`.goto('state-name')`:直接跳到此状态;

`.add('state-name', callback)`:添加状态;

`.active('state-name')`:执行状态;

`.exec()`:执行状态;
  
