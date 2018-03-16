---
title: "Logic apps içinde Twitter Bağlayıcısı'nı kullanmayı öğrenin | Microsoft Docs"
description: "Twitter Bağlayıcısı REST API parametrelerle genel bakış"
services: 
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: eb953ee7701d407b9b75a0699f53b9b64828a0e5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-twitter-connector"></a>Twitter Bağlayıcısı ile çalışmaya başlama
Twitter Bağlayıcısı ile şunları yapabilirsiniz:

* Tweetler ve get tweet'leri
* Erişim zaman çizelgeleri, arkadaşlarınıza ve followers
* Diğer Eylemler ve bu makalede açıklanan Tetikleyicileri gerçekleştirin

Kullanılacak [tüm bağlayıcıların](apis-list.md), ilk mantıksal uygulama oluşturmanız gerekir. Tarafından başlayabiliriz [şimdi mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).  

## <a name="connect-to-twitter"></a>Twitter’a Bağlanma
Mantıksal uygulamanızı herhangi bir hizmet erişebilmeniz için önce ilk önce oluşturmanız gerekir bir *bağlantı* hizmet. A [bağlantı](connectors-overview.md) bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar.  

### <a name="create-a-connection-to-twitter"></a>Twitter bağlantı oluşturun.
> [!INCLUDE [Steps to create a connection to Twitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a>Bir Twitter tetikleyici kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Bu örnekte, kullandığınız **yeni tweet zaman nakledilir** için #Seattle aramak için tetikleyici. Ve #Seattle bulunursa, Dropbox dosyasında tweet metinden ile güncelleştirin. Bir kurumsal örnekte, şirketinizin adı aramak ve tweet metinden ile bir SQL veritabanı güncelleştirmesi.

1. Girin *twitter* arama kutusuna logic apps tasarımcısında seçip **yeni tweet gönderildiğinde Twitter -** tetikleyici   
   ![Twitter tetikleyici görüntü 1](./media/connectors-create-api-twitter/trigger-1.png)  
2. Girin *#Seattle* içinde **arama metni** denetimi  
   ![Twitter tetikleyici görüntü 2](./media/connectors-create-api-twitter/trigger-2.png) 

Bu noktada, mantıksal uygulamanızı diğer tetikleyiciler ve Eylemler iş akışı bir farklı çalıştır başlayacak bir tetikleyici ile yapılandırıldı. 

> [!NOTE]
> Bir mantıksal uygulama işlevsel olması en az bir tetikleyici ve bir eylem içermelidir. Bir eylem eklemek için sonraki bölümde adımları kullanın.

## <a name="add-a-condition"></a>Koşul Ekle
Biz yalnızca 50'den fazla kullanıcısı olan kullanıcılardan tweet'leri ilgilendiğiniz. Bu nedenle, followers sayısı onaylar bir koşulu, mantıksal uygulama ilk eklenir.  

1. Seçin **+ yeni adım** #Seattle içinde yeni bir tweet bulunduğunda almak istediğiniz eylem eklemek için  
   ![Twitter eylem görüntü 1](../../includes/media/connectors-create-api-twitter/action-1.png)  
2. Seçin **bir koşul eklemek** bağlantı.  
   ![Twitter koşul görüntü 1](../../includes/media/connectors-create-api-twitter/condition-1.png)   
   Açılır **koşulu** burada kontrol edebilirsiniz koşulları gibi denetim *eşittir*, *olan değerinden*, *değerinden daha büyük*, *içeren*, vb.  
   ![Twitter koşul görüntü 2](../../includes/media/connectors-create-api-twitter/condition-2.png)   
3. Seçin **bir değer seçin** denetim. Bu denetimde herhangi bir önceki eylem veya Tetikleyiciler bir veya daha fazla özellik seçebilirsiniz. Bu özellik değeri koşul doğru veya yanlış olarak değerlendirilir.
   ![Twitter koşul görüntü 3](../../includes/media/connectors-create-api-twitter/condition-3.png)   
4. Seçin **...** kullanılabilir olan tüm özellikler görebilmeniz için özellikler listesini genişletin.        
   ![Twitter koşul görüntü 4](../../includes/media/connectors-create-api-twitter/condition-4.png)   
5. Seçin **Followers sayısı** özelliği.    
   ![Twitter koşul görüntü 5](../../includes/media/connectors-create-api-twitter/condition-5.png)   
6. Followers sayısı özelliği şimdi değer denetiminde olduğuna dikkat edin.    
   ![Twitter koşul görüntü 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. Seçin **değerinden daha büyük** işleçleri listesinden.    
   ![Twitter koşul görüntü 7](../../includes/media/connectors-create-api-twitter/condition-7.png)   
8. İşleneni olarak 50 girin *değerinden daha büyük* işleci.  
   Koşul artık eklenir. İş kullanarak Kaydet **kaydetmek** menüsündeki bağlantısı.    
   ![Twitter koşul görüntü 8](../../includes/media/connectors-create-api-twitter/condition-8.png)   

## <a name="use-a-twitter-action"></a>Bir Twitter eylemi kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

Yoktur göre bir tetikleyici, tetikleyici tarafından bulunan tweet'leri içeriğini yeni bir tweet yazılarını bir eylem ekleyin. Bu kılavuz için yalnızca 50'den fazla followers sahip kullanıcılardan tweet'leri nakledilir.  

Sonraki adımda, 50'den fazla followers sahip bir kullanıcı tarafından gönderilen her tweet özelliklerini kullanarak tweet yazılarını bir Twitter eylem ekleyin.  

1. Seçin **Eylem Ekle**. Bu adım, diğer eylemler ve Tetikleyiciler için arayabileceğiniz arama denetimini açar.  
   ![Twitter koşul görüntü 9](../../includes/media/connectors-create-api-twitter/condition-9.png)   
2. Girin *twitter* arama kutusuna seçip **Twitter - tweet sonrası** eylem. Bu adımı açılır **tweet sonrası** gönderilmesini tweet için tüm ayrıntıları gireceğiniz denetim.      
   ![Eylem görüntü 1-5 twitter](../../includes/media/connectors-create-api-twitter/action-1-5.png)   
3. Seçin **Tweet metin** denetim. Tüm çıktıları önceki Eylemler ve mantıksal uygulama Tetikleyicileri görünür. Bu çıktılar birini seçin ve yeni tweet tweet metninin bir parçası olarak kullanın.     
   ![Twitter eylem görüntü 2](../../includes/media/connectors-create-api-twitter/action-2.png)   
4. Seçin **kullanıcı adı**   
5. Kullanıcı adı sonra hemen girin *diyor:* tweet metin denetimi.
6. Seçin *Tweet metin*.       
   ![Twitter eylem görüntüsü 3](../../includes/media/connectors-create-api-twitter/action-3.png)   
7. İş akışınızı etkinleştirmek için çalışmanızı kaydedin ve #Seattle diyez ile tweet gönderecek.


## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/twitterconnector/). 

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)