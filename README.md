# tiny-validator
易用的表单验证器

### 使用
#### 验证表单
foo.js
```javascript
import validator from '?/tiny-validator'

let formData = {
    name: '',
    mobile: 123456, // 字段值为非字符串，在匹配规则时会被String()强制转换为字符串
    password: 'error',
    city: ['重庆', '南岸']
}

let formRules = {
    name: [
        { required: true, message: '姓名不能为空' }
    ],
    mobile: [
        { required: true, message: '姓名不能为空' },
        { rule: 'mobile', message: '手机号格式错误' } // rule值为字符串表示预设规则名
    ],
    password: [
        { rule: /abc/, message: '密码错误' } // rule值为正则表达式，使用该自定义规则匹配
    ],
    city: [
        { validator: validator.customize.arrNotEmpty, message: '请选择城市' } // 自定义验证器
    ]
}

let rst = validator.validate(formData, formRules); // 所有字段验证通过，则返回true

console.log(rst);

// 输出：
[
    { field: 'name', message: '姓名不能为空' },
    { field: 'mobile', message: '手机号格式错误' },
    { field: 'password', message: '密码错误' }
]
```
rules.js
```javascript
export default {
    mobile: {
        pattern: /1\d{10}/,
        message: '手机号格式错误'
    }
}
```
customize.js
```javascript
export default {
    arrNotEmpty
}

// 判断数组不为空
function arrNotEmpty(value) {
    if(value instanceof Array) {
        return value.length != 0
    }
    else {
        return false
    }
}
```
#### 验证单个字段
```javascript
import validator from '?/tiny-validator'

// single - 多条验证规则
let value1 = '7777777',
    rules1 = [
        { required: true, message: '姓名不能为空' },
        { rule: 'mobile', message: '手机号格式错误' }
    ]
    
let rst1 = validator.single(value1, rules1); // "手机号格式错误"

// match - 单条验证规则
let value2 = 'abc';
let rst2 = validator.match(value2, /abc/); // true
```