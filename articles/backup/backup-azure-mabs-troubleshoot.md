---
title: "Azure Backup sunucusu sorunlarını giderme | Microsoft Docs"
description: "Yükleme, kayıt Azure yedekleme sunucusu ve yedekleme ve geri yükleme uygulama iş yüklerinin sorunlarını giderin."
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
ms.date: 11/24/2017
ms.author: pullabhk;markgal;
<<<<<<< HEAD
ms.openlocfilehash: 696f86f616575364bb65021260daf0c8458fc4e9
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
=======
ms.openlocfilehash: e9517672138a4ea7577af1295dea13771733983e
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="troubleshoot-azure-backup-server"></a>Azure Backup Sunucusu sorunlarını giderme

Azure yedekleme sunucusu kullanırken karşılaştığınız hataları gidermek için aşağıdaki tablolarda bilgileri kullanın.

## <a name="invalid-vault-credentials-provided"></a>Geçersiz kasa kimlik bilgileri sağlandı 

Bu sorunu çözümlemek için izlediği [aşağıdaki sorun giderme adımlarını](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues).

## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-the-server"></a>Sunucu üzerindeki DPM aracı Düzenleyicisi hizmeti ile iletişim hatası nedeniyle aracı işlemi başarısız oldu 

Bu sorunu çözümlemek için izlediği [aşağıdaki sorun giderme adımlarını](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues). 

## <a name="setup-could-not-update-registry-metadata"></a>Kurulum, kayıt defteri meta veriler güncelleştirilemedi

