---
title: 'Öğretici: Azure portalını kullanarak MySQL için Azure veritabanı tasarlama'
description: Bu öğreticide, oluşturma ve MySQL server ve Azure portalını kullanarak bir veritabanı için Azure veritabanı'nı yönetme açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: tutorial
ms.date: 03/20/2018
ms.custom: mvc
ms.openlocfilehash: d9c6a16dd7e6c32a71d496abe8a67e23cc075a6d
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515815"
---
# <a name="tutorial-design-an-azure-database-for-mysql-database-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak MySQL için Azure veritabanı tasarlama
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

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Sık kullandığınız web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
MySQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) içinde oluşturulur.

1. **Veritabanları** > **MySQL için Azure Veritabanı**’na gidin. **Veritabanları** kategorisi altında MySQL Sunucusunu bulamazsanız, kullanılabilir tüm veritabanı hizmetlerini görüntülemek için **Tümünü gör**’e tıklayın. Ayrıca arama kutusuna **MySQL için Azure Veritabanı** yazarak hizmeti hızlıca bulabilirsiniz.
   
   ![MySQL’e gidin](./media/tutorial-design-database-using-portal/1-Navigate-to-MySQL.png)

2. **MySQL için Azure Veritabanı** kutucuğuna ve ardından **Oluştur**’a tıklayın. MySQL için Azure Veritabanı formunu doldurun.
   
   ![Oluşturma formu](./media/tutorial-design-database-using-portal/2-create-form.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması** 
    ---|---|---
    Sunucu adı | Benzersiz sunucu adı | Azure veritabanınızı MySQL sunucusuna tanıtan benzersiz bir ad seçin. Örneğin, demosunucum. Girdiğiniz sunucu adına *.mysql.database.azure.com* etki alanı adı eklenir. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ila 63 karakter arası içermelidir.
    Abonelik | Aboneliğiniz | Sunucunuz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | *myresourcegroup* | Yeni veya mevcut bir kaynak grubu adı girin.
    Kaynak seçme | *Boş* | Sıfırdan yeni bir sunucu oluşturmak için *Boş*’u seçin. (Mevcut bir MySQL için Azure Veritabanı sunucusunun bir coğrafi yedeğinden bir sunucu oluşturuyorsanız, *Yedek* seçeneğini belirleyin).
    Sunucu yöneticisi oturum açma | myadmin | Sunucuya bağlanırken kullanılacak oturum açma hesabı. Yönetici oturum açma adı **azure_superuser**, **admin**, **administrator**, **root**, **guest** veya **public** olamaz.
    Parola | *Tercih ettiğiniz* | Sunucu yönetici hesabı için yeni bir parola girin. 8 ila 128 karakter arası içermelidir. Parolanız şu kategorilerin üçünden karakterler içermelidir: İngilizce büyük harfler, İngilizce küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #, % vb.).
    Parolayı onayla | *Tercih ettiğiniz*| Yönetici hesabı parolasını onaylayın.
    Location | *Kullanıcılarınıza en yakın bölge*| Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
    Version | *En son sürüm*| En son sürüm (başka bir sürüm gerektiren belirli gereksinimleriniz yoksa).
    Fiyatlandırma katmanı | **Genel Amaçlı**, **5. Nesil**, **2 sanal çekirdek**, **5 GB**, **7 gün**, **Coğrafi Olarak Yedekli** | Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Fiyatlandırma katmanı**'nı seçin. Ardından, **Genel Amaçlı** sekmesini seçin. *5. Nesil*, *2 sanal çekirdek*, *5 GB* ve *7 gün*; **İşlem Nesli**, **Sanal Çekirdek**, **Depolama** ve **Yedekleme Bekletme Dönemi** için varsayılan değerlerdir. Bu kaydırıcıları olduğu gibi bırakabilirsiniz. Coğrafi olarak yedekli depolamada sunucu yedeklerinizi etkinleştirmek için, **Fazladan Yedek Seçenekleri**’nde **Coğrafi Olarak Yedeklemeli**’yi seçin. Bu fiyatlandırma katmanı seçimini kaydetmek için **Tamam**’ı seçin. Sonraki ekran görüntüsü bu seçimleri yakalar.
    
   ![Fiyatlandırma katmanı](./media/tutorial-design-database-using-portal/3-pricing-tier.png)

   > [!TIP]
   > İle **otomatik büyüme** etkin iş yükünüzü etkilemeden, ayrılmış bir sınır yaklaştığı olduğunda depolama sunucunuzu artırır.

3. **Oluştur**’a tıklayın. Bir veya iki dakika içinde, bulutta MySQL sunucusu için yeni bir Azure Veritabanı çalışmaya başlayacaktır. Dağıtım işlemini izlemek için araç çubuğunda **Bildirimler** düğmesine tıklayabilirsiniz.

## <a name="configure-firewall"></a>Güvenlik duvarını yapılandırma
MySQL için Azure Veritabanları bir güvenlik duvarı tarafından korunur. Varsayılan olarak, sunucuya ve sunucu içindeki veritabanlarına yönelik tüm bağlantılar reddedilir. MySQL için Azure Veritabanı’na ilk kez bağlanmadan önce, güvenlik duvarını istemcinin genel ağ IP adresini (veya IP adres aralığını) ekleyecek şekilde yapılandırın.

1. Yeni oluşturduğunuz sunucuya ve ardından **Bağlantı güvenliği**’ne tıklayın.
   
   ![Bağlantı güvenliği](./media/tutorial-design-database-using-portal/1-Connection-security.png)
2. **IP’mi Ekle** seçeneğini kullanabilir veya güvenlik duvarı kurallarını buradan yapılandırabilirsiniz. Kuralları oluşturduktan sonra **Kaydet**’e tıklamayı unutmayın.
Artık mysql komut satırı aracını veya MySQL Workbench GUI aracını kullanarak sunucuya bağlanabilirsiniz.

> [!TIP]
> MySQL için Azure Veritabanı sunucusu, 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece Azure MySQL sunucusuna bağlanamazsınız.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
Azure portalından MySQL için Azure Veritabanı sunucunuz için tam **Sunucu adı** ve **Sunucu yöneticisi oturum açma adını** alın. Tam sunucu adını, mysql komut satırı aracı kullanarak sunucunuza bağlanmak için kullanırsınız. 

1. [Azure portalı](https://portal.azure.com/)’nda soldaki menüden **Tüm kaynaklar**’a tıklayın, adı yazın ve MySQL için Azure Veritabanı sunucunuzu arayın. Ayrıntıları görüntülemek için sunucu adını seçin.

2. **Genel bakış** sayfasında, **Sunucu Adı** ve **Sunucu yöneticisi oturum açma adı** değerlerini not edin. Panoya kopyalamak için her alanın yanındaki kopyala düğmesine tıklayabilirsiniz.
   ![4-2 sunucu özellikleri](./media/tutorial-design-database-using-portal/2-server-properties.png)

Bu örnekte sunucu adı olduğunu *demosunucum.MySQL.Database.Azure.com*, Sunucu Yöneticisi oturum açma bilgileri ise *myadmin\@demosunucum*.

## <a name="connect-to-the-server-using-mysql"></a>mysql kullanarak sunucuya bağlanma
MySQL sunucusu için Azure Veritabanınız ile bağlantı kurmak üzere [mysql komut satırı aracını](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) kullanın. mysql komut satırı aracını tarayıcıdaki Azure Cloud Shell’den veya yerel olarak yüklenmiş mysql araçlarını kullanarak kendi makinenizden çalıştırabilirsiniz. Azure Cloud Shell’i başlatmak için bu makaledeki bir kod bloğu üzerinde `Try It` düğmesine tıklayın veya Azure portalını ziyaret ederek sağ üstteki araç çubuğunda `>_` simgesine tıklayın. 

Bağlanma komutunu yazın:
```azurecli-interactive
mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
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

   ![10-1 veritabanını geri yükleme](./media/tutorial-design-database-using-portal/1-restore-a-db.png)

2. **Geri Yükle** formunu gerekli bilgiler ile doldurun.
   
   ![10-2 geri yükleme formu](./media/tutorial-design-database-using-portal/2-restore-form.png)
   
   - **Geri yükleme noktası**: Bir-için listelenen dilimi içerisindeki geri yüklemek istediğiniz belirli bir noktaya seçin. Yerel saat diliminizi UTC'ye dönüştürdüğünüzden emin olun.
   - **Yeni sunucuya geri**: Geri yüklemek istediğiniz yeni bir sunucu adı sağlayın.
   - **Konum**: Bölge, kaynak sunucuyla aynıdır ve değiştirilemez.
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
