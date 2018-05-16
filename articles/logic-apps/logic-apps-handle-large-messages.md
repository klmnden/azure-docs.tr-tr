---
title: Azure mantıksal uygulamaları büyük iletileri işlemek | Microsoft Docs
description: Logic apps içinde Öbekleme ile büyük ileti boyutları nasıl ele alınacağını öğrenin
services: logic-apps
documentationcenter: ''
author: shae-hurst
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.devlang: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.date: 4/27/2018
ms.author: shhurst
ms.openlocfilehash: a99fbdd7c9beb32f640d5ca623f8bcda3cb9aba4
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="handle-large-messages-with-chunking-in-logic-apps"></a>Logic Apps içinde Öbekleme ile büyük iletilerini işleme

İletilerini işlerken Logic Apps ileti içeriği en büyük boyutu sınırlar. Yardımcı azaltmak yükü bu sınırı oluşturulan depolamak ve büyük iletileri işleme. Bu boyuttan daha büyük iletileri işlemek için Logic Apps için *öbek* küçük iletilerine büyük boyutlu bir ileti. Böylece, belirli koşullar altında Logic Apps kullanarak büyük dosyaları hala aktarabilirsiniz. Logic Apps, bağlayıcılar veya HTTP üzerinden diğer hizmetlerle iletişim kurarken büyük iletileri tüketebileceği ancak *yalnızca* parçalar. Bu durum bağlayıcılar, ayrıca Öbekleme desteklemelidir veya Logic Apps ve bu hizmetleri arasındaki temel HTTP iletisi exchange Öbekleme kullanmalısınız anlamına gelir.

Bu makalede, sınırından daha büyük iletileri için destek Öbekleme yukarı nasıl ayarlayacağınızı gösterir.

## <a name="what-makes-messages-large"></a>İletilerin "büyük" ne yapar?

İletilerin bu iletileri işleme hizmetini temel alan "büyük". Büyük iletileri tam boyut sınırını Logic Apps ve bağlayıcıları arasında farklılık gösterir. Logic Apps ve bağlayıcıları doğrudan öbekli gerekir büyük iletileri kullanamayacaklarını. Logic Apps ileti boyutu sınırı için bkz: [Logic Apps sınırlarını ve yapılandırmasını](../logic-apps/logic-apps-limits-and-config.md).
Her bağlayıcı'nın ileti boyutu sınırı için bkz: [bağlayıcı belirli Teknik Ayrıntılar](../connectors/apis-list.md).

### <a name="chunked-message-handling-for-logic-apps"></a>Öbekli ileti Logic Apps için işleme

Logic Apps doğrudan iletisi boyut sınırını öbekli iletileri çıkışlarından kullanamazsınız. Öbekleme destekleyen Eylemler şu çıktıları ileti içeriği erişebilir. Bu nedenle, büyük iletileri işleyen eylem karşılamalıdır *ya da* bu ölçütler:

* Bu eylem bir bağlayıcıya ait olduğunda Öbekleme yerel olarak destekler. 
* Bu eylemin çalışma zamanı configuration etkin destek Öbekleme vardır. 

Aksi takdirde, büyük içerik çıkış erişmeyi denediğinde, bir çalışma zamanı hatası alırsınız. Öbekleme etkinleştirmek için bkz: [destek Öbekleme yukarı ayarlamak](#set-up-chunking).

### <a name="chunked-message-handling-for-connectors"></a>Öbekli ileti bağlayıcıları için işleme

Logic Apps ile iletişim hizmetleri kendi ileti boyutu sınırları olabilir. Bu sınırlar genellikle Logic Apps sınırdan daha küçük. Örneğin, bir bağlayıcı Öbekleme destekleyen varsayılarak, Logic Apps çalışmazken bir bağlayıcı 30 MB ileti büyük olarak düşünebilirsiniz. Bu bağlayıcı'nın sınırı ile uyum sağlamak için Logic Apps herhangi bir iletisi 30 MB'den daha büyük küçük parçalara ayırır.

Öbekleme destek bağlayıcıları için temel kümeleme Protokolü son kullanıcılara görünmez durumdadır. Ancak, gelen iletileri bağlayıcılar boyutu sınırları aştığında bu bağlayıcıların çalışma zamanı hataları oluşturmak için tüm bağlayıcılar Öbekleme, destekler.

<a name="set-up-chunking"></a>

## <a name="set-up-chunking-over-http"></a>HTTP üzerinden Öbekleme yukarı ayarlayın

Genel HTTP senaryolarda büyük içerik indirmeleri bölebilirsiniz ve böylece mantıksal uygulamanızı ve bir uç nokta büyük iletileri değiştirebilir HTTP üzerinden yükler. Ancak, Logic Apps bekliyor şekilde iletileri öbek gerekir. 

Bir uç nokta yüklemeleri veya yüklemeleri için Öbekleme etkinleştirmişse, uygulamanızın HTTP eylemleri otomatik olarak büyük iletileri öbek. Aksi takdirde, destek uç Öbekleme yukarı ayarlamanız gerekir. Sahibi değilseniz veya uç nokta veya bağlayıcı denetlemek, parçalama yukarı ayarlama seçeneği sahip olmayabilir.

Bir HTTP eylem zaten Öbekleme etkinleştirirseniz değil, ayrıca, ayrıca eylemin içinde Öbekleme yukarı ayarlamanız gerekir `runTimeConfiguration` özelliği. Eylem içinde bu özellik, daha sonra açıklandığı gibi doğrudan kod düzenleyicisinde görünümü ya da Logic Apps burada açıklandığı gibi Tasarımcısı'nda ayarlayabilirsiniz:

1. HTTP eylemin sağ üst köşedeki üç nokta düğmesini seçin (**...** ) ve ardından **ayarları**.

   ![Eylem, Ayarlar menüsünü açın](./media/logic-apps-handle-large-messages/http-settings.png)

2. Altında **içerik aktarımı**ayarlayın **izin Öbekleme** için **üzerinde**.

   ![Öbekleme üzerinde Aç](./media/logic-apps-handle-large-messages/set-up-chunking.png)

3. Yüklemeleri veya yüklemeleri için Öbekleme yukarı ayarı devam etmek için aşağıdaki bölümlere devam edin.

<a name="download-chunks"></a>

## <a name="download-content-in-chunks"></a>Öbek içeriği indirme

Çok sayıda uç noktaları büyük iletileri bir HTTP GET isteği aracılığıyla yüklendiğinde parçalar otomatik olarak gönder. Öbekli iletileri HTTP üzerinden bir uç noktasından indirmek için uç nokta kısmi içerik isteklerini desteklemesi gerekir veya *öbekli yüklemeleri*. Mantıksal uygulamanızı içerik indirmek için bir uç nokta bir HTTP GET isteği gönderir ve uç nokta "206" durum koduyla yanıt yanıt öbekli içeriği içerir. Logic Apps bir uç nokta kısmi istekleri destekleyip desteklemediğini denetleyemezsiniz. Ancak, mantıksal uygulamanızı "206" ilk yanıtı aldığında, mantıksal uygulamanızı otomatik olarak tüm içerik indirmek için birden çok istek gönderir.

Bir uç nokta kısmi içerik desteği olup olmadığını denetlemek için HEAD isteği gönderin. Bu istek, yanıt içerip içermediğini belirlemenize yardımcı olur `Accept-Ranges` üstbilgi. Bu şekilde, uç nokta öbekli yüklemeleri destekler ancak öbekli içerik göndermez yapabilecekleriniz *önermek* ayarlayarak bu seçenek `Range` HTTP GET isteği başlığı. 

Logic Apps mantıksal uygulamanızı bir uç noktasından öbekli içerik indirmek için kullandığı ayrıntılı işlem adımları açıklanmaktadır:

1. Mantıksal uygulamanızı uç noktasına bir HTTP GET isteği gönderir.

   İstek üstbilgisi isteğe bağlı olarak dahil edebileceğiniz bir `Range` içerik öbekleri isteyen bir bayt aralığı açıklar alan.

2. Uç nokta "206" durum kodu ve bir HTTP ileti gövdesini ile yanıt verir.

    Bu öbek içeriği ilgili ayrıntılar yanıtın içinde görünür `Content-Range` Logic Apps yardımcı olacak bilgileri de dahil olmak üzere, üst belirlemek başlangıç ve bitiş öbek yanı sıra, tüm içeriği toplam boyutu için Öbekleme önce.

3. Mantıksal uygulamanızı otomatik olarak izleme HTTP GET istekleri gönderir.

    Tüm içeriği alınana kadar mantıksal uygulamanızı izleme GET istekleri gönderir.

Örneğin, bu eylem tanımını ayarlar bir HTTP GET isteği gösterir `Range` üstbilgi. Üstbilgi *öneren* öbekli içerik uç nokta ile kullanmalıdır:

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

GET isteğini "Aralık" üstbilgisini ayarlar "bayt 0-1023 =", bir bayt aralığı olduğu. Uç nokta için kısmi içerik isteklerini destekliyorsa, uç nokta içerik bir Öbek ile istenen aralıktan yanıt verir. Uç noktada bağlı olarak, "Aralık" üstbilgi alan için tam biçim farklı olabilir.

<a name="upload-chunks"></a>

## <a name="upload-content-in-chunks"></a>Öbek içeriğinde karşıya yükle

HTTP eylemi öbekli içerik yüklemek için eylemin eylemin kümeleme desteğini etkinleştirilmiş `runtimeConfiguration` özelliği. Bu ayar, kümeleme Protokolü başlatmak için eylem verir. Mantıksal uygulamanızı sonra hedef uç noktasına ilk POST veya PUT ileti gönderebilir. Uç nokta önerilen öbek boyutu ile yanıt sonra mantıksal uygulamanızı içerik öbekleri içeren HTTP PATCH isteği göndererek izleyen.

Bu adımları Logic Apps mantıksal uygulamanızı öbekli içerik için bir bitiş noktasına yüklemek için kullandığı ayrıntılı işlem açıklanmaktadır:

1. Mantıksal uygulamanızı boş ileti gövdesi olan ilk bir HTTP POST veya PUT İsteği gönderir. İstek üstbilgisi mantıksal uygulamanızı yığınlar halinde karşıya istediği içerik hakkındaki bu bilgiler içerir:

   | Logic Apps üstbilgi alanı isteği | Değer | Tür | Açıklama |
   |---------------------------------|-------|------|-------------|
   | **x-ms-aktarım-modu** | öbekli | Dize | İçerik yığınlar halinde karşıya yüklendiğini gösterir |
   | **x-ms-content-length** | <*içerik uzunluğu*> | Tamsayı | Tüm içerik boyutu Öbekleme önce bayt |
   ||||

2. Uç nokta "200" başarı durum kodunu ve isteğe bağlı bu bilgilerle yanıt verir:

   | Uç nokta yanıt üstbilgi alanı | Tür | Gerekli | Açıklama |
   |--------------------------------|------|----------|-------------|
   | **x-ms-öbek-boyutu** | Tamsayı | Hayır | Bayt cinsinden önerilen öbek boyutu |
   | **Konum** | Dize | Hayır | URL konumu HTTP PATCH iletilerin gönderileceği yeri |
   ||||

3. Mantıksal uygulamanızı oluşturur ve bu bilgilerle her izleme iletileri - HTTP PATCH gönderir:

   * Bir içerik öbek temel alarak **x-ms-öbek-boyutu** veya tüm içerik toplamda kadar bazı dahili olarak hesaplanan boyutu **x-ms-content-length** sırayla yüklenir

   * Bu her düzeltme eki iletisinde gönderilen içerik öbek Üstbilgi ayrıntılarını:

     | Logic Apps üstbilgi alanı isteği | Değer | Tür | Açıklama |
     |---------------------------------|-------|------|-------------|
     | **Content-Range** | <*Aralık*> | Dize | Örneğin Bitiş değeri ve toplam içerik boyutu, başlangıç değeri de dahil olmak üzere geçerli bir içerik öbek bayt aralığı: "bayt 0-1023/10100 =" |
     | **İçerik türü** | <*içerik türü*> | Dize | Öbekli içerik türü |
     | **Content-Length** | <*içerik uzunluğu*> | Dize | Bayt cinsinden geçerli öbek boyutu uzunluğu |
     |||||

4. Her PATCH isteği sonra "200" durum koduyla yanıt vererek her öbek giriş uç noktası onaylar.

Örneğin, bu eylem tanımı için bir uç nokta öbekli içerik yüklemek için bir HTTP POST isteği gösterir. Eylemin içinde `runTimeConfiguration` özelliği, `contentTransfer` özellik kümeleri `transferMode` için `chunked`:

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