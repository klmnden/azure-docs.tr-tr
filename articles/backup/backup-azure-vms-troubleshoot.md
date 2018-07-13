---
title: Azure sanal makine yedekleme sorunlarını giderme
description: Backup ve Azure sanal makinelerini geri yükleme sorunlarını giderme
services: backup
author: trinadhk
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 01/21/2018
ms.author: trinadhk
ms.openlocfilehash: a5828b4e4f42c349246845bd003e874fb0352bae
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008085"
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure sanal makine yedekleme sorunlarını giderme
Aşağıdaki tabloda listelenen bilgilerle Azure Backup kullanarak sırasında karşılaşılan hataları giderebilirsiniz.

| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| VM artık mevcut olmadığından işlem gerçekleştirilemiyor. -Yedekleme verilerini silmeden sanal makine korumayı durdurun. Daha fazla bilgi http://go.microsoft.com/fwlink/?LinkId=808124 |Bu, birincil sanal makine silinmiş, ancak yedekleme ilkesini sanal Makineyi yedeklemek için arama devam gerçekleşir. Bu hatayı düzeltmek için: <ol><li> Sanal makine ile aynı ada ve aynı kaynak grubu adı [bulut hizmeti adı] yeniden oluşturun<br>(VEYA)</li><li> Sanal makine içeren veya içermeyen yedekleme verilerini silme korumayı durdurun. [Daha fazla ayrıntı](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| -Sanal makinede ağ bağlantısı olmaması nedeniyle başarısız oldu. anlık görüntü işlemi, VM ağ erişimi bulunduğundan emin olun. Başarılı olması anlık görüntü için ya da güvenilir Azure veri merkezi IP aralıkları veya ağ erişimi için bir proxy sunucusu ayarlayın. Daha fazla ayrıntı için başvurmak http://go.microsoft.com/fwlink/?LinkId=800034. Proxy sunucusu kullanıyorsanız, proxy sunucu ayarlarının doğru yapılandırıldığından emin olun. | Bu hata, sanal makineye giden internet bağlantısı reddettiğinizde oluşturulur. VM anlık görüntü uzantısı için temel alınan diskler sanal makinenin bir anlık görüntüsünü almak Internet bağlantısı gereklidir. [Daha fazla bilgi edinin](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine) engellenen ağ erişim nedeniyle anlık görüntü hatalarını düzeltme konusunda. |
| VM Aracısı Azure Backup hizmeti ile iletişim kuramıyor. -VM'nin ağ bağlantısı olduğundan ve VM aracısını en son ve çalışıyor olduğundan emin olun. Daha fazla bilgi için bkz  http://go.microsoft.com/fwlink/?LinkId=800034 |Bu hata, VM Aracısı ile ilgili bir sorun veya başka bir yolla Azure altyapısı için ağ erişimi engellendi oluşturulur. [Daha fazla bilgi edinin](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vm-agent-unable-to-communicate-with-azure-backup) sorunları VM'yi hata ayıklama hakkında anlık görüntüsünü alın.<br> VM Aracısı herhangi bir sorun neden, ardından VM'yi yeniden başlatın. Bazen hatalı bir VM durumu sorunlarına neden olabilir ve bu "bozuk durum" VM'nin yeniden başlatılmasının sıfırlar. |
| VM başarısız sağlama durumunda - Lütfen VM'yi yeniden başlatın ve VM'yi yedekleme için çalışıyor veya kapalı durumda olduğundan emin olun | Bu durum, bir uzantı hataları VM başarısız sağlama durumunda olması durumuna müşteri adayları oluşur. Uzantılar listesine dönün ve başarısız bir uzantısı olup olmadığını, kaldırmak ve sanal makineyi yeniden başlatmayı deneyin. Tüm uzantıları çalışır durumda olduğundan, VM aracısı hizmetinin çalışıp çalışmadığını denetleyin. Aksi durumda, VM Aracısı hizmetini yeniden başlatın. | 
| VMSnapshot uzantısı işlemi için yönetilen disk - başarısız oldu. Lütfen yedekleme işlemini yeniden deneyin. Sorun devam ederse konumundaki yönergeleri 'http://go.microsoft.com/fwlink/?LinkId=800034'. Başarısız olursa, lütfen Microsoft desteğine başvurun | Azure Backup hizmeti anlık görüntüsünü tetiklemek başarısız olduğunda bu hata oluştu. [Daha fazla bilgi edinin](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vmsnapshot-extension-operation-failed) sorunları VM hata ayıklama hakkında anlık görüntüsünü alın. |
| Kopyalama - depolama hesabında yeterli boş alan sanal makinenin bir anlık görüntü depolama hesabı mevcut sanal makineye bağlı premium depolama disklerindeki verilerle eşit miktarda boş alan olduğundan emin olun | VM yedekleme yığını V1 Premium Vm'lere olması durumunda, biz anlık görüntü depolama hesabına kopyalayın. Bu anlık görüntü üzerinde çalışır, yedekleme yönetim trafiği, premium diskler kullanarak bir uygulama için kullanılabilir IOPS sayısını kısıtlamaz emin olmaktır. Azure Backup hizmeti, kasa için depolama hesabındaki kopyalanan bu konumdan, depolama hesabı ve aktarım veri anlık görüntü kopyalayabilmeniz adına Microsoft yalnızca % 50 (17,5 TB) tahsis toplam depolama hesabı alanı önerir. | 
| VM aracısı yanıt vermediği işlemi gerçekleştirilemiyor. |Bu hata, VM Aracısı ile ilgili bir sorun veya başka bir yolla Azure altyapısı için ağ erişimi engellendi oluşturulur. Windows Vm'leri için VM aracısı hizmet durumunun services'ı ve aracıyı Denetim Masası'ndaki Programlar görüntülenip denetleyin. Program denetiminin kaldırmayı deneyin paneli ve belirtildiği gibi aracıyı yeniden yükleme [aşağıda](#vm-agent). Aracıyı yeniden yükledikten sonra doğrulamak için geçici bir yedeklemeyi tetikleyin. |
| Kurtarma Hizmetleri Uzantısı işlemi başarısız oldu. -Lütfen en son sanal makine Aracısı sanal makinede mevcut olduğundan ve aracı hizmetinin çalıştığından emin olun. Lütfen yedekleme işlemini yeniden deneyin ve başarısız olursa Microsoft desteğine başvurun. |Bu hata, VM aracısının güncel olduğunda oluşturulur. VM aracısını güncelleştirmek için aşağıdaki "VM aracısını güncelleştirme" bölümüne bakın. |
| Sanal makine yok. -Lütfen sanal makinenin var olduğundan emin olun veya farklı bir sanal makine seçin. |Bu, birincil VM silindiği ancak yedekleme gerçekleştirmek bir VM için aramak yedekleme ilkesini devam olduğunda gerçekleşir. Bu hatayı düzeltmek için: <ol><li> Sanal makine ile aynı ada ve aynı kaynak grubu adı [bulut hizmeti adı] yeniden oluşturun<br>(VEYA)<br></li><li>Yedekleme verilerini silmeden sanal makineyi korumayı durdurun. [Daha fazla ayrıntı](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Komut yürütme başarısız oldu. -Başka bir işlem, bu öğe üzerinde şu anda sürüyor. Önceki işlemin tamamlanmasını bekleyin ve sonra yeniden deneyin |Mevcut bir yedekleme VM üzerinde çalışıyor ve var olan bir işi devam ederken yeni bir işi başlatılamıyor. |
| Yedekleme kasasından VHD kopyalama zaman aşımına uğradı; Lütfen işlemi birkaç dakika içinde yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. | Bu depolama tarafında geçici bir hata varsa veya yedekleme hizmeti kasası için zaman aşımı süresi içinde veri aktarımı için VM'yi barındıran depolama hesabından yeterli IOPS almıyorsa gerçekleşir. Uyguladığınız emin [en iyi uygulamalar](backup-azure-vms-introduction.md#best-practices) yedekleme ayarlanırken. Farklı bir depolama VM yedekleme hesap yüklenmedi ve yeniden deneyin taşımayı deneyin.|
| Yedekleme bir iç hata ile başarısız oldu - Lütfen işlemi birkaç dakika içinde yeniden deneyin. Sorun devam ederse, Microsoft Support başvurun |2 nedenlerden dolayı bu hatayı alabilirsiniz: <ol><li> VM depolama erişmede geçici bir sorun yoktur. Lütfen denetleyin [Azure durumu](https://azure.microsoft.com/status/) tüm devam eden işlem ilgili bir sorun, depolama veya ağ bölgesinde olup olmadığını görmek için. Ardından, sorun çözüldükten sonra yedekleme işini yeniden deneyin. <li>Özgün VM silinmiştir ve bu nedenle, kurtarma noktası alınamaz. Silinen bir VM için yedekleme verileri tutmak, ancak yedekleme hataları Kaldır: sanal Makinenin korumasını kaldırın ve verileri tutmak için bu seçeneği belirleyin. Bu eylem, zamanlanmış bir yedekleme işi ve yinelenen hata iletileri durdurur. |
| Seçilen öğe üzerinde - Azure kurtarma Hizmetleri Uzantısı yüklenemedi VM Aracısı Azure kurtarma Hizmetleri uzantısı için önkoşuldur. Azure VM aracısını yükleyin ve kayıt işlemini yeniden başlatın |<ol> <li>VM aracısının doğru şekilde yüklenip yüklenmediğini kontrol edin. <li>VM yapılandırması bayrağı doğru ayarlandığından emin olun.</ol> [Daha fazla bilgi edinin](#validating-vm-agent-installation) VM aracısı ve VM Aracısı yüklemesini doğrulamak nasıl yükleneceğine. |
| "COM + Microsoft Distributed Transaction Düzenleyicisi ile konuşamadı uzantısı yükleme işlemi şu hatayla başarısız oldu |Bu, genellikle COM + hizmet çalışmıyor anlamına gelir. Bu sorunu çözme konusunda yardım için Microsoft desteğine başvurun. |
| Anlık görüntü işlemi, "Bu sürücü BitLocker Sürücü Şifrelemesi tarafından kilitlendi. VSS işlem hatasıyla başarısız oldu Denetim Masası'ndan bu sürücünün kilidini açmanız gerekir. |Sanal makinedeki tüm sürücüleri için BitLocker'ı açın ve VSS sorunun çözülüp çözülmediğine gözlemleyin |
| VM yedeklemeleri izin veren bir durumda değil. |<ul><li>VM kapalı geçici bir durum çalışan ve bilgisayarı arasında olup olmadığını denetleyin. VM durumu aşağıdakilerden biri olması ve tetiklemek ise, bekleme yeniden yedekleme. <li> VM, bir Linux VM ve kullandığı [güvenliği artırılmış Linux] çekirdek modülü ise, Linux Aracısı yolu dışla gerekir (_/var/lib/waagent_) güvenlik ilkesi emin yedekleme uzantısı yüklenir.  |
| Azure sanal makinesi bulunamadı. |Bu, birincil VM silindiği ancak yedekleme gerçekleştirmek bir sanal makine aramak yedekleme ilkesini devam olduğunda gerçekleşir. Bu hatayı düzeltmek için: <ol><li>Sanal makine ile aynı ada ve aynı kaynak grubu adı [bulut hizmeti adı] yeniden oluşturun <br>(VEYA) <li> Yedekleme işleri oluşturulmayacak şekilde bu VM için korumayı devre dışı bırakın. </ol> |
| Sanal makine Aracısı sanal makinede mevcut değil - Lütfen tüm önkoşul ve VM aracısını yükleyin ve ardından işlemi yeniden başlatın. |[Daha fazla bilgi edinin](#vm-agent) VM Aracısı yükleme ve VM Aracısı yüklemesini doğrulamak nasıl hakkında bilgi. |
| VSS yazıcılarının hatalı durumda olduğundan anlık görüntü işlemi başarısız oldu |Kötü durumdaki VSS (birim gölge kopyası hizmeti) yazıcıları yeniden başlatmanız gerekir. Yükseltilmiş bir komut isteminden Bunu başarmak için çalıştırın _vssadmin listesi Yazıcılar_. Çıktı, tüm VSS yazıcılarının ve bunların durumunu içerir. Değil "[1] Stable" durumu, her VSS yazıcısı için VSS Yazıcı, komutları yükseltilmiş komut isteminden aşağıdaki çalıştırarak yeniden başlatın:<br> _net stop serviceName_ <br> _net start serviceName_|
| Yapılandırma ayrıştırma hatası nedeniyle anlık görüntü işlemi başarısız oldu |Bu değiştirilen izinleri MachineKeys dizini nedeniyle gerçekleşir: _%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br>Lütfen aşağıdaki komutu çalıştırın ve varsayılan olanları MachineKeys dizin üzerindeki izinleri doğrulayın:<br>_ICACLS %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br><br> Varsayılan izinler şunlardır:<br>Everyone:(R,W) <br>BUILTIN\Administrators:(F)<br><br>İzinleri MachineKeys dizinde varsayılandan farklı görürseniz, lütfen izinleri düzeltmek, sertifikayı silin ve yedeklemeyi tetiklemek için aşağıdaki adımları izleyin.<ol><li>İzinleri MachineKeys dizinde düzeltin.<br>Dizinde Explorer güvenlik özellikleri ve Gelişmiş güvenlik ayarları kullanılarak izinler varsayılan değerlere sıfırla, herhangi bir ek (varsayılan) daha kullanıcı nesnesi dizinden kaldırın ve 'Herkes' için özel erişim izinlerine sahip olduğundan emin olun:<br>-Liste klasörü / veri okuma <br>-Öznitelikleri okuma <br>-Okuma genişletilmiş öznitelikleri <br>-Dosya Oluştur / Veri Yaz <br>-Klasör Oluştur / Veri Ekle<br>-Yazma öznitelikleri<br>-Genişletilmiş öznitelikleri yazma<br>-Okuma izinleri<br><br><li>Tüm sertifikaları 'İçin verilen' alanıyla silmek = "Windows Azure hizmet yönetimi için uzantıları" veya "Windows Azure CRP sertifika Oluşturucu".<ul><li>[Sertifikalar (yerel bilgisayar) konsolunu açın](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx)<li>Tüm sertifikaları sil (altında kişisel sertifikalar ->) 'İçin verilen' alanıyla = "Windows Azure hizmet yönetimi için uzantıları" veya "Windows Azure CRP sertifika Oluşturucu".</ul><li>VM yedeklemeyi tetikleme. </ol>|
| Azure Backup hizmeti yedekleme, şifrelenmiş sanal makineleri için yeterli izinlere için Key Vault yok. |Yedekleme hizmet sağlanması bu izinleri PowerShell'de bölümünde anlatılan adımları kullanarak **yedeklemeyi etkinleştir** bölümünü [PowerShell belgeleri](backup-azure-vms-automation.md). |
|COM + Microsoft Dağıtılmış İşlem Düzenleyicisi ile konuşamadı - yüklenmesini anlık görüntü uzantısı hatasıyla başarısız oldu | Lütfen "COM + System Application" windows hizmetini yeniden başlatmayı deneyin (yükseltilmiş komut isteminden - _net Başlat COMSysApp_). <br>Başlatma sırasında başarısız olursa, lütfen aşağıdaki adımları uygulayın:<ol><li> "Dağıtılmış İşlem Düzenleyicisi" hizmetinin oturum açma hesabı "Ağ Hizmeti" olduğunu doğrulayın. Yüklü değilse, lütfen "Ağ Hizmeti" değiştirin, bu hizmeti yeniden başlatın ve sonra "COM + System Application" hizmetini yeniden başlatmayı deneyin.'<li>Başlamak yine de başarısız olursa kaldırma/hizmet "Dağıtılmış İşlem Düzenleyicisi" aşağıdaki adımları izleyerek yükle:<br> -MSDTC hizmetini durdurun<br> -Bir komut istemi (cmd) açın <br> -Komut çalıştırma "msdtc-kaldırma" <br> -Komut çalıştırma "msdtc-yükleyin" <br> -MSDTC hizmetini Başlat<li>"COM + System Application" Windows hizmetini başlatın ve başlatıldıktan sonra portaldan yedekleme tetikleyin.</ol> |
|  COM + hatası nedeniyle anlık görüntü işlemi başarısız oldu | Önerilen eylemi "COM + System Application" windows hizmetini yeniden başlatmaktır (yükseltilmiş komut isteminden - _net Başlat COMSysApp_). Sorun devam ederse, VM'yi yeniden başlatın. VM'nin yeniden başlatılmasının yaramazsa deneyin [VMSnapshot uzantısını kaldırma](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#cause-3-the-backup-extension-fails-to-update-or-load) ve el ile yedekleme tetikleyin. |
| Bir veya daha fazla bağlama noktası sanal dosya sistemi ile tutarlı bir anlık görüntüsünü almak için dondurulamadı. | Aşağıdaki adımları kullanın: <ol><li>Tüm bağlı cihazları kullanarak dosya-sistem durumunu denetleme _'tune2fs'_ komutu.<br> Örn: tune2fs -l/dev/sdb1 \| grep "Dosya sistemi durumu" <li>İçin hangi dosya sistemi durumu değil temiz kullanarak cihazları çıkarın _'umount'_ komutu <li> Kullanarak bu cihazları üzerinde FileSystemConsistency denetimi Çalıştır _'fsck'_ komutu <li> Cihazları yeniden bağlayın ve yedeklemeyi deneyin.</ol> |
| Güvenli ağ iletişim kanalı oluşturma hatası nedeniyle anlık görüntü işlemi başarısız oldu | <ol><Li> Regedit.exe bir yükseltilmiş modda çalıştırarak kayıt defteri Düzenleyicisi'ni açın. <li> Tüm sürümlerini tanımlar. Sistemde NetFramework. Kayıt defteri anahtarı "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft" hiyerarşisi altında mevcut olduklarından <li> Her biri için. Kayıt defteri anahtarı mevcut NetFramework anahtar ekleyin: <br> "SchUseStrongCrypto"=dword:00000001 </ol>|
| Visual Studio 2012 için Visual C++ yeniden dağıtılabilir yüklemesindeki hata nedeniyle anlık görüntü işlemi başarısız oldu | İçin C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion gidin ve vcredist2012_x64 yükleyin. Bu hizmeti yüklemesi izin vermek için kayıt defteri anahtarı değerini yani kayıt defteri anahtarının değeri doğru değerine ayarlandığından emin olun _HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSIServer_ 3 ve 4'değil. Hala sorunlarla karşılaşıyorsanız, yükleme hizmeti çalıştırarak yeniden _MSIEXEC/unregister_ ardından _MSIEXEC /REGISTER_ yükseltilmiş bir komut isteminden.  |


## <a name="jobs"></a>İşler
| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| İptal, bu proje türü için desteklenmiyor: Lütfen iş tamamlanana kadar bekleyin. |None |
| İş iptal edilebilir bir durumda değil - iş tamamlanana kadar bekleyin. <br>OR<br> Seçili iş iptal edilebilir bir durumda değil; Lütfen işin tamamlanmasını bekleyin. |Tüm olasılığını içinde iş neredeyse tamamlandı. İş tamamlanana kadar bekleyin.|
| İş sürüyor değil - iptal yalnızca devam eden işler için desteklenen olduğundan iptal edilemiyor. Lütfen denemesi iptal üzerinde devam eden işi. |Bu, geçici bir durum nedeniyle gerçekleşir. Bir dakika bekleyin ve iptal etme işlemi yeniden deneyin. |
| Başarısız işi iptal et-iş tamamlanana kadar bekleyin. |None |

## <a name="restore"></a>Geri Yükleme
| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| Geri yükleme bulut iç hatayla başarısız oldu |<ol><li>DNS ayarları ile bulut hizmetine geri yüklemeye çalıştığınız yapılandırılmıştır. Kontrol edebilirsiniz <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-yuvası "Üretim" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Yapılandırılmış adresi varsa, bu DNS ayarlarını yapılandırıldığını anlamına gelir.<br> <li>Bulut hizmeti olduğu için geri çalıştığınız ReservedIP ile yapılandırılmış ve bulut hizmetinde mevcut Vm'lere durdurulmuş durumdadır.<br>Bir bulut hizmeti, powershell cmdlet'lerini kullanarak IP ayırdığı denetleyebilirsiniz:<br>$deployment = get-AzureDeployment - ServiceName "servicename"-"Üretim" $yuvası DEP Reservedıpname <br><li>Aşağıdaki özel ağ yapılandırmaları ile sanal makine aynı bulut hizmetine geri yüklemek çalışıyorsunuz. <br>-Sanal makineler altında Yük Dengeleyiciyi yapılandırma (iç ve dış)<br>-Birden çok ayrılmış IP ile sanal makineler<br>-Birden çok NIC içeren sanal makineler<br>Lütfen yeni bir bulut hizmeti kullanıcı Arabiriminde seçin veya edinmek [konuları geri](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) özel ağ yapılandırmaları olan VM'ler için.</ol> |
| Seçilen DNS adı zaten alınmış - Lütfen farklı bir DNS adı belirtin ve yeniden deneyin. |Bulut hizmeti adı için DNS adını buraya başvurur (genellikle ile biten. cloudapp.net). Bu benzersiz olması gerekir. Bu hatayla karşılaşırsanız, geri yükleme sırasında farklı bir VM adı seçmeniz gerekir. <br><br> Bu hata, yalnızca Azure portalı kullanıcılarına gösterilir. Yalnızca diskleri geri yükler ve VM'yi oluşturmak değildir çünkü PowerShell aracılığıyla geri yükleme işlemi başarılı olur. Diski geri yükleme işleminden VM açıkça sizin tarafınızdan oluşturulduğunda hata karşılaştığı. |
| Belirtilen sanal ağ yapılandırması doğru değil - Lütfen farklı bir sanal ağ yapılandırması belirtin ve yeniden deneyin. |None |
| Belirtilen bulut hizmeti değil geri yüklenen sanal makine yapılandırmasıyla eşleşen - Lütfen ayrılmış IP kullanmayan farklı bir bulut hizmeti belirtin veya geri yükleme için başka bir kurtarma noktası seçin bir ayrılmış IP kullanıyor. |None |
| Bulut hizmetine giriş uç noktası sayısı sınırına ulaştı.-farklı bir bulut hizmeti belirterek veya var olan bir uç noktayı kullanarak işlemi yeniden deneyin. |None |
| Yedekleme kasası ve hedef depolama hesabı olan iki farklı bölgede - geri yükleme işleminde belirtilen depolama hesabı yedekleme kasasıyla aynı Azure bölgesinde olduğundan emin olun. |None |
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

* [Aracı MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir.
* Klasik sanal makineler için [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) aracının yüklü olduğunu belirtmek için. Bu adım, Resource Manager sanal makineler için gerekli değildir.

Linux Vm'leri için:

* Dağıtım depodan en son sürümü yükleyin. Biz **önemle tavsiye** yalnızca dağıtım depo üzerinden yükleme aracı. Paket adı hakkında daha fazla bilgi için bkz [Linux Aracısı depo](https://github.com/Azure/WALinuxAgent) 
* Klasik VM'ler için [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) aracının yüklü olduğunu belirtmek için. Bu adım, Resource Manager sanal makineler için gerekli değildir.

### <a name="updating-the-vm-agent"></a>VM Aracısı'nı güncelleştirme
Windows Vm'leri için:

* VM Aracısı'nı güncelleştirmek için [VM Aracısı ikili dosyalarının](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yeniden yüklenmesi yeterlidir. Ancak, VM Aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun gerekir.

Linux Vm'leri için:

* Yönergeleri takip edin [Linux VM Aracısı güncelleştirilirken](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Biz **önemle tavsiye** yalnızca dağıtım deposu aracılığıyla güncelleştirme Aracısı. Aracı kodu doğrudan github'dan indiriliyor ve güncelleştirirken önermiyoruz. En son aracıyı, dağıtım için kullanılabilir durumda değilse, lütfen en son aracıyı yükleme hakkında yönergeler için dağıtım desteği ulaşın. En son kontrol edebilirsiniz [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) github deposundaki bilgiler.

### <a name="validating-vm-agent-installation"></a>VM Aracısı yüklemesini doğrulama
Windows vm'lerinde VM Aracısı'nın sürümünü denetlemek nasıl:

1. Azure sanal makinede oturum açın ve klasöre gidin *C:\WindowsAzure\Packages*. Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.
2. Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün sürümü alanı 2.6.1198.718 olmalıdır veya üzeri

## <a name="troubleshoot-vm-snapshot-issues"></a>VM anlık görüntüsü sorunları giderme
VM yedekleme, temel alınan depolama alanı için anlık görüntü komutları verme üzerinde kullanır. Depolama veya gecikmeleri erişimin bir anlık görüntü görevi yürütme olmaması, yedekleme işinin başarısız olmasına neden olabilir. Aşağıdaki anlık görüntü görevi başarısız olmasına neden olabilir.

1. NSG kullanarak depolamaya ağ erişimi engellendi<br>
    Hakkında daha fazla bilgi için bilgi [ağ erişimini etkinleştir](backup-azure-arm-vms-prepare.md#establish-network-connectivity) depolama kullanarak ya da IP beyaz listesi veya Ara sunucu üzerinden.
2. Yapılandırılmış Sql Server Yedekleme ile VM anlık görüntü görevi gecikmesi neden olabilir <br>
   Varsayılan VM yedekleme sorunları VSS tam yedekleme ile Windows Vm'leri üzerinde. Sql sunucuları ve Sql Server Yedekleme yapılandırılıp yapılandırılmadığını çalıştıran VM'ler üzerinde bu anlık görüntü yürütme gecikmesi neden olabilir. Yedekleme hataları nedeniyle anlık görüntü sorunları yaşıyorsanız Lütfen aşağıdaki kayıt defteri anahtarını ayarlayın.

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```
3. VM RDP kapatılmış olduğundan VM durumu yanlış bildirdi.  <br>
   Sanal makinede RDP, lütfen onay Portalı'nda kapatırsanız bu sanal Makinenin durumu doğru şekilde yansıtılır. Aksi halde, lütfen portalında sanal makine panosunda 'Kapat' seçeneğini kullanarak VM'yi kapatın.
4. Dörtten fazla Vm'leri aynı bulut hizmeti paylaşıyorsanız, yedekleme sürelerine yoktur dörtten fazla VM yedeklemeleri aynı anda başlatılır. Aşama için birden çok yedekleme ilkelerini yapılandırın. Bir saat arasında ilkeleri uzaklıkta kez yedekleme başlangıç yaymayı deneyin.
5. VM, en yüksek CPU/bellek çalışıyor.<br>
   Yüksek CPU sanal makine çalışıyorsa usage(>90%) veya bellek anlık görüntü görevi sıraya alındı ve Gecikmeli sonunda zaman aşımına uğradı alır. Böyle durumlarda isteğe bağlı yedekleme deneyin.

<br>

## <a name="networking"></a>Ağ
Tüm uzantıları gibi Backup uzantısının çalışması için genel internet erişimi gerekir. Genel İnternet'e erişim olmaması kendisini çeşitli şekillerde bildirilebilir:

* Uzantı yüklemesi başarısız olabilir
* (Örneğin, disk anlık görüntüsü) yedekleme işlemleri başarısız olabilir
* Yedekleme işlemi durumunu görüntüleme başarısız olabilir

Genel internet adresleri çözmek için gereken geliştirilmiştir [burada](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Sanal ağ için DNS yapılandırmalarını kontrol edin ve Azure URI'ler çözümlenebildiğinden emin olmak gerekir.

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
