---
title: "Azure geçiş sorunlarını giderme | Microsoft Docs"
description: "Azure geçirmek hizmet ve sorun giderme ipuçları için sık karşılaşılan bilinen sorunlar genel bir bakış sağlar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: troubleshooting
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 1fcc9e12e63eda73d53ae2085bc2a64d31ea2067
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-azure-migrate"></a>Azure Geçişi sorunlarını giderme

## <a name="troubleshoot-common-errors"></a>Sık karşılaşılan sorunları giderme

[Azure geçirme](migrate-overview.md) geçiş Azure için şirket içi iş yüklerini değerlendirir. Dağıtma ve Azure geçişi kullanırken sorunları gidermek için bu makaleyi kullanın.


**Toplayıcı internet'e bağlanabiliyor değil**

Bu durum, kullanmakta olduğunuz makine bir proxy'nin arkasında olduğunda ortaya çıkar. Proxy gerekiyorsa yetkilendirme kimlik bilgilerini sağladığınızdan emin olun.
Herhangi bir URL tabanlı bir güvenlik duvarı proxy kullanıyorsanız giden bağlantıyı denetlemek için bu URL'leri gerekli beyaz liste ile emin olun:

**URL** | **Amacı**  
--- | ---
*. portal.azure.com | Azure hizmetiyle bağlantısını denetleyin ve zaman eşitlemesini doğrulamak için gerekli verir.
*. oneget.org | Gerekli powershell indirmek için vCenter Powerclı modülü temel.

**Toplayıcı Proje kimliği kullanarak proje bağlanamaz ve anahtar ı portaldan kopyalanır.**

Kopyalanır ve doğru bilgileri yapıştırılan emin olun. Sorunu gidermek için Microsoft İzleme Aracısı'nı (MMA) gibi yükleyin:

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

**Sorunu** | **Düzeltme**
--- | ---
Önyükleme türü desteklenmiyor | Bir geçiş çalıştırmadan önce BIOS'a değiştirin.
Disk sayısı sınırını aşıyor | Kullanılmayan disklerin geçiş işleminden önce makine kaldırın.
Disk boyutu sınırını aşıyor | Disklerin geçiş işleminden önce 4 TB değerinden küçük küçültün. 
Belirtilen konumda kullanılabilir disk | Geçirmeden önce hedef konumda disk olduğundan emin olun.
Disk için belirtilen artıklık kullanılamıyor | Disk değerlendirme ayarlar (varsayılan olarak LRS) tanımlı artıklık depolama türünü kullanmanız gerekir.
İç hata nedeniyle disk uygunluğu belirlenemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin. 
VM gerekli çekirdek ve bellek bulunamadı | Azure, uygun bir VM türüne ince uygulanamadı. Geçirmeden önce bellek ve şirket içi makineyi çekirdek sayısını azaltın. 
Bir veya daha fazla uygun diskler. | Yapma emin şirket içi disklerdir 4 TB veya altında önce bir geçiş çalıştırın.
Bir veya daha fazla uygun ağ bağdaştırıcıları. | Geçişten önce makineden kullanılmayan ağ bağdaştırıcılarını kaldırın.
İç hata nedeniyle VM uygunluğu belirleyemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin. 
İç hata nedeniyle bir veya daha fazla disk uygunluğuna belirleyemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin.
İç hata nedeniyle bir veya daha fazla ağ bağdaştırıcısı uygunluğuna belirleyemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin.
VM için gerekli depolama performansı bulunamadı. | Makine için gerekli depolama performansı (IOPS/üretilen iş) Azure VM destek aşıyor. Geçiş işleminden önce makine için depolama gereksinimlerini azaltır.
VM için gerekli ağ performansını bulunamadı. | Makine için gereken ağ performansı (çıkış) Azure VM destek aşıyor. Makine için ağ gereksinimlerini azaltır. 
VM için belirtilen fiyatlandırma katmanı bulunamadı. | Fiyatlandırma katmanı ayarlarını kontrol edin. 
VM belirtilen konumda bulunamadı. | Geçişten önce farklı bir hedef konum kullanın.
Linux işletim sistemi sorunları desteği | 64 bit çalışan desteklenen bunlarla emin olun [işletim sistemleri](../virtual-machines/linux/endorsed-distros.md).
Windows işletim sistemi sorunları desteği | Desteklenen bir işletim sistemi çalıştırdığınızdan emin olun. [Daha fazla bilgi](concepts-assessment-calculation.md#azure-suitability-analysis)
Bilinmeyen işletim sistemi. | VCenter belirtilen işletim sistemi doğru olup olmadığını denetleyin ve bulma işlemi yineleyin.
Visual Studio aboneliği gerektirir. | Windows istemci işletim sistemleri, yalnızca Visual Studio (MSDN) Aboneliklerde desteklenir.


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
 



