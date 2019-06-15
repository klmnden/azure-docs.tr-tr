---
title: Azure Site Recovery kullanarak çok katmanlı Citrix XenDesktop ve XenApp dağıtımı için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Bu makalede olağanüstü durum kurtarma ayarlama açıklanır fo Azure Site Recovery kullanarak Citrix XenDesktop ve XenApp dağıtımları.
author: ponatara
manager: abhemraj
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ponatara
ms.openlocfilehash: 68f12bb7335da0a996aeadd752f59db0aa360a8e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61038261"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-citrix-xenapp-and-xendesktop-deployment"></a>çok katmanlı Citrix XenApp ve XenDesktop dağıtımı için olağanüstü durum kurtarmayı ayarlama



Citrix XenDesktop masaüstlerini ve uygulamaları herhangi bir kullanıcı için herhangi bir yerde bir ondemand hizmet olarak sunan bir Masaüstü Sanallaştırma çözümüdür. FlexCast teslim teknolojisiyle XenDesktop hızlı ve güvenli bir şekilde uygulamaları ve Masaüstü kullanıcılarına teslim edebilirsiniz.
Bugün, Citrix XenApp herhangi bir olağanüstü durum kurtarma özellikleri sağlamaz.

İyi bir olağanüstü durum kurtarma çözümü, yukarıdaki etrafında kurtarma planları modelleme izin vermelidir karmaşık uygulama mimarileri ve bu nedenle tek tıklamayla sağlayan çeşitli katmanları arasındaki uygulama eşlemeleri işlemek için özelleştirilmiş adım ekleme yeteneği de daha düşük bir RTO için önde gelen bir olağanüstü durum olması halinde bir çözüm emin görüntüsü.

Bu belge, Hyper-V ve VMware vSphere platformlarında Citrix XenApp dağıtımları için şirket içi bir olağanüstü durum kurtarma çözümü oluşturmaya yönelik adım adım yönergeler sağlar. Bu belge de bir yük devretme testi (olağanüstü durum kurtarma tatbikatı) ve kurtarma planları, desteklenen yapılandırmalar ve önkoşullar'ı kullanarak azure'a planlanmamış yük devretme gerçekleştirmeyi açıklar.


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakileri bildiğinizden emin olun:

1. [Bir sanal makine Azure'a çoğaltılırken](site-recovery-vmware-to-azure.md)
1. Nasıl yapılır [bir kurtarma ağı tasarlama](site-recovery-network-design.md)
1. [Azure'a yük devretme testi yapılması](site-recovery-test-failover-to-azure.md)
1. [Azure'a yük devretme gerçekleştirmeden](site-recovery-failover.md)
1. Nasıl yapılır [bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
1. Nasıl yapılır [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Dağıtım modelleri

Citrix XenApp ve XenDesktop grubu genellikle aşağıdaki dağıtım modeli vardır:

**Dağıtım modeli**

Citrix XenApp ve XenDesktop dağıtımı ile AD DNS sunucusu, SQL veritabanı sunucusu, Citrix Delivery Controller, StoreFront sunucusu, XenApp Master (VDA), Citrix XenApp License Server

![Dağıtım modeli 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a>Site Recovery desteği

Bu makalede amacıyla VMware sanal makinelerini Citrix dağıtımlarında 6.0 vSphere tarafından yönetilen / DR'yi kurma için System Center VMM 2012 R2 kullanılmış.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | **İkincil bir siteye** | **Azure’a**
--- | --- | ---
**Hyper-V** | Kapsam içinde değil | Evet
**VMware** | Kapsam içinde değil | Evet
**Fiziksel sunucu** | Kapsam içinde değil | Evet

### <a name="versions"></a>Sürümler
Müşteriler, Hyper-V veya VMware üzerinde çalışan sanal makineleri veya fiziksel sunucuları olarak XenApp bileşenleri dağıtabilirsiniz. Azure Site Recovery, fiziksel ve sanal dağıtımları azure'da koruyabilirsiniz.
XenApp 7,7 ya da daha sonra Azure'da desteklendiğinden, yalnızca bu sürümleri ile dağıtımları yerine Azure'a olağanüstü durum kurtarma veya geçiş için çalışılabilir.

### <a name="things-to-keep-in-mind"></a>Akılda tutulması gereken noktalar

1. Koruma ve kurtarma şirket içi XenApp yayımlanan Masaüstü ve sunucu işletim sistemi kullanarak makineleri XenApp sunmak için yayımlanan uygulamaları dağıtımlar desteklenir.

2. Koruma ve kurtarma şirket içi dağıtımların masaüstü işletim sistemi makinelerinin VDI masaüstü istemcisi için Windows 10 dahil olmak üzere, sanal masaüstlerini kullanma desteklenmiyor. Site Recovery, makineleri OS'es masaüstüyle kurtarma desteklemiyor olmasıdır.  Ayrıca, bazı istemci sanal masaüstü işletim sistemleri (örn.) Windows 7) lisanslaması Azure'da henüz desteklenmemektedir. Azure’da istemci/sunucu masaüstlerini lisanslama hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/licensing-faq/).

3.  Azure Site Recovery çoğaltma ve mevcut şirket içi MCS veya PV'ler kopyaları koruyun.
Azure RM teslim denetleyicisinden sağlama kullanarak bu kopyaları yeniden oluşturmanız gerekir.

4. NetScaler NetScaler üzerinde Freebsd'ye temel alır ve Azure Site Recovery koruması FreeBSD işletim sistemi desteklemiyor Azure Site Recovery kullanılarak korunamaz. Azure'a yük devretmenin ardından Azure Marketi yeni NetScaler gereçten yapılandırmak ve dağıtmak gerekir.


## <a name="replicating-virtual-machines"></a>Çoğaltılan sanal makineler

Citrix XenApp dağıtım aşağıdaki bileşenlerden çoğaltma ve kurtarma sağlamak için korunması gerekir.

* AD DNS sunucusunun koruma
* SQL veritabanı sunucusunun koruma
* Citrix Delivery Controller'ın koruma
* StoreFront sunucusu koruma.
* XenApp Master (VDA) koruma
* Citrix XenApp License Server koruması


**AD DNS sunucusu çoğaltma**

Lütfen [korumak Active Directory ve Azure Site Recovery ile DNS](site-recovery-active-directory.md) çoğaltma ve Azure'da bir etki alanı denetleyicisinin yapılandırma kılavuzu.

**SQL veritabanı sunucusu çoğaltma**

Lütfen [SQL Server olağanüstü durum kurtarma ve Azure Site Recovery ile SQL Server'ı koruma](site-recovery-sql.md) SQL sunucuları koruma için önerilen seçenekleri ayrıntılı teknik Rehber için.

İzleyin [bu kılavuz](site-recovery-vmware-to-azure.md) bir bileşen sanal makineleri Azure'a çoğaltmaya başlamak için.

![XenApp bileşenlerinin koruma](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

**İşlem ve ağ ayarları**

Makine korunduktan sonra (durumunun çoğaltılan öğeler altından "korumalı" olarak), işlem ve ağ ayarlarının yapılandırılması gerekir.
Buna işlem ve ağ > işlem özellikleri, Azure VM adını ve hedef boyutu belirtebilirsiniz.
Gerekirse Azure gereksinimlerine uymak için adı değiştirin. Ayrıca, görüntüleyebilir ve hedef ağ, alt ağ ve Azure VM'sine atanan IP adresi hakkında bilgi ekleyin.

Şunlara dikkat edin:

* Hedef IP adresini ayarlayabilirsiniz. Bir IP adresi sağlamazsanız yük devredilen makine DHCP kullanır. Yük devretmede kullanılamayan bir adres ayarlarsanız yük devretme işe yaramaz. Hedef IP adresi, yük devretme ağı testinde kullanılabilirse aynı IP adresi yük devretme sınamasında da kullanılabilir.

* AD/DNS sunucusu, şirket içi adresini alma, Azure sanal ağı için DNS sunucusu olarak aynı adresi belirtmenizi sağlar.

Ağ bağdaştırıcılarının sayısı, hedef sanal makine için sizin belirlediğiniz boyuta göre aşağıdaki gibi belirlenmiştir:

*   Kaynak makinedeki ağ bağdaştırıcılarının sayısı, hedef makine boyutu için verilen ağ bağdaştırıcısı sayısına eşitse veya daha azsa hedef makine kaynakla aynı sayıda bağdaştırıcıya sahip olur.
*   Kaynak sanal makinenin bağdaştırıcı sayısı, hedef boyut için izin verilen sayıyı aşarsa maksimum hedef boyutu kullanılır.
* Örneğin, kaynak makinenin iki bağdaştırıcısı varsa ve hedef makine boyutu dört adet bağdaştırıcıyı destekliyorsa hedef makinenin iki bağdaştırıcısı olur. Kaynak makinenin iki bağdaştırıcısı varken hedef boyut yalnızca bir bağdaştırıcıyı destekliyorsa hedef makinenin bir bağdaştırıcısı olur.
*   Sanal makine birden çok ağ bağdaştırıcısı varsa bunların tümü aynı ağa bağlanır.
*   Sanal makinede birden çok ağ bağdaştırıcısı varsa, listede gösterilen ilk bir Azure sanal makinesinde varsayılan ağ bağdaştırıcısı olur.


## <a name="creating-a-recovery-plan"></a>Bir kurtarma planı oluşturma

XenApp bileşen VM'ler için çoğaltmayı etkinleştirdikten sonra sonraki adım bir kurtarma planı oluşturmaktır.
Bir kurtarma planı grupları birlikte sanal makineler için yük devretme ve kurtarma benzer gereksinimlerine sahip.  

**Bir kurtarma planı oluşturma adımları**

1. Kurtarma planı'nda XenApp bileşen sanal makineleri ekleyin.
2. Kurtarma planları tıklayın -> + kurtarma planı. Kurtarma planı için kullanımı kolay bir ad belirtin.
3. VMware sanal makineleri için: Kaynak seçin VMware işlem sunucusu, hedef olarak Microsoft Azure dağıtım modeli Resource Manager ve öğeleri seçin'e tıklayın.
4. Hyper-V sanal makineleri için: Kaynak VMM sunucusu, hedef olarak Microsoft Azure ve olarak Resource Manager dağıtım modeli seçin ve öğeleri üzerinde tıklayın ve sonra da VM'ler XenApp dağıtım'ı seçin.

### <a name="adding-virtual-machines-to-failover-groups"></a>Sanal makineler için yük devretme grupları ekleme

Kurtarma planları, belirli bir başlangıç düzeni, komut dosyaları veya el ile gerçekleştirilen eylemler için yük devretme grupları eklemek için özelleştirilebilir. Aşağıdaki gruplar kurtarma planına eklenmesi gerekir.

1. Yük devretme Group1: AD DNS
2. Yük devretme grup2: SQL Server VM’leri
2. Yük devretme grup3: VDA ana görüntü VM
3. Yük devretme Grup4: Teslim denetleyicisi ve mağaza server Vm'leri


### <a name="adding-scripts-to-the-recovery-plan"></a>Betikler, kurtarma planına ekleme

Betikleri önce veya sonra belirli bir grubu bir kurtarma planında çalıştırılabilir. El ile gerçekleştirilen eylemleri de dahil ve yük devretme sırasında gerçekleştirilen.

Özelleştirilmiş bir kurtarma planı benzer aşağıda:

1. Yük devretme Group1: AD DNS
2. Yük devretme grup2: SQL Server VM’leri
3. Yük devretme grup3: VDA ana görüntü VM

   >[!NOTE]     
   >Adım 4, 6 ve 7 el ile veya betik eylemleri içeren bir şirket içi XenApp için yalnızca geçerli > MCS/PV'ler kataloglarıyla ortam.

4. El ile veya komut dosyası eylemi Grup 3: Ana VDA VM'yi kapatın.
Ana VDA Azure'a devredilen VM'nin çalışır durumda olacaktır. Azure barındırma kullanan yeni MCS katalog oluşturma için ana VDA VM (de ayrılan) durdu gereken, durumu. Azure portalından VM'yi kapatın.

5. Yük devretme Grup4: Teslim denetleyicisi ve mağaza server Vm'leri
6. Grup3 el ile veya komut dosyası eylemi 1:

    ***Azure RM konak Bağlantısı Ekle***

    Azure'da yeni MCS katalogları sağlamak için teslim denetleyicisi makinesinde Azure konak bağlantı oluşturun. Bu konuda açıklanan adımları izleyerek [makale](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).

7. Grup3 el ile veya komut dosyası eylemi 2:

    ***MCS Azure kataloglarında yeniden oluşturun***

    Birincil sitede mevcut MCS veya PV'ler klonlar Azure'a çoğaltılmaz. Çoğaltılan ana VDA ve teslim denetleyicisinden sağlama Azure kullanarak bu kopyaları yeniden oluşturmanız gerekir. Bu konuda açıklanan adımları izleyerek [makale](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) Azure'da MCS katalog oluşturma.

![Kurtarma planı XenApp bileşenleri](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   >Betiklere göz kullanabilirsiniz [konumu](https://github.com/Azure/azure-quickstart-templates/tree/master/asr-automation-recovery/scripts) üzerinden yeni IP'ler, başarısız olan DNS güncelleştirmek için > sanal makineler veya yük dengeleyicide başarısız eklemek için yük devredilen sanal makine, gerekirse.


## <a name="doing-a-test-failover"></a>Yük devretme testi yapılması

İzleyin [bu kılavuz](site-recovery-test-failover-to-azure.md) yük devretme testi yapmak için.

![Kurtarma planı](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a>Bir yük devretme gerçekleştirmeden

İzleyin [bu kılavuz](site-recovery-failover.md) , yaparken bir yük devretme.

## <a name="next-steps"></a>Sonraki adımlar

Yapabilecekleriniz [daha fazla bilgi edinin](https://aka.ms/citrix-xenapp-xendesktop-with-asr) Citrix XenApp ve XenDesktop dağıtımlarını Bu teknik incelemesi yineleme hakkında. Kılavuzuna bakın [diğer uygulamaları çoğaltma](site-recovery-workload.md) Site RECOVERY'yi kullanarak.
