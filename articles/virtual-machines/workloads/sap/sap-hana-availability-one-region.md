---
title: Bir Azure bölgesi içinde SAP HANA kullanılabilirlik | Microsoft Docs
description: Bir Azure bölgesindeki Azure yerel VM'ler SAP HANA işlemlerini açıklar.
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
ms.date: 03/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 47dba73a6c22d11953485a69435000d3d2fe6f55
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="sap-hana-availability-within-one-azure-region"></a>Bir Azure bölgesi içinde SAP HANA kullanılabilirlik
Bu makale bir Azure bölgesi içinde birkaç kullanılabilirlik senaryoları açıklanmıştır. Azure dünya genelinde yaygın birçok bölgeler sahiptir. Azure bölgeleri listesi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/). SAP HANA vm'lerde bir Azure bölgesinde dağıtmak için Microsoft HANA örneği ile tek bir VM'ye dağıtımını sunar. Yüksek kullanılabilirlik için iki VM içinde iki HANA örneğiyle dağıtabilmeniz için bir [Azure kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) kullanılabilirlik için HANA sistem çoğaltma kullanır. 

Azure genel önizlemesi şu anda sunumu [Azure kullanılabilirlik bölgeler (Önizleme)](https://docs.microsoft.com/azure/availability-zones/az-overview). Bu makalede, biz kullanılabilirlik bölgeleri ayrıntılı belirtmeyiz. Ancak, kullanılabilirlik bölgeleri karşı kullanılabilirlik kümelerini kullanarak hakkında genel tartışma dahil etmeyin.

Azure bir kullanılabilirlik kümesinin ve kullanılabilirlik bölge arasındaki fark nedir? Kullanılabilirlik bölgeleri Burada sunulan azure bölgeleri birden çok veri merkezi vardır. Veri merkezleri içinde güç kaynağı, soğutma ve ağ kaynağı bağımsızdır. Sunulan iki veya üç kullanılabilirlik dilimlerinde uygulamaları dağıtabilmek için tek bir Azure bölgesi içinde farklı bölgelere sunumu nedenidir. Güç kaynağı veya ağ sorunları yalnızca bir kullanılabilirlik bölge altyapısı etkileyeceği varsayılarak, uygulama dağıtımınızı bir Azure bölgesi içinde hala kullanılabilirlik bölgeleri kullanırsanız tamamen çalışır durumdadır. Bazı sınırlı kapasite ortaya çıkabilir. Örneğin, bir bölgede VM'ler kaybolmuş olabilir, ancak diğer iki bölgede VM'ler hala açık ve çalışıyor olması. 
 
Bir Azure kullanılabilirlik kümesi, bir Azure veri merkezi içinde dağıtıldığında kullanılabilirlik kümesinde yerleştirin VM kaynakları hatası birbirinden yalıtılmış sağlamaya yardımcı olur mantıksal bir gruplandırma bir özelliktir. Azure içinde bir kullanılabilirlik yerleştirin VM'ler birden fazla fiziksel sunucu, işlem rafları, depolama birimi ve ağ anahtarları arasında çalışma ayarlamanızı sağlar. Azure bazı belgelerde bu yapılandırmayı yerleştirmeleri ile farklı verilir [güncelleştirme ve hata etki alanları](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Genellikle bu yerleşimi Azure veri merkezi içinde ' dir. Güç kaynağı ve ağ sorunları, dağıttığınız datacenter etkileyeceği varsayılarak, bir Azure bölgesindeki tüm kapasite etkilenecek.

Azure kullanılabilirlik bölgeleri temsil eden veri merkezleri yerleşimini çoğu uygulama ve veri merkezleri arasında belirli bir uzaklık için kabul edilebilir farklı bölgelerde dağıtılan hizmetler arasındaki ağ gecikmesi teslim arasında bir Güvenlik Açığı ' dir. Doğal catastrophes ideal güç, ağ kaynağı ve bu bölgedeki tüm kullanılabilirlik bölgeler için altyapı etkileyen olmayacaktır. Ancak, monumental doğal catastrophes gösterildiği kullanılabilirlik bölgeleri bir bölge içinde istediğiniz kullanılabilirlik her zaman sağlamayabilir. Porto Riko Adası 20 Eylül 2017 üzerinde isabet hortum Maria düşünün. Hortum 90 mil wide Adası temel bir yaklaşık yüzde 100 Kararma neden oldu.

## <a name="single-vm-scenario"></a>Tek VM senaryosu

Bir tek VM senaryosunda, SAP HANA örneği için bir Azure VM oluşturun. İşletim sistemi diski ve tüm veri diskleri barındırmak için Azure Premium depolama kullanın. Azure çalışma süresi yüzde 99,9 SLA ve diğer Azure bileşenlerinin SLA'ları, müşterileriniz için kullanılabilirlik SLA'ları karşılamak yeterli olur. Bu senaryoda Azure kullanılabilirlik DBMS katman çalıştıran VM'ler için kümesi yararlanan gerek yoktur. Bu senaryoda, iki farklı özellikleri kullanır:

- Azure VM otomatik (Ayrıca Azure hizmet onarma olarak adlandırılır) yeniden başlatma
- SAP HANA otomatik yeniden başlatma

Azure VM otomatik yeniden başlatma veya hizmet onarma, iki düzeyde çalışır Azure içindeki bir işlevsellik olan:

- Azure sunucusu ana bilgisayar sunucu ana bilgisayarda barındırılan bir VM'nin durumunu denetler.
- Azure yapı denetleyicisi sistem durumunu ve sunucu ana kullanılabilirliğini izler.

Bir sistem durumu denetimi işlevsellik Azure sunucusu ana bilgisayarda barındırılan tüm VM durumunu izler. Bir VM nonhealthy bir duruma geçmesi durumunda, VM durumunu denetler Azure ana bilgisayar aracısı tarafından VM yeniden başlatılabilir. Yapı denetleyicisi, ana bilgisayar donanımı sorunlarını belirtebilecek birçok farklı parametreler denetleyerek konak durumunu denetler. Ayrıca, ağ yoluyla konağının erişilebilirlik denetler. Ana bilgisayar sorunlarının bir göstergesi şu olayları neden olabilir:

- Konak, varsa kötü bir durum, ana bilgisayarın yeniden başlatma ve ana bilgisayarda çalışan sanal makineleri yeniden bildirir.
- Ana bilgisayar sistem durumu iyi değilse, yeniden başlatma, ana bilgisayarın yeniden başlatma ve ilk olarak sağlıklı bir ana bilgisayar üzerindeki ana bilgisayar üzerinde barındırılan sanal makineleri yeniden sonra. Bu durumda, konak iyi olarak işaretlenir. Temizlenmiş veya değiştirilmesi kadar başka dağıtımlar için kullanılmaz.
- Sağlıksız ana bilgisayar yeniden başlatma işlemi sırasında sorunlar varsa, hemen yeniden sağlıklı bir ana bilgisayar üzerindeki VM'nin. 

Ana bilgisayar ve Azure tarafından sağlanan VM izleme ile ana bilgisayar sorunlarla Azure VM'ler sağlıklı bir Azure ana bilgisayarda otomatik olarak yeniden başlatılır. 

Bu senaryoda Bel ikinci yeniden VM çalışır VM sonra otomatik olarak başlar HANA hizmetini yeniden başlatır olgu özelliğidir. Ayarlayabilirsiniz [HANA hizmet otomatik yeniden başlatma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) çeşitli HANA hizmetlerin izleme Hizmetleri aracılığıyla.

Bir SAP HANA yapılandırmasına soğuk yük devretme düğümü ekleyerek bu tek VM senaryo iyileştirebilir. Bu kurulum SAP HANA belgelerinde adlı [konak otomatik yük devretme](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html). Bu yapılandırma burada sunucu donanımı sınırlıdır ve tek sunucu düğümü ana bilgisayar olarak ayrılması bir şirket içi dağıtım durumunda otomatik yük devretme düğümü üretim konaklarını kümesi için mantıklı olabilir. Ancak burada altyapının Azure sağlıklı hedef sunucu için başarılı bir VM yeniden başlatma sağlar, Azure'da, SAP HANA konak otomatik yük devretme dağıtmak için doesn't make Sense. Bu nedenle, HANA konak otomatik yük devretme için bekleme bir düğüm foresees hiçbir referans mimarisi sunuyoruz. Bu durum, SAP HANA genişleme yapılandırmaları için de geçerlidir.

## <a name="availability-scenarios-for-two-different-vms"></a>İki farklı sanal makineler için kullanılabilirlik senaryoları

İki Azure sanal makineleri bir Azure kullanılabilirlik kümesi içinde kullanırsanız, Azure kullanılabilirlik kümesi içinde bir Azure bölgesi yerleştirilir varsa bu iki VM'ler arasında açık kalma süresi artırabilirsiniz. Azure temel kurulumunda şöyle olabilir:

![Tüm Katmanlar ile iki VM diyagramı](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Farklı bir kullanılabilirlik senaryolarını göstermek için Diyagramdaki katmanlar bazılarını göz ardı edilir. Diyagram VM'ler, konaklar, kullanılabilirlik kümeleri ve Azure bölgeleri tarif yalnızca katmanları gösterilmektedir. Azure sanal ağ örnekleri, kaynak grupları ve abonelikler bir rolü bu bölümde açıklanan senaryolarda yürütülmez.

### <a name="replicate-backups-to-a-second-virtual-machine"></a>İkinci bir sanal makine için çoğaltma yedekleri

En ilkel kurulumları yedekleri kullanmaya biridir. Özellikle, işlem günlüğü yedeklemeleri başka bir Azure VM için bir sanal makineden sevk olabilir. Azure depolama türünü seçebilirsiniz. Bu kurulum, ikinci VM ilk VM üzerinde gerçekleştirilen zamanlanmış yedeklemeler kopyasını komut dosyası için sorumluluğu size aittir. İkinci VM örnekleri kullanmanız gerekiyorsa, tam, artımlı/fark ve işlem günlüğü yedeklemeleri ihtiyacınız noktasına geri yüklemelisiniz. 

Mimari şöyle görünür:

![Depolama çoğaltma ile iki VM diyagramı](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Bu ayar harika kurtarma noktası hedefi (RPO) ve kurtarma süresi hedefi (RTO) zamanları ulaşmak için uygun değildir. RTO kez özellikle tam kopyalanan Yedekleme kullanarak tam veritabanını geri yüklemek için gereken nedeniyle düşerdi. Ancak, bu kurulum ana örnekleri üzerindeki istenmeyen verileri silinmeye karşı kurtarmak için yararlıdır. Herhangi bir zamanda bu kurulum ile zaman içinde belirli bir noktaya geri verileri ayıklamak ve Silinen ana örneğinizi alın. Bu nedenle, bu yedek kopya yöntemi diğer yüksek kullanılabilirlik işlevselliği ile birlikte kullanmak için mantıklı olabilir. 

Yedeklemeleri kopyalanan olsa da, SAP HANA örneğini çalıştıran ana VM daha küçük bir VM kullanmanız mümkün olabilir. VHD'ler daha az sayıda küçük VM'ler iliştirebilirsiniz aklınızda bulundurun. Tek tek VM türleri sınırları hakkında daha fazla bilgi için bkz: [azure'daki Linux sanal makineler için Boyutlar](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

### <a name="sap-hana-system-replication-without-automatic-failover"></a>SAP HANA sistem çoğaltma otomatik yük devretme olmadan

Bu bölümde açıklanan senaryoları SAP HANA sistem çoğaltma kullanın. SAP belgeler için bkz: [sistem çoğaltma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html). Bazı farklılıklar RTO olduklarından iki Azure sanal makineleri tek bir Azure bölgesindeki farklı yapılandırmalara sahip. Genel olarak, otomatik yük devretme olmadan senaryoları özellikle bir Azure bölgesindeki VM'ler için geçerli olmayabilir. Azure altyapısında çoğu hataları için Azure hizmet onarma birincil VM başka bir ana bilgisayarda yeniden olmasıdır. Bu yapılandırma hatası senaryoları bakımından burada yardımcı olabilecek bazı kenar durumlar vardır. Veya bazı durumlarda, daha fazla verimlilik hayata geçirmek bir müşteri isteyebilirsiniz.

#### <a name="sap-hana-system-replication-without-auto-failover-and-without-data-preload"></a>SAP HANA sistem çoğaltma otomatik yük devretme ve veri önyüklemesi olmadan

Bu senaryoda, bir RPO 0 elde etmek için zaman uyumlu bir biçimde veri taşımak için SAP HANA sistem çoğaltma kullanın. Diğer taraftan, yük devretme veya HANA örneği önbelleğine önceden veri gerekmeyen yeterince uzun bir RTO sahip. Bu durumda, aşağıdaki eylemleri gerçekleştirerek yapılandırmanızda ekonomi daha fazla elde mümkündür:

- Başka bir SAP HANA ikinci VM'yi çalıştırır. İkinci VM SAP HANA örneğinde sanal makinenin bellek çoğunu alır. Genellikle, ikinci VM için bir yük devretme oluşması durumunda bu adıdır. Böylece ikinci VM'deki hedeflenen HANA örneğinin önbelleğine çoğaltılan veriler yüklenebilir ikinci VM kapatma.
- Daha küçük bir VM boyutu ikinci VM'de kullanın. Bir yük devretme gerçekleşirse, el ile yük devretme önce ek bir adım vardır. Bu adımda, kaynak VM boyutu VM'i yeniden boyutlandırın. Senaryo şöyle görünür:

![Depolama çoğaltma ile iki VM diyagramı](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Veri önyükleme HANA sistem çoğaltma hedefi kullanmasanız bile, en az 64 GB bellek gerekir. Yeterli bellek hedef örneği bellekte rowstore verileri tutmak için 64 GB yanı sıra da gerekir.

#### <a name="sap-hana-system-replication-without-auto-failover-and-with-data-preload"></a>SAP HANA sistem çoğaltma otomatik yük devretme olmadan ve veri önyüklemesi

Bu senaryoda, ikinci VM'deki HANA örneğine çoğaltılan verilerini önceden yüklenir. Bu iki veri önceden değil avantajları ortadan kaldırır. Bu durumda, ikinci VM üzerinde başka bir SAP HANA sistem çalıştırılamıyor. Ayrıca daha küçük bir VM boyutu kullanamazsınız. Bu nedenle, müşteriler, bu senaryo nadiren uygulayın.

### <a name="sap-hana-system-replication-with-automatic-failover"></a>SAP HANA sistem çoğaltma otomatik yük devretme ile

Standart ve en yaygın kullanılabilirlik yapılandırması bir Azure bölgesi, SLES Linux çalıştıran iki Azure VM içinde tanımlanan bir yük devretme kümesi vardır. SLES Linux kümesi dayanır [Pacemaker](http://www.linux-ha.org/wiki/Pacemaker) framework ile birlikte bir [STONITH](http://linux-ha.org/wiki/STONITH) aygıt. 

SAP HANA açısından kullanılan çoğaltma modu eşitlenen ve otomatik yük devretme yapılandırılır. İkinci bir VM'de, SAP HANA örneği etkin bekleme düğümü olarak davranır. Bekleme düğüm birincil SAP HANA örneğinden değişiklik kayıtlarını zaman uyumlu akışı alır. İşlemler HANA birincil düğümdeki uygulama tarafından kaydedilen gibi yürütme kaydı alınan ikincil SAP HANA düğüm onaylayıncaya kadar uygulamaya yürütme onaylamak için birincil HANA düğüm bekler. SAP HANA iki zaman uyumlu çoğaltma modu sunar. Ayrıntılar ve bu iki zaman uyumlu çoğaltma modu arasındaki farkların açıklaması için SAP makalesine bakın [SAP HANA sistem çoğaltma için çoğaltma modları](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html).

Genel yapılandırma şöyle görünür:

![Depolama çoğaltma ve yük devretme ile iki VM diyagramı](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

Çünkü bir RPO elde etmenizi sağlar, bu çözüm tercih edebileceğiniz = 0 ve son derece düşük RTO. SAP HANA istemci bağlantısı SAP HANA istemcileri HANA sistem çoğaltma yapılandırması bağlanmak için sanal IP adresi kullanmak için yapılandırın. Bu ikincil düğümünde bir yük devretme gerçekleşirse uygulamayı yeniden yapılandırmak gereğini ortadan kaldırır. Bu senaryoda, birincil ve ikincil VM'ler için Azure VM SKU'ları aynı olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu yapılandırmalar Azure ayarlama hakkında adım adım yönergeler için bkz:

- [SAP HANA sistem çoğaltma Azure VM'de ayarlama](sap-hana-high-availability.md)
- [Sistem çoğaltma kullanarak SAP HANA için yüksek kullanılabilirlik](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

SAP HANA kullanılabilirlik Azure bölgeler arasında hakkında daha fazla bilgi için bkz:

- [SAP HANA kullanılabilirlik Azure bölgeler arasında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

