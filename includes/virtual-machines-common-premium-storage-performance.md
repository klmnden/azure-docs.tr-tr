# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium Storage: Yüksek performans tasarımı
## <a name="overview"></a>Genel Bakış
Bu makalede, Azure Premium Storage kullanarak yüksek performanslı uygulamalar oluşturmaya yönelik yönergeler sağlar. Performansı en iyi yöntemler, uygulamanız tarafından kullanılan teknolojiler uygulanabilir birlikte bu belgede sağlanan yönergeleri kullanabilirsiniz. Yönergeleri göstermek için bu belge boyunca bir örnek olarak Premium depolama üzerinde çalışan SQL Server kullandınız.

Biz bu makalede depolama katmanı için performans senaryosu olsa da, uygulama katmanı iyileştirmek gerekir. Örneğin, bir SharePoint grubunda Azure Premium Storage barındırıyorsanız, veritabanı sunucusu iyileştirmek için bu makalede SQL Server örneklerinden kullanabilirsiniz. Ayrıca, SharePoint grubunun Web sunucusu ve çoğu performans almak için uygulama sunucusu iyileştirin.

Bu makalede Azure Premium Storage uygulama performansı en iyi duruma getirme hakkında sık sorulan sorular aşağıdaki sorunuza yanıt almanıza yardımcı,

* Uygulama performansını ölçmek nasıl?  
* Neden, beklenen yüksek performanslı görmediğinizden?  
* Hangi Etkenler Premium depolama, uygulama performansını etkiler?  
* Bu etkenler Premium depolama, uygulamanızın performansını nasıl etkiler?  
* Nasıl, IOPS, bant genişliği ve gecikme süresi için en iyi duruma getirebilirsiniz?  

Premium depolama üzerinde çalışan iş yüklerini yüksek performans hassas olduğundan özellikle Premium Storage için bu yönergeleri sağladık. Uygun olan yerlerde örnekler sağladık. Ayrıca standart depolama disklerle Iaas VM'ler üzerinde çalıştırılan uygulamalar bu yönergeleri bazıları uygulayabilirsiniz.

Premium depolama alanına yeniyseniz, başlamadan önce ilk okuma [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md) ve [Azure Storage ölçeklenebilirlik ve performans hedefleri](../articles/storage/common/storage-scalability-targets.md) makaleleri.

## <a name="application-performance-indicators"></a>Uygulama performans göstergeleri
Bir uygulama da ya da olmasın performans göstergeleri gibi kullanarak gerçekleştiriyor olup olmadığını biz değerlendirmek, ne kadar hızlı bir uygulama bir kullanıcı isteği, istek başına bir uygulama işleme ne kadar veri işleme, kaç uygulama işleme süresinin kendi İsteği gönderdikten sonra bir yanıt elde etmek için bir kullanıcının sahip, ne kadar süreyle belirli bir süre içinde istekleri. Bu performans göstergeleri için teknik koşulları:, IOPS, üretilen iş veya bant genişliği ve gecikme süresi.

Bu bölümde, Premium depolama bağlamında yaygın performans göstergelerini aşağıdakiler ele alınacaktır. Toplama uygulama gereksinimleri, aşağıdaki bölümde, bu performans göstergeleri uygulamanızın ölçmek öğreneceksiniz. Daha sonra en iyi duruma getirme uygulama performansını, bu performans göstergelerini ve bunları en iyi duruma getirmek için öneriler etkileyen faktörler hakkında bilgi edineceksiniz.

## <a name="iops"></a>IOPS
IOPS sayı uygulamanızı depolama diskleri bir saniyede gönderdiği istekleri. Bir giriş/çıkış işlem okunamadı veya sıralı veya rastgele yazma. Hemen çok sayıda eş zamanlı kullanıcı isteklerini işlemek bir çevrimiçi perakende Web sitesi gibi OLTP uygulamaları gerekir. Kullanıcı isteklerini Ekle olan ve uygulama hızlı bir şekilde işlemelidir yoğun veritabanı işlemleri güncelleştirin. Bu nedenle, çok yüksek IOPS OLTP uygulamalar gerektirir. Bu tür uygulamalar, küçük ve rastgele g/ç istekleri milyonlarca işleyin. Bu tür bir uygulamanız varsa, IOPS için en iyi duruma getirmek için uygulama altyapı tasarlamanız gerekir. Sonraki bölümde *uygulama performansı en iyi duruma getirme*, biz ayrıntılı olarak yüksek IOPS almak için dikkate almanız gereken tüm faktörleri ele almaktadır.

Ne zaman bir premium depolama disk Azure sağlarken, garantili IOPS sayısını disk belirtimine göre yüksek ölçekli VM ekleyin. Örneğin, bir P50 disk 7500 IOPS sağlar. Her yüksek ölçekli VM boyutu da sürdürebilmek belirli bir IOPS sınırı vardır. Örneğin, standart GS5 VM 80.000 IOPS sınırı vardır.

## <a name="throughput"></a>Aktarım hızı
Üretilen iş ya da bant genişliği, uygulamanızın belirli bir aralıktaki depolama diskleri gönderiyor veri miktarıdır. Uygulamanızı girdi/çıktı işlemleri büyük GÇ birim boyutlarını ile çalışıyorsa, yüksek verimlilik gerektirir. Veri ambarı uygulamalarını veri büyük bölümünü aynı anda erişebilir ve yaygın olarak toplu işlemleri tarama yoğun işlemleri sorunu eğilimindedir. Diğer bir deyişle, bu tür uygulamalar, daha yüksek verimlilik gerektirir. Bu tür bir uygulamanız varsa, üretilen iş için en iyi duruma getirmek için altyapı tasarlamanız gerekir. Sonraki bölümde, biz bunu başarmanın ayarlamak Etkenler ayrıntılı olarak açıklanmaktadır.

Ne zaman bir premium depolama disk Azure hükümleri verimlilik, disk belirtimine göre bir yüksek ölçekli VM ekleyin. Örneğin, P50 disk 250 MB ikinci bir disk verimlilik sağlar. Her yüksek ölçekli VM boyutu bu karşılayabilir belirli üretilen iş sınırı da sahiptir. Örneğin, standart GS5 VM 2.000 saniyede bir en yüksek verimlilik sahiptir. 

Üretilen iş ve formülde gösterildiği gibi IOPS arasında bir ilişki yoktur.

![](media/premium-storage-performance/image1.png)

Bu nedenle, uygulamanızın gerektirdiği en iyi performans ve IOPS değerlerini belirlemek önemlidir. Bir en iyi duruma getirme çalışırken diğeri de etkilenen. Bir sonraki bölümde *uygulama performansı en iyi duruma getirme*, biz IOPS ve üretilen iş en iyi duruma getirme hakkında daha ayrıntılı olarak ele alınacaktır.

## <a name="latency"></a>Gecikme süresi
Gecikme uygulamanın tek bir istek almak, depolama diskleri göndermek ve yanıt istemciye göndermek için gereken süre anlamına gelmektedir. Bu uygulamanın performans IOPS ve üretilen iş yanı sıra kritik bir ölçüsüdür. Premium depolama diskini gecikme isteği bilgilerini almak ve uygulamanıza geri iletişim kurmak için gereken süre anlamına gelmektedir. Premium depolama tutarlı düşük gecikme süreleri sağlar. Salt okunur ana bilgisayar premium depolama disklerde önbelleğe almayı etkinleştirirseniz, çok daha düşük okuma gecikmesi alabilirsiniz. Disk önbelleği sonraki bölümünde daha ayrıntılı üzerinde aşağıdakiler ele alınacaktır *uygulama performansı en iyi duruma getirme*.

Daha yüksek IOPS ve üretilen iş almak için uygulamanızı iyileştirirken gecikme, uygulamanızın etkiler. Uygulama performans ayarlama sonra her zaman gecikmesini yüksek gecikme beklenmeyen davranışları önlemek için uygulamanın değerlendirin.

## <a name="gather-application-performance-requirements"></a>Uygulama Performans gereksinimlerini toplama
Azure Premium Storage üzerinde çalışan yüksek performanslı uygulamalar tasarlamanın ilk adımı, uygulamanızın performansı gereksinimlerini anlamaktır. Performans gereksinimlerini topladıktan sonra uygulamanızı en iyi performans elde etmek için en iyi duruma getirebilirsiniz.

Önceki bölümde, genel performans göstergeleri, IOPS, üretilen iş ve gecikme açıklanmıştır. Bu performans göstergelerinin uygulamanızı istenen kullanıcı deneyimini sunmak için kritik olan tanımlamanız gerekir. Örneğin, yüksek IOPS için bir saniyede milyonlarca işlem işleme OLTP uygulamalar için en önemli. Oysa yüksek verimlilik büyük miktarlarda verinin bir saniyede işleme veri ambarı uygulamalar için önemlidir. Son derece düşük gecikme süresi, Web siteleri akış canlı video gibi gerçek zamanlı uygulamalar için önemlidir.

