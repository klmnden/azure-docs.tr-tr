---
title: XML - Azure mantıksal uygulamaları doğrula | Microsoft Docs
description: Enterprise Integration Pack kullanarak Azure Logic Apps ile B2B senaryoları için şemalarda XML doğrulama
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: jeconnoc
editor: cgronlun
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 901f0e576d28ab163fe2c46dff0594a338fbaf73
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299608"
---
# <a name="validate-xml-for-enterprise-integration"></a>XML için Kurumsal tümleştirme doğrula

Genellikle B2B senaryolarda anlaşmanın ortakları veri işleme başlamadan önce bunlar exchange iletileri geçerli emin olmanız gerekir. XML doğrulaması Bağlayıcısı'nı kullanın ve kurumsal tümleştirme paketindeki kullanarak önceden tanımlanmış bir şemayla belgeleri doğrulayabilirsiniz.

## <a name="validate-a-document-with-the-xml-validation-connector"></a>XML doğrulaması Bağlayıcısı ile bir belgeyi doğrula

1. Bir mantıksal uygulama oluşturma ve [uygulama tümleştirmesi hesabınıza bağlamak](../logic-apps/logic-apps-enterprise-integration-accounts.md "bir mantıksal uygulama için bir tümleştirme hesabı bağlamak bilgi") XML verileri doğrulamak için kullanmak istediğiniz şeması vardır.

2. Ekleme bir **isteği - olduğunda bir HTTP isteği alındığında** mantıksal uygulamanızı tetikleyiciye.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-1.png)

3. Eklemek için **XML doğrulama** eylemi seçin **Eylem Ekle**.

4. Filtre uygulamak istediğiniz bir tüm eylemler için girin *xml* arama kutusuna. Seçin **XML doğrulama**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-2.png)

5. Doğrulamak istediğiniz XML içeriği belirtmek için işaretleyin **içerik**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-1-5.png)

6. Gövde etiketi doğrulamak istediğiniz içeriği seçin.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-3.png)

7. Önceki doğrulamak için kullanmak istediğiniz şema belirtmek için *içerik* giriş, seçin **şema adı**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-4.png)

8. Çalışmanızı kaydedin  

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-5.png)

Şimdi, doğrulama Bağlayıcısı'nı ayarlama ile yapılır. Gerçek dünya uygulamada SalesForce gibi bir satır iş kolu (LOB) uygulamasının doğrulanmış verilerini depolamak isteyebilirsiniz. Salesforce doğrulanmış çıkış göndermek için bir eylem ekleyin.

Doğrulama eyleminizi sınamak için HTTP uç noktası için bir isteği oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")   

