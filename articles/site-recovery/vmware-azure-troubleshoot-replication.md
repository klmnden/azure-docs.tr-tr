---
title: Azure Site Recovery kullanarak VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için çoğaltma sorunlarını giderme | Microsoft Docs
description: Bu makalede VMware vm'lerinin olağanüstü durum kurtarma sırasında yaygın çoğaltma sorunlarına yönelik sorun giderme bilgileri ve fiziksel sunucuları Azure'a Azure Site Recovery kullanarak sağlar.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: c53dc81da9469c0628adbd3751dc818997fa4d05
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063687"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>VMware Vm'lerini ve fiziksel sunucular için çoğaltma sorunlarını giderme

Azure Site Recovery kullanarak VMware sanal makineleri veya fiziksel sunucuları koruduğunuzda, belirli bir hata iletisi görebilirsiniz. Bu makalede, şirket içi VMware Vm'leri ve fiziksel sunucuları Azure'a kullanarak çoğaltma yaptığınızda çalıştırdığınızca bazı yaygın sorunlar [Site Recovery](site-recovery-overview.md).

## <a name="initial-replication-issues"></a>İlk çoğaltma sorunları

İlk çoğaltma hatalarını genellikle işlem sunucusu ile Azure arasında veya kaynak sunucu ile işlem sunucusu arasında bağlantı sorunları nedeniyle oluşup. Çoğu durumda, aşağıdaki bölümlerde yer alan adımları tamamlayarak bu sorunları giderebilirsiniz.

### <a name="check-the-source-machine"></a>Kaynak makine denetleyin

Kaynak makine denetleyebilirsiniz aşağıdaki listeyi gösterir yolları:

*  Kaynak sunucuda komut satırında aşağıdaki komutu çalıştırarak HTTPS bağlantı noktası (varsayılan HTTPS bağlantı noktası 9443 olduğu) ile işlem sunucusu ping Telnet kullanın. Komut ve sorunları o blok güvenlik duvarı bağlantı noktası için ağ bağlantısı sorunları denetler.


   `telnet <process server IP address> <port>`


   > [!NOTE]
   > Telnet bağlantısını test etmek için kullanın. Kullanmayın `ping`. Telnet yüklü değilse, listelenen adımları tamamlamak [Telnet istemcisi yükleme](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx).

   İşlem sunucusuna bağlanamıyorsa, işlem sunucusunun gelen bağlantı noktası 9443'e izin verin. Örneğin, bir çevre ağına ağınız varsa, işlem sunucusunun gelen bağlantı noktası 9443'e izin vermeniz gereken veya denetimli alt ağ. Ardından, sorunun nerede oluştuğunu görmek için kontrol edin.

*  Durumunu **Inmage Scout VX Aracısı-Sentinel/OutpostStart** hizmeti. Hizmet çalışmıyorsa, hizmeti başlatın ve sorunun nerede oluştuğunu görmek için kontrol edin.   

### <a name="check-the-process-server"></a>İşlem sunucusu denetleyin

İşlem sunucusu denetleyebilirsiniz aşağıdaki listeyi gösterir yolları:

*  **İşlem sunucusu etkin bir şekilde veri Azure'a gönderme olup olmadığını denetleyin**.

   1. İşlem sunucusu, Görev Yöneticisi'ni (Ctrl + Shift + Esc tuşlarına basın) açın.
   2. Seçin **performans** sekmesine tıklayın ve ardından **açık Kaynak İzleyicisi** bağlantı. 
   3. Üzerinde **Kaynak İzleyicisi** sayfasında **ağ** sekmesi. Altında **ağ etkinliği işlemlerle**, kontrol olup olmadığını **cbengine.exe** etkin bir şekilde büyük miktarlarda veri gönderiyor.

        ![Ağ etkinliği ile işlem birimlerini gösteren ekran görüntüsü](./media/vmware-azure-troubleshoot-replication/cbengine.png)

   Büyük bir veri hacmi cbengine.exe gönderme değil, aşağıdaki bölümlerde yer alan adımları tamamlayın.

