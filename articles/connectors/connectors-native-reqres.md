---
title: "İstek ve yanıt eylemlerini kullanın | Microsoft Docs"
description: "İstek ve yanıt tetikleyici ve bir Azure mantıksal uygulama eylemde genel bakış"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 58210db585befd7ce915d4579d4d0303eb15bff3
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-request-and-response-components"></a>İstek ve yanıt bileşenleriyle çalışmaya başlayın
Bir mantıksal uygulama içinde istek ve yanıt bileşenleriyle gerçek zamanlı olarak olaylara yanıt verebilir.

Örneğin, şunları yapabilirsiniz:

* Bir mantıksal uygulama yoluyla şirket içi veritabanından verilerle bir HTTP isteğine yanıt.
* Bir mantıksal uygulama bir dış Web kancası olaydan tetikler.
* İstek ve yanıt eylemi başka bir mantıksal uygulama içinde ile bir mantıksal uygulama çağırın.

İstek ve yanıt eylemleri bir mantıksal uygulama kullanmaya başlamak için bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-http-request-trigger"></a>HTTP isteği tetikleyici kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](connectors-overview.md).

Burada, örnek dizisi mantığı Uygulama Tasarımcısı'nda bir HTTP isteği ayarlama konusunda verilmiştir.

1. Tetikleyici eklemek **isteği - olduğunda bir HTTP isteği alındığında** mantığı uygulamanıza. İsteğe bağlı olarak bir JSON şeması sağlayın (gibi bir araç kullanarak [JSONSchema.net](http://jsonschema.net)) istek gövdesi için. Bu HTTP istek özellikleri için belirteçleri oluşturmak tasarımcı sağlar.
2. Başka bir eylem ekleyebilirsiniz, böylece mantıksal uygulama kaydedebilirsiniz.
3. Mantıksal uygulama kaydedildikten sonra HTTP istek URL'si istek kartından alabilirsiniz.
4. Bir HTTP POST (gibi bir araç kullanabilirsiniz [Postman](https://www.getpostman.com/)) mantıksal uygulama URL'sini tetikler.

> [!NOTE]
> Bir yanıt eylemi tanımlarsanız olmayan bir `202 ACCEPTED` yanıtını çağırana hemen döndürülür. Bir yanıt özelleştirmek için yanıt eylemini kullanabilirsiniz.
> 
> 

![Yanıt tetikleyici](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>HTTP yanıtının eylem kullanın
HTTP yanıtının eylem, yalnızca bir HTTP isteğiyle tetiklenen bir iş akışında kullanıldığında geçerlidir. Bir yanıt eylemi tanımlarsanız olmayan bir `202 ACCEPTED` yanıtını çağırana hemen döndürülür.  İş akışı içinde herhangi bir adımı sırasında bir yanıt eylemi ekleyebilirsiniz. Mantıksal uygulama yalnızca gelen istek yanıt için bir dakika için açık tutar.  Bir yanıt iş akışını gönderilen (ve bir yanıt eylemi tanımında varsa) dakika sonra bir `504 GATEWAY TIMEOUT` çağırana döndürülür.

Bir HTTP yanıtının eylem ekleme şöyledir:

1. Seçin **yeni adım** düğmesi.
2. Seçin **Eylem Ekle**.
3. Eylem arama kutusuna yazın **yanıt** yanıt eylem listelemek için.
   
    ![Yanıt eylemi seçin](./media/connectors-native-reqres/using-action-1.png)
4. HTTP yanıt iletisi için gerekli olan parametreleri ekleyin.
   
    ![Yanıt eylemi tamamlamak](./media/connectors-native-reqres/using-action-2.png)
5. Kaydetmek için araç sol üst köşesindeki tıklayın ve mantıksal uygulamanızı kaydetmek yayımlama hem (etkinleştirin).

## <a name="request-trigger"></a>Tetikleyici isteği
Aşağıda, bu bağlayıcıyı destekler tetikleyici için Ayrıntılar verilmiştir. Bir tek istek tetikleyici yoktur.

| Tetikleyici | Açıklama |
| --- | --- |
| İstek |Bir HTTP isteği alındığında gerçekleşir |

## <a name="response-action"></a>Yanıt eylemi
Aşağıda, bu bağlayıcıyı destekler eylemi için Ayrıntılar verilmiştir. Bir istek tetikleyicisini eşlik yükleyen yalnızca kullanılabilir tek yanıt eylemi yok.

| Eylem | Açıklama |
| --- | --- |
| Yanıt |Bağıntılı HTTP isteği için bir yanıt döndürür |

### <a name="trigger-and-action-details"></a>Tetikleyici ve eylem ayrıntıları
Aşağıdaki tablolar tetikleyici ve eylem için girdi alanlarının açıklar ve ilgili ayrıntıları çıktı.

#### <a name="request-trigger"></a>Tetikleyici isteği
Bir gelen HTTP istek tetikleyici için giriş alanını verilmiştir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| JSON şeması |Şema |HTTP isteği gövdesinin JSON şeması |

<br>

**Çıkış Ayrıntıları**

İstek için çıkış ayrıntıları verilmiştir.

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Üst bilgiler |nesne |İstek üst bilgileri |
| Gövde |nesne |İstek nesnesi |

#### <a name="response-action"></a>Yanıt eylemi
HTTP yanıtının eylem için girdi alanlarının verilmiştir. A * gerekli bir alan olduğu anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Durum kodu * |statusCode |HTTP durum kodu |
| Üst bilgiler |headers |Tüm yanıt üstbilgilerini eklemek için bir JSON nesnesi |
| Gövde |body |Yanıt gövdesi |

## <a name="next-steps"></a>Sonraki adımlar
Şimdi, platform deneyin ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic apps diğer kullanılabilir bağlayıcılar bakarak keşfedebilirsiniz bizim [API'leri listesi](apis-list.md).