Ardından, uygulamanızın yaşam süresi boyunca en yüksek performans gereksinimlerini ölçer. Aşağıdaki örnek denetim başlangıç kullanın. En yüksek performans gereksinimlerini normal sırasında en yüksek ve yoğun olmayan saatlerde iş yükü nokta kaydedin. Tüm iş yükleri düzeyleri için gereksinimleri tanımlayarak, uygulamanızın genel performans gereksinimi belirlemek mümkün olacaktır. Örneğin, bir e-ticaret Web sitesi normal iş yükünü bir yıl ile çoğu günlerde hizmet işlemleri olacaktır. Web sitesinin en yüksek iş yükü tatilde veya özel satış olayları sırasında hizmet işlemleri olacaktır. En yüksek iş yükü sınırlı bir süre için genellikle deneyimli olmakla birlikte, normal işlem iki veya daha fazla kez ölçeklendirmek için uygulamanızı gerektirebilir. 50. Yüzdeliğini, 90 yüzdebirlik ve 99 yüzdebirlik gereksinimleri öğrenin. Bu performans gereksinimleri herhangi aykırı değerlerini filtrelemenize yardımcı olur ve doğru değerleri için en iyi duruma getirme üzerinde çabalarınız odaklanabilirsiniz.

**Uygulama Performans gereksinimlerini denetim listesi**

| **Performans gereksinimleri** | **50. Yüzdeliğini** | **90 yüzdebirlik** | **99 yüzdebirlik** |
| --- | --- | --- | --- |
| En çok, Saniye başına işlem | | | |
| % Okuma işlemleri | | | |
| % Yazma işlemleri | | | |
| % Rastgele işlemleri | | | |
| % Sıralı işlemleri | | | |
| G/ç isteği boyutu | | | |
| Ortalama işleme | | | |
| En çok, Aktarım hızı | | | |
| Min. Gecikme süresi | | | |
| Ortalama gecikme süresi | | | |
| En çok, CPU | | | |
| Ortalama CPU | | | |
| En çok, Bellek | | | |
| Ortalama bellek | | | |
| Sıra Derinliği | | | |

> [!NOTE]
> Beklenen gelişmeye uygulamanızın üzerinde göre bu sayı ölçeklendirme göz önünde bulundurmalısınız. Daha sonra performansı artırmak için altyapı değiştirmek için daha zor olabileceğinden büyüme, önceden planlamak için iyi bir fikirdir.
>
>

Varolan bir uygulamaya sahip ve Premium depolama birimine taşımak istiyorsanız, önce varolan uygulama için yukarıda bir denetim listesi oluşturun. Ardından, Premium depolama, uygulamanızda prototipi oluşturmak ve açıklanan yönergeleri göre uygulama tasarım *uygulama performansı en iyi duruma getirme* bu belgenin sonraki bölümünde. Sonraki bölümde performans ölçümleri toplamak için kullanabileceğiniz araçları açıklamaktadır.

Bir denetim listesi, varolan uygulamanız için prototip benzer oluşturun. Benchmarking araçlarını kullanarak iş yüklerini benzetimini ve prototip uygulama performansına ölçebilirsiniz. Bölümüne bakarak [Benchmarking](#benchmarking) daha fazla bilgi için. Premium depolama veya eşleşen aşan uygulama performans gereksinimlerinizi belirlemek için yaparak. Ardından, üretim uygulamanız için aynı yönergeleri uygulayabilirsiniz.

### <a name="counters-to-measure-application-performance-requirements"></a>Uygulama performansı gereksinimlerini ölçmek için sayaçları
Performans gereksinimlerini, uygulamanızın ölçmek için en iyi yoludur işletim sistemi tarafından sağlanan performans izleme araçları kullanmak için. Linux için Windows için PerfMon ve iostat kullanabilirsiniz. Bu araçlar yukarıdaki bölümde açıklanan her ölçü karşılık gelen sayaçları yakalayın. Uygulamanızı çalışırken, normal, en yüksek ve yoğun olmayan saatlerde iş yükleri Bu sayaçlar değerlerini yakalama gerekir.

PerfMon sayaçları işlemci, bellek ve her mantıksal disk ve fiziksel disk sunucunuzun için kullanılabilir. Premium depolama diskleri bir VM ile kullandığınızda, fiziksel disk sayaçları için her premium depolama diski, ve mantıksal disk sayaçları premium depolama disklerde oluşturulan her birim için. Uygulama İş yükünüzün konak disklerin değerlerini yakalama gerekir. Mantıksal ve fiziksel diskler arasında bire bir eşleme varsa, fiziksel disk sayaçları başvurabilir; Aksi takdirde mantıksal disk sayaçları başvurun. Linux üzerinde iostat komutu CPU ve disk kullanımı raporu oluşturur. Disk kullanımı raporu fiziksel aygıt veya bölüm başına istatistikler sağlar. Bir veritabanı sunucusu, veri ve günlük ile ayrı disklerde varsa, her iki diskin için bu verileri toplayın. Aşağıdaki tablo diskler, işlemci ve bellek sayaçlarını açıklanmaktadır:

| Sayaç | Açıklama | PerfMon | Iostat |
| --- | --- | --- | --- |
| **IOPS veya saniye başına işlem** |Saniye başına depolama disk verilen g/ç istek sayısı. |Disk Okuma/sn <br> Disk Yazma/sn |TP'leri <br> r/s <br> w/s |
| **Disk okuma ve yazma işlemleri** |% Okuma ve yazma disk üzerinde gerçekleştirilen işlemler. |% Disk okuma süresi <br> % Disk yazma süresi |r/s <br> w/s |
| **Aktarım hızı** |Okunan veya diske saniye başına yazılan veri miktarı. |Disk Okuma Bayt/sn <br> Disk Yazma Bayt/sn |kB_read/s <br> kB_wrtn/s |
| **Gecikme süresi** |Bir disk g/ç isteği tamamlamak için toplam süre. |Ortalama Disk sn/Okuma <br> Ortalama disk sn/yazma |await <br> svctm |
| **G/ç boyutu** |G/ç boyutu, depolama diskleri sorunları ister. |Ortalama Disk bayt/okuma <br> Ortalama Disk Bayt/yazma |avgrq sz |
| **Sıra Derinliği** |Formu okuma veya depolama diske yazılan üzere bekleyen istekleri bekleyen g/ç sayısı. |Geçerli Disk Sırası Uzunluğu |avgqu sz |
| **Maks. Bellek** |Uygulamanın düzgün çalışması için gerekli bellek miktarı |% Kullanılan kaydedilmiş bayt |Vmstat kullanın |
| **Maks. CPU** |Uygulamanın düzgün çalışması için gerekli CPU tutar |% İşlemci zamanı |% kul |

Daha fazla bilgi edinmek [iostat](https://linux.die.net/man/1/iostat) ve [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).

## <a name="optimizing-application-performance"></a>Uygulama performansı en iyi duruma getirme
Premium depolama üzerinde çalışan bir uygulama performansını etkileyen ana faktörler yapısı, g/ç isteği, VM boyutunu, Disk boyutu, diskler, Disk önbelleğe alma, çoklu iş parçacığı kullanımı ve sıra derinliği sayısı adı verilir. Bu etkenler bazıları, sistem tarafından sağlanan düğmelerini ile denetleyebilirsiniz. Çoğu uygulama sıra derinliği ve g/ç boyutu doğrudan değiştirmek için bir seçenek vermeyebilir. Örneğin, SQL Server kullanıyorsanız, g/ç boyutu ve sıra derinliği seçemezsiniz. SQL Server çoğu performans almak için en iyi g/ç boyutu ve sıra derinliği değerleri seçer. Böylece performans gereksinimlerini karşılamak için uygun kaynaklara sağlayabilirsiniz Etkenler her iki tür, uygulama performansı üzerindeki etkisini anlamak önemlidir.

Bu bölümde, uygulama performansını iyileştirmek ne kadar gerektiğini belirlemek için oluşturduğunuz uygulama gereksinimleri denetim listesine bakın. Üzerinde bağlı olarak, bu bölümdeki hangi Etkenler ince ayar gerekecektir belirlemek mümkün olacaktır. Uygulama performansı her faktörü etkilerini tanığı için uygulama Kurulum Kıyaslama araçları çalıştırın. Başvurmak [Benchmarking](#Benchmarking) Windows ve Linux VM'ler ortak Kıyaslama araçları çalıştırmak için adımları için bu makalenin sonunda bölüm.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>IOPS, üretilen iş ve gecikme süresi bir bakışta en iyi duruma getirme
Aşağıdaki tabloda tüm performans Etkenler ve IOPS, üretilen iş ve gecikme süresi en iyi duruma getirmek için gereken adımları özetler. Bu Özet aşağıdaki bölümler her anlatmaktadır çok daha kapsamlı bir faktördür.

| &nbsp; | **IOPS** | **Aktarım hızı** | **Gecikme süresi** |
| --- | --- | --- | --- |
| **Örnek senaryo** |İkinci oranı başına çok yüksek işlem gerektiren Kurumsal OLTP uygulama. |Kurumsal veri uygulama işleme büyük miktarlarda veri ambarı. |Çevrimiçi oyun gibi kullanıcı isteklerine anlık yanıtlar gerektiren gerçek zamanlı uygulamaları. |
| Performans Etkenler | &nbsp; | &nbsp; | &nbsp; |
| **G/ç boyutu** |Küçük g/ç boyutu, daha yüksek IOPS ortaya çıkarır. |Daha büyük g/ç boyutu için daha yüksek verimlilik ortaya çıkarır. | &nbsp;|
| **VM boyutu** |Uygulama gereksiniminden daha büyük IOPS sağlayan bir VM boyutu kullanın. VM boyutları ve bunların IOPS sınırlarını buraya bakın. |Uygulama gereksiniminden daha büyük verimi sınırına sahip bir VM boyutu kullanın. VM boyutları ve bunların işleme sınırları buraya bakın. |Teklifler sınırları uygulama gereksiniminden daha büyük ölçeklendirme bir VM boyutu kullanın. VM boyutları ve bunların sınırlarını buraya bakın. |
| **Disk boyutu** |Uygulama gereksiniminden daha büyük IOPS sunan bir disk boyutu kullanın. Disk boyutları ve bunların IOPS sınırlarını buraya bakın. |Üretilen iş sınırı uygulama gereksiniminden daha büyük bir disk boyutu kullanın. Disk boyutları ve bunların işleme sınırları buraya bakın. |Teklifler sınırları uygulama gereksiniminden daha büyük ölçeklendirme bir disk boyutu kullanın. Disk boyutları ve bunların sınırlarını buraya bakın. |
| **VM ve Disk ölçek sınırları** |Seçilen VM boyutu IOPS sınırı bağlı premium depolama disk tarafından yönlendirilen toplam IOPS değerinden büyük olmalıdır. |Seçilen VM boyutu işleme sınırını bağlı premium depolama disk tarafından yönlendirilen toplam verimlilik daha büyük olmalıdır. |Seçilen VM boyutu ölçeği sınırları ekli premium depolama diskleri toplam ölçek sınırlarını büyük olmalıdır. |
| **Disk önbelleği** |Premium depolama disklerde daha yüksek okuma IOPS almak için okuma yoğun işlemleri ile salt okunur önbellek etkinleştirin. | &nbsp; |Premium depolama disklerde gecikmeleri çok düşük okuma almaya hazır yoğun işlemlerin salt okunur önbellek etkinleştirin. |
| **Disk şeritleme** |Birden çok disk kullanın ve bunları birlikte bir birleşik yüksek IOPS ve üretilen iş sınırı almak için şeritler. VM başına birleşik sınırı bağlı premium disklerin toplam sınırlardan daha yüksek olması gerektiğini unutmayın. | &nbsp; | &nbsp; |
| **Şerit boyutu** |OLTP uygulamalarda görülen rastgele küçük g/ç deseni için daha küçük stripe boyutu. Örneğin, SQL Server OLTP uygulama için Şerit boyutu 64 KB kullanın. |Veri ambarı uygulamalarda görülen sıralı büyük g/ç düzeni büyük stripe boyutu. Örneğin, SQL Server veri ambarı uygulaması için 256 KB Şerit boyutu kullanın. | &nbsp; |
| **Çoklu iş parçacığı kullanımı** |Çoklu iş parçacığı yüksek IOPS ve üretilen iş götürür Premium depolama alanına daha yüksek sayıda istekleri göndermek için kullanın. Örneğin, SQL Server'da SQL Server için daha fazla CPU ayırmak için yüksek bir MAXDOP değer ayarlayın. | &nbsp; | &nbsp; |
| **Sıra Derinliği** |Daha büyük sıra derinliği daha yüksek IOPS sağlar. |Daha büyük sıra derinliği daha yüksek verimlilik verir. |Daha küçük sıra derinliği düşük gecikme verir. |

## <a name="nature-of-io-requests"></a>G/ç istekleri yapısı
Bir g/ç isteği, uygulamanızın gerçekleştirme giriş/çıkış işlem birimidir. G/ç istekleri, rastgele veya sıralı, doğası tanımlayan okuma veya yazma, küçük veya büyük, uygulamanızın performansı gereksinimlerini belirlemenize yardımcı olur. Doğru uygulama altyapınızı tasarlarken kararlar almak için g/ç istekleri yapısını anlamak çok önemlidir.

G/ç boyutu daha önemli faktörler biridir. G/ç boyutu, uygulamanız tarafından oluşturulan giriş/çıkış işlem isteği boyutudur. G/ç boyutu, özellikle IOPS ve uygulama elde edebilirsiniz olan bant genişliği üzerinde performansı önemli bir etkisi vardır. Aşağıdaki formülü IOPS, arasındaki ilişkiyi gösterir g/ç boyutu ve bant genişliği/işleme.  
    ![](media/premium-storage-performance/image1.png)

Bazı uygulamalar, bazı uygulamalar desteklerken, onların g/ç boyutu, alter olanak tanır. Örneğin, SQL Server en iyi g/ç boyutu kendisini belirler ve kullanıcıların değiştirmek için tüm düğmelerini sağlamaz. Diğer taraftan, Oracle adlı bir parametre sağlar [DB\_ENGELLEME\_BOYUTU](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) yapılandırabileceğiniz kullandığı veritabanının g/ç isteği boyutu.

G/ç boyutu değiştirmenize izin vermez, bir uygulama kullanıyorsanız, uygulamanız için en uygun KPI performansını iyileştirmek için bu makaledeki yönergeleri kullanın. Örneğin,

* OLTP uygulamanın milyonlarca küçük ve rastgele g/ç istek oluşturur. Bu tür bir g/ç istekleri işlemek için daha yüksek IOPS almak için uygulama altyapınızı tasarlamanız gerekir.  
* Bir veri ambarı uygulama büyük ve sıralı g/ç istekleri oluşturur. Bu tür bir g/ç istekleri işlemek için daha yüksek bant genişliği veya işleme almak için uygulama altyapınızı tasarlamanız gerekir.

G/ç boyutu değiştirmenize izin verir, bir uygulama kullanıyorsanız, diğer performans kuralları ek olarak g/ç boyutu bu altın kural kullanın,

* Daha yüksek IOPS almak için daha küçük g/ç boyutu. Örneğin, bir OLTP uygulama için 8 KB.  
* Daha yüksek bant genişliği verimliliği almak için daha büyük g/ç boyutu. Örneğin, 1024 KB bir veri ambarı uygulaması için.

İşte bir örnek nasıl, IOPS ve üretilen iş/bant genişliği, uygulamanız için hesaplayabilirsiniz üzerinde. P30 disk kullanarak bir uygulamayı göz önünde bulundurun. En fazla IOPS ve üretilen iş/bant genişliği P30 disk elde edebilirsiniz saniyede sırasıyla 5000 IOP ve 200 MB olabilir. Şimdi, maksimum IOPS P30 diskten uygulamanızın gerektirdiği ve daha küçük bir g/ç boyutu 8 KB gibi kullanırsanız, alma kuramaz elde edilen bant genişliği 40 saniye başına MB'dir. Ancak, en yüksek verimlilik/bant genişliği P30 diskten uygulamanızın gerektirdiği ve daha büyük bir g/ç boyutu 1024 KB gibi kullanıyorsanız, sonuçta elde edilen IOPS daha az olacaktır 200 IOPS. Bu nedenle, her iki, uygulamanızın IOPS ve üretilen iş/bant genişliği gereksinimleri karşıladığından emin GÇ boyutunu ayarlayın. Aşağıdaki tabloda farklı GÇ boyutları ve bunların karşılık gelen IOPS ve üretilen iş için bir P30 disk özetler.

| Uygulama dağıtımı gereksinimi | G/ç boyutu | IOPS | Üretilen iş/bant genişliği |
| --- | --- | --- | --- |
| Maks. IOPS |8 KB |5.000 |Saniye başına 40 MB |
| En fazla üretilen işi |1.024 KB |200 |200 MB / saniye |
| En büyük verimi + yüksek IOPS |64 KB |3,200 |200 MB / saniye |
| Maksimum IOPS + yüksek verimlilik |32 KB |5.000 |Saniye başına 160 MB |

IOPS ve tek premium depolama diskini en büyük değerden daha yüksek bant genişliği elde etmek için birlikte şeritli birden çok premium diskleri kullanın. Örneğin, bir birleşik IOPS 10.000 IOPS sayısı veya 400 MB birleşik verimini saniyede almak için iki P30 disk şeritler. Sonraki bölümde açıklandığı gibi birleştirilmiş destekleyen bir VM boyutu kullanmalısınız IOPS ve üretilen iş disk.

> [!NOTE]
> IOPS veya diğer da artırır işleme arttıkça, üretilen iş ya da disk veya VM IOPS sınırları herhangi birini artan zaman yaşamadığı emin olun.
>
>

Uygulama performansı g/ç boyutu etkilerini tanığı için VM ve diskleri Kıyaslama araçları çalıştırabilirsiniz. Birden çok test çalışmaları oluşturma ve etkisini görmek için her çalışma için farklı g/ç boyutu kullanın. Başvurmak [Benchmarking](#Benchmarking) daha fazla ayrıntı için bu makalenin sonunda bölüm.

## <a name="high-scale-vm-sizes"></a>Yüksek ölçekli VM boyutları
Bir uygulama tasarlama başlattığınızda yapmak için ilk şey, uygulamanızı barındırmak için bir VM seçin biridir. Premium depolama daha yüksek işlem gücü ve yüksek yerel disk g/ç performansı gerektiren uygulamaların yüksek ölçek VM boyutları ile birlikte gelir. Bu sanal makineleri yerel disk için daha hızlı işlemcilerde, daha yüksek bellek çekirdek oranı ve bir Solid-State sürücüsü (SSD) sağlayın. DS, DSv2 ve GS serisi VM'ler yüksek ölçek Premium Storage destekleyen VM'ler örnekleridir.

Yüksek ölçekli VM'ler farklı boyutlarda farklı sayıda CPU çekirdekleri, bellek, işletim sistemi ve geçici disk boyutu ile kullanılabilir. Her VM boyutu en fazla VM iliştirebilirsiniz veri diski sayısı da sahiptir. Bu nedenle, seçilen VM boyutu ne kadar bellek, işleme etkiler ve depolama kapasitesi, uygulamanız için kullanılabilir. İşlem de etkiler ve depolama maliyeti. Örneğin, aşağıda bir DS serisi, DSv2 serisi ve GS serisi en büyük VM boyutu belirtimleri şunlardır:

| VM boyutu | CPU çekirdekleri | Bellek | VM disk boyutları | En çok, Veri diskleri | Önbellek boyutu | IOPS | Bant genişliği önbellek g/ç sınırları |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |İŞLETİM SİSTEMİ = 1023 GB <br> Yerel SSD 224 GB = |32 |576 GB |50.000 IOPS <br> Saniye başına 512 MB |4.000 IOPS ve saniye başına 33 MB |
| Standard_GS5 |32 |448 GB |İŞLETİM SİSTEMİ = 1023 GB <br> Yerel SSD 896 GB = |64 |4224 GB |80.000 IOPS <br> Saniye başına 2.000 MB |5000 IOP ve saniyede 50 MB |

Tüm kullanılabilir Azure VM boyutlarını tam listesini görmek için bkz [Windows VM boyutları](../articles/virtual-machines/windows/sizes.md) veya [Linux VM boyutları](../articles/virtual-machines/linux/sizes.md). Karşılamak ve istenen uygulama performans gereksinimlerinizi ölçeklendirme bir VM boyutu seçin. Buna ek olarak, VM boyutlarını seçerken önemli noktalar aşağıdaki dikkate alın.

*Ölçek sınırları*  
VM başına ve disk başına maksimum IOPS sınırları farklı ve birbirlerinden bağımsız. Uygulama, kendisine bağlı premium disklerin yanı sıra VM sınırları içinde IOPS yürüten emin olun. Aksi takdirde, uygulama performansı azaltma karşılaşırsınız.

Örnek olarak, en çok 4.000 IOPS bir uygulama dağıtımı gereksinimi olduğunu varsayın. Bunun için DS1 VM P30 diskte sağlayın. P30 disk en fazla 5000 IOP sunabilir. Bununla birlikte, DS1 VM 3,200 IOPS sınırlıdır. Sonuç olarak, uygulama performansı 3,200 IOPS adresindeki VM sınırına tarafından kısıtlı ve performans olacaktır. Bu durumu önlemek için her ikisini de olacak bir diski ve VM boyutu uygulama gereksinimlerini karşılayan'ı seçin.

*İşlemi maliyeti*  
Çoğu durumda, Premium depolama ile işlemi, genel maliyeti standart depolama kullanmaktan daha düşük olduğunu mümkündür.

Örneğin, 16.000 IOPS gerektiren bir uygulama göz önünde bulundurun. Bu performans elde etmek için bir standart gerekir\_16.000 32 standart depolama 1 TB diskleri kullanan bir maksimum IOPS verebilirsiniz D14 Azure Iaas sanal. Her 1TB standart depolama diskini en fazla 500 IOPS elde edebilirsiniz. Bu VM aylık tahmini maliyeti $1,570 olacaktır. Aylık maliyeti 32 standart depolama $1,638 olacaktır. Tahmini aylık toplam maliyetini $3,208 olacaktır.

Ancak, aynı uygulama Premium depolama üzerinde barındırılıyorsa, daha küçük bir VM boyutu ve daha az premium depolama diskleri, böylece ilişkin genel maliyeti azaltır gerekir. Standart bir\_DS13 VM dört P30 diskler kullanarak 16.000 IOPS gereksinimi karşılamak. DS13 VM 25,600 bir maksimum IOPS ve her P30 disk maksimum IOPS 5.000, sahiptir. Genel olarak, bu yapılandırma 5.000 x 4 = 20.000 elde edebilirsiniz IOPS. Bu VM aylık tahmini maliyeti $1,003 olacaktır. Dört P30 premium depolama diskleri aylık maliyeti $544.34 olacaktır. Tahmini aylık toplam maliyetini $1,544 olacaktır.

Aşağıdaki tabloda, standart ve Premium depolama için Maliyet dökümü bu senaryonun özetler.

| &nbsp; | **Standart** | **Premium** |
| --- | --- | --- |
| **VM maliyet aylık** |$1,570.58 (standart\_D14) |$1,003.66 (standart\_DS13) |
| **Aylık maliyeti** |$1,638.40 (32 x 1 TB disk) |$544.34 (4 x P30 diskler) |
| **Aylık genel maliyeti** |$3,208.98 |$1,544.34 |

*Linux Distro'lar*  

Azure Premium Storage ile aynı düzeyde performans VM'ler için Windows ve Linux çalıştıran alın. Linux distro'lar birçok türdeki destekliyoruz ve tam listesine bakın [burada](../articles/virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Farklı distro'lar farklı türlerde iş yükleri için daha uygun olduğunu dikkate almak önemlidir. Performans, iş yükü çalıştıran distro bağlı olarak farklı düzeylerde görürsünüz. Uygulamanız ile Linux distro'lar test edin ve en iyi olanı seçin.

Linux Premium Storage ile çalışırken, yüksek performans sağlamak için gerekli sürücüleri hakkında en son güncelleştirmeleri denetleyin.

## <a name="premium-storage-disk-sizes"></a>Premium depolama Disk boyutları
Azure Premium Storage yedi disk boyutları şu anda sunar. Her disk boyutu IOPS, bant genişliği ve depolama için bir farklı ölçek sınırı vardır. Sağ uygulama gereksinimleri ve yüksek ölçekli VM boyutuna bağlı olarak Premium depolama Disk boyutu seçin. Aşağıdaki tabloda yedi disk boyutları ve yeteneklerini gösterir. P4 ve P6 boyutları: şu anda yalnızca yönetilen diskler için desteklenir.

| Premium diskler türü  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Disk boyutu           | 32 GB | 64 GB | 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Disk başına IOPS       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Disk başına aktarım hızı | Saniye başına 25 MB  | Saniye başına 50 MB  | Saniyede 100 MB | 150 MB / saniye | 200 MB / saniye | Saniye başına 250 MB | Saniye başına 250 MB | 


Seçilen disk üzerinde bağlıdır seçtiğiniz kaç tane disk boyutu. Uygulama dağıtımı gereksinimi karşılamak için tek bir P50 disk veya birden çok P10 disk kullanabilirsiniz. Seçim yaparken, aşağıda listelenen hesabında dikkate alınacak noktalar alın.

*Ölçek sınırları (IOPS ve üretilen iş)*  
Her Premium disk boyutu IOPS ve üretilen iş sınırları VM ölçek sınırlamaları uygulanmaya farklı ve bağımsız olur. Toplam IOPS ve üretilen iş disklerden seçilen VM boyutu ölçeği sınırlarda olduğundan emin olun.

Örneğin, en fazla 250 MB/sn üretilen iş bir uygulama gereksinimdir ve tek bir P30 diske sahip DS4 VM kullanıyorsanız. DS4 VM en fazla 256 MB/sn üretilen iş verebilirsiniz. Ancak, tek bir P30 disk 200 MB/sn üretilen iş sınırı vardır. Sonuç olarak, uygulama 200 MB/sn disk sınırı nedeniyle, kısıtlı. Bu sınır aşmak için birden fazla veri diski VM'ye sağlamak veya disklerinizi P40 veya P50 yeniden boyutlandırın.

> [!NOTE]
> Okuma önbelleği tarafından sunulan disk IOPS ve üretilen işi, bu nedenle disk sınırları için değil konu dahil edilmez. Önbellek VM başına ayrı kendi IOPS ve üretilen iş sınırı vardır.
>
> Örneğin, başlangıçta okuma ve yazma işlemleri 60 MB/sn ve 40 MB/sn sırasıyla ' dir. Zaman içerisinde, önbellek warms ve daha fazla bilgi ve daha fazla okuma önbellekten görev yapar. Ardından, diskten daha yüksek yazma verimlilik elde edebilirsiniz.
>
>

*Disk sayısı*  
Uygulama gereksinimleri değerlendirerek gerekir disk sayısını belirler. Her VM boyutu sınırı VM'e ekleyin diskleri sayısı da sahiptir. Genellikle, bu iki kez çekirdek sayısıdır. Seçtiğiniz VM boyutu gerekli disk sayısını desteklediğinden emin olun.

Premium depolama diskleri için standart depolama diskleri karşılaştırıldığında daha yüksek performans özelliklerine sahip unutmayın. Bu nedenle, uygulamanızı Azure Iaas standart depolama Premium depolama alanına kullanarak VM geçiriyorsanız, uygulamanız için aynı veya daha yüksek performans elde etmek için daha az premium diskleri olasılıkla gerekir.

## <a name="disk-caching"></a>Disk önbelleği
Azure Premium Storage yararlanan yüksek ölçek VM'ler BlobCache adlı bir çok katmanlı önbelleğe alma teknoloji vardır. BlobCache önbelleğe alma işlemi için sanal makine RAM ve yerel SSD bileşimini kullanır. Bu önbellek, Premium depolama kalıcı diskleri ve VM yerel diskler için kullanılabilir. Varsayılan olarak, bu önbellek ayarı okuma/yazma için işletim sistemi diskleri ve salt okunur için Premium depolama alanında barındırılan veri diskleri için ayarlanır. Premium depolama disklerde etkin disk önbelleğe alma ile yüksek ölçekli VM'ler temel disk performansı aşan son derece yüksek düzeyde performans elde edebilirsiniz.

> [!WARNING]
> Azure disk önbellek ayarını değiştirme ayırır ve hedef disk yeniden iliştirir. İşletim sistemi diski ise, VM yeniden başlatılır. Tüm uygulamalar/disk önbellek ayarını değiştirmeden önce bu kesintisi tarafından etkilenebilecek hizmetlerini durdurun.
>
>

BlobCache nasıl çalıştığı hakkında daha fazla bilgi için iç başvuran [Azure Premium Storage](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blog postası.

Diskleri doğru yetenek kümesini önbelleğini etkinleştirmek önemlidir. Premium disk üzerinde disk önbelleği etkinleştirmelisiniz mı iş yükü desenine bu diski bağımlı değil işleme. Aşağıdaki tabloda, varsayılan işletim sistemi ve veri diskleri için önbellek ayarlarını gösterir.

| **Disk türü** | **Varsayılan önbellek ayarı** |
| --- | --- |
| İşletim sistemi diski |ReadWrite |
| Veri diski |None |

Veri diskleri için önerilen disk önbellek ayarlarını şunlardır,

| **Önbelleğe alma ayarını disk** | **Bu ayarı kullanmak ne zaman öneriye** |
| --- | --- |
| None |Hiçbiri salt yazılır ve ağır yazma diskleri için konak önbelleği yapılandırın. |
| salt okunur |Konak önbelleği salt okunur ve okuma-yazma diskler için salt okunur olarak yapılandırın. |
| ReadWrite |Yalnızca uygulamanızı düzgün bir şekilde gerektiğinde kalıcı disklere önbelleğe alınmış veri yazma işliyorsa konak önbelleği ReadWrite yapılandırın. |

*salt okunur*  
Premium depolama verileri önbelleğe alma diskleri ReadOnly yapılandırarak, düşük okuma gecikme süresine ulaşmanız ve çok yüksek okuma IOPS ve üretilen iş uygulamanız için alın. İki nedeni budur,

1. Okuma VM belleği ve yerel SSD önbelleğinden gerçekleştirilen, Azure blob depolama veri diskten okuma hızlıdır.  
2. Premium depolama IOPS disk doğrultusunda önbellek ve üretilen iş sunulan okumaları sayılmaz. Bu nedenle, uygulamanızı daha yüksek toplam IOPS ve üretilen iş elde edebilirsiniz.

*ReadWrite*  
Varsayılan olarak, işletim sistemi disk ReadWrite önbelleğe alma etkin sahiptir. Yakın zamanda ReadWrite diskleri yanında verileri önbelleğe alma desteği ekledik. Önbelleğe alma ReadWrite kullanıyorsanız, kalıcı disklere önbellekten veri yazmak için uygun bir yol olması gerekir. Örneğin, SQL Server önbelleğe alınan veriler kalıcı depolama disklere, kendi yazma işler. VM çökerse uygulamayla gerekli verileri kalıcı işlemiyor ReadWrite önbelleği kullanılarak veri kaybına yol açabilir.

Örnek olarak, aşağıdakileri yaparak Premium depolama üzerinde çalışan SQL Server için bu yönergeleri uygulayabilirsiniz

1. "Salt okunur" önbelleği premium depolama disklerde veri dosyalarını barındıran yapılandırın.  
   a.  Hızlı veri sayfaları çok daha hızlı önbellekten alınır olduğundan SQL Server sorgusu süresi verileri doğrudan diskler ile karşılaştırıldığında daha düşük önbellekten okur.  
   b.  Okuma önbelleğinden hizmet veren, ek işleme premium veri disklerinden kullanılabilir anlamına gelir. SQL Server daha fazla veri sayfaları ve diğer işlemleri yedekleme/geri yükleme gibi alma doğrultusunda bu ek üretilen iş kullanın, toplu iş yükleri ve dizin oluşturur.  
2. Yapılandırma "Hiçbiri" premium depolama disklerde günlük dosyalarını barındıran önbelleğe.  
   a.  Günlük dosyaları öncelikle ağır yazma işlemleri sahiptir. Bu nedenle, salt okunur önbellekten faydalanmaz.

## <a name="disk-striping"></a>Disk şeritleme
VM birkaç premium depolama kalıcı disklerle bağlı olduğu büyük ölçekli, diskleri birlikte bunların IOPS, bant genişliği ve depolama kapasitesini toplamak için şeritli.

Windows, depolama alanları stripe disklere birlikte kullanabilirsiniz. Bir havuzda her disk için bir sütun yapılandırmanız gerekir. Aksi takdirde, şeritli birim genel performansını disklere trafik düzensiz dağıtım nedeniyle beklenenden daha düşük olabilir.

Önemli: Sunucu Yöneticisi kullanıcı Arabirimi kullanarak, toplam şeritli birim için 8 adede kadar sütun sayısı ayarlayabilirsiniz. Birden fazla 8 disk eklerken birim oluşturmak için PowerShell kullanın. PowerShell kullanarak, sütun sayısı eşit disk sayısını ayarlayabilirsiniz. Örneğin, tek kümesi 16 diskler varsa; 16 sütunlarında belirtin *NumberOfColumns* parametresinin *New-VirtualDisk* PowerShell cmdlet'i.

Linux üzerinde birlikte stripe disklere MDADM yardımcı programını kullanın. Ayrıntılı adımlar Linux şeritleme disklerde başvurmak için [yapılandırma yazılım RAID Linux'ta](../articles/virtual-machines/linux/configure-raid.md).

*Şerit boyutu*  
Disk şeritleme önemli bir yapılandırmada stripe boyutudur. Şerit boyutu veya blok boyutu en küçük öbek üzerinde şeritli birim uygulama gideren veri değil. Yapılandırdığınız Şerit boyutu uygulama ve onun istek düzeni türüne bağlıdır. Yanlış Şerit boyutu seçerseniz, bu, uygulamanızın performans için müşteri adayları GÇ uyuşmazlığın neden olabilir.

Uygulamanız tarafından oluşturulan bir g/ç isteği disk stripe boyutundan daha büyükse Örneğin, depolama sistemi stripe birim sınırlarında birden çok diskte yazar. Bu verilere erişmek için zaman olduğunda, isteği tamamlamak için birden fazla şeritli birimler arasında arama gerekecektir. Bu tür davranış toplu etkisini önemli ölçüde performans düşüşüne neden olabilir. Diğer taraftan, g/ç isteği boyutu stripe boyutundan daha küçükse ve doğası gereği rastgele ise, g/ç istekleri bir performans sorununa yol açtığını ve sonuçta g/ç performansı önemli aynı disk üzerinde ekleyebilir.

Uygulamanızı çalıştıran iş yükü türüne bağlı olarak, uygun stripe boyutu seçin. Rastgele küçük g/ç istekleri için daha küçük bir Şerit boyutu kullanın. Oysa için büyük sıralı g/ç istekleri daha büyük bir Şerit boyutu kullanır. Premium depolama üzerinde çalışan stripe boyutu uygulaması için önerileri öğrenin. SQL Server için Şerit boyutu 64 KB OLTP iş yükleri için veri ambarı iş yükleri için 256 KB ve yapılandırın. Bkz: [Azure Vm'lerde SQL Server için performans en iyi uygulamaları](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md#disks-guidance) daha fazla bilgi için.

> [!NOTE]
> En fazla 32 premium depolama diskleri DS serisi VM üzerinde ve GS serisi VM 64 premium depolama disklerde birlikte şeritler.
>
>

## <a name="multi-threading"></a>Çoklu iş parçacığı kullanımı
Azure Premium Storage platformu yüksek düzeyde paralel olacak şekilde tasarlanmıştır. Bu nedenle, çok iş parçacıklı bir uygulama, bir tek iş parçacıklı uygulaması daha çok yüksek performans elde eder. Çok iş parçacıklı bir uygulama görevlerinin birden çok iş parçacıkları arasında böler ve en fazla VM ve disk kaynakları yararlanarak yürütülmesinin verimliliğini artırır.

Örneğin, uygulamanızın tek bir çekirdek iki iş parçacığı kullanarak VM üzerinde çalışıyorsa, CPU verimliliği elde etmek için iki iş parçacıkları arasında geçiş yapabilirsiniz. Bir disk g/ç tamamlamak için bekleyen bir iş parçacığı olsa da, CPU'nun için başka bir iş parçacığı geçiş yapabilirsiniz. Bu şekilde, iki iş parçacığı tek bir iş parçacığı misiniz fazlasını gerçekleştirebilirsiniz. VM birden fazla çekirdek varsa, her çekirdek Paralel Görevler yürütebilir olduğundan daha fazla çalışma süresini azaltır.

Piyasada uygulamanın tek uygulayan şeklini değiştirmek mümkün olmayabilir iş parçacığı veya çoklu iş parçacığı kullanımı. Örneğin, SQL Server Çoklu CPU ve çok çekirdekli işleyebilir. Ancak, SQL Server hangi koşullar altında bir sorgu işlemek için bir veya daha fazla iş parçacığı özelliğinden yararlanır karar verir. Bu sorguları çalıştırmak ve çoklu iş parçacığı kullanan dizinleri oluşturun. Büyük tabloları birleştirme ve kullanıcıya geri dönmeden önce verileri sıralama içerir bir sorgu için SQL Server büyük olasılıkla birden çok iş parçacığı kullanır. Ancak, bir kullanıcı SQL Server tek bir iş parçacığı veya birden çok iş parçacığı kullanarak bir sorgu yürütüp yürütmediğini denetleyemezsiniz.

Çoklu iş parçacığı veya paralel bu etkilemek için alter yapılandırma ayarlarını bir uygulamayı işleme. Örneğin, SQL Server durumunda maksimum paralellik derecesi yapılandırma olur. Bu ayar MAXDOP, SQL Server paralel zaman kullanabilir işlemci en fazla sayısını yapılandırmanızı sağlar adlı işleniyor. Tek tek sorgular veya dizin işlemleri için MAXDOP yapılandırabilirsiniz. Sisteminizin bir performans kritik uygulama için kaynakları dengelemek istediğinizde faydalıdır.

Örneğin, SQL Server kullanıyorsanız, uygulamanızı bir büyük sorguyu ve aynı zamanda bir dizin işlemi yürütme söyleyin. Bize büyük sorguya karşılaştırıldığında daha fazla kullanıcı olması için dizin işlemi istediğinizi varsayalım. Böyle bir durumda, sorgu için MAXDOP değerden daha yüksek bir değere dizin işlemi MAXDOP değerini ayarlayabilirsiniz. Bu şekilde, SQL Server daha fazla büyük sorguya ayırabilirsiniz işlemci sayısını karşılaştırılan dizin işlemi için yararlanabilirsiniz işlemci sayısına sahip. Unutmayın, her işlem için SQL Server kullanacak iş parçacığı sayısını kontrol etmez. En fazla işlemci için ayrılmış sayısını kontrol edebilirsiniz çoklu iş parçacığı kullanımı.

Daha fazla bilgi edinmek [derece, paralellik](https://technet.microsoft.com/library/ms188611.aspx) SQL Server'daki. Etkileyen gibi ayarları performansını iyileştirmek için uygulamanızı ve yapılandırmalarına çoklu iş parçacığı bulmak.

## <a name="queue-depth"></a>Sıra Derinliği
Sıra Derinliği veya sırası uzunluğu veya sıra boyutu bekleyen g/ç istekleri sistemde sayısıdır. Sıra Derinliği değeri, uygulamanızın hangi depolama diskleri işlenmeye, satır kaç g/ç işlemleri belirler. Bu makale viz. içinde IOPS, üretilen iş ve gecikme konuştuğumuz tüm üç uygulama performans göstergeleri etkiler.

Sıra Derinliği ve çoklu iş parçacığı olan yakından ilgili. Sıra Derinliği değeri çoklu iş parçacığı ne kadar seçilebileceğini gösterir uygulama tarafından elde edilebilir. Sıra Derinliği büyükse, uygulama daha fazla işlem eşzamanlı olarak, diğer bir deyişle, yürütebilir birden çok iş parçacığı. Uygulamanın çok iş parçacıklı olsa bile sıra derinliği, küçükse, eş zamanlı yürütme için tabandaki yeterli istekleri sahip değil.

Genellikle, raf devre dışı uygulamalar sıra derinliği değiştirmenizi olduğundan izin verme durumunda yanlış bunu iyi'den daha fazla zarar ayarlayın. Uygulamaları en iyi performans almak için sıra derinliği sağdaki değerini ayarlar. Ancak, böylece uygulamanız ile performans sorunlarını giderebilirsiniz bu kavramı anlamak önemlidir. Sıra Derinliği etkilerini sisteminizde Kıyaslama Araçlar çalıştırarak da görebilirsiniz.

Bazı uygulamalar sıra derinliği etkilemek için ayarları sağlayın. Örneğin, SQL Server MAXDOP (maksimum paralellik derecesi) ayarında önceki bölümde açıklanmıştır. MAXDOP sıra derinliği etkilemek için bir yoldur ve çoklu iş parçacığı kullanımı, sıra derinliği değeri SQL Server'ın doğrudan değiştirmez ancak.

*Yüksek sıra derinliği*  
Disk üzerinde daha fazla işlemleri yukarı yüksek sıra derinliği satırları. Disk vaktinden kendi sırasındaki sonraki istek bilir. Sonuç olarak, disk işlemleri önceden zamanlayabilir ve en iyi bir sırayla işlem. Uygulama diske daha fazla isteği gönderme olduğundan, diskin daha fazla paralel IOs işleyebilir. Sonuç olarak, uygulama daha yüksek IOPS elde edebilirsiniz olacaktır. Uygulama daha fazla isteği işleme olduğundan, uygulamanın toplam verimlilik da artırır.

Genellikle, bir uygulama 8 ile en yüksek verimlilik elde edebilirsiniz-16 + bekleyen GÇ ekli disk başına. Sıra Derinliği ise, uygulama sistemde yeterli IOS dağıtmaya değil ve belirli bir dönemde daha az miktarda işleme. Diğer bir deyişle, daha az işleme.

Örneğin, SQL Server'da "4" için bir sorgu MAXDOP değeri ayarlanırken SQL Server, en fazla dört çekirdek sorguyu yürütmek için kullanabileceğiniz bildirir. SQL Server en iyi sıra derinliği değeri ve sorgu yürütmesi için çekirdek sayısı ne olduğunu belirler.

*En iyi sıra derinliği*  
Çok yüksek sıra derinliği değeri de kendi sakıncaları vardır. Sıra Derinliği değeri çok yüksekse, uygulamanın çok yüksek IOPS sürücü dener. Uygulama yeterli sağlanan IOPS kalıcı disklerle olmadığı sürece, bu uygulama gecikme sürelerini olumsuz yönde etkileyebilir. Formül aşağıdaki IOPS, gecikme ve sıra derinliği arasındaki ilişkiyi gösterir.  
    ![](media/premium-storage-performance/image6.png)

Yüksek bir değer, ancak uygulama için yeterli IOPS gecikmeleri etkilemeden sunabilir bir en iyi değeri için sıra derinliği yapılandırmamalısınız. Örneğin, uygulama gecikme süresi 1 milisaniyelik olması gerekiyorsa, sıra derinliği 5.000 IOPS elde etmek için gerekli olan QD = 5000 x 0,001 = 5.

*Şeritli birim için sıra derinliği*  
Her diski ayrı ayrı bir en yüksek sıra derinliği sahip olacağı şekilde, bir şeritli birim için yeterince yüksek sıra derinliğini korumak. Örneğin, bir sıra derinliği 2 iter uygulamanın göz önünde bulundurun ve Şerit içine 4 disk yok. İki g/ç isteği iki disklere geçer ve iki disk kalan boş olacaktır. Bu nedenle, tüm diskleri meşgul gibi sıra derinliği yapılandırın. Aşağıdaki formülü şeritli birimler sıra derinliğini belirlemek nasıl gösterir.  
    ![](media/premium-storage-performance/image7.png)

## <a name="throttling"></a>Azaltma
Azure Premium Storage hükümleri IOPS ve üretilen iş sayısı VM boyutları ve seçtiğiniz disk boyutları bağlı olarak belirtilen. Uygulamanızı IOPS veya üretilen işi ne tanıtıcısı VM veya disk bu sınırları sürücü dener verdiğinizde, Premium Storage bu kısıtlama. Bu, uygulamanızda performans biçiminde bildirimleri. Bu daha yüksek gecikme anlamına gelir, üretilen iş alt veya IOPS düşürün. Premium depolama kısıtlama değil, uygulamanızın tümüyle kaynaklarını elde özellikli nelerdir aşan başarısız olabilir. Bu nedenle, kısıtlama nedeniyle performans sorunlarını önlemek için her zaman, uygulamanız için yeterli kaynak sağlayın. Biz VM boyutları ve Disk boyutları bölümler yukarıda açıklanan dikkate alın. Değerlendirmesi, uygulamanızı barındırmak için ihtiyacınız olan kaynakları bulmak için en iyi yoludur.

## <a name="benchmarking"></a>Değerlendirmesi
Değerlendirmesi, farklı iş yükleri, uygulamanızın üzerinde benzetimini yapma ve her iş yükü için uygulama performansını ölçmek işlemidir. Bir önceki bölümde açıklanan adımları kullanarak, uygulama performans gereksinimlerini aldınız. Uygulamayı barındıran sanal makinelerin Kıyaslama Araçlar çalıştırarak uygulamanızı Premium Storage ile elde edebilirsiniz performans düzeylerini belirleyebilirsiniz. Bu bölümde, Azure Premium Storage diskler ile sağlanan standart bir DS14 VM değerlendirmesi örnekleri sunuyoruz.

Ortak Kıyaslama araçları Iometer ve FIO, Windows ve Linux için sırasıyla kullandık. Bu araçlar iş yükü gibi bir üretim benzetimi birden çok iş parçacığı oluşturma ve sistem performansının ölçebilirsiniz. Araçları kullanarak, bir uygulama için normalde değiştiremezsiniz blok boyutu ve sıra derinliğini gibi parametreleri de yapılandırabilirsiniz. Bu, yüksek ölçekte VM premium diskleri farklı türde uygulama iş yükleri için sağlanan en yüksek performans sürücü için daha fazla esneklik sağlar. Ziyaret Kıyaslama her aracı hakkında daha fazla bilgi edinmek için [Iometer](http://www.iometer.org/) ve [FIO](http://freecode.com/projects/fio).

Aşağıdaki örneklerde takip etmek için standart DS14 VM oluşturun ve 11 Premium depolama diskleri VM'e ekleyin. 11 disklerin NoCacheWrites adlı bir birim şeritler ve ana bilgisayar "None" önbelleğe almayı ile 10 diskleri yapılandırın. Ana bilgisayar "Salt okunur olarak" kalan disk üzerinde önbelleğe almayı yapılandırın ve bu diski CacheReads adlı bir birim oluşturun. Bu ayarı kullanarak, standart DS14 VM maksimum okuma ve yazma performansı görmeye olacaktır. Premium diskler ile DS14 VM oluşturma hakkında ayrıntılı adımlar için Git [oluşturma ve kullanma bir Premium depolama hesabı için sanal makine veri diski](../articles/virtual-machines/windows/premium-storage.md).

*Önbelleği ısıtma*  
Salt okunur ana bilgisayar önbelleğe almayı diskle disk sınırdan daha yüksek IOPS vermek olacaktır. İlk ana önbellekten bu maksimum okuma performansı almak için bu disk önbelleği sıcak gerekir. Bu, hangi Kıyaslama aracı CacheReads birimde yol açacak okuma IOs gerçekten önbellek ve diskin değil doğrudan isabetler olduğunu sağlar. Tek önbellek ek IOPS Önbelleği İsabetli Okuma sonucunda disk etkin.

> **Önemli:**  
> VM yeniden başlatılıncaya kadar her zaman değerlendirmesi, çalıştırmadan önce önbelleği sıcak gerekir.
>
>

#### <a name="iometer"></a>Iometer
[Iometer Aracı'nı indirme](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) VM üzerinde.

*Sınama dosyası*  
Iometer Kıyaslama test çalışacağı biriminde depolanan bir test dosyası kullanır. Okuma sürücüler ve IOPS ve üretilen iş disk ölçmek için bu test dosyaya yazar. Bir sağlamadınız değilse Iometer bu test dosyası oluşturur. İobw.tst CacheReads ve NoCacheWrites birimlerde adlı 200 GB test dosyası oluşturun.

*Access belirtimleri*  
Belirtimleri isteği % okuma/yazma g/ç boyutu, % rastgele/sıralı yapılandırılmış olan Iometer içinde "Access belirtimleri" sekmesini kullanarak. Aşağıda açıklanan senaryoların her biri için bir erişim belirtimi oluşturun. Access belirtimleri oluşturun ve "Kaydet" uygun bir ad gibi – RandomWrites\_8K, RandomReads\_8 K. Karşılık gelen belirtimini test senaryosu çalıştırırken seçin.

Access belirtimleri maksimum IOPS yazma senaryo örneği aşağıda gösterilen,  
    ![](media/premium-storage-performance/image8.png)

*Maksimum IOPS Test belirtimleri*  
Maksimum IOPS göstermek için daha küçük isteği boyutu kullanın. 8 K istek boyutu ve rastgele yazma ve okuma için belirtimler oluşturmak.

| Erişim belirtimi | İsteği boyutu | Rastgele % | Okuma % |
| --- | --- | --- | --- |
| RandomWrites\_8 K |8K |100 |0 |
| RandomReads\_8 K |8K |100 |100 |

*En yüksek verimlilik Test belirtimleri*  
En yüksek verimlilik göstermek için daha büyük isteği boyutu kullanın. 64 K istek boyutu ve rastgele yazma ve okuma için belirtimler oluşturmak.

| Erişim belirtimi | İsteği boyutu | Rastgele % | Okuma % |
| --- | --- | --- | --- |
| RandomWrites\_64 K |64K |100 |0 |
| RandomReads\_64 K |64K |100 |100 |

*Iometer testi çalıştırma*  
Önbelleği sıcak için aşağıdaki adımları gerçekleştirin

1. Aşağıda gösterilen değerleri içeren iki erişim belirtimleri oluşturun

   | Ad | İsteği boyutu | Rastgele % | Okuma % |
   | --- | --- | --- | --- |
   | RandomWrites\_1 MB |1MB |100 |0 |
   | RandomReads\_1 MB |1MB |100 |100 |
2. Aşağıdaki parametrelerle önbellek disk başlatma için Iometer testi çalıştırın. Üç çalışan iş parçacığı, hedef birimi ve 128 sıra derinliği için kullanın. Test "çalışma zamanı" süresini 2hrs için "Kurulum Test" sekmesinde ayarlayın.

   | Senaryo | Hedef birim | Ad | Süre |
   | --- | --- | --- | --- |
   | Önbellek diski başlatın |CacheReads |RandomWrites\_1 MB |2hrs |
3. Aşağıdaki parametrelerle önbellek disk ısıtma için Iometer testi çalıştırın. Üç çalışan iş parçacığı, hedef birimi ve 128 sıra derinliği için kullanın. Test "çalışma zamanı" süresini 2hrs için "Kurulum Test" sekmesinde ayarlayın.

   | Senaryo | Hedef birim | Ad | Süre |
   | --- | --- | --- | --- |
   | Önbellek diski sıcak |CacheReads |RandomReads\_1 MB |2hrs |

Önbellek disk warmed sonra aşağıda listelenen test senaryoları ile devam edin. Iometer testi çalıştırmak için en az üç çalışan iş parçacığı için kullanmak **her** hedef birim. Her çalışan iş parçacığı için hedef birimi seçin, sıra derinliği ayarlamak ve karşılık gelen test senaryosu çalıştırmak için aşağıdaki tabloda gösterildiği gibi kaydedilmiş test belirtimleri birini seçin. Tabloda ayrıca beklenen sonuçları IOPS ve üretilen iş için bu testleri çalıştırırken gösterilir. Tüm senaryolarda, küçük g/ç boyutu 8KB ve 128 yüksek sıra derinliği kullanılır.

| Test senaryosu | Hedef birim | Ad | Sonuç |
| --- | --- | --- | --- |
| En çok, Okuma IOPS |CacheReads |RandomWrites\_8 K |50.000 IOPS |
| En çok, IOPS yazma |NoCacheWrites |RandomReads\_8 K |64.000 IOPS |
| En çok, Birleşik IOPS |CacheReads |RandomWrites\_8 K |100.000 IOPS |
| NoCacheWrites |RandomReads\_8 K | &nbsp; | &nbsp; |
| En çok, Okuma MB/sn |CacheReads |RandomWrites\_64 K |524 MB/sn |
| En çok, Yazma MB/sn |NoCacheWrites |RandomReads\_64 K |524 MB/sn |
| Birleşik MB/sn |CacheReads |RandomWrites\_64 K |1000 MB/sn |
| NoCacheWrites |RandomReads\_64 K | &nbsp; | &nbsp; |

Iometer ekran görüntüleri için toplam IOPS ve üretilen iş senaryolarını test sonuçları aşağıda verilmiştir.

*Birleşik okuma ve yazma işlemleri maksimum IOPS*  
![](media/premium-storage-performance/image9.png)

*Birleşik okuma ve yazma işlemleri en yüksek verimlilik*  
![](media/premium-storage-performance/image10.png)

### <a name="fio"></a>FIO
FIO Linux VM'ler Kıyaslama depolama için popüler bir araçtır. Farklı GÇ boyutları, sıralı seçmek için esneklik veya rastgele okuma ve yazma. Çalışan iş parçacıkları veya belirtilen g/ç işlemleri gerçekleştirmek için işlemler olarak çoğaltılır. Her iş parçacığı iş dosyalarını kullanarak gerçekleştirmelidir g/ç işlemleri türünü belirtebilirsiniz. Aşağıdaki örneklerde gösterildiği senaryo başına bir proje dosyası oluşturduk. Premium depolama üzerinde çalışan farklı iş yükleri Kıyaslama için bu iş dosyalarında belirtimleri değiştirebilirsiniz. Örneklerde, standart DS 14 VM çalışan bir kullanıyoruz **Ubuntu**. Başlangıcı içinde açıklanan aynı kurulumunda [bölüm değerlendirmesi](#Benchmarking) ve yarı Kıyaslama testleri çalıştırmadan önce önbelleği.

Başlamadan önce [FIO karşıdan](https://github.com/axboe/fio) ve sanal makinenize yükleyin.

Ubuntu için aşağıdaki komutu çalıştırın

```
apt-get install fio
```

Yazma işlemlerini yürüten için dört çalışan iş parçacıkları ve dört çalışan iş parçacığı disklerde yönlendirmeli okuma işlemleri için kullanacağız. Yazma çalışanları, "None" ayarlamak önbelleği ile 10 diskleri bulunduğundan "nocache" birim trafiği yürüten. Okuma çalışanları, 1 "Salt okunur" önbellek kümesi olduğundan "readcache" birim trafiği yürüten.

*Maksimum yazma IOPS*  
Proje dosyası yazma maksimum IOPS almak için aşağıdaki belirtimlere oluşturun. "Fiowrite.ini" adı.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Önceki bölümlerde ele alınan tasarım yönergeleri uyumlu olan izleme önemli noktalar unutmayın. Bu belirtimler disk maksimum IOPS için gerekli olan,  

* 256 yüksek sıra derinliği.  
* Küçük blok boyutu 8 KB.  
* Birden çok iş parçacığı rastgele yazma işlemlerini gerçekleştirme.

30 saniye FIO test kapalı kazandırın için aşağıdaki komutu çalıştırın  

```
sudo fio --runtime 30 fiowrite.ini
```

Test çalışırken VM IOPS sayısı, yazma ve Premium diskleri ileterek görmeye olacaktır. Aşağıdaki örnekte gösterildiği gibi kendi maksimum IOPS sınırı 50.000 IOPS yazma DS14 VM iletmektir.  
    ![](media/premium-storage-performance/image11.png)

*Maksimum IOPS okuma*  
Proje dosyası maksimum okuma IOPS almak için aşağıdaki belirtimlere oluşturun. "Fioread.ini" adı.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Önceki bölümlerde ele alınan tasarım yönergeleri uyumlu olan izleme önemli noktalar unutmayın. Bu belirtimler disk maksimum IOPS için gerekli olan,

* 256 yüksek sıra derinliği.  
* Küçük blok boyutu 8 KB.  
* Birden çok iş parçacığı rastgele yazma işlemlerini gerçekleştirme.

30 saniye FIO test kapalı kazandırın için aşağıdaki komutu çalıştırın

```
sudo fio --runtime 30 fioread.ini
```

Test çalışırken VM okunan IOPS sayısı ve Premium diskleri ileterek görmeye olacaktır. Aşağıdaki örnekte gösterildiği gibi birden fazla 64.000 okuma IOPS DS14 VM iletmektir. Disk ve önbellek performansı birleşimidir.  
    ![](media/premium-storage-performance/image12.png)

*En fazla okuma ve yazma IOPS*  
Şununla iş dosyası oluşturma en fazla almak için özellikleri birleştirilmiş okuma ve yazma IOPS. "Fioreadwrite.ini" adı.

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Önceki bölümlerde ele alınan tasarım yönergeleri uyumlu olan izleme önemli noktalar unutmayın. Bu belirtimler disk maksimum IOPS için gerekli olan,

* 128 yüksek sıra derinliği.  
* Küçük blok boyutu 4 KB.  
* Birden çok iş parçacığı rastgele gerçekleştirme okur ve yazar.

30 saniye FIO test kapalı kazandırın için aşağıdaki komutu çalıştırın

```
sudo fio --runtime 30 fioreadwrite.ini
```

Test çalışırken birleşik okuma sayısını görmek ve IOPS VM yazma kuramaz ve Premium diskleri gönderiliyor. Aşağıdaki örnekte gösterildiği gibi DS14 VM 100. 000'den fazla birleşik okuma ve yazma IOPS iletmektir. Disk ve önbellek performansı birleşimidir.  
    ![](media/premium-storage-performance/image13.png)

*En büyük verimi birleştirilmiş*  
En fazla almak için okuma ve yazma verimlilik birlikte, daha büyük blok boyutu ve büyük sıra derinliği okuma ve yazma işlemleri gerçekleştiren birden çok iş parçacığı ile kullanın. 64 KB blok boyutunu ve sıra derinliği 128 kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Azure Premium Storage hakkında daha fazla bilgi edinin:

* [Premium Depolama: Azure Sanal Makine İş Yükleri için Yüksek Performanslı Depolama](../articles/virtual-machines/windows/premium-storage.md)  

SQL Server kullanıcıları için SQL Server için performans en iyi uygulamaları makalelerini okuyun:

* [Azure sanal makinelerde SQL Server için performans en iyi yöntemler](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure Premium Storage Azure VM'deki SQL Server için en yüksek performans sağlar](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)
