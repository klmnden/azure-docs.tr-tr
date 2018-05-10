---
title: Video oynatıcı uygulamaları geliştirme
description: Konu oynatıcı çerçevelerine ve Media Services akış medyadan tüketebileceği kendi istemci uygulamalarınızı geliştirmek için kullanabileceğiniz eklentileri bağlantılar sağlar.
author: Juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 55e419fc-4c39-4902-9c62-f41cfcd86c6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: 71d8b020fe96094c15965fd82615e3182e333990
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="develop-video-player-applications"></a>Video oynatıcı uygulamaları geliştirme
## <a name="overview"></a>Genel Bakış
Azure Media Services, şunlar dahil olmak üzere çoğu platform için zengin ve dinamik istemci oynatıcı uygulamaları oluştururken ihtiyacınız olan araçları sağlar: iOS Cihazları, Android Cihazları, Windows, Windows Phone, Xbox ve Alıcı kutuları. Bu konu aynı zamanda SDK'ları ve Azure Media Services akış medyadan tüketebileceği kendi istemci uygulamalarınızı geliştirmek için kullanabileceğiniz oynatıcı çerçevelerine bağlantılar sağlar.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 
 
## <a name="azure-media-player"></a>Azure Media Player
[Azure Media Player](http://aka.ms/ampinfo) olan medya içeriği çok çeşitli tarayıcılar ve cihazlar üzerinde Microsoft Azure Media Services'den çalmak için yerleşik bir web video oynatıcı. Azure Media Player, HTML5, medya kaynağı Uzantıları (MSE) ve şifrelenmiş medya Uzantıları (EME) bir zenginleştirilmiş Uyarlamalı akış deneyim sağlamak üzere gibi endüstri standartları kullanır. Bu standartlar bir aygıt veya bir tarayıcıda kullanılabilir olmadığında, Azure Media Player, Flash ve Silverlight temel teknoloji olarak kullanır. Kullanılan kayıttan yürütme teknolojiye bakılmaksızın, geliştiricilerin API'lere erişim için birleşik bir JavaScript arabirim sahip olur. Bu Azure Media Services tarafından sunulan içeriğin geniş aralık cihazların ve herhangi bir ek çaba olmayan tarayıcılar arasında çalınacak sağlar.

Microsoft Azure Media Services DASH, kesintisiz akış ve HLS akış biçimlerine ile sunulacak içerik için içerik yürütürken sağlar. Azure Media Player bu çeşitli biçimler dikkate alır ve platform/tarayıcı yetenekleri göre en iyi bağlantıyı otomatik olarak yürütülür. Microsoft Azure Media Services da dinamik varlıklar şifrelemeyle PlayReady şifreleme sağlar veya Zarf şifreleme AES-128 bit. PlayReady şifre çözme için Azure Media Player verir ve uygun şekilde yapılandırıldığında şifrelenmiş içerik AES-128 bit. 

Daha fazla bilgi için:

* [Azure Media Player](http://aka.ms/ampinfo)
* [Azure Media Player belgeleri](http://aka.ms/ampdocs) 
* [Blog azure Media Player alma başlatıldı](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
* [Azure Media Player en son bilgilerle güncel kalmak kaydolun](http://aka.ms/ampsignup)
* [Yeni özellik istekleri, fikirleri, geri bildirim ekleme](http://aka.ms/ampuservoice) 

## <a name="other-tools-for-creating-player-applications"></a>Oynatıcı uygulamaları oluşturmak için diğer araçları
Aşağıdaki Sdk'lardan birini kullanabilirsiniz:

* [Kesintisiz akış istemci SDK'sı](http://www.iis.net/downloads/microsoft/smooth-streaming) 
* [Kesintisiz akış Windows mağazası uygulaması](media-services-build-smooth-streaming-apps.md)
* [Microsoft ortam platformu: Player Framework](http://playerframework.codeplex.com/) 
* [HTML5 Oynatıcısının Framework belgelerine bakın](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
* [Microsoft için OSMF eklentisi akış kesintisiz](https://www.microsoft.com/download/details.aspx?id=36057) 
* [Kit taşıma lisanslama Microsoft® kesintisiz akış istemcisi](http://aka.ms/sspk) 
* [XBOX Video uygulama geliştirme](http://xbox.create.msdn.com/) 

## <a name="advertising"></a>Reklam
Azure Media Services, Windows ortam platformu aracılığıyla ad ekleme için destek sağlar: oynatıcı çerçevelerine. Ad desteğiyle oynatıcı çerçevelerine Windows 8, Silverlight, Windows Phone 8 ve iOS cihazları için kullanılabilir. Her player framework oynatıcı uygulaması uygulamak nasıl oluşturulduğunu gösteren örnek kodunu içerir. Medyanızı ekleyebilirsiniz ads üç farklı türde bulunmaktadır:

Doğrusal – ana videoyu duraklatmak tam çerçeve ads

Doğrusal – ana video yürütme gibi görüntülenen katmana reklam, genellikle bir logo veya player içinde yerleştirilen diğer statik görüntü

Yardımcı – dışında player görüntülenen reklamları

Reklam ana videonun Zaman Çizelgesi herhangi bir noktada yerleştirilebilir. Ad yürütmek ne zaman ve hangi ads yürütmek için player bildirmeniz gerekir. Bu yapılır bir dizi standart XML tabanlı dosyaları kullanılarak: Video Ad Hizmeti şablonu (VAST), Dijital Video birden çok Ad çalma listesi (VMAP), medya soyut sıralama şablonu (a) ve Dijital Video Oynatıcı Ad arabirim tanımı (VPAID). BÜYÜK dosyaları görüntülemek için hangi ads belirtin. VMAP dosyaları çeşitli reklam oynatın ve büyük XML içeren zamanı belirtin. A dosyaları da büyük XML içerebilen dizisi reklam için başka bir yoludur. VPAID dosyaları video oynatıcı ve ad veya ad sunucusu arasında bir arabirim tanımlar. Daha fazla bilgi için bkz: [ekleme Ads](https://msdn.microsoft.com/library/dn387398.aspx).

Kapalı açıklamalı alt yazı ve canlı akış videoları ads desteği hakkında daha fazla bilgi için bkz: [desteklenen alanında Kapalı açıklamalı alt yazı ve Ad ekleme standartları](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[DASH.js ile bir MPEG-DASH Uyarlamalı Akış Videosunu bir HTML5 Uygulamasına ekleme](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js deposu](https://github.com/Dash-Industry-Forum/dash.js)

