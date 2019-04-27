---
title: Dışarı aktarma veya kullanıcı verilerini - Content Moderator Sil
titlesuffix: Azure Cognitive Services
description: Content Moderator verilerinizi silme veya dışarı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: pafarley
ms.openlocfilehash: 1cd70bee22e56e2580b322b93e0f121261d97a94
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60699190"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Dışarı aktarma veya Content Moderator kullanıcı verilerini silme

Content Moderator, hizmeti çalıştırmak için kullanıcı verilerini toplar, ancak imajlarını görüntülemek, dışarı aktarma ve kullanarak, verileri silmek için tam denetim [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) ve [denetimi ve İnceleme API'lerini](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Dışarı aktarma ve Content Moderator kullanıcı verilerini silme hakkında daha fazla bilgi için aşağıdaki tabloya bakın.

| Veriler | Dışarı aktarma işlemi | İşlemi Siler |
| ---- | ---------------- | ---------------- |
| Hesap bilgileri (Abonelik anahtarları) | Yok | Azure portal (Azure abonelikleri) kullanarak silin. Veya **silme Team** düğmesine [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |
| Özel eşleme için görüntüleri | Çağrı [Get görüntü kimlikleri API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Görüntüleri bir özel tek yönlü karmasını biçiminde depolanır ve gerçek görüntüleri ayıklamak için bir yolu yoktur. | Çağrı [tüm görüntüleri API'sini silmek](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Veya Azure portalını kullanarak Content Moderator kaynağını silin. |
| Özel eşleme için koşulları | CAL [tüm koşulları API Al](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | Çağrı [tüm koşulları API Sil](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Veya Azure portalını kullanarak Content Moderator kaynağını silin. |
| Etiketler | Yok | Kullanım **Sil** simgesi gözden geçirme UI etiketi Ayarları sayfasında her etiket için kullanılabilir. Veya **silme Team** düğmesine [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |
| İncelemeler | Çağrı [Get gözden geçirme API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Kullanım **silme Team** düğmesine [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası.
| Kullanıcılar | Yok | Kullanım **Sil** simgesi her kullanıcı için kullanılabilir [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. Veya **silme Team** düğmesine [gözden geçirme UI](https://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |

