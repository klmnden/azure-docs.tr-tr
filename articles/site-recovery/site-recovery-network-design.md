---
title: "Azure Site Recovery ile olağanüstü durum kurtarma için ağ altyapınızı tasarlama | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery için ağ tasarım konuları anlatılmaktadır."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: jwhit
editor: 
ms.assetid: ce787731-d930-4f00-a309-e2fbc2e1f53b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: pratshar
ms.openlocfilehash: 5b6fb7bac852b29663866e99758241bd5a7ab59e
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="designing-your-network-for-disaster-recovery"></a>Ağınız olağanüstü durum kurtarma için tasarlama

Bu makalede kullanırken ağ konuları ele [Azure Site Recovery](site-recovery-overview.md) için olağanüstü durum kurtarma şirket içi Azure'a veya ikincil bir site içi. Bu, ikincil bir konuma yük devretme sonrasında IP adres aralıklarını ve alt ağlar üzerinde tanımlama odaklanır.

## <a name="overview"></a>Genel Bakış

Site Recovery koruma ve kurtarma iş sürekliliği olağanüstü durum kurtarma (BCDR) amacıyla sanallaştırılmış uygulama düzenler bir Microsoft Azure hizmetidir.

7/24 bağlantı dünyasında, önemli iş altyapısını ve uygulamaları takip edin ve çalışır durumdadır. Kuruluşunuzun normal işlemler hızlı bir şekilde devam edebilmeniz için BCDR amacı arızalı bileşenlere geri yüklemektir. Olası olaylar ile mücadele etmek için olağanüstü durum kurtarma stratejilerini geliştirmek çok zor. Bu özellikle olanak dışı olaylar için gelecekte tahmin etmeye devralınmış zorluğunu kaynaklanır. Ve beklediğinizden çok daha büyük catastrophes karşı koruma yeterli ölçülerin bakım yüksek maliyetini nedeniyle.

BCDR planlaması için çok önemli bir kurtarma süresi hedefi (RTO) ve kurtarma noktası hedefi (RPO) BCDR planınızda tanımlanması gerekir. Bir veri merkezi bir olağanüstü durum sağlar, hızlı bir şekilde kullanabilirsiniz (düşük RTO) çoğaltılmış sanal makinelerini ikincil veri merkezi veya Microsoft Azure en az veri kaybı (düşük RPO'ya) bulunan çevrimiçi.

Yük devretme Site başlangıçta belirtilen sanal makineleri birincil veri merkezinden ikincil veri merkezine veya Azure (bağlı olarak senaryo) kopyalar ve çoğaltmaları düzenli aralıklarla yeniler kurtarma işlemi, mümkün hale getirilir. Altyapı planlama sırasında ağ tasarımı, RTO toplantı şirketten ve RPO hedefleri engelleyebilirsiniz olası performans sorunu düşünülmelidir.  

Yöneticiler bir olağanüstü durum kurtarma çözümü dağıtmak planlama yaparken, fikirlerini anahtar sorulara yük devretme işlemi tamamlandıktan sonra nasıl sanal makine erişilebilir olacaktır biridir. Site kurtarma için bir sanal makine için yük devretme sonrasında bağlı olurdu ağı seçmek yöneticinin sağlar. Azure birincil site olduğunu veya bir VMM sunucusu tarafından yönetilen bir şirket içi site bırakılırsa, ardından bu ağ eşlemesi kullanılarak elde edilir. Daha fazla bilgi edinmek [ağ eşlemesi, Azure için Azure DR](site-recovery-network-mapping-azure-to-azure.md) ve [ağ eşlemesi vmm'den](site-recovery-network-mapping.md)


Kurtarma sitesi için ağ tasarlarken, yönetici iki seçenek vardır:

* Kurtarma sitesinde ağ için farklı bir IP adresi aralığı kullanın. Bu senaryoda, sanal makine yük devretme sonrasında yeni bir IP adresi alır ve DNS güncelleştirme yapmak yönetici olması gerekir. 
* Kurtarma sitesinde ağ için aynı IP adresi aralığı kullanın. Bazı senaryolarda, yöneticiler bile yük devretme sonrasında birincil siteye sahip oldukları IP adreslerini korumak tercih eder. Normal bir senaryoda, IP adreslerini yeni konumunu belirtmek için rotalar güncelleştirmek bir yönetici gerekir. Ancak uzatılan bir alt birincil ve kurtarma siteleri arasında dağıtıldığı senaryoda, sanal makineler için IP adreslerini korunuyor çekici bir seçenek haline gelir. Bir alt ağ üzerinden bir şirket içi ağ bir Azure ağı veya iki Azure ağlar arasında uzatma mümkün değildir.  

Yöneticiler bir olağanüstü durum kurtarma çözümü dağıtmak planlama yaparken, fikrini anahtar sorulara yük devretme işlemi tamamlandıktan sonra nasıl uygulamaları ulaşılabilir biridir. Modern uygulamalar neredeyse her zaman bir hizmete bir siteden başka bir ağ sınamasını temsil eder böylece fiziksel olarak taşıyarak dereceye ağa bağlıdır. Bu sorunu olağanüstü durum kurtarma çözümleri ile dağıtılır, başlıca iki yolu vardır. İlk yaklaşım, sabit IP adresleri korumaktır. Taşıma hizmetleri ve farklı fiziksel konumlarda olan barındırma sunucuları rağmen uygulamaları IP adresi yapılandırmasıyla bunları yeni konuma getirin. İkinci yaklaşımı tamamen IP adresini kurtarılan siteye geçiş sırasında değiştirilmesidir. Her iki yaklaşımın aşağıda özetlenen birkaç uygulama Çeşitlemeler vardır.

Kurtarma sitesi için ağ tasarlarken, yönetici iki seçenek vardır:

## <a name="option-1-retain-ip-addresses"></a>Seçenek 1: IP adreslerini korur
Bir olağanüstü durum kurtarma işlemi açısından sabit IP adresleri uygulamak için en kolay yöntem gibi görünüyor, ancak az yaygın olan bir yaklaşım hale getiren uygulamada bir dizi olası zorluklar sahiptir. Azure Site Recovery tüm senaryolarda IP adreslerini korumak için yeteneği sağlar. Bir IP korumak karar önce uygun düşünce yük devretme yetenekleri uygular kısıtlamaları verilmelidir. Bize veya IP adresleri, korumak için bir karar vermek için yardımcı olabilecek etmenleri arayın. Bu iki yolla Uzatılan bir alt ağ kullanarak veya tam alt ağ yük devretme işleminden elde edilebilir.

### <a name="stretched-subnet"></a>Esnetilen alt ağ
Burada alt aynı anda hem birincil hem de DR konumları içinde kullanılabilir hale getirilir. Basitçe bu ikinci siteye bir sunucu ve kendi IP (Katman 3) yapılandırmasını taşıyın ve ağ trafiğini yeni konuma otomatik olarak yönlendirecek anlamına gelir. Bu sunucu açısından uğraşmanız Önemsiz, ancak belirli zorluklar vardır:

* Bir katman 2 (veri bağlantı katmanı) açısından Uzatılan bir VLAN yönetebilirsiniz ağ ekipmanları gerektiriyor, ancak bu daha az soruna şimdi geniş çapta kullanılabilir olduğu gibi bir işlem haline gelmiştir. İkinci ve daha zor sorunu temelde tek hata noktası haline olası hata etki alanı her iki sitede, genişletilmiş VLAN yayarak olmasıdır. Bu bir ender olsa da, bir yayın storm başlatıldı ancak yalıtılmış kaldırılamıyor gerçekleştirilmedi. Son Bu sorun hakkında karma görüşlerini bulduğunuz ve birçok başarılı uygulamalarının yanı sıra "biz bu teknolojiyi burada uygulamak olmaz." gördünüz
* Esnetilen alt DR sitesi olarak Microsoft Azure kullanıyorsanız mümkün değildir.

### <a name="subnet-failover"></a>Alt ağ yük devretme
Birden çok sitede alt uzatma olmadan yukarıda açıklanan esnetilen alt çözüm avantajlarından yararlanabilmek için alt ağ yük devretme uygulamak mümkündür. Burada herhangi belirli alt ağ aynı anda Site 1 veya Site 2'ancak hiçbir her iki site de mevcut olacaktır. IP adres alanı bir yük devretme durumunda korumak için alt ağların bir siteden diğerine taşımak yönlendirici altyapı programlı olarak düzenlemek mümkündür. Alt ağlar ile ilişkili taşıyabilir bir yük devretme senaryosunda VM'ler korumalı. Bu yaklaşımın asıl sakıncası tüm alt ağı taşımak için yüklü bir arıza olması durumunda değil. Bu Tamam olabilir, ancak yük devretme ayrıntı düzeyi etkileyebilir.

Nasıl Contoso adlı kurgusal bir kuruluş kendi Vm'lerini sırasında başarısız olan bir kurtarma konumu tüm alt ağ çoğaltma yapabiliyor inceleyelim. Şimdi nasıl Contoso iki arasında sanal makineleri çoğaltılırken alt ağlarını yönetebilmek için ilk bakış şirket içi konumlara ve ardından biz nasıl alt ağ yük devretme ne zaman çalıştığını ele alacağız [Azure olağanüstü durum kurtarma sitesi olarak kullanılan](#failover-to-azure).

#### <a name="fail-over-from-on-premises-to-azure"></a>Şirket içinden Azure'a yük 
Azure Site Recovery, sanal makineleriniz için bir olağanüstü durum kurtarma sitesi olarak kullanılacak Azure sağlar.  

Woodgrove Bank adlı kurgusal bir şirket satır iş kolu uygulamalarını barındırma şirket içi altyapı sahip olduğu bir senaryo inceleyelim ve Azure mobil uygulamalarını barındıran. Azure ve şirket içi sunucularda Woodgrove Bank VM'ler arasında bağlantısı, siteden siteye (S2S) sanal özel ağ (VPN) ya da ExpressRoute tarafından sağlanır. Siteden siteye VPN Woodgrove Bank'ın şirket içi ağ bir uzantısı olarak görüntülenmesine Azure Woodgrove Bank'ın sanal ağ sağlar. Bu iletişim, Woodgrove Bank kenarı ve Azure sanal ağı arasındaki siteden siteye VPN ile etkinleştirilir. Şimdi Woodgrove Birincil Azure bölgesindeki başka bir Azure bölgesine çalışan kendi iş yüklerini çoğaltmak için Site Recovery kullanmak istiyor. Bu seçenek ekonomik bir DR seçeneği istediği ve genel bulut ortamlarında verileri depolamak mümkün olan Woodgrove gereksinimlerini karşılar. Uygulamalar ve yapılandırma, sabit kodlanmış IP adreslerini bağlı, olan Woodgrove vardır ve bu nedenle bir gereksinim için başka bir bölgede Azure üzerinden başarısız olduktan sonra uygulamaları için IP adreslerini korumak için.

Woodgrove IP adresi aralığından (172.16.1.0/24, 172.16.2.0/24) IP adresleri atamak Azure'da çalışan kendi kaynaklarına karar vermiştir.

IP adreslerini korurken kendi sanal makinelerini Azure'a çoğaltma Woodgrove için bir Azure sanal ağı oluşturulması gerekir. Uygulamaları şirket içi sitesinden Azure'a sorunsuz bir şekilde yük devredebildiğini böylece şirket içi ağ uzantısı olmalıdır. Azure, Azure üzerinde oluşturulan sanal ağlar için noktadan siteye yanı sıra, siteden siteye VPN bağlantısı eklemenizi sağlar. Siteden siteye bağlantı kurma olduğunda Azure ağ trafiği Azure uzatma alt ağlar desteklemediği için IP adres aralığı yalnızca şirket içi IP adresi aralığından farklı olması durumunda (Azure yerel ağ çağırır) şirket içi konuma yönlendirmek izin verir.  Bunun anlamı, varsa alt 192.168.1.0/24 şirket içi, yapamazsınız, Azure ağında bir yerel ağ 192.168.1.0/24 ekleyin. Azure alt ağda etkin VM yok ve yalnızca DR amaçları için alt ağ oluşturulmaktadır bilmiyor çünkü bu beklenir. Doğru ağ trafiği bir Azure ağı dışında alt ağ ve yerel ağ yönlendirmek için çakışmaması gerekir.

![Alt ağ yük devretme önce](./media/site-recovery-network-design/network-design7.png)

Önce yük devretme

Kendi iş gereksinimlerini karşılayacak Woodgrove yardımcı olmak için aşağıdaki iş akışı uygulamak ihtiyacımız var:

* Ek bir ağ oluşturun, bize nerede başarısız üzerinden sanal makineleri oluşturulan kurtarma ağ çağırın.
* Bir VM için IP yük devretme sonrasında tutulur emin olmak için VM Özellikleri'nin altında yapılandırma sekmesine gidin VM içi sahip aynı IP belirtin ve ardından Kaydet'i tıklatın. VM üzerinden başarısız olduğunda, Azure Site Recovery sanal makine için sağlanan IP atayın.

![Ağ Özellikleri](./media/site-recovery-network-design/network-design8.png)

Yük devretme tetiklenir ve sanal makineleri kurtarma ağ istenen IP ile oluşturulan sonra bu ağ bağlantısını kullanarak kurulabilir bir [Vnet'e bağlantı](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Bu eylem komut dosyasıyla gerekiyorsa.  Biz Azure, bir yük devretme durumunda bile alt ağ yük devretme hakkında önceki bölümde açıklandığı gibi yollar uygun şekilde bu 192.168.1.0/24 artık Azure'a taşındı yansıtacak şekilde değiştirilmesi gerekir.

![Alt ağ yük devretme sonrasında](./media/site-recovery-network-design/network-design9.png)

Yük devretme sonrasında

Yukarıdaki resimde gösterildiği gibi 'Azure Ağ' yoksa. Yük devretme sonrasında 'Birincil Site' ve 'Kurtarma Ağ' arasında bir siteden siteye VPN bağlantısı oluşturabilirsiniz.  


#### <a name="fail-over-to-a-secondary-on-premises-site"></a>Bir ikincil şirket içi siteye yük devri
Bize bir senaryo burada VM'lerin her birinde IP'si olmaya devam eder ve tüm alt birlikte başarısız-üzerinden istiyoruz arayın. Birincil site alt 192.168.1.0/24 içinde çalışan uygulamalar vardır. Yük devretme gerçekleştiğinde, bu alt ağın parçası olan tüm sanal makineler için kurtarma sitesi üzerinden devredilir ve IP adreslerini korur. Yollar uygun alt ağ 192.168.1.0/24 ait tüm sanal makineleri kurtarma siteye şimdi taşınmış olgu yansıtacak şekilde değiştirilmesi gerekir.

Aşağıdaki çizimde birincil site kurtarma sitesi, üçüncü sitesi ve birincil site ve üçüncü site ve kurtarma sitesi arasındaki yolları uygun şekilde değiştirilmesi gerekir.

Aşağıdaki resim yük devretme önce alt ağları göster. Alt ağ 192.168.0.1/24 yük devretmeden önce birincil sitede etkin ve yük devretme sonrasında kurtarma sitesini etkin hale gelir

![Önce yük devretme](./media/site-recovery-network-design/network-design2.png)

Önce yük devretme

Aşağıdaki resimde, yük devretme sonrasında ağları ve alt ağlar gösterilmektedir.

![Yük devretme sonrasında](./media/site-recovery-network-design/network-design3.png)

Yük devretme sonrasında

İkincil site içi ise ve, ardından belirli bir sanal makine için koruma etkinleştirilirken yönetmek için bir VMM sunucusu kullanıyorsanız, Site Recovery aşağıdaki iş akışı uygun ağ kaynaklarına ayırır:

* Site Recovery her System Center VMM örneği için ilgili ağ üzerinde tanımlanan statik IP adres havuzundan sanal makine üzerindeki her ağ arabirimi için bir IP adresi ayırır.
* Yönetici ağ için aynı IP adresi havuzu, birincil site ağda IP adresi havuzu olarak kurtarma sitede çoğaltma sanal makinesi IP adresine ayrılırken tanımlıyorsa, Site Recovery aynı IP adresi ayırma Birincil sanal makinenin.  IP adresi VMM'de ayrılmış, ancak yük devretme IP adresi olarak Hyper-v ana bilgisayarda ayarlı değil. Bir Hyper-v konağı yük devretme IP adresi yalnızca yük devretme önce ayarlanır.


Aynı IP adresi kullanılabilir durumda değilse, Site Recovery tanımlanan IP adresi havuzundan diğer bazı bir kullanılabilir IP adresi ayırır.

VM için koruma etkinleştirildikten sonra sanal makineye ayrılan IP doğrulamak için örnek komut dosyası kullanabilirsiniz. Aynı IP yük devretme IP ayarlayın ve yük devretme sırasında VM'ye atanan:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

> [!NOTE]
> Burada sanal makineler DHCP kullanmak senaryosunda, IP adreslerinin tamamen Site Recovery denetimi dışında yönetimidir. Kurtarma sitesinde IP adreslerini hizmet veren DHCP sunucusu aynı aralıktan birincil site yeniden kullanılabileceği emin olmak bir yönetici sahiptir.
>
>



## <a name="option-2-changing-ip-addresses"></a>Seçenek 2: IP adreslerini değiştirme
Bu yaklaşım en yaygın ne anlatıldığı üzerinde temel gibi görünüyor. Yük devretme kümesinde dahil her VM IP adresini değiştirme formu alır. Bu yaklaşımın bir dezavantajı ' IPX olan uygulama artık IPy olduğunu öğrenin ' gelen ağ gerektirir. IPX ve IPy mantıksal adları olsa bile, DNS girişlerini genellikle değiştirilemez ya da ağda Temizlenen zorunda ve önbelleğe alınmış ağ tablolardaki güncelleştirilmesi veya temizlenen girişleriniz, bu nedenle bir kapalı kalma süresi DNS altyapısı nasıl olmuştur bağlı bağlı olarak görülebilir ayarlayın. Bu sorunları intranet uygulamalarını durumunda düşük TTL değerleri kullanarak ve kullanılarak azaltılabilir [Site Recovery ile Azure Traffic Manager](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/), Internet tabanlı uygulamalar için

### <a name="changing-the-ip-addresses---illustration"></a>IP adresleri - çizim değiştirme
Bize senaryo burada birincil ve kurtarma siteler arasında farklı IP'leri kullanmayı planladığınıza arayın. Biz de sahip üçüncü site gelen uygulamaların birincil veya kurtarma barındırıldığı aşağıdaki örnekte site erişilebilir.

![Farklı bir IP - yük devretme önce](./media/site-recovery-network-design/network-design10.png)


Yukarıdaki resimde alt 192.168.1.0/24 alt birincil sitede barındırılan bazı uygulamalar vardır ve yük devretme sonrasında alt ağ 172.16.1.0/24 kurtarma sitede gündeme şekilde yapılandırılmıştır. Tüm üç siteleri birbirine erişebilmesi için VPN bağlantıları/ağ yolları uygun şekilde yapılandırılmış.

Bir veya daha fazla uygulama üzerinden başarısız olduktan sonra gösterir aşağıda resim olarak bunlar kurtarma alt ağda geri yüklenir. Bu durumda biz aynı anda tüm alt ağ vermesine kısıtlanmıştır değil. Herhangi bir değişiklik, VPN veya ağ yolları yapılandırmak için gereklidir. Bir yük devretme ve bazı DNS güncelleştirmeleri, uygulamalar erişilebilir kalır emin olmanızı sağlar. DNS dinamik güncelleştirmelere izin verecek şekilde yapılandırılmışsa, sanal makineleri kendilerini bir yük devretme sonrasında başladıktan sonra yeni IP kullanarak kaydetmeniz.

![Farklı bir IP - yük devretme sonrasında](./media/site-recovery-network-design/network-design11.png)


Başarısız-üzerinden sonra çoğaltma sanal makinesi birincil sanal makinenin IP adresi ile aynı olmayan bir IP adresi olabilir. Sanal makineler, kullanmakta olduğunuz DNS sunucusu güncelleştirilecek başladıktan sonra. DNS girişlerini genellikle değiştirilemez ya da ağda Temizlenen gerekir ve bu durum değişiklikleri yer alırken kapalı kalma süresi ile karşılaştığı sık karşılaşılan bir durum değildir güncelleştirilmesi veya temizlenen, ağ tablolardaki önbelleğe alınmış girişlerin sahip. Bu sorunu azaltmak için:

* Düşük TTL değeri için intranet uygulamalarını kullanma.
* Azure trafik Yöneticisi Site Recovery ile Internet tabanlı uygulamalar için kullanma.
* (Dinamik DNS kayıt yapılandırılmışsa, komut dosyasını gerekli değildir) bir zamanında güncelleştirme emin olmak için DNS sunucusu güncelleştirmek için aşağıdaki komut dosyası kurtarma planı içinde kullanma

        param(
        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

### <a name="changing-the-ip-addresses--disaster-recovery-to-azure"></a>Değiştirme IP adresleri – Azure olağanüstü durum kurtarma
[Ağ altyapı kurulumu bir olağanüstü durum kurtarma sitesi olarak Microsoft Azure](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) blog gönderisi IP adreslerini koruma gereksinimi kullanılmadığında gerekli Azure ağ altyapısını ayarlamak nasıl anlatılmaktadır. Uygulamayı açıklayan ile başlar ve ardından ağ yerinde ve Azure ve yük devretme testi ve planlanmış bir yük devretme nasıl tamamlanırken nasıl ayarlanacağını bakın.

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [ağ eşlemesi](site-recovery-network-mapping.md).
