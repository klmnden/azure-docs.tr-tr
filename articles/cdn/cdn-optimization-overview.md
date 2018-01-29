---
title: "Azure içerik teslim senaryonuz için en iyi duruma getirme"
description: "İçeriğinizi belirli senaryoları için teslimini en iyi duruma getirme"
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: rli
ms.openlocfilehash: fa4cf306eeb1a8372da5b2a86ac73fbba2832666
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Azure içerik teslim senaryonuz için en iyi duruma getirme

İçerik için geniş global kullanıcılara iletirken, içeriğinizi en iyi duruma getirilmiş teslimini sağlamak için önemlidir. Azure içerik teslim ağı, sahip olduğunuz içerik türüne göre teslim deneyimi en iyi duruma getirebilirsiniz. Bir Web sitesi, bir canlı akış, bir video veya büyük bir dosya yüklemek için içerik olabilir. Bir içerik teslim ağı (CDN) uç noktası oluştururken bir senaryoda belirtmeniz **için en iyi duruma getirilmiş** seçeneği. Tercih ettiğiniz CDN uç noktasından teslim içerik için hangi iyileştirme uygulandığını belirler.

En iyi duruma getirme seçenekleri içerik teslim performansını artırmak için en iyi yöntem davranışları kullanacak şekilde tasarlanmıştır ve daha iyi kaynak boşaltabilirsiniz. Senaryo seçimlerinizi kısmi önbelleğe alma, nesne Öbekleme ve Kaynak başarısızlığı yeniden deneme ilkesi için yapılandırmaları değiştirerek performansını etkiler. 

Bu makale, çeşitli en iyi duruma getirme özellikleri ve ne zaman kullanmanız gerektiğine genel bakış sağlar. Özellikleri ve sınırlamaları hakkında daha fazla bilgi için her tek tek en iyi duruma getirme türü ilgili makalelerine bakın.

> [!NOTE]
> **İçin en iyi duruma getirilmiş** seçenekleri seçtiğiniz sağlayıcı göre değişir. CDN sağlayıcıları geliştirme senaryoya bağlı olarak, farklı şekillerde uygulayın. 

## <a name="provider-options"></a>Sağlayıcı seçenekleri

**Akamai'den Azure içerik teslim ağı** aşağıdaki iyileştirmeleri destekler:

