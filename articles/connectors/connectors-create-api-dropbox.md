---
title: Dropbox - Azure mantıksal uygulamaları bağlama | Microsoft Docs
description: Karşıya yükleme ve Dropbox REST API'leri ve Azure Logic Apps ile dosyalarını yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 07/15/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 77203788a6329ed4c5b58419fbcf48a48da91b30
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295056"
---
# <a name="get-started-with-the-dropbox-connector"></a>Dropbox Bağlayıcısı ile çalışmaya başlama
Dosyalarınızı yönetmek için Dropbox'a bağlanın. Karşıya yükleme gibi çeşitli eylemleri, güncelleştirme, almak ve Dropbox dosyaları silin.

Kullanılacak [tüm bağlayıcıların](apis-list.md), ilk mantıksal uygulama oluşturmanız gerekir. Tarafından başlayabiliriz [şimdi mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-dropbox"></a>Dropbox'a bağlanın
Mantıksal uygulamanızı herhangi bir hizmet erişebilmeniz için önce ilk önce oluşturmanız gerekir bir *bağlantı* hizmet. Bağlantı bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, Dropbox'a bağlanmak için önce bir Dropbox gerekir *bağlantı*. Bir bağlantı oluşturmak için normalde bağlanmak istediğiniz hizmete erişmek için kullandığınız kimlik bilgilerini sağlamanız gerekir. Bu nedenle, Dropbox örnekte, kimlik bilgileri Dropbox hesabınıza Dropbox bağlantı oluşturmak için gerekir. [Bağlantılar hakkında daha fazla bilgi edinin]()

### <a name="create-a-connection-to-dropbox"></a>Dropbox bağlantı oluşturun.
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Bir Dropbox tetikleyici kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Bu örnekte, kullanacağız **bir dosya oluşturulduğunda** tetikleyici. Bu tetikleyici ortaya çıktığında, biz çağıracak **alma yolunu kullanarak dosya içeriğini** Dropbox eylem. 

1. Girin *dropbox* Logic Apps tasarımcısında arama kutusuna, ardından **bir dosya oluşturulduğunda, Dropbox -** tetikleyici.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Dosya oluşturma izlemek istediğiniz klasörü seçin. Seçin... (kırmızı kutu içinde tanımlanan) ve tetikleyici giriş için seçmek istediğiniz klasöre göz atın.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Bir Dropbox eylemi kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Tetikleyici eklenen, yeni dosyanın içeriğini alacak bir eylem eklemek için aşağıdaki adımları izleyin.

1. Seçin **+ yeni adım** yeni bir dosya oluşturulduğunda, almak istediğiniz eylem eklemek için.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Seçin **Eylem Ekle**. Bu açılır, herhangi bir işlem arayabileceğiniz arama kutusu yapmak istiyorsunuz.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Girin *dropbox* Dropbox'a ilgili eylemler için aranacak.  
4. Seçin **Dropbox - yolunu kullanarak dosya alma içeriği** ne zaman gerçekleştirilecek eylemi olarak seçilen Dropbox klasöründe yeni bir dosya oluşturulur. Eylem denetim bloğu açılır. Mantıksal uygulamanızı, bunu daha önceden yapmadıysanız, Dropbox hesabınıza erişmeniz için yetkilendirmek için istenir.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Seçin... (sağ tarafında bulunan **dosya yolu** denetimi) ve kullanmak istediğiniz dosya yoluna göz atın. Veya kullanmak **dosya yolu** logic app oluşturma hızlandırmak için belirteç.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Çalışmanızı kaydedin ve iş akışını etkinleştirmek için Dropbox'yeni bir dosya oluşturun.  

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/dropbox/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).