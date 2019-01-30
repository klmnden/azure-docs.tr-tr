---
title: Content Moderator'ın gözden geçirme aracı ayarları - Content Moderator'ı yapılandırma
titlesuffix: Azure Cognitive Services
description: Takım, etiketler, bağlayıcı, iş akışları ve kimlik bilgilerini almak veya yapılandırın.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 32563b429764a45efbef952310ab6615567d42f0
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210939"
---
# <a name="configure-review-tool-settings"></a>Gözden geçirme aracı ayarlarını yapılandırma

Gözden geçirme aracı Panoda Ayarlar sekmesini kullanarak tanımlayıp birçok bileşen değiştirmek kolaydır.

![Content Moderator gözden geçirmesi ayarları](images/settings-1.png)

## <a name="team-and-subteams"></a>Takım ve alt takımlar

Bu sekmeden, ekipler ve alt ekipler yönetin. Yalnızca bir takım olabilir, ancak yapabilecekleriniz [birden çok alt ekipler oluşturma](subteams.md) ve gelecekteki üyeleri için davetiye gönderin. Davetler gönderdikten sonra bunları izlemek, takım üyelerinin izinlerini değiştirmek ve ek kullanıcılar davet edin. Takım üyeleri, daveti kabul sonra bu üyeler için farklı alt ekipler atayabilirsiniz. Takım üyelerinin rollerinin Yöneticiler ya da gözden geçirenler olması ayarlayabilirsiniz: Gözden Geçiren işleyemez, Yöneticiler diğer kullanıcıları davet edebildiğiniz.

![Content Moderator takım ayarları](images/settings-2-team.png)

## <a name="tags"></a>Etiketler

Bu, şunları yapabilirsiniz [özel etiketler tanımlamak](tags.md) kısa kodu, adı ve etiketlerinizi için açıklama girerek. Oluşturduktan sonra incelemeleri sırasında kullanılabilir. Görünürlük kapatıp açarak farklı incelemeleri için farklı etiketleri kullanabilirsiniz.

![Content Moderator ayarları etiketleri](images/settings-3-tags.png)

## <a name="connectors"></a>Bağlayıcılar

İş akışlarını gözden geçirme aracı ile iletişim kurmak için bağlayıcılar kullanarak işlevselliği ekleyin. İnceleme aracını yönetme için içerik Moderator API'leri varsayılan iş akışı ile içerik çağırır. Gözden geçirme aracı için kaydolduğunuzda, otomatik-hükümler Moderator API'si kimlik bilgileri sizin için. Bir bağlayıcı kullanılabilir olduğu sürece diğer bağlayıcı API'leri, tümleştirme de destekler. Bazı bağlayıcılar kullanıma hazır gerçekleştirdik.

[Bağlayıcılar sekmesi](connectors.md) bağlayıcılar yönettiği yerdir. Ekleyin veya bağlayıcıları silme ve abonelik anahtarınız için belirli bir bağlayıcı bulunamadı. Bu özel iş akışlarınıza eklemek için Bağlan'a tıklayın. 

![Content Moderator bağlayıcı ayarları](images/settings-4-connectors.png)

## <a name="workflows"></a>İş akışları

İş akışları, iş akışları sekmesinden yönetin. Örnek bir dosya karşıya yükleyerek iş akışları test edebilirsiniz. Ayrıca [özel iş akışlarınızı tanımlamanızı](workflows.md) resim ve metin (bağlayıcılar sekmesinde bulunur) API bağlayıcıları kullanarak için. 

![Content Moderator iş akışı ayarları](images/settings-5-workflows.png)

## <a name="credentials"></a>Kimlik Bilgileri

Bu sekme, dahil edilen API'ler Content Moderator ile (görüntü denetimi, metin denetimi, listesi yönetimi, iş akışı ve gözden geçirme API'leri) kullanmanız gerekir, Content Moderator abonelik anahtarı hızlı erişim sağlar.
 
![Content Moderator kimlik bilgileri](images/settings-6-credentials.png)
