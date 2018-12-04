---
title: Promies_A+
date: 2017-12-23 17:08:31
categories: JS
tags: Promise
---

<h2>带你手写Promise A源码</h2>
  <p style="text-indent: 2em">我们都知道，在现代化的前端工程化项目中，由于Promise对异步的管理极其方便，使得其的应用越来越广泛，这也就意味着深度理解掌握Promise原理是我们现代化的前端工程师们必备技能之一，本人刚开始也只是一直使用Promise，没有深究其原理，但是随着项目应用越来越深入，愈发觉得对Promise的理解过于浅薄，所以特意查了下资料，结合自己的理解，写出一版Promise A+规范来加深自己的理解</p>

```javascript
class Promise {
    constructor(excutorCallBack) {
        this.status = 'pending';
        this.value = undefined;
        this.fulfilledAry = [];
        this.rejectedAry = [];

        //=>执行EXCUTOR（异常捕获）
        let resolveFn = result => {
            let timer = setTimeout(() => {
                clearTimeout(timer);
                if (this.status !== 'pending') return;
                this.status = 'fulfilled';
                this.value = result;
                this.fulfilledAry.forEach(item => item(this.value));
            }, 0);
        };
        let rejectFn = reason => {
            let timer = setTimeout(() => {
                clearTimeout(timer);
                if (this.status !== 'pending') return;
                this.status = 'rejected';
                this.value = reason;
                this.rejectedAry.forEach(item => item(this.value));
            }, 0);
        };
        try {
            excutorCallBack(resolveFn, rejectFn);
        } catch (err) {
            //=>有异常信息按照REJECTED状态处理
            rejectFn(err);
        }
    }

    then(fulfilledCallBack, rejectedCallBack) {
        //=>处理不传递的状况
        typeof fulfilledCallBack !== 'function' ? fulfilledCallBack = result => result : null;
        typeof rejectedCallBack !== 'function' ? rejectedCallBack = reason => {
            throw new Error(reason instanceof Error ? reason.message : reason);
        } : null;

        //=>返回一个新的PROMISE实例
        return new Promise((resolve, reject) => {
            this.fulfilledAry.push(() => {
                try {
                    let x = fulfilledCallBack(this.value);
                    x instanceof Promise ? x.then(resolve, reject) : resolve(x);
                } catch (err) {
                    reject(err);
                }
            });
            this.rejectedAry.push(() => {
                try {
                    let x = rejectedCallBack(this.value);
                    x instanceof Promise ? x.then(resolve, reject) : resolve(x);
                } catch (err) {
                    reject(err);
                }
            });
        });
    }

    catch (rejectedCallBack) {
        return this.then(null, rejectedCallBack);
    }

    static all(promiseAry = []) { //=>Promise.all()
        return new Promise((resolve, reject) => {
            //=>INDEX:记录成功的数量 RESULT:记录成功的结果
            let index = 0,
                result = [];
            for (let i = 0; i < promiseAry.length; i++) {
                //=>promiseAry[i]:
                //每一个需要处理的PROMISE实例
                promiseAry[i].then(val => {
                    index++;
                    result[i] = val; //=>索引需要和promiseAry对应上，保证结果的顺序和数组顺序一致
                    if (index === promiseAry.length) {
                        resolve(result);
                    }
                }, reject);
            }
        });
    }
}

module.exports = Promise;
```