*  **İşlem sunucusu Azure Blob depolama alanına bağlanıp bağlanamadığınızı denetleyin**.

   Seçin **cbengine.exe**. Altında **TCP bağlantılarını**, Azure blogunda depolama URL'si işlem sunucusundan bağlantı olup olmadığını denetleyin.

   ![Cbengine.exe ve Azure Blob Depolama URL'si arasındaki bağlantıları gösteren ekran görüntüsü](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

   İşlem sunucusundan bağlantı Denetim Masası ' nda Azure blogunda depolama URL'sine yoksa seçin **Hizmetleri**. Aşağıdaki hizmetlerin çalışır durumda olup olmadığını denetleyin:

   *  cxprocessserver
   *  Inmage Scout VX Aracısı-Sentinel/Outpost
   *  Microsoft Azure Kurtarma Hizmetleri Aracısı
   *  Microsoft Azure Site Recovery Hizmeti
   *  tmansvc

   Veya çalışmadığından herhangi bir hizmeti yeniden başlatın. Sorunun nerede oluştuğunu görmek için kontrol edin.

*  **İşlem sunucusu bağlantı noktası 443'ü kullanarak Azure genel IP adresine bağlanıp bağlanamadığınızı denetleyin**.

   %ProgramFiles%\Microsoft Azure kurtarma Hizmetleri Agent\Temp, en son CBEngineCurr.errlog dosyasını açın. Dosyada arayın **443** veya dizesi için **başarısız bağlantı denemesi**.

   ![Hata gösteren ekran görüntüsü Temp klasörü günlüğe kaydeder.](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

   Sorunları gösteriliyorsa işlem sunucusu, komut satırında (IP adresi, önceki görüntüde maskelenir), Azure genel IP adresine ping atmayı Telnet kullanın. Azure genel IP adresi, bağlantı noktası 443'ü kullanarak, bir CBEngineCurr.currLog dosyasında bulabilirsiniz:

   `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

   Bağlanamıyorsanız, erişim sorununu sonraki adımda açıklandığı gibi güvenlik duvarı veya proxy ayarları nedeniyle olup olmadığını denetleyin.

*  **IP adresi tabanlı güvenlik duvarı işlem sunucusu üzerindeki erişimi engeller olup olmadığını denetleyin**.

   Sunucuda IP adresi tabanlı güvenlik duvarı kurallarını kullanırsanız, tam listesini indirin [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Güvenlik Duvarı yapılandırmanızda Güvenlik Duvarı'nın azure'a (ve varsayılan HTTPS bağlantı noktası, 443) iletişimine izin verdiğinden emin olmak için IP adresi aralıklarını ekleyin. (Erişim denetimi ve kimlik yönetimi için kullanılır) Azure Batı ABD bölgesinde ve aboneliğinizin Azure bölgesi için IP adresi aralıklarına izin verin.

*  **İşlem sunucusu üzerindeki URL tabanlı bir güvenlik duvarı erişimi engelliyor olup olmadığını denetleyin**.

   Sunucuda bir URL tabanlı güvenlik duvarı kuralı kullanırsanız, güvenlik duvarı yapılandırması için aşağıdaki tabloda listelenen URL'leri ekleyin:

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

*  **İşlem sunucusu üzerindeki ara sunucu ayarları erişimi engellemek olup olmadığını denetleyin**.

   Bir ara sunucu kullanıyorsanız, proxy sunucusu adı DNS sunucusu tarafından çözümlenir emin olun. Yapılandırma sunucusunu ayarladıktan sağladığınız değeri denetleyin, kayıt defteri anahtarına gidin **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.

   Ardından, veri göndermek için aynı ayarları Azure Site Recovery aracısı tarafından kullanıldığından emin olun: 
      
   1. Arama **Microsoft Azure yedekleme**. 
   2. Açık **Microsoft Azure Backup**ve ardından **eylem** > **özelliklerini değiştirme**. 
   3. Üzerinde **Proxy Yapılandırması** sekmesinde proxy adresi görmeniz gerekir. Proxy adresi kayıt defteri ayarlarında gösterilen proxy adresi ile aynı olmalıdır. Aksi durumda, aynı adrese değiştirin.

*  **İşlem sunucusu üzerindeki bant genişliğini kısıtlama kısıtlı olup olmadığını denetleyin**.

   Bant genişliğini artırın ve ardından Sorun oluşmaya devam edip etmediğini denetleyin.

## <a name="source-machine-isnt-listed-in-the-azure-portal"></a>Kaynak makine Azure Portalı'nda listelenmez

Site RECOVERY'yi kullanarak çoğaltmayı etkinleştirmek için kaynak makine seçmeye çalıştığınızda, makine aşağıdaki nedenlerden biri için kullanılabilir olmayabilir:

*  VCenter altında iki sanal makine aynı örneğini UUID varsa, Azure portalında ilk sanal makine yapılandırma sunucusu tarafından bulunan gösterilir. Bu sorunu çözmek için iki sanal makine aynı örneğini UUID sahip olun.
*  OVF şablonu veya birleşik Kurulumu kullanarak yapılandırma sunucusunu ayarladıktan ayarladığınızda, doğru vCenter kimlik bilgilerini emin olun. Kurulum sırasında eklenen kimlik bilgilerini doğrulamak için bkz: [otomatik bulma için kimlik bilgilerini değiştirme](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
*  VCenter erişmek için sağlanan izinler gerekli izinlere sahip değilseniz, sanal makineleri keşfetmek için hatası gerçekleşebilir. İçinde açıklanan izinleri olun [otomatik bulma için bir hesap hazırlama](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) vCenter kullanıcı hesabına eklenir.
*  Site Recovery sanal makine zaten korunuyorsa, sanal makine koruma portalında seçmek kullanılamaz. Portalda aradığınız sanal makineyi başka bir kullanıcı tarafından veya farklı bir abonelikte zaten korunmuyor emin olun.

## <a name="protected-virtual-machines-arent-available-in-the-portal"></a>Korunan sanal makinelerin portalda kullanılamaz

Sistemdeki yinelenen girişler varsa altında Site Recovery çoğaltılan sanal makineler, Azure portalında mevcut değildir. Eski girişleri silmek ve sorunu çözmek öğrenmek için bkz: [Azure Site Recovery VMware-Azure: Yinelenen veya eski girdilerin temizlenmesini nasıl](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma ihtiyacınız varsa, sorunuzu gönderin [Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Etkin bir topluluk sunuyoruz ve biri, Mühendislerimiz yardımcı olabilir.
