---
title: "Çekirdek verizon'dan raporları | Microsoft Docs"
description: "Aşağıdaki raporlar kullanarak, CDN için kullanım desenlerini görüntüleyebilirsiniz: bant genişliği, aktarılan verileri, isabet, önbellek durumları, önbellek isabet oranı, IPv4/IPv6 aktarılan veriler."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: fa828bfa736d677fb4881e5cc2628c0e03eb8749
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="core-reports-from-verizon"></a>Verizon'dan çekirdek raporları

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

İçin Verizon profilleri Yönet portalı yoluyla Verizon çekirdek raporları kullanarak, aşağıdaki raporları, CDN için kullanım desenlerini görüntüleyebilirsiniz:

* Bant Genişliği
* Aktarılan veriler
* İsabetli
* Önbellek durumları
* İsabetli Önbellek okuması oranı
* IPv4/IPv6 veri aktarma

## <a name="accessing-verizon-core-reports"></a>Verizon çekirdek raporları erişme
1. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili Yönet düğmesi](./media/cdn-reports/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **Analytics** sekmesini ve ardından üzerine gelerek **çekirdek raporları** çıkma. Menü rapora tıklayın.
   
    ![CDN Yönetim Portalı - çekirdek Raporlar menüsü](./media/cdn-reports/cdn-core-reports.png)

3. Her bir rapor için bir tarih aralığında seçin **tarih aralığı** listesi. Önceden tanımlanmış tarih aralığı gibi seçebilirsiniz **Bugün** veya **bu hafta**, ya da seçebilirsiniz **özel** ve el ile Takvim simgelere tıklayarak bir tarih aralığı girin. 

4. Bir tarih aralığı seçtikten sonra tıklayın **Git** raporu oluşturmak için. 

4. Verileri Excel biçiminde dışarı aktarmak istiyorsanız, yukarıdaki Excel simgesini tıklatın **Git** düğmesi.

## <a name="bandwidth"></a>Bant Genişliği
Bant genişliği raporun belirli süre boyunca, MB/sn cinsinden HTTP ve HTTPS için CDN bant genişliği kullanımını gösteren bir grafik ve veri tablosu oluşur. Tüm POP üzerinden veya belirli POP için bant genişliği kullanımını görüntüleyebilirsiniz. Bu rapor, POP için dağıtım ve trafik ani görüntülemenizi sağlar.

Gelen **kenar düğümleri** listesinden, **tüm kenar düğümleri** tüm düğümleri gelen trafiği görmek veya belirli bir bölge seçin.

Raporun beş dakikada bir güncellenir.

![Bant genişliği raporu](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Aktarılan veriler
Bu rapor, belirli süre boyunca, GB, HTTP ve HTTPS için CDN trafiği kullanım gösteren bir grafik ve veri tablo oluşur. Tüm POP üzerinden veya belirli POP için trafiği kullanım görüntüleyebilirsiniz. Bu rapor, dağıtım ve trafik ani POP görüntülemenizi sağlar.

Gelen **kenar düğümleri** listesinden, **tüm kenar düğümleri** tüm düğümleri gelen trafiği görmek veya belirli bir bölge seçin.

Raporun beş dakikada bir güncellenir.

![Rapor aktarılan veriler](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>İsabet (durum kodu)
Bu rapor içeriğiniz için isteği durum kodları dağıtımını açıklar. İçerik için her istek HTTP durum kodu oluşturur. Durum kodu kenar POP isteğin nasıl işleneceğini açıklar. Örneğin, bir 4xx durum kodu bir hata oluştuğunu göstergesidir isteğin başarılı bir şekilde bir istemciye sunulduğu 2xx durum kodu gösterir. HTTP durum kodları hakkında daha fazla bilgi için bkz: [listesi, HTTP durum kodları](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

![İsabet raporu](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Önbellek durumları
Bu rapor İsabetli Önbellek okuma sayısı ve istemci istekleri için İsabetsiz Önbellek okuma sayısı dağıtımını açıklar. Hızlı performans Önbelleği İsabetli Okuma sonuçlandığından İsabetsiz Önbellek okuma sayısı ve süresi dolan önbelleği isabetli okuma en aza indirerek veri teslim hızlarını en iyi duruma getirebilirsiniz. 

Önbellek isabetsizliği azaltmak için aşağıdaki kullanımını en aza indirmek için kaynak sunucunuzu yapılandırın: 
 * `no-cache`Yanıt Üstbilgileri
 * Sorgu dizesi, kesinlikle gerekli olmadıkça önbelleğe alma  
 * Olmayan alınabilir yanıt kodları

Süresi dolan Önbelleği İsabetli Okuma azaltmak için bir varlığın ayarlamak `max-age` istekleri kaynak sunucuya sayısını en aza indirmek için uzun bir süre için.

![Önbellek durumları raporu](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Ana önbellek durumlar şunlardır:
* TCP_HIT: kenar sunucudan sundu. Nesne önbellekte idi ve onun Maksimum yaş aştı değil.
* TCP_MISS: kaynak sunucudan sundu. Nesne önbellekte değil ve yanıt geri kaynağa olmuştur.
* TCP_EXPIRED _MISS: kaynağına sahip COLLECTION sonra kaynak sunucudan hizmet. Nesne önbellekte olsa da, kendi max-age aşmıştı. Yeniden doğrulanması kaynağına sahip yeni bir yanıt kaynaktan tarafından değiştirilen önbellek nesnesi ile sonuçlandı.
* TCP_EXPIRED _HIT: kaynağına sahip COLLECTION sonra kenarından hizmet. Nesne önbelleğinde oldu, ancak kendi max-age aşmıştı. Kaynak sunucu ile yeniden doğrulanması değiştirilmemiş önbellek nesnesinde sonuçlandı.

### <a name="full-list-of-cache-statuses"></a>Önbellek durumları tam listesi
* TCP_HIT - bir istek istemciye doğrudan POP sunulduğunda bu durum bildirilir. İstemcinin en yakın POP önbelleğe alınır ve geçerli yaşam süresi (TTL) sahip olduğunda bir varlık hemen POP sunulur. TTL aşağıdaki yanıt üstbilgileri tarafından belirlenir:
  
  * Cache-Control: s-maxage
  * Cache-Control:, max-age
  * Süre Sonu:
* TCP_MISS: Bu durum, istenen varlık önbelleğe alınmış bir sürümü, istemcinin en yakın POP üzerinde bulunamadı gösterir. Varlık, kaynak sunucu veya bir kaynak kalkan sunucu istenmektedir. Kaynak sunucu veya kaynak kalkan sunucu bir varlık döndürürse, istemciye hizmet ve hem istemci hem de edge sunucusunda önbelleğe alınmış. Aksi takdirde, 200 durum kodu (örneğin, 403 Yasak veya 404 bulunamadı) döndürülür.
* TCP_EXPIRED_HIT: süresi dolan TTL bir varlığı hedefleyen bir isteği istemciye doğrudan POP sunulduğu, bu durum raporlanır. Örneğin, varlık, max-age zaman süresi doldu. 
  
   Süresi dolmuş bir isteği yeniden doğrulanması istekte kaynak sunucuya genellikle sonuçlanır. TCP_EXPIRED_HIT durumu gerçekleşmesi, kaynak sunucuyu varlık daha yeni bir sürümü mevcut değil belirtmeniz gerekir. Bu durum genellikle bir varlığın Cache-Control ve Expires üstbilgileri güncelleştirmesinde sonuçlanır.
* TCP_EXPIRED_MISS: süresi dolan bir önbelleğe alınan varlık daha yeni bir sürümü istemciye POP sunulduğunda bu durum raporlanır. Önbelleğe alınan bir varlık için TTL'nin süresi sona erdiğinde, bu durum oluşur (örneğin, süresi doldu, max-age) ve kaynak sunucu, varlık, yeni bir sürümünü döndürür. Varlık bu yeni sürümü, önbelleğe alınan sürümü yerine istemciye hizmet verir. Ek olarak, bu uç sunucusunu ve istemcide önbelleğe alınır.
* CONFIG_NOCACHE: Bu durum, müşteriye özgü yapılandırma POP kenar varlık önbelleğe alınmasını önledi olduğunu gösterir.
* HİÇBİRİ - bu durum, bir önbellek içerik yenilik denetimi gerçekleştirilmemiş gösterir.
* TCP_CLIENT_REFRESH_MISS: kenar POP eski varlık yeni bir sürümü kaynak sunucudan veri almak için bir tarayıcı gibi bir HTTP istemcisi zorlar, bu durum raporlanır. Varsayılan olarak, kaynak sunucudan veri varlığı yeni bir sürümünü almak için uç sunucuların zorlama sunucuları bir HTTP istemcisi engeller.
* TCP_PARTIAL_HIT: vuruş kısmen önbelleğe alınmış bir varlık için bir bayt aralığı isteği sonuçlanır olduğunda bu durum raporlanır. İstenen bayt aralığı, istemciye POP hemen sunulur.
* UNCACHEABLE: Bir varlığı zaman 's bu durum bildirilir `Cache-Control` ve `Expires` üstbilgileri belirtmek, POP veya HTTP istemci tarafından önbelleğe alınması gereken değil. Bu tür istekleri kaynak sunucudan hizmet alır.

## <a name="cache-hit-ratio"></a>İsabetli Önbellek okuması oranı
Bu rapor, doğrudan önbelleğinden sunulduğunu önbelleğe alınan istek sayısı yüzdesini gösterir.

Bu rapor, aşağıdaki ayrıntıları sağlar:

* İstenen içerik istemciye en yakın POP önbelleğe alınmadı.
* İsteğin doğrudan sunduğumuz ağ kenarından sunulduğu.
* İstek yeniden doğrulanması kaynak sunucu ile gerekli değil.

Raporun içermez:

* Ülke filtreleme seçeneklerini nedeniyle reddedildi istek sayısı.
* Bunlar değil konumlandırılmalıdır başlıklarını belirtmek varlıklar için istek sayısı. Örneğin, `Cache-Control: private`, `Cache-Control: no-cache`, veya `Pragma: no-cache` üstbilgileri bir varlık önbelleğe alınmasını engellemek.
* Kısmen önbelleğe alınmış içeriği için bayt aralığı isteklerini.

Formül: (TCP_ İSABET / (TCP_ İSABET + TCP_MISS)) * 100

![İsabetli Önbellek okuması oranı raporu](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPv6 aktarılan veriler
Bu rapor, IPv4 vs IPv6 trafiği kullanım dağılımını gösterir.

![IPv4/IPv6 aktarılan veriler](./media/cdn-reports/cdn-ipv4-ipv6.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Raporları yalnızca son 18 ay içinde oluşturulabilir.

