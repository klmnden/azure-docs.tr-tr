---
title: İçerik denetleyicinin gözden geçirme Aracı'nın ayarlarını yapılandırma | Microsoft Docs
description: Yapılandırma veya takım, etiketler, bağlayıcılar, iş akışları ve kimlik bilgilerini alın.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: a3432a1d8f424fbe78570f47b774c6e7942e16b1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351508"
---
# <a name="about-review-tool-settings"></a>Gözden geçirme aracı ayarları hakkında #

Gözden geçirme aracı Panoda ayarları sekmesini kullanarak tanımlayabilir ve birçok bileşen değiştirmek çok kolaydır.

![İçerik denetleyici inceleme ayarları](images/settings-1.png)

## <a name="team-and-subteams"></a>Takım ve Subteams ## 

Bu sekmeden, ekip ve subteams yönetin. Yalnızca bir ekip olabilir, ancak yapabilecekleriniz [birden çok subteams oluşturma](subteams.md) ve gelecekteki üyelerine Davetleri Gönder. Davetiye gönderdikten sonra bunları izleyebilirsiniz, takım üyeleri için izinleri değiştirmek ve ek kullanıcıları davet. Takım üyeleri daveti kabul eden sonra bu üyeler için farklı subteams atayabilirsiniz. Yöneticiler veya gözden geçirenler olarak ekip üyelerinin rolleri ayarlayabilirsiniz: gözden geçirenler işleyemez, Yöneticiler diğer kullanıcıların davet.

![İçerik denetleyici takım ayarları](images/settings-2-team.png)

## <a name="tags"></a>Etiketler ##

Burasıdır [özel etiketler tanımlamak](tags.md) kısa bir kod, ad ve açıklama, etiketler için girerek. Oluşturduktan sonra incelemeleri sırasında kullanılabilir. Görünürlüğü kapatıp açarak farklı incelemeler için farklı etiketlerini kullanabilirsiniz.

![İçerik denetleyici ayarları etiketleri](images/settings-3-tags.png)

## <a name="connectors"></a>Bağlayıcılar ##

İş akışları için İnceleme aracı ile iletişim kurmak için bağlayıcılar kullanılarak işlevselliği ekleyin. Gözden geçirme Aracı'nı yönetme için varsayılan iş akışı ile içerik yönetici API'ları içerik çağırır. Gözden geçirme aracı için kaydolduğunuzda, otomatik sağlarken, denetleyici API kimlik bilgileri. Bir bağlayıcı kullanılabilir olduğu sürece diğer bağlayıcı API'lerini, tümleştirme de destekler. Biz birkaç bağlayıcılar kullanılabilir kutu dışı yaptınız.

[Bağlayıcılar sekmesini](connectors.md) nerede bağlayıcılar yönetmek olan. Ekleyin veya bağlayıcılarını silip ve belirli bir bağlayıcı için abonelik anahtarınızı bulun. Bu, özel iş akışlarınızı eklemek için Bağlan'ı tıklatın. 

![İçerik denetleyici bağlayıcılar ayarları](images/settings-4-connectors.png)

## <a name="workflows"></a>İş akışları ##

İş akışları iş akışları sekmesinden yönetin. Örnek dosya karşıya yükleyerek iş akışları test edebilirsiniz. Ayrıca [özel iş akışları tanımlamak](workflows.md) görüntü ve (bağlayıcılar sekmesinde bulunur) kullanılabilir API bağlayıcıları kullanarak metin. 

![İçerik yönetici iş akışı ayarları](images/settings-5-workflows.png)

## <a name="credentials"></a>Kimlik Bilgileri ##

Bu sekme içerik yöneticiyi ile (Görüntü Yönetimi, metin denetleme, liste yönetimi, iş akışı ve gözden geçirme API'leri) dahil API'ları kullanmak için gerekir, içerik denetleyici abonelik anahtarınızı hızlı erişim sağlar.
 
![İçerik yönetici kimlik bilgileri](images/settings-6-credentials.png)
