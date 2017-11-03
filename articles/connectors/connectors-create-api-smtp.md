---
title: "SMTP Bağlayıcısı Azure Logic Apps içinde | Microsoft Docs"
description: "Logic apps ile Azure uygulama hizmeti oluşturun. E-posta göndermek için SMTP'ye bağlanın."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1cf96bbf8bd215d7ddb3c99860a5cb4e668be3c2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-smtp-connector"></a>SMTP Bağlayıcısı ile çalışmaya başlama
E-posta göndermek için SMTP'ye bağlanın.

Kullanılacak [tüm bağlayıcıların](apis-list.md), ilk mantıksal uygulama oluşturmanız gerekir. Tarafından başlayabiliriz [şimdi mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>SMTP Bağlan
Mantıksal uygulamanızı herhangi bir hizmet erişebilmeniz için önce ilk önce oluşturmanız gerekir bir *bağlantı* hizmet. A [bağlantı](connectors-overview.md) bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, SMTP bağlanmak için önce bir SMTP gerekir *bağlantı*. Bir bağlantı oluşturmak için normalde, bağlandığınız hizmete erişmek için kullandığınız kimlik bilgilerini girin. Bu nedenle, SMTP örnekte, bağlantı adı, SMTP sunucu adresi ve SMTP bağlantı oluşturmak için kullanıcı oturum açma bilgileri için kimlik bilgilerini girin.  

### <a name="create-a-connection-to-smtp"></a>SMTP bağlantı oluşturun.
> [!INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a>Bir SMTP tetikleyicisi kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Bu örnekte, SMTP kendi, tetikleyici olmadığından kullanacağız **bir nesne oluşturulduğunda Salesforce -** tetikleyici. Salesforce'ta yeni bir nesne oluşturulduğunda, bu tetikleyici etkinleştirir. Sağlayacak şekilde her bir yeni sağlama Salesforce içinde oluşturulur Bizim örneğimizde, bunu yaparız bir *e-posta Gönder* eylem oluşur oluşturulan yeni müşteri adayına ilişkin bir bildirim ile SMTP bağlayıcısı aracılığıyla.

1. Girin *salesforce* arama kutusuna logic apps tasarımcısında seçip **bir nesne oluşturulduğunda Salesforce -** tetikleyici.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. **Bir nesne oluşturulduğunda** denetim görüntülenir.
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. Seçin **nesne türü** seçip *neden* nesneleri listesinde. Bu adımda, her bir yeni sağlama Salesforce'ta oluşturulduğunda, mantıksal uygulamanızı uyarır tetikleyici oluşturmakta olduğunuz belirten.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. Tetikleyici oluşturuldu.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>SMTP eylemi kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Tetikleyici eklenen, yeni sağlama Salesforce'ta oluşturulduğunda gerçekleşir bir SMTP eylem eklemek için aşağıdaki adımları izleyin.

1. Seçin **+ yeni adım** yeni bir sağlama oluşturulduğunda, almak istediğiniz eylem eklemek için.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. Seçin **Eylem Ekle**. Bu açılır, herhangi bir işlem arayabileceğiniz arama kutusu yapmak istiyorsunuz.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. Girin *smtp* SMTP ilgili eylemler için aranacak.  
4. Seçin **SMTP - e-posta Gönder** ne zaman gerçekleştirilecek eylemi yeni sağlama oluşturulur. Eylem denetim bloğu açılır. Bunu daha önceden yapmadıysanız, smtp Tasarımcı bloğu içindeki bağlantı gerekir.  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. İstenen e-posta bilgilerinizi giriş **SMTP - e-posta Gönder** bloğu.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. İş akışınızı etkinleştirmek için çalışmanızı kaydedin.  

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/smtpconnector/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).