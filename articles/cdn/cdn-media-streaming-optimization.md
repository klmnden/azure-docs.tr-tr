---
title: Medya Azure CDN ile en iyi duruma getirme
description: Akış medya dosyaları kesintisiz teslim için en iyi duruma getirme
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: v-deasim
ms.openlocfilehash: 8a2b69aaa601e1d00152f57841a4d67f98680181
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33766181"
---
# <a name="media-streaming-optimization-with-azure-cdn"></a>Medya Azure CDN ile en iyi duruma getirme 
 
Yüksek tanımlı video kullanımını sorunlarla büyük dosyaların verimli bir şekilde teslim için oluşturur internet üzerinde artmaktadır. Müşteriler, isteğe bağlı video kesintisiz çalınmasını beklediğiniz veya ağları ve istemcilerin çeşitli video varlıklar tüm dünya çapında canlı. Medya dosyaları için hızlı ve verimli teslim mekanizması kesintisiz ve eğlenceli tüketici deneyimi sağlamak için önemlidir.  

Canlı akış medya eşzamanlı kullanıcıların sayısı ve büyük boyutlara nedeniyle teslim etmek özellikle zor olabilir. Tutulacak kullanıcıların uzun gecikmelere neden. Canlı akışlar önceden önbelleğe alınamaz ve büyük gecikmeleri izleyicilere kabul edilebilir değil çünkü video parçaları zamanında teslim edilmelidir. 

Akış istek desenlerini yeni zorluklar ortaya de sağlar. İsteğe bağlı video için popüler bir canlı akışı veya yeni bir seri serbest bırakıldığında görüntüleyiciler milyonlara aynı anda akış isteyebilir. Bu durumda, akıllı istek birleştirme varlıklar henüz önbelleğe olmayan alındığında kaynak sunucuları doldurmaya değil önemlidir.
 

## <a name="media-streaming-optimizations-for-azure-cdn-from-microsoft"></a>Microsoft Azure CDN iyileştirmelerini medya

**Azure CDN standart Microsoft** uç noktaları, genel web teslim en iyi duruma getirme türü doğrudan kullanarak akış medya varlıklar teslim et. 

İyileştirme için medya **Azure CDN standart Microsoft** için Canlı etkilidir veya isteğe bağlı video teslimi için bağımsız medya parçasının kullanan medya akış. Bu işlem, aşamalı indirme aracılığıyla veya bayt aralığı isteklerini kullanarak aktarılan tek bir büyük varlık farklıdır. Medya teslimi bu stili hakkında daha fazla bilgi için bkz [büyük dosya indirme en iyi duruma getirme Azure CDN ile](cdn-large-file-optimization.md).

Genel ortam teslim veya isteğe bağlı video medya teslim iyileştirme türleri medya varlıklar daha hızlı teslim etmek için arka uç iyileştirmeler etkinleştirilerek Azure içerik teslim ağı (CDN) kullanın. Bunlar ayrıca yapılandırmaları zamanla öğrenilen en iyi uygulamaları temel ortam varlıkları için kullanın.

### <a name="partial-cache-sharing"></a>Kısmi önbellek paylaşımı
Kısmi önbellek paylaşımı kısmen önbelleğe alınmış içeriği yeni isteklere hizmet vermek CDN sağlar. Örneğin, ilk istek ile CDN bir önbellek isabetsizliği sonuçlanırsa, istek kaynağa gönderilir. Bu içeriği eksik CDN önbelleğine yüklendiğinden karşın, bu veri alma CDN diğer isteklere başlatabilirsiniz. 


## <a name="media-streaming-optimizations-for-azure-cdn-from-verizon"></a>Medya verizon'dan Azure CDN için en iyi duruma getirme

**Azure CDN standart verizon'dan** ve **verizon'dan Azure CDN Premium** uç noktaları, genel web teslim en iyi duruma getirme türü doğrudan kullanarak akış medya varlıklar teslim et. CDN birkaç özellikleri doğrudan varsayılan medya varlıklar dağıtımına yardımcı.

### <a name="partial-cache-sharing"></a>Kısmi önbellek paylaşımı

Kısmi önbellek paylaşımı kısmen önbelleğe alınmış içeriği yeni isteklere hizmet vermek CDN sağlar. Örneğin, ilk istek ile CDN bir önbellek isabetsizliği sonuçlanırsa, istek kaynağa gönderilir. Bu içeriği eksik CDN önbelleğine yüklendiğinden karşın, bu veri alma CDN diğer isteklere başlatabilirsiniz. 

### <a name="cache-fill-wait-time"></a>Önbellek dolgu bekleme süresi

 Önbellek dolgu bekleme süresi özelliğinin tüm istekler aynı kaynak için HTTP yanıt üstbilgilerini kaynak sunucudan gelen kadar tutmak için uç sunucusunu zorlar. Süreölçer süresi dolmadan önce kaynaktan HTTP yanıt üstbilgilerini ulaşırsa, askıya tüm istekleri büyüyen önbellek dışına sunulur. Aynı anda önbellek kaynaktan verileri tarafından girilir. Varsayılan olarak, önbellek dolgu bekleme süresi 3000 milisaniye ayarlanır. 

 
## <a name="media-streaming-optimizations-for-azure-cdn-from-akamai"></a>Medya akamai'den Azure CDN için en iyi duruma getirme
 
