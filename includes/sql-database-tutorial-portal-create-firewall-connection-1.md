---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 12/10/2018
ms.author: genemi
ms.openlocfilehash: ab31ee82e8035fe888fa70b5796aef2c2b2939b2
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728580"
---
## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalda](https://portal.azure.com/) oturum açma

## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma

Tanımlı bir dizi içinde bir Azure SQL veritabanı mevcut [işlem ve depolama kaynaklarını](../articles/sql-database/sql-database-service-tiers-dtu.md). Veritabanını çalışır altında bir [Azure kaynak grubu](../articles/azure-resource-manager/resource-group-overview.md) ve [Azure SQL veritabanı mantıksal sunucusu](../articles/sql-database/sql-database-features.md).

Boş bir SQL veritabanı oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

1. Üzerinde **yeni** sayfasında **veritabanları** > **SQL veritabanı**.

   ![create empty-database](../articles/sql-database/media/sql-database-design-first-database/create-empty-database.png)

1. İçinde **SQL veritabanı** bölmesinde aşağıdaki değerleri seçin veya yazın:

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Veritabanı adı** | *Veritabanınız* | Geçerli veritabanı adları için bkz [veritabanı tanımlayıcıları](/sql/relational-databases/databases/database-identifiers). |
   | **Abonelik** | *yourSubscription*  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | *yourResourceGroup* | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions). |
   | **Kaynak seçme** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

   ![veritabanı oluşturma](../articles/sql-database/media/sql-database-design-first-database/create-database.png)

   1. Seçin **sunucu** yeni veritabanınız için bir sunucuyu yapılandırmak için. Ardından, yazın veya aşağıdaki değerleri seçin:

      | Ayar       | Önerilen değer | Açıklama |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions). |
      | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz [veritabanı tanımlayıcıları](/sql/relational-databases/databases/database-identifiers).|
      | **Parola** | Geçerli bir parola | Parolanız en az 8 karakter bulunmalı ve şu kategorilerin üçünden karakterler kullanmanız gerekir: büyük harf karakterler, küçük harf karakterler, sayılar ve alfasayısal olmayan karakter. |
      | **Konum** | Geçerli bir konum | Bölgeler hakkında daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/). |

      **Seç**’i seçin.

      ![create database-server](../articles/sql-database/media/sql-database-design-first-database/create-database-server.png)

   1. Seçin **fiyatlandırma katmanı** Hizmet katmanını, dtu'ların sayısını ve depolama miktarını belirtmek için. Dtu ve her hizmet katmanı için kullanılabilir olan depolama seçeneklerini keşfedin.

      Sunucu katmanını, dtu'ların sayısını ve depolama miktarını seçtikten sonra seçin **Uygula**.

   1. Girin bir **harmanlama** boş bir veritabanı için (Bu öğretici için varsayılan değeri kullanın). Harmanlamalar hakkında daha fazla bilgi için bkz: [harmanlamaları](/sql/t-sql/statements/collations).

1. Tamamladığınıza göre şimdi **SQL veritabanı** form, select **Oluştur** veritabanını oluşturmak için. Bu adım gerçekleştirebileceğiniz bir dakikalık bir tamamlanması buçuk kadar.

1. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

     ![bildirim](../articles/sql-database/media/sql-database-design-first-database/notification.png)

## <a name="create-a-firewall-rule"></a>Bir güvenlik duvarı kuralı oluşturma

SQL veritabanı hizmeti, dış uygulama ve araçların sunucuya ya da sunucu üzerindeki herhangi bir veritabanına bağlanmasını önlemek için sunucu düzeyinde bir güvenlik duvarı oluşturur. Oluşturmak için bu adımları bir [SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı](../articles/sql-database/sql-database-firewall-configure.md) istemcinizin IP adresi. Bu işlem yalnızca IP adresiniz için SQL veritabanı güvenlik duvarı üzerinden dış bağlantıları sağlar.

> [!NOTE]
> SQL veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, yöneticinize 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL veritabanı sunucusuna bağlanamıyor.

1. Dağıtım tamamlandıktan sonra seçin **SQL veritabanları** seçin ve sol taraftaki menüden *veritabanınız* üzerinde **SQL veritabanları** sayfası. **Genel bakış** , veritabanı açılır ve tam sunucu adını gösteren sayfası (gibi *yourserver.database.windows.net*) ve daha fazla yapılandırma seçenekleri sağlar.

1. Sunucunuz ve sonraki adımlarda veritabanlarına bağlanmak için tam sunucu adını kopyalayın.

   ![sunucu adı](../articles/sql-database/media/sql-database-design-first-database/server-name.png)

1. Seçin **sunucu güvenlik duvarını Ayarla** araç. **Güvenlik Duvarı ayarları** SQL veritabanı sunucusu için sayfası açılır.

   ![sunucu güvenlik duvarı kuralı](../articles/sql-database/media/sql-database-design-first-database/server-firewall-rule.png)

   1. Seçin **istemci IP'si Ekle** geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

   1. **Kaydet**'i seçin. Geçerli IP adresiniz için mantıksal sunucuda 1433 numaralı bağlantı noktası açılarak sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

   1. Seçin **Tamam** ve kapatın **Güvenlik Duvarı ayarları** sayfası.

IP adresiniz, güvenlik duvarı üzerinden artık geçirebilirsiniz ve artık SQL veritabanı sunucusunu ve veritabanlarını SSMS veya seçtiğiniz başka bir aracı kullanarak bağlanabilirsiniz. Daha önce oluşturduğunuz sunucu yönetici hesabı kullandığınızdan emin olun.

> [!IMPORTANT]
> Varsayılan olarak, SQL veritabanı güvenlik duvarı üzerinden erişim tüm Azure Hizmetleri için etkindir. Seçin **OFF** tüm Azure Hizmetleri için devre dışı bırakmak için bu sayfadaki.
