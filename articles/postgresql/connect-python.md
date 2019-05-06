---
title: -Tek bir sunucu PostgreSQL için Azure veritabanı'na bağlanmak için Python kullanma
description: Bu hızlı başlangıçta, bağlanıp - tek bir sunucu PostgreSQL için Azure veritabanı'ndan veri sorgulamak için kullanabileceğiniz Python kod örneği sağlanmıştır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc, devcenter
ms.devlang: python
ms.topic: quickstart
ms.date: 5/6/2019
ms.openlocfilehash: 4d7988ad590e6d57d9da37f46557f99fccaad294
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067233"
---
# <a name="azure-database-for-postgresql---single-server-use-python-to-connect-and-query-data"></a>PostgreSQL - tek bir sunucu için Azure veritabanı: Bağlanmak ve veri sorgulamak için Python kullanma
Bu hızlı başlangıçta [Python](https://python.org) kullanarak PostgreSQL için bir Azure Veritabanı'na nasıl bağlanacağınız gösterilmiştir. macOS, Ubuntu Linux ve Windows platformlarındaki veritabanında yer alan verileri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerinin nasıl kullanıldığını da gösterir. Bu makaledeki adımlarda, Python kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve PostgreSQL için Azure Veritabanı ile çalışmaya yeni başladığınız varsayılır.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıçta, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynaklar kullanılmaktadır:
- [DB Oluşturma - Portal](quickstart-create-server-database-portal.md)
- [DB oluşturma - CLI](quickstart-create-server-database-azure-cli.md)

Şunları da yapmanız gerekir:
- [Python](https://www.python.org/downloads/)'ı yükleme
- [pip](https://pip.pypa.io/en/stable/installing/) paketi yüklü ([python.org](https://python.org) adresinden indirilen Python 2 >=2.7.9 veya Python 3 >=3.4 ikili dosyalarıyla çalışıyorsanız pip zaten yüklüdür).

## <a name="install-the-python-connection-libraries-for-postgresql"></a>PostgreSQL için Python bağlantı kitaplıklarını yükleme
Veritabanına bağlanmanızı ve bu veritabanını sorgulamanızı sağlayan [psycopg2](http://initd.org/psycopg/docs/install.html) paketini yükleyin. psycopg2, en yaygın platformlar (Linux, OSX, Windows) için [wheel](https://pythonwheels.com/) paketleri biçiminde [PyPI üzerinde sağlanmaktadır](https://pypi.python.org/pypi/psycopg2/). Tüm bağımlılıklar dahil olmak üzere modülün ikili sürümünü almak için pip yüklemesini kullanın.

1. Kendi bilgisayarınızda bir komut satırı arabirimi başlatın:
    - Linux’ta Bash kabuğunu başlatın.
    - macOS’ta Terminal’i başlatın.
    - Windows’da, Başlat Menüsünden Komut İstemi’ni başlatın.
2. Aşağıdaki gibi bir komut çalıştırarak en son pip sürümünü kullandığınızdan emin olun:
    ```cmd
    pip install -U pip
    ```

3. psycopg2 paketini yüklemek için şu komutu çalıştırın:
    ```cmd
    pip install psycopg2
    ```

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
PostgreSQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**'a tıklayın ve oluşturduğunuz sunucuyu (örneğin, **mydemoserver**) arayın.
3. Sunucunun adına tıklayın.
4. Sunucunun **Genel Bakış** panelinden **Sunucu adı** ile **Sunucu yöneticisi oturum açma adı**’nı not alın. Parolanızı unutursanız, bu panelden parolayı da sıfırlayabilirsiniz.
 ![PostgreSQL için Azure Veritabanı sunucu adı](./media/connect-python/1-connection-string.png)

## <a name="how-to-run-python-code"></a>Python kodu çalıştırma
Bu makale, her biri belirli bir işlevi gerçekleştiren toplam dört kod örneği içerir. Aşağıdaki yönergelerde metin dosyası oluşturma, kod bloğu ekleme ve daha sonra çalıştırmak üzere dosyayı kaydetme gösterilir. Her kod bloğu için birer tane olmak üzere dört ayrı dosya oluşturduğunuzdan emin olun.

- Sık kullandığınız metin düzenleyicisini kullanarak yeni bir dosya oluşturun.
- Aşağıdaki bölümlerde yer alan kod örneklerinden birini kopyalayın ve metin dosyasına yapıştırın. **host**, **dbname**, **user** ve **password** parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.
- Dosyayı .py uzantısıyla (örneğin postgres.py) proje klasörünüze kaydedin. Windows’da çalıştırıyorsanız, dosyayı kaydederken UTF-8 kodlamasını seçtiğinizden emin olun. 
- Komut İstemi, Terminal veya Bash kabuğunu çalıştırın ve proje klasörünüzün dizinini `cd postgres` örneğindeki gibi değiştirin.
-  Kodu çalıştırmak için Python komutunu ve ardından dosya adını yazın; örneğin `Python postgres.py`.

> [!NOTE]
> Python sürüm 3'ten başlayarak, hatasıyla karşılaşabilirsiniz `SyntaxError: Missing parentheses in call to 'print'` aşağıdaki kod bloklarını çalıştırırken: Bu hatayla karşılaşırsanız, her `print "string"` komutu çağrısını parantez kullanan bir işlev çağrısıyla değiştirin; örneğin, `print("string")`.

## <a name="connect-create-table-and-insert-data"></a>Bağlanma, tablo oluşturma ve veri ekleme
Bağlanmak ve **INSERT** SQL deyimiyle [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) işlevini kullanarak verileri yüklemek için aşağıdaki kodu kullanın. PostgreSQL veritabanında SQL sorgusu yürütmek için [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) işlevi kullanılır. host, dbname, user ve password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mydemoserver.postgres.database.azure.com"
user = "mylogin@mydemoserver"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print "Finished dropping table (if existed)"

# Create table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print "Finished creating table"

# Insert some data into table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print "Inserted 3 rows of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

Kod başarıyla çalıştıktan sonra çıktı aşağıdaki gibi görünür:

![Komut satırı çıkışı](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a>Verileri okuma
**SELECT** SQL deyimi ile [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) işlevini kullanarak, eklenen verileri okumak için aşağıdaki kodu kullanın. Bu işlev bir sorguyu kabul eder ve [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall) kullanılarak yinelenebilen bir sonuç kümesi döndürür. host, dbname, user ve password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mydemoserver.postgres.database.azure.com"
user = "mylogin@mydemoserver"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()

# Print all rows
for row in rows:
    print "Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2]))

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a>Verileri güncelleştirme
**UPDATE** SQL deyimiyle [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) işlevini kullanarak, eklediğiniz inventory satırını güncelleştirmek için aşağıdaki kodu kullanın. host, dbname, user ve password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mydemoserver.postgres.database.azure.com"
user = "mylogin@mydemoserver"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a>Verileri silme
**DELETE** SQL deyimiyle [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) işlevini kullanarak, daha önce eklediğiniz bir inventory öğesini silmek için aşağıdaki kodu kullanın. host, dbname, user ve password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mydemoserver.postgres.database.azure.com"
user = "mylogin@mydemoserver"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print "Deleted 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
