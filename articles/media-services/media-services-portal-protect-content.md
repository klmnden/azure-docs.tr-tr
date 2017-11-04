---
title: "Azure portalını kullanarak içerik koruma ilkelerini yapılandırma | Microsoft Docs"
description: "Bu makalede Azure portal içerik koruma ilkelerini yapılandırmak için nasıl kullanılacağı gösterilmektedir. Makale ayrıca varlıklarınızı dinamik şifrelemeyi etkinleştirmek nasıl gösterir."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 67b3fa9936daebeafb7e87fe3a7b0c7e0105b3b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Azure portalını kullanarak içerik koruma ilkelerini yapılandırma
> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
> 
> 

## <a name="overview"></a>Genel Bakış
Microsoft Azure Media Services (AMS), depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdan güvenli olanak sağlar. Media Services göndermenizi sağlayan içeriğinizi Gelişmiş Şifreleme Standardı ((128-bit şifreleme anahtarları kullanılarak) AES ile), PlayReady ve/veya Widevine DRM ve Apple FairPlay kullanarak ortak şifreleme (CENC) dinamik olarak şifrelenmiş. 

AMS DRM lisansları teslim etmek için bir hizmet sunar ve AES anahtarları yetkili istemcilere temizleyin. Azure portalında bir oluşturmanızı sağlayan **anahtarı/lisans yetkilendirme ilkesi** şifrelemeleri tüm türleri.

Bu makalede Azure portalıyla içerik koruma ilkeleri yapılandırma gösterilmektedir. Makale ayrıca varlıklarınızı için dinamik şifreleme uygulamak nasıl gösterir.


> [!NOTE]
> Koruma ilkeleri oluşturmak için Klasik Azure portalı kullandıysanız, ilkeleri görüntülenmeyebilir [Azure portal](https://portal.azure.com/). Ancak, tüm eski ilkeleri hala mevcut. Azure Media Services .NET SDK kullanarak bunları inceleyin veya [Azure-Media-Services-Gezgini](https://github.com/Azure/Azure-Media-Services-Explorer/releases) Aracı (varlık sağ tıklatın ilkeleri görmek için görüntüleme bilgileri (F4) -> -> içerik anahtarlar sekmesini tıklatın -> tıklatın Aç anahtar). 
> 
> Yeni ilkeleri kullanarak, varlık şifrelemek isterseniz, Azure portalıyla yapılandırmak, Kaydet'i tıklatın ve dinamik şifreleme yeniden uygulayın. 
> 
> 

## <a name="start-configuring-content-protection"></a>İçerik koruma yapılandırma Başlat
İçerik koruma, AMS hesabınızı genel yapılandırmaya başlamak için portalı kullanmak için aşağıdakileri yapın:

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. Seçin **ayarları** > **içerik koruma**.

![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Anahtar/lisans yetkilendirme ilkesi
AMS anahtarını veya lisans isteğinde kullanıcıların kimliklerini doğrulama birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesinin tarafınızdan yapılandırılması ve istemciye delived için istemci anahtarı/lisans sırayla tarafından karşılanır. İçerik anahtarı yetkilendirme ilkesini bir veya daha fazla yetkilendirme kısıtlamaları olabilir: **açmak** veya **belirteci** kısıtlama.

Azure portalında bir oluşturmanızı sağlayan **anahtarı/lisans yetkilendirme ilkesi** şifrelemeleri tüm türleri.

### <a name="open"></a>Açık
Açık sınırlama Sistem anahtarı anahtar istekte herkes için teslim eder, anlamına gelir. Bu kısıtlama, test amaçları için yararlı olabilir. 

### <a name="token"></a>Belirteç
Belirteç kısıtlamalı ilkenin beraberinde bir Güvenli Belirteç Hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, basit Web belirteçleri (SWT) biçimini ve JSON Web Token (JWT) biçimlerindeki belirteçleri destekler. Media Services, güvenli belirteç hizmetleri sağlamaz. Özel bir STS oluşturabilir veya Microsoft Azure ACS sorunu belirteçleri yararlanın. STS, belirteç kısıtlamasına yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar teslim hizmeti istenen anahtarı (veya lisans) istemcinin belirtecin geçerli olup olmadığını ve bu belirteci yapılandırmış anahtarı (veya lisans) için talepleri döndürür.

Belirteç yapılandırma İlkesi sınırlı olduğunda, birincil doğrulama anahtarı, veren ve İzleyici parametreleri belirtmeniz gerekir. Birincil doğrulama anahtar belirteci ile imzalandığı anahtarı içerir, veren belirtecini veren güvenli belirteç hizmeti. Kaynak belirteci erişimini yetkilendirir veya (bazen kapsam denir) İzleyici belirteç amacı açıklanır. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular.

![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady haklar şablonu
PlayReady haklar şablonu hakkında ayrıntılı bilgi için bkz: [Media Services PlayReady lisans şablonu genel bakış](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Kalıcı olmayan
Lisans kalıcı olarak yapılandırırsanız, player lisans kullanırken, yalnızca bellekte tutulur.  

![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Kalıcı
Lisans kalıcı olarak yapılandırırsanız, istemci üzerinde kalıcı depolama alanına kaydedilir.

![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine haklar şablonu
Widevine haklar şablonu hakkında ayrıntılı bilgi için bkz: [Widevine lisans şablonu genel bakış](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Temel
Seçtiğinizde, **temel**, şablonu tüm varsayılanları değerlerle oluşturulur.

### <a name="advanced"></a>Gelişmiş
Widevine yapılandırmalarının öncelikli seçeneği hakkında ayrıntılı açıklamalar için bkz: [bu](media-services-widevine-license-template-overview.md) konu.

![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay yapılandırma
FairPlay şifrelemeyi etkinleştirmek için uygulama sertifika ve uygulama gizli anahtarı (İSTEYİN) FairPlay yapılandırma seçeneği sağlamanız gerekir. FairPlay yapılandırması ve gereksinimleri hakkında ayrıntılı bilgi için bkz: [bu](media-services-protect-hls-with-fairplay.md) makalesi.

![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Varlık için dinamik şifreleme Uygula
Dinamik şifreleme yararlanmak için kaynak dosyanızı bit hızı Uyarlamalı MP4 dosyaları kümesine kodlayın gerekir.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Şifrelemek istediğiniz bir varlığı seçin
Tüm varlıklarınızı görmek için seçin **ayarları** > **varlıklar**.

![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>AES veya DRM ile şifreleme
Tuşuna sonra **şifrele** bir varlık üzerinde iki seçenek sunulur: **AES** veya **DRM**. 

#### <a name="aes"></a>AES
AES anahtar şifrelemesi tüm akış protokollerine etkinleştirilecek Temizle: kesintisiz akış, HLS ve MPEG-DASH.

![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
DRM sekmesini seçin, içerik koruma ilkelerinin farklı seçenek sunulur (hangi artık yapılandırmış olmanız gerekir) + akış protokolleri kümesi.

* **PlayReady ve Widevine MPEG-DASH ile** -PlayReady ve Widevine DRMs MPEG-DASH akışınızı dinamik olarak şifreler.
* **PlayReady ve Widevine MPEG-DASH ile + HLS ile FairPlay** -PlayReady ve Widevine DRMs olan MPEG-DASH akış dinamik olarak şifreler. Ayrıca, HLS akış FairPlay ile şifreler.
* **PlayReady yalnızca kesintisiz akış, HLS ve MPEG-DASH ile** -kesintisiz akış, HLS, MPEG-DASH akışları PlayReady DRM ile dinamik olarak şifreler.
* **Yalnızca MPEG-DASH ile Widevine** -, MPEG-DASH Widevine DRM ile dinamik olarak şifreler.
* **FairPlay yalnızca HLS ile** -FairPlay olan, HLS akış dinamik olarak şifreler.

FairPlay şifrelemesini etkinleştirmek için içerik koruma ayarlar dikey FairPlay yapılandırma seçeneği uygulama sertifika ve uygulama gizli anahtarı (İSTEYİN) sağlamanız gerekir.

![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Şifreleme seçimi yaptıktan sonra basın **Uygula**.

>[!NOTE] 
>Yürüt planlıyorsanız, bir AES HLS Safari'de şifrelenmiş bkz [bu blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

