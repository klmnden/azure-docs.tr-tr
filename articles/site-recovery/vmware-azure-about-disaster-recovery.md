---
title: Azure'a VMware vm'lerinin olağanüstü durum kurtarma hakkında Azure Site Recovery kullanarak | Microsoft Docs
description: Bu makalede, Azure Site Recovery hizmetini kullanarak azure'a VMware vm'lerinin olağanüstü durum kurtarma için genel bir bakış sağlar.
author: raynew
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 5/30/2019
ms.author: raynew
ms.openlocfilehash: a00c129126886bd71c82940aa340a8db29cf7a0e
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417784"
---
# <a name="about-disaster-recovery-of-vmware-vms-to-azure"></a>Azure'da olağanüstü durum kurtarma VMware vm'lerinin hakkında

Bu makalede, Azure'ı kullanarak şirket içi VMware Vm'leri için olağanüstü durum kurtarma için genel bir bakış sağlar. [Azure Site Recovery](site-recovery-overview.md) hizmeti.

## <a name="what-is-bcdr"></a>BCDR nedir?

Bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine işletmenizi çalışır durumda tutma yardımcı olur. Planlanan kapalı kalma süresi ve beklenmeyen kesintiler sırasında BCDR verileri güvende ve kullanılabilir kalmasını sağlar ve uygulama çalışmaya devam sağlar. Bölgesel eşleştirme ve yüksek oranda kullanılabilir depolama alanı gibi platform BCDR özelliklerine ek olarak, Azure kurtarma Hizmetleri, BCDR çözümünüzün bir parçası olarak sağlar. Kurtarma hizmetleri şunlardır: 

- [Azure yedekleme](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) , şirket içi ve Azure sanal makine verilerini yedekler. Bir dosya ve klasörleri, belirli iş yükleri veya tüm VM'yi yedekleyebilirsiniz. 
- [Azure Site Recovery](site-recovery-overview.md) uygulamaları ve şirket içi makine ya da Azure Iaas Vm'leri üzerinde çalışan iş yükleri için esneklik ve olağanüstü durum kurtarma sağlar. Site Recovery çoğaltma işlemlerini yönetir ve kesintiler durumunda, azure'a yük devretme işler. Ayrıca, birincil sitenize azure'dan kurtarma işler. 

## <a name="how-does-site-recovery-do-disaster-recovery"></a>Site Recovery, olağanüstü durum kurtarma nasıl musunuz?

1. Azure ile şirket içi siteniz hazırlanıyor sonra ayarlayın ve şirket içi makineleriniz için çoğaltmayı etkinleştirin.
2. Site Recovery ilk çoğaltma ilkesi ayarlarınızı uygun olarak makinenin yönetir.
3. İlk çoğaltma sonrasında, Site Recovery, delta değişikliklerinin Azure'a çoğaltır. 
4. Her şeyin beklendiği gibi çoğaltma yaparken, bir olağanüstü durum kurtarma tatbikatı çalıştırma.
    - Ayrıntıya yardımcı olur, yük devretme gerçek ihtiyaç duyduğunuzda beklendiği gibi çalıştığından emin olun.
    - Ayrıntıya üretim ortamınızı etkilemeden test yük devretme gerçekleştirir.
5. Bir kesinti oluşursa, Azure'a bir tam yük devretme çalıştırın. Tek bir makine üzerinden başarısız olabilir veya aynı anda birden çok makinede yük gerçekleştiren bir kurtarma planı oluşturabilirsiniz.
6. Yük devretmede, yönetilen diskler veya depolama hesapları VM verilerden Azure Vm'leri oluşturulur. Kullanıcılar uygulamaları ve iş yüklerini Azure VM'den erişmeye devam etmek
7. Şirket içi siteniz yeniden kullanılabilir olduğunda, Azure'dan yeniden çalışma.
8. İlk duruma döndürme ve birincil çalışan bir kez daha sonra tekrar şirket içi Vm'leri Azure'a çoğaltmaya başlayın.


## <a name="how-do-i-know-if-my-environment-is-suitable-for-disaster-recovery-to-azure"></a>Ortamımın azure'a olağanüstü durum kurtarma için uygun olup olmadığını nasıl anlarım?

Site Recovery, desteklenen bir VMware VM veya fiziksel sunucu üzerinde çalışan tüm iş yüklerini çoğaltabilirsiniz. Ortamınızda kontrol etmeniz gereken noktalar şunlardır:

