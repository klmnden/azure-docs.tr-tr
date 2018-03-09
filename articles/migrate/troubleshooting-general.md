---
title: "Azure geçiş sorunlarını giderme | Microsoft Docs"
description: "Azure geçirmek hizmet ve sorun giderme ipuçları için sık karşılaşılan bilinen sorunlar genel bir bakış sağlar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: troubleshooting
ms.date: 02/21/2018
ms.author: raynew
ms.openlocfilehash: e1e7a1a57f780ef477379dfb1ceaead0c8654970
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="troubleshoot-azure-migrate"></a>Azure Geçişi sorunlarını giderme

## <a name="troubleshoot-common-errors"></a>Sık karşılaşılan hataları giderme

[Azure geçirme](migrate-overview.md) geçiş Azure için şirket içi iş yüklerini değerlendirir. Dağıtma ve Azure geçişi kullanırken sorunları gidermek için bu makaleyi kullanın.


**Toplayıcı internet'e bağlanabiliyor değil**

Bu durum, kullanmakta olduğunuz makine bir proxy'nin arkasında olduğunda ortaya çıkar. Proxy gerekiyorsa yetkilendirme kimlik bilgilerini sağladığınızdan emin olun.
Herhangi bir URL tabanlı bir güvenlik duvarı proxy kullanıyorsanız giden bağlantıyı denetlemek için bu URL'leri gerekli beyaz liste ile emin olun:

**URL** | **Amacı**  
--- | ---
*.portal.azure.com | Azure hizmetiyle bağlantısını denetleyin ve zaman eşitlemesini doğrulamak için gerekli verir.
*.oneget.org | Gerekli powershell indirmek için vCenter Powerclı modülü temel.

**Toplayıcı Proje kimliği kullanarak proje bağlanamaz ve anahtar ı portaldan kopyalanır.**

Kopyalanır ve doğru bilgileri yapıştırılan emin olun. Sorunu gidermek için Microsoft İzleme Aracısı'nı (MMA) yükleyin ve MMA projeye şu şekilde bağlanabildiğinizi doğrulayın:

