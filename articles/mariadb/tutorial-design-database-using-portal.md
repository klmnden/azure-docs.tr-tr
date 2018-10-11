---
title: 'Öğretici: Azure portalını kullanarak MariaDB için Azure Veritabanı tasarlama'
description: Bu öğreticide, Azure Portalı kullanarak MariaDB için Azure Veritabanı sunucusunun ve veritabanının nasıl oluşturulup yönetileceği açıklanır.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: tutorial
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 2e1cd0d28c544b2e4c5dc86c7a6db39a5d20c1ed
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991020"
---
# <a name="tutorial-design-an-azure-database-for-mariadb-database-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak MariaDB için Azure Veritabanı tasarlama
MariaDB için Azure Veritabanı, bulutta üst düzeyde kullanılabilir olan MySQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure portalını kullanarak, sunucunuzu kolayca yönetebilir ve bir veritabanı tasarlayabilirsiniz.

Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> * MariaDB için Azure Veritabanı oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için mysql komut satırı aracı kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Sık kullandığınız web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-mariadb-server"></a>MariaDB için Azure Veritabanı sunucusu oluşturma
MariaDB için Azure Veritabanı sunucusu, tanımlı bir dizi işlem ve depolama kaynağı <!--[compute and storage resources](./concepts-compute-unit-and-storage.md)--> ile oluşturulur. Sunucu, [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) içinde oluşturulur.

1. Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin.

2. Hizmeti bulmak için arama kutusuna **MariaDB için Azure Veritabanı** yazın.
   
   ![MySQL’e gidin](./media/tutorial-design-database-using-portal/1-Navigate-to-mariadb.png)

