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
ms.openlocfilehash: 3772dbdc8582eea1b2eac368784878a8a36d34ad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62125329"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>Azure Kullanılabilirlik Alanlarıyla SAP iş yükü yapılandırmaları
[Azure kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview) Azure sağlayan bir yüksek kullanılabilirlik özelliklerinden biridir. Kullanılabilirlik alanları kullanarak SAP iş yüklerinizi azure'da genel kullanılabilirliğini artırır. Bu özellik zaten bazı durumlarda kullanılabilir [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/). Gelecekte daha fazla bölgede kullanılabilir olacaktır.

Bu grafik, SAP yüksek kullanılabilirlik temel mimarisini gösterir:

![Standart yüksek kullanılabilirlik yapılandırması](./media/sap-ha-availability-zones/standard-ha-config.png)

SAP uygulama katmanına bir Azure genelinde dağıtılan [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). SAP Central Services'in yüksek kullanılabilirlik için ayrı bir kullanılabilirlik kümesinde iki VM dağıtabilirsiniz. Windows Server Yük Devretme Kümelemesi ya da Pacemaker (Linux) bir altyapı veya yazılım sorunu olması durumunda otomatik yük devretme ile yüksek kullanılabilirlik çerçeve kullanın. Bu dağıtımları hakkında daha fazla bilgi için bkz:

- [Bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Dosya paylaşımı kullanarak bir Windows Yük devretme kümesinde Küme: SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [SAP uygulamaları için SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Azure sanal makineler Red Hat Enterprise Linux üzerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Benzer bir mimariye DBMS katmanı SAP NetWeaver, S/4HANA veya Hybris sistemleri için geçerlidir. Bir altyapı ya da yazılım arızasına karşı korumak için bir yük devretme kümesi çözümünü ile Aktif/Pasif modda DBMS katman dağıtırsınız. Yük devretme kümesi çözümünü bir DBMS özgü yük devretme framework, Windows Server Yük Devretme Kümelemesi veya Pacemaker olabilir.

Azure kullanılabilirlik alanları kullanarak aynı mimariyi dağıtmak için daha önce açıklanan mimarisi için bazı değişiklikler yapmanız gerekir. Bu makalede, bu değişiklikler açıklanmaktadır.

## <a name="considerations-for-deploying-across-availability-zones"></a>Kullanılabilirlik alanları genelinde dağıtma konuları


Kullanılabilirlik alanları kullanırken aşağıdakileri göz önünde bulundurun:

- Bir Azure bölgesi içinde çeşitli kullanılabilirlik alanları arasındaki uzaklıkları ilgili tutarlılık garantisi yoktur.
- Kullanılabilirlik alanları, ideal bir DR çözümü değildir. Doğal felaketlere ağır zarar power altyapıları dahil olmak üzere dünya bölgelerde yaygın zarar görmesine neden olabilir. Uzaklıkları çeşitli bölgeler arasında uygun bir DR çözümü oluşturan için yeterince büyük olmayabilir.
- Kullanılabilirlik alanları arası ağ gecikmesinin tüm Azure bölgelerinde aynı değil. Bazı durumlarda, dağıtın ve kabul edilebilir bir bölgeden ağ gecikmesi DBMS VM etkin olduğundan, SAP uygulama katmanı farklı bölgeler arasında çalıştırın. Ancak, bazı Azure bölgelerinde etkin DBMS sanal makine ve SAP uygulama örneği arasında bir gecikme farklı bölgelerde dağıtıldığında, SAP iş süreçleri için kabul edilebilir olmayabilir. Bu durumlarda, dağıtım mimarisi uygulama için bir etkin/etkin mimari veya bölgeler arası ağ gecikme süresi çok yüksek olduğu bir Aktif/Pasif mimari ile farklı olması gerekir.
- Kararınız bölgeler arasındaki ağ gecikme süresi, kullanılabilirlik ana bölgelerini kullanmak karar verirken temel. Ağ gecikmesi iki alanda önemli bir rol oynar:
    - Zaman uyumlu çoğaltma için gereken iki DBMS örneği arasındaki gecikme süresi. İş yükünüz ölçeklenebilirliğini etkiler daha yüksek ağ gecikme süresi, daha büyük olasılıkla.
    - Ağ gecikmesi arasındaki farkı bir SAP iletişim örneği, bölge etkin DBMS örneği ile çalışan bir VM ile benzer bir VM başka bir bölge içinde. Bu fark arttıkça, etkileyebileceği iş süreçleri ve batch çalışma süresi de artar, bağımlı olup olmadığını çalıştıkları işleri, bölge DBMS ile veya farklı bir bölgede.

Kullanılabilirlik alanları genelinde Azure Vm'leri dağıtma ve aynı Azure bölgesindeki yük devretme çözümler oluşturmak, bazı kısıtlamalar geçerlidir:

- Kullanmalısınız [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) için Azure kullanılabilirlik alanları dağıttığınızda. 
- Bir Azure aboneliği temelinde sabit fiziksel bölgeleri için bölge numaralandırmalar eşleme. Farklı Aboneliklerde, SAP sistemlerini dağıtmak için kullanıyorsanız, her abonelik için ideal bölgeleri tanımlamanız gerekir.
- Azure kullanılabilirlik kümeleri Azure kullanılabilirlik alanı içinde dağıtamazsınız. Bir ya da diğer sanal makineler için dağıtım çerçeve seçin.
- Kullanamazsınız bir [temel Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) yük devretme kümesi çözümleri Windows Server Yük Devretme Kümelemesi veya Linux Pacemaker göre oluşturmak için. Bunun yerine, kullanmanız gereken [Azure standart yük dengeleyici SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones).



## <a name="the-ideal-availability-zones-combination"></a>İdeal kullanılabilirlik birleşimi
Kullanılabilirlik ana bölgelerini kullanmak nasıl karar vermeden önce belirlemeniz gerekir:

- Bir Azure bölgesinin üç bölgeler arasındaki ağ gecikme süresi. Bu bölgeler arası ağ trafiği düşük ağ gecikme süresi ile bölgeleri seçmenize olanak sağlar.
- Seçtiğiniz, bölgeleri, biri içinde VM VM gecikme süresi ve seçtiğiniz iki bölgeleri arası ağ gecikmesinin arasındaki fark.
- Dağıtmanız gereken VM türleri seçtiğiniz iki bölgeleri kullanılabilir olup olmadığını belirleme. Bazı VM'ler özellikle M serisi VM'ler, bazı SKU'ları yalnızca iki bölgeleri üç kullanılabilir olduğu durumlarla karşılaşabilirsiniz.

## <a name="network-latency-between-and-within-zones"></a>Ağ gecikme süresini bölgeler içinde ve arasında
Farklı bölgeler arasında gecikme süresini belirlemek için gerekir:

- DBMS Örneğinizde üç tüm bölgeler için kullanmak istediğiniz VM SKU dağıtın. Emin [Azure hızlandırılmış ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) Bu ölçüm alırken etkinleştirilir.
- En düşük ağ gecikme süresi ile iki bölgeleri bulduğunuzda, üç kullanılabilirlik alanında VM uygulama katmanı kullanmak istediğiniz VM SKU'ların başka bir üç VM dağıtın. Seçtiğiniz iki DBMS bölgelerde iki DBMS VM karşı ağ gecikme süresini ölçün. 
- Kullanım **niping** ölçüm aracı olarak. Bu araç, SAP, SAP destek notları açıklanan [#500235](https://launchpad.support.sap.com/#/notes/500235) ve [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Komutları için gecikme süresi ölçümlerini belgelenen odaklanır. Çünkü **ping** işe yaramazsa Azure hızlandırılmış ağ kod yollarını biz bunu kullanmanız önerilmez.

Ölçülerinizi ve kullanılabilirlik alanlarında VM SKU'larınız kullanılabilirliğini bağlı olarak, bazı kararlar gerekir:

- DBMS katman için ideal bölgelerini tanımlar.
- Bir, iki veya üç tüm bölgeler arasında etkin SAP uygulama katmanınızdaki dağıtmak ağ gecikme süresi içinde bölgesine farklar dilimlerinde göre isteyip istemediğinizi belirleyin.
- Aktif/Pasif yapılandırma veya bir aktif/aktif yapılandırma, bir uygulama açısından dağıtmak isteyip istemediğinizi belirleyin. (Bu yapılandırmalar, bu makalenin sonraki bölümlerinde açıklanmıştır.)

Bu kararları vermekte da hesap SAP'nin ağ gecikme süresi önerileri SAP Not açıklandığı gibi göz [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E).

> [!IMPORTANT]
> Ölçümler ve aldığınız kararlara ölçümleri gerçekleştirdiğiniz yapılandırırken kullandığınız Azure aboneliği için geçerli değildir. Başka bir Azure aboneliği kullanıyorsanız, ölçüleri tekrarlamanız gerekir. Numaralandırılmış bölgelerin eşlemesi başka bir Azure aboneliği için farklı olabilir.


> [!IMPORTANT]
> Daha önce açıklanan ölçümleri destekleyen her bir Azure bölgesinde farklı sonuçlar sağladığını beklenmez [kullanılabilirlik](https://docs.microsoft.com/azure/availability-zones/az-overview). Ağ gecikme süresi gereksinimleriniz aynı olsa bile, bölgeler arasındaki ağ gecikmesini farklı olabileceği için farklı Azure bölgelerindeki farklı dağıtım stratejilerini benimseyen gerekebilir. Bazı Azure bölgelerinde üç farklı bölgeler arasında ağ gecikme süresi büyük ölçüde farklı olabilir. Diğer bölgelerde, üç farklı bölgeler arasında ağ gecikme süresi, daha tekdüzen olabilir. Her zaman talep 1 ve 2 milisaniye arasındaki bir ağ gecikmesini doğru değil. Azure bölgelerinde kullanılabilirlik alanları arası ağ gecikmesinin genelleştirilmiş olmalıdır.

## <a name="activeactive-deployment"></a>Etkin/etkin dağıtım
İki veya üç dilimlerinde etkin, SAP uygulama sunucuları dağıtmak için bu dağıtım mimarisi aktif/aktif olarak adlandırılır. Sıraya alma çoğaltma kullanan SAP Central Services'in örneği iki bölgeleri arasında dağıtılır. Aynı, SAP Central hizmet olarak aynı alanları genelinde dağıtılan DBMS katmanı için geçerlidir.

Bu yapılandırmayı dikkate alındığında, iş yükünüzü ve DBMS zaman uyumlu çoğaltma için kabul edilebilir Bu teklif bölgeler arası ağ gecikmesi bölgenizde iki kullanılabilirlik bulmanız gerekir. Ayrıca emin olmak için seçtiğiniz bölgeleri içinde ağ gecikme süresi arasında değişim istediğiniz ve bölgeler arası ağ gecikme süresi çok büyük değildir. Büyük Çeşitlemeler istemediğiniz için bu, bağlı olarak bir iş bölge DBMS sunucuyla çalıştığına veya boyunca, iş sürecinizdeki veya toplu işleri çalıştırma sürelerini bölgeleri. Bazı farklılıklar kabul edilebilir, ancak Etkenler farkının değil.

Basitleştirilmiş bir şema aktif/aktif dağıtımının iki bölgeleri arasında şu şekilde görünebilir:

![Aktif/Aktif bölge dağıtımı](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

Bu yapılandırma için aşağıdaki maddeler geçerlidir:

- Kullanılabilirlik kümeleri Azure kullanılabilirlik alanları dağıtılamıyor çünkü Azure kullanılabilirlik alanları tüm VM'ler için hata ve güncelleme etki alanları kabul eder.
- SAP Central Services'in DBMS katman ve yük devretme kümeleri için yük Dengeleyiciler, kullanmanız gereken [standart SKU Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). Temel Load Balancer dilimlerinde çalışmaz.
- Bölgeler birlikte alt ağları olan, SAP sistemini barındırmak için dağıtılan Azure sanal ağı uzatılır. Sanal ağlar her bölge için ayrı yoktur.
- Tüm sanal dağıttığınız makineler için kullanmanız gereken [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). Yönetilmeyen diskler, bölgesel dağıtımları için desteklenmez.
- Azure Premium depolama ve [Ultra SSD depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) dilimlerinde herhangi bir türde depolama çoğaltmasını desteklemez. Uygulama (DBMS veya SAP Central Services'in) önemli verilerin çoğaltılması gerekir.
- Aynı paylaşılan disk (Windows), CIFS Paylaşımı (Windows) veya bir NFS paylaşımına (Linux) paylaşılan sapmnt dizini için geçerlidir. Bu paylaşılan diskler veya paylaşımları bölgeleri arasında çoğaltılan bir teknoloji kullanmanız gerekir. Bu teknolojiler desteklenir:
  - Windows için bir küme çözümü kullanan SIOS DataKeeper açıklandığı gibi [SAP ASCS/SCS örneği, Azure'da bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk).
  - SUSE Linux için bir NFS paylaşım açıklandığı gibi yerleşik [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
    
    Şu anda açıklandığı gibi Microsoft genişleme dosya sunucusu kullanan çözüm [SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirlik için Azure'u hazırlama altyapı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share), değil bölgeler arasında desteklenir.
- Üçüncü bir bölge oluşturmanız durumunda SBD cihaz barındırmak için kullanılan bir [SUSE Linux Pacemaker küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) veya ek uygulama örnekleri.
- İş açısından kritik işlemler için çalışma zamanı tutarlılık elde etmek için doğrudan belirli toplu işleri ve kullanıcıların bölge içinde uygulama örnekleri aşağıda belirtilenleri deneyebilirsiniz SAP kullanarak etkin DBMS örneği ile sunucu grupları, oturum açma grupları veya RFC grupları toplu. Ancak, bölgesel yük devretme durumunda, bölge içindeki VM'ler üzerinde çalışan örnekleri bu grupları el ile taşımanız gerekir etkin DB VM ile.  
- Her bölge etkin olmayan iletişim örnekleri dağıtmak isteyebilirsiniz. Bu uygulama örneklerinizin bir parçası kullanılan bir bölge hizmet dışı ise bir önceki kaynak kapasitesini anında geri dönmek etkinleştirmektir.


## <a name="activepassive-deployment"></a>Aktif/Pasif dağıtım
Kabul edilebilir bir delta arasındaki ağ gecikme süresi bir bölge içinde ve bölgeler arası ağ trafiğinin gecikme süresini bulamazsanız, SAP uygulama katmanı açısından Aktif/Pasif karakteri olan bir mimari dağıtabilirsiniz. Tanımladığınız bir *etkin* bölgeyi tam uygulama katmanına dağıtmak ve hem active DBMS hem de SAP Central Services'in örneği çalıştırma girişimi burada bölgedir. Bu tür bir yapılandırma ile farklılıkları, bir işi çalıştığına göre etkin DBMS örneği ile veya iş hareketleri içinde değil, bölge ve batch işleri uç çalışma zamanı olmayan emin olmanız gerekir.

Mimarinin temel düzeni şöyle görünür:

![Aktif/Pasif bölge dağıtımı](./media/sap-ha-availability-zones/active_passive_zones_deployment.png)

Bu yapılandırma için aşağıdaki maddeler geçerlidir:

- Kullanılabilirlik kümeleri Azure kullanılabilirlik alanları dağıtılamıyor. Bu nedenle, bu durumda, bir güncelleştirme ve hata etki alanı için uygulama katmanınızdaki sizde. Yalnızca tek bir bölgede dağıtılan olmasıdır. Bu yapılandırma, dağıttığınız bir Azure kullanılabilirlik kümesinde uygulama katmanı önerir başvuru mimarisini daha az tercih edilir.
- Bu mimari kullandığınızda, durum yakından izleyin ve etkin DBMS ve SAP Central Services'in örnekleri dağıtılan uygulama katmanınızdaki ile aynı bölgedeki tutmaya çalışın gerekir. SAP Central hizmet veya DBMS örneği devretmesi durumunda, el ile bölgeye mümkün olduğunca hızlı bir şekilde dağıtılan SAP uygulama katmanı ile yük devredebildiğini emin olmanız gerekir.
- SAP Central Services'in DBMS katman ve yük devretme kümeleri için yük Dengeleyiciler, kullanmanız gereken [standart SKU Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). Temel Load Balancer dilimlerinde çalışmaz.
- Bölgeler birlikte alt ağları olan, SAP sistemini barındırmak için dağıtılan Azure sanal ağı uzatılır. Sanal ağlar her bölge için ayrı yoktur.
- Tüm sanal dağıttığınız makineler için kullanmanız gereken [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). Yönetilmeyen diskler, bölgesel dağıtımları için desteklenmez.
- Azure Premium depolama ve [Ultra SSD depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) dilimlerinde herhangi bir türde depolama çoğaltmasını desteklemez. Uygulama (DBMS veya SAP Central Services'in) önemli verilerin çoğaltılması gerekir.
- Aynı paylaşılan disk (Windows), CIFS Paylaşımı (Windows) veya bir NFS paylaşımına (Linux) paylaşılan sapmnt dizini için geçerlidir. Bu paylaşılan diskler veya paylaşımları bölgeleri arasında çoğaltılan bir teknoloji kullanmanız gerekir. Bu teknolojiler desteklenir:
    - Windows için bir küme çözümü kullanan SIOS DataKeeper açıklandığı gibi [SAP ASCS/SCS örneği, Azure'da bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk).
    - SUSE Linux için bir NFS paylaşım açıklandığı gibi yerleşik [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
    
  Şu anda açıklandığı gibi Microsoft genişleme dosya sunucusu kullanan çözüm [SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirlik için Azure'u hazırlama altyapı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share), değil bölgeler arasında desteklenir.
- Üçüncü bir bölge oluşturmanız durumunda SBD cihaz barındırmak için kullanılan bir [SUSE Linux Pacemaker küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) veya ek uygulama örnekleri.
- Pasif etkinliği olmayan VM'ler dağıtmalısınız bölge arıza durumunda uygulama kaynaklarını başlatabilmeniz bölge (bir DBMS açısından).
    - [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) etkinliği olmayan Vm'leri bölgeler arasında etkin Vm'lerini çoğaltma şu anda işleyemiyor. 
- Veren, ikinci bir bölgede SAP uygulama katmanına otomatik olarak başlamaya ek olarak, bölge başarısız olması durumunda Automation yatırım yapmalısınız.

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma yapılandırması
Microsoft bu konak farklı Azure kullanılabilirlik alanları bir Azure bölgesinde coğrafi uzaklıkları tesis arasında hakkındaki tüm bilgileri paylaşmadığından. Yine de bazı müşteriler bölgeleri, sıfır kurtarma noktası hedefi (RPO) taahhüt birleşik yüksek kullanılabilirlik ve olağanüstü durum kurtarma için bir yapılandırma kullanıyor. Başka bir deyişle, olağanüstü durum kurtarma durumunda bile herhangi bir taahhüt veritabanı işlemleri kaybetmek olmamalıdır. 

> [!NOTE]
> Böyle bir yapılandırma yalnızca belirli durumlarda kullanmanızı öneririz. Örneğin, veri güvenlik veya uyumluluk nedenleriyle Azure bölgesi ayrıldığında, onu kullanabilirsiniz. 

Bu tür bir yapılandırma nasıl görünebileceği ilişkin bir örnek aşağıda verilmiştir:

![Yüksek kullanılabilirlik DR bölgelerde birleştirilmiş](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

Bu yapılandırma için aşağıdaki maddeler geçerlidir:

- Ya da işiniz bir kullanılabilirlik alanı barındırma özellikleri arasında önemli bir uzaklık yok veya bir belirli Azure bölgesi içinde kalmak zorunda varsayılarak. Kullanılabilirlik kümeleri Azure kullanılabilirlik alanları dağıtılamıyor. Bu nedenle, bu durumda, bir güncelleştirme ve hata etki alanı için uygulama katmanınızdaki sizde. Yalnızca tek bir bölgede dağıtılan olmasıdır. Bu yapılandırma, dağıttığınız bir Azure kullanılabilirlik kümesinde uygulama katmanı önerir başvuru mimarisini daha az tercih edilir.
- Bu mimari kullandığınızda, durum yakından izleyin ve etkin DBMS ve SAP Central Services'in örnekleri dağıtılan uygulama katmanınızdaki ile aynı bölgedeki tutmaya çalışın gerekir. SAP Central hizmet veya DBMS örneği devretmesi durumunda, el ile bölgeye mümkün olduğunca hızlı bir şekilde dağıtılan SAP uygulama katmanı ile yük devredebildiğini emin olmanız gerekir.
- Üretim uygulama örnekleri etkin QA uygulama örneklerini çalıştıran sanal makineleri önceden yüklenmiş olması gerekir.
- Bir bölge başarısız olması durumunda QA uygulama örnekleri kapatacak ve üretim örnekleri bunun yerine başlatın. Bunun çalışmasını sağlamak için sanal uygulama örneklerinin adları kullanmanız gerektiğini unutmayın.
- SAP Central Services'in DBMS katman ve yük devretme kümeleri için yük Dengeleyiciler, kullanmanız gereken [standart SKU Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). Temel Load Balancer dilimlerinde çalışmaz.
- Bölgeler birlikte alt ağları olan, SAP sistemini barındırmak için dağıtılan Azure sanal ağı uzatılır. Sanal ağlar her bölge için ayrı yoktur.
- Tüm sanal dağıttığınız makineler için kullanmanız gereken [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). Yönetilmeyen diskler, bölgesel dağıtımları için desteklenmez.
- Azure Premium depolama ve [Ultra SSD depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) dilimlerinde herhangi bir türde depolama çoğaltmasını desteklemez. Uygulama (DBMS veya SAP Central Services'in) önemli verilerin çoğaltılması gerekir.
- Aynı paylaşılan disk (Windows), CIFS Paylaşımı (Windows) veya bir NFS paylaşımına (Linux) paylaşılan sapmnt dizini için geçerlidir. Bu paylaşılan diskler veya paylaşımları bölgeleri arasında çoğaltılan bir teknoloji kullanmanız gerekir. Bu teknolojiler desteklenir:
    - Windows için bir küme çözümü kullanan SIOS DataKeeper açıklandığı gibi [SAP ASCS/SCS örneği, Azure'da bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk).
    - SUSE Linux için bir NFS paylaşım açıklandığı gibi yerleşik [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).

  Şu anda açıklandığı gibi Microsoft genişleme dosya sunucusu kullanan çözüm [SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirlik için Azure'u hazırlama altyapı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share), değil bölgeler arasında desteklenir.
- Üçüncü bir bölge oluşturmanız durumunda SBD cihaz barındırmak için kullanılan bir [SUSE Linux Pacemaker küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) veya ek uygulama örnekleri.





## <a name="next-steps"></a>Sonraki adımlar
Azure kullanılabilirlik alanları genelinde dağıtmak için bir sonraki adımlardan birkaçı şunlardır:

- [Azure'da bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirlik için Azure altyapısını hazırlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






