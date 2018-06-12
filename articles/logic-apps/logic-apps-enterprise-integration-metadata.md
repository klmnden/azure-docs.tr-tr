---
title: Tümleştirme hesap yapı meta verileri - Azure mantıksal uygulamaları yönetme | Microsoft Docs
description: Ekleme veya yapı meta veri tümleştirme hesaplarından Azure Logic Apps için alma
author: padmavc
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/23/2018
ms.author: LADocs; padmavc
ms.openlocfilehash: 3e7ef6aef9bc1062ae0f76adfbaf086961fcaa94
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298374"
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>Tümleştirme hesapları logic apps için yapı meta verilerini yönetme

Bu meta veri çalışma zamanı sırasında mantıksal uygulamanızı almak ve tümleştirme hesaplarında yapıları için özel meta verileri tanımlayın. Örneğin, iş ortakları, anlaşmaları, şemalar ve haritalar - gibi yapılarını meta verilerini anahtar-değer çiftleri kullanarak tüm deposu meta verilerini belirtebilirsiniz. 

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Meta veri yapıları tümleştirme hesapları ekleyin

1. Azure portalında oluşturma bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md).

2. Örneğin bir yapı tümleştirme hesabınıza ekleyin, bir [iş ortağı](logic-apps-enterprise-integration-partners.md), [sözleşmesi](logic-apps-enterprise-integration-agreements.md), veya [şema](logic-apps-enterprise-integration-schemas.md).

3. Yapıyı seçin, **Düzenle**ve meta veri ayrıntılarını girin.

   ![Meta veri girin](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Logic apps için yapılardan meta veri alma

1. Azure portalında oluşturma bir [mantıksal uygulama](quickstart-create-first-logic-app-workflow.md).

2. Oluşturma bir [mantıksal uygulamanızı bir bağlantıdan tümleştirme hesabınıza](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

3. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici gibi ekleme **isteği** veya **HTTP** mantığı uygulamanıza.

4. Tetikleyici altında seçin **yeni adım** > **Eylem Ekle**. Bul ve bu eylem seçin "tümleştirme hesabı" için arama: **tümleştirme hesabı - tümleştirme hesap yapı arama**

   ![Tümleştirme hesap yapı arama seçin](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Seçin **yapay nesne türü** ve sağlamak **yapı adı**. Örneğin:

   ![Yapay nesne türü seçin ve yapı adı belirtin](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Örnek: Alma ortak meta veriler

Bu meta verileri ile bu iş ortağı sahip olduğunu varsayın `routingUrl` ayrıntıları:

![İş ortağı "routingURL" meta veri bulma](media/logic-apps-enterprise-integration-metadata/image6.png)

1. Mantıksal uygulamanızı, tetikleyici eklemek bir **tümleştirme hesabı - tümleştirme hesap yapı arama** için iş ortağınızla için eylem ve bir **HTTP** eylem, örneğin:

   ![Tetikleyici, yapı arama ve HTTP eylemi mantığı uygulamanıza ekleme](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Mantıksal Uygulama Tasarımcısı araç çubuğunda URI almak için seçin **kod görünümü** mantığı uygulamanız için. Mantıksal uygulama tanımını bu örnekteki gibi görünmelidir:

   ![Arama arama](media/logic-apps-enterprise-integration-metadata/image5.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Anlaşmaları hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-agreements.md)
