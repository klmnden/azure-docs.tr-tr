---
title: Azure Traffic Manager uç nokta izleme | Microsoft Docs
description: Bu makalede, Traffic Manager uç nokta izleme ve otomatik bir uç nokta yük devretme yüksek kullanılabilirlik uygulamalarını dağıtmalarını Azure müşterilerine yardımcı olmak için nasıl kullandığını anlamanıza yardımcı olabilir
services: traffic-manager
author: asudbring
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/04/2018
ms.author: allensu
ms.openlocfilehash: 7aee68ef41b696549aa1db4386d467b55cd2d981
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071068"
---
# <a name="traffic-manager-endpoint-monitoring"></a>Traffic Manager uç nokta izleme

Azure Traffic Manager, yerleşik uç nokta izleme ve otomatik bir uç nokta yük devretme içerir. Bu özellik Azure bölgesi hatalar dahil olmak üzere uç nokta hatalarına karşı dayanıklı olan yüksek kullanılabilirlik uygulamaları sunmanıza yardımcı olur.

## <a name="configure-endpoint-monitoring"></a>Uç nokta izlemeyi yapılandırma

Uç nokta izleme yapılandırmak için aşağıdaki ayarları Traffic Manager profilinizin belirtmeniz gerekir:

* **Protokol**. HTTP, HTTPS veya TCP Traffic Manager uç noktanızı yoklama zaman sistem durumu denetlemek için kullandığı protokolü olarak seçin. HTTPS izleme SSL sertifikanızı geçerli--olup olmadığını, yalnızca sertifika mevcut olduğunu denetler doğrulamaz.
* **Bağlantı noktası**. İstek için kullanılan bağlantı noktasını seçin.
* **Yol**. Bu yapılandırma ayarının hangi yolunu belirtmek için gerekli bir ayardır yalnızca HTTP ve HTTPS protokolleri için geçerlidir. Bu ayar TCP protokolü sonuçları hata izleme için sağlama. HTTP ve HTTPS protokolü için göreli yolunu ve Web sayfası veya izleme erişen dosyanın adını verin. Eğik çizgi (/), göreli yol geçerli bir giriştir. Bu değer, dosyanın kök dizininde (varsayılan) olduğunu gösterir.
* **Özel üst bilgi ayarları** Traffic Manager uç noktaları bir profili altındaki gönderir, bu yapılandırma, eklemenize yardımcı olur, sistem durumu için belirli HTTP üstbilgileri ayarının denetler. Özel üst bilgiler, bu profildeki tüm uç noktalar için geçerli olacak şekilde, bir profil düzeyinde and / or yalnızca uç noktanın için geçerli bir uç nokta düzeyinde belirtilebilir. Özel üst bilgiler, bir konak üstbilgisi belirterek hedeflerine doğru şekilde yönlendirilmesini sistem durumu denetimleri çok müşterili bir ortamda uç noktalarına sahip olmak için kullanabilirsiniz. Farklı şekilde işler ve bu ayar HTTP (S) istekleri, Traffic Manager tanımlamak için kullanılan benzersiz üst kaynağı ekleyerek de kullanabilirsiniz. En fazla sekiz başlığı: değer çiftleri seprated virgülle belirtebilirsiniz. Örneğin, "header1:value1, header2:value2". 
* **Beklenen durum kodu aralığı** Bu ayar 200 299, 301 301 biçiminde birden fazla başarı kodu aralığı belirtmenize olanak sağlar. Traffic Manager, sistem durumu denetimi başlatıldığında bir uç noktasından yanıt olarak bu durum kodları alınır, bu Uç noktalara sağlıklı olarak işaretler. En fazla 8 durum kodu aralığı belirtebilirsiniz. Bu ayar, yalnızca HTTP ve HTTPS protokolü ve tüm uç noktalar için geçerlidir. Bu ayarı Traffic Manager profili düzeyinde ve varsayılan başarılı durum kodu 200 değeri tanımlanır.
* **Yoklama aralığı**. Bu değer, bir uç nokta Traffic Manager araştırma aracılardan gelen sistem durumu için ne sıklıkla kontrol belirtir. Burada iki değer belirtebilirsiniz: 30 saniye (normal algılama) ve 10 saniye (hızlı algılama). Hiçbir değer sağlanmışsa, profil 30 saniyelik bir varsayılan değere ayarlar. Ziyaret [Traffic Manager fiyatlandırma](https://azure.microsoft.com/pricing/details/traffic-manager) Hızlı Yoklama fiyatlandırması hakkında daha fazla bilgi edinmek için sayfa.
* **Hatalarının sayısı kaydırmadan kaçınma şansınız**. Bu değer, bu uç nokta iyi durumda olmayan olarak işaretleme önce bir Traffic Manager Araştırma Aracı göstereceği kaç hataları belirtir. Değeri 0 ile 9 arasında değişebilir. 0 değeri tek bir izleme hata sorunlu olarak işaretleneceğini bu endpoint neden olabilir. Hiçbir değer belirtilmemişse, varsayılan değer 3'ün kullanır.
* **Yoklama zaman aşımı**. Bu özellik, bir sistem durumu kontrolü araştırma uç noktasına gönderilirken bir hata denetleyen Traffic Manager Araştırma Aracı olduğunu düşünmeden önce beklemesi gereken süreyi belirtir. Yoklama aralığı 30 saniye olarak ayarlarsanız, 5 ve 10 saniye arasında bir zaman aşımı değeri ayarlayabilirsiniz. Hiçbir değer belirtilmemişse, varsayılan değer 10 saniye olarak kullanır. Yoklama aralığı 10 saniye olarak ayarlarsanız, 5 ila 9 saniye arasında bir zaman aşımı değeri ayarlayabilirsiniz. Hiçbir zaman aşımı değeri belirtilmediği takdirde, 9 saniye varsayılan değeri kullanır.

    ![Traffic Manager uç nokta izleme](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

    **Şekil:  Traffic Manager uç nokta izleme**

## <a name="how-endpoint-monitoring-works"></a>Uç nokta izleme nasıl çalışır

İzleme Protokolü HTTP veya HTTPS ' ayarlanırsa, Traffic Manager Araştırma Aracı protokol, bağlantı noktası ve göreli yol verilen kullanarak uç noktaya bir GET isteği yapar. Geri 200 Tamam yanıtı alır ya da yapılandırılan tüm yanıtlar **beklenen durum kodu \*aralıkları**, sonra da bu uç nokta sağlıklı olarak kabul edilir. Yanıt farklı bir değer ise veya sonra aracıyı yeniden dener (Bu ayarı 0 ise hiçbir yeniden dener yapılır), sayı hataları kaydırmadan kaçınma şansınız ayarına göre araştırma Traffic Manager belirtilen zaman aşımı süresi içinde yanıt alınamazsa. Art arda hata sayısını, sayı hataları kaydırmadan kaçınma şansınız ayarından daha yüksekse, bu uç nokta olarak sağlıksız olarak işaretlenir. 

İzleme Protokolü TCP ise, Traffic Manager araştırma aracı kullanarak belirtilen bağlantı noktası TCP bağlantısı isteği başlatır. Bağlantı isteğine yanıt veren bir uç nokta yanıt verirse, bu sistem durumu denetimi başarılı işaretlenir ve Traffic Manager Araştırma Aracı TCP bağlantısı sıfırlar. Yanıt farklı bir değer ise veya zaman aşımı süresi içinde yanıt alınamazsa belirtilmezse, Traffic Manager aracısını yeniden dener (Bu ayarı 0 ise hiçbir yeniden dener yapılan), sayı hataları kaydırmadan kaçınma şansınız ayarına göre algılanıyor. Art arda hata sayısını, sayı hataları kaydırmadan kaçınma şansınız ayarından daha yüksekse, bu uç nokta sağlıksız olarak işaretlenir.

Tüm durumlarda, Traffic Manager, birden çok konumlardan araştırmaları ve art arda hata belirleme her bölge içinde gerçekleşir. Bu, aynı zamanda uç noktaları yoklama aralığı için kullanılan ayarından daha yüksek sıklığa sahip Traffic Manager'dan sistem durumu araştırmaları alıyorsunuz anlamına gelir.

>[!NOTE]
>HTTP veya HTTPS protokolü izleme için bir ortak uç nokta tarafında uygulamanızda - Örneğin, /health.aspx özel bir sayfa uygulamak için uygulamadır. İzleme için bu yolu kullanarak, performans sayaçları denetimi veya veritabanı kullanılabilirlik doğrulama gibi uygulamaya özel denetimleri gerçekleştirebilirsiniz. Bu özel denetimler bağlı olarak, sayfanın uygun HTTP durum kodu döndürür.

Traffic Manager profilindeki tüm uç noktaları, izleme ayarlarını paylaşın. Farklı uç noktalar için farklı izleme ayarlarını kullanmanız gerekiyorsa, oluşturabileceğiniz [iç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings).

## <a name="endpoint-and-profile-status"></a>Uç nokta ve profil durumu

Etkinleştirebilir ve Traffic Manager profillerini ve uç noktaları devre dışı bırakın. Ancak, Traffic Manager'ın bir sonuç ayarları ve işlemleri otomatik olarak uç nokta durumundaki bir değişikliği de oluşabilir.

### <a name="endpoint-status"></a>Uç nokta durumu

Etkinleştirebilir veya belirli bir uç noktayı devre dışı. Yine de iyi olabilir, temel alınan hizmete etkilenmez. Uç nokta durumunu değiştirme, uç nokta Traffic Manager profilindeki kullanılabilirliğini denetler. Bir uç nokta durumu devre dışı bırakıldığında, Traffic Manager sistem durumu denetlemez ve uç nokta bir DNS yanıtına dahil edilmez.

### <a name="profile-status"></a>Profil durumu

Profil durumu ayarını kullanarak, etkinleştirebilir veya belirli bir profili devre dışı bırakın. Uç nokta durumunu tek bir uç nokta etkiler, profil durumu tüm uç noktalar dahil olmak üzere tüm profil etkiler. Bir profili devre dışı bıraktığınızda, uç noktaları için sistem durumu denetimi ve uç nokta bir DNS yanıtına dahil edilir. Bir [NXDOMAIN](https://tools.ietf.org/html/rfc2308) yanıt kodu, DNS sorgusu döndürülür.

### <a name="endpoint-monitor-status"></a>Uç nokta izleme durumu

Uç nokta izleme durumu uç nokta durumunu gösteren bir Traffic Manager tarafından oluşturulan bir değerdir. Bu ayarı el ile değiştiremezsiniz. Uç nokta izleme durumu uç nokta izleme sonuçlarını ve yapılandırılmış uç noktası durumu birleşimidir. Uç Noktası İzleyicisi durumu olası değerler aşağıdaki tabloda gösterilmiştir:

| Profil durumu | Uç nokta durumu | Uç nokta izleme durumu | Notlar |
| --- | --- | --- | --- |
| Devre dışı |Enabled |Etkin değil |Profili devre dışı bırakıldı. Uç nokta durumu etkindir; ancak, profil durumu (devre dışı) önceliklidir. Devre dışı profillerindeki uç noktalar izlenmeyen. DNS sorgu için bir NXDOMAIN yanıt kodu döndürülür. |
| &lt;Tüm&gt; |Devre dışı |Devre dışı |Uç noktayı devre dışı bırakıldı. Devre dışı uç noktalar izlenmeyen. Uç nokta DNS yanıtlarını bulunmaz, bu nedenle, trafiği almaz. |
| Enabled |Enabled |Çevrimiçi |Uç nokta izlenir ve iyi durumda. Bu DNS yanıtları bulunur ve trafik alabilir. |
| Enabled |Enabled |Düşürüldü |Uç nokta izleme sistem durumu denetimi başarısız oluyor. Uç nokta DNS yanıtlarını dahil değildir ve trafiği almaz. <br>Bu konuda bir özel tüm uç noktalar düşürülmüş ise bu durumda tümünün sorgu yanıtta döndürülmesi kabul edilir olduğunu).</br>|
| Enabled |Enabled |CheckingEndpoint |İzlenen uç nokta, ancak birinci Araştırmanın sonuçlarını henüz alınamadı. CheckingEndpoint genellikle hemen ekleme veya profilinde bir uç noktası etkinleştirildikten sonra oluşan geçici bir durumdur. Bu durumdaki bir uç nokta DNS yanıtları bulunur ve trafik alabilir. |
| Enabled |Enabled |Durduruldu |Uç noktaya işaret eden bir web uygulaması çalışmıyor. Web uygulaması ayarlarını kontrol edin. Bu, uç nokta türü iç içe uç noktasını ise ve alt profili devre dışı bırakılır veya etkin değil de oluşabilir. <br>Durduruldu durumundaki bir uç nokta izlemesi yapılmaz. Bu DNS yanıtlarını dahil değildir ve trafiği almaz. Bu konuda bir özel tüm uç noktalar düşürülmüş ise bu durumda tümünün sorgu yanıtta döndürülmesi kabul edilecek ' dir.</br>|

İç içe uç noktalar için uç nokta izleme durumu nasıl hesaplandığını hakkında daha fazla ayrıntı için bkz [iç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md).

>[!NOTE]
> Web uygulamanızı standart katmanda veya üstünde çalışmıyor bir uç nokta durduruldu İzleyici durumu üzerinde App Service'te oluşabilir. Daha fazla bilgi için [Traffic Manager tümleştirmesi App Service ile](/azure/app-service/web-sites-traffic-manager).

### <a name="profile-monitor-status"></a>Profil İzleyicisi durumu

Profil İzleyicisi durumu, yapılandırılan profili durumu ve tüm uç noktalar için uç nokta izleme durum değerleri birleşimidir. Olası değerler aşağıdaki tabloda açıklanmıştır:

| (Yapılandırılmış gibi) profil durumu | Uç nokta izleme durumu | Profil İzleyicisi durumu | Notlar |
| --- | --- | --- | --- |
| Devre dışı |&lt;tüm&gt; veya bir profille tanımlanmış uç nokta yok. |Devre dışı |Profili devre dışı bırakıldı. |
| Enabled |En az bir uç nokta durumu düzeyi düşürüldü. |Düşürüldü |Hangi uç noktaları ilgili daha fazla dikkat etmeniz gereken belirlemek için tek bir uç nokta durum değerleri gözden geçirin. |
| Enabled |En az bir uç nokta durumu çevrimiçi olarak. Uç nokta Degraded durumuna sahip. |Çevrimiçi |Hizmet trafiği kabul ediyor. Başka bir eylem gerekli değildir. |
| Enabled |En az bir uç nokta CheckingEndpoint durumudur. Hiçbir Exchange Online veya Degraded durumu noktalarıdır. |CheckingEndpoints |Bu geçiş durumu, oluşturulan veya etkinleştirilen, bir profil olduğunda gerçekleşir. Uç nokta sistem ilk kez olup olmadığı denetleniyor. |
| Enabled |Devre dışı veya durdurulmuş profildeki tüm uç noktaların durumları olan ya da tanımlanmış uç nokta profile sahip. |Etkin değil |Uç nokta etkin olan ancak profil hala etkin. |

## <a name="endpoint-failover-and-recovery"></a>Uç nokta yük devretme ve kurtarma

Traffic Manager, düzenli aralıklarla sağlıklı uç noktalar dahil olmak üzere her uç noktasının durumunu denetler. Traffic Manager uç nokta sağlıklı duruma gelir ve geri döndürme taşır algılar.

Aşağıdaki olaylardan herhangi biri gerçekleştiğinde bir uç nokta sağlam değil:

- İzleme Protokolü, HTTP veya HTTPS ise:
    - 200 yanıt ya da belirtilen durum aralığı içermeyen bir yanıt **beklenen durum kodu aralığı** ayarı (301/302 yeniden yönlendirme veya farklı 2xx kod dahil) alınır.
- İzleme protokol TCP olduğunda: 
    - Traffic Manager tarafından gönderilen bağlantı kurma denemesi eşitleme isteğine yanıt olarak ACK veya SYN ACK dışındaki bir yanıt aldı.
- Zaman aşımı. 
- Uç noktası erişilebilir olmaması kaynaklanan herhangi başka bir bağlantı sorunu.

Sorun giderme başarısız denetimler hakkında daha fazla bilgi için bkz: [durumu Azure Traffic Manager'da düşürülmüş sorun giderme](traffic-manager-troubleshooting-degraded.md). 

Zaman çizelgesinde aşağıdaki şekilde aşağıdaki ayarlara sahip bir Traffic Manager uç nokta izleme işleminin ayrıntılı açıklamasıdır: İzleme Protokolü HTTP olduğu, araştırma aralığı 30 saniye, tolere edilen hata sayısı 3 olan, zaman aşımı değeri 10 saniye ve DNS TTL'yi, 30 saniye.

![Traffic Manager uç nokta yük devretme ve yeniden çalışma dizisi](./media/traffic-manager-monitoring/timeline.png)

**Şekil:  Traffic manager uç nokta yük devretme ve kurtarma dizisi**

1. **ALMA**. Her uç nokta için sistemin izlenmesi Traffic Manager İzleme ayarlarında belirtilen yolda bir GET isteği yapar.
2. **Belirtilen izleme ayarlarını Traffic Manager profili 200 Tamam'ı veya özel kod aralığı** . Bir HTTP 200 OK izleme sistemi bekliyor veya veya özel kod aralığı 10 saniye içinde döndürülecek ayarları ileti izleme Traffic Manager profili belirtilmedi. Bu yanıt aldığında, hizmet kullanılabilir tanır.
3. **Denetimler arasındaki 30 saniye**. Uç nokta sistem durumu denetimi, her 30 saniyede tekrarlanır.
4. **Hizmet kullanılamıyor**. Hizmet kullanılamaz. Traffic Manager kadar sonraki sistem durumu denetimi oynatacaklarını bilmez.
5. **İzleme yola erişme girişiminde**. İzleme sistemi için bir GET isteği yapar, ancak bu 10 saniyelik zaman aşımı süresi içinde yanıt almaz (Alternatif olarak, bir 200 yanıt alınması). Ardından üç kez daha, 30 saniyelik aralıklarla çalışır. Ardından bir deneme başarılı olursa, deneme sayısı sıfırlanır.
6. **Durumu ayarlamak için Degraded**. Dördüncü ardışık hatadan sonra kullanılabilir uç nokta durumu izleme sistemi Degraded işaretler.
7. **Trafik, diğer Uç noktalara yönlendirilmesiyle**. Traffic Manager DNS ad sunucuları güncelleştirilir ve Traffic Manager uç noktası artık DNS sorgularına yanıt verir. Yeni bağlantılar kullanılabilir başka Uç noktalara yönlendirilir. Ancak, bu uç nokta içeren önceki DNS yanıtları hala yinelenen DNS sunucuları ve DNS istemcileri tarafından önbelleğe alınabilir. İstemciler, DNS Önbellek süresi dolana kadar uç noktasını kullanmaya devam eder. DNS Önbellek süre sonu gibi istemciler yeni DNS sorguları ve farklı Uç noktalara yönlendirilir. Önbelleğe alma süresi, Traffic Manager profili, örneğin, 30 saniye TTL ayar tarafından denetlenir.
8. **Durum denetimleri devam**. Traffic Manager Degraded durumu sahipken uç noktasının durumunu denetlemek devam eder. Traffic Manager için sistem durumu uç noktanın döndürdüğü algılar.
9. **Hizmet yeniden çevrimiçi gelen**. Hizmet kullanılabilir hale gelir. İzleme sistemi, sonraki sistem durumu denetimi başarılı olana dek uç nokta Traffic Manager'da Degraded durumu korur.
10. **Trafik hizmeti sürdürür**. Traffic Manager, bir GET isteği gönderir ve 200 Tamam durumu yanıtı alır. Hizmeti, sağlıklı bir duruma döndürdü. Traffic Manager ad sunucularıyla güncelleştirilir ve hizmetin DNS adı, DNS yanıtlarının kullanıma el başlar. Trafiği uç noktaya döndüren diğer uç noktalardan süresinin dolmasını ve varolan bağlantılar diğer uç noktalar olarak sonlandırılır önbelleğe alınan DNS yanıtları döndürür.

    > [!NOTE]
    > Traffic Manager DNS düzeyinde çalışır çünkü herhangi bir uç nokta için varolan bağlantılar etkilemek olamaz. Uç noktalar (veya değiştirilen profili ayarları, yük devretme ve yeniden çalışma sırasında) arasındaki trafiği yönlendirir, Traffic Manager yeni bağlantıları kullanılabilir Uç noktalara yönlendirir. Ancak, diğer uç noktalardan bu oturum sonlandırılana kadar mevcut bağlantıları üzerinden trafiği almaya devam edebilir. Mevcut bağlantılardan boşaltma trafiği etkinleştirmek için uygulamalar her uç nokta ile kullanılan oturum süresi sınırlamanız gerekir.

## <a name="traffic-routing-methods"></a>Trafik yönlendirme yöntemleri

Bir uç nokta Degraded durumuna sahip olduğunda, DNS sorgularına yanıt artık döndürülür. Bunun yerine, başka bir uç nokta seçilen döndürdü ve. Nasıl alternatif uç nokta seçilir profilinde yapılandırılan trafik yönlendirme yöntemini belirler.

* **Öncelik**. Uç noktaları bir öncelik listesi oluşturur. Listedeki ilk kullanılabilir uç nokta her zaman döndürülür. Bir uç nokta durumu düzeyi düşürüldü, ardından sonraki kullanılabilir uç nokta döndürülür.
* **Ağırlıklı**. Tüm kullanılabilir uç nokta rastgele atanan ağırlıkları ve diğer kullanılabilir uç noktalar ağırlıkları göre seçilir.
* **Performans**. Son kullanıcıya en yakın uç nokta döndürülür. Uç noktanın kullanılamıyorsa, Traffic Manager trafik sonraki en yakın Azure bölgesinde uç noktalarına taşır. Performans trafiği yönlendirme için alternatif bir yük devretme planları kullanarak yapılandırabilirsiniz [iç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region).
* **Coğrafi**. Sorgu isteği temel alarak coğrafi konumu hizmet eşlenmiş uç IP'ler döndürülür. Uç noktanın kullanılamıyorsa, yalnızca bir profilinde bir uç nokta için bir coğrafi konumda eşlenebilir olduğundan başka bir uç nokta yük devretme için seçilir değil (daha fazla ayrıntı bulunan [SSS](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)). Coğrafi yönlendirme kullanırken en iyi uygulama, müşterilerinin profil uç noktaları olarak birden fazla uç nokta ile iç içe Traffic Manager profillerini kullanmasını öneririz.
* **Birden çok değerli** IPv4/IPv6 adresleriyle eşlenen birden fazla uç nokta döndürülür. Bu profil için bir sorgu alındığında, sağlıklı uç noktalar göre döndürülen **en fazla kayıt sayısı yanıt** belirttiğiniz değeri. İki uç nokta, yanıtları varsayılan sayısıdır.
* **Alt ağ** bir IP adresi aralıklarını kümesine eşlenmiş uç nokta döndürülür. Bu IP adresinden bir istek alındığında, bu IP adresi için eşlenmiş bir uç nokta döndürdü. 

Daha fazla bilgi için [Traffic Manager trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md).

> [!NOTE]
> Tüm uygun uç noktalar düşürülmüş durumuna sahip olduğunda normal trafik yönlendirme davranışı için bir özel durum oluşur. "En iyi çaba" deneme trafik Yöneticisi yapar ve *tüm Degraded durum uç noktalar gerçekten çevrimiçi bir durumda ise olarak yanıt veren*. Bu davranış, herhangi bir uç nokta DNS yanıtında döndürmediğine olacaktır alternatif için tercih edilir. Devre dışı veya durdurulmuş uç noktaları izlenmez; bu nedenle, bunlar trafiği için uygun olarak kabul edilmez.
>
> Bu durum genellikle hizmet tarafından hatalı yapılandırılması gibi neden olur:
>
> * Traffic Manager sistem durumu denetimleri engelleyen bir erişim denetim listesi [ACL].
> * İzleme bağlantı noktası veya Traffic manager profili protokolünde hatalı bir yapılandırma.
>
> Traffic Manager sistem durumu denetimleri düzgün şekilde yapılandırılmadıysa, gibi ancak Traffic Manager yönlendirme trafiği görünebilir bu davranışının sonucu olan *olduğu* düzgün bir şekilde çalışıyor. Ancak, bu durumda, uç nokta yük devretme genel uygulama kullanılabilirliği etkiler şey olamaz. Profil Degraded durumu bir çevrimiçi durumunu göstermektedir denetlemek önemlidir. Traffic Manager sistem durumu denetimleri beklendiği gibi çalıştığını çevrimiçi bir durumu gösterir.

Sorun giderme hakkında daha fazla bilgi için sistem durumu denetimleri başarısız oldu bkz [durumu Azure Traffic Manager'da düşürülmüş sorun giderme](traffic-manager-troubleshooting-degraded.md).

## <a name="next-steps"></a>Sonraki adımlar

Bilgi [Traffic Manager nasıl çalışır?](traffic-manager-how-it-works.md)

Daha fazla bilgi edinin [trafik yönlendirme yöntemlerini](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen

Bilgi edinmek için nasıl [Traffic Manager profili oluşturma](traffic-manager-manage-profiles.md)

[Degraded durumu sorunlarını giderme](traffic-manager-troubleshooting-degraded.md) üzerinde bir Traffic Manager uç noktası
