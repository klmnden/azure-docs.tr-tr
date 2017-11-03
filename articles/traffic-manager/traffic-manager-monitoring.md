---
title: "Azure trafik Yöneticisi uç nokta izleme | Microsoft Docs"
description: "Bu makalede nasıl trafik Yöneticisi uç nokta izleme ve otomatik uç nokta yük devretme yüksek kullanılabilirlik uygulamaları dağıtmak Azure müşterilere yardımcı olmak için kullandığı anlamanıza yardımcı olabilir"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: fff25ac3-d13a-4af9-8916-7c72e3d64bc7
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2017
ms.author: kumud
ms.openlocfilehash: 3b30aa04854b779c25582abafc0f9ebba65b71ba
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="traffic-manager-endpoint-monitoring"></a>Trafik Yöneticisi uç nokta izleme

Azure Traffic Manager yerleşik uç nokta izleme ve otomatik uç nokta yük devretme içerir. Bu özellik Azure bölgesi hatalar dahil olmak üzere son nokta hatalarına karşı dayanıklı yüksek kullanılabilirlik uygulamaları sunmanıza yardımcı olur.

## <a name="configure-endpoint-monitoring"></a>Uç nokta izlemeyi yapılandırma

Uç noktası izleme yapılandırmak için aşağıdaki ayarları Traffic Manager profilinize belirtmeniz gerekir:

* **Protokol**. HTTP, HTTPS veya TCP protokol olarak trafik Yöneticisi uç noktanızı yoklama zaman, sistem durumunu denetlemek için kullandığını seçin. HTTPS izleme, SSL sertifikası geçerli--olup, yalnızca sertifika bulunduğunu denetler doğrulamaz.
* **Bağlantı noktası**. İstek için kullanılan bağlantı noktası seçin.
* **Yol**. Bu yapılandırma ayarının hangi yolunu belirtmek için gerekli bir ayardır yalnızca HTTP ve HTTPS protokolleri için geçerli değil. Bu ayar için bir hata Protokolü sonuçlarında izleme TCP sağlama. TCP protokolü için göreli yol ve Web sayfası veya izleme eriştiğinde dosyanın adını verin. Eğik çizgi (/), göreli yolu geçerli bir giriştir. Bu değer dosyasının kök dizininde (varsayılan) olduğunu gösterir.
* **Yoklama aralığı**. Bu değer, ne sıklıkta bir uç nokta Traffic Manager yoklama aracısından sistem durumu denetlenme belirtir. Burada iki değerleri belirtebilirsiniz: 30 (normal yoklama) saniye ile 10 saniye (Hızlı Yoklama). Hiçbir değer verdiyse, profil 30 saniye varsayılan değerini ayarlar. Ziyaret [trafik Yöneticisi fiyatlandırma](https://azure.microsoft.com/pricing/details/traffic-manager) Hızlı Yoklama fiyatlandırma hakkında daha fazla bilgi için sayfa.
* **Hatalarının sayısı izin**. Bu değer, o uç noktası sağlıksız olarak işaretleme önce bir trafik Yöneticisi yoklama Aracısı göstereceği kaç hataları belirtir. Değeri 0 ile 9 arasında değişebilir. Bir değeri 0 anlamına gelir, bu uç sorunlu olarak tek bir izleme hata neden olabilir. Herhangi bir değer belirtilirse, varsayılan değer 3'ün kullanır.
* **Zaman aşımı izleme**. Bu özellik, bir sistem durumu denetimi araştırma uç noktasına gönderilirken bir hata denetleme trafik Yöneticisi yoklama Aracısı olduğunu düşünmeden önce beklemesi gereken süre miktarını belirtir. Yoklama aralığı 30 saniye olarak ayarlarsanız, zaman aşımı değeri 5 ile 10 saniye arasında ayarlayabilirsiniz. Herhangi bir değer belirtilirse, varsayılan değer 10 saniye olarak kullanır. Yoklama aralığı 10 saniye olarak ayarlarsanız, zaman aşımı değeri 5-9 saniye arasında bir değer ayarlayabilirsiniz. Hiçbir zaman aşımı değeri belirtilirse, varsayılan değer 9 saniye olarak kullanır.

![Trafik Yöneticisi uç nokta izleme](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

**Şekil 1: Trafik Yöneticisi uç nokta izleme**

## <a name="how-endpoint-monitoring-works"></a>Uç nokta izleme nasıl çalışır

İzleme Protokolü HTTP veya HTTPS ayarlanırsa, trafik Yöneticisi Araştırma Aracı protokolü, bağlantı noktası ve göreli yol verilen kullanarak uç noktaya bir GET isteği yapar. 200 Tamam yanıt geri alır, bu uç sağlıklı olarak değerlendirilir. Yanıt farklı bir değer ise veya, sonra aracıyı yeniden çalışır (Bu ayar 0 ise hiçbir yeniden dener yapılır), numarası hataları izin ayarına göre yoklama trafik Yöneticisi belirtilen zaman aşımı süresi içinde yanıt alınmazsa. Ardışık sayısı izin numarası, hataları ayarından daha yüksek ise, bu uç sorunlu olarak işaretlenir. 

Trafik Yöneticisi Araştırma Aracı izleme protokol TCP ise, belirtilen bağlantı noktası kullanarak bir TCP bağlantısı isteği başlatır. Uç nokta bağlantı kurmak için bir yanıt istek için yanıt verirse, bu sistem durumu denetimi başarılı işaretlenmiş ve trafik Yöneticisi yoklama Aracısı TCP bağlantısı sıfırlar. Yanıt farklı bir değer ise veya zaman aşımı süresi içinde yanıt alınmazsa belirtilmezse, aracı yeniden çalışır (Bu ayar 0 hiçbir yeniden çalışır hale getirilir), numarası hataları izin ayarına göre yoklama trafik Yöneticisi. Ardışık sayısı izin numarası, hataları ayarından daha yüksek ise, bu uç sağlıksız olarak işaretlenmiş.

Tüm durumlarda trafik Yöneticisi birden çok konumlardan araştırmaları ve her bölge içinde ardışık hatası belirleme olur. Bu aynı zamanda uç noktaları sistem durumu araştırmalarının trafik Yöneticisi'nden yoklama aralığı için kullanılan ayarından daha yüksek bir sıklıkta almadığı anlamına gelir.

>[!NOTE]
>HTTP veya HTTPS protokolü izleme için yaygın bir uç nokta tarafında, uygulamanızda - Örneğin, /health.aspx özel bir sayfa uygulamak için uygulamadır. Bu yolu, izleme için kullanarak, uygulamaya özgü denetimleri, performans sayaçları denetleme veya veritabanı kullanılabilirlik doğrulama gibi gerçekleştirebilirsiniz. Bu özel denetimler bağlı olarak, uygun bir HTTP durum kodu sayfasını döndürür.

Trafik Yöneticisi profili içindeki tüm uç noktaları izleme ayarlarını paylaşır. Farklı uç noktalar için farklı izleme ayarları kullanmanız gerekiyorsa, oluşturabileceğiniz [iç içe trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings).

## <a name="endpoint-and-profile-status"></a>Uç nokta ve profil durumu

Etkinleştirme ve trafik Yöneticisi profillerinizi ve uç noktaları devre dışı bırakabilirsiniz. Ancak, bir sonuç trafik Yöneticisi'nin ayarlarını ve işlemleri otomatik olarak bir uç nokta durumu değişikliği de oluşabilir.

### <a name="endpoint-status"></a>Uç nokta durumu

Etkinleştirmek veya belirli bir uç noktası devre dışı bırakabilirsiniz. Hala sağlıklı olabilir, temel alınan hizmet etkilenmez. Uç nokta durumu değiştirme trafik Yöneticisi Profil uç kullanılabilirliğini denetler. Bir uç nokta durumu devre dışı bırakıldığında, trafik Yöneticisi, sistem durumunu denetlemez ve uç nokta bir DNS yanıtına dahil edilmez.

### <a name="profile-status"></a>Profil durumu

Profil durumu ayarı kullanarak, etkinleştirebilir veya belirli bir profili devre dışı bırakın. Uç nokta durumu tek bir uç nokta etkiler, profil durumu tüm uç noktalar dahil tüm profil etkiler. Bir profili devre dışı bıraktığınızda, uç noktaları için sistem durumu denetlenmez ve uç nokta bir DNS yanıtına dahil edilir. Bir [NXDOMAIN](https://tools.ietf.org/html/rfc2308) yanıt kodu için DNS sorgusu döndürülür.

### <a name="endpoint-monitor-status"></a>Uç nokta izleme durumu

Uç nokta izleme durumu uç noktasının durumu gösteren bir trafik Yöneticisi tarafından oluşturulan bir değerdir. Bu ayarı el ile değiştirilemiyor. Uç nokta izleme durumu, uç nokta izleme sonuçlarını ve yapılandırılmış uç noktası durumu birleşimidir. Uç nokta izleme durumu olası değerler aşağıdaki tabloda gösterilmektedir:

| Profil durumu | Uç nokta durumu | Uç nokta izleme durumu | Notlar |
| --- | --- | --- | --- |
| Devre dışı |Etkin |Etkin olmayan |Profili devre dışı bırakıldı. Uç nokta durumu etkindir ancak profil durumu (devre dışı) önceliklidir. Devre dışı profilleri uç noktalarını izlenmeyen. Bir NXDOMAIN yanıt kodu için DNS sorgusu döndürülür. |
| &lt;tüm&gt; |Devre dışı |Devre dışı |Uç noktası devre dışı bırakıldı. Devre dışı uç noktaları izlenmeyen. Uç nokta DNS yanıtları bulunmaz, bu nedenle, trafiği almaz. |
| Etkin |Etkin |Çevrimiçi |Uç nokta izlenir ve sağlıklı durumda. DNS yanıtları bulunur ve trafik alabilir. |
| Etkin |Etkin |Düşürülmüş |Uç nokta izleme sistem durumu denetimi başarısız oluyor. Uç nokta DNS yanıtları bulunmaz ve trafik almaz. <br>Bu istisna tüm uç noktaları bozulduğunu varsa, bu durumda bunların tümünün sorgu yanıtta döndürülen kabul edilir olduğunu).</br>|
| Etkin |Etkin |CheckingEndpoint |Uç nokta izlenen, ancak ilk araştırmasını sonuçlarını henüz alınamadı. CheckingEndpoint genellikle ekleyerek veya bir uç nokta profilinde etkinleştirme hemen sonra oluşan geçici bir durumdur. Bu durumdaki bir uç nokta DNS yanıtları bulunur ve trafik alabilir. |
| Etkin |Etkin |Durduruldu |Uç noktalarını için bulut hizmeti veya web uygulamasının çalışmıyor. Bulut hizmeti veya web uygulaması ayarlarını kontrol edin. Bu, iç içe geçmiş tür uç noktasını uç nokta ise ve alt profili devre dışı bırakılmış veya etkin değil de oluşabilir. <br>Bir uç nokta durduruldu durumu olan izlenmiyor. DNS yanıtları bulunmaz ve trafik almaz. Bu istisna tüm uç noktaları bozulduğunu varsa, bu durumda bunların tümünün sorgu yanıtında döndürülecek olarak kabul edilir ' dir.</br>|

Uç nokta izleme durumu iç içe geçmiş uç noktaları için nasıl hesaplandığını hakkında daha fazla bilgi için bkz [iç içe trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md).

### <a name="profile-monitor-status"></a>Profil İzleyici durumu

Profil İzleyici durumu, yapılandırılmış profil durumu ve tüm uç noktalar için uç nokta İzleyici durum değerleri birleşimidir. Olası değerler aşağıdaki tabloda açıklanmıştır:

| (Yapılandırıldığı gibi) profil durumu | Uç nokta izleme durumu | Profil İzleyici durumu | Notlar |
| --- | --- | --- | --- |
| Devre dışı |&lt;tüm&gt; veya bir profille tanımlanmış uç nokta yok. |Devre dışı |Profili devre dışı bırakıldı. |
| Etkin |En az bir uç nokta düşürülür. |Düşürülmüş |Hangi uç noktaları daha fazla ilgilenilmesi belirlemek için tek bitiş noktası durum değerleri gözden geçirin. |
| Etkin |En az bir uç nokta durumu çevrimiçi olur. Uç nokta yok Degraded durumuna sahip. |Çevrimiçi |Hizmet trafiği kabul ediyor. Başka bir eylem gerekli değildir. |
| Etkin |En az bir uç nokta CheckingEndpoint durumudur. Çevrimiçi veya Degraded durumunu hiçbir noktalarıdır. |CheckingEndpoints |Bu geçiş durumu oluşturduysanız veya etkin bir profili oluşur. Uç nokta durumu ilk kez olup olmadığı denetleniyor. |
| Etkin |Profildeki tüm uç noktaları durumları devre dışı veya durdurulmuş olan veya hiç tanımlanmış uç nokta profiline sahip. |Etkin olmayan |Uç nokta yok etkindir, ancak profil hala etkin. |

## <a name="endpoint-failover-and-recovery"></a>Uç nokta yük devretme ve kurtarma

Trafik Yöneticisi düzenli aralıklarla sağlıksız uç noktalar dahil tüm uç durumunu denetler. Trafik Yöneticisi bir uç nokta iyi olur ve geri dönüş getirir algılar.

Aşağıdaki olaylardan biri gerçekleştiğinde bir uç nokta sağlam değil:
- İzleme Protokolü, HTTP veya HTTPS ise:
    - (Farklı 2xx kodu veya bir 301/302 yeniden yönlendirme dahil) 200 olmayan yanıt aldı.
- İzleme protokol TCP olduğunda: 
    - Bağlantı kurma girişiminde trafik Yöneticisi tarafından gönderilen eşitleme isteğine yanıt olarak ACK veya Eşitlemeye ACK dışındaki bir yanıt aldı.
- Zaman aşımı. 
- Erişilebilir olmaması uç kaynaklanan herhangi diğer bağlantı sorunu.

Sorun giderme başarısız denetimleri hakkında daha fazla bilgi için bkz: [sorun giderme düşürülmüş durumu Azure Traffic Manager üzerindeki](traffic-manager-troubleshooting-degraded.md). 

Şekil 2'deki aşağıdaki zaman çizelgesi aşağıdaki ayarlara sahip trafik Yöneticisi uç noktası izleme işlemini ayrıntılı bir açıklaması olup: olan HTTP protokolü izleme, yoklama aralığı 30 saniye, toleranslı sayısı olan 3, zaman aşımı değeri 10 saniye, DNS TTL ise 30 saniyedir.

![Trafik Yöneticisi uç nokta yük devretme ve yeniden çalışma dizisi](./media/traffic-manager-monitoring/timeline.png)

**Şekil 2: Yük devretme ve kurtarma trafik Yöneticisi uç noktası sırası**

1. **ALMA**. Her uç noktası için sistem izleme trafik Yöneticisi İzleme ayarlarında belirtilen yol bir GET isteği gerçekleştirir.
2. **200 TAMAM**. İzleme sistemi, 10 saniye içinde döndürülecek HTTP 200 Tamam iletisine bekliyor. Bu yanıt aldığında, hizmet kullanılabilir olduğunu algılar.
3. **Denetimler arasındaki 30 saniye**. Uç noktası sistem durumu denetimi, her 30 saniyede yinelenir.
4. **Hizmet kullanılamıyor**. Hizmet kullanılamaz duruma gelir. Trafik Yöneticisi kadar sonraki sistem durumu denetimi bilmez.
5. **İzleme yolu erişmeyi denediği**. İzleme sistemi, bir GET isteği yapar, ancak 10 saniye zaman aşımı süresi içinde bir yanıt almaz (Alternatif olarak, 200 yanıt alınması). 30 saniyelik aralıklarda üç kez daha, daha sonra çalışır. Bir deneme başarılı olursa, deneme sayısı sıfırlanır.
6. **Durumunu ayarlamak için Degraded**. Dördüncü ardışık hatasından sonra izleme sistemi kullanılamaz uç nokta durumu Degraded işaretler.
7. **Trafik diğer Uç noktalara yönlendirilmesiyle**. Trafik Yöneticisi DNS ad sunucularını güncelleştirilir ve trafik Yöneticisi uç noktası artık DNS sorgularına yanıt verir. Yeni bağlantıları için kullanılabilir diğer uç noktaları yönlendirilir. Ancak, bu bitiş noktası içeren önceki DNS yanıtları hala yinelemeli DNS sunucuları ve DNS istemcileri tarafından önbelleğe alınmış olabilir. İstemcileri DNS Önbellek süresi doluncaya kadar uç nokta kullanmaya devam edin. DNS Önbellek süresi gibi istemciler yeni DNS sorgularını olun ve farklı uç noktalar için yönlendirilir. Önbellek süresi trafik Yöneticisi profili, örneğin, 30 saniye TTL ayarında tarafından denetlenir.
8. **Sistem durumu denetler devam**. Trafik Yöneticisi uç noktası durumunu Degraded durum sahipken denetlemeye devam eder. Trafik Yöneticisi uç noktası için sistem durumu geri döndüğünde algılar.
9. **Hizmet gelen tekrar çevrimiçi**. Hizmet kullanılabilir hale gelir. Uç nokta izleme sistemi, sonraki sistem durumu denetimi gerçekleştirir kadar Degraded durumunu trafik Yöneticisi'nde korur.
10. **Hizmet sürdürür trafiği**. Trafik Yöneticisi bir GET isteği gönderir ve 200 Tamam durumu yanıtı alır. Hizmet bir sağlık durumuna geri döndü. Trafik Yöneticisi ad sunucuları güncelleştirilir ve hizmetin DNS adı DNS yanıtları çıkışı el başlar. Trafik uç noktasına diğer uç noktaları süresinin dolmasını ve varolan bağlantılar diğer uç noktalar olarak sonlandırıldı bu iade önbelleğe alınmış DNS yanıtlarını olarak döndürür.

    > [!NOTE]
    > Trafik Yöneticisi DNS düzeyinde çalıştığı için herhangi bir uç nokta var olan bağlantılara etkilemek olamaz. Trafik Yöneticisi uç noktaları (veya değiştirilen profili ayarları, yük devretme veya yeniden çalışma sırasında) arasındaki trafiği yönlendirir, kullanılabilir uç noktaları için yeni bağlantı yönlendirir. Ancak, diğer uç noktaları Bu oturumlar durduruluncaya kadar mevcut bağlantıları üzerinden trafiği almaya devam edebilir. Varolan bağlantılarından boşaltmak trafiği etkinleştirmek için uygulamaları her bitiş noktası ile kullanılan bir oturum süresi sınırlamanız gerekir.

## <a name="traffic-routing-methods"></a>Trafik yönlendirme yöntemleri

Bir uç nokta Degraded durumunda olduğunda DNS sorgularına yanıt artık döndürülür. Bunun yerine, alternatif bir uç nokta seçilen döndürülen ve. Alternatif uç nokta nasıl seçilir profilinde yapılandırılan trafik yönlendirme yöntemini belirler.

* **Öncelik**. Uç noktaları öncelikli listesi oluşturur. Listedeki ilk kullanılabilir uç nokta her zaman döndürülür. Bir uç nokta durumu düzeyi, bir sonraki kullanılabilir uç nokta döndürülür.
* **Ağırlıklı**. Herhangi bir kullanılabilir uç nokta rastgele atanan ağırlıkları ve kullanılabilir bir uç ağırlıkları göre seçilir.
* **Performans**. Bitiş noktası son kullanıcı için en yakın döndürülür. Bu uç kullanılamıyorsa, trafik Yöneticisi uç noktalarına sonraki en yakın Azure bölgesinde trafik taşınır. Kullanarak performans trafik yönlendirme için diğer yük devretme planlarını yapılandırabilirsiniz [iç içe trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region).
* **Coğrafi**. Sorgu isteği temel alarak coğrafi konum hizmet eşlenmiş uç noktası IP'ın döndürülür. Bu uç kullanılamıyorsa, coğrafi konum yalnızca bir uç nokta profilde eşlenebilir olduğundan başka bir uç noktası, yük devretme için seçilmez (daha fazla ayrıntı bulunan [SSS](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)). Coğrafi yönlendirme kullanırken en iyi uygulama, iç içe trafik Yöneticisi profilleri ile birden fazla uç profil uç noktalar olarak kullanmak üzere müşteriler öneririz.

Daha fazla bilgi için bkz: [Traffic Manager trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md).

> [!NOTE]
> Tüm uygun uç noktaları düşürülmüş durumuna sahip normal trafik yönlendirme davranışını bir özel durum oluşur. "En iyi çaba" denemesi trafik Yöneticisi yapar ve *tüm Degraded durum uç noktaları gerçekte bir çevrimiçi olarak durumdaysa yanıt*. Bu davranış herhangi bir uç nokta DNS yanıtında değil döndürülecek olabilecek alternatif için tercih edilir. Devre dışı veya durdurulmuş uç noktaları izlenmez, bu nedenle, bunlar trafiği için uygun olarak kabul edilmez.
>
> Bu durum genellikle göre hizmet hatalı yapılandırılması gibi nedeniyle oluşur:
>
> * Trafik Yöneticisi sistem engelleyen bir erişim denetim listesi [ACL] denetler.
> * İzleme bağlantı noktası veya trafik Yöneticisi profili protokolünde hatalı bir yapılandırma.
>
> Bu davranış sonucu Traffic Manager sistem durumu denetimlerinin doğru yapılandırılmamışsa, bu trafik Yöneticisi olarak ancak Yönlendirme trafiği görünür olan *olan* düzgün çalışmıyor. Ancak, bu durumda, uç nokta yük devretme genel uygulama kullanılabilirliği etkiler meydana olamaz. Profil Degraded durumu çevrimiçi bir durumu gösterir denetlemek önemlidir. Çevrimiçi durumu Traffic Manager sistem durumu denetimlerinin beklendiği gibi çalıştığını gösterir.

Sorun giderme hakkında daha fazla bilgi için sistem durumu denetimi başarısız oldu bkz [sorun giderme düşürülmüş durumu Azure Traffic Manager üzerindeki](traffic-manager-troubleshooting-degraded.md).



## <a name="next-steps"></a>Sonraki adımlar

Bilgi [trafik Yöneticisi nasıl çalışır?](traffic-manager-how-traffic-manager-works.md)

Daha fazla bilgi edinmek [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen

Bilgi edinmek için nasıl [bir Traffic Manager profili oluşturma](traffic-manager-manage-profiles.md)

[Degraded durumu sorun giderme](traffic-manager-troubleshooting-degraded.md) trafik Yöneticisi uç noktada
