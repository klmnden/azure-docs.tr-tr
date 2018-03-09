---
title: "Bir Azure bölgesi içinde SAP HANA kullanılabilirlik | Microsoft Docs"
description: "SAP HANA işlemleri Azure yerel vm'lerde"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/5/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 28fe9bc7c8cb1d59df404b7dd7429def54648a18
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-within-one-azure-region"></a>Bir Azure bölgesi içinde SAP HANA kullanılabilirlik
Bu bölümde, bir Azure bölgesi içinde kullanılabilirlik senaryolar açıklanmaktadır birkaç senaryo sunulur. Azure tüm dünyadaki yayılır birçok bölgeler sahiptir. Azure bölgeleri listesi için başvurun [Azure bölgeleri](https://azure.microsoft.com/regions/) makalesi. SAP HANA bir Azure bölgesi içinde vm'lerinde dağıtma, Microsoft HANA örneği ile tek bir VM'ye dağıtımını sunar. Veya yüksek kullanılabilirlik için iki VM içindeki iki HANA örnekleri ile dağıtabilirsiniz bir [Azure kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) , kullandığınız HANA sistem çoğaltma kullanılabilirlik sağlamak için. Azure ortak önizlemesini sahip [Azure kullanılabilirlik bölgeleri](https://docs.microsoft.com/azure/availability-zones/az-overview). Bu kullanılabilirlik bölgeler henüz ayrıntılı olarak ele alınacak yapmayacağınız. Bazı genel düşüncelerinizi kullanılabilirlik kümeleri kullanılabilirlik bölgeleri karşı kullanımını geçici dışında.

Azure kullanılabilirlik kümeleri ve kullanılabilirlik bölgeler arasındaki fark nedir? Kullanılabilirlik bölgeleri sunulacak nerede bulunacağını Azure bölgeleri için bölgeler güç kaynağı, soğutma ve ağ kaynağı bağımsız birden çok veri merkezi sahiptir. Tek bir Azure bölgesi içinde farklı bölgelere sunumu nedeni satırındaki veya üç kullanılabilirlik sunulan bölgeler arasında uygulamaları dağıtmak sağlamaktır. Güç kaynakları ve/veya ağ sorunları yalnızca bir kullanılabilirlik bölge altyapısını etkileyeceği varsayılarak, uygulama dağıtımınızı bir Azure bölgesi içinde hala tamamen çalışır durumdadır. Sonuçta bir bölgede bazı sanal makineleri bu yana bazı sınırlı kapasiteye sahip kaybolmuş olabilir. Ancak sanal makineleri diğer iki bölgede hala hazır ve çalışır. 
 
Azure'da içinde yerleştirin VM kaynakları emin olmak için kullanabileceğiniz bir mantıksal gruplandırma Özelliği Azure kullanılabilirlik kümesi karşın, bu olan bir Azure veri merkezi içinde dağıtıldığında birbirinden yalıtılmış hatası. Azure, bir Kullanılabilirlik Kümesi içine yerleştirdiğiniz sanal makinelerin birden fazla fiziksel sunucuda, bilgi işlem rafında, depolama biriminde ve ağ anahtarında çalışmasını sağlar. Veya bazı diğer Azure belgelerde olduğu gibi onu farklı yerleşimi verilir [güncelleştirme ve hata etki alanlarını](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Genellikle bu yerleşimi Azure veri merkezi içinde ' dir. Güç kaynakları ve/veya ağ sorunları, dağıtılan datacenter etkileyeceği varsayılarak, bir Azure bölgesindeki tüm kapasite etkilenecek.

Azure kullanılabilirlik bölgeleri temsil eden veri merkezleri yerleşimini çoğu uygulama ve veri merkezleri arasında belirli bir mesafe için kabul edilebilir farklı bölgelerde dağıtılan hizmetler arasındaki ağ gecikmesi teslim arasında bir Güvenlik Açığı ' dir. Böylece doğal catastrophes ideal olarak güç ve ağ kaynağı ve bu bölgedeki tüm kullanılabilirlik bölgeler için altyapı etkileyebilecek değil. Koyduysa, kullanılabilirlik bölgeler her zaman istediğiniz gibi tek bir bölge içinde kullanılabilirlik sağlamak kuramıyor olabilir ancak monumental doğal catastrophes olarak. Porto Riko Adası 20/08/2017 üzerinde isabet ve temel bir yakın % 100 siyah genişletme 90 mil geniş Adası neden Maria hortum düşünün.   
  


## <a name="single-vm-scenario"></a>Tek bir VM senaryo
Bu senaryoda, SAP HANA örneği için bir Azure sanal makinesi oluşturdunuz. Azure Premium Storage işletim sistemi diski ve tüm veri diskleri barındırmak için kullanılır. Azure tarafından % 99,9 çalışma süresi SLA ve diğer Azure bileşenlerinin SLA'ları, müşterilerinizin doğrultusunda, kullanılabilirlik SLA'ları karşılamak yeterli olur. Bu senaryoda DBMS katman çalıştıran VM'ler için yararlanan bir Azure kullanılabilirlik kümesi gerek yoktur. Bu senaryoda, iki farklı özellikleri kullanır:

- Azure VM otomatik (Ayrıca Azure hizmet onarma başvurulan) yeniden başlatın
- SAP HANA otomatik yeniden başlatma

Azure VM otomatik yeniden veya 'hizmet Onarma' iki düzeyde çalışır Azure içindeki bir işlevsellik olan:

- Azure sunucusu ana bilgisayar sunucu ana bilgisayarda barındırılan bir VM'nin durumu denetleniyor
- Azure yapı denetleyicisi sunucu ana kullanılabilirliğini ve sistem durumu izleme

Bir Azure sunucusu ana bilgisayarda barındırılan her VM için barındırılan sanal makine sistem durumu izleme bir sistem durumu denetimi işlevselliği. VM'ler iyi olmayan bir duruma dönmesi durumda VM durumunu denetler Azure ana bilgisayar aracısı tarafından VM yeniden başlatılabilir. Azure yapı denetleyicisi konak donanım ile ilgili sorunları gösteren birçok farklı parametreler denetleyerek konak durumu denetleniyor, ancak aynı zamanda ağ aracılığıyla konak erişilebilirlik denetler. Ana bilgisayar sorunlarının bir göstergesi gibi eylemleri neden olabilir:

- Ana bilgisayarın yeniden başlatın ve konak kötü bir durum sinyalleri varsa, ana bilgisayarda çalışan sanal makineleri yeniden başlatma
- Ana bilgisayarı ve yeniden başlatılması durumunda yeniden başlatmanın ardından konak sağlıklı bir durumda değil, ilk olarak sağlıklı bir ana bilgisayar üzerindeki ana bilgisayarda sunulan sanal makine yeniden başlatma. Bu durumda, konak olduğu olması moduna geçmesini sağlıklı ve temizlenmiş veya değiştirilen kadar başka dağıtımlar için kullanılan değil olarak işaretlenmelidir.
- Sağlıksız konak sorunları yeniden başlatma işleminde sahip olduğu durumlarda iyi bir ana bilgisayar üzerindeki VM'nin hemen yeniden başlatın. 

Ana bilgisayar ve Azure tarafından sağlanan VM izleme ile ana bilgisayar sorunları yaşar Azure VM'ler sağlıklı bir Azure ana bilgisayarda otomatik olarak yeniden başlatılır 

Böyle bir senaryoda Bel ikinci içinde yeniden VM çalıştıran HANA hizmetinizi VM yeniden başlatma otomatik olarak başlatılmasını olgu özelliğidir. [HANA hizmet otomatik yeniden başlatma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) farklı HANA Hizmetleri izleme Hizmetleri aracılığıyla yapılandırılabilir.

Bu tek bir VM senaryo bir SAP HANA yapılandırmasına soğuk yük devretme düğümü ekleyerek geliştirilmiş. Veya SAP HANA belgelerinde belirtilmiştir gibi [konak otomatik yük devretme](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html). Bu yapılandırma bir sunucu donanımı sınırlıdır ve bir dizi üretim konaklar için konak otomatik yük devretme düğüm olarak tek bir sunucu düğümü ayrılması şirket içi dağıtım durumlarda mantıklı. Ancak burada altyapının Azure sağlıklı hedef sunucu bir başarılı bir VM yeniden başlatma için sağlayan Azure gibi durumlarda, SAP HANA konak otomatik yük devretme senaryosu dağıtmak için mantıklı değildir. 

Sonuç olarak, HANA konak otomatik yük devretme için bekleme düğümü foresees hiçbir referans mimarisi sahip. Bu durum, SAP HANA genişleme yapılandırmaları için de geçerlidir.


## <a name="availability-scenarios-involving-two-different-vms"></a>İki farklı VM içeren kullanılabilirlik senaryoları
İçinde iki Azure sanal makineleri kullanarak Azure kullanılabilirlik kümeleri bir Azure kullanılabilirlik kümesindeki bir Azure bölgesi içinde bu VM'LERİN girdiyseniz bu iki VM'ler arasında yukarı süresini artırmak olanak tanır. Azure temel kurulumunda grafik aşağıda gösterildiği gibi görünür: ![tüm katmanlar ile iki VM](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Farklı bir kullanılabilirlik senaryolarını göstermek için yukarıdaki Katmanlar kesme ve sanal makineleri, ana katmanları için sınırlı grafik birkaçıdır kullanılabilirlik kümeleri ve Azure bölgeleri. Azure sanal ağlar, kaynak grupları ve abonelikleri açıklanan senaryoları için bir rol oynar yok.

### <a name="replicating-backups-to-second-virtual-machine"></a>İkinci sanal makine için çoğaltma yedekleri
En ilkel kurulumları birini yedeklemeler, özellikle, başka bir Azure sanal makineye bir sanal makineden sevk işlem günlüğü yedeklemeleri sağlamaktır. Herhangi bir Azure depolama türü seçimi sahiptir. İlk VM ikinci VM üzerinde gerçekleştirilen zamanlanmış yedeklemeler kopyasını komut dosyası için sorumlu olacaktır. İkinci VM örneklerini gerektiren kullanarak durumunda, tam, artımlı/fark ve işlem günlüğü yedeklemeleri ihtiyacınız noktasına geri yüklemek gereksiniminiz olacaktır. Mimarisi gibi görünür: ![depolama çoğaltma ile iki VM](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Bu kurulum harika RPO ve RTO süreleri sağlamak için çok uygun değil. Özellikle RTO kez tam olarak kopyalanan yedeklemeleri tam veritabanı geri yüklenmesi gereken nedeniyle düşerdi. Ancak bu kurulum ana örnekleri üzerindeki istenmeyen verileri silinmeye karşı kurtarmak için kullanılabilir. zaman içinde belirli bir noktaya geri yüklemenizi herhangi bir zamanda misiniz böyle bir kurulum ile verileri ayıklamak ve ana örneğine silinen verileri alın. Bu nedenle, bu tür yedek kopya yöntemi diğer yüksek kullanılabilirlik işlevselliği ile birlikte kullanmak için anlamlı yapabilirsiniz. Yalnızca yedeklemelerin ne zaman kopyalanır olduğu süre boyunca, alabilirsiniz ana SAP HANA daha küçük bir VM ile birlikte örnekleri çalışır. Ancak eklenebilecek VHD'ler daha az sayıda küçük VM'ye sahip göz önünde bulundurun. Denetleme [azure'daki Linux sanal makineler için Boyutlar](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) tek tek VM türlerinin sınırları için.

### <a name="using-sap-hana-system-replication-without-automatic-failover"></a>SAP HANA sistem çoğaltma otomatik yük devretme olmadan kullanma
Aşağıdaki senaryolar için SAP HANA sistem çoğaltma kullanıyor. Belge verilen SAP makaleyle başlayarak bulunabilir [sistem çoğaltma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html). Tek bir Azure bölgesindeki iki Azure sanal makineleri arasındaki bazı farklar kurtarma süresi hedefi olan iki farklı yapılandırmaları vardır. Genel olarak, otomatik yük devretme olmaması ile senaryoları bir Azure bölgesi içinde senaryolar için uygun olmayabilir. Söz konusu Azure altyapısında çoğu hatası durumlarda Azure hizmet onarma birincil VM başka bir ana bilgisayarda yeniden işaretleneceğini nedenidir. Yalnızca bu tür bir yapılandırma hatası senaryoları bakımından burada yardımcı olabilecek bazı kenar durumlar vardır. Veya bazı durumlarda bir müşteri olarak, özellikle verimliliği hayata geçirmek istiyorsanız.

#### <a name="using-hana-system-replication-without-auto-failover-and-without-data-pre-load"></a>HANA sistem çoğaltma otomatik yük devretme ve veri ön yük olmadan kullanma 
Bu, SAP HANA sistem çoğaltma zaman uyumlu bir biçimde veri taşıma amacıyla bir kurtarma noktası hedefi (RPO) 0 elde etmek için kullandığınız bir senaryodur. Diğer tarafta elinizde bir yeterince uzun kurtarma süresi hedefi (gerekmeyen RTO), bu nedenle, yük devretme ya da veri ön yük HANA örneği önbelleğine. Böyle bir durumda, daha fazla yapılandırmanızı tarafından içine öğesidir sürücü olanakları vardır:

- Sanal makinenin belleği, çoğu alan ikinci VM'yi başka bir SAP HANA örneği çalıştırabilirsiniz. Genellikle bu tür bir örneğini ikinci VM yük devretme durumunda kapatılabilir bir örneği olur. Bu nedenle, çoğaltılan veriler ikinci VM'deki hedeflenen HANA örneğinin önbelleğine yüklenen.
- Daha küçük bir VM boyutu ikinci VM olarak kullanabilirsiniz. Bir yük devretme durumunda olduğu kaynak VM boyutu VM'ye resize el ile yük devretme önce adım gerekir. Senaryo şuna benzer:

![Depolama çoğaltma ile iki VM](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Bile veri ön yük HANA sistem çoğaltma hedefi, en az 64 GB bellek gerekir ve hedef örneği bellekte rowstore verileri tutmak için yeterli belleğin dışında.

#### <a name="using-hana-system-replication-without-auto-failover-and-with-data-pre-load"></a>HANA sistem çoğaltma otomatik yük devretme olmadan ve veri ön yük ile kullanma
Önce sunulan senaryosuna ikinci VM'deki HANA örneğine çoğaltılan verileri önceden yüklenmiş farktır. Bu verileri önceden yüklenmemesi senaryoyla sahip iki avantajı da ortadan kaldırır. Bu durumda, ikinci VM üzerinde başka bir SAP HANA sistem çalıştırılamıyor. Ya da daha küçük bir VM boyutu kullanabilirsiniz. Bu nedenle, bu tümcesi müşterilerle uygulanan bir senaryodur


### <a name="using-sap-hana-system-replication-with-automatic-failover"></a>SAP HANA sistem çoğaltma otomatik yük devretme ile kullanma

Bir Azure bölgesi içinde standart kullanılabilirlik yapılandırması, müşterilerin çoğu SAP HANA ile uygulama, iki Azure SLES Linux çalıştıran sanal makineler tanımlı bir yük devretme kümesine sahip olduğu bir yapılandırmadır. SLES Linux kümesi dayanır [Pacemaker](http://www.linux-ha.org/wiki/Pacemaker) framework ile birlikte bir [STONITH](http://linux-ha.org/wiki/STONITH) aygıt. SAP HANA açısından kullanılan çoğaltma modu eşitlenir ve otomatik yük devretme yapılandırılır. İkinci bir VM'de, SAP HANA örneği değişiklik kayıtların zaman uyumlu bir akış birincil SAP HANA örneğinden alan etkin bir bekleme düğümünün gibi davranır. İşlemler HANA birincil düğümdeki uygulama tarafından kaydedilen gibi yürütme kaydı alınan ikincil SAP HANA düğüm onaylanıp kadar uygulamaya yürütme onaylamak için birincil HANA düğüm bekler. SAP HANA iki farklı zaman uyumlu çoğaltma modları. Ayrıntılar ve bu iki zaman uyumlu çoğaltma modları hakkında farklar için makaleyi okuyun [SAP HANA sistem çoğaltma için çoğaltma modları](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html)

Genel yapılandırma gibi görünüyor

![Depolama çoğaltma ve yük devretme ile iki VM](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

Bu çözüm, bir RPO elde etmenizi sağladığından seçilen = 0 ve aşırı düşük RTO kez. SAP HANA istemci bağlantısı HANA sistem çoğaltma yapılandırması bağlanmak için sanal IP adresi SAP HANA istemcilerin kullandığı şekilde yapılandırın. Bu uygulamayı bir yük devretme durumunda ikincil düğüme yeniden yapılandırmak için bir gereksinimini ortadan kaldırır. Bu çözümde, Azure VM için SKU'ları aynı olması birincil veya ikincil gerekiyor.  


## <a name="next-steps"></a>Sonraki Adımlar
Bu tür bir yapılandırma Azure ayarlama konusunda adım adım yönergeler gerekiyorsa, makaleleri okuyun:

- [SAP HANA sistem Azure VM'ler çoğaltmasında Kurulumu](sap-hana-high-availability.md)
- [SAP HANA sistemi çoğaltması kullanmak için Azure – bölümü 4 – yüksek kullanılabilirliğine, SAP](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

SAP HANA kullanılabilirliği hakkında daha fazla bilgi Azure bölgeler arasında ihtiyacınız varsa okuyun:

- [SAP HANA kullanılabilirlik Azure bölgeler arasında](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

