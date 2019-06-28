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
ms.openlocfilehash: da03c5247b8ebe0a3305b08a05d661264497663f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188624"
---
* Azure SQL veritabanı kullanıyorsanız, altındaki adımları [Azure SQL veritabanına bağlanma](#connect-azure-sql-db). 

* SQL Server kullanıyorsanız, altındaki adımları [SQL Server'a Bağlan](#connect-sql-server).

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Azure SQL veritabanı'na bağlanma

1. SQL tetikleyici veya eylemi için bağlantı bilgilerini sorduğunda, şu adımları izleyin:

   1. Bağlantınız için bir ad oluşturun.

   2. SQL server'ınızı seçin ve ardından veritabanınızı seçin. 

      Yalnızca SQL server'ınızı seçtikten sonra veritabanı listesi görünür.
 
   3. Sunucunuz için kullanıcı adınızı ve parolanızı sağlayın.

      Bu bilgi ya da Azure portalında, SQL veritabanı özellikleri bölümünde veya bağlantı dizenizi bulabilirsiniz: 
      
      "Kullanıcı kimliği = <*kullanıcıadınız*>"
      <br>
      "Parola = <*yourPassword*>"

   Bu örnek, bir tetikleyici için bağlantı bilgilerini gösterir, ancak adımları çok eylemleri için çalışır.

   ![Azure SQL veritabanı bağlantısı oluşturma](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)
   <br>
   Yıldız işareti (*) gerekli değerleri belirtin.

   | Özellik | Değer | Ayrıntılar | 
   |----------|-------|---------| 
   | Bağlantı Adı | <*sql bağlantısı My*> | Bağlantınız için bir ad | 
   | SQL Server Adı | <*my-sql-server*> | SQL sunucunuzun adı |
   | SQL veritabanı adı | <*sql veritabanı benim*>  | SQL veritabanınız için adı | 
   | Kullanıcı adı | <*My-sql-username*> | Veritabanınıza erişmek için kullanıcı adı |
   | Parola | <*my-sql-password*> | Veritabanınıza erişmek için parola | 
   |||| 

2. İşiniz bittiğinde **Oluştur**’u seçin.

3. Bağlantınızı oluşturduktan sonra devam [ekleme SQL tetikleyicisi](#add-sql-trigger) veya [SQL ekleme eylemi](#add-sql-action).

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>SQL Server'a bağlanma

Ağ geçidiniz seçmeden önce emin olun, zaten [veri ağ geçidinizi ayarlamak](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection). Bağlantınızı oluştururken bu şekilde, ağ geçidi ağ geçitleri listesinde görüntülenir.

1. SQL tetikleyici veya eylemi için bağlantı bilgilerini sorduğunda, şu adımları izleyin:

   1. Tetikleyici veya eylemi seçin **şirket içi veri ağ geçidi üzerinden Bağlan** SQL server seçenekleri görünür.

   2. Bağlantınız için bir ad oluşturun.

   3. İçin SQL server'ınızı adresini sağlayın, sonra veritabanınız için bir ad sağlayın.
   
      Bağlantı dizenizi bu bilgiyi bulabilirsiniz: 
      
      * "Server = <*yourServerAddress*>"
      * "Veritabanı = <*yourDatabaseName*>"

   4. Sunucunuz için kullanıcı adınızı ve parolanızı sağlayın.

      Bağlantı dizenizi bu bilgiyi bulabilirsiniz: 
      
      * "Kullanıcı kimliği = <*kullanıcıadınız*>"
      * "Parola = <*yourPassword*>"

   5. SQL server, Windows veya temel kimlik doğrulaması kullanıyorsa, kimlik doğrulaması türünü seçin.

   6. Daha önce oluşturduğunuz şirket içi veri ağ geçidi için bir ad seçin.
   
      Ağ geçidiniz listesinde görünüp görünmediğini denetlemek, doğru [geçidinizi ayarlamak](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection).

   Bu örnek, bir tetikleyici için bağlantı bilgilerini gösterir, ancak adımları çok eylemleri için çalışır.

   ![SQL Server bağlantı oluşturma](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)
   <br>
   Yıldız işareti (*) gerekli değerleri belirtin.

   | Özellik | Değer | Ayrıntılar | 
   |----------|-------|---------| 
   | Şirket içi ağ geçidi üzerinden Bağlan | Bu seçenek, önce SQL Server ayarlarını seçin. | | 
   | Bağlantı Adı | <*sql bağlantısı My*> | Bağlantınız için bir ad | 
   | SQL Server Adı | <*my-sql-server*> | SQL sunucunuzun adı |
   | SQL veritabanı adı | <*sql veritabanı benim*>  | SQL veritabanınız için adı |
   | Kullanıcı adı | <*My-sql-username*> | Veritabanınıza erişmek için kullanıcı adı |
   | Parola | <*my-sql-password*> | Veritabanınıza erişmek için parola | 
   | Kimlik doğrulaması türü | Windows veya temel | İsteğe bağlı: SQL server tarafından kullanılan kimlik doğrulama türü | 
   | Ağ geçitleri | <*data gateway My*> | Şirket içi veri ağ geçidi adı | 
   |||| 

2. İşiniz bittiğinde **Oluştur**’u seçin. 

3. Bağlantınızı oluşturduktan sonra devam [ekleme SQL tetikleyicisi](#add-sql-trigger) veya [SQL ekleme eylemi](#add-sql-action).
