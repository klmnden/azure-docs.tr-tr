---
title: "İş ortakları için Azure Media Services Widevine lisansları teslim kullanmayı | Microsoft Docs"
description: "Bu makalede, PlayReady ve Widevine DRMs AMS tarafından dinamik olarak şifrelenmiş bir akış sağlamak üzere Azure Media Services (AMS) nasıl kullanabileceğinizi açıklar. Medya Hizmetleri PlayReady lisans sunucusundan PlayReady lisans gelir ve castLabs lisans sunucusu tarafından Widevine lisans teslim edilir."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5bcad5a4-c0bb-4871-9cce-808a913c53e6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 6867e4f910970121df3858516c6bab3114c3c6f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Azure Media Services’ta Widevine lisansları vermek için iş ortakları kullanma
## <a name="overview"></a>Genel Bakış
Microsoft Azure Media Services ile ortak şifreleme (CENC) belirtimi uyarınca şifrelenir Widevine DRM korumalı MPEG-DASH teslim etmenizi sağlar.

Media Services .NET SDK sürüm 3.5.2'de ile başlayarak, Media Services, Widevine lisans şablonu yapılandırmak ve Widevine lisansı alabilmeniz sağlar. Widevine lisansları teslim etmenize yardımcı olması için şu AMS ortaklarını da kullanabilirsiniz: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

## <a name="castlabs"></a>castLabs
Kullanabileceğiniz [castLabs](http://castlabs.com/company/partners/azure/) Widevine lisansları teslim etmek için. Daha fazla bilgi için bkz: [DRM sunmak için castLabs kullanarak Azure Media Services lisansları](media-services-castlabs-integration.md)

## <a name="axinom"></a>Axinom
Kullanabileceğiniz [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) Widevine lisansları teslim etmek için. Daha fazla bilgi için bkz: [DRM sunmak için Axinom kullanarak Azure Media Services lisansları](media-services-axinom-integration.md)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca bkz.
[PlayReady ve/ya Widevine dinamik ortak şifreleme işlemini kullanma](media-services-protect-with-drm.md)

[Mingfei'nın blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

