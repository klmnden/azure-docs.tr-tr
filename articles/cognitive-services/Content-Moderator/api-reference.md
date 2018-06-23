---
title: Azure içerik denetleyici API Başvurusu | Microsoft Docs
description: İçerik Yönetimi hakkında bilgi edinin ve API'ler için içerik denetleyici gözden geçirin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 94de9b91cc242e8b7e5479cacab8380adac551f6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351563"
---
# <a name="content-moderator-api-reference"></a>Content Moderator API'si başvurusu

Aşağıdaki yollarla Azure içerik denetleyici API'leri ile çalışmaya başlama: (Ayrıca bkz. [kimlik bilgilerini yönetme](review-tool-user-guide/credentials.md).)

- Azure portalında [içerik denetleyici API'leri abone](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator).
- Kaydolun [içerik denetleyici gözden geçirme aracı](http://contentmoderator.cognitive.microsoft.com/). Bkz: [Hızlı Başlangıç](quick-start.md).

## <a name="moderation-apis"></a>Yönetim API’leri

Sonrası yönetimini akışlarınızı ayarlamak için aşağıdaki içerik denetleyici API'leri kullanabilirsiniz.

| Açıklama | Başvuru |
| -------------------- |-------------|
| **Görüntü Yönetimi API**<br /><br />Görüntüleri tarama ve etiketleri, güvenirlik puanları ve diğer bilgiler ayıklandı kullanarak olası yetişkin ve saldırganlardan içeriği algılayabilir. <br /><br />Yayımlama, reddetme veya sonrası yönetimini iş akışınızda içeriği gözden geçir için bu bilgileri kullanın. <br /><br />| [Görüntü Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "Görüntü Yönetimi API Başvurusu")   |
| **Metin denetleme API**<br /><br />Metin içeriği tarar. Uygunsuz metin koşulları ve kişisel bilgileri (PII) döndürülür. <br /><br />Yayımlama, reddetme veya sonrası yönetimini iş akışınızda içeriği gözden geçir için bu bilgileri kullanın.<br /><br /> | [Metin denetleme API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "metin denetleme API Başvurusu")   |
| **Video yönetimini API**<br /><br />Videolar tarama ve olası yetişkin ve saldırganlardan içeriği algılayabilir. <br /><br />Yayımlama, reddetme veya sonrası yönetimini iş akışınızda içeriği gözden geçir için bu bilgileri kullanın.<br /><br /> | [Video yönetimini API genel bakış](video-moderation-api.md "Video yönetimini API genel bakış")   |
| **Liste yönetim API'si**<br /><br />Oluşturun ve özel dışlama veya ekleme listesini görüntüler ve metinler yönetin. Etkinleştirilirse, **görüntü - eşleşen** ve **metin - ekran** işlemleri yapıp özel listelerinizi karşı gönderilen içerik benzer eşleştirme. <br /><br />Verimlilik için makine öğrenme tabanlı yönetimini adımı atlayabilirsiniz.<br /><br /> | [Liste yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "listesi Yönetimi API Başvurusu")   |

## <a name="review-api"></a>Gözden geçirme API

Gözden geçirme API aşağıdaki bileşenlere sahiptir:

| Açıklama | Başvuru |
| -------------------- |-------------|
| **İşler**<br /><br /> Resim ve metin içeriği için tarama ve İnceleme Yönetimi iş akışlarını başlatmak. Denetleme işi Görüntü Yönetimi API ve metin denetleme API kullanarak içeriğinizi tarar. Denetleme işleri ve incelemeler oluşturmak için iş akışlarını varsayılan tanımlı kullanın. <br /><br />İnsan denetleyici otomatik atanan etiketleri ve tahmin veri gözden geçirdikten ve içerik yönetimini karar gönderilen sonra gözden geçirme API, API uç tüm bilgileri gönderir.<br /><br /> | [Proje başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "proje başvurusu")   |
| **Gözden geçirme**<br /><br />Görüntü veya metin incelemeler İnsan denetleyiciler için doğrudan oluşturmak için İnceleme Aracı'nı kullanın.<br /><br /> İnsan denetleyici otomatik atanan etiketleri ve tahmin veri gözden geçirdikten ve içerik yönetimini karar gönderilen sonra gözden geçirme API, API uç tüm bilgileri gönderir.<br /><br /> | [Gözden başvuru](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "gözden başvurusu")   |
| **İş akışları**<br /><br />Oluştur, Güncelleştir ve ekibinizin oluşturduğu özel iş akışları hakkında bilgi almak. Gözden geçirme Aracı'nı kullanarak iş akışları tanımlar. <br /> <br />İş akışları genellikle içerik denetleyici kullanır, ancak gözden geçirme aracında bağlayıcılar olarak kullanılabilen belirli bir API de kullanabilirsiniz.<br /><br /> | [İş akışı başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "iş akışı başvurusu")   |


