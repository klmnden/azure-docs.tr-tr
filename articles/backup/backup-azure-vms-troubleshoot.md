---
title: "Azure sanal makine yedekleme hatalarıyla ilgili sorunları giderme | Microsoft Docs"
description: "Yedekleme ve geri yükleme Azure sanal makinelerin sorun giderme"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 73214212-57a4-4b57-a2e2-eaf9d7fde67f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: trinadhk;markgal;jpallavi;
ms.openlocfilehash: d09208596de4609faace67e11926ad30f68cd901
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure sanal makine yedekleme sorunlarını giderme
Azure Backup bilgileri kullanarak, aşağıdaki tabloda listelenen sırasında oluşan hatalar giderebilirsiniz.

## <a name="backup"></a>Backup

### <a name="error-the-specified-disk-configuration-is-not-supported"></a>Hata: Belirtilen Disk yapılandırması desteklenmiyor

> [!NOTE]
> 1 TB’den düşük ve yönetilmeyen disklere sahip sanal makineler için yedeklemeyi destekleyen özel bir önizlememiz var. Ayrıntı için [büyük disk VM yedekleme desteği için özel Önizleme](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)
>
>

Azure Backup disk boyutları şu anda desteklememektedir [1023 GB'den büyük](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm). 
- 1 TB’den büyük diskleriniz varsa, 1 TB’den düşük [yeni diskleri kullanıma açın](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) <br>
- Ardından 1 TB’den büyük olan diskten verileri kopyalayıp yeni oluşturulan 1 TB’den küçük disklere aktarın. <br>
- Tüm verilerin kopyalandığından emin olun ve 1 TB’den büyük diskleri kaldırın
- Yedeklemeyi başlatın.

| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| VM artık mevcut olmadığından işlem gerçekleştirilemiyor. -Sanal makine yedekleme verileri silmeden korumayı durdurun. Http://go.microsoft.com/fwlink/?LinkId=808124 daha fazla bilgi |Bu birincil VM silinmez, ancak yedekleme İlkesi yedeklemek için bir VM arayan devam olur. Bu hatayı düzeltmek için: <ol><li> Aynı kaynak grubu adı [bulut hizmet adı] ve aynı ada sahip sanal makine oluşturun<br>(VEYA)</li><li> Sanal makine ile veya yedekleme verileri silme olmadan korumayı durdurun. [Daha fazla ayrıntı](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Anlık görüntü işlemi sanal makinedeki - ağ bağlantısı nedeniyle başarısız oldu, VM ağ erişimi bulunduğundan emin olun. Başarılı olması anlık görüntü da beyaz liste Azure veri merkezi IP aralıkları veya ağ erişimi için bir proxy sunucusu ayarlayın. Daha fazla ayrıntı için http://go.microsoft.com/fwlink/?LinkId=800034 için başvurun. Proxy sunucu kullanıyorsanız, proxy sunucusu ayarlarının doğru şekilde yapılandırıldığından emin olun | Sanal makinede giden internet bağlantısı Reddet olduğunda bu hata atılır. VM anlık görüntü uzantısının temel diskleri sanal makinenin anlık görüntüsünü için Internet bağlantısı gereklidir. [Daha fazla bilgi edinin](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine) nasıl engellenen ağ erişim nedeniyle anlık görüntü hataları düzeltin. |
| VM Aracısı Azure Backup hizmetiyle iletişim kuramıyor. -VM ağ bağlantısı olduğunu ve VM Aracısı en son ve çalıştığından emin olun. Daha fazla bilgi için lütfen için http://go.microsoft.com/fwlink/?LinkId=800034 bakın |Bu hata, VM Aracısı ile ilgili bir sorun veya başka bir yolla Azure altyapısı için ağ erişim engellendi atılır. [Daha fazla bilgi edinin](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vm-agent-unable-to-communicate-with-azure-backup) sorunları VM'yi hata ayıklama hakkında anlık görüntüsünü alın.<br> VM Aracısı sorunları neden, VM'yi yeniden başlatın. Bazen yanlış bir VM durum sorunlara neden olabilir ve VM'yi yeniden başlatırken bu "bozuk" sıfırlar. |
| Başarısız sağlama durumda VM - Lütfen VM'yi yeniden başlatın ve VM yedekleme için çalışıyor veya kapatma durumda olduğundan emin olun | Bu durum, bir uzantı hataları VM sağlama başarısız durumunda olması durumuna müşteri adayları oluşur. Uzantılar listesine gidin ve başarısız bir uzantısı olup olmadığını, kaldırmak ve sanal makineyi yeniden başlatmayı deneyin. Tüm uzantıları çalışır durumda olduğundan, VM aracısı hizmetinin çalışıp çalışmadığını denetleyin. Aksi durumda, VM Aracısı hizmetini yeniden başlatın. | 
| VMSnapshot uzantısı işlemi için yönetilen diskleri - başarısız oldu Lütfen yedekleme işlemini yeniden deneyin. Sorun devam ederse, 'http://go.microsoft.com/fwlink/?LinkId=800034' yönergeleri izleyin. Daha fazla başarısız olursa, lütfen Microsoft Destek'e başvurun | Bir anlık görüntü tetiklemek Azure Backup hizmeti başarısız olduğunda bu hata oluştu. [Daha fazla bilgi edinin](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vmsnapshot-extension-operation-failed) sorunları VM hata ayıklama hakkında anlık görüntüsünü alın. |
| Kopyalama anlık görüntü depolama hesabındaki - yeterli boş alan nedeniyle sanal makinenin depolama hesabı mevcut sanal makineye bağlı premium depolama disklerdeki verileri eşdeğer boş alan olduğundan emin | Premium VM'ler durumunda biz anlık görüntü depolama hesabına kopyalayın. Bu anlık görüntü üzerinde çalışır, yedekleme yönetim trafiği premium diskleri kullanarak uygulama kullanılabilir IOPS sayısı sınırı değil emin olmaktır. Microsoft Azure Backup hizmeti, kasaya depolama hesabındaki kopyalanan bu konumdan, depolama hesabı ve aktarım veri anlık görüntü kopyalayabilmeniz için toplam depolama hesabı alanı % 50'yalnızca tahsis önerir. | 
| VM aracısı yanıt verebilir durumda olmadığından işlem gerçekleştirilemiyor |Bu hata, VM Aracısı ile ilgili bir sorun veya başka bir yolla Azure altyapısı için ağ erişim engellendi atılır. Windows VM'ler için hizmetleri ve Aracısı'nı Denetim Masası'ndaki Programlar görüntülenip VM aracısı hizmet durumunu denetleyin. Program denetiminden kaldırmayı deneyin paneli ve belirtildiği gibi aracıyı yeniden yükleme [aşağıda](#vm-agent). Aracıyı yeniden yükledikten sonra doğrulamak için geçici bir yedeklemeyi tetikleyin. |
| Kurtarma Hizmetleri Uzantısı işlemi başarısız oldu. -Lütfen en son sanal makine Aracısı sanal makinede bulunduğundan ve aracı hizmetinin çalıştığından emin olun. Lütfen yedekleme işlemini yeniden deneyin ve başarısız olursa, Microsoft Destek'e başvurun. |VM Aracısı güncel olduğunda bu hata oluşturulur. VM Aracısı'nı güncelleştirmek için aşağıdaki "VM aracısını güncelleştirme" bölümüne bakın. |
| Sanal makine yok. -Lütfen sanal makinenin var olduğundan emin olun veya farklı bir sanal makine seçin. |Bu, birincil VM silindiği, ancak yedekleme gerçekleştirmek bir VM için aramak yedekleme ilkenizi devam durumlarda gerçekleşir. Bu hatayı düzeltmek için: <ol><li> Aynı kaynak grubu adı [bulut hizmet adı] ve aynı ada sahip sanal makine oluşturun<br>(VEYA)<br></li><li>Yedekleme verilerini silmeden sanal makine korumasını durdurun. [Daha fazla ayrıntı](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Komut yürütme başarısız oldu. -Başka bir işlem şu anda bu öğeyi sürüyor. Önceki işlem tamamlanana kadar bekleyin ve sonra yeniden deneyin |Var olan bir yedeğini VM üzerinde çalıştığından ve var olan iş çalışırken yeni bir işi başlatılamıyor. |
| VHD'ler kopyalama zaman aşımına uğradı yedekleme Kasası'ndan Lütfen işlemi birkaç dakika içinde yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. | Bu depolama tarafında geçici bir hata varsa veya yedekleme hizmeti yeterli IOPS kasa için zaman aşımı süresi içinde veri aktarımı için VM barındırma depolama hesabından almaktır değil gerçekleşir. Uyguladığınız emin olun [en iyi uygulamalar](backup-azure-vms-introduction.md#best-practices) yedekleme ayarı oluştu. Farklı bir depolama alanına VM yedekleme hesap yüklü değil ve yeniden deneyin taşımayı deneyin.|
| Yedekleme bir iç hatayla başarısız oldu - Lütfen işlemi birkaç dakika içinde yeniden deneyin. Sorun devam ederse, Microsoft Support başvurun |2 nedenlerden dolayı bu hatayı alabilirsiniz: <ol><li> VM depolama erişirken geçici bir sorun yoktur. Lütfen denetleyin [Azure durum](https://azure.microsoft.com/en-us/status/) herhangi devam eden işlem ilgili bir sorun, depolama veya bölgede ağ olup olmadığını görmek için. Sorun çözüldükten sonra yedekleme işini yeniden deneyin. <li>Orijinal VM silinecek ve bu nedenle, kurtarma noktası alınamaz. Silinen bir VM için yedekleme verileri tutmak, ancak yedekleme hataları kaldırmak için: VM korumasını kaldırın ve verileri koruma seçeneğini seçin. Bu eylem, zamanlanmış yedekleme işi ve yinelenen hata iletileri durdurur. |
| Seçilen öğe üzerinde - Azure kurtarma Hizmetleri Uzantısı yüklenemedi VM Aracısı Azure kurtarma Hizmetleri uzantısı için önkoşuldur. Azure VM aracısı yükleyin ve kayıt işlemini yeniden başlatın |<ol> <li>VM Aracısı'nı doğru şekilde yüklenip yüklenmediğini denetleyin. <li>VM yapılandırma bayrağı doğru ayarlandığından emin olun.</ol> [Daha fazla bilgi](#validating-vm-agent-installation) VM aracısı ve VM Aracısı yüklemesini doğrulamak nasıl yükleme hakkında. |
| Uzantı yüklemesi "COM + için Microsoft Distributed Transaction Coordinator konuşun başaramadı hatasıyla başarısız oldu |Bu, genellikle COM + hizmeti çalışmadığı anlamına gelir. Bu sorunu düzeltme konusunda yardım için Microsoft Destek'e başvurun. |
| Anlık görüntü işlemi, "Bu sürücünün BitLocker Sürücü Şifrelemesi tarafından kilitlenmiş. VSS işlemi hatasıyla başarısız oldu Denetim Masası'ndan bu sürücünün kilidini açmanız gerekir. |VM üzerindeki tüm sürücüleri için BitLocker'ı açın ve VSS sorunun giderilip giderilmediğini inceleyin |
| VM yedeklemeleri izin veren bir durumda değil. |<ul><li>VM aşağı çalışan ve bilgisayarı arasında geçici bir durumda olup olmadığını denetleyin. İse, bunlardan birini olmasını ve tetiklemek için VM durumu bekleyin yeniden yedekleme. <li> VM bir Linux VM kullanır [güvenlik Gelişmiş Linux] çekirdek modülü ise, Linux Aracısı yolu dışla gerekir (_/var/lib/waagent_) emin yedekleme uzantısını yapmak için ilke güvenlikten yüklü.  |
| Azure sanal makine bulunamadı. |Bu, birincil VM silindiği, ancak yedekleme gerçekleştirmek bir VM aramak yedekleme ilkenizi devam durumlarda gerçekleşir. Bu hatayı düzeltmek için: <ol><li>Aynı kaynak grubu adı [bulut hizmet adı] ve aynı ada sahip sanal makine oluşturun <br>(VEYA) <li> Yedekleme işleri oluşturulmayacak şekilde bu VM için korumayı devre dışı bırakın. </ol> |
| Sanal makine Aracısı sanal makinede mevcut değil - Lütfen tüm önkoşul ve VM aracısı yükleyin ve işlemi yeniden deneyin. |[Daha fazla bilgi](#vm-agent) VM Aracısı yükleme ve VM Aracısı yüklemesini doğrulamak nasıl hakkında. |
| Anlık görüntü işlemi hatalı durumda VSS yazıcılarının nedeniyle başarısız oldu |Hatalı durumda (birim gölge kopyası hizmeti) VSS yazıcılarının yeniden başlatmanız gerekir. Bu, yükseltilmiş bir komut isteminden elde etmek için Çalıştır _vssadmin listesi yazıcılarının_. Çıktı tüm VSS yazıcılarının ve durumlarını içerir. Değil "[1] kararlı" durumu olan her VSS yazıcısı için VSS Yazıcı komutları yükseltilmiş komut isteminden aşağıdaki çalıştırarak yeniden başlatın:<br> _net stop serviceName_ <br> _net start serviceName_|
| Anlık görüntü işlemi ayrıştırma bir yapılandırma hatası nedeniyle başarısız oldu |Bu MachineKeys dizinindeki değiştirilen izinler nedeniyle gerçekleşir: _%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br>Lütfen aşağıdaki komutunu çalıştırın ve varsayılan olanları MachineKeys dizin üzerindeki izinleri doğrulayın:<br>_icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br><br> Varsayılan izinler şunlardır:<br>Everyone:(R,W) <br>BUILTIN\Administrators:(F)<br><br>MachineKeys dizinindeki varsayılandan farklı izinleri görürseniz, lütfen izinleri düzeltmek, sertifikayı silmek ve yedekleme tetiklemek için aşağıdaki adımları izleyin.<ol><li>İzinleri MachineKeys dizinde düzeltin.<br>Explorer güvenlik özellikleri ve Gelişmiş güvenlik ayarları dizinde kullanarak, izinler varsayılan değerlere sıfırla, herhangi bir ek (varsayılan) daha kullanıcı nesnesi dizinden kaldırın ve 'Everyone' özel erişim için izin verilen emin olun:<br>-Liste klasörü / veri okuma <br>-Öznitelikleri okuması <br>-Genişletilmiş öznitelikleri okuması <br>-Dosya Oluştur / Veri Yaz <br>-Klasör Oluştur / Veri Ekle<br>-Öznitelikleri yazma<br>-Yazma genişletilmiş öznitelikleri<br>-Okuma izinleri<br><br><li>Tüm sertifikaları 'Issued To' alanıyla silmek = "Windows Azure hizmet yönetimi için uzantıları" veya "Windows Azure CRP sertifika Oluşturucu".<ul><li>[Sertifikalar (yerel bilgisayar) konsolunu açın](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx)<li>Tüm sertifikaları sil (Sertifikalar -> kişisel altında) 'İçin verilen' alanıyla = "Windows Azure hizmet yönetimi için uzantıları" veya "Windows Azure CRP sertifika Oluşturucu".</ul><li>Tetikleyici VM yedekleme. </ol>|
| Sanal makine BEK ile tek başına şifrelenmiş olarak doğrulaması başarısız oldu. Yalnızca şifrelenmiş BEK ve KEK ile sanal makineleri için yedeklemeleri etkinleştirilebilir. |Sanal makine, BitLocker şifreleme anahtarını ve anahtar şifreleme anahtarı kullanarak şifrelenmelidir. Bundan sonra yedekleme etkinleştirilmelidir. |
| Azure Backup hizmeti, anahtar kasası için yeterli izinlere için yedekleme şifrelenmiş sanal makinelerin yok. |Yedekleme hizmeti sağlanmalıdır PowerShell'de bu izinleri belirtilen adımları kullanarak **yedeklemeyi etkinleştir** bölümünü [PowerShell belgelerine](backup-azure-vms-automation.md). |
|Yüklemesini anlık görüntü uzantı hatasıyla başarısız oldu - COM + Microsoft Dağıtılmış İşlem Düzenleyicisi ile iletişim kuramadı | Lütfen windows hizmeti "COM + Sistem uygulaması" başlatmayı deneyin (yükseltilmiş komut isteminden - _net Başlat COMSysApp_). <br>Başlatma sırasında başarısız olursa, lütfen aşağıdaki adımları izleyin:<ol><li> "Dağıtılmış İşlem Düzenleyicisi" hizmetinin oturum açma hesabı "Network Service" olduğunu doğrulayın. Değilse, lütfen "Network Service" değiştirin, bu hizmeti yeniden başlatın ve "COM + Sistem uygulaması" hizmetini başlatmak daha sonra deneyin.'<li>Başlatmak yine başarısız olursa, kaldırma/hizmet "Dağıtılmış İşlem Düzenleyicisi" aşağıdaki adımları izleyerek yükle:<br> -MSDTC hizmetini durdurun<br> -Bir komut istemi (cmd) açın <br> -Komutu çalıştır "msdtc-kaldırma" <br> -Komutu çalıştır "msdtc-yükleyin" <br> -MSDTC hizmeti Başlat<li>Windows hizmeti "COM + Sistem uygulaması" başlatın ve başlatıldıktan sonra portalından yedeklemeyi başlatın.</ol> |
|  Anlık görüntü işlemi COM + hata nedeniyle başarısız oldu | Önerilen eylemi "COM + Sistem uygulaması" windows hizmeti yeniden başlatmaktır (yükseltilmiş komut isteminden - _net Başlat COMSysApp_). Sorun devam ederse, VM'yi yeniden başlatın. VM'yi yeniden başlatırken işe yaramazsa, deneyin [VMSnapshot uzantısını kaldırma](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#cause-3-the-backup-extension-fails-to-update-or-load) ve yedeklemeyi el ile başlatın. |
| Bir veya daha fazla bağlama noktalarının dosya sistemi ile tutarlı bir anlık görüntü almak için VM'nin dondurmak başarısız oldu | Aşağıdaki adımları kullanın: <ol><li>Tüm takılı cihazlar kullanarak dosya sistemi durumunu denetleyip _'tune2fs'_ komutu.<br> Örneğin: tune2fs -m/dev/sdb1 \| grep "Dosya sistemi durumu" <li>İçin hangi dosya sistemi durumu değil temiz kullanarak cihazları çıkarın _'umount'_ komutu <li> Kullanarak bu cihazları üzerinde FileSystemConsistency denetimi Çalıştır _'fsck'_ komutu <li> Aygıtları yeniden bağlayın ve yedekleme deneyin.</ol> |
| Anlık görüntü işlemi iletişim kanalını güvenli ağ oluşturma hatası nedeniyle başarısız oldu | <ol><Li> Yükseltilmiş modda regedit.exe çalıştırarak kayıt defteri Düzenleyicisi'ni açın. <li> Tüm sürümleri tanımlayın. NetFramework sistemde mevcut. Kayıt defteri anahtarı "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft" hiyerarşisi altında mevcut <li> Her biri için. Kayıt defteri anahtarında mevcut NetFramework anahtarı ekleyin: <br> "SchUseStrongCrypto"=dword:00000001 </ol>|
| Visual C++ yeniden dağıtılabilir yüklenmesi için Visual Studio 2012 hata nedeniyle anlık görüntü işlemi başarısız oldu | Navigate to C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion and install vcredist2012_x64. Bu hizmeti yüklemesi izin vermek için kayıt defteri anahtarı değerini yani kayıt defteri anahtarının değeri doğru değerine ayarlandığından emin olun _HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSIServer_ 3 ve 4'değil ayarlayın. Yükleme sorunları devam bakacak, çalıştırarak Yükleme hizmeti yeniden başlatın _MSIEXEC /UNREGISTER_ arkasından _MSIEXEC /REGISTER_ yükseltilmiş bir komut isteminden.  |


## <a name="jobs"></a>İşler
| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| İptal bu iş türü için desteklenmeyen - iş tamamlanana kadar bekleyin. |Hiçbiri |
| İş iptal edilebilen bir durumda değil - iş tamamlanana kadar bekleyin. <br>OR<br> Seçilen işin iptal edilebilen bir durumda değil - işi tamamlamak lütfen bekleyin. |Tüm olasılığını içinde iş neredeyse tamamlandı. İş tamamlanana kadar bekleyin.|
| İş sürüyor değil - iptal yalnızca sürmekte olan işleri için desteklenen olduğundan iptal edilemez. Lütfen girişimi iptal bir sürüyor işi. |Bu hatanın nedeni geçici bir durum nedeniyle oluşur. Bir dakika bekleyin ve İptal işlemi yeniden deneyin. |
| Başarısız işi iptal-işi tamamlanana kadar bekleyin. |Hiçbiri |

## <a name="restore"></a>Geri Yükleme
| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| Geri yükleme bulut iç hata vererek başarısız oldu |<ol><li>Bulut hizmeti geri yüklemeye çalıştığınız DNS ayarlarıyla yapılandırılır. Kontrol edebilirsiniz <br>$deployment get-AzureDeployment - ServiceName "ServiceName" =-yuvası "Üretim" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Yapılandırılmış adresi varsa, bu DNS ayarlarının yapılandırıldığını anlamına gelir.<br> <li>Bulut hizmeti olduğu için geri yüklemeye çalıştığınız ReservedIP ile yapılandırıldığından ve bulut hizmetindeki VM'ler durdurulmuş durumda.<br>Bir bulut hizmeti powershell cmdlet'lerini kullanarak IP ayrılmış denetleyebilirsiniz:<br>$deployment get-AzureDeployment - ServiceName "servicename" =-yuvası "Üretim" $DEP Reservedıpname <br><li>Aşağıdaki özel ağ yapılandırmaları bir sanal makineyle aynı bulut hizmetine geri yüklemeye çalıştığınız. <br>-Sanal makineler yük dengeleyici yapılandırması (dahili ve harici)<br>-Sanal makinelerle birden çok ayrılmış IP<br>-Birden çok NIC içeren sanal makinelere<br>Lütfen yeni bir bulut hizmeti kullanıcı Arabiriminde seçin veya lütfen [konuları geri](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) özel ağ yapılandırmaları olan VM'ler için.</ol> |
| Seçili DNS adı zaten alınmış - Lütfen farklı bir DNS adı belirtin ve yeniden deneyin. |Bulut hizmeti adı için DNS adını buraya başvuruyor (genellikle ile biten. cloudapp.net). Bu, benzersiz olması gerekir. Bu hatayla karşılaşırsanız, geri yükleme sırasında farklı bir VM adı seçmeniz gerekir. <br><br> Bu hata yalnızca Azure portalında kullanıcılara gösterilir. Yalnızca diskleri geri yükler ve VM oluşturma değil çünkü PowerShell aracılığıyla geri yükleme işlemi başarılı olur. Disk geri yükledikten sonra işlemi VM açıkça sizin tarafınızdan oluşturulduğunda hata karşılaştığı. |
| Belirtilen sanal ağ yapılandırması doğru değil - Lütfen farklı bir sanal ağ yapılandırması belirtin ve yeniden deneyin. |Hiçbiri |
| Belirtilen bulut hizmeti değil geri yüklenen sanal makine yapılandırmasıyla eşleşen - Lütfen ayrılmış IP kullanarak değil, farklı bir bulut hizmeti belirtin veya geri yüklemek için başka bir kurtarma noktası seçin bir ayrılmış IP kullanıyor. |None |
| Bulut hizmeti giriş uç noktası sayısı sınırına - mevcut bir uç noktası kullanarak veya farklı bir bulut hizmeti belirterek işlemi yeniden deneyin. |Hiçbiri |
| Yedekleme kasası ve hedef depolama hesabı olan iki farklı bölgelerde - geri yükleme işleminde belirtilen depolama hesabı aynı Azure bölgesinde yedekleme kasası olarak olduğundan emin olun. |Hiçbiri |
| Bir geri yükleme işlem için belirtilen depolama hesabına desteklenen - yalnızca temel/standart depolama hesapları ile yerel olarak yedekli veya coğrafi olarak yedekli çoğaltma ayarları desteklenir. Lütfen desteklenen depolama hesabı seçin |None |
| Geri yükleme işlemi için belirtilen depolama hesabı türü çevrimiçi değil - geri yükleme işleminde belirtilen depolama hesabı çevrimiçi olduğundan emin olun |Azure Storage veya kesinti nedeniyle geçici bir hata nedeniyle gerçekleşebilir. Lütfen başka bir depolama hesabı seçin. |
| Kaynak grubu kotasına ulaşıldı - Lütfen Azure Portalı'ndan bazı kaynak gruplarını silin veya sınırları artırmak için Azure desteğine başvurun. |Hiçbiri |
| Seçilen alt ağ yok - Lütfen var olan bir alt ağ seçin |None |
| Yedekleme Hizmeti'nin aboneliğinizdeki kaynaklara erişme yetkisi yok. |Bu, ilk geri yükleme bölümünde belirtilen adımları kullanarak diskleri sorunu çözmek için **yedeklenmiş diskleri geri yükleme** içinde [VM seçerek geri yükleme yapılandırmasını](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Bundan sonra belirtilen PowerShell adımları kullanın [geri yüklenen disklerden bir VM oluşturmak](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) geri yüklenen disklerden tam VM oluşturmak için. |

## <a name="backup-or-restore-taking-time"></a>Yedekleme veya geri yükleme sürüyor
Görürseniz, backup(>12 hours) veya geri alma time(>6 hours):
* Anlamak [yedekleme saati katkıda bulunan Etkenler](backup-azure-vms-introduction.md#total-vm-backup-time) ve [zaman geri katkıda bulunan Etkenler](backup-azure-vms-introduction.md#total-restore-time).
* Takip ettiğinizden emin olun [yedekleme en iyi yöntemler](backup-azure-vms-introduction.md#best-practices). 

## <a name="vm-agent"></a>VM Aracısı
### <a name="setting-up-the-vm-agent"></a>VM Aracısı'nı ayarlama
Genellikle, VM Aracısı zaten Azure galerisinden oluşturulan VM'ler bulunur. Ancak, şirket içi veri merkezlerinden Geçişi sanal makineleri VM Aracısı'nın yüklü sahip olmaz. Bu tür VM'ler için VM Aracısı açıkça yüklü olması gerekir.

Windows VM'ler için:

* [Aracı MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir.
* Klasik sanal makineler için [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) aracının yüklü olduğunu belirtmek için. Bu adım, Resource Manager sanal makineler için gerekli değildir.

Linux VM'ler için:

* Son dağıtım depodan yükleyin. Biz **tavsiye** yalnızca dağıtım deposu üzerinden yükleme Aracısı. Paket adı hakkında daha fazla bilgi için lütfen [Linux Aracısı deposu](https://github.com/Azure/WALinuxAgent) 
* Klasik VM'ler için [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) aracının yüklü olduğunu belirtmek için. Bu adım, Resource Manager sanal makineler için gerekli değildir.

### <a name="updating-the-vm-agent"></a>VM Aracısı'nı güncelleştirme
Windows VM'ler için:

* VM Aracısı'nı güncelleştirmek için [VM Aracısı ikili dosyalarının](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yeniden yüklenmesi yeterlidir. Ancak, VM Aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştığından emin olmak gerekir.

Linux VM'ler için:

* Yönergeleri izleyerek [güncelleştirme Linux VM Aracısı](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Biz **tavsiye** yalnızca dağıtım deposu aracılığıyla güncelleştirme Aracısı. Doğrudan github'dan aracısı kodu indiriliyor ve güncelleştirmeden önermiyoruz. En son aracı, dağıtım için kullanılabilir durumda değilse, lütfen en son aracı yüklemek yönergeler dağıtım desteği için ulaşın. Son kontrol edebilirsiniz [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) github deposunu bilgileri.

### <a name="validating-vm-agent-installation"></a>VM Aracısı yüklemesini doğrulama
Windows vm'lerde VM Aracısı'nın sürümünü denetlemek nasıl:

1. Azure sanal makinede oturum açın ve klasöre gidin *C:\WindowsAzure\Packages*. Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.
2. Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün sürümü alanı 2.6.1198.718 olmalıdır veya üzeri

## <a name="troubleshoot-vm-snapshot-issues"></a>VM anlık görüntü sorunlarını giderme
VM yedekleme anlık görüntü komutları için temel alınan depolama veren kullanır. Depolama veya gecikmeleri erişimi bir anlık görüntü görev yürütme olmaması, yedekleme işi başarısız olmasına neden olabilir. Aşağıdaki anlık görüntü görev başarısız olmasına neden olabilir.

1. NSG kullanılarak depolamaya ağ erişimi engellendi<br>
    Nasıl konusunda daha fazla bilgi edinin [ağ erişimini etkinleştir](backup-azure-arm-vms-prepare.md#establish-network-connectivity) ya da uygulamaları güvenilir listeye almayı IP kullanarak depolama alanına veya proxy sunucu üzerinden.
2. Yapılandırılan Sql Server Yedekleme ile VM anlık görüntü görev gecikmeye neden olabilir <br>
   Varsayılan VM yedekleme sorunları VSS tam yedekleme ile Windows sanal makineleri üzerinde. Sql sunucuları ve Sql Server Yedekleme yapılandırılıp yapılandırılmadığını çalıştırmakta olan VM'ler üzerinde bu anlık görüntü yürütme gecikmesi neden olabilir. Lütfen yedekleme hataları nedeniyle anlık görüntü sorunları yaşıyorsanız, aşağıdaki kayıt defteri anahtarını ayarlayın.

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```
3. RDP VM'yi kapatın olduğundan VM durumu yanlış bildirdi.  <br>
   RDP, sanal makineyi kapatmanız değilse, portala VM durumu düzgün yansıtılmasını gözden geçirin. Aksi halde, lütfen VM panosunda 'Kapatma' seçeneğini kullanarak portal VM'yi kapatın.
4. Birden fazla dört sanal makineleri aynı bulut hizmetine paylaşıyorsanız, yedekleme kez böylece hiçbir dörtten fazla VM yedeklemeleri aynı anda kullanmaya hazırlamak için birden çok yedekleme ilkelerini yapılandırın. Arasında ilkeler birbirinden saatte bir kez yedekleme başlangıç yaymak deneyin.
5. VM yüksek CPU/bellek çalışıyor.<br>
   Sanal makine yüksek CPU çalışıyorsa usage(>90%) veya bellek, anlık görüntü görev sıraya, Gecikmeli ve sonunda zaman aşımına uğradı alır. Böyle durumlarda talep üzerine yedekleme deneyin.

<br>

## <a name="networking"></a>Ağ
Tüm uzantıları gibi Backup uzantısının çalışması için ortak internet erişimi gerekir. Genel internet erişimi olmaması kendisini çeşitli şekillerde bildirilebilir:

* Genişletme yüklemesinin başarısız olabilir
* (Örneğin, disk anlık görüntü) yedekleme işlemleri başarısız olabilir
* Yedekleme işlemi durumunu görüntüleme başarısız olabilir

Ortak internet adresleri çözmek için gereken geliştirilmiştir [burada](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Vnet DNS yapılandırmalarını denetleyin ve Azure URI'ler çözümlenebildiğinden emin olmak gerekir.

Ad çözümlemesi doğru yaptıktan sonra Azure IP'leri erişimi de sağlanmalıdır. Azure altyapı erişimi engelini kaldırmak için aşağıdaki adımlardan birini izleyin:

1. Beyaz liste Azure veri merkezi IP aralıkları.
   * Listesini almak [Azure veri merkezi IP](https://www.microsoft.com/download/details.aspx?id=41653) Güvenilenler listesine olmalıdır.
   * IP'leri kullanarak engellemesini [yeni NetRoute](https://technet.microsoft.com/library/hh826148.aspx) cmdlet'i. Azure VM dahilinde bu cmdlet'i yükseltilmiş bir PowerShell penceresi (yönetici olarak çalıştır) çalıştırın.
   * (Bir yerinde yoksa) kuralları NSG'yi IP'leri erişmesine izin vermek için ekleyin.
2. Akış HTTP trafiği için bir yol oluşturma
   * Varsa bazı ağ kısıtlaması yerde (bir ağ güvenlik grubu, örneğin) trafiği yönlendirmek için bir HTTP proxy sunucusu dağıtın. Bir HTTP Proxy sunucusu dağıtmak için adımları bulunan [burada](backup-azure-arm-vms-prepare.md#establish-network-connectivity).
   * (Bir yerinde yoksa) kuralları NSG'yi HTTP Proxy sunucudan INTERNET'e erişim izni ekleyin.

> [!NOTE]
> DHCP çalışmaya Iaas VM yedekleme Konuk içinde etkinleştirilmesi gerekir.  Statik bir özel IP gerekiyorsa platformu ile yapılandırmanız gerekir. VM içindeki DHCP seçeneği sol etkinleştirilmiş olmalıdır.
> Hakkında daha fazla bilgi görüntülemek [statik iç özel IP ayarı](../virtual-network/virtual-networks-reserved-private-ip.md).
>
>
