---
title: MySQL Workbench’ten MariaDB için Azure Veritabanı’na bağlanma
description: Bu Hızlı Başlangıçta, MySQL Workbench kullanarak MariaDB için Azure Veritabanı'na bağlanmak ve buradan veri sorgulamak için uygulanması gereken adımlar verilmektedir.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.custom: mvc
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: b3a4d00381361b5299e86b959d9775318ae81e88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998262"
---
# <a name="azure-database-for-mariadb-use-mysql-workbench-to-connect-and-query-data"></a>MariaDB için Azure Veritabanı: MySQL Workbench kullanarak bağlanma ve veri sorgulama
Bu hızlı başlangıçta MySQL Workbench uygulamasını kullanarak MariaDB için Azure Veritabanı'na nasıl bağlanacağınız gösterilmiştir. 

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıçta, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynaklar kullanılmaktadır:
- [Azure portalını kullanarak MariaDB için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MariaDB için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mariadb-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a>MySQL Workbench’i yükleme
[MySQL web sitesinden](https://dev.mysql.com/downloads/workbench/) MySQL Workbench’i indirip bilgisayarınıza yükleyin.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
MariaDB için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.

2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**'a tıklayın ve oluşturduğunuz sunucuyu (örneğin, **mydemoserver**) arayın.

3. Sunucunun adına tıklayın.

4. Sunucunun **Genel Bakış** panelinden **Sunucu adı** ile **Sunucu yöneticisi oturum açma adı**’nı not alın. Parolanızı unutursanız, bu panelden parolayı da sıfırlayabilirsiniz.
 ![MariaDB için Azure Veritabanı sunucusu adı](./media/connect-workbench/1_server-overview-name-login.png)

## <a name="connect-to-server-using-mysql-workbench"></a>MySQL Workbench kullanarak sunucuya bağlanma 
MySQL Workbench ile MariaDB için Azure Veritabanı sunucusuna bağlanmak için aşağıdaki adımları kullanın:

1.  Bilgisayarınızda MySQL Workbench uygulamasını başlatın. 

2.  **Yeni Bağlantı Oluştur** iletişim kutusundaki **Parametreler** sekmesine aşağıdaki bilgileri girin:

    ![yeni bağlantı oluştur](./media/connect-workbench/2-setup-new-connection.png)

    | **Ayar** | **Önerilen değer** | **Alan açıklaması** |
    |---|---|---|
    |   Bağlantı Adı | Tanıtım Bağlantısı | Bu bağlantı için bir etiket belirtin. |
    | Bağlantı Yöntemi | Standart (TCP/IP) | Standart (TCP/IP) yeterlidir. |
    | Ana Bilgisayar Adı | *sunucu adı* | MariaDB için Azure Veritabanını oluştururken kullandığınız sunucu adı değerini belirtin. Gösterilen örnek sunucumuz: mydemoserver.mariadb.database.azure.com. Örnekte gösterildiği gibi tam etki alanı adını (\*.mariadb.database.azure.com) kullanın. Sunucu adınızı anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin.  |
    | Bağlantı noktası | 3306 | MariaDB için Azure Veritabanı’na bağlanırken her zaman bağlantı noktası olarak 3306 kullanın. |
    | Kullanıcı adı |  *sunucu yöneticisi oturum açma adı* | MariaDB için Azure Veritabanı’nı oluştururken girdiğiniz sunucu yöneticisi oturum açma kullanıcı adını yazın. Bizim örnek kullanıcı adımız myadmin@mydemoserver. Kullanıcı adını anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. Biçim şöyledir: *username@servername*.
    | Parola | parolanız | Parolayı kaydetmek için **Kasada Depola...** düğmesine tıklayın. |

3.   Tüm parametrelerin doğru yapılandırılıp yapılandırılmadığını test etmek için **Bağlantıyı Sına**’ya tıklayın. 

4.   Bağlantıyı kaydetmek için **Tamam**’a tıklayın. 

5.   **MySQL Bağlantıları** listesinde sunucunuza karşılık gelen kutucuğa tıklayıp bağlantının kurulmasını bekleyin.

        Sorgularınızı yazabileceğiniz boş bir düzenleyici içeren yeni bir SQL sekmesi açılır.
    
        > [!NOTE]
        > Varsayılan olarak SSL bağlantısının güvenli olması gerekir ve MariaDB için Azure Veritabanı sunucunuzda zorunlu tutulur. Normalde MySQL Workbench'in sunucunuza bağlanması için SSL sertifikalı ek yapılandırma gerekmez ancak SSL CA sertifikasını MySQL Workbench sunucunuza bağlamanızı öneririz. SSL’yi devre dışı bırakmanız gerekiyorsa, Azure portalına gidin ve Bağlantı güvenliği sayfasına tıklayarak SSL bağlantısını zorunlu kıl iki durumlu düğmesini devre dışı bırakın.

## <a name="create-table-insert-read-update-and-delete-data"></a>Tablo oluşturma, ekleme, okuma, güncelleştirme ve verileri silme
1. Bazı örnek verileri görmek için örnek SQL kodunu kopyalayıp boş bir SQL sekmesine tıklayın.

    Bu kod quickstartdb adlı boş bir veritabanı oluşturur ve sonra stok adlı bir örnek tablo oluşturur. Birkaç satır ekler ve sonra bu satırları okur. Verileri bir güncelleştirme deyimiyle değiştirir ve satırları yeniden okur. Son olarak bir satırı siler ve satırları yeniden okur.
    
    ```sql
    -- Create a database
    -- DROP DATABASE IF EXISTS quickstartdb;
    CREATE DATABASE quickstartdb;
    USE quickstartdb;
    
    -- Create a table and insert rows
    DROP TABLE IF EXISTS inventory;
    CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
    INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
    INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
    INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    
    -- Read
    SELECT * FROM inventory;
    
    -- Update
    UPDATE inventory SET quantity = 200 WHERE id = 1;
    SELECT * FROM inventory;
    
    -- Delete
    DELETE FROM inventory WHERE id = 2;
    SELECT * FROM inventory;
    ```

    Ekran görüntüsünde, SQL Workbench’te örnek bir SQL kodu ve kod çalıştırıldıktan sonra oluşan çıktı gösterilmektedir.
    
    ![Örnek SQL kodunu çalıştırmak için MySQL Workbench SQL Sekmesi](media/connect-workbench/3-workbench-sql-tab.png)

2. Örnek SQL Kodunu çalıştırmak için **SQL Dosyası** sekmesindeki araç çubuğunda bulunan şimşek simgesine tıklayın.
3. Sayfanın ortasındaki **Sonuç Izgarası** bölümünde üç sekme halinde sonuçların yer aldığına dikkat edin. 
4. Sayfanın en altındaki **Çıktı** listesine dikkat edin. Her komutun durumu gösterilir. 

MySQL Workbench kullanarak MariaDB için Azure Veritabanı’na bağlandınız ve SQL dilini kullanarak verileri sorguladınız.

<!--
## Next steps
> [!div class="nextstepaction"]
> [Migrate your database using Export and Import](./concepts-migrate-import-export.md)
-->