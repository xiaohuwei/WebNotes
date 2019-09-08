##  使用场景

> Vuex 强调使用单一状态树，即在一个项目里只有一个 store，这个 store 集中管理了项目中所有的数据以及对数据的操作行为。但是这样带来的问题是 store 可能会非常臃肿庞大不易维护，所以就需要对状态树进行模块化的拆分。

首先介绍下基本的组件化规则：你可以根据项目组件的划分来拆分 store，每个模块里管理着当前组件的状态以及行为，最后将这些模块在根 store 进行组合。下面的代码块就是一个最简单的模块化。

~~~js
const moduleA = {
    state: { ... },
    getters: { ... }
    mutations: { ... }
};

const moduleB = {
    state: { ... },
    getters: { ... },
    mutations: { ... },
    actions: { ... }
};

const store = new Vuex.Store({
    modules: {
        a: moduleA,
        b: moduleB
    }
});

console.log(store.state.a); // moduleA 的 state

~~~

## State

> 在 Vuex 模块化中，`state` 是唯一会根据组合时模块的别名来添加层级的，后面的 `getters`、`mutations` 以及 `actions` 都是直接合并在 `store `下。
>
> 例如，访问模块 a 中的` state`，要通过 `store.state.a`，访问根 `store` 上申明的 `state`，依然是通过 `store.state.xxx `直接访问。

~~~js
const moduleA = {
    state: {
        maState: 'A'
    }
};

const moduleB = {
    state: {
        mbState: 'B'
    }
};

const store = new Vuex.Store({
    modules: {
        a: moduleA,
        b: moduleB
    },
    state: {
        rtState: 'Root'
    }
});

console.log(store.state.a.maState); // A
console.log(store.state.b.mbState); // B
console.log(store.state.rtState); // Root

~~~

## **Getters**

> 与` state` 不同的是，不同模块的 `getters` 会直接合并在 `store.getters` 下

~~~js
const moduleA = {
    state: {
        count: 1
    },
    getters: {
        maGetter(state, getters, rootState) {
            return state.count + rootState.b.count;
        }
    }
};

const moduleB = {
    state: {
        count: 2
    },
    getters: {
        mbGetter() {
            return 'Hello Vuex';
        }
    }
};

const store = {
    modules: {
        a: moduleA,
        b: moduleB
    }
};
//跟state不一样  getters不区分模块 统一通过.getters.xxx调用
console.log(store.getters.maGetter); // 3
console.log(store.getters.mbGetter); // Hello Vuex
~~~

 `getters` 的回调函数所接收的前两个参数，模块化后需要用到第三个参数——`rootState`。下面是解释

1. `state`，模块中的 `state` 仅为模块自身中的 `state`；

2. `getters`，等同于 `store.getters`；
3. `rootState`，全局 `state`。通过 `rootState`，模块中的`getters` 就可以引用别的模块中的 `state` 了，十分方便。

**注意：由于 getters 不区分模块，所以不同模块中的 getters 如果重名，Vuex 会报出 'duplicate getter key: [重复的getter名]' 错误。**

## **Mutations**

> `mutations` 与 `getters` 类似，不同模块的 `mutation` 均可以通过 `store.commit` 直接触发。

~~~js
const moduleA = {
    state: {
        count: 1
    },
    mutations: {
        sayCountA(state) {
            console.log('Module A count: ', state.count);
        }
    }
};

const moduleB = {
    state: {
        count: 2
    },
    mutations: {
        sayCountB(state) {
            console.log('Module B count: ', state.count);
        }
    }
};

const store = {
    modules: {
        a: moduleA,
        b: moduleB
    }
};

store.commit('sayCountA'); // Module A count: 1
store.commit('sayCountB'); // Module B count: 2 
~~~

## **Actions**

> 与 `mutations` 类似，不同模块的 `actions` 均可以通过`store.dispatch` 直接触发。

~~~js
const moduleA = {
    state: {
        count: 1
    },
    mutations: {
        sayCountA(state) {
            console.log('Module A count: ', state.count);
        }
    },
    actions: {
        maAction(context) {
            context.dispatch('mbAction');
        }
    }
};

const moduleB = {
    state: {
        count: 2
    },
    mutations: {
        sayCountB(state, num) {
            console.log('Module B count: ', state.count+num);
        }
    },
    action: {
        mbAction({ commit, rootState }) {
            commit('sayCountA');
            commit('sayCountB', rootState.a.count);
        }
    }
};

const store = {
    modules: {
        a: moduleA,
        b: moduleB
    }
};

store.dispatch('maAction'); // Module A count: 1、Module B count: 3
~~~

从上例可以看出

`action` 的回调函数接收一个 `context`上下文参数

`context` 包含：1. `state`、2. `rootState`、3. `getters`、4. `mutations`、5. `actions` 五个属性，为了简便可以在参数中解构。

在 `action` 中可以通过 `context.commit` 跨模块调用 `mutation`，同时一个模块的 `action` 也可以调用其他模块的 action。

同样的，当不同模块中有同名 `action` 时，通过 `store.dispatch` 调用，会依次触发所有同名 `actions`。

> 最后有一点要注意的是，将 `store` 中的` state` 绑定到 Vue 组件中的 `computed` 计算属性后，对` state` 进行更改需要通过 `mutation` 或者 `action`，在 Vue 组件中直接进行赋值` (this.myState = 'ABC')` 是不会生效的。

