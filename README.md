# nick-validator
一个基于 Node.js 的后端参数校验器

## 使用手册

### 使用条件

### 安装

### 使用
```js
// 导入 nick-validator 库
const NickValidator = require('nick-validator')

// 自定义校验器，继承自 NickValidator
class RegisterValidator extends NickValidator{
    constructor () {
        super()
        // 在 constructor 中定义需要验证的字段，以 this.字段名 为名称
        // 值为数组，里面可以包含一个或多个校验规则
        this.email = [
            new Rule('isEmail', '不符合 Email 校验规范')
        ]
        this.password1 = [
            new Rule('isLength', '密码至少6个字符，最多32个字符', {
                min: 6,
                max: 32
            }),
            new Rule('matches', '密码不符合规范', '^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]')
        ]
        this.password2 = this.password1
        this.nickname = [
            new Rule('isLength', '昵称不符合长度规范', {
                min: 4,
                max: 32
            })
        ]
    }
    // 如果普通校验规则不能完成校验，可以自定义校验函数，以 validateXXX 的命名方式命名
    // 其中，XXX 是被校验的参数名称，整个函数名才用驼峰命名方式
    validatePassword (vals) {
        const psw1 = vals.body.password1
        const psw2 = vals.body.password2
        if (psw1 !== psw2) throw new Error('两个密码必须相同')
    }
    
    // 支持异步校验
    async validateEmail (vals) {
        const email = vals.body.email
        const user = await User.findOne({
            where: {
                email
            }
        })
        if (user) {
            throw new Error('email 已存在')
        }
    }
}
```

## License

This project is licensed under the MIT License - see the LICENSE.md file for details

## Acknowledgement
