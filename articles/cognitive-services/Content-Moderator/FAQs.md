---
title: Sık sorulan sorular - Content Moderator
titlesuffix: Azure Cognitive Services
description: Content Moderator hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 11/21/2016
ms.author: sajagtap
ms.openlocfilehash: 99b95a26a91d14edbef264ac3b107f8462e0ef5e
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57762186"
---
# <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

#### <a name="what-does-my-content-moderator-subscription-include"></a>Abonelik dahil my Content Moderator nedir?
Erişim gözden geçirme aracı ve API'ler için Content Moderator aboneliğinize dahildir. Bir ya da diğer veya her ikisi de iş gereksinimlerinize bağlı olarak kullanmak isteyip istemediğinizi karar verebilirsiniz.

#### <a name="what-are-the-limitsrestrictions-of-the-content-that-can-be-moderated-by-using-the-api"></a>API kullanılarak yönetilebilen içerikler için limitler/kısıtlamalar nelerdir?
API kullanırken, görüntüleri en az 128 Pixel ve en büyük dosya boyutu 4 MB olması gerekir. Metin en fazla 1024 karakter uzunluğunda olabilir. Video süresi sınırlı değildir.

#### <a name="what-happens-if-the-content-passed-to-the-text-api-or-the-image-api-exceeds-the-size-limits"></a>Metin API’sine veya görüntü API’sine geçirilen içeriğin, boyut sınırını aşması durumunda ne olur?
Metin çevirisi API'si, metnin izin verilenden daha uzun olduğunu bildiren bir hata kodu döndürür. Görüntü API'si aynı zamanda görüntü boyutu gereksinimlerini karşılamıyor bildiren bir hata kodu döndürür.

#### <a name="do-you-keep-the-images-text-or-videos-that-are-submitted-for-moderation"></a>Resim, metin veya denetimi için gönderilen videolar korunsun mu?
İçeriğinizi size aittir ve Microsoft tarafından izniniz olmadan çocuğunuz tutulabileceği değil. Microsoft, sektör lideri güvenlik önlemleri içeriğinizin korunmasına yardımcı olmak için kullanır.

#### <a name="can-i-use-content-moderator-to-screen-for-illegal-child-exploitation-images"></a>Content Moderator ekrana geçersiz alt istismarıyla görüntüler için kullanabilir miyim?
Hayır. Ancak, tam kuruluşlar kullanabilir [PhotoDNA bulut hizmeti](https://www.microsoft.com/photodna "Microsoft PhotoDNA bulut hizmeti") ekranına bu içerik türü için.

#### <a name="up-to-how-many-review-teams-can-a-user-join-can-the-user-switch-between-teams"></a>Bir kullanıcı en fazla kaç takımlar gözden birleştirebilir miyim? Kullanıcı, takımlar arasında geçiş yapabilir?
Bir kullanıcı aynı anda bir takım katılabilirsiniz.

#### <a name="what-kind-of-team-member-roles-are-supported-by-the-review-tool-how-are-they-different"></a>Ne tür bir takım üyesi tarafından İnceleme aracı rolleri desteklenir? Bunların birbirinden farkı nedir?
Studio, şu anda yönetici ve İnceleme rol atama sağlar. Yöneticiler, diğer kullanıcıları davet ve gözden geçirenler, yalnızca denetleme sonuçları ve etiketi gözden geçirmek veya içerik etiketini kaldırma sırasında yapılandırma ayarlarına erişebilirsiniz.

#### <a name="what-is-a-tag-does-the-review-tool-support-custom-tags"></a>Etiket nedir? İnceleme aracını, özel etiketler destekliyor mu?
Bir etikettir veya müstehcen vb. için yetişkin, 'r' için 'bir' gibi bir denetimi bayrak gösteren iki harfli kısa kodu. Yöneticiler, iş için yeni etiketler gerektiği şekilde tanımlayabilirsiniz.

#### <a name="how-many-team-members-can-i-have-in-my-review-team"></a>Kaç takım üyelerinin gözden geçirme takımım sahip olabilir miyim?
Ekip yöneticisi de dahil olmak üzere en fazla beş ekip üyeniz olabilir.
