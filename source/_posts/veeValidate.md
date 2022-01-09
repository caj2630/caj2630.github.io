---
title: '关于vee-validate插件使用'
date: 2021-11-10
categories:
- [项目扩展]
- [校验]
- [vue]
- [ElementUi]
tags:
- vee-validate3.x
---

## 使用vee-validate

安装 npm install vee-validate（3.x版本的）

### 引入并配置规则
```ecmascript 6
import { extend } from 'vee-validate';
import { required, email } from 'vee-validate/dist/rules';
import { localize } from 'vee-validate'; // 设置语言 默认是en
import zh_CN from 'vee-validate/dist/locale/zh_CN.json';

// 方法一
// 此方法需要name="邮箱"，name为中文
localize('zh_CN', zh_CN);

// 方法二
// filed文件内容：email:"邮箱"等多个字段,配置中文
import filed from "./filed";
localize("zh_CN", {
    messages: {
        ...zh_CN.messages,
        required: "请输入{_field_}", // 自定义的必填校验的文字展示
    },
    names: {
        ...filed,
    },
    fields: {
        email: {
            email: "{_field_}格式不正确",
            required: "请输入{_field_}",
        },
    },
});

// 导出规则
extend('required', {
  ...required,
  message: 'This field is required'
});

extend('email', {
  ...email,
  message: 'This field must be a valid email'
});

// 自定义导出规则
// 身份证校验
extend("customRegIdCard", {
    message: () => {
        return "身份证格式不正确";
    },
    validate: (value) => {
        const reg =
            /(^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{2}[0-9Xx]$)/;

        return reg.test(value);
    },
});
```
### 使用
```html
<ValidationObserver ref="observe">
    <ValidationProvider name="email" rules="required|email">
        <div v-solt="{ errors }">
            <input v-model="email">
            <p>{{ errors[0] }}</p>
        </div>
    </ValidationProvider>
</ValidationObserver>
<script>
    import {ValidationObserver, ValidationProvider} from "vee-validate";
    export default {
        components: {
            ValidationObserver,
            ValidationProvider
        }
        methods: {
            async onsubmit(){
                const res = await this.$refs.observe.validate()
                if (!res) return // 校验不通过
                console.log('校验通过，继续执行提交信息')
            }
        }
    };
</script>
```
### 结合ElementUI使用
```html
<ValidationObserver ref="observe">
    <el-form>
        <ValidationProvider name="email" rules="required|email" v-solt="{ errors }">
            <el-form-item label="邮箱" error="errors[0]">
                <el-input v-module="value"/>
            </el-form-item>
        </ValidationProvider>
    </el-form>
</ValidationObserver>
```


