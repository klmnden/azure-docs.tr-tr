---
title: Azure Logic Apps B2B Kurumsal tümleştirmeleri - oluştur | Microsoft Docs
description: Azure Logic Apps Enterprise Integration Pack ile B2B veri alır
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.date: 07/08/2016
ms.openlocfilehash: 05368f627c5e9482a43d5e30b0e16b1d47f6217c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60999186"
---
# <a name="receive-b2b-data-with-azure-logic-apps-and-enterprise-integration-pack"></a>Azure Logic Apps ve Enterprise Integration Pack ile B2B veri alma

İş ortakları ve anlaşmalar olan tümleştirme hesabı oluşturduktan sonra mantıksal uygulamanız için işletmeler arası (B2B) iş akışı oluşturmak hazır olursunuz [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Önkoşullar

AS2 ve X12 kullanmak için Eylemler, Kurumsal tümleştirme hesabı olması gerekir. Bilgi [Kurumsal tümleştirme hesabı oluşturma](../logic-apps/logic-apps-enterprise-integration-accounts.md).

## <a name="create-a-logic-app-with-b2b-connectors"></a>Mantıksal uygulama B2B Bağlayıcılarla oluşturma

AS2 ve X12 kullanan B2B mantıksal uygulama oluşturmak için bu adımları bir alım-satım ortağından veri almak için Eylemler:

1. Bir mantıksal uygulama oluşturup [uygulamanızı tümleştirme hesabınıza bağlayın](../logic-apps/logic-apps-enterprise-integration-accounts.md).

2. Ekleme bir **isteği - zaman bir HTTP isteği alındığında** mantıksal uygulamanızın tetikleyicisi.

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. Eklenecek **AS2 kod çözme** seçme eylemini **Eylem Ekle**.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. Tüm Eylemler, istediğiniz bir filtre uygulamak için sözcüğünü girin **as2** arama kutusuna.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. Seçin **AS2 - AS2 kod çözme ileti** eylem.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. Ekleme **gövdesi** giriş olarak kullanılmasını istediğiniz. 
   Bu örnekte, mantıksal uygulama tetiklenir HTTP isteği gövdesinin seçin. Üst bilgilerinde girişlerinin bir ifade girin veya **ÜSTBİLGİLERİ** alan:

    @triggerOutputs() ['üst bilgileri']

7. Gerekli olanları Ekle **üstbilgileri** için AS2, HTTP istek üst bilgilerinde bulabilirsiniz. 
   Bu örnekte, mantıksal uygulama tetikleyicisi üst bilgiler HTTP isteğinin seçin.

8. Artık kod çözme X12 ileti eylemi ekleyin. Seçin **Eylem Ekle**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. Tüm Eylemler, istediğiniz bir filtre uygulamak için sözcüğünü girin **x12** arama kutusuna.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. Seçin **X12-kod çözme X12 ileti** eylem.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. Şimdi, bu eylem için giriş belirtmeniz gerekir. 
    Bu giriş AS2 önceki eylem çıktısı bulunmaktadır.

    Gerçek ileti içeriği bir JSON nesnesidir ve base64 ile kodlanmış, olduğundan bir ifade giriş olarak belirtmeniz gerekir. 
    Aşağıdaki ifade girin **X12 DÜZ dosya iletisi için kod çözme** giriş alanı:
    
    @base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Content'])

    Şimdi veri öğeleri bir JSON nesnesinde çıkış ve ticaret iş ortağı alınan X12 kodunu çözmek için adımları ekleyin. 
    İş ortağı veri alındığını bildirmek için geri AS2 iletisi değerlendirme bildirim (MDN'de) bir HTTP yanıt eylemi içeren bir yanıt gönderebilir.

12. Eklenecek **yanıt** eylemi seçin **Eylem Ekle**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. Tüm Eylemler, istediğiniz bir filtre uygulamak için sözcüğünü girin **yanıt** arama kutusuna.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. Seçin **yanıt** eylem.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. MDN çıktısından erişmeye **kod çözme X12 ileti** eylemi, yanıt olarak **GÖVDESİ** Bu ifade ile alan:

    @base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Content'])

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. Çalışmanızı kaydedin.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

Artık B2B mantıksal uygulamanızı ayarlama. Gerçek bir uygulamada, kodu çözülmüş X12 saklamak isteyebilirsiniz satır iş kolu (LOB) uygulaması veya veri deposuna verileri. Kendi iş KOLU uygulamalarınızı bağlayın ve mantıksal uygulamanızda bu API'leri kullanmak için ek eylemler ekleyebilir veya özel API'ları yazma.

## <a name="features-and-use-cases"></a>Özellikler ve kullanım örnekleri

* AS2 ve X12 eylemleri kodlanacağını ve logic apps içinde sektör standardı protokolleri kullanarak ticari ortaklar arasında veri alışverişi bırakın.
* Ticari ortaklar ile veri değişimi için olan veya olmayan her AS2 ve X12 kullanabilirsiniz.
* B2B eylemlerinin tümleştirme hesabınızdaki iş ortaklarının ve sözleşmelerin kolayca oluşturun ve bunları mantıksal uygulamada kullanma yardımcı olur.
* Mantıksal uygulamanız diğer eylemler ile genişlettiğinizde, gönderebilir ve diğer uygulamalar ve SalesForce gibi hizmetler arasında veri alma.

## <a name="learn-more"></a>Daha fazla bilgi edinin
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md)
