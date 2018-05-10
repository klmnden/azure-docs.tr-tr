---
title: Kayıttan yürütme için varolan oynatıcıları içeriğinizi - Azure kullanın | Microsoft Docs
description: Bu konu mevcut oynatıcıları listeler içeriğinizi kayıttan yürütmek üzere kullanabilirsiniz.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: juliako
ms.openlocfilehash: a64d32747371aab2bd927e53c5d43e30de9108f9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="playing-your-content-with-existing-players"></a>Varolan oyuncularla içeriğinizi oynatma
Azure Media Services, kesintisiz akış, HTTP canlı akış ve MPEG-Dash gibi birçok popüler akış biçimlerine destekler. Bu konu, akışları test etmek için kullanabileceğiniz varolan oynatıcıları işaret eder.

### <a name="the-azure-portal-media-services-content-player"></a>Azure portal medya Hizmetleri içerik Oynatıcısı
**Azure** portalı, videonuzu test etmek için kullanabileceğiniz bir içerik oynatıcı sağlar.

Üzerinde istediğiniz videoya tıklayın (, olduğundan emin olun [yayımlanan](media-services-portal-publish.md)) tıklatıp **Yürüt** portalın altındaki düğmesini.

Bazı dikkate alınması gereken noktalar vardır:

* **MEDYA HİZMETLERİ İÇERİK OYNATICISI** varsayılan akış uç noktasından oynatır. Varsayılan dışı bir akış uç noktasından oynatmak istiyorsanız başka bir oynatıcı kullanın. Örneğin, [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player
Kullanım [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) kayıttan yürütme için aşağıdaki biçimlerden birinde, içerik (düz veya korumalı):

* Kesintisiz Akış
* MPEG DASH
* HLS
* Aşamalı MP4

### <a name="flash-player"></a>Flash Player
#### <a name="aes-encrypted-with-token"></a>AES ile şifrelenmiş belirteci ile
[http://aestoken.azurewebsites.net](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a>Silverlight oynatıcıları
#### <a name="monitoring"></a>İzleme
[http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor)

#### <a name="playready-with-token"></a>PlayReady belirteci ile
[http://sltoken.azurewebsites.net](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>TİRE oynatıcıları
[http://dashplayer.azurewebsites.net](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

### <a name="other"></a>Diğer
HLS da URL'leri test etmek için:

* **Safari** bir iOS cihazına veya
* **3ivx HLS Player** Windows.

## <a name="developing-video-players"></a>Video oynatıcı geliştirme
Kendi oynatıcıları geliştirme hakkında daha fazla bilgi için bkz: [video oynatıcı geliştirme](media-services-develop-video-players.md)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