1. Toplayıcı VM üzerinde karşıdan [MMA](https://go.microsoft.com/fwlink/?LinkId=828603).
2. Yüklemeyi başlatmak için indirilen dosyayı çift tıklatın.
3. Kur'da, üzerinde **Hoş Geldiniz** sayfasında, **sonraki**. Üzerinde **Lisans Koşulları'nı** sayfasında, **ediyorum** lisans kabul etmek için.
4. İçinde **hedef klasörü**, saklamak veya varsayılan yükleme klasörünü değiştirmek > **sonraki**.
5. İçinde **aracı Kur Seçenekleri**seçin **Azure günlük analizi (OMS)** > **sonraki**.
6. Tıklatın **Ekle** yeni bir günlük analizi çalışma alanı eklemek için. Proje kimliği ve kopyaladığınız anahtarını yapıştırın. Ardından **İleri**'ye tıklayın.
7. Aracı projeye bağlanabildiğinizi doğrulayın. Başaramazsa ayarlarını doğrulayın. Aracı bağlanabilirsiniz ancak Toplayıcı kullanamazsanız, desteğe başvurun.


**802. hata: bir tarih ve saat eşitleme hatası alıyorum.**

Sunucu saati eşitleme olabilir geçerli saatle beş dakikadan fazla ile. Geçerli saat gibi eşleşecek şekilde VM toplayıcısında saatin değiştirin:

1. VM bir yönetici komut istemi açın.
2. Saat dilimi denetlemek için w32tm /tz çalıştırın.
3. Zaman eşitleme için w32tm/resync çalıştırın.

**Proje anahtarımı "==" simgeleri sonuna doğru sahiptir. Bu, diğer alfasayısal karakterler toplayıcısı tarafından kodlanır. Bu beklenen bir durumdur?**

Evet, her proje anahtarı sonlandırır "ile ==". Toplayıcı işlemeden önce Proje anahtarı şifreler.

**Sıfır olarak diskler ve ağ bağdaştırıcıları için performans verilerini gösterir**

VCenter sunucusu istatistikleri ayarı düzeyinde değerinden üç ayarlandıysa, bu durum oluşabilir. Düzey üç veya daha yüksek, vCenter işlem, depolama ve ağ için VM performans geçmişini saklar. Küçüktür düzeyi üç için vCenter depolama ve ağ verileri, ancak yalnızca CPU ve bellek verileri depolamaz. Bu senaryoda, performans verileri gösterildiği gibi Azure geçirmek sıfır ve Azure geçiş diskleri ve şirket içi makinelerden toplanan meta verileri temel ağlar için boyutu öneri sağlar.

Disk ve ağ performans verileri koleksiyonunu etkinleştirmek için üç istatistikleri ayarları düzeyini değiştirin. Daha sonra en az bir gün onu değerlendirmek ortamınız bulmak için bekleyin. 

**Yüklenen aracıları ve bağımlılık görselleştirme grupları oluşturmak için kullanılan bildirimi. Artık yük devretme, "Görünüm bağımlılıkları" yerine "aracı yükleme" eylemi makineler Göster sonrası**
* POST planlanmış veya planlanmamış bir yük devretme şirket içi makineler kapalıdır ve eşdeğer makineler çalışmaya Azure'da başlar. Bu makineleri farklı bir MAC adresi alın. Bunlar Sunucusu'ndan olup kullanıcı veya şirket içi IP adresini korumak seçiminize göre göre farklı bir IP adresi. MAC ve IP adreslerini farklıysa, Azure geçirmek şirket içi makineler tüm hizmet Haritası bağımlılık verilerle ilişkilendirmez ve bağımlılıklarını görüntüleme yerine aracıları yüklemek için kullanıcıya sorar.
* Test yük devretme sonrası beklendiği gibi şirket içi makineler açık kalır. Eşdeğer makineleri Azure üzerinde çalışmaya başlar, farklı bir MAC adresi edinmeli ve farklı bir IP adresi elde. Kullanıcı bu makinelere giden OMS trafiği engelleyen sürece, Azure geçirmek şirket içi makineler tüm hizmet Haritası bağımlılık verilerle ilişkilendirmez ve bağımlılıklarını görüntüleme yerine aracıları yüklemek için kullanıcıya sorar.


## <a name="troubleshoot-readiness-issues"></a>Hazır olma durumu sorunlarını giderme

**Sorunu** | **Fix**
--- | ---
Desteklenmeyen önyükleme türü | Azure VM'ler EFI Önyükleme türüyle desteklemez. Bir geçiş çalıştırmadan önce için BIOS önyükleme türüne dönüştürmek için önerilir. <br/><br/>Kullanabileceğiniz [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) , VM önyükleme türünü geçiş sırasında BIOS'a dönüştürecektir gibi böyle VM'ler geçişini yapmak için.
Disk sayısı, sınırı aşıyor | Kullanılmayan disklerin geçiş işleminden önce makine kaldırın.
Disk boyutu, sınırı aşıyor | Azure destekleyen sahip bir disk boyutu en fazla 4 TB. Disklerin geçiş işleminden önce 4 TB değerinden küçük küçültün. 
Disk, belirtilen konumda kullanılamıyor | Geçirmeden önce hedef konumda disk olduğundan emin olun.
Disk, belirtilen çoğaltma için kullanılamıyor | Disk değerlendirme ayarlar (varsayılan olarak LRS) tanımlı artıklık depolama türünü kullanmanız gerekir.
Bir iç hata nedeniyle disk uygunluğu belirlenemedi | Grubu için yeni bir değerlendirme oluşturmayı deneyin. 
Gerekli çekirdeklere ve belleğe sahip VM bulunamadı | Azure, uygun bir VM türüne ince uygulanamadı. Geçirmeden önce bellek ve şirket içi makineyi çekirdek sayısını azaltın. 
Bir veya daha fazla uygun diskler. | Yapma emin şirket içi disklerdir 4 TB veya altında önce bir geçiş çalıştırın.
Bir veya daha fazla uygun ağ bağdaştırıcıları. | Geçişten önce makineden kullanılmayan ağ bağdaştırıcılarını kaldırın.
İç hata nedeniyle VM uygunluğu belirleyemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin. 
İç hata nedeniyle bir veya daha fazla disk uygunluğuna belirleyemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin.
İç hata nedeniyle bir veya daha fazla ağ bağdaştırıcısı uygunluğuna belirleyemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin.
VM için gerekli depolama performansı bulunamadı. | Makine için gerekli depolama performansı (IOPS/üretilen iş) Azure VM destek aşıyor. Geçiş işleminden önce makine için depolama gereksinimlerini azaltır.
VM için gerekli ağ performansını bulunamadı. | Makine için gereken ağ performansı (çıkış) Azure VM destek aşıyor. Makine için ağ gereksinimlerini azaltır. 
VM belirtilen fiyatlandırma katmanında bulunamadı. | Fiyatlandırma katmanı standart olarak ayarlanırsa, Azure'a geçirmeden önce VM downsizing göz önünde bulundurun. Boyutlandırma katmanı Basic ise, değerlendirme fiyatlandırma katmanı standart olarak değiştirmeyi düşünün. 
VM belirtilen konumda bulunamadı. | Geçişten önce farklı bir hedef konum kullanın.
Bilinmeyen işletim sistemi | VM işletim sistemi 'Diğer' vCenter Server'da, hangi nedeniyle VM Azure hazırlık Azure geçirmek tanımlayamıyor belirtildi. Makinede çalışan işletim sistemi olduğundan emin olun [desteklenen](https://aka.ms/azureoslist) makineyi geçirmeden önce Azure tarafından.
Koşullu desteklenen Windows işletim sistemi | İşletim sistemi sonunu destek tarihi geçtikten ve bir özel destek sözleşmesi (CSA) için gereken [destek Azure'da](https://aka.ms/WSosstatement), Azure'a geçirmeden önce işletim sistemi yükseltme yapmayı düşünün.
Desteklenmeyen Windows işletim sistemi | Azure destekleyen yalnızca [seçili Windows işletim sistemi sürümleri](https://aka.ms/WSosstatement), Azure'a geçirmeden önce makine işletim sistemini yükseltme yapmayı düşünün. 
Linux işletim sistemi koşullu onaylanır | Azure Symantec'in yalnızca [seçili Linux işletim sistemi sürümleri](../virtual-machines/linux/endorsed-distros.md), Azure'a geçirmeden önce makine işletim sistemini yükseltme yapmayı düşünün.
Unendorsed Linux işletim sistemi | Makine Azure'da önyükleme, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanan, işletim sistemine yükseltin bir [Linux sürüm destekli](../virtual-machines/linux/endorsed-distros.md) Azure'a geçirmeden önce
İşletim sistemi bit genişliği desteklenmiyor | 32-bit işletim sistemi ile sanal makineleri Azure'da önyükleme, ancak 32-bit VM işletim sistemini yükseltmek için önerilen için 64-bit Azure'a geçirmeden önce.
Visual Studio aboneliği gerektirir. | Makine içeren bir Windows istemci işletim sistemi içinde çalıştığı yalnızca Visual Studio abonelikte desteklenmiyor.


## <a name="collect-logs"></a>Günlüklerini toplayın

**VM toplayıcısında nasıl günlükleri toplamak?**

Günlüğe kaydetme varsayılan olarak etkindir. Günlükleri aşağıdaki gibi bulunur:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Windows için olay izleme toplamak için aşağıdakileri yapın:

1. VM toplayıcısı üzerinde bir PowerShell komut penceresi açın.
2. Çalıştırma **Get-EventLog - günlükadı uygulama | export-csv eventlog.csv**.

**Portal ağ trafiği günlüklerini nasıl topluyor mu?**

1. Tarayıcıyı açın ve gidin ve oturum açma [portalı](https://portal.azure.com).
2. Geliştirici Araçları başlatmak için F12 tuşuna basın. Gerekirse, ayarı temizlemek **temizleyin Gezinti girişlerde**.
3. Tıklatın **ağ** sekmesini tıklatın ve ağ trafiğini yakalama Başlat:
 - Chrome'seçin **Koru günlük**. Kayıt otomatik olarak başlamanız gerekir. Kırmızı bir daire trafiği yakalama yapılıyor gösterir. Görünmüyorsa, siyah bir daire başlatmak için tıklatın
 - Edge/IE'de kaydı otomatik olarak başlamanız gerekir. Seçili değilse, yeşil Oynat düğmesini tıklatın.
4. Hata yeniden oluşturmayı deneyin.
5. Kaydedilirken hatayla sonra durdurmanıza ve kaydedilen etkinliği bir kopyasını kaydedin:
 - Chrome'sağ tıklatın ve **içerikle HAR Kaydet**. Bu zips ve günlükleri .har dosyası olarak dışarı aktarır.
 - Edge/IE'de tıklatın **verme yakalanan trafiği** simgesi. Bu zips ve günlük dışa aktarır.
6. Gidin **konsol** sekmesi tüm uyarılar veya hatalar için denetleyin. Konsol günlüğe kaydetmek için:
 - Chrome'konsol günlüğünde herhangi bir yere sağ tıklayın. Seçin **Farklı Kaydet**, aktarın ve günlük zip.
 - Edge/IE'de sağ tıklatın ve hataları **tüm kopyalayın**. 
7. Geliştirici Araçları'nı kapatın.
 

## <a name="vcenter-errors"></a>vCenter hataları

### <a name="error-unhandledexception-internal-error-occured-systemiofilenotfoundexception"></a>Hata UnhandledException iç hata oluştu: System.IO.FileNotFoundException

Bu, Toplayıcı sürümleri 1.0.9.5'den görülen bir sorundur. Toplayıcı sürüm 1.0.9.2 veya 1.0.8.59 gibi pre-GA sürümleri kullanıyorsanız, bu sorunu karşı karşıya kalırsınız. İzleyin [bağlamak için ayrıntılı bir yanıt forumlarında burada verilen](https://social.msdn.microsoft.com/Forums/azure/en-US/c1f59456-7ba1-45e7-9d96-bae18112fb52/azure-migrate-connect-to-vcenter-server-error?forum=AzureMigrate).

[Sorunu düzeltmek için toplayıcınız yükseltme](https://aka.ms/migrate/col/checkforupdates).

### <a name="error-unabletoconnecttoserver"></a>Error UnableToConnectToServer

VCenter Server "Servername.com:9443" hatası nedeniyle bağlantı kurulamadı: https://Servername.com:9443/iletiyi kabul edebilecek sdk dinleme bitiş noktası vardı.

Makine belirtilen vCenter sunucusu adı veya bağlantı noktası speficified çözümleyemiyor Toplayıcı yanlış olduğunda meydana gelir. Bağlantı noktası belirtilmezse, varsayılan olarak 443 numaralı bağlantı noktası numarası bağlanmak Toplayıcı dener.

1. Toplayıcı makineden sunucuadı.com ping işlemi yapmayı deneyin.
2. 1. adım başarısız olursa, IP adresi vCenter sunucusuna bağlanmak deneyin.
3. VCenter bağlanmak için doğru bağlantı noktası numarasını belirleyin.
4. Son olarak vCenter sunucunun hazır ve çalışır durumda olup olmadığını denetleyin.
 

