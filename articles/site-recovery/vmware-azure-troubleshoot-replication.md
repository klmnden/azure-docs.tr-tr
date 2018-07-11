---
title: VMware VM ve fiziksel sunucu çoğaltması Azure Site Recovery ile azure'a yönelik çoğaltma sorunlarını giderme | Microsoft Docs
description: Bu makalede, VMware Vm'leri ve fiziksel sunucuları Azure Site Recovery ile Azure'a çoğaltırken yaygın çoğaltma sorunlarını giderme sağlar.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: f305f552d576f58914bc33351331f1da3c68bc23
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951657"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>VMware Vm'lerini ve fiziksel sunucular için çoğaltma sorunlarını giderme

VMware sanal makineleri veya fiziksel sunucuları Azure Site Recovery kullanılarak korunurken, belirli bir hata iletisi alabilirsiniz. Bu makalede karşılaşabileceğiniz çoğaltma VMware Vm'lerini ve fiziksel sunucuları kullanılarak Azure'a şirket, bazı yaygın sorunlar [Azure Site Recovery](site-recovery-overview.md).

## <a name="initial-replication-issues"></a>İlk çoğaltma sorunlarını.

Çoğu durumda, kaynak sunucu işlem sunucusu veya işlem sunucusu Azure arasında bağlantı sorunları size destek karşılaştığınız ilk çoğaltma hatalarını kaynaklanır. Çoğu durumda, aşağıda listelenen adımları izleyerek bu sorunları giderebilirsiniz.

### <a name="verify-the-source-machine"></a>Kaynak makine doğrulayın
* Kaynak sunucu makine komut satırından Telnet https bağlantı noktası (varsayılan 9443) ile işlem sunucusu herhangi bir ağ bağlantısı sorunları veya güvenlik duvarı bağlantı noktası engelleme sorunları olup olmadığını görmek için aşağıda gösterildiği gibi ping işlemi yapmak için kullanın.

    `telnet <PS IP address> <port>`
> [!NOTE]
    > Telnet kullanın, PING bağlantısını test etmek için kullanmayın.  Telnet yüklü değilse, adımları listesi izleyin [burada](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Alınamazsa bağlanmak, işlem sunucusunun gelen bağlantı noktası 9443'e izin ver ve sorunu hala çıkar olmadığını denetleyin. Bazı durumlarda, işlem sunucusu bu soruna neden olan DMZ olduğu görülmüştür.

* Hizmet durumunu `InMage Scout VX Agent – Sentinel/OutpostStart` sorun devam ederse, çalışan ve onay değilse.   

## <a name="verify-the-process-server"></a>İşlem sunucusu doğrulayın

* **İşlem sunucusu etkin bir şekilde veri Azure'a gönderme, denetleyin**

İşlem sunucusu makineden Görev Yöneticisi'ni (Ctrl-Shift-Esc tuşuna basın) açın. Performans sekmesine gidin ve 'Açık Kaynak İzleyicisi' bağlantısına tıklayın. Kaynak Yöneticisi'nden ağ sekmesine gidin. Büyük miktarda veri (MB) cinsinden cbengine.exe 'Ağ etkinliği ile işlem' de etkin bir şekilde gönderme, kontrol edin.

![Çoğaltmayı etkinleştirme](./media/vmware-azure-troubleshoot-replication/cbengine.png)

Aksi durumda, aşağıda listelenen adımları izleyin:

* **İşlem sunucusu, Azure Blob bağlanabiliyor olup olmadığını denetleyin**: seçin ve 'TCP işlem sunucusundan Azure depolama blobu URL'si bağlantısı olup olmadığını görmek için bağlantıları' görüntülemek için cbengine.exe denetleyin.

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

Gelen son CBEngineCurr.errlog açın `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` araması: 443 ve bağlantı denemesi başarısız oldu.

![Çoğaltmayı etkinleştirme](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

Sorun varsa, işlem sunucusu komut satırından, bağlantı noktası 443 aracılığıyla CBEngineCurr.currLog içinde bulundu (içinde görüntü maskelenmiş), Azure genel IP adresine ping atmayı telnet kullanın.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Bağlanmak erişemiyorsanız, güvenlik duvarı veya Proxy sonraki adımda açıklandığı gibi nedeniyle erişim sorunu olup olmadığını denetleyin.


* **İşlem sunucusu üzerindeki IP adresi tabanlı güvenlik duvarı erişimi engellemediğini varsa denetleyin**: sunucu üzerinde bir IP adresi tabanlı güvenlik duvarı kuralları kullanıyorsanız, ardından Microsoft Azure veri merkezi IP aralıkları tam listesini indirin [burada](https://www.microsoft.com/download/details.aspx?id=41653) ve bunları Azure (ve HTTPS (443) bağlantı noktası) ile iletişim sağlarlar emin olmak için Güvenlik Duvarı'nı yapılandırmanıza ekleyin.  Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.

* **URL tabanlı güvenlik duvarı işlem sunucusu üzerindeki erişimi engellemediğini varsa denetleyin**: sunucuda bir URL tabanlı güvenlik duvarı kuralları kullanıyorsanız, aşağıdaki URL'ler için güvenlik duvarı yapılandırması eklendi olduğundan emin olun.

  `*.accesscontrol.windows.net:` Erişim denetimi ve kimlik yönetimi için kullanılır

  `*.backup.windowsazure.com:` Çoğaltma veri aktarımı ve düzenlemesi için kullanılır

  `*.blob.core.windows.net:` Çoğaltılan verileri depolayan depolama hesabına erişim için kullanılır

  `*.hypervrecoverymanager.windowsazure.com:` Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır

  `time.nist.gov` ve `time.windows.com`: sistem ile genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.

URL'ler için **Azure kamu Bulutu**:

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us`

* **İşlem sunucusu üzerindeki ara sunucu ayarları erişim unsur olmadığını kontrol**.  Bir ara sunucu kullanıyorsanız, proxy sunucusu adı DNS sunucusu tarafından çözüyor emin olun.
Yapılandırma sunucusu Kurulum zamanında sağlanan denetlemek için. Kayıt defteri anahtarına gidin

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Artık aynı ayarları Azure Site Recovery aracısı tarafından veri göndermek için kullanıldığından emin olun.
Arama Microsoft Azure yedekleme

Açın ve eylemini tıklatın > özelliklerini değiştirme. Proxy yapılandırma sekmesi altında kayıt defteri ayarları tarafından gösterildiği gibi aynı olmalıdır proxy adresi görmeniz gerekir. Aksi halde, lütfen aynı adrese değiştirin.


* **Kısıtlama bant genişliği işlem sunucusu üzerindeki sınırlı değildir, kontrol**: bant genişliğini artırın ve sorunu hala mevcut olup olmadığını denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız olursa, ardından sorgunuza sonrası [Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Etkin bir topluluk sunuyoruz ve biri, Mühendislerimiz yardımcı olması mümkün olacaktır.
