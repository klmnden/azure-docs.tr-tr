---
title: Tümleştirme hesabı yapıt meta verileri - Azure Logic Apps yönetme | Microsoft Docs
description: Azure Logic Apps Enterprise Integration Pack ile tümleştirme hesapları yapıt meta verileri almak veya ekleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 02/23/2018
ms.openlocfilehash: 537014c2780fe94cfb35806759f8bcbd974c4c95
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128812"
---
# <a name="manage-artifact-metadata-from-integration-accounts-in-azure-logic-apps-with-enterprise-integration-pack"></a>Yapıt meta verileri Azure Logic Apps Enterprise Integration Pack ile tümleştirme hesapları yönetin

Tümleştirme hesapları yapıtlar için özel meta verileri tanımlayabilir ve bu meta veri çalışma zamanı sırasında mantıksal uygulamanız için alınamıyor. Örneğin, anahtar-değer çiftleri kullanarak tüm meta veri depolama, iş ortakları, sözleşmeler, şemalar ve haritalar - gibi yapıtlar için meta verileri belirtebilirsiniz. 

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Tümleştirme hesapları yapılara meta verileri ekleme

1. Azure portalında oluşturma bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md).

2. Tümleştirme hesabına, örneğin bir yapıt ekleyin, [iş ortağı](logic-apps-enterprise-integration-partners.md), [sözleşmesi](logic-apps-enterprise-integration-agreements.md), veya [şema](logic-apps-enterprise-integration-schemas.md).

3. Bir yapı seçin, **Düzenle**ve meta verileri ayrıntılarını girin.

   ![Meta verileri girin](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Logic apps için yapılardan meta verileri alma

1. Azure portalında oluşturma bir [mantıksal uygulama](quickstart-create-first-logic-app-workflow.md).

2. Oluşturma bir [mantıksal uygulamanızın bir bağlantıdan tümleştirme hesabınıza](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

3. Logic Apps Tasarımcısı'nda gibi bir tetikleyici ekleme **istek** veya **HTTP** mantıksal uygulamanız için.

4. Tetikleyici altında seçin **yeni adım** > **Eylem Ekle**. Arama "tümleştirme hesabı için" bulun ve ardından şu eylemi seçin: **tümleştirme hesabı - tümleştirme hesabı Yapıt arama**

   ![Tümleştirme hesabı Yapıt arama seçin](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Seçin **Yapıt türü** ve **Yapıt adı**. Örneğin:

   ![Yapıt türü seçin ve yapıt adını belirtin](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Örnek: Meta veri alınamadı iş ortağı

Bu meta veriler ile bu iş ortağı sahip olduğunu varsayın `routingUrl` ayrıntıları:

![İş ortağı "routingURL" meta veri Bul](media/logic-apps-enterprise-integration-metadata/image6.png)

1. Mantıksal uygulamanızı, tetikleyici ekleme bir **tümleştirme hesabı - tümleştirme hesabı Yapıt arama** , iş ortağı için eylem ve bir **HTTP** eylem, örneğin:

   ![Mantıksal uygulamanızın tetikleyici yapıt arama ve HTTP eylemi ekleme](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Mantıksal Uygulama Tasarımcısı araç çubuğunda URI alınacak seçin **kod görünümü** mantıksal uygulamanız için. Mantıksal uygulama tanımınızı şu örnekteki gibi görünmelidir:

   ![Arama arama](media/logic-apps-enterprise-integration-metadata/image5.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Sözleşmeleri hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-agreements.md)
