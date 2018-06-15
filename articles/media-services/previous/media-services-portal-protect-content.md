---
title: Azure portalını kullanarak içerik koruma ilkeleri yapılandırma | Microsoft Docs
description: Bu makalede Azure portal içerik koruma ilkelerini yapılandırmak için nasıl kullanılacağı gösterilmektedir. Makale ayrıca varlıklarınızı dinamik şifrelemeyi etkinleştirmek nasıl gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 8603716d30e1061ca9d600f2c053e90ff50c2433
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790387"
---
# <a name="configure-content-protection-policies-by-using-the-azure-portal"></a>Azure portalını kullanarak içerik koruma ilkeleri yapılandırma
 Azure Media Services ile depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdan güvenliğini sağlayabilirsiniz. Media Services, dinamik olarak Gelişmiş Şifreleme Standardı (AES ile) 128-bit şifreleme anahtarları kullanılarak şifrelenmiş içeriğinizi teslim etmek için kullanabilirsiniz. Sizin de PlayReady ve/veya Widevine dijital hak yönetimi (DRM) ve Apple FairPlay kullanarak ortak şifreleme (CENC) kullanabilirsiniz. 

AES anahtarları yetkili istemcilere temizleyin ve Media Services DRM lisansları teslim etmek için bir hizmet sağlar. Bir anahtar/lisans yetkilendirme ilkesi şifrelemeleri tüm türleri oluşturmak için Azure portalını kullanabilirsiniz.

Bu makalede, portal kullanarak içerik koruma İlkesi yapılandırmak gösterilmiştir. Makale ayrıca varlıklarınızı için dinamik şifreleme uygulamak nasıl gösterir.

## <a name="start-to-configure-content-protection"></a>İçerik koruma yapılandırmak için başlatın
Media Services hesabınızı kullanarak genel içerik koruma yapılandırmak için portalı kullanmak için aşağıdaki adımları uygulayın:

1. İçinde [portal](https://portal.azure.com/), Media Services hesabınızı seçin.

2. Seçin **ayarları** > **içerik koruma**.

    ![Content Protection](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Anahtar/lisans yetkilendirme ilkesi
Media Services anahtarını veya lisans isteğinde kullanıcıların kimliklerini doğrulama birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesini yapılandırmanız gerekir. Anahtar/lisans teslim edilebilir önce istemci İlkesi karşılaması gerekir. İçerik anahtarı yetkilendirme ilkesinin açık veya belirteç kısıtlaması şeklinde bir veya daha fazla yetkilendirme kısıtlaması olabilir.

Portal şifrelemeleri tüm türleri için bir anahtar/lisans yetkilendirme ilkesi oluşturmak için kullanabilirsiniz.

### <a name="open-authorization"></a>Açık yetkilendirme
Açık sınırlama Sistem anahtarı anahtar istekte herkes sunduğundan emin anlamına gelir. Bu kısıtlama, test amaçları için yararlı olabilir. 

### <a name="token-authorization"></a>Belirteci yetkilendirme
Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, basit bir web token (SWT) ve JSON Web Token (JWT) biçimlerinde belirteçleri destekler. Media Services bir STS sağlamaz. Özel bir STS oluşturun veya Azure erişim denetimi hizmeti sorunu belirteçleri kullanın. STS, belirteç kısıtlamasına yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Belirtecin geçerli olduğu ve anahtarı (veya lisans) için yapılandırılmış talep belirteci eşleştiğinden, Media Services anahtar teslim hizmeti istemciye istenen anahtarı (veya lisans) döndürür.

Belirteç kısıtlanmış İlkesi yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametreleri belirtmeniz gerekir. Birincil doğrulama anahtar belirteci ile imzalandığı anahtarı içerir. Verici belirteç veren güvenli bir belirteç hizmetidir. Kaynak belirteci erişimini yetkilendirir veya (bazen kapsam denir) İzleyici belirteç amacı açıklanır. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular.

![Anahtar/lisans yetkilendirme ilkesi](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>PlayReady lisans şablonu
PlayReady lisans şablonu etkin işlevselliği, PlayReady lisans ayarlar. PlayReady lisans şablonu hakkında daha fazla bilgi için bkz: [Media Services PlayReady lisans şablonu genel bakış](media-services-playready-license-template-overview.md).

### <a name="nonpersistent"></a>Kalıcı olmayan
Bir lisans kalıcı olmayan olarak yapılandırırsanız, yalnızca player lisans kullanırken bellekte tutulur.  

![Kalıcı olmayan içerik koruma](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Kalıcı
Bir lisans kalıcı olarak yapılandırırsanız, istemci üzerinde kalıcı depolama alanına kaydedilir.

![Kalıcı içerik koruma](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Widevine lisans şablonu
Widevine lisans şablonu etkin işlevselliği, Widevine lisansları ayarlar.

### <a name="basic"></a>Temel
Seçtiğinizde, **temel**, şablonu tüm varsayılan değerlerle oluşturulur.

### <a name="advanced"></a>Gelişmiş
Widevine haklar şablonu hakkında daha fazla bilgi için bkz: [Widevine lisans şablonu genel bakış](media-services-widevine-license-template-overview.md).

![Gelişmiş içerik koruma](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay yapılandırması
FairPlay şifrelemeyi etkinleştirmek için seçin **FairPlay yapılandırma**. Ardından **uygulaması sertifikası** ve girin **uygulama gizli anahtar**. FairPlay yapılandırma ve gereksinimleri hakkında daha fazla bilgi için bkz: [, HLS Apple FairPlay veya Microsoft PlayReady ile içerik korumak](media-services-protect-hls-with-FairPlay.md).

![FairPlay yapılandırması](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Varlık için dinamik şifreleme Uygula
Dinamik şifreleme yararlanmak için kaynak dosyanızı bit hızı Uyarlamalı MP4 dosyaları kümesine kodlayın.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Şifrelemek istediğiniz bir varlığı seçin
Tüm varlıklarınızı görmek için seçin **ayarları** > **varlıklar**.

![Varlıklar seçeneği](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>AES veya DRM ile şifreleme
Seçtiğinizde, **şifrele** bir varlık için iki seçenek görürsünüz: **AES** veya **DRM**. 

#### <a name="aes"></a>AES
AES anahtar şifreleme tüm akış protokolleri üzerinde etkin Temizle: kesintisiz akış, HLS ve MPEG-DASH.

![Şifreleme yapılandırması](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
1. Siz seçtikten sonra **DRM**, (Bu noktası tarafından yapılandırılması gerekir) farklı içerik koruma ilkeleri ve akış protokolleri kümesi bakın:

    a. **PlayReady ve Widevine MPEG-DASH ile** PlayReady ve Widevine DRMs MPEG-DASH akışınızı dinamik olarak şifreler.

    b. **PlayReady ve Widevine MPEG-DASH ile + FairPlay HLS ile** PlayReady ve Widevine DRMs MPEG-DASH akışınızı dinamik olarak şifreleyin. Bu seçenek ayrıca, HLS akış FairPlay ile şifreler.

    c. **PlayReady yalnızca kesintisiz akış, HLS ve MPEG-DASH ile** kesintisiz akış, HLS ve MPEG-DASH akışları PlayReady DRM ile dinamik olarak şifreler.

    d. **Yalnızca MPEG-DASH ile Widevine** MPEG-DASH Widevine DRM ile dinamik olarak şifreler.
    
    e. **FairPlay yalnızca HLS ile** FairPlay olan, HLS akış dinamik olarak şifreler.

2. FairPlay şifrelemeyi etkinleştirmek için **içerik koruma genel ayarları** dikey penceresinde, select **FairPlay yapılandırma**. Ardından **uygulaması sertifikası**ve girin **uygulama gizli anahtar**.

    ![Şifreleme türü](./media/media-services-portal-content-protection/media-services-content-protection009.png)

3. Şifreleme seçimi yaptıktan sonra seçin **Uygula**.

>[!NOTE] 
>Bir AES ile şifrelenmiş HLS Safari ile oynatmayı düşünüyorsanız, blog gönderisine bakın [şifrelenmiş HLS Safari'de](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

