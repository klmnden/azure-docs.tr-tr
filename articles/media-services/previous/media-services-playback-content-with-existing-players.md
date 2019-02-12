---
title: Mevcut oynatıcılarda oynatma için içeriğinizi - Azure kullanın | Microsoft Docs
description: Bu konu, mevcut oynatıcılarda listeler içeriğinizi kayıttan yürütmek üzere kullanabilirsiniz.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: ae057feb70d9ddb2c2dae10bae20eca1b613f76f
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55996979"
---
# <a name="playing-your-content-with-existing-players"></a>İçeriğinizi mevcut oynatıcılarda oynatma
Azure Media Services, kesintisiz akış, HTTP canlı akış ve MPEG-Dash gibi birçok popüler akış biçimlerinde destekler. Bu konuda, akışlarınız test etmek için kullanabileceğiniz mevcut oynatıcılarda işaret eder.

### <a name="the-azure-portal-media-services-content-player"></a>Azure portal medya Hizmetleri içerik Oynatıcısı
**Azure** portal, videonuzu test etmek için kullanabileceğiniz bir içerik oynatıcı sağlar.

İstediğiniz videoya tıklayın (, olduğundan emin olun [yayımlanan](media-services-portal-publish.md)) tıklayıp **Play** portalın alt kısmındaki düğmesi.

Bazı dikkate alınması gereken noktalar vardır:

* **MEDYA HİZMETLERİ İÇERİK OYNATICISI** varsayılan akış uç noktasından oynatır. Varsayılan dışı bir akış uç noktasından oynatmak istiyorsanız başka bir oynatıcı kullanın. Örneğin, [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player
Kullanım [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) kayıttan yürütme için aşağıdaki biçimlerden birinde içerik (açık ya da korumalı):

* Kesintisiz Akış
* MPEG DASH
* HLS
* Aşamalı MP4

### <a name="flash-player"></a>Flash Player
#### <a name="aes-encrypted-with-token"></a>AES şifreli belirteciyle
[http://aestoken.azurewebsites.net](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a>Silverlight oynatıcıları

#### <a name="playready-with-token"></a>Belirteci ile PlayReady
[http://sltoken.azurewebsites.net](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>DASH oynatıcılar
[http://dashplayer.azurewebsites.net](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

### <a name="other"></a>Diğer
HLS ayrıca URL'leri test etmek için:

* **Safari** bir iOS cihazında veya
* **3ivx HLS Player** Windows üzerinde.

## <a name="developing-video-players"></a>Video oynatıcı geliştirme
Oyuncuların kendi geliştirme hakkında daha fazla bilgi için bkz: [video oynatıcılar geliştirme](media-services-develop-video-players.md)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
