---
title: "VMware/fiziksel Azure koruması hatalarını giderme | Microsoft Docs"
description: "Bu makalede ortak VMware makine çoğaltma hataları ve bunları ile ilgili sorunları giderme"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/26/2017
ms.author: asgang
ms.openlocfilehash: 6ebec2e06566b1e2d6834fdd81c0d8b2801b80b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a>Şirket içi VMware/fiziksel sunucu çoğaltma sorunlarını giderme
VMware sanal makineleri veya fiziksel sunucuları Azure Site RECOVERY'yi kullanarak korurken, belirli bir hata iletisi alabilirsiniz. Bu makalede bazı karşılaştı, sorun giderme bunları gidermek için adımları yanı sıra daha sık karşılaşılan hata iletileri ayrıntılarını verir.


## <a name="initial-replication-is-stuck-at-0"></a>İlk çoğaltma %0 takıldı
Kaynak sunucu işlemi sunucu veya işlem sunucusu Azure arasında bağlantı sorunları sizi desteğe karşılaştığınız ilk çoğaltma hataların çoğu kaynaklanır.
Çoğu durumda, kendi kendini yapabilecekleriniz aşağıda listelenen adımları izleyerek bu sorunları giderme.

###<a name="check-the-following-on-source-machine"></a>Kaynak makinede aşağıdakileri denetleyin
* Kaynak sunucu makine komut satırından Telnet https bağlantı noktası (varsayılan 9443) işlem sunucusuyla herhangi bir ağ bağlantısı sorunları veya güvenlik duvarı bağlantı noktası engelleme sorunları olup olmadığını görmek için aşağıda gösterildiği gibi ping işlemi yapmak için kullanın.
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > Telnet kullanın, PING bağlantısını test etme için kullanmayın.  Telnet yüklü değilse, adımları listesini izleyen [burada](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Alınamazsa bağlanmak, bağlantı noktasının 9443 işlem sunucusundaki izin ve sorun hala çıkar olmadığını denetleyin. İşlem sunucusu bu soruna neden olan DMZ olduğu bazı durumlarda açıldı.

* Hizmetinin durumunu denetlemek `InMage Scout VX Agent – Sentinel/OutpostStart` sorun devam ederse çalıştığından ve onay değilse.   
 
###<a name="check-the-following-on-process-server"></a>İŞLEM sunucusunda aşağıdakileri denetleyin

* **İşlem sunucusu etkin olarak veri Azure'a dağıtmaya olmadığını denetleyin** 

İşlem sunucusu makineden Görev Yöneticisi'ni (Ctrl-Shift-Esc tuşuna basın) açın. Performans sekmesine gidin ve 'Açık Kaynak İzleyicisi' bağlantısını tıklatın. Kaynak Yöneticisi'nden ağ sekmesine gidin. Cbengine.exe 'Ağ etkinliği işlemlerle' ın büyük miktarda veri (MB) cinsinden etkin olarak gönderme olmadığını denetleyin.

![Çoğaltmayı etkinleştirme](./media/site-recovery-protection-common-errors/cbengine.png)

Aksi halde aşağıda listelenen adımları izleyin:

* **İşlem sunucusu Azure Blob bağlanabiliyor olup olmadığını denetle**: seçin ve 'TCP Azure Storage blobu URL işlem sunucusu bağlantısını olup olmadığını görmek için bağlantıları' görüntülemek için cbengine.exe denetleyin.

![Çoğaltmayı etkinleştirme](./media/site-recovery-protection-common-errors/rmonitor.png)

Ardından gidin, Denetim Masası'na > Hizmetleri, aşağıdaki hizmetleri hazır ve çalışır olup olmadığını denetleyin:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
(Re) Hangi çalışmıyor herhangi bir hizmeti başlatın ve sorunu hala olup olmadığını kontrol edin.

* **İşlem sunucusu bağlantı noktası 443 aracılığıyla Azure genel IP adresine bağlanabiliyor olup olmadığını denetleyin**

En son CBEngineCurr.errlog gelen açmak `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` arayın ve: 443 ve bağlantı girişimi başarısız oldu.

![Çoğaltmayı etkinleştirme](./media/site-recovery-protection-common-errors/logdetails1.png)

Sorun varsa, işlem sunucusunu komut satırından 443 numaralı bağlantı noktasını kullanarak CBEngineCurr.currLog içinde bulundu (içinde görüntünün üzerinde maskelenmiş), Azure genel IP adresine ping işlemi telnet kullanın.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Bağlanma sorunu, güvenlik duvarı veya bir sonraki adımda açıklandığı gibi Proxy nedeniyle erişim sorunu olup olmadığını denetleyin.


* **IP adresi tabanlı Güvenlik Duvarı'nı işlem sunucusu engellemediğini varsa erişimi denetleme**: sunucu üzerinde bir IP adresi tabanlı güvenlik duvarı kuralları kullanıyorsanız, Microsoft Azure veri merkezi IP aralıkları tam listesi karşıdan [burada](https://www.microsoft.com/download/details.aspx?id=41653) ve bunları Azure (ve HTTPS (443 numaralı) bağlantı noktası) iletişimi sağlarlar emin olmak için güvenlik duvarı yapılandırmanızı ekleyin.  Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.

* **İşlem sunucusu URL'si tabanlı güvenlik duvarının erişimi engellemediğini varsa denetleyin**: sunucu üzerinde temel URL güvenlik duvarı kuralları kullanıyorsanız, aşağıdaki URL'ler için güvenlik duvarı yapılandırmasını eklenir olduğundan emin olun. 
     
  `*.accesscontrol.windows.net:` Erişim denetimi ve kimlik yönetimi için kullanılır

  `*.backup.windowsazure.com:` Çoğaltma veri aktarımı ve düzenlemesi için kullanılır

  `*.blob.core.windows.net:`Erişim depoları veri kopyalandığı depolama hesabı için kullanılan

  `*.hypervrecoverymanager.windowsazure.com:` Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır

  `time.nist.gov`ve `time.windows.com`: Sistem ve genel saat arasındaki zaman eşitlemesini denetlemek için kullanılır.

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

![Çoğaltmayı etkinleştirme](./media/site-recovery-protection-common-errors/mab.png)

Bunu açıp eylemini tıklatın > özelliklerini değiştir. Proxy Yapılandırması sekmesinde, kayıt defteri ayarları tarafından gösterildiği gibi aynı olması gerekir proxy adresi görmeniz gerekir. Aksi durumda, lütfen aynı adrese değiştirin.

![Çoğaltmayı etkinleştirme](./media/site-recovery-protection-common-errors/mabproxy.png)

* **Bant genişliği azaltma işlemi sunucu üzerinde sınırlı değildir, denetleme**: bant genişliğini artırmak ve sorunu hala olup olmadığını kontrol edin.

##<a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma gereksinim duyarsanız, sorgunuza sonrası [ASR Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Etkin bir topluluk sahibiz ve bizim mühendisleri biri size yardımcı olmak mümkün olacaktır.