---
title: İlk ilişkisel veritabanı - tasarlama C# -Azure SQL veritabanı | Microsoft Docs
description: Azure SQL veritabanı ile tek bir veritabanındaki ilk ilişkisel veritabanı tasarlamayı öğrenin C# ADO.NET kullanarak.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: MightyPen
ms.author: genemi
ms.reviewer: carlrab
manager: craigg-msft
ms.date: 02/08/2019
ms.openlocfilehash: ce46a6b8d4e2bc57625f9202349718dfbaedc660
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553222"
---
# <a name="tutorial-design-a-relational-database-in-a-single-database-within-azure-sql-database-cx23-and-adonet"></a>Öğretici: Azure SQL veritabanı C içindeki tek bir veritabanında ilişkisel veritabanı tasarlama&#x23; ve ADO.NET

Azure SQL veritabanı, bir ilişkisel veritabanı olarak-hizmet (DBaaS), Microsoft Cloud (Azure) ' dir. Bu öğreticide, Visual Studio ile ADO.NET ve Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalı * kullanarak tek veritabanı oluşturma
> * Azure portalını kullanarak bir sunucu düzeyinde IP güvenlik duvarı kuralı oluşturma
> * ADO.NET ve Visual Studio ile veritabanına bağlanma
> * ADO.NET ile tablo oluşturma
> * ADO.NET ile veri ekleme, güncelleştirme ve silme
> * ADO.NET ile veri sorgulama

* Azure aboneliğiniz yoksa, [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Önkoşullar

Yüklemesini [Visual Studio 2017](https://www.visualstudio.com/downloads/)

## <a name="create-a-blank-single-database"></a>Boş bir tek veritabanı oluşturma

Azure SQL veritabanı'nda tek bir veritabanı bir dizi işlem ve depolama kaynakları oluşturulur. Veritabanı içinde oluşturulan bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve kullanarak yönetilen bir [veritabanı sunucusu](sql-database-servers.md).

Boş bir tek veritabanı oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yeni** sayfasında, Azure Market bölümünde **Veritabanları**’nı seçin ve ardından **Öne Çıkan** bölümünde **SQL Veritabanı**’na tıklayın.

   ![create empty-database](./media/sql-database-design-first-database/create-empty-database.png)

3. Doldurun **SQL veritabanı** önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle oluşturur:

    | Ayar       | Önerilen değer | Açıklama |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Veritabanı adı** | *Veritabanınız* | Geçerli veritabanı adları için bkz [veritabanı tanımlayıcıları](/sql/relational-databases/databases/database-identifiers). |
    | **Abonelik** | *yourSubscription*  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
    | **Kaynak grubu** | *yourResourceGroup* | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions). |
    | **Kaynak seçme** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

4. Tıklayın **sunucu** mevcut bir veritabanı sunucusunu kullanabilir veya oluşturmaya ve yeni bir veritabanı sunucusunu yapılandırın. Mevcut bir sunucu seçin veya tıklatın **yeni sunucu oluştur** doldurun **yeni sunucu** formunu aşağıdaki bilgilerle:

    | Ayar       | Önerilen değer | Açıklama |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions). |
    | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz [veritabanı tanımlayıcıları](/sql/relational-databases/databases/database-identifiers). |
    | **Parola** | Geçerli bir parola | Parolanız en az 8 karakter bulunmalı ve şu kategorilerin üçünden karakterler kullanmanız gerekir: büyük harf karakterler, küçük harf karakterler, sayılar ve alfasayısal olmayan karakter. |
    | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

    ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

5. **Seç**'e tıklayın.
6. Hizmet katmanını, DTU veya sanal çekirdek sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Dtu/sanal çekirdek ve her hizmet katmanı için kullanılabilir olan depolama sayısı seçeneklerini araştırın.

    Hizmet katmanını, Dtu veya sanal çekirdek sayısı ve depolama alanı miktarını seçtikten sonra **Uygula**.

7. Girin bir **harmanlama** boş bir veritabanı için (Bu öğretici için varsayılan değeri kullanın). Harmanlamalar hakkında daha fazla bilgi için bkz. [Harmanlamalar](/sql/t-sql/statements/collations)

