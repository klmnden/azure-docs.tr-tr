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
ms.openlocfilehash: e9ee0041061296e313b3372dce13b5b86b2369c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="core-reports-from-verizon"></a>Verizon'dan çekirdek raporları

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

İçin Verizon profilleri Yönet portalı yoluyla Verizon çekirdek raporları kullanarak, aşağıdaki raporları, CDN için kullanım desenlerini görüntüleyebilirsiniz:

* Bant Genişliği
* Aktarılan veriler
* İsabet sayısı
* Önbellek durumları
* İsabetli Önbellek okuması oranı
* IPv4/IPv6 veri aktarma

## <a name="accessing-verizon-core-reports"></a>Verizon çekirdek raporları erişme
1. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-reports/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **Analytics** sekmesini ve ardından üzerine gelerek **çekirdek raporları** çıkma.  İstenen rapor menüde tıklayın.
   
    ![CDN Yönetim Portalı - çekirdek Raporlar menüsü](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>Bant Genişliği
Bant genişliği raporun belirli bir süre boyunca HTTP ve HTTPS için bant genişliği kullanımını gösteren bir grafik ve veri tablosu oluşur. Tüm CDN POP veya belirli bir CDN POP arasında bant genişliği kullanımını görüntüleyebilirsiniz. Bu rapor, CDN POP MB/sn cinsinden arasında trafiği ani ve dağıtım görüntülemenizi sağlar.

* Belirli bir bölge/düğüm açılır listeden seçin veya tüm düğümlerde gelen trafiği görmek için tüm kenar düğümleri seçin.
* Veri bugün/bu hafta için/bu ay, vb. görüntülemek veya özel tarihleri girin ve ardından için tarih aralığını seçin **Git** seçiminizi güncelleştirilir emin olmak için.
* Dışarı aktarma ve verilerin nasıl yanında bulunan excel sayfası simgesini tıklatarak karşıdan **Git**.

Raporun beş dakikada bir güncellenir.

![Bant genişliği raporu](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Aktarılan veriler
Bu rapor trafiği kullanım HTTP ve HTTPS için belirli bir süre boyunca belirtilen bir grafik ve veri tablosu oluşur. Tüm CDN POP veya belirli bir POP üzerinden trafik kullanım görüntüleyebilirsiniz. Bu rapor, CDN POP GB arasında trafiği ani ve dağıtım görüntülemenizi sağlar.

* Belirli bir bölge/düğüm açılır listeden seçin veya tüm notları gelen trafiği görmek için tüm kenar düğümleri seçin.
* Veri bugün/bu hafta için/bu ay, vb. görüntülemek veya özel tarihleri girin ve ardından için tarih aralığını seçin **Git** seçiminizi güncelleştirilir emin olmak için.
* Dışarı aktarma ve verilerin nasıl yanında bulunan excel sayfası simgesini tıklatarak karşıdan **Git**.

Raporun beş dakikada bir güncellenir.

![Rapor aktarılan veriler](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>İsabet (durum kodu)
Bu rapor içeriğiniz için isteği durum kodları dağıtımını açıklar. İçerik için her istek HTTP durum kodu oluşturur. Durum kodu kenar POP isteğin nasıl işleneceğini açıklar. Örneğin, bir 4xx durum kodu bir hata oluştuğunu göstergesidir isteğin başarılı bir şekilde bir istemciye sunulduğu 2xx durum kodu gösterir. HTTP durum kodları hakkında daha fazla bilgi için bkz: [durum kodları](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

* Veri bugün/bu hafta için/bu ay, vb. görüntülemek veya özel tarihleri girin ve ardından için tarih aralığını seçin **Git** seçiminizi güncelleştirilir emin olmak için.
* Dışarı aktarma ve verilerin nasıl yanında bulunan excel sayfası tıklayarak karşıdan **Git**.

![İsabet raporu](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Önbellek durumları
Bu rapor İsabetli Önbellek okuma sayısı ve istemci isteği için İsabetsiz Önbellek okuma sayısı dağıtımını açıklar. Hızlı performans Önbelleği İsabetli Okuma geldiğinden, veri teslim hızlarını en aza İsabetsiz Önbellek okuma sayısı ve süresi dolan Önbelleği İsabetli Okuma göre en iyi duruma getirebilirsiniz. İsabetsiz önbellek okuma sayısı "no-cache" yanıt üstbilgilerini atama önlemek için kaynak sunucunuzu yapılandırma, kesinlikle gerekli olduğunda sorgu dizesi önbelleğe önleme ve alınabilir olmayan yanıt kodları önleme azaltabilir. Bir varlığın, max-age istekleri kaynak sunucuya sayısını en aza indirmek mümkün olduğunca uzun yaparak, süresi dolan Önbelleği İsabetli Okuma önleyebilirsiniz.

![Önbellek durumları raporu](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Ana önbellek durumlar şunlardır:
* TCP_HIT: Kenarından sundu. Nesne önbellekte idi ve onun Maksimum yaş aştı değil.
* TCP_MISS: Kaynaktan sundu. Nesne önbellekte değil ve yanıt geri kaynağa olmuştur.
* TCP_EXPIRED _MISS: kaynağına sahip COLLECTION sonra kaynaktan alınan hizmet. Nesne önbellekte olsa da, kendi max-age aşmıştı. Yeniden doğrulanması kaynağına sahip yeni bir yanıt kaynaktan tarafından değiştirilen önbellek nesnesi ile sonuçlandı.
* TCP_EXPIRED _HIT: kaynağına sahip COLLECTION sonra kenarından hizmet. Nesne önbelleğinde oldu, ancak kendi max-age aşmıştı. Kaynak sunucu ile yeniden doğrulanması değiştirilmemiş önbellek nesnesinde sonuçlandı.
* Veri bugün/bu hafta için/bu ay, vb. görüntülemek veya özel tarihleri girin ve ardından için tarih aralığını seçin **Git** seçiminizi güncelleştirilir emin olmak için.
* Dışarı aktarma ve verilerin nasıl yanında bulunan excel sayfası simgesini tıklatarak karşıdan **Git**.

### <a name="full-list-of-cache-statuses"></a>Önbellek durumları tam listesi
* TCP_HIT - bir istek istemciye doğrudan POP sunulduğunda bu durum bildirilir. İstemcinin en yakın POP önbelleğe alınır ve geçerli yaşam süresi (TTL) sahip olduğunda bir varlık hemen POP sunulur. TTL aşağıdaki yanıt üstbilgileri tarafından belirlenir:
  
  * Cache-Control: s-maxage
  * Cache-Control:, max-age
  * Süre sonu
* TCP_MISS - bu durum, istenen varlık önbelleğe alınmış bir sürümü, istemcinin en yakın POP üzerinde bulunamadı gösterir. Varlık, kaynak sunucu veya bir kaynak kalkan sunucu istenmektedir. Kaynak sunucu veya kaynak kalkan sunucu bir varlık döndürürse, istemciye hizmet ve hem istemci hem de edge sunucusunda önbelleğe alınmış. Aksi takdirde, 200 durum kodu (örneğin, 403 Yasak veya 404 bulunamadı) döndürülür.
* TCP_EXPIRED _HIT - süresi dolmuş bir TTL bir varlığı hedefleyen bir isteğin doğrudan POP istemciye sunulduğu, bu durum bildirilir. Örneğin, varlık, max-age zaman süresi doldu. 
  
    Süresi dolmuş bir isteği yeniden doğrulanması istekte kaynak sunucuya genellikle sonuçlanır. TCP_EXPIRED _HIT gerçekleşmesi, kaynak sunucuyu varlık daha yeni bir sürümü mevcut değil belirtmeniz gerekir. Bu durum genellikle bir varlığın Cache-Control ve Expires üstbilgileri güncelleştirmesinde sonuçlanır.
* TCP_EXPIRED _MISS - süresi dolmuş bir önbelleğe alınan varlık daha yeni bir sürümü istemciye POP sunulduğunda bu durum bildirilir. Önbelleğe alınan bir varlık için TTL'nin süresi sona erdiğinde, bu durum oluşur (örneğin, süresi doldu, max-age) ve kaynak sunucu, varlık, yeni bir sürümünü döndürür. Varlık bu yeni sürümü, önbelleğe alınan sürümü yerine istemciye hizmet verir. Ek olarak, bu uç sunucusunu ve istemcide önbelleğe alınır.
* CONFIG_NOCACHE - bu durum, bizim kenar POP müşteriye özgü yapılandırmasına önbelleğe alınması varlık engelledi gösterir.
* HİÇBİRİ - bu durum, bir önbellek içerik yenilik denetimi gerçekleştirilmemiş gösterir.
* TCP_ CLIENT_REFRESH _MISS - bir tarayıcı gibi bir HTTP istemcisi bir kenar eski varlık yeni bir sürümü kaynak sunucudan almak üzere POP zorlar, bu durum bildirilir.
  
    Varsayılan olarak, kaynak sunucudan veri varlığı yeni bir sürümünü almak için kenar sunucularımızda zorlama sunucularımızda bir HTTP istemci engeller.
* TCP_ PARTIAL_HIT - vuruş kısmen önbelleğe alınmış bir varlık için bir bayt aralığı isteği sonuçlanır olduğunda bu durum bildirilir. İstenen bayt aralığı, istemciye POP hemen sunulur.
* UNCACHEABLE - bir varlığın Cache-Control ve Expires üstbilgileri belirttiğinizde, POP veya HTTP istemci tarafından önbelleğe alınması gereken değil, bu durum bildirilir. Bu tür istekleri kaynak sunucudan hizmet alır.

## <a name="cache-hit-ratio"></a>İsabetli Önbellek okuması oranı
Bu rapor, doğrudan önbelleğinden sunulduğunu önbelleğe alınan istek sayısı yüzdesini gösterir.

Bu rapor, aşağıdaki ayrıntıları sağlar:

* İstenen içerik istemciye en yakın POP önbelleğe alınmadı.
* İsteğin doğrudan sunduğumuz ağ kenarından sunulduğu.
* İstek yeniden doğrulanması kaynak sunucu ile gerekli değil.

Raporun içermez:

* Ülke filtreleme seçeneklerini nedeniyle reddedildi istek sayısı.
* Bunlar değil konumlandırılmalıdır başlıklarını belirtmek varlıklar için istek sayısı. Örneğin, Cache-Control: özel, Cache-Control: no-cache veya Pragma: no-cache üstbilgileri bir varlık önbelleğe alınmasını engellemek.
* Kısmen önbelleğe alınmış içeriği için bayt aralığı isteklerini.

Formül: (TCP_ İSABET / (TCP_ İSABET + TCP_MISS)) * 100

* Veri bugün/bu hafta için/bu ay, vb. görüntülemek veya özel tarihleri girin ve ardından için tarih aralığını seçin **Git** seçiminizi güncelleştirilir emin olmak için.
* Dışarı aktarma ve verilerin nasıl yanında bulunan excel sayfası simgesini tıklatarak karşıdan **Git**.

![İsabetli Önbellek okuması oranı raporu](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPv6 aktarılan veriler
Bu rapor, IPv4 vs IPv6 trafiği kullanım dağılımını gösterir.

![IPv4/IPv6 aktarılan veriler](./media/cdn-reports/cdn-ipv4-ipv6.png)

* Özel tarih girmek veya veri bugün/bu hafta için/bu ay, vb. görüntülemek için tarih aralığını seçin.
* Ardından **Git** seçiminizi güncelleştirilir emin olmak için.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Raporları yalnızca son 18 ay içinde oluşturulabilir.

