## angular表单概览
### 响应式表单
我们先从一个实际的例子开始
```
//ts
name = new FormControl('');
//html
<input [formControl]="name">
```
让我们想想，当FooComponent组件创建的时候，发生了什么？  
首先注入器创建了FooComponent实例，随着FooComponent的创建，名为name的FormControl实例也创建了，然后，angular读取模板中的input element的[formControl]属性，创建了一个formControl指令，并把FooComponent中名为name的属性传给了formControl指令。  
**我们创建了一个formControl指令**。  
那创建formControl指令的时候发生了什么？让我们进一步看看。

可以看到，formControlDirective指令注入了一组ControlValueAccessor。  
同时formControlDirective指令接收了传入的formControl属性。  
就像下图一样。

然后组件生命周期钩子ngOnChanges方法开始执行,主要做了两件事
```
setUpControl(this.form, this);
this.form.updateValueAndValidity({emitEvent: false});
```
### setUpControl(this.form, this);
我们通过valueAccessor写入了formControl实例的初始值。
在setUpViewChangePipeline函数里我们给valueAccessor设置了registerOnChange方法。  
在setUpBlurPipeline函数里我们给valueAccessor设置了registerOnTouched方法。

这就是formControl指令创建时做的所有事。
