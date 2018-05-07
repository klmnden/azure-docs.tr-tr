---
title: "Azure Media Kırpıcıyı ile Klip Oluştur | Microsoft Docs"
description: "Azure Media Kırpıcıyı, medya klipleri varlıklarından oluşturmaya yönelik bir araç genel bakış"
services: media-services
keywords: "küçük; subclip; kodlama; ortam"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: f3822386d0d16b1feaf16853424329558a18f910
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-clips-with-azure-media-clipper"></a>Azure Media Kırpıcıyı ile Klip Oluştur
Azure Media Kırpıcıyı medya klipleri oluşturmak için kullanıcılar bir arabirim sağlamak web geliştiricileri sağlayan ücretsiz bir JavaScript kitaplığıdır. Bu araç, herhangi bir web sayfasında tümleştirilebilir ve varlıkları yüklenirken ve kırpma işlerini göndermenin için API'ler sağlar.

Azure Media Kırpıcıyı sağlar:
- Ön Kurşun kırpma ve sonrası slate gelen arşivler Canlı 
- Video vurgular AMS Canlı olayları, Canlı arşivler ya da fMP4 VOD dosyaları oluşturma 
- Birden fazla kaynaktan videolar birleştirme 
- AMS medya varlıklarınızı Özet küçük resimler üretir 
- Çerçeve doğruluk ile küçük video 
- Dinamik bildirim filtreleri resimleri grubu (GOP) doğruluk ile canlı var ve VOD varlıklar oluşturur 
- Medya Hizmetleri hesabınızı varlıkları karşı kodlama işleri oluşturmak

Yeni özellikler istemek için fikirleri ya da geribildirim sağlamak, Gönder [Azure Media Services için UserVoice](http://aka.ms/amsvoice/). Varsa ve belirli sorunları, sorular veya hataları, Media Services açılan bir satırında ekip Bul amcinfo@microsoft.com.

Aşağıdaki resimde Kırpıcıyı arabirimi gösterilmektedir: ![Azure medya Kırpıcıyı](media/media-services-azure-media-clipper-overview/media-services-azure-media-clipper-interface.PNG)

## <a name="release-notes"></a>Sürüm notları
Kırpıcıyı blog yayını, çeşitli bilinen sorunlar ve değişim günlüğü Kırpıcıyı'nın en son sürümü için aşağıdaki listeye bakın:
- [Blog gönderisi](https://azure.microsoft.com/blog/azure-media-clipper/)
- [Bilinen sorunlar listesi](https://amp.azure.net/libs/amc/latest/docs/known_issues.html)
- [Değişim günlüğü](https://amp.azure.net/libs/amc/latest/docs/changelog.html)

## <a name="browser-support"></a>Tarayıcı desteği
Azure Media Kırpıcıyı modern HTML5 teknolojileri kullanılarak oluşturulmuştur ve aşağıdaki tarayıcılardan destekler:

- Microsoft Edge 13 +
- Internet Explorer 11 +
- Chrome 54 +
- Safari 10 +
- Firefox 50 +

> [!NOTE]
> Yalnızca Azure Media Services akışlardan HTML5 kayıttan şu anda desteklenmiyor.

## <a name="language-support"></a>Dil desteği
Kırpıcıyı pencere öğesi 18 aşağıdaki dillerde kullanılabilir:
- Çince (Basitleştirilmiş)
- Çince (Geleneksel)
- Çekçe
- Felemenkçe, Flemish
- Türkçe
- Fransızca
- Almanca
- Macarca
- İtalyanca
- Japonca
- Kore dili
- Lehçe
- Portekizce (Brezilya)
- Portekizce (Portekiz)
- Rusça
- İspanyolca
- İsveç dili
- Türkçe

## <a name="next-steps"></a>Sonraki adımlar
Azure Media Kırpıcıyı kullanmaya başlamak için okuma [Başlarken](media-services-azure-media-clipper-getting-started.md) makale pencere öğesi dağıtma hakkında ayrıntılı bilgi için.
