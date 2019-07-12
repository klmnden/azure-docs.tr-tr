---
title: Azure sanal makineleri yedekleme hatalarında sorun giderme
description: Backup ve Azure sanal makinelerini geri yükleme sorunlarını giderme
services: backup
author: srinathvasireddy
manager: sivan
ms.service: backup
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: srinathv
ms.openlocfilehash: d7b99e7076e52db004bba7155922f4b144f2ad0a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704891"
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure sanal makine yedekleme sorunlarını giderme
Aşağıda listelenen bilgilerle Azure Backup'ı kullanırken karşılaşılan hataları giderebilirsiniz:

## <a name="backup"></a>Backup
Bu bölümde, Azure sanal makine yedekleme işlemi başarısız kapsar.

## <a name="copyingvhdsfrombackupvaulttakinglongtime---copying-backed-up-data-from-vault-timed-out"></a>CopyingVHDsFromBackUpVaultTakingLongTime - kopyalama zaman aşımına uğradı kasasından yedeklenen verileri

Hata kodu: CopyingVHDsFromBackUpVaultTakingLongTime <br/>
Hata iletisi: Yedeklenen zaman aşımına uğradı kasasından verileri kopyalama

Bu, geçici depolama hataları veya yetersiz depolama hesabı yedekleme hizmeti için zaman aşımı süresi içinde kasaya veri aktarmak için IOPS nedeniyle gerçekleşebilir. Bunları kullanarak VM yedeklemeyi yapılandırma [en iyi uygulamalar](backup-azure-vms-introduction.md#best-practices) ve yedekleme işlemini yeniden deneyin.

## <a name="usererrorvmnotindesirablestate---vm-is-not-in-a-state-that-allows-backups"></a>UserErrorVmNotInDesirableState - VM yedeklemeleri izin veren bir durumda değil.

Hata kodu: UserErrorVmNotInDesirableState <br/>
Hata iletisi: Sanal makine yedeklemelere izin veren bir durumda değil.<br/>

VM başarısız durumda yedekleme işlemi başarısız oldu. Başarılı yedekleme VM durumu çalışıyor, durduruldu veya durduruldu (serbest bırakıldı) olmalıdır.

* VM arasında geçici bir durumda ise **çalıştıran** ve **kapatma**, bekleme durumu değiştirmek. Ardından, yedekleme işini tetikler.
*  VM bir Linux VM ve Security-Enhanced Linux çekirdek modülü kullanır, Azure Linux Aracısı yolu dışla **/var/lib/waagent** emin olun ve güvenlik ilkesi yedekleme uzantısı yüklenir.

## <a name="usererrorfsfreezefailed---failed-to-freeze-one-or-more-mount-points-of-the-vm-to-take-a-file-system-consistent-snapshot"></a>Bir veya daha fazla bağlama noktası, dosya sistemi ile tutarlı bir anlık görüntü almak üzere sanal dondurma UserErrorFsFreezeFailed - başarısız oldu

Hata kodu: UserErrorFsFreezeFailed <br/>
Hata iletisi: Dosya sisteminde tutarlı anlık görüntü almak için VM'nin bir veya daha fazla takma noktası dondurulamadı.

* Dosya sistem durumunu kullanarak tüm bağlı cihazları denetleme **tune2fs** Örneğin, komut **tune2fs -l/dev/sdb1 \\** .\| grep **Dosyasistemidurumu**.
* Kullanarak, dosya sistemi durumu temizlenmedi, cihazları çıkarın **umount** komutu.
* Bu cihazlarda dosya sistemi tutarlılık denetimi çalıştırmak **fsck** komutu.
* Cihazları yeniden bağlayın ve yedekleme işlemini yeniden deneyin.</ol>

## <a name="extensionsnapshotfailedcom--extensioninstallationfailedcom--extensioninstallationfailedmdtc---extension-installationoperation-failed-due-to-a-com-error"></a>ExtensionSnapshotFailedCOM / ExtensionInstallationFailedCOM / ExtensionInstallationFailedMDTC - uzantısı yükleme/işlemi başarısız oldu nedeniyle bir COM + hatası

Hata kodu: ExtensionSnapshotFailedCOM <br/>
Hata iletisi: COM + hatası nedeniyle anlık görüntü işlemi başarısız oldu

Hata kodu: ExtensionInstallationFailedCOM  <br/>
Hata iletisi: Uzantı yükleme/işlemi bir COM + hatası nedeniyle başarısız oldu

Hata kodu: ExtensionInstallationFailedMDTC <br/>
Hata iletisi: "COM + Microsoft Distributed Transaction Düzenleyicisi ile konuşamadı uzantısı yükleme işlemi şu hatayla başarısız oldu <br/>

Yedekleme işlemi başarısız oldu Windows hizmeti ile ilgili bir sorun nedeniyle **COM + Sistem** uygulama.  Bu sorunu çözmek için şu adımları izleyin:

* Başlangıç/Windows hizmetini yeniden başlatmayı deneyin **COM + System Application** (yükseltilmiş bir komut isteminden **-net Başlat COMSysApp**).
* Olun **Dağıtılmış İşlem Düzenleyicisi** Hizmetleri olarak çalıştığı **ağ hizmeti** hesabı. Aksi takdirde, çalışacak şekilde değiştirmeniz **ağ hizmeti** hesap ve yeniden **COM + System Application**.
* Alınamazsa hizmetini yeniden başlatın, ardından yeniden **Dağıtılmış İşlem Düzenleyicisi** izleyerek service aşağıdaki adımları:
    * MSDTC hizmetini durdurun
    * Komut istemini (cmd) açın
    * Komut Çalıştır "msdtc-kaldırma"
    * Çalıştır komutu "msdtc-yükleyin"
    * MSDTC hizmetini başlatın
* Windows hizmeti başlatın **COM + System Application**. Sonra **COM + System Application** başladığında, Azure portalında yedekleme işini tetikler.</ol>

## <a name="extensionfailedvsswriterinbadstate---snapshot-operation-failed-because-vss-writers-were-in-a-bad-state"></a>ExtensionFailedVssWriterInBadState - anlık görüntü işlemi VSS yazıcılarının hatalı durumda olduğundan başarısız oldu

Hata kodu: ExtensionFailedVssWriterInBadState <br/>
Hata iletisi: VSS yazıcılarının hatalı durumda olduğundan anlık görüntü işlemi başarısız oldu.

Kötü durumdaki VSS yazıcıları yeniden başlatın. Yükseltilmiş bir komut isteminden çalıştırın ```vssadmin list writers```. Çıktı, tüm VSS yazıcılarının ve bunların durumunu içerir. Her VSS yazıcısının olmayan durumuna sahip **[1] kararlı**, VSS Yazıcı yeniden, yükseltilmiş bir komut isteminden aşağıdaki komutları çalıştırın:

  * ```net stop serviceName```
  * ```net start serviceName```

## <a name="extensionconfigparsingfailure--failure-in-parsing-the-config-for-the-backup-extension"></a>ExtensionConfigParsingFailure - yedekleme uzantısı için yapılandırma ayrıştırma hatası

Hata kodu: ExtensionConfigParsingFailure<br/>
Hata iletisi: Yedekleme uzantısı için yapılandırma ayrıştırılırken hata oluştu.

Bu hata, üzerinde değiştirilen izinler nedeniyle oluşur. **MachineKeys** dizin: **%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**.
Aşağıdaki komutu çalıştırın ve doğrulayın, izinlerini **MachineKeys** directory varsayılan değerleri olan:**icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**.

Varsayılan izinler şunlardır:
* Herkesin: (R, G)
* BUILTIN\Administrators: (F)

İzinler görürseniz **MachineKeys** varsayılan farklı bir dizin izinlerini düzeltin, sertifikayı silin ve yedeklemeyi tetiklemek için aşağıdaki adımları izleyin:

1. İzinleri düzelt **MachineKeys** dizin. Dizinde Explorer güvenlik özellikleri ve Gelişmiş güvenlik ayarları kullanarak, izinler varsayılan değerlere sıfırlayın. Tüm kullanıcı nesnelerini hariç Varsayılanları dizininden ve emin Kaldır **herkes** izni olan özel erişim gibi:

    * Liste klasörü/veri okuma
    * Okuma öznitelikleri
    * Okuma genişletilmiş öznitelikleri
    * Dosya Oluştur/Veri Yaz
    * Klasör Oluştur/Veri Ekle
    * Yazma öznitelikleri
    * Genişletilmiş öznitelikleri yazma
    * Okuma izinleri
2. Tüm sertifikaları sil burada **çıkarılan** Klasik dağıtım modeli veya **Windows Azure CRP sertifika Oluşturucu**:
    * [Sertifika yerel bilgisayar konsolu açın](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx).
    * Altında **kişisel** > **sertifikaları**, tüm sertifikaları sil burada **çıkarılan** Klasik dağıtım modeli veya **Windows Azure CRP Sertifika Oluşturucu**.
3. VM yedekleme işini tetikler.

## <a name="extensionstuckindeletionstate---extension-state-is-not-supportive-to-backup-operation"></a>ExtensionStuckInDeletionState - uzantı durumu yedekleme işlemini desteklemiyor değil

Hata kodu: ExtensionStuckInDeletionState <br/>
Hata iletisi: Uzantı durumu yedekleme işlemini desteklemiyor değil

Yedekleme işlemi yedekleme uzantısı tutarsız durumu nedeniyle başarısız oldu. Bu sorunu çözmek için şu adımları izleyin:

* Konuk Aracı'nın yüklü olduğundan ve yanıt verdiğinden emin olun
* Azure portalında **Sanal Makine** > **Tüm Ayarlar** > **Uzantılar**'a gidin
* VmSnapshot veya VmSnapshotLinux yedekleme uzantısını seçin ve **Kaldır**'a tıklayın
* Yedekleme uzantısını sildikten sonra yedekleme işlemini yeniden deneyin
* Sonraki yedekleme işleminde yeni uzantı istenen durumda yüklenecektir

## <a name="extensionfailedsnapshotlimitreachederror---snapshot-operation-failed-as-snapshot-limit-is-exceeded-for-some-of-the-disks-attached"></a>Bazı bağlı disklerin ExtensionFailedSnapshotLimitReachedError - anlık görüntü işlemi başarısız oldu anlık görüntü sınırı aşıldı

Hata kodu: ExtensionFailedSnapshotLimitReachedError  <br/>
Hata iletisi: Bazı bağlı diskler için anlık görüntü işlemi başarısız oldu anlık görüntü sınırı aşıldı

Anlık görüntü işlemi başarısız oldu anlık görüntü sınırı bazı bağlı disklerin eşiğini aştı. Tamamlamak aşağıdaki sorun giderme adımlarını ve işlemi yeniden deneyin.

* Disk blobu gerekli olmayan anlık görüntülerini silin. Disk blobu silmek için dikkatli olun, sadece anlık görüntü BLOB'ları silinmesi gerekir.
* Geçici silme VM disk üzerinde depolama hesapları etkinleştirilirse, geçici silmeyi bekletme süresi herhangi bir noktada izin verilen maksimum değerden mevcut anlık görüntüleridir gibi yapılandırın.
* Azure Site Recovery yedeklenen VM'yi etkinse, sonra gerçekleştirin aşağıda:

    * Değerini olun **isanysnapshotfailed** /etc/azure/vmbackup.conf false olarak ayarlayın
    * Azure Site Recovery, yedekleme işlemi çakışmayacak şekilde farklı bir zamanda zamanlayın.

## <a name="extensionfailedtimeoutvmnetworkunresponsive---snapshot-operation-failed-due-to-inadequate-vm-resources"></a>ExtensionFailedTimeoutVMNetworkUnresponsive - anlık görüntü işlemi, VM kaynaklarının yetersizliği nedeniyle başarısız oldu.

Hata kodu: ExtensionFailedTimeoutVMNetworkUnresponsive<br/>
Hata iletisi: Anlık görüntü işlemi, VM kaynaklarının yetersizliği nedeniyle başarısız oldu.

Anlık görüntü işlemi gerçekleştirilirken ağ çağrılarındaki gecikme nedeniyle sanal makine yedekleme işlemi başarısız. Bu sorunu çözmek için 1. adımı uygulayın. Sorun devam ederse 2. ve 3. adımları deneyin.

**1. adım**: Konak üzerinden anlık görüntü oluşturma

Yükseltilmiş (yönetici) komut isteminden aşağıdaki komutu çalıştırın:

```
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v SnapshotMethod /t REG_SZ /d firstHostThenGuest /f
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v CalculateSnapshotTimeFromHost /t REG_SZ /d True /f
```

Bu, anlık görüntünün Konuk yerine konak üzerinden alınmasını sağlar. Yedekleme işlemini yeniden deneyin.

**2. adım**: VM (daha az CPU/IOPS vb.) daha az yük altında olduğunda bir süre için yedekleme zamanlamasını değiştirmeyi deneyin

**3. adım**: Deneyin [VM'nin boyutunu artırma](https://azure.microsoft.com/blog/resize-virtual-machines/) ve işlemi yeniden deneyin

## <a name="common-vm-backup-errors"></a>Genel sanal makine yedekleme hataları

| Hata Ayrıntıları | Geçici Çözüm |
| ------ | --- |
| Hata kodu: 320001<br/> Hata iletisi: VM artık mevcut olmadığından işlem gerçekleştirilemiyor. <br/> <br/> Hata kodu: 400094 <br/> Hata iletisi: Sanal makine yok <br/> <br/>  Bir Azure sanal makinesi bulunamadı.  |Birincil VM silindiği ancak yedekleme ilkesini yedeklemek için bir VM için yine de görünür. Bu hata oluşuyor. Bu hatayı düzeltmek için aşağıdaki adımları uygulayın: <ol><li> Sanal makine ile aynı ada ve aynı kaynak grubu adı, yeniden oluşturma **bulut hizmeti adı**,<br>**or**</li><li> İle veya olmadan, yedekleme verilerini silmeden sanal makine korumayı durdurun. Daha fazla bilgi için [sanal makineleri korumayı durdurun](backup-azure-manage-vms.md#stop-protecting-a-vm).</li></ol>|
| VM başarısız sağlama durumunda olup: <br>VM'yi yeniden başlatın ve VM'nin çalıştığından emin olun veya kapatma. | Uzantı hataları birini VM başarısız sağlama durumu koyarsa bu hata oluşur. Başarısız bir uzantısı varsa, kaldırana uzantılar listesine denetleyin ve sanal makineyi yeniden başlatarak gidin. Tüm uzantıları çalışır durumda olduğundan, VM aracısı hizmetinin çalışıp çalışmadığını denetleyin. Aksi durumda, VM Aracısı hizmetini yeniden başlatın. |
|Hata kodu: UserErrorBCMPremiumStorageQuotaError<br/> Hata iletisi: Depolama hesabında yeterli boş alan sanal makinenin anlık görüntüsü kopyalanamadı. | VM yedekleme yığını V1 üzerinde Premium VM'ler için şu anlık görüntü depolama hesabına kopyalayın. Bu adım, bir anlık görüntü üzerinde çalışır, yedekleme yönetim trafiği, premium diskler kullanarak bir uygulama için kullanılabilir IOPS sayısını kısıtlamaz emin olur. <br><br>Yalnızca yüzde 50, toplam depolama hesabı alanının 17,5 TB ayırmanız önerilir. Daha sonra kasa için depolama hesabındaki kopyalanan bu konumdan Azure Backup hizmeti anlık görüntü depolama hesabı ve aktarım veri kopyalayabilirsiniz. |
| Sanal makine çalışmadığından Microsoft Kurtarma Hizmetleri Uzantısı yüklenemedi <br>VM Aracısı, Azure kurtarma Hizmetleri uzantısı için bir önkoşuldur. Azure sanal makine Aracısı yükleme ve kayıt işlemi yeniden başlatın. |<ol> <li>VM Aracısı düzgün yüklü olup olmadığını denetleyin. <li>VM yapılandırması bayrağı doğru ayarlandığından emin olun.</ol> VM aracısı ve VM Aracısı yüklemesini doğrulamak nasıl yükleme hakkında daha fazla bilgi edinin. |
| Anlık görüntü işlemi başarısız oldu, Birim Gölge Kopyası Hizmeti (VSS) işlemi hata **Bu sürücü BitLocker Sürücü Şifrelemesi tarafından kilitlendi. Denetim Masası'ndan bu sürücünün kilidini açmanız gerekir.** |Sanal makinedeki tüm sürücüleri için BitLocker'ı açın ve VSS sorunun çözülüp çözülmediğine bakın. |
| VM yedeklemeleri izin veren bir durumda değil. |<ul><li>VM arasında geçici bir durumda ise **çalıştıran** ve **kapatma**, bekleme durumu değiştirmek. Ardından, yedekleme işini tetikler. <li> VM bir Linux VM ve Security-Enhanced Linux çekirdek modülü kullanır, Azure Linux Aracısı yolu dışla **/var/lib/waagent** emin olun ve güvenlik ilkesi yedekleme uzantısı yüklenir.  |
| VM Aracısı sanal makinede mevcut değil: <br>Tüm önkoşul ve VM aracısını yükleyin. Ardından, işlemi yeniden başlatın. |Daha fazla bilgi edinin [VM aracısını yükleme ve VM Aracısı yüklemesini doğrulamak nasıl](#vm-agent). |
| Yedekleme, bir veya daha fazla bağlama noktaları sanal dosya sistemi tutarlı bir anlık görüntüsünü almak için dondurulamadı. | Aşağıdaki adımları izleyin: <ul><li>Kullanarak tüm bağlı cihazları dosya sistem durumunu kontrol **'tune2fs'** komutu. Bir örnek **tune2fs -l/dev/sdb1 \\** .\| grep **dosya sistemi durumu**. <li>Kendisi için dosya sistemi durumu olmayan temiz kullanarak cihazları çıkarın **'umount'** komutu. <li> Bu cihazlarda dosya sistemi tutarlılık denetimi çalıştırmak **'fsck'** komutu. <li> Cihazları yeniden bağlayın ve yedeklemeyi deneyin.</ol> |
| Anlık görüntü işlemi, bir güvenli ağ iletişim kanalı oluşturma hatası nedeniyle başarısız oldu. | <ol><li> Kayıt Defteri Düzenleyicisi'ni çalıştırarak açmak **regedit.exe** yükseltilmiş modda. <li> Sisteminizde mevcut .NET Framework'ün tüm sürümler tanımlayın. Kayıt defteri anahtarının hiyerarşisi altında mevcut oldukları **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft**. <li> Kayıt defteri anahtarı mevcut her .NET Framework için aşağıdaki anahtarını ekleyin: <br> **SchUseStrongCrypto"=dword:00000001**. </ol>|
| Visual Studio 2012 için Visual C++ yeniden dağıtılabilir yükleme hatası nedeniyle anlık görüntü işlemi başarısız. | İçin C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion gidin ve vcredist2012_x64 yükleyin.<br/>Hizmet yüklemesini sağlayan kayıt defteri anahtar değeri doğru değerine ayarlanmış olduğundan emin olun. Diğer bir deyişle, ayarlama **Başlat** değerini **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSIServer** için **3** değil **4**. <br><br>Yükleme sorunlarını hala varsa, yükleme hizmeti çalıştırarak yeniden **MSIEXEC/unregister** ardından **MSIEXEC /REGISTER** yükseltilmiş bir komut isteminden.  |


## <a name="jobs"></a>İşler

| Hata Ayrıntıları | Geçici Çözüm |
| --- | --- |
| İptal, bu proje türü için desteklenmez: <br>İş tamamlanana kadar bekleyin. |Yok. |
| İş, iptal edilebilir bir durumda değil: <br>İş tamamlanana kadar bekleyin. <br>**or**<br> Seçili iş, iptal edilebilir bir durumda değil: <br>İşin tamamlanmasını bekleyin. |Neredeyse tamamlanmış iş olduğundan emin olma olasılığı yüksektir. İş tamamlanana kadar bekleyin.|
| Yedekleme, devam eden olmadığından iş iptal edilemiyor: <br>İptal, yalnızca devam eden işler için desteklenir. Devam eden işi iptal etmeyi deneyin. |Bu hata geçici bir durum nedeniyle oluşur. Bir dakika bekleyin ve İptal işlemi yeniden deneyin. |
| Yedekleme işi iptal edilemedi: <br>İş tamamlanana kadar bekleyin. |None |

## <a name="restore"></a>Geri Yükleme

| Hata Ayrıntıları | Geçici Çözüm |
| --- | --- |
| Geri yükleme, bir bulut iç hatayla başarısız oldu. |<ol><li>Geri yükleme çalıştığınız bulut hizmeti DNS ayarlarıyla yapılandırılır. Kontrol edebilirsiniz: <br>**$deployment = get-AzureDeployment - ServiceName "ServiceName"-yuvası "Üretim" Get-AzureDns - DnsSettings $deployment. DnsSettings**.<br>Varsa **adresi** DNS ayarlarını yapılandırıldıysa, yapılandırılır.<br> <li>Yapılacak çalıştığınız geri yüklemek bulut hizmeti ile yapılandırılmış **ReservedIP**, ve bulut hizmetindeki var olan VM'ler durdurulmuş durumda. Bir bulut hizmeti aşağıdaki PowerShell cmdlet'lerini kullanarak bir IP ayırdığı denetleyebilirsiniz: **$deployment = Get-AzureDeployment - ServiceName "servicename"-"Üretim" $yuvası DEP Reservedıpname**. <br><li>Bir sanal makineyle aynı bulut hizmetine aşağıdaki özel ağ yapılandırmalarını geri yüklemek çalışıyorsunuz: <ul><li>Sanal makineleri yük dengeleyici yapılandırmasını, iç ve dış altında.<li>Birden çok ayrılmış IP ile sanal makineler. <li>Birden çok NIC içeren sanal makineler. </ul><li>Kullanıcı Arabirimi veya bkz: yeni bir bulut hizmeti seçin [konuları geri](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations) özel ağ yapılandırmaları olan VM'ler için.</ol> |
| Seçilen DNS adı zaten alınmış: <br>Farklı bir DNS adı belirtip yeniden deneyin. |Bu DNS adı genellikle ile biten bulut hizmeti adı başvurduğu **. cloudapp.net**. Bu adın benzersiz olması gerekir. Bu hatayı alırsanız, geri yükleme sırasında farklı bir VM adı seçmeniz gerekir. <br><br> Bu hata, yalnızca Azure portalı kullanıcılarına gösterilir. Yalnızca diskleri geri yükler ve VM'yi oluşturmak değildir çünkü PowerShell aracılığıyla geri yükleme işlemi başarılı olur. Diski geri yükleme işleminden VM açıkça sizin tarafınızdan oluşturulduğunda hata karşılaştığı. |
| Belirtilen sanal ağ yapılandırması doğru değil: <br>Farklı sanal ağ yapılandırması belirtin ve yeniden deneyin. |Yok. |
| Belirtilen bulut hizmeti, geri yüklenen sanal makine yapılandırmasını eşleşmeyen bir ayrılmış IP kullanıyor: <br>Ayrılmış IP kullanmıyor farklı bir bulut hizmeti belirtin. Veya geri yükleme için başka bir kurtarma noktası seçin. |Yok. |
| Bulut hizmetine giriş uç noktaları sayısı sınırına ulaştı: <br>İşlemi farklı bir bulut hizmeti belirterek veya var olan bir uç nokta kullanarak yeniden deneyin. |None |
| Kurtarma Hizmetleri kasası ve hedef depolama hesabı, iki farklı bölgede şunlardır: <br>Geri yükleme işleminde belirtilen depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı Azure bölgesinde olduğundan emin olun. |Yok. |
| Geri yükleme işlemi için belirtilen depolama hesabı desteklenmiyor: <br>Yerel olarak yedekli veya coğrafi olarak yedekli çoğaltma ayarlarıyla yalnızca temel veya standart depolama hesapları desteklenmektedir. Desteklenen bir depolama hesabı seçin. |None |
| Çevrimiçi geri yükleme işlemi için belirtilen depolama hesabı türü değil: <br>Geri yükleme işleminde belirtilen depolama hesabı çevrimiçi olduğundan emin olun. |Bu hata, Azure Depolama'daki geçici bir hata nedeniyle veya kesinti nedeniyle gerçekleşebilir. Başka bir depolama hesabı seçin. |
| Kaynak grubu kotasına ulaşıldı: <br>Bazı kaynak gruplarını, Azure portalından silin veya limitleri artırmak için Azure desteğine başvurun. |None |
| Seçilen alt ağ yok: <br>Var olan bir alt ağ seçin. |Yok. |
| Backup hizmeti, aboneliğinizdeki kaynaklara erişmek için yetkiniz yok. |Bu hatayı gidermek için ilk diskleri adımları kullanarak geri [yedeklenen diskleri geri](backup-azure-arm-restore-vms.md#restore-disks). PowerShell adımları kullanın [geri yüklenen diskten VM oluşturma](backup-azure-vms-automation.md#restore-an-azure-vm). |

## <a name="backup-or-restore-takes-time"></a>Yedekleme veya geri yükleme sürüyor
Yedekleme 12 saatten uzun sürer ve geri yükleme 6 saatten uzun sürer, gözden [en iyi uygulamalar](backup-azure-vms-introduction.md#best-practices) ve [performansla ilgili önemli noktalar](backup-azure-vms-introduction.md#backup-performance)

## <a name="vm-agent"></a>VM Aracısı
### <a name="set-up-the-vm-agent"></a>VM aracısını ayarlama
Genellikle, VM Aracısı zaten Azure galerisinden oluşturulan VM'ler bulunur. Ancak, sanal makineler, şirket içi veri merkezlerinden geçişi, VM aracısı yüklü olmaz. Bu sanal makineler için VM Aracısı açıkça yüklü olması gerekir.

#### <a name="windows-vms"></a>Windows VM'leri

* [Aracı MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için yönetici ayrıcalıkları gerekir.
* Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için [VM özelliğini güncelleştirin](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) aracının yüklü olduğunu belirtmek için. Bu adım, Azure Resource Manager sanal makineler için gerekli değildir.

#### <a name="linux-vms"></a>Linux VM'leri

* Dağıtım depodan aracının en son sürümünü yükleyin. Paket adı hakkında daha fazla bilgi için bkz: [Linux Aracısı depo](https://github.com/Azure/WALinuxAgent).
* Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için [bu blog kullanın](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) VM özelliğini güncelleştirin ve aracının yüklü olduğunu doğrulayın. Bu adım, Resource Manager sanal makineler için gerekli değildir.

### <a name="update-the-vm-agent"></a>VM aracısını güncelleştirme
#### <a name="windows-vms"></a>Windows VM'leri

* VM aracısını güncelleştirmek için yeniden [VM Aracısı ikili dosyalarının](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Aracıyı güncelleştirmeden önce VM Aracısı güncelleştirilirken herhangi bir yedekleme işlemleri ortaya emin olun.

#### <a name="linux-vms"></a>Linux VM'leri

* Linux VM aracısını güncelleştirmek için makaledeki yönergeleri [Linux VM Aracısı güncelleştirilirken](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

    > [!NOTE]
    > Her zaman aracısını güncelleştirmek için dağıtım depoyu kullanın.

    Aracı kodu Github'dan yüklemeyin. En son aracı dağıtımınız için kullanılabilir değilse, en son aracıyı almak yönergeler dağıtım desteği başvurun. En son de göz atabilirsiniz [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) GitHub deposundaki bilgiler.

### <a name="validate-vm-agent-installation"></a>VM Aracısı yüklemesini doğrula

Windows vm'lerinde VM Aracısı'nın sürümünü doğrulayın:

1. Azure sanal makinede oturum açın ve klasöre gidin **C:\WindowsAzure\Packages**. Bulmanız gerekir **WaAppAgent.exe** dosya.
2. Dosyaya sağ tıklayın ve Git **özellikleri**. Ardından **ayrıntıları** sekmesi. **Ürün sürümü** alanı 2.6.1198.718 olmalıdır veya üzeri.

## <a name="troubleshoot-vm-snapshot-issues"></a>VM anlık görüntüsü sorunları giderme
VM yedekleme, temel alınan depolama alanı için anlık görüntü komutları verme üzerinde kullanır. Depolama veya anlık görüntü görevi gecikme çalıştırma erişimi olmaması, yedekleme işinin başarısız olmasına neden olabilir. Aşağıdaki koşullar anlık görüntü görevi başarısız olmasına neden olabilir:

- **NSG kullanarak depolamaya ağ erişimi engellendi**. Hakkında daha fazla bilgi için bilgi [ağ erişim](backup-azure-arm-vms-prepare.md#establish-network-connectivity) kullanarak ya da izin verilenler IP'lerin veya bir ara sunucu üzerinden depolama.
- **Yapılandırılmış SQL Server Yedekleme ile VM anlık görüntü görevi gecikmesi neden olabilecek**. Varsayılan olarak, VM yedekleme VSS bir tam yedekleme Windows Vm'lerinde oluşturur. SQL Server Yedekleme yapılandırıldı, SQL Server çalıştıran sanal makineler, anlık görüntü gecikmeler oluşabilir. Anlık görüntü gecikmeler yedekleme hatasına neden olursa, aşağıdaki kayıt defteri anahtarını ayarlayın:

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```

- **VM durumu VM'ye RDP kapatılmış olduğundan yanlış bildirilir**. Sanal makineyi kapatmak için Uzak Masaüstü kullandıysanız, portaldaki VM durumu doğru olduğunu doğrulayın. Durumu doğru değilse kullanın **kapatma** portalı sanal makine Panosu'ndan sanal makineyi seçeneği.
- **Dörtten fazla Vm'leri paylaşımı aynı bulut hizmeti arasında birden çok yedekleme İlkesi, VM'ler yayılan**. Yedekleme zamanları, bu nedenle hiçbir dörtten fazla VM yedeklemeleri aynı anda başlatmak kademelendirebilirsiniz. İlkeleri başlangıç zamanlarını en az bir saat olarak ayırmak deneyin.
- **Yüksek CPU veya bellek VM çalışan**. Yüksek bellek veya CPU kullanımı yüzde 90'dan daha fazla sanal makine çalışıyorsa, anlık görüntü görevi sıraya alındı ve Gecikmeli. Sonunda, zaman aşımına uğrar. Bu sorun ortaya çıkar, isteğe bağlı yedekleme deneyin.

## <a name="networking"></a>Ağ
Tüm uzantıları gibi yedekleme uzantıları çalışması için genel internet erişimi gerekir. Genel İnternet'e erişim olmaması kendisini çeşitli şekillerde bildirilebilir:

* Uzantı yüklemesi başarısız olabilir.
* Disk anlık görüntü gibi yedekleme işlemleri başarısız olabilir.
* Yedekleme işlemi durumunu görüntüleme başarısız olabilir.

Genel internet adresleri çözmek için gereken içinde ele alınmıştır [bu Azure destek blogu](https://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Sanal ağ için DNS yapılandırmalarını kontrol edin ve Azure URI'ler çözülebilir emin olun.

Ad çözümlemesi doğru yapıldıktan sonra Azure IP'ler için erişim de sağlanması gerekir. Azure altyapı erişim engelini kaldırmak için aşağıdaki adımlardan birini izleyin:

- Azure veri merkezi IP aralıkları listesi izin ver:
   1. ' Nın listesini alın [Azure datacenter IP'leri](https://www.microsoft.com/download/details.aspx?id=41653) izin olması için liste.
   1. Kullanarak IP'ler engellemesini [New-NetRoute](https://docs.microsoft.com/powershell/module/nettcpip/new-netroute) cmdlet'i. Bu cmdlet Azure VM'de yükseltilmiş bir PowerShell penceresinde çalıştırın. Yönetici olarak çalıştırın.
   1. IP'ler için erişime izin vermek için bir yerde varsa NSG kuralları ekleyin.
- Akışı HTTP trafiği için bir yol oluşturun:
   1. Yerinde bazı ağ kısıtlaması varsa, trafiği yönlendirmek için bir HTTP proxy sunucusu dağıtın. Bir ağ güvenlik grubu buna bir örnektir. Bir HTTP Ara sunucusunu dağıtmak için adımları görmek [ağ bağlantısı kurma](backup-azure-arm-vms-prepare.md#establish-network-connectivity).
   1. Bir yerinde, internet'e HTTP Ara sunucuya erişim izni yoksa NSG kuralları ekleyin.

> [!NOTE]
> DHCP çalışmak için konuk içinde Iaas VM yedekleme için etkinleştirilmesi gerekir. Statik özel IP gerekiyorsa, Azure portal veya PowerShell yapılandırın. VM içindeki DHCP seçeneği etkin olduğundan emin olun.
> PowerShell aracılığıyla statik bir IP ayarlama hakkında daha fazla bilgi alın:
> - [Mevcut bir VM'ye statik iç IP ekleme](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm)
> - [Bir ağ arabirimine atanan özel bir IP adresi için ayırma yöntemini değiştirme](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface)
>
>
