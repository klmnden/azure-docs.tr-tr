---
title: include dosyası
description: include dosyası
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/15/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: 4ffda692da0ab7b63f7376c36dfab0bec914e334
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37138074"
---
* Azure SQL veritabanı kullanıyorsanız, adımları altında [Azure SQL veritabanına bağlanma](#connect-azure-sql-db). 

* SQL Server kullanıyorsanız, adımları altında [SQL Server'a Bağlan](#connect-sql-server).

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Azure SQL veritabanına bağlan

1. SQL tetikleyici veya eylem için bağlantı bilgilerini istediğinde, şu adımları izleyin:

   1. Bağlantınız için bir ad oluşturun.

   2. SQL server'ınızdaki seçin ve ardından, veritabanınızı seçin. 

      Yalnızca SQL server'ınızdaki seçtikten sonra veritabanı listesi görüntülenir.
 
   3. Sunucunuz için kullanıcı adı ve parola sağlayın.

      Bu bilgiler Azure portalında ya da SQL veritabanı özellikleri'nin altında veya bağlantı dizenizi bulabilirsiniz: 
      
      "Kullanıcı kimliği = <*kullanıcıadınız*>"
      <br>
      "Parola = <*yourPassword*>"

   Bu örnek, bir tetikleyici için bağlantı bilgilerini gösterir, ancak bu eylemler için çok adımlarını.

   ![Azure SQL veritabanına bağlantı oluşturun](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)
   <br>
   Yıldız işareti (*) gerekli değerleri belirtin.

   | Özellik | Değer | Ayrıntılar | 
   |----------|-------|---------| 
   | Bağlantı Adı | <*sql bağlantısı My*> | Bağlantınız için ad | 
   | SQL Sunucusu Adı | <*sql server My*> | SQL Server adı |
   | SQL Veritabanı Adı | <*sql veritabanı My*>  | SQL veritabanınızın adı | 
   | Kullanıcı adı | <*My-sql-username*> | Veritabanınıza erişmek için kullanıcı adı |
   | Parola | <*sql parolası My*> | Veritabanınıza erişmek için parola | 
   |||| 

2. İşiniz bittiğinde **Oluştur**’u seçin.

3. Bağlantınızı oluşturduktan sonra devam [ekleme SQL tetikleyici](#add-sql-trigger) veya [ekleme SQL eylem](#add-sql-action).

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>SQL Server'a bağlanma

Ağ geçidiniz seçebilmeniz için önce emin olun, zaten [veri ağ geçidi kurun](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection). Bağlantınızı oluştururken bu şekilde, ağ geçidi ağ geçitleri listesinde görüntülenir.

1. SQL tetikleyici veya eylem için bağlantı bilgilerini istediğinde, şu adımları izleyin:

   1. Tetikleyici ya da eylemi seçin **Connect şirket içi veri ağ geçidi üzerinden** böylece SQL server seçenekleri görünür.

   2. Bağlantınız için bir ad oluşturun.

   3. SQL server için adresini sağlayın, sonra veritabanınız için bir ad sağlayın.
   
      Bağlantı dizenizi bu bilgiyi bulabilirsiniz: 
      
      * "Sunucu = <*yourServerAddress*>"
      * "Veritabanı = <*yourDatabaseName*>"

   4. Sunucunuz için kullanıcı adı ve parola sağlayın.

      Bağlantı dizenizi bu bilgiyi bulabilirsiniz: 
      
      * "Kullanıcı kimliği = <*kullanıcıadınız*>"
      * "Parola = <*yourPassword*>"

   5. SQL server'ınızdaki Windows veya temel kimlik doğrulaması kullanıyorsa, kimlik doğrulama türünü seçin.

   6. Daha önce oluşturduğunuz, şirket içi veri ağ geçidi için bir ad seçin.
   
      Ağ geçidiniz listede görünmüyorsa, denetleyin, doğru [, ağ geçidi kurun](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection).

   Bu örnek, bir tetikleyici için bağlantı bilgilerini gösterir, ancak bu eylemler için çok adımlarını.

   ![SQL Server bağlantısı oluşturma](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)
   <br>
   Yıldız işareti (*) gerekli değerleri belirtin.

   | Özellik | Değer | Ayrıntılar | 
   |----------|-------|---------| 
   | Şirket içi ağ geçidi bağlanma | Bu seçenek önce SQL Server ayarlarını seçin. | | 
   | Bağlantı Adı | <*sql bağlantısı My*> | Bağlantınız için ad | 
   | SQL Sunucusu Adı | <*sql server My*> | SQL Server adı |
   | SQL Veritabanı Adı | <*sql veritabanı My*>  | SQL veritabanınızın adı |
   | Kullanıcı adı | <*My-sql-username*> | Veritabanınıza erişmek için kullanıcı adı |
   | Parola | <*sql parolası My*> | Veritabanınıza erişmek için parola | 
   | Kimlik Doğrulama Türü | Windows veya Basic | İsteğe bağlı: SQL server tarafından kullanılan kimlik doğrulama türü | 
   | Ağ geçitleri | <*data gateway My*> | Şirket içi veri ağ geçidiniz için ad | 
   |||| 

2. İşiniz bittiğinde **Oluştur**’u seçin. 

3. Bağlantınızı oluşturduktan sonra devam [ekleme SQL tetikleyici](#add-sql-trigger) veya [ekleme SQL eylem](#add-sql-action).
