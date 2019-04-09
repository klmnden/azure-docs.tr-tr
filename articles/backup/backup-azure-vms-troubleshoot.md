---
title: Azure sanal makineleri yedekleme hatalarında sorun giderme
description: Backup ve Azure sanal makinelerini geri yükleme sorunlarını giderme
services: backup
author: srinathv
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: srinathv
ms.openlocfilehash: b8d1152856935c239a59eb9133aaf48d26a5a8b6
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59259958"
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure sanal makine yedekleme sorunlarını giderme
Aşağıdaki tabloda listelenen bilgilerle Azure Backup kullanarak sırasında karşılaşılan hataları giderebilirsiniz:

| Hata Ayrıntıları | Geçici çözüm |
| ------ | --- |
| Sanal makine (VM) artık mevcut olmadığından yedekleme işlemi özel durumu alınamadı: <br>Yedekleme verilerini silmeden sanal makineyi korumayı durdurun. Daha fazla bilgi için [sanal makineleri korumayı durdurun](https://go.microsoft.com/fwlink/?LinkId=808124). |Birincil VM silindiği ancak yedekleme ilkesini yedeklemek için bir VM için yine de görünür. Bu hata oluşuyor. Bu hatayı düzeltmek için aşağıdaki adımları uygulayın: <ol><li> Sanal makine ile aynı ada ve aynı kaynak grubu adı, yeniden oluşturma **bulut hizmeti adı**,<br>**or**</li><li> İle veya olmadan, yedekleme verilerini silmeden sanal makine korumayı durdurun. Daha fazla bilgi için [sanal makineleri korumayı durdurun](https://go.microsoft.com/fwlink/?LinkId=808124).</li></ol> |
| Azure sanal makine Aracısı (VM Aracısı), Azure Backup hizmeti ile iletişim kuramıyor: <br>VM'nin ağ bağlantısı olduğundan ve en son VM Aracısı olduğundan emin olun ve çalıştırma. Daha fazla bilgi için [Azure yedekleme sorunlarını giderme hata: Aracı veya uzantı sorunları](https://go.microsoft.com/fwlink/?LinkId=800034). |VM Aracısı ile ilgili bir sorun varsa veya herhangi bir şekilde Azure altyapısı için ağ erişim engellendi, bu hata oluşur. Daha fazla bilgi edinin [VM hata ayıklama anlık görüntüsü sorunları](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup). <br><br>VM Aracısı sorunlarına neden olmaktan değil, VM'yi yeniden başlatın. Hatalı bir VM durumu sorunlarına neden olabilir ve VM'nin yeniden başlatılmasının durumunu sıfırlar. |
| VM başarısız sağlama durumunda olup: <br>VM'yi yeniden başlatın ve VM'nin çalıştığından emin olun veya kapatma. | Uzantı hataları birini VM başarısız sağlama durumu koyarsa bu hata oluşur. Başarısız bir uzantısı varsa, kaldırana uzantılar listesine denetleyin ve sanal makineyi yeniden başlatarak gidin. Tüm uzantıları çalışır durumda olduğundan, VM aracısı hizmetinin çalışıp çalışmadığını denetleyin. Aksi durumda, VM Aracısı hizmetini yeniden başlatın. |
| Yedekleme, depolama hesabında yeterli boş alan nedeniyle sanal makinenin anlık görüntüsünü kopyalayın alınamadı: <br>Depolama hesabı'nın sanal makineye eklenen premium depolama disklerini mevcut verilere eşit boş alan olduğundan emin olun. | VM yedekleme yığını V1 üzerinde Premium VM'ler için şu anlık görüntü depolama hesabına kopyalayın. Bu adım, bir anlık görüntü üzerinde çalışır, yedekleme yönetim trafiği, premium diskler kullanarak bir uygulama için kullanılabilir IOPS sayısını kısıtlamaz emin olur. <br><br>Yalnızca yüzde 50, toplam depolama hesabı alanının 17,5 TB ayırmanız önerilir. Daha sonra kasa için depolama hesabındaki kopyalanan bu konumdan Azure Backup hizmeti anlık görüntü depolama hesabı ve aktarım veri kopyalayabilirsiniz. |
| VM aracısı yanıt değil. yedekleme işlemi gerçekleştirilemiyor. |VM Aracısı ile ilgili bir sorun varsa veya herhangi bir şekilde Azure altyapısı için ağ erişim engellendi, bu hata oluşur. Windows Vm'leri için VM aracısı hizmet durumunu services'ı ve aracıyı Denetim Masası'ndaki Programlar görüntülenip denetleyin. <br><br>Deneyin program Denetim Masası'ndan kaldırıp aracı açıklandığı [VM Aracısı](#vm-agent). Aracıyı yeniden yükledikten sonra doğrulamak için geçici bir yedeklemeyi tetikleyin. |
| Kurtarma Hizmetleri Uzantısı işlemi başarısız oldu: <br>En son VM Aracısı sanal makinede mevcut olduğundan ve VM aracısı hizmetinin çalıştığından emin olun. Yedekleme işlemini yeniden deneyin. Yedekleme işlemi başarısız olursa, Microsoft Support başvurun. |VM aracısının güncel olduğunda bu hata oluşur. VM aracısını güncelleştirmek için sorun giderme Azure sanal makine yedekleme bakın. |
| Sanal makine yok: <br>Sanal makinenin var olduğundan emin olun veya farklı bir sanal makine seçin. |Birincil VM silindiği ancak yedekleme ilkesini yedeklemek için bir VM için yine de görünür. Bu hata oluşur. Bu hatayı düzeltmek için aşağıdaki adımları uygulayın: <ol><li> Sanal makine ile aynı ada ve aynı kaynak grubu adı, yeniden oluşturma **bulut hizmeti adı**,<br>**or**<br></li><li>Yedekleme verilerini silmeden sanal makineyi korumayı durdurun. Daha fazla bilgi için [sanal makineleri korumayı durdurun](https://go.microsoft.com/fwlink/?LinkId=808124).</li></ol> |
| Komut başarısız oldu: <br>Başka bir işlem, bu öğe üzerinde şu anda sürüyor. Önceki işlem tamamlanana kadar bekleyin. Ardından, işlemi yeniden deneyin. |Var olan bir yedekleme işi çalıştığından ve geçerli iş tamamlanana kadar yeni bir iş yeniden başlatılamıyor. |
| Kurtarma Hizmetleri kasası, zaman aşımına uğradı kopyalama Vhd'lerden: <br>Birkaç dakika sonra işlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. | Depolama tarafında geçici bir hata varsa veya yedekleme hizmeti yeterli depolama hesabı zaman aşımı süresi içinde Kasası'na veri aktarmak için IOPS almazsa, bu hata oluşur. Takip ettiğinizden emin olun [en iyi uygulamalar, Vm'lerinizin yapılandırırken](backup-azure-vms-introduction.md#best-practices). Yüklü değilse farklı bir depolama hesabı için sanal makinenizin taşıyın ve yedekleme işini yeniden deneyin.|
| Yedekleme bir iç hatayla başarısız oldu: <br>Birkaç dakika sonra işlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. |İki nedenden dolayı bu hatayı alırsınız: <ul><li> VM depolama erişmede geçici bir sorun yoktur. Denetleme [Azure durumu site](https://azure.microsoft.com/status/) işlem, depolama veya ağ sorunları bölgesinde olup olmadığını görmek için. Sorun çözümlendikten sonra yedekleme işini yeniden deneyin. <li> Özgün VM silinmiştir ve kurtarma noktası alınamaz. Silinen bir VM için yedekleme verileri tutmak, ancak yedekleme hataları kaldırmak için VM korumasını kaldırın ve verileri korumak için bu seçeneği belirleyin. Bu eylem, zamanlanmış bir yedekleme işi ve yinelenen hata iletileri durdurur. |
| Yedeklemek için Azure kurtarma Hizmetleri Uzantısı seçili öğeye yüklenemedi: <br>VM Aracısı, Azure kurtarma Hizmetleri uzantısı için bir önkoşuldur. Azure sanal makine Aracısı yükleme ve kayıt işlemi yeniden başlatın. |<ol> <li>VM Aracısı düzgün yüklü olup olmadığını denetleyin. <li>VM yapılandırması bayrağı doğru ayarlandığından emin olun.</ol> VM aracısı ve VM Aracısı yüklemesini doğrulamak nasıl yükleme hakkında daha fazla bilgi edinin. |
| Uzantı yüklemesi başarısız oldu, hata **COM + Microsoft Dağıtılmış İşlem Düzenleyicisi ile iletişim kuramıyor**. |Bu hata genellikle COM + hizmet çalışmadığından emin anlamına gelir. Bu sorunu çözme konusunda yardım için Microsoft Support başvurun. |
| Anlık görüntü işlemi başarısız oldu, Birim Gölge Kopyası Hizmeti (VSS) işlemi hata **Bu sürücü BitLocker Sürücü Şifrelemesi tarafından kilitlendi. Denetim Masası'ndan bu sürücünün kilidini açmanız gerekir.** |Sanal makinedeki tüm sürücüleri için BitLocker'ı açın ve VSS sorunun çözülüp çözülmediğine bakın. |
| VM yedeklemeleri izin veren bir durumda değil. |<ul><li>VM arasında geçici bir durumda ise **çalıştıran** ve **kapatma**, bekleme durumu değiştirmek. Ardından, yedekleme işini tetikler. <li> VM bir Linux VM ve Security-Enhanced Linux çekirdek modülü kullanır, Azure Linux Aracısı yolu dışla **/var/lib/waagent** güvenlik ilkesi ve emin olun, Azure yedekleme uzantısı yüklenir.  |
| Bir Azure sanal makinesi bulunamadı. |Birincil VM silindiği ancak yedekleme ilkesi için silinen sanal Makinenin yine de görünür. Bu hata oluşur. Aşağıdaki şekilde bu hatayı düzeltmek: <ol><li>Sanal makine ile aynı ada ve aynı kaynak grubu adı, yeniden oluşturma **bulut hizmeti adı**, <br>**or** <li> Yedekleme işleri oluşturulmaz bu nedenle bu VM için korumayı devre dışı bırakın. </ol> |
| VM Aracısı sanal makinede mevcut değil: <br>Tüm önkoşul ve VM aracısını yükleyin. Ardından, işlemi yeniden başlatın. |Daha fazla bilgi edinin [VM aracısını yükleme ve VM Aracısı yüklemesini doğrulamak nasıl](#vm-agent). |
| VSS yazıcılarının hatalı durumda olduğundan anlık görüntü işlemi başarısız oldu. |Kötü durumdaki VSS yazıcıları yeniden başlatın. Yükseltilmiş bir komut isteminden çalıştırın ```vssadmin list writers```. Çıktı, tüm VSS yazıcılarının ve bunların durumunu içerir. Her VSS yazıcısının olmayan durumuna sahip **[1] kararlı**, VSS Yazıcı yeniden, yükseltilmiş bir komut isteminden aşağıdaki komutları çalıştırın: <ol><li>```net stop serviceName``` <li> ```net start serviceName```</ol>|
| Anlık görüntü işlemi, bir yapılandırma ayrıştırma hatası nedeniyle başarısız oldu. |Bu hata, üzerinde değiştirilen izinler nedeniyle oluşur. **MachineKeys** dizin: **%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**. <br> Aşağıdaki komutu çalıştırın ve doğrulayın, izinleri **MachineKeys** directory varsayılan değerleri şunlardır:<br>**ICACLS %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**. <br><br>Varsayılan izinler şunlardır: <ul><li>Herkesin: (R, G) <li>BUILTIN\Administrators: (F)</ul> İzinler görürseniz **MachineKeys** varsayılan farklı bir dizin izinlerini düzeltin, sertifikayı silin ve yedeklemeyi tetiklemek için aşağıdaki adımları izleyin: <ol><li>İzinleri düzelt **MachineKeys** dizin. Dizinde Explorer güvenlik özellikleri ve Gelişmiş güvenlik ayarları kullanarak, izinler varsayılan değerlere sıfırlayın. Tüm kullanıcı nesnelerini hariç Varsayılanları dizininden ve emin Kaldır **herkes** izni olan özel erişim gibi: <ul><li>Liste klasörü/veri okuma <li>Okuma öznitelikleri <li>Okuma genişletilmiş öznitelikleri <li>Dosya Oluştur/Veri Yaz <li>Klasör Oluştur/Veri Ekle<li>Yazma öznitelikleri<li>Genişletilmiş öznitelikleri yazma<li>Okuma izinleri </ul><li>Tüm sertifikaları sil burada **çıkarılan** Klasik dağıtım modeli veya **Windows Azure CRP sertifika Oluşturucu**:<ol><li>[Sertifika yerel bilgisayar konsolu açın](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx).<li>Altında **kişisel** > **sertifikaları**, tüm sertifikaları sil burada **çıkarılan** Klasik dağıtım modeli veya **Windows Azure CRP Sertifika Oluşturucu**.</ol> <li>VM yedekleme işini tetikler. </ol>|
| Azure Backup hizmeti için yeterli izinlerin Azure anahtar kasası için yedekleme şifrelenmiş sanal makinelerin sahip değil. |Yedekleme hizmeti adımları kullanarak bu izinleri PowerShell'de sağlamak [geri yüklenen diskten VM oluşturma](backup-azure-vms-automation.md). |
|Anlık görüntü uzantısını yükleme işlemi başarısız oldu, hata **COM + Microsoft Dağıtılmış İşlem Düzenleyicisi ile iletişim kuramıyor**. | Windows hizmeti yükseltilmiş bir komut isteminden başlatmak **COM + System Application**. Bir örnek **net Başlat COMSysApp**. Başlatmak hizmet başarısız olursa, ardından aşağıdaki adımları uygulayın:<ol><li> Hizmetin oturum açma hesabı olduğundan emin olun **Dağıtılmış İşlem Düzenleyicisi** olduğu **ağ hizmeti**. Yüklü değilse, oturum açma hesabına geçemeyeceğinizi **ağ hizmeti** ve hizmeti yeniden başlatın. Başlatmayı denerseniz **COM + System Application**.<li>Varsa **COM + System Application** olmaz başlatın, kaldırma ve hizmeti yüklemek için aşağıdaki adımları **Dağıtılmış İşlem Düzenleyicisi**: <ol><li>MSDTC hizmetini durdurun. <li>Bir komut istemi açın **cmd**. <li>Komutunu çalıştırın ```msdtc -uninstall```. <li>Komutunu çalıştırın ```msdtc -install```. <li>MSDTC hizmetini başlatın. </ol> <li>Windows hizmeti başlatın **COM + System Application**. Sonra **COM + System Application** başladığında, Azure portalında yedekleme işini tetikler.</ol> |
|  Anlık görüntü işlemi, bir COM + hatası nedeniyle başarısız oldu. | Windows hizmetini yeniden başlatmanızı öneririz **COM + System Application** yükseltilmiş bir komut isteminden **net Başlat COMSysApp**. Sorun devam ederse, VM'yi yeniden başlatın. VM'nin yeniden başlatılmasının yaramazsa deneyin [VMSnapshot uzantısını kaldırma](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout) ve el ile yedekleme tetikleyin. |
| Yedekleme, bir veya daha fazla bağlama noktaları sanal dosya sistemi tutarlı bir anlık görüntüsünü almak için dondurulamadı. | Aşağıdaki adımları izleyin: <ul><li>Kullanarak tüm bağlı cihazları dosya sistem durumunu kontrol **'tune2fs'** komutu. Bir örnek **tune2fs -l/dev/sdb1 \\** .\| grep **dosya sistemi durumu**. <li>Kendisi için dosya sistemi durumu olmayan temiz kullanarak cihazları çıkarın **'umount'** komutu. <li> Bu cihazlarda dosya sistemi tutarlılık denetimi çalıştırmak **'fsck'** komutu. <li> Cihazları yeniden bağlayın ve yedeklemeyi deneyin.</ol> |
| Anlık görüntü işlemi, bir güvenli ağ iletişim kanalı oluşturma hatası nedeniyle başarısız oldu. | <ol><li> Kayıt Defteri Düzenleyicisi'ni çalıştırarak açmak **regedit.exe** yükseltilmiş modda. <li> Sisteminizde mevcut .NET Framework'ün tüm sürümler tanımlayın. Kayıt defteri anahtarının hiyerarşisi altında mevcut oldukları **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft**. <li> Kayıt defteri anahtarı mevcut her .NET Framework için aşağıdaki anahtarını ekleyin: <br> **SchUseStrongCrypto"=dword:00000001**. </ol>|
| Visual Studio 2012 için Visual C++ yeniden dağıtılabilir yükleme hatası nedeniyle anlık görüntü işlemi başarısız. | İçin C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion gidin ve vcredist2012_x64 yükleyin. Bu hizmeti yüklemesi sağlayan kayıt defteri anahtar değeri doğru değerine ayarlanmış olduğundan emin olun. Diğer bir deyişle, kayıt defteri anahtarı değerini **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSIServer** ayarlanır **3** değil **4**. <br><br>Yükleme sorunlarını hala varsa, yükleme hizmeti çalıştırarak yeniden **MSIEXEC/unregister** ardından **MSIEXEC /REGISTER** yükseltilmiş bir komut isteminden.  |


## <a name="jobs"></a>İşler

| Hata Ayrıntıları | Geçici çözüm |
| --- | --- |
| İptal, bu proje türü için desteklenmez: <br>İş tamamlanana kadar bekleyin. |None |
| İş, iptal edilebilir bir durumda değil: <br>İş tamamlanana kadar bekleyin. <br>**or**<br> Seçili iş, iptal edilebilir bir durumda değil: <br>İşin tamamlanmasını bekleyin. |Neredeyse tamamlanmış iş olduğundan emin olma olasılığı yüksektir. İş tamamlanana kadar bekleyin.|
| Yedekleme, devam eden olmadığından iş iptal edilemiyor: <br>İptal, yalnızca devam eden işler için desteklenir. Devam eden işi iptal etmeyi deneyin. |Bu hata geçici bir durum nedeniyle oluşur. Bir dakika bekleyin ve İptal işlemi yeniden deneyin. |
| Yedekleme işi iptal edilemedi: <br>İş tamamlanana kadar bekleyin. |None |

## <a name="restore"></a>Geri Yükleme

| Hata Ayrıntıları | Geçici çözüm |
| --- | --- |
| Geri yükleme, bir bulut iç hatayla başarısız oldu. |<ol><li>Geri yükleme çalıştığınız bulut hizmeti DNS ayarlarıyla yapılandırılır. Kontrol edebilirsiniz: <br>**$deployment = get-AzureDeployment - ServiceName "ServiceName"-yuvası "Üretim" Get-AzureDns - DnsSettings $deployment. DnsSettings**.<br>Varsa **adresi** DNS ayarlarını yapılandırıldıysa, yapılandırılır.<br> <li>Yapılacak çalıştığınız geri yüklemek bulut hizmeti ile yapılandırılmış **ReservedIP**, ve bulut hizmetindeki var olan VM'ler durdurulmuş durumda. Bir bulut hizmeti aşağıdaki PowerShell cmdlet'lerini kullanarak bir IP ayırdığı denetleyebilirsiniz: **$deployment = Get-AzureDeployment - ServiceName "servicename"-"Üretim" $yuvası DEP Reservedıpname**. <br><li>Bir sanal makineyle aynı bulut hizmetine aşağıdaki özel ağ yapılandırmalarını geri yüklemek çalışıyorsunuz: <ul><li>Sanal makineleri yük dengeleyici yapılandırmasını, iç ve dış altında.<li>Birden çok ayrılmış IP ile sanal makineler. <li>Birden çok NIC içeren sanal makineler. </ul><li>Kullanıcı Arabirimi veya bkz: yeni bir bulut hizmeti seçin [konuları geri](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations) özel ağ yapılandırmaları olan VM'ler için.</ol> |
| Seçilen DNS adı zaten alınmış: <br>Farklı bir DNS adı belirtip yeniden deneyin. |Bu DNS adı genellikle ile biten bulut hizmeti adı başvurduğu **. cloudapp.net**. Bu adın benzersiz olması gerekir. Bu hatayı alırsanız, geri yükleme sırasında farklı bir VM adı seçmeniz gerekir. <br><br> Bu hata, yalnızca Azure portalı kullanıcılarına gösterilir. Yalnızca diskleri geri yükler ve VM'yi oluşturmak değildir çünkü PowerShell aracılığıyla geri yükleme işlemi başarılı olur. Diski geri yükleme işleminden VM açıkça sizin tarafınızdan oluşturulduğunda hata karşılaştığı. |
| Belirtilen sanal ağ yapılandırması doğru değil: <br>Farklı sanal ağ yapılandırması belirtin ve yeniden deneyin. |None |
| Belirtilen bulut hizmeti, geri yüklenen sanal makine yapılandırmasını eşleşmeyen bir ayrılmış IP kullanıyor: <br>Ayrılmış IP kullanmıyor farklı bir bulut hizmeti belirtin. Veya geri yükleme için başka bir kurtarma noktası seçin. |None |
| Bulut hizmetine giriş uç noktaları sayısı sınırına ulaştı: <br>İşlemi farklı bir bulut hizmeti belirterek veya var olan bir uç nokta kullanarak yeniden deneyin. |None |
| Kurtarma Hizmetleri kasası ve hedef depolama hesabı, iki farklı bölgede şunlardır: <br>Geri yükleme işleminde belirtilen depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı Azure bölgesinde olduğundan emin olun. |None |
| Geri yükleme işlemi için belirtilen depolama hesabı desteklenmiyor: <br>Yerel olarak yedekli veya coğrafi olarak yedekli çoğaltma ayarlarıyla yalnızca temel veya standart depolama hesapları desteklenmektedir. Desteklenen bir depolama hesabı seçin. |None |
| Çevrimiçi geri yükleme işlemi için belirtilen depolama hesabı türü değil: <br>Geri yükleme işleminde belirtilen depolama hesabı çevrimiçi olduğundan emin olun. |Bu hata, Azure Depolama'daki geçici bir hata nedeniyle veya kesinti nedeniyle gerçekleşebilir. Başka bir depolama hesabı seçin. |
| Kaynak grubu kotasına ulaşıldı: <br>Bazı kaynak gruplarını, Azure portalından silin veya limitleri artırmak için Azure desteğine başvurun. |None |
| Seçilen alt ağ yok: <br>Var olan bir alt ağ seçin. |None |
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

- **NSG kullanarak depolamaya ağ erişimi engellendi**. Hakkında daha fazla bilgi için bilgi [ağ erişim](backup-azure-arm-vms-prepare.md#establish-network-connectivity) kullanarak her iki beyaz IP'lerin veya bir ara sunucu üzerinden depolama.
- **Yapılandırılmış SQL Server Yedekleme ile VM anlık görüntü görevi gecikmesi neden olabilecek**. Varsayılan olarak, VM yedekleme VSS bir tam yedekleme Windows Vm'lerinde oluşturur. SQL Server Yedekleme yapılandırıldı, SQL Server çalıştıran sanal makineler, anlık görüntü gecikmeler oluşabilir. Anlık görüntü gecikmeler yedekleme hatasına neden olursa, aşağıdaki kayıt defteri anahtarını ayarlayın:

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```

- **VM durumu VM'ye RDP kapatılmış olduğundan yanlış bildirilir**. Sanal makineyi kapatmak için Uzak Masaüstü kullandıysanız, portaldaki VM durumu doğru olduğunu doğrulayın. Durumu doğru değilse kullanın **kapatma** portalı sanal makine Panosu'ndan sanal makineyi seçeneği.
- **Dörtten fazla Vm'leri paylaşımı aynı bulut hizmeti arasında birden çok yedekleme İlkesi, VM'ler yayılan**. Yedekleme zamanları, bu nedenle hiçbir dörtten fazla VM yedeklemeleri aynı anda başlatmak kademelendirebilirsiniz. İlkeleri başlangıç zamanlarını en az bir saat olarak ayırmak deneyin.
- **Yüksek CPU veya bellek VM çalışan**. Yüksek bellek veya CPU kullanımı yüzde 90'dan daha fazla sanal makine çalışıyorsa, anlık görüntü görevi sıraya alındı ve Gecikmeli. Sonunda, zaman aşımına uğrar. Bu sorun ortaya çıkar, isteğe bağlı yedekleme deneyin.

## <a name="troubleshoot-backup-of-encrypted-vms"></a>Şifrelenmiş Vm'leri yedekleme sorunlarını giderme

### <a name="azure-backup-doesnt-have-permissions-for-key-vault-access"></a>Azure yedekleme, anahtar kasası erişim izinleri yok
- **Hata kodu**: UserErrorKeyVaultPermissionsNotConfigured
- **Hata iletisi**: Azure Backup hizmeti yedekleme, şifrelenmiş sanal makineleri için yeterli izinlere için Key Vault yok.
- **Çözüm**: Anahtar Kasası'nda Azure Backup izinleri atamanız [portalı](backup-azure-vms-encryption.md#provide-permissions), veya [PowerShell](backup-azure-vms-automation.md#enable-protection)

### <a name="the-vm-cant-be-restored-because-the-associated-key-vault-doesnt-exist"></a>İlişkili anahtar kasası olmadığından VM geri yüklenemiyor
- **Çözüm**: Emin olun [bir Key Vault oluşturdunuz](../key-vault/quick-create-portal.md#create-a-vault).
- **Çözüm**: İzleyin [bu yönergeleri](backup-azure-restore-key-secret.md) bir anahtar ve gizli anahtar Kasası'nda mevcut olmaması durumunda geri yüklemek için.

### <a name="the-vm-cant-be-restored-because-the-associated-key-doesnt-exist"></a>VM ilişkili anahtar mevcut olmadığından geri yüklenemiyor
- **Hata kodu**: UserErrorKeyVaultKeyDoesNotExist
- **Hata iletisi**: Bu VM ile ilişkili anahtar mevcut olmadığından bu şifreli VM'yi geri yükleyemezsiniz.
- **Çözüm**: İzleyin [bu yönergeleri](backup-azure-restore-key-secret.md) bir anahtar ve gizli anahtar Kasası'nda mevcut olmaması durumunda geri yüklemek için.

### <a name="the-vm-cant-be-restored-because-azure-backup-doesnt-have-authorization"></a>Azure Backup yetkilendirme olmadığı için sanal Makineyi geri yüklenemiyor
- **Hata kodu**: ProviderAuthorizationFailed/UserErrorProviderAuthorizationFailed
- **Hata iletisi**: Yedekleme Hizmeti'nin aboneliğinizdeki kaynaklara erişme yetkisi yok.
- **Çözüm**: Disklerin, önerildiği gibi geri yükleyin. [Daha fazla bilgi edinin](backup-azure-vms-encryption.md#restore-an-encrypted-vm). 


## <a name="networking"></a>Ağ
Tüm uzantıları gibi Azure Backup uzantısının çalışması için genel internet erişimi olmalıdır. Genel İnternet'e erişim olmaması kendisini çeşitli şekillerde bildirilebilir:

* Uzantı yüklemesi başarısız olabilir.
* Disk anlık görüntü gibi yedekleme işlemleri başarısız olabilir.
* Yedekleme işlemi durumunu görüntüleme başarısız olabilir.

Genel internet adresleri çözmek için gereken içinde ele alınmıştır [bu Azure destek blogu](https://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Sanal ağ için DNS yapılandırmalarını kontrol edin ve Azure URI'ler çözülebilir emin olun.

Ad çözümlemesi doğru yapıldıktan sonra Azure IP'ler için erişim de sağlanması gerekir. Azure altyapı erişim engelini kaldırmak için aşağıdaki adımlardan birini izleyin:

- Beyaz liste Azure veri merkezi IP aralıkları:
   1. ' Nın listesini alın [Azure datacenter IP'leri](https://www.microsoft.com/download/details.aspx?id=41653) Güvenilenler listesine eklenmek için.
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
