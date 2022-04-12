GOLANG - JWT-GO
===============

```go
package user

import (
	"errors"
	"github.com/dgrijalva/jwt-go"
	"time"
)

var jwtSecret = []byte("password")

type Claims struct {
	jwt.StandardClaims
	User
}

func GenerateToken(user *User) (string, error) {
	nowTime := time.Now()
	expireTime := nowTime.Add(1000 * time.Hour)
	claims := Claims{
		User: *user,
		StandardClaims: jwt.StandardClaims{
			ExpiresAt: expireTime.Unix(),
			Issuer:    "gin-blog",
		},
	}
	tokenClaims := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
	token, err := tokenClaims.SignedString(jwtSecret)
	return token, err
}

func ParseToken(token string) (*User, error) {
	tokenClaims, err := jwt.ParseWithClaims(token, &Claims{}, func(token *jwt.Token) (interface{}, error) {
		return jwtSecret, nil
	})
	if tokenClaims != nil {
		if claims, ok := tokenClaims.Claims.(*Claims); ok && tokenClaims.Valid {
			return &claims.User, nil
		}
		return nil, errors.New("token Claims not Valid")
	}
	return nil, err
}


```

