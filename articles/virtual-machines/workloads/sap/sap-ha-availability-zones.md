---
title: Azure kullanılabilirlik alanları ile SAP iş yükü yapılandırmaları | Microsoft Docs
description: Yüksek kullanılabilirlik mimarisi ve senaryolar için Azure kullanılabilirlik alanları kullanarak SAP NetWeaver
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/03/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e319fb5bc1e0ed3354a09b685405656c7f3131bc
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55858814"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>Azure kullanılabilirlik alanları ile SAP iş yükü yapılandırmaları

Azure'un sunduğu yüksek kullanılabilirlik özelliklerinden birini [Azure kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview). Kullanılabilirlik alanları kullanarak, azure'da SAP iş yükü genel kullanılabilirliğini artırır. Kullanılabilirlik alanları ile çeşitli sunulur [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/) zaten. Daha fazla bölge izler. 

SAP yüksek kullanılabilirlik temel mimarisini, grafikte görüntülenen şekilde açıklanabilir:

![Standart HA yapılandırma](./media/sap-ha-availability-zones/standard-ha-config.png)

SAP uygulama katmanına bir Azure genelinde dağıtılan [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). SAP Central Services'in yüksek kullanılabilirlik için ayrı bir kullanılabilirlik kümesinde iki VM dağıtın. Yüksek kullanılabilirlik çerçeve altyapı veya yazılım sorunu olması durumunda otomatik yük devretme ile Windows Yük devretme küme hizmetlerini veya Pacemaker (Linux) kullanın. Bu tür dağıtımlar gerçekleşen belgeler aşağıda listelenmiştir:

- [Bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Dosya paylaşımı kullanarak bir Windows Yük devretme kümesinde Küme: SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [SAP uygulamaları için SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Azure sanal makineler Red Hat Enterprise Linux üzerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Benzer bir mimariye DBMS katmanı SAP NetWeaver, S/4HANA veya Hybris sistemleri için geçerlidir. Bir altyapı ya da yazılım arızasına karşı korumak için bir yük devretme kümesi çözümünü ile Aktif/Pasif modda DBMS katman dağıtırsınız. Yük devretme kümesi çözümünü ya da DBMS belirli bir yük devretme framework, Windows Yük devretme küme hizmetlerini veya Pacemaker olabilir. 

Azure kullanılabilirlik alanları kullanarak aynı mimariyi dağıtmak için yukarıda özetlenen mimarisinin bazı değişiklikler gereklidir. Bu değişiklikler, bu belgede açıklanan.

## <a name="considerations-deploying-across-availability-zones"></a>Kullanılabilirlik alanları genelinde dağıtma konuları

Kullanılabilirlik alanları kullanırken dikkate alınması gereken bazı şeyler vardır:

- Azure kullanılabilirlik alanları, bir Azure bölgesi içinde farklı bölgeler arasında belirli bir uzaklık garanti etmez.
- Azure kullanılabilirlik alanları, ideal bir DR çözümü değildir. Doğal afetler geniş açabilir power altyapı ağır zarar dahil olmak üzere bazı dünya bölgeleri zarar yayılır. Farklı bölgeler arasındaki uzaklığı uygun bir DR çözümü olarak nitelemek için yeterince büyük olmayabilir.
- Kullanılabilirlik alanları genelinde ağ gecikme süresi, farklı Azure bölgelerinde değişir. Dağıtabilir ve bir bölgeden ağ gecikmesi etkin DBMS sanal makine için bir iş işlemi etkisini kabul edilebilir olduğu SAP uygulama katmanı farklı bölgeler arasında çalıştırın. burada durumlar vardır. Ancak, bazı Azure bölgelerinde çok zorlayıcı ve SAP iş süreçleri için etkin DBMS sanal makine ve farklı bölgelere dağıtılan SAP uygulama örneği arasında bir gecikme olabilir. Sonuç olarak, dağıtım mimarisi uygulama için bir etkin/etkin mimari veya çapraz bölge gecikme süresi çok yüksek olduğu Aktif/Pasif mimari ile farklı olması gerekir.
- Hangi yapılandırmasında farklı bölgeler arasında ölçmek için ağ gecikme süresi göre Azure kullanılabilirlik alanları kullanabileceğinizi karar verin. Ağ gecikme süresi iki alanda önemli bir rol oynar:
    - Oluşturulan bir zaman uyumlu çoğaltma için gereken iki DBMS örneği arasındaki gecikme süresi. Daha yüksek ağ gecikme süresi, yüksek etkisi ölçeklenebilirlik, iş yükünün olası
    - Ağ gecikmesi arasındaki farkı bir SAP iletişim örneği, bölge etkin DBMS örneği ile çalışan bir VM ile benzer bir VM başka bir bölge içinde. Daha yüksek bu fark, iş süreçleri ve bağımlı olup olmadığını çalıştıkları toplu işler çalıştırma süresi yüksek etkisi, bölge DBMS ile veya farklı bir bölgede.


Kullanılabilirlik alanları ve yük devretme çözümleri için aynı Azure bölgesindeki oluşturma arasında Azure Vm'leri dağıtırken bazı kısıtlamalar vardır. Bu sınırlamalar aşağıdaki listede ele alınmaktadır:

- Kullanarak [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) Azure kullanılabilirlik alanına dağıtmak için zorunludur 
- Bir Azure aboneliği temelinde sabit fiziksel bölgeleri için bölge numaralandırmalar eşleme. Farklı Aboneliklerde, SAP sistemlerini dağıtmak için kullanıyorsanız, her abonelik için ideal bölgeleri tanımlamanız gerekir.
- Azure kullanılabilirlik kümeleri Azure kullanılabilirlik alanı içinde dağıtamazsınız. Azure kullanılabilirlik alanı ya da bir Azure kullanılabilirlik sanal makineler için dağıtım çerçeve kümesi seçin.
- Kullanamazsınız bir [temel Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) yük devretme kümesi çözümleri Windows Yük devretme Küme Hizmetleri veya Linux Pacemaker göre oluşturmak için. Bunun yerine kullanmanız gereken [Azure standart yük dengeleyici SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)



## <a name="ideal-availability-zones-combination"></a>İdeal kullanılabilirlik birleşimi
Kullanılabilirlik alanları yararlanmak nasıl karar vermeden önce size bir araştırma yapabileceğiniz gerekir:

- Bir Azure bölgesinin üç farklı bölge arasındaki ağ gecikme süresi. Bu nedenle, çapraz bölge ağ trafiğini en düşük ağ gecikme ile bölgeleri seçebilirsiniz
- Seçtiğiniz VM VM gecikme tarafından seçmiş bölge içindeki iki bölgeleri arası ağ gecikmesinin arasındaki fark
- Bir deyim olup dağıtmak için ihtiyacınız olan VM türleri kullanılabilir, tercih ettiğiniz iki bölgeleri. M serisi sanal makineler ile özellikle, karşılaşabileceğiniz farklı M serisi VM SKU'ları yalnızca iki bölgeleri üç kullanılabilir nerede bulunacağını durumlarda olacak

### <a name="network-latency-between-zones-and-within-zone"></a>Bölge içinde ve bölgeler arasındaki ağ gecikme süresi
Farklı bölgeler arasında bir gecikme ne olduğunu öğrenmek için gerekir:

- DBMS Örneğinizde üç tüm bölgeler için kullanmak istediğiniz VM SKU dağıtın. Emin olun [Azure hızlandırılmış ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) Bu ölçüm gerçekleştirirken etkinleştirilir.
- En düşük ağ gecikme süresi ile iki bölgeleri buldukça, üç kullanılabilirlik alanları genelinde uygulama katmanı VM olarak kullanmak istediğiniz VM SKU'ların başka bir üç VM dağıtın. Ağ gecikme süresi iki 'DBMS VM' karşı iki farklı 'DBMS' bölgeleri tercih ettiğiniz ölçün. 
- Ölçmek için bir araç olarak kullanmak **niping**. SAP destek notları açıklandığı çalışan SAP aracından [#500235](https://launchpad.support.sap.com/#/notes/500235) ve [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Komutları için gecikme süresi ölçümlerini belgelenen SAP odaklanır. Kullanarak **ping** beri önerilmez **ping** Azure çalışmaz hızlandırılmış ağ kod yolları.

Ölçümler ve kullanılabilirlik alanlarında VM SKU'larınız kullanılabilirliğini bağlı olarak, kararlar gerekir:

- DBMS katman için ideal bölgeleri tanımlayın
- Bir, iki, tüm üç dilimlerinde etkin SAP uygulama katmanınızdaki dağıtmak istiyorsanız sorusuna yanıt farklar ağ gecikme süresi içinde bölgesine dilimlerinde dayalı
- Aktif/Pasif ya da bir uygulama açısından bir aktif/aktif yapılandırma (daha sonra bakın) dağıtmak istiyorsanız soruyu yanıtlayın.

Bu kararların aynı zamanda geri çağırma SAP'nin ağ gecikme süresi önerileri SAP Not açıklandığı gibi [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E).

### <a name="be-aware-of"></a>Dikkat edilmesi

> [!IMPORTANT]
> Ölçümler ve, kararları Bu ölçümler yapmadan kullandığınız Azure aboneliği için geçerli. Başka bir Azure aboneliği kullanıyorsanız, numaralandırılmış bölgelerin eşlemesi için başka bir Azure aboneliğine değişebilir olduğundan ölçüleri yineleme gerekir.


> [!IMPORTANT]
> Beklenen destekleyen tüm Azure bölgelerinde farklı sonuçlar yukarıda gerçekleştirildiği gibi ölçümleri gösteren [kullanılabilirlik](https://docs.microsoft.com/azure/availability-zones/az-overview). Ağ gecikme süresi gereksinimleriniz aynı olsa bile, bölgeler arasındaki ağ gecikmesini farklı olabileceği farklı Azure bölgelerindeki farklı dağıtım stratejilerini benimseyen gerekebilir. Bazı Azure bölgelerinde üç farklı bölge arasındaki ağ gecikme süresi büyük ölçüde farklı olabilir beklenmektedir. Diğer bölgelerde, üç farklı bölge arasındaki ağ gecikmesini daha tekdüzen iken. Talep olmadığını **her zaman** 2 milisaniye 1 saniyeye bölge arasında bir ağ gecikmesi olan **yanlış**. Azure bölgelerinde kullanılabilirlik alanları arası ağ gecikmesinin genelleştirilmiş olmalıdır.


## <a name="activeactive-deployment"></a>Etkin/etkin dağıtım
İki veya üç dilimlerinde etkin SAP iletişim örneklerinizi dağıtmak için bu dağıtım mimarisi aktif/aktif olarak adlandırılır. SAP Central Services'in'ı kuyruğa çoğaltma kullanarak iki bölgeleri arasında dağıtılacak geçiyor. Aynı, SAP Central hizmetiyle aynı bölgeler genelinde dağıtıma giden DBMS katmanı için geçerlidir.

Bu tür bir yapılandırma contemplate için iki kullanılabilirlik alanları, zaman uyumlu DBMS çoğaltma ve iş yükünüz için kabul edilebilir çapraz bölge ağ gecikme süresi sunan, bölgenizdeki bulmak gerekir. Buna ek olarak, seçtiğiniz bölgeleri içinde ağ gecikme süresi arasında değişim istediğiniz ve çok büyük olmasına bölge ağ gecikme süresi arasında. İkincisi için neden, kullanmak istemiyorsanız çok büyük Çeşitlemeler İş süreçlerinizi, çalışma sürelerini veya toplu işleri, bağımlı bir iş çalışır, bölge olmadığını DBMS sunucusuyla veya bölgeler arasında emin olan. Bazı farklılıklar kabul edilebilir, ancak Etkenler farkının değil.

Aktif/Aktif dağıtımının iki bölgeleri arasında basitleştirilmiş bir şema gibi görünebilir:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

Bu yapılandırma için aşağıdaki maddeler geçerlidir:

- Kullanılabilirlik kümeleri Azure kullanılabilirlik alanlarında dağıtılamıyor olduğundan, Azure kullanılabilirlik alanları hatası ve güncelleme etki alanları tüm VM'ler için değerlendir
- Kullandığınız SAP Central Services'in DBMS katman ve yük devretme kümeleri için Azure yük dengeleyicileri ihtiyacınız olacak şekilde [standart SKU yük dengeleyicide](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). Temel Azure load balancer'a dilimlerinde çalışmaz
- Azure sanal ağı ve SAP sistemini barındırmak için dağıtılan kendi alt bölgeler arasında uzatılır. **Gerekmeyen** ayrı her bölge için sanal ağlar
- Tüm sanal dağıtım için makineleri'nin [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). Yönetilmeyen diskler bölgesel dağıtımları için desteklenmez
- Azure Premium depolama veya [Ultra SSD depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) bölgeler arasında depolama çoğaltması herhangi bir türde desteklemediğinden. Sonuç olarak, (DBMS veya SAP Central Services'in) önemli verileri çoğaltmak için uygulamanın sorumluluğudur
- NFS paylaşım (Linux) ya da aynı bir paylaşılan disk (Windows), CIFS Paylaşımı (Windows), paylaşılan sapmnt dizini için geçerlidir. Böyle bir paylaşılan disk veya paylaşım bölgeler arasında çoğaltan bir teknoloji kullanmanız gerekir. Şu anda aşağıdaki teknolojileri desteklenir:
    - Windows için bir küme çözümü kullanan açıklandığı gibi SIOS Datakeeper [SAP ASCS/SCS örneği, Azure'da bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) dilimlerinde desteklenir
    - SUSE Linux için bir NFS paylaşım açıklandığı gibi yerleşik [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) desteklenir
    - Şu anda açıklandığı gibi Windows ölçek genişletme Dosya Hizmetleri (SOFS) kullanarak çözümü [SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirlik için Azure'u hazırlama altyapı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **dilimlerinde desteklenmiyor**
- Üçüncü bir bölge oluşturmanız durumunda SBD cihaz barındırmak için kullanılan bir [SUSE Linux pacemaker kümesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) veya ek uygulama örnekleri
- İş açısından kritik işlemler için çalışma zamanı tutarlılık elde etmek için doğrudan belirli toplu işleri ve kullanıcıların bölge içinde uygulama örnekleri aşağıda belirtilenleri deneyebilirsiniz SAP Batch sunucu grupları, oturum açma grupları veya RFC grupları kullanarak etkin DBMS örneği. Ancak, bölgesel yük devretme durumunda active DB VM ile bölgeli içindeki VM'ler üzerinde çalışan örnekleri bu grupları el ile taşımanız gerekir.  
- Her uygulama örneklerinizin bir parçası kullanılan bir bölge hizmet dışı ise önceki kaynak kapasitesi hemen almak için bölge bazı etkinliği olmayan iletişim örneklerini dağıtmak istediğinize karar verin


## <a name="activepassive-deployment"></a>Aktif/Pasif dağıtım
Arasındaki ağ gecikme süresi içinde bir bölge ve çapraz bölge ağ trafiğini kabul edilebilir bir delta bulamazsanız, SAP uygulama katmanı açısından bir Aktif/Pasif karakter dağıtabileceğiniz bir mimariye sahiptir. Bölge bir 'active' alanı tam uygulama katmanına burada dağıttığınız ve burada her ikisi de etkin DBMS ve SAP Central Services'in örnek çalıştırma denemesi tanımlayın. Bu tür bir yapılandırma ile iş hareketlerinde aşırı çalıştırma Çeşitleme yok ve toplu işler, bağımlı bir proje çalıştığına göre etkin DBMS örneğiyle veya bölge emin olun.

Bu tür bir mimari temel yerleşimini şu şekilde görünür:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

Bu yapılandırma için aşağıdaki maddeler geçerlidir:

- Kullanılabilirlik kümeleri Azure kullanılabilirlik alanlarında dağıtılamıyor. Bu nedenle, bu durumda, bir güncelleştirme ve hata etki alanı için uygulama katmanınızdaki vardır. Bu yalnızca bir bölgede dağıtılır nedenidir. Bu başvuru mimarisi, bir Azure kullanılabilirlik kümesinde uygulama katmanına dağıtmak foresees karşılaştırıldığında, hafif bir setback yapılandırmadır.
- Bu tür bir mimari işletim yakından izleyin ve etkin DBMS ve SAP Central Hizmetleri örnekleri dağıtılan uygulama katmanınızdaki ile aynı bölgedeki tutmaya çalışın gerekir. SAP Central hizmet veya DBMS örneği devretmesi durumunda, el ile bölgeye mümkün olduğunca erken dağıtılan SAP uygulama katmanı ile yük devredebildiğini emin olmanız gerekir
- Kullandığınız SAP Central Services'in DBMS katman ve yük devretme kümeleri için Azure yük dengeleyicileri ihtiyacınız olacak şekilde [standart SKU yük dengeleyicide](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). Temel Azure load balancer'a dilimlerinde çalışmaz
- Azure sanal ağı ve SAP sistemini barındırmak için dağıtılan kendi alt bölgeler arasında uzatılır. **Gerekmeyen** ayrı her bölge için sanal ağlar
- Tüm sanal dağıttığınız makineler için kullanmanız gereken [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). Yönetilmeyen diskler bölgesel dağıtımları için desteklenmez
- Azure Premium depolama veya [Ultra SSD depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) bölgeler arasında depolama çoğaltması herhangi bir türde desteklemediğinden. Sonuç olarak, (DBMS veya SAP Central Services'in) önemli verileri çoğaltmak için uygulamanın sorumluluğudur
- NFS paylaşım (Linux) ya da aynı bir paylaşılan disk (Windows), CIFS Paylaşımı (Windows), paylaşılan sapmnt dizini için geçerlidir. Böyle bir paylaşılan disk veya paylaşım bölgeler arasında çoğaltan bir teknoloji kullanmanız gerekir. Şu anda aşağıdaki teknolojileri desteklenir:
    - Windows için bir küme çözümü kullanan açıklandığı gibi SIOS Datakeeper [SAP ASCS/SCS örneği, Azure'da bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) dilimlerinde desteklenir
    - SUSE Linux için bir NFS paylaşım açıklandığı gibi yerleşik [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) desteklenir
    - Şu anda açıklandığı gibi Windows ölçek genişletme Dosya Hizmetleri (SOFS) kullanarak çözümü [SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirlik için Azure'u hazırlama altyapı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **dilimlerinde desteklenmiyor**
- Üçüncü bir bölge oluşturmanız durumunda SBD cihaz barındırmak için kullanılan bir [SUSE Linux pacemaker kümesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) veya ek uygulama örnekleri
- Bölge arıza durumunda uygulama kaynaklarını başlatabilmeniz için edilgen bölgeyi (açısından bir DBMS), etkin olmayan Vm'leri dağıtma
    - Kullanamazsınız [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) etkin Vm'leri etkinliği olmayan Vm'leri bölgeler arasında çoğaltmak için. Bu anda, Azure Site Recovery böyle bir işlevi yerine getirmek mümkün değil
- İkinci bir bölgede SAP uygulama katmanına otomatik olarak başlamaya ek olarak, bölge başarısız olması durumunda, sağlar Otomasyon uygulamasına yatırım

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma yapılandırması
Microsoft Azure bölgesinde, farklı Azure kullanılabilirlik bölgeleri barındırma özellikleri arasındaki coğrafi uzaklıktan herhangi bir bilgi paylaşmaz. Bu aslında rağmen bazı müşteriler taahhüt birleşik bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma yapılandırması için bölgeler yararlanıyor bir **R**kurtarma **P**oint **O**sıfır bjective (RPO). Yani, olağanüstü durum kurtarma durumda bile herhangi bir taahhüt veritabanı işlemleri kaybederseniz değil. 

> [!NOTE]
> Böyle bir yapılandırma, yalnızca belirli durumlarda önerilir. Örneğin, veri güvenliği ve uyumluluğu nedenlerden dolayı Azure bölgesinin nerede ayrılamazsınız durumları. 

Bu grafikte gibi bu tür bir yapılandırma nasıl bakarak bir örnek gösterilmiştir:

![combined_ha_dr_in_zones](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

Bu yapılandırma için aşağıdaki maddeler geçerlidir:

- Ya da işiniz var olduğunu varsayarak önemli bir uzaklık bir kullanılabilirlik alanı barındırma özellikleri arasında. Veya bir belirli Azure bölgesiyle kalmak için zorlanır. Kullanılabilirlik kümeleri Azure kullanılabilirlik alanlarında dağıtılamıyor. Bu nedenle, bu durumda, bir güncelleştirme ve hata etki alanı ile uygulama katmanınızdaki sona erecektir. Bu yalnızca bir bölgede dağıtılır nedenidir. Bir Azure kullanılabilirlik kümesinde uygulama katmanına dağıtmak foresees başvuru mimarisi ile karşılaştırıldığında bir setback budur.
- Bu tür bir mimari işletim yakından izleyin ve etkin DBMS ve SAP Central Hizmetleri örnekleri dağıtılan uygulama katmanınızdaki ile aynı bölgedeki tutmaya çalışın gerekir. SAP Central hizmet veya DBMS örneği devretmesi durumunda, el ile bölgeye mümkün olduğunca erken dağıtılan SAP uygulama katmanı ile yük devredebildiğini emin olmanız gerekir
- Etkin QA uygulama örneklerini çalıştıran sanal makineleri önceden yüklenmiş üretim uygulama örnekleri var.
- Bölgesel bir arıza durumunda QA uygulama örnekleri kapatacak ve bunun yerine üretim örneği başlatın. Bunun çalışmasını sağlamak uygulama örnekleri için sanal adları ile çalışmak için ihtiyacınız olduğunu unutmayın
- Kullandığınız SAP Central Services'in DBMS katman ve yük devretme kümeleri için Azure yük dengeleyicileri ihtiyacınız olacak şekilde [standart SKU yük dengeleyicide](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). Temel Azure load balancer'a dilimlerinde çalışmaz
- Azure sanal ağı ve SAP sistemini barındırmak için dağıtılan kendi alt bölgeler arasında uzatılır. **Gerekmeyen** ayrı her bölge için sanal ağlar
- Tüm sanal dağıttığınız makineler için kullanmanız gereken [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). Yönetilmeyen diskler bölgesel dağıtımları için desteklenmez
- Azure Premium depolama veya [Ultra SSD depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) bölgeler arasında depolama çoğaltması herhangi bir türde desteklemediğinden. Sonuç olarak, (DBMS veya SAP Central Services'in) önemli verileri çoğaltmak için uygulamanın sorumluluğudur
- NFS paylaşım (Linux) ya da aynı bir paylaşılan disk (Windows), CIFS Paylaşımı (Windows), paylaşılan sapmnt dizini için geçerlidir. Böyle bir paylaşılan disk veya paylaşım bölgeler arasında çoğaltan bir teknoloji kullanmanız gerekir. Şu anda aşağıdaki teknolojileri desteklenir:
    - Windows için bir küme çözümü kullanan açıklandığı gibi SIOS Datakeeper [SAP ASCS/SCS örneği, Azure'da bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) dilimlerinde desteklenir
    - SUSE Linux için bir NFS paylaşım açıklandığı gibi yerleşik [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) desteklenir
    - Bu anda, açıklandığı gibi Windows ölçek genişletme Dosya Hizmetleri (SOFS) kullanarak çözümü [SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirlik için Azure'u hazırlama altyapı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **dilimlerinde desteklenmiyor**
- Üçüncü bir bölge oluşturmanız durumunda SBD cihaz barındırmak için kullanılan bir [SUSE Linux pacemaker kümesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) veya ek uygulama örnekleri





## <a name="next-steps"></a>Sonraki Adımlar
Azure kullanılabilirlik alanları genelinde dağıtmak için sonraki adımlar denetleyin:

- [Azure'da bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirlik için Azure altyapısını hazırlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