8. Tamamladığınıza göre şimdi **SQL veritabanı** formunda, tıklayın **Oluştur** tek veritabanı sağlanması. Bu adım birkaç dakika sürebilir.

9. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

   ![bildirim](./media/sql-database-design-first-database/notification.png)

## <a name="create-a-server-level-ip-firewall-rule"></a>Sunucu düzeyinde IP güvenlik duvarı kuralı oluşturma

SQL veritabanı hizmeti, sunucu düzeyinde bir IP Güvenlik Duvarı oluşturur. Bu güvenlik duvarı dış uygulama ve araçların bir güvenlik duvarı kuralı kendi IP Güvenlik Duvarı üzerinden izin vermesi dışında sunucuya ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını engeller. Tek veritabanı dış bağlantıları etkinleştirmek için ilk IP adresini (veya IP adresi aralığı) için bir IP güvenlik duvarı kuralı eklemeniz gerekir. Oluşturmak için bu adımları bir [SQL veritabanı sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-firewall-configure.md).

> [!IMPORTANT]
> SQL veritabanı hizmeti, 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bu hizmetine bir kurumsal ağ içinden bağlanmaya çalışıyorsanız, 1433 numaralı bağlantı noktası üzerinden giden trafiğe ağınızın güvenlik duvarı tarafından izin verilmiyor. Yöneticiniz 1433 numaralı bağlantı noktasını açmadığı sürece bu durumda, tek bir veritabanına bağlanamıyor.

1. Dağıtım tamamlandıktan sonra tıklayın **SQL veritabanları** 'a tıklayın ve sol taraftaki menüden *veritabanınız* üzerinde **SQL veritabanları** sayfası. Veritabanınıza ilişkin genel bakış sayfası açılır ve tam gösteren **sunucu adı** (gibi *yourserver.database.windows.net*) ve daha fazla yapılandırma seçenekleri sağlar.

2. Sunucunuzun ve veritabanları için SQL Server Management Studio'dan bağlanmak için bu tam sunucu adını kopyalayın.

   ![sunucu adı](./media/sql-database-design-first-database/server-name.png)

3. Araç çubuğunda **Sunucu güvenlik duvarını ayarla**’ya tıklayın. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır.

   ![sunucu düzeyinde IP güvenlik duvarı kuralı](./media/sql-database-design-first-database/server-firewall-rule.png)

4. Tıklayın **istemci IP'si Ekle** geçerli IP adresinizi yeni bir IP güvenlik duvarı kuralına eklemek için araç çubuğunda. Bir IP güvenlik duvarı kuralı, tek bir IP adresi veya bir IP adresi aralığı için 1433 numaralı bağlantı noktasını açabilirsiniz.

5. **Kaydet**’e tıklayın. SQL veritabanı sunucusu üzerindeki 1433 numaralı bağlantı noktasını açma geçerli IP adresiniz için sunucu düzeyinde IP güvenlik duvarı kuralı oluşturulur.

6. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

IP adresiniz, artık IP Güvenlik Duvarı üzerinden geçirebilirsiniz. Artık SQL Server Management Studio veya seçtiğiniz başka bir aracı kullanarak tek veritabanına bağlanabilirsiniz. Daha önce oluşturduğunuz sunucu yönetici hesabı kullandığınızdan emin olun.

> [!IMPORTANT]
> Varsayılan olarak, SQL veritabanı IP Güvenlik Duvarı üzerinden erişim tüm Azure Hizmetleri için etkindir. Tüm Azure hizmetleri için devre dışı bırakmak isterseniz bu sayfadaki **KAPALI** öğesine tıklayın.

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel veritabanı görevlerini gibi veritabanı ve tablo oluşturma hakkında bilgi edindiniz, veritabanına bağlanmak, verileri yüklemek ve sorguları çalıştırın. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veritabanı oluşturma
> * Güvenlik duvarı kuralı ayarlama
> * [Visual Studio ve C#](sql-database-connect-query-dotnet-visual-studio.md) ile veritabanına bağlanma
> * Tablo oluşturma
> * Ekleme, güncelleştirme, silme ve verileri Sorgulama

Veri geçişi hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [DMS kullanarak SQL Server'ı çevrimdışı Azure SQL Veritabanına geçirme](../dms/tutorial-sql-server-to-azure-sql.md)
