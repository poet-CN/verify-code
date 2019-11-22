获取验证码插件，依赖vue，element ui

接收参数：
* type: 验证码类型；
* phone: 手机号；
* disableBtn: Boolean格式，是否采用文字形式的“获取验证码”，若为否或不传，则为按钮形式；
* np: necessary params，Array格式，必要参数，可不传。若传，则校验必要参数是否存在，若不存在则不执行获取验证码操作。
