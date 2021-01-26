# msgpack web token

由于msgpack性能比json好，所以将json转成msgpack来将token序列化。

本代码完全由[github.com/dgrijalva/jwt-go](https://github.com/dgrijalva/jwt-go) 改编过来，只是将json序列化改成了msgpack序列化，具体的使用和原作者一模一样。