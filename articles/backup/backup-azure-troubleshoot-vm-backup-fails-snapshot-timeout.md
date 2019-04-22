---
title: 'Azure Backup hatalarında sorunları giderme: Konuk Aracısı durum yok'
description: Belirtiler, nedenleri ve çözümleri Aracısı, uzantı ve disk için ilgili Azure Backup hatası sayısı.
services: backup
author: genlin
manager: cshepard
keywords: Azure yedekleme; VM Aracısı; Ağ bağlantısı;
ms.service: backup
ms.topic: troubleshooting
ms.date: 12/03/2018
ms.author: genli
ms.openlocfilehash: ae89ab811015fca9bcb50fcc149534754533c25f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59491526"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Azure Backup hatalarında sorunları giderme: Aracı veya uzantı ile ilgili sorunlar

Bu makale yardımcı olacak sorun giderme adımlarını uzantısı ve VM Aracısı ile iletişim için ilgili Azure Backup hataları gidermek sağlar.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]



## <a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable - VM Aracısı Azure Backup ile iletişim kuramıyor

**Hata kodu**: UserErrorGuestAgentStatusUnavailable <br>
**Hata iletisi**: VM Aracısı Azure Backup ile iletişim kuramıyor<br>

Kaydolun ve bir VM yedekleme hizmeti için zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM Aracısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez, yedekleme başarısız olabilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:<br>
**1. neden: [VM Aracısı yüklendi, ancak (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2. neden: [Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [Anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**4. neden: [Backup uzantısı, güncelleştirmek veya yüklemek başarısız](#the-backup-extension-fails-to-update-or-load)**  
**5. neden: [VM internet erişimi yok](#the-vm-has-no-internet-access)**

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>Anlık görüntü durumu için VM aracısıyla GuestAgentSnapshotTaskStatusError - geçemedi

**Hata kodu**: GuestAgentSnapshotTaskStatusError<br>
**Hata iletisi**: Anlık görüntü durumu için VM aracısı ile iletişim kurulamadı <br>

Kaydolun ve bir VM için Azure Backup hizmeti zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM yedekleme uzantısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez yedekleme hatası meydana gelebilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:  
**1. neden: [VM Aracısı yüklendi, ancak (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**2. neden: [Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [VM internet erişimi yok](#the-vm-has-no-internet-access)**

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>UserErrorRpCollectionLimitReached - geri yükleme noktası koleksiyonu en yüksek sınırına ulaştı

**Hata kodu**: UserErrorRpCollectionLimitReached <br>
**Hata iletisi**: Geri yükleme noktası koleksiyonu en yüksek sınırına ulaştı. <br>
* Otomatik temizleme kurtarma noktasının önleme kurtarma noktası kaynak grubu üzerinde bir kilit ise bu sorun oluşabilir.
* Bu sorun ayrıca birden çok yedekleme günde tetiklenen oluşabilir. Şu anda günde yalnızca bir yedekleme anlık geri yükleme noktalarını yapılandırılan anlık görüntü saklama başına 1-5 gün boyunca korunur ve yalnızca 18 anlık RPs herhangi bir zamanda bir VM ile ilişkili olabilir öneririz. <br>

Önerilen eylem:<br>
Bu sorunu çözmek için VM kaynak grubu üzerindeki kilidi kaldırın ve temizleme tetiklemek için işlemi yeniden deneyin.
> [!NOTE]
> Yedekleme hizmeti, geri yükleme noktası koleksiyonu depolamak için sanal makinenin kaynak grubundan ayrı bir kaynak grubu oluşturur. Müşterilerin, Backup hizmeti tarafından kullanım için oluşturduğunuz kaynak grubunda değil kilitlemek için önerilir. Backup hizmeti tarafından oluşturulan kaynak grubunun adlandırma biçimi şu şekildedir: AzureBackupRG_`<Geo>`_`<number>` örn: AzureBackupRG_northeurope_1

**1. adım: [Geri yükleme noktası kaynak grubundan kilidi kaldırın](#remove_lock_from_the_recovery_point_resource_group)** <br>
**2. adım: [Geri yükleme noktası koleksiyonunu Temizle](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured - yedekleme şifrelenmiş vm'leri yedekleme için anahtar kasası için yeterli izinlere sahip değil

**Hata kodu**: UserErrorKeyvaultPermissionsNotConfigured <br>
**Hata iletisi**: Yedekleme, şifrelenmiş VM'lerin anahtar kasasına yedekleme için yeterli izinlere sahip değil. <br>

Yedekleme işleminin şifrelenmiş VM'ler üzerinde başarılı olması için bu anahtar kasasına erişmek için izinleri olmalıdır. Bu yapılabilir kullanarak [Azure portalında](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption) aracılığıyla veya [PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection).

## <a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork - anlık görüntü işlemi, sanal makinede ağ bağlantısı olmaması nedeniyle başarısız oldu

**Hata kodu**: ExtensionSnapshotFailedNoNetwork<br>
**Hata iletisi**: Sanal makinede ağ bağlantısı olmaması nedeniyle Anlık Görüntü işlemi başarısız oldu<br>

Kaydolun ve bir VM için Azure Backup hizmeti zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM yedekleme uzantısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez yedekleme hatası meydana gelebilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:    
**1. neden: [Anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**2. neden: [Backup uzantısı, güncelleştirmek veya yüklemek başarısız](#the-backup-extension-fails-to-update-or-load)**  
**3. neden: [VM internet erişimi yok](#the-vm-has-no-internet-access)**

## <a name="ExtentionOperationFailed-vmsnapshot-extension-operation-failed"></a>ExtentionOperationFailedForManagedDisks - VMSnapshot uzantısı işlemi başarısız oldu

**Hata kodu**: ExtentionOperationFailedForManagedDisks <br>
**Hata iletisi**: VMSnapshot uzantısı işlemi başarısız oldu<br>

Kaydolun ve bir VM için Azure Backup hizmeti zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM yedekleme uzantısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez yedekleme hatası meydana gelebilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:  
**1. neden: [Anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**2. neden: [Backup uzantısı, güncelleştirmek veya yüklemek başarısız](#the-backup-extension-fails-to-update-or-load)**  
**3. neden: [VM Aracısı yüklendi, ancak (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**4. neden: [Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed / BackUpOperationFailedV2 - yedekleme başarısız olursa bir iç hata ile

**Hata kodu**: BackUpOperationFailed / BackUpOperationFailedV2 <br>
**Hata iletisi**: Yedekleme bir iç hata ile başarısız oldu - Lütfen işlemi birkaç dakika içinde yeniden deneyin <br>

Kaydolun ve bir VM için Azure Backup hizmeti zamanlama sonra yedekleme zaman içinde nokta anlık görüntüsünü almak için VM yedekleme uzantısı ile iletişim kurarak iş başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenmez yedekleme hatası meydana gelebilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve sonra işlemi yeniden deneyin:  
**1. neden: [VM, ancak bunu yüklü Aracı (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**2. neden: [Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [Anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**4. neden: [Backup uzantısı, güncelleştirmek veya yüklemek başarısız](#the-backup-extension-fails-to-update-or-load)**  
**5. neden: Yedekleme hizmeti, bir kaynak grubu kilidi nedeniyle eski geri yükleme noktalarını silmek için izne sahip değil** <br>
**6. neden: [VM internet erişimi yok](#the-vm-has-no-internet-access)**

## <a name="usererrorunsupporteddisksize---currently-azure-backup-does-not-support-disk-sizes-greater-than-4095gb"></a>UserErrorUnsupportedDiskSize - şu anda Azure Backup, 4095 GB'den büyük disk boyutlarını desteklemez

**Hata kodu**: UserErrorUnsupportedDiskSize <br>
**Hata iletisi**: Azure Backup şu anda 4095 GB'den büyük disk boyutlarını desteklememektedir <br>

Disk boyutu 4095 GB'tan büyük ile VM'yi yedeklerken, yedekleme işlemi başarısız olabilir. Büyük diskler için destek yakında sunulacaktır.  

## <a name="usererrorbackupoperationinprogress---unable-to-initiate-backup-as-another-backup-operation-is-currently-in-progress"></a>UserErrorBackupOperationInProgress - şu anda başka bir yedekleme işlemi devam ediyor gibi yedekleme başlatamıyor

**Hata kodu**: UserErrorBackupOperationInProgress <br>
**Hata iletisi**: Yedekleme şu anda başka bir yedekleme işlemi devam ediyor olarak başlatılamıyor<br>

Devam eden var olan bir yedekleme işi olduğundan son yedekleme işi başarısız oldu. Geçerli iş tamamlanana kadar yeni bir yedekleme işi başlatamazsınız. Devam eden yedekleme işlemi tetikleyen veya başka bir yedekleme işlemleri zamanlama önce tamamlanan emin olun. Yedekleme işlerinin durumunu denetlemek için gerçekleştirmek aşağıdaki adımları:

1. Azure portalında oturum açın ve **Tüm hizmetler**'e tıklayın. Kurtarma Hizmetleri yazın ve **Kurtarma Hizmetleri kasaları**’na tıklayın. Kurtarma hizmetleri kasalarının listesi görünür.
2. Kurtarma Hizmetleri kasalarının listesinden, yedeklemenin yapılandırıldığı bir kasa seçin.
3. Kasa Panosu menüsünde tıklatın **yedekleme işleri** tüm yedekleme işleri görüntüler.

    * Bir yedekleme işi devam ediyorsa, yedekleme işini iptal edin veya tamamlamak için bekleyin.
        * Yedekleme işi sağ yedekleme işini iptal edin ve tıklayın **iptal** veya [PowerShell](https://docs.microsoft.com/en-us/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0).
    * Farklı bir kasadaki yedekleme yapılandırdıysanız, ardından eski kasaya çalışan hiçbir yedekleme işleri emin olun. Varsa, ardından yedekleme işini iptal edin.
        * Yedekleme işi sağ yedekleme işini iptal edin ve tıklayın **iptal** veya [PowerShell](https://docs.microsoft.com/en-us/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0)
4. Yedekleme işlemini yeniden deneyin.

Zamanlanmış yedekleme işlemi ile sonraki yedekleme yapılandırması çakışan uzun sürüyorsa incelediniz [en iyi](backup-azure-vms-introduction.md#best-practices), [yedekleme performansı](backup-azure-vms-introduction.md#backup-performance) ve [göz önünde bulundurarak geri yükleme ](backup-azure-vms-introduction.md#backup-and-restore-considerations).


## <a name="causes-and-solutions"></a>Nedenler ve çözümler

### <a name="the-vm-has-no-internet-access"></a>VM internet erişimi yok
Dağıtım gereksinim, VM, internet erişimi yok. Veya, Azure altyapısı için erişimi engelleyen kısıtlamalar olabilir.

Düzgün çalışması için yedekleme uzantısını Azure genel IP adreslerine bağlantısı gerektirir. Uzantı komutları VM anlık görüntülerini yönetmek için bir Azure depolama uç noktasına (HTTPs URL'si) gönderir. Uzantı genel internet erişimi yoksa, yedekleme sonunda başarısız olur.

####  <a name="solution"></a>Çözüm
Ağ sorunu çözmek için bkz [ağ bağlantısı kurma](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

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

1. /Etc/waagent.conf dosyasında aşağıdaki satırı bulun: **Ayrıntılı günlüğe yazmayı etkinleştir (y | n)**
2. Değişiklik **Logs.Verbose** değerini *n* için *y*.
3. Değişikliği kaydetmek ve ardından daha önce bu bölümde açıklanan adımları izleyerek waagent yeniden başlatın.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntünün alınması
Sanal makine yedekleme, temel alınan depolama hesabı için bir anlık görüntü komutu gönderdikten üzerinde kullanır. Yedekleme, depolama hesabına erişimi olduğundan veya anlık görüntü görevi yürütme gecikmesi nedeniyle başarısız olabilir.

#### <a name="solution"></a>Çözüm
Aşağıdaki koşullar anlık görüntü görevi başarısız olmasına neden:

| Nedeni | Çözüm |
| --- | --- |
| Uzak Masaüstü Protokolü (RDP) VM'yi kapatın, çünkü VM durumu yanlış bildirilir. | VM ile RDP kapatırsanız, sanal Makinenin durumu doğru olup olmadığını belirlemek için portalı denetleyin. Doğru değilse, portaldaki VM kullanarak kapatma **kapatma** sanal makine Panosu'ndan seçeneği. |
| Sanal makine DHCP'den konak ya da yapı adresi alınamıyor. | DHCP çalışmak için konuk içinde Iaas VM yedekleme için etkinleştirilmesi gerekir. VM konak ya da yapı adresi DHCP yanıttan 245 erişemiyorsanız, indirin veya tüm Uzantıları'nı çalıştırın. Statik özel IP gerekiyorsa, içinde yapılandırmalısınız **Azure portalında** veya **PowerShell** ve VM içindeki DHCP seçeneği etkin olduğundan emin olun. [Daha fazla bilgi edinin](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface) PowerShell ile statik bir IP adresi ayarlama hakkında.

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
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Git **tüm kaynakları seçeneği**, geri yükleme noktası koleksiyonu kaynak grubunu seçin aşağıdaki biçimde AzureBackupRG_`<Geo>`_`<number>`.
3. İçinde **ayarları** bölümünden **kilitleri** kilitler görüntülenecek.
4. Kilidi kaldırmak için üç noktayı seçin ve **Sil**.

    ![Kilit silme](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean_up_restore_point_collection"></a> Geri yükleme noktası koleksiyonunu Temizle
Kilit kaldırdıktan sonra geri yükleme noktalarını temizlenmesi gerekir. Geri yükleme noktaları temizlemek için aşağıdaki yöntemlerden herhangi birini izleyin:<br>
* [Temiz geri yükleme noktası koleksiyonu geçici yedekleme çalıştırarak](#clean-up-restore-point-collection-by-running-ad-hoc-backup)<br>
* [Temiz geri yükleme noktası koleksiyonu Azure portalından](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-ad-hoc-backup"></a>Temiz geri yükleme noktası koleksiyonu geçici yedekleme çalıştırarak
Kilit kaldırdıktan sonra bir ad geçici/el ile yedekleme tetikleyin. Bu, geri yükleme noktalarını otomatik olarak temizlenir garanti eder. Bu ad geçici/el ile işlem ilk kez başarısız beklediğiniz; Ancak, bunu el ile silinmesini geri yükleme noktaları yerine otomatik temizleme sağlayacaktır. Temizleme sonrasında, sonraki zamanlanmış yedekleme başarılı olması gerekir.

> [!NOTE]
> Otomatik temizleme ad geçici/el ile yedeklemeyi tetikleme birkaç saat sonra gerçekleşir. Zamanlanmış yedeklemenizi yine başarısız sonra listelenen adımları kullanarak geri yükleme noktası koleksiyonu el ile silmeyi deneyin [burada](#clean-up-restore-point-collection-from-azure-portal).

#### <a name="clean-up-restore-point-collection-from-azure-portal"></a>Temiz geri yükleme noktası koleksiyonu Azure portalından <br>

Noktaları, kaynak grubundaki kilit nedeniyle temizlenmez koleksiyonu geri yüklemeyi el ile temizlemek için aşağıdaki adımları deneyin:
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Üzerinde **Hub** menüsünde tıklayın **tüm kaynaklar**, aşağıdaki biçimde AzureBackupRG_ kaynak grubunu seçin`<Geo>`_`<number>` , VM'nin bulunduğu.

    ![Kilit silme](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Kaynak grubuna tıklayın **genel bakış** dikey penceresi görüntülenir.
4. Seçin **gizli türleri Göster** gizli tüm kaynakları görüntülemek için seçeneği. Geri yükleme noktası koleksiyonları aşağıdaki biçimde AzureBackupRG_ seçin`<VMName>`_`<number>`.

    ![Kilit silme](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Tıklayın **Sil**, geri yükleme noktası koleksiyonunu temizlemek için.
6. Yedekleme işlemi yeniden deneyin.

> [!NOTE]
 >Kaynak (RP koleksiyonu) büyük varsa, ardından aynı portaldan silinmesi geri yükleme noktası sayısı zaman aşımına uğrayabilir ve başarısız. Burada tüm geri yükleme noktaları görünürlüğe sürede silinmez ve işlem zaman aşımına bilinen bir CRP sorun budur; ancak silme işlemi genellikle 2 veya 3 denemeden sonra başarılı olur.
