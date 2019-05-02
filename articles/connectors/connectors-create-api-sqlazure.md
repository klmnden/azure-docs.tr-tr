---
title: SQL Server veya Azure SQL veritabanı - Azure Logic Apps bağlayın | Microsoft Docs
description: Erişim ve Azure Logic Apps ile iş akışlarını otomatikleştirerek şirket içinde veya bulutta bulunan SQL veritabanlarını yönetme
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
ms.openlocfilehash: 998fcba50636cd92b14bdbe1633c2548e84a6bfc
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696414"
---
# <a name="connect-to-sql-server-or-azure-sql-database-from-azure-logic-apps"></a>Azure Logic Apps'ten SQL Server veya Azure SQL veritabanına bağlanma

Bu makalede, SQL Server Bağlayıcısı ile bir mantıksal uygulama içinde SQL veritabanınızdaki verileri nasıl erişeceği gösterilmektedir. Böylece, görevler, süreçleri ve SQL veri ve kaynaklarınız mantıksal uygulamalar oluşturarak yönetme iş akışlarını otomatik hale getirebilirsiniz. Bağlayıcı için hem de çalışır [şirket içi SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) ve [Azure SQL veritabanı bulutta](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview). 

SQL veritabanınız veya Dynamics CRM Online gibi diğer sistemlerde olaylar tarafından tetiklendiğinde çalışan mantıksal uygulamalar oluşturabilirsiniz. Ayrıca mantıksal uygulamalarınızı alma, ekleme ve SQL sorguları ve saklanan yordamların yürütülmesi yanı sıra veri silme. Örneğin, Dynamics CRM Online içinde yeni kayıtlar için otomatik olarak denetler, tüm kayıtları için SQL veritabanı sunucunuza öğeler ekler ve ardından e-posta uyarıları gönderir, bir mantıksal uygulama oluşturabilirsiniz.

Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bağlayıcısı özel teknik bilgiler için bkz. <a href="https://docs.microsoft.com/connectors/sql/" target="blank">SQL Server bağlayıcı başvurusu</a>.

## <a name="prerequisites"></a>Önkoşullar

* Mantıksal uygulama burada SQL veritabanınıza erişim gerekir. Bir SQL tetikleyicisi ile birlikte mantıksal uygulamanızı başlatmak için gereken bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Bir [Azure SQL veritabanı](../sql-database/sql-database-get-started-portal.md) veya [SQL Server veritabanı](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database) 

  Mantıksal uygulamanızı sonuçları operations çağırırken geri dönebilmek tablolarınızı veri olması gerekir. Bir Azure SQL veritabanı oluşturun, dahil olan örnek veritabanları kullanabilirsiniz. 

* SQL sunucu adınızı, veritabanı adı, kullanıcı adınızı ve parolanızı. Mantıksal SQL sunucunuza erişmek için yetki verebilir böylece, bu kimlik bilgileri gerekir. 

  * Azure SQL veritabanı için bu Ayrıntılar bağlantı dizesindeki ya da Azure portalında SQL veritabanı özellikleri bölümünde bulabilirsiniz:

    "Server = tcp: <*SunucuAdınız*>. katalog database.windows.net,1433;Initial = <*yourDatabaseName*>; Persist Security Info = False; Kullanıcı Kimliği = <*kullanıcıadınız*>; Parola = <*yourPassword*>; MultipleActiveResultSets = False; Şifreleme = True; TrustServerCertificate = False; Bağlantı zaman aşımı = 30; "

  * SQL Server için bu Ayrıntılar bağlantı dizesinde bulabilirsiniz: 

    "Server=<*yourServerAddress*>;Database=<*yourDatabaseName*>;User Id=<*yourUserName*>;Password=<*yourPassword*>;"

* Logic apps, SQL Server gibi şirket içi sistemlere bağlanabilmeniz gerekir [bir şirket içi veri ağ geçidi ayarlama](../logic-apps/logic-apps-gateway-install.md). Bu şekilde mantıksal uygulamanız için SQL bağlantı oluşturduğunuzda ağ geçidini seçebilirsiniz.

<a name="add-sql-trigger"></a>

## <a name="add-sql-trigger"></a>SQL tetikleyicisi Ekle

Azure Logic Apps'te, her mantıksal uygulama ile başlamalıdır bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay harekete geçirilir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her zaman tetikleyici Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

1. Azure portalı ya da Visual Studio, Logic Apps Tasarımcısı açılır bir boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

