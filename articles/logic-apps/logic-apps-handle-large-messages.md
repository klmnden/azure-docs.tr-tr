---
title: Büyük iletileri - Azure Logic Apps işleme | Microsoft Docs
description: Büyük ileti boyutları, Azure Logic Apps'te Öbekleme ile nasıl ele alınacağını öğrenin
services: logic-apps
documentationcenter: ''
author: shae-hurst
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.devlang: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.date: 4/27/2018
ms.author: shhurst
ms.openlocfilehash: 5aa5ea2a39a0fb9f969e965fed14063522197cda
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60303800"
---
# <a name="handle-large-messages-with-chunking-in-azure-logic-apps"></a>Azure Logic Apps'te Öbekleme ile büyük iletileri işleme

İletilerini işlerken, Logic Apps ileti içeriği için en büyük boyutu sınırlar. Yükü azaltmaya yardımcı olur, bu sınır depolamak ve büyük iletileri işleme oluşturuldu. Bu sınırdan büyük iletileri işlemek için Logic Apps için *öbek* daha küçük iletilere büyük bir ileti. Bu şekilde, belirli koşullar altında Logic Apps kullanarak büyük dosyaları yine de aktarabilirsiniz. Logic Apps bağlayıcıları veya HTTP üzerinden diğer hizmetleriyle iletişim kurarken, büyük iletiler kullanabilir ancak *yalnızca* öbekler halinde. Bu durum, bağlayıcılar Öbekleme da desteklemesi gerekir veya Logic Apps ve bu hizmetler arasındaki temel alınan HTTP ileti değişimi Öbekleme kullanmalısınız anlamına gelir.

Bu makalede, sınırdan büyük iletileri işleme eylemleri için Öbekleme nasıl ayarlayacağınızı gösterir. Mantıksal uygulama tetikleyicileri, artan nedeniyle Öbekleme desteklemez, birden çok ileti alışverişi ek yükü. 

## <a name="what-makes-messages-large"></a>İletileri "large" ne yapar?

Bu iletileri işleme hizmetini temel alan iletileri "large". Büyük iletilerin tam boyut sınırını Logic Apps ve bağlayıcıları arasında farklılık gösterir. Logic Apps ve bağlayıcıları hem doğrudan öbekli gerekir büyük iletiler kullanamıyor. Logic Apps ileti boyutu sınırı'için bkz: [Logic Apps limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md).
Her bağlayıcı'nın ileti boyutu sınırı için bkz: [bağlayıcının belirli Teknik Ayrıntılar](../connectors/apis-list.md).

### <a name="chunked-message-handling-for-logic-apps"></a>Öbekli ileti Logic Apps için işleme

Logic Apps iletisi boyut sınırını öbekli iletileri çıkışları doğrudan kullanamazsınız. Öbekleme destekleyen Eylemler bu çıkışları ileti içeriği erişebilirsiniz. Bu nedenle, büyük iletileri işleyen bir eylem karşılamalıdır *ya da* bu ölçütü:

* Bu eylem bir bağlayıcıya ait olduğunda Öbekleme yerel olarak destekler. 
* Bu eylemin runtime yapılandırmasında etkin destek Öbekleme vardır. 

