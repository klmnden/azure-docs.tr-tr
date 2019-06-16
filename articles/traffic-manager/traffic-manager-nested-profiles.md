---
title: Azure'da iç içe Traffic Manager profilleri
titlesuffix: Azure Traffic Manager
description: Bu makalede, Azure Traffic Manager'ın 'İç içe profiller' özelliği açıklanmaktadır.
services: traffic-manager
documentationcenter: ''
author: asudbring
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/22/2018
ms.author: allensu
ms.openlocfilehash: 3c5459d0474ecd45501e634c4777fa178386183c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071147"
---
# <a name="nested-traffic-manager-profiles"></a>İç İçe Traffic Manager profilleri

Traffic Manager trafik yönlendirme yöntemleri, Traffic Manager'ın her son kullanıcıdan hangi uç noktaya trafik alması gereken nasıl seçer denetlemenize izin veren bir dizi içerir. Daha fazla bilgi için [Traffic Manager trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md).

Traffic Manager profillerine tek bir trafik yönlendirme yöntemini belirtir. Ancak, tek bir Traffic Manager profili tarafından sağlanan yönlendirme çok daha karmaşık trafik yönlendirme gerektiren senaryolar da vardır. Traffic Manager profillerini birden fazla trafik yönlendirme yöntemini avantajlarını birleştirin iç içe yerleştirebilirsiniz. İç içe profiller, daha büyük destek ve daha karmaşık uygulama dağıtımları için varsayılan Traffic Manager davranışı geçersiz kılma olanak tanır.

Aşağıdaki örnekler, çeşitli senaryolarda iç içe Traffic Manager profilleri kullanmayı gösterir.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Örnek 1: 'Performans' ve 'Ağırlıklı' trafik yönlendirme birleştirme

Bir uygulama aşağıdaki Azure bölgeleri içinde dağıttığınız varsayalım: Batı ABD, Batı Avrupa ve Doğu Asya. Kullanıcıya en yakın bölgeyi trafiği dağıtmak için Traffic Manager'ın 'Performans' trafik yönlendirme yöntemini kullanın.

![Tek Traffic Manager profili][4]

Şimdi, hizmetiniz için bir güncelleştirme Bunu daha yaygın olarak sıralı önce test etmek istediğiniz varsayalım. Küçük bir yüzdesine test dağıtımınız için trafiği yönlendirmek için 'ağırlıklı' trafik yönlendirme yöntemini kullanmak istiyorsunuz. Var olan üretim dağıtımlarıyla birlikte yönetebileceksiniz sınama dağıtımı Batı Avrupa'da ayarlarsınız.

Her iki 'ağırlıklı' birleştirilemez ve ' performans trafiği yönlendirme tek profilde. Bu senaryoyu desteklemek için iki Batı Avrupa uç noktaları ve 'Ağırlıklı' trafik yönlendirme yöntemini kullanarak bir Traffic Manager profili oluşturun. Ardından, bu 'alt' profili 'parent' profiline bir uç nokta ekleyin. Üst profili, hala performans trafiği yönlendirme yöntemini kullanır ve diğer genel dağıtımlar uç noktalar olarak içerir.

Bu örnekte, aşağıdaki diyagramda gösterilmektedir:

![İç İçe Traffic Manager profilleri][2]

Bu yapılandırmada üzerinden üst profili yönlendirilen trafik, trafiği bölgeler arasında normalde dağıtır. Batı Avrupa içinde iç içe geçmiş profil üretim ve test uç noktaları atanan ağırlıklara göre trafiği dağıtır.

