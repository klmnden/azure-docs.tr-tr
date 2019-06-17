---
title: Azure portal ile akış uç noktalarını yönetme | Microsoft Docs
description: Bu konuda, Azure portal ile akış uç noktalarını yönetme gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: femila
editor: ''
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 1775bbb2913f6b1a985ca7ec9e89bafed42fd0e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61129737"
---
# <a name="manage-streaming-endpoints-with-the-azure-portal"></a>Azure portal ile akış uç noktalarını yönetme 

Bu makalede, akış uç noktaları yönetmek için Azure portalını kullanma gösterilmektedir. 

>[!NOTE]
>Gözden geçirdiğinizden emin olun [genel bakış](media-services-streaming-endpoints-overview.md) makalesi. 

Akış uç noktasını ölçeklendirme hakkında daha fazla bilgi için bkz. [bu](media-services-portal-scale-streaming-endpoints.md) makalesi.

## <a name="start-managing-streaming-endpoints"></a>Akış uç noktalarını yönetmeye başlama 

Hesabınız için akış uç noktalarını yönetmeye başlamak için aşağıdakileri yapın.

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. İçinde **ayarları** dikey penceresinde **akış uç noktaları**.
   
    ![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> Akış uç noktanızı çalışır durumda olduğunda yalnızca faturalandırılırsınız.

## <a name="adddelete-a-streaming-endpoint"></a>Bir akış uç noktası ekleme/silme

>[!NOTE]
>Varsayılan akış uç noktası silinemiyor.

Azure portalını kullanarak akış uç noktası ekleme/silme için aşağıdakileri yapın:

1. Bir akış uç noktası eklemek için tıklatın **+ uç nokta** sayfanın üstünde. 

    Farklı CDN'ler veya CDN ve doğrudan erişimi olmasını planlıyorsanız, birden çok akış uç noktalarını isteyebilirsiniz.

2. Bir akış uç noktasını silmek için basın **Sil** düğmesi.      
3. Tıklayın **Başlat** düğmesini akış uç noktasını başlatın.
   
    ![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a id="configure_streaming_endpoints"></a>Akış uç noktasını yapılandırma
Akış uç noktası aşağıdaki özellikleri yapılandırmak aşağıdakileri sağlar:

* Erişim denetimi
* Önbellek denetimi
* Çoklu site erişim ilkeleri

Bu özellikler hakkında ayrıntılı bilgi için bkz. [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

>[!NOTE]
>CDN etkinleştirildiğinde, IP access erişemiyor. IP erişim, yalnızca CDN olmadığında geçerlidir.

Aşağıdakileri yaparak akış uç noktası yapılandırabilirsiniz:

1. Yapılandırmak istediğiniz akış uç noktasını seçin.
2. Tıklayın **ayarları**.

Alanları kısa bir açıklamasını izler.

![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. En yüksek önbellek İlkesi: Bu akış uç noktası sunulan varlıkların önbellek ömrünü yapılandırmak için kullanılır. Hiçbir değer olarak ayarlanırsa varsayılan değer kullanılır. Varsayılan değerleri, doğrudan Azure depolama alanında da tanımlanabilir. Akış uç noktası için Azure CDN etkinleştirildiğinde önbellek İlkesi değeri 600 saniyeden kısa bir süre için ayarlanmamalıdır.  
2. İzin verilen IP adresleri: yayımlanan akış uç noktasını bağlamak için izin verilecek IP adresleri belirtmek için kullanılır. Belirtilen IP adresleri, herhangi bir IP adresi bağlanmanız mümkün olacaktır. IP adresleri, tek bir IP adresi (örneğin, ' 10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi (örneğin, ' 10.0.0.1/22') kullanarak bir IP aralığı veya IP adresi ve noktalı ondalık alt ağ maskesi kullanarak bir IP aralığı belirtilebilir (örneğin, 10.0.0.1 ' () 255.255.255.0)').
3. Akamai imza üst bilgisi kimlik doğrulaması için yapılandırma: Akamai sunucularından gelen imza üst bilgisi kimlik doğrulama isteği nasıl yapılandırıldığını belirtmek için kullanılır. Süre sonu UTC biçiminde olur.

## <a name="scale-your-premium-streaming-endpoint"></a>Premium akış uç noktası ölçeklendirin

Daha fazla bilgi için [bu makaleye](media-services-portal-scale-streaming-endpoints.md) bakın.

## <a id="enable_cdn"></a>Azure CDN tümleştirmesini etkinleştirme

Varsayılan akış uç noktası Azure CDN tümleştirmesi, yeni bir hesap oluşturduğunuzda, varsayılan olarak etkindir.

Daha sonra devre dışı bırak / CDN kullanılabilir hale getirmek isterseniz, akış uç noktanızı olmalıdır **durduruldu** durumu. Bu iki saate kadar etkin Azure CDN tümleştirmesi ve değişikliklerin tüm CDN POP'larına arasında etkin olması ele geçirebilir. Ancak, kullanabilirsiniz akış uç noktası ve kesintileri olmadan akışı akış uç noktasından Başlat ve tümleştirme tamamlandıktan sonra akışı CDN'den sağlanır. Akış uç noktanızı olacaktır sağlama süresi boyunca **başlangıç** durumu ve performans gözlemleyin.

CDN tümleştirmesi, Çin ve Federal devlet bölgeler dışındaki tüm Azure veri merkezlerinde etkinleştirilir.

Etkinleştirildiğinde, **erişim denetimi**, ** özel ana bilgisayar adı, ve **Akamai imza kimlik doğrulaması** yapılandırması devre dışı.
 
> [!IMPORTANT]
> Azure CDN ile Azure Media Services tümleştirmesi uygulandığını **verizon'dan Azure CDN** standart akış uç noktaları. Premium akış uç noktaları kullanarak tüm yapılandırılabilir **Azure CDN fiyatlandırma katmanları ve sağlayıcıları**. Azure CDN özellikleri hakkında daha fazla bilgi için bkz: [CDN'ye genel bakış](../../cdn/cdn-overview.md).
 
### <a name="additional-considerations"></a>Diğer konular

* CDN akış uç noktası için etkinleştirildiğinde, istemcileri doğrudan kaynaktan alınan içerik isteğinde bulunamazsınız. İçeriğinizi ile veya olmadan CDN test etme olanağı gerekiyorsa, CDN etkin olmayan başka bir akış uç noktası oluşturabilirsiniz.
* CDN etkinleştirdikten sonra akış uç noktası ana bilgisayar adı aynı kalır. CDN etkinleştirildikten sonra medya Hizmetleri iş akışınıza herhangi bir değişiklik yapmanız gerekmez. Örneğin, akış uç noktası ana bilgisayar adı, CDN etkinleştirdikten sonra strasbourg.streaming.mediaservices.windows.net ise, tam aynı ana bilgisayar adı kullanılır.
* Yeni akış uç noktaları için yeni bir uç noktası oluşturarak CDN etkinleştirebilirsiniz; Mevcut akış uç noktaları için ilk uç noktayı durdurmak ve ardından etkinleştir/devre CDN gerekir.
* Standart akış uç noktası yalnızca yapılandırılabilir **standart Verizon CDN sağlayıcısı** Klasik Azure portalını kullanarak. Ancak, REST API'lerini kullanarak diğer Azure CDN sağlayıcıları etkinleştirebilirsiniz.

## <a name="configure-cdn-profile"></a>CDN profili yapılandırma

CDN profili seçerek yapılandırabileceğiniz **yönetme CDN** üstten düğme.

![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