Aksi takdirde, büyük içerik çıktısı erişmeye çalışırken, bir çalışma zamanı hatası alırsınız. Öbekleme etkinleştirmek için bkz: [destek Öbekleme yedekleme kümesi](#set-up-chunking).

### <a name="chunked-message-handling-for-connectors"></a>Öbekli ileti bağlayıcıları için işleme

Logic Apps ile iletişim kuran hizmeti, kendi ileti boyutu sınırlarını olabilir. Bu sınırlar, çoğunlukla Logic Apps sınırdan daha küçük olur. Örneğin, parçalama bir bağlayıcıyı destekler varsayarak değilken Logic Apps bağlayıcı 30 MB'lık ileti büyük olarak düşünebilirsiniz. Bu bağlayıcının sınırına uymak için Logic Apps herhangi bir ileti 30 MB değerinden daha büyük küçük öbeklere böler.

Öbekleme desteklemek için bağlayıcılar, temel alınan kümeleme Protokolü son kullanıcılara görünmezdir. Ancak, gelen iletileri bağlayıcıları boyut sınırını aşması durumunda, bu bağlayıcıları çalışma zamanı hataları oluşturmak için tüm bağlayıcılar parçalama, destekler.

<a name="set-up-chunking"></a>

## <a name="set-up-chunking-over-http"></a>HTTP üzerinden Öbekleme yukarı ayarlayın

Genel HTTP senaryolarda büyük içerik yüklemeleri bölebilirsiniz ve böylece büyük iletiler gönderip alabilir, mantıksal uygulamanız ve bir uç nokta HTTP üzerinden yükler. Ancak, iletileri Logic Apps bekliyor bir şekilde öbek gerekir. 

Bir uç nokta indirme veya karşıya yüklemeleri için Öbekleme etkinleştirilmişse, mantıksal uygulamanızın HTTP eylemleri otomatik olarak büyük iletiler öbek. Aksi takdirde, uç noktasında destek Öbekleme yukarı ayarlamanız gerekir. Öbekleme ayarlama seçeneğini görmüyorsanız veya kendi uç nokta veya bağlayıcı denetim olmayabilir.

HTTP eylem zaten Öbekleme etkinleştir değil, ayrıca, eylemin içinde Öbekleme yukarı ayarlamanız gerekir `runTimeConfiguration` özelliği. Eylem içinde bu özellik, daha sonra açıklandığı gibi doğrudan kod düzenleyicisinde görüntüle veya burada açıklandığı gibi Logic Apps Tasarımcısı'nda ayarlayabilirsiniz:

1. HTTP eylem sağ üst köşedeki üç nokta düğmesini ( **...** ) ve ardından **ayarları**.

   ![Eylem, tıklayarak ayarlar menüsünü açın.](./media/logic-apps-handle-large-messages/http-settings.png)

2. Altında **içerik aktarımı**ayarlayın **izin Öbekleme** için **üzerinde**.

   ![Öbekleme üzerinde Aç](./media/logic-apps-handle-large-messages/set-up-chunking.png)

3. İndirme veya karşıya yüklemeleri için Öbekleme yedekleme ayarı devam etmek için aşağıdaki bölümleri ile devam edin.

<a name="download-chunks"></a>

## <a name="download-content-in-chunks"></a>Parçalar halinde içeriği indir

Çok sayıda uç noktaları, bir HTTP GET isteği indirildiğinde öbekler halinde otomatik olarak büyük iletileri gönderir. Öbekli iletileri HTTP üzerinden bir uç noktasından indirmek için uç nokta kısmi içerik isteklerini desteklemesi gerekir veya *öbekli indirmeler*. Mantıksal uygulamanız içerik indirmek için bir uç nokta için bir HTTP GET isteği gönderir ve uç nokta "206" durum koduyla yanıt, yanıt öbekli içeriğe sahip. Logic Apps, bir uç nokta kısmi istekleri destekleyip desteklemediğini kontrol edemezsiniz. Ancak, mantıksal uygulamanızı ilk "206" yanıtı alır, mantıksal uygulamanızı otomatik olarak tüm içerik indirmek için birden çok istek gönderir.

Bir uç nokta kısmi içerik destekleyip desteklemediğini kontrol etmek için HEAD isteği gönderin. Bu istek, yanıt içerip içermediğini anlamak yardımcı olur `Accept-Ranges` başlığı. Bu şekilde, uç nokta öbekli yüklemeleri destekler ancak öbekli içerik göndermez, şunları yapabilirsiniz *Öner* ayarlayarak bu seçeneği `Range` , HTTP GET isteği başlığı. 

Bu adımlar, mantıksal uygulamanız için bir uç noktasından öbekli içeriği yüklemek için Logic Apps kullanan ayrıntılı işlemi açıklanmaktadır:

1. Mantıksal uygulamanızı uç noktasına bir HTTP GET isteği gönderir.

   İstek üst bilgisi isteğe bağlı olarak dahil edebilirsiniz bir `Range` içerik öbekleri istemeye ilişkin bir bayt aralığı tanımlayan alan.

2. Uç nokta "206" durum kodu ve bir HTTP ileti gövdesi ile yanıt verir.

    Bu öbekteki içerik hakkındaki ayrıntıları yanıtın içinde görünür `Content-Range` Logic Apps yardımcı olacak bilgileri dahil olmak üzere, üstbilgi belirlemek başlangıç ve bitiş öbek yanı sıra, tüm içeriği toplam boyutu için Öbekleme önce.

3. Mantıksal uygulamanızı izleme HTTP GET isteklerini otomatik olarak gönderir.

    Tüm içeriği alınana kadar mantıksal uygulamanızı izleme GET istekleri gönderir.

Örneğin, bu eylem tanımı ayarlayan bir HTTP GET isteği gösterir `Range` başlığı. Üst bilgi *önerir* öbekli içerik uç noktası ile yanıt vermelidir:

```json
"getAction": {
    "inputs": {
        "headers": {
            "Range": "bytes=0-1023"
        },
       "method": "GET",
       "uri": "http://myAPIendpoint/api/downloadContent"
    },
    "runAfter": {},
    "type": "Http"
}
```

GET isteği ayarlar "Aralık" üst "bayt 0 1023 =", bayt aralığı olduğu. Uç nokta için kısmi içerik isteklerini destekler, uç nokta istenen aralıktan içerik bir Öbek ile yanıt verir. Uç noktasına göre tam biçim "Aralık" üstbilgi alanı için farklı olabilir.

<a name="upload-chunks"></a>

## <a name="upload-content-in-chunks"></a>Parçalar halinde içeriği karşıya yükleme

Öbekli HTTP eylem içeriği karşıya yüklemek için eylem eylemin kümeleme desteğini etkinleştirmiş olmanız gerekir `runtimeConfiguration` özelliği. Bu ayar, kümeleme Protokolü eylemini verir. Mantıksal uygulamanızı, ardından hedef uç noktaya ilk POST veya PUT ileti gönderebilir. Önerilen öbek boyutu ile uç nokta yanıt sonra mantıksal uygulamanızın içerik öbekleri içeren HTTP PATCH isteği göndererek izleyen.

Bu adımları bir uç noktaya mantıksal uygulamanızdan öbekli içerik yüklemek için Logic Apps kullanan ayrıntılı işlemi açıklanmaktadır:

1. Mantıksal uygulamanız boş bir ileti gövdesi ile ilk bir HTTP POST veya PUT İsteği gönderir. İstek üst bilgisi bu öbekler halinde karşıya yüklemek için mantıksal uygulamanızı isteyen içerikle ilgili bilgiler içerir:

   | Logic Apps üstbilgi alanı istek | Değer | Tür | Açıklama |
   |---------------------------------|-------|------|-------------|
   | **x-ms-transfer-mode** | öbekli | String | İçerik öbekler halinde karşıya yüklendiğini belirtir. |
   | **x-ms-content-length** | <*içerik uzunluğu*> | Integer | Bayt Öbekleme önce tüm içerik boyutu |
   ||||

2. Uç nokta, "200" başarılı durum kodu ve isteğe bağlı bu bilgilerle yanıt verir:

   | Uç nokta yanıt üstbilgi alanı | Tür | Gerekli | Açıklama |
   |--------------------------------|------|----------|-------------|
   | **x-ms-öbek boyutu** | Integer | Hayır | Bayt cinsinden önerilen öbek boyutu |
   | **Location** | String | Hayır | URL konumu HTTP PATCH iletilerin gönderileceği adresi |
   ||||

3. Mantıksal uygulamanızı oluşturur ve bu bilgileri her izleme iletileri - HTTP PATCH gönderir:

   * İçerik öbek temel alarak **x-ms-öbek boyutu** veya tüm içerik toplam kadar bazı dahili olarak hesaplanan boyutu **x-ms-content-length** sıralı olarak yüklenir

   * Her düzeltme iletisiyle gönderilen içerik öbek ilgili bu üst bilgisi ayrıntıları:

     | Logic Apps üstbilgi alanı istek | Değer | Tür | Açıklama |
     |---------------------------------|-------|------|-------------|
     | **Content-Range** | <*Aralığı*> | String | Bitiş değeri ve toplam içerik boyutu, örneğin başlangıç değeri de dahil olmak üzere geçerli bir içerik öbek için bayt aralığı: "bayt 0-1023/10100 =" |
     | **Content-Type** | <*içerik türü*> | String | Öbekli içerik türü |
     | **İçerik uzunluğu** | <*içerik uzunluğu*> | String | Bayt cinsinden geçerli öbek boyutu uzunluğu |
     |||||

4. Her bir PATCH isteği sonra "200" durum kodu ile yanıt vererek, her öbek için giriş uç noktası onaylar.

Örneğin, bir uç noktaya öbekli içeriği karşıya yükleme için bir HTTP POST isteği bu eylemi tanımı gösterilmektedir. Eylemin içinde `runTimeConfiguration` özelliği `contentTransfer` özellik kümeleri `transferMode` için `chunked`:

```json
"postAction": {
    "runtimeConfiguration": {
        "contentTransfer": {
            "transferMode": "chunked"
        }
    },
    "inputs": {
        "method": "POST",
        "uri": "http://myAPIendpoint/api/action",
        "body": "@body('getAction')"
    },
    "runAfter": {
    "getAction": ["Succeeded"]
    },
    "type": "Http"
}
```