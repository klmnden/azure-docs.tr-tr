---
title: İstek ve yanıt eylemleri kullanın | Microsoft Docs
description: İstek ve yanıt tetikleyici ve eylem, bir Azure mantıksal uygulaması'na genel bakış
services: ''
documentationcenter: ''
author: jeffhollan
manager: erikre
editor: ''
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 0f6ee8729cbed9cb8baf3668f7b1a332bc5eddc1
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58892838"
---
# <a name="get-started-with-the-request-and-response-components"></a>İstek ve yanıt bileşenleri ile çalışmaya başlama
Mantıksal uygulama içinde istek ve yanıt bileşenleriyle, olaylar için gerçek zamanlı olarak yanıt verebilirsiniz.

Örneğin, şunları yapabilirsiniz:

* Mantıksal uygulama üzerinden şirket içi veritabanından verileri olan bir HTTP isteği yanıt.
* Bir mantıksal uygulamadan bir dış Web kancası olayı tetikleyin.
* İstek ve yanıt eylemi içinde başka bir mantıksal uygulama ile bir mantıksal uygulamayı çağırın.

İstek ve yanıt eylemleri bir mantıksal uygulama çalıştırmasında kullanmaya başlamak için bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-http-request-trigger"></a>HTTP isteği tetikleyicisini kullanma
Bir tetikleyici bir mantıksal uygulamada tanımlanan iş akışını başlatmak için kullanılan bir olaydır. 
[Tetikleyiciler hakkında daha fazla bilgi](../connectors/apis-list.md).

Mantıksal Uygulama Tasarımcısı'nda bir HTTP isteği ayarlamak nasıl bir örnek dizisi aşağıda verilmiştir.

1. Tetikleyici ekleme **isteği - zaman bir HTTP isteği alındığında** mantıksal uygulamanızda. İsteğe bağlı olarak bir JSON şeması sağlayın (gibi bir araç kullanarak [JSONSchema.net](https://jsonschema.net)) için istek gövdesi. Bu HTTP isteğinde özellikleri için belirteçleri oluşturmak tasarımcı sağlar.
2. Mantıksal uygulama kaydedebilir, böylece başka bir eylem ekleme.
3. Mantıksal uygulamayı kaydettikten sonra istek kartından HTTP istek URL'sini alabilirsiniz.
4. Bir HTTP POST (gibi bir araç kullanabilirsiniz [Postman](https://www.getpostman.com/)) mantıksal uygulama URL'sine tetikler.

> [!NOTE]
> Bir yanıt eylemi tanımlamazsanız bir `202 ACCEPTED` yanıt hemen çağırana döndürülür. Yanıt eylemi, bir yanıt özelleştirmek için kullanabilirsiniz.
> 
> 

![Yanıt tetikleyicisi](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>HTTP yanıt eylemi kullanın
HTTP yanıt eylemi, yalnızca bir HTTP isteği tarafından tetiklenip tetiklenmediğini bir iş akışında kullanıldığında geçerlidir. Bir yanıt eylemi tanımlamazsanız bir `202 ACCEPTED` yanıt hemen çağırana döndürülür.  İş akışı içinde herhangi bir adımı sırasında bir yanıt eylemi ekleyebilirsiniz. Mantıksal uygulama yalnızca gelen isteği bir yanıt için bir dakika açık tutar.  Bir iş akışından hiç yanıt gönderildi (ve bir yanıt eylemi tanımında mevcut değilse) dakika sonra bir `504 GATEWAY TIMEOUT` çağırana döndürülür.

Bir HTTP yanıt eylemi ekleme şöyledir:

1. Seçin **yeni adım** düğmesi.
2. Seçin **Eylem Ekle**.
3. Eylem arama kutusuna **yanıt** yanıt eylemi listelemek için.
   
    ![Yanıt eylemi seçin](./media/connectors-native-reqres/using-action-1.png)
4. HTTP yanıt iletisi için gerekli olan herhangi bir parametre ekleyin.
   
    ![Yanıt eylemi tamamlayın](./media/connectors-native-reqres/using-action-2.png)
5. Kaydetmek için araç çubuğunun sol üst köşesindeki tıklayın ve mantıksal uygulamanızı kaydetmek yayımlama hem (etkinleştir).

## <a name="request-trigger"></a>İstek tetikleyicisi
Bu bağlayıcıyı destekler tetikleyici için Ayrıntılar aşağıda verilmiştir. Tek bir istek tetikleyicisi yoktur.

| Tetikleyici | Açıklama |
| --- | --- |
| İstek |Bir HTTP isteği alındığında gerçekleşir. |

## <a name="response-action"></a>Yanıt eylemi
Bu bağlayıcıyı destekler eylemini ilgili ayrıntıları aşağıda verilmiştir. Bu istek tetikleyicisi ile birlikte, yalnızca bir tek bir yanıt eylemi yoktur.

| Eylem | Açıklama |
| --- | --- |
| Yanıt |Bağıntılı HTTP isteği bir yanıt döndürür |

### <a name="trigger-and-action-details"></a>Tetikleyici ve eylem ayrıntıları
Aşağıdaki tablo tetikleyici ve eylem için giriş alanlarını açıklar ve ilgili ayrıntıları çıktı.

#### <a name="request-trigger"></a>İstek tetikleyicisi
Gelen HTTP isteği tetikleyicisinden için giriş alanını verilmiştir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| JSON şeması |Şema |HTTP istek gövdesi JSON şeması |

<br>

**Çıkış ayrıntıları**

İstek için çıkış ayrıntıları aşağıda verilmiştir.

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Üst bilgiler |object |İstek üst bilgileri |
| Gövde |object |İstek nesnesi |

#### <a name="response-action"></a>Yanıt eylemi
HTTP yanıt eylemi için giriş alanlarını verilmiştir. A * gerekli alan olduğu anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Durum kodu * |statusCode |HTTP durum kodu |
| Üst bilgiler |Üst bilgileri |Tüm yanıt üstbilgilerini eklemek için bir JSON nesnesi |
| Gövde |body |Yanıt gövdesi |

## <a name="next-steps"></a>Sonraki adımlar
Şimdi, platformu deneyin ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Diğer bağlayıcıları logic apps'teki bakarak keşfedebilirsiniz bizim [API listesi](apis-list.md).

