---
title: SAP iş yükü planlama ve dağıtım denetim listesi | Microsoft Docs
description: Planlama ve azure'da SAP iş yükü dağıtımları için Denetim listesi
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
ms.date: 04/01/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 93fae0babdee5eac87d50679fdd5b2b938c4df2e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65236904"
---
# <a name="sap-workload-on-azure-planning-and-deployment-checklist"></a>SAP iş yüküne Azure planlama ve dağıtım denetim listesi 

Bu denetim listesi, SAP NetWeaver, S/4hana'yı ve Azure hizmet olarak altyapı Hybris uygulamaları taşıma müşteriler için tasarlanmıştır.  Bu denetim proje süresi boyunca bir müşteri ya da SAP iş ortağı tarafından incelenmelidir. Denetimlerin birçok proje ve planlama aşamasında başında yürütülür önemlidir. Dağıtım tamamlandığında, dağıtılan Azure altyapı veya SAP yazılım sürümleri temel değişiklikleri karmaşık hale gelebilir. Başlıca kilometre taşlarınız bir proje boyunca, bu denetim listesini gözden geçirin.  Yeniden mühendislik ve gerekli değişiklikleri test etmek için yeterli zaman var ve büyük bir sorun haline gelmeden önce küçük sorunları algılanabilir. Denetim listesi hiçbir şekilde tamamlanması talepleri. Tek tek durumunuza bağımlı olabilir gerçekleştirilmesi gereken çok daha fazla denetim. 

Birleştirilmiş denetim listesi, Azure'nın bağımsız olan görevleri içermez.  Örnek: Azure ortak Bulutuna veya bir barındırma sağlayıcısına taşıma sırasında uygulama arabirimleri değişikliği SAP.    

Bu denetim zaten dağıtılmış sistemler için de kullanılabilir. Yazma Hızlandırıcı, kullanılabilirlik ve yeni VM türleri gibi yeni özellikler dağıttığınız beri eklenmiş olabilir.  Bu nedenle, düzenli aralıklarla Azure platformundaki yeni özellikleri haberdar olmak için denetim listesini gözden geçirmek kullanışlıdır. 

## <a name="project-preparation-and-planning-phase"></a>Proje hazırlama ve planlama aşaması
Bu aşamada, bir Azure genel bulut üzerinde SAP iş yükü geçişi planlanmaktadır. En düşük varlıkları ve öğeleri kümesini ele alınan ve liste gibi tanımlanan:

1. Bu belge üst düzey tasarım belge – içermesi gerekir:
    1. Mevcut envanter SAP bileşenleri, uygulamaları ve azure'da hedef uygulama envanteri
    2. Oluşturma ve farklı taraf atamalarını bir sorumluluk atama matris (sorumluluklarını tanımlayan RACI) ile çalışma. Yüksek düzeyde başlatmak ve daha ayrıntılı düzeyleri aktarım hızı için planlama ve ilk dağıtımları çalışma
    2. Yüksek düzeyli çözüm mimarisi
    3. Uygulamasına dağıtmak için Azure bölgelerinin kararı. Azure bölgelerinin listesi için kontrol [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/). Her Azure bölgesi içinde kullanılabilir olan hizmetler için makale onay [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/)
    4. Mimari, şirket içinden Azure'a bağlanmak için ağ. Kendiniz hakkında bilgi sahibi olmak için [Azure sanal veri merkezi şeması](https://docs.microsoft.com/azure/architecture/vdc/)
    5. Yüksek iş etkisine sahip veriler Azure'da çalıştırmaya yönelik güvenlik ilkeleri'ni kullanın. Malzeme başlama okumak için [Azure güvenlik belgeleri](https://docs.microsoft.com/azure/security/)
2.  Teknik tasarım belge – içeren:
    1.  Bir çözüm Blok Diyagramı 
    2.  İşlem, depolama ve ağ bileşenleri azure'da boyutlandırma. Azure Vm'lerde SAP boyutlandırması için SAP destek Not başvurun [#1928533](https://launchpad.support.sap.com/#/notes/1928533) 
    3.  İş sürekliliği ve olağanüstü durum kurtarma mimarisi
    4.  Ayrıntılı işletim sistemi, DB, çekirdek ve SAP paketi sürümleri destekler. Bu bir biçimde Azure Vm'lerinde SAP NetWeaver veya S/4hana'yı tarafından desteklenen herhangi bir işletim sistemi sürümünü desteklenir. Aynı DBMS sürümleri için geçerlidir. Bu, aşağıdaki kaynaklardan Hizala ve gerekirse bir SAP içinde olması için SAP sürümleri, DBMS sürümleri veya işletim sistemi sürümleri yükseltme için kullanıma ve Azure penceresi desteklenen zorunludur. SAP içerisinde olduğunuz ve Azure SAP ve Microsoft tarafından tam destek almak için yayın birleşimler desteklenmez, bu zorunludur. Gerekirse, bazı yazılım bileşenleri yükseltmeyi planlamak gerekir. Desteklenen SAP, işletim sistemi ve DBMS yazılım hakkında daha fazla ayrıntı şu konumlarda belgelenmiştir:
        1.  SAP destek Not [#1928533](https://launchpad.support.sap.com/#/notes/1928533). Bu Not, Azure Vm'lerinde desteklenen en düşük işletim sistemi sürümleri tanımlar. Ayrıca, çoğu olmayan HANA veritabanı için gerekli en düşük veritabanı sürümleri tanımlar. Not, SAP boyutlandırma farklı desteklenen SAP Azure VM türleri de sunar.
        2.  SAP destek Not [#2039619](https://launchpad.support.sap.com/#/notes/2039619). Not, Azure'da Oracle destek matrisini tanımlar. Oracle yalnızca Windows ve Oracle Linux konuk işletim sistemi azure'da SAP iş yükü olarak desteklediğini unutmayın. Bu destek ifadesi, SAP örnekleri çalışan SAP uygulama katmanı için geçerlidir. Bununla birlikte, Oracle Pacemaker aracılığıyla Oracle Linux'ta SAP Central Services'in için yüksek kullanılabilirlik desteklemiyor. Oracle Linux'ta ASCS için yüksek kullanılabilirlik gerekiyorsa, SIOS koruma grubu için Linux yararlanarak gerekir. Ayrıntılı SAP sertifika verileri, SAP destek Not denetlemek [#1662610 - Linux için SIOS koruma grubu için destek ayrıntıları](https://launchpad.support.sap.com/#/notes/1662610). SAP Central Services'in DBMS katmanı olarak Oracle birlikte desteklenen Windows için SAP Windows Yük devretme küme yük devretmesi çözüm desteklenmiyor. 
        3.  SAP destek Not [#2235581](https://launchpad.support.sap.com/#/notes/2235581) farklı bir işletim sistemi üzerinde SAP HANA sürümleri için destek matrisi alınamıyor
        4.  Desteklenen Azure Vm'lerinde SAP HANA ve [HANA büyük örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) listelenen [burada](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)
        5.  [SAP ürün kullanılabilirliği Matrisi](https://support.sap.com/en/)
        6.  Diğer SAP belirli ürünlerin diğer SAP notları  
    5.  SAP üretim sistemleri için katı 3 katmanlı tasarımlar öneririz. Birleştirme ASCS + uygulama sunucularında aynı VM önerilmez.  Çoklu SID küme yapılandırmasını kullanarak SAP Central Services'in ile Windows azure'da konuk işletim sistemi olarak desteklenir. Azure'da Linux işletim sistemleri ile SAP Central Services'in çoklu SID küme yapılandırmaları desteklenmez ancak. Windows konuk işletim sistemi çalışması için belgeler bulunabilir:
        1.  [Paylaşılan disk azure'da Windows Server Yük Devretme Kümelemesi ve yüksek kullanılabilirlikle çoklu SID SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-shared-disk)
        2.  [Azure'da Windows Server Yük Devretme Kümelemesi ve dosya paylaşımı ile yüksek kullanılabilirlik çoklu SID SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-file-share)
    6.  Yüksek kullanılabilirlik ve olağanüstü durum kurtarma mimarisi
        1.  Yüksek kullanılabilirlik ve olağanüstü durum kurtarma mimarisi görünmesi için gerekenler RTO ve RPO göre tanımlama
        2.  Aynı bölge içinde yüksek kullanılabilirlik için istenen DBMS Azure'da sunduğu sahip denetleyin. Birçok DBMS, üretim sistemleri için öneririz zaman uyumlu bir etkin bekleme zaman uyumlu yöntemleri sunar. Ayrıca SAP onay belgeleri başlayarak farklı veritabanları için ilgili [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) ve ilgili belgeler
            1.  Windows yük Devreme Küme Hizmeti'ni paylaşılan disk ile DBMS katman olarak, örneğin, SQL Server için açıklanan yapılandırmayla [burada](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server?view=sql-server-2017) olduğu **değil** desteklenir. Bunun yerine gibi çözümleri:
                1.  [SQL Server AlwaysOn](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups) 
                2.  [Oracle Data Guard](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-oracle-dataguard)
                3.  [HANA sistem çoğaltması](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
        3.  Farklı Azure bölgeleri arasında olağanüstü durum kurtarma için hangi olanaklar farklı DBMS satıcılar tarafından sunulan denetleyin. Bunların çoğu zaman uyumsuz çoğaltma veya günlük aktarma destekler
        4.  SAP uygulama katmanı için iş regresyon, ideal olarak üretim dağıtımlarınıza çoğaltmaları olan test sistemleri aynı Azure bölgesindeki veya DR bölgenizi çalıştırırsınız olup olmadığını tanımlar. İkinci durumda, iş regresyon Sistem Kurtarma hedefi olarak üretim için hedefleyebilir
        5.  Üretim dışı sistemlere DR sitede yerleştirmemeye karar verirseniz, Azure Site Recovery ile SAP uygulama katmanı Azure DR bölgeye çoğaltmak uygulanabilir yöntemi olarak arayın. Ayrıca bkz: [çok katmanlı SAP NetWeaver uygulama dağıtımı için olağanüstü durum kurtarmayı ayarlama](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap) 
        6.  Birleştirilmiş bir HA/DR yapılandırması kullanmaya karar verirseniz, yararlanarak [Azure kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview) kendiniz Azure bölgeleri hakkında bilgi sahibi kullanılabilirlik alanları kullanılabilir olduğunda ve tarafından tanıtılan kısıtlamaları ile yapın iki kullanılabilirlik alanları arasında daha yüksek ağ gecikme süresi  
3.  Müşteri/iş ortağı tüm SAP arabirimleri (SAP ve SAP olmayan) bir envanterini oluşturmanız gerekir. 
4.  Tasarım Foundation Services tasarım - bu tasarım gibi öğeleri içerir.
    1.  Active Directory ve DNS tasarlama
    2.  Farklı SAP sistemlerini atama ile Azure içindeki ağ topolojisi
    3.  [Rol tabanlı erişim](https://docs.microsoft.com/azure/role-based-access-control/overview) yapısı farklı Takımlarınız altyapısını ve azure'da SAP uygulamaları yönetmek için
    3.  Kaynak grubu topolojisi 
    4.  [Etiketleme stratejisi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#tags-and-billing)
    5.  VM'ler için kuralı ve diğer altyapı bileşenlerini ve/veya mantıksal adları adlandırma
5.  Microsoft Premier Destek sözleşmesi – MS teknik hesap yöneticinizle (TAM) belirleyin. Destek için SAP tarafından gereksinimleri SAP destek not okuma [#2015553](https://launchpad.support.sap.com/#/notes/2015553) 
6.  Azure Abonelikleriniz ve farklı abonelikler için çekirdek kota sayısını tanımlayın. [Azure abonelikleri kotaları artırmak için destek isteği açın](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) gerektiği şekilde 
7.  Veri azaltma ve veri geçişi için SAP veri geçişi, Azure'a planlayın. SAP NetWeaver sistemler için SAP çok sayıda sınırlı veri hacmini nasıl şirket yönergeleri sahiptir. Yayımlanan SAP [bu çok büyük bir kılavuz](https://help.sap.com/http.svc/rc/2eb2fba8f8b1421c9a37a8d7233da545/7.0/en-US/Data_Management_Guide_Version_70E.PDF) SAP ERP sistemleriyle ortamında veri yönetimi hakkında. Ancak, bazı içerikleri genel NetWeaver ve S/4hana'yı sistemleri için geçerlidir.
8.  Tanımlamak ve otomatik dağıtım yaklaşımı karar verin. Otomasyon altyapısı dağıtımlarını azure'da arkasında belirlenimci bir şekilde dağıtın ve belirleyici sonuçlar almak için hedefidir. Birçok müşteri, PowerShell veya temel CLI betiklerini kullanır. Ancak, SAP için Azure altyapısını dağıtma ve hatta SAP yazılımı yüklemek için kullanılan açık kaynaklı teknolojiler vardır. Örnekler Github'da bulunabilir:
    1.  [Azure bulutunda otomatik SAP dağıtımları](https://github.com/Azure/sap-hana)
    2.  [SAP HANA yüklemesi](https://github.com/AzureCAT-GSI/SAP-HANA-ARM)
9.  Normal Tasarım ve dağıtım gözden geçirme temposu, müşteri olarak arasında tanımlama sistem entegratörü, Microsoft ve diğer taraflar dahil

 
## <a name="pilot-phase-strongly-recommended"></a>Pilot aşaması (önerilir)
 
Pilot proje planlama ve hazırlama için paralel ya da önce çalıştırabilirsiniz. Aşama yaklaşımlar ve tasarım planlama ve hazırlık aşamasında yapılan test etmek için de kullanılabilir. Pilot aşaması, gerçek bir kavram kanıtı için uzatılmış. Ayarlama ve tam bir HA/DR çözümü doğrulamak için önerilen bir pilot dağıtım sırasında güvenlik tasarımı yanı sıra. Bazı müşteri durumlarda ölçeklenebilirlik testleri bu aşamada yürütülebilecek. Diğer müşteriler SAP sanal sistemlerinin dağıtımını pilot aşaması kullanın. Bir pilot çalıştırmak amacıyla Azure'a geçirmek istediğiniz bir sistem tanımlanan varsayıyoruz şekilde.

1. Azure'a veri aktarımı iyileştirin. Yüksek oranda bağımlı müşteri durumları aktarımı [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) şirket içinden Express bağlantı hattı bant genişliği yeterli olsaydı hızlı oldu. Diğer müşteriler ile İnternet'e giderek daha hızlı olmasını anladığınızda
2. Bir SAP durumunda bir dışarı aktarma ve içeri aktarma ilişkin Veritabanı verilerinin içerir, heterojen platform geçişi, test edin ve dışarı aktarma en iyi duruma getirmek ve aşamaları alın. Hedef platformu olarak SQL Server içeren büyük geçişler için öneriler bulunabilir [burada](https://techcommunity.microsoft.com/t5/Running-SAP-Applications-on-the/SAP-OS-DB-Migration-to-SQL-Server-8211-FAQ-v6-2-April-2017/ba-p/368070). Birleştirilmiş sürüm yükseltme gerekmez durumunda geçiş İzleyici/SWPM yaklaşım alabilir veya [SAP DMO](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) işlem geçiş SAP sürüm yükseltmesi ile birleştirerek ve belirli bir kaynak ve hedef DBMS platformu Karşılama birleşimleri, örneğin, belgelenen olarak [toplam 2.0 SP03, veritabanı geçiş seçeneği (DMO)](https://launchpad.support.sap.com/#/notes/2631152). 
   1.  Azure ve alma performansını dışarı aktarma dosyası yükleme kaynağı olarak dışarı aktarın.  Dışarı ve içeri aktarma arasında çakışma en üst düzeye çıkarın
   2.  Veritabanı, altyapı boyutlandırma yansıtmak için hedef ve hedef platform arasında sesini değerlendirecek    
   3.  Doğrulama ve zamanlama en iyi duruma getirme 
3. Teknik doğrulama 
   1. Sanal Makine Türleri
      1.  SAP destek notları, SAP HANA donanım dizin ve SAP yeniden desteklenen VM'ler için Azure, hiçbir değişiklik desteklenen işletim sistemi sürümleri için bu VM türleri ve sürümleri, SAP ve DBMS desteklenen olan emin olmak için PAM Kaynakları Doğrula
      2.  Yeniden boyutlandırma, uygulamanızın, Azure'da dağıttığınız altyapıyı ve doğrulayın. Mevcut uygulamaları taşıma durumunda, genellikle gerekli SAP kullandığınız altyapıdan türetebilirsiniz ve [SAP Kıyaslama Web sayfası](https://www.sap.com/dmc/exp/2018-benchmark-directory/#/sd) ve SAP desteği Not listelenen SAP sayıları karşılaştırın [#1928533](https://launchpad.support.sap.com/#/notes/1928533). Ayrıca [bu makalede](https://techcommunity.microsoft.com/t5/Running-SAP-Applications-on-the/SAPS-ratings-on-Azure-VMs-8211-where-to-look-and-where-you-can/ba-p/368208) unutmayın
      3.  Değerlendirmek ve en fazla depolama aktarım hızı ve ağ aktarım hızı planlama aşamasında, seçtiğiniz farklı VM türleri ile ilgili Azure Vm'leriniz boyutlandırması test edin. Veri bulunabilir:
          1.  [Azure'da Windows sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). Dikkate almak önemlidir **maksimum önbelleğe alınmamış disk aktarım hızı** boyutlandırma için
          2.  [Azure'da Linux sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) dikkate almak önemlidir **maksimum önbelleğe alınmamış disk aktarım hızı** boyutlandırma için
   2. Depolama
      1.  Kullanım [Azure standart SSD depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd) performans olmayan hassas DBMS dağıtım ve SAP uygulama katmanları temsil eden VM'ler için minimum
      2.  Kullanmamaya öneriyoruz [Azure standart HDD diskleri](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd) genel
      2.  Kullanım [Azure Premium depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) uzaktan performans duyarlı olan tüm DBMS VM'ler için
      2.  Kullanım [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/)
      3.  Azure yazma Hızlandırıcı, M serisi ile DBMS günlük sürücüleri için kullanın. Yazma Hızlandırıcı sınırları ve açıklandığı gibi kullanım bilmeniz [yazma Hızlandırıcısı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator)
      4.  Farklı DBMS türleri için denetleme [genel SAP DBMS belgeleri ilgili](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) ve DBMS özel belgeler için genel belge işaret
      5.  SAP HANA için Ayrıntılar bölümünde belgelendirilen [SAP HANA altyapısı yapılandırmaları ve işlemleri Azure üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
      6.  Hiçbir zaman bağlama Azure veri diskleri cihaz kimliğini kullanarak bir Azure Linux VM Bunun yerine, evrensel benzersiz tanımlayıcı (UUID) kullanın. Bağlama Azure veri diskleri için grafik araçları örneğin kullanırken dikkatli olun. Diskleri UUID kullanarak bağlandığından emin emin olmak için/etc/fstab girişleri denetleyin
          1.  Daha fazla ayrıntı bulunabilir [burada](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#connect-to-the-linux-vm-to-mount-the-new-disk)
   3. Ağ
      1.  Test, sanal ağ altyapınızı ve SAP uygulamalarınızı farklı Azure sanal ağları içinde veya arasında dağıtılması değerlendirin
          1.  Hub ve uç sanal ağ mimarisini veya dayalı tek bir Azure sanal ağ içinde microsegmentation yaklaşımı değerlendirin
              1.  Veri değişimi arasında nedeniyle maliyetleri [Azure sanal ağlar eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). Maliyetleri denetleme [sanal ağ fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network/)
              2.  Hızlı avantajlarından kesin değişiklik Ayır uygulamalar veya sanal makineleri bir sanal ağ alt ağında barındırılan bir güvenlik riski burada dönüştü durumlar için bir sanal ağdaki bir alt ağ için NSG ile Azure sanal ağları arasında eşleme
              3.  Merkezi günlük kaydı ve Azure'da yerleşik sanal veri merkezi şirket içi ve dış dünya arasındaki ağ trafiği denetleme
          2.  Değerlendirmek ve test SAP uygulama katmanında ve SAP DBMS'yi katman arasında veri yolu. 
              1.  Tüm yerleşimini [Azure ağ sanal Gereçleri](https://azure.microsoft.com/solutions/network-appliances/) SAP uygulama ve bir SAP NetWeaver, Hybris veya S/4hana'yı DBMS katman arasındaki iletişim yolunun içinde temel SAP sistemlerini desteklenmiyor hiç
              2.  SAP uygulama katmanında ve SAP DBMS'yi değil eşlenen farklı Azure sanal ağları yerleştirme desteklenmiyor.
              3.  [Azure ASG ve NSG kuralları](https://docs.microsoft.com/azure/virtual-network/security-overview) SAP uygulama katmanında ve SAP DBMS'yi katman arasında rotaları tanımlamak için desteklenir
          3.  Emin olun [Azure hızlandırılmış ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) SAP uygulama katmanı ve SAP DBMS'yi katmanı VM'ler üzerinde etkindir. Farklı işletim sistemi düzeyleri hızlandırılmış ağ ile Azure desteklemek için gerekli olup olmadığını göz önünde bulundurun:
              1.  Windows Server 2012 R2 veya daha yeni sürümleri
              2.  SUSE Linux 12 SP3 veya daha yeni sürümleri
              3.  RHEL 7.4 ya da daha yeni sürümleri
              4.  Oracle Linux 7.5. RHCKL çekirdek kullanarak, yayın 3.10.0-862.13.1.el7 olması gerekir. Oracle UEK kullanarak çekirdek sürüm 5 gereklidir
          4.   Test ve SAP desteği Not göre SAP uygulama katmanında VM DBMS VM arasındaki ağ gecikme süresi değerlendirme [#500235](https://launchpad.support.sap.com/#/notes/500235) ve SAP desteği Not [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). SAP destek Not ağ gecikme süresi Kılavuzu karşı sonuçlarını değerlendirmek [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Ağ gecikme süresi, Orta ve iyi aralığında olmalıdır. Özel durumlar geçerlidir HANA büyük örneği ile VM'ler arasındaki trafiği belgelendiği gibi birimleri [burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)
          5.   ILB dağıtımları doğrudan sürücü dönüşü kullanmaya ayarlandığından emin olun. Bu ayar Azure Ilb DBMS katmanında yüksek kullanılabilirlik yapılandırmaları için kullanıldığı durumlarda gecikme süresini azaltır.
          6.   Azure Load Balancer, Linux konuk işletim sistemleri onay Linux parametresi ağ ile birlikte kullanıyorsanız **net.ipv4.tcp_timestamps** ayarlanır **0**. Önerileri SAP'ın eski sürümlerinde karşı Not [#2382421](https://launchpad.support.sap.com/#/notes/2382421). SAP note, bu arada parametresi Azure Load balancer'ları ile birlikte çalışmak için 0 olarak ayarlanması gerekir olgu yansıtacak şekilde güncelleştirilir.
   4. Yüksek kullanılabilirlik ve olağanüstü durum kurtarma dağıtımları. 
      1. SAP uygulama katmanı belirli bir Azure kullanılabilirlik alanı tanımlamadan dağıtırsanız, SAP iletişim örneği veya bir ara yazılım örnekleri tek bir SAP sistemiyle çalışan tüm sanal makineler dağıtılır emin olun. bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). 
         1.   DBMS ve SAP Central Services'in için yüksek kullanılabilirlik gerektirmeyen durumunda bu VM'lerin aynı kullanılabilirlik kümesi SAP uygulama katmanı olarak dağıtılabilir
      2. SAP Central Services'in ve etkin olmayan çoğaltmalar yüksek kullanılabilirlik için DBMS katman koruyorsanız, SAP Central Services'in bir ayrı kullanılabilirlik kümesi'ndeki ve başka bir kullanılabilirlik kümesinde iki DBMS düğümü için iki düğüme sahip
      3. Azure kullanılabilirlik alanına dağıtırsanız, kullanılabilirlik kümeleri yararlanamaz. Ancak bölgelere iki farklı kullanılabilirlik bölgeleri arasında en küçük gecikme süresini göster, etkin ve Pasif Central Services'in düğümler dağıtma emin olmanız gerekir.
         1.   Kullanmanız gereken akılda tutulması [Azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) Windows veya Pacemaker yük devretme kümeleri DBMS ve SAP Central Services'in katman için kullanılabilirlik alanları genelinde oluşturma durumu. [Temel yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) bölgesel dağıtımları için kullanılamaz 
   5. Zaman aşımı ayarları
      1. SAP NetWeaver Geliştirici izlemeler farklı SAP örneklerinin denetleyin ve kuyruğa sunucusu ve SAP iş işlemleri arasında hiçbir bağlantı sonları belirtilmiştir emin olun. Bu iki kayıt defteri parametreleri ayarlayarak bu bağlantı sonlarını önlenebilir:
         1.   HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveTime 120000 - = Ayrıca bkz: [bu makalede](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957549(v=technet.10))
         2.   HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveInterval 120000 - = Ayrıca bkz: [bu makalede](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957548(v=technet.10)) 
      2. Bir şirket içi arasında GUI zaman aşımları önlemek için SAP GUI arabirimleri ve Azure'da dağıtılmış SAP uygulama katmanları dağıtılırsa, aşağıdaki parametreleri default.pfl veya örneği profilinde ayarlanmış olup olmadığını denetleyin:
         1.   rdisp/keepalive_timeout 3600 =
         2.   rdisp/keepalive = 20
      3. Windows Yük devretme kümesi yapılandırma kullanırsanız, duyuracağı düğümlerinde yanıt vermek zaman Azure için doğru ayarlandığından emin olun. Microsoft makalesi [ayarlama yük devretme kümesi ağ eşikleri](https://techcommunity.microsoft.com/t5/Failover-Clustering/Tuning-Failover-Cluster-Network-Thresholds/ba-p/371834) parametreleri ve bu yük devretme sensitivities nasıl etkilediği listeler. Küme düğümleri aynı alt ağda olduğunu varsayarsak, aşağıdaki parametreleri değiştirilmelidir:
         1.   SameSubNetDelay = 2000
         2.   SameSubNetThreshold = 15
         3.   RoutingHistorylength = 30
4. Test, yüksek kullanılabilirlik ve olağanüstü durum kurtarma yordamları
   1. Yük devretme durumları benzetimini yapmak Vm'lerini (Windows konuk işletim sistemi) kapatılıyor veya işletim sistemleri, yük devretme yapılandırmaları tasarlandığı gibi çalışması anlamak için Panik modunda (Linux konuk işletim sistemi) koyarak. 
   2. Bir yük devretme çalıştırmak için gereken zamanlarınızı ölçün. Bir kez uzun zaman alıyorsa, göz önünde bulundurun:
      1.   SUSE Linux için SBD cihazları devretmeyi hızlandırmak için yerine Azure Çitlemek aracıyı kullanın
      2.   Verileri yeniden çok uzun sürerse daha fazla depolama bant genişliği sağlamak için SAP HANA için düşünün
   3. Yedekleme/geri yükleme sırası ve zamanlama test edin ve gerekirse ayarlamaya. Yalnızca yedekleme sürelerine yeterli olduğundan emin olun. Ayrıca geri yükleme test ve geri yükleme etkinliklerini zamanlama gerçekleştirin. geri yükleme sürelerini RTO burada bir veritabanı üzerinde RTO'nuz kullanır veya VM geri yükleme işlemi SLA'lar içinde olduğundan emin olun
   4. Bölge DR işlevsellik ve mimari üzerinde test
5. Güvenlik denetimleri
   1.  Azure rol geçerliliğini test, uygulanan Erişim (RBAC) mimarisi tabanlı. Ayırın ve erişim ve farklı takımlar izinleri sınırlamak için hedeftir. Örnek olarak, SAP temel takım üyeleri Vm'leri dağıtma ve diskleri Azure Depolama'dan belirli bir Azure sanal ağa atamak mümkün olması gerekir. Ancak SAP temel takım kendi sanal ağlar oluşturabilir veya var olan sanal ağların ayarlarını değiştirme olmamalıdır. Diğer tarafta ağ ekibi üyelerinin SAP uygulama ve DBMS VM'lerin çalıştığı sanal ağlara Vm'leri dağıtma olanağına olmamalıdır. Ya da ağ ekibi üyelerinin Vm'leri özniteliklerini değiştirebilir veya Vm'leri veya diskleri hatta silebilir olmalıdır.  
   2.  Doğrulama [NSG ve ASC](https://docs.microsoft.com/azure/virtual-network/security-overview) kuralları beklendiği gibi çalıştığını ve korumalı kaynaklara kalkanı
   3.  Şifrelenmesini gerektiren tüm kaynakları şifrelendiğinden emin olun. Tanımlayabilir ve sertifikalar, yedekleme depolamak ve bu sertifikalara erişen ve şifrelenmiş varlıkları geri yüklemek için işlemler yürütebilirsiniz. 
   4.  Kullanım [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq) ve/veya işletim sistemi diskleri burada açısından olası bir işletim sisteminden desteklemek için
   5.  Çok fazla katmanları şifreleme kullanılmış denetleyin. Azure Disk şifrelemesi kullanın ve ardından şirket DBMS saydam veritabanı şifrelemesi yöntemlerinden biri olan üst sınırlı anlamlı yapar
6. Performans testi
   1.  SAP izleme ve ölçümlere dayalı SAP en çok 10 çevrimiçi geçerli uygulama için uygunsa výkonu 
   2.  SAP izleme ve ölçümlere dayalı SAP içinde ilk 10 toplu işleri geçerli uygulama için uygunsa karşılaştırın 
   3.  SAP izleme ve ölçümlere dayalı SAP SAP sistemine arabirimleri aracılığıyla veri aktarımları karşılaştırın. Burada, aktarımı artık şirket içinden Azure'a giden gibi farklı konumları arasında bilmesi arabirimleri odaklanın 


## <a name="non-production-phase"></a>Üretim dışı aşaması 
Bu aşamada başarılı bir pilot veya PoC sonra üretim dışı SAP sistemlerini Azure'a dağıtmak başlatıyorsanız varsayılır. Tüm dersleri ve deneyimler kavram kanıtı dışında bu tür bir dağıtım için uyarlanmış olmalıdır. Bu tür dağıtımlar da içinde tüm ölçütleri ve Poc'de listelenen adımları uygulayın. Bu aşamada, geliştirme sistemleri genellikle dağıtmak, sistemleri birim testleri ve iş regresyon sistemlerini Azure'a sınar. Bu en az bir üretim dışı sistemi bir SAP uygulama satırı gelecekteki üretim sistemini yükleneceği tam yüksek kullanılabilirlik yapılandırması aynıdır önerilir. Bu aşamada dikkate almanız gereken ek adımlar şunlardır:  

1.  Taşımadan önce eski Azure platformundaki sistemlerden CPU kullanımı, depolama aktarım hızı ve IOPS verilerini gibi kaynak tüketim verilerini toplayın. Özellikle DBMS katman birimi, uygulama katmanı birimlerinden de. Ayrıca ağ ve depolama gecikme süresini ölçün.
2.  Bir sistemi kullanılabilirlik kullanım süresi desenlerini kaydedin. Hedeftir üretim dışı sistemlere 7/24 kullanılabilir olması gerekip gerekmediğini kullanıma tahmin etmek veya belirli bir haftalık veya aylık aşamalarında kapatılabilir üretim dışı sistemlere olup olmadığı
3.  Test edin ve Azure VM'ler için kendi işletim sistemi görüntüleri oluşturmak isteyip istemediğinizi veya bir görüntüyü Azure görüntü Galerisi dışında kullanmak isteyip istemediğinizi tanımlayın. Azure galeri dışı bir görüntüsü kullanıyorsanız, işletim sistemi satıcınıza destek sözleşmesi yansıtır görüntünün doğru dikkate aldığınızdan emin olun. Bazı işletim sistemi satıcıları için kendi lisans görüntülerini getirme Azure galerileri sunar. Başkalarının işletim sistemi görüntüleri, destek, Azure tarafından alıntılanmış fiyat eklenir. Kendi işletim sistemi görüntüleri oluşturmaya karar verirseniz, aşağıdaki makalelerde belgeleri bulabilirsiniz:
    1.  Temel azure'da dağıtılan Windows VM genelleştirilmiş görüntü oluşturabilirsiniz [bu belgeleri](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)
    2.  Bir Linux göre azure'da dağıtılan VM genelleştirilmiş görüntü oluşturabilirsiniz [bu belgeleri](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image)
3.  Azure sanal makine galerisinden SUSE ve Red Hat Linux görüntüleri kullanıyorsanız, görüntüleri için Azure VM galerisinde Linux satıcıları tarafından sağlanan SAP kullanmanız gerekir
4.  SAP, Microsoft destek sözleşmeleri ile ilgili olan destek gereksinimlerini karşılayan emin olun. SAP destek Not bilgi bulunabilir [#2015553](https://launchpad.support.sap.com/#/notes/2015553). HANA büyük örnekleri belgesine bakın [ekleme gereksinimleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-onboarding-requirements)
4.  Doğru kişilerin sahip olduğumuzdan emin olun [planlı bakım bildirimlerini](https://azure.microsoft.com/blog/a-new-planned-maintenance-experience-for-your-virtual-machines/), bu nedenle, zaman içinde kapalı kalma süresi ve Vm'leri yeniden başlatmayı seçebilirsiniz
5.  Kararlı bir şekilde Microsoft sunu kanallarda gibi Azure belgelerine bakın [Channel9](https://channel9.msdn.com/) dağıtımlarınız için geçerli olabilecek yeni işlevselliği için
6.  Azure için onay SAP notları ilgili ister destek Not [#1928533](https://launchpad.support.sap.com/#/notes/1928533) yeni VM SKU'ları veya yeni desteklenen işletim sistemi ve DBMS sürümü. Yeni VM daha eski bir VM'ye karşı türleri fiyatlandırması, bu nedenle, en iyi fiyat/performans oranı ile Vm'leri dağıtma olanağına Karşılaştır
7.  SAP destek notları, SAP HANA donanım dizin ve SAP PAM yeniden desteklenen VM'ler için Azure içindeki herhangi bir değişiklik vardı, desteklenen işletim sistemi sürümleri serbest bırakır, bu Vm'leri ve desteklenen SAP ve DBMS emin olmak için kaynakları doğrula
8.  Denetleme [burada](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) yeni HANA sertifikalı ve Azure ile planlanması ve sonuçta daha iyi fiyat performansı bildirime birimleri almak için değiştirin olanlara ile karşılaştırma fiyatlandırma SKU'ları için 
9.  Dağıtım betiklerinizi yeni VM türleri yararlanın ve yeni özellikleri kullanmak istediğiniz Azure birleşimi için uyum
10. Altyapı Dağıtımı sonra test ve SAP uygulama katmanında VM ile DBMS VM arasındaki ağ gecikmesini SAP destek Not göre değerlendirme [#500235](https://launchpad.support.sap.com/#/notes/500235) ve SAP desteği Not [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). SAP destek Not ağ gecikme süresi Kılavuzu karşı sonuçlarını değerlendirmek [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Ağ gecikme süresi, Orta ve iyi aralığında olmalıdır. Özel durumlar geçerlidir HANA büyük örneği ile VM'ler arasındaki trafiği belgelendiği gibi birimleri [burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance). Kısıtlamaları hiçbiri içinde bahsedilen emin [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#azure-network-considerations) ve [SAP HANA altyapısı yapılandırmaları ve işlemleri Azure üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations) Dağıtımınız için geçerlidir
11. İş yükü uygulamadan önce kavram aşamasında listelendiği gibi tüm denetimleri gerçekleştirme
12. İş yükü geçerli olduğundan, bu sistemlerin kaynak tüketimini Azure'da kaydetmek ve, eski bir platformdan diğerine aldığınız kayıtlarla karşılaştırın. Daha büyük bir fark olduğunu görürseniz, gelecekteki dağıtımlar VM boyutlandırması ayarlayın. De azalır, downsizing, depolama ve ağ bant genişlikleri VM olması durumunda göz önünde bulundurun:
    1.  [Azure'da Windows sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). 
    2.  [Azure'daki Linux sanal makinesi boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) 
13. Sistem kopyalama işlevselliği ve işlemler üzerinde çalışır. Proje ekipleri, geliştirme ortamına veya bu nedenle, bir test sistemini kopyalamak kolaylaştırmak için yeni sistemleri hızlı alabilirsiniz hedeftir. Göz önünde bulundurun [SAP LaMa](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+Landscape+Management+%28SAP+LaMa%29+at+a+Glance) gibi görevleri gerçekleştiren bir araç olarak.
14. En iyi duruma getirmek ve takımınızın Azure rol tabanlı erişim, izinleri ve işlemleri bir tarafındaki bir görev ayrımı sahip olduğunuzdan emin olmak için performansından. Diğer tarafta, Azure altyapısı görevlerini gerçekleştirmek için etkinleştirilmiş tüm takımlar istediğiniz.
15. Alıştırma, test ve gibi görevleri yürütmek personelinizin etkinleştirmek için belgeyi yüksek kullanılabilirlik ve olağanüstü durum kurtarma yordamları. Eksiklikleri belirlemek ve uyum dağıtımlarınızı tümleştiriyoruz yeni Azure işlevi

 
## <a name="production-preparation-phase"></a>Üretim Hazırlık aşaması 
Bu aşamada deneyimleri ve üretim dışı dağıtımlarınıza dersleri toplayıp bunları gelecekte üretim dağıtımları uygulamak istiyor. Ayrıca önce aşamalarına, ayrıca iş, geçerli barındırma konumunuz ile Azure arasında veri aktarımı hazırlamanız gerekir. 

1.  Gerekli SAP sürüm yükseltme, üretim sistemleri Azure'a taşımadan önce çalışır
2.  Üretim sistemindeki geçişten sonra gerçekleştirilmesi gereken iş testleri ve işletme sahipleri işlev üzerinde kabul ediyorum
    1.  Bu testleri barındırma geçerli konumunda kaynak sistemleriyle yürütüldüğünden emin olun. Sistem Azure'a taşındıktan sonra ilk kez gerçekleştirilen testler istemiyorsunuz
2.  Azure'a geçiş işlemi üretim test edin. Tüm üretim sistemlerine aynı zaman çerçevesinde azure'a değil durumunda, barındırma aynı konumda olması gereken üretim sistemlerine grupları oluşturun. Alıştırma ve test veri geçişi. Gibi ortak yöntemleri listesi:
    1.  Temel ve azure'da veritabanı içerik eşitlemek için SQL Server AlwaysOn, HANA sistem çoğaltması veya günlük aktarma birlikte yedekleme/geri yükleme gibi DBMS yöntemleri kullanma
    2.  Daha küçük veritabanları için yedekleme/geri yükleme kullanın
    3.  Kullanım SAP geçiş SAP SWPM aracına uygulanan heterojen geçişler gerçekleştirmeyi İzleyicisi
    4.  Kullanım [SAP DMO](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) SAP sürüm yükseltmesi ile birleştirerek gerekirse işlemi. Kaynak ve hedef DBMS tüm bileşimleri desteklenir göz önünde bulundurun. Belirli SAP destek notları DMO farklı sürümleri için daha fazla bilgi bulunabilir. Örneğin, [toplam 2.0 SP04, veritabanı geçiş seçeneği (DMO)](https://launchpad.support.sap.com/#/notes/2644872)
    5.  Veri aktarımı internet üzerinden veya ExpressRoute aracılığıyla yedeklemeler geçmeniz veya SAP dosyaları dışarı aktarma durumunda aktarım hızı daha iyi olup olmadığını test edin. İnternet üzerinden veri taşımayı çalışması için bazı gelecekteki üretim sistemleri için yerinde olması gerekir, NSG/ASG güvenlik kuralları değiştirmeniz gerekebilir
3.  Taşımadan önce eski Azure platformundaki sistemlerden CPU kullanımı, depolama aktarım hızı ve IOPS verilerini gibi kaynak tüketim verilerini toplayın. Özellikle DBMS katman birimi, uygulama katmanı birimlerinden de. Ayrıca ağ ve depolama gecikme süresini ölçün.
4.  SAP destek notları, SAP HANA donanım dizin ve SAP PAM yeniden desteklenen VM'ler için Azure içindeki herhangi bir değişiklik vardı, desteklenen işletim sistemi sürümleri serbest bırakır, bu Vm'leri ve desteklenen SAP ve DBMS emin olmak için kaynakları doğrula 
4.  Dağıtım betikleri VM türleri ve Azure işlevleri üzerinde karar en son değişikliklerle uyum
5.  Altyapı ve uygulama dağıtımını olduktan sonra farklı bir dizi denetimlerinde doğrulamak için sipariş edebilirsiniz:
    1.  Doğru VM türleri doğru öznitelikleri ve depolama alanı boyutları ile dağıtılan
    2.  VM'lerin doğru ve istenen işletim sistemi sürümleri ve düzeltme eklerini gerekir ve Tekdüzen denetleyin
    3.  VM'lerin sağlamlaştırma olarak gerekli ve Tekdüzen denetleyin
    4.  Doğru uygulama sürümleri ve düzeltme eklerinin yüklü dağıtılan ve denetleyin
    5.  Vm'leri Azure kullanılabilirlik kümeleri halinde planlı olarak dağıtılan
    6.  Azure Premium depolama, gecikme süresi hassas diskler için kullanıldı veya burada [tek bir sanal makine SLA'sı % 99,9 düzeyinde](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) gereklidir
    7.  Azure yazma Hızlandırıcı doğru dağıtım denetle
        1.  Depolama alanları, VM içinde olduğundan emin olun veya eşlikli doğru Azure yazma Hızlandırıcı desteğe ihtiyaç duyan diskler üzerinde oluşturulmuş
            1.  Denetleme [Linux'ta yazılım RAID yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
            2.  Denetleme [azure'da bir Linux VM'de LVM'yi yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)
    8.  [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) özel olarak kullanıldı
    9.  VM'lerin doğru kullanılabilirlik kümeleri ve kullanılabilirlik bölgeleri dağıtılan
    10. Emin olun [Azure hızlandırılmış ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) vm'lerinde SAP uygulama katmanı ve SAP DBMS'yi katman kullanılan etkin
    11. Hiçbir yerleşimini [Azure ağ sanal Gereçleri](https://azure.microsoft.com/solutions/network-appliances/) SAP uygulama ile SAP NetWeaver DBMS katmanı arasındaki iletişim yolunun, SAP sistemlerini Hybris veya S/4hana'yı göre
    12. ASG ve NSG kuralları istenen ve planlı olarak iletişimine izin verme veya bunları engelleme iletişim gerektiğinde
    13. Zaman aşımı ayarlarını daha önce açıklandığı gibi doğru şekilde ayarlandı
    14. Test ve SAP desteği Not göre SAP uygulama katmanında VM DBMS VM arasındaki ağ gecikme süresi değerlendirme [#500235](https://launchpad.support.sap.com/#/notes/500235) ve SAP desteği Not [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). SAP destek Not ağ gecikme süresi Kılavuzu karşı sonuçlarını değerlendirmek [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Ağ gecikme süresi, Orta ve iyi aralığında olmalıdır. Özel durumlar geçerlidir HANA büyük örneği ile VM'ler arasındaki trafiği belgelendiği gibi birimleri [burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)
    15. Şifreleme gerektiğinde ve gerekli bir şifreleme yöntemi ile dağıtılan denetleyin
    16. Arabirimleri ve diğer uygulamaları yeni dağıtılan altyapı bağlanıp bağlanamadığınızı denetleyin
6.  Azure planlı bakımı için tepki için bir playbook oluşturmak. Planlı bakım durumunda başlatılması sistemleri ve VM'lerin düzeni tanımlama
    

## <a name="go-live-phase"></a>Canlı aşaması gidin
Go-Live aşaması için playbook'ları önceki aşamada geliştirilen izlemek emin olmanız gerekir. Test ve eğitim adımları yürütün. Yapılandırmaları ve işlem son dakika değişiklikleri kabul etme. Yanı sıra, aşağıdaki ölçütler geçerlidir:

1. Azure portal izleme ve diğer izleme araçları çalıştığını doğrulayın.  Perfmon (Windows) veya ÖİB (Linux) önerilen araçlar şunlardır: 
    1.  CPU sayaçları 
        1.  Ortalama CPU süresi – toplam (tüm CPU)
        2.  Ortalama CPU süresine – tek tek her işlemci (m128 VM'de şekilde 128 işlemciler)
        3.  CPU saati çekirdek – tek tek her işlemci
        4.  CPU zamanı kullanıcı – tek tek her işlemci
    5.  Bellek 
        1.  Boş bellek
        2.  Bellek Sayfa/sn
        3.  Bellek sayfası kullanıma/sn
    4.  Disk 
        1.  Disk KB/sn – tek tek disk başına okuma 
        2.  Disk Okuma/sn – tek tek disk başına
        3.  Disk MS/okuma – tek tek disk başına okuma
        4.  Disk yazma kb/sn – tek tek disk başına 
        5.  Disk Yazma/sn – tek tek disk başına
        6.  Tek tek disk başına disk yazma ms/okuma:
    5.  Ağ 
        1.  Ağ Paket/sn
        2.  Ağ paketlerinin çıkış/sn
        3.  Ağ kb/sn
        4.  Ağ kb çıkış/sn 
2.  Veri geçiş sonrasında ile işletme sahipleri varılmış tüm doğrulama testlerini gerçekleştirin. Yalnızca doğrulama testinin sonuçları özgün kaynak sistemleri için sonuçları sahip olduğu kabul edin.
3.  Arabirimleri olup çalıştığından ve olup diğer uygulamalar, yeni dağıtılan üretim sistemleriyle iletişim kurabildiğinden denetleyin
4.  SAP işlem STMS üzerinden aktarım ve düzeltme sistem denetimi
5.  Üretim için sistem sunulduktan sonra veritabanı yedeklemelerini gerçekleştirme
6.  Üretim için sistem sunulduktan sonra SAP uygulama katmanı VM'ler için VM yedeklemeleri gerçekleştirin
7.  SAP için geçerli go-live parçası değildi sistemleri aşama, ancak SAP sistemleriyle iletişim go-live bu aşamada Azure'a taşıdı, ana bilgisayar adı arabellek SM51 sıfırlamak ihtiyacınız. Bu adım adları Azure'a taşındı uygulama örnekleri ile ilişkili önbelleğe alınmış eski IP adreslerinin RID  


## <a name="post-production"></a>Üretim gönderin
Bu aşamada, tüm izleme, işletim ve sistem yönetme hakkında olduğu. Bir SAP açısından bakıldığında, eski barındırma konumunuz ile gerçekleştirmek için gerekli her zamanki görevleri uygulayın. Yapmak istediğiniz azure belirli görevler şunlardır:

1. Azure faturaları yüksek şarj sistemler için analiz edin
2. Fiyat/performans verimliliği yan VM ve depolama yan en iyi duruma getirme
3. Sistemleri kapatılabilir süresini iyileştirme  


## <a name="next-steps"></a>Sonraki Adımlar
Belgelere bakın:

- [Azure sanal makineleri planlama ve uygulama için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Azure sanal makineler dağıtım için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

