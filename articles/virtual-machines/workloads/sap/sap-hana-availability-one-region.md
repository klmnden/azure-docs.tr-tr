---
title: Bir Azure bölgesi içinde SAP HANA kullanılabilirliği | Microsoft Docs
description: Bir Azure bölgesindeki Azure yerel sanal makineler'de SAP HANA işlemleri açıklar.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 687012e73b4b0c869b491ac1c9ea128662b23510
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60391506"
---
# <a name="sap-hana-availability-within-one-azure-region"></a>Bir Azure bölgesi içinde SAP HANA kullanılabilirliği
Bu makalede bir Azure bölgesi içinde birden fazla kullanılabilirlik senaryoları açıklar. Azure, dünyanın her yerinde yayılan birçok bölgedeki sahiptir. Azure bölgelerinin listesi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/). Microsoft, SAP HANA vm'lerinde bir Azure bölgesi içinde dağıtmak için bir HANA örneği ile tek bir VM dağıtımı sunar. Yüksek kullanılabilirlik için iki VM içindeki iki HANA örnekleri ile dağıtabileceğiniz bir [Azure kullanılabilirlik kümesine](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) , HANA sistem çoğaltması kullanılabilirlik için kullanır. 

Şu anda Azure tarafından sağlanan [Azure kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview). Bu makalede kullanılabilirlik alanları ayrıntılı olarak açıklanmamaktadır. Ancak, kullanılabilirlik alanları kullanılabilirlik kümeleri kullanma hakkında bir genel tartışma içerir.

Kullanılabilirlik alanları Burada sunulan azure bölgeleri, birden çok veri merkezinde vardır. Güç kaynağı, soğutma ve ağ kaynağı veri merkezleri bağımsızdır. Tek bir Azure bölgesi içinde farklı bölgelere sunan nedeni, iki veya üç sunulan kullanılabilirlik alanında uygulamaları dağıtmaktır. Bölgeler arasında dağıtma gücünü verir ve yalnızca bir Azure kullanılabilirlik alanı altyapı etkileyen ağ, bir Azure bölgesi içinde uygulama dağıtımınızı çalışır durumda. Bazı sınırlı kapasite ortaya çıkabilir. Örneğin, bir bölgedeki Vm'leri kaybolmuş olabilir, ancak diğer iki bölgeleri içindeki Vm'leri hala açık ve çalışıyor olması. 
 
