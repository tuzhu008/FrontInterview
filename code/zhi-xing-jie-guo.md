# 执行结果

* 1

  ```js
  setTimeout(() => console.log(1), 0);

  new Promise((reslove, reject) => {
    console.log(2);
    resolve();
  }).then(() => {
    console.log(3)
  }).then(() => {
    console.log(4)
  })
  process.nextTick(() => console.log(5));

  console.log(6);
  ```




