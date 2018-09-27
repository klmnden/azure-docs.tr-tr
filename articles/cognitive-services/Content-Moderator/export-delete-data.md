---
title: Dışarı aktarma veya verilerinizi - Content Moderator Sil
titlesuffix: Azure Cognitive Services
description: Content Moderator verilerinizi silme veya dışarı öğrenin.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 9e671fa9735406ec425b105bb121d3a029ef19dc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227240"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Dışarı aktarma veya Content Moderator kullanıcı verilerini silme

Content Moderator, hizmeti çalıştırmak için kullanıcı bilgileri toplar, ancak müşterilerin kullanarak görüntüleme, verme ve kendi verilerini silme üzerinde tam denetime sahip [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) ve [API'leri](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Dışarı aktarma ve Content Moderator kullanıcı verilerini silme hakkında daha fazla bilgi için aşağıdaki tabloya bakın.

| Veriler | Dışarı aktarma işlemi | İşlemi Siler |
| ---- | ---------------- | ---------------- |
| Hesap bilgileri (Abonelik anahtarları) | Yok | Azure portal (Azure abonelikleri) kullanarak silin. Veya **silme Team** düğmesine [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |
| Özel eşleme için görüntüleri | [Resim kimlikleri alma](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Görüntüleri bir özel tek yönlü karmasını biçiminde depolanır ve gerçek görüntüleri ayıklamak için bir yolu yoktur. | [Tüm görüntüleri silin](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Veya Azure portalını kullanarak Content Moderator kaynağını silin. |
| Özel eşleme için koşulları | [Tüm terimler Al](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Tüm koşulları Sil](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Veya Azure portalını kullanarak Content Moderator kaynağını silin. |
| Etiketler | Yok | Kullanım **Sil** simgesi gözden geçirme UI etiketi Ayarları sayfasında her etiket için kullanılabilir. Veya **silme Team** düğmesine [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |
| İncelemeler | [Gözden geçirme alın](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Kullanım **silme Team** düğmesine [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası.
| Kullanıcılar | Yok | Kullanım **Sil** simgesi her kullanıcı için kullanılabilir [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. Veya **silme Team** düğmesine [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |

