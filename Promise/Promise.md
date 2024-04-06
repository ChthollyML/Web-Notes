## 为什么需要 Promise
在使用 ajax 请求数据的时候常常会遇到一种情况，``req 1`` 请求了数据 `` data 1``，``req 2 `` 请求需要携带 ``data 2`` 的参数来请求，那么就必须要将 ``req 2`` 写在 ``req 1 `` 的请求回调之中，因为执行顺序是先将所有的请求先发送在处理回调的。
那如果有很多个这样的请求，就会形成回调地狱。
Promise 的出现就是想要解决回调地狱问题
## Promise 的状态
Promise 首先是一个构造函数，可以实例化一个 promise 对象
Promise 在初始化的时候接受一个函数作为参数，函数中还需要有 `resolve和reject` 两个参数
其内部有 `promisestate，promiseresult` 两个属性
> PromiseState：promise 状态 
> 1. Pending () 准备状态
> 2. Fulfilled（）已完成状态，成功
> 3. Rejected（） 拒绝状态

状态的改变，promise 通过转变 resolve 和 reject 函数改变状态，状态改变是一次性的
在 `then（）` 函数中的 resolve 函数 return 可以将返回的 promise 实例状态改变
## Promise 的结果
在调用 `resolve和reject` 传递的参数来改变当前 promise 对象的结果
## Promise 的方法
原型 `proto`
原型的方法
```
1. catch
2. constructor
3. finally
4. then
5. symbol
```
###  `then` 方法
参数：success 函数（当 promise 执行了 resolve 函数后执行），success 函数的参数是 Promise 的结果
failed 函数（当 promise 执行了 reject 函数后执行），同理
返回值：
还是一个 promise 对象，状态是 pending，
但是在 `then（）` 函数中的 resolve 函数 ``return`` 可以将返回的 promise 实例状态改变，这就可以让 `.then()` 函数传递下去
但是如果出错 `error`，会把返回的 promise 实例状态改为 `rejected`
> 当 promise 的状态不改变，不执行 then 方法

### `catch` 方法
当 promise 中执行了 `` reject`` 函数时或者出现 `error`，会执行 catch 函数
和 `then` 的第二个函数参数一样，一般都会使用 `catch`，在 `then` 中只传入一个参数
### `finally` 方法
 `promise.finally` 方法的回调函数不接受任何参数，这意味着 `finally` 没有办法知道，前面的 `Promise` 状态到底是 `fulfilled` 还是 `rejected` 。这表明，`finally` 方法里面的操作，应该是与 `Promise` 状态无关的，不依赖于 `Promise` 的执行结果。
### `all` 方法
`Promise.all` 接受一个 `promise` 对象的数组作为参数，当这个数组里的所有 `Promise` 对象全部变为 `resolve` 或者 `reject` 状态的时候，它才会去调用 `.then` 方法。
Promise.All ()重要细节点 （面试常考）：
- 如果所有的 Promise 中只有一个执行错误，那么整个 Promise. All 不会走 Promise.All (). Then () 而是走 Promise.All (). Catch ()这个流程了。但是要注意的是虽然走到了 Promise.All (). Catch ()这个流程，但是其他 Promise 还是会正常执行，但不会返回结果
- 要注意 Promise.All ()的返回值顺序，Promise.All (). Then ()的返回值顺序和传入的顺序是一致的，笔试时遇到手写 Promise.All ()时要注意。
### `allsettled` 方法
`Promise.AllSettled ()`的入参和 Promise. All、Promise. Race 一样，接受一个 promise 对象的数组作为参数, 也是同时开始、并行执行的。但是 Promise. AllSettled 的返回值需要注意以下几点：
Promise. AllSettled 不会走进 catch，当所有输入 Promise 都被履行或者拒绝时， statusesPromise 会解析一个具有具体完成状态的数组
{ status: 'fulfilled', value: value } ：如果相应的 promise 被履行
{ status: 'rejected', reason: reason }：如果相应的 promise 被拒绝

### `race` 方法
`Promise.rece()` 的使用方法和 `Promise.all` 一样，接收一个 `promise` 对象的数组为参数，`Promise.race` 是要有一个 promise 对象进入 `Fulfilled` 或者 `Rejected` 状态的话，就会继续进行后面的处理。这里依旧有两个点要注意：
- 和 Promise. All 一样是所有数组当中的 Promise 同时并行的
- Promise. Race 在第一个 Promise 对象变为 Fulfilled 之后，并不会取消其他 promise 对象的执行。
- Promise. Race 接受的是一个 Promise 对象数组，但是返回的确实最先完成 Fulfilled 或者最先被 Rejected 的一个 Promise 的结果
	第一个执行的（resolve 和 reject 都可以）
	
### `any` 方法
 `Promise.any` 的入参和 `Promise.all、Promise.race、Promise.allSettled` 一样，接收一个 `promise` 对象的数组作为参数。
 - 只要其中有一个 Promise 成功执行，就会返回已经成功执行的 Promise 的结果
- 如果这个 promise 对象的数组中没有一个 promise 可以成功执行（即所有的 promise 都失败 ），就返回一个失败的 promise 和 AggregateError 类型的实例，它是 Error 的一个子类，用于把单一的错误集合在一起
第一个 resolve 的