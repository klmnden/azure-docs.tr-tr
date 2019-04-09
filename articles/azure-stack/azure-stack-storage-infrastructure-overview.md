---
title: Azure Stack için depolama altyapısını yönetme | Microsoft Docs
description: Azure Stack için depolama altyapısını yönetme.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: ''
ms.date: 03/11/2019
ms.author: mabrigg
ms.lastreviewed: 03/11/2019
ms.reviewer: jiahan
ms.openlocfilehash: 416d75b254d0fbe14a0b39e5ae77d09a48e548f6
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59271297"
---
# <a name="manage-storage-infrastructure-for-azure-stack"></a>Azure Stack için depolama altyapısını yönetme

*Şunlara uygulanır Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack depolama altyapı kaynaklarını işletimsel durumunu ve sistem durumu açıklar. Bu kaynaklar, depolama sürücülerini ve birimleri içerir. Bu konu başlığı altındaki bilgiler, bir sürücü için bir havuz eklenemez gibi çeşitli sorunlarını girişimi sırasında olmada çok yararlı olabilir.

## <a name="understand-drives-and-volumes"></a>Sürücüleri ve birimler anlama

### <a name="drives"></a>Sürücüleri

Windows Server yazılımı tarafından desteklenen, Azure Stack depolama özellikleri depolama alanları doğrudan (S2D) ve bir yüksek performanslı, ölçeklenebilir ve sağlamak için Windows Server Yük Devretme Kümelemesi ve dayanıklı depolama hizmeti ile tanımlar.

Azure Stack tümleşik sistem iş ortakları, çok çeşitli depolama esnekliği de dahil olmak üzere çok sayıda çözüm çeşitlemeleri sunar. Şu anda üç sürücü türleri bir birleşimini seçebilirsiniz: NVMe (geçici olmayan belleği Express), SATA/SSD (katı hal sürücüsü)'ı SAS HDD (Sabit Disk sürücüsü).

Depolama alanları doğrudan, depolama performansını en üst düzeye çıkarmak için bir önbellek sunar. Tek veya birden çok sürücü türleri ile Azure Stack gereç depolama alanları doğrudan otomatik olarak kullanan tüm sürücülerin "Hızlı" (NVMe &gt; SSD &gt; HDD) önbelleğe alma türü. Kalan sürücüler kapasite için kullanılır. Sürücüleri "tamamen flash" veya "karma" dağıtım gruplandırılır:

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image1.png)

Dağıtımlar, depolama performansını en üst düzeye çıkarmak amacıyla tamamen flash ve döngüsel sabit disk sürücülerinin (HDD) içermez.

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image2.png)

Karma dağıtımlar, performans ve kapasite dengelemek için veya kapasiteyi en üst düzeye çıkarmak için hedefleyin ve döngüsel sabit disk sürücülerinin (HDD) içerir.

Önbellek davranışı için önbelleğe sürücüleri türlerini göre otomatik olarak belirlenir. Yalnızca yazma işlemleri için katı hal sürücüleri (NVMe SSD için önbelleğe alma gibi) önbelleğe alırken önbelleğe alınır. Bu kapasite sürücülerindeki toplam trafik için kapasite sürücülerini azaltarak ve kendi ömürlerine genişletme wear azaltır. Bu arada, okuma flash ömrü önemli ölçüde etkilemez çünkü ve katı hal sürücüleri Evrensel okuma gecikme süresi düşük sunduklarından okuma önbelleğe alınmaz. Sabit disk sürücülerini (HDD'ler için önbelleğe alma SSD gibi), hem okuma ve yazma önbelleğe alınır, flash gibi gecikme süresi sağlamak için önbelleğe alırken (genellikle yaklaşık 10 daha iyi x) hem de.

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image3.png)

