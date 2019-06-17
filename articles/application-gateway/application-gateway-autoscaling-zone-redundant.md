---
title: Otomatik ölçeklendirme ve bölgesel olarak yedekli Application Gateway v2
description: Bu makalede, Azure uygulama Standard_v2 ve otomatik ölçeklendirme ve bölgesel olarak yedekli özellikler içeren WAF_v2 SKU tanıtılmaktadır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/13/2019
ms.author: victorh
ms.openlocfilehash: 7cf6b4984f3941da3b2cd0e4eada5eb1d87f2b01
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67054737"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-v2"></a>Otomatik ölçeklendirme ve bölgesel olarak yedekli Application Gateway v2 

Application Gateway ve Web uygulaması Güvenlik Duvarı (WAF) ayrıca Standard_v2 hem WAF_v2 SKU altında bulunur. V2 SKU performans geliştirmeleri sunar ve otomatik ölçeklendirme, bölge artıklığı ve statik VIP'ler için destek gibi önemli yeni özellikleri için destek ekler. Mevcut özellikleri standart ve WAF SKU altında yeni v2 SKU içinde listelenen birkaç özel durum desteklenmeye devam [karşılaştırma](#differences-with-v1-sku) bölümü.

Yeni v2 SKU aşağıdaki geliştirmeleri içerir:

- **Otomatik ölçeklendirme**: Uygulama ağ geçidi veya WAF dağıtımlar altında SKU otomatik ölçeklendirme ölçeğini artırabilir veya trafik yük düzenleri değişen aşağı dayalı. Otomatik ölçeklendirme ayrıca sağlama sırasında dağıtım boyutu veya örnek sayısı seçme gereksinimini de ortadan kaldırır. Bu SKU, doğru esneklik sunar. Standard_v2 ve WAF_v2 SKU, Application Gateway hem de sabit kapasite (otomatik ölçeklendirmeyi devre dışı) otomatik ölçeklendirme etkin modda çalışabilir. Sabit kapasite modu, tutarlı ve öngörülebilir iş yüklerine sahip senaryolar için kullanışlıdır. Otomatik ölçeklendirme modu, uygulama trafiği varyans bkz uygulamalarda yararlıdır.
- **Bölge yedekliliği**: Bir uygulama ağ geçidi veya WAF dağıtımını birden fazla kullanılabilirlik, bir Traffic Manager ile her bölgedeki ayrı bir Application Gateway örneğinden sağlamaya gerek kaldırma yayılabilir. Tek bir bölge veya uygulama ağ geçidi örneklerinin dağıtıldığı birden çok bölgeyi bölge hatalarına karşı daha dayanıklı hale getiren seçebilirsiniz. Uygulamalar için arka uç havuzu kullanılabilirlik alanları genelinde benzer şekilde dağıtılabilir.

  Bölge artıklığı, yalnızca Azure bölgeleri kullanılabilir olduğu kullanılabilir. Diğer bölgelerde, diğer tüm özellikler desteklenir. Daha fazla bilgi için [Azure kullanılabilirlik alanları nedir?](../availability-zones/az-overview.md#services-support-by-region)
- **Statik VIP**: Uygulama ağ geçidi v2 SKU destekler statik VIP özel olarak yazın. Bu, bir yeniden başlatma işleminden sonra bile dağıtım yaşam döngüsü için bu uygulama ağ geçidiyle ilişkili VIP de değişmez sağlar.
- **Üstbilgi yeniden yazma**: Uygulama ağ geçidi eklemek, kaldırmak veya HTTP istek ve yanıt üstbilgileri v2 SKU ile güncelleştirme sağlar. Daha fazla bilgi için [uygulama ağ geçidi ile yeniden HTTP üstbilgileri](rewrite-http-headers.md)
- **Anahtar kasası tümleştirmeyi (Önizleme)** : Uygulama ağ geçidi v2, etkin HTTPS dinleyicileri için bağlı sunucu sertifikaları için (genel önizlemede) anahtar kasası ile tümleştirmeyi destekler. Daha fazla bilgi için [sertifikaları Key Vault ile SSL sonlandırma](key-vault-certs.md).
- **Azure Kubernetes Service giriş denetleyicisine (Önizleme)** : Application Gateway v2 giriş denetleyicisine Giriş bir Azure Kubernetes Service (AKS kümesi olarak bilinen AKS) için kullanılacak Azure Application Gateway sağlar. Daha fazla bilgi için [belgeleri sayfasını](https://azure.github.io/application-gateway-kubernetes-ingress/).
- **Performans iyileştirmeleri**: En fazla 5 X daha iyi SSL SKU sunar v2 standart/WAF SKU karşılaştırıldığında performans yük boşaltma.
- **Daha hızlı dağıtım ve güncelleştirme zamanı** v2 SKU standart/WAF SKU karşılaştırıldığında daha hızlı dağıtım ve güncelleştirme süresi sağlar. Bu ayrıca, WAF yapılandırma değişikliklerini de içerir.

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Desteklenen bölgeler

WAF_v2 SKU ve Standard_v2 aşağıdaki bölgelerde kullanılabilir: Kuzey Orta ABD, Güney Orta ABD, Batı ABD, Batı ABD 2, Doğu ABD, Doğu ABD 2, Orta ABD, Kuzey Avrupa, Batı Avrupa, Güneydoğu Asya, Fransa Orta, Birleşik Krallık Batı, Japonya Doğu, Japonya Batı. Ek bölgeler gelecekte eklenecektir.

## <a name="pricing"></a>Fiyatlandırma

V2 SKU ile fiyatlandırma modeli tarafından tüketim temelli ve örnek sayılarını veya boyutları artık ekli değil. V2 SKU fiyatlandırması, iki bileşenden oluşur:

- **Sabit fiyat** -saatlik budur (ya da kısmi saat) Standard_v2 veya WAF_v2 ağ geçidi sağlamak için fiyat.
- **Kapasite Birimi fiyat** -sabit maliyete ek olarak ücretlendirilir, tüketim tabanlı maliyet budur. Kapasite birimi ücretlendirmesi ayrıca saatlik olarak veya kısmen saatlik olarak hesaplanır. Kapasite biriminde üç boyut bulunur: işlem birimi, kalıcı bağlantılar ve aktarım hızı. İşlem birimi, kullanılan işlemci kapasitesini ölçer. İşlem birimi etkileyen TLS bağlantılarını/sn, URL yeniden yazma hesaplamaları ve WAF kural işleme faktörlerdir. Kalıcı bağlantı belirli bir fatura aralık application gateway'e yerleşik TCP bağlantılarının ölçümüdür. Belirtilen bir fatura aralık içinde sistem tarafından işlenen ortalama megabit/sn aktarım hızıdır.

Her bir kapasite birimi, en fazla oluşur: birim veya 2500 kalıcı bağlantılar veya 2.22 MB/sn aktarım hızı 1 işlem.

Birim kılavuzu işlem:

- **Standard_v2** -her işlem birimi RSA 2048 bit anahtar TLS sertifikası ile saniyede yaklaşık 50 bağlantıyı sahiptir.
- **WAF_v2** - her birim, yaklaşık saniyede 10 eşzamanlı isteği destekleyebilir, 2 KB GET/GÖNDERİ daha az trafik % 70'in ile 70-%30 karışımını istekleri için işlem ve yüksek kaldı. WAF performans yanıt boyutu tarafından şu anda etkilenmez.

> [!NOTE]
> Her örnek, şu anda yaklaşık 10 kapasite birimleri destekleyebilir.
> İşlem birimi ele alınan istek sayısı durumunda WAF gelen istek boyutu ve TLS sertifika anahtar boyutu, anahtar değişim algoritması, üstbilgi yeniden gibi çeşitli ölçütlere bağlıdır. İşlem birimi başına istek oranını belirlemek için uygulama testleri almanızı öneririz. Kapasite Birimi hem işlem birimi faturalama başlamadan önce bir ölçü olarak kullanılabilir hale getirilir.

Aşağıdaki tabloda, örnek fiyatları gösterir ve yalnızca gösterme amaçlıdır.

**Fiyatlandırma ABD Doğu bölgesinde**:

|              SKU adı                             | Sabit fiyat ($/ saat)  | Kapasite Birimi Fiyat ($/ CU-saat)   |
| ------------------------------------------------- | ------------------- | ------------------------------- |
| Standard_v2                                       |    0.20             | 0.0080                          |
| WAF_v2                                            |    0.36             | 0.0144                          |

Diğer fiyatlandırma bilgileri için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/application-gateway/). Faturalandırma, 1 Temmuz 2019 üzerinde başlatmak üzere zamanlandı.

**Örnek 1**

Bir uygulama ağ geçidi Standard_v2 beş örnek sabit kapasite ile otomatik ölçeklendirmeyi el ile ölçeklendirme modu olmadan sağlanır.

Sabit fiyat 744(hours) = * $0,20 $148.8 = <br>
Kapasite Birimi 744 = (saat) * örnek başına 10 kapasite birimi * 5 örnekleri * $0.008 kapasite birimi saatte $297.6 =

Toplam Fiyat = $148.8 + $297.6 $446.4 =

**Örnek 2**

Bir ay için bir uygulama ağ geçidi standard_v2 sağlanır ve isteğe bağlı olarak bu süre boyunca 25 yeni SSL bağlantıları/sn, 8.88 MB/sn veri aktarımını ortalama alır. Bağlantılarını süreli kısa olduğunu varsayarsak, fiyatınızın olacaktır:

Sabit fiyat 744(hours) = * $0,20 $148.8 =

Kapasite Birimi fiyatı 744(hours) = * en fazla (saniye başına bağlantılar için 25/50 işlem birimi, aktarım hızı için 8.88/2.22 kapasite birimi) * $0.008 = 744 * 4 * 0.008 $23.81 =

Toplam Fiyat = $148. 23.81 8 + = $172.61

> [!NOTE]
> Max işlevi içinde bir değer çiftinin en büyük değeri döndürür.

**Örnek 3**

Bir uygulama ağ geçidi WAF_v2 bir ay için sağlanır. Bu süre boyunca 25 yeni SSL bağlantıları/sn, 8.88 MB/sn veri aktarımını ortalama aldığı ve saniyede 80 isteği yapar. Bağlantıları kısa olduğunu varsayarak beklenir ve uygulama için işlem birimi hesaplama 10 RPS işlem birimi desteklediğini, fiyatınızın olacaktır:

Sabit fiyat 744(hours) = * $0.36 $267.84 =

Kapasite Birimi fiyatı 744(hours) = * Maks (işlem birimi Max(25/50 for connections/sec, 80/10 WAF RPS), aktarım hızı için 8.88/2.22 kapasite birimi) * $0.0144 = 744 * 8 * 0.0144 $85.71 =

Toplam Fiyat = $267.84 + $85.71 $353.55 =

> [!NOTE]
> Max işlevi içinde bir değer çiftinin en büyük değeri döndürür.

## <a name="scaling-application-gateway-and-waf-v2"></a>Application Gateway ve WAF v2 ölçeklendirme

Application Gateway ve WAF yapılandırılabilir iki modda ölçeklendirmek için:

- **Otomatik ölçeklendirme** - etkin, otomatik ölçeklendirme uygulama ağ geçidi ve WAF v2 SKU'ları uygulama trafiği gereksinimlerine göre ölçeği artırın veya azaltın. Bu mod, uygulamanız için daha iyi esneklik sunar ve uygulama ağ geçidi boyutu veya örnek sayısının tahmin gereğini ortadan kaldırır. Bu mod ağ geçidi sağlanan en yüksek kapasite için öngörülen en fazla trafik yükü çalışmak üzere gerektirmeyen maliyetten tasarruf sağlar. Müşteriler, minimum ve maksimum isteğe bağlı olarak örnek sayımı belirtmelisiniz. Uygulama ağ geçidi ve WAF v2, trafiği olmaması durumunda bile belirtilen en az örnek sayısı 'un altına düşersek yoksa, kapasite alt sınırı sağlar. Bu en düşük kapasiteden bile tüm trafik olmaması için faturalandırılırsınız. Ayrıca isteğe bağlı olarak, uygulama ağ geçidi örnekleri belirtilen sayıda ölçeklendirilemez sağlar bir en fazla örnek sayısı belirtebilirsiniz. Ağ Geçidi tarafından sunulan trafik miktarı faturalandırılmaya devam. Örnek sayısı 0'dan 125 için değişebilir. En fazla örnek sayısı için varsayılan değer 20'dir belirtilmediyse.
- **El ile** -ağ geçidi otomatik ölçeklendirme burada olmaz el ile modu alternatif olarak seçebilirsiniz. Hangi uygulama ağ geçidi veya WAF işleyebilir, değerinden daha fazla trafik varsa bu modda, trafiği kaybına yol açabilir. El ile modu ile örnek sayısı belirtme zorunludur. Örnek sayısı 1 ile 125 örneklerine farklılık gösterebilir.

## <a name="feature-comparison-between-v1-sku-and-v2-sku"></a>SKU v1 ve v2 SKU arasında özellik karşılaştırması

Aşağıdaki tabloda her SKU ile sunulan özellikler karşılaştırılmaktadır.

|                                                   | v1 SKU   | v2 SKU   |
| ------------------------------------------------- | -------- | -------- |
| Otomatik ölçeklendirme                                       |          | &#x2713; |
| Bölge artıklığı                                   |          | &#x2713; |
| Statik VIP                                        |          | &#x2713; |
| Azure Kubernetes Service (AKS) giriş denetleyicisine |          | &#x2713; |
| Azure Anahtar Kasası tümleştirme                       |          | &#x2713; |
| HTTP (S) üst bilgileri yeniden yazma                           |          | &#x2713; |
| URL tabanlı yönlendirme                                 | &#x2713; | &#x2713; |
| Birden çok site barındırma                             | &#x2713; | &#x2713; |
| Trafik yeniden yönlendirmesi                               | &#x2713; | &#x2713; |
| Web uygulaması güvenlik duvarı (WAF)                    | &#x2713; | &#x2713; |
| Güvenli Yuva Katmanı (SSL) sonlandırma            | &#x2713; | &#x2713; |
| Uçtan uca SSL şifrelemesi                         | &#x2713; | &#x2713; |
| Oturum benzeşimi                                  | &#x2713; | &#x2713; |
| Özel hata sayfaları                                | &#x2713; | &#x2713; |
| WebSocket desteği                                 | &#x2713; | &#x2713; |
| HTTP/2 desteği                                    | &#x2713; | &#x2713; |
| Bağlantı boşaltma                               | &#x2713; | &#x2713; |

> [!NOTE]
> Otomatik ölçeklendirme v2 SKU artık destekliyor [varsayılan sistem durumu araştırmalarının](application-gateway-probe-overview.md#default-health-probe) otomatik olarak kendi arka uç havuzundaki tüm kaynakların durumunu izleyin ve iyi durumda olmadığı kabul bu arka uç üyeler vurgulayın. Varsayılan durum yoklaması, herhangi bir özel araştırma yapılandırması sahip olmayan arka uçları için otomatik olarak yapılandırılır. Daha fazla bilgi için bkz. [sistem durumu araştırmalarının application Gateway'i](application-gateway-probe-overview.md).

## <a name="differences-with-v1-sku"></a>V1 SKU ile farkları

|Fark|Ayrıntılar|
|--|--|
|Kimlik doğrulama sertifikası|Desteklenmiyor.<br>Daha fazla bilgi için [ile Application Gateway uçtan uca SSL'ne genel bakış](ssl-overview.md#end-to-end-ssl-with-the-v2-sku).|
|Standard_v2 ve standart Application Gateway, aynı alt ağda karıştırma|Desteklenmiyor|
|Kullanıcı tanımlı yol (UDR) uygulama ağ geçidi alt ağı üzerinde|Desteklenmiyor|
|Gelen bağlantı noktası aralığı için NSG| -65200 ila 65535 Standard_v2 için SKU<br>-65503 için 65534 standart SKU için.<br>Daha fazla bilgi için [SSS](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet).|
|Azure Tanılama'da Performans Günlükleri|Desteklenmiyor.<br>Azure ölçümleri kullanılmalıdır.|
|Faturalandırma|Faturalandırma, 1 Temmuz 2019 üzerinde başlatmak üzere zamanlandı.|
|FIPS modundayken|Bunlar şu anda desteklenmemektedir.|
|ILB yalnızca modu|Bu şu anda desteklenmiyor. Genel ve ILB modu birlikte desteklenir.|
|Netwatcher tümleştirme|Desteklenmiyor.|
|Azure Güvenlik Merkezi tümleştirmesi|Henüz kullanılamıyor.

## <a name="migrate-from-v1-to-v2"></a>v1'den v2'ye geçiş

Azure PowerShell Betiği, otomatik ölçeklendirme SKU v2, v1 uygulama ağ geçidi/WAF'den geçirmenize yardımcı olmak için PowerShell galerisinde kullanılabilir. Bu betik yapılandırma v1 geçidinizden kopyalamanıza yardımcı olur. Trafik geçiş yine sizin sorumluluğunuzdur. Daha fazla bilgi için [geçirme Azure Application Gateway v1'den v2](migrate-v1-v2.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: Azure Application Gateway - Azure portalı ile doğrudan web trafiği](quick-create-portal.md)
- [Azure PowerShell kullanarak bir ayrılmış sanal IP adresiyle bir otomatik ölçeklendirme, bölge yedekli uygulama ağ geçidi oluşturma](tutorial-autoscale-ps.md)
- Daha fazla bilgi edinin [Application Gateway](overview.md).
- Daha fazla bilgi edinin [Azure Güvenlik Duvarı](../firewall/overview.md).
