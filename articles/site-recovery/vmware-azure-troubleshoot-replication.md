---
title: "VMware sanal ve fiziksel sunucu çoğaltma Azure Site Recovery ile azure'a çoğaltma sorunlarını giderme | Microsoft Docs"
description: "Bu makale, Azure Site Recovery ile azure'a VMware Vm'lerini ve fiziksel sunucu çoğaltırken için ortak çoğaltma sorunlarını giderme sağlar."
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: asgang
ms.openlocfilehash: 9291840428c9a8d7ba5d65bc94ce5964728316f3
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>VMware Vm'lerini ve fiziksel sunucuları için çoğaltma sorunlarını giderme

OU, VMware sanal makineleri veya fiziksel sunucuları Azure Site RECOVERY'yi kullanarak korurken belirli bir hata iletisi alabilirsiniz. Bu makalede çoğaltma VMware Vm'lerini ve fiziksel sunucuları Azure kullanarak şirket içi, karşılaşabileceğiniz bazı yaygın sorunlar [Azure Site Recovery](site-recovery-overview.md).

## <a name="initial-replication-issues"></a>İlk çoğaltma sorunlarını.

Çoğu durumda, kaynak sunucu işlemi sunucu veya işlem sunucusu Azure arasında bağlantı sorunları sizi desteğe karşılaştığınız ilk çoğaltma hatalarını kaynaklanır. Çoğu durumda, aşağıda listelenen adımları izleyerek bu sorunları giderebilirsiniz.

### <a name="verify-the-source-machine"></a>Kaynak makine doğrulayın
* Kaynak sunucu makine komut satırından Telnet https bağlantı noktası (varsayılan 9443) işlem sunucusuyla herhangi bir ağ bağlantısı sorunları veya güvenlik duvarı bağlantı noktası engelleme sorunları olup olmadığını görmek için aşağıda gösterildiği gibi ping işlemi yapmak için kullanın.

    `telnet <PS IP address> <port>`
> [!NOTE]
    > Telnet kullanın, PING bağlantısını test etme için kullanmayın.  Telnet yüklü değilse, adımları listesini izleyen [burada](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Alınamazsa bağlanmak, bağlantı noktasının 9443 işlem sunucusundaki izin ve sorun hala çıkar olmadığını denetleyin. İşlem sunucusu bu soruna neden olan DMZ olduğu bazı durumlarda açıldı.

* Hizmetinin durumunu denetlemek `InMage Scout VX Agent – Sentinel/OutpostStart` sorun devam ederse çalıştığından ve onay değilse.   

## <a name="verify-the-process-server"></a>İşlem sunucusu doğrulayın

* **İşlem sunucusu etkin olarak veri Azure'a dağıtmaya olmadığını denetleyin**

İşlem sunucusu makineden Görev Yöneticisi'ni (Ctrl-Shift-Esc tuşuna basın) açın. Performans sekmesine gidin ve 'Açık Kaynak İzleyicisi' bağlantısını tıklatın. Kaynak Yöneticisi'nden ağ sekmesine gidin. Cbengine.exe 'Ağ etkinliği işlemlerle' ın büyük miktarda veri (MB) cinsinden etkin olarak gönderme olmadığını denetleyin.

![Çoğaltmayı etkinleştirme](./media/vmware-azure-troubleshoot-replication/cbengine.png)

Aksi halde, aşağıda listelenen adımları izleyin:

* **İşlem sunucusu Azure Blob bağlanabiliyor olup olmadığını denetle**: seçin ve 'TCP Azure Storage blobu URL işlem sunucusu bağlantısını olup olmadığını görmek için bağlantıları' görüntülemek için cbengine.exe denetleyin.

![Çoğaltmayı etkinleştirme](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

Ardından gidin, Denetim Masası'na > Hizmetleri, aşağıdaki hizmetleri hazır ve çalışır olup olmadığını denetleyin:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     *
(Re) Sorun devam ederse çalışmıyor hizmeti ve onay başlatın.

* **İşlem sunucusu bağlantı noktası 443 aracılığıyla Azure genel IP adresine bağlanabiliyor olup olmadığını denetleyin**

En son CBEngineCurr.errlog gelen açmak `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` arayın ve: 443 ve bağlantı girişimi başarısız oldu.

![Çoğaltmayı etkinleştirme](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

Sorun varsa, işlem sunucusunu komut satırından 443 numaralı bağlantı noktasını kullanarak CBEngineCurr.currLog içinde bulundu (içinde görüntünün üzerinde maskelenmiş), Azure genel IP adresine ping işlemi telnet kullanın.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Bağlanma sorunu, güvenlik duvarı veya bir sonraki adımda açıklandığı gibi Proxy nedeniyle erişim sorunu olup olmadığını denetleyin.


* **IP adresi tabanlı Güvenlik Duvarı'nı işlem sunucusu erişimi engellemediğini varsa denetleyin**: sunucu üzerinde bir IP adresi tabanlı güvenlik duvarı kuralları kullanıyorsanız, Microsoft Azure veri merkezi IP aralıkları tam listesi karşıdan [burada](https://www.microsoft.com/download/details.aspx?id=41653) ve bunları Azure (ve HTTPS (443 numaralı) bağlantı noktası) iletişimi sağlarlar emin olmak için güvenlik duvarı yapılandırmanızı ekleyin.  Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.

* **İşlem sunucusu URL'si tabanlı güvenlik duvarının erişimi engellemediğini varsa denetleyin**: sunucuda URL tabanlı güvenlik duvarı kuralları kullanıyorsanız aşağıdaki URL'ler için güvenlik duvarı yapılandırmasını eklenmiş olduğundan emin olun.

  `*.accesscontrol.windows.net:` Erişim denetimi ve kimlik yönetimi için kullanılır

  `*.backup.windowsazure.com:` Çoğaltma veri aktarımı ve düzenlemesi için kullanılır

  `*.blob.core.windows.net:` Erişim depoları veri kopyalandığı depolama hesabı için kullanılan

  `*.hypervrecoverymanager.windowsazure.com:` Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır

  `time.nist.gov` ve `time.windows.com`: Sistem ve genel saat arasındaki zaman eşitlemesini denetlemek için kullanılır.

URL'ler için **Azure Bulutu**:

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us`

* **İşlem sunucusundaki proxy ayarlarını erişim engellemediğinden varsa denetleyin**.  Bir Proxy sunucu kullanıyorsanız, proxy sunucu adı DNS sunucusu tarafından çözülürken emin olun.
Yapılandırma sunucusu Kurulum sırasında sağlanan denetlemek için. Kayıt defteri anahtarına gidin

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Şimdi aynı ayarları Azure Site Recovery aracısı tarafından veri göndermek için kullanıldığından emin olun.
Arama Microsoft Azure yedekleme

Bunu açıp eylemini tıklatın > özelliklerini değiştir. Proxy Yapılandırması sekmesinde, kayıt defteri ayarları tarafından gösterildiği gibi aynı olması gerekir proxy adresi görmeniz gerekir. Aksi durumda, lütfen aynı adrese değiştirin.


* **Bant genişliği azaltma işlemi sunucu üzerinde sınırlı değildir, denetleme**: bant genişliğini artırmak ve sorunu hala olup olmadığını kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma gereksinim duyarsanız, sorgunuza sonrası [Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Etkin bir topluluk sahibiz ve bizim mühendisleri biri size yardımcı olmak mümkün olacaktır.
