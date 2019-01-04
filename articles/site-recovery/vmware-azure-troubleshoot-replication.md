---
title: VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery ile azure'a olağanüstü durum kurtarma için çoğaltma sorunlarını giderme | Microsoft Docs
description: Bu makalede VMware vm'lerinin olağanüstü durum kurtarma sırasında yaygın çoğaltma sorunlarına yönelik sorun giderme bilgileri ve fiziksel sunucuları Azure Site Recovery ile azure'a sağlar.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: 1c37b764b47856d3a369228d3f224f2a464029bb
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790665"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>VMware Vm'lerini ve fiziksel sunucular için çoğaltma sorunlarını giderme

VMware sanal makineleri veya fiziksel sunucuları Azure Site Recovery kullanılarak korunurken, belirli bir hata iletisi alabilirsiniz. Bu makalede karşılaşabileceğiniz çoğaltma VMware Vm'lerini ve fiziksel sunucuları kullanılarak Azure'a şirket, bazı yaygın sorunlar [Azure Site Recovery](site-recovery-overview.md).


## <a name="initial-replication-issues"></a>İlk çoğaltma sorunları

Çoğu durumda, kaynak sunucu işlem sunucusu veya işlem sunucusu Azure arasında bağlantı sorunları size destek karşılaştığınız ilk çoğaltma hatalarını kaynaklanır. Çoğu durumda, aşağıda listelenen adımları izleyerek bu sorunları giderebilirsiniz.

### <a name="verify-the-source-machine"></a>Kaynak makine doğrulayın
* Kaynak sunucu makine komut satırından Telnet https bağlantı noktası (varsayılan 9443) ile işlem sunucusu herhangi bir ağ bağlantısı sorunları veya güvenlik duvarı bağlantı noktası engelleme sorunları olup olmadığını görmek için aşağıda gösterildiği gibi ping işlemi yapmak için kullanın.

    `telnet <PS IP address> <port>`
