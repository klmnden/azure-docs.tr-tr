---
title: Trafik Yöneticisi profillerine iç içe geçmiş | Microsoft Docs
description: Bu makalede, Azure trafik Yöneticisi'nin 'İç içe geçmiş profiller' özelliği açıklanmaktadır.
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''
ms.assetid: f1b112c4-a3b1-496e-90eb-41e235a49609
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 1ac4ec2775ca9f690f5adf4f939908f8cee3f715
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23876981"
---
# <a name="nested-traffic-manager-profiles"></a>İç içe trafik Yöneticisi profilleri

Traffic Manager trafik yönlendirme yöntemleri, hangi uç noktaya trafiği her son kullanıcıdan alması gereken trafik Yöneticisi nasıl seçtiği denetlemenize izin veren bir dizi içerir. Daha fazla bilgi için bkz: [Traffic Manager trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md).

Her trafik Yöneticisi profili, tek bir trafik yönlendirme yöntemini belirtir. Ancak, tek bir trafik Yöneticisi profili tarafından sağlanan yönlendirme olandan daha karmaşık trafik yönlendirme gerektiren senaryolar vardır. Birden çok trafik yönlendirme yöntemini yararları birleştirmek için Traffic Manager profillerini yerleştirebilirsiniz. İç içe geçmiş profilleri destek daha büyük ve daha karmaşık uygulama dağıtımları için varsayılan trafik Yöneticisi davranışı geçersiz kılmanıza olanak sağlar.

Aşağıdaki örnekler, çeşitli senaryolarda iç içe trafik Yöneticisi profilleri kullanmayı gösterir.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Örnek 1: Birleştirme 'Performans' ve 'Weighted' trafik yönlendirme

Bir uygulama aşağıdaki Azure bölgelerindeki dağıtılan varsayalım: Batı ABD, Batı Avrupa ve Doğu Asya. Kullanıcıya en yakın bölgeyi trafiği dağıtmak için trafik Yöneticisi'nin 'Performans' trafik yönlendirme yöntemini kullanın.

![Tek trafik Yöneticisi profili][4]

Şimdi, hizmetiniz için bir güncelleştirme, daha fazla bilgi yaygın olarak çalışırken önce test etmek istediğiniz varsayalım. Test dağıtımınız için trafiği küçük bir yüzdesi yönlendirmek için 'ağırlıklı' trafik yönlendirme yöntemini kullanmak istediğiniz. Var olan üretim dağıtımına yanında sınama dağıtımı Batı Avrupa'da ayarlarsınız.

Hem 'Weighted' birleştiremez ve ' performans trafik yönlendirme tek bir profilde. Bu senaryoyu desteklemek için iki Batı Avrupa uç noktaları ve 'Weighted' trafik yönlendirme yöntemini kullanarak bir Traffic Manager profili oluşturun. Ardından, bu 'alt' profili 'parent' profili için bir uç noktası olarak ekleyin. Üst profili hala performans trafik yönlendirme yöntemini kullanır ve diğer genel dağıtımlar uç noktalar olarak içerir.

Aşağıdaki diyagramda bu örnek gösterilmektedir:

![İç içe trafik Yöneticisi profilleri][2]

Bu yapılandırmada, üst profili yönlendirilen trafiği trafiği bölgeler arasında normalde dağıtır. Batı Avrupa içinde iç içe profil üretim ve test uç noktalarına atanan ağırlıklara göre trafiği dağıtır.

Üst Profil 'Performans' trafik yönlendirme yöntemini kullandığında, her bitiş konumu atanması gerekir. Uç nokta yapılandırdığınızda konumu atanır. Dağıtımınız için en yakın Azure bölgesi seçin. Azure bölgeleri Internet gecikme tablo tarafından desteklenmeyen konum değerlerdir. Daha fazla bilgi için bkz: ['Performans' Traffic Manager trafik yönlendirme yöntemini](traffic-manager-routing-methods.md#performance).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Örnek 2: İç içe geçmiş profillerinde uç nokta izleme

Trafik Yöneticisi etkin olarak her hizmet uç noktası durumunu izler. Bir uç nokta sağlıksız durumda, trafik Yöneticisi, hizmet kullanılabilirliğini korumak için kullanıcılar alternatif Uç noktalara yönlendirir. Bu uç nokta izleme ve yük devretme davranış tüm trafik yönlendirme yöntemleri için geçerlidir. Daha fazla bilgi için bkz: [trafik Yöneticisi uç noktası izleme](traffic-manager-monitoring.md). Uç nokta izleme iç içe profil için farklı şekilde çalışır. İç içe geçmiş profilleriyle üst profili sistem durumu denetimlerinin alt doğrudan gerçekleştirmez. Bunun yerine, alt profilinin uç noktaları durumunu alt profili genel durumunu hesaplamak için kullanılır. Bu sistem durumu bilgileri, iç içe profil hiyerarşisinde yukarı yayılır. Üst profili toplanmış bu sistem durumu doğrudan trafiğe alt profiline belirlemek için kullanır. Bkz: [SSS](traffic-manager-FAQs.md#traffic-manager-nested-profiles) iç içe profil sistem durumu izleme hakkında ayrıntılar için.

Önceki örneği döndüren, Batı Avrupa üretim dağıtımında başarısız varsayalım. Varsayılan olarak, 'alt' profili sınama dağıtımı için tüm trafiğini yönlendirir. Test dağıtımını de başarısız olursa, tüm alt uç noktaları sağlıksız olduğundan alt profilinin trafik alması gerektiğini değil üst profili belirler. Ardından, üst profili diğer bölgeler için trafiği dağıtır.

![İç içe profil yük devretme (varsayılan davranış)][3]

Bu düzenleme ile mutluluk olabilir. Veya tüm trafiği Batı Avrupa için sınırlı alt trafik yerine sınama dağıtımı için şimdi gittiği ilgili olabilir. Sınama dağıtımı durumunu bağımsız olarak, Batı Avrupa üretim dağıtımında başarısız olduğunda diğer bölgeler için yük devri istiyorsunuz. Bu yük devretme etkinleştirmek için üst profilindeki bir uç nokta olarak alt profili yapılandırırken 'MinChildEndpoints' parametresini belirtebilir. Parametre en az alt profildeki kullanılabilir uç nokta sayısını belirler. Varsayılan değer, '1' dir. Bu senaryo için 2'ye MinChildEndpoints değerini ayarlayın. Bu eşiğin altına üst profil kullanılamaz hale gelmesine tüm alt profili göz önünde bulundurur ve diğer uç noktalardan trafiğini yönlendirir.

Aşağıdaki şekilde, bu yapılandırma gösterilmektedir:

!['MinChildEndpoints' ile profil yük devretme iç içe geçmiş = 2][4]

> [!NOTE]
> Trafik yönlendirme 'Priority' yöntemi, tüm trafik tek bir uç nokta dağıtır. Bu nedenle bulunmaktadır az amacı '1' dışında bir MinChildEndpoints ayarı için bir alt profili.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Örnek 3: trafiği 'Performans' akışındaki öncelikli yük devretme bölgeleri

Trafik yönlendirme 'Performans' yöntemi için varsayılan davranış, uç nokta en yakın sonraki aşırı yükleme ve basamaklı bir dizi hata neden önlemek üzere tasarlanmıştır. Bir uç nokta başarısız olduğunda, bu uç noktasına yönergelerine uygun tüm trafiği bir uç noktalara tüm bölgeler arasında eşit olarak dağıtılır.

!['Performans' trafiği ile varsayılan yük devretme yönlendirme][5]

Ancak, Batı ABD Batı Avrupa trafik yükünü devretme tercih ve her iki uç noktaları kullanılamaz duruma geldiğinde diğer bölgelere trafiği yalnızca doğrudan varsayalım. Trafik yönlendirme 'Priority' yöntemiyle bir alt profili kullanarak bu çözüm oluşturabilirsiniz.

!['Performans' trafiğine tercihe bağlı yük devretme kümelemesiyle yönlendirme][6]

Batı Avrupa endpoint Batı ABD endpoint daha yüksek önceliğe sahip olduğundan, her iki uç noktaları çevrimiçi olduğunda tüm trafiği Batı Avrupa uç noktasına gönderilir. Batı Avrupa başarısız olursa, trafiğin Batı ABD yönlendirilir. Batı Avrupa ve Batı ABD yalnızca başarısız olduğunda iç içe geçmiş profili ile Doğu Asya trafik yönlendirilir.

Tüm bölgeler için bu deseni yineleyebilirsiniz. Üst profilindeki tüm üç uç noktaları her bir öncelikli yük devretme sırası sağlayan üç alt profilleriyle değiştirin.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Örnek 4: 'Performans' aynı bölgede birden çok uç noktalar arasında trafiği yönlendirme denetleme

'Birden fazla uç belirli bir bölgede bulunan bir profilde trafik yönlendirme yöntemini kullanılan performans' varsayalım. Varsayılan olarak, bu bölgeye yönlendirilen trafiği, bu bölgedeki tüm kullanılabilir uç noktaları arasında eşit olarak dağıtılır.

!['Performans' Yönlendirme bölge trafik dağılımı (varsayılan davranış) trafiği][7]

Birden çok uç nokta Batı Avrupa'da eklemek yerine, bu uç ayrı alt profilinde iliştirilir. Alt profili üst Batı Avrupa'da yalnızca uç noktası olarak eklenir. Alt profildeki ayarları, o bölge içinde öncelik tabanlı veya ağırlıklı trafik yönlendirme etkinleştirerek Batı Avrupa ile trafik dağılımı kontrol edebilirsiniz.

!['Performans' trafiği özel bölge trafik dağılımı ile yönlendirme][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Örnek 5: Her uç nokta izleme ayarları

Trafik Yöneticisi sorunsuz geçirmek için kullandığınız varsayalım eski trafiğinden Azure üzerinde barındırılan yeni bir bulut tabanlı sürüme web sitesini şirket içi. Site durumunu izlemek için giriş sayfası URI kullanmak istediğiniz eski site için. Ancak yeni bulut tabanlı sürümü için özel bir izleme sayfası uyguluyorsanız (yol ' / monitor.aspx'), ek denetimleri içerir.

![Trafik Yöneticisi uç noktası (varsayılan davranış) izleme][9]

Trafik Yöneticisi profili izleme ayarları tek bir profili içindeki tüm uç noktaları için geçerlidir. İç içe geçmiş profilleriyle, farklı alt profili her site için farklı izleme ayarlarını tanımlamak için kullanın.

![Trafik Yöneticisi uç noktası uç nokta başına ayarlarla izleme][10]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [trafik Yöneticisi profilleri](traffic-manager-overview.md)

Bilgi edinmek için nasıl [bir Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

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
