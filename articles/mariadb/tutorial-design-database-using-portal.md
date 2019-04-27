---
title: 'Öğretici: Azure portalını kullanarak MariaDB için Azure veritabanı tasarlama'
description: Bu öğreticide, Azure portalı kullanarak MariaDB için Azure Veritabanı sunucusunun ve veritabanının nasıl oluşturulup yönetileceği açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: tutorial
ms.date: 04/15/2019
ms.custom: mvc
ms.openlocfilehash: 1eb24d90c3aefa81f53a3e31c0bd460f45e5a250
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60626388"
---
# <a name="tutorial-design-an-azure-database-for-mariadb-database-by-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak MariaDB veritabanı için Azure veritabanı tasarlama

MariaDB için Azure Veritabanı, bulutta yüksek oranda kullanılabilir olan MySQL veritabanlarını çalıştırmak, yönetmek ve ölçeklendirmek için kullanılan, yönetilen bir hizmettir. Azure portalı kullanarak, sunucunuzu kolayca yönetebilir ve bir veritabanı tasarlayabilirsiniz.

Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> * MariaDB için Azure Veritabanı oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için mysql komut satırı aracı kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Tarayıcınızda [Azure portala](https://portal.azure.com/) gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-mariadb-server"></a>MariaDB için Azure Veritabanı sunucusu oluşturma

Tanımlı bir dizi [işlem ve depolama kaynağı](concepts-pricing-tiers.md) ile MariaDB için Azure Veritabanı sunucusu oluşturulur. Sunucu, [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) içinde oluşturulur.

1. Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin.

2. Seçin **veritabanları** > **MariaDB için Azure veritabanı**. Ayrıca **MariaDB** hizmeti bulmak için arama kutusuna.
   
   ![MySQL'e gidin](./media/tutorial-design-database-using-portal/1-Navigate-to-mariadb.png)

3. **MariaDB için Azure Veritabanı** kutucuğunu ve ardından **Oluştur**’u seçin. Gerekli bilgileri girin veya seçin.
   
   ![Oluşturma formu](./media/tutorial-design-database-using-portal/2-create-form.png)

    Ayar | Önerilen değer | Alan açıklaması 
    ---|---|---
    Sunucu adı | *benzersiz sunucu adı* | MariaDB için Azure Veritabanı sunucunuzu tanımlayan benzersiz bir ad seçin. Örneğin, **demosunucum**. Girdiğiniz sunucu adına *.mariadb.database.azure.com* etki alanı adı eklenir. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ile 63 arasında karakter içermelidir.
    Abonelik | *aboneliğiniz* | Sunucunuz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandırıldığı aboneliği seçin.
    Kaynak grubu | **myresourcegroup** | Yeni bir kaynak grubu adı girin veya var olan bir kaynak grubunu seçin.
    Kaynak seçme | **Boş** | Yeni bir sunucu oluşturmak için **Boş**’u seçin. (Mevcut bir MariaDB için Azure Veritabanı sunucusunun coğrafi yedeğinden bir sunucu oluşturuyorsanız, **Yedek**'i seçin.)
    Sunucu yöneticisi oturum açma | **myadmin** | Sunucuya bağlanırken kullanılacak kendi oturum açma hesabı. Yönetici oturum açma adı **azure_superuser**, **admin**, **administrator**, **root**, **guest** veya **public** olamaz.
    Parola | *tercih ettiğiniz* | Sunucu yönetici hesabı için yeni bir parola girin. 8 ile 128 arasında karakter içermelidir. Parolanız şu kategorilerin üçünden karakterler içermelidir: İngilizce büyük harfler, İngilizce küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #, % vb.).
    Parolayı onayla | *tercih ettiğiniz*| Yönetici hesabı parolasını onaylayın.
    Location | *kullanıcılarınıza en yakın bölge*| Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
    Version | *en son sürüm*| En son sürüm (başka bir sürüm kullanmak için belirli gereksinimleriniz yoksa).
    Fiyatlandırma katmanı | Açıklamaya bakın. | Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Fiyatlandırma katmanı** > **Genel Amaçlı**'yı seçin. Aşağıdaki ayarlar için varsayılan değerleri kullanın:<br><ul><li>**İşlem Oluşturma** (Gen 5)</li><li>**Sanal çekirdek** (4 çekirdek)</li><li>**Depolama** (100 GB)</li><li>**Yedekleme Saklama Dönemi** (7 gün)</li></ul><br>Coğrafi olarak yedekli depolamada sunucu yedeklerinizi etkinleştirmek için **Fazladan Yedek Seçenekleri**’nde **Coğrafi Olarak Yedeklemeli**’yi seçin. <br><br>Bu fiyatlandırma katmanı seçimini kaydetmek için **Tamam**’ı seçin. Sonraki ekran görüntüsü bu seçimleri yakalar.
    
   ![Fiyatlandırma katmanı](./media/tutorial-design-database-using-portal/3-pricing-tier.png)

4. **Oluştur**’u seçin. Bir veya iki dakika içinde, bulutta yeni bir MariaDB için Azure Veritabanı sunucusu çalışmaya başlayacaktır. Dağıtım işlemini izlemek için araç çubuğunda **Bildirimler**’i seçin.

## <a name="configure-the-firewall"></a>Güvenlik duvarını yapılandırma

MariaDB için Azure Veritabanı bir güvenlik duvarı tarafından korunur. Varsayılan olarak, sunucuya ve sunucu içindeki veritabanlarına yönelik tüm bağlantılar reddedilir. MariaDB için Azure Veritabanı’na ilk kez bağlanmadan önce, güvenlik duvarını istemci bilgisayarın genel ağ IP adresini (veya IP adres aralığını) ekleyecek şekilde yapılandırın.

1. Yeni oluşturduğunuz sunucuyu ve ardından **Bağlantı güvenliği**’ni seçin.
   
   ![Bağlantı güvenliği](./media/tutorial-design-database-using-portal/1-Connection-security.png)
2. Burada **IP’mi Ekle** seçeneğini kullanabilir veya güvenlik duvarı kurallarını yapılandırabilirsiniz. Kuralları oluşturduktan sonra **Kaydet**'i seçmeyi unutmayın.

Artık mysql komut satırı aracını veya MySQL Workbench aracını kullanarak sunucuya bağlanabilirsiniz.

> [!TIP]
> MariaDB için Azure Veritabanı sunucusu, 3306 numaralı bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda MariaDB için Azure Veritabanı sunucusuna bağlanabilmeniz için BT departmanınızın 3306 numaralı bağlantı noktasını açması gerekir.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Azure portalından MariaDB için Azure Veritabanı sunucunuz için tam **Sunucu adı** (tam) ve **Sunucu yöneticisi oturum açma adı** değerlerini alın. Tam sunucu adını, mysql komut satırı aracı kullanarak sunucunuza bağlanmak için kullanırsınız. 

1. [Azure portalın](https://portal.azure.com/) sol tarafındaki menüden **Tüm kaynaklar**'ı seçin. MariaDB için Azure Veritabanı sunucunuzun adını girerek arama yapın. Sunucu ayrıntılarını görüntülemek için sunucu adını seçin.

2. **Genel Bakış** sayfasında **Sunucu adı** ve **Sunucu yöneticisi oturum açma adı** değerlerini not edin. Değeri panoya kopyalamak için her alanın yanındaki **kopyala** düğmesine tıklayabilirsiniz.

   ![Sunucu özellikleri](./media/tutorial-design-database-using-portal/2-server-properties.png)

Bu örnekte sunucu adı şöyledir **mydemoserver.mariadb.database.azure.com** ve Sunucu Yöneticisi oturum açma adını **myadmin\@demosunucum**.

## <a name="connect-to-the-server-by-using-mysql"></a>mysql kullanarak sunucuya bağlanma

MariaDB için Azure Veritabanı sunucunuzla bağlantı kurmak üzere [mysql komut satırı aracını](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) kullanın. mysql komut satırı aracını tarayıcıdaki Azure Cloud Shell’den veya yerel olarak yüklenmiş mysql araçlarını kullanarak bilgisayarınızdan çalıştırabilirsiniz. Azure Cloud Shell’i başlatmak için bu makaledeki bir kod bloğu üzerinde **Deneyin** düğmesine tıklayın veya Azure portalı ziyaret ederek sağ üstteki araç çubuğunda **>_** simgesine tıklayın. 

Bağlanmak için şu komutu girin:

```azurecli-interactive
mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

## <a name="create-a-blank-database"></a>Boş veritabanı oluşturma

Sunucuya bağlandıktan sonra, çalışmak üzere boş bir veritabanı oluşturun:

```sql
CREATE DATABASE mysampledb;
```

İstemde, bağlantıyı bu yeni oluşturulan veritabanınıza geçirmek için şu komutu çalıştırın:

```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Veritabanında tablo oluşturma

Artık MariaDB için Azure Veritabanı'na nasıl bağlanacağınızı bildiğinize göre bazı temel görevleri tamamlayabilirsiniz.

İlk olarak, bir tablo oluşturun ve bu tabloya bazı veriler yükleyin. Envanter bilgilerini depolayan bir tablo oluşturalım:

```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-in-the-tables"></a>Tablolara veri yükleme

Bir tablonuz olduğuna göre içine bazı veriler ekleyin. Açık olan Komut İstemi penceresinde şu sorguyu çalıştırarak birkaç veri satırı ekleyin:

```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

## <a name="query-and-update-the-data-in-the-tables"></a>Tablolardaki verileri sorgulama ve güncelleştirme

Veritabanı tablosundan bilgi almak için şu sorguyu çalıştırın:

```sql
SELECT * FROM inventory;
```

Ayrıca tablolardaki verileri de güncelleştirebilirsiniz:

```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Verileri aldığınızda satır güncelleştirilir:

```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Bir veritabanını daha önceki bir noktaya geri yükleme

Önemli bir veritabanı tablosunu yanlışlıkla sildiğinizi ve verileri kolayca kurtaramayacağınızı düşünün. MariaDB için Azure Veritabanı, sunucuyu önceki bir noktaya geri yükleyerek, veritabanlarının bir kopyasının yeni sunucunuzda oluşturulmasını sağlar. Bu yeni sunucuyu silinen verilerinizi kurtarmak için kullanabilirsiniz. Şu adımlar, örnek sunucuyu tablo eklenmeden önceki bir noktaya geri yükler:

1. Azure portalında MariaDB için Azure Veritabanınızı bulun. **Genel Bakış** sayfasında **Geri yükle**'yi seçin.

   ![Veritabanını geri yükleme](./media/tutorial-design-database-using-portal/1-restore-a-db.png)

2. **Geri yükle** sayfasında aşağıdaki bilgileri girin veya seçin:
   
   ![Geri yükleme formu](./media/tutorial-design-database-using-portal/2-restore-form.png)
   
   - **Geri yükleme noktası**: İçin listelenen zaman dilimi içinde geri yüklemek istediğiniz zamanda bir nokta seçin. Yerel saat diliminizi UTC'ye dönüştürdüğünüzden emin olun.
   - **Yeni sunucuya geri**: Geri yüklemek için yeni bir sunucu adı girin.
   - **Konum**: Bölge, kaynak sunucuyla aynıdır ve değiştirilemez.
   - **Fiyatlandırma katmanı**: Fiyatlandırma katmanı, kaynak sunucu ile aynıdır ve değiştirilemez.
   
3. Sunucuyu, tablo silinmeden önceki [belirli bir noktaya geri yüklemek](./howto-restore-server-portal.md) için **Tamam**’a tıklayın. Bir sunucuyu geri yüklemek, sunucunun seçtiğiniz zamanda yeni bir kopyasını oluşturur. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalını şunları yapmayı öğrenmek için kullanırsınız:

> [!div class="checklist"]
> * MariaDB için Azure Veritabanı oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için mysql komut satırı aracı kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

> [!div class="nextstepaction"]
> [MariaDB için Azure Veritabanı'na uygulama bağlama](./howto-connection-string.md)
