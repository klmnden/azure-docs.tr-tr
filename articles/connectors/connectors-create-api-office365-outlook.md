---
title: "Office 365 Outlook Bağlayıcısı Logic Apps içinde ekleme | Microsoft Docs"
description: "Office 365 ile etkileşimi etkinleştirmek için Office 365 Bağlayıcısı ile mantıksal uygulamalar oluşturun. Örneğin: oluşturma, düzenleme ve kişiler ve takvim öğeleri güncelleştirme."
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 51b8e3de639b5cce954547adb77ff13b79ad6747
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-office-365-outlook-connector"></a>Office 365 Outlook Bağlayıcısı ile çalışmaya başlama
Office 365 Outlook Bağlayıcısı, Office 365'te Outlook ile etkileşim sağlar. Bu Bağlayıcıyı oluşturmak, düzenlemek ve kişiler ve takvim öğeleri, güncelleştirme ve ayrıca almak, Gönder ve e-posta için için kullanın.

Office 365 Outlook ile:

* Office 365 e-posta ve Takvim özeliklerin kullanarak, iş akışı oluşturma. 
* Tetikleyiciler, bir Takvim öğesi güncelleştirildiğinde bir yeni e-posta ve daha fazla olduğunda, iş akışını başlatmak için kullanın.
* Eylemler bir e-posta Gönder, yeni bir takvim olayı ve daha fazlasını oluşturmak için kullanın. Örneğin, Salesforce (tetikleyici) yeni bir nesne olduğunda, Office 365 Outlook (bir eylem) bir e-posta gönderin. 

Bu konuda, bir mantıksal uygulama Office 365 Outlook Bağlayıcısı'nı kullanmayı gösterir ve ayrıca tetikleyiciler ve eylemler listelenmektedir.

> [!NOTE]
> Makalenin bu sürümü Logic Apps genel kullanılabilirlik (GA) için geçerlidir.
> 
> 

Logic Apps hakkında daha fazla bilgi için bkz: [logic apps nedir](../logic-apps/logic-apps-overview.md) ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-office-365"></a>Office 365’e bağlanın
Mantıksal uygulamanızı herhangi bir hizmete erişebilmesi için önce oluşturduğunuz bir *bağlantı* hizmet. Bağlantı bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, Office 365 Outlook'a bağlanmak için önce bir Office 365 gerekir *bağlantı*. Bir bağlantı oluşturmak için normalde bağlanmak istediğiniz hizmete erişmek için kullandığınız kimlik bilgilerini girin. Bu nedenle Office 365 Outlook ile bağlantı oluşturmak için Office 365 hesabınıza kimlik bilgilerini girin.

## <a name="create-the-connection"></a>Bağlantı oluşturma
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Bir tetikleyici kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. Tetikleyiciler "hizmet bir aralığı ve istediğiniz sıklığı yoklama". [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Mantıksal uygulama "Tetikleyiciler listesini almak için office 365" yazın:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Seçin **Office 365 yaklaşan bir olay yakında başlatırken Outlook -**. Bir bağlantı zaten varsa, bir takvim aşağı açılan listeden seçin.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Oturum açmak için istenirse, oturum bağlantısı oluşturmak için Ayrıntılar girin. [Bağlantı oluşturmak](connectors-create-api-office365-outlook.md#create-the-connection) bu konudaki adımları listeler. 
   
   > [!NOTE]
   > Takvim olay güncelleştirildiğinde bu örnekte, mantıksal uygulama çalışır. Bu tetikleyici sonuçlarını görmek için bir kısa mesaj gönderir başka bir eylem ekleyin. Örneğin, Twilio ekleyin *ileti gönder* eylem bu metinleri, takvim olayının 15 dakika içinde başlatırken. 
   > 
   > 
3. Seçin **Düzenle** düğmesine tıklayın ve ayarlama **sıklığı** ve **aralığı** değerleri. Örneğin, 15 dakikada bir yoklamak için tetikleyicinin isterseniz, daha sonra ayarlamak **sıklığı** için **dakika**ve **aralığı** için **15**. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.

## <a name="use-an-action"></a>Bir eylem kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Artı işaretini seçin. Birkaç seçeneğiniz bkz: **Eylem Ekle**, **bir koşul eklemek**, veya biri **daha fazla** seçenekleri.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. Seçin **Eylem Ekle**.
3. Metin kutusuna "kullanılabilir tüm eylemlerin bir listesini almak için office 365" yazın.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. Bizim örneğimizde seçin **Office 365 Outlook - kişi oluşturma**. Bir bağlantı zaten varsa, ardından **klasörü kimliği**, **verilen ad**ve diğer özellikleri:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Bağlantı bilgilerini istenirse, bağlantı oluşturmak için ayrıntılarını girin. [Bağlantı oluşturmak](connectors-create-api-office365-outlook.md#create-the-connection) bu konuda bu özellikleri açıklar. 
   
   > [!NOTE]
   > Bu örnekte, Office 365 Outlook içinde yeni bir kişi oluşturun. İlgili kişi oluşturmanız için başka bir tetikleyici çıktısını kullanabilirsiniz. Örneğin, SalesForce ekleyin *bir nesne oluşturulduğunda* tetikleyici. Office 365 Outlook eklemek *kişi oluşturma* SalesForce alanları Office 365'te yeni yeni kişi oluşturmak için kullandığı eylem. 
   > 
   > 
5. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/office365connector/). 

## <a name="next-steps"></a>Sonraki Adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API'leri listesi](apis-list.md).