Üst profili 'Performans' trafik yönlendirme yöntemini kullandığında, her bir uç nokta konumu atanması gerekir. Uç nokta yapılandırdığınızda konumu atanır. Dağıtımınız için en yakın Azure bölgesi seçin. Azure bölgeleri, Internet gecikme tablo tarafından desteklenmeyen konum değerlerdir. Daha fazla bilgi için ['Performans' Traffic Manager trafik yönlendirme yöntemine](traffic-manager-routing-methods.md#performance).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Örnek 2: İç içe geçmiş profillerinde uç nokta izleme

Traffic Manager, etkin bir şekilde her bir hizmet uç noktasının durumunu izler. Bir uç nokta kötü durumda, Traffic Manager, hizmet kullanılabilirliğini korumak için alternatif uç noktaları kullanıcıların yönlendirir. Bu uç nokta izleme ve yük devretme davranışı, tüm trafik yönlendirme yöntemleri için geçerlidir. Daha fazla bilgi için [Traffic Manager uç nokta izleme](traffic-manager-monitoring.md). Uç nokta izleme, iç içe profiller için farklı şekilde çalışır. İç içe geçmiş profilleri ile üst profili sistem durumu denetimleri alt doğrudan gerçekleştirmez. Bunun yerine, sistem alt profilin uç nokta, alt profili genel durumunu hesaplamak için kullanılır. Bu sistem durumu bilgileri, iç içe geçmiş profil hiyerarşisinde yukarı yayılır. Üst profili bu toplanan sistem durumu alt profili trafiği yönlendirmek belirlemek için kullanır. Bkz: [SSS](traffic-manager-FAQs.md#traffic-manager-nested-profiles) iç içe geçmiş profilleri sistem durumu izleme tam Ayrıntılar için.

Önceki örnekte döndürmek, Batı Avrupa'daki Üretim dağıtımı başarısız olduğunu varsayalım. Varsayılan olarak 'alt' profili sınama dağıtımı için tüm trafiği yönlendirir. Test dağıtımını de başarısız olursa, tüm alt uç noktalar sağlıksız olduğu alt profili trafiği etkinleştirilmediğinden üst profili belirler. Ardından, üst profil, diğer bölgelere trafiği dağıtır.

![İç içe profil yük devretme (varsayılan davranış)][3]

Bu düzenleme ile mutlu olabilir. Veya tüm trafiği Batı Avrupa için sınırlı bir alt kümesi trafik yerine sınama dağıtımı artık tıklarsam ilgili olabilir. Sınama dağıtımı durumunu bağımsız olarak, Batı Avrupa'daki Üretim dağıtımı başarısız olduğunda diğer bölgelere yük devretme istiyorsunuz. Bu yük devretmeyi etkinleştirmek için üst profilinde bir uç nokta olarak alt profilini yapılandırırken 'MinChildEndpoints' parametresini belirtebilir. Parametresi, alt profildeki kullanılabilir uç noktaları en az sayısını belirler. Varsayılan değer, '1' dir. Bu senaryo için MinChildEndpoints değer 2 olarak ayarlayın. Bu eşiğin altına üst profil kullanılamaz olarak tüm alt profili göz önünde bulundurur ve trafik diğer Uç noktalara yönlendirir.

Aşağıdaki şekilde, bu yapılandırma gösterilmektedir:

![Profil yük devretme 'MinChildEndpoints' ile iç içe geçmiş = 2][4]

> [!NOTE]
> Trafik yönlendirme 'Öncelik' yöntemi, tek bir uç nokta tüm trafiği dağıtır. Bu nedenle var. az amaçlı '1' dışındaki bir MinChildEndpoints ayarı için bir alt profili

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Örnek 3: 'Performans' trafik yönlendirme, öncelikli yük devretme bölgeleri

Son kullanıcılara en düşük ağ gecikme süresi açısından "en yakın" uç noktasına yönlendirilir farklı coğrafi konumlarda uç noktaları varsa varsayılan 'Performans' trafik yönlendirme yöntemini davranışıdır.

Ancak, Batı ABD Batı Avrupa trafik yükünü devretme tercih eder ve her iki bitiş kullanılamadığında trafiğini diğer bölgelere yalnızca doğrudan varsayalım. Bu çözüm 'Priority' trafik yönlendirme yöntemi ile bir alt profili kullanarak oluşturabilirsiniz.

!['Performans' trafiğine tercihe bağlı yük devretme ile yönlendirme][6]

Her iki bitiş noktası çevrimiçi olduğunda tüm trafiğin, Batı Avrupa uç nokta Batı ABD uç nokta daha yüksek önceliğe sahip olduğundan, Batı Avrupa uç noktasına gönderilir. Batı Avrupa başarısız olursa, Batı ABD için kendi trafik yönlendirilir. Batı Avrupa ve Batı ABD hem yalnızca başarısız olduğunda iç içe geçmiş profili ile Doğu Asya trafik yönlendirilir.

Bu düzen tüm bölgeler için yineleyebilirsiniz. Üst profilinde üç tüm uç noktalar her öncelikli yük devretme sırası sağlayan üç alt profil ile değiştirin.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Örnek 4: Aynı bölgede birden fazla uç noktalar arasında trafiği 'Performans' yönlendirmesi denetleme

Belirli bir bölgede birden fazla uç noktası olan bir profilde kullanılan trafik yönlendirme yöntemini 'Performans' varsayalım. Varsayılan olarak, bu bölgeye yönlendirilen trafik, bu bölgedeki tüm kullanılabilir uç noktalar arasında eşit olarak dağıtılır.

!['Performans' trafiği yönlendirme bölge trafik dağılımı (varsayılan davranış)][7]

Birden fazla uç nokta Batı Avrupa'da eklemek yerine, bu Uç noktalara ayrı alt profilinde içine alınır. Alt profili üst Batı Avrupa'daki yalnızca uç nokta olarak eklenir. Alt profildeki ayarları trafik dağılımı Batı Avrupa ile bu bölge içinde öncelik tabanlı veya ağırlıklı trafik yönlendirme etkinleştirerek denetleyebilirsiniz.

!['Performans' trafik yönlendirme ile özel bölge trafik dağılımı][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Örnek 5: Her uç nokta izleme ayarları

Traffic Manager sorunsuz bir şekilde geçirmek için kullandığınız varsayalım eski gelen trafiği Azure üzerinde barındırılan yeni bir bulut tabanlı sürümü web sitesine şirket içi. Eski site için site durumunu izlemek için URI giriş sayfasında kullanmak istediğiniz. Ancak yeni bulut tabanlı sürümü için özel bir izleme sayfası uyguluyor (yolu ' / monitor.aspx'), ek denetimler içerir.

![Traffic Manager uç noktası (varsayılan davranış) izleme][9]

Traffic Manager profili İzleme ayarlarında, tek bir profili içindeki tüm uç noktalar için geçerlidir. İç içe geçmiş profilleri ile farklı alt profili site başına farklı izleme ayarları tanımlamak için kullanın.

![İzleme uç noktası başına ayarlarını traffic Manager uç noktası][10]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Traffic Manager profilleri](traffic-manager-overview.md)

Bilgi edinmek için nasıl [Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png
