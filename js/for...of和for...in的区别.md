- for of是ES6新增的，for in 是ES5的

- for of是遍历对象的值的，for in是遍历对象的属性名

  > 由于这个原因，所以for in不适合遍历数组，因为数组没有属性名，会返回1 2 3 4

- 使用

  ```
  for (var key in arr){
      console.log(arr[key]);
  }
  
  for (var value of arr){
      console.log(value);
  }
  ```

  