---
title: Azure Media Clipper'ı ile küçük resimleri oluşturun | Microsoft Docs
description: Medya klipleri varlıklarından oluşturmaya yönelik bir araç olan Azure Media Clipper'ne genel bakış
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 35f1f359b44af00000ccd9047673b80ca541d376
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61243876"
---
# <a name="create-clips-with-azure-media-clipper"></a>Küçük resimleri ile Azure Media Clipper'ı oluşturma 

Azure Media Clipper'ı kullanıcılarının medya klipler oluşturmak için bir arabirim sağlamak, web geliştiricilerin sağlayan ücretsiz bir JavaScript kitaplığıdır. Bu araç, herhangi bir web sayfasında tümleştirilebilir ve varlıklar yükleniyor ve kırpma işlerini gönderme için API'ler sağlar.

Azure Media Clipper'ı ile yapabilecekleriniz:
- Öncesi Kurşun trim ve sonrası maskeleme görüntüsü gelen arşivleri Canlı 
- AMS Canlı etkinlikler, Canlı arşivlerinizin ya da fMP4 VOD dosyaları VIDEO oluştur 
- Videoları birden çok kaynaktan birleştirme 
- AMS'yi medya varlıklarınızı Özet klipler oluşturmak 
- Çerçeve doğrulukla videoları küçük 
- Resim grubu (GOP) doğrulukla mevcut canlı ve VOD varlıklar üzerinde dinamik bildirim filtreleri oluşturma 
- Kodlama işleri media services hesabınızı varlıkları karşı üretir

Yeni özellikleri istemek için fikirleri ya da geribildirim sağlamak, Gönder [Azure Media Services için UserVoice](https://aka.ms/amsvoice/). Varsa ve belirli sorunları, soru veya hataları, Media Services bırakma ekip bir satırı Bul amcinfo@microsoft.com.

Aşağıdaki görüntüde Clipper arabirimi gösterir: ![Azure Media Clipper'ı](media/media-services-azure-media-clipper-overview/media-services-azure-media-clipper-interface.PNG)

## <a name="release-notes"></a>Sürüm notları
Clipper blog gönderisinde, çeşitli bilinen sorunlar ve changelog Clipper'ı en son sürümü için aşağıdaki listeye bakın:
- [Blog gönderisi](https://azure.microsoft.com/blog/azure-media-clipper/)
- [Bilinen sorunların listesi](https://amp.azure.net/libs/amc/latest/docs/known_issues.html)
- [Changelog](https://amp.azure.net/libs/amc/latest/docs/changelog.html)

## <a name="browser-support"></a>Tarayıcı desteği
Azure Media Clipper'ı modern HTML5 teknolojiler kullanılarak oluşturulmuştur ve aşağıdaki tarayıcılardan destekler:

- Microsoft Edge 13+
- Internet Explorer 11+
- Chrome 54+
- Safari 10+
- Firefox 50+

> [!NOTE]
> Yalnızca HTML5 kayıttan yürütme akışları Azure Media Services tarafından şu anda desteklenmiyor.

## <a name="language-support"></a>Dil desteği
Clipper pencere 18 aşağıdaki dillerde kullanılabilir:
- Çince (Basitleştirilmiş)
- seçenekleri yerine
- Çekçe
- Hollanda dili, Flemish
- Türkçe
- Fransızca
- Almanca
- Macarca
- İtalyanca
- Japonca
- Korece
- Lehçe
- Portekizce (Brezilya)
- Portekizce (Portekiz)
- Rusça
- İspanyolca
- İsveççe
- Türkçe

## <a name="next-steps"></a>Sonraki adımlar
Azure Media Clipper'ı kullanmaya başlamak için okuma [Başlarken](media-services-azure-media-clipper-getting-started.md) pencere dağıtma hakkında daha fazla ayrıntı için makaleyi.
