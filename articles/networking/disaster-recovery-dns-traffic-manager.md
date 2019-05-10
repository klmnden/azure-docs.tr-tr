---
title: Azure DNS ve Traffic Manager'ı kullanarak olağanüstü durum kurtarma | Microsoft Docs
description: Azure DNS ve Traffic Manager'ı kullanarak olağanüstü durum kurtarma çözümleri genel bakış.
services: dns
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/08/2018
ms.author: kumud
ms.openlocfilehash: a560cc526e73f3ce7e851f2a545f9b16fa53b423
ms.sourcegitcommit: 1d257ad14ab837dd13145a6908bc0ed7af7f50a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65501734"
---
# <a name="disaster-recovery-using-azure-dns-and-traffic-manager"></a>Azure DNS ve Traffic Manager kullanarak olağanüstü durum kurtarma

Olağanüstü durum kurtarma uygulama işlevselliği ciddi bir kaybından kurtarma üzerinde odaklanır. Bir olağanüstü durum kurtarma çözümü seçmek için iş ve teknoloji sahipleri bir olağanüstü durum sırasında gibi - gerekli kullanılamıyor, kısmen kullanılabilir işlevsellik veya Gecikmeli kullanılabilirlik aracılığıyla işlevsellik düzeyine önce belirlemelisiniz veya tam olarak kullanılabilir.
Çoğu Kurumsal müşteriler için bir uygulama ya da altyapı düzeyinde yük devretme karşı dayanıklılık bir çok bölgeli mimarisini seçim. Müşteriler, yük devretme ve yedekli bir mimari üzerinden yüksek kullanılabilirlik elde etmek için savaşta çeşitli yaklaşımlar seçebilir. Popüler yaklaşım bazıları şunlardır:

- **Etkin olmayan hazırda bekleme ile Aktif-Pasif**: Yük devretme gereksinimini oluncaya kadar bu yük devretme çözümde VM'ler ve diğer bekleme bölgede çalışan gereçlere giden etkin değil. Ancak, üretim ortamına biçiminde yedeklemeler, VM görüntüleri veya Resource Manager şablonları, farklı bir bölgeye çoğaltılır. Bu yük devretme mekanizması, uygun maliyetli ancak tam bir yük devretme gerçekleştirmek için çok uzun sürüyor.
 
    ![Etkin olmayan hazırda bekleme ile Aktif/Pasif](./media/disaster-recovery-dns-traffic-manager/active-passive-with-cold-standby.png)
    
    *Şekil - soğuk bekleme olağanüstü durum kurtarma yapılandırması ile Aktif/Pasif*

- **Pilot ışık ile Aktif/Pasif**: Bu yük devretme çözümde, bekleme ortamı en az bir yapılandırma ile ayarlanır. Kurulum, yalnızca çalışan yalnızca bir minimal ve önemli uygulama kümesi desteklemek için gerekli hizmet yok. Kendi yerel biçiminde bu senaryo yalnızca en az bir işlevi yürütmek ancak ölçek artırıp toplu üretim iş yükünün bir yük devretme gerçekleşirse yararlanmak için ek hizmetler oluşturma.
    
    ![Pilot ışık ile Aktif/Pasif](./media/disaster-recovery-dns-traffic-manager/active-passive-with-pilot-light.png)
    
    *Şekil: Pilot ışık olağanüstü durum kurtarma yapılandırması ile Aktif/Pasif*

- **Etkin bekleme ile Aktif/Pasif**: Bu yük devretme çözümdeki bekleme bölge önceden warmed ve temel yük almaya hazır, otomatik ölçeklendirme etkinleştirilir ve tüm örneklerini çalışmaya devam. Bu çözüm tam üretim yük gerçekleştirilecek ölçeklenmez ancak işlevsel ve tüm hizmetleri ve çalışıyor. Bu çözüm, pilot ışık yaklaşım genişletilmiş bir sürümüdür.
    
    ![Etkin bekleme ile Aktif/Pasif](./media/disaster-recovery-dns-traffic-manager/active-passive-with-warm-standby.png)
    
    *Şekil: Orta Gecikmeli bekleme olağanüstü durum kurtarma yapılandırması ile Aktif/Pasif*
    
Yük devretme ve yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz: [Azure uygulamaları için olağanüstü durum kurtarma](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).


## <a name="planning-your-disaster-recovery-architecture"></a>Olağanüstü durum kurtarma Mimarinizi planlama

Olağanüstü durum kurtarma Mimarinizi ayarı doğrultusunda teknik iki unsur vardır:
-  Örnekler, veri ve yapılandırmaları birincil ve hazır bekleyen ortamlar arasında çoğaltmak için bir dağıtım mekanizması kullanıyor. Bu tür bir olağanüstü durum kurtarma, yerel olarak Microsoft Azure iş ortağı Gereçleri/Hizmetleri Veritas veya NetApp gibi aracılığıyla Azure Site Recovery aracılığıyla gerçekleştirilebilir. 
- Yöneltmektir bekleme site birincil siteden ağ/web trafiği için bir çözüm geliştirme. Bu tür bir olağanüstü durum kurtarma, Azure DNS, Azure trafiği Manager(DNS) veya üçüncü taraf genel yük dengeleyicileri gerçekleştirilebilir.

Bu makalede, ağ ve Web trafiğini yeniden yönlendirmesi aracılığıyla yaklaşımları sınırlıdır. Azure Site kurtarma işlemini ayarlama yönergeleri için bkz. [Azure Site Recovery belgeleri](https://docs.microsoft.com/azure/site-recovery/).
DNS, DNS genellikle genel ve dış veri merkezine ve tüm bölge veya kullanılabilirlik bölgesi (AZ) düzeyinde hatalardan yalıtılmış olduğundan, ağ trafiğini yöneltmektir en verimli mekanizmalarını biridir. DNS tabanlı yük devretme mekanizması kullanabilirsiniz ve Azure'da iki DNS hizmetleri aynı şekilde - Azure DNS (yetkili DNS) ve Azure Traffic Manager'ın (DNS tabanlı akıllı trafik yönlendirme) gerçekleştirebilirsiniz. 

DNS'de, bu makalede sağlanan çözümleri tartışmak için yaygın olarak kullanılan birkaç kavramları anlamak önemlidir:
- **DNS A kaydı** – A kayıtlarının bir etki alanı bir IPv4 adresine işaret eden işaretçileridir. 
- **CNAME veya Canonical adı** -bu kayıt türü için başka bir DNS kaydını işaret edecek şekilde kullanılır. CNAME bir IP adresi ancak bunun yerine işaretçi ile IP adresi içeren bir kayda yanıt vermiyor. 
- **Ağırlıklı yönlendirme** – bir hizmet uç noktalarına ağırlık ilişkilendirin ve ardından üzerinde atanan ağırlıklara göre trafiği dağıtmak seçebilirsiniz. Bu yönlendirme yöntemini dört trafik yönlendirme mekanizmalarını Traffic Manager içinde biridir. Daha fazla bilgi için [ağırlıklı yönlendirme yöntemini](../traffic-manager/traffic-manager-routing-methods.md#weighted).
- **Öncelikli yönlendirme** – öncelikli yönlendirme uç nokta sistem durumu denetimleri dayanır. Varsayılan olarak, Azure Traffic manager tüm trafiği için en yüksek öncelik uç noktasına gönderir ve bir hata veya olağanüstü durum, Traffic Manager ikincil uç noktaya trafiği yönlendirir. Daha fazla bilgi için [öncelikli yönlendirme yöntemini](../traffic-manager/traffic-manager-routing-methods.md#priority).

## <a name="manual-failover-using-azure-dns"></a>Azure DNS kullanarak el ile yük devretme
Azure DNS'ye el ile yük devretme çözümü olağanüstü durum kurtarma için yedekleme siteye yük devretme için standart DNS mekanizmasını kullanır. Azure DNS ile el ile seçeneği etkin olmayan hazırda bekleme veya pilot ışık yaklaşım ile birlikte kullanıldığında en iyi şekilde çalışır. 

![Azure DNS kullanarak el ile yük devretme](./media/disaster-recovery-dns-traffic-manager/manual-failover-using-dns.png)

*Şekil - el ile yük devretme Azure DNS kullanma*

Çözüm için yapılan tahminler şunlardır:
- Birincil ve ikincil uç noktaları, genellikle değişmez statik IP'ye sahip. Birincil site için IP 100.168.124.44 ve ikincil site için IP 100.168.124.43 varsayalım.
- Her iki birincil ve ikincil site için bir Azure DNS bölgesi yok. Örneğin, birincil site için uç nokta prod.contoso.com ve yedekleme sitesi için dr.contoso.com şeklindedir. Www bilinen ana uygulama için bir DNS kaydı\.contoso.com de bulunur.   
- TTL veya kuruluştaki ayarlamak RTO SLA altında ' dir. Örneğin bir kuruluş daha iyi RTO, 60 dakika, tercihen alt'den küçük olan uygulama olağanüstü durum yanıt TTL olmalıdır, 60 dakika olarak ayarlar. 
  El ile yük devretme için Azure DNS'yi gibi ayarlayabilirsiniz:
- DNS bölgesi oluşturma
- DNS bölgesi kayıtları oluşturma
- CNAME kaydı güncelleştir

### <a name="step-1-create-a-dns"></a>1. Adım: Bir DNS oluşturma
DNS bölgesi oluşturma (örneğin, www\.contoso.com) aşağıda gösterildiği gibi:

![Azure'da bir DNS bölgesi oluşturma](./media/disaster-recovery-dns-traffic-manager/create-dns-zone.png)

*Şekil - Azure'da bir DNS bölgesi oluşturma*

### <a name="step-2-create-dns-zone-records"></a>2. Adım: DNS bölgesi kayıtları oluşturma

Bu bölge içinde üç kayıtları oluşturma (örneğin - www\.contoso.com ve prod.contoso.com dr.consoto.com) olarak aşağıdaki göster.

![DNS bölgesi kayıtları oluşturma](./media/disaster-recovery-dns-traffic-manager/create-dns-zone-records.png)

*Şekil - Azure'da DNS bölge kayıtları oluşturma*

Bu senaryoda, site, www\.contoso.com de belirtilen RTO ve üretim sitesini prod.contoso.com için işaret eden bir 30 dakika, TTL sahiptir. Bu yapılandırma normal iş işlemleri sırasında dir. Prod.contoso.com dr.contoso.com ve TTL 300 saniye veya 5 dakika olarak ayarlandı. Bir Azure hizmeti gibi Azure İzleyici ya da Azure App Insights izleme kullanabilirsiniz veya tüm iş ortağı çözümlerini Dynatrace gibi izleme, izleme veya uygulama ya da sanal altyapı düzeyinde hataları algılamak giriş büyütülmesi çözümleri bile kullanabilirsiniz.

### <a name="step-3-update-the-cname-record"></a>3. adım: CNAME kaydı güncelleştirin

Hata algılandığında, kayıt değeri dr.contoso.com için aşağıda gösterildiği gibi işaret edecek şekilde değiştirin:
       
![CNAME kaydı güncelleştir](./media/disaster-recovery-dns-traffic-manager/update-cname-record.png)

*Şekil - azure'da CNAME kaydı güncelleştirme*

30 dakika içinde bu sırada çoğu Çözümleyicileri Yenile önbelleğe alınan bölge dosyası, tüm sorgu www\.contoso.com için dr.contoso.com yönlendirilirsiniz.
Ayrıca, CNAME değeri değiştirmek için aşağıdaki Azure CLI komutunu çalıştırabilirsiniz:
 ```azurecli
   az network dns record-set cname set-record \
   --resource-group 123 \
   --zone-name contoso.com \
   --record-set-name www \
   --cname dr.contoso.com
```
Bu adım, el ile veya Otomasyon aracılığıyla gerçekleştirilebilir. El ile Konsolu aracılığıyla veya Azure CLI tarafından yapılabilir. API ve Azure SDK'sı, CNAME güncelleştirmeyi el ile müdahale gerekli olacak şekilde otomatik hale getirmek için kullanılabilir. Otomasyon, Azure işlevleri aracılığıyla veya bir üçüncü taraf izleme uygulaması veya hatta şirket içinden oluşturulabilir.

### <a name="how-manual-failover-works-using-azure-dns"></a>Azure DNS kullanarak nasıl el ile yük devretme çalışır
DNS sunucusu yük devretme veya olağanüstü durum bölgenin dışında olduğundan, karşı kapalı kalma süresi yalıtılmış. Bu kullanıcının uygun maliyetli bir kolayca yük devretme senaryosu mimari olanak tanır ve tüm zaman işleci olağanüstü durum sırasında ağ bağlantısı olduğunu varsayarak çalışır ve Çevir yapabilirsiniz. Çözüm komut dosyası, ardından bir sunucu veya betiği çalıştıran hizmet, üretim ortamına etkileyen sorun karşı yalıtılmış emin emin olmanız gerekir. Ayrıca, dünyanın dört bir yanındaki bir çözümleyici yok uzun süre önbelleğe uç noktayı tutar ve müşteriler RTO sitede erişebilir bölgeye göre ayarlanmış düşük TTL göz önünde bulundurun. Bazı prewarming ve diğer yönetim etkinliği – gerekli olabileceğinden etkin olmayan hazırda bekleme ve pilot ışık, bir de Çevir yapmadan önce yeterli süre vermek.

## <a name="automatic-failover-using-azure-traffic-manager"></a>Azure Traffic Manager kullanarak otomatik yük devretme
Karmaşık mimarileri ve birden fazla kaynak ile aynı işlevi gerçekleştirebilir varsa, kaynaklarınızın sistem durumunu denetleyin ve gelen trafiği iyi durumda olmayan resource sağlam yönlendirmek için Azure Traffic Manager'ın (DNS dayalı) yapılandırabilirsiniz Kaynak. Aşağıdaki örnekte, tam bir dağıtım hem birincil bölge hem de ikincil bölgeye sahiptir. Bu dağıtım, bulut Hizmetleri ile eşitlenmiş veritabanı içerir. 

![Azure Traffic Manager kullanarak otomatik yük devretme](./media/disaster-recovery-dns-traffic-manager/automatic-failover-using-traffic-manager.png)

*Şekil - Azure Traffic Manager kullanarak otomatik yük devretme*

Ancak, yalnızca birincil bölge etkin bir şekilde kullanıcılardan ağ istekleri işlediğinin. Yalnızca birincil bölgeye bir hizmet kesintisi yaşandığında ikincil bölgeye etkin hale gelir. Bu durumda, tüm yeni ağ istekleri ikincil bölgeye yol. Veritabanı yedeklemeden bu yana neredeyse anında, yük Dengeleyiciler sahip işaretli durumu olabilir IP'ler örnekleri her zaman en ve çalışıyorsa, bu topoloji düşük bir RTO ve herhangi bir el ile müdahale olmadan yük devretme için gitmek için bir seçenek sağlar. İkincil bir yük devretme bölge hemen birincil bölgeye hatadan sonra canlı kullanıma hazır olması gerekir.
Bu senaryo ' % s'kullanım sahip Azure Traffic Manager için ideal olan sistem durumu denetimleri http dahil olmak üzere çeşitli türleri için yerleşik bir araştırmalarla / https ve TCP. Azure Traffic manager, aşağıda açıklandığı gibi bir hata oluşursa, yük devretme için yapılandırılmış bir kural altyapısı de vardır. Traffic Manager'ı kullanarak aşağıdaki çözümü düşünelim:
- Müşteri, bir statik IP 100.168.124.44 ve statik IP adresi olarak 100.168.124.43 ile dr.contoso.com bilinen bir bölge 2 uç noktası olarak prod.contoso.com bilinen bölge 1 uç noktası vardır. 
-   Bu ortamların her birinde, bir yük dengeleyici gibi genel kullanıma yönelik özelliği aracılığıyla fronted. Yük Dengeleyici DNS tabanlı bir uç nokta veya tam etki alanı adı (FQDN) yukarıda gösterildiği şekilde yapılandırılabilir.
-   Neredeyse gerçek zamanlı çoğaltma bölge 1, bölge 2'deki tüm örnekleri olan. Ayrıca, makine görüntülerini güncel olduğundan ve tüm yazılım/yapılandırma verilerini Yama ve bölge 1'ayarlarına uygun olarak.  
-   Otomatik ölçeklendirme, önceden yapılandırılmış. 

Azure Traffic Manager ile yük devri yapılandırmak için adımlar aşağıdaki gibidir:
1. Yeni bir Azure Traffic Manager profili oluşturma
2. Traffic Manager profili içindeki uç noktalar oluşturma
3. Sistem durumu denetimi ve yük devretme yapılandırmasını ayarlayın

### <a name="step-1-create-a-new-azure-traffic-manager-profile"></a>1. Adım: Yeni bir Azure Traffic Manager profili oluşturma
Adı contoso123 ile yeni bir Azure Traffic manager profili oluşturun ve öncelikli olarak yönlendirme yöntemini seçin. Aksi takdirde, mevcut bir kaynak grubunu seçip, ile ilişkilendirmek istediğiniz önceden var olan bir kaynak grubu varsa, yeni bir kaynak grubu oluşturun.

![Traffic Manager profili oluştur](./media/disaster-recovery-dns-traffic-manager/create-traffic-manager-profile.png)

*Şekil - Traffic Manager profili oluşturma*

### <a name="step-2-create-endpoints-within-the-traffic-manager-profile"></a>2. Adım: Traffic Manager profili içindeki uç noktalar oluşturma

Bu adımda üretim ve olağanüstü durum kurtarma sitelerinde noktası uç noktaları oluşturun. Burada, seçtiğiniz **türü** olarak kaynak, Azure'da barındırılan, ancak dış uç noktası, ardından seçebilirsiniz **Azure uç noktası** de. Seçerseniz **Azure uç noktası**, ardından bir **hedef kaynak** ya da diğer bir deyişle bir **App Service** veya **genel IP** tarafından ayrılmış Azure. Öncelikli olarak ayarla **1** , bölge 1 için birincil hizmet olduğundan.
Benzer şekilde, olağanüstü durum kurtarma uç nokta Traffic Manager içinde de oluşturun.

![Olağanüstü durum kurtarma uç noktaları oluşturma](./media/disaster-recovery-dns-traffic-manager/create-disaster-recovery-endpoint.png)

*Şekil - olağanüstü durum kurtarma uç noktaları oluşturma*

### <a name="step-3-set-up-health-check-and-failover-configuration"></a>3. adım: Sistem durumu denetimi ve yük devretme yapılandırmasını ayarlayın

Bu adımda, DNS TTL'yi 10 saniyeye, çoğu internet'e yönelik özyinelemeli çözümleyiciler tarafından kabul ayarlayın. Bu yapılandırma hiçbir DNS Çözümleyicisi 10 saniyeden fazla bilgilerini önbelleğe alacağı anlamına gelir. Uç Nokta İzleyicisi ayarları için geçerli kümesinin yoludur / veya kök, ancak bir yolu, örneğin, prod.contoso.com/index değerlendirmek için uç nokta ayarlarını özelleştirebilirsiniz. Gösterir aşağıdaki örnekte **https** araştırma protokolü olarak. Ancak, seçebileceğiniz **http** veya **tcp** de. Protokol seçimi son uygulama gereksinimlerinize bağlıdır. Yoklama aralığı 10 saniye için hızlı yoklama sağlayan ayarlanır ve yeniden 3 olarak ayarlayın. Sonuç olarak, Traffic Manager yapmayacağınıza ikinci uç nokta yük devretme üç ardışık aralıkları hata kaydedin. Şu formül olarak ayarlayın, bir otomatik yük devretme için toplam süreyi tanımlar: Yük devretme için zaman TTL = + yeniden deneyin * Probing aralığı ve bu durumda, değer 10 + 3 * 10 = 40 saniye (en fazla).
Yeniden deneme 1 ve TTL'ye ayarlanmışsa, 10 saniye için yük devretme 10 + 1 * 10 = 20 saniye sonra zaman ayarlanır. Yeniden deneme daha büyük bir değere ayarlayın **1** hatalı pozitif sonuçları veya herhangi bir alt ağ blips nedeniyle yük devretmeleri olasılığını ortadan kaldırmak için. 


![Sistem durumu denetiminin ayarlayın](./media/disaster-recovery-dns-traffic-manager/set-up-health-check.png)

*Şekil - sistem durumu denetimi ve yük devretme yapılandırmasını ayarlayın*

### <a name="how-automatic-failover-works-using-traffic-manager"></a>Traffic Manager kullanarak nasıl otomatik yük devretme çalışır

Birincil uç noktaya bir olağanüstü durum sırasında araştırıldığı ve durum değişikliklerini **düşürülmüş** ve olağanüstü durum kurtarma siteniz olarak kalır **çevrimiçi**. Varsayılan olarak, Traffic Manager tüm trafiği birincil (en yüksek öncelik) uç noktasına gönderir. Birincil uç azaltılmış görünüyorsa, Traffic Manager trafiği sağlıklı kaldığı sürece ikinci uç noktasına yönlendirir. Bir ek yük devretme uç noktalar olarak hizmet ya da, yük Dengeleyiciler uç noktalar arasında yük paylaşımı olarak daha fazla uç noktaları Traffic Manager içindeki yapılandırma seçeneği vardır.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).
- Daha fazla bilgi edinin [Azure DNS](../dns/dns-overview.md).









