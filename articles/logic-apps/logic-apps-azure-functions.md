---
title: "Azure Logic Apps ile Azure işlevleri için özel kod | Microsoft Docs"
description: "Oluşturma ve Azure Logic Apps ile Azure işlevleri için özel kod çalıştırma"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 18442c87b049200fac5ed41cc7034ba7a848b8d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a>Ekleme ve logic apps için özel kod aracılığıyla Azure işlevleri çalıştırın

C# veya node.js özel parçacıkları logic apps içinde çalıştırmak için Azure işlevleri aracılığıyla özel işlevler oluşturabilirsiniz. 
[Azure işlevleri](../azure-functions/functions-overview.md) server ücretsiz Microsoft Azure ve bu görevleri gerçekleştirmek için yararlı olan bilgi işlem sunar:

* Gelişmiş Biçimlendirme veya logic apps alanların işlem
* Bir iş akışında hesaplamalar.
* C# veya node.js desteklenen işlevlere sahip mantığı uygulama işlevini genişletme

## <a name="create-custom-functions-for-your-logic-apps"></a>Logic apps için özel işlevler oluştur

Gelen bir işlev Azure işlevleri portalındaki oluşturmanızı öneririz **Genel Web kancası - düğüm** veya **Genel Web kancası - C#** şablonları. Sonuç bir otomatik doldurulmuş kabul eden bir şablon oluşturur `application/json` mantığı uygulamasından. Bu şablondan Oluştur işlevleri otomatik olarak algılanır ve altında mantığı Uygulama Tasarımcısı'nda görünen **my bölgede Azure işlevleri.**

Azure portalında üzerinde **tümleştir** bölmesini işlevinizi için şablonunuzu göstermek, **modu** kümesine **Web kancası** ve **Web kancası türü** ayarlanır **genel JSON**. 

Web kancası işlevleri bir isteğini kabul edin ve yöntemiyle uygulamasına geçirmek bir `data` değişkeni. Gibi noktalı gösterim kullanılarak yükünüzü özelliklerini erişebilirsiniz `data.function-name`. Örneğin, bir tarih saat değeri bir tarih dizeye dönüştürür basit bir JavaScript işlevi aşağıdaki gibi görünür:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a>Mantığı uygulamalardan Azure işlevlerini çağırma

Aboneliğinizi kapsayıcılarında listelemek ve, mantığı Uygulama Tasarımcısı'nda, aramak istediğiniz işlevi seçmek için tıklatın **Eylemler** menü ve arasından seçim **Azure işlevleri my bölgede**.

İşlev seçtikten sonra bir giriş yükü nesne belirtmeniz istenir. İleti mantıksal uygulama işlevine gönderir ve bir JSON nesnesi olmalıdır nesnesidir. Örneğin, içinde geçirmek istiyorsanız **son değişiklik** tarih Salesforce tetikleyiciden işlevi yükü bu örnekteki gibi görünmelidir:

![Son değiştirilme tarihi][1]

## <a name="trigger-logic-apps-from-a-function"></a>Bir işlevden tetikleyici mantıksal uygulamalar

Bir işlev içinde bir mantıksal uygulama tetikleyebilir. Bkz: [Logic apps aranabilir uç noktalar olarak](logic-apps-http-endpoint.md). El ile tetikleyicisine sahip bir mantıksal uygulama oluşturun, sonra mantıksal uygulama URL'si istediğiniz yükü ile gönderilen el ile tetikleyici için bir HTTP POST işlevinizin oluşturmak.

### <a name="create-a-function-from-logic-app-designer"></a>Mantıksal Uygulama Tasarımcısı'ndan bir işlev oluşturun

Tasarımcısı'ndan bir node.js Web kancası işlevi de oluşturabilirsiniz. İlk olarak seçin **Azure işlevleri my bölgede** ve işlevinizi için bir kapsayıcı seçin. Henüz bir kapsayıcınız yoksa, birinden oluşturmanıza gerek [Azure işlevleri portalına](https://functions.azure.com/signin). Ardından **Yeni Oluştur**.  

İşlem yapmak istediğiniz verileri temel alan bir şablon oluşturmak için bir işlevdeki geçirmek için planlama context nesnesi belirtin. Bu nesne bir JSON nesnesi olmalıdır. Örneğin, bir FTP eylemden dosya içeriğinde geçirirseniz, bağlam yükü aşağıdaki gibi görünür:

![Bağlam yükü][2]

> [!NOTE]
> Bu nesne bir dize olarak cast değildi olduğundan, içeriği doğrudan JSON yükü eklenir. Ancak, nesne (diğer bir deyişle, bir dize veya bir JSON nesnesi/dizi) bir JSON belirteci değilse, bir hata oluşur. Dize olarak nesnesi yayınlanamıyor teklifleri bu makalede ilk çizimde gösterildiği gibi ekleyin.
> 

Tasarımcı sonra satır içi oluşturabileceğiniz bir işlevi şablon oluşturur. Değişkenleri işlevdeki geçirmek için planlama bağlam temel alınarak önceden oluşturulur.

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
