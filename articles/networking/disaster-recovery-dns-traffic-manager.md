---
title: Azure DNS ve trafik Yöneticisi kullanarak olağanüstü durum kurtarma | Microsoft Docs
description: Azure DNS ve trafik Yöneticisi kullanarak olağanüstü durum kurtarma çözümleri genel bakış.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
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
ms.openlocfilehash: d608378f9b3ff3179f9e37ef13f88c65a645d018
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112995"
---
# <a name="disaster-recovery-using-azure-dns-and-traffic-manager"></a>Azure DNS ve Traffic Manager kullanarak olağanüstü durum kurtarma

Olağanüstü durum kurtarma uygulama işlevselliğinin ciddi kaybından kurtarma odaklanır. Bir olağanüstü durum kurtarma çözümü seçmek için iş ve teknoloji sahipleri öncelikle bir felaket sırasında gibi - gerekli kullanılamıyor, sınırlı işlevsellik ya da Gecikmeli kullanılabilirlik aracılığıyla kısmen kullanılabilir işlevsellik düzeyine belirlemeniz gerekir veya tam olarak kullanılabilir.
Çoğu Kurumsal müşteriler bir uygulama veya altyapı düzeyi yük devretme karşı dayanıklılık için bölgeli mimari seçim. Müşteriler, yük devretme ve yedek mimarisi aracılığıyla yüksek kullanılabilirlik elde etmek için rtifika çeşitli yaklaşımlar seçebilirsiniz. Popüler yaklaşımlar bazıları şunlardır:

- **Aktif-Pasif soğuk bekleme ile**: Bu yük devretme çözümü VM'ler ve diğer cihazları çalıştıran bekleme bölgede olmayan etkin bir yük devretme gereksinimini kadar. Ancak, üretim ortamında, yedeklemeler, VM görüntülerini veya farklı bir bölge için Resource Manager şablonları biçiminde çoğaltılır. Bu yük devretme mekanizması ekonomiktir ancak eksiksiz bir yük devretme gerçekleştirmek için çok uzun sürüyor.
 
    ![Etkin/pasif soğuk bekleme ile](./media/disaster-recovery-dns-traffic-manager/active-passive-with-cold-standby.png)
    
    *Şekil - soğuk bekleme olağanüstü durum kurtarma yapılandırması ile etkin/Pasif*

- **Etkin/pasif pilot açık ile**: Bu yük devretme çözümü bekleme ortamında en az bir yapılandırma ile ayarlanır. Kurulum yalnızca yalnızca bir minimal ve kritik kümesini uygulamaları destekleyecek şekilde çalışan gerekli hizmet yok. Özgün biçimiyle, bu senaryo yalnızca en az işlevselliği yürütme ancak ölçeği ve bir yük devretme gerçekleşirse üretim yük toplu yapılacak ek hizmetler oluşturma.
    
    ![Etkin/pasif pilot ışık ile](./media/disaster-recovery-dns-traffic-manager/active-passive-with-pilot-light.png)
    
    *Şekil: Etkin/pasif pilot ışık olağanüstü durum kurtarma yapılandırması ile*

- **Etkin/pasif sıcak bekleme ile**: Bu yük devretme çözümdeki bekleme bölge önceden warmed ve temel yük almaya hazır, otomatik ölçeklendirme açıktır ve tüm örnekleri hazır ve çalışır. Bu çözüm, tam üretim yük yapılacak ölçeklenmez ancak işlevseldir, ve tüm hizmetlerin çalışır. Bu çözüm, pilot ışık yaklaşımın genişletilmiş bir sürümüdür.
    
    ![Etkin/pasif sıcak bekleme ile](./media/disaster-recovery-dns-traffic-manager/active-passive-with-warm-standby.png)
    
    *Şekil: Etkin/pasif ile sıcak bekleme olağanüstü durum kurtarma yapılandırması*
    
Yük devretme ve yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz: [Azure uygulamaları için olağanüstü durum kurtarma](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).


## <a name="planning-your-disaster-recovery-architecture"></a>Olağanüstü durum kurtarma Mimarinizi planlama

