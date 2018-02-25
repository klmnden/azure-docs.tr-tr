---
title: "Azure Site Recovery kullanarak çok katmanlı Citrix XenDesktop ve XenApp dağıtım çoğaltmak | Microsoft Docs"
description: "Bu makalede, koruma ve Azure Site Recovery kullanarak Citrix XenDesktop ve XenApp dağıtımları kurtarma açıklar."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: ponatara
ms.openlocfilehash: b117525a4851dee5366aeda77c8aaefd1fdde375
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a>Azure Site Recovery kullanarak çok katmanlı Citrix XenApp ve XenDesktop dağıtım Çoğalt

## <a name="overview"></a>Genel Bakış

Citrix XenDesktop masaüstleri ve uygulamalar bir ondemand hizmet olarak herhangi bir kullanıcı için herhangi bir yere teslim eden bir Masaüstü Sanallaştırma çözümüdür. FlexCast teslim teknolojisiyle XenDesktop hızlı ve güvenli bir şekilde uygulamalara ve masaüstlerine kullanıcılara sunabilir.
Bugün, Citrix XenApp herhangi olağanüstü durum kurtarma özellikleri sağlamaz.

İyi olağanüstü durum kurtarma çözümü kurtarma planları yukarıdaki geçici modelleme izin vermelidir karmaşık uygulama mimarilerindeki ve bu nedenle tek tıklamayla emin sağlama görüntüsü çözüm için daha düşük RTO önde gelen bir olağanüstü durum durumunda çeşitli katmanları arasındaki uygulama eşlemeleri işlemek için özelleştirilmiş adımları ekleme yeteneği de vardır.

Bu belge, Hyper-V ve VMware vSphere platformlarda Citrix XenApp dağıtımları için şirket içi olağanüstü durum kurtarma çözümü oluşturmak için adım adım yönergeler sağlar. Bu belgede bir test yük devretme (olağanüstü durum kurtarma ayrıntıya) ve kurtarma planları, desteklenen yapılandırmalar ve önkoşullar kullanarak Azure planlanmamış yük devretme gerçekleştirme de açıklanmaktadır.


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakileri bildiğinizden emin olun:

1. [Bir sanal makine Azure'a çoğaltma](site-recovery-vmware-to-azure.md)
1. Nasıl yapılır [kurtarma ağını tasarlama](site-recovery-network-design.md)
1. [Azure'a yük devretme sınamasını yapmak](site-recovery-test-failover-to-azure.md)
1. [Azure için bir yük devretme işleminden](site-recovery-failover.md)
1. Nasıl yapılır [bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
1. Nasıl yapılır [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Dağıtım modelleri

Bir Citrix XenApp ve XenDesktop grubu genellikle aşağıdaki dağıtım modeli vardır:

**Dağıtım modeli**

Citrix XenApp ve XenDesktop dağıtımı ile AD DNS sunucusu, SQL veritabanı sunucusu, Citrix teslim denetleyicisi mağaza sunucusu XenApp ana (VDA), Citrix XenApp lisans

![Dağıtım modeli 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a>Site kurtarma desteği

Bu makalede amacıyla VMware sanal makineleri Citrix dağıtımlarında 6.0 vSphere tarafından yönetilen / System Center VMM 2012 R2 DR kurulumu için kullanılmış.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | İkincil bir siteye | **Azure’a**
--- | --- | ---
**Hyper-V** | Kapsamda değil | Evet
**VMware** | Kapsamda değil | Evet
**Fiziksel sunucu** | Kapsamda değil | Evet

### <a name="versions"></a>Sürümler
Müşteriler XenApp bileşenlerini Hyper-V veya VMware üzerinde çalışan sanal makineleri veya fiziksel sunucuları olarak dağıtabilirsiniz. Azure Site Recovery hem fiziksel hem de sanal dağıtımları Azure koruyabilirsiniz.
Azure'da XenApp 7.7 veya sonraki desteklenen olduğundan, yalnızca bu sürümleri dağıtımlar yerine Azure için olağanüstü durum kurtarma veya geçiş için çalışılabilir.

### <a name="things-to-keep-in-mind"></a>Göz önünde bulundurmanız gerekenler

1. Koruma ve kurtarma şirket içi sunucu işletim sistemi kullanarak XenApp sunmak için makineler yayımlanan uygulama ve XenApp yayımlanan Masaüstü dağıtımları desteklenir.

2. Koruma ve kurtarma Masaüstü VDI istemcisi için Windows 10 ' da dahil olmak üzere sanal masaüstlerini sunmak için masaüstü işletim sistemi makineleri kullanarak şirket içi dağıtımlarının desteklenmiyor. ASR Masaüstü OS'es makinelerle kurtarılmasını desteklemediğinden budur.  Ayrıca, bazı istemci sanal masaüstü işletim sistemleri (ör.) Windows 7), Azure'da lisans için henüz desteklenmiyor. Azure’da istemci/sunucu masaüstlerini lisanslama hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/licensing-faq/).

3.  Azure Site Recovery çoğaltabilir ve mevcut şirket içi MCS veya PV'ler klonlar koruyun.
Azure RM teslim denetleyicisinden sağlama kullanarak bu klonlar yeniden oluşturmanız gerekecek.

4. NetScaler NetScaler FreeBSD üzerinde temel alır ve Azure Site Recovery koruması FreeBSD işletim sistemi desteklemiyor gibi Azure Site Recovery kullanarak korunamaz. Azure'a yük devretme sonrasında Azure Market yerden yeni bir NetScaler Gereci yapılandırmak ve dağıtmak gerekir.


## <a name="replicating-virtual-machines"></a>Çoğaltma sanal makineler

Citrix XenApp dağıtım aşağıdaki bileşenlerden çoğaltma ve kurtarma etkinleştirmek için korunması gerekir.

* AD DNS sunucusunun koruma
* SQL veritabanı sunucusuna koruma
* Citrix teslim denetleyicisinin koruma
* Mağaza sunucusunun korumasını.
* Koruma XenApp yöneticisinin (VDA)
* Citrix XenApp lisans sunucusu koruma


**AD DNS sunucusu çoğaltma**

Lütfen [korumak Active Directory ve Azure Site Recovery ile DNS](site-recovery-active-directory.md) çoğaltma ve bir etki alanı denetleyicisi Azure'da yapılandırma kılavuzu üzerinde.

**SQL veritabanı sunucusu çoğaltma**

Lütfen [SQL Server olağanüstü durum kurtarma ve Azure Site Recovery ile SQL Server'ı koruma](site-recovery-sql.md) SQL sunucuları koruma için önerilen seçenekleri hakkında ayrıntılı teknik Kılavuzu.

İzleyin [bu kılavuz](site-recovery-vmware-to-azure.md) bir bileşen sanal makinelerin Azure'a çoğaltılması başlatmak için.

![XenApp bileşenlerinin koruma](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

**İşlem ve ağ ayarları**

Korunan makineler sonra (durum gösterildiğinden çoğaltılan öğeler altında "korumalı"), bilgi işlem ve ağ ayarlarının yapılandırılması gerekir.
Buna işlem ve ağ > işlem özellikleri, Azure VM adını ve hedef boyutu belirtebilirsiniz.
Gerekirse Azure gereksinimlerine uymak için adı değiştirin. Ayrıca, görüntülemek ve hedef ağ, alt ağ ve Azure VM'ye atanacak IP adresi hakkında bilgi ekleyebilirsiniz.

Şunlara dikkat edin:

* Hedef IP adresini ayarlayabilirsiniz. Bir IP adresi sağlamazsanız yük devredilen makine DHCP kullanır. Yük devretmede kullanılamayan bir adres ayarlarsanız yük devretme çalışmaz. Hedef IP adresi, yük devretme ağı testinde kullanılabilirse aynı IP adresi yük devretme sınamasında da kullanılabilir.

* AD/DNS sunucusu için şirket içi adres korunuyor aynı adres Azure sanal ağı için DNS sunucusu olarak belirtmenize olanak sağlar.

Ağ bağdaştırıcılarının sayısı, hedef sanal makine için sizin belirlediğiniz boyuta göre aşağıdaki gibi belirlenmiştir:

*   Kaynak makinedeki ağ bağdaştırıcılarının sayısı, hedef makine boyutu için verilen ağ bağdaştırıcısı sayısına eşitse veya daha azsa hedef makine kaynakla aynı sayıda bağdaştırıcıya sahip olur.
*   Kaynak sanal makinenin bağdaştırıcı sayısı, hedef boyut için izin verilen sayıyı aşarsa maksimum hedef boyutu kullanılır.
* Örneğin, kaynak makinenin iki bağdaştırıcısı varsa ve hedef makine boyutu dört adet bağdaştırıcıyı destekliyorsa hedef makinenin iki bağdaştırıcısı olur. Kaynak makinenin iki bağdaştırıcısı varken hedef boyut yalnızca bir bağdaştırıcıyı destekliyorsa hedef makinenin bir bağdaştırıcısı olur.
*   Sanal makinede birden fazla ağ bağdaştırıcısı varsa bunların tümü aynı ağa bağlanır.
*   Sanal makinede birden fazla ağ bağdaştırıcısı varsa, listede gösterilen ilk bir Azure sanal makinesinde varsayılan ağ bağdaştırıcısı olur.


## <a name="creating-a-recovery-plan"></a>Bir kurtarma planı oluşturma

Çoğaltma XenApp bileşen VM'ler için etkinleştirildikten sonra sonraki adım bir kurtarma planı oluşturmaktır.
Bir kurtarma grupları birlikte sanal makinelerle yük devretme ve kurtarma için benzer gereksinimlerini planlayın.  

**Bir kurtarma planı oluşturma adımları**

1. Kurtarma planı'nda XenApp bileşen sanal makineleri ekleyin.
2. Kurtarma planları tıklatın -> + kurtarma planı. Kurtarma planı için kullanımı kolay bir ad sağlayın.
3. VMware sanal makineler için: VMware işlem sunucusu, hedef olarak Microsoft Azure ve Resource Manager ve öğeleri seçebilir tıklayarak dağıtım modeli olarak kaynak seçme.
4. Hyper-V sanal makineleri için: kaynak VMM sunucusunu seçin, hedef Microsoft Azure ve olarak Resource Manager dağıtım modeli olarak ve Select öğelerde'ı tıklatın ve ardından XenApp dağıtım VM'ler seçin.

### <a name="adding-virtual-machines-to-failover-groups"></a>Sanal makineler yük devretme gruplara ekleme

Kurtarma planları belirli başlama sırasını, komut dosyaları veya el ile gerçekleştirilen eylemleri için yük devretme grupları eklemek için özelleştirilebilir. Aşağıdaki grupların kurtarma planına eklenmesi gerekir.

1. Yük devretme Group1: AD DNS
2. Yük devretme grup2: SQL Server Vm'leri
2. Yük devretme Group3: VDA ana görüntü VM
3. Yük devretme Grup4: Teslim denetleyici ve mağaza server Vm'leri


### <a name="adding-scripts-to-the-recovery-plan"></a>Betikleri kurtarma planına ekleniyor

Komut dosyaları önce veya sonra belirli bir grubu bir kurtarma planı çalıştırabilirsiniz. El ile yapılan eylemler olabilir de dahil ve yük devretme sırasında gerçekleştirilir.

Özelleştirilmiş kurtarma planı benzer altında:

1. Yük devretme Group1: AD DNS
2. Yük devretme grup2: SQL Server Vm'leri
3. Yük devretme Group3: VDA ana görüntü VM

   >[!NOTE]     
   >Adım 4, 6 ve 7 el ile veya komut dosyası eylemleri içeren bir şirket içi XenApp yalnızca geçerli > MCS/PV'ler kataloglarıyla ortamı.

4. Grup 3 el ile veya komut dosyası eylemi: kapatma ana VDA VM ana VDA üzerinden Azure'a başarısız olduğunda VM çalışır durumda olacaktır. Azure ARM barındırma kullanarak yeni MCS kataloglar oluşturmak için ana VDA VM durduruldu (ayrılan de) olması gereklidir durumu. Azure Portalı'ndan VM'yi kapatın.

5. Yük devretme Grup4: Teslim denetleyici ve mağaza server Vm'leri
6. Group3 el ile veya komut dosyası eylemi 1:

    ***Azure RM ana bilgisayar bağlantı Ekle***

    Azure'da yeni MCS kataloglar sağlamak için teslim denetleyicisi makinesinde Azure ARM ana bilgisayar bağlantı oluşturun. Bu konuda açıklanan adımları izleyerek [makale](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).

7. Group3 el ile veya komut dosyası eylemi 2:

    ***MCS kataloglar azure'da yeniden oluşturun***

    Birincil sitede mevcut MCS veya PV'ler klonlar Azure'a çoğaltılmamış. Çoğaltılan ana VDA ve Azure teslim denetleyicisinden sağlama ARM kullanarak bu klonlar yeniden oluşturmanız gerekecek. Bu konuda açıklanan adımları izleyerek [makale](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) MCS kataloglar oluşturma.

![Kurtarma planı XenApp bileşenleri](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   >Konumundaki komut dosyalarını kullanabilirsiniz [konumu](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) üzerinden DNS başarısız, yeni IP ile güncelleştirmek için > sanal makineleri veya başarısız bir yük dengeleyicideki eklemek için sanal makine, gerekirse üzerinden.


## <a name="doing-a-test-failover"></a>Yük devretme sınamasını yapmak

İzleyin [bu kılavuz](site-recovery-test-failover-to-azure.md) yük devretme sınamasını yapmak için.

![Kurtarma planı](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a>Bir yük devretme işleminden

İzleyin [bu kılavuz](site-recovery-failover.md) , yaparken bir yük devretme.

## <a name="next-steps"></a>Sonraki adımlar

Yapabilecekleriniz [daha fazla bilgi edinin](https://aka.ms/citrix-xenapp-xendesktop-with-asr) Bu teknik incelemede Citrix XenApp ve XenDesktop dağıtımlarda çoğaltma hakkında. Kılavuzlar bakabilir [diğer uygulamaları çoğaltmak](site-recovery-workload.md) Site RECOVERY'yi kullanarak.
