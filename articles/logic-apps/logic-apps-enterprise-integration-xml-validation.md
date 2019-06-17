---
title: B2B Kurumsal tümleştirme - Azure Logic Apps için XML doğrulama | Microsoft Docs
description: XML şemaları için Azure Logic Apps Enterprise Integration Pack ile B2B çözümleri ile doğrula
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.date: 07/08/2016
ms.openlocfilehash: 8db0dbadd944007ff953f9ea69695bf988ffebb7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60996351"
---
# <a name="validate-xml-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic apps'teki B2B Kurumsal tümleştirme için XML doğrulama

Genellikle B2B senaryolarda, bir anlaşma ortak veri işleme başlamadan önce kullanıcılar exchange ileti geçerli emin olmanız gerekir. Enterprise Integration Pack kullanımı XML doğrulama Bağlayıcısı'nı kullanarak belgeleri önceden tanımlanmış bir şemaya karşı doğrulayabilir.

## <a name="validate-a-document-with-the-xml-validation-connector"></a>XML doğrulaması Bağlayıcısı ile bir belgeyi doğrula

1. Bir mantıksal uygulama oluşturma ve [app tümleştirme hesabı'na bağlantı](../logic-apps/logic-apps-enterprise-integration-accounts.md "öğrenmek için mantıksal uygulama tümleştirme hesabı bağlamak") XML verileri doğrulamak için kullanmak istediğiniz şema sahiptir.

2. Ekleme bir **isteği - zaman bir HTTP isteği alındığında** mantıksal uygulamanızın tetikleyicisi.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-1.png)

3. Eklenecek **XML doğrulama** eylemi seçin **Eylem Ekle**.

4. Tüm Eylemler, istediğiniz bir filtre uygulamak için girin *xml* arama kutusuna. Seçin **XML doğrulama**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-2.png)

5. Doğrulamak istediğiniz XML içeriği belirtmek için seçin **içerik**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-1-5.png)

6. Doğrulamak istediğiniz içerik gövde etiketini seçin.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-3.png)

7. Önceki doğrulamak için kullanmak istediğiniz şema belirtmek için *içeriği* giriş öğesini **şema adı**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-4.png)

8. Çalışmanızı kaydedin  

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-5.png)

Şimdi, doğrulama Bağlayıcınızı ayarı ile gerçekleştirilir. Gerçek bir uygulamada, SalesForce gibi bir satır iş kolu (LOB) uygulaması doğrulanmış veri depolamak isteyebilirsiniz. Salesforce'a doğrulanmış çıkış göndermek için bir eylem ekleme.

Doğrulama işleminizi test etmek için HTTP uç noktaya bir istek olun.

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")   

