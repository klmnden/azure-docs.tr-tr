---
title: "Node.js’den PostgreSQL için Azure Veritabanı’na bağlanma | Microsoft Docs"
description: "Bu hızlı başlangıçta, PostgreSQL için Azure Veritabanı’na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz Node.js kod örneği sağlanmıştır."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 06/23/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 6a51fcf4f4494e5b32ccf6dabb19f8d004bb20d4
ms.contentlocale: tr-tr
ms.lasthandoff: 06/26/2017

---

# <a name="azure-database-for-postgresql-use-nodejs-to-connect-and-query-data"></a>PostgreSQL için Azure Veritabanı: Bağlanmak ve veri sorgulamak için Node.js’yi kullanma
Bu hızlı başlangıçta, Windows, Ubuntu Linux ve Mac platformlarından [Node.js](https://nodejs.org/) kullanılarak PostgreSQL için Azure Veritabanı’na nasıl bağlanılacağı gösterilmiştir. Hızlı başlangıçta, veritabanında verileri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerinin nasıl kullanılacağı da gösterilmiştir. Bu makaledeki adımlarda, Node.js kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve PostgreSQL için Azure Veritabanı ile çalışmaya yeni başladığınız varsayılır.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıç, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynakları kullanır:
- [DB Oluşturma - Portal](quickstart-create-server-database-portal.md)
- [DB oluşturma - CLI](quickstart-create-server-database-azure-cli.md)

Şunları da yapmanız gerekir:
- [Node.js](https://nodejs.org)’yi yükleme
- [pg](https://www.npmjs.com/package/pg) paketini yükleme. 

## <a name="install-nodejs"></a>Node.js’yi yükleme 
Platformunuza bağlı olarak Node.js’yi yüklemek için:

### <a name="mac-os"></a>**Mac OS**
Mac OS X ve **Node.js**’ye yönelik kullanımı kolay bir paket yöneticisi olan **brew** programını yüklemek için aşağıdaki komutları girin.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install node
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
**Node.js**’yi ve Node.js’nin paket yöneticisi olan **npm**’yi yüklemek için aşağıdaki komutları girin.

```bash
sudo apt-get install -y nodejs npm
```

### <a name="windows"></a>**Windows**
[Node.js indirme sayfasını](https://nodejs.org/en/download/) ziyaret edin ve istediğiniz Windows yükleyici seçeneğini belirleyin.

## <a name="install-pg-client"></a>pg istemcisini yükleme
node.js için eksiksiz bir engellemeye yol açmayan JavaScript istemcisi olan [pg](https://www.npmjs.com/package/pg)’yi yükleyin. Bu, PostgreSQL’e bağlanmak ve PostgreSQL’i sorgulamak için kullanışlı bir istemcidir.

Bunu yapmak için komut satırından JavaScript’e yönelik düğüm paketi yöneticisini (npm) çalıştırarak pg istemcisini yükleyin.
```bash
npm install pg
```

Yüklü paketleri listeleyerek yüklemeyi doğrulayın.
```bash
npm list
```
Liste komut çıktısı, her bileşenin sürümünü onaylar. 
```
`-- pg@6.2.3
  +-- buffer-writer@1.0.1
  +-- packet-reader@0.3.1
etc...
```

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
PostgreSQL için Azure Veritabanı’na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve kısa süre önce oluşturduğunuz **mypgserver-20170401** sunucusunu aratın.
3. **mypgserver-20170401** sunucu adına tıklayın.
4. Sunucunun **Genel Bakış** sayfasını seçin. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin.
 ![PostgreSQL için Azure Veritabanı - Sunucu Yöneticisi Oturum Açma](./media/connect-nodejs/1-connection-string.png)
5. Sunucunuzun oturum açma bilgilerini unuttuysanız **Genel Bakış** sayfasına giderek Sunucu yöneticisi oturum açma adını görüntüleyin ve gerekirse parolayı sıfırlayın.

## <a name="running-the-javascript-code-in-nodejs"></a>Node.js’de JavaScript kodunu çalıştırma
`node` yazarak Node.js’yi bash kabuğundan veya windows komut isteminden başlatabilir, ardından isteme kopyalayıp yapıştırarak örnek JavaScript kodunu etkili bir şekilde çalıştırabilirsiniz. Alternatif olarak, JavaScript kodunu bir metin dosyasına kaydedebilir ve `node filename.js` öğesini, bunu çalıştıracak bir parametre olarak dosya adıyla başlatabilirsiniz.

## <a name="connect-create-table-and-insert-data"></a>Bağlanma, tablo oluşturma ve veri ekleme
Aşağıdaki kodu kullanarak bağlanın ve **CREATE TABLE** ve **INSERT INTO** SQL deyimlerini kullanarak verileri yükleyin.
[pg.Client](https://github.com/brianc/node-postgres/wiki/Client) nesnesi, PostgreSQL sunucusuyla arabirim oluşturmak için kullanılır. [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) işlevi, sunucuyla bağlantı kurmak için kullanılır. [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) işlevi, PostgreSQL veritabanına karşı SQL sorgusunu yürütmek için kullanılır. 

Ana bilgisayar, veritabanı adı, kullanıcı ve parola parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;
    else
    {
        queryDatabase();
    }
});

function queryDatabase()
{
    client.query(
        ' \
            DROP TABLE IF EXISTS inventory; \
            CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER); \
            INSERT INTO inventory (name, quantity) VALUES (\'banana\', 150); \
            INSERT INTO inventory (name, quantity) VALUES (\'orange\', 154); \
            INSERT INTO inventory (name, quantity) VALUES (\'apple\', 100); \
        ',
        function (err)
    {
        console.log("Connection established");

        if (err)
            throw err;
        else
        {
            client.end(function (err)
            {
                if (err)
                    throw err;

                // Else closing connection finished without error
                console.log("Closed client connection");
            });

            console.log("Finished execution, exiting now");
            process.exit()
        }
    });
}
```

## <a name="read-data"></a>Verileri okuma
**SELECT** SQL deyimini kullanarak bağlanmak ve verileri okumak için aşağıdaki kodu kullanın. [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) nesnesi, PostgreSQL sunucusuyla arabirim oluşturmak için kullanılır. [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) işlevi, sunucuyla bağlantı kurmak için kullanılır. [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) işlevi, PostgreSQL veritabanına karşı SQL sorgusunu yürütmek için kullanılır. 

Ana bilgisayar, veritabanı adı, kullanıcı ve parola parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};


const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;

    else
    {
        console.log("Connected to Azure Database for PostgreSQL server:" + config.host);
        queryDatabase();
    }
});

function queryDatabase()
{
    // Declare array to hold query result set
    const results = [];

    console.log("Running query to PostgreSQL server:" + config.host);

    // Perform query
    var query = client.query('SELECT * FROM inventory;');

    // Print result set
    query.on('row', function(row)
    {
        console.log("Read " + JSON.stringify(row));
    });

    // Exit program after execution
    query.on('end', function(row)
    {
        console.log("Finished execution, exiting now");
        process.exit()
    });
}
```

## <a name="update-data"></a>Verileri güncelleştirme
**UPDATE** SQL deyimini kullanarak bağlanmak ve verileri okumak için aşağıdaki kodu kullanın. [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) nesnesi, PostgreSQL sunucusuyla arabirim oluşturmak için kullanılır. [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) işlevi, sunucuyla bağlantı kurmak için kullanılır. [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) işlevi, PostgreSQL veritabanına karşı SQL sorgusunu yürütmek için kullanılır. 

Ana bilgisayar, veritabanı adı, kullanıcı ve parola parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;
    else
    {
        queryDatabase();
    }   
});

function queryDatabase()
{
    client.query('UPDATE inventory SET quantity= 1000 WHERE name=\'banana\';', function (err, result)
    {
        console.log("Connection established");

        if (err)
            throw err;
        else
        {
            client.end(function (err)
            {
                if (err)
                    throw err;
                
                // Else closing connection finished without error
                console.log("Closed client connection");
            });             
        }

        console.log("Finished execution, exiting now");
        process.exit()
    });
}
```

## <a name="delete-data"></a>Verileri silme
**DELETE** SQL deyimini kullanarak bağlanmak ve verileri okumak için aşağıdaki kodu kullanın. [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) nesnesi, PostgreSQL sunucusuyla arabirim oluşturmak için kullanılır. [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) işlevi, sunucuyla bağlantı kurmak için kullanılır. [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) işlevi, PostgreSQL veritabanına karşı SQL sorgusunu yürütmek için kullanılır. 

Ana bilgisayar, veritabanı adı, kullanıcı ve parola parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;
    else
    {
        queryDatabase();
    }   
});

function queryDatabase()
{
    client.query('DELETE FROM inventory WHERE name=\'apple\';', function (err, result)
    {
        console.log("Connection established");
        
        if (err)
            throw err;
        else
        {
            client.end(function (err)
            {
                if (err)
                    throw err;
                
                // Else closing connection finished without error
                console.log("Closed client connection");
            });

            console.log("Finished execution, exiting now");
            process.exit()          
        }
    }); 
}
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı aktarma ve İçeri aktarmayı kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)