Bu sorunu çözümlemek için izlediği [aşağıdaki sorun giderme adımlarını](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#installation-issues).




## <a name="installation-issues"></a>Yükleme sorunları

| İşlem | Hata ayrıntıları | Geçici çözüm |
|-----------|---------------|------------|
|Yükleme | Kurulum, kayıt defteri meta veriler güncelleştirilemedi. Bu güncelleştirme hatası depolama tüketimini overusage için yol açabilir. Bu durumu önlemek için ReFS kırpma kayıt defteri girdisini güncelleştirin. | Kayıt defteri anahtarı ayarlamak **SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim**. Dword değerini 1 olarak ayarlayın. |
|Yükleme | Kurulum, kayıt defteri meta veriler güncelleştirilemedi. Bu güncelleştirme hatası depolama tüketimini overusage için yol açabilir. Bu durumu önlemek için birim SnapOptimization kayıt defteri girdisini güncelleştirin. | Kayıt defteri anahtarını oluşturun **SOFTWARE\Microsoft veri koruma Manager\Configuration\VolSnapOptimization\WriteIds** boş bir dize değerine sahip. |
## <a name="registration-and-agent-related-issues"></a>Kayıt ve aracı ile ilgili sorunlar
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Bir kasaya kaydetme | Geçersiz kasa kimlik bilgileri sağlandı. Dosya bozuk veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile. | Bu hatayı düzeltmek için: <ul><li> Kasadan en son kimlik bilgileri dosyasını indirin ve tekrar deneyin. <br>(VEYA)</li> <li> Önceki eylemi işe yaramazsa, farklı bir yerel dizine kimlik bilgilerini indirme deneyin veya yeni bir kasa oluşturun. <br>(VEYA)</li> <li> Tarih güncelleştirmeyi deneyin ve saat ayarlarını açıklandığı gibi [bu blog](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/). <br>(VEYA)</li> <li> C:\windows\temp birden fazla 65000 dosyaları olup olmadığını denetleyin. Eski dosyaları başka bir konuma taşıyın veya Temp klasörü içinde öğeleri silin. <br>(VEYA)</li> <li> Sertifikaların durumunu denetleyin. <br> a. Açık **bilgisayar sertifikalarını yönetmek** (Denetim Masasında). <br> b. Genişletme **kişisel** ve onun alt düğümlerini **Sertifikalar**.<br> c.  Sertifikayı kaldırmak **Windows Azure Araçları**. <br> d. Azure Backup istemcisindeki kayıt işlemini yeniden deneyin. <br> (VEYA) </li> <li> Her Grup İlkesi yerinde olup olmadığını denetleyin. </li></ul> |
| Aracıların korumalı sunucularına iletme | Üzerindeki DPM aracı Düzenleyicisi hizmetindeki bir iletişim hatası nedeniyle aracı işlemi başarısız \<sunucuadı >. | **Üründe gösterilen önerilen eylemi işe yaramazsa, aşağıdaki adımlar**: <ul><li> Bir bilgisayar güvenilmeyen bir etki alanından bağlıyorsanız izleyin [adımları](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx). <br> (VEYA) </li><li> Bir bilgisayar güvenilen bir etki alanından bağlıyorsanız özetlenen adımları kullanarak sorun giderme [bu blog](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/). <br>(VEYA)</li><li> Sorun giderme adımı olarak virüsten koruma devre dışı bırakmayı deneyin. Sorun çözümlenirse, önerilen içinde virüsten koruma ayarlarını değiştirmek [bu makalede](https://technet.microsoft.com/library/hh757911.aspx).</li></ul> |
| Aracıların korumalı sunucularına iletme | Sunucu için belirtilen kimlik bilgileri geçersiz. | **Üründe gösterilen önerilen eylemi işe yaramazsa, aşağıdaki adımlar**: <br> Koruma aracısını el ile belirtildiği gibi üretim sunucusundaki yüklemeye [bu makalede](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual).|
| Azure Yedekleme aracısı Azure yedekleme hizmetine bağlanamadı (kimlik: 100050) | Azure Yedekleme aracısı Azure yedekleme hizmetine bağlanamadı. | **Üründe gösterilen önerilen eylemi işe yaramazsa, aşağıdaki adımlar**: <br>1. Yükseltilmiş isteminden aşağıdaki komutu çalıştırın: **psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe**. Bu, Internet Explorer penceresi açar. <br/> 2. Git **Araçları** > **Internet Seçenekleri** > **bağlantıları** > **LAN Ayarları**. <br/> 3. Sistem hesabı için proxy ayarlarını doğrulayın. Proxy IP ve bağlantı noktası ayarlayın. <br/> 4. Internet Explorer'ı kapatın.|
| Azure Backup Aracısı yüklemesi başarısız oldu | Microsoft Azure kurtarma Hizmetleri yüklemesi başarısız oldu. Microsoft Azure kurtarma Hizmetleri yüklemesi tarafından sisteme yapılan tüm değişiklikler geri alındı. (KİMLİK: 4024) | El ile Azure aracısını yükleyin.


## <a name="configuring-protection-group"></a>Koruma grubu yapılandırma
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Koruma gruplarını yapılandırma | DPM, korunan uygulama bileşeni oluşturamadı bilgisayar (korunan bilgisayar adı). | Seçin **yenileme** ilgili veri kaynağı/bileşen düzeyinde yapılandırma koruma grubu kullanıcı Arabirimi ekranında. |
| Koruma gruplarını yapılandırma | Koruma yapılandırılamıyor | Korunan bir SQL server sunucusuysa, sysadmin rolü izinleri Korunan bilgisayarda sistem hesabına (ntauthority\system adlı) açıklandığı şekilde sağlandığını doğrulayın [bu makalede](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx).
| Koruma gruplarını yapılandırma | Bu koruma grubu için depolama havuzunda yeterli boş alan yok. | Depolama havuzuna eklenmiş diskler [bir bölüm içermemelidir](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Diskler üzerinde mevcut olan birimleri silin. Sonra depolama havuzuna ekleyin.|
| İlke değişikliği |Yedekleme İlkesi değiştirilemedi. Hata: Bir iç hata nedeniyle [0x29834] geçerli işlem başarısız oldu. Lütfen bir süre geçtikten sonra işlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. |**Neden:**<br/>Bu hata, altında üç koşul oluşur: güvenlik ayarları etkinleştirildiğinde, daha önce belirtilen en düşük değerleri aşağıda saklama aralığını çalıştığınızda ve desteklenmeyen bir sürümünde olduğunda. (Desteklenmeyen sürümler Microsoft Azure yedekleme sunucusu sürümü 2.0.9052 ve güncelleştirme 1'in Azure yedekleme sunucusu aşağıda olanlardır.) <br/>**Önerilen eylem:**<br/> İlke ile ilgili güncelleştirdi ile devam etmek için yukarıda belirtilen minimum bekletme saklama dönemi ayarlayın. (En düşük saklama süresi yedi günlük, haftalık, üç hafta için dört hafta için aylık veya bir yıl için yıllık gündür.) <br><br>İsteğe bağlı olarak, başka bir yedekleme aracısı ve Azure yedekleme sunucusu tüm güvenlik güncelleştirmelerini yararlanacak şekilde güncelleştirmek için bir yaklaşımdır tercih edilir. |

## <a name="backup"></a>Backup
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Backup | Çoğaltma tutarsız | Koruma grubu Sihirbazı'nı otomatik tutarlılık denetimi seçeneğinde açık olduğunu doğrulayın. Çoğaltma tutarsızlık ve ilgili öneriler nedenler hakkında daha fazla bilgi için Microsoft TechNet makalesine bakın [çoğaltma tutarsız](https://technet.microsoft.com/library/cc161593.aspx).<br> <ol><li> Sistem durumu/BMR yedekleme durumunda, Windows Server Yedekleme korunan sunucuda yüklü olduğunu doğrulayın.</li><li> DPM/Microsoft Azure yedekleme sunucusu DPM depolama havuzundaki alan ilgili sorunlar için denetleyin ve gerektikçe depolama alanı ayır.</li><li> Korunan sunucuda Birim Gölge Kopyası Hizmeti durumunu denetleyin. Devre dışı durumda ise, el ile başlayacak şekilde ayarlayın. Hizmet bir sunucuda başlatın. Ardından DPM/Microsoft Azure yedekleme sunucusu konsola geri dönün ve tutarlılık denetimiyle birlikte bir eşitleme başlatın.</li></ol>|
| Backup | İş çalışırken beklenmeyen bir hata oluştu. Cihaz hazır değil. | **Üründe gösterilen önerilen eylemi işe yaramazsa, aşağıdaki adımları uygulayın:** <br> <ul><li>Koruma grubundaki öğelerin üzerinde sınırsız için gölge kopya depolama alanı ayarlayın ve ardından tutarlılık denetimi çalıştırın.<br></li> (VEYA) <li>Varolan koruma silmeyi deneyin grubu ve birden çok yeni grup oluşturma. Her yeni koruma grubu içinde ayrı bir öğe olmalıdır.</li></ul> |
| Backup | Yalnızca sistem durumu yedeklemesi yedekleme yapıyorsanız, korunan bilgisayarda Sistem Durumu yedeğini depolamak için yeterli boş alan olduğunu doğrulayın. | <ol><li>Windows Server Yedekleme için korunan makineye yüklü olduğunu doğrulayın.</li><li>Yeterli alan olduğunu Korunan bilgisayarda sistem durumu için doğrulayın. Bu korumalı bilgisayara Git doğrulayın yapmanın en kolay yolu Windows Server Yedekleme'yi açın, Seçimlerinizde tıklayın ve ardından BMR seçin. Kullanıcı arabirimini daha sonra ne kadar alan gerekli olduğunu bildirir. Açık **WSB** > **yerel yedekleme** > **yedekleme zamanlaması** > **yedekleme yapılandırması seçin**  >  **Tam sunucu** (boyutu görüntülenir). Bu boyut doğrulama için kullanın.</li></ol>
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | Hata iletisi diyorsa "Windows Azure Yedekleme aracısı Seçili birimin anlık görüntüsünü oluşturamadı" çoğaltma ve kurtarma noktası birim alanı artırmayı deneyin.
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | Hata iletisi "Windows Azure Yedekleme aracısı OBEngine hizmetine bağlanamıyor" diyorsa OBEngine Hizmetleri bilgisayarda çalışan listesinde var olduğunu doğrulayın. OBEngine hizmetinin çalışmıyorsa OBEngine hizmetinin başlatmak için "net start OBEngine" komutunu kullanın.
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | Hata iletisi diyorsa "Bu sunucunun şifreleme parolası ayarlanmamış. Lütfen bir şifreleme parolası yapılandırın"deneyin bir şifreleme parolası yapılandırma. Başarısız olursa, aşağıdaki adımları uygulayın: <br> <ol><li>Karalama konumu var olduğunu doğrulayın. Bu kayıt defterinde belirtilen konumdur **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config**, adıyla **ScratchLocation** var olmalıdır.</li><li> Karalama konum mevcutsa, eski parolayı kullanarak yeniden deneyin. *Bir şifreleme parolası yapılandırdığınız zaman, güvenli bir konuma kaydedin.*</li><ol>
| Backup | BMR için yedekleme hatası | BMR boyutu büyükse, bazı uygulama dosyalarını işletim sistemi sürücüye taşıyın ve yeniden deneyin. |
| Backup | Yeni bir Microsoft Azure yedekleme sunucusu üzerinde bir VMware VM yeniden koruma seçeneği eklemek kullanılabilir olarak göstermez. | VMware özellikleri, bir Microsoft Azure yedekleme sunucusu eski, kullanımdan kaldırılan örneğe işaret. Bu sorunu gidermek için:<br><ol><li>VCenter (SC-VMM eşdeğeri), Git **Özet** sekmesini ve sonra **özel öznitelikleri**.</li>  <li>Eski Microsoft Azure yedekleme sunucusu adından silmek **DPMServer** değeri.</li>  <li>Yeni Microsoft Azure yedekleme sunucusu geri dönün ve Syf değiştirme  Siz seçtikten sonra **yenileme** düğmesi, VM koruma eklemek için kullanılabilir olarak bir onay kutusu görünür.</li></ol> |
| Backup | Dosyaları ve paylaşılan klasörlerin erişilirken hata oluştu | TechNet makalesinde önerilen virüsten koruma ayarlarını değiştirmeyi deneyin [DPM sunucusunda virüsten koruma yazılımı çalıştırma](https://technet.microsoft.com/library/hh757911.aspx).|
| Backup | VMware VM için çevrimiçi kurtarma noktası oluşturma işleri başarısız olur. DPM, ChangeTracking bilgileri alınmaya çalışılırken VMware bir hatayla karşılaştı. ErrorCode - FileFaultFault (ID 33621) |  <ol><li> VMware üzerinde CTK etkilenen VM'ler için sıfırlayın.</li> <li>Bu bağımsız disk VMware yerinde olmadığını denetler.</li> <li>Etkilenen VM'ler için korumayı durdurun ve yeniden koruyun **yenileme** düğmesi. </li><li>Bir bilgi etkilenen VM'ler için çalıştırın.</li></ol>|


## <a name="change-passphrase"></a>Parolayı Değiştir
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Parolayı Değiştir |Girilen PIN güvenlik doğru değil. Bu işlemi tamamlamak için PIN doğru güvenlik sağlar. |**Neden:**<br/> Bu hata (örneğin, bir parola değiştirme) önemli bir işlem gerçekleştirirken güvenlik PIN süresi doldu veya geçersiz bir girdiğinizde oluşur. <br/>**Önerilen eylem:**<br/> İşlemi tamamlamak için geçerli bir güvenlik PIN girmeniz gerekir. PIN almak için Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na gidin. Ardından **ayarları** > **özellikleri** > **oluşturmak güvenlik PIN**. Bu PIN, parolayı değiştirmek için kullanın. |
| Parolayı Değiştir |İşlem başarısız oldu. KİMLİĞİ: 120002 |**Neden:**<br/>Bu hata, güvenlik ayarları etkinleştirildiğinde ya da desteklenmeyen bir sürümünü kullanırken parola değiştirmeye çalıştığınızda oluşur.<br/>**Önerilen eylem:**<br/> Parolayı değiştirmek için yedekleme aracısını 2.0.9052 olan en düşük sürüm güncelleştirmeniz gerekir. Ayrıca Azure yedekleme sunucusu en düşük güncelleştirme 1 için güncelleştirme ve geçerli bir güvenlik PIN girmeniz gerekir. PIN almak için Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na gidin. Ardından **ayarları** > **özellikleri** > **oluşturmak güvenlik PIN**. Bu PIN, parolayı değiştirmek için kullanın. |


## <a name="configure-email-notifications"></a>E-posta bildirimleri yapılandırma
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Bir Office 365 hesabınızı kullanarak e-posta bildirimlerini ayarlama |Hata Kimliği: 2013| **Neden:**<br> Office 365 hesabı kullanmaya çalışıyor <br>**Önerilen eylem:**<ol><li> Emin olmak için ilk şey, Exchange'de "İzin anonim geçiş üzerinde bir alma bağlayıcısında" DPM sunucunuz için ayarlandığını ' dir. Bu yapılandırma hakkında daha fazla bilgi için bkz: [alma bağlayıcısında anonim geçişte izin](http://technet.microsoft.com/en-us/library/bb232021.aspx) TechNet'te.</li> <li> Bir iç SMTP geçişi kullanın ve Office 365 sunucunuz kullanarak ayarlamanız gerekir, bir geçiş olması için IIS ayarlayabilirsiniz. DPM sunucusuna yapılandırma [IIS kullanarak SMTP o365'e geçiş](https://technet.microsoft.com/en-us/library/aa995718(v=exchg.65).aspx).<br><br> **Önemli:** kullandığınızdan emin olun user@domain.com biçimi ve *değil* etkialanı\kullanıcı.<br><br><li>SMTP sunucusu olarak yerel sunucu adını kullanacak şekilde noktası DPM 587 bağlantı noktası. Ardından e-postaları alınması gereken kullanıcı e-posta üzerine gelin.<li> Kullanıcı adı ve parola DPM SMTP Kurulum sayfasında DPM açıktır etki alanındaki etki alanı hesabı olması gerekir. </li><br> **Not**: SMTP sunucu adresleri değiştirirken, yeni ayarları değişiklik, ayarlar kutusunu kapatın ve yeni değer gösterdiğinden emin olmak için yeniden açın.  Her zaman sadece değiştirmek ve test bu şekilde test en iyi uygulamadır şekilde etkili olabilmesi yeni ayarları neden.<br><br>Bu işlem sırasında herhangi bir zamanda bu ayarları DPM Konsolu kapatmak ve aşağıdaki kayıt defteri anahtarlarını düzenleyerek temizleyebilirsiniz: **HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> SMTPPassword silin ve SMTPUserName anahtarları**. Yeniden başlattığınızda, kullanıcı Arabirimi yeniden ekleyebilirsiniz.