**Azure CDN standart akamai'den** akış medya varlıklar kullanıcılar için verimli bir şekilde ölçekli dünya çapında sunan bir özellik sunar. Kaynak sunucu üzerindeki yükü azalttığı özelliği gecikmelerini azaltır. Bu özellik standart Akamai fiyatlandırma katmanı ile kullanılabilir. 

İyileştirme için medya **akamai'den Azure CDN standart** için Canlı etkilidir veya isteğe bağlı video teslimi için bağımsız medya parçasının kullanan medya akış. Bu işlem, aşamalı indirme aracılığıyla veya bayt aralığı isteklerini kullanarak aktarılan tek bir büyük varlık farklıdır. Medya teslimi bu stili hakkında daha fazla bilgi için bkz [büyük dosya iyileştirme](cdn-large-file-optimization.md).

Genel medya teslim veya isteğe bağlı video medya teslim en iyi duruma getirme türü bir CDN uç iyileştirmeler etkinleştirilerek medya varlıklar daha hızlı teslim etmek için kullanın. Bunlar ayrıca yapılandırmaları zamanla öğrenilen en iyi uygulamaları temel ortam varlıkları için kullanın.

### <a name="configure-an-akamai-cdn-endpoint-to-optimize-media-streaming"></a>Akış medya en iyi duruma getirmek için bir Akamai CDN uç noktasını yapılandırma
 
Teslim Azure Portalı aracılığıyla büyük dosyalar için en iyi duruma getirmek için içerik teslim ağı (CDN) uç noktası yapılandırabilirsiniz. Bunu yapmak için REST API'leri veya istemci SDK birini de kullanabilirsiniz. Aşağıdaki adımlar işlemi için Azure Portalı aracılığıyla gösterir bir **akamai'den Azure CDN standart** profili:

1. Bir Akamai üzerinde yeni bir uç noktası eklemek için **CDN profili** sayfasında, **Endpoint**.
  
    ![Yeni uç nokta](./media/cdn-media-streaming-optimization/cdn-new-akamai-endpoint.png)

2. İçinde **için en iyi duruma getirilmiş** aşağı açılan listesinden, **isteğe bağlı medya akış Video** isteğe bağlı video varlıklar. Bir birleşimi canlı ve isteğe bağlı video akış bunu yaparsanız, seçin **genel akış**.

    ![Akış seçili](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Uç nokta oluşturduktan sonra en iyi duruma getirme belirli ölçütlere uyan tüm dosyaları için geçerlidir. Aşağıdaki bölümde bu işlemi açıklanmaktadır. 

### <a name="caching"></a>Önbelleğe alma

Varsa **akamai'den Azure CDN standart** algılar varlık akış bildirimi veya parçası olduğundan, genel web teslim farklı önbelleğe alma sona erme sürelerinden kullanır. (Aşağıdaki tabloda tam listesine bakın.) Cache-control veya Expires üstbilgileri kaynaktan gönderilen her zaman, dikkate gibi. Varlık medya varlık değilse, genel web teslimi için zaman aşımı süresinin kullanarak önbelleğe alır.

Çok sayıda kullanıcı henüz mevcut olmayan bir parça istediğinde kısa negatif önbelleğe alma saat kaynak boşaltması için yararlıdır. Burada paketler bu ikinci kaynaktan kullanılamaz bir canlı akış örneğidir. Artık önbelleğe alma aralığı video içeriği genellikle değiştirdiğinden değil kaynak gelen istekleri boşaltma yardımcı olur.
 

|   | Genel web teslimatı | Genel medya akışı | İsteğe bağlı video medya  
--- | --- | --- | ---
Önbelleğe alma: pozitif <br> HTTP 200, 203, 300, <br> 301, 302 ve 410 | 7 gün |365 gün | 365 gün   
Önbelleğe alma: negatif <br> HTTP 204, 305, 404, <br> ve 405 | None | 1 saniye | 1 saniye
 
### <a name="deal-with-origin-failure"></a>Kaynak hata ile Dağıt  

Genel ortam teslim ve isteğe bağlı video medya teslim kaynak zaman aşımları ve tipik istek modelleri için en iyi yöntemler dayalı bir yeniden deneme günlük da vardır. Örneğin, genel medya teslim için canlı ve isteğe bağlı video medya teslim, zamana duyarlı doğası nedeniyle daha kısa bir bağlantı zaman aşımı kullandığı olduğundan canlı akış.

Bağlantı zaman aşımına uğradığında CDN "504 - ağ geçidi zaman aşımı" hatası istemciye göndermeden önce sayısı yeniden dener. 

Dosya türü ve boyutunu koşullar listesini bir dosyaya eşleşen CDN davranışı medya akış için kullanır. Aksi takdirde, genel web teslim kullanır.
   
### <a name="conditions-for-media-streaming-optimization"></a>En iyi duruma getirme medya için koşullar 

Aşağıdaki tabloda medya için en iyi duruma getirme geçirilmesi için ölçüt kümesini listeler: 
 
Desteklenen akış türleri | Dosya uzantıları  
--- | ---  
Apple HLS | m3u8, m3u, m3ub, anahtarı, ts, aac
Adobe HDS | f4m, f4x, drmmeta, önyükleme, f4f,<br>Seg parça URL yapısı <br> (regex eşleşen: ^(/.*)Seq(\d+)-Frag(\d+)
TİRE | mpd, tire, dıvx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, webm, mp4a, m4a, ISMA
Kesintisiz akış | / bildirimi//QualityLevels/parçaları /
  
 
