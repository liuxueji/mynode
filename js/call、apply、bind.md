语法：函数.call()、函数.apply()、函数.bind()

共同点：都能改变this指向

不同点：

- call、apply是立即执行。bind不能理解执行，因为bind返回的是一个函数，需要添加()
- 传入参数不同：call、bind传入多个参数。apply传入数组

应用场景：

1. 用apply的情况

   ```js
   var arr = [1,2,3,4,5]
   console.log(Math.max(arr)) // NaN
   ```

   > 这种情况，Math.max只能传入多个参数，如果是数组的话则不能传入，此时用apply解决

   ```js
   var arr = [1, 2, 3, 4, 5];
   console.log(Math.max.apply(null, arr)); // 5
   ```

2. 用bind的情况

   ```js
   <button id='btn'>点击</button>
   <h1 id='h1'>aaa</h1>
   
   let btn = document.getElementById('btn')
   let h1 = document.getElementById('h1')
   btn.onclick = function(){
   	console.log(this.id)
   }
   ```

   > 此时点击按钮，输出btn的id。
   >
   > 那么我希望能够点击btn，如何输出h1的id
   >
   > 此时，就需要使用bind()来改变this指向，并且不执行

   ```js
   <button id='btn'>点击</button>
   <h1 id='h1'>aaa</h1>
   
   let btn = document.getElementById('btn')
   let h1 = document.getElementById('h1')
   btn.onclick = function(){
   	console.log(this.id)
   }.bind(h1)
   ```

3. 用call的情况

   除了以上情况，都能使用call方法，例如：需要接收多个参数、需要改变this指向、需要执行的函数。

总结：

首先，call、apply、bind的共同点是改变this指向。然后它们的区别是，call传入多个参数、apply传入一个数组、bind传入多个参数，并且call、apply会立即指向、bind不会立即执行。应用场景的话，一般情况下使用call。如果要接收一个数组，例如使用Math.max时需要接收多个参数，而我们的值又是数组时，就可以使用apply。如果需要修改this指向，但又不需要立马执行，使用bind，例如，dom有一个点击事件，我们通过点击事件，改变this指向，输出另一个id，此时就需要bind。