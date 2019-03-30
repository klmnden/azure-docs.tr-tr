---
title: Küme Kaynak Yöneticisi küme açıklama | Microsoft Docs
description: Service Fabric kümesi, hata etki alanları ve yükseltme etki alanları, düğüm özellikleri ve küme kaynak yöneticisi için düğüm kapasiteleri belirterek açıklayan.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7cd4a54a62d7304587c55338f088c504e40a74af
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670687"
---
# <a name="describing-a-service-fabric-cluster"></a>Service fabric kümesi açıklayan
Service Fabric Küme Kaynak Yöneticisi bir kümesini tanımlamak için çeşitli mekanizmalar sağlar. Çalışma zamanı sırasında küme kaynak yöneticisi, kümede çalışan hizmetler yüksek kullanılabilirliğini sağlamak için bu bilgileri kullanır. Önemli kurallar zorlarken, ayrıca küme içindeki kaynak tüketimini iyileştirmek çalışır.

## <a name="key-concepts"></a>Önemli kavramlar
Küme Kaynak Yöneticisi, bir kümeyi tanımlama çeşitli özelliklerini destekler:

* Hata etki alanları
* Upgrade Domains
* Düğüm özellikleri
* Düğüm kapasiteleri

## <a name="fault-domains"></a>Hata etki alanları
Hata etki alanı tüm Eşgüdümlü hatası alanıdır. (Bu sürücü hataları hatalı NIC bellenimi için güç kaynağı hatalarından kendi çeşitli nedenlerle başarısız) tek bir makine hata etki alanı olduğu. Güç veya tek bir konumda tek bir kaynak paylaşımı makineler olduğu gibi aynı Ethernet anahtara bağlı makineler, aynı hata etki alanında olur. Donanım hatalarına çakıştırmayı doğal olduğundan, hata etki alanları kendiliğinden Cerberus ve Service fabric'te bir URI'leri olarak temsil edilir.

Service Fabric Hizmetleri güvenli bir şekilde yerleştirmek için bu bilgileri kullandığından hata etki alanlarını doğru şekilde ayarlanması önemlidir. Service Fabric Hizmetleri gitmek bir hizmet hata etki alanı (bazı bileşen hatasından kaynaklanır) kaybına neden olacak şekilde yerleştirmek istememektedir. Azure ortamı Service Fabric, doğru sizin adınıza kümedeki düğümlerin yapılandırmak için ortam tarafından sağlanan hata etki alanı bilgileri kullanır. Küme ayarlama zaman tanımlanan için Service Fabric tek başına, hata etki alanları 

> [!WARNING]
> Service Fabric için sağlanan hata etki alanı bilgilerinin doğru olduğunu önemlidir. Örneğin, Service Fabric kümenizin düğümlerine beş fiziksel konakları üzerinde çalışan, 10 sanal makineler içinde çalıştığını kabul edelim. Bu durumda olsa bile 10 sanal makineler, vardır yalnızca 5 farklı (üst düzey) hata etki alanları. Aynı fiziksel ana bilgisayarı paylaşan Vm'leri aynı kök hata etki alanı, fiziksel ana bilgisayarları başarısız olursa, Vm'leri Eşgüdümlü hatasıyla karşılaşan beri paylaşmak neden olur.  
>
> Service Fabric değiştirme için bir düğüm arıza etki alanını bekliyor. Sanal makinelerin yüksek kullanılabilirlik gibi emin olmanın başka mekanizmalar [HA-VMs](https://technet.microsoft.com/library/cc967323.aspx) saydam geçiş sanal makinelerin bir konaktan diğerine kullandıkları Service Fabric ile çakışmalarına neden olabilir. Bu mekanizmalar başlatmayın yeniden yapılandırın veya VM içinde çalışan kod bildirir. Bu nedenle, bunlar **desteklenmiyor** Service Fabric çalıştırmak için ortamları olarak kümeleri. Service Fabric, işe yüksek kullanılabilirlik yalnızca teknoloji olacak. Dinamik VM geçişi gibi mekanizmaların SAN'lar, veya diğer gerekli değildir. Service Fabric, bu mekanizmalar ile birlikte kullandıysanız _azaltmak_ uygulama kullanılabilirliği ve güvenilirliği, karmaşıklık, tanıtmak için hata merkezi kaynakları ekleyin ve güvenilirlik yazılımınız ve Service fabric'te değerlerle çakışabilir kullanılabilirlik stratejileri. 
>
>

Grafikte, biz hata etki alanlarına katkıda bulunabilir ve tüm farklı hataya neden olan etki alanları listesinde tüm varlıkları renk. Bu örnekte, veri merkezleri ("DC"), raflar ("R") ve dikey pencereleri ("B") sahip. Kısıtlanmamışsa, birden fazla sanal makine her dikey pencerede tutar, hata etki alanı hiyerarşideki başka bir katmanı olabilir.

<center>

![Hata etki alanları düzenlenmiş düğümleri][Image1]
</center>

Çalışma zamanı sırasında Service Fabric Küme Kaynak Yöneticisi hata etki alanları kümedeki göz önünde bulundurur ve düzenleri planları. Durum bilgisi olan yinelemeler veya belirli bir hizmetin durum bilgisi olmayan örnekleri ayrı hata etki alanları şekilde dağıtılır. Hata etki alanları arasında hizmet dağıtma, hata etki alanı hiyerarşinin herhangi bir düzeyde başarısız olduğunda hizmetin kullanılabilirliğini gizliliğinin tehlikeye atılmasını sağlar.

Service Fabric'in Küme Kaynak Yöneticisi kaç Katmanlar vardır hata etki alanı hiyerarşisinde önemli değil. Bununla birlikte, hiyerarşideki herhangi bir kısmının kaybı içinde çalışan hizmetleri etkilemez emin olmak çalışır. 

Her hata etki alanı hiyerarşideki derinlik düzeyini düğümlerde aynı sayıda varsa en iyisidir. Hata etki alanları "ağacı" kümenizde dengesiz ise, bu hizmetleri en iyi ayırma anlamak için Küme Kaynak Yöneticisi için zorlaştırır. Bazı etki alanlarına etki daha fazla diğer etki alanı Hizmetleri'nin kullanılabilirliğini kaybı anlamına imbalanced hata etki alanları düzenler. Sonuç olarak, küme kaynak yöneticisi olarak iki amaçları arasında kaldırılır: "Heavy" Bu etki alanında bunlar üzerinde Hizmetleri yerleştirerek makineleri'nin istediği ve onu bir etki alanı kaybı sorunlara neden olmayan diğer etki alanlarında Hizmetleri lıbcmtd.lib istiyor. 

İmbalanced etki alanları ne gibi görünüyor? Aşağıdaki diyagramda, iki farklı küme düzenleri göstereceğiz. İlk örnekte, düğümler, hata etki alanları arasında eşit şekilde dağıtılır. İkinci örnekte, bir hata etki alanı, başka bir hata etki alanı daha pek çok daha fazla düğüm var. 

<center>

![İki farklı küme düzenleri][Image2]
</center>

Azure'da, hata etki alanı bir düğüm içeren seçim sizin için yönetilir. Ancak, sağlamanız düğüm sayısına bağlı olarak, yine de hata etki alanları ile bunlara diğerlerinden daha fazla düğüme sahip kalabilirsiniz. Örneğin, beş hata etki alanları kümesinde sahip, ancak yedi düğümleri sağlamak için belirli bir NodeType varsayalım. Daha fazla düğüm ile bu durumda, ilk iki hata etki alanları edersiniz. Yalnızca birkaç örnekleriyle daha fazla NodeType dağıtmaya devam ederseniz sorun yarışacağından alır. Her düğüm düğüm sayısını bırakmanız önerilir, bu nedenle bir birden çok hata etki alanları sayısı türüdür.

## <a name="upgrade-domains"></a>Yükseltme etki alanları
Yükseltme etki alanları kümesinin düzenini anlama Service Fabric Küme Kaynak Yöneticisi yardımcı olan başka bir özelliğidir. Yükseltme etki alanları aynı anda yükseltilir düğümleri kümesini tanımlar. Yükseltme etki alanları, Küme Kaynak Yöneticisi anlamak ve yükseltme gibi yönetim işlemlerini düzenlemek yardımcı olur.

Yükseltme etki alanları çok hata etki alanları gibi ancak birkaç önemli farklar da bulunur. İlk olarak, alanları, Eşgüdümlü donanım hatalarının, hata etki alanlarını tanımlayın. Yükseltme etki alanları, diğer taraftan, ilke tarafından tanımlanır. Kaç tane yerine bu ortamı tarafından dikte istediğiniz karar alın. Yükseltme çok düğümleri yaptığınız gibi etki alanınız. Başka bir hata etki alanları ve yükseltme etki alanları arasında yükseltme etki alanı hiyerarşik olmadığından farktır. Bunun yerine, bunlar gibi basit bir etiketin daha fazla. 

Aşağıdaki diyagramda, üç hata etki alanlarında üç yükseltme etki alanı şeritli gösterilmektedir. Ayrıca, burada her farklı hata ve yükseltme etki alanları sona eriyor bir olası yerleşimini üç farklı kopyaya için bir durum bilgisi olan hizmet gösterir. Bu yerleşimi hata etki alanı hizmeti yükseltmesi sırasında ortasında kaybı sağlar ve kod ve verilerin bir kopyasını çözümlenmedi.  

<center>

![Hata ve yükseltme etki alanları ile yerleştirme][Image3]
</center>

Artıları ve eksileri sahip çok sayıda yükseltme etki alanları için vardır. Daha fazla yükseltme etki alanını yükseltmenin her bir adım daha ayrıntılı ve bu nedenle daha az sayıda düğüm veya hizmetleri etkiler anlamına gelir. Sonuç olarak, daha az Hizmetleri bir anda taşımak daha az değişim sıklığı sisteme giriş var. Yükseltme sırasında sunulan herhangi bir sorundan etkilenen hizmet daha az olduğundan güvenilirliği artırmak için eğilimi gösterir. Daha fazla yükseltme etki alanları, ayrıca diğer düğümlerdeki yükseltme etkisini işlemek için daha az arabellek ihtiyacınız olduğu anlamına gelir. Örneğin, beş yükseltme etki alanları varsa, her düğüm yaklaşık %20 trafiğinizin ele. Bu yükseltme etki alanı yükseltmesi aşağı duruma getirmeniz gerekiyorsa, bu Yük genellikle bir yere gitmek gerekir. Dört kalan yükseltme etki alanı olduğundan, her yaklaşık %5 toplam trafiğin yer olması gerekir. Daha fazla yükseltme etki düğümler küme üzerinde daha az arabellek ihtiyacınız olduğu anlamına gelir. Örneğin, 10 yükseltme etki alanları yerine tablonuz varsa göz önünde bulundurun. Bu durumda, her UD yalnızca yaklaşık %10 toplam trafiğin işlenmesi. Bir yükseltme adımları, kümeye aracılığıyla her etki alanı yalnızca yaklaşık toplam trafik %1.1 yer olması gerekir. Daha fazla yükseltme etki alanları genellikle daha az ayrılmış kapasite gerektiğinden düğümlerinizi yüksek kullanımı, çalıştırmanıza olanak tanır. Aynı hata etki alanları için geçerlidir.  

Çok sayıda yükseltme etki alanları yaşama dezavantajı, yükseltmeleri uzun eğilimindedir ' dir. Service Fabric, bir yükseltme etki alanı tamamlandıktan ve bir yükseltmeye başlatmadan önce denetimlerini gerçekleştiren süresi kısa bir süre bekler. Bu gecikme, yükseltmeye devam etmeden önce yükseltme tarafından sunulan olan algılama sorunları etkinleştirin. Artırabilen kabul edilebilir olduğundan, hatalı değişiklikleri bir anda hizmeti, çok fazla etkilenmesini önler.

Çok az sayıda yükseltme etki alanları, birçok negatif yan etkileri vardır: Genel kapasite tek tek her yükseltme etki alanı kullanılamıyor ve büyük bir kısmı yükseltiliyor kullanılamıyor. Örneğin, yalnızca üç yükseltme etki alanı varsa, aynı anda yaklaşık 1/3 genel hizmet veya küme kapasitesi edilmektedir. Kalan iş yükünü işlemek için kümenizin yeterli kapasiteye sahip olduğundan çok hizmetinizin aşağı aynı anda sahip arzu değil. O arabelleğe anlamına gelir normal işlem sırasında aksi durumdakinden daha az yüklenen düğümleri olan koruma. Bu, hizmetiniz çalıştırma maliyetini artırır.

Toplam hata veya yükseltme etki alanları sayısını nasıl örtüşecek üzerindeki kısıtlamaları veya bir ortamda gerçek sınır yoktur. Birçok ortak deseni vardır. Bununla birlikte:

- 1:1, hata etki alanları ve yükseltme etki alanları eşlendi
- Bir yükseltme etki alanı her düğüm (fiziksel veya sanal işletim sistemi örneği)
- Burada hata etki alanları ve yükseltme etki alanları bir matris çizgileri genellikle çalışan makineler ile form "şeritli" veya "matris" modeli

<center>

![Hata ve yükseltme etki alanı düzeni][Image4]
</center>

Hangi düzenin seçmek için hiçbir en iyi yanıt, her bazı Artıları ve eksileri vardır. Örneğin, 1FD:1UD modeli ayarlamak basit bir işlemdir. 1 yükseltme etki alanı başına düğüm modeli, insanların için kullanılan gibi çoğu. Yükseltme sırasında her düğüm bağımsız olarak güncelleştirilir. Bu, nasıl küçük makineler kümesini el ile geçmişte yükseltildiği için benzer. 

Burada, UD ve Fd'ler bir tablo oluşturur ve düğümleri çapraz başlangıç yerleştirilir FD/UD matris en yaygın modelidir. Bu, varsayılan olarak Azure Service Fabric kümelerinde kullanılan modelidir. Çok sayıda düğüme sahip kümeler için her şeyi yoğun matris yukarıdaki desen gibi bakan sonlandırır.

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Hata ve yükseltme etki alanı kısıtlamaları ve bunun sonucunda oluşan davranışı
### <a name="default-approach"></a>*Varsayılan yaklaşımı*
Varsayılan olarak, Küme Kaynak Yöneticisi hata ve yükseltme etki alanları arasında dengeli Hizmetleri tutar. Bu olarak modellenir bir [kısıtlaması](service-fabric-cluster-resource-manager-management-integration.md). Hata ve yükseltme etki alanı kısıtlaması durumları: "Bir verili hizmet bölümü için hiçbir zaman olmalıdır bir fark hiyerarşisinin aynı düzeyde iki tüm etki alanları arasında hizmet nesneleri (durum bilgisi olmayan hizmet örneği veya durum bilgisi olan hizmet çoğaltmalar) sayısını birden büyük". Bu kısıtlama, "en büyük fark" garantisi sağlar varsayalım. Hata ve yükseltme etki alanı kısıtlaması, belirli taşıma veya yukarıda belirtilen kural ihlal düzenlemeleri engeller. 

Bir örneğe göz atalım. Altı düğümü, beş hata etki alanları ve beş yükseltme etki alanları ile yapılandırılmış olan bir küme sahip olduğunuzu düşünelim.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

*Yapılandırma 1*

Artık bir TargetReplicaSetSize ile (veya Instancecount bir durum bilgisi olmayan hizmet) bir hizmet beş oluştururuz olduğunu varsayalım. Çoğaltmaları N1 N5 gelirsiniz. Aslında, N6 hiçbir zaman kaç bu oluşturduğunuz gibi hizmetleri bağımsız olarak kullanılır. Peki ama neden? Geçerli düzeni ve N6 seçilirse ne olacağını arasındaki fark bakalım.

İşte, yapılandırdığımıza düzenini ve çoğaltmaları hata ve yükseltme etki alanı başına toplam sayısı:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1. |1. |1. |1 |- |

*Düzen 1*


Bu düzen, hata etki alanı ve yükseltme etki alanı başına düğüm açısından dengelenir. Ayrıca, hata ve yükseltme etki alanı başına çoğaltma sayısı bakımından dengelenir. Her etki düğümler aynı sayıda ve çoğaltmaları aynı sayıda sahiptir.

Şimdi biz N6 N2 yerine kullanmışsınız ne olacağını konumunda göz atalım. Nasıl çoğaltmaları ardından dağıtılması?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1. |1. |1 |- |

*Düzen 2*


Bu düzen, hata etki alanı kısıtlaması için "en büyük fark" garantisi, bizim tanımını ihlal ediyor. FD1 sıfır, bir en büyük fark büyük olduğu iki toplam FD0 FD1 arasındaki fark yapmadan sahipken FD0 iki çoğaltma vardır. Küme Kaynak Yöneticisi kısıtlamayı ihlal olduğundan, bu düzenleme izin vermez. Benzer şekilde biz N2 ve (yerine N1 ve N2) N6 kaldırdıysa biz alın:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1. |1. |1. |1 |- |

*Düzen 3*


Bu düzen açısından hata etki alanlarında dengelenir. UD1 iki sahipken UD0 sıfır yineleme olduğundan ancak artık, yükseltme etki alanı kısıtlamasını ihlal. Bu nedenle, bu düzen ayrıca geçersiz ve küme kaynak yöneticisi tarafından çekilmesi olmaz.

Durum bilgisi olan yinelemeler ya da durum bilgisi olmayan örnekleri dağıtılması için bu yaklaşım, en iyi olası hata toleransı sağlar. Bir durumda bir etki alanı aşağı gittiğinde çoğaltmaları/örnek en az sayıda kaybolur. 

Öte yandan, bu yaklaşım çok katı ve kümenin tüm kaynakları kullanmak için izin verme. Belirli bir küme yapılandırmaları için belirli düğümler kullanılamaz. Bu hizmet, hizmetlerinizin yerleştirme değil uyarı iletilerini kaynaklanan Fabric neden olabilir. Önceki örnekte, küme düğümleri bazıları olamaz (verilen örnekte N6) kullanılır. Bu kümeye (N7 – N10) düğümleri ekleseniz bile N1 – N5 hata ve yükseltme etki alanı kısıtlamaları nedeniyle üzerinde yalnızca çoğaltmalar/örnek konulabilir. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | |N10 |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | |N9 |N5 |

*Yapılandırma 2*


### <a name="alternative-approach"></a>*Alternatif bir yaklaşım*

Küme Kaynak Yöneticisi, en düşük düzeyde güvenlik hala güvence altına almak sırasında yerleştirme sağlayan hata ve yükseltme etki alanı kısıtlaması başka bir sürümünü destekler. Farklı hata ve yükseltme etki alanı kısıtlaması gibi belirtilebilir: "Bir verili hizmet bölümü için etki alanları arasında çoğaltma dağıtım bölüm çekirdek kayıp saptanmamış emin olmanız gerekir". Bu kısıtlama "çekirdek güvenli" garantisi sağlar varsayalım. 

> [!NOTE]
>Bir durum bilgisi olan hizmet için tanımlarız *çekirdek kayıp* bölüm çoğaltmalarını çoğunu basılı olduğunda aynı anda bir durumda. Örneğin, beş TargetReplicaSetSize ise, çekirdek herhangi üç kopyaya kümesini temsil eder. Benzer şekilde, 6 TargetReplicaSetSize ise dört çoğaltmaları çekirdek için gereklidir. Bölüm normal olarak çalışmaya devam etmek istiyorsa her iki durumda da ikiden fazla çoğaltma aşağı aynı anda olabilir. Bir durum bilgisi olmayan hizmet için de yoktur *çekirdek kayıp* gibi durum bilgisi olmayan hizmetler örnekleri çoğunu aşağı git aynı anda bile normal şekilde çalışmaya devam eder. Bu nedenle, durum bilgisi olan hizmetler metnin geri kalanında odaklanacağız.
>

Önceki örneğe geri dönelim. Kısıtlama "çekirdek güvenli" sürümü ile tüm üç belirli düzenleri geçerli olacaktır. Bu durum, olacaktır bile FD0 üçüncü düzende hatası ikinci düzen veya UD1 bölüm çekirdek (çoğaltmalarını çoğunu yukarı olmaya) hala gerekir çünkü. Kısıtlama bu sürümü ile N6 neredeyse her zaman yararlı.

"Güvenli çekirdek" yaklaşım "en büyük fark" yaklaşım göre neredeyse tüm küme topolojisi geçerli çoğaltma dağıtımları daha kolay olduğu gibi daha fazla esneklik sağlar. Bazı hataları daha da kötüsü olduğundan ancak, bu yaklaşım en iyi hataya dayanıklılık özelliklerini garanti edemez. En kötü durum senaryosunda, tek bir etki alanı ve ek bir çoğaltma hatası ile çoğaltmalar çoğunu kaybedilebilir. Örneğin, çekirdek 5 çoğaltmalar veya örnekleri ile kaybetmek gerektiği 3 hataları yerine, yalnızca iki hataları olan çoğu artık kaybedebilirsiniz. 

### <a name="adaptive-approach"></a>*Uyarlanabilir bir yaklaşım*
Her iki yaklaşım güçlü ve zayıf olduğundan, bu iki stratejiler birleştiren uyarlanabilir bir yaklaşım tanıttık.

> [!NOTE]
> Bu, Service Fabric sürüm 6.2 ile başlayarak varsayılan davranışı olacaktır. 
> 
> Uyarlanabilir bir yaklaşım, varsayılan "en büyük fark" mantığı kullanır ve yalnızca gerekli olduğunda "çekirdek güvenli" mantıksal anahtarları. Küme Kaynak Yöneticisi otomatik olarak hangi stratejiyi nasıl kümeyi ve Hizmetleri yapılandırılmış en bakarak gerekli olduğunu belirler. Belirli bir hizmet için: *TargetReplicaSetSize hata etki alanları sayısı ve yükseltme etki alanlarının sayısı ile kalansız ise **ve** düğüm sayısını (hata etki alanları sayısı) küçük veya ona eşit olduğu * (yükseltme etki alanları sayısı) kümesi Resource Manager, bu hizmet için "temel çekirdek" mantıksal kullanmalıdır.* Küme Kaynak Yöneticisi durum bilgisi olmayan hizmetler için ilgili olmaması çekirdek kayıp rağmen durum bilgisiz ve durum bilgisi olan hizmetler için bu yaklaşım kullanacağını aklınızda size aittir.

Önceki örneğe geri dönelim ve küme artık 8 düğüm (küme yine beş hata etki alanları ve beş yükseltme etki alanları ve bu küme kalır beş barındırılan hizmetin TargetReplicaSetSize yapılandırılmış) olduğunu varsayalım. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | | |N5 |

*Yapılandırma 3*

Küme Kaynak Yöneticisi, gerekli tüm koşulların karşılandığından olduğundan hizmet dağıtan, "çekirdek tabanlı" mantıksal yararlanacaktır. Bu, N6 – N8 kullanımını etkinleştirir. Tek bir olası hizmet dağıtım, bu durumda gibi görünebilir:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | | | |0 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |2 |1 |1 |0 |1 |- |

*Düzen 4*

Hizmetinizin TargetReplicaSetSize dört (örneğin) azaltılmıştır, küme kaynak yöneticisi bu değişikliği dikkat edin ve TargetReplicaSetSize artık FD ve Ud'ler sayısına göre bölünebilen olmadığı için "en büyük fark" mantığı kullanılarak Sürdür. Sonuç olarak, belirli yineleme hareketleri, böylece hata etki alanı ve yükseltme etki alanı mantığı "en büyük fark" sürümünü değil ihlal N1 N5 düğümlerini üzerindeki dört çoğaltmaların kalan dağıtılabilmesi için meydana gelir. 

Geri dördüncü düzenini ve beş TargetReplicaSetSize aranıyor. N1 kümeden kaldırılırsa, yükseltme etki alanı sayısı için dört eşit olur. Küme Kaynak Yöneticisi yeniden UD sayısına eşit olarak hizmetin TargetReplicaSetSize artık bölmeye değil olarak "en büyük fark" mantığı kullanılarak başlatılır. Sonuç olarak, çoğaltma yeniden oluşturulduğunda R1, böylece hata ve yükseltme etki alanı kısıtlaması değil ihlal üzerinde N4 yerleşmesi sahiptir.

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |YOK |YOK |YOK |YOK |YOK |YOK |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | |R1 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1. |1. |1. |1 |- |

*Düzen 5*

## <a name="configuring-fault-and-upgrade-domains"></a>Hata ve yükseltme etki alanlarında yapılandırma
Hata etki alanları ve yükseltme etki alanları tanımlama yapılır otomatik olarak Azure'da barındırılan Service Fabric dağıtımları. Service Fabric alır ve Azure ortam bilgileri kullanır.

Kendi kümenizi oluşturuyorsanız (veya belirli bir topoloji geliştirme çalıştırmak istediğiniz varsa), hata etki alanı ve yükseltme etki alanı bilgileri kendiniz sağlayabilir. Bu örnekte, üç "veri merkezleri" (her üç Raflarla) kapsayan bir dokuz düğüm yerel geliştirme kümesi tanımlarız. Bu küme, bu üç veri merkezleri arasında şeritli üç yükseltme etki de vardır. Yapılandırmasına bir örnek aşağıda verilmiştir: 

ClusterManifest.xml

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one-box /one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```

tek başına dağıtımlarında ClusterConfig.json

```json
"nodes": [
  {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm3",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm4",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm5",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm6",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm7",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm8",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm9",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD3"
  }
],
```

> [!NOTE]
> Azure Resource Manager aracılığıyla kümeleri tanımlarken, hata etki alanları ve yükseltme etki alanları Azure tarafından atanır. Bu nedenle, Azure Resource Manager şablonunuzu tanımında düğüm türleri ve sanal makine ölçek kümeleri, hata etki alanı veya yükseltme etki alanı bilgilerini içermez.
>

## <a name="node-properties-and-placement-constraints"></a>Düğüm özellikleri ve yerleştirme kısıtlamaları
Bazen (aslında çoğu zaman), belirli iş yükleri yalnızca belirli türdeki düğümleri üzerinde çalıştığından emin olmak istiyorsanız yedekleyeceksiniz. Örneğin, başkalarının olmayabilir ancak bazı iş yükü GPU veya SSD gerektirebilir. Burada neredeyse her n katmanlı mimari belirli iş yükleri için donanım desteği, harika bir örnektir. Belirli makineler ön uç ya da API uygulamasının yan hizmet hizmet ve istemcileri veya Internet'e açık. Genellikle, farklı donanım kaynakları olan farklı makineler işlem ve depolama katmanları iş işleyin. Genellikle bunlar _değil_ doğrudan istemciler veya Internet'e açık. Service Fabric bekliyor. belirli iş yükleri belirli donanım yapılandırmalarını temel çalıştırmak için gereken yere durumlar vardır. Örneğin:

* var olan bir n katmanlı uygulama bir Service Fabric ortamına "yükseltilmiş ve kaydırılacak" kaldırıldı
* performans, ölçek ve güvenlik yalıtımı nedeniyle belirli donanım üzerinde çalıştırılacak bir iş yükü ister
* Bir iş yükü İlkesi veya kaynak tüketimi nedeniyle diğer iş yükleri gelen yalıtılmış olmalıdır

Bu tür yapılandırmaları desteklemek için Service Fabric düğümlere uygulanması etiketleri birinci sınıf bir kavramı vardır. Bu etiketler adlı **düğümü özellikleri**. **Yerleştirme kısıtlamaları** olan bir veya daha fazla düğüm özelliklerini seçin, tek tek hizmetlerine bağlı deyimleri. Yerleştirme kısıtlamaları Hizmetleri çalıştırdığı tanımlar. Kısıtlamaları kümesini Genişletilebilir - herhangi bir anahtar/değer çifti çalışabilir. 

<center>

![Düzen farklı iş yüklerini küme][Image5]
</center>

### <a name="built-in-node-properties"></a>Düğüm özellikleri yerleşik
Service Fabric tanımlamadan girmelerine gerek otomatik olarak kullanılabilecek bazı varsayılan düğümü özellikleri tanımlar. Her düğümde tanımlanan varsayılan Özellikler **NodeType** ve **NodeName**. Yerleştirme kısıtlama olarak örneğin yazabilirsiniz `"(NodeType == NodeType03)"`. NodeType aşağıdakilerden en yaygın olarak kullanılan özellikleri genellikle bulduk. 1:1 ile bir makine türü karşılık gelen olduğundan yararlı olur. Her makine türü, geleneksel bir n katmanlı uygulama iş yükü türüne karşılık gelir.

<center>

![Yerleştirme kısıtlamaları ve düğümü özellikleri][Image6]
</center>

## <a name="placement-constraint-and-node-property-syntax"></a>Yerleşim kısıtlaması ve düğüm özelliği söz dizimi 
Düğüm özelliği için belirtilen değer bir dize, bool, olabilir veya signed long. Hizmet bildirimine bir yerleştirme denir *kısıtlaması* olduğundan burada hizmet kümesinde çalışabilen kısıtlar. Kısıtlama kümedeki farklı bir düğüme özelliklerinde çalışır herhangi bir Boole ifadesi olabilir. Bu boolean ifadeler geçerli Seçici verilmiştir:

1) belirli ifadeler oluşturmak için koşullu denetimler

| Deyim | Sözdizimi |
| --- |:---:|
| "equal" | "==" |
| "eşit değildir" | "!=" |
| "büyüktür" | ">" |
| "büyüktür veya eşittir" | ">=" |
| "az" | "<" |
| "az veya buna eşit" | "<=" |

2) gruplandırma ve mantıksal işlemler için Boolean ifadeleri

| Deyim | Sözdizimi |
| --- |:---:|
| "ve" | "&&" |
| "veya" | "&#124;&#124;" |
| "not" | "!" |
| "olarak tek bir deyim grubunu" | "()" |

Temel kısıtlama ifadeleri bazı örnekleri aşağıda verilmiştir.

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Burada genel yerleştirme kısıtlaması ifadesi "True" olarak değerlendirir düğümler üzerinde yerleştirilen hizmet olabilir. Bu özelliği içeren herhangi bir yerleştirme kısıtlaması tanımlanmış bir özellik olmayan düğümleri eşleşmiyor.

Verilen düğüm türü için tanımlanan aşağıdaki düğüm özellik varsayalım:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan. 

> [!NOTE]
> Azure Resource Manager şablonunuzda düğüm türü genellikle parametreli. "NodeType01 yerine", "[parameters('vmNodeType1Name')]" gibi görünür.
>

```json
"nodeTypes": [
    {
        "name": "NodeType01",
        "placementProperties": {
            "HasSSD": "true",
            "NodeColor": "green",
            "SomeProperty": "5"
        },
    }
],
```

Hizmet yerleşimi oluşturabilirsiniz *kısıtlamaları* gibi aşağıdaki gibi bir hizmet için:

C#

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// add other required servicedescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Tüm düğümleri NodeType01 geçerliyse, bu düğüm türü kısıtlaması ile de seçebilirsiniz "(NodeType == NodeType01)".

Bir hizmetin yerleştirme kısıtlamaları hakkında harika şeyler bunlar dinamik olarak çalışma zamanı sırasında güncelleştirilmesi biridir. Bu nedenle gerekiyorsa, kümedeki bir hizmet gezinme, ekleyip kaldırabilirsiniz gereksinimleri, vb. Service Fabric bile bu tip değişiklikler yapıldığında hizmet yedekleme ve kullanılabilir kalmasını sağlayarak üstlenir.

C# İÇİN:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Yerleştirme kısıtlamaları için her farklı adlandırılmış hizmet örneğinde belirtilir. Güncelleştirmeler her zaman yer alan (üzerine yaz) ne daha önce belirtildi.

Küme tanımı, bir düğümde özelliklerini tanımlar. Bir düğümün özelliklerini değiştirme, bir küme yapılandırmasını yükseltme gerektirir. Bir düğümün özellikleri yükseltme yeni özelliklerini raporlamak için yeniden başlatma için etkilenen her düğüme gerektirir. Bu toplu yükseltmeler, Service Fabric tarafından yönetilir.

## <a name="describing-and-managing-cluster-resources"></a>Açıklayan ve küme kaynaklarını yönetme
Tüm orchestrator'ın en önemli işlerden biri kümedeki kaynak tüketimini yönetmenize yardımcı olmaktır. Küme kaynaklarını yönetme, birkaç farklı bir şey anlamına gelebilir. İlk olarak, burada makine aşırı yüklenmiş olmayan sağlamaktır. Bu makineler, işleyebileceğinden daha fazla hizmet çalıştırmadığınız emin olmak anlamına gelir. İkinci olarak, Dengeleme ve Hizmetleri verimli bir şekilde çalışmasını için kritik olan en iyi duruma getirme yoktur. Maliyet etkin veya performans duyarlı hizmet teklifleri, diğerleri soğuk çalışırken sık erişimli olarak bazı düğümler izin veremez. Sık erişimli düğümleri kaynak çakışması ve performansın düşmesine neden ve israfı ve artan maliyetleri soğuk düğümleri temsil eder. 

Service Fabric kaynakları olarak temsil eden `Metrics`. Service Fabric tanımlamak istediğiniz herhangi bir mantıksal veya fiziksel kaynağa ölçümleridir. "WorkQueueDepth" veya "MemoryInMb" gibi şeyleri ölçümleri örnekleridir. Düğümler üzerinde Service Fabric yöneten Fiziksel kaynaklar hakkında bilgi için bkz [kaynak İdaresi](service-fabric-resource-governance.md). Özel Ölçümler ve kullanımları yapılandırma hakkında daha fazla bilgi için bkz: [bu makalede](service-fabric-cluster-resource-manager-metrics.md)

Ölçümler, yerleştirmelerin kısıtlamaları ve düğüm özellikleri farklıdır. Düğüm, düğümlerin kendilerini statik tanımlayıcıları özelliklerdir. Ölçümler, düğümleri olan kaynakları ve bir düğüm üzerinde çalıştırdığınızda hizmetlerini kullanma açıklanmaktadır. Bir düğüm özelliği "HasSSD" olabilir ve true veya false olarak ayarlanamıyor. Kullanılabilir alan miktarını, SSD ve ne kadar hizmetler tarafından kullanılan "DriveSpaceInMb" gibi bir ölçüm olabilir. 

Gibi yerleştirme kısıtlamaları ve düğüm özellikleri için Service Fabric Küme Kaynak Yöneticisi ölçüm adları anlamı anlamıyor olduğunu unutmayın. Ölçüm adları yalnızca dizelerdir. Birim belirsiz olabilir, oluşturduğunuz ölçüm adları bir parçası olarak bildirmek için iyi bir uygulamadır.

## <a name="capacity"></a>Kapasite
Tüm kaynak etkinleştirdiyseniz *Dengeleme*, Service Fabric'in Küme Kaynak Yöneticisi hala olun hiçbir düğüm, kapasite aşımı sona erdiği. Kapasite aşımları yönetme küme çok dolu veya iş yükü düğümlerinden büyük sürece mümkündür. Kapasite olan başka bir *kısıtlaması* küme kaynak yöneticisi olan bir düğüm bir kaynaktan ne kadar anlamak için kullanır. Kalan kapasite da küme için bir bütün olarak izlenir. Kapasite hem de hizmet düzeyinde tüketimi açısından ölçümleri cinsinden ifade edilir. Örneğin, ölçüm "ClientConnections" olabilir ve belirli bir düğümün 32768 "ClientConnections" için bir kapasiteye sahip olabilir. Diğer düğümler, düğüm üzerinde çalıştıran bazı hizmet varsayalım, şu anda "ClientConnections" ölçümün 32256 tüketiyor diğer sınırlamaları olabilir.

Çalışma zamanı sırasında Kalan kapasite kümedeki ve düğümlerdeki Küme Kaynak Yöneticisi izler. Kapasite izlemek için her bir hizmetin kullanım hizmetin çalıştırıldığı düğüme ait kapasiteden Küme Kaynak Yöneticisi çıkarır. Bu bilgileri kullanarak Service Fabric Küme Kaynak Yöneticisi yerleştirin veya kapasite aşımı düğümlerini yapılmaz, böylece çoğaltmaları taşımak nereye ekleyeceğimi.

<center>

![Küme düğümlerini ve kapasite][Image7]
</center>

C# İÇİN:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "ClientConnections";
metric.PrimaryDefaultLoad = 1024;
metric.SecondaryDefaultLoad = 0;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

Küme bildiriminde tanımlanan kapasiteler görebilirsiniz:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan. 

```json
"nodeTypes": [
    {
        "name": "NodeType03",
        "capacities": {
            "ClientConnections": "65536",
        }
    }
],
```

Yaygın olarak bir hizmetin değişiklikleri dinamik olarak yükleyin. Yineleme yükü "ClientConnections" nun 2048 olarak 1024'ten değiştirildi, ancak ardından çalıştığı üzerinde düğüm yalnızca bu ölçümü için kalan 512 kapasitesi olduğu varsayalım. Bu düğümde yeterli alan olmadığından artık, çoğaltma veya örneğinin yerleştirme geçersiz. Küme Kaynak Yöneticisi etkisini göstermeye ve düğüm kapasitesi aşağıda geri almak vardır. Bu, bir veya daha fazla yineleme veya örnekleri, bu düğümden diğer düğümlere taşınmasını göre kapasite aşımı düğümü üzerindeki yükü azaltır. Çoğaltmaları taşırken, küme kaynak yöneticisi bu hareketleri maliyetini en aza indirmek çalışır. Taşıma maliyeti ele alınmıştır [bu makalede](service-fabric-cluster-resource-manager-movement-cost.md) Küme Kaynak Yöneticisi hakkında daha fazla yeniden Dengeleme stratejileri ve kuralları tanımlanmıştır [burada](service-fabric-cluster-resource-manager-metrics.md).

## <a name="cluster-capacity"></a>Küme kapasitesi
Service Fabric Küme Kaynak Yöneticisi, genel kümenin çok dolu nasıl korunsun mu? İyi yok çok dinamik yük ile bunu yapabilirsiniz. Küme Kaynak Yöneticisi tarafından gerçekleştirilen eylemler bağımsız olarak kendi yük depo hizmeti olabilir. Sonuç olarak, yarın ünlü olduğunda bugün payı bolca kümenizle underpowered olabilir. Sorunları önlemek için yerleşik bazı denetimler vardır. Bununla birlikte. Yapabiliriz ilk şey, kümenin dolmasına neden olan yeni iş yüklerini oluşturulmasını önlemek ' dir.

Durum bilgisi olmayan hizmet oluşturma ve onunla ilişkili bazı yük sahip varsayalım. Hizmet hakkında "DiskSpaceInMb" ölçüm önemser varsayalım. Ayrıca, hizmetin her örneği için "DiskSpaceInMb", beş birimi gittiğini varsayalım. Hizmet üç örneklerini oluşturmak istiyorsunuz. Harika! Bu nedenle 15 birimleri "siparişi için bize bile kümedeki bulunması DiskSpaceInMb" ihtiyacımız anlamına bu hizmet örnekleri oluşturmak mümkün olmayacaktır. Küme Kaynak Yöneticisi sürekli kapasitesi ve her bir ölçüm kullanımını hesaplar kümedeki kalan kapasite belirleyebilirsiniz. Yeterli alan yoksa, Küme Kaynak Yöneticisi oluşturma hizmeti çağrısı reddeder.

Gereksinim yalnızca 15 birimleri kullanılabilir olduğundan, birçok farklı yöntem bu alanı ayrılamadı. Örneğin, olabilir bir kalan birim kapasitesi 15 farklı düğümlere ya da kapasite beş farklı düğümlere üç kalan birimi. Küme Kaynak Yöneticisi, bu yüzden beş birimleri kullanılabilir üç düğümü şeyler düzenleyebilirsiniz hizmeti yerleştirir. Küme yeniden düzenleme küme dolmak veya mevcut hizmetlerden herhangi bir nedenden dolayı birleştirilmiş olamaz sürece genellikle mümkündür.

## <a name="buffered-capacity"></a>Arabelleğe alınan kapasite
Başka bir özellik kümesi Kaynak Yöneticisi'nin arabelleğe alınan kapasitesidir. Bazı genel düğüm kapasitesinden büyük bir kısmının ayırma sağlar. Bu kapasite arabelleği yalnızca Hizmetleri yükseltme ve düğüm hataları sırasında yerleştirmek için kullanılır. Arabelleğe alınan kapasite ölçüm tüm düğümler için genel olarak belirtilir. Kapasite çekme değeri, hata ve yükseltme etki alanlarında kümedeki sahip sayısı için kullanılan bir işlevdir. Daha fazla hata ve yükseltme etki alanlarında, daha düşük bir sayı için arabelleğe alınan kapasite seçebilir anlamına gelir. Daha fazla etki alanı varsa, kümenizi yükseltme ve hataları sırasında kullanılamaz olarak küçük miktarlarda bekleyebilirsiniz. Düğüm kapasitesi bir ölçüm için ayrıca belirttiyseniz, arabelleğe alınmış kapasite belirtme yalnızca mantıklıdır.

Arabelleğe alınan kapasitesini belirlemek nasıl bir örnek aşağıda verilmiştir:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan:

```json
"fabricSettings": [
  {
    "name": "NodeBufferPercentage",
    "parameters": [
      {
          "name": "SomeMetric",
          "value": "0.15"
      },
      {
          "name": "SomeOtherMetric",
          "value": "0.20"
      }
    ]
  }
]
```

Küme bir ölçüm için arabelleğe alınan kapasite yetersiz olduğunda yeni hizmetler oluşturma başarısız olur. Arabellek korumak için yeni hizmetler oluşturulmasını önleme, kapasite aşımı gitmek düğümleri yükseltmeleri ve hataları neden olmadığını sağlar. Arabelleğe alınan kapasite isteğe bağlıdır ancak bir ölçüm için bir kapasite tanımlayan herhangi bir küme önerilir.

Küme Kaynak Yöneticisi, bu yük bilgiler sunar. Her bir ölçüm için bu bilgileri içerir: 
  - arabelleğe alınan kapasite ayarları
  - Toplam Kapasite
  - Geçerli tüketimi
  - Dengeli veya her ölçümü olup olmadığı kabul edilir
  - Standart sapma hakkındaki istatistiklerdir
  - en iyi olan düğümleri ve en az yük  
  
Aşağıda, çıktının bir örneği bakın:

```posh
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2016 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2016 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a>Sonraki adımlar
* Mimari ve bilgi akışını içinde Küme Kaynak Yöneticisi hakkında daha fazla bilgi için [bu makalede](service-fabric-cluster-resource-manager-architecture.md)
* Birleştirme ölçümleri tanımlama, yaymak yerine düğümlerde yük birleştirmek için bir yoludur. Birleştirme yapılandırma konusunda bilgi için bkz [bu makalede](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* En baştan başlatmak ve [için Service Fabric Küme Kaynak Yöneticisi giriş yapın](service-fabric-cluster-resource-manager-introduction.md)
* Küme Kaynak Yöneticisi yönetir ve kümedeki yük dengeleyen hakkında bilgi almak için makalesine göz atın [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png
