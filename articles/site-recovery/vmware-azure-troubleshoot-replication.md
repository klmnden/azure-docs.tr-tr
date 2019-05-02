---
title: Azure Site Recovery kullanarak VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için çoğaltma sorunlarını giderme | Microsoft Docs
description: Bu makalede VMware vm'lerinin olağanüstü durum kurtarma sırasında yaygın çoğaltma sorunlarına yönelik sorun giderme bilgileri ve fiziksel sunucuları Azure'a Azure Site Recovery kullanarak sağlar.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/14/2019
ms.author: mayg
ms.openlocfilehash: 3221b2bd18b8b0756f280d88fffc6016d0498b8f
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924820"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>VMware Vm'lerini ve fiziksel sunucular için çoğaltma sorunlarını giderme

Bu makalede bazı yaygın sorunlar ve şirket içi VMware Vm'leri ve fiziksel sunucuları azure'a çoğalttığınızda, çalıştırdığınızca belirli hataları [Site Recovery](site-recovery-overview.md).

## <a name="step-1-monitor-process-server-health"></a>1. Adım: İşlem sunucusu durumunu izleme

Site Recovery kullanan [işlem sunucusu](vmware-physical-azure-config-process-server-overview.md#process-server) alabilir ve çoğaltılan verileri En İyileştir ve Azure'a gönderin.

Portalında, bağlı olan ve işlem sunucusu ile ilişkili kaynak makinelerden için düzgün çalışmasını ve çoğaltma İlerliyor emin olmak için işlem sunucularının durumunu izlemek öneririz.

- [Hakkında bilgi edinin](vmware-physical-azure-monitor-process-server.md) işlem sunucularını izleme.
- [En iyi uygulamaları inceleme](vmware-physical-azure-troubleshoot-process-server.md#best-practices-for-process-server-deployment)
- [Sorun giderme](vmware-physical-azure-troubleshoot-process-server.md#check-process-server-health) işlem sunucu durumu.

## <a name="step-2-troubleshoot-connectivity-and-replication-issues"></a>2. Adım: Bağlantı ve çoğaltma sorunlarını giderme

İlk ve devam eden çoğaltma hatalarını genellikle işlem sunucusu ile Azure arasında veya kaynak sunucu ile işlem sunucusu arasında bağlantı sorunları nedeniyle oluşup. 

Bu sorunları çözmek için [bağlantı ve çoğaltma sorunlarını giderme](vmware-physical-azure-troubleshoot-process-server.md#check-connectivity-and-replication).




## <a name="step-3-troubleshoot-source-machines-that-arent-available-for-replication"></a>3. Adım: Çoğaltma için kullanılamayan kaynak makineler sorunlarını giderme

Site RECOVERY'yi kullanarak çoğaltmayı etkinleştirmek için kaynak makine seçmeye çalıştığınızda, makine aşağıdaki nedenlerden biri için kullanılabilir olmayabilir:

* **Aynı içeren iki sanal makine örnek UUİD'si**: VCenter altında iki sanal makine aynı örneğini UUID varsa, Azure portalında ilk sanal makine yapılandırma sunucusu tarafından bulunan gösterilir. Bu sorunu çözmek için iki sanal makine aynı örneğini UUID sahip olun. Bu senaryo, burada VM yedekleme etkin hale gelir ve bulma Kayıtlarımız günlüğe örneklerinde yaygın olarak görülür. Başvurmak [Azure Site Recovery Azure'a VMware: Yinelenen veya eski girdilerin temizlenmesini nasıl](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) çözümlenecek.
* **Yanlış vCenter kullanıcı kimlik bilgilerini**: OVF şablonu veya birleşik Kurulumu kullanarak yapılandırma sunucusunu ayarladıktan ayarladığınızda, doğru vCenter kimlik bilgilerini emin olun. Kurulum sırasında eklenen kimlik bilgilerini doğrulamak için bkz: [otomatik bulma için kimlik bilgilerini değiştirme](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* **vCenter ayrıcalıklar yetersiz**: VCenter erişmek için sağlanan izinler gerekli izinlere sahip değilseniz, sanal makineleri keşfetmek için hatası gerçekleşebilir. İçinde açıklanan izinleri olun [otomatik bulma için bir hesap hazırlama](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) vCenter kullanıcı hesabına eklenir.
* **Azure Site Recovery yönetim sunucuları**: Sanal Makine Yönetim sunucusu bir veya daha fazla aşağıdaki rollerin - altında kullanılıyorsa, yapılandırma sunucusu /scale-out işlem sunucusu / ana hedef sunucusu, sonra da sanal makineyi portaldan seçme mümkün olmayacaktır. Yönetim sunucuları yinelenemez.
* **Zaten korumalı/başarısız üzerinden Azure Site Recovery Hizmetleri aracılığıyla**: Sanal makine zaten korumalı veya Site Recovery yük devretti, sanal makineyi Portalı'nda koruma için işaretleyin kullanılamaz. Portalda aradığınız sanal makineyi başka bir kullanıcı tarafından veya farklı bir abonelikte zaten korunmuyor emin olun.
* **bağlı vCenter**: VCenter bağlı durumda olup olmadığını denetleyin. Doğrulamak için kurtarma Hizmetleri Kasası'na gidin > Site Recovery altyapısı > yapılandırma sunucuları >'a tıklayın, ilgili yapılandırma sunucusunda > hakkınız ilişkili sunucular ilgili ayrıntıları içeren bir dikey pencere açar. VCenter bağlı olup olmadığını denetleyin. "Bağlanmadı" bir durumda ise, sorunu çözün ve ardından [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server) portalında. Bundan sonra sanal makine portalda listelenir.
* **ESXi güç kapalı**: Altında sanal makinenin bulunduğu ESXi ana bilgisayar ise, kapalı sonra sanal makine listede bulunmayan veya Azure portalında seçilebilir olmayacak. ESXi konağı Aç [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server) portalında. Bundan sonra sanal makine portalda listelenir.
* **Yeniden başlatma bekleniyor**: Sanal makine üzerinde bekleyen bir yeniden başlatma yoksa, daha sonra Azure portalında makine seçmek mümkün olmayacaktır. Yeniden başlatma bekleniyor etkinliklerini tamamlamak için olun [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Bundan sonra sanal makine portalda listelenir.
* **Nebyl nalezen IP**: Sanal makine ile ilişkili geçerli bir IP adresi yoksa, daha sonra Azure portalında makine seçmek mümkün olmayacaktır. Sanal makine için geçerli bir IP adresi atamak için olun [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Bundan sonra sanal makine portalda listelenir.

### <a name="troubleshoot-protected-virtual-machines-greyed-out-in-the-portal"></a>Korunan sanal makinelerin portalda gri sorunlarını giderme

Sistemdeki yinelenen girişler varsa altında Site Recovery çoğaltılan sanal makineler, Azure portalında mevcut değildir. Eski girişleri silmek ve sorunu çözmek öğrenmek için bkz [Azure Site Recovery VMware-Azure: Yinelenen veya eski girdilerin temizlenmesini nasıl](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).

## <a name="common-errors-and-solutions"></a>Genel hatalar ve çözümleri

### <a name="initial-replication-issues-error-78169"></a>İlk çoğaltma sorunlarını [Hata 78169]

Bir yukarıdaki olduğunu sağlama üzerinde hiçbir bağlantı, bant genişliği veya zaman ilgili sorunları eşitleyebilir, emin olun:

- Hiçbir virüsten koruma yazılımı, Azure Site Recovery engelliyor. Bilgi [daha fazla](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) üzerinde Azure Site Recovery için gereken klasör dışlamaları.

### <a name="missing-app-consistent-recovery-points-error-78144"></a>Uygulamayla tutarlı kurtarma noktaları [Hata 78144] eksik

 Bu, Birim Gölge Kopyası Hizmeti (VSS) ile ilgili sorunlar nedeniyle gerçekleşir. Bunu çözmek için: 
 
- Azure Site Recovery agent'ın yüklü sürümü en az 9.22.2 olduğunu doğrulayın. 
- Bir hizmet olarak Windows Hizmetleri VSS sağlayıcısı yüklü olduğunu doğrulayın ve ayrıca Azure Site Recovery VSS sağlayıcısı listelendiğini kontrol etmek için bileşen hizmeti MMC doğrulayın.
- VSS sağlayıcısı yüklü değilse bkz [sorunlarını giderme makalesine yükleme hatası](vmware-azure-troubleshoot-push-install.md#vss-installation-failures).

- VSS devre dışıysa,
    - VSS sağlayıcısı hizmeti başlangıç türü değerine ayarlandığını doğrulayın **otomatik**.
    - Şu hizmetleri yeniden başlatın:
        - VSS hizmeti
        - Azure Site Recovery VSS sağlayıcısı
        - VDS hizmeti

### <a name="source-machines-with-high-churn-error-78188"></a>Kaynak makine ile yüksek değişim sıklığı [Hata 78188]

Olası nedenler:
- Listelenen sanal makine disklerinin veri değişim hızı (yazılan bayt/sn) birden fazla [Azure Site Recovery tarafından desteklenen sınırları](site-recovery-vmware-deployment-planner-analyze-report.md#azure-site-recovery-limits) çoğaltma hedefi depolama hesabı türü.
- Değişim sıklığı oranı ani bir depo olan hangi yüksek miktarda veri Beklemede karşıya yükleme.

Bu sorunu çözmek için:
- Hedef depolama hesabı türü (standart veya Premium) kaynakta değişim sıklığı oranı gereksinime uygun şekilde sağlandığından emin olun.
- Gözlemlenen değişim sıklığı geçici ise, bekleyen veriler karşıya flow'unu yakalayın ve kurtarma noktaları oluşturmak için birkaç saat bekleyin.
- Sorun devam ederse, Site Recovery kullanın [dağıtım Planlayıcısı](site-recovery-deployment-planner.md#overview) çoğaltma planlamanıza yardımcı olacak.

### <a name="source-machines-with-no-heartbeat-error-78174"></a>Sinyal yok [Hata 78174] ile kaynak makineler

Azure Site Recovery Mobility Aracısı kaynak makinede yapılandırma sunucusu (CS) ile iletişim halinde ortaya çıkar.

Bu sorunu çözmek için kaynak VM yapılandırma sunucusuna ağ bağlantısı doğrulamak için aşağıdaki adımları kullanın:

1. Kaynak makinenin çalışır durumda olduğunu doğrulayın.
2. Kaynak makineye yönetici ayrıcalıklarına sahip bir hesap kullanarak oturum açın.
3. Şu hizmetlerin çalıştığını doğrulayın ve yeniden Hizmetleri:
   - Svagents (Inmage Scout VX Aracısı)
   - Inmage Scout uygulama hizmeti
4. Kaynak makinede hata ayrıntıları için konumundaki günlükleri inceleyin:

       C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log
    
### <a name="process-server-with-no-heartbeat-error-806"></a>İşlem sunucusu ile hiçbir sinyal [Hata 806]
Var olması durumunda sinyal alınmadı işlem sunucusu'nden (PS) kontrol edin:
1. PS VM hazır ve çalışır durumda
2. PS hata ayrıntıları için aşağıdaki oturum açmasına denetleyin:

       C:\ProgramData\ASR\home\svsystems\eventmanager*.log
       and
       C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

### <a name="master-target-server-with-no-heartbeat-error-78022"></a>Ana hedef sunucu ile hiçbir sinyal [Hata 78022]

Azure Site Recovery Mobility Aracısı ana Hedef'te'yapılandırma sunucusu ile iletişim halinde ortaya çıkar.

Sorunu çözmek için hizmet durumunu doğrulamak için aşağıdaki adımları kullanın:

1. Ana hedef VM'nin çalıştığından emin olun.
2. Ana hedef VM'de yönetici ayrıcalıklarına sahip bir hesap kullanarak oturum açın.
    - Svagents hizmetinin çalıştığını doğrulayın. Çalışıyorsa, hizmeti yeniden başlatın.
    - Hata ayrıntıları için konumundaki günlükleri kontrol edin:
        
          C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma ihtiyacınız varsa, sorunuzu gönderin [Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Etkin bir topluluk sunuyoruz ve biri, Mühendislerimiz yardımcı olabilir.
