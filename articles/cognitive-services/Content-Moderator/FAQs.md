---
title: Azure içerik denetleyici için sık sorulan sorular | Microsoft Docs
description: İçerik denetleyici hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 11/21/2016
ms.author: sajagtap
ms.openlocfilehash: 7bf6c67bd0a83d9455d14253f4f4b2becafd29ba
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351593"
---
# <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

#### <a name="what-does-my-content-moderator-subscription-include"></a>My içerik abonelik dahil denetleyici nedir?
İçerik denetleyici aboneliğiniz gözden geçirme araç ve API'leri erişim içerir. Bir ya da diğer veya her ikisi de iş gereksinimlerinize bağlı olarak kullanmak isteyip istemediğinize karar verebilirsiniz.

#### <a name="what-are-the-limitsrestrictions-of-the-content-that-can-be-moderated-by-using-the-api"></a>API kullanılarak yönetilebilen içerikler için limitler/kısıtlamalar nelerdir?
API kullanırken, görüntüleri 128 piksel minimum ve maksimum dosya boyutu 4 MB olması gerekir. Metin en fazla 1024 karakter uzunluğunda olabilir. Video süreye göre bir sınır yoktur.

#### <a name="what-happens-if-the-content-passed-to-the-text-api-or-the-image-api-exceeds-the-size-limits"></a>Metin API’sine veya görüntü API’sine geçirilen içeriğin, boyut sınırını aşması durumunda ne olur?
API metin izin verilenden daha uzun olduğunu bildiren bir hata kodu metni döndürür. Görüntünün API de görüntünün boyut gereksinimlerini karşılamıyor bildiren bir hata kodu döndürür.

#### <a name="do-you-keep-the-images-text-or-videos-that-are-submitted-for-moderation"></a>Resimler, metin veya denetleme için gönderilen videolar tutarım?
İçeriğinizi kendi ve Microsoft tarafından sizin izniniz olmadan korunmayabilir değil. Microsoft, içeriğinizi korumak için endüstri lideri güvenlik önlemleri kullanır.

#### <a name="can-i-use-content-moderator-to-screen-for-illegal-child-exploitation-images"></a>İçerik denetleyici ekranına geçersiz alt istismarı görüntüler için kullanabilir miyim?
Hayır. Ancak, tam kuruluşlar kullanabilir [PhotoDNA bulut hizmeti](https://www.microsoft.com/photodna "Microsoft PhotoDNA bulut hizmeti") ekranına bu içerik türü için.

#### <a name="up-to-how-many-review-teams-can-a-user-join-can-the-user-switch-between-teams"></a>Kaç tane takımlar gözden kadar kullanıcı birleştirebilir miyim? Kullanıcı takımlar arasında geçiş yapabilirsiniz?
Bir kullanıcı aynı anda bir ekip birleştirebilirsiniz.

#### <a name="what-kind-of-team-member-roles-are-supported-by-the-review-tool-how-are-they-different"></a>Ne tür bir ekip üyesi rollerini gözden geçirme aracı tarafından destekleniyor mu? Bunların birbirinden farkı nedir?
Studio şu anda yönetici ve İnceleme rolleri atama sağlar. Yöneticiler, diğer kullanıcıları davet ve gözden geçirenler, yalnızca denetleme sonuçları ve etiket gözden geçirmek veya içerik etiketini kaldırma sırasında yapılandırma ayarlarını erişebilir.

#### <a name="what-is-a-tag-does-the-review-tool-support-custom-tags"></a>Bir etiketi nedir? Gözden geçirme Aracı'nı özel etiketler destekliyor mu?
Bir etiketi olan veya saldırganlardan vb. için yetişkin, 'r' 'bir' gibi bir yönetimini bayrak gösterir kısa bir kod iki harfli. Yöneticiler, gerektiğinde, iş için yeni etiketler tanımlayabilir.

#### <a name="how-many-team-members-can-i-have-in-my-review-team"></a>Kaç tane ekip üyelerinin my İnceleme takımı sahip olabilir miyim?
Ekip yöneticisi de dahil olmak üzere en fazla beş ekip üyeniz olabilir.
