---
title: SQL Server veya Azure SQL veritabanı - Azure mantıksal uygulamaları bağlamak | Microsoft Docs
description: Erişim ve şirket içi veya bulutta SQL veritabanları Azure Logic Apps ile iş akışları otomatik hale getirerek yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/15/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: dccb91c782408a5fed5c3ef1b68f9918823ce402
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296297"
---
# <a name="connect-to-sql-server-or-azure-sql-database-from-azure-logic-apps"></a>SQL Server veya Azure SQL veritabanı için Azure mantığı uygulamalardan Bağlan

Bu makalede, SQL Server connector ile bir mantıksal uygulama içinde SQL veritabanınızdaki verileri nasıl erişebileceğiniz gösterilmektedir. Böylece, görevler, işlemler ve logic apps oluşturarak SQL veri ve kaynaklarınız yönetme iş akışlarını otomatikleştirebilirsiniz. Bağlayıcı çalıştığı her ikisi için [şirket içi SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) ve [Azure SQL Database'i bulutta](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview). 

Olayları SQL veritabanınız veya Dynamics CRM Online gibi diğer sistemler tarafından tetiklenen oluşturulduğunda çalışan mantıksal uygulamalar oluşturabilirsiniz. Ayrıca mantıksal uygulamalarınızı almak, Ekle ve SQL sorguları ve saklı yordamlar yürütme yanı sıra veri silin. Örneğin, Dynamics CRM Online yeni kayıtları otomatik olarak denetler, yeni kayıtları için SQL veritabanı öğeleri ekler ve ardından e-posta uyarıları gönderir. bir mantıksal uygulama oluşturabilirsiniz.

Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. Logic apps yeniyseniz, gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bağlayıcı özgü teknik bilgi için bkz: <a href="https://docs.microsoft.com/connectors/sql/" target="blank">SQL Server Bağlayıcısı başvurusu</a>.

## <a name="prerequisites"></a>Önkoşullar

* Burada SQL veritabanınız erişmeniz mantıksal uygulama. Mantıksal uygulamanızı SQL tetikleyici ile başlatmak için gereken bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Bir [Azure SQL veritabanı](../sql-database/sql-database-get-started-portal.md) veya [SQL Server veritabanı](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database) 

  Mantıksal uygulamanızı sonuçları işlemleri çağrılırken dönebilmek tablolarınızı verilerine sahip olması gerekir. Bir Azure SQL Database oluşturursanız, dahil edilen örnek veritabanları kullanabilirsiniz. 

* SQL server adı, veritabanı adı, kullanıcı adınızı ve parolanızı. Böylece, SQL sunucunuza erişmek için mantığınızı yetki verebilir, bu kimlik bilgileri gerekir. 

  * Azure SQL veritabanı için bağlantı dizesinde veya SQL veritabanı özellikleri'nin altında Azure Portalı'nda bu ayrıntıları bulabilirsiniz:

    "Sunucu = tcp: <*SunucuAdınız*>. database.windows.net,1433;Initial katalog = <*yourDatabaseName*>; Kalıcı güvenlik bilgisi = False; Kullanıcı Kimliği = <*kullanıcıadınız*>; Parola = <*yourPassword*>; MultipleActiveResultSets = False; Şifreleme = True; TrustServerCertificate = False; Bağlantı zaman aşımı = 30; "

  * SQL Server için bağlantı dizesinde bu ayrıntıları bulabilirsiniz: 

    "Sunucu = <*yourServerAddress*>; Veritabanı = <*yourDatabaseName*>; Kullanıcı Kimliği = <*kullanıcıadınız*>; Parola = <*yourPassword*>; "

* SQL Server gibi şirket içi sistemlere mantıksal uygulamalar bağlanabilmesi için önce şunları yapmalısınız [bir şirket içi veri ağ geçidi kurun](../logic-apps/logic-apps-gateway-install.md). Mantıksal uygulamanız için SQL bağlantı oluşturduğunuzda, bu şekilde, ağ geçidi seçebilirsiniz.

<a name="add-sql-trigger"></a>

## <a name="add-sql-trigger"></a>SQL tetikleyicisi ekleyin

Azure Logic Apps içinde her mantıksal uygulama başlamalı ve bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay olduğunda etkinleşir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her tetikleyici ateşlenir Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

1. Azure portal ya da Visual Studio Logic Apps Tasarımcısı açılır boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

2. Arama kutusuna "sql server", filtre olarak girin. Tetikleyiciler listeden istediğiniz SQL Tetikleyici seçin. 

   Bu örnek için bu Tetikleyici seçin: **öğeyi oluşturulduğunda, SQL Server -**

   !["SQL Server - öğeyi oluşturulduğunda" seçin tetikleyici](./media/connectors-create-api-sqlazure/sql-server-trigger.png)

3. Bağlantı ayrıntılarını istenirse [SQL Bağlantısı şimdi oluşturmak](#create-connection). 
   Ya da bağlantı zaten varsa, seçin **tablo adı** listeden istediğiniz.

   ![Tablo seçin](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

4. Ayarlama **aralığı** ve **sıklığı** mantıksal uygulamanızı tablo ne sıklıkla denetleyeceğini belirtebilir özellikleri.

   Bu örnekte, yalnızca seçili tablo, hiçbir şey denetler. 
   Daha farklı işlemler gerçekleştirmek istediğiniz görevleri eylemler ekleyin. 
   
   Örneğin, tabloda yeni öğeyi görüntülemek için diğer eylemler eklemek gibi tablodan alan sahip bir dosya oluşturun ve e-posta uyarıları göndermek. 
   Bu bağlayıcı veya diğer bağlayıcıları için diğer eylemler hakkında bilgi edinmek için [Logic Apps Bağlayıcılar](../connectors/apis-list.md).

5. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**. 

   Bu adım, otomatik olarak etkinleştirir ve mantıksal uygulamanızı azure'da yayımlar. 

<a name="add-sql-action"></a>

## <a name="add-sql-action"></a>SQL Eylem Ekle

Azure mantıksal uygulamaları içinde bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) tetikleyicinin veya başka bir eylem izler, iş akışınızı bir adımdır. Bu örnekte, mantıksal uygulama ile başlayan [yineleme tetikleyici](../connectors/connectors-native-recurrence.md)ve bir satır SQL veritabanından alır bir eylemi çağırır.

1. Azure portal ya da Visual Studio Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. Bu örnek, Azure portalını kullanır.

2. Tetikleyici veya eylemi altında mantığı Uygulama Tasarımcısı'nda seçin **yeni adım** > **Eylem Ekle**.

   !["Yeni adım"Eylem Ekle"", seçin](./media/connectors-create-api-sqlazure/add-action.png)
   
   Varolan adımlar arasındaki bir eylem eklemek amacıyla bağlanan oku fareyi hareket ettirin. 
   Artı işaretini seçin (**+**) görünür ve ardından **Eylem Ekle**.

2. Arama kutusuna "sql server", filtre olarak girin. Eylemler listesinden istediğiniz herhangi bir SQL işlem seçin. 

   Bu örnekte, tek bir kaydını alır bu eylem seçin: **SQL Server - Get satır**

   !["Sql server" girin, "SQL Server - Get satır" seçin](./media/connectors-create-api-sqlazure/select-sql-get-row.png) 

3. Bağlantı ayrıntılarını istenirse [SQL Bağlantısı şimdi oluşturmak](#create-connection). 
   Veya bağlantınız varsa, seçin bir **tablo adı**ve girin **satır kimliği** istediğiniz kaydı için.

   ![Tablo adı girin ve satır kimliği](./media/connectors-create-api-sqlazure/table-row-id.png)
   
   Bu örnek yalnızca bir satır seçili tablodan, hiçbir şey döndürür. 
   Bu satırda verileri görüntülemek için daha sonra gözden geçirmek için satırdan alanlara sahip bir dosya oluşturun diğer eylemler ekleyin ve bu dosyayı bir bulut depolama hesabında depolamak. Bu bağlayıcı ya da diğer bağlayıcıları diğer eylemler hakkında bilgi edinmek için [Logic Apps Bağlayıcılar](../connectors/apis-list.md).

4. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**. 

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="process-data-in-bulk"></a>Toplu işlem verileri

Bağlayıcı aynı anda tüm sonuçlar döndürmez veya yapı ve boyutu üzerinde daha iyi denetim sonuç kümeleri için istediğiniz kadar büyük sonuç kümeleri ile çalışırken, kullanabileceğiniz *sayfalandırma*, hangi olanlar yönetmenize yardımcı Sonuç olarak daha küçük ayarlar. 

[!INCLUDE [Set up pagination for results exceeding default page size](../../includes/connectors-pagination-bulk-data-transfer.md)]

### <a name="create-a-stored-procedure"></a>Saklı yordam oluşturma

Alma veya birden çok satır ekleme, mantıksal uygulamanızı bu öğeler kullanarak yineleyebilirsiniz bir [ *döngü kadar* ](../logic-apps/logic-apps-control-flow-loops.md#until-loop) bunlar içinde [sınırları](../logic-apps/logic-apps-limits-and-config.md). Ancak, bazen mantıksal uygulamanızı binlerce veya çağrılar veritabanı için maliyetleri en aza indirmek istediğiniz satırları, milyonlarca gibi çok büyük kayıt kümeleriyle çalışmak sahip değil. 

Bunun yerine, oluşturabileceğiniz bir <a href="https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine" target="blank"> *saklı yordamı* </a> , SQL örneğinizin çalışır ve kullanır **seçin - ORDER BY** deyimi sonuçları istediğiniz gibi düzenleyin. Bu çözüm büyüklüğüne ve yapısına sonuçlarınızı üzerinde daha fazla denetim sağlar. SQL Server bağlayıcı'nın kullanarak mantıksal uygulamanızı saklı yordam çağrıları **saklı yordam yürütme** eylem. 

Çözüm Ayrıntılar için bu makalelere bakın:

* <a href="https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx" target="_blank">Logic Apps ile SQL sayfalandırma toplu veri aktarımı için</a>

* <a href="https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql" target="_blank">SEÇİN - BY yan tümcesi sipariş</a>

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Bu bağlayıcı'nın tetikleyiciler, Eylemler ve sınırları hakkında teknik bilgi için bkz: [bağlayıcı başvuru ayrıntıları](/connectors/sql/). 

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcılar](../connectors/apis-list.md)

