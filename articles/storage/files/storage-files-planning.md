---
title: Bir Azure dosyaları dağıtımını planlama | Microsoft Docs
description: Bir Azure dosyaları dağıtımını planlarken dikkate almanız gerekenler öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 04/25/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 6506a93914cfbc10f37980c4b916a93aa9aad75d
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67564413"
---
# <a name="planning-for-an-azure-files-deployment"></a>Azure Dosyaları dağıtımı planlama

[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları endüstri standardı SMB protokolünü erişilebilen bulutta sunar. Azure dosyaları tam olarak yönetildiğinden, üretim senaryolarında dağıtma dağıtılması ve dosya sunucusu veya NAS cihazınızın yönetilmesi daha kolaydır. Bu makalede, kuruluşunuzdaki üretim kullanımı için Azure dosya paylaşımını dağıtırken göz önünde bulundurmanız konularını ele alır.

## <a name="management-concepts"></a>Yönetim Kavramları

 Aşağıdaki diyagramda Azure dosyaları yönetim yapıları gösterilir:

![Dosya Yapısı](./media/storage-files-introduction/files-concepts.png)

* **Depolama hesabı**: Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için, [Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) konusuna bakın.

* **Paylaşım**: Dosya depolama paylaşımı azure'daki bir SMB dosyası paylaşımıdır. Tüm dizinler ve dosyalar üst paylaşımda oluşturulmalıdır. Bir hesapta sınırsız sayıda paylaşım olabilir ve Paylaşım sınırsız sayıda dosya paylaşımı 5 TiB toplam kapasitesini dosya depolayabilir.

* **Dizin**: Dizinlerin isteğe bağlı hiyerarşisi.

* **Dosya**: Paylaşımdaki bir dosya. Bir dosya boyutu en fazla 1 TiB olabilir.

* **URL biçimi**: Bir Azure dosya paylaşımına dosya REST protokolü ile yapılan istekler için dosyalar şu URL biçimi kullanılarak adreslenebilir:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/<file>
    ```

## <a name="data-access-method"></a>Veri erişimi yöntemi

Azure dosyaları teklifleri iki, yerleşik, kullanışlı, ayrı ayrı veya birbiriyle birlikte verilerinize erişmek için kullanabileceğiniz yöntemler verilere:

1. **Doğrudan bulut erişimi**: Herhangi bir Azure dosya paylaşımının tarafından bağlanabilir [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md), ve/veya [Linux](storage-how-to-use-files-linux.md) sektörde standart sunucu ileti bloğu (SMB) protokolü ile veya dosya REST API'si aracılığıyla. SMB ile Azure dosya paylaşımında doğrudan okuma ve yazma işlemleri için dosya paylaşımında yapılır. Azure'da VM tarafından bağlamak için SMB istemci işletim sisteminde en az desteklemelidir SMB 2.1. Şirket içinde kullanıcı iş istasyonunda en az iş istasyonu tarafından desteklenen SMB istemcisi desteklemelidir gibi bağlamak için SMB 3.0 (ile şifreleme). SMB ek olarak, yeni uygulamalar veya hizmetler dosya paylaşımını dosya yazılım geliştirme için bir kolayca ve ölçeklenebilir bir uygulama programlama arabirimi sağlayan REST aracılığıyla doğrudan erişebilir.
2. **Azure dosya eşitleme**: Azure dosya eşitleme ile paylaşımları Windows sunucuları şirket içi veya azure'de çoğaltılabilir. Kullanıcılarınızın Windows Server üzerinden dosya paylaşımı gibi SMB veya NFS paylaşım yoluyla erişir. Bu, hangi veriler erişilen ve uzakta bir Azure veri merkezlerinden gibi bir şube ofis senaryosunda değiştiren senaryoları için kullanışlıdır. Veri arasında birden fazla Windows Server uç noktası, gibi birden çok şube ofis arasında çoğaltılabilir. Son olarak, Azure dosyaları'na tüm verileri, sunucunun hala erişilebilir, ancak sunucu verilerin tam bir kopyasını yok şekilde katmanlanmış verileri. Bunun yerine, verileri sorunsuz bir şekilde, kullanıcı tarafından açıldığında çağrılır.

Aşağıdaki tabloda, kullanıcılar ve uygulamalar Azure dosya paylaşımınızı nasıl erişeceği gösterilmektedir:

| | Doğrudan bulut erişimi | Azure Dosya Eşitleme |
|------------------------|------------|-----------------|
| Hangi protokollerin kullanılacağını ihtiyacınız var? | Azure dosyaları SMB 2.1 ve SMB 3.0 dosya REST API'sini destekler. | Azure dosya paylaşımınızı (SMB, NFS, FTPS, vb.) Windows Server'da desteklenen bir protokolü üzerinden erişim |  
| İş yükünüz nerede kullanıyorsunuz? | **Azure'da**: Azure dosyaları, verilere doğrudan erişime olanak sağlar. | **Yavaş ağ ile şirket içi**: Windows, Linux ve Macos'ta istemciler, Azure dosya paylaşımınızın hızlı bir önbellek bir yerel şirket içi Windows dosya paylaşımı bağlayabilir. |
| ACL'ler düzeyini ihtiyacınız var? | Paylaşım ve dosya düzeyi. | Paylaşım, dosya ve kullanıcı düzeyi. |

## <a name="data-security"></a>Veri güvenliği

Azure dosyaları, veri güvenliğini sağlamaya yönelik çeşitli yerleşik seçenekler vardır:

* Her iki üzerinden hat protokolleri şifreleme desteği: SMB 3.0 şifreleme ve HTTPS üzerinden dosya REST. Varsayılan olarak: 
    * SMB 3.0 şifrelemesini destekleyen istemciler, gönderin ve şifreli bir kanal veri alın.
    * SMB 3.0 şifreleme ile desteklemeyen istemciler içi veri merkezi SMB 2.1 veya SMB 3.0 üzerinden şifreleme olmadan iletişim kurabilir. SMB istemcileri inter-datacenter SMB 2.1 veya SMB 3.0 üzerinden şifreleme olmadan iletişim kurmasına izin verilmez.
    * İstemciler, HTTP veya HTTPS ile dosya REST üzerinden iletişim kurabilirsiniz.
* Bekleyen şifreleme ([Azure depolama hizmeti şifrelemesi](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): Depolama hizmeti şifrelemesi (SSE) tüm depolama hesapları için etkinleştirildi. Bekleyen verileri tam olarak yönetilen anahtarlarla şifrelenir. Bekleyen şifreleme depolama maliyetlerini artırabilir veya performansı düşürebilir desteklemez. 
* Şifrelenmiş veriler aktarım sırasında isteğe bağlı gereksinimi: Seçili olduğunda, Azure dosyaları şifresiz kanal veri erişimi reddeder. Özellikle, yalnızca HTTPS ve SMB 3.0 şifreleme bağlantılarıyla izin verilir.

    > [!Important]  
    > Güvenli veri aktarımı gerektiren başarısız olmasına eski SMB istemcileriniz değil şifrelemesi ile SMB 3.0 ile iletişim kurabilen neden olur. Daha fazla bilgi için [Windows üzerinde bağlama](storage-how-to-use-files-windows.md), [Linux üzerinde bağlama](storage-how-to-use-files-linux.md), ve [Macos'ta bağlama](storage-how-to-use-files-mac.md).

En yüksek güvenlik için her zaman her iki şifreleme bekleyen etkinleştirme ve modern istemciler verilerinize erişmek için kullandığınız her veri aktarım sırasında şifreleme etkinleştirilmesi önerilir. Örneğin, bir Windows Server 2008 R2 yalnızca SMB 2.1 destekleyen, sanal makinede paylaşımını gerekiyorsa SMB 2.1 şifreleme desteklemediğinden depolama hesabınıza şifrelenmemiş trafiği izin vermeniz gerekir.

Azure dosya paylaşımınızı erişmek için Azure dosya eşitleme'ı kullanıyorsanız her zaman HTTPS ve SMB 3.0 şifrelemesi ile bekleyen verilerin şifrelenmesi gereksiniminiz olup bağımsız olarak, Windows sunucularını verilerinizi eşitlemek için kullanacağız.

## <a name="file-share-performance-tiers"></a>Dosya Paylaşımı performans katmanları

Azure dosyaları iki performans katmanı sunar: standart ve premium.

### <a name="standard-file-shares"></a>Standart dosya paylaşımları

Standart dosya paylaşımları, sabit disk sürücülerinin (HDD'ler) tarafından desteklenir. Standart dosya paylaşımları gibi genel amaçlı dosya paylaşımları ve geliştirme/test ortamları performans değişkenliğine daha az duyarlı olan g/ç iş yükleri için güvenilir performans sağlar. Standart dosya paylaşımları, yalnızca Kullandıkça Öde faturalandırma modeli içinde kullanılabilir.

Standart dosya paylaşımlarının boyutu 5 TiB kadar bir GA teklifi olarak kullanılabilir. En çok 100 TiB 5 TiB büyük tüm paylaşımları büyük dosya paylaşımları, şu anda Önizleme teklifi olarak mevcut olsa da.

> [!IMPORTANT]
> Bkz: [büyük dosya paylaşımları (standart katman) sağlamak](#onboard-to-larger-file-shares-standard-tier) adımları ekleme yanı sıra kapsamı ve önizleme kısıtlamaları bölümü.

### <a name="premium-file-shares"></a>Premium dosya paylaşımları

Premium dosya paylaşımları, katı hal sürücüleri (SSD) tarafından desteklenir. Premium dosya paylaşımları, tutarlı, yüksek performanslı ve düşük gecikme süreli, yoğun g/ç iş yükleri için çoğu g/ç işlemleri için Tek haneli milisaniye içinde sağlayın. Bu çok çeşitli veritabanları, web sitesi barındırma ve geliştirme ortamları gibi iş yükleri için uygun sağlar. Premium dosya paylaşımları, yalnızca sağlanan faturalama modelinde kullanılabilir. Premium dosya paylaşımları, standart dosya paylaşımlarından ayrı bir dağıtım modeli kullanın.

Azure Backup, premium dosya paylaşımları için kullanılabilir ve Azure Kubernetes hizmeti premium dosya paylaşımları 1.13 sürümü ve üzerini destekler.

Premium dosya paylaşımı oluşturma konusunda bilgi almak istiyorsanız, konu üzerinde makalemizi bakın: [Bir premium Azure dosya depolama hesabının nasıl oluşturulacağını](storage-how-to-create-premium-fileshare.md).

Şu anda, standart dosya paylaşımı ve premium dosya paylaşımı arasında doğrudan dönüştürülemiyor. Her iki katmana geçmek istiyorsanız, bu katmanda yeni bir dosya paylaşımı oluşturma ve el ile verilerin, özgün paylaşımından oluşturduğunuz yeni paylaşıma kopyalayın. Robocopy veya AzCopy gibi desteklenen Azure dosyaları kopyalama araçlardan herhangi birini kullanarak bunu yapabilirsiniz.

> [!IMPORTANT]
> Premium dosya paylaşımları, yalnızca LRS ile kullanılabilir ve depolama hesapları sunan birçok bölgede kullanılabilir. Premium dosya paylaşımları Bölgenizde kullanılabilir olup olmadığını öğrenmek için bkz: [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=storage) Azure sayfası.

### <a name="provisioned-shares"></a>Sağlanan paylaşımlar

Premium dosya paylaşımları, temel bir sabit GiB/IOPS/işleme oranını sağlanır. Bir IOPS ve Paylaşım başına maksimum sınırlara kadar 0,1 MiB/sn aktarım hızı, sağlanan her GiB için paylaşım verilir. Sağlama izin verilen en düşük, en düşük IOPS/aktarım hızı ile 100 GiB ' dir.

En iyi çaba ilkesine göre tüm paylaşımlar üç IOPS sağlanan depolama GiB başına en fazla 60 dakika veya daha uzun paylaşımın boyutuna bağlı olarak veri bloğu. Yeni paylaşımlar üzerinde sağlanan kapasitesine göre tam veri bloğu kredi ile başlayın.

Paylaşımları 1 GiB artışlarla sağlanması gerekir. En küçük boyutu 100 GiB, bir sonraki boyuta 101 GiB ve böyle devam eder.

> [!TIP]
> Temel IOPS = 1 * GiB sağlandı. (En fazla 100.000 IOPS kadar).
>
> Veri bloğu sınırı 3 = * temel IOPS. (En fazla 100.000 IOPS kadar).
>
> Çıkış oranı = 60 MiB/sn + 0,06 * GiB sağlandı
>
> Giriş oranı = 40 MiB/sn + 0.04 * GiB sağlandı

Paylaşım boyutu, herhangi bir zamanda artırılabilir ancak yalnızca son artış itibaren 24 saat sonra azaltılabilir. Boyutu artışı olmadan 24 saat boyunca bekledikten sonra tekrar artırmak kadar istediğiniz gibi birçok kez paylaşım boyutu düşürebilir. Boyutu değişiklikten sonra birkaç dakika içinde IOPS/işleme ölçek değişiklikler geçerli olacaktır.

Aşağıda, kullanılan GiB, sağlanan paylaşım boyutunu azaltmak mümkündür. Bunu yaparsanız, verileri kaybetmez ancak yine de kullanılan boyutu için Faturalandırılacak ve kullanılan boyutunun sağlanan paylaşımının (temel IOPS, üretilen iş ve veri bloğu IOPS) performansı alırsınız.

Aşağıdaki tabloda, sağlanan paylaşım boyutları için bu formül, bazı örnekler gösterilmektedir:

|Kapasite (GiB) | Temel IOPS | IOPS veri bloğu | Egress (MiB/s) | Giriş (MiB/sn) |
|---------|---------|---------|---------|---------|
|100         | 100     | En fazla 300     | 66   | 44   |
|500         | 500     | En fazla 1500   | 90   | 60   |
|1,024       | 1,024   | En fazla 3.072   | 122   | 81   |
|5,120       | 5,120   | En fazla 15.360  | 368   | 245   |
|10,240      | 10,240  | En fazla 30.720  | 675 | 450   |
|33,792      | 33,792  | En fazla 100.000 | 2,088 | 1,392   |
|51,200      | 51,200  | En fazla 100.000 | 3,132 | 2,088   |
|102,400     | 100,000 | En fazla 100.000 | 6,204 | 4,136   |

> [!NOTE]
> Dosya paylaşımlarını performans makine ağ sınırları, kullanılabilir ağ bant genişliği, g/ç boyutu, diğer faktörlerden arasında bir paralellik tabidir. En yüksek performansı ölçeği elde etmek için birden çok VM arasında yükü dağıtabilir. Lütfen [sorun giderme kılavuzu](storage-troubleshooting-files-performance.md) bazı genel performans sorunlarını ve geçici çözümler.

### <a name="bursting"></a>Genişletme

Premium dosya paylaşımlarını bir faktör üç kadar kendi IOPS veri bloğu. Genişletme, otomatik olarak ve kredi sisteme göre çalışır. En iyi çaba ilkesine ve yığma sınırı üzerinde çalışır Patlaması garantisi değil, dosya paylaşımları veri bloğu *kadar* sınırı.

Dosya paylaşımınızın trafiği IOPS temel altında olduğunda bir veri bloğu kovada KREDİLERİ toplar. Örneğin, 100 temel IOPS bir 100 GiB paylaşımı vardır. Paylaşım gerçek trafiği için belirli 1 saniyelik aralıklarla 40 IOPS olduysa, 60 kullanılmayan IOPS için bir veri bloğu demet alacak. İşlem ' % s'temeli IOPS aşılmasına KREDİLERİ ardından daha sonra kullanılacak.

> [!TIP]
> Veri bloğu demet boyutunu temel IOPS = * 2 * 3600.

Bir Paylaşımı ' % s'temeli IOPS aşıyor ve veri bloğu kovada KREDİLERİ olan her veri bloğu. Paylaşımları paylaşımları 50 TiB daha küçük bir saat süreyle veri bloğu sınırı, yalnızca kalır ancak kalan KREDİLERİ sürece veri bloğu devam edebilirsiniz. Paylaşımları 50 TiB daha büyük, teknik olarak bu bir saatlik sınırın aşılmasına neden olabilir, Yukarı ancak, bu iki saate tahakkuk eden aşırı KREDİLERİ sayısına bağlıdır. Bir kredi temel IOPS dışına her GÇ kullanır ve tüm KREDİLERİ tüketilen sonra paylaşım IOPS taban çizgisine döndürür.

Paylaşım KREDİLERİ üç durumu vardır:

- Dosya Paylaşımı ' % s'temeli IOPS sayısından az kullanılırken uygulanıyor.
- Dosya Paylaşımı Patlaması zaman azalan.
- Hiçbir KREDİLERİ veya temel sabit kalan IOPS kullanımda olması.

Yeni dosya paylaşımları başlangıcı tam krediler kendi veri bloğu demet sayısı ile. Paylaşım IOPS IOPS temel sunucu tarafından azaltıldığı 'un altına düşersek veri bloğu KREDİLERİ tahakkuk değil.

## <a name="file-share-redundancy"></a>Dosya Paylaşımı yedeklilik

Standart Azure dosya paylaşımları desteği üç veri yedekliliği seçenekleri: yerel olarak yedekli depolama (LRS), bölgesel olarak yedekli depolama (ZRS) ve coğrafi olarak yedekli depolama (GRS).

Azure dosyaları premium paylaşımları yalnızca yerel olarak yedekli depolama (LRS) destekler.

Aşağıdaki bölümlerde farklı yedekliliği seçenekleri arasındaki farklar açıklanmaktadır:

### <a name="locally-redundant-storage"></a>Yerel olarak yedekli depolama

[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Bölgesel olarak yedekli depolama

[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Coğrafi olarak yedekli depolama

> [!Warning]  
> Azure dosya paylaşımınızı GRS depolama hesabındaki bir bulut uç noktası olarak kullanıyorsanız, depolama hesabı yük devretme başlatma olmamalıdır. Bunun yapılması ayrıca çalışma ve Mayıs durdurmak için neden Eşitleme beklenmeyen veri kaybı durumunda yeni katmanlı dosyalar neden olur. Bir Azure bölgesi kaybı söz konusu olduğunda, Microsoft Azure dosya eşitleme ile uyumlu bir şekilde depolama hesabı yük devretmeyi tetikler.

Coğrafi olarak yedekli depolama (GRS), en az % 99,99999999999999'si sağlamak için tasarlanmıştır (16 9) mil birincil bölgeden yüzlerce ikincil bir bölgeye veri çoğaltma ile belirli bir yıl boyunca nesnelerin dayanıklılık. Etkin GRS depolama hesabınızın sahip, verilerinizi tam bölgesel bir kesinti veya birincil bölgeye kurtarılamaz bir olağanüstü durum olması durumunda bile kalıcı olur.

Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) kullanmayı seçerseniz, herhangi bir bölgede şu anda Azure dosya okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) desteklememektedir bilmelidir. RA-GRS depolama hesabındaki dosya paylaşımlarına GRS hesapları yaptıkları gibi çalışır ve dolu GRS fiyatlarıdır.

Veriler yalnızca Microsoft ikincil bölgeye birincil bir yük devretme başlatır, okumak kullanılabilir ancak bu GRS, verilerinizi ikincil bir bölgede başka bir veri merkezine çoğaltır.

Etkin GRS ile bir depolama hesabı için tüm verileri ilk kez çoğaltılır yerel olarak yedekli depolama (LRS). Bir güncelleştirme, öncelikle birincil konuma taahhüt eder ve LRS kullanılarak çoğaltılır. Güncelleştirmeyi zaman uyumsuz olarak GRS kullanarak ikincil bir bölgeye çoğaltılır. İkincil konuma veri yazıldığında de LRS kullanarak içinde bu konuma çoğaltılır.

Birincil ve ikincil bölgeler, çoğaltmalar ayrı hata etki alanları arasında yönetme ve yükseltme etki alanları içindeki bir depolama ölçek birimi. Depolama ölçek birimi, veri merkezi içinde temel çoğaltma birimidir. Bu düzeyde çoğaltma LRS tarafından sağlanır; Daha fazla bilgi için [yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](../common/storage-redundancy-lrs.md).

Hangi çoğaltma seçeneği kullanmak için karar verirken, aşağıdaki noktaları göz önünde bulundurun:

* Bölgesel olarak yedekli depolama (ZRS), yüksek oranda kullanılabilirlik ile zaman uyumlu çoğaltma sağlar ve bazı senaryolar için GRS daha iyi bir seçim olabilir. ZRS hakkında daha fazla bilgi için bkz. [ZRS](../common/storage-redundancy-zrs.md).
* Zaman uyumsuz çoğaltma, ikincil bölgeye çoğaltılır, birincil bölgeye yazılır veri andan itibaren bir gecikme içerir. Birincil bölgeden verilerin kurtarılamaması durumunda bölgesel bir olağanüstü durum yaşandığında ikincil bölgeye henüz çoğaltılan henüz değişiklikler kaybolabilir.
* GRS ile Microsoft ikincil bölgeye yük devretme başlatır sürece çoğaltma okuma veya yazma erişimi için kullanılabilir değildir. Bir yük devretme durumunda okuma ve yük devretme sonrasında bu verilere yazma erişimi tamamlandı. Daha fazla bilgi için lütfen bkz [olağanüstü durum kurtarma Kılavuzu](../common/storage-disaster-recovery-guidance.md).

## <a name="onboard-to-larger-file-shares-standard-tier"></a>Ekleme büyük dosya paylaşımları (standart katman)

Bu bölüm, yalnızca standart dosya paylaşımları için de geçerlidir. Tüm premium dosya paylaşımları ile 100 tib'a kadar bir GA teklifi olarak kullanılabilir.

### <a name="restrictions"></a>Kısıtlamalar

- (Var olan depolama hesapları genişletilemiyor) yeni bir genel amaçlı depolama hesabı oluşturmanızı gerektirir.
- LRS, GRS hesabı dönüştürme için bir abonelik daha büyük dosya paylaşımları önizlemesi için kabul edildikten sonra oluşturulan tüm yeni depolama hesapları hakkında mümkün olmayacaktır.

### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Standart dosya paylaşımları 5 TiB kadar tüm bölgelerde kullanılabilir. Belirli bölgelerde 100 TiB sınırı ile kullanılabilir, aşağıdaki tabloda bu bölgeler listelenir:

|Bölge  |Desteklenen yedeklilik  |Var olan depolama hesaplarını destekler  |
|---------|---------|---------|
|Güneydoğu Asya     |LRS|Hayır         |
|Batı Avrupa     |LRS|Hayır         |
|Batı ABD 2     |LRS, ZRS|Hayır         |


### <a name="steps-to-onboard"></a>Adımları eklemek için

Aboneliğinize daha büyük dosya paylaşımları önizlemesi kaydetmek için aşağıdaki PowerShell komutlarını çalıştırın:

```powershell
Register-AzProviderFeature -FeatureName AllowLargeFileShares -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```
Her iki komutu çalıştırdıktan sonra aboneliğiniz otomatik olarak onaylanır.

Kayıt durumunuzu doğrulamak için aşağıdaki komutu çalıştırabilirsiniz:

```powershell
Get-AzProviderFeature -FeatureName AllowLargeFileShares -ProviderNamespace Microsoft.Storage
```

Durumunuzu güncelleştirmek 15 dakika kadar sürebilir **kayıtlı**. Durumunuzu olduğunda **kayıtlı**, kullanılmakta olan olmalıdır.

### <a name="use-larger-file-shares"></a>Büyük dosya paylaşımları kullanın

Büyük dosya paylaşımları'ı kullanmaya başlamak için yeni bir genel amaçlı v2 depolama hesabı ve yeni bir dosya paylaşımı oluşturun.

## <a name="data-growth-pattern"></a>Veri büyümesi deseni

Bugün, bir Azure dosya paylaşımı için boyut üst sınırı 5 TiB olduğu (Önizleme 100 tib'a kadar). Şu anki bu sınırlama nedeniyle, bir Azure dosya paylaşımı dağıtırken beklenen veri artışına düşünmelisiniz.

Azure dosya eşitleme ile tek bir Windows dosya sunucusu için birden çok Azure dosya paylaşımları eşitlenecek mümkündür. Bu, şirket içi olabilir eski, büyük dosya paylaşımlarını Azure dosya eşitleme ile sağlanabildiğinden emin olmak sağlar. Daha fazla bilgi için [bir Azure dosya eşitleme dağıtımı planlama](storage-files-planning.md).

## <a name="data-transfer-method"></a>Veri aktarım yöntemi

Var olan bir dosyadan veri paylaşımı, bir şirket içi dosya paylaşımı gibi Azure dosyalarına aktarma toplu olarak kolay pek çok seçenek vardır. Birkaç popüler olanları (kapsamlı olmayan liste) şunları içerir:

* **Azure dosya eşitleme**: Bir Azure dosya paylaşımı ("bulut uç noktasına") ve bir Windows dizin ad alanı ("sunucu uç noktası") arasında bir ilk eşitleme işleminin bir parçası olarak Azure dosya eşitleme tüm veriler var olan bir dosya paylaşımından Azure dosyaları'na çoğaltır.
* **[Azure içeri/dışarı aktarma](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : Azure içeri/dışarı aktarma hizmeti, bir Azure veri merkezine sabit disk sürücüleri sevkiyat tarafından bir Azure dosya paylaşımına güvenli bir şekilde büyük miktarlarda veri aktarmanıza olanak sağlar. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)** : Robocopy Windows ve Windows Server ile birlikte gelen bilinen kopya bir araçtır. Robocopy, yerel dosya paylaşımını bağlama ve ardından hedef Robocopy komutunu olarak bağlı konumu kullanarak Azure dosyaları veri aktarmak için kullanılabilir.
* **[AzCopy](../common/storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : AzCopy, en iyi performansı sunan basit komutlar kullanılarak Azure dosyaları yanı sıra, Azure Blob Depolama, gelen ve giden veri kopyalamak için tasarlanan bir komut satırı yardımcı programıdır.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
* [Azure dosyaları'nı dağıtma](storage-files-deployment-guide.md)
* [Azure dosya eşitleme dağıtımı](storage-sync-files-deployment-guide.md)