* [Genel web teslim](#general-web-delivery) 

* [Genel medya](#general-media-streaming)

* [İsteğe bağlı video medya](#video-on-demand-media-streaming)

* [Büyük dosya indirme](#large-file-download)

* [Dinamik site hızlandırma](#dynamic-site-acceleration) 

**Verizon'dan Azure içerik teslim ağı** aşağıdaki iyileştirmeleri destekler:

* [Genel web teslim](#general-web-delivery) (Ayrıca medya akış ve büyük dosya indirme içerik için kullanılabilir)

* [Dinamik site hızlandırma](#dynamic-site-acceleration) 

Performans Çeşitlemeler, teslimat için en iyi sağlayıcısı seçmek için farklı sağlayıcılar arasında test öneririz.

## <a name="select-and-configure-optimization-types"></a>Seçin ve en iyi duruma getirme türlerini yapılandır

Yeni bir uç noktası oluşturmak için senaryo en iyi şekilde eşleşen bir en iyi duruma getirme türü ve sunmak için uç nokta istediğiniz içerik türü seçin. **Genel web teslim** varsayılan seçimdir. Mevcut **akamai'den Azure içerik teslim ağı** uç noktaları, en iyi duruma getirme seçeneği dilediğiniz zaman güncelleştirebilirsiniz. Bu değişiklik CDN teslim kesme değil. 

1. İçinde bir **akamai'den Azure içerik teslim ağı** profil, bir uç nokta seçin.

    ![Uç nokta seçimi ](./media/cdn-optimization-overview/01_Akamai.png)

2. Altında **ayarları**seçin **en iyi duruma getirme**. Ardından, bir tür seçin **için en iyi duruma getirilmiş** aşağı açılan liste.

    ![En iyi duruma getirme ve türü seçimi](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Belirli senaryolar için en iyi duruma getirme

CDN uç noktası Bu senaryolardan biri için en iyi duruma getirebilirsiniz. 

### <a name="general-web-delivery"></a>Genel web teslimatı

Genel web teslim en yaygın iyileştirme seçenektir. Web sayfaları ve web uygulamaları gibi genel web içerik iyileştirme için tasarlanmıştır. Bu iyileştirme dosyası için de kullanılabilir ve video indirir.

Tipik bir Web sitesi dinamik ve statik içeriği içerir. Statik içerik görüntüleri, JavaScript kitaplıklarını ve önbelleğe alınmış ve farklı kullanıcılara teslim stil sayfaları içerir. Dinamik içerik, bir kullanıcı profili için uyarlanmış haber öğeleri gibi tek bir kullanıcı için kişiselleştirilmiş. Alışveriş sepeti içeriği gibi her bir kullanıcı için benzersiz olduğundan dinamik içerik önbelleğe değil. Genel web teslim, Web sitenizin tamamını en iyi duruma getirebilirsiniz. 

> [!NOTE]
> Kullanırsanız **akamai'den Azure içerik teslim ağı**, bu en iyi duruma getirme, ortalama dosya boyutu 10 MB'den küçük ise kullanmak isteyebilirsiniz. Ortalama dosya boyutu 10 MB'den büyük ise, seçin **büyük dosya indirme** gelen **için en iyi duruma getirilmiş** aşağı açılan liste.

### <a name="general-media-streaming"></a>Genel medya akışı

Canlı akış ve isteğe bağlı video akış için uç nokta kullanmanız gerekiyorsa, en iyi duruma getirme Genel medya öneririz.

İstemcide geç gelen paketler sık video içeriği arabelleğe alma gibi ek olarak, düzeyi düşürülmüş görüntüleme deneyimini neden olabileceğinden Media akışı hassas, saattir. En iyi duruma getirme akış medya medya içerik teslim gecikmesini azaltır ve kullanıcılar için kesintisiz bir akış deneyimi sağlar. 

Bu senaryo, Azure medya hizmeti müşteriler için yaygın bir durumdur. Azure media Services'i kullanırken, canlı ve isteğe bağlı akış için kullanılan bir akış uç noktasını alın. Bu senaryo ile müşteriler bunlar Canlı isteğe bağlı Akış değiştirdiğinizde, başka bir uç noktasına geçiş gerekmez. Genel ortam akış en iyi duruma getirme, bu tür senaryosu destekler.

**Verizon'dan Azure içerik teslim ağı** akış medya içeriği teslim etmek için genel web teslim en iyi duruma getirme türünü kullanır.

Medya akış en iyi duruma getirme hakkında daha fazla bilgi için bkz: [en iyi duruma getirme medya](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>İsteğe bağlı video medya

İsteğe bağlı video medya akış iyileştirme isteğe bağlı video akış içeriği artırır. İsteğe bağlı video akış için bir uç nokta kullanırsanız, bu seçeneği kullanmak isteyebilirsiniz.

**Verizon'dan Azure içerik teslim ağı** akış medya içeriği teslim etmek için genel web teslim en iyi duruma getirme türünü kullanır.

Medya akış en iyi duruma getirme hakkında daha fazla bilgi için bkz: [en iyi duruma getirme medya](cdn-media-streaming-optimization.md).

> [!NOTE]
> Uç nokta isteğe bağlı video içeriği öncelikle hizmet veriyorsa, bu en iyi duruma getirme türünü kullanın. Bu en iyi duruma getirme ve en iyi duruma getirme Genel medya arasındaki en önemli fark, bağlantı yeniden deneme zaman aşımı ' dir. Zaman aşımı canlı akış senaryoları ile çalışmak için daha kısadır.

### <a name="large-file-download"></a>Büyük dosya indirme

Kullanırsanız **akamai'den Azure içerik teslim ağı**, büyük dosya indirme 1,8 GB'den büyük dosyalar sunmak için kullanmanız gerekir. **Verizon'dan Azure içerik teslim ağı** dosya indirme boyutu, genel web teslim iyileştirmeden bir sınırlama yoktur.

Kullanırsanız **akamai'den Azure içerik teslim ağı**, büyük dosya yüklemeleri iyileştirilmiş içeriği 10 MB'tan büyük. Ortalama dosya boyutu 10 MB'den daha küçükse, genel web teslim kullanmak isteyebilirsiniz. Ortalama dosyaları boyutları tutarlı bir şekilde 10 MB'den büyük olursa, büyük dosyalar için ayrı bir uç noktası oluşturmak için daha etkili olabilir. Örneğin, bellenim ve yazılım güncelleştirmeleri genellikle büyük dosyalarıdır.

**Verizon'dan Azure içerik teslim ağı** büyük dosya indirme içerik ulaştırmak için genel web teslim en iyi duruma getirme türünü kullanır.

Büyük dosya en iyi duruma getirme hakkında daha fazla bilgi için bkz: [büyük dosya iyileştirme](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Dinamik site hızlandırma

 Dinamik site hızlandırma kullanılabilir hem de **akamai'den Azure içerik teslim ağı** ve **verizon'dan Azure içerik teslim ağı** profilleri. Bu iyileştirme kullanmak için ek bir ücret gerektirir; Daha fazla bilgi için bkz: [içerik teslim ağı fiyatlandırma](https://azure.microsoft.com/pricing/details/cdn/).

Dinamik site hızlandırma gecikme süresi ve dinamik içerik performansını çeşitli teknikleri içerir. Teknikler rota ve ağ iyileştirme, TCP en iyi duruma getirme ve daha fazlasını içerir. 

Bu iyileştirme alınabilir bulunmayan çok sayıda yanıtları içeren bir web uygulaması hızlandırmak için kullanabilirsiniz. Arama sonuçları, teslim alma işlemleri veya gerçek zamanlı verileri örnektir. Statik verileri çekirdek CDN önbelleğe alma özelliklerini kullanmaya devam edebilirsiniz. 

Dinamik site hızlandırma hakkında daha fazla bilgi için bkz: [dinamik site hızlandırma](cdn-dynamic-site-acceleration.md).



