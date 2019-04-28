---
title: Video oynatıcı uygulamaları geliştirme
description: Konu, oynatıcı çerçeveleri ve Media Services akış medyadan kullanabilecek kendi istemci uygulamalarınızı geliştirmek için kullanabileceğiniz eklentileri için bağlantılar sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 55e419fc-4c39-4902-9c62-f41cfcd86c6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: b8d4ff3e833dcbe92802845796e3b826735b68ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465652"
---
# <a name="develop-video-player-applications"></a>Video oynatıcı uygulamaları geliştirme
## <a name="overview"></a>Genel Bakış
Azure Media Services, şunlar dahil olmak üzere çoğu platform için zengin ve dinamik istemci oynatıcı uygulamaları oluştururken ihtiyacınız olan araçları sağlar: iOS Cihazları, Android Cihazları, Windows, Windows Phone, Xbox ve Alıcı kutuları. Bu konuda ayrıca SDK'lar ve oynatıcı çerçeveleri, Azure Media Services akış medyadan kullanabilecek kendi istemci uygulamalarınızı geliştirmek için kullanabileceğiniz bağlantılar sağlar.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 
 
## <a name="azure-media-player"></a>Azure Media Player
[Azure Media Player](https://aka.ms/ampinfo) olan medya içeriği çok çeşitli tarayıcılar ve cihazlar üzerinde Microsoft Azure Media Services'den kayıttan yürütmek için yerleşik bir web video oynatıcı. Azure Media Player, zenginleştirilmiş bir Uyarlamalı akış deneyimi sağlamak için HTML5, medya kaynağı Uzantıları (MSE) ve şifreli medya Uzantıları (EME) gibi endüstri standartlarından yararlanır. Bu standartlar, bir cihazda veya tarayıcıda mevcut olmadığı durumlarda, Azure Media Player, Flash ve Silverlight'a geri dönüş teknolojisi olarak kullanır. Kullanılan oynatma teknolojisinden bağımsız olarak geliştiriciler, API'lere birleştirilmiş bir JavaScript arabirimi gerekir. Bu Azure Media Services tarafından sunulan içerik için geniş aralığında cihazlar ve tarayıcılar herhangi ek bir çaba olmadan, yürütülecek sağlar.

İçerik yürütürken içerik DASH, kesintisiz akış ve HLS akış biçimlerinde sunulacak için Microsoft Azure Media Services sağlar. Azure Media Player, aşağıdaki biçimlerde hesaba katar ve platformuna/tarayıcı özelliklerine en iyi bağlantıyı otomatik olarak yürütülür. Microsoft Azure Media Services PlayReady şifrelemesi ile varlıkları dinamik şifreleme için de sağlar. veya Zarf şifreleme AES-128 bit. Azure Media Player PlayReady şifre çözme için izin verir ve uygun şekilde yapılandırıldığında şifrelenmiş içerik AES 128 bit. 

Daha fazla bilgi için:

* [Azure Media Player](https://aka.ms/ampinfo)
* [Azure Media Player belgeleri](https://aka.ms/ampdocs) 
* [Azure Media Player alma Blog başlatıldı](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
* [En son Azure Media Player ile güncel kalmak kaydolun](https://aka.ms/ampsignup)
* [Yeni özellik talebi, fikirleri, geri bildirim Ekle](https://aka.ms/ampuservoice) 

## <a name="other-tools-for-creating-player-applications"></a>Oynatıcı uygulamaları oluşturmaya yönelik diğer araçlar
Aşağıdaki Sdk'lardan birini kullanabilirsiniz:

* [Kesintisiz akış istemci SDK'sı](https://www.iis.net/downloads/microsoft/smooth-streaming) 
* [Kesintisiz akış Windows Store uygulaması](media-services-build-smooth-streaming-apps.md)
* [Microsoft Media platformu: Player çerçevesi](https://playerframework.codeplex.com/) 
* [HTML5 Player Framework belgeleri](https://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
* [Microsoft eklentisi akış OSMF için kesintisiz](https://www.microsoft.com/download/details.aspx?id=36057) 
* [Lisans Microsoft® kesintisiz akış istemci taşıma kitini](https://aka.ms/sspk) 
* [XBOX Video uygulaması geliştirme](https://xbox.create.msdn.com/) 

## <a name="advertising"></a>Reklam
Azure Media Services, Windows Media platformu aracılığıyla reklam ekleme için destek sağlar: Oynatıcı çerçeveleri. Ad desteğiyle oynatıcı çerçeveleri, Windows 8, Silverlight, Windows Phone 8 ve iOS cihazlar için kullanılabilir. Her player çerçevesi player uygulamasının nasıl uygulanacağını gösteren örnek kodunu içerir. Medyanızı ekleyebileceğiniz reklam üç farklı tür vardır:

Doğrusal – ana videoyu Duraklat tam çerçeve reklamları

Doğrusal – ana videoyu oynatmaya olarak görüntülenen katmana reklam, genellikle bir logo veya player içinde yerleştirilen diğer statik görüntü

Yardımcısı – dışında player görüntülenen reklamları

Reklam ana video zaman çizelgesi içinde herhangi bir noktada yerleştirilebilir. Ad yürütmek ne zaman ve hangi reklam yürütmek için player söylemeniz gerekir. Bu işlemi standart XML tabanlı dosyalar kümesi kullanarak: Video Ad hizmet şablonu (VAST), Dijital Video birden çok Ad çalma listesi (VMAP), medya sıralama şablonu (a) ve Dijital Video Oynatıcı Ad arabirim tanımı (VPAID) soyut. BÜYÜK dosyaları görüntülemek için hangi reklam belirtin. VMAP dosyaları çeşitli reklam yürütmek ve geniş XML içeren ne zaman belirtin. A dosyaları büyük XML de içerebilen dizisi reklam için başka bir yoludur. VPAID dosyaları, video oynatıcı ad veya ad sunucusu arasında bir arabirim tanımlar. Daha fazla bilgi için [reklam ekleme](https://msdn.microsoft.com/library/dn387398.aspx).

Kapalı Açıklamalı Altyazı ve canlı akış video de reklam desteği hakkında daha fazla bilgi için bkz: [desteklenen Kapalı Açıklamalı Altyazı ve Ad ekleme standartları](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[DASH.js ile bir MPEG-DASH Uyarlamalı Akış Videosunu bir HTML5 Uygulamasına ekleme](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js deposu](https://github.com/Dash-Industry-Forum/dash.js)

