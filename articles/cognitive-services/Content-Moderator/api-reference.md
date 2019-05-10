---
title: API Başvurusu - Content Moderator
titlesuffix: Azure Cognitive Services
description: Çeşitli içerik denetleme hakkında bilgi edinin ve API'ler için Content Moderator gözden geçirin.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: reference
ms.date: 04/30/2019
ms.author: sajagtap
ms.openlocfilehash: 19144ae40e67127b656cedd61199b732b1c05e86
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236615"
---
# <a name="content-moderator-api-reference"></a>Content Moderator API'si başvurusu

Aşağıdaki yollarla Azure Content Moderator API'leri ile kullanmaya başlayın:

- Azure portalında [Content Moderator API'si için abone](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator).
- Bkz: [deneyin Content Moderator Web'de](quick-start.md) ile kaydolmak için [Content Moderator İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/).

## <a name="moderation-apis"></a>Yönetim API’leri

Aşağıdaki Content Moderator API'sine sonrası denetimi akışlarınızı ayarlamak için kullanabilirsiniz.

| Açıklama | Başvuru |
| -------------------- |-------------|
| **Görüntü denetimi API'si**<br /><br />Tarama görüntüleri ve etiketleri, güven puanları ve diğer ayıklanan bilgileri kullanarak olası yetişkinlere yönelik ve müstehcen içerikleri algılayın. <br /><br />Yayımlama, reddetme veya sonrası denetimi iş akışınızda içeriği gözden geçir için bu bilgileri kullanın. <br /><br />| [Görüntü Denetim API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "görüntü denetim API'si başvurusu")   |
| **Metin denetimi API'si**<br /><br />Metin içeriğini tarar. Küfür hüküm ve kişisel veriler döndürülür. <br /><br />Yayımlama, reddetme veya sonrası denetimi iş akışınızda içeriği gözden geçir için bu bilgileri kullanın.<br /><br /> | [Metin denetimi API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "metin denetimi API'si başvurusu")   |
| **Video denetimi API'si**<br /><br />Videoları taramak ve olası yetişkinlere yönelik ve müstehcen içerikleri algılayın. <br /><br />Yayımlama, reddetme veya sonrası denetimi iş akışınızda içeriği gözden geçir için bu bilgileri kullanın.<br /><br /> | [Video denetimi API'sine genel bakış](video-moderation-api.md "Video denetimi API'sine genel bakış")   |
| **Listeyi yönetim API'si**<br /><br />Oluşturun ve resim ve metin özel dışlama veya ekleme listeleri yönetin. Etkinleştirilirse, **görüntü - eşleşen** ve **metin - ekran** işlemleri yapıp özel listelerinizle gönderilen içerik benzer öğe eşleştirmesi. <br /><br />Verimlilik için makine öğrenme tabanlı denetimi adımı atlayabilirsiniz.<br /><br /> | [Yönetim API başvuru listesinde](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "listesi yönetim API Başvurusu")   |

## <a name="review-apis"></a>API’leri inceleme

Gözden geçirme API aşağıdaki bileşenlere sahiptir:

| Açıklama | Başvuru |
| -------------------- |-------------|
| **İşler**<br /><br /> Hem resim ve metin içeriği için tarama ve gözden geçirme denetimi iş akışları başlatın. Bir denetimi işi görüntü denetim API'si ve metin denetimi API'si kullanarak içeriğinizi tarar. Denetimi işleri tanımlanmış kullanın ve iş akışlarını gözden geçirmeleri oluşturmak için varsayılan. <br /><br />İnsan moderatör otomatik olarak atanan etiketleri ve tahmin verilerini inceleme ve içerik denetleme karar gönderilen sonra gözden geçirme API, API uç noktanıza tüm bilgileri gönderir.<br /><br /> | [Proje başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "proje başvurusu")   |
| **Gözden geçirmeler**<br /><br />Görüntü veya metin incelemeleri İnsan Moderatörler için doğrudan oluşturmak için İnceleme aracını kullanın.<br /><br /> İnsan moderatör otomatik olarak atanan etiketleri ve tahmin verilerini inceleme ve içerik denetleme karar gönderilen sonra gözden geçirme API, API uç noktanıza tüm bilgileri gönderir.<br /><br /> | [Gözden geçirin, başvuru](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "başvuru gözden geçirme")   |
| **İş akışları**<br /><br />Oluşturma, güncelleştirme ve ekibinizin oluşturduğu özel iş akışları hakkında ayrıntılı bilgi edinin. İnceleme aracını kullanarak iş akışlarını tanımlarsınız. <br /> <br />İş akışları genellikle Content Moderator kullanır, ancak gözden geçirme Aracı'nda bağlayıcılar olarak kullanılabilen belirli bir API de kullanabilirsiniz.<br /><br /> | [İş akışı başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "iş akışı başvurusu")   |