Olağanüstü durum kurtarma Mimarinizi ayarı doğru iki teknik yönleri şunlardır:
-  Birincil ve yedek ortamlar arasında örnekleri, veri ve yapılandırmaları çoğaltmak için bir dağıtım mekanizması kullanıyor. Bu tür bir olağanüstü durum kurtarma Microsoft Azure iş ortağı cihazları/Hizmetleri VERITAS veya NetApp gibi aracılığıyla yerel olarak Azure Site Recovery aracılığıyla yapılabilir. 
- Bekleme siteye ağ/web trafiği birincil siteden yönlendir yönelik bir çözüm geliştirme. Bu tür bir olağanüstü durum kurtarma, Azure DNS, Azure trafiği Manager(DNS) veya üçüncü taraf genel yük dengeleyici gerçekleştirilebilir.

Bu makalede, ağ ve Web trafik yeniden yönlendirmesi aracılığıyla yaklaşımlar sınırlıdır. Azure Site Recovery ayarlamak yönergeler için bkz: [Azure Site kurtarma belgeleri](https://docs.microsoft.com/azure/site-recovery/).
DNS DNS genellikle genel ve dış veri merkezine olduğundan ve hiçbir bölge veya kullanılabilirlik bölge (AZ) düzeyi hatalarından yalıtılmış ağ trafiğini yönlendir en verimli mekanizmalarını biridir. Azure'da, aynı bazı şekilde - Azure DNS (yetkili DNS) ve Azure trafik Yöneticisi (DNS tabanlı akıllı trafik yönlendirme) iki DNS hizmetleri gerçekleştirmek ve bir DNS tabanlı yük devretme mekanizması kullanabilirsiniz. 

DNS'de, bu makalede sağlanan çözüm tartışmak için yaygın olarak kullanılan bazı kavramları anlamak önemlidir:
- **DNS A kaydı** – A kayıtlarını olan bir IPv4 adresi için bir etki alanına gelin işaretçileri. 
- **CNAME veya Canonical adı** -bu kayıt türü için başka bir DNS kaydı işaret etmek için kullanılır. CNAME, IP adresini içeren kaydı için bir IP adresi ancak bunun yerine işaretçi yanıt vermiyor. 
- **Yönlendirme ağırlıklı** – bir hizmet uç noktalarına ağırlık ilişkilendirmek ve atanan ağırlıklarını göre trafiği dağıtmak seçebilirsiniz. Bu yönlendirme yöntemi için dört trafik yönlendirme mekanizmaları Traffic Manager içindeki kullanılabilir biridir. Daha fazla bilgi için bkz: [ağırlıklı yönlendirme yöntemi](../traffic-manager/traffic-manager-routing-methods.md#weighted).
- **Öncelik yönlendirme** – öncelik yönlendirme uç noktaları sistem durumu denetimleri dayanır. Varsayılan olarak, Azure trafik Yöneticisi tüm trafiğe en yüksek öncelik uç noktasına gönderir ve başarısızlık veya olağanüstü durum sırasında Traffic Manager trafik ikincil uç noktasına yönlendirir. Daha fazla bilgi için bkz: [öncelik yönlendirme yöntemi](../traffic-manager/traffic-manager-routing-methods.md#priority).

## <a name="manual-failover-using-azure-dns"></a>Azure DNS kullanarak el ile yük devretme
Olağanüstü durum kurtarma için Azure DNS'ye el ile yük devretme çözümü yedekleme siteye yük devretme için standart DNS mekanizması kullanır. Azure DNS aracılığıyla el ile seçeneği soğuk bekleme veya pilot açık yaklaşım ile birlikte kullanıldığında en iyi şekilde çalışır. 

![Azure DNS kullanarak el ile yük devretme](./media/disaster-recovery-dns-traffic-manager/manual-failover-using-dns.png)

*Şekil - Azure DNS kullanarak el ile yük devretme*

Çözüm için yapılan varsayımlar şunlardır:
-   Birincil ve ikincil uç noktaları genellikle değişmez statik IP'ye sahip. Birincil site için IP 100.168.124.44 ve ikincil site için IP 100.168.124.43 söyleyin.
-   Bir Azure DNS bölgesi için hem bir birincil ve ikincil site yok. Birincil site için deyin uç noktası prod.contoso.com ve yedekleme dr.contoso.com sitedir. Www.contoso.com olarak bilinen ana uygulama için bir DNS kaydı da bulunmaktadır.   
-   TTL veya Kuruluşunuzda ayarlanmış RTO SLA altındaki ' dir. Örneğin, bir kuruluş RTO 60 dakika, tercihen alt'den küçük TTL olmalıdır sonra 60 dakika olarak uygulama olağanüstü durum yanıtı, daha iyi ayarlarsa. Azure DNS'ye el ile yük devretme için aşağıdaki gibi ayarlayabilirsiniz:
1. DNS bölgesi oluşturma
2. DNS bölge kayıtlarını oluşturun
3. CNAME kaydı güncelleştir

### <a name="step-1-create-a-dns"></a>1. adım: bir DNS oluşturma
Bir DNS bölgesi (örneğin, www.contoso.com) aşağıda gösterildiği gibi oluşturun:

![Bir DNS bölgesi oluşturma](./media/disaster-recovery-dns-traffic-manager/create-dns-zone.png)

*Şekil - bir DNS bölgesi oluşturma*

### <a name="step-2-create-dns-zone-records"></a>2. adım: DNS bölge kayıtları oluşturma

Bu bölge içinde Göster aşağıdaki üç kayıtları (örneğin - www.contoso.com, prod.contoso.com ve dr.consoto.com) oluşturun.

![DNS bölge kayıtlarını oluşturun](./media/disaster-recovery-dns-traffic-manager/create-dns-zone-records.png)

*Şekil - DNS bölge kayıtları oluşturma*

Bu senaryoda, site, www.contoso.com iyi belirtilen RTO ve üretim sitesini prod.contoso.com için işaret eden bir TTL 30 dakika değeri yok. Normal iş işlemleri sırasında yapılandırmadır. TTL prod.contoso.com dr.contoso.com ve 300 saniye veya 5 dakika olarak ayarlandı. İzleme hizmeti Azure İzleyicisi'ni veya Azure App Insights gibi Azure kullanabilirsiniz veya çözümlerini Dynatrace gibi izleme herhangi bir ortak dahi izlemek veya uygulama veya sanal altyapı düzeyi hataları algılar, ev büyütülmesi çözümleri kullanabilirsiniz.

### <a name="step-3-update-the-cname-record"></a>3. adım: CNAME kaydı güncelleştir

Hata algılandığında, aşağıda gösterildiği gibi dr.contoso.com için işaret edecek şekilde kayıt değerini değiştirin:
       
![CNAME kaydı güncelleştir](./media/disaster-recovery-dns-traffic-manager/update-cname-record.png)

*Şekil - Azure CNAME kaydı güncelleştirmek*

30 önbelleğe alınan bölge dosyası yenilenecek çoğu çözümleyiciler sırasında dakika içinde www.contoso.com için herhangi bir sorgu için dr.contoso.com yönlendirilir.
Ayrıca, CNAME değerini değiştirmek için aşağıdaki Azure CLI komutu çalıştırabilirsiniz:
 ```azurecli
   az network dns record-set cname set-record \
   --resource-group 123 \
   --zone-name contoso.com \
   --record-set-name www \
   --cname dr.contoso.com
```
Bu adım, el ile ya da Otomasyon üzerinden çalıştırılabilir. El ile Azure CLI veya Konsolu aracılığıyla yapılabilir. Azure SDK'sı ve API, böylece herhangi bir el ile müdahale gerekli değildir CNAME güncelleştirme otomatik hale getirmek için kullanılabilir. Otomasyon Azure işlevleri aracılığıyla veya bir üçüncü taraf izleme uygulama içinde veya hatta şirket içi yerleşik hale getirilebilir.

### <a name="how-manual-failover-works-using-azure-dns"></a>Azure DNS kullanarak nasıl el ile yük devretme çalışır
DNS sunucusu yük devretme veya olağanüstü durum bölgesi dışından olduğundan, karşı kapalı kalma süresi yalıtımlı. Bu kullanıcının uygun maliyetli bir basit yük devretme senaryosu düzenlenmesine olanak tanır ve tüm zaman işleci olağanüstü durum sırasında ağ bağlantısı olduğunu varsayarak çalışır ve Çevir yapabilirsiniz. Çözüm komut dosyası, sonra bir sunucu veya komut dosyası çalıştırarak hizmet üretim ortamına etkileyen sorun karşı yalıtılmış olduğundan emin olmalısınız. Ayrıca, bölge karşı dünyanın dört bir çözümleyici yok uzun süre önbelleğe endpoint tutar ve müşteriler RTO sitede erişebilir ayarlandı düşük TTL göz önünde bulundurun. Bazı prewarming ve diğer yönetim etkinliği – gerekebilecek beri soğuk bekleme ve pilot ışık için biri de Çevir yapmadan önce yeterli bir süre vermesi gerekir.

## <a name="automatic-failover-using-azure-traffic-manager"></a>Azure trafik Yöneticisi'ni kullanarak otomatik yük devretme
Karmaşık mimarisi ve kaynaklar aynı işlevi gerçekleştirebilecek birden çok kümesini varsa, kaynaklarınızın sistem durumunu denetleyin ve gelen trafiği iyi durumda kaynak iyi yönlendirmek için Azure trafik Yöneticisi'ni (DNS dayalı) yapılandırabilirsiniz Kaynak. Aşağıdaki örnekte, hem birincil bölge hem de ikincil bölge tam dağıtımını vardır. Bu dağıtım, bulut Hizmetleri ile eşitlenmiş veritabanı içerir. 

![Azure trafik Yöneticisi'ni kullanarak otomatik yük devretme](./media/disaster-recovery-dns-traffic-manager/automatic-failover-using-traffic-manager.png)

*Şekil - Azure trafik Yöneticisi'ni kullanarak otomatik yük devretme*

Ancak, yalnızca birincil bölge etkin olarak kullanıcıların ağ isteklerini işliyor. Yalnızca birincil bölge hizmet kesintisi yaşadığında ikincil bölge etkin hale gelir. Bu durumda, tüm yeni ağ isteklerini ikincil bölge'ye yönlendirir. Veritabanı yedeklemeden neredeyse anlık, yük Dengeleyiciler olduğunu işaretli durumu olabilir IP'leri örnekleri her zaman en ve çalıştırmak, bu topoloji bir düşük RTO ve el ile müdahalesi olmadan yük devretme gitmek için bir seçenek sağlar. Yük devretme ikincil bölge hemen birincil bölge hatasından sonra servise hazır olması gerekir.
Bu senaryo ' % s'kullanım sahip Azure trafik Yöneticisi için ideal olan, sistem durumu denetimlerinin http dahil olmak üzere çeşitli türleri için yerleşik araştırmalar / https ve TCP. Azure trafik Yöneticisi, aşağıda açıklandığı gibi bir hata oluşursa, yük devretme için yapılandırılmış bir kural altyapısı da sahiptir. Şimdi trafik Yöneticisi'ni kullanarak aşağıdaki çözümünü göz önünde bulundurun:
- Müşteri, statik IP adresi 100.168.124.44 ve statik IP adresi olarak 100.168.124.43 ile dr.contoso.com bilinen bir bölge #2 uç noktası olarak ile prod.contoso.com bilinen bölge #1 uç noktası vardır. 
-   Bu ortamların her birinde bir yük dengeleyici gibi genel kullanıma yönelik bir özellik aracılığıyla fronted. Yük Dengeleyici, yukarıda gösterildiği gibi bir DNS tabanlı uç noktası veya bir tam etki alanı adı (FQDN) sağlamak için yapılandırılabilir.
-   Bölge 1 ile gerçek zamanlı çoğaltma yakın, bölge 2'deki tüm örneklerini alır. Ayrıca, makine görüntülerini güncel ve tüm yazılım/yapılandırma verilerini düzeltme eki ve uygun olarak bölge 1.  
-   Otomatik ölçeklendirmeyi önceden önceden yapılandırılmıştır. 

Yük devretme Azure Traffic Manager ile yapılandırmak için adımlar aşağıdaki gibidir:
1. Yeni bir Azure Traffic Manager profili oluşturma
2. Uç noktalarda trafik Yöneticisi profili oluştur
3. Sistem durumu denetimi ve yük devretme yapılandırmasını ayarlama

### <a name="step-1-create-a-new-azure-traffic-manager-profile"></a>1. adım: yeni bir Azure Traffic Manager profili oluşturma
Ad contoso123 ile yeni bir Azure trafik Yöneticisi profili oluşturun ve öncelikli olarak yönlendirme yöntemini seçin. Aksi takdirde, var olan bir kaynak grubunu seçip, ilişkilendirmek istediğiniz önceden var olan bir kaynak grubunuz varsa, yeni bir kaynak grubu oluşturun.

![Trafik Yöneticisi profili oluştur](./media/disaster-recovery-dns-traffic-manager/create-traffic-manager-profile.png)
*şekil - trafik Yöneticisi profili oluştur*

### <a name="step-2-create-endpoints-within-the-traffic-manager-profile"></a>2. adım: uç noktalarda trafik Yöneticisi profili oluşturma

Bu adımda, üretim ve olağanüstü durum kurtarma sitelerinde noktası uç noktaları oluşturun. Burada, seçtiğiniz **türü** kaynak, Azure'da barındırılan, ancak dış uç noktası, olarak sonra seçebileceğiniz **Azure uç noktası** de. Seçerseniz **Azure uç noktası**seçeneğini belirleyip bir **hedef kaynak** ya da başka bir deyişle bir **uygulama hizmeti** veya **genel IP** tarafından ayrılan Azure. Öncelikli olarak ayarla **1** bölge 1 için birincil hizmet olduğundan.
Benzer şekilde, olağanüstü durum kurtarma uç nokta Traffic Manager içinde de oluşturun.

![Olağanüstü durum kurtarma uç noktaları oluşturma](./media/disaster-recovery-dns-traffic-manager/create-disaster-recovery-endpoint.png)

*Şekil - olağanüstü durum kurtarma uç noktaları oluşturun.*

### <a name="step-3-set-up-health-check-and-failover-configuration"></a>3. adım: Sistem durumu denetimi ve yük devretme yapılandırması ayarlama

Bu adımda, DNS TTL 10 saniye olarak, çoğu internet'e yönelik özyinelemeli çözümleyicileri tarafından dikkate alınır ayarlayın. Bu yapılandırma hiçbir DNS Çözümleyicisi 10 saniyeden fazla bilgileri önbelleğe alır anlamına gelir. Uç nokta izleme ayarları için geçerli ayarlanan yoludur / veya kök, ancak bir yolu, örneğin, prod.contoso.com/index değerlendirmek için uç nokta ayarlarını özelleştirebilirsiniz. Gösterir aşağıdaki örnekte **https** yoklama protokol olarak. Ancak, seçebileceğiniz **http** veya **tcp** de. Protokol seçimi bitiş uygulamasına bağlıdır. Yoklama aralığı 10 saniye için hızlı yoklama sağlayan ayarlanır ve yeniden deneme 3'e ayarlanır. Sonuç olarak, trafik yöneticisi olacak ikinci uç nokta yük devretme üç ardışık aralıkları kaydolması bir hata durumunda. Aşağıdaki formülü otomatik yük devretme için toplam süreyi tanımlar: yük devretme süresini TTL = + yeniden deneme * Probing aralığı ve bu durumda, değer 10 + 3 * 10 = 40 (Maks) saniyedir.
Yeniden deneme 1 ve TTL ayarlarsanız 10 saniye için yük devretme 10 + 1 * 10 = 20 saniye sonra süresini ayarlanır. Yeniden deneme daha büyük bir değere ayarlayın **1** yerine hatalı pozitif sonuç veya herhangi bir alt ağ blips nedeniyle olasılığını ortadan kaldırmak için. 


![Sistem durumu Denetim ayarlayın](./media/disaster-recovery-dns-traffic-manager/set-up-health-check.png)

*Şekil - sistem durumu denetimi ve yük devretme yapılandırmasını ayarlama*

### <a name="how-automatic-failover-works-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanarak otomatik yük devretme çalışır

Bir olağanüstü durum sırasında birincil endpoint araştırılan ve durum değişikliklerini **düşürülmüş** ve olağanüstü durum kurtarma sitesini kalır **çevrimiçi**. Varsayılan olarak, trafik Yöneticisi tüm trafiği birincil (en yüksek öncelik) uç noktasına gönderir. Birincil uç nokta düşürülmüş görünürse, trafik Yöneticisi trafiğini sağlıklı kaldığı sürece ikinci uç noktasına yönlendirir. Bir ek yük devretme uç noktası görevi görecek veya, yük Dengeleyiciler uç noktalar arasında yük paylaşımı olarak daha fazla uç noktaları Traffic Manager içindeki yapılandırma seçeneği vardır.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).
- Daha fazla bilgi edinmek [Azure DNS](../dns/dns-overview.md).