> [!NOTE]
    > Telnet kullanın, PING bağlantısını test etmek için kullanmayın.  Telnet yüklü değilse, adımları listesi izleyin [burada](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Alınamazsa bağlanmak, işlem sunucusunun gelen bağlantı noktası 9443'e izin ver ve sorunu hala çıkar olmadığını denetleyin. Bazı durumlarda, işlem sunucusu bu soruna neden olan DMZ olduğu görülmüştür.

* Hizmet durumunu `InMage Scout VX Agent – Sentinel/OutpostStart` sorun devam ederse, çalışan ve onay değilse.   

### <a name="verify-the-process-server"></a>İşlem sunucusu doğrulayın

* **İşlem sunucusu etkin bir şekilde veri Azure'a gönderme, denetleyin**

İşlem sunucusu makineden Görev Yöneticisi'ni (Ctrl-Shift-Esc tuşuna basın) açın. Performans sekmesine gidin ve 'Açık Kaynak İzleyicisi' bağlantısına tıklayın. Kaynak Yöneticisi'nden ağ sekmesine gidin. Büyük miktarda veri (MB) cinsinden cbengine.exe 'Ağ etkinliği ile işlem' de etkin bir şekilde gönderme, kontrol edin.

![Çoğaltmayı etkinleştirme](./media/vmware-azure-troubleshoot-replication/cbengine.png)

Aksi durumda, aşağıda listelenen adımları izleyin:

* **İşlem sunucusu, Azure Blob bağlanabiliyor olup olmadığını denetleyin**: Seçin ve 'TCP işlem sunucusundan Azure depolama blobu URL'si bağlantısı olup olmadığını görmek için bağlantıları' görüntülemek için cbengine.exe denetleyin.

![Çoğaltmayı etkinleştirme](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

Denetim Masası'na, ardından gidin > hizmetler, aşağıdaki hizmetleri ayarlayıp çalıştırmaya olup olmadığını denetleyin:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     *
(Yeniden) Sorun devam ederse, herhangi bir hizmeti çalışmıyor ve onay başlatın.

* **İşlem sunucusu bağlantı noktası 443'ü kullanarak Azure genel IP adresine bağlanmanız mümkün olup olmadığını denetleyin**

Gelen son CBEngineCurr.errlog açın `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` için arama yapın: 443 ve bağlantı denemesi başarısız oldu.

![Çoğaltmayı etkinleştirme](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

Sorun varsa, işlem sunucusu komut satırından, bağlantı noktası 443 aracılığıyla CBEngineCurr.currLog içinde bulundu (içinde görüntü maskelenmiş), Azure genel IP adresine ping atmayı telnet kullanın.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Bağlanmak erişemiyorsanız, güvenlik duvarı veya Proxy sonraki adımda açıklandığı gibi nedeniyle erişim sorunu olup olmadığını denetleyin.


* **İşlem sunucusu üzerindeki IP adresi tabanlı güvenlik duvarı erişimi engellemediğini varsa denetleyin**: Sunucu üzerinde bir IP adresi tabanlı güvenlik duvarı kuralları kullanıyorsanız, ardından Microsoft Azure veri merkezi IP aralıkları tam listesini indirin [burada](https://www.microsoft.com/download/details.aspx?id=41653) ve iletişimi sağlarlar emin olmak için Güvenlik Duvarı'nı yapılandırmanıza ekleyin Azure (ve HTTPS (443) bağlantı noktası).  Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.

* **URL tabanlı güvenlik duvarı işlem sunucusu üzerindeki erişimi engellemediğini varsa denetleyin**:  Sunucuda bir URL tabanlı güvenlik duvarı kuralları kullanıyorsanız, aşağıdaki URL'ler için güvenlik duvarı yapılandırması eklenen emin olun.

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

* **İşlem sunucusu üzerindeki ara sunucu ayarları erişim unsur olmadığını kontrol**.  Bir ara sunucu kullanıyorsanız, proxy sunucusu adı DNS sunucusu tarafından çözüyor emin olun.
Yapılandırma sunucusu Kurulum zamanında sağlanan denetlemek için. Kayıt defteri anahtarına gidin

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Artık aynı ayarları Azure Site Recovery aracısı tarafından veri göndermek için kullanıldığından emin olun.
Arama Microsoft Azure yedekleme

Açın ve eylemini tıklatın > özelliklerini değiştirme. Proxy yapılandırma sekmesi altında kayıt defteri ayarları tarafından gösterildiği gibi aynı olmalıdır proxy adresi görmeniz gerekir. Aksi halde, lütfen aynı adrese değiştirin.


* **Kısıtlama bant genişliği işlem sunucusu üzerindeki sınırlı değildir, kontrol**:  Bant genişliğini artırın ve sorunu hala mevcut olup olmadığını denetleyin.

## <a name="source-machine-to-be-protected-through-site-recovery-is-not-listed-on-azure-portal"></a>Azure portalında Site Recovery ile korunacak kaynak makine listede değil

Makine Azure Site Recovery ile çoğaltmayı etkinleştirmek için kaynak makinenin seçmek çalışırken, aşağıdaki sebeplerden dolayı devam edebilmeniz kullanılamıyor olabilir

* Portalda, varsa iki sanal makine ile aynı örnek UUİD'si, vCenter altında daha sonra yapılandırma sunucusu tarafından bulunan ilk sanal makine gösterilir. Çözmek için iki sanal makine yok'ın aynı örneğine UUID sahip olun.
* Doğru vCenter kimlik bilgilerini sırasında belirlenen OVF şablonu veya birleştirilmiş aracılığıyla yapılandırmasının yukarı eklediğinizden emin olun. Eklenen kimlik bilgilerini doğrulamak için paylaşılan yönergelerine bakın [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* Sağlanan vCenter erişmek için yeterli izniniz yok, sanal makineleri bulma hatası için neden olabilir. Sağlanan izinler olun [burada](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) vCenter kullanıcı hesabına eklenir.
* Site Recovery sanal makine zaten korunuyorsa, ardından, koruma için kullanılamaz. Portalda aradığınız sanal makineyi başka bir kullanıcı tarafından veya diğer abonelikler altında korunmuyor emin olun.

## <a name="protected-virtual-machines-are-greyed-out-in-the-portal"></a>Korumalı sanal makineleri dışarı portalda gri

Altında Site Recovery çoğaltılan sanal makineler olmadığını anlamak gri sistemdeki yinelenen girişler vardır. Verilen yönergelerine başvurun [burada](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) eski girişleri silmek ve sorunu çözmek için.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız olursa, ardından sorgunuza sonrası [Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Etkin bir topluluk sunuyoruz ve biri, Mühendislerimiz yardımcı olması mümkün olacaktır.
