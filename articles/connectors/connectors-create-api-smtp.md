---
title: SMTP Bağlayıcısı Azure Logic Apps içinde | Microsoft Docs
description: Logic apps ile Azure uygulama hizmeti oluşturun. E-posta göndermek için SMTP bağlayın.
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 516110abc1786d99bc719d47d61475cdc2ebcc4b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296076"
---
# <a name="get-started-with-the-smtp-connector"></a>SMTP Bağlayıcısı ile çalışmaya başlama
E-posta göndermek için SMTP bağlayın.

Kullanılacak [tüm bağlayıcıların](apis-list.md), ilk mantıksal uygulama oluşturmanız gerekir. Tarafından başlayabiliriz [şimdi mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-smtp"></a>SMTP Bağlan
Mantıksal uygulamanızı herhangi bir hizmet erişebilmeniz için önce ilk önce oluşturmanız gerekir bir *bağlantı* hizmet. A [bağlantı](connectors-overview.md) bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, SMTP bağlanmak için önce bir SMTP gerekir *bağlantı*. Bir bağlantı oluşturmak için normalde, bağlandığınız hizmete erişmek için kullandığınız kimlik bilgilerini girin. Bu nedenle, SMTP örnekte, bağlantı adı, SMTP sunucu adresi ve SMTP bağlantı oluşturmak için kullanıcı oturum açma bilgileri için kimlik bilgilerini girin.  

### <a name="create-a-connection-to-smtp"></a>SMTP bağlantı oluşturun.
> [!INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a>Bir SMTP tetikleyicisi kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Bu örnekte, SMTP kendi tetikleyici sahip değil. Bu nedenle, kullanmak **bir nesne oluşturulduğunda Salesforce -** tetikleyici. Salesforce'ta yeni bir nesne oluşturulduğunda, bu tetikleyici etkinleştirir. Böylece her yeni bir sağlama Salesforce içinde oluşturulur bu örnek için onu ayarlanmış bir *e-posta Gönder* eylem oluşur oluşturulan yeni sağlama içeren bir bildirim SMTP Bağlayıcısı'nı kullanarak.

1. Girin *salesforce* arama kutusuna logic apps tasarımcısında seçip **bir nesne oluşturulduğunda Salesforce -** tetikleyici.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. **Bir nesne oluşturulduğunda** denetim görüntülenir.
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. Seçin **nesne türü** seçip *neden* nesneleri listesinde. Bu adımda, uygulamanızın yeni bir sağlama Salesforce'ta oluşturulduğunda bildiren bir tetikleyici oluşturuyorsunuz.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. Tetikleyici oluşturuldu.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>SMTP eylemi kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Tetikleyici eklenen, yeni sağlama Salesforce'ta oluşturulduğunda oluşan bir SMTP eylemi eklemek için aşağıdaki adımları kullanın.

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