Kullanılabilir yapılandırma depolama için Azure Stack OEM iş ortağı denetleyebilirsiniz (https://azure.microsoft.com/overview/azure-stack/partners/) ayrıntılı belirtimi için.

> [!Note]  
> Azure Stack Gereci hem HDD ve SSD (veya NVMe) sürücüsü ile karma bir dağıtımda teslim edilebilir. Ancak daha hızlı türü sürücüleri önbellek sürücüleri olarak kullanılır ve tüm kalan sürücüler kapasite sürücülerini bir havuz olarak olarak kullanılacaktır. Kiracı verilerini (BLOB'lar, tablolar, kuyruklar ve diskler) kapasite sürücülerde yerleştirilir. Bu nedenle premium diskler sağlama veya premium depolama hesabı türü nesneleri gelmez seçerek garanti edilir SSD üzerinde ayrılacak veya NVMe sürücüler ve daha yüksek performans elde edin.

### <a name="volumes"></a>Birimler

*Depolama hizmeti* kullanılabilir depolama sistemi ve Kiracı verileri tutmak için ayrılan ayrı birime bölümler. Sürücüleri hata toleransı, ölçeklenebilirlik ve performans avantajlarının depolama alanları doğrudan'ı tanıtmak için depolama havuzundaki birimler birleştirin.

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image4.png)

Azure Stack depolama havuzunda oluşturulan birimlerin üç tür vardır:

-   Altyapı: Ana bilgisayar dosyaları Azure Stack altyapısı Vm'leri ve Çekirdek Hizmetleri tarafından kullanılan.

-   VM Temp: konak geçici diskler Kiracı Vm'lere bağlı ve bu verileri bu disklerin depolanır.

-   Nesne Store: konak Kiracı verilerini bloblar, tablolar, kuyruklar ve VM disklerini hizmet.

VM Temp ve nesne Store birimler sayısı Azure Stack dağıtım eşit sayıda düğüme olmakla birlikte, bir çok düğümlü dağıtımda üç altyapı birimleri görür:

-   Bir dört düğümlü dağıtım dört eşit VM Temp ve dört eşit nesne Store birimler vardır.

-   Kümeye yeni bir düğüm eklerseniz, her iki tür oluşturulması için yeni bir birim olacaktır.

-   Birim sayısı, hatalı düğüm bile kalır veya kaldırılır.

-   Azure Stack geliştirme Seti'ni kullanırsanız, birden çok paylaşımları ile tek bir birim yok.

Birimleri depolama alanları doğrudan sürücü veya sunucu hataları gibi donanım sorunlarına karşı korumak ve yazılım güncelleştirmeleri gibi sunucu bakım boyunca sürekli kullanılabilirliği Etkinleştir için dayanıklılık sağlar. Azure Stack dağıtım veri esnekliği sağlamak için üç yönlü yansıtma kullanıyor. Kiracı verilerin üç kopyasını nerede bunlar önbellekte kavuşmak, farklı sunucular yazılır:

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image5.png)

Yansıtma, tüm verilerin birden çok kopyasını tutarak, hataya dayanıklılık sağlar. Bu verilerin nasıl şeritli ve yerleştirilmiş önemsiz değil (daha fazla bilgi için bu blog bakın), ancak kesinlikle yansıtma kullanarak depolanan tüm veriler yazılır, sunabilen, say true birden çok kez. Her kopya için farklı fiziksel olarak yazılır (farklı sunucular farklı sürücülere) donanım başarısız bağımsız olarak kabul edilir. Üç yönlü yansıtma güvenli bir şekilde en az iki donanım sorunları (sürücü veya sunucu) aynı anda dayanabilir. Örneğin, bir sunucu zaman aniden yeniden başlatılıyor başka bir sürücüye veya sunucu başarısız olur, tüm veriler kalırken güvenli ve sürekli olarak erişilebilir.

## <a name="volume-states"></a>Birim durumları

Hangi durumu volumess içinde olduğunu bulmak için aşağıdaki PowerShell komutlarını kullanın:

\$scaleunit\_adı (Get-AzsScaleUnit) =\[0\].name

\$alt\_adı = (Get-AzsStorageSubSystem - ScaleUnit \$scaleunit\_adı)\[0\].name

Get-AzsVolume - ScaleUnit \$scaleunit\_- StorageSubSystem ad \$alt\_adı | Select-Object VolumeLabel, HealthStatus, OperationalStatus, RepairStatus, açıklama, eylem, TotalCapacityGB, RemainingCapacityGB

Ayrılmış birim ve düşürülmüş/eksik bir birim gösteren çıktının bir örneği aşağıda verilmiştir:

| VolumeLabel | HealthStatus | OperationalStatus |
|-------------|--------------|------------------------|
| ObjStore_1 | Bilinmeyen | Ayrılmış |
| ObjStore_2 | Uyarı | {Düşürülmüş, eksik} |

Aşağıdaki bölümler, sistem durumu ve işletimsel durumlarını listeler.

### <a name="volume-health-state-healthy"></a>Birim durumu: Sorunsuz

| İşlemsel durum | Açıklama |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tamam | Birim, iyi durumda. |
| Yetersiz | Verileri eşit olarak sürücülerde yazılan değil.<br> <br>**Eylem:** Lütfen depolama havuzunda sürücü kullanımını iyileştirmek için desteğe başvurun. Kılavuzu kullanarak günlük dosya toplama işlemi başlamadan önce https://aka.ms/azurestacklogfiles. Başarısız bağlantı geri yüklendikten sonra yedekten geri yüklemeniz gerekebilir. |


### <a name="volume-health-state-warning"></a>Birim durumu: Uyarı

Birimi bir uyarı sistem durumunda olduğunda, bir veya daha fazla kopyasını kullanılamıyor, ancak Azure Stack, verilerinizin en az bir kopyasını hala okuyabilirsiniz anlamına gelir.

| İşlemsel durum | Açıklama |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hizmette | Azure Stack birim gibi ekleme veya bir sürücüyü kaldırma sonrasında onarıyor. Onarım işlemi tamamlandığında, birim için Tamam durumu döndürmelidir.<br> <br>**Eylem:** Birim onarma tamamlamak Azure Stack için bekleyin ve daha sonra durumu denetleyin. |
| Eksik | Bir veya daha fazla sürücü başarısız oldu veya eksik olduğundan birimin esnekliği azaltılır. Bununla birlikte, eksik sürücüleri verilerinizi güncel kopyalarını içerir.<br> <br>**Eylem:** Eksik sürücüleri yeniden, başarısız olan herhangi bir sürücü değiştirin ve çevrimdışı sunucuları çevrimiçi. |
| Düşürüldü | Birimin esnekliği, çünkü bir veya daha fazla sürücü başarısız oldu veya eksik olabilir ve bu sürücüde verilerinizin güncel kopyasını vardır azaltılır.<br> <br>**Eylem:** Eksik sürücüleri yeniden, başarısız olan herhangi bir sürücü değiştirin ve çevrimdışı sunucuları çevrimiçi. |

 

### <a name="volume-health-state-unhealthy"></a>Birim durumu: İyi durumda değil

Bir birim bir kötü sistem durumu durumda olduğunda, birimdeki verilerin tümünün veya şu anda erişilemiyor.

| İşlemsel durum | Açıklama |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Artıklık | Çok fazla sürücüleri başarısız olduğundan veri birimi kaybetti.<br> <br>**Eylem:** Lütfen desteğe başvurun. Kılavuzu kullanarak günlük dosya toplama işlemi başlamadan önce https://aka.ms/azurestacklogfiles. |


### <a name="volume-health-state-unknown"></a>Birim durumu: Bilinmeyen

Birim içinde bilinmeyen durumu sanal disk ayrılmış duruma da olabilir.

| İşlemsel durum | Açıklama |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ayrılmış | Depolama aygıtı hata oluştu. erişilemez olmasına neden olabilir. Bazı veriler kaybolabilir.<br> <br>**Eylem:** <br>1. Fiziksel denetleyin ve ağ bağlantısını tüm depolama cihazı.<br>2. Tüm cihazları doğru bir şekilde bağlantınız varsa, lütfen desteğe başvurun. Kılavuzu kullanarak günlük dosya toplama işlemi başlamadan önce https://aka.ms/azurestacklogfiles. Başarısız bağlantı geri yüklendikten sonra yedekten geri yüklemeniz gerekebilir. |

## <a name="drive-states"></a>Sürücü durumları

Sürücü durumunu izlemek için aşağıdaki PowerShell komutlarını kullanın:

 

\$scaleunit\_adı (Get-AzsScaleUnit) =\[0\].name

\$alt\_adı = (Get-AzsStorageSubSystem - ScaleUnit \$scaleunit\_adı)\[0\].name

, Seri numarası

 

Get-AzsDrive - ScaleUnit \$scaleunit\_- StorageSubSystem ad \$alt\_adı | Select-Object StorageNode, PhysicalLocation, HealthStatus, OperationalStatus, açıklama, eylem, kullanım, CanPool, CannotPoolReason, seri numarası, Model, MediaType, CapacityGB

Aşağıdaki bölümlerde, bir sürücü kullanılabilir sistem durumları açıklanmaktadır.

### <a name="drive-health-state-healthy"></a>Sürücü durumu: Sorunsuz

| İşlemsel durum | Açıklama |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Tamam | Birim, iyi durumda. |
| Hizmette | Sürücü bazı iç kayıt tutma işlemlerini gerçekleştiriyor. İşlem tamamlandığında, sürücü Tamam durumu döndürmelidir. |

### <a name="drive-health-state-healthy"></a>Sürücü durumu: Sorunsuz

Uyarı durumu can sürücüde okuyun ve başarıyla veri yazma ancak bir sorun vardır.

| İşlemsel durum | Açıklama |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İletişim kaybedildi | Sürücüye bağlantısı kesildi.<br> <br>**Eylem:** Tüm sunucuları tekrar çevrimiçi duruma getirin. Bu sorunu çözmüyorsa, sürücünün yeniden bağlanın. Bu gerçekleştiği tutar, tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Öngörülen hata | Sürücünün hata yakında tahmin edildiğinde.<br> <br>**Eylem:** Mümkün olan en kısa sürede tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| G/ç hatası | Sürücü erişirken geçici bir hata oluştu.<br> <br>**Eylem:** Bu gerçekleştiği tutar, tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Geçici hata | Sürücüyü geçici bir hata oluştu. Bu genellikle sürücünün yanıt vermiyor, ancak bunu Ayrıca, depolama alanları koruyucu bölümü sürücüden uygunsuz bir şekilde kaldırıldı doğrudan gelebilir anlamına gelir. <br> <br>**Eylem:** Bu gerçekleştiği tutar, tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Olağan dışı bir gecikme süresi | Sürücü bazen yanıt vermiyor ve hatanın belirtileri gösteriliyor.<br> <br>**Eylem:** Bu gerçekleştiği tutar, tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Havuzdan kaldırılıyor | Azure Stack, sürücü, depolama havuzundan kaldırma işleminde ' dir.<br> <br>**Eylem:** Sürücü kaldırılmasını tamamlamak Azure Stack için bekleyin ve daha sonra denetleyin.<br>Bu durum devam ederse, lütfen desteğe başvurun. Kılavuzu kullanarak günlük dosya toplama işlemi başlamadan önce https://aka.ms/azurestacklogfiles. |
|  |  |
| Bakım modu başlangıç | Sürücüye bakım moduna almadan sürecinde Azure yığınıdır. Bu geçici bir durumdur - sürücü yakında In Bakım modu durumunda olması gerekir.<br> <br>**Eylem:** İşlemi tamamlamak Azure Stack için bekleyin ve daha sonra denetleyin. |
| Bakım modunda | Sürücü okuma durdurma bakım modunda ve sürücüye yazar. Bu, genellikle sürücünün PNU veya FRU çalışıyor gibi Azure Stack yönetim görevleri anlamına gelir. Ancak yönetici aynı zamanda bakım modunda sürücü yerleştirebilirsiniz.<br> <br>**Eylem:** Yönetim görevi tamamlamak Azure Stack için bekleyin ve daha sonra denetleyin.<br>Bu durum devam ederse, lütfen desteğe başvurun. Kılavuzu kullanarak günlük dosya toplama işlemi başlamadan önce https://aka.ms/azurestacklogfiles. |
|  |  |
| Bakım modunu durdurma | Azure Stack, sürücü tekrar çevrimiçi duruma getirmeden sürecinde ' dir. Bu geçici bir durumdur - sürücü yakında başka bir durumda - ideal olarak iyi durumda olması gerekir.<br> <br>**Eylem:** İşlemi tamamlamak Azure Stack için bekleyin ve daha sonra denetleyin. |

 

### <a name="drive-health-state-unhealthy"></a>Sürücü durumu: İyi durumda değil

Kötü durumda bir sürücü şu anda yazılan veya olamaz erişilebilir.

| İşlemsel durum | Açıklama |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Böl | Sürücü havuzdan ayrılmış haline gelir.<br> <br>**Eylem:** Sürücü yeni bir disk ile değiştirin. Bu disk kullanmanız gerekiyorsa, disk sistemden kaldırmak, hiç yararlı veri diskte olduğundan emin olun, disk silme ve ardından diski yeniden takın. |
| Kullanılamaz | Fiziksel disk çözüm satıcınız tarafından desteklenmediği için karantinaya alındı. Çözüm için onaylanmış ve doğru bir disk üretici yazılımı olan diskleri desteklenir.<br> <br>**Eylem:** Onaylanan üretici ve model numarası çözümü içeren bir disk sürücüsüne değiştirin. |
| Eski meta veriler | Yeni disk, önceden kullanılmış ve bilinmeyen depolama sistemi verileri içerebilir. Disk karantinaya alındı.        <br> <br>**Eylem:** Sürücü yeni bir disk ile değiştirin. Bu disk kullanmanız gerekiyorsa, disk sistemden kaldırmak, hiç yararlı veri diskte olduğundan emin olun, disk silme ve ardından diski yeniden takın. |
| Tanınmayan meta veriler | Tanınmayan meta veriler genellikle sürücü üzerindeki farklı bir havuzdan meta verileri olduğu anlamına gelir sürücüdeki bulunamadı.<br> <br>**Eylem:** Sürücü yeni bir disk ile değiştirin. Bu disk kullanmanız gerekiyorsa, disk sistemden kaldırmak, hiç yararlı veri diskte olduğundan emin olun, disk silme ve ardından diski yeniden takın. |
| Başarısız ortam | Sürücü başarısız oldu ve depolama alanları tarafından artık kullanılmayacak.<br> <br>**Eylem:** Mümkün olan en kısa sürede tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Cihaz donanım hatası | Bu sürücüde bir donanım hatası vardı. <br> <br>**Eylem:** Mümkün olan en kısa sürede tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Üretici yazılımı güncelleştiriliyor | Azure Stack, sürücü bellenimini güncelleştirme işlemidir. Bu pencere, genellikle bir dakikadan kısa sürer ve hangi sırasında havuzdaki diğer sürücülerin kodlayıp zaman okuyan ve yazan geçici bir durumdur.<br> <br>**Eylem:** Güncelleştirmeyi tamamlamak Azure Stack için bekleyin ve daha sonra denetleyin. |
| Başlatılıyor | Sürücü işlemi için hazırlanıyor. Bu geçici bir durum - tamamlandıktan sonra sürücü farklı işletimsel durumuna geçiş olmalıdır.<br> <br>**Eylem:** İşlemi tamamlamak Azure Stack için bekleyin ve daha sonra denetleyin. |
 

## <a name="reasons-a-drive-cant-be-pooled"></a>Bir sürücü havuza nedenleri

Bazı sürücüler yalnızca Azure Stack depolama havuzunda olmasını hazır değil. Neden bir sürücü bir sürücünün CannotPoolReason özelliğine bakılarak havuz için uygun değil kullanıma bulabilirsiniz. Aşağıdaki tabloda her biri nedeniyle biraz daha ayrıntılı bilgi sağlar.

| Neden | Açıklama |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Donanım uyumlu değil | Sistem sağlığı hizmeti kullanılarak belirtilen onaylanan depolama modelleri listesinde sürücü değil.<br> <br>**Eylem:** Sürücü yeni bir disk ile değiştirin. |
| Üretici yazılımı uyumlu değil | Fiziksel sürücü bellenimini, sistem sağlığı hizmeti kullanarak onaylı üretici yazılımı sürümlerini listesinde değil.<br> <br>**Eylem:** Sürücü yeni bir disk ile değiştirin. |
| Küme tarafından kullanımda | Sürücü, şu anda bir yük devretme kümesi tarafından kullanılır.<br> <br>**Eylem:** Sürücü yeni bir disk ile değiştirin. |
| Çıkarılabilir medya | Sürücünün bir çıkarılabilir sürücü olarak sınıflandırılır. <br> <br>**Eylem:** Sürücü yeni bir disk ile değiştirin. |
| Sağlıklı durumda değil | Sürücü, iyi durumda olmayan ve değiştirilmesi gerekebilir.<br> <br>**Eylem:** Sürücü yeni bir disk ile değiştirin. |
| Yeterli kapasite | Sürücüdeki boş alanı alma bölümler vardır.<br> <br>**Eylem:** Sürücü yeni bir disk ile değiştirin. Bu disk kullanmanız gerekiyorsa, disk sistemden kaldırmak, hiç yararlı veri diskte olduğundan emin olun, disk silme ve ardından diski yeniden takın. |
| Doğrulama devam ediyor | Sistem sağlığı hizmeti kullanmak için sürücü veya sürücü bellenimini onaylanırsa görmek için denetliyor.<br> <br>**Eylem:** İşlemi tamamlamak Azure Stack için bekleyin ve daha sonra denetleyin. |
| Doğrulama başarısız oldu | Sistem sağlığı hizmeti kullanmak için sürücü veya sürücü bellenimini onaylanırsa görmeye denetlenemedi.<br> <br>**Eylem:** Lütfen desteğe başvurun. Kılavuzu kullanarak günlük dosya toplama işlemi başlamadan önce https://aka.ms/azurestacklogfiles. |
| Çevrimdışı | Çevrimdışı sürücüdür. <br> <br>**Eylem:** Lütfen desteğe başvurun. Kılavuzu kullanarak günlük dosya toplama işlemi başlamadan önce https://aka.ms/azurestacklogfiles. |

## <a name="next-step"></a>Sonraki adım

[Depolama kapasitesi yönetme](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-storage-shares) 