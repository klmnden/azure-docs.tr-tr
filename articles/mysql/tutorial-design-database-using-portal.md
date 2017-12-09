---
title: "İlk Azure veritabanınızı MySQL veritabanı - Azure Portal için Tasarım | Microsoft Docs"
description: "Bu öğretici, oluşturma ve MySQL server ve Azure portalını kullanarak veritabanı için Azure veritabanı yönetme açıklanmaktadır."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.topic: tutorial
ms.date: 11/03/2017
ms.custom: mvc
ms.openlocfilehash: a7f38484e000b05a57cad9bc95abb255414d0162
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>İlk Azure veritabanınızı MySQL veritabanı için tasarlama
MySQL için Azure Veritabanı, bulutta yüksek oranda kullanılabilir olan MySQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure Portalı'nı kullanarak, kolayca sunucunuzu yönetin ve bir veritabanı tasarlayın.

Bu öğreticide, bilgi edinmek için Azure portalını kullanın nasıl yapılır:

> [!div class="checklist"]
> * MySQL için Azure bir veritabanı oluşturun
> * Sunucu Güvenlik Duvarı'nı yapılandırma
> * Bir veritabanı oluşturmak için MySQL komut satırı aracını kullanın
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Sık kullanılan web tarayıcınızı açın ve ziyaret [Microsoft Azure portal](https://portal.azure.com/). Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
MySQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) içinde oluşturulur.

