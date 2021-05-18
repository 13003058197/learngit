# 安装

```bash

npm install -g @vue/cli
vue -V
vue create my-project

npm nit vite-app <project-name>
yarn create vite-app <project-name>
```

# 基础

```js
// 定义全局冲属性
import {createApp} from "vue"
cosnt app = createApp(App)
app.config.globalPropeties.Axios = Axios; // 读取 this.Axios
app.mount("#app")
```



# 非父子组件传值

```js
npm i --save mitt

import mitt from "mitt"
const emitter = mitt();
emitter.on("xxx",function(data){})
emitter.emit("xxx",data)
```

# 自定义v-model

```js
<a-input v-model:foo ="foo">;
    
    <input :value="foo" @input="$emit('update:foo', $event.target.value)">
    
    export {
        props: ['foo'],
         
    }
        
// 绑定多个值    
<a-input v-model:fist-name ="fistName" v-model:last-name ="lastName">;
{
    props: ['fistName', 'lastName']
}
    
```

# Attribute 继承

```js
<div v-bind="$attrs">
export {
	inheritAttrs:false // 禁止继承
}
```

# vue3

## Teleport

```
<teleport to="body">
 // teleport的内容显示到body中
</teleport>
```

## setup

```html
<div>
    {{title}}
    {{user.name}}
</div>
<script>
import { ref , reactive} from "vue"
export {
	name: "Home",
    setup(){
        // ref 定义字符串，number，boolean，数据类型的响应数据
        cosnt title = ref("标题")
        // reactive 定义对象
        const user = reactive({
            name: "zs",
            age: 19
        })
        return {
            title,
            user
        }
    }
}
</script>

```

