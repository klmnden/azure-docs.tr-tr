---
title: 'Azure portalı: Bir SQL veritabanı güvenlik duvarı kuralı oluşturun. | Microsoft Docs'
description: SQL Veritabanı sunucusu düzeyinde bir güvenlik duvarı kuralı oluşturma
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 589d3fa8c0ee8c8f374cd4f34f17401caa46d265
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462265"
---
# <a name="quickstart-create-a-server-level-firewall-rule-for-your-sql-database-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalı kullanarak SQL veritabanınız için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturun

Bu hızlı başlangıç, bir Azure SQL veritabanına şirket içi bir kaynaktan bağlanmanızı sağlamak üzere veritabanı için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturmanın adımlarını göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta oluşturulan kaynakları kullanan [Azure portalında bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md) , başlangıç noktası olarak.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL veritabanı hizmeti, sunucu düzeyinde bir güvenlik duvarı oluşturur. Bu güvenlik duvarı, uygulama ve araçların güvenlik duvarını açmak üzere bir güvenlik duvarı kuralı oluşturmadığınız sürece sunucu ya da herhangi bir sunucu veritabanına bağlanmasını engeller. Azure dışında bir IP adresinden bir bağlantı için belirli bir IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturun. Güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı](sql-database-firewall-configure.md).

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden gelen bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe verilmeyebilir. Bu durumda BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL veritabanı sunucusuna bağlanamıyor.
>

İstemcinizin IP adresi için bir sunucu düzeyinde güvenlik duvarı kuralı oluşturun ve IP adresiniz yalnızca SQL veritabanı güvenlik duvarı üzerinden dış bağlantıları etkinleştirmek için aşağıdaki adımları izleyin.

1. Sonra [önkoşul Azure SQL veritabanı](#prerequisites) dağıtım tamamlandığında, seçin **SQL veritabanları** sol taraftaki menüden seçip **mySampleDatabase** üzerinde **SQL veritabanları** sayfası. Veritabanınıza ilişkin genel bakış sayfası açılır ve tam sunucu adı (örneğin, **mynewserver-20170824.database.windows.net**) görüntülenerek daha fazla yapılandırma seçeneği sunulur.

2. Sunucunuza ve veritabanlarına diğer hızlı başlangıçlar bağlanırken kullanması için bu tam sunucu adını kopyalayın.

   ![sunucu adı](./media/sql-database-get-started-portal/server-name.png)

3. Seçin **sunucu güvenlik duvarını Ayarla** araç. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır.

   ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Seçin **istemci IP'si Ekle** geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

   > [!IMPORTANT]
   > Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için etkindir. Seçin **OFF** tüm Azure Hizmetleri için devre dışı bırakmak için bu sayfadaki.
   >

5. **Kaydet**’i seçin. SQL veritabanı sunucusu üzerindeki 1433 numaralı bağlantı noktasını açma geçerli IP adresiniz için sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

6. Kapat **Güvenlik Duvarı ayarları** sayfası.

SQL Server Management Studio veya seçtiğiniz başka bir aracı kullanarak, artık SQL veritabanı sunucusuna ve sunucuya ait veritabanlarına için daha önce oluşturduğunuz sunucu yönetici hesabıyla bu IP adresinden bağlanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Sonraki adımlar](#next-steps)’a geçip farklı yöntemler kullanarak veritabanını sorgulama hakkında bilgi edinmek istiyorsanız bu kaynakları kaydedin. Bu hızlı başlangıçta oluşturduğunuz kaynakları silmek istiyorsanız, aşağıdaki adımları kullanın.

1. Azure portalında sol taraftaki menüden seçin **kaynak grupları** seçip **myResourceGroup**.
2. Kaynak grubu sayfanızda seçin **Sil**, türü **myResourceGroup** metin kutusuna ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

- Artık bir veritabanınız olduğuna göre, şunlar dahil tercih ettiğiniz araçlar ya da diller ile veritabanında [bağlanma ve sorgulama](sql-database-connect-query.md) işlemleri yapabilirsiniz:
  - [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
  - [Azure Data Studio kullanarak bağlanma ve sorgulama](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- İlk veritabanınızı tasarlamayı, tablolar oluşturmayı ve veri eklemeyi öğrenmek için, şu öğreticilerden birine bakın:
  - [SSMS kullanarak ilk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
  - [C# ve ADO.NET ile bir Azure SQL veritabanı tasarlama ve bağlama](sql-database-design-first-database-csharp.md)
