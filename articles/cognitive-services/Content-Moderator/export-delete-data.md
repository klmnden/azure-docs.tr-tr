---
title: Dışarı aktarma veya verilerinizi - Content Moderator Sil
titlesuffix: Azure Cognitive Services
description: Content Moderator verilerinizi silme veya dışarı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: pafarley
ms.openlocfilehash: 356cc2274f4e29ece75abce392dcf71f13a439fc
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55217705"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Dışarı aktarma veya Content Moderator kullanıcı verilerini silme

Content Moderator, hizmeti çalıştırmak için kullanıcı bilgileri toplar, ancak müşterilerin kullanarak görüntüleme, verme ve kendi verilerini silme üzerinde tam denetime sahip [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) ve [API'leri](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Dışarı aktarma ve Content Moderator kullanıcı verilerini silme hakkında daha fazla bilgi için aşağıdaki tabloya bakın.

| Veriler | Dışarı aktarma işlemi | İşlemi Siler |
| ---- | ---------------- | ---------------- |
| Hesap bilgileri (Abonelik anahtarları) | Yok | Azure portal (Azure abonelikleri) kullanarak silin. Veya **silme Team** düğmesine [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |
| Özel eşleme için görüntüleri | [Resim kimlikleri alma](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Görüntüleri bir özel tek yönlü karmasını biçiminde depolanır ve gerçek görüntüleri ayıklamak için bir yolu yoktur. | [Tüm görüntüleri silin](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Veya Azure portalını kullanarak Content Moderator kaynağını silin. |
| Özel eşleme için koşulları | [Tüm terimler Al](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Tüm koşulları Sil](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Veya Azure portalını kullanarak Content Moderator kaynağını silin. |
| Etiketler | Yok | Kullanım **Sil** simgesi gözden geçirme UI etiketi Ayarları sayfasında her etiket için kullanılabilir. Veya **silme Team** düğmesine [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |
| İncelemeler | [Gözden geçirme alın](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Kullanım **silme Team** düğmesine [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası.
| Kullanıcılar | Yok | Kullanım **Sil** simgesi her kullanıcı için kullanılabilir [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. Veya **silme Team** düğmesine [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |

