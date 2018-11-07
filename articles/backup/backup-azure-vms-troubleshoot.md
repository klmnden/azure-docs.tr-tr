---
title: Azure sanal makine yedekleme sorunlarını giderme
description: Backup ve Azure sanal makinelerini geri yükleme sorunlarını giderme
services: backup
author: trinadhk
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 8/7/2018
ms.author: trinadhk
ms.openlocfilehash: 90e03c66717cafc1cd33f4629e88aba8e76c2c3f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245757"
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure sanal makine yedekleme sorunlarını giderme
Aşağıdaki tabloda listelenen bilgilerle Azure Backup kullanarak sırasında karşılaşılan hataları giderebilirsiniz.

| Hata Ayrıntıları | Geçici çözüm |
| --- | --- |
| VM artık mevcut olmadığından işlem gerçekleştirilemiyor. -Yedekleme verilerini silmeden sanal makine korumayı durdurun. Daha fazla bilgi http://go.microsoft.com/fwlink/?LinkId=808124 |Bu, birincil sanal makine silinmiş, ancak yedekleme ilkesini sanal Makineyi yedeklemek için arama devam gerçekleşir. Bu hatayı düzeltmek için: <ol><li> Sanal makine ile aynı ada ve aynı kaynak grubu adı [bulut hizmeti adı] yeniden oluşturun<br>(VEYA)</li><li> Sanal makine içeren veya içermeyen yedekleme verilerini silme korumayı durdurun. [Daha fazla ayrıntı](https://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| -Sanal makinede ağ bağlantısı olmaması nedeniyle başarısız oldu. anlık görüntü işlemi, VM ağ erişimi bulunduğundan emin olun. Başarılı olması anlık görüntü için ya da güvenilir Azure veri merkezi IP aralıkları veya ağ erişimi için bir proxy sunucusu ayarlayın. Daha fazla bilgi için http://go.microsoft.com/fwlink/?LinkId=800034. Proxy sunucusu kullanıyorsanız, proxy sunucu ayarlarının doğru yapılandırıldığından emin olun. | Sanal makineye giden internet bağlantısı Reddet oluşur. VM anlık görüntü uzantısı, temel alınan diskler bir anlık görüntüsünü almak için internet bağlantısı gerektirir. [Engellenen ağ erişim nedeniyle anlık görüntü hataları düzeltmeye bakın](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine). |
| VM Aracısı Azure Backup hizmeti ile iletişim kuramıyor. -VM'nin ağ bağlantısı olduğundan ve VM aracısını en son ve çalışıyor olduğundan emin olun. Daha fazla bilgi için makaleye bakın http://go.microsoft.com/fwlink/?LinkId=800034. |Bu hata, VM Aracısı ile ilgili bir sorun veya başka bir yolla Azure altyapısı için ağ erişimi engellendi oluşturulur. [Daha fazla bilgi edinin](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup) sorunları VM'yi hata ayıklama hakkında anlık görüntüsünü alın.<br> VM Aracısı sorunlarına neden olmaktan değil, VM'yi yeniden başlatın. Hatalı bir VM durumu sorunlarına neden olabilir ve VM'nin yeniden başlatılmasının durumunu sıfırlar. |
| VM başarısız sağlama durumunda - VM'yi yeniden başlatın ve VM'nin çalıştığından emin olun veya kapatın. | VM başarısız sağlama durumunda olması durumuna uzantıları hataları birini müşteri adayları bu hata oluşur. Uzantılar listesine dönün ve başarısız bir uzantısı olup olmadığını, kaldırmak ve sanal makineyi yeniden başlatmayı deneyin. Tüm uzantıları çalışır durumda olduğundan, VM aracısı hizmetinin çalışıp çalışmadığını denetleyin. Aksi durumda, VM Aracısı hizmetini yeniden başlatın. |
| VMSnapshot uzantısı işlemi başarısız oldu - yönetilen diskleri yedekleme işlemini yeniden deneyin. Sorun devam ederse konumundaki yönergeleri 'http://go.microsoft.com/fwlink/?LinkId=800034'. Sorun devam ederse Microsoft desteğine başvurun. | Backup hizmeti anlık görüntüsünü tetiklemek başarısız olduğunda bu hata oluşur. [Daha fazla bilgi edinin](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#ExtentionOperationFailed-vmsnapshot-extension-operation-failed) sorunları VM hata ayıklama hakkında anlık görüntüsünü alın. |
| Kopyalama - depolama hesabında yeterli boş alan sanal makinenin bir anlık görüntü depolama hesabı mevcut sanal makineye bağlı premium depolama disklerindeki verilerle eşit miktarda boş alan olduğundan emin olun | VM yedekleme yığını V1 Premium Vm'lere olması durumunda, biz anlık görüntü depolama hesabına kopyalayın. Bu anlık görüntü üzerinde çalışır, yedekleme yönetim trafiği, premium diskler kullanarak bir uygulama için kullanılabilir IOPS sayısını kısıtlamaz emin olmaktır. Azure Backup hizmeti, kasa için depolama hesabındaki kopyalanan bu konumdan, depolama hesabı ve aktarım veri anlık görüntü kopyalayabilmeniz adına Microsoft yalnızca % 50 (17,5 TB) tahsis toplam depolama hesabı alanı önerir. | 
| VM aracısı yanıt vermediği işlemi gerçekleştirilemiyor. |Bu hata, VM Aracısı ile ilgili bir sorun veya başka bir yolla Azure altyapısı için ağ erişimi engellendi oluşturulur. Windows Vm'leri için VM aracısı hizmet durumunun services'ı ve aracıyı Denetim Masası'ndaki Programlar görüntülenip denetleyin. Program denetiminin kaldırmayı deneyin paneli ve belirtildiği gibi aracıyı yeniden yükleme [aşağıda](#vm-agent). Aracıyı yeniden yükledikten sonra doğrulamak için geçici bir yedeklemeyi tetikleyin. |
| Kurtarma Hizmetleri Uzantısı işlemi başarısız oldu. -En son sanal makine Aracısı sanal makinede mevcut olduğundan emin ve Aracısı hizmeti çalışıyor. Yedekleme işlemini yeniden deneyin. Yedekleme işlemi başarısız olursa Microsoft desteğine başvurun. |Bu hata, VM aracısının güncel olduğunda oluşturulur. VM aracısını güncelleştirmek için aşağıdaki "VM aracısını güncelleştirme" bölümüne bakın. |
| Sanal makine yok. - Veya olduğundan emin olun sanal makinenin var, select farklı bir sanal makine. |Birincil VM silindiği ancak yedekleme için bir VM aramak yedekleme ilkesini devam gerçekleşir. Bu hatayı düzeltmek için: <ol><li> Sanal makine ile aynı ada ve aynı kaynak grubu adı [bulut hizmeti adı] yeniden oluşturun<br>(VEYA)<br></li><li>Yedekleme verilerini silmeden sanal makineyi korumayı durdurun. [Daha fazla ayrıntı](https://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Komut yürütme başarısız oldu. -Başka bir işlem, bu öğe üzerinde şu anda sürüyor. Önceki işlemi tamamlanana kadar bekleyin ve sonra işlemi yeniden deneyin. |Var olan bir yedekleme işi çalıştığından ve geçerli iş tamamlanana kadar yeni bir iş yeniden başlatılamıyor. |
| Recovery Services VHD kopyalama kasa zaman aşımına uğradı - işlemi birkaç dakika içinde yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. | Depolama tarafında geçici bir hata varsa veya yedekleme hizmeti yeterli depolama hesabı zaman aşımı süresi içinde Kasası'na veri aktarmak için IOPS almazsa oluşur. Takip ettiğinizden emin olun [en iyi uygulamalar, Vm'lerinizin yapılandırırken](backup-azure-vms-introduction.md#best-practices). Yüklü değilse farklı bir depolama hesabı için sanal makinenizin taşıyın ve yedekleme işini yeniden deneyin.|
| Yedekleme bir iç hata ile başarısız oldu - işlemi birkaç dakika içinde yeniden deneyin. Sorun devam ederse, Microsoft Support başvurun |İki nedenden dolayı bu hatayı alabilirsiniz: <ol><li> VM depolama erişmede geçici bir sorun yoktur. Denetleme [Azure durumu site](https://azure.microsoft.com/status/) bölgede bilgi işlem, depolama veya ağ sorunları olup olmadığını görmek için. Sorun çözüldükten sonra yedekleme işini yeniden deneyin. <li>Özgün VM silinmiştir ve kurtarma noktası alınamaz. Silinen bir VM için yedekleme verileri tutmak, ancak yedekleme hataları Kaldır: VM korumasını ve verileri korumak için bu seçeneği belirleyin. Bu eylem, zamanlanmış bir yedekleme işi ve yinelenen hata iletileri durdurur. |
| Seçilen öğe üzerinde - Azure kurtarma Hizmetleri Uzantısı yüklenemedi VM Aracısı Azure kurtarma Hizmetleri uzantısı için önkoşuldur. Azure VM aracısını yükleyin ve kayıt işlemini yeniden başlatın |<ol> <li>VM aracısının doğru şekilde yüklenip yüklenmediğini kontrol edin. <li>VM yapılandırması bayrağı doğru ayarlandığından emin olun.</ol> [Daha fazla bilgi edinin](#validating-vm-agent-installation) VM aracısı ve VM Aracısı yüklemesini doğrulamak nasıl yükleneceğine. |
| "COM + Microsoft Distributed Transaction Düzenleyicisi ile konuşamadı uzantısı yükleme işlemi şu hatayla başarısız oldu |Bu, genellikle COM + hizmet çalışmıyor anlamına gelir. Bu sorunu çözme konusunda yardım için Microsoft desteğine başvurun. |
| Anlık görüntü işlemi, "Bu sürücü BitLocker Sürücü Şifrelemesi tarafından kilitlendi. VSS işlem hatasıyla başarısız oldu Denetim Masası'ndan bu sürücünün kilidini açmanız gerekir. |Sanal makinedeki tüm sürücüleri için BitLocker'ı açın ve VSS sorunun çözülüp çözülmediğine gözlemleyin |
| VM yedeklemeleri izin veren bir durumda değil. |<ul><li>VM arasında geçici bir durumda ise **çalıştıran** ve **kapatma**durumu değiştirmek bekleyin ve ardından yedekleme işini tetikleme. <li> VM bir Linux VM ve Gelişmiş Güvenlik Linux çekirdek modülü kullanır, Linux Aracısı yolu hariç tutmak (_/var/lib/waagent_) güvenlik ilkesi ve emin yedekleme uzantısı yüklenir.  |
| Azure sanal makinesi bulunamadı. |Birincil VM silinmiş, ancak yedekleme ilkesi için silinen sanal Makinenin bakarak devam eder. Bu hata oluşur. Bu hatayı düzeltmek için: <ol><li>Sanal makine ile aynı ada ve aynı kaynak grubu adı [bulut hizmeti adı] yeniden oluşturun <br>(VEYA) <li> Yedekleme işleri oluşturulmayacak şekilde bu VM için korumayı devre dışı bırakın. </ol> |
| Sanal makine Aracısı sanal makinede mevcut değil - Lütfen tüm önkoşul ve VM aracısını yükleyin ve ardından işlemi yeniden başlatın. |[Daha fazla bilgi edinin](#vm-agent) VM Aracısı yükleme ve VM Aracısı yüklemesini doğrulamak nasıl hakkında bilgi. |
| VSS yazıcılarının hatalı durumda olduğundan anlık görüntü işlemi başarısız oldu |Kötü durumdaki VSS (birim gölge kopyası hizmeti) yazıcıları yeniden başlatmanız gerekir. Yükseltilmiş bir komut isteminden Bunu başarmak için çalıştırın ```vssadmin list writers```. Çıktı, tüm VSS yazıcılarının ve bunların durumunu içerir. Değil "[1] Stable" durumu, her VSS yazıcısı için VSS Yazıcı, komutları yükseltilmiş komut isteminden aşağıdaki çalıştırarak yeniden başlatın:<br> ```net stop serviceName``` <br> ```net start serviceName```|
| Yapılandırma ayrıştırma hatası nedeniyle anlık görüntü işlemi başarısız oldu |Bu değiştirilen izinleri MachineKeys dizini nedeniyle gerçekleşir: _%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br>Lütfen aşağıdaki komutu çalıştırın ve varsayılan olanları MachineKeys dizin üzerindeki izinleri doğrulayın:<br>_ICACLS %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br><br> Varsayılan izinler şunlardır:<br>Everyone:(R,W) <br>BUILTIN\Administrators:(F)<br><br>İzinleri MachineKeys dizinde varsayılandan farklı görürseniz, lütfen izinleri düzeltmek, sertifikayı silin ve yedeklemeyi tetiklemek için aşağıdaki adımları izleyin.<ol><li>İzinleri MachineKeys dizinde düzeltin.<br>Explorer güvenlik özellikleri ve Gelişmiş güvenlik ayarları dizinde kullanarak izinler varsayılan değerlere sıfırlayın. (Varsayılan) hariç tüm kullanıcı nesnelerini dizinden kaldırmak ve olun **herkes** izni olan özel erişim için:<br>-Liste klasörü / veri okuma <br>-Öznitelikleri okuma <br>-Okuma genişletilmiş öznitelikleri <br>-Dosya Oluştur / Veri Yaz <br>-Klasör Oluştur / Veri Ekle<br>-Yazma öznitelikleri<br>-Genişletilmiş öznitelikleri yazma<br>-Okuma izinleri<br><br><li>Tüm sertifikaları sil burada **çıkarılan** Klasik dağıtım modeli veya **Windows Azure CRP sertifika Oluşturucu**.<ul><li>[Sertifikalar (yerel bilgisayar) konsolunu açın](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx)<li>Altında **kişisel** > **sertifikaları**, tüm sertifikaları sil burada **çıkarılan** Klasik dağıtım modeli veya **Windows Azure CRP Sertifika Oluşturucu**.</ul><li>VM yedekleme işini tetikler. </ol>|
| Azure Backup hizmeti yedekleme, şifrelenmiş sanal makineleri için yeterli izinlere için Key Vault yok. |Yedekleme hizmet sağlanması bu izinleri PowerShell'de bölümünde anlatılan adımları kullanarak **yedeklemeyi etkinleştir** bölümünü [PowerShell belgeleri](backup-azure-vms-automation.md). |
|COM + Microsoft Dağıtılmış İşlem Düzenleyicisi ile konuşamadı - yüklenmesini anlık görüntü uzantısı hatasıyla başarısız oldu | Yükseltilmiş bir komut isteminden, örneğin "COM + System Application", Windows hizmetini başlatın _net Başlat COMSysApp_. <br>Ardından, başlatmak hizmet başarısız olursa:<ol><li> "Ağ Hizmeti" hizmet "Dağıtılmış İşlem Düzenleyicisi" olan açık hesap of Günlük emin olun. Yüklü değilse, "Network Service" hizmetini yeniden başlatın ve "COM + System Application" başlatmayı deneyin için oturum açma hesabının değiştirin.'<li>Varsa "COM + Sistem uygulaması başlangıç kalmaz, kaldırma/Yükle"Dağıtılmış İşlem Düzenleyicisi"hizmeti için aşağıdaki adımları kullanın:<br> -MSDTC hizmetini durdurun<br> -Bir komut istemi (cmd) açın <br> -Komutunu çalıştırın ```msdtc -uninstall``` <br> -Komutunu çalıştırın ```msdtc -install``` <br> -MSDTC hizmetini Başlat<li>"COM + System Application" Windows hizmetini başlatın. Bir kez "COM + Sistem uygulaması başladığında, Azure portalından bir yedekleme işi tetikleme.</ol> |
|  COM + hatası nedeniyle anlık görüntü işlemi başarısız oldu | Önerilen eylemi "COM + System Application" windows hizmetini yeniden başlatmaktır (yükseltilmiş komut isteminden - _net Başlat COMSysApp_). Sorun devam ederse, VM'yi yeniden başlatın. VM'nin yeniden başlatılmasının yaramazsa deneyin [VMSnapshot uzantısını kaldırma](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#cause-3-the-backup-extension-fails-to-update-or-load) ve el ile yedekleme tetikleyin. |
| Bir veya daha fazla bağlama noktası sanal dosya sistemi ile tutarlı bir anlık görüntüsünü almak için dondurulamadı. | Aşağıdaki adımları kullanın: <ol><li>Tüm bağlı cihazları kullanarak dosya-sistem durumunu denetleme _'tune2fs'_ komutu.<br> Örn: tune2fs -l/dev/sdb1 \| grep "Dosya sistemi durumu" <li>İçin hangi dosya sistemi durumu değil temiz kullanarak cihazları çıkarın _'umount'_ komutu <li> Kullanarak bu cihazları üzerinde FileSystemConsistency denetimi Çalıştır _'fsck'_ komutu <li> Cihazları yeniden bağlayın ve yedeklemeyi deneyin.</ol> |
| Güvenli ağ iletişim kanalı oluşturma hatası nedeniyle anlık görüntü işlemi başarısız oldu | <ol><Li> Regedit.exe bir yükseltilmiş modda çalıştırarak kayıt defteri Düzenleyicisi'ni açın. <li> Sisteminizde mevcut .NET Framework'ün tüm sürümler tanımlayın. Kayıt defteri anahtarı "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft" hiyerarşisi altında mevcut olduklarından <li> İçin her .net Framework kayıt defteri anahtarı mevcut anahtar ekleyin: <br> "SchUseStrongCrypto"=dword:00000001 </ol>|
| Visual Studio 2012 için Visual C++ yeniden dağıtılabilir yüklemesindeki hata nedeniyle anlık görüntü işlemi başarısız oldu | İçin C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion gidin ve vcredist2012_x64 yükleyin. Bu hizmeti yüklemesi izin vermek için kayıt defteri anahtarı değerini yani kayıt defteri anahtarının değeri doğru değerine ayarlandığından emin olun _HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSIServer_ 3 ve 4'değil. Hala sorunlarla karşılaşıyorsanız, yükleme hizmeti çalıştırarak yeniden _MSIEXEC/unregister_ ardından _MSIEXEC /REGISTER_ yükseltilmiş bir komut isteminden.  |


## <a name="jobs"></a>İşler
| Hata Ayrıntıları | Geçici çözüm |
| --- | --- |
| İptal, bu proje türü için desteklenmiyor: Lütfen iş tamamlanana kadar bekleyin. |None |
| İş iptal edilebilir bir durumda değil - iş tamamlanana kadar bekleyin. <br>OR<br> Seçili iş iptal edilebilir bir durumda değil; Lütfen işin tamamlanmasını bekleyin. |Tüm olasılığını içinde iş neredeyse tamamlandı. İş tamamlanana kadar bekleyin.|
| İş sürüyor değil - iptal yalnızca devam eden işler için desteklenen olduğundan iptal edilemiyor. Lütfen denemesi iptal üzerinde devam eden işi. |Bu, geçici bir durum nedeniyle gerçekleşir. Bir dakika bekleyin ve iptal etme işlemi yeniden deneyin. |
| Başarısız işi iptal et-iş tamamlanana kadar bekleyin. |None |

## <a name="restore"></a>Geri Yükleme
| Hata Ayrıntıları | Geçici çözüm |
| --- | --- |
| Geri yükleme bulut iç hatayla başarısız oldu |<ol><li>DNS ayarları ile bulut hizmetine geri yüklemeye çalıştığınız yapılandırılmıştır. Kontrol edebilirsiniz <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-yuvası "Üretim" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Yapılandırılmış adresi varsa, bu DNS ayarlarını yapılandırıldığını anlamına gelir.<br> <li>Bulut hizmeti olduğu için geri çalıştığınız ReservedIP ile yapılandırılmış ve bulut hizmetinde mevcut Vm'lere durdurulmuş durumdadır.<br>Bir bulut hizmeti, powershell cmdlet'lerini kullanarak IP ayırdığı denetleyebilirsiniz:<br>$deployment = get-AzureDeployment - ServiceName "servicename"-"Üretim" $yuvası DEP Reservedıpname <br><li>Aşağıdaki özel ağ yapılandırmaları ile sanal makine aynı bulut hizmetine geri yüklemek çalışıyorsunuz. <br>-Sanal makineler altında Yük Dengeleyiciyi yapılandırma (iç ve dış)<br>-Birden çok ayrılmış IP ile sanal makineler<br>-Birden çok NIC içeren sanal makineler<br>Lütfen yeni bir bulut hizmeti kullanıcı Arabiriminde seçin veya edinmek [konuları geri](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) özel ağ yapılandırmaları olan VM'ler için.</ol> |
| Seçilen DNS adı zaten alınmış - Lütfen farklı bir DNS adı belirtin ve yeniden deneyin. |Bulut hizmeti adı için DNS adını buraya başvurur (genellikle ile biten. cloudapp.net). Bu benzersiz olması gerekir. Bu hatayla karşılaşırsanız, geri yükleme sırasında farklı bir VM adı seçmeniz gerekir. <br><br> Bu hata, yalnızca Azure portalı kullanıcılarına gösterilir. Yalnızca diskleri geri yükler ve VM'yi oluşturmak değildir çünkü PowerShell aracılığıyla geri yükleme işlemi başarılı olur. Diski geri yükleme işleminden VM açıkça sizin tarafınızdan oluşturulduğunda hata karşılaştığı. |
| Belirtilen sanal ağ yapılandırması doğru değil - Lütfen farklı bir sanal ağ yapılandırması belirtin ve yeniden deneyin. |None |
| Belirtilen bulut hizmeti değil geri yüklenen sanal makine yapılandırmasıyla eşleşen - Lütfen ayrılmış IP kullanmayan farklı bir bulut hizmeti belirtin veya geri yükleme için başka bir kurtarma noktası seçin bir ayrılmış IP kullanıyor. |None |
| Bulut hizmetine giriş uç noktası sayısı sınırına ulaştı.-farklı bir bulut hizmeti belirterek veya var olan bir uç noktayı kullanarak işlemi yeniden deneyin. |None |
| Kurtarma Hizmetleri kasası ve hedef depolama hesabı olan iki farklı bölgede - depolama geri yükleme işleminde belirtilen hesap, Kurtarma Hizmetleri kasanız ile aynı Azure bölgesinde olduğundan emin olun. |None |
| Desteklenen depolama hesabı için geri yükleme işlemi belirtilen - yalnızca temel/standart depolama hesapları ile yerel olarak yedekli veya coğrafi olarak yedekli çoğaltma ayarlarına desteklenir. Lütfen desteklenen bir depolama hesabı seçin |None |
| Geri yükleme işlemi için belirtilen depolama hesabı türü çevrimiçi değil - geri yükleme işleminde belirtilen depolama hesabı çevrimiçi olduğundan emin olun |Bu, Azure Depolama'da veya kesinti nedeniyle geçici bir hata nedeniyle gerçekleşebilir. Lütfen başka bir depolama hesabı seçin. |
| Kaynak grubu kotasına ulaşıldı - Lütfen bazı kaynak gruplarını, Azure portalından silin veya limitleri artırmak için Azure desteğine başvurun. |None |
| Seçilen alt ağ yok - Lütfen var olan bir alt ağ seçin |None |
| Yedekleme Hizmeti'nin aboneliğinizdeki kaynaklara erişme yetkisi yok. |Bu, ilk geri yükleme bölümünde belirtilen adımları kullanarak diskleri çözümlenecek **diskleri geri yükleme yedeği** içinde [seçerek VM geri yükleme Yapılandırması](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Bundan sonra belirtilen PowerShell adımları kullanın [geri yüklenen diskten VM oluşturma](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) geri yüklenen disklerden tam VM oluşturma. |

## <a name="backup-or-restore-taking-time"></a>Yedekleme veya geri yükleme zaman ayırdığınız
Görürseniz, backup(>12 hours) veya geri alma time(>6 hours):
* Anlamak [yedekleme süresini etkileyen faktörler](backup-azure-vms-introduction.md#total-vm-backup-time) ve [geri yükleme süresini etkileyen faktörler](backup-azure-vms-introduction.md#total-restore-time).
* Takip ettiğinizden emin olun [en iyi yedekleme](backup-azure-vms-introduction.md#best-practices).

## <a name="vm-agent"></a>VM Aracısı
### <a name="setting-up-the-vm-agent"></a>VM Aracısı ayarlama
Genellikle, VM Aracısı zaten Azure galerisinden oluşturulan VM'ler bulunur. Ancak, sanal makineler, şirket içi veri merkezlerinden geçişi, VM aracısının yüklü bulunmaz. Bu tür VM'ler için VM Aracısı açıkça yüklü olması gerekir.

Windows Vm'leri için:

* [Aracı MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir.
* Klasik sanal makineler için [VM özelliğini güncelleştirin](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) aracının yüklü olduğunu belirtmek için. Bu adım, Resource Manager sanal makineler için gerekli değildir.

Linux Vm'leri için:

* Dağıtım depodan aracının en son sürümünü yükleyin. Paket adı hakkında daha fazla bilgi için bkz: [Linux Aracısı depo](https://github.com/Azure/WALinuxAgent).
* Klasik VM'ler için [blog girişine VM özelliğini güncelleştirin kullanmasını](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx)ve aracının yüklü doğrula. Bu adım, Resource Manager sanal makineler için gerekli değildir.

### <a name="updating-the-vm-agent"></a>VM Aracısı'nı güncelleştirme
Windows Vm'leri için:

* VM aracısını güncelleştirmek için yeniden [VM Aracısı ikili dosyalarının](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Aracıyı güncelleştirmeden önce VM Aracısı güncelleştirilirken herhangi bir yedekleme işlemleri ortaya emin olun.

Linux Vm'leri için:

* Linux VM aracısını güncelleştirmek için makaledeki yönergeleri [Linux VM Aracısı güncelleştirilirken](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
**Dağıtım havuzu aracısını güncelleştirmek için her zaman kullanmalısınız**. Aracı kodu Github'dan yüklemeyin. En son aracı dağıtımınız için kullanılabilir değilse, en son aracıyı alma yönergeleri dağıtım desteği ulaşın. En son de göz atabilirsiniz [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) GitHub deposundaki bilgiler.

### <a name="validating-vm-agent-installation"></a>VM Aracısı yüklemesini doğrulama

Windows vm'lerinde VM Aracısı'nın sürümünü doğrulamak için:

1. Azure sanal makinede oturum açın ve klasöre gidin *C:\WindowsAzure\Packages*. Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.
2. Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün sürümü alanı 2.6.1198.718 olmalıdır veya üzeri

## <a name="troubleshoot-vm-snapshot-issues"></a>VM anlık görüntüsü sorunları giderme
VM yedekleme, temel alınan depolama alanı için anlık görüntü komutları verme üzerinde kullanır. Depolama veya gecikmeleri erişimin bir anlık görüntü görevi yürütme olmaması, yedekleme işinin başarısız olmasına neden olabilir. Aşağıdaki anlık görüntü görevi başarısız olmasına neden olabilir.

1. NSG kullanarak depolamaya ağ erişimi engellendi<br>
    Hakkında daha fazla bilgi için bilgi [ağ erişimini etkinleştir](backup-azure-arm-vms-prepare.md#establish-network-connectivity) depolama kullanarak ya da IP beyaz listesi veya Ara sunucu üzerinden.
2. Yapılandırılmış Sql Server Yedekleme ile VM anlık görüntü görevi gecikmesi neden olabilir <br>
   Varsayılan olarak, VM yedekleme VSS bir tam yedekleme Windows Vm'lerinde oluşturur. SQL Server çalıştıran VM'ler, SQL Server ile yedekleme yapılandırılmış, anlık görüntü gecikme. Anlık görüntü gecikmeler yedekleme hatalarına neden, aşağıdaki kayıt defteri anahtarını ayarlayın.

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```

3. VM RDP kapatılmış olduğundan VM durumu yanlış bildirdi.  <br>
   Sanal makineyi kapatmak için Uzak Masaüstü kullandıysanız, portaldaki VM durumu doğru olduğunu doğrulayın. Durumu doğru değilse kullanın **kapatma** portalı sanal makine Panosu'ndan sanal makineyi seçeneği.
4. Dörtten fazla Vm'leri aynı bulut hizmeti paylaşıyorsanız, VM'ler arasında birden çok yedekleme ilkelerini yayılır. Yedekleme zamanları yoktur dörtten fazla VM yedeklemeleri aynı anda başlatmak kademelendirebilirsiniz. İlkeleri başlangıç zamanlarını en az bir saat olarak ayırmak deneyin.
5. VM, en yüksek CPU/bellek çalışıyor.<br>
   Sanal makine yüksek bellek veya CPU çalışıyorsa usage(>90%), anlık görüntü görev sıraya, Gecikmeli ve sonunda kez kullanıma alınır. Bu durumda, bir isteğe bağlı yedekleme deneyin.

<br>

## <a name="networking"></a>Ağ
Tüm uzantıları gibi Backup uzantısının çalışması için genel internet erişimi gerekir. Genel İnternet'e erişim olmaması kendisini çeşitli şekillerde bildirilebilir:

* Uzantı yüklemesi başarısız olabilir
* (Örneğin, disk anlık görüntüsü) yedekleme işlemleri başarısız olabilir
* Yedekleme işlemi durumunu görüntüleme başarısız olabilir

Genel internet adresleri çözmek için gereken geliştirilmiştir [burada](https://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Sanal ağ için DNS yapılandırmalarını kontrol edin ve Azure URI'ler çözümlenebildiğinden emin olmak gerekir.

Ad çözümlemesi doğru yaptıktan sonra Azure IP'ler için erişim de sağlanması gerekir. Azure altyapı erişim engelini kaldırmak için aşağıdaki adımlardan birini izleyin:

1. Beyaz liste Azure veri merkezi IP aralıkları.
   * ' Nın listesini alın [Azure datacenter IP'leri](https://www.microsoft.com/download/details.aspx?id=41653) Güvenilenler listesine eklenmek için.
   * IP'ler kullanarak engellemesini [New-NetRoute](https://docs.microsoft.com/powershell/module/nettcpip/new-netroute) cmdlet'i. Bu cmdlet Azure VM'de (yönetici olarak çalıştır) yükseltilmiş bir PowerShell penceresinde çalıştırın.
   * (Yerinde varsa) kuralları IP'ler için erişime izin vermek için NSG ekleyin.
2. Akışı HTTP trafiği için bir yol oluşturma
   * Varsa bir yerde (bir ağ güvenlik grubu, örneğin) bazı ağ kısıtlaması trafiği yönlendirmek için bir HTTP proxy sunucusu dağıtın. Bir HTTP Ara sunucusunu dağıtmak için adımları bulunan [burada](backup-azure-arm-vms-prepare.md#establish-network-connectivity).
   * (Bir yerde varsa) kuralları NSG ile INTERNET'e HTTP Ara sunucuya erişim izni ekleyin.

> [!NOTE]
> DHCP, Iaas VM Backup'ın çalışma Konuk içinde etkinleştirilmelidir.  Statik özel IP gerekiyorsa, platformu ile yapılandırmanız gerekir. VM içindeki DHCP seçeneği sol etkinleştirilmesi gerekir.
> Hakkında daha fazla bilgi görüntülemek [iç statik özel IP ayarı](../virtual-network/virtual-networks-reserved-private-ip.md).
>
>