3. **MariaDB için Azure Veritabanı** kutucuğuna ve ardından **Oluştur**’a tıklayın. MariaDB için Azure Veritabanı formunu doldurun.
   
   ![Oluşturma formu](./media/tutorial-design-database-using-portal/2-create-form.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması** 
    ---|---|---
    Sunucu adı | Benzersiz sunucu adı | MariaDB için Azure Veritabanı sunucunuzu tanımlayan benzersiz bir ad seçin. Örneğin, demosunucum. Girdiğiniz sunucu adına *.mariadb.database.azure.com* etki alanı adı eklenir. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ila 63 karakter arası içermelidir.
    Abonelik | Aboneliğiniz | Sunucunuz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | *myresourcegroup* | Yeni veya mevcut bir kaynak grubu adı girin.    Kaynak grubu|*myresourcegroup*| Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.
    Kaynak seçme | *Boş* | Sıfırdan yeni bir sunucu oluşturmak için *Boş*’u seçin. (Mevcut bir MariaDB için Azure Veritabanı sunucusunun coğrafi yedeğinden bir sunucu oluşturuyorsanız, *Yedek*'i seçersiniz).
    Sunucu yöneticisi oturum açma | myadmin | Sunucuya bağlanırken kullanılacak oturum açma hesabı. Yönetici oturum açma adı **azure_superuser**, **admin**, **administrator**, **root**, **guest** veya **public** olamaz.
    Parola | *Tercih ettiğiniz* | Sunucu yönetici hesabı için yeni bir parola girin. 8 ila 128 karakter arası içermelidir. Parolanız şu üç kategoride yer alan karakterlerden oluşmalıdır: İngilizce büyük ve küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #, %, vb.).
    Parolayı onayla | *Tercih ettiğiniz*| Yönetici hesabı parolasını onaylayın.
    Konum | *Kullanıcılarınıza en yakın bölge*| Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
    Sürüm | *En son sürüm*| En son sürüm (başka bir sürüm gerektiren belirli gereksinimleriniz yoksa).
    Fiyatlandırma katmanı | **Genel Amaçlı**, **5. Nesil**, **2 sanal çekirdek**, **5 GB**, **7 gün**, **Coğrafi Olarak Yedekli** | Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Fiyatlandırma katmanı**'nı seçin. Ardından, **Genel Amaçlı** sekmesini seçin. *5. Nesil*, *2 sanal çekirdek*, *5 GB* ve *7 gün*; **İşlem Nesli**, **Sanal Çekirdek**, **Depolama** ve **Yedekleme Bekletme Dönemi** için varsayılan değerlerdir. Bu kaydırıcıları olduğu gibi bırakabilirsiniz. Coğrafi olarak yedekli depolamada sunucu yedeklerinizi etkinleştirmek için, **Fazladan Yedek Seçenekleri**’nde **Coğrafi Olarak Yedeklemeli**’yi seçin. Bu fiyatlandırma katmanı seçimini kaydetmek için **Tamam**’ı seçin. Sonraki ekran görüntüsü bu seçimleri yakalar.
    
   ![Fiyatlandırma katmanı](./media/tutorial-design-database-using-portal/3-pricing-tier.png)

4. **Oluştur**’a tıklayın. Bir veya iki dakika içinde, bulutta yeni bir MariaDB için Azure Veritabanı sunucusu çalışmaya başlayacaktır. Dağıtım işlemini izlemek için araç çubuğunda **Bildirimler** düğmesine tıklayabilirsiniz.

## <a name="configure-firewall"></a>Güvenlik duvarını yapılandırma
MariaDB için Azure Veritabanları bir güvenlik duvarı tarafından korunur. Varsayılan olarak, sunucuya ve sunucu içindeki veritabanlarına yönelik tüm bağlantılar reddedilir. MariaDB için Azure Veritabanı’na ilk kez bağlanmadan önce, güvenlik duvarını istemcinin genel ağ IP adresini (veya IP adres aralığını) ekleyecek şekilde yapılandırın.

1. Yeni oluşturduğunuz sunucuya ve ardından **Bağlantı güvenliği**’ne tıklayın.
   
   ![Bağlantı güvenliği](./media/tutorial-design-database-using-portal/1-Connection-security.png)
2. **IP’mi Ekle** seçeneğini kullanabilir veya güvenlik duvarı kurallarını buradan yapılandırabilirsiniz. Kuralları oluşturduktan sonra **Kaydet**’e tıklamayı unutmayın.
Artık mysql komut satırı aracını veya MySQL Workbench GUI aracını kullanarak sunucuya bağlanabilirsiniz.

> [!TIP]
> MariaDB için Azure Veritabanı sunucusu, 3306 numaralı bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece MariaDB için Azure Veritabanı sunucusuna bağlanamazsınız.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
Azure portalından MariaDB için Azure Veritabanı sunucunuz için tam **Sunucu adı** ve **Sunucu yöneticisi oturum açma adı** alın. Tam sunucu adını, mysql komut satırı aracı kullanarak sunucunuza bağlanmak için kullanırsınız. 

1. [Azure portalı](https://portal.azure.com/)’nda soldaki menüden **Tüm kaynaklar**’a tıklayın, adı yazın ve MariaDB için Azure Veritabanı sunucunuzu arayın. Ayrıntıları görüntülemek için sunucu adını seçin.

2. **Genel bakış** sayfasında, **Sunucu Adı** ve **Sunucu yöneticisi oturum açma adı** değerlerini not edin. Panoya kopyalamak için her alanın yanındaki kopyala düğmesine tıklayabilirsiniz.
   ![4-2 sunucu özellikleri](./media/tutorial-design-database-using-portal/2-server-properties.png)

Bu örnekte sunucu adı *mydemoserver.mariadb.database.azure.com*, sunucu yöneticisi oturum açma bilgileri ise *myadmin@mydemoserver* şeklindedir.

## <a name="connect-to-the-server-using-mysql"></a>mysql kullanarak sunucuya bağlanma
MariaDB için Azure Veritabanı sunucunuzla bağlantı kurmak üzere [mysql komut satırı aracını](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) kullanın. mysql komut satırı aracını tarayıcıdaki Azure Cloud Shell’den veya yerel olarak yüklenmiş mysql araçlarını kullanarak kendi makinenizden çalıştırabilirsiniz. Azure Cloud Shell’i başlatmak için bu makaledeki bir kod bloğu üzerinde `Try It` düğmesine tıklayın veya Azure portalını ziyaret ederek sağ üstteki araç çubuğunda `>_` simgesine tıklayın. 

Bağlanma komutunu yazın:
```azurecli-interactive
mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

## <a name="create-a-blank-database"></a>Boş veritabanı oluşturma
Sunucuya bağlandıktan sonra, birlikte çalışacağınız boş bir veritabanı oluşturun.
```sql
CREATE DATABASE mysampledb;
```

İstemde, bağlantıyı bu yeni oluşturulan veritabanına değiştirmek için şu komutu çalıştırın:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Veritabanında tablo oluşturma
Artık MariaDB için Azure Veritabanı'na nasıl bağlanacağınızı bildiğinize göre bazı temel görevleri tamamlayabilirsiniz:

İlk olarak, bir tablo oluşturun ve bu tabloya bazı veriler yükleyin. Envanter bilgilerini depolayan bir tablo oluşturalım.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Tablolara veri yükleme
Bir tablonuz olduğuna göre içine bazı veriler ekleyin. Açık komut istemi penceresinde, birkaç veri satırı eklemek için şu sorguyu çalıştırın.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Daha önce oluşturduğunuz tabloda artık iki satırlık örnek verileriniz var.

## <a name="query-and-update-the-data-in-the-tables"></a>Tablolardaki verileri sorgulama ve güncelleştirme
Veritabanı tablosundan bilgi almak için şu sorguyu yürütün.
```sql
SELECT * FROM inventory;
```

Ayrıca tablolardaki verileri de güncelleştirebilirsiniz.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Siz veri aldıkça satır güncelleştirilir.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Bir veritabanını daha önceki bir noktaya geri yükleme
Önemli bir veritabanı tablosunu yanlışlıkla sildiğinizi ve verileri kolayca kurtaramayacağınızı düşünün. MariaDB için Azure Veritabanı, sunucuyu bir zaman noktasına geri yükleyerek, veritabanlarının bir kopyasının yeni sunucuda oluşturulmasını sağlar. Bu yeni sunucuyu silinen verilerinizi kurtarmak için kullanabilirsiniz. Şu adımlar, örnek sunucuyu tablo eklenmeden önceki bir noktaya geri yükler.

1. Azure portalında MariaDB için Azure Veritabanınızı bulun. **Genel bakış** sayfası üzerinde, araç çubuğunda **Geri yükle**’ye tıklayın. Geri Yükle sayfası açılır.

   ![10-1 veritabanını geri yükleme](./media/tutorial-design-database-using-portal/1-restore-a-db.png)

2. **Geri Yükle** formunu gerekli bilgiler ile doldurun.
   
   ![10-2 geri yükleme formu](./media/tutorial-design-database-using-portal/2-restore-form.png)
   
   - **Geri yükleme noktası**: Listelenen zaman dilimi içerisindeki geri yüklemek istediğiniz noktayı seçin. Yerel saat diliminizi UTC'ye dönüştürdüğünüzden emin olun.
   - **Yeni sunucuya geri yükleme**: Geri yüklemek istediğiniz yeni bir sunucu adı sağlayın.
   - **Konum**: Bölge, kaynak sunucu ile aynıdır ve değiştirilemez.
   - **Fiyatlandırma katmanı**: Fiyatlandırma katmanı, kaynak sunucu ile aynıdır ve değiştirilemez.
   
3. Sunucuyu, tablo silinmeden önceki <!--[restore to a point in time](./howto-restore-server-portal.md)--> zamana geri yüklemek için **Tamam**’a tıklayın. Bir sunucuyu geri yüklemek, belirttiğiniz zamandan itibaren sunucunun yeni bir kopyasını oluşturur. 

<!--## Next steps
In this tutorial, you use the Azure portal to learned how to:

> [!div class="checklist"]
> * Create an Azure Database for MariaDB
> * Configure the server firewall
> * Use mysql command-line tool to create a database
> * Load sample data
> * Query data
> * Update data
> * Restore data

><> [!div class="nextstepaction"]
> [How to connect applications to Azure Database for MariaDB](./howto-connection-string.md)-->
