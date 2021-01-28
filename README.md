# msgpack web token

由于msgpack性能比json好，所以将json转成msgpack来将token序列化。

本代码完全由[github.com/dgrijalva/jwt-go](https://github.com/dgrijalva/jwt-go) 改编过来，只是将json序列化改成了msgpack序列化，具体的使用和原作者一模一样。

##example
```
package main

import (
	"encoding/json"
	"fmt"
	"time"

	"github.com/dcsunny/mwt"
)

func main() {
	secret := []byte("test")
	fmt.Println("DEMO1:")
	t := mwt.New(mwt.SigningMethodHS256)
	token, err := t.SignedString(secret)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(token)

	_t, err := mwt.Parse(token, func(token *mwt.Token) (interface{}, error) {
		return secret, nil
	})
	if err != nil {
		fmt.Println(err)
		return
	}
	j, _ := json.Marshal(_t)
	fmt.Println(string(j))
	fmt.Println("----------------------------")
	fmt.Println("DEMO2:")
	expireAt := time.Now().Add(time.Duration(2) * time.Second).Unix()
	t1 := mwt.NewWithClaims(mwt.SigningMethodHS256, mwt.StandardClaims{
		ExpiresAt: expireAt,
	})
	token, err = t1.SignedString(secret)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(token)
	_t1, err := mwt.ParseWithClaims(token, &mwt.StandardClaims{}, func(token *mwt.Token) (interface{}, error) {
		return secret, nil
	})
	if err != nil {
		fmt.Println(err)
		return
	}
	j, _ = json.Marshal(_t1)
	fmt.Println(string(j))
	fmt.Println("wait 3s...")
	time.Sleep(time.Second * 3)
	_t2, err := mwt.ParseWithClaims(token, &mwt.StandardClaims{}, func(token *mwt.Token) (interface{}, error) {
		return secret, nil
	})
	if err != nil {
		fmt.Println(err)
		return
	}
	j, _ = json.Marshal(_t2)
	fmt.Println(string(j))
}

```