---
title: "Azure Backup sunucusu sorunlarını giderme | Microsoft Docs"
description: "Yükleme, kayıt Azure yedekleme sunucusu ve yedekleme ve geri yükleme uygulama iş yüklerinin sorunlarını giderme"
services: backup
documentationcenter: 
author: pvrk
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: pullabhk;markgal;
ms.openlocfilehash: 7ef808e933d1c3ff88e18766cd3a29138959297a
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="troubleshoot-azure-backup-server"></a>Azure Backup Sunucusu sorunlarını giderme

Azure yedekleme sunucusu bilgileri kullanarak aşağıdaki tabloda listelenen sırasında oluşan hatalar giderebilirsiniz.

## <a name="error-invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Hata: Geçersiz kasa kimlik bilgileri sağlandı. Dosya bozuk veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile 

Bunlar [sorun giderme adımları] izleyin (https://docs.microsoft.com/en-us/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues.), bu sorunu gidermek için.

## <a name="error-the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-server"></a>Hata: Sunucusundaki DPM Agent Coordinator hizmeti ile iletişim hatası nedeniyle aracı işlemi başarısız oldu 

Bunlar [sorun giderme adımları] izleyin (https://docs.microsoft.com/en-us/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues.), bu sorunu gidermek için.

## <a name="error-setup-could-not-update-registry-metadata"></a>Hata: Kurulum, kayıt defteri meta veriler güncelleştirilemedi

Bunlar [sorun giderme adımları] izleyin (https://docs.microsoft.com/en-us/azure/backup/backup-azure-mabs-troubleshoot#installation-issues.), bu sorunu gidermek için.


## <a name="installation-issues"></a>Yükleme sorunları

| İşlem | Hata ayrıntıları | Geçici çözüm |
|-----------|---------------|------------|
|Yükleme | Kurulum, kayıt defteri meta veriler güncelleştirilemedi. Bu güncelleştirme hatası depolama tüketimini kullanım neden olabilir. Lütfen güncelleştirmede ReFS kırpma kayıt defteri girdisini önlemek için. | SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim kayıt defteri anahtarını ayarlayın. Dword değerini 1 olarak ayarlayın. |
|Yükleme | Kurulum, kayıt defteri meta veriler güncelleştirilemedi. Bu güncelleştirme hatası depolama tüketimini kullanım neden olabilir. Lütfen güncelleştirmede birim SnapOptimization kayıt defteri girdisini önlemek için. | Boş dize değeri ile SOFTWARE\Microsoft veri koruma Manager\Configuration\VolSnapOptimization\WriteIds, bir kayıt defteri anahtarı oluşturun. |

## <a name="registration-and-agent-related-issues"></a>Kayıt ve aracı ilgili sorunlar
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Bir kasaya kaydetme | Geçersiz kasa kimlik bilgileri sağlandı. Dosya bozuk veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile | Bu hatayı düzeltmek için: <ol><li> Kasadan en son kimlik bilgileri dosyasını indirin ve tekrar deneyin <br>(VEYA)</li> <li> Yukarıdaki eylem işe yaramazsa, farklı bir yerel dizine kimlik bilgilerini indirme deneyin veya yeni bir kasa oluşturun <br>(VEYA)</li> <li> Tarih güncelleştirmeyi deneyin ve zaman içinde belirtildiği gibi ayarları [bu Web günlüğü](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/) <br>(VEYA)</li> <li> C:\windows\temp birden fazla 65000 dosyaları olup olmadığını denetleyin. Eski dosyaları başka bir konuma taşıyın veya Temp klasörü içinde öğeleri silin <br>(VEYA)</li> <li> Sertifikaların durumunu denetleyin <br> a. "Bilgisayar sertifikaları (Denetim Masası'nda)" yönetin <br> b. "Kişisel" ve onun alt düğümlerini "Sertifikalar" açın <br> c.  "Windows Azure Araçları" sertifikayı Kaldır <br> d. Azure Backup istemcisindeki kayıt işlemini yeniden deneyin <br> (VEYA) </li> <li> Her Grup İlkesi yerinde olup olmadığını denetleyin </li></ol> |
| Aracıların korumalı sunucularına iletme | Üzerindeki DPM aracı Düzenleyicisi hizmetindeki bir iletişim hatası nedeniyle aracı işlemi başarısız \<sunucuadı > | **Üründe gösterilen önerilen eylem işe yaramazsa**, <ol><li> Bir bilgisayar güvenilmeyen bir etki alanından bağlıyorsanız izleyin [bu](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx) adımları <br> (VEYA) </li><li> Bir bilgisayar güvenilen bir etki alanından bağlıyorsanız özetlenen adımları kullanarak sorun giderme [bu Web günlüğü](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/) <br>(VEYA)</li><li> Sorun giderme adımı olarak virüsten koruma devre dışı bırakmayı deneyin. Sorun çözümlenirse, önerilen [Bu makalede] olarak virüsten koruma ayarlarını değiştirme (https://technet.microsoft.com/library/hh757911.aspx)</li></ol> |
| Aracıların korumalı sunucularına iletme | Sunucusu için belirtilen kimlik bilgileri geçersiz | **Üründe gösterilen önerilen eylem işe yaramazsa**, <br> koruma aracısını el ile belirtildiği gibi üretim sunucusundaki yüklemeye [bu makalede](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual)|
| Azure Yedekleme aracısı Azure yedekleme hizmetine bağlanamadı (kimlik: 100050) | Azure Yedekleme aracısı Azure yedekleme hizmetine bağlanamadı. | **Üründe gösterilen önerilen eylem işe yaramazsa**, <br>1. Psexec yükseltilmiş gelen komut isteminde, komutu çalıştırın -i - Internet explorer penceresi açılır s "c:\Program Files\Internet Explorer\iexplore.exe". <br/> 2. Araçlar -> Internet Seçenekleri -> bağlantıları LAN Ayarları ->. <br/> 3. Sistem hesabı için proxy ayarlarını doğrulayın. Proxy IP ve bağlantı noktası ayarlayın. <br/> 4. Internet Explorer'ı kapatın.|
| Azure Backup Aracısı yüklemesi başarısız oldu | Microsoft Azure kurtarma Hizmetleri yüklemesi başarısız oldu. Microsoft Azure kurtarma Hizmetleri yüklemesi sistem tarafından yapılan tüm değişiklikler geri alındı. (KİMLİK: 4024) | El ile Azure aracısını yükleyin


## <a name="configuring-protection-group"></a>Koruma grubu yapılandırma
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Koruma gruplarını yapılandırma | DPM, korunan bilgisayarda (korunan bilgisayar adı) uygulama bileşeni numaralandıramadı | '' İlgili veri kaynağı/bileşen düzeyinde yapılandırma koruma grubu kullanıcı Arabirimi ekranında Yenile |
| Koruma gruplarını yapılandırma | Koruma yapılandırılamıyor | Korunan bir SQL server sunucusuysa, sysadmin rolü izinleri Korunan bilgisayarda sistem hesabına (ntauthority\system adlı) içinde belirtildiği gibi sağlanmış olan olup olmadığını denetleyin [bu makalede](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx)
| Koruma gruplarını yapılandırma | Bu koruma grubu için depolama havuzunda yeterli boş alan yok | Depolama havuzuna eklenen diskler [bir bölüm içermemelidir](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Disklerde var olan tüm birimler silin ve sonra depolama havuzuna ekleyin|
| İlke değişikliği |Yedekleme İlkesi değiştirilemedi. Hata: Bir iç hata nedeniyle [0x29834] geçerli işlem başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse lütfen Microsoft desteğine başvurun. |**Neden:**<br/>Güvenlik ayarları etkin, desteklenmeyen bir sürümü (aşağıda MAB sürüm 2.0.9052 ve Azure yedekleme sunucusu güncelleştirme 1 ') bulunuyor ve, yukarıda belirtilen minimum değerler aşağıda saklama aralığını çalıştığınızda bu hatayı gelir. <br/>**Önerilen eylem:**<br/> Bu durumda, yukarıda belirtilen minimum bekletme (günlük, haftalık, üç hafta aylık veya yıllık yedekleme için bir yıl için dört hafta için yedi gün) saklama dönemi ayarlamalısınız İlkesi ile devam etmek için güncelleştirdi ilgili. İsteğe bağlı olarak, tercih edilen yaklaşım yedekleme aracısını ve Azure yedekleme sunucusu tüm güvenlik güncelleştirmelerini yararlanacak şekilde güncelleştirmek olur. |

## <a name="backup"></a>Backup
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Backup | Çoğaltma tutarsız | Lütfen koruma grubu Sihirbazı'nı açık seçeneği işaretleyin. Bu otomatik consitency emin olun. Çoğaltma tutarsızlık ve ilgili öneriler nedenleri hakkında daha fazla ayrıntı [burada](https://technet.microsoft.com/library/cc161593.aspx) <br> <ol><li> Windows Server Yedekleme yüklü olup olmadığını veya korumalı sunucuda değil, sistem durumu/BMR yedekleme durumunda lütfen denetleyin </li><li> Denetlemek için alanı ilgili sorunlar DPM depolama havuzu DPM/MABS sunucusunda ve gerektikçe depolama alanı Ayır </li><li> Korunan sunucuda Birim Gölge Kopyası Hizmeti durumunu denetleyin. Devre dışı durumda ise, sunucu üzerinde hizmeti el ile başlatın ve şekilde ayarlayın. Ardından DPM/MABS konsola geri dönün ve tutarlılık denetimiyle birlikte bir eşitleme başlatın.</li></ol>|
| Backup | İş çalışırken beklenmeyen bir hata oluştu, cihaz hazır değil | **Üründe gösterilen önerilen eylem işe yaramazsa**, <br> <ol><li>Koruma grubundaki öğelerin üzerinde sınırsız için gölge kopya depolama alanı ayarlama ve tutarlılık denetimini Çalıştır <br></li> (VEYA) <li>Varolan bir koruma grubu silmeyi deneyin ve birden çok yenilerini – tek tek her öğesinde biriyle oluşturun</li></ol> |
| Backup | Yalnızca sistem durumu yedeklemesi yedekleme yapıyorsanız, korunan bilgisayarda Sistem Durumu yedeğini depolamak için yeterli boş alan olup olmadığını doğrulayın | <ol><li>WSB, korunan bir makinede yüklü olduğundan emin olun</li><li>Yeterli alan sistem durumunu korumalı bilgisayar üzerinde var olduğunu doğrulayın: korunan bilgisayara WSB açın ve Seçimlerinizde'ı tıklatın ve BMR seçin gitmek için bunu yapmanın en kolay yolu değil. Kullanıcı arabirimini sonra ne kadar bu gerekli bir alandır söyler. Açık WSB -> yerel yedekleme -> Yedekleme zamanlaması seçin yedekleme yapılandırması -> tam sunucu -> (boyutu görüntülenir). Bu boyut doğrulama için kullanın.</li></ol>
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | Hata iletisi diyorsa "Windows Azure Yedekleme aracısı Seçili birimin anlık görüntüsünü oluşturamadı", Lütfen çoğaltma ve kurtarma noktası birim alanı artırmayı deneyin.
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | Hata iletisi "Windows Azure Yedekleme aracısı OBEngine hizmetine bağlanamıyor" diyorsa OBEngine Hizmetleri bilgisayarda çalışan listesinde var olduğunu doğrulayın. OBEngine hizmetinin çalışır durumda değilse OBEngine hizmetini başlatmak için "net start OBEngine" komutunu kullanın.
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | Hata iletisi diyorsa "Bu sunucunun şifreleme parolası ayarlanmamış. Lütfen bir şifreleme parolası yapılandırın"bir şifreleme parolası yapılandırmayı deneyin. Başarısız olursa, <br> <ol><li>Karalama konumu veya mevcut olup olmadığını denetleyin. Kayıt defterinde HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config "ScratchLocation" adıyla belirtilen konuma var olmalıdır.</li><li> Karalama konum mevcutsa, eski parolayı kullanarak yeniden deneyin. **Lütfen bir şifreleme parolası yapılandırdığınız zaman, güvenli bir konuma kaydedin**</li><ol>
| Backup | BMR için yedekleme hatası | BMR boyutu çok büyük, ise işletim sistemi sürücüsü için bazı uygulama dosyalarını taşıdıktan sonra yeniden deneyin |
| Backup | VMWare VM yeni bir MABS sunucuya yeniden koruma eklemek kullanılabilir olarak göstermez. | VMWare özellikleri, eski bir Çekildi MABS sunucuda işaret. Bu sorunu gidermek için: içinde (SC-VMM eşdeğeri) VCenter 'Özet' sekmesi, ardından 'Özel öznitelikleri' için gidin.  Eski MABS sunucu adı 'DPMServer' değerinden silin.  Yeni MABS sunucuya geri dönün ve Syf değiştirme  'Yenile' düğmesini kullandıktan sonra VM koruma eklemek için kullanılabilir olarak bir onay kutusu sunulur. |
| Backup | Dosyaları ve paylaşılan klasörlerin erişilirken hata oluştu | Önerilen virüsten koruma ayarlarını değiştirmeyi deneyin [burada](https://technet.microsoft.com/library/hh757911.aspx)|
| Backup | VMware VM için çevrimiçi kurtarma noktası oluşturma işleri başarısız olur. DPM, ChangeTracking bilgileri alınmaya çalışılırken VMware hatayla karşılaştı. ErrorCode - FileFaultFault (ID 33621) |  1. Etkilenen VM'ler için VMWare üzerinde ctk Sıfırla <br/> Bağımsız disk VMWare yerinde değil onay <br/> Etkilenen VM'ler için korumayı durdurun ve yeniden Yenile düğmesini ile koruyun <br/> Bir bilgi etkilenen VM'ler için Çalıştır|


## <a name="change-passphrase"></a>Parola değiştirme
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Parola değiştirme |Güvenlik girdiğiniz PIN yanlış. Bu işlemi tamamlamak için doğru güvenlik PIN sağlar. |**Neden:**<br/> (Parola değiştirme gibi) kritik işlemi gerçekleştirirken geçersiz veya süresi dolmuş güvenlik PIN girdiğinizde, bu hata verilir. <br/>**Önerilen eylem:**<br/> İşlemi tamamlamak için geçerli güvenlik PIN girmeniz gerekir. PIN almak için Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na gidin > Ayarlar > Özellikler > Güvenlik PIN oluşturun. Bu PIN, parolayı değiştirmek için kullanın. |
| Parola değiştirme |İşlem başarısız oldu. KİMLİĞİ: 120002 |**Neden:**<br/>Bu hata, güvenlik ayarları etkin, parola değiştirmeye ve desteklenmeyen sürümü gelir.<br/>**Önerilen eylem:**<br/> Parolayı değiştirmek için önce en düşük sürüm en düşük 2.0.9052 ve Azure yedekleme sunucusuna minimum güncelleştirme 1, geçerli güvenlik PIN enter yedekleme aracısını güncelleştirmeniz gerekir. PIN almak için Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na gidin > Ayarlar > Özellikler > Güvenlik PIN oluşturun. Bu PIN, parolayı değiştirmek için kullanın. |


## <a name="configure-email-notifications"></a>E-posta bildirimleri yapılandırma
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Office365 hesabı kullanarak e-posta bildirimlerini ayarlama çalışılıyor. | Hata kimliği alma: 2013| **Neden:**<br/> Office 365 hesabı kullanmaya çalışıyor <br/> **Önerilen eylem:**<br/> Emin olmak için ilk şey "İzin anonim geçiş üzerinde bir alma bağlayıcısında" DPM sunucunuz için Exchange'deki Kurulum olmasıdır. Bu yapılandırma konusunda bağlantı şöyledir: http://technet.microsoft.com/en-us/library/bb232021.aspx <br/> Bir iç SMTP geçişi kullanın ve Office 365 sunucunuz kullanarak ayarlamak için gerekir, bu geçiş olması için IIS ayarlayabilirsiniz. <br/> IIS https://technet.microsoft.com/en-us/library/aa995718(v=exchg.65).aspx to setup IIS to relay to O365 kullanarak SMTP o365'e geçiş yapabilmek için DPM sunucusu yapılandırmanız gerekecektir <br/> Önemli Not: adım 3 -> g II ->, kullandığınızdan emin olun user@domain.com biçimi ve olmayan etki alanı\kullanıcı <br/> SMTP sunucusu, 587 numaralı bağlantı noktalarını ve ardından e-postaları alınması gereken kullanıcı e-posta olarak yerel servername kullanılacak noktası DPM. <br/> Kullanıcı adı ve parola DPM SMTP Kurulum sayfasında DPM açıktır etki alanındaki bir etki alanı hesabı olmalıdır. <br/> Not: SMTP sunucusunun adresini değiştirmek için yeni ayarları değişiklik, ayarları kutusu kapatıp emin olmak için yeni değer yansıtır.  Bu şekilde test en iyi uygulamadır yalnızca değiştirme ve sınama her zaman yeni ayarları gidersiniz değil. <br/> Bu işlem sırasında herhangi bir zamanda bu ayarları DPM Konsolu kapatmak ve aşağıdaki kayıt defteri anahtarlarını düzenleyerek temizleyebilir:<br/> HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> SMTPPassword ve SMTPUserName anahtarlarını silin. <br/> Yeniden başlattığınızda kullanıcı Arabiriminde geri ekleyebilirsiniz.