- VMware sanal makinelerini çoğaltıyorsanız, VMware sanallaştırma sunucularını doğru sürümlerini kullanıyorsunuz? [Burayı](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers).
- Desteklenen bir işletim sistemi çalıştıran çoğaltmak istediğiniz makinelerde mi? [Burayı](vmware-physical-azure-support-matrix.md#replicated-machines).
- Linux olağanüstü durum kurtarma için desteklenen dosya sistemi/Konuk depolama makineler çalışıyor? [Burayı tıklatın](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage)
- Çoğaltmak istediğiniz makinelerin Azure gereksinimlerine uygun? [Burayı](vmware-physical-azure-support-matrix.md#azure-vm-requirements).
- Ağ yapılandırmanızı destekleniyor mu? [Burayı](vmware-physical-azure-support-matrix.md#network).
- Depolama yapılandırmanızı destekleniyor mu? [Burayı](vmware-physical-azure-support-matrix.md#storage).


## <a name="what-do-i-need-to-set-up-in-azure-before-i-start"></a>Ben başlamadan önce Azure'da ayarlamak ne gerekiyor?

Azure'da aşağıdaki hazırlamanız gerekir:

1. Azure hesabınızı kullanarak Azure'da VM oluşturmak için gerekli izinlere sahip olduğunu doğrulayın.
2. Yük depolama hesapları ya da yönetilen diskler yük devretme sonrasında oluşturulan Azure Vm'lerinin katılacağı bir Azure ağı oluşturun.
3. Site Recovery için bir Azure kurtarma Hizmetleri kasası ayarlama. Kasa, Azure portalında bulunan ve dağıtmak, yapılandırmak, düzenleyin, izleme ve sorun giderme, Site Recovery dağıtımı için kullanılır.

*Daha fazla yardıma mı ihtiyacınız var?*

Azure tarafından ayarlama konusunda bilgi [Hesap doğrulama](tutorial-prepare-azure.md#verify-account-permissions), oluşturma bir [ağ](tutorial-prepare-azure.md#set-up-an-azure-network), ve [bir kasası ayarlama](tutorial-prepare-azure.md#create-a-recovery-services-vault).



## <a name="what-do-i-need-to-set-up-on-premises-before-i-start"></a>Şirket içi başlarım önce ayarlamak ne gerekiyor?

Yapmanız gerekenler şirket içinde:

1. Hesapları birkaç ayarlamanız gerekir:

    - VMware sanal makinelerini çoğaltıyorsanız, bir hesap vCenter sunucusunun veya vSphere ESXi konaklarına erişim için Site Recovery Vm'leri otomatik olarak bulmak için gereklidir.
    - Bir hesabı, Site Recovery Mobility Hizmeti Aracısı her fiziksel makine veya çoğaltmak istediğiniz VM üzerinde yüklemek için gereklidir.

2. VMware altyapınızı uyumluluğunu denetleyin, daha önce başarmadık gerekir.
3. Yük devretmeden sonra Azure Vm'lerine bağlanabildiğinden emin olun. RDP şirket içi Windows makineler veya SSH Linux makinelerinde ayarlayın.

*Daha fazla yardıma mı ihtiyacınız var?*
- Hesaplar için hazırlama [otomatik bulma](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) ve [Mobility hizmeti yüklemesi](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation).
- [Doğrulama](vmware-azure-tutorial-prepare-on-premises.md#check-vmware-requirements) VMware ayarlarınızı uyumludur.
- [Hazırlama](vmware-azure-tutorial-prepare-on-premises.md#prepare-to-connect-to-azure-vms-after-failover) yük devretmeden sonra Azure'da bağlantı.
- Yük devretme sonrasında Azure Vm'leri için IP adresini ayarlama hakkında daha fazla ayrıntılı yardım istiyorsanız [bu makaleyi okuyun](concepts-on-premises-to-azure-networking.md).

## <a name="how-do-i-set-up-disaster-recovery"></a>Olağanüstü durum kurtarma işlemini nasıl ayarlayabilirim?

Azure ve şirket içi altyapınızı aldıktan sonra olağanüstü durum kurtarma ayarlama ayarlayabilirsiniz.

1. Dağıtmanız gereken bileşenleri hakkında bilgilere [VMware-Azure arası mimari](vmware-azure-architecture.md)ve [fiziksel-Azure arası mimari](physical-azure-architecture.md). Vardır bileşenlerinin nasıl hepsi bir araya getireceğinizi anlamanız önemlidir.
2. **Kaynak ortamı**: Dağıtımında ilk adım, çoğaltma kaynak ortamınızı ayarlayın. Neleri çoğaltmak istediğinizi ve çoğaltmak istediğiniz belirt
3. **Yapılandırma sunucusu**: Şirket içi kaynak ortamınızda bir yapılandırma sunucusu ayarlamanız gerekir:
    - Tek şirket içi makineyi yapılandırma sunucusudur. VMware olağanüstü durum kurtarma için indirilebilir bir OVF şablondan dağıtılabilir bir VMware VM olarak dağıtılmasını öneririz.
    - Yapılandırma sunucusu şirket içi ve Azure arasındaki iletişimleri koordine eder.
    - Birkaç diğer bileşenlerin configuration server makinesinde çalıştırın.
        - İşlem sunucusu alır, en iyi duruma getirir ve önbellek depolama hesabına azure'da çoğaltma verilerini gönderir. Aynı zamanda çoğaltmak istediğiniz makinelere Mobility hizmetinin otomatik olarak yüklenmesini işleyen ve VMware sunucuları üzerinde sanal makineleri otomatik olarak bulunmasını gerçekleştirir.
        - Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.
    - Otomatik bulma ve Mobility hizmeti yüklemesi için oluşturulan hesapları belirtme ve yapılandırma sunucusunun MySQL Server ve VMware powerclı'yı indirme kasaya kaydetme ayarlama içerir.
4. **Hedef ortam**: Azure aboneliğinizi ve ağ ayarlarını belirterek hedef Azure ortamı ayarlayın.
5. **Çoğaltma İlkesi**: Çoğaltma nasıl gerçekleşeceğini belirtin. Ayarları dahil ne sıklıkta kurtarma noktaları oluşturulur ve depolanır ve mi uygulamayla tutarlı anlık görüntü oluşturulmalıdır.
6. **Çoğaltmayı etkinleştirme**. Şirket içi makineler için çoğaltma işlemini etkinleştirirsiniz. Mobility hizmetini yükleme için bir hesabı oluşturduysanız, bir makine için çoğaltmayı etkinleştirdiğinizde, sonra yüklenir. 

*Daha fazla yardıma mı ihtiyacınız var?*

- Bu adımların hızlı bir kılavuz için deneyebileceğiniz bizim [VMware öğretici](vmware-azure-tutorial.md), ve [fiziksel sunucu izlenecek](physical-azure-disaster-recovery.md).
- [Daha fazla bilgi edinin](vmware-azure-set-up-source.md) kaynak ortamı ayarlama hakkında daha fazla.
- [Hakkında bilgi edinin](vmware-azure-deploy-configuration-server.md) yapılandırma sunucusu gereksinimleri ve yapılandırma sunucusu VMware çoğaltması için bir OVF şablonu ile ayarlama. Kullanamazsınız herhangi bir nedenden dolayı bir şablon veya fiziksel sunucuları çoğaltıyorsanız [bu yönergeleri kullanın](physical-azure-set-up-source.md#set-up-the-source-environment).
- [Daha fazla bilgi edinin](vmware-azure-set-up-target.md) hedef ayarlar hakkında daha fazla.
- [Daha fazla bilgi edinin](vmware-azure-set-up-replication.md) bir çoğaltma ilkesi ayarlama hakkında.
- [Bilgi](vmware-azure-enable-replication.md) çoğaltmayı etkinleştirme ve [hariç](vmware-azure-exclude-disk.md) diskleri çoğaltmanın dışında.


## <a name="something-went-wrong-how-do-i-troubleshoot"></a>Sorunlarını nasıl giderebilirim sorun?

- İlk adım, deneyin [dağıtımınızı izleme](site-recovery-monitor-and-troubleshoot.md) çoğaltılan öğeler, işler ve altyapı sorunları durumunu doğrulayın ve hataları tanımlayın.
- İlk çoğaltma tamamlayamıyor veya devam eden çoğaltma beklendiği gibi çalışmıyorsa [bu makaleyi gözden geçirin](vmware-azure-troubleshoot-replication.md) yaygın hatalar ve sorun giderme ipuçları için.
- Çoğaltmak istediğiniz makinelere Mobility hizmetinin otomatik yükleme ile ilgili sorunlar yaşıyorsanız, sık karşılaşılan hatalar gözden [bu makalede](vmware-azure-troubleshoot-push-install.md).
- Yük devretme beklendiği gibi çalışmıyorsa, sık karşılaşılan iade [bu makalede](site-recovery-failover-to-azure-troubleshoot.md).
- Yeniden çalışma çalışmıyorsa, sorununuzu görünür olup olmadığını denetleyin. [bu makalede](vmware-azure-troubleshoot-failback-reprotect.md).



## <a name="next-steps"></a>Sonraki adımlar

Artık bir yerde daha fazla çoğaltma ile gerekir [bir olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md) yük devretmenin beklendiği gibi çalıştığından emin olmak için. 
