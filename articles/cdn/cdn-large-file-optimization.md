---
title: Azure CDN ile büyük dosya indirme iyileştirme
description: Bu makalede, ne kadar büyük dosya yüklemeleri iyileştirilebilir açıklanmaktadır.
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
ms.openlocfilehash: 2bdb6bdea7b6180e34458883d026161403e4cb58
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33766215"
---
# <a name="large-file-download-optimization-with-azure-cdn"></a>Azure CDN ile büyük dosya indirme iyileştirme

Internet üzerinden teslim içeriğinin dosya boyutları, gelişmiş işlevler, geliştirilmiş grafik ve zengin medya içeriği nedeniyle büyümeye devam. Bu büyüme birçok faktöre tarafından yönetilir: geniş bant sızma, büyük uygun maliyetli depolama aygıtları, yüksek tanımlı video ve internet'e bağlı cihazlar (IOT) yaygın arttıkça. Büyük dosyalar için hızlı ve verimli teslim mekanizması kesintisiz ve eğlenceli tüketici deneyimi sağlamak için önemlidir.

Büyük dosyaları teslimini çeşitli zorluklar sahiptir. İlk olarak, uygulamaların tüm verileri sıralı olarak indirmesini değil çünkü büyük bir dosyayı indirmek için geçen ortalama süre önemli olabilir. Bazı durumlarda, ilk bölümü önce dosyayı son parçası uygulamalar indirmesini. Az miktarda bir dosya istenen veya bir kullanıcı bir indirme duraklatır, yükleme başarısız olabilir. İçerik teslim ağı (CDN) dosyanın tamamı kaynak sunucudan sonra karşıdan yükleme de kadar gecikebilir. 

İkinci olarak, bir kullanıcının machine ve dosya arasındaki gecikme süresi içerik aktarılma görüntüleyebilirler hızını belirler. Ayrıca, Ağ Tıkanıklığı ve kapasite sorunları verimliliği de etkiler. Sunucular ve kullanıcılar arasında büyük uzaklıklar kalite azaltır gerçekleşmesi, paket kaybı başka fırsatları oluşturun. Sınırlı verimlilik tarafından kalite azalmasına neden ve artan paket kaybı tamamlamak bir dosya indirme bekleme süresini artırabilir. 

Üçüncü birçok büyük dosyayı kendi bütün teslim edilmedi. Kullanıcılar aracılığıyla yarısı bir yüklemeyi iptal edin veya yalnızca ilk birkaç dakika uzun bir MP4 video izleyin. Bu nedenle, yazılım ve medya teslim şirketler yalnızca istenen dosyanın bölümünü sunmak istiyorsunuz. İstenen bölümleri verimli dağıtımını kaynak sunucudan çıkış trafiği azaltır. Etkili bir dağıtım, bellek ve g/ç baskısı kaynak sunucu üzerinde de azaltır. 


## <a name="optimize-for-delivery-of-large-files-with-azure-cdn-from-microsoft"></a>Microsoft Azure CDN büyük dosyalarla teslimi için en iyi duruma getirme

**Azure CDN standart Microsoft** uç noktaları teslim dosya boyutu bir büyük harf olmayan büyük dosyaları. Ek özellikler büyük dosyalar teslimini hızlandırmak için varsayılan olarak etkinleştirilir.

### <a name="object-chunking"></a>Öbekleme nesnesi 

**Azure CDN standart Microsoft** nesne Öbekleme adında bir teknik kullanır. Büyük bir dosya istendiğinde, CDN dosyasının küçük parça kaynaktan alır. CDN POP sunucunun tam veya bayt aralığı dosya isteği aldıktan sonra CDN uç sunucu dosyayı 8 MB'lık parçalar kaynaktan ister. 

CDN sınırında öbek ulaştıktan sonra bu önbelleğe alınmış ve hemen kullanıcıya sunulan. CDN ardından sonraki öbek paralel prefetches. Bu önceden getirme içeriği gecikmesini azaltır kullanıcı öncesinde bir öbek kalmasını sağlar. Bu işlem tüm kadar devam eder (isteniyorsa) dosyası indirilir, tüm bayt aralığı (isteniyorsa) kullanılabilir, veya istemci bağlantıyı sonlandırır. 

Bayt aralığı isteği hakkında daha fazla bilgi için bkz: [RFC 7233](https://tools.ietf.org/html/rfc7233).

CDN alındığında gibi tüm öbekleri önbelleğe alır. Dosyanın tamamı CDN önbellekte önbelleğe alınacak gerekmez. Dosya veya bayt aralıkları için sonraki istekleri CDN önbellekten sunulur. Aksi takdirde tüm öbek üzerinde CDN önbelleğe alınan, hazırlık öbekleri kaynaktan istemek için kullanılır. Bu iyileştirme bayt aralığı isteklerini destekleyen yeteneğini kaynak sunucu üzerinde kullanır; Kaynak sunucu bayt aralığı isteklerini desteklemiyorsa, bu en iyi duruma getirme etkin değil. 

### <a name="conditions-for-large-file-optimization"></a>Büyük dosya iyileştirme için koşullar
Büyük dosya iyileştirme özellikleri için **Azure CDN standart Microsoft** genel web teslim en iyi duruma getirme türünü kullandığınızda varsayılan olarak etkinleştirilir. En büyük dosya boyutu üzerinde hiçbir sınır vardır.


## <a name="optimize-for-delivery-of-large-files-with-azure-cdn-from-verizon"></a>Verizon'dan Azure CDN büyük dosyalarla teslimi için en iyi duruma getirme

**Azure CDN standart verizon'dan** ve **verizon'dan Azure CDN Premium** uç noktaları teslim dosya boyutu bir büyük harf olmayan büyük dosyaları. Ek özellikler büyük dosyalar teslimini hızlandırmak için varsayılan olarak etkinleştirilir.

### <a name="complete-cache-fill"></a>Tam önbellek doldurma

Varsayılan tam önbellek doldurma özelliğinin CDN ilk istek terk kaybolduğunda veya bir dosya önbelleğine çekmesine olanak sağlar. 

Tam önbellek dolgu büyük varlıkları için kullanışlıdır. Genellikle, kullanıcıların onları baştan yüklemeyin. Aşamalı indirme kullanırlar. Varsayılan davranış, varlık kaynak sunucusundan bir arka planda getirmeye başlatmak için uç sunucusunu zorlar. Daha sonra kenar sunucunun yerel önbellekteki varlıktır. Tam nesne önbellekte sonra uç sunucusunu önbelleğe alınmış nesnenin CDN bayt aralığı isteklerini karşılar.

Varsayılan davranış kuralları altyapısında aracılığıyla devre dışı bırakılabilir **verizon'dan Azure CDN Premium**.

### <a name="peer-cache-fill-hot-filing"></a>Eş önbelleği hot dosyalama doldurun

Varsayılan eş önbellek dolgu hot dosyalama özelliği bir karmaşık özel algoritması kullanır. Bant genişliğine bağlı sunucuları önbelleğe alma ek kenar kullanır ve büyük, yüksek oranda popüler nesneler için istemci isteklerini yerine getirmek için ölçümleri toplama ister. Bu özellik, bir kullanıcının kaynak sunucuya ek istekler çok sayıda gönderilen bir durum önler. 

### <a name="conditions-for-large-file-optimization"></a>Büyük dosya iyileştirme için koşullar

Büyük dosya iyileştirme özellikleri için **verizon'dan Azure CDN standart** ve **verizon'dan Azure CDN Premium** genel web teslim en iyi duruma getirme türünü kullandığınızda varsayılan olarak etkinleştirilir. En büyük dosya boyutu üzerinde hiçbir sınır vardır. 


## <a name="optimize-for-delivery-of-large-files-with-azure-cdn-standard-from-akamai"></a>Akamai'den Azure CDN standart büyük dosyalarla teslimi için en iyi duruma getirme

**Azure CDN standart akamai'den** profil uç noktaları büyük dosyalar kullanıcılar için verimli bir şekilde ölçekli dünya çapında sunan bir özellik sunar. Kaynak sunucu üzerindeki yükü azalttığı özelliği gecikmelerini azaltır.

Büyük dosya en iyi duruma getirme türü özelliği, ağ iyileştirmeleri ve büyük dosyalar daha hızlı ve daha responsively sunmak için yapılandırmaları açar. Genel web teslimat ile **akamai'den Azure CDN standart** uç noktaları 1,8 GB yalnızca aşağıdaki dosyaları önbelleğe alır ve tünel (değil önbellek) dosyaları 150 GB'a kadar. Büyük dosya iyileştirme önbellekleri 150 GB'a kadar dosyaları.

Belirli koşullar karşılandığında büyük dosya iyileştirmesi etkili olur. Koşullar dahil kaynak sunucunun nasıl çalıştığını ve boyutları ve istenen dosya türleri. 

### <a name="configure-an-akamai-cdn-endpoint-to-optimize-delivery-of-large-files"></a>Büyük dosyaları teslimini en iyi duruma getirmek için bir Akamai CDN uç noktasını yapılandırma

Yapılandırabileceğiniz, **akamai'den Azure CDN standart** teslim Azure Portalı aracılığıyla büyük dosyalar için en iyi duruma getirmek için uç noktası. Bunu yapmak için REST API'leri veya istemci SDK birini de kullanabilirsiniz. Aşağıdaki adımlar işlemi için Azure Portalı aracılığıyla gösterir bir **akamai'den Azure CDN standart** profili:

1. Bir Akamai üzerinde yeni bir uç noktası eklemek için **CDN profili** sayfasında, **Endpoint**.

    ![Yeni uç nokta](./media/cdn-large-file-optimization/cdn-new-akamai-endpoint.png)    
 
2. İçinde **için en iyi duruma getirilmiş** aşağı açılan listesinden, **büyük dosya indirme**.

    ![Seçili büyük dosya en iyi duruma getirme](./media/cdn-large-file-optimization/cdn-large-file-select.png)


CDN uç noktası oluşturduktan sonra büyük dosya iyileştirmeler belirli ölçütlere uyan tüm dosyaları için geçerlidir. Aşağıdaki bölümde bu işlemi açıklanmaktadır.

### <a name="object-chunking"></a>Öbekleme nesnesi 

Büyük dosya iyileştirme ile **akamai'den Azure CDN standart** nesne Öbekleme adında bir teknik kullanır. Büyük bir dosya istendiğinde, CDN dosyasının küçük parça kaynaktan alır. CDN POP sunucunun tam veya bayt aralığı dosya isteği aldıktan sonra bu en iyi duruma getirme için desteklenen dosya türü olup olmadığını denetler. Ayrıca, dosya türünü dosya boyutu gereksinimleri karşılayıp karşılamadığını denetler. Dosya boyutu 10 MB'den büyük ise, CDN uç sunucu dosyayı 2 MB'lık parçalar kaynaktan ister. 

CDN sınırında öbek ulaştıktan sonra bu önbelleğe alınmış ve hemen kullanıcıya sunulan. CDN ardından sonraki öbek paralel prefetches. Bu önceden getirme içeriği gecikmesini azaltır kullanıcı öncesinde bir öbek kalmasını sağlar. Bu işlem tüm kadar devam eder (isteniyorsa) dosyası indirilir, tüm bayt aralığı (isteniyorsa) kullanılabilir, veya istemci bağlantıyı sonlandırır. 

Bayt aralığı isteği hakkında daha fazla bilgi için bkz: [RFC 7233](https://tools.ietf.org/html/rfc7233).

CDN alındığında gibi tüm öbekleri önbelleğe alır. Dosyanın tamamı CDN önbellekte önbelleğe alınacak gerekmez. Dosya veya bayt aralıkları için sonraki istekleri CDN önbellekten sunulur. Aksi takdirde tüm öbek üzerinde CDN önbelleğe alınan, hazırlık öbekleri kaynaktan istemek için kullanılır. Bu iyileştirme bayt aralığı isteklerini destekleyen yeteneğini kaynak sunucu üzerinde kullanır; Kaynak sunucu bayt aralığı isteklerini desteklemiyorsa, bu en iyi duruma getirme etkin değil.

### <a name="caching"></a>Önbelleğe alma
Büyük dosya en iyi duruma getirme, genel web teslim farklı varsayılan önbelleğe alma sona erme sürelerinden kullanır. Pozitif hem HTTP yanıt kodlarına dayalı negatif önbelleğini arasında ayırır. Kaynak sunucusu önbellek denetim aracılığıyla bir sona erme saati belirtir veya sona erme yanıt üstbilgisi, CDN değerini korur. Kaynak belirtmiyor ve dosyayı bu en iyi duruma getirme türü için türü ve boyutunu koşullara uyan CDN büyük dosya iyileştirme için varsayılan değerleri kullanır. Aksi takdirde, CDN varsayılanları Genel web kullanır.


|    | Genel web | Büyük dosya iyileştirmesi 
--- | --- | --- 
Önbelleğe alma: pozitif <br> HTTP 200, 203, 300, <br> 301, 302 ve 410 | 7 gün |1 gün  
Önbelleğe alma: negatif <br> HTTP 204, 305, 404, <br> ve 405 | None | 1 saniye 

### <a name="deal-with-origin-failure"></a>Kaynak hata ile Dağıt

Kaynak okuma zaman aşımı uzunluğu, genel web teslimi büyük dosya en iyi duruma getirme türü için iki dakika için iki saniye gelen artar. Bu artış erken zaman aşımı bağlantısı önlemek için daha büyük dosya boyutlarına hesapları.

Bağlantı zaman aşımına uğradığında CDN "504 - ağ geçidi zaman aşımı" hatası istemciye göndermeden önce sayısı yeniden dener. 

### <a name="conditions-for-large-file-optimization"></a>Büyük dosya iyileştirme için koşullar

Aşağıdaki tabloda büyük dosya iyileştirme için yeterli olması için ölçüt kümesini listeler:

Koşul | Değerler 
--- | --- 
Desteklenen dosya türleri | 3g 2, 3gp, asf, AVI, bz2, dmg, exe, f4v, flv, <br> GZ hdp, ISO, jxr, m4v, mkv, mov, mp4, <br> MPEG, mpg, mts, pkg, qt, rm, swf, tar, <br> tgz, wdp, webm, webp, wma, wmv, ZIP  
Küçük dosya boyutu | 10 MB 
En büyük dosya boyutu | 150 GB 
Kaynak sunucu özellikleri | Bayt aralığı isteklerini desteklemesi gerekir 

## <a name="additional-considerations"></a>Diğer konular

Bu en iyi duruma getirme türü için aşağıdaki ek konuları göz önünde bulundurun:

- Kümeleme işlemi kaynak sunucuya ek istekler üretir. Ancak, genel birimin kaynaktan teslim verilerin çok daha küçüktür. Kümeleme sonuçlarında CDN konumundaki önbelleğe alma özelliklerini daha iyi.

- Dosyanın küçük parça teslim edildiğinden kaynak bellek ve g/ç baskısı azaltılır.

- CDN önbelleğe alınmış öbekleri için vardır hiçbir ek istekler kaynağa içeriğin süresi dolar veya önbellekten çıkarılmasına kadar.

- Kullanıcılar aralık isteklerini CDN ile normal bir dosya gibi algılanan yapabilir. En iyi duruma getirme, yalnızca geçerli bir dosya türü ise ve bayt aralığı 10 MB ve 150 GB arasında ise geçerlidir. İstenen ortalama dosya boyutu 10 MB'den küçük ise, genel web teslim kullanın.

