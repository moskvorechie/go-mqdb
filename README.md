# MySQL connect module

Connection module to MySQL database

## Usage example

```golang
package main

import "github.com/moskvorechie/go-mqdb/v3"

func main() {

	// Open connection
	db, err := mqdb.New(mqdb.Config{
		Host: "127.0.0.1",
		Port: 30007,
		Name: "go-mqdb",
		User: "go-mqdb",
		Pass: "go-mqdb",
	})
	if err != nil {
		panic(err)
	}

	// Check query
	err = db.Exec("SHOW DATABASES").Error
	if err != nil {
		panic(err)
	}

	// Create table
	type User struct {
		gorm.Model
		Name     string
		Age      int
		Birthday time.Time
	}
	user := User{Name: "test", Age: 18, Birthday: time.Now()}
	err = db.AutoMigrate(&user)
	if err != nil {
		panic(err)
	}

	// Truncate
	err = db.Exec("TRUNCATE TABLE users").Error
	if err != nil {
		panic(err)
	}

	// Create user
	err = db.Create(&user).Error
	if err != nil {
		panic(err)
	}
	if user.ID <= 0 {
		panic("bad user")
	}

	// Get user
	user = User{}
	err = db.First(&user, "name = ? AND age = ?", "test", 18).Error
	if err != nil {
		panic(err)
	}

	// Drop user
	err = db.Delete(&user).Error
	if err != nil {
		panic(err)
	}
}

```
