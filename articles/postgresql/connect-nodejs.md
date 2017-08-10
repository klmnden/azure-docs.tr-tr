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
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: ddc364f2a0b8a8bb0a4a2c3a563c007470415991
ms.contentlocale: tr-tr
ms.lasthandoff: 08/10/2017

---

# <a name="azure-database-for-postgresql-use-nodejs-to-connect-and-query-data"></a>PostgreSQL için Azure Veritabanı: Bağlanmak ve veri sorgulamak için Node.js’yi kullanma
Bu hızlı başlangıçta, Windows, Ubuntu Linux ve Mac platformlarından [Node.js](https://nodejs.org/) kullanılarak PostgreSQL için Azure Veritabanı’na nasıl bağlanılacağı gösterilmiştir. Hızlı başlangıçta, veritabanında verileri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerinin nasıl kullanılacağı da gösterilmiştir. Bu makaledeki adımlarda, Node.js kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve PostgreSQL için Azure Veritabanı ile çalışmaya yeni başladığınız varsayılır.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıçta, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynaklar kullanılmaktadır:
- [DB Oluşturma - Portal](quickstart-create-server-database-portal.md)
- [DB oluşturma - CLI](quickstart-create-server-database-azure-cli.md)

Şunları da yapmanız gerekir:
- [Node.js](https://nodejs.org)’yi yükleme

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
PostgreSQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve kısa süre önce oluşturduğunuz **mypgserver-20170401** sunucusunu aratın.
3. **mypgserver-20170401** sunucu adına tıklayın.
4. Sunucunun **Genel Bakış** sayfasını seçin. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin.
 ![PostgreSQL için Azure Veritabanı - Sunucu Yöneticisi Oturum Açma Bilgileri](./media/connect-nodejs/1-connection-string.png)
5. Sunucunuzun oturum açma bilgilerini unuttuysanız **Genel Bakış** sayfasına giderek Sunucu yöneticisi oturum açma adını görüntüleyin ve gerekirse parolayı sıfırlayın.

## <a name="running-the-javascript-code-in-nodejs"></a>Node.js’de JavaScript kodunu çalıştırma
`node` yazarak Node.js’yi bash kabuğundan veya windows komut isteminden başlatabilir, ardından isteme kopyalayıp yapıştırarak örnek JavaScript kodunu etkili bir şekilde çalıştırabilirsiniz. Alternatif olarak, JavaScript kodunu bir metin dosyasına kaydedebilir ve `node filename.js` öğesini, bunu çalıştıracak bir parametre olarak dosya adıyla başlatabilirsiniz.

## <a name="connect-create-table-and-insert-data"></a>Bağlanma, tablo oluşturma ve veri ekleme
Aşağıdaki kodu kullanarak bağlanın ve **CREATE TABLE** ve **INSERT INTO** SQL deyimlerini kullanarak verileri yükleyin.
[pg.Client](https://github.com/brianc/node-postgres/wiki/Client) nesnesi, PostgreSQL sunucusuyla arabirim oluşturmak için kullanılır. [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) işlevi, sunucuyla bağlantı kurmak için kullanılır. [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) işlevi, PostgreSQL veritabanına karşı SQL sorgusunu yürütmek için kullanılır. 

Ana bilgisayar, veritabanı adı, kullanıcı ve parola parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```javascript
const pg = require('pg');

const config = {
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: '<name_of_database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DROP TABLE IF EXISTS inventory;
        CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
        INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
        INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
        INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    `;

    client
        .query(query)
        .then(() => {
            console.log('Table created successfully!');
            client.end(console.log('Closed client connection'));
        })
        .catch(err => console.log(err))
        .then(() => {
            console.log('Finished execution, exiting now');
            process.exit();
        });
}
```

## <a name="read-data"></a>Verileri okuma
**SELECT** SQL deyimini kullanarak bağlanmak ve verileri okumak için aşağıdaki kodu kullanın. [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) nesnesi, PostgreSQL sunucusuyla arabirim oluşturmak için kullanılır. [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) işlevi, sunucuyla bağlantı kurmak için kullanılır. [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) işlevi, PostgreSQL veritabanına karşı SQL sorgusunu yürütmek için kullanılır. 

Ana bilgisayar, veritabanı adı, kullanıcı ve parola parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```javascript
const pg = require('pg');

const config = {
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: '<name_of_database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else { queryDatabase(); }
});

function queryDatabase() {
  
    console.log(`Running query to PostgreSQL server: ${config.host}`);

    const query = 'SELECT * FROM inventory;';

    client.query(query)
        .then(res => {
            const rows = res.rows;

            rows.map(row => {
                console.log(`Read: ${JSON.stringify(row)}`);
            });

            process.exit();
        })
        .catch(err => {
            console.log(err);
        });
}
```

## <a name="update-data"></a>Verileri güncelleştirme
**UPDATE** SQL deyimini kullanarak bağlanmak ve verileri okumak için aşağıdaki kodu kullanın. [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) nesnesi, PostgreSQL sunucusuyla arabirim oluşturmak için kullanılır. [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) işlevi, sunucuyla bağlantı kurmak için kullanılır. [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) işlevi, PostgreSQL veritabanına karşı SQL sorgusunu yürütmek için kullanılır. 

Ana bilgisayar, veritabanı adı, kullanıcı ve parola parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```javascript
const pg = require('pg');

const config = {
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: '<name_of_database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `UPDATE inventory 
                   SET quantity= 1000 WHERE name='banana';`;

    client
        .query(query)
        .then(() => {
            console.log('Update completed succesfully!');
            client.end(console.log('Closed client connection'));
        })
        .catch(err => console.log(err))
        .then(() => {
            console.log('Finished execution, exiting now');
            process.exit();
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
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)

