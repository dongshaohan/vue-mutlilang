# vue-multilang: 前端多语言加载器
> control of languages in vue2

## Installation

```bash
# npm
npm install vue-multilang --save-dev
```

## Example

```bash
cd vue-multilang
npm install
npm run dev
```

## Get Started

in main.js
```bash
import Vue from 'vue'
import App from './App.vue'
import VueMultiLang from "vue-multilang"

Vue.use(VueMultiLang)

let multiLang = new VueMultiLang({
    path: './example/lang',
    version: 1,
    lang: ['ar', 'vi', 'th', 'id']
})
new Vue({
    el: '#app',
    multiLang, // 实例名约定 必须multiLang 参考router(vue-router)、store(vuex)
    render: h => h(App)
})
```
in HTML
```bash
<script src="/assets/js/vue.min.js"></script>
<script src="/assets/js/vue-multilang.js"></script>

let multiLang = new VueMultiLang({
    path: './example/lang',
    version: 1,
    lang: ['ar', 'vi', 'th', 'id']
})
new Vue({
    el: '#app',
    multiLang, // 实例名约定 必须multiLang 参考router(vue-router)、store(vuex)
    render: h => h(App)
})
```

More details:
- **langCode**: 语言码
- **countryCode**: 国家码
- **langObj**: 语言包数据
- **onReady**: 语言包加载完毕后的回调函数 可写多个
    - **@param fn(data)**: data是语言包数据
- **template**: 模板内容替换 动态替换语言包的内容 匹配规则为%s
    - **@param key**: 语言包字段
    - **@param args**: 动态参数列表
    
use in components - this.$lang inside of any component
```bash 
export default {
    ...,
    data() {
        return {
            langObj: {},
            lang: ''
        }
    },
    computed: {
        text() {
            /**
             * {'rank': 'my name is %s'} from langObj 
             */
             
            // result: 'my name is dongshaohan'
            return this.$lang.template('rank', ['dongshaohan'])
        }
    },
    created() {
        this.$lang.onReady(() => {
            this.langObj = this.$lang.langObj
            this.langCode = this.$lang.langCode
            this.countryCode = this.$lang.countryCode
        });
    },
    ...
}
```
use in window - window.$multiLang
```bash
window.$multiLang.langCode
window.$multiLang.countryCode
window.$multiLang.langObj
window.$multiLang.onReady
window.$multiLang.template
```

## Config
|name|required|type|introduction|
|----|----|----|----|
|lang|yes|array|项目需要配置的语言码集合，写成数组形式['en', 'th', 'id']|
|path|no|string|语言包路径，默认是空字符串|
|defaultLang|no|string|设置默认语言，防止匹配不到语言包出错|
|version|no|string|语言包文件版本号，去缓存|
|langUrlRegExp|no|string|URL语言码匹配规则，默认/\blang=(.+?)\b/i|
|langUaRegExp|no|string|UA语言码匹配规则，默认/\blang\/(.+?)\b/i|
|locationUrlRegExp|no|string|URL国家码匹配规则，默认/\blocation=(.+?)\b/i|
|locationUaRegExp|no|string|UA国家码匹配规则，默认/\blocation\/(.+?)\b/i|
|rtlList|no|array|阅读习惯从右到左的语言码集合|
|dataType|no|string|语言包文件类型，默认json|
|callback|no|function|语言包加载成功后回调，参数data为返回的值|