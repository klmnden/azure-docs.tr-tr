---
title: Dropbox - Azure Logic Apps'ı bağlama | Microsoft Docs
description: Karşıya yükleme ve Dropbox REST API'lerini ve Azure Logic Apps ile dosyaları yönetme
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
ms.openlocfilehash: 256a0b34d5050e17abe5bb98ca0c13ab0b61787e
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094447"
---
# <a name="get-started-with-the-dropbox-connector"></a>Dropbox Bağlayıcısı ile çalışmaya başlama
Dosyalarınızı yönetmek için Dropbox'a bağlanın. Karşıya yükleme gibi çeşitli eylemler gerçekleştirebilirsiniz, güncelleştirme, alma ve dropbox'ta dosyaları silin.

Kullanılacak [bağlayıcıyı](apis-list.md), ilk mantıksal uygulamanızı oluşturmak gerekir. Tarafından oluşturabileceğinize dair [artık bir mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-dropbox"></a>Dropbox'a bağlanın
Mantıksal uygulamanızı herhangi bir hizmete erişebilmeniz için önce ilk oluşturmak için ihtiyacınız bir *bağlantı* hizmeti. Bir bağlantı, bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, Dropbox'a bağlanın, öncelikle bir Dropbox isteyen *bağlantı*. Bir bağlantı oluşturmak için normalde bağlanmak istediğiniz hizmete erişmek için kullandığınız kimlik bilgilerini sağlamanız gerekir. Bu nedenle, Dropbox örnekte, kimlik bilgilerini Dropbox hesabınıza Dropbox bağlantısı oluşturmak için ihtiyacınız olacaktı. 

### <a name="create-a-connection-to-dropbox"></a>Dropbox bağlantı oluşturun
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Bir Dropbox tetikleyicisini kullanma
Bir tetikleyici bir mantıksal uygulamada tanımlanan iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Bu örnekte, kullanacağız **bir dosya oluşturulduğunda** tetikleyici. Bu tetikleyiciyi ortaya çıktığında, biz göndereceği **yolunu kullanarak dosya içeriğini Al** Dropbox eylemini. 

1. Girin *dropbox* Logic Apps Tasarımcısı'nda arama kutusuna seçip **Dropbox - bir dosya oluşturulduğunda** tetikleyici.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Dosya oluşturma izlemek istediğiniz klasörü seçin. Seçin... (kırmızı kutu içinde tanımlanan) ve tetikleyici giriş için seçmek istediğiniz klasöre göz atın.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Bir Dropbox eylemini kullanma
Bir eylem, bir mantıksal uygulamada tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Tetikleyici eklendi artık, yeni dosyanın içeriği alacak bir eylem eklemek için bu adımları izleyin.

1. Seçin **+ yeni adım** yeni bir dosya oluşturulduğunda almak istediğiniz eylem ekleme.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Seçin **Eylem Ekle**. Bu açılır, herhangi bir işlem arayabileceğiniz arama kutusuna yapmak istiyorsunuz.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Girin *dropbox* Dropbox'a ilgili eylemler için aranacak.  
4. Seçin **Dropbox - dosya alma içeriği yolu kullanarak** ne zaman gerçekleştirilecek eylemi, seçili Dropbox klasörüne yeni bir dosya oluşturulur. Eylem denetim bloğu açılır. Mantıksal uygulamanızı, bunu daha önce yapmadıysanız, Dropbox hesabına erişmesi için yetki istenecektir.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Seçin... (sağ tarafında bulunan **dosya yolu** denetimi) ve kullanmak istediğiniz dosya yoluna göz atın. Ya da kullanmak **dosya yolu** logic app oluşturma işleminiz hızlandırmak için belirteç.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Çalışmanızı kaydedin ve iş akışınızı etkinleştirin Dropbox'ta yeni bir dosya oluşturun.  

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/dropbox/).

## <a name="more-connectors"></a>Daha fazla bağlayıcı
Geri Git [API listesi](apis-list.md).
