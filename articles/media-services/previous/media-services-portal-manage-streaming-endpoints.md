---
title: Azure portal ile akış uç noktalarını yönetme | Microsoft Docs
description: Bu konu Azure portal ile akış uç noktalarını yönetme gösterir.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: cfowler
editor: ''
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: juliako
ms.openlocfilehash: 542780766cfa90026d5ff492fcf7b579cb2d7029
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790352"
---
# <a name="manage-streaming-endpoints-with-the-azure-portal"></a>Azure portal ile akış uç noktalarını yönetme

Bu makalede Azure portal akış uç noktalarını yönetmek için nasıl kullanılacağı gösterilmektedir. 

>[!NOTE]
>Gözden geçirdiğinizden emin olun [genel bakış](media-services-streaming-endpoints-overview.md) makalesi. 

Akış uç ölçeklendirme hakkında daha fazla bilgi için bkz: [bu](media-services-portal-scale-streaming-endpoints.md) makalesi.

## <a name="start-managing-streaming-endpoints"></a>Akış uç noktalarını yönetmeye başlama 

Hesabınız için akış uç noktalarını yönetmeye başlamak için aşağıdakileri yapın.

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. İçinde **ayarları** dikey penceresinde, select **akış uç noktaları**.
   
    ![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> Akış uç noktanızı çalışır durumda olduğunda yalnızca faturalandırılır.

## <a name="adddelete-a-streaming-endpoint"></a>Bir akış uç ekleme/silme

>[!NOTE]
>Varsayılan akış uç silinemiyor.

Azure portalı kullanarak akış uç noktasını ekleme/silme için aşağıdakileri yapın:

1. Akış uç noktası eklemek için tıklatın **+ uç nokta** sayfanın üst kısmındaki. 

    Farklı CDN'ler veya CDN ve doğrudan erişimi olmasını planlıyorsanız, birden çok akış uç noktaları isteyebilirsiniz.

2. Akış uç noktasını silmek için basın **silmek** düğmesi.      
3. Tıklatın **Başlat** akış uç başlamak için Başlat.
   
    ![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a id="configure_streaming_endpoints"></a>Akış uç noktasını yapılandırma
Akış uç noktası aşağıdaki özellikleri yapılandırmanıza olanak sağlar:

* Erişim denetimi
* Önbellek denetimi
* Site erişim ilkeleri arası

Bu özellikler hakkında ayrıntılı bilgi için bkz: [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

>[!NOTE]
>CDN etkinleştirildiğinde, IP erişim erişemiyor. IP erişim uygulanabilir için CDN yok.

Aşağıdakileri yaparak akış uç noktasını yapılandırabilirsiniz:

1. Yapılandırmak istediğiniz akış uç noktasını seçin.
2. Tıklatın **ayarları**.

Alanları kısa bir açıklamasını izler.

![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. En büyük önbellek İlkesi: Bu akış uç noktası aracılığıyla sunulan varlıklar için önbellek ömrünü yapılandırmak için kullanılır. Herhangi bir değer ayarlarsanız varsayılan değer kullanılır. Varsayılan değerleri de doğrudan Azure depolama alanında tanımlanabilir. Azure CDN akış uç noktası için etkinleştirilirse, önbellek İlkesi değeri 600 saniyeden daha kısa bir süre için ayarlamalısınız değil.  
2. İzin verilen IP adreslerini: yayımlanan akış uç noktasına bağlanmasına izin IP adresleri belirtmek için kullanılır. Belirtilen herhangi bir IP adresi varsa, herhangi bir IP adresi bağlanabilmesi. IP adresleri, tek bir IP adresi (örneğin, ' 10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi (örneğin, ' 10.0.0.1/22') kullanarak bir IP aralığı veya IP adresi ve noktalı ondalık alt ağ maskesi kullanarak bir IP aralığı belirtilebilir (örneğin, 10.0.0.1 ' () 255.255.255.0)').
3. Akamai imzası üstbilgi kimlik doğrulaması için yapılandırma: İmza üstbilgi kimlik doğrulama isteği Akamai sunucularından nasıl yapılandırıldığını belirtmek için kullanılır. Süre sonu UTC biçiminde değil.

## <a name="scale-your-premium-streaming-endpoint"></a>Akış uç noktası, Premium ölçeklendirme

Daha fazla bilgi için [bu makaleye](media-services-portal-scale-streaming-endpoints.md) bakın.

## <a id="enable_cdn"></a>Azure CDN tümleştirmeyi etkinleştir

Yeni bir hesap oluşturduğunuzda, varsayılan akış uç noktası Azure CDN tümleştirme varsayılan olarak etkindir.

Akış uç noktanızı olması gerekir, daha sonra devre dışı bırak/CDN etkinleştir istiyorsanız, **durduruldu** durumu. İki saate kadar etkin Azure CDN tümleştirme ve değişikliklerin tüm CDN POP etkin olması ele geçirebilir. Ancak, can akış uç noktası ve akış kesintileri olmadan akış uç noktasından başlatmak ve tümleştirme tamamlandıktan sonra akışı CDN teslim edilir. Akış uç noktanızı olacak sağlama süresi boyunca **başlangıç** durumu ve degredad performans gözlemlemek.

CDN tümleştirme Çin ve Federal Hükümet bölgeler dışında tüm Azure veri merkezlerinde etkin.

Etkinleştirildiğinde, **erişim denetimi**, ** özel ana bilgisayar adı, ve **Akamai imzası kimlik doğrulaması** yapılandırması devre dışı.
 
> [!IMPORTANT]
> Azure CDN ile Azure Media Services tümleştirme uygulandığını **verizon'dan Azure CDN** için standart akış uç noktaları. Akış uç noktaları premium tüm kullanılarak yapılandırılabilir **Azure CDN fiyatlandırma katmanlarına ve sağlayıcıları**. Azure CDN özellikler hakkında daha fazla bilgi için bkz: [CDN'ye genel bakış](../../cdn/cdn-overview.md).
 
### <a name="additional-considerations"></a>Diğer konular

* CDN akış uç noktası için etkinleştirildiğinde, istemcileri doğrudan kaynaktan içerik isteğinde bulunamaz. İçeriğinizi ile veya olmadan CDN test olanağı gerekiyorsa, CDN etkin olmayan başka bir akış uç noktası oluşturabilirsiniz.
* Akış uç noktası ana bilgisayar adı aynı CDN etkinleştirdikten sonra kalır. CDN etkinleştirildikten sonra media services iş akışınıza değişiklik gerekmez. Akış uç noktası ana bilgisayar adı strasbourg.streaming.mediaservices.windows.net, CDN etkinleştirdikten sonra Örneğin, tam aynı ana bilgisayar adı kullanılır.
* Yeni akış uç noktaları için yeni bir uç noktası oluşturarak CDN etkinleştirebilirsiniz; Varolan akış uç noktaları için ilk uç noktayı durdurmak ve ardından CDN bırakmak gerekir.
* Akış uç noktası standart yalnızca kullanılarak yapılandırılabilir **standart Verizon CDN sağlayıcısı** Klasik Azure portalını kullanarak. Ancak, REST API'lerini kullanarak diğer Azure CDN sağlayıcıları etkinleştirebilirsiniz.

## <a name="configure-cdn-profile"></a>CDN profili yapılandırma

CDN profili seçerek yapılandırabileceğiniz **yönetmek CDN** üstten düğme.

![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

