---
title: "Go dilini kullanarak PostgreSQL için Azure Veritabanı’na bağlanma | Microsoft Docs"
description: "Bu hızlı başlangıçta, PostgreSQL için Azure Veritabanı’na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz Go programlama dili örneği sağlanmıştır."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: go
ms.topic: hero-article
ms.date: 06/29/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: f3e7e57993b5253a895640854672da431c00854b
ms.contentlocale: tr-tr
ms.lasthandoff: 07/04/2017

---

<a id="azure-database-for-postgresql-use-go-language-to-connect-and-query-data" class="xliff"></a>

# PostgreSQL için Azure Veritabanı: Bağlanmak ve veri sorgulamak için Go dilini kullanma
Bu hızlı başlangıçta, [Go](https://golang.org/) dilinde yazılmış kod kullanılarak PostgreSQL için Azure Veritabanı’na nasıl bağlanılacağı gösterilmiştir. Hızlı başlangıçta, veritabanında verileri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerinin nasıl kullanılacağı da gösterilmiştir. Bu makalede, Go kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve PostgreSQL için Azure Veritabanı ile çalışmaya yeni başladığınız varsayılır.

<a id="prerequisites" class="xliff"></a>

## Ön koşullar
Bu hızlı başlangıç, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynakları kullanır:
- [DB Oluşturma - Portal](quickstart-create-server-database-portal.md)
- [DB Oluşturma - Azure CLI](quickstart-create-server-database-azure-cli.md)

<a id="install-go-and-pq" class="xliff"></a>

## Go ve pq yükleme
- Platformunuza uygun [yükleme yönergelerine](https://golang.org/doc/install) göre Go’yu indirip yükleyin.
- Projeniz için C:\Postgresql\ gibi bir klasör oluşturun. Komut satırını kullanarak dizini `cd C:\Postgres\` gibi bir proje klasörüyle değiştirin.
- Aynı dizindeyken `go get github.com/lib/pq` komutunu yazarak [Pure Go Postgres sürücüsünü (pq)](https://github.com/lib/pq) proje klasörünüze indirin.

<a id="build-and-run-go-code" class="xliff"></a>

## Go kodunu derleme ve çalıştırma 
- Kodu * go uzantısına sahip bir metin dosyasına ve `C:\postgres\read.go` gibi bir proje klasörünüze kaydedin.
- Kodu çalıştırmak için dizini `cd postgres` proje klasörünüzle değiştirin, ardından uygulamayı derleyip çalıştırmak için `go run read.go` komutunu yazın.
- Kodu yerel bir uygulamada derlemek için `go build read.go`, ardından uygulamayı çalıştırmak için read.exe’yi başlatın.

<a id="get-connection-information" class="xliff"></a>

## Bağlantı bilgilerini alma
PostgreSQL için Azure Veritabanı’na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve oluşturduğunuz sunucuyu, örneğin **mypgserver-20170401**’i aratın.
3. **mypgserver-20170401** sunucu adına tıklayın.
4. Sunucunun **Genel Bakış** sayfasını seçin. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin.
 ![PostgreSQL için Azure Veritabanı - Sunucu Yöneticisi Oturum Açma](./media/connect-go/1-connection-string.png)
5. Sunucunuzun oturum açma bilgilerini unuttuysanız **Genel Bakış** sayfasına gidip Sunucu yöneticisi oturum açma adını görüntüleyin. Gerekirse parolayı sıfırlayın.

<a id="connect-and-create-a-table" class="xliff"></a>

## Bir tabloyu bağlama ve oluşturma
**CREATE TABLE** SQL deyimini kullanarak bir tabloyu bağlamak ve oluşturmak ve ardından **INSERT INTO** SQL deyimlerini kullanarak tabloya satırlar eklemek için aşağıdaki kodu kullanın.

Kod üç paketi içeri aktarır: [sql paketi](https://golang.org/pkg/database/sql/), Postgres sunucusuyla iletişim kuran sürücü olarak [pq paketi](http://godoc.org/github.com/lib/pq) ve komut satırında yazdırılan girdi ve çıktı için [fmt paketi](https://golang.org/pkg/fmt/).

Kod, [sql.Open()](http://godoc.org/github.com/lib/pq#Open) yöntemini çağırarak PostgreSQL için Azure Veritabanı’na bağlanır ve [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) yöntemini kullanarak bağlantıyı kontrol eder. İşlem boyunca, veritabanı sunucusu için bağlantı havuzunu tutan bir [veritabanı tanıtıcı](https://golang.org/pkg/database/sql/#DB) kullanılır. Kod, birkaç SQL komutunu çalıştırmak için birkaç kez [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) yöntemini çağırır. Her seferinde hata oluşup olmadığını kontrol etmek ve hata oluşmuşsa acil çıkış yapmak için kullanılan özel bir checkError() yöntemi.


`HOST`, `DATABASE`, `USER` ve `PASSWORD` parametrelerini kendi değerlerinizle değiştirin. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed)")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table")

    // Insert some data into table.
    sql_statement := "INSERT INTO inventory (name, quantity) VALUES ($1, $2);"
    _, err = db.Exec(sql_statement, "banana", 150)
    checkError(err)
    _, err = db.Exec(sql_statement, "orange", 154)
    checkError(err)
    _, err = db.Exec(sql_statement, "apple", 100)
    checkError(err)
    fmt.Println("Inserted 3 rows of data")
}
```

<a id="read-data" class="xliff"></a>

## Verileri okuma
**SELECT** SQL deyimini kullanarak bağlanmak ve verileri okumak için aşağıdaki kodu kullanın. 

Kod üç paketi içeri aktarır: [sql paketi](https://golang.org/pkg/database/sql/), Postgres sunucusuyla iletişim kuran sürücü olarak [pq paketi](http://godoc.org/github.com/lib/pq) ve komut satırında yazdırılan girdi ve çıktı için [fmt paketi](https://golang.org/pkg/fmt/).

Kod, [sql.Open()](http://godoc.org/github.com/lib/pq#Open) yöntemini çağırarak PostgreSQL için Azure Veritabanı’na bağlanır ve [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) yöntemini kullanarak bağlantıyı kontrol eder. İşlem boyunca, veritabanı sunucusu için bağlantı havuzunu tutan bir [veritabanı tanıtıcı](https://golang.org/pkg/database/sql/#DB) kullanılır. [db. Query()](https://golang.org/pkg/database/sql/#DB.Query) yöntemi çağrılarak select sorgusu çalıştırılır ve ortaya çıkan satırlar [rows](https://golang.org/pkg/database/sql/#Rows) türünden bir değişkende tutulur. Kod, [rows.Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) yöntemini kullanarak geçerli satırdaki sütun veri değerlerini okur ve [rows.Next()](https://golang.org/pkg/database/sql/#Rows.Next) yineleyicisini kullanarak başka satır kalmayana dek satırları döndürür. Her satırın sütun değerleri konsol çıkışına yazdırılır. Her seferinde hata oluşup olmadığını kontrol etmek ve hata oluşmuşsa acil çıkış yapmak için kullanılan özel bir checkError() yöntemi.

`HOST`, `DATABASE`, `USER` ve `PASSWORD` parametrelerini kendi değerlerinizle değiştirin. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Read rows from table.
    var id int
    var name string
    var quantity int

    sql_statement := "SELECT * from inventory;"
    rows, err := db.Query(sql_statement)
    checkError(err)

    for rows.Next() {
        switch err := rows.Scan(&id, &name, &quantity); err {
        case sql.ErrNoRows:
            fmt.Println("No rows were returned")
        case nil:
            fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
        default:
            checkError(err)
        }
    }
}
```

<a id="update-data" class="xliff"></a>

## Verileri güncelleştirme
**UPDATE** SQL deyimini kullanarak bağlanmak ve verileri güncelleştirmek için aşağıdaki kodu kullanın.

Kod üç paketi içeri aktarır: [sql paketi](https://golang.org/pkg/database/sql/), Postgres sunucusuyla iletişim kuran sürücü olarak [pq paketi](http://godoc.org/github.com/lib/pq) ve komut satırında yazdırılan girdi ve çıktı için [fmt paketi](https://golang.org/pkg/fmt/).

Kod, [sql.Open()](http://godoc.org/github.com/lib/pq#Open) yöntemini çağırarak PostgreSQL için Azure Veritabanı’na bağlanır ve [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) yöntemini kullanarak bağlantıyı kontrol eder. İşlem boyunca, veritabanı sunucusu için bağlantı havuzunu tutan bir [veritabanı tanıtıcı](https://golang.org/pkg/database/sql/#DB) kullanılır. Kod, tabloyu güncelleştiren SQL deyimini çalıştırmak için [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) yöntemini çağırır. Hata oluşup olmadığını kontrol etmek ve hata oluşmuşsa acil çıkış yapmak için kullanılan özel bir checkError() yöntemi.

`HOST`, `DATABASE`, `USER` ve `PASSWORD` parametrelerini kendi değerlerinizle değiştirin. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Modify some data in table.
    sql_statement := "UPDATE inventory SET quantity = $2 WHERE name = $1;"
    _, err = db.Exec(sql_statement, "banana", 200)
    checkError(err)
    fmt.Println("Updated 1 row of data")
}
```

<a id="delete-data" class="xliff"></a>

## Verileri silme
**DELETE** SQL deyimini kullanarak bağlanmak ve verileri okumak için aşağıdaki kodu kullanın. 

Kod üç paketi içeri aktarır: [sql paketi](https://golang.org/pkg/database/sql/), Postgres sunucusuyla iletişim kuran sürücü olarak [pq paketi](http://godoc.org/github.com/lib/pq) ve komut satırında yazdırılan girdi ve çıktı için [fmt paketi](https://golang.org/pkg/fmt/).

Kod, [sql.Open()](http://godoc.org/github.com/lib/pq#Open) yöntemini çağırarak PostgreSQL için Azure Veritabanı’na bağlanır ve [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) yöntemini kullanarak bağlantıyı kontrol eder. İşlem boyunca, veritabanı sunucusu için bağlantı havuzunu tutan bir [veritabanı tanıtıcı](https://golang.org/pkg/database/sql/#DB) kullanılır. Kod, tabloyu güncelleştiren SQL deyimini çalıştırmak için [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) yöntemini çağırır. Hata oluşup olmadığını kontrol etmek ve hata oluşmuşsa acil çıkış yapmak için kullanılan özel bir checkError() yöntemi.

`HOST`, `DATABASE`, `USER` ve `PASSWORD` parametrelerini kendi değerlerinizle değiştirin. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Delete some data from table.
    sql_statement := "DELETE FROM inventory WHERE name = $1;"
    _, err = db.Exec(sql_statement, "orange")
    checkError(err)
    fmt.Println("Deleted 1 row of data")
}
```

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı aktarma ve İçeri aktarmayı kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)

