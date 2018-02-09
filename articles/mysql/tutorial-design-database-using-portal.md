---
title: "İlk MySQL veritabanı için Azure Veritabanınızı tasarlama - Azure Portal | Microsoft Docs"
description: "Bu öğreticide, Azure Portalı kullanarak MySQL sunucusu ve veritabanı için Azure Veritabanının nasıl oluşturulup yönetileceği açıklanır."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: tutorial
ms.date: 11/03/2017
ms.custom: mvc
ms.openlocfilehash: 76cccf9e2ce0a1e59b43646c43ac165d46dade4a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>İlk MySQL veritabanı için Azure Veritabanınızı tasarlama
MySQL için Azure Veritabanı, bulutta yüksek oranda kullanılabilir olan MySQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure portalını kullanarak, sunucunuzu kolayca yönetebilir ve bir veritabanı tasarlayabilirsiniz.

Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> * MySQL için Azure Veritabanı oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için mysql komut satırı aracı kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Sık kullandığınız web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
MySQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) içinde oluşturulur.

1. **Veritabanları** > **MySQL için Azure Veritabanı**’na gidin. **Veritabanları** kategorisi altında MySQL Sunucusunu bulamazsanız, kullanılabilir tüm veritabanı hizmetlerini görüntülemek için **Tümünü gör**’e tıklayın. Ayrıca arama kutusuna **MySQL için Azure Veritabanı** yazarak hizmeti hızlıca bulabilirsiniz.
![2-1 MySQL’ye git](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. **MySQL için Azure Veritabanı** kutucuğuna ve ardından **Oluştur**’a tıklayın.

Bu örnekte MySQL için Azure Veritabanı formunu aşağıdaki bilgilerle doldurun:

| **Ayar** | **Önerilen değer** | **Alan Açıklaması** |
|---|---|---|
| *Sunucu adı* | myserver4demo  | Sunucu adı genel olarak benzersiz olmalıdır. |
| *Abonelik* | mysubscription | Açılan listeden aboneliğinizi seçin. |
| *Kaynak grubu* | myresourcegroup | Bir kaynak grubu oluşturun veya mevcut olanlardan birini kullanın. |
| *Sunucu yöneticisi oturum açma bilgileri* | myadmin | Yönetici hesabı adını ayarlayın. |
| *Parola* |  | Güçlü bir yönetici hesabı parolası belirleyin. |
| *Parolayı onayla* |  | Yönetici hesabı parolasını onaylayın. |
| *Konum* |  | Kullanılabilir bir bölge seçin. |
| *Sürüm* | 5.7 | En son sürümü seçin. |
| *Performansı yapılandır* | Temel, 50 İşlem Birimi, 50 GB  | **Fiyatlandırma katmanı**, **İşlem Birimleri**, **Depolama (GB)** öğelerini seçip **Tamam**’a tıklayın. |
| *Panoya Sabitle* | İşaretli | Sunucuyu daha sonra kolayca bulabilmeniz için bu kutunun işaretlenmesi önerilir |
Sonra, **Oluştur**’a tıklayın. Bir veya iki dakika içinde, bulutta MySQL sunucusu için yeni bir Azure Veritabanı çalışmaya başlayacaktır. Dağıtım işlemini izlemek için araç çubuğunda **Bildirimler** düğmesine tıklayabilirsiniz.

## <a name="configure-firewall"></a>Güvenlik duvarını yapılandırma
MySQL için Azure Veritabanları bir güvenlik duvarı tarafından korunur. Varsayılan olarak, sunucuya ve sunucu içindeki veritabanlarına yönelik tüm bağlantılar reddedilir. MySQL için Azure Veritabanı’na ilk kez bağlanmadan önce, güvenlik duvarını istemcinin genel ağ IP adresini (veya IP adres aralığını) ekleyecek şekilde yapılandırın.

1. Yeni oluşturduğunuz sunucuya ve ardından **Bağlantı güvenliği**’ne tıklayın.
   ![3-1 Bağlantı güvenliği](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. **IP’mi Ekle** seçeneğini kullanabilir veya güvenlik duvarı kurallarını buradan yapılandırabilirsiniz. Kuralları oluşturduktan sonra **Kaydet**’e tıklamayı unutmayın.
Artık mysql komut satırı aracını veya MySQL Workbench GUI aracını kullanarak sunucuya bağlanabilirsiniz.

> [!TIP]
> MySQL için Azure Veritabanı sunucusu, 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece Azure MySQL sunucusuna bağlanamazsınız.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
Azure portalından MySQL için Azure Veritabanı sunucunuz için tam **Sunucu adı** ve **Sunucu yöneticisi oturum açma adını** alın. Tam sunucu adını, mysql komut satırı aracı kullanarak sunucunuza bağlanmak için kullanırsınız. 

1. [Azure portalı](https://portal.azure.com/)’nda soldaki menüden **Tüm kaynaklar**’a tıklayın, adı yazın ve MySQL için Azure Veritabanı sunucunuzu arayın. Ayrıntıları görüntülemek için sunucu adını seçin.

2. Ayarlar üst bilgisi altında, **Özellikler**’e tıklayın. **SUNUCU ADI** ve **SUNUCU YÖNETİCİSİ OTURUM AÇMA ADI** değerlerini not edin. Panoya kopyalamak için her alanın yanındaki kopyala düğmesine tıklayabilirsiniz.
   ![4-2 sunucu özellikleri](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

Bu örnekte sunucu adı *myserver4demo.mysql.database.azure.com*, sunucu yöneticisi oturum açma bilgileri ise *myadmin@myserver4demo* şeklindedir.

## <a name="connect-to-the-server-using-mysql"></a>mysql kullanarak sunucuya bağlanma
MySQL sunucusu için Azure Veritabanınız ile bağlantı kurmak üzere [mysql komut satırı aracını](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) kullanın. mysql komut satırı aracını tarayıcıdaki Azure Cloud Shell’den veya yerel olarak yüklenmiş mysql araçlarını kullanarak kendi makinenizden çalıştırabilirsiniz. Azure Cloud Shell’i başlatmak için bu makaledeki bir kod bloğu üzerinde `Try It` düğmesine tıklayın veya Azure portalını ziyaret ederek sağ üstteki araç çubuğunda `>_` simgesine tıklayın. 

Bağlanma komutunu yazın:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
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
Artık MySQL veritabanı için Azure Veritabanına nasıl bağlanacağınızı bildiğinize göre bazı temel görevleri tamamlayabilirsiniz:

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
Önemli bir veritabanı tablosunu yanlışlıkla sildiğinizi ve verileri kolayca kurtaramayacağınızı düşünün. MySQL için Azure Veritabanı, sunucuyu bir zaman noktasına geri yükleyerek, veritabanlarının bir kopyasının yeni sunucuda oluşturulmasını sağlar. Bu yeni sunucuyu silinen verilerinizi kurtarmak için kullanabilirsiniz. Şu adımlar, örnek sunucuyu tablo eklenmeden önceki bir noktaya geri yükler.

1. Azure portalında MySQL için Azure Veritabanınızı bulun. **Genel bakış** sayfası üzerinde, araç çubuğunda **Geri yükle**’ye tıklayın. Geri Yükle sayfası açılır.

   ![10-1 veritabanını geri yükleme](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. **Geri Yükle** formunu gerekli bilgiler ile doldurun.
   
   ![10-2 geri yükleme formu](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Geri yükleme noktası**: Listelenen zaman dilimi içerisindeki geri yüklemek istediğiniz noktayı seçin. Yerel saat diliminizi UTC'ye dönüştürdüğünüzden emin olun.
   - **Yeni sunucuya geri yükleme**: Geri yüklemek istediğiniz yeni bir sunucu adı sağlayın.
   - **Konum**: Bölge, kaynak sunucu ile aynıdır ve değiştirilemez.
   - **Fiyatlandırma katmanı**: Fiyatlandırma katmanı, kaynak sunucu ile aynıdır ve değiştirilemez.
   
3. Sunucuyu, tablo silinmeden önceki [bir zamana geri yüklemek](./howto-restore-server-portal.md) için **Tamam**’a tıklayın. Bir sunucuyu geri yüklemek, belirttiğiniz zamandan itibaren sunucunun yeni bir kopyasını oluşturur. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalını şunları yapmayı öğrenmek için kullanırsınız:

> [!div class="checklist"]
> * MySQL için Azure Veritabanı oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için mysql komut satırı aracı kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

> [!div class="nextstepaction"]
> [MySQL için Azure Veritabanına nasıl uygulama bağlanır](./howto-connection-string.md)
