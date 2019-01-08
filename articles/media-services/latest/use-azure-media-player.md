---
title: Azure Media Player - Azure ile kayıttan yürütme | Microsoft Docs
description: Bu konu Azure Media Player genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/03/2018
ms.author: juliako
ms.openlocfilehash: 4dd0f697d16d7a2fbeec8f712d98e6a7c439cb4e
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064687"
---
# <a name="azure-media-player-overview"></a>Azure Media Player genel bakış

Azure Media Player, Microsoft Azure Media Services'den medya içeriği çok çeşitli tarayıcılar ve cihazlar üzerinde yürütme işlemi yerleşik web video oynatıcı ' dir. Azure Media Player, zenginleştirilmiş bir Uyarlamalı akış deneyimi sağlamak için HTML5, medya kaynağı Uzantıları (MSE) ve şifreli medya Uzantıları (EME) gibi endüstri standartlarından yararlanır. Bu standartlar, bir cihazda veya tarayıcıda mevcut olmadığı durumlarda, Azure Media Player, Flash ve Silverlight'a geri dönüş teknolojisi olarak kullanır. Kullanılan oynatma teknolojisinden bağımsız olarak geliştiriciler, API'lere birleştirilmiş bir JavaScript arabirimi gerekir. Bu Azure Media Services tarafından sunulan içerik için geniş aralığında cihazlar ve tarayıcılar herhangi ek bir çaba olmadan, yürütülecek sağlar.

Microsoft Azure Media Services içerik yürütürken HLS, DASH, kesintisiz akış biçimlerinde akış ile sunulan için içerik sağlar. Azure Media Player, aşağıdaki biçimlerde hesaba katar ve platformuna/tarayıcı özelliklerine en iyi bağlantıyı otomatik olarak yürütülür. Media Services PlayReady şifrelemesi ile varlıkları dinamik şifreleme için de tanır veya Zarf şifreleme AES-128 bit. Azure Media Player PlayReady şifre çözme için izin verir ve uygun şekilde yapılandırıldığında şifrelenmiş içerik AES 128 bit. 

[Ücretsiz denemenizi başlatın](http://azure.microsoft.com/en-us/pricing/free-trial/)

## <a name="use-azure-media-player-demo-page"></a>Azure Media Player tanıtım sayfasını kullanın

### <a name="start-using"></a>Kullanmaya başlayın

Kullanabileceğiniz [Azure Media Player tanıtımını sayfa](http://aka.ms/amp) Azure Media Services örnekleri veya kendi akış yürütülecek.  

Yeni bir videoyu oynatmak için farklı bir URL ve ENTER tuşuna yapıştırın **güncelleştirme**.

(Örneğin, teknoloji, dil veya şifreleme) çeşitli kayıttan yürütme seçeneklerini yapılandırmak için basın **Gelişmiş Seçenekler**.

![Azure Media Player](./media/azure-media-player/home-page.png)

### <a name="monitor-diagnostics-of-a-video-stream"></a>Video akışının Tanılama izleme

Kullanabileceğiniz [Azure Media Player tanıtımını sayfa](http://aka.ms/amp) tanılama video akışının izlemek için. 

![Azure Media Player tanılama](./media/azure-media-player/diagnostics.png)

## <a name="set-up-azure-media-player-in-your-html"></a>Azure Media Player, HTML ayarlama

Azure Media Player, ayarlamak kolay bir işlemdir. Yalnızca Media Services hesabınızdan medya içeriklerinin temel kayıttan yürütme almak için birkaç dakika sürer. Bkz: [Azure Media Player belgeleri](https://aka.ms/ampdocs) ayarlama ve Azure Media Player'ı yapılandırma hakkında ayrıntılı bilgi için. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Media Player belgeleri](https://aka.ms/ampdocs)
- [Azure Media Player örnekleri](https://aka.ms/ampsamples)
