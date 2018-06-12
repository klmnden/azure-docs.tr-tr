---
title: B2B solutions - Azure mantıksal uygulamaları oluşturma | Microsoft Docs
description: Kurumsal tümleştirme paketinde B2B özelliklerini kullanarak logic apps içinde veri alma
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: jeconnoc
editor: cgronlun
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: a27a413ba9a0d974cf90fe842d5fc325ab308a56
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298126"
---
# <a name="receive-data-in-logic-apps-with-the-b2b-features-in-the-enterprise-integration-pack"></a>Logic apps Enterprise Integration Pack B2B özelliklerinde ile veri alma

İş ortakları ve anlaşmaları olan bir tümleştirme hesap oluşturduktan sonra mantıksal uygulamanızı bir işletmeler için (B2B) iş akışı oluşturmak hazır [Kurumsal tümleştirme paketi](logic-apps-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Önkoşullar

AS2 ve X12 kullanmak için Eylemler, bir kurumsal tümleştirme hesabınızın olması gerekir. Bilgi [Kurumsal tümleştirme hesap oluşturmak nasıl](../logic-apps/logic-apps-enterprise-integration-accounts.md).

## <a name="create-a-logic-app-with-b2b-connectors"></a>İle B2B bağlayıcılar mantıksal uygulama oluşturma

AS2 ve X12 kullanan B2B mantıksal uygulama oluşturmak için aşağıdaki adımları izleyin ticari ortak veri almak için Eylemler:

1. Ardından bir mantıksal uygulama oluşturma [uygulamanızı tümleştirme hesabınıza bağlamak](../logic-apps/logic-apps-enterprise-integration-accounts.md).

2. Ekleme bir **isteği - olduğunda bir HTTP isteği alındığında** mantıksal uygulamanızı tetikleyiciye.

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. Eklemek için **kod çözme AS2** eylem, select **Eylem Ekle**.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. Tüm eylemleri istediğiniz bir filtre uygulamak için sözcüğü girin **as2** arama kutusuna.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. Seçin **AS2 - kod çözme AS2 ileti** eylem.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. Ekleme **gövde** giriş olarak kullanmak istediğiniz. Bu örnekte, mantıksal uygulama tetikler HTTP istek gövdesi seçin. Üst bilgilerde girdi bir ifade girin veya **ÜSTBİLGİLERİ** alan:

    @triggerOutputs() ['üst bilgileri']

7. Gerekli eklemek **üstbilgileri** HTTP istek üst bilgilerinde bulabilirsiniz AS2 için. Bu örnekte, mantıksal uygulama tetikleyen HTTP isteği üstbilgisi seçin.

8. Kod çözme X12 ileti eylemi. Şimdi ekleyin. Seçin **Eylem Ekle**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. Tüm eylemleri istediğiniz bir filtre uygulamak için sözcüğü girin **x12** arama kutusuna.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. Seçin **X12-X12 kod çözme ileti** eylem.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. Şimdi bu eylem için giriş belirtmeniz gerekir. Bu giriş, önceki AS2 eylemi çıktısını olabilir.

    Gerçek ileti içerik bir JSON nesnesinde ve base64 ile kodlanmış, olduğundan, giriş olarak bir ifade belirtmeniz gerekir. 
    Aşağıdaki ifade girin **X12 DÜZ dosya ileti için kod çözme** giriş alanı:
    
    @base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Content'])

    Şimdi veri öğeleri bir JSON nesnesinde çıkış ve ticaret ortağından alınan X12 kod çözme adımlarını ekleyin. 
    İş ortağı veri alındığını bildirmek için AS2 ileti değerlendirme bildirim (MDN) bir HTTP yanıt eylem içeren bir yanıtı geri gönderebilirsiniz.

12. Eklemek için **yanıt** eylemi seçin **Eylem Ekle**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. Tüm eylemleri istediğiniz bir filtre uygulamak için sözcüğü girin **yanıt** arama kutusuna.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. Seçin **yanıt** eylem.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. MDN çıktısından erişmek için **kod çözme X12 ileti** eylemi, yanıt ayarlama **gövde** Bu ifade ile alan:

    @base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Content'])

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. Çalışmanızı kaydedin.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

Şimdi yapılır B2B mantıksal uygulamanızı ayarlama. Gerçek dünya uygulamada kodu çözülmüş X12 saklamak isteyebilirsiniz satır iş kolu (LOB) uygulaması veya veri deposuna verileri. Kendi iş KOLU uygulamalarınızı bağlanmak ve bu API'leri mantıksal uygulamanızı kullanmak için daha fazla eylemler ekleyebilir veya özel API'ları yazma.

## <a name="features-and-use-cases"></a>Özellikleri ve kullanım örnekleri

* AS2 ve X12 Eylemler kodlanacağını ve logic apps içinde endüstri standardı protokoller kullanarak ticaret ortakları arasında veri değişimi bırakın.
* Ticari ortaklar ile veri değişimi için ile veya olmadan birbirine AS2 ve X12 kullanabilirsiniz.
* B2B Eylemler iş ortakları ve anlaşmaları tümleştirme hesabınızı kolayca oluşturun ve bir mantıksal uygulama kullanmasına yardımcı olur.
* Mantıksal uygulamanızı diğer eylemler ile genişlettiğinizde, gönderebilir ve diğer uygulamalarla SalesForce gibi hizmetler arasında veri alırsınız.

## <a name="learn-more"></a>Daha fazla bilgi edinin
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md)
