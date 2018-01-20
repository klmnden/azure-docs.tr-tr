---
title: "Mantıksal uygulamalarınızı Azure SQL Veritabanı Bağlayıcısı ekleme | Microsoft Docs"
description: "Azure SQL veritabanı bağlayıcı REST API parametrelerle genel bakış"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d8a319d0-e4df-40cf-88f0-29a6158c898c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: def2b65f009c377233c45356f8fa661b86d73f51
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-azure-sql-database-connector"></a>Azure SQL Veritabanı Bağlayıcısı ile çalışmaya başlama
Azure SQL Veritabanı Bağlayıcısı'nı kullanarak, tablolardaki verileri yönetmek, kuruluşunuz için iş akışları oluşturun. 

SQL Database:

* Müşteriler veritabanına yeni bir müşteri ekleyerek veya bir sırada siparişler veritabanını güncelleştirmek, iş akışı oluşturma.
* Bir satır veri almak, yeni bir satır ekleyin ve hatta silmek için Eylemler kullanın. Örneğin, bir kayıt Dynamics CRM Online içinde (tetikleyici) oluşturulduğunda, bir satır bir Azure SQL veritabanı'nda (bir eylem) ekleyin. 

Bu konuda, bir mantıksal uygulama SQL Veritabanı Bağlayıcısı'nı kullanmayı gösterir ve ayrıca eylemleri listeler.

Logic Apps hakkında daha fazla bilgi için bkz: [logic apps nedir](../logic-apps/logic-apps-overview.md) ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-azure-sql-database"></a>Azure SQL veritabanına bağlan
Mantıksal uygulamanızı herhangi bir hizmete erişebilmesi için önce oluşturduğunuz bir *bağlantı* hizmet. Bağlantı bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, SQL veritabanına bağlanmak için önce bir SQL veritabanı oluşturma *bağlantı*. Bir bağlantı oluşturmak için normalde bağlandığınız hizmete erişmek için kullandığınız kimlik bilgilerini girin. Bu nedenle, SQL veritabanı'nda bağlantı oluşturmak için SQL veritabanı kimlik bilgilerinizi girin. 

#### <a name="create-the-connection"></a>Bağlantı oluşturma
> [!INCLUDE [Create the connection to SQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a>Bir tetikleyici kullanın
Bu bağlayıcı hiçbir tetikleyici yok. Bir yineleme tetikleyici, bir HTTP Web kancası tetikleyici, tetikleyici diğer bağlayıcıları ve daha fazla ile kullanılabilir gibi mantıksal uygulamayı başlatmak için diğer Tetikleyicileri kullanın. [Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) bir örnek sağlar.

## <a name="use-an-action"></a>Bir eylem kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Artı işaretini seçin. Birkaç seçeneğiniz bkz: **Eylem Ekle**, **bir koşul eklemek**, veya biri **daha fazla** seçenekleri.
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. Seçin **Eylem Ekle**.
3. Metin kutusuna, kullanılabilir tüm eylemlerin bir listesini almak için "sql" yazın.
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. Bizim örneğimizde seçin **SQL Server - Get satır**. Bir bağlantı zaten varsa, ardından **tablo adı** açılan dan listesinde ve girin **satır kimliği** döndürmek istediğiniz.
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    Bağlantı bilgilerini istenirse, bağlantı oluşturmak için ayrıntılarını girin. [Bağlantı oluşturmak](connectors-create-api-sqlazure.md#create-the-connection) bu konuda bu özellikleri açıklar. 
   
   > [!NOTE]
   > Bu örnekte, biz bir tablodan bir satırı döndürür. Bu satır verileri görmek için tablodaki alanların kullanarak bir dosya oluşturur başka bir eylem ekleyin. Örneğin, bulut depolama hesabında yeni bir dosya oluşturmak için adı ve Soyadı alanlarını kullanan bir OneDrive eylem ekleyin. 
   > 
   > 
5. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/sql/). 

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API'leri listesi](apis-list.md).