2. Arama kutusuna filtreniz olarak "sql server" girin. Tetikleyiciler listesinden istediğiniz SQL tetikleyicisini seçin. 

   Bu örnek için şu tetikleyiciyi seçin: **SQL Server - bir öğe oluşturulduğunda**

   !["SQL Server - bir öğe oluşturulduğunda"'ı tetikleyicisi](./media/connectors-create-api-sqlazure/sql-server-trigger.png)

3. Bağlantı ayrıntıları için istenirse [artık SQL bağlantınızı oluşturmak](#create-connection). 
   Veya, bağlantınız zaten varsa, seçin **tablo adı** listeden istediğiniz.

   ![Tablo seçin](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

4. Ayarlama **aralığı** ve **sıklığı** özellikleri, mantıksal uygulamanızı tablo ne sıklıkla denetleyeceğini belirtebilir.

   Bu örnekte, yalnızca seçili tablo, başka bir şey denetler. 
   Daha çok ilginizi çeken bir şey yapmak için gerçekleştirmek istediğiniz görevleri eylemler ekleyin. 
   
   Örneğin, tabloda yeni öğe görüntülemek için diğer eylemler ekleyin gibi tablodan alan içeren bir dosya oluşturun ve ardından e-posta uyarıları göndermek. 
   Bu bağlayıcı veya diğer bağlayıcıları için diğer eylemler hakkında bilgi edinmek için bkz. [Logic Apps bağlayıcıları](../connectors/apis-list.md).

5. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**. 

   Bu adım, otomatik olarak sağlar ve mantıksal uygulamanızı Canlı Azure'da yayımlar. 

<a name="add-sql-action"></a>

## <a name="add-sql-action"></a>SQL Eylem Ekle

Azure Logic apps'te bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) akışınıza bir tetikleyici veya başka bir eylem izleyen bir adımdır. Bu örnekte, mantıksal uygulama ile başlar [yinelenme tetikleyicisini](../connectors/connectors-native-recurrence.md)ve SQL veritabanı'ndan bir satır alır bir eylem çağırır.

1. Azure portalı ya da Visual Studio, mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın. Bu örnek, Azure portalını kullanır.

2. Mantıksal Uygulama Tasarımcısı, tetikleyici veya eylemi seçin **yeni adım** > **Eylem Ekle**.

   !["Yeni adım"Eylem Ekle"", seçin](./media/connectors-create-api-sqlazure/add-action.png)
   
   Var olan adımlar arasında bir eylem eklemek için bağlantı okun üzerine fareyi hareket ettirin. 
   Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

2. Arama kutusuna filtreniz olarak "sql server" girin. Eylem listesinden istediğiniz herhangi bir SQL işlem'i seçin. 

   Bu örnekte, tek bir kaydı alır. Bu eylemi seçin: **SQL Server - satır Al**

   ![Girin "sql server", "SQL Server - Get satır" seçin](./media/connectors-create-api-sqlazure/select-sql-get-row.png) 

3. Bağlantı ayrıntıları için istenirse [artık SQL bağlantınızı oluşturmak](#create-connection). 
   Veya, bağlantınız varsa, seçin bir **tablo adı**girin **satır kimliği** istediğiniz kaydı.

   ![Tablo adını girin ve satır kimliği](./media/connectors-create-api-sqlazure/table-row-id.png)
   
   Bu örnek seçili tablosundan başka bir şey yalnızca bir satır döndürür. 
   Bu satırdaki verileri görüntülemek için daha sonra gözden geçirmek için satırdan alanlara sahip bir dosya oluşturun. diğer eylemler ekleyin ve bu dosyayı bulut depolama hesabında depolama. Diğer Eylemler bu bağlayıcının ya da diğer bağlayıcılar hakkında bilgi edinmek için bkz. [Logic Apps bağlayıcıları](../connectors/apis-list.md).

4. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**. 

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="handle-bulk-data"></a>Toplu veri işleme

Bazı durumlarda, bağlayıcı aynı anda tüm sonuçlar döndürmüyor veya büyüklüğüne ve yapısına üzerinde daha iyi denetim sonucu kümeleriniz için istediğiniz kadar büyük sonuç kümeleri çalışmak gerekebilir. Bu büyük sonuç kümelerini işleyebilir bazı yöntemler aşağıda verilmiştir:

* Sonuçları daha küçük kümeleri olarak yönetmenize yardımcı olmak için açma *sayfalandırma*. Daha fazla bilgi için [toplu veri, kayıt ve öğeleri sayfalandırma kullanarak elde](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md).

* Sonuçlar istediğiniz şekilde düzenler bir saklı yordamı oluşturun.

  Alma veya birden çok satır ekleme, mantıksal uygulamanız bu satırlara kullanarak yineleyebilirsiniz bir [ *until döngüsü* ](../logic-apps/logic-apps-control-flow-loops.md#until-loop) bunlar içinde [sınırları](../logic-apps/logic-apps-limits-and-config.md). 
  Ancak kayıt kümeleri çok büyük, örneğin binlerce veya milyonlarca satır, veritabanı çağrıları kaynaklanan maliyetleri en aza indirmek istediğiniz çalışmaya olduğunda mantıksal uygulamanız gerekir.

  Sonuçları, istediğiniz şekilde düzenlemek için oluşturabileceğiniz bir [ *saklı yordamı* ](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) kullanır ve SQL örneği'nde çalışan **seçin - ORDER BY** deyimi. 
  Bu çözüm, boyutu ve sonuçlarınızı yapısı üzerinde daha fazla denetim sağlar. 
  SQL Server bağlayıcının kullanarak mantıksal uygulamanızı saklı yordam çağrıları **saklı yordam yürütme** eylem.

  Çözüm Ayrıntılar için şu makalelere bakın:

  * [Logic Apps ile toplu veri aktarımı için SQL sayfalandırma](https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx)

  * [SEÇİN - ORDER BY yan tümcesi](https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql)

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Bu bağlayıcının tetikleyicileri, Eylemler ve sınırları hakkında teknik bilgiler için bkz. [bağlayıcının başvuru ayrıntıları](/connectors/sql/). 

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)

