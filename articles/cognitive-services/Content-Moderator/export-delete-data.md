---
title: Dışarı aktarma veya içerik aracı - Azure Bilişsel hizmetler verilerinizi silme | Microsoft Docs
description: Dışarı aktarma veya içerik denetleyici verilerinizi silmek öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 1dbb645a033c6db5ffa9003a53f30fd927131298
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355373"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Dışarı aktarma veya içerik yönetici kullanıcı verilerini sil

Denetleyici hizmeti çalıştırmak için kullanıcı verilerini toplar, ancak müşterilerin kullanarak görüntüleme, dışarı aktarma ve kendi veri silme üzerinde tam denetime sahip içerik [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) ve [API'leri](https://docs.microsoft.com/en-us/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Dışarı aktarma ve içerik yönetici kullanıcı verilerini silme hakkında daha fazla bilgi için aşağıdaki tabloya bakın.

| Veriler | Dışarı aktarma işlemi | İşlemi Siler |
| ---- | ---------------- | ---------------- |
| Hesap bilgileri (Abonelik anahtarları) | Yok | Azure Portalı'nı (Azure abonelikleri) kullanarak silin. Veya **silmek takım** düğmesini [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |
| Özel eşleme için görüntüleri | [Resim kimlikleri alma](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Görüntüleri özel tek yönlü karma biçiminde depolanır ve gerçek görüntüleri ayıklamak için bir yolu yoktur. | [Tüm görüntüleri silin](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Veya Azure portalını kullanarak içerik denetleyici kaynak silin. |
| Özel eşleme koşulları | [Tüm koşulları Al](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Tüm koşulları silme](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Veya Azure portalını kullanarak içerik denetleyici kaynak silin. |
| Etiketler | Yok | Kullanım **silmek** simgesi gözden geçirme UI etiketi Ayarları sayfasında her etiket için kullanılabilir. Veya **silmek takım** düğmesini [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |
| İncelemeler | [Gözden geçirme Al](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Kullanım **silmek takım** düğmesini [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası.
| Kullanıcılar | Yok | Kullanım **silmek** simgesi her kullanıcı için kullanılabilir [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. Veya **silmek takım** düğmesini [gözden geçirme UI](http://contentmoderator.cognitive.microsoft.com/) takım Ayarları sayfası. |

