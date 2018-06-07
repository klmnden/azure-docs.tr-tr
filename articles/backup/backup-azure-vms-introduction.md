---
title: Azure'da VM yedekleme altyapınızı planlama
description: Azure sanal makineleri yedeklemek planlama yaparken önemli noktalar
services: backup
author: markgalioto
manager: carmonm
keywords: sanal makineleri yedekleme, sanal makineleri yedekleme
ms.service: backup
ms.topic: conceptual
ms.date: 3/23/2018
ms.author: markgal
ms.openlocfilehash: 92122e7dc62e0f402bcddff099984e6e2c605fae
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606095"
---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>Azure’da sanal makine yedekleme altyapınızı planlama
Bu makalede, performansı ve VM yedekleme altyapınızı planlamanıza yardımcı olması için kaynak önerileri sağlar. Ayrıca, Backup hizmeti önemli yönlerini tanımlar; Bu yönlerinin, mimarisi belirlemede önemli kapasite planlaması ve zamanlama. Seçtiğiniz varsa [ortamınızı hazırlanmış](backup-azure-arm-vms-prepare.md), planlama sonraki adım başlamadan önce [Vm'leri yedekleme için](backup-azure-arm-vms.md). Azure sanal makineler hakkında daha fazla bilgiye ihtiyacınız varsa bkz [Virtual Machines belgeleri](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="how-does-azure-back-up-virtual-machines"></a>Azure nasıl mu sanal makineleri yedekleyin?
Ne zaman bir yedekleme işi Azure Backup hizmeti zaman içinde nokta anlık almak için yedekleme uzantısını hizmet Tetikleyicileri zamanlanan saatte başlatır. Azure Backup hizmeti kullandığı _VMSnapshot_ pencerelerinde, uzantı ve _VMSnapshotLinux_ Linux uzantı. Uzantısı ilk VM yedekleme sırasında yüklenir. Uzantıyı yüklemek için VM çalıştırması gerekir. VM çalışmıyorsa Backup hizmeti, temel alınan depolamanın anlık görüntüsünü alır (VM durduğunda herhangi bir uygulama yazma işlemi gerçekleşmediği için).

Windows VM'lerinin anlık görüntüsü alınırken, Backup hizmeti Birim Gölge Kopyası Hizmeti (VSS) ile eşgüdümlü çalışarak sanal makine disklerinin tutarlı bir anlık görüntüsünü elde eder. Linux VM'ler yedekliyorsanız, bir VM anlık görüntü duruma getirirken tutarlılığını sağlamak için kendi özel komut dosyaları yazabilirsiniz. Bu komut dosyalarını Çağırma ile ilgili ayrıntılar bu makalenin sonraki bölümlerinde sağlanır.

Azure Backup hizmeti anlık görüntüyü aldıktan sonra veriler kasaya aktarılır. Verimliliği maksimuma çıkarmak için hizmet yalnızca bir önceki yedeklemeden itibaren değişmiş olan veri bloklarının aktarımını yapar.

![Azure sanal makine yedekleme mimarisi](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.

> [!NOTE]
> 1. Azure yedekleme yedekleme işlemi sırasında sanal makineye bağlı geçici disk içermez. Daha fazla bilgi için blog bakın [geçici depolama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
> 2. Depolama düzeyi anlık görüntü ve kasa için bu anlık görüntüyü aktarır azure yedekleme alır değiştirmeyin depolama hesabı anahtarlarını yedekleme işi tamamlanana kadar.
> 3. Premium VM'ler için Azure yedekleme anlık görüntü depolama hesabınıza kopyalar. Bu yedekleme hizmetinin yeterli IOPS verileri aktarmak için kasa için kullandığı emin olmaktır. Bu ek kopyasını depolama boyutu ayrılmış VM başına ücret kesilir. 
>

### <a name="data-consistency"></a>Veri tutarlılığı
Yedekleme ve geri yükleme iş açısından kritik verileri veri üreten uygulamaları sırasında yedeklenmesi olarak kritik verileri karmaşık iş çalışıyor. Bu sorunu çözmek için Azure Backup uygulamayla tutarlı yedeklemeler Windows ve Linux VM'ler için destekler
#### <a name="windows-vm"></a>Windows VM
Azure yedekleme Windows Vm'lerinde VSS tam yedeklemeler gerçekleştirir (daha fazla bilgi edinin [VSS tam yedekleme](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). VSS kopya yedeklemelerinin etkinleştirmek için aşağıdaki kayıt defteri anahtarını VM ayarlanması gerekir.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```

#### <a name="linux-vms"></a>Linux VM'leri
Azure Backup komut dosyası bir çerçeve sağlar. Linux Vm'leri yedekleme olduğunda uygulama tutarlılığı sağlamak için özel öncesi ve yedekleme iş akışı ve ortam denetimi sonrası komut dosyaları oluşturun. Azure yedekleme öncesi betik VM anlık görüntüsü çıkarmadan önce çağırır ve VM anlık görüntü işi tamamlandıktan sonra sonrası komut dosyasını çağırır. Daha fazla ayrıntı için bkz: [öncesi betiği ve sonrası betik kullanarak uygulama tutarlı VM yedeklemeleri](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).
> [!NOTE]
> Azure yedekleme yalnızca çağırır müşteri tarafından yazılan öncesi ve sonrası komut dosyaları. Öncesi betiği ve sonrası betikleri başarıyla çalıştırırsanız, Azure yedekleme kurtarma noktası uygulama tutarlı işaretler. Ancak, müşterinin özel komut dosyaları için uygulama tutarlılığı sorumlu kullanıldığında.
>


Bu tablo tutarlılık türlerini açıklar ve Azure VM sırasında altında ortaya koşullar yedekleme ve geri yükleme yordamlarını.

| Tutarlılık | VSS tabanlı | Açıklama ve ayrıntıları |
| --- | --- | --- |
| Uygulama tutarlılığı |Windows için Evet|Uygulama tutarlılığı, sağlar gibi iş yükleri için idealdir:<ol><li> VM *ön*. <li>Yoktur *bozulma*. <li>Yoktur *veri kaybı*.<li> Verileri zamanında uygulama VSS veya ön/son betik kullanarak yedekleme--içeren tarafından veri kullanan uygulama tutarlıdır.</ol> <li>*Windows sanal makineleri*-en Microsoft iş yükleri için veri tutarlılığı ilgili iş yüküne özgü eylemleri gerçekleştirebilirsiniz VSS yazıcılarının vardır. Örneğin, Microsoft SQL Server işlem günlüğü dosyasını ve veritabanına yazma doğru şekilde yapıldığını sağlar bir VSS yazıcısı olduğu. Azure Windows VM yedeklemeler için bir uygulama tutarlı bir kurtarma noktası oluşturmak için backup uzantısı gerekir VSS iş akışının çağırmak ve VM anlık görüntüsü gerçekleştirilmesi için tamamlanması. Azure VM anlık doğru olması tüm Azure VM uygulamaların VSS yazıcılarının da tamamlamanız gerekir. (Öğrenin [VSS Temelleri](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) ve derin ayrıntılarını daha yakından inceleyin [nasıl çalıştığını](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx)). </li> <li> *Linux VM'ler*-müşteriler yürütebilir [özel öncesi betiği ve uygulama tutarlılığı sağlamak için sonrası komut dosyası](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). </li> |
| Dosya sistemi tutarlılığı |Evet - Windows tabanlı bilgisayarlar için |Kurtarma noktası olabilir burada iki senaryo vardır *dosya sistemi tutarlı*:<ul><li>Linux VM'ler için Azure'da, yedeklerini öncesi-script/sonrası-script veya öncesi-script/sonrası-script başarısız olduysa olmadan. <li>VSS azure'da Windows VM için yedekleme sırasında hata oluştu.</li></ul> Her iki bu durumda yapılabilir en iyi emin olmak için verilmiştir: <ol><li> VM *ön*. <li>Yoktur *bozulma*.<li>Yoktur *veri kaybı*.</ol> Uygulamaları kendi "Düzeltme li" mekanizması geri yüklenen veriler üzerinde uygulamanız gerekir. |
| Kilitlenme tutarlılığı |Hayır |Bu durum, bir sanal makine (aracılığıyla ya da bir yazılım veya donanım sıfırlama) "kilitlenme" yaşayan eşdeğerdir. Kilitlenme tutarlılığı normal yedekleme sırasında Azure sanal makine kapatılır ortaya çıkar. Kilitlenme tutarlı bir kurtarma noktası garanti veri tutarlılığını geçici depolama ortamına--işletim sistemi veya uygulama perspektifinden ya da sağlar. Yalnızca yedekleme sırasında disk üzerinde zaten var. veri yakalanan ve yedeklendi. <br/> <br/> Varken hiçbir garanti genellikle, işletim sistemini başlatır ve ardından disk kontrol ederek Bozulması hataları düzeltmek için chkdsk gibi yordamı. Herhangi bir bellek içi veri veya diske aktarılmamış yazma kaybolur. Verileri geri alma yapılmalıdır durumda uygulama genellikle kendi doğrulama mekanizmasını ile izler. <br><br>İşlem günlüğü girişleri veritabanında mevcut değil varsa veri geri tutarlı olana kadar örnek olarak, veritabanı yazılımına yapar. (Gibi Dağıtılmış birimler) birden çok sanal disklere veri dağıldığında kilitlenme tutarlı bir kurtarma noktası verilerin doğruluğu garanti sağlar. |

## <a name="performance-and-resource-utilization"></a>Performans ve kaynak kullanımı
Dağıtılan şirket içi yedekleme yazılımı gibi kapasite ve kaynak kullanımı için gereksinimleri azure'da VM yedeklerken planlamanız gerekir. [Azure depolama sınırlarını](../azure-subscription-service-limits.md#storage-limits) nasıl yapılandıracağınıza en yüksek performans iş yüklerini çalıştırmak için en az etkiyle almak için VM dağıtımlarının tanımlayın.

Aşağıdaki Azure depolama sınırları yedekleme performansını planlarken dikkat edin:

* Depolama hesabı başına en fazla çıkış
* Depolama hesabı başına toplam istek oranı

### <a name="storage-account-limits"></a>Depolama hesabı sınırları
Yedekleme verilerini bir depolama hesabından kopyalanır, ölçümleri depolama hesabının ikinci (IOPS) ve çıkış (veya üretilen iş) başına girdi/çıktı işlemleri ekler. Aynı anda sanal makineleri de IOPS ve üretilen iş kullanıyor. Hedef depolama hesabı sınırları yedekleme ve sanal makine trafiği aşmayan sağlamaktır.

### <a name="number-of-disks"></a>Disk sayısı
Yedekleme işlemi, yedekleme işi mümkün olan en kısa sürede tamamlamak çalışır. Bunu yaparken, mümkün olduğunca fazla kaynak tüketir. Ancak, tüm g/ç işlemleri tarafından sınırlı *tek Blob için hedef işleme*, saniye başına 60 MB sınırı vardır. Her VM'in disklerini yedeklemek yedekleme işlemi, hızı en üst düzeye çıkarmak için yapılan bir girişim de çalışır *paralel*. Bir VM dört diskler varsa, tüm dört diskleri paralel yedekleme hizmeti çalışır. **Diskleri sayısı** depolama hesabı yedekleme trafiği belirlemede en önemli etken olduğundan, yedeklenen.

### <a name="backup-schedule"></a>Yedekleme zamanlaması
Performansını etkiler ek bir etmen **yedekleme zamanlaması**. İlkeler tüm sanal makineleri aynı anda yedeklenir şekilde yapılandırırsanız, trafiği sıkışması zamanladınız. Paralel tüm diskleri yedeklemek yedekleme işlemi çalışır. Bir depolama hesabından yedekleme trafiğini azaltmak için hiç örtüşmeyen günün farklı zamanında farklı Vm'leri yedekleme.

## <a name="capacity-planning"></a>Kapasite planlaması
Önceki Etkenler bir araya getirilmesi, depolama hesabı kullanımı gereksinimlerini planlamanız gerekir. Karşıdan [VM yedekleme kapasite planlama Excel elektronik tablosu](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) disk ve yedekleme zamanlaması seçimi etkisini görmek için.

### <a name="backup-throughput"></a>Yedekleme işleme
Yedeklenmekte olan her disk için Azure yedekleme disk üzerindeki blokları okur ve yalnızca değiştirilen verileri (artımlı yedeklemeyi) depolar. Aşağıdaki tabloda ortalama Yedekleme hizmetini işleme değerleri gösterir. Aşağıdaki veriler kullanılarak, belirli bir boyutta bir diski yedeklemek için gereken süreyi tahmin edebilirsiniz.

| Yedekleme işlemi | İyi verimlilik |
| --- | --- |
| İlk yedekleme |160 MB/sn |
| Artımlı yedekleme (DR) |640 Mbps <br><br> (Yedeklenmesi gereken) değiştirilen veri diski dağılmış olan, üretilen iş önemli ölçüde bırakır.|

## <a name="total-vm-backup-time"></a>Toplam VM yedekleme saati
Çoğu yedekleme zaman harcanır sırasında okuma ve veri kopyalama, diğer işlemlerin bir VM'yi yedeklemek için gereken toplam süreyi katkıda:

* Gereken süre [veya yedekleme uzantısını güncelleştirmesini](backup-azure-arm-vms.md).
* Anlık görüntü saati bir anlık görüntü tetiklemek için geçen süredir. Anlık görüntüler yakın zamanlanmış yedekleme saati tetiklenir.
* Sıra bekleme süresi. Birden çok müşteri yedeklemelerden işleme yedekleme hizmeti olduğundan, yedekleme veya kurtarma Hizmetleri kasasına yedekleme verileri anlık görüntüden kopyalama hemen başlatılamayabilir. Yoğun saatler içinde yüklenemedi, bekleme sekiz saat işlenmekte olan yedekleme sayısı nedeniyle uzatabilirsiniz. Ancak, toplam VM yedekleme günlük yedekleme ilkeleri için 24 saatten az saattir. <br>
**Bu, yalnızca artımlı yedeklemeler için ve ilk yedek için geçerli tutar. İlk yedekleme süresi orantılıdır ve veri boyutuna bağlı bağlı olarak 24 saatten uzun olabilir ve zaman yedeği alınır.**
* Veri aktarımı zamanı, depolama kasası için artımlı değişiklikler önceki yedekten işlem ve bu değişiklikleri aktarmak yedekleme hizmeti için gereken zamanı.

### <a name="why-am-i-observing-longer12-hours-backup-time"></a>Neden ı Gözlemleme longer(>12 hours) yedekleme zamanı?
Yedekleme iki aşamadan oluşur: anlık görüntüler alma ve anlık görüntüleri Kasası'na aktarma. Yedekleme hizmeti için depolama en iyi duruma getirir. Anlık görüntü verilerini bir kasaya aktarırken hizmeti yalnızca artımlı değişiklikler önceki anlık görüntüden aktarır.  Artımlı değişiklikler belirlemek için hizmet sağlama toplamı blokların hesaplar. Bir blok değiştirdiyseniz, blok kasaya gönderilmek üzere bloğu olarak tanımlanır. Ardından hizmeti ayrıntısına her veri aktarmak için en aza indirmek üzere fırsatlar arayan tanımlanan blokları, daha fazla. Tüm değişen blokları değerlendirdikten sonra hizmet değişiklikleri birleştirir ve bunları kasaya gönderir. Bazı eski uygulamalarda küçük, parçalanmış yazma depolama için uygun değildir. Anlık görüntü çok sayıda küçük, parçalanmış yazma içeriyorsa, hizmet uygulamalar tarafından yazılan veriler işlemek için ek zaman harcayan. Azure, önerilen uygulama yazma bloğundan VM içinde çalışan uygulamalar için en az 8 KB ' dir. Uygulamanızı değerinden 8 KB'lik bir blok kullanıyorsa, yedekleme performansının parametreden etkilenir. Uygulamanızın performansını artırmak için ayarlama hakkında bilgi için bkz: [uygulamalarını Azure storage ile en iyi performans için ayarlama](../virtual-machines/windows/premium-storage-performance.md). Premium depolama örnekler makaleyi yedekleme performansı kullanılsa da Kılavuzu standart depolama diskleri için geçerlidir.

## <a name="total-restore-time"></a>Toplam geri yükleme süresi
Bir geri yükleme işlemi iki ana alt görevden oluşur: Seçilen müşteri depolama hesabına geri kasadan veri kopyalama ve sanal makine oluşturma. Geri kasadan veri kopyalama, Azure yedekleme dahili olarak depolandığı ve müşteri depolama hesabı depolandığı bağlıdır. Verileri kopyalamak için harcanan süre bağlıdır:
* Hizmet işleri birden çok müşterilerden aynı anda geri yükleme işlemleri beri. sıra bekleme süresi - geri yükleme bir kuyruğa koymak isteklerdir.
* Veri kopyalama zamanı - veri kasadan müşteri depolama hesabına kopyalanır. Geri yükleme süresi üzerinde IOPS bağlıdır ve verimlilik Azure Backup hizmeti seçili müşteriyi depolama hesabındaki alır. Geri yükleme işlemi sırasında kopyalama süresini azaltmak için diğer uygulama yazma ve okuma yüklenmemiş bir depolama hesabı seçin.

## <a name="best-practices"></a>En iyi uygulamalar
Sanal makineleri için yedeklemeleri yapılandırılırken bu yöntemler aşağıdaki öneririz:

* 10'dan fazla Klasik sanal makineleri aynı anda yedeklemek için aynı bulut hizmetinden zamanlama yok. Birden çok Vm'leri aynı bulut hizmetinden yedekleme istiyorsanız, bir saate göre yedekleme Başlangıç saatlerini basamaklandırma.
* Aynı anda yedeklemek için birden fazla 40 VM'ler zamanlamayın.
* VM, yoğun olmayan saatlerde yedeklemelerin. Bu şekilde yedekleme hizmeti IOPS verileri müşteri depolama hesabından kasaya aktarmak için kullanır.
* Bir ilke farklı depolama hesaplarında yayılan VM'ler uygulanan emin olun. En fazla 20 önerdiğimiz tek bir depolama hesabına toplam disklerden aynı yedekleme zamanlaması tarafından korunmalıdır. 20 diskler daha büyük bir depolama hesabı varsa, bu sanal makineleri yedekleme işlemi aktarımı aşamasında gerekli IOPS almak için birden çok ilke arasında yayılır.
* Aynı depolama hesabı için Premium depolama üzerinde çalışan bir VM geri yüklemeyin. Geri yükleme işlemi işlemi yedekleme işlemi ile örtüşür, yedekleme için kullanılabilir IOPS azaltır.
* Yığında VM yedekleme V1 Premium VM yedekleme için Azure Backup hizmeti, kasaya depolama hesabındaki kopyalanan bu konumdan, depolama hesabı ve aktarım veri anlık görüntü kopyalayabilirsiniz böylece toplam depolama hesabı alanı % 50'yalnızca tahsis önerilir.
* Yedekleme 2.7 için emin olun, python sürümü Linux sanal makineleri üzerinde etkin

## <a name="data-encryption"></a>Veri şifrelemesi
Azure yedekleme, Yedekleme işleminin bir parçası olarak verilerini şifrelemez. Ancak, VM dahilinde verileri şifrelemek ve sorunsuz bir şekilde korumalı verileri yedekleme (daha fazla bilgi edinin [şifrelenmiş verilerin yedekleme](backup-azure-vms-encryption.md)).

## <a name="calculating-the-cost-of-protected-instances"></a>Korumalı örnekler maliyetini hesaplama
Azure Backup yedeklenir Azure sanal makineleri olan tabi [Azure yedekleme fiyatlandırması](https://azure.microsoft.com/pricing/details/backup/). Korumalı örnekler hesaplaması dayanır *gerçek* geçici depolama hariç olmak üzere sanal makinede--tüm verilerin toplamı olan sanal makine boyutu.

Vm'leri yedekleme için fiyatlandırma sanal makineye bağlı her veri diski için desteklenen en büyük boyutu dayanmıyor. Fiyatlandırma veri diski saklanan gerçek verileri temel alır. Benzer şekilde, Yedekleme depolaması faturanızda Azure Yedekleme'de her kurtarma noktası gerçek veri toplamını olduğu depolanan veri miktarını temel alır.

Örneğin, bir boyutta A2 standart iki ek veri disklerinin her biri 1 TB maksimum boyuta sahip olan sanal makine alın. Aşağıdaki tabloda her bu diskleri saklanan gerçek verileri verir:

| Disk türü | Maksimum boyut | Gerçek veri yok |
| --------- | -------- | ----------- |
| İşletim sistemi diski |1023 GB |17 GB |
| Yerel disk / geçici disk |135 GB |5 GB (yedekleme için yer almayan) |
| Veri diski 1 |1023 GB |30 GB |
| Veri diski 2 |1023 GB |0 GB |

Sanal makine gerçek boyutuna, 17 GB + 30 GB + 0 GB = 47 GB bu durumda olur. Bu korumalı örnek boyutu (47 GB) aylık fatura temelini olur. Sanal makine veri miktarı arttıkça, korumalı örnek boyutu için fatura değişiklikleri buna uygun olarak kullanılır.

Faturalama, ilk başarılı yedekleme tamamlanana kadar başlatılmaz. Bu noktada, hem depolama hem de korumalı örnekleri için fatura başlar. Sanal makine için bir kasa içinde depolanan tüm yedekleme verileri var olduğu sürece faturalandırma devam eder. Sanal makinede korumayı durdurun, ancak sanal makine yedekleme verilerini bir kasada var olması gerekir, faturalandırma devam eder.

Belirtilen sanal makine için fatura yalnızca koruma durdurulursa ve tüm yedekleme verileri silinir durdurur. Koruma durduğunda ve etkin yedek iş yok, son başarılı VM yedeğinin boyutu aylık fatura için kullanılan korumalı örnek boyutu olur.

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Sonraki adımlar
* [Sanal makineleri yedekleme](backup-azure-arm-vms.md)
* [Sanal makine yedeklemesi yönetme](backup-azure-manage-vms.md)
* [Sanal makineleri geri yükleme](backup-azure-arm-restore-vms.md)
* [VM yedekleme sorunlarını giderme](backup-azure-vms-troubleshoot.md)
