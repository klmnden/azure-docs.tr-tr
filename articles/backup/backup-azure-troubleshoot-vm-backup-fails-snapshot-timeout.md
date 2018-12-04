---
title: 'Azure Backup hatalarında sorunları giderme: Konuk Aracısı durumu kullanılamıyor'
description: Belirtiler, nedenleri ve çözümleri Aracısı, uzantı ve disk için ilgili Azure Backup hatası sayısı.
services: backup
author: genlin
manager: cshepard
keywords: Azure yedekleme; VM Aracısı; Ağ bağlantısı;
ms.service: backup
ms.topic: troubleshooting
ms.date: 12/03/2018
ms.author: genli
ms.openlocfilehash: 9f26a51a8da2c3fec3ff180dbc8c8de08bb0a93a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52833882"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Azure Backup hatalarında sorunları giderme: aracı veya uzantı ile ilgili sorunlar

Bu makale yardımcı olacak sorun giderme adımlarını uzantısı ve VM Aracısı ile iletişim için ilgili Azure Backup hataları gidermek sağlar.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable - VM Aracısı Azure Backup ile iletişim kuramıyor

**Hata kodu**: UserErrorGuestAgentStatusUnavailable <br>
**Hata iletisi**: VM Aracısı Azure Backup ile iletişim kuramıyor<br>