Azure kullanılabilirlik kümesi, Azure veri merkezinde dağıtıldığında kullanılabilirlik kümesi yerleştirdiğiniz sanal makine kaynaklarının hatası birbirinden yalıtılmış olduğundan emin olun yardımcı olan bir mantıksal gruplama yeteneğidir. Azure, bir Kullanılabilirlik Kümesi içine yerleştirdiğiniz sanal makinelerin birden fazla fiziksel sunucuda, bilgi işlem rafında, depolama biriminde ve ağ anahtarında çalışmasını sağlar. Bazı Azure belgelerinde bu yapılandırmayı farklı yerleşimi verilir [güncelleştirme ve hata etki alanları](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Bu yerleşimi, genellikle Azure veri merkezinde biçimindedir. Güç kaynağı ve ağ sorunları dağıtmakta olduğunuz veri merkezi etkileyecek varsayılarak, bir Azure bölgesindeki tüm kapasite etkiler.

Azure kullanılabilirlik alanları temsil eden veri merkezleri yerleşimini sunan bir uzaklık veri merkezleri arasında ve farklı bölgede dağıtılan hizmetler arasındaki kabul edilebilir ağ gecikme süresi arasında bir güvenlik ihlali var. Doğal taşıyabilir, ideal güç, ağ kaynağı ve altyapı için tüm kullanılabilirlik alanları bu bölgede etkiler mıydı. Ancak, iyi doğal taşıyabilir gösterildiği gibi kullanılabilirlik alanları, tek bir bölge içinde istediğiniz kullanılabilirlik her zaman sağlamayabilir. Porto Riko Adası 20 Eylül 2017'de tıklama kasırga Maria düşünün. Kasırga 90 mil geniş adaya temel bir yaklaşık yüzde 100 Kararma neden oldu.

## <a name="single-vm-scenario"></a>Tek VM senaryosu

Tek VM senaryoda, bir Azure VM için SAP HANA örneği oluşturun. İşletim sistemi diski ve tüm veri disklerini barındırmak için Azure Premium depolama kullanın. Azure çalışma süresini yüzde 99,9 SLA ve diğer Azure bileşenlerini SLA'ları, müşterileriniz için kullanılabilirlik SLA'ları karşılamak yeterlidir. Bu senaryoda, DBMS katman çalıştıran VM'ler için bir Azure kullanılabilirlik kümesine yararlanarak gerek vardır. Bu senaryoda, iki farklı özelliklerini kullanır:

- Azure VM otomatik (Ayrıca Azure hizmet onarımı olanaklarından yararlanarak olarak adlandırılır) yeniden başlatma
- SAP HANA otomatik yeniden başlatma

Azure VM otomatik yeniden başlatma veya hizmet onarımı olanaklarından yararlanarak, bir, iki düzeyde çalışan azure'da bir işlevdir:

- Azure sunucu ana sunucu ana bilgisayarda barındırılan bir sanal makine durumunu denetler.
- Azure yapı denetleyicisi, durumunu ve kullanılabilirliğini sunucu konağının izler.

Bir sistem durumu denetimi işlevselliği bir Azure sunucusu konağında barındırılan her VM izler. Bir VM, iyi durumda olmayan bir duruma girerse, VM durumunu denetleyen bir Azure konağına aracı tarafından VM yeniden başlatılabilir. Yapı denetleyicisi, konak donanımı sorunlarını belirtebilecek birçok farklı parametreler denetleyerek konak durumunu denetler. Konağın ağ aracılığıyla erişilebilirlik de denetler. Konak sorunlarının göstergesidir şu olayları neden olabilir:

- Konak sinyalleri hatalı bir durum, konağın yeniden başlatılmasını ve ana bilgisayarda çalışmakta olan sanal makinelerin yeniden başlatma tetiklenir.
- Konak sistem durumu iyi başarılı yeniden başlatma sonrası değilse, özgün olarak sağlıklı konak sunucusuna artık sağlıksız düğüm üzerinde olan sanal makinelerin bir yeniden dağıtma işlemi başlatılır. Bu durumda, özgün ana bilgisayar sağlam değil olarak işaretlenir. Değiştirilen temizlenmiş veya kadar başka dağıtımlar için kullanılmayacak.
- İyi durumda olmayan konak yeniden başlatma işlemi sırasında sorunlar varsa, hemen bir yeniden başlatma sağlıklı bir konaktaki VM'lerin tetiklenir. 

Konak ve Azure tarafından sağlanan VM izleme ile konak sorunlarla Azure Vm'lerinin Azure sağlıklı bir konağa otomatik olarak yeniden başlatılır. 

>[!IMPORTANT]
>Azure hizmet onarımı olanaklarından yararlanarak Linux konuk işletim Sisteminin bir çekirdek Panik durumda olduğu sanal makineleri yeniden başlatmaz. Sık kullanılan Linux sürümlerini varsayılan ayarları olmayan otomatik olarak yeniden başlatılmasını sanal makineleri veya Linux çekirdeğinin Panik durumda olduğu bir sunucu. Bunun yerine varsayılan işletim sistemi analiz etmek için bir çekirdek hata ayıklayıcısı iliştirebilir için çekirdek Panik durumda tutmak için foresees. Azure konuk işletim sistemi ile bir sanal makine bir böyle bir durumda otomatik olarak yeniden başlatarak bu davranışı uygularken. Bu tür örnekleri ender varsayılır. VM yeniden etkinleştirmek için varsayılan davranışı üzerine yazabilirsiniz. Varsayılan davranış /etc/sysctl.conf ' kernel.panic' parametresi etkinleştirin. Bu parametre için zaman saniyelerle ifade edilir. Ortak önerilen 20-30 saniye, bu parametre ile yeniden başlatma tetiklemeden önce beklenecek değerlerdir. Ayrıca bkz: <https://gitlab.com/procps-ng/procps/blob/master/sysctl.conf>.

Bu senaryoda dayanan ikinci çalıştırmaları yeniden başlatılan bir sanal makinede otomatik olarak başlar sonra VM'ye HANA hizmeti yeniden olgu özelliğidir. Ayarlayabileceğiniz [HANA hizmet otomatik yeniden başlatma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) çeşitli HANA Hizmetleri izleme Hizmetleri aracılığıyla.

Bu VM tek senaryo, bir SAP HANA yapılandırmasına soğuk yük devretme düğüm ekleyerek iyileştirebilir. Bu kurulum SAP HANA belgelerinde çağrılır [ana bilgisayar otomatik yük devretme](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html). Bu yapılandırmayı nereye sunucu donanımı sınırlıdır ve konak tek sunuculu düğüm ayırmayı bir şirket içi dağıtım durumda üretim konak kümesi için otomatik yük devretme düğümü mantıklı olabilir. Ancak burada altyapının Azure sağlıklı hedef sunucu için başarılı bir VM'yi yeniden başlatma sağlar, Azure'da, SAP HANA ana bilgisayar otomatik yük devretme dağıtmak için anlam ifade etmez. Azure hizmet onarımı nedeniyle, HANA ana bilgisayar otomatik yük devretme için bekleme bir düğüm foresees hiçbir başvuru mimarisi yoktur. 

### <a name="special-case-of-sap-hana-scale-out-configurations-in-azure"></a>Azure'da SAP HANA genişleme yapılandırmaları, özel durum
SAP HANA genişleme yapılandırmaları için yüksek kullanılabilirlik ve VM çalışır duruma gibi Azure sanal makineleri ve SAP HANA örneği yeniden düzeltme ve yeniden çalıştırma hizmeti kullanmaktadır. Daha sonraki bir zamanda tanıtılmak üzere HANA sistem çoğaltması üzerinde temel yüksek kullanılabilirlik mimarisi oluşturacaksınız. 


## <a name="availability-scenarios-for-two-different-vms"></a>İki farklı sanal makineler için kullanılabilirlik senaryoları

İki Azure sanal makineler bir Azure kullanılabilirlik kümesi içinde kullanırsanız, bir Azure kullanılabilirlik kümesinde tek Azure bölgesi içinde yerleştirilir, bu iki VM arasında çalışma süresini artırabilir. Azure'da temel kurulum gibi görünür:

![Tüm Katmanlar ile iki VM diyagramı](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Farklı kullanılabilirlik senaryolarını göstermek için Diyagramdaki katmanlar birkaçını göz ardı edilir. Diyagramda, VM'ler, konaklar, kullanılabilirlik kümeleri ve Azure bölgeleri belirleyen katmanları gösterilmektedir. Azure sanal ağ örnekleri, kaynak grupları ve abonelikler yoksa bu bölümde açıklanan senaryolarında bir rol oynar.

### <a name="replicate-backups-to-a-second-virtual-machine"></a>İkinci bir sanal makine çoğaltma yedeklemelerin

En temel ayarlar, yedeklemeler kullanmaktır. Özellikle, bir VM'den başka bir Azure VM ile birlikte gelen işlem günlüğü yedeklemeleri olabilir. Azure depolama türünü seçebilirsiniz. Bu kurulum, ikinci bir sanal makine için ilk VM'de gerçekleştirilen zamanlanmış yedeklemeler kopyasını komut dosyası için sorumlu olursunuz. İkinci sanal makine örnekleri'ni kullanmanız gerekiyorsa tam, artımlı/değişiklik ve işlem günlüğü yedeklemeleri gereken noktasına geri yüklemelisiniz. 

Mimari aşağıdaki gibi görünür:

![Depolama çoğaltması ile iki VM diyagramı](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Bu kurulum, harika bir kurtarma noktası hedefi (RPO) ve kurtarma süresi hedefi (RTO) süreleri elde etmek için pek uygun değildir. RTO kez tam olarak kopyalanmış yedeklerin kullanarak tam bir veritabanı geri yükleme gereksiniminden nedeniyle özellikle etkilese. Ancak, bu Kurulum, ana örneklerinde istenmeyen verileri silinmeye karşı kurtarılması için kullanışlıdır. Herhangi bir zamanda bu kurulum ile belirli bir noktaya geri yükleme, verileri ayıklamak ve Silinen veriler, ana örneğine içeri aktarabilirsiniz. Bu nedenle, bir yedek kopya yöntemi, diğer yüksek kullanılabilirlik işlevselliği ile birlikte kullanmak için mantıklı olabilir. 

Yedeklemeleri kopyalanır, ancak SAP HANA örneği üzerinde çalıştığı ana VM değerinden daha küçük bir VM kullanmanız mümkün olabilir. VHD daha küçük bir sayı için en küçük Vm'lerden ekleyebilirsiniz aklınızda bulundurun. Tek VM türleri sınırları hakkında daha fazla bilgi için bkz: [azure'da Linux sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

### <a name="sap-hana-system-replication-without-automatic-failover"></a>SAP HANA sistem çoğaltma otomatik yük devretme olmadan

Bu bölümde açıklanan senaryolardan SAP HANA sistem çoğaltması kullanın. SAP belgelerine bakın [sistem çoğaltma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html). Senaryoları otomatik yük devretme olmadan bir Azure bölgesi içinde yapılandırmaları için ortak değildir. Bir yapılandırma olmadan otomatik yük devretme, bir Pacemaker Kurulum önleme rağmen izlemenize ve yük devretme el ile obligates. Bu alır ve de çabalarınızı itibaren müşterilerin çoğu iyileştirme bunun yerine bir Azure hizmeti FQDN'yi. Bu yapılandırma hatası senaryoları açısından burada yardımcı olabilecek bazı uç örnekler vardır. Veya bazı durumlarda, daha fazla verimlilik hayata geçirmek bir müşteri isteyebilirsiniz.

#### <a name="sap-hana-system-replication-without-auto-failover-and-without-data-preload"></a>SAP HANA sistem çoğaltma otomatik yük devretme ve önyükleme veri olmadan

Bu senaryoda, 0'ın bir RPO elde etmek için zaman uyumlu bir şekilde veri taşımak için SAP HANA sistem çoğaltması kullanın. Öte yandan, yük devretme veya HANA örneği önbelleğe önceden veri gerekmeyen yeterince uzun bir RTO sahip. Bu durumda, aşağıdaki eylemleri gerçekleştirerek daha fazla yapılandırmanızı ekonomisine elde etmek mümkündür:

- İkinci bir sanal makine içinde başka bir SAP HANA örneği çalıştırın. İkinci VM SAP HANA örneğinde bellek sanal makinenin çoğunu alır. İkinci bir sanal makine bir yük devretme, ikinci sanal makine tam olarak yüklenen verileri içeren ve böylece çoğaltılan veriler, ikinci bir sanal makine hedeflenen HANA örneğinde önbelleğine yüklenebilir çalışan SAP HANA örneği kapatmanız gerekir. durumda.
- İkinci VM üzerinde daha küçük bir VM boyutu kullanın. Bir yük devretme gerçekleşirse, el ile yük devretmeden önce ek bir adım vardır. Bu adımda, kaynak VM boyutu için VM'yi yeniden boyutlandırın. 
 
Senaryo şuna benzer:

![Depolama çoğaltması ile iki VM diyagramı](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Veri preload HANA sistem çoğaltma hedef kullanmasanız bile, en az 64 GB bellek gerekir. Ayrıca, hedef örnek bellekte rowstore verileri tutmak için 64 GB yanı sıra yeterli bellek gerekir.

#### <a name="sap-hana-system-replication-without-auto-failover-and-with-data-preload"></a>SAP HANA sistem çoğaltma otomatik yük devretme olmadan ve verileri önyükle

Bu senaryoda, ikinci VM HANA örneğinde çoğaltılan verilerini birlikte önceden yüklenir. Bu verileri önceden olmayan iki avantajlarını ortadan kaldırır. Bu durumda, ikinci bir sanal makine üzerinde başka bir SAP HANA sisteminde çalıştıramazsınız. Ayrıca, daha küçük bir VM boyutu kullanamazsınız. Bu nedenle, müşteriler, bu senaryo nadiren uygulayın.

### <a name="sap-hana-system-replication-with-automatic-failover"></a>Otomatik Yük devretme ile SAP HANA sistem çoğaltması

Standart ve en yaygın kullanılabilirlik yapılandırması, bir Azure bölgesi, iki Azure SLES Linux çalıştıran Vm'leri içinde tanımlanan bir yük devretme kümesi vardır. SLES Linux kümesi dayanır [Pacemaker](http://www.linux-ha.org/wiki/Pacemaker) framework ile birlikte bir [STONITH](http://linux-ha.org/wiki/STONITH) cihaz. 

Bir SAP HANA açısından kullanılan çoğaltma modu eşitlenir ve otomatik yük devretme yapılandırılır. İkinci VM SAP HANA örneği etkin bekleme düğümü olarak görev yapar. Bekleme düğümün eşzamanlı bir akış değişiklik kayıtları birincil SAP HANA örneği alır. İşlem HANA birincil düğümdeki uygulama tarafından kaydedilen gibi uygulama için yürütme ikincil SAP HANA düğüm yürütme kaydı alınan doğrulayana kadar onaylamak için birincil HANA düğüm bekler. SAP HANA iki zaman uyumlu çoğaltma modu sunar. Ayrıntılarını ve bu iki zaman uyumlu çoğaltma modu arasındaki farkların açıklaması için SAP bkz [SAP HANA sistem çoğaltması çoğaltma modu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html).

Genel yapılandırma şuna benzer:

![Depolama çoğaltma ve yük devretme ile iki VM diyagramı](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

Çünkü bir RPO elde etmenizi sağlar, bu çözümü tercih edebileceğiniz = 0 ve düşük bir RTO. SAP HANA istemciler, HANA sistem çoğaltma yapılandırmaya bağlanmak için sanal IP adresi kullanır. böylece, SAP HANA istemci bağlantısı yapılandırın. Bu tür bir yapılandırma ikincil düğüme bir yük devretme gerçekleşirse uygulamasını yeniden gereğini ortadan kaldırır. Bu senaryoda, birincil ve ikincil VM'ler için Azure VM SKU'ları aynı olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu yapılandırmalar azure'da ayarlama hakkında adım adım yönergeler için bkz:

- [Azure vm'lerde SAP HANA sistem çoğaltması ayarlama](sap-hana-high-availability.md)
- [SAP hana sistem çoğaltması kullanarak yüksek kullanılabilirlik](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

Azure bölgeleri arasında SAP HANA kullanılabilirliği hakkında daha fazla bilgi için bkz:

- [Azure bölgeleri arasında SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

