---
layout: post
title: Handle Unreachable Mysql in Go Server
---
Jadi kasusnya begini

1. Ada sebuah web server yang menggunakan resource mysql
2. Jika mysql tiba - tiba mati maka user akan di arahkan ke halaman maintenance
3. Jika mysql sudah hidup kembali, maka aplikasi akan menjalankan tugas seperti biasa
4. Semua hal di atas harus di handle secara otomatis


## Simple Server
Pada server yang menggunakan mysql kita buat kira - kira seperti ini.

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"net/http"

	_ "github.com/go-sql-driver/mysql"
)

func main() {
	db, err := sql.Open("mysql", "user:pass@/db")
	if err != nil {
		log.Fatal(err)
	}

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		rows, err := db.Query("SHOW TABLES")
		if err != nil {
			log.Fatal(err)
		}
		defer rows.Close()
		rows.Next()
		fmt.Fprintf(w, "Working well...`")
	})
	http.ListenAndServe(":8000", nil)
}
```

Jika hanya menggunakan kode seperti ini, maka ketika kita menggunakan `db` padahal mysql mati, 
maka akan terjadi error runtime pointer dereference, karena kita mencoba melakukan aktifitas pada `nil` connection.

## Handler error 
Untuk melakukan handle error kita bisa melakukan wrapper pada function Handler untuk pengecekan error.

Sebelum membuat wrapper kita pisahkan terlebih dahulu fungsi handler keluar main, lalu menambahkan pengecekan error

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"net/http"

	_ "github.com/go-sql-driver/mysql"
)

func homeHandler(db *sql.DB, w http.ResponseWriter, r *http.Request) {
	rows, err := db.Query("SHOW TABLES")
	if err != nil {
		log.Fatal(err)
	}
	defer rows.Close()
	rows.Next()
	fmt.Fprintf(w, "Working well...`")
	return
}

func main() {
	db, err := sql.Open("mysql", "user:pass@/db")
	if err != nil {
		log.Fatal(err)
	}

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		homeHandler(db, w, r)
	})
	http.ListenAndServe(":8000", nil)
}
```