Kaydolun ve bir VM yedekleme hizmeti için zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM Aracısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez, yedekleme başarısız olabilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:<br>
**1. neden: [aracı VM ancak onun içinde yanıt vermeyen (Windows VM'ler için) yüklenir](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2. neden: [VM'e yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**4. neden: [güncelleştirmek veya yüklemek yedekleme uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)**  
**5. neden: [VM internet erişimi yok](#the-vm-has-no-internet-access)**

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>Anlık görüntü durumu için VM aracısıyla GuestAgentSnapshotTaskStatusError - geçemedi

**Hata kodu**: GuestAgentSnapshotTaskStatusError<br>
**Hata iletisi**: anlık görüntü durumu için VM aracısıyla iletişim kurulamadı <br>

Kaydolun ve bir VM için Azure Backup hizmeti zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM yedekleme uzantısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez yedekleme hatası meydana gelebilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:  
**1. neden: [aracı VM ancak onun içinde yanıt vermeyen (Windows VM'ler için) yüklenir](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**2. neden: [VM'e yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [VM internet erişimi yok](#the-vm-has-no-internet-access)**

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>UserErrorRpCollectionLimitReached - geri yükleme noktası koleksiyonu en yüksek sınırına ulaştı

**Hata kodu**: UserErrorRpCollectionLimitReached <br>
**Hata iletisi**: geri yükleme noktası koleksiyonu en yüksek sınırına ulaştı. <br>
* Otomatik temizleme kurtarma noktasının önleme kurtarma noktası kaynak grubu üzerinde bir kilit ise bu sorun oluşabilir.
* Bu sorun ayrıca birden çok yedekleme günde tetiklenen oluşabilir. RPs 7 gün boyunca bekletilir anlık olarak günde yalnızca bir yedekleme şu anda öneririz ve 18 yalnızca anlık RPs herhangi bir zamanda bir VM ile ilişkili olabilir. <br>

Önerilen eylem:<br>
Bu sorunu çözmek için kaynak grubu üzerindeki kilidi kaldırın ve temizleme tetiklemek için işlemi yeniden deneyin.

> [!NOTE]
    > Yedekleme hizmeti, geri yükleme noktası koleksiyonu depolamak için sanal makinenin kaynak grubundan ayrı bir kaynak grubu oluşturur. Müşterilerin, Backup hizmeti tarafından kullanım için oluşturduğunuz kaynak grubunda değil kilitlemek için önerilir. Backup hizmeti tarafından oluşturulan kaynak grubu adlandırma biçimi: AzureBackupRG_`<Geo>`_`<number>` örn: AzureBackupRG_northeurope_1

**1. adım: [kilit geri yükleme noktası kaynak grubundan Kaldır](#remove_lock_from_the_recovery_point_resource_group)** <br>
**2. adım: [geri yükleme noktası koleksiyonunu Temizle](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured - yedekleme, şifrelenmiş VM'lerin anahtar kasasına yedekleme için yeterli izinlere sahip değil.

**Hata kodu**: UserErrorKeyvaultPermissionsNotConfigured <br>
**Hata iletisi**: yedekleme, şifrelenmiş VM'lerin anahtar kasasına yedekleme için yeterli izinlere sahip değil. <br>

Yedekleme işleminin şifrelenmiş VM'ler üzerinde başarılı olması için bu anahtar kasasına erişmek için izinleri olmalıdır. Bu yapılabilir kullanarak [Azure portalında](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption#provide-permissions-to-backup) aracılığıyla veya [PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection)

## <a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork - anlık görüntü işlemi, sanal makinede ağ bağlantısı olmaması nedeniyle başarısız oldu

**Hata kodu**: ExtensionSnapshotFailedNoNetwork<br>
**Hata iletisi**: anlık görüntü işlemi sanal makinede ağ bağlantısı olmaması nedeniyle başarısız oldu<br>

Kaydolun ve bir VM için Azure Backup hizmeti zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM yedekleme uzantısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez yedekleme hatası meydana gelebilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:    
**1. neden: [anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**2. neden: [güncelleştirmek veya yüklemek yedekleme uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)**  
**3. neden: [VM internet erişimi yok](#the-vm-has-no-internet-access)**

## <a name="ExtentionOperationFailed-vmsnapshot-extension-operation-failed"></a>ExtentionOperationFailedForManagedDisks - VMSnapshot uzantısı işlemi başarısız oldu

**Hata kodu**: ExtentionOperationFailedForManagedDisks <br>
**Hata iletisi**: VMSnapshot uzantısı işlemi başarısız oldu<br>

Kaydolun ve bir VM için Azure Backup hizmeti zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM yedekleme uzantısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez yedekleme hatası meydana gelebilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:  
**1. neden: [anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**2. neden: [güncelleştirmek veya yüklemek yedekleme uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)**  
**3. neden: [aracı VM ancak onun içinde yanıt vermeyen (Windows VM'ler için) yüklenir](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**4. neden: [VM'e yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed / BackUpOperationFailedV2 - yedekleme başarısız olursa bir iç hata ile

**Hata kodu**: BackUpOperationFailed / BackUpOperationFailedV2 <br>
**Hata iletisi**: yedekleme bir iç hata ile başarısız oldu - Lütfen işlemi birkaç dakika içinde yeniden deneyin <br>

Kaydolun ve bir VM için Azure Backup hizmeti zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM yedekleme uzantısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez yedekleme hatası meydana gelebilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:  
**1. neden: [VM ancak bunu yüklü Aracı (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**2. neden: [VM'e yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**4. neden: [güncelleştirmek veya yüklemek yedekleme uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)**  
**5. neden: [Backup hizmeti, bir kaynak grubu kilidi nedeniyle eski geri yükleme noktalarını silme izni yok](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)** <br>
**6. neden: [VM internet erişimi yok](#the-vm-has-no-internet-access)**

## <a name="usererrorunsupporteddisksize---currently-azure-backup-does-not-support-disk-sizes-greater-than-1023gb"></a>UserErrorUnsupportedDiskSize - şu anda Azure Backup 1023 GB'den büyük disk boyutlarını desteklemez

**Hata kodu**: UserErrorUnsupportedDiskSize <br>
**Hata iletisi**: şu anda Azure Backup 1023 GB'den büyük disk boyutlarını desteklemez <br>

Kasanız, Azure VM yedekleme yığını v2'ye yükseltilmemiş olduğundan disk boyutu 1023 GB'tan büyük ile VM'yi yedeklerken, yedekleme işlemi başarısız olabilir. Yükseltme için Azure VM yedekleme yığını V2 sağlayacak 4 TB'a kadar destekler. Bu gözden [avantajları](backup-upgrade-to-vm-backup-stack-v2.md), [konuları](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade)ve ardından izleyerek yükseltmeye devam edin [yönergeleri](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).  

## <a name="usererrorstandardssdnotsupported---currently-azure-backup-does-not-support-standard-ssd-disks"></a>Standart SSD disk şu anda Azure Backup UserErrorStandardSSDNotSupported - desteklemiyor

**Hata kodu**: UserErrorStandardSSDNotSupported <br>
**Hata iletisi**: şu anda Azure Backup, standart SSD disk desteklemiyor <br>

Şu anda Azure Backup, Azure VM yedekleme yığını v2'ye yükseltilmiş kasaları için standart SSD diskleri destekler. Bu gözden [avantajları](backup-upgrade-to-vm-backup-stack-v2.md), [konuları](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade)ve ardından izleyerek yükseltmeye devam edin [yönergeleri](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).


## <a name="causes-and-solutions"></a>Nedenler ve çözümler

### <a name="the-vm-has-no-internet-access"></a>VM internet erişimi yok
Dağıtım gereksinim, VM, internet erişimi yok. Veya, Azure altyapısı için erişimi engelleyen kısıtlamalar olabilir.

Düzgün çalışması için yedekleme uzantısını Azure genel IP adreslerine bağlantısı gerektirir. Uzantı komutları VM anlık görüntülerini yönetmek için bir Azure depolama uç noktasına (HTTPs URL'si) gönderir. Uzantı genel internet erişimi yoksa, yedekleme sonunda başarısız olur.

Sanal makine trafiği yönlendirmek için bir ara sunucusunu dağıtmak mümkündür.
##### <a name="create-a-path-for-https-traffic"></a>HTTPs trafiği için bir yol oluşturma

1. Ağ kısıtlamaları (örneğin, bir ağ güvenlik grubu) yerinde varsa, trafiği yönlendirmek için bir HTTPs proxy sunucusu dağıtın.
2. Erişim HTTPs Ara sunucunun internet'e izin vermek için kurallar varsa ağ güvenlik grubuna ekleyin.

VM yedeklemeleri için bir HTTPs proxy'si kurma hakkında bilgi edinmek için bkz: [Azure sanal makinelerini yedeklemek için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Yedeklenen sanal makine ya da proxy sunucusu üzerinden trafik yönlendirilir Azure genel IP adreslerine erişim gerektirir.

####  <a name="solution"></a>Çözüm
Sorunu çözmek için aşağıdaki yöntemlerden birini deneyin:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Bölgeyi karşılık gelen bir Azure depolama alanına erişime izin ver

Kullanabileceğiniz [hizmet etiketleri](../virtual-network/security-overview.md#service-tags) belirli bir bölgenin depolama bağlantılara izin vermek için. Depolama hesabına erişime izin veren kuralın kural daha yüksek önceliğe söz konusu bloklar internet erişimi olduğundan emin olun.

![Bir bölge için depolama etiketlere sahip ağ güvenlik grubu](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Hizmet etiketleri yapılandırmak için adım adım yordam anlamak için izleyin [bu videoyu](https://youtu.be/1EjLQtbKm1M).

> [!WARNING]
> Depolama hizmet etiketleri Önizleme aşamasındadır. Bunlar yalnızca belirli bölgelerde kullanılabilir. Bölgelerin bir listesi için bkz. [hizmet etiketleri depolama](../virtual-network/security-overview.md#service-tags).

Azure yönetilen diskler kullanıyorsanız, güvenlik duvarları hakkında ek bağlantı noktası açma (bağlantı noktası 8443) gerekebilir.

Ayrıca, giden Internet trafiği için bir yol alt ağınız yoksa, kendi alt ağına hizmet etiketi "Microsoft.Storage" olan bir hizmet uç noktası eklemeniz gerekir.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>VM Aracısı yüklendi, ancak (Windows VM'ler için) yanıt vermiyor

#### <a name="solution"></a>Çözüm
VM Aracısı bozulduysa veya hizmet durdurulmuş. VM aracısını yeniden yüklemeyi en son sürümü Al yardımcı olur. Ayrıca, hizmeti ile iletişimi yeniden yardımcı olur.

1. Windows Azure Konuk aracı hizmetini VM Hizmetleri (services.msc) çalışıp çalışmadığını belirleyin. Windows Azure Konuk aracı hizmetini yeniden başlatın ve yedeklemeyi başlatın deneyin.    
2. Windows Azure Konuk aracı hizmetini Hizmetleri'nde, Denetim Masası ' nda görünür değilse Git **programlar ve Özellikler** Windows Azure Konuk aracısı yüklü olup olmadığını belirlemek için.
4. Windows Azure Konuk Aracısı görünürse **programlar ve Özellikler**, Windows Azure Konuk Aracısı'nı kaldırın.
5. İndirme ve yükleme [Aracısı MSI en son sürümünü](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Yüklemeyi tamamlamak için yönetici hakları olmalıdır.
6. Windows Azure Konuk aracı hizmetleri Hizmetleri'nde göründüğünü doğrulayın.
7. Bir talep üzerine yedekleme gerçekleştirin:
    * Portalında **Şimdi Yedekle**.

Ayrıca, doğrulayın [Microsoft .NET 4.5 yüklü](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) VM. .NET 4.5 hizmetiyle iletişim kurmak VM aracısı gereklidir.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil

#### <a name="solution"></a>Çözüm
Aracı veya uzantı ilgili hataları Linux Vm'leri için en güncel olmayan bir VM Aracısı etkileyen sorunları nedeniyle oluşur. Bu sorunu gidermek için bu yönergeleri izleyin:

1. Yönergelerini izleyin [Linux VM Aracısı güncelleştirilirken](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Biz *önemle tavsiye* yalnızca bir dağıtım deposu aracılığıyla aracıyı güncelleştirin. Aracı kodu doğrudan Github'dan indiriliyor ve güncelleştirirken önermiyoruz. Dağıtımınız için en son aracıyı yüklemek yönergeler mevcut, ilgili kişi dağıtım desteği değilse. En son aracı için denetlenecek Git [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) GitHub deposunda sayfası.

2. Aşağıdaki komutu çalıştırarak Azure Aracısı VM üzerinde çalıştığından emin olun: `ps -e`

 İşlem çalışıyor durumda değilse, aşağıdaki komutları kullanarak yeniden başlatın:

 * Ubuntu için: `service walinuxagent start`
 * Diğer dağıtımlar için: `service waagent start`

3. [Otomatik yeniden başlatma aracı yapılandırma](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Yeni bir test yedekleme çalıştırın. Hata devam ederse aşağıdaki günlüklerini sanal makineden toplayın:

   * /var/lib/waagent/*.XML
   * /var/log/waagent.log
   * / var/oturum/azure / *

Ayrıntılı günlük kaydı için waagent gerekiyorsa aşağıdaki adımları izleyin:

1. /Etc/waagent.conf dosyasında aşağıdaki satırı bulun: **ayrıntılı günlüğe yazmayı etkinleştir (y | n)**
2. Değişiklik **Logs.Verbose** değerini *n* için *y*.
3. Değişikliği kaydetmek ve ardından daha önce bu bölümde açıklanan adımları izleyerek waagent yeniden başlatın.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması
Sanal makine yedekleme, temel alınan depolama hesabı için bir anlık görüntü komutu gönderdikten üzerinde kullanır. Yedekleme, depolama hesabına erişimi olduğundan veya anlık görüntü görevi yürütme gecikmesi nedeniyle başarısız olabilir.

#### <a name="solution"></a>Çözüm
Aşağıdaki koşullar anlık görüntü görevi başarısız olmasına neden:

| Nedeni | Çözüm |
| --- | --- |
| Uzak Masaüstü Protokolü (RDP) VM'yi kapatın, çünkü VM durumu yanlış bildirilir. | VM ile RDP kapatırsanız, sanal Makinenin durumu doğru olup olmadığını belirlemek için portalı denetleyin. Doğru değilse, portaldaki VM kullanarak kapatma **kapatma** sanal makine Panosu'ndan seçeneği. |
| Sanal makine DHCP'den konak ya da yapı adresi alınamıyor. | DHCP çalışmak için konuk içinde Iaas VM yedekleme için etkinleştirilmesi gerekir. VM konak ya da yapı adresi DHCP yanıttan 245 erişemiyorsanız, indirin veya tüm Uzantıları'nı çalıştırın. Statik özel IP gerekiyorsa, içinde yapılandırmalısınız **Azure portalı** veya **PowerShell** ve VM içindeki DHCP seçeneği etkin olduğundan emin olun. PowerShell aracılığıyla statik bir IP ayarlama hakkında daha fazla bilgi için bkz. [Klasik VM](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm) ve [Resource Manager VM](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface).

### <a name="the-backup-extension-fails-to-update-or-load"></a>Backup uzantısı, güncelleştirmek veya yüklemek başarısız
Uzantıları yüklenemiyor bir anlık görüntü alınamadığından backup başarısız olur.

#### <a name="solution"></a>Çözüm

VMSnapshot uzantısı, yeniden yüklemek için zorlamak için uzantıyı kaldırın. Sonraki yedekleme girişimi uzantıyı yükler.

Uzantıyı kaldırmak için:

1. İçinde [Azure portalında](https://portal.azure.com/)yedekleme hatası yaşayan VM'yi gidin.
2. Seçin **ayarları**.
3. **Uzantılar**'ı seçin.
4. Seçin **Vmsnapshot uzantısı**.
5. Seçin **kaldırma**.

Linux VM, VMSnapshot uzantısı Azure Portalı'nda görünmüyorsa için [Azure Linux aracısını güncelleştirme](../virtual-machines/linux/update-agent.md), ve ardından yedeklemeyi çalıştırma.

Bu adımları tamamladıktan sonraki yedekleme sırasında yüklenmesi uzantısı neden olur.

### <a name="remove_lock_from_the_recovery_point_resource_group"></a>Kurtarma noktası kaynak grubundan kilidi kaldırın
1. [Azure Portal](http://portal.azure.com/) oturum açın.
2. Git **tüm kaynakları seçeneği**, geri yükleme noktası koleksiyonu kaynak grubunu seçin aşağıdaki biçimde AzureBackupRG_`<Geo>`_`<number>`.
3. İçinde **ayarları** bölümünden **kilitleri** kilitler görüntülenecek.
4. Kilidi kaldırmak için üç noktayı seçin ve **Sil**.

    ![Kilit silme ](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean_up_restore_point_collection"></a> Geri yükleme noktası koleksiyonunu Temizle
Kilit kaldırdıktan sonra geri yükleme noktalarını temizlenmesi gerekir. Geri yükleme noktaları temizlemek için aşağıdaki yöntemlerden herhangi birini izleyin:<br>
* [Geri yükleme noktası koleksiyonu çalışan geçici yedekleme tarafından Temizle](#clean-up-restore-point-collection-by-running-ad-hoc-backup)<br>
* [Temiz geri yükleme noktası koleksiyonu Azure portalından](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-ad-hoc-backup"></a>Geri yükleme noktası koleksiyonu çalışan geçici yedekleme tarafından Temizle
Kilit kaldırdıktan sonra bir ad-geçici/el ile yedekleme tetikleyin. Bu, geri yükleme noktalarını otomatik olarak temizlenir garanti eder. Bu ad-geçici/el ile işlem ilk kez başarısız olmasına beklediğiniz; Ancak, bunu el ile silinmesini geri yükleme noktaları yerine otomatik temizleme sağlayacaktır. Temizleme sonrasında, sonraki zamanlanmış yedekleme başarılı olması gerekir.

> [!NOTE]
    > Otomatik temizleme ad-geçici/el ile yedeklemeyi tetikleme birkaç saat sonra gerçekleşir. Zamanlanmış yedeklemenizi yine başarısız sonra listelenen adımları kullanarak geri yükleme noktası koleksiyonu el ile silmeyi deneyin [burada](#clean-up-restore-point-collection-from-azure-portal).

#### <a name="clean-up-restore-point-collection-from-azure-portal"></a>Temiz geri yükleme noktası koleksiyonu Azure portalından <br>

Noktaları, kaynak grubundaki kilit nedeniyle temizlenmez koleksiyonu geri yüklemeyi el ile temizlemek için aşağıdaki adımları deneyin:
1. [Azure Portal](http://portal.azure.com/) oturum açın.
2. Üzerinde **Hub** menüsünde tıklayın **tüm kaynaklar**, aşağıdaki biçimde AzureBackupRG_ kaynak grubunu seçin`<Geo>`_`<number>` , VM'nin bulunduğu.

    ![Kilit silme ](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Kaynak grubuna tıklayın **genel bakış** dikey penceresi görüntülenir.
4. Seçin **gizli türleri Göster** gizli tüm kaynakları görüntülemek için seçeneği. Geri yükleme noktası koleksiyonları aşağıdaki biçimde AzureBackupRG_ seçin`<VMName>`_`<number>`.

    ![Kilit silme ](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Tıklayın **Sil**, geri yükleme noktası koleksiyonunu temizlemek için.
6. Yedekleme işlemi yeniden deneyin.