1. Gidin **veritabanları** > **Azure veritabanı için MySQL**. MySQL sunucusu altında bulamazsa **veritabanları** kategori tıklatın **tümünü görmek** tüm kullanılabilir veritabanı hizmetleri göstermek için. Ayrıca yazabilirsiniz **Azure veritabanı için MySQL** hizmeti hızla bulmak için arama kutusuna.
![MySQL için 2-1 gidin](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Tıklatın **Azure veritabanı için MySQL** kutucuğuna ve ardından **oluşturma**.

Bu örnekte, Azure veritabanı için MySQL form aşağıdaki bilgilerle doldurun:

| **Ayar** | **Önerilen değer** | **Alan Açıklaması** |
|---|---|---|
| *Sunucu adı* | myserver4demo  | Sunucu adı genel olarak benzersiz olmalıdır. |
| *Abonelik* | mysubscription | Açılan listeden aboneliğinizi seçin. |
| *Kaynak grubu* | myresourcegroup | Bir kaynak grubu oluşturun veya mevcut olanlardan birini kullanın. |
| *Sunucu yöneticisi oturum açma bilgileri* | myadmin | Kurulum Yönetici hesap adı. |
| *Parola* |  | Güçlü bir yönetici hesabı parolası belirleyin. |
| *Parolayı onayla* |  | Yönetici hesabı parolasını onaylayın. |
| *Konum* |  | Kullanılabilir bir bölge seçin. |
| *Sürüm* | 5.7 | En son sürümü seçin. |
| *Performansı yapılandır* | Temel, 50 birimleri, 50 GB işlem  | **Fiyatlandırma katmanı**, **İşlem Birimleri**, **Depolama (GB)** öğelerini seçip **Tamam**’a tıklayın. |
| *Panoya Sabitle* | İşaretli | Sunucuyu daha sonra kolayca bulabilmeniz için bu kutunun işaretlenmesi önerilir |
Sonra, **Oluştur**’a tıklayın. Bir veya iki dakika içinde, bulutta MySQL sunucusu için yeni bir Azure Veritabanı çalışmaya başlayacaktır. Tıklayabilirsiniz **bildirimleri** dağıtım işlemi izlemek için araç çubuğunda.

## <a name="configure-firewall"></a>Güvenlik duvarını yapılandırma
Azure veritabanları MySQL için bir güvenlik duvarı tarafından korunur. Varsayılan olarak, sunucu ve veritabanları Sunucusu'ndaki tüm bağlantıları reddedilir. Azure veritabanı için MySQL için ilk kez bağlanmadan önce istemci makinenin ortak ağ IP adresi (veya IP adresi aralığı) eklemek için Güvenlik Duvarı'nı yapılandırın.

1. Yeni oluşturulan sunucunuz tıklayın ve ardından **bağlantı güvenliği**.
   ![3 1 bağlantı güvenliği](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. Yapabilecekleriniz **eklemek IP**, veya güvenlik duvarı kurallarını yapılandırın. Kuralları oluşturduktan sonra **Kaydet**’e tıklamayı unutmayın.
Şimdi, mysql komut satırı aracını veya MySQL çalışma ekranı GUI aracını kullanarak sunucuya bağlanabilir.

> [!TIP]
> MySQL sunucusu için Azure veritabanı 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. BT departmanınız 3306 bir bağlantı noktası açar sürece bu durumda, Azure MySQL sunucusuna bağlanamıyor.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
Tam alma **sunucu adı** ve **sunucu yönetici oturum açma adı** Azure portalından MySQL server için Azure veritabanınızın. Mysql komut satırı aracını kullanarak sunucunuza bağlanmak için tam sunucu adını kullanın. 

1. İçinde [Azure portal](https://portal.azure.com/), tıklatın **tüm kaynakları** sol taraftaki menüden bir ad yazın ve arama MySQL server için Azure veritabanınızın. Ayrıntılarını görüntülemek için sunucu adını seçin.

2. Ayarlar altında başlığını tıklatın **özellikleri**. Aşağı Not **sunucu adı** ve **sunucu yönetici oturum açma adı**. Panoya kopyalamak için her alanın yanındaki Kopyala düğmesini tıklatabilirsiniz.
   ![4-2 sunucu özellikleri](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

Bu örnekte sunucu adı *myserver4demo.mysql.database.azure.com*, sunucu yöneticisi oturum açma bilgileri ise *myadmin@myserver4demo* şeklindedir.

## <a name="connect-to-the-server-using-mysql"></a>MySQL kullanarak sunucuya bağlanın
MySQL sunucusu için Azure Veritabanınız ile bağlantı kurmak üzere [mysql komut satırı aracını](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) kullanın. Tarayıcıda Azure bulut Kabuğu'ndan veya mysql araçlarının yüklü kullanarak yerel olarak kendi makineden mysql komut satırı aracını çalıştırabilirsiniz. Azure bulut Kabuğu'nu başlatmak için tıklatın `Try It` bu makaledeki kod bloğu düğmesine veya Azure portalını ziyaret edin ve `>_` sağ üst araç çubuğunda simge. 

Bağlanma komutunu yazın:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Boş veritabanı oluşturma
Bir kez sunucusuna bağlandıktan sonra çalışmak için boş bir veritabanı oluşturun.
```sql
CREATE DATABASE mysampledb;
```

İsteminde bağlantı yeni veritabanı oluşturulan bu geçiş yapmak için aşağıdaki komutu çalıştırın:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Veritabanında tabloları oluşturma
MySQL veritabanı için Azure veritabanına bağlanmak nasıl bildiğinize göre bazı temel görevleri tamamlayın:

İlk olarak, bir tablo oluşturun ve bazı verilerle yükleyin. Envanter bilgilerini depolayan bir tablo oluşturalım.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Veri tablolarına yükleme
Bir tablo sahip olduğunuza göre bazı veriler içine ekleyin. Açık komut istemi penceresinde, bazı veri satırı eklemek için aşağıdaki sorguyu çalıştırın.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Şimdi örnek verilerin daha önce oluşturduğunuz tabloya iki satır var.

## <a name="query-and-update-the-data-in-the-tables"></a>Sorgulamak ve tablolarındaki verileri güncelleyin
Veritabanı tablosundan bilgi almak için aşağıdaki sorguyu çalıştırın.
```sql
SELECT * FROM inventory;
```

Ayrıca tablolardaki verileri güncelleştirebilirsiniz.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Veri aldığınızda satır buna göre güncelleştirilir.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Bir veritabanını daha önceki bir noktaya geri yükleme
Önemli veritabanı tablosu yanlışlıkla sildiyseniz ve verileri kolayca kurtaramazsınız düşünün. Azure veritabanı için MySQL sunucu zaman içinde bir noktaya geri veritabanlarını yeni sunucuyu oluşturulmasını sağlar. Bu yeni sunucu silinen verilerinizi kurtarmak için kullanabilirsiniz. Tablo eklenmeden önce aşağıdaki adımları örnek sunucunun bir noktaya geri yükleme.

1. Azure portalında Azure veritabanı için MySQL bulun. Üzerinde **genel bakış** sayfasında, **geri** araç çubuğunda. Geri yükleme sayfası açılır.

   ![10-1, bir veritabanını geri yükleyin](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Doldurmak **geri** form gerekli bilgileri.
   
   ![10-2 geri yükleme formu](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Geri yükleme noktası**: bir noktası için listelenen zaman çerçevesi içinde geri yüklemek istediğiniz zaman seçin. Yerel saat dilimi UTC'ye dönüştürülecek emin olun.
   - **Yeni bir sunucuya geri**: geri yüklemek istediğiniz yeni bir sunucu adı sağlayın.
   - **Konum**: bölge kaynak sunucu ile aynı olması ve değiştirilemez.
   - **Fiyatlandırma katmanı**: fiyatlandırma katmanı kaynak sunucu ile aynıdır ve değiştirilemez.
   
3. Tıklatın **Tamam** sunucuya geri yüklemek için [zaman içinde bir noktaya geri](./howto-restore-server-portal.md) tablo silinmeden önce. Bir sunucuyu geri belirttiğiniz zaman içinde nokta itibariyle sunucunun yeni bir kopyasını oluşturur. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portal öğrenilen için nasıl kullanın:

> [!div class="checklist"]
> * MySQL için Azure bir veritabanı oluşturun
> * Sunucu Güvenlik Duvarı'nı yapılandırma
> * Bir veritabanı oluşturmak için MySQL komut satırı aracını kullanın
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

> [!div class="nextstepaction"]
> [MySQL için Azure veritabanı uygulamalara bağlanma](./howto-connection-string.md)
