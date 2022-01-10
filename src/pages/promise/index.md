---
title: 手写promise
date: 2021-08-16 22:13:28
spoiler: 省略.
cta: 'js'
---

这边记录下promise，估计回头又忘光了
新建myPromise.js文件
```jsx
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'

class myPromise {
    constructor(executor) {
        // executor(this.resolve, this.reject)
        try {
            executor(this.resolve, this.reject)
        } catch(error) {
            this.reject(error)
        }
    }

    static resolve(parameter) {
        if (parameter instanceof myPromise) {
            return parameter
        }
        return new myPromise(resolve => {
            resolve(parameter)
        })
    }
    static reject(parameter) {
        return new myPromise(reject => {
            reject(parameter)
        })
    }

    status = PENDING
    value = null
    reason = null

    // onFulfilledCallback = null
    // onRejectedCallback = null
    onFulfilledCallbacks = []
    onRejectedCallbacks = []

    resolve = (value) => {
        if (this.status === PENDING) {
            this.status = FULFILLED
            this.value = value
            // this.onFulfilledCallback && this.onFulfilledCallback(value)
            while (this.onFulfilledCallbacks && this.onFulfilledCallbacks.length > 0) {
                this.onFulfilledCallbacks.shift()(value)
            }
        }
    }

    reject = (reason) => {
        if (this.status === PENDING) {
            this.status = REJECTED
            this.reason = reason
            // this.onRejectedCallback && this.onRejectedCallback(reason)
            while(this.onRejectedCallbacks && this.onRejectedCallbacks.length > 0) {
                this.onRejectedCallbacks.shift()(reason)
            }
        }
    }

    then(onFulfilled, onRejected) {
        // if(this.status === FULFILLED) {
        //     onFulfilled(this.value)
        // } else if (this.status === REJECTED) {
        //     onRejected(this.reason)
        // } else if (this.status === PENDING) {
        //     this.onFulfilledCallbacks.push(onFulfilled)
        //     this.onRejectedCallbacks.push(onRejected)
        // }
        onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value
        onRejected = typeof onRejected === 'function' ? onRejected : reason => { throw reason }
        const promise2 = new myPromise((resolve, reject) => {
            if (this.status === FULFILLED) {
                queueMicrotask(() => {
                    try {
                        const x = onFulfilled(this.value)
                        resolvePromise(promise2, x, resolve, reject)
                    } catch(error) {
                        reject(error)
                    }
                })
            } else if (this.status === REJECTED) {
                // onRejected(this.reason)
                queueMicrotask(() => {
                    try {
                        const x = onRejected(this.reason)
                        resolvePromise(promise2, x, resolve, reject)
                    } catch(error) {
                        reject(error)
                    }
                })
            } else if (this.status === PENDING) {
                // this.onFulfilledCallbacks.push(onFulfilled)
                // this.onRejectedCallbacks.push(onRejected)
                this.onFulfilledCallbacks.push(() => {
                    queueMicrotask(() => {
                        try {
                            const x = onFulfilled(this.value)
                            resolvePromise(promise2, x, resolve, reject)
                        } catch(error) {
                            reject(error)
                        }
                    })
                })
                this.onRejectedCallbacks.push(() => {
                    queueMicrotask(() => {
                        try {
                            const x = onRejected(this.reason)
                            resolvePromise(promise2, x, resolve, reject)
                        } catch(error) {
                            reject(error)
                        }
                    })
                })
            }
        })
        return promise2
    }
}

function resolvePromise(promise2, x, resolve, reject) {
    if (promise2 === x) {
        return reject(new TypeError('Chaining cycle detected for promise #<Promise>'))
    }
    if (x instanceof myPromise) {
        x.then(resolve, reject)
    } else {
        resolve(x)
    }
}

module.exports = myPromise
```

新建test.js

```jsx
const myPromise = require('./myPromise')
const promise = new myPromise((resolve, reject) => {
    resolve('success')
})

promise.then(value => {
    console.log('resolve,' + value)
}, reason => {
    console.log('reject,' + reason)
})
```

$ node test.js