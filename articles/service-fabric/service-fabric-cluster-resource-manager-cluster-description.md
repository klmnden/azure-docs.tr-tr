---
title: Küme Kaynak Yöneticisi'ni kullanarak bir kümeyi tanımlama | Microsoft Docs
description: Service Fabric kümesi, hata etki alanları ve yükseltme etki alanları, düğüm özellikleri ve küme kaynak yöneticisi için düğüm kapasiteleri belirterek açıklanmaktadır.
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
ms.openlocfilehash: 22ccb21a208bbe8e825bff9f7602bfca05990816
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67271653"
---
# <a name="describe-a-service-fabric-cluster-by-using-cluster-resource-manager"></a>Küme Kaynak Yöneticisi'ni kullanarak bir Service Fabric kümesini açıklar.
Azure Service Fabric Küme Kaynak Yöneticisi özelliği bir kümesini tanımlamak için çeşitli mekanizmalar sağlar:

* Hata etki alanları
* Yükseltme etki alanları
* Düğüm özellikleri
* Düğüm kapasiteleri

Çalışma zamanı sırasında küme kaynak yöneticisi, kümede çalışan hizmetler yüksek kullanılabilirliğini sağlamak için bu bilgileri kullanır. Bu önemli kurallar zorlarken ayrıca küme içindeki kaynak tüketimi iyileştirmek çalışır.

## <a name="fault-domains"></a>Hata etki alanları
Hata etki alanı tüm Eşgüdümlü hatası alanıdır. Tek bir makine, bir hata etki alanıdır. Hatalı NIC bellenimi için sürücü hatalarına gücünden çeşitli nedenlerden dolayı kendi tedarik hatalarda başarısız olabilir. 

Aynı Ethernet anahtara bağlı makineler aynı hata etki alanındadır. Bu nedenle tek bir kaynak güç veya tek bir konumda paylaşan makinelerdir. 

Donanım hatalarına çakıştırmayı doğal olduğundan, hata etki alanları kendiliğinden hiyerarşiktir. Bunlar, Service fabric'te bir URI'leri olarak temsil.

Service Fabric Hizmetleri güvenli bir şekilde yerleştirmek için bu bilgileri kullandığından hata etki alanlarını doğru şekilde ayarlanması önemlidir. Service Fabric Hizmetleri gitmek bir hizmet (bazı bileşen hatasından kaynaklanır) hata etki alanı kaybına neden olacak şekilde yerleştirmek istememektedir. 

Service Fabric, Azure ortamında doğru sizin adınıza kümedeki düğümlerin yapılandırmak için ortam tarafından sağlanan hata etki alanı bilgilerini kullanır. Örnekleri tek başına Service fabric için hata etki alanları küme ayarladığınız zaman tanımlanır. 

> [!WARNING]
> Service Fabric için sağlanan hata etki alanı bilgilerin doğru olduğunu önemlidir. Örneğin, Service Fabric kümenizin düğümlerine 5 fiziksel konakları üzerinde çalışan, 10 sanal makineler içinde çalıştığını kabul edelim. Bu durumda olsa bile 10 sanal makineler, vardır yalnızca 5 farklı (üst düzey) hata etki alanları. Fiziksel ana bilgisayarları başarısız olursa, Vm'leri Eşgüdümlü hatasıyla karşılaşan çünkü aynı fiziksel ana bilgisayarı paylaşan Vm'leri aynı kök hata etki paylaşmak neden olur.  
>
> Service Fabric değiştirme için bir düğüm arıza etki alanını bekliyor. Sanal makinelerin yüksek kullanılabilirlik gibi emin olmanın başka mekanizmalar [HA-VMs](https://technet.microsoft.com/library/cc967323.aspx), Service Fabric ile çakışmalara neden olabilir. Bu mekanizmalar saydam geçiş sanal makinelerin bir konaktan diğerine kullanın. Bunlar yeniden yapılandırın veya VM içinde çalışan kod bildir kullanmayın. Bu nedenle, oldukları *desteklenmiyor* Service Fabric çalıştırmak için ortamları olarak kümeleri. 
>
> Service Fabric, işe yüksek kullanılabilirlik yalnızca teknoloji olacak. Dinamik VM geçişi ve SAN gibi mekanizmaların gerekli değildir. Bu mekanizmalar, Service Fabric ile birlikte kullanılması durumunda bunlar _azaltmak_ uygulama kullanılabilirliği ve güvenilirliği. Bunlar karmaşıklık tanıtmak, hata merkezi kaynakları eklemek ve de Service Fabric ile çakışan güvenilirlik ve kullanılabilirlik stratejileri kullanın nedenidir. 
>
>

Aşağıdaki grafikte, biz hata etki alanlarına katkıda bulunabilir ve neden tüm farklı hata etki alanları listesinde tüm varlıkları rengi. Bu örnekte, veri merkezleri ("DC"), raflar ("R") ve dikey pencereleri ("B") sahip. Her dikey pencerede birden fazla sanal makine barındırıyorsa, hata etki alanı hiyerarşideki başka bir katmanı olabilir.

<center>

![Hata etki alanları düzenlenmiş düğümleri][Image1]
</center>

Çalışma zamanı sırasında Service Fabric Küme Kaynak Yöneticisi hata etki alanları kümedeki göz önünde bulundurur ve düzenleri planları. Durum bilgisi olan yinelemeler ya da durum bilgisi olmayan hizmet örnekleri ayrı hata etki alanlarında bulunmaları dağıtılır. Hata etki alanları arasında hizmet dağıtma, hata etki alanı hiyerarşinin herhangi bir düzeyde başarısız olduğunda hizmetinin kullanılabilirliğini tehlikede olmayan sağlar.

Küme Kaynak Yöneticisi kaç Katmanlar vardır hata etki alanı hiyerarşisinde önemli değil. Hiyerarşinin herhangi bir bölümünün kaybı içinde çalışan hizmetleri etkilemediğinden emin olmak çalışır. 

Düğümler aynı sayıda hata etki alanı hiyerarşisinde derinliği her düzeyinde ise en iyisidir. Hata etki alanları "ağacı", kümenizde dengesiz, küme kaynak hizmetlerinin en iyi ayırma anlamaya Yöneticisi daha zor olur. İmbalanced hata etki alanı düzenleri, bazı etki alanlarına kaybı daha fazla diğer etki alanı Hizmetleri'nin kullanılabilirliğini etkileyen anlamına gelir. Sonuç olarak, küme kaynak yöneticisi olarak iki amaçları arasında kaldırılır: 

* Bu makineler "heavy" Bu etki alanında hizmetleri üzerinde yerleştirerek kullanmak istiyor. 
* Bir etki alanı kaybı sorunlara neden olmayan diğer etki alanlarında Hizmetleri lıbcmtd.lib istiyor. 

İmbalanced etki alanları ne gibi görünüyor? Aşağıdaki diyagramda iki farklı küme düzenleri gösterilmektedir. İlk örnekte, düğümler, hata etki alanları arasında eşit şekilde dağıtılır. İkinci örnekte, bir hata etki alanı, başka bir hata etki alanı daha pek çok daha fazla düğüm var. 

<center>

![İki farklı küme düzenleri][Image2]
</center>

Azure'da hangi hata etki alanı bir düğüm içeriyor. seçim sizin için yönetilir. Ancak, sağlamanız düğüm sayısına bağlı olarak, hala bazılarında bunlara fazla düğümünüz var. hata etki alanları ile kalabilirsiniz. 

Örneğin, beş hata etki alanları kümesinde sahip, ancak yedi düğüm düğüm türü için sağlama söyleyin (**NodeType**). Daha fazla düğüm ile bu durumda, ilk iki hata etki alanları edersiniz. Daha fazla dağıtmak devam ederseniz **NodeType** örnekler yalnızca birkaç örnekleri ile sorun yarışacağından alır. Bu nedenle, her düğüm türü düğüm sayısını bir birden çok hata etki alanları sayısı olduğunu öneririz.

## <a name="upgrade-domains"></a>Yükseltme etki alanları
Yükseltme etki alanları, Service Fabric küme kaynak yöneticisi küme düzenini anlamanıza yardımcı olan başka bir özelliğidir. Yükseltme etki alanları aynı anda yükseltilir düğümleri kümesini tanımlar. Yükseltme etki alanları, Küme Kaynak Yöneticisi anlamak ve yükseltme gibi yönetim işlemlerini düzenlemek yardımcı olur.

Yükseltme etki alanları çok hata etki alanları gibi ancak birkaç önemli farklar da bulunur. İlk olarak, alanları, Eşgüdümlü donanım hatalarının, hata etki alanlarını tanımlayın. Yükseltme etki alanları, diğer taraftan, ilke tarafından tanımlanır. Kaç tane sayı dikte ortam yapmasına izin vermek yerine istediğiniz karar alın. Düğümleri olarak sayıda yükseltme etki alanları olabilir. Başka bir hata etki alanları ve yükseltme etki alanları arasında yükseltme etki alanı hiyerarşik olmadığından farktır. Bunun yerine, bunlar gibi basit bir etiketin daha fazla. 

Aşağıdaki diyagramda, üç hata etki alanlarında şeritli üç yükseltme etki alanları gösterir. Ayrıca, burada her farklı hata ve yükseltme etki alanı içinde sona eriyor bir olası yerleşimini üç farklı kopyaya için bir durum bilgisi olan hizmet gösterir. Bu yerleşimi hata etki alanı hizmeti yükseltmesi sırasında ortasında kaybı sağlar ve kod ve verilerin bir kopyasını çözümlenmedi.  

<center>

![Yerleştirme ile hata ve yükseltme etki alanları][Image3]
</center>

Artıları ve eksileri sahip çok sayıda yükseltme etki alanları için vardır. Daha fazla yükseltme etki alanları, yükseltmenin her bir adım daha ayrıntılı ve daha az sayıda düğüm veya hizmetleri etkiler anlamına gelir. Daha az değişim sıklığı sisteme Giriş bir anda taşımak daha az hizmetleri vardır. Yükseltme sırasında sunulan herhangi bir sorundan etkilenen hizmetinin daha az olduğundan bu güvenilirliği artırmak için eğilimlidir. Daha fazla yükseltme etki alanları diğer düğümlerdeki yükseltme etkisini işlemek için daha az arabellek gerektiğini anlamına gelir. 

Beş yükseltme etki alanları varsa, örneğin, her düğüm yaklaşık yüzde 20 oranında trafiğinizin ele alınıyor. Bu yükseltme etki alanı yükseltmesi aşağı duruma getirmeniz gerekiyorsa, bu Yük genellikle bir yere gitmek gerekir. Dört yükseltme etki alanları kalan olduğundan, her yaklaşık yüzde 5 ' toplam trafiğin yer olması gerekir. Daha fazla yükseltme etki alanları, kümedeki düğümler üzerinde daha az arabellek gerektiğini anlamına gelir. 

10 yükseltme etki alanları yerine tablonuz varsa göz önünde bulundurun. Bu durumda, her bir yükseltme etki alanı toplam trafiğin yalnızca yaklaşık yüzde 10 işleme. Bir yükseltme adımları, kümeye aracılığıyla her etki alanı yalnızca yaklaşık toplam trafik 1.1 yüzde yer olması gerekir. Daha az ayrılmış kapasite gerektiğinden fazla yükseltme etki alanları genellikle düğümlerinizi yüksek kullanımı, çalıştırmanıza olanak tanır. Aynı hata etki alanları için geçerlidir.  

Çok sayıda yükseltme etki alanları yaşama dezavantajı, yükseltmeleri uzun eğilimindedir ' dir. Service Fabric, bir yükseltme etki alanını tamamlandıktan ve bir yükseltmeye başlatmadan önce denetimlerini gerçekleştiren kısa bir süre bekler. Bu gecikme, yükseltmeye devam etmeden önce yükseltme tarafından sunulan olan algılama sorunları etkinleştirin. Artırabilen kabul edilebilir olduğundan, hatalı değişiklikleri bir anda hizmeti, çok fazla etkilenmesini önler.

Çok az sayıda yükseltme etki alanları varlığını birçok olumsuz etkilere sahiptir. Her bir yükseltme etki alanı olsa da aşağı ve yükseltilmekte olan genel kapasite büyük bir kısmı kullanılamaz. Örneğin, yalnızca üç yükseltme etki alanları varsa, aynı anda yaklaşık üçte birinin hizmet veya küme kapasitesi yönlendiriyoruz. İş yükünü işlemek için yeterli kapasite kümenizi geri kalanında gerektiğinden çok hizmetinizin aşağı aynı anda sahip arzu değil. Normal işlem sırasında anlamına o arabelleğe koruyun, düğümleri aksi durumdakinden daha az yüklü olan. Bu, hizmetiniz çalıştırma maliyetini artırır.

Gerçek bir ortam veya nasıl örtüşecek üzerindeki kısıtlamaları hatası veya yükseltme etki alanlarının sayısı sınırı yoktur. Ancak, ortak desenler vardır:

- 1:1, hata etki alanları ve yükseltme etki alanları eşlendi
- Bir yükseltme etki alanı başına düğüm (fiziksel veya sanal işletim sistemi örneği)
- Burada hata etki alanları ve yükseltme etki alanlarının bir matris çizgileri genellikle çalışan makineler ile form "şeritli" veya "matris" modeli

<center>

![Hata ve yükseltme etki alanlarının düzenleri][Image4]
</center>

Hangi düzenin seçmek için en iyi yanıt yoktur. Her avantajları ve dezavantajları vardır. Örneğin, 1FD:1UD modeli ayarlamak basit bir işlemdir. Düğüm model başına bir yükseltme etki alanı için hangi kişi gibi en çok kullanılan modelidir. Yükseltme sırasında her düğüm bağımsız olarak güncelleştirilir. Bu, nasıl küçük makineler kümesini el ile geçmişte yükseltildiği için benzer.

Burada, hata etki alanları ve yükseltme etki alanlarının bir tablo oluşturur ve düğümleri çapraz başlangıç yerleştirilir FD/UD matris en yaygın modelidir. Bu, varsayılan olarak Azure Service Fabric kümelerinde kullanılan modelidir. Birçok düğüm içeren kümeler için her şeyi gibi yoğun matris desen aranıyor sona erer.

> [!NOTE]
> Azure'da barındırılan Service Fabric kümeleri varsayılan strateji değiştirmeyi desteklemez. Yalnızca tek başına kümeler, özelleştirme sunar.
>

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Hata ve yükseltme etki alanı kısıtlamaları ve bunun sonucunda oluşan davranışı
### <a name="default-approach"></a>Varsayılan yaklaşımı
Varsayılan olarak, Küme Kaynak Yöneticisi hata ve yükseltme etki alanları arasında dengeli Hizmetleri tutar. Bu olarak modellenir bir [kısıtlaması](service-fabric-cluster-resource-manager-management-integration.md). Hata ve yükseltme etki alanları durumları kısıtlaması: "Bir verili hizmet bölümü için hiçbir zaman olmalıdır bir fark hiyerarşisinin aynı düzeyde iki tüm etki alanları arasında hizmet nesneleri (durum bilgisi olmayan hizmet örneği veya durum bilgisi olan hizmet çoğaltmalar) sayısını birden büyük."

Bu kısıtlamayı bir "en büyük fark" garantisi sağladığını varsayalım. Hata ve yükseltme etki alanları için kısıtlaması belirli taşır veya kuralı ihlal düzenlemeleri engeller.

Örneğin, beş hata etki alanları ve beş yükseltme etki alanları ile yapılandırılmış altı düğümü olan bir küme sahip olduğunuzu düşünelim.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

Artık bir hizmetle oluştururuz varsayalım bir **TargetReplicaSetSize** (veya bir durum bilgisi olmayan hizmet **Instancecount**) beş değeri. Çoğaltmaları N1 N5 gelirsiniz. Aslında, N6 hiçbir zaman kaç bu oluşturduğunuz gibi hizmetleri bağımsız olarak kullanılır. Peki ama neden? Geçerli düzeni ve N6 seçilirse ne olacağını arasındaki fark bakalım.

İşte, yapılandırdığımıza düzenini ve çoğaltmaları hata ve yükseltme etki alanı başına toplam sayısı:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1\. |1\. |1\. |1 |- |

Bu düzen, hata etki alanı ve yükseltme etki alanı başına düğüm açısından dengelenir. Ayrıca, hata ve yükseltme etki alanı başına çoğaltma sayısı bakımından dengelenir. Her etki düğümler aynı sayıda ve çoğaltmaları aynı sayıda sahiptir.

Şimdi biz N6 N2 yerine kullanmışsınız ne olacağını konumunda göz atalım. Nasıl çoğaltmaları ardından dağıtılması?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1\. |1\. |1 |- |

Bu düzen, hata etki alanı kısıtlaması için "en büyük fark" garantisi, bizim tanımını ihlal ediyor. FD1 sıfır sahipken FD0 iki çoğaltma vardır. Bir en büyük fark büyük olan iki, toplam FD0 FD1 arasındaki fark var. Küme Kaynak Yöneticisi kısıtlamayı ihlal olduğundan, bu düzenleme izin vermez. Benzer şekilde, biz N2 ve (yerine N1 ve N2) N6 kaldırdıysa biz alın:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1\. |1\. |1\. |1 |- |

Bu düzen açısından hata etki alanlarında dengelenir. Ancak sıfır çoğaltmaları UD0 vardır ve iki UD1 olduğundan yükseltme etki alanı kısıtlamasını ihlal şimdi. Bu düzen ayrıca geçersiz ve küme kaynak yöneticisi tarafından çekilmesi olmaz.

Durum bilgisi olan yinelemeler ya da durum bilgisi olmayan örnekleri dağıtılması için bu yaklaşım, en iyi olası hata toleransı sağlar. Bir etki alanı kalırsa, çoğaltmaları/örnek en az sayıda kaybolur. 

Öte yandan, bu yaklaşım çok katı ve kümenin tüm kaynakları kullanmak için izin verme. Belirli bir küme yapılandırmaları için belirli düğümler kullanılamaz. Bu uyarı iletilerini kaynaklanan hizmetlerinizi yerleştirmemeye Service Fabric neden olabilir. Önceki örnekte, küme düğümleri bazıları olamaz (örnekte N6) kullanılır. Bu küme (N10 N7) düğümler eklediyseniz bile çoğaltmaları/örnek hata ve yükseltme etki alanı kısıtlamaları nedeniyle yalnızca N1 – N5 üzerinde yerleştirilir. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | |N10 |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | |N9 |N5 |



### <a name="alternative-approach"></a>Alternatif bir yaklaşım

Küme Kaynak Yöneticisi, hata ve yükseltme etki alanları için kısıtlama başka bir sürümünü destekler. Yerleştirme, minimum düzeyde güvenlik hala güvence altına almak çalışırken sağlar. Alternatif kısıtlama gibi belirtilebilir: "Bir verili hizmet bölümü için etki alanları arasında çoğaltma dağıtım bölüm çekirdek kayıp saptanmamış emin olmanız gerekir." Bu kısıtlama "çekirdek güvenli" garantisi sağladığını varsayalım. 

> [!NOTE]
> Bir durum bilgisi olan hizmet için tanımlarız *çekirdek kayıp* bölüm çoğaltmalarını çoğunu basılı olduğunda aynı anda bir durumda. Örneğin, varsa **TargetReplicaSetSize** beştir, çekirdek herhangi üç kopyaya kümesini temsil eder. Benzer şekilde, varsa **TargetReplicaSetSize** altı dört çoğaltmaları çekirdek için gerekli olan. Bölüm normal olarak çalışmaya devam etmek istiyorsa her iki durumda da, ikiden fazla çoğaltma aşağı aynı anda olabilir. 
>
> Bir durum bilgisi olmayan hizmet için de yoktur *çekirdek kayıp*. Durum bilgisi olmayan hizmetler örnekleri çoğunu aşağı git aynı anda bile normal olarak çalışmaya devam eder. Bu nedenle, bu makalenin geri kalanında, durum bilgisi olan hizmetler odaklanacağız.
>

Önceki örneğe geri dönelim. Kısıtlama "çekirdek güvenli" sürümü ile tüm üç düzenleri geçerli olacaktır. İkinci düzende FD0 başarısız oldu veya üçüncü düzende UD1 başarısız olsa bile bölüm çekirdek yine de gerekir. (Çoğaltmaları çoğunu hala yukarı olacaktır.) Bu kısıtlamanın sürümüyle N6 neredeyse her zaman kullanılabilir.

"Güvenli çekirdek" yaklaşım "en büyük fark" yaklaşım göre daha fazla esneklik sağlar. Neden neredeyse her küme topolojisi geçerli çoğaltma dağıtımları daha kolay olmasıdır. Bazı hataları daha da kötüsü olduğundan ancak, bu yaklaşım en iyi hataya dayanıklılık özelliklerini garanti edemez. 

En kötü durum senaryosunda, tek bir etki alanı ve ek bir çoğaltma hatası ile çoğaltmalar çoğunu kaybolabilir. Örneğin, beş çoğaltmalar veya örnekleri ile çekirdek kaybetmek için gerekli üç hatadan yerine yalnızca iki hataları olan çoğu artık kaybedebilir. 

### <a name="adaptive-approach"></a>Uyarlanabilir bir yaklaşım
Her iki yaklaşım, güçlü ve zayıf olduğundan, bu iki stratejiler birleştiren uyarlanabilir bir yaklaşım tanıttık.

> [!NOTE]
> Bu sürüm 6.2 Service Fabric ile başlayarak varsayılan davranıştır. 
> 
> Uyarlanabilir bir yaklaşım, varsayılan "en büyük fark" mantığı kullanır ve yalnızca gerekli olduğunda "çekirdek güvenli" mantıksal anahtarları. Küme Kaynak Yöneticisi otomatik olarak hangi stratejiyi nasıl kümeyi ve Hizmetleri yapılandırılmış en bakarak gerekli olduğunu belirler.
> 
> Küme Kaynak Yöneticisi, bir hizmeti iki Bu koşullar için "temel çekirdek" mantıksal kullanmanız gerekir:
>
> * **TargetReplicaSetSize** hizmet hata etki alanları sayısı ve yükseltme etki alanlarının sayısı ile kalansız için.
> * Düğüm sayısına eşit veya hata etki alanı yükseltme etki alanlarının sayı ile çarpılan küçük.
>
> Çekirdek kayıp durum bilgisi olmayan hizmetler için geçerli değildir olsa bile Küme Kaynak Yöneticisi durum bilgisiz ve durum bilgisi olan hizmetler için bu yaklaşımı kullanın aklınızda size aittir.

Önceki örneğe geri dönelim ve bir küme düğümü artık sahip olduğunu varsayın. Küme yine beş hata etki alanları ve beş yükseltme etki alanları ile yapılandırılmış ve **TargetReplicaSetSize** kümede barındırılan hizmet değerini beş kalır. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | | |N5 |

Gerekli tüm koşulların karşılandığından çünkü Küme Kaynak Yöneticisi hizmetini dağıtmak, "çekirdek tabanlı" mantığı kullanılacak. Bu, N6 N8 kullanımını etkinleştirir. Bu durumda bir olası hizmet dağıtım şuna benzeyebilir:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | | | |0 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |2 |1 |1 |0 |1 |- |

Hizmetinizin **TargetReplicaSetSize** değeri azaltıldı dört (örneğin), Küme Kaynak Yöneticisi, bu değişikliği görürsünüz. "En büyük fark" mantığı kullanarak devam edecek **TargetReplicaSetSize** artık bölünebilen hata etki alanları ve yükseltme etki alanlarının sayısına göre değil. Sonuç olarak, belirli yineleme hareketleri kalan dört çoğaltmaları N1 N5 düğümlerini dağıtmak için meydana gelir. Hata etki alanı ve yükseltme etki alanı mantığı "en büyük fark" sürümü bu şekilde, ihlal değil. 

Önceki düzendeki varsa **TargetReplicaSetSize** değeri ise beş ve N1, kümeden kaldırılır, yükseltme etki alanlarının sayısı dört eşit olur. Küme Kaynak Yöneticisi yeniden başlatan yükseltme etki alanlarının sayısı hizmetin eşit olarak bölmeye değil çünkü "en büyük fark" mantığı kullanılarak **TargetReplicaSetSize** artık değeri. Sonuç olarak, çoğaltma yeniden oluşturulduğunda R1, böylece hata ve yükseltme etki alanı kısıtlaması ihlal edilmediğini üzerinde N4 yerleşmesi sahiptir.

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |Yok |Yok |Yok |Yok |Yok |Yok |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | |R1 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1\. |1\. |1\. |1 |- |

## <a name="configuring-fault-and-upgrade-domains"></a>Hata ve yükseltme etki alanlarını yapılandırma
Azure'da barındırılan Service Fabric dağıtımlarda, hata etki alanları ve yükseltme etki alanları otomatik olarak tanımlanır. Service Fabric alır ve Azure ortam bilgileri kullanır.

Kendi kümenizi oluşturuyorsanız (veya belirli bir topoloji geliştirme çalıştırmak istediğiniz varsa), hata etki alanı sağlayın ve kendiniz yükseltme etki alanı bilgileri. Bu örnekte, üç veri merkezleri (her üç Raflarla) kapsayan bir dokuz düğümlü yerel geliştirme kümesi tanımlarız. Bu küme, ayrıca bu üç veri merkezleri arasında şeritli üç yükseltme etki alanları bulunur. ClusterManifest.xml yapılandırmasının bir örnek aşağıda verilmiştir:

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one box/one single server -->
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

Bu örnek için tek başına dağıtımlarında ClusterConfig.json kullanır:

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
> Azure Resource Manager aracılığıyla kümeleri tanımlarken Azure atar, hata etki alanı ve yükseltme etki alanlarında. Tanımı düğüm türleri ve sanal makine ölçek kümelerine için Azure Resource Manager şablonunuzu hata etki alanı veya yükseltme etki alanı hakkında bilgi içermez.
>

## <a name="node-properties-and-placement-constraints"></a>Düğüm özellikleri ve yerleştirme kısıtlamaları
Bazen (aslında çoğu zaman) belirli iş yükleri yalnızca belirli türdeki düğümleri üzerinde çalıştığından emin olmak istersiniz. Örneğin, bazı iş yükleri Gpu'lar veya SSD gerektirebilir ve diğerleri görünmeyebilir. 

Neredeyse her n katmanlı mimari belirli iş yükleri için donanım desteği, harika bir örnektir. Belirli makineler ön uç veya uygulamanın API Hizmet tarafı olarak hizmet ve istemcileri veya Internet'e açık. Genellikle, farklı donanım kaynakları olan farklı makineler işlem ve depolama katmanları iş işleyin. Genellikle bunlar _değil_ doğrudan istemciler veya Internet'e açık. 

Service Fabric, belirli donanım yapılandırmalarını temel çalıştırmak için bazı durumlarda, belirli iş yükleri gerekebilir bekliyor. Örneğin:

* Var olan bir n katmanlı uygulama bir Service Fabric ortamına "yükseltilmiş ve kaydırılacak" kaldırıldı.
* Bir iş yükü, performans, ölçek ve güvenlik yalıtımı nedeniyle belirli donanım üzerinde çalıştırılmalıdır.
* Bir iş yükü İlkesi veya kaynak tüketimi nedeniyle diğer iş yükleri gelen yalıtılmış olması gerekir.

Bu tür yapılandırmaları desteklemek için Service Fabric düğümleri için uygulayabileceğiniz etiketleri içerir. Bu etiketler adlı *düğümü özellikleri*. *Yerleştirme kısıtlamaları* olan bir veya daha fazla düğüm Özellikler'i seçin, tek tek hizmetlerine bağlı deyimleri. Yerleştirme kısıtlamaları Hizmetleri çalıştırdığı tanımlar. Kısıtlamaları kümesini genişletilebilir. Herhangi bir anahtar/değer çifti çalışabilir. 

<center>

![Bir küme düzeni için farklı iş yükleri][Image5]
</center>

### <a name="built-in-node-properties"></a>Yerleşik düğümü özellikleri
Service Fabric, bunları tanımlamanız gerekmez, otomatik olarak kullanılabilen bazı varsayılan düğümü özellikleri tanımlar. Her düğümde tanımlanan varsayılan Özellikler **NodeType** ve **NodeName**. 

Örneğin, yerleştirme kısıtlama olarak yazabilirsiniz `"(NodeType == NodeType03)"`. **NodeType** yaygın olarak kullanılan bir özelliktir. Bir makine türü ile 1:1 karşılık olmadığından yararlı olur. Her makine türü, geleneksel bir n katmanlı uygulama iş yükü türüne karşılık gelir.

<center>

![Yerleştirme kısıtlamaları ve düğümü özellikleri][Image6]
</center>

## <a name="placement-constraints-and-node-property-syntax"></a>Yerleştirme kısıtlamaları ve düğüm özelliği söz dizimi 
Düğüm özelliği için belirtilen değer bir dize olabilir, Boolean, veya signed long. Hizmet bildirimine bir yerleştirme denir *kısıtlaması* çünkü burada hizmet kümesinde çalışabilen kısıtlar. Kısıtlama, küme düğümü özelliklerinde çalışır herhangi bir Boole ifadesi olabilir. Bu Boolean ifadeler geçerli Seçici verilmiştir:

* Belirli ifadeler oluşturmak için koşullu denetler:

  | Deyimi | Sözdizimi |
  | --- |:---:|
  | "equal" | "==" |
  | "eşit değildir" | "!=" |
  | "büyüktür" | ">" |
  | "büyüktür veya eşittir" | ">=" |
  | "az" | "<" |
  | "az veya buna eşit" | "<=" |

* Boolean ifadeleri gruplandırma ve mantıksal işlemler için:

  | Deyimi | Sözdizimi |
  | --- |:---:|
  | "ve" | "&&" |
  | "veya" | "&#124;&#124;" |
  | "not" | "!" |
  | "olarak tek bir deyim grubunu" | "()" |

Temel kısıtlama ifadeleri bazı örnekleri aşağıda verilmiştir:

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Burada genel yerleştirme kısıtlaması ifadesi "True" olarak değerlendirir düğümler üzerinde yerleştirilen hizmet olabilir. Tanımlanmış bir özellik yoksa düğümleri özelliği içeren herhangi bir yerleştirme kısıtlaması eşleşmiyor.

Aşağıdaki düğüm özellikleri ClusterManifest.xml bir düğüm türü için tanımlanmış olan varsayalım:

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

Aşağıdaki örnek, ClusterConfig.json Azure'da barındırılan kümeleri için tek başına dağıtımlarında veya Template.json için tanımlanan düğüm özellikleri gösterir. 

> [!NOTE]
> Azure Resource Manager şablonunuzda düğüm türü genellikle parametreli. Gibi görünür `"[parameters('vmNodeType1Name')]"` NodeType01 yerine.
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

Hizmet yerleşimi oluşturabilirsiniz *kısıtlamaları* aşağıdaki gibi bir hizmet için:

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// Add other required ServiceDescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

```PowerShell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Tüm düğümleri NodeType01 geçerliyse, bu düğüm türü kısıtlaması ile de seçebilirsiniz `"(NodeType == NodeType01)"`.

Bir hizmetin yerleştirme kısıtlamaları, çalışma zamanı sırasında dinamik olarak güncelleştirilebilir. Gerekiyorsa, bir hizmeti kümedeki yerleri, ekleyin ve gereksinimleri kaldırın ve benzeri. Service Fabric bile bu tip değişiklikler yapıldığında hizmet yedekleme ve kullanılabilir kalmasını sağlar.

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

```PowerShell
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Yerleştirme kısıtlamaları için her adlandırılmış hizmet örneğinde belirtilir. Güncelleştirmeler her zaman yer alan (üzerine yaz) ne daha önce belirtildi.

Küme tanımı, bir düğümde özelliklerini tanımlar. Bir düğümün özelliklerini değiştirme, küme yapılandırması için bir yükseltme gerektirir. Bir düğümün özellikleri yükseltme yeni özelliklerini raporlamak için yeniden başlatma için etkilenen her düğüme gerektirir. Service Fabric, bu yükseltmeyi yönetir.

## <a name="describing-and-managing-cluster-resources"></a>Açıklayan ve küme kaynaklarını yönetme
Tüm orchestrator'ın en önemli işlerden biri kümedeki kaynak tüketimini yönetmenize yardımcı olmaktır. Küme kaynaklarını yönetme, birkaç farklı bir şey anlamına gelebilir. 

İlk olarak, burada makine aşırı yüklenmiş olmayan sağlamaktır. Bu makineler, işleyebileceğinden daha fazla hizmet çalıştırmadığınız emin olmak anlamına gelir. 

İkinci olarak, yoktur Dengeleme ve en iyi duruma getirme, hangi Hizmetleri verimli bir şekilde çalışmasını için kritik öneme sahiptir. Uygun maliyetli veya performans açısından duyarlı hizmet teklifleri, diğerleri soğuk çalışırken sık erişimli olarak bazı düğümler izin veremez. Sık erişimli düğümleri kaynak çakışması ve performansın düşmesine neden. Soğuk düğümleri, israfı ve artan maliyetleri temsil eder. 

Service Fabric kaynakları olarak temsil eden *ölçümleri*. Service Fabric tanımlamak istediğiniz herhangi bir mantıksal veya fiziksel kaynağa ölçümleridir. "WorkQueueDepth" veya "MemoryInMb." örnekler ölçüm Düğümler üzerinde Service Fabric yöneten Fiziksel kaynaklar hakkında bilgi için bkz [kaynak İdaresi](service-fabric-resource-governance.md). Özel Ölçümler ve kullanımları yapılandırma hakkında daha fazla bilgi için bkz: [bu makalede](service-fabric-cluster-resource-manager-metrics.md).

Ölçümler, yerleştirme kısıtlamaları ve düğüm özellikleri farklıdır. Düğüm, düğümlerin kendilerini statik tanımlayıcıları özelliklerdir. Ölçümler, düğümleri olan kaynakları ve bir düğümde çalıştırdıklarında hizmetlerini kullanma açıklanmaktadır. Bir düğüm özelliği olabilir **HasSSD** ve true veya false olarak ayarlanabilir. Kullanılabilir alan miktarını, SSD ve ne kadar hizmetler tarafından kullanılan "DriveSpaceInMb." gibi bir ölçüm olabilir 

Tıpkı yerleştirme kısıtlamaları ve düğüm özellikleri için Service Fabric Küme Kaynak Yöneticisi hangi ölçümleri ortalama adlarını anlamıyor. Ölçüm adları yalnızca dizelerdir. Birim belirsiz olabilir, oluşturduğunuz ölçüm adları bir parçası olarak bildirmek için iyi bir uygulamadır.

## <a name="capacity"></a>Kapasite
Tüm kaynak etkinleştirdiyseniz *Dengeleme*, Service Fabric Küme Kaynak Yöneticisi hala olun hiçbir düğüm kapasitesi gider. Kapasite aşımları yönetme küme çok dolu veya iş yükü düğümlerinden büyük sürece mümkündür. Kapasite olan başka bir *kısıtlaması* küme kaynak yöneticisi olan bir düğüm bir kaynaktan ne kadar anlamak için kullanır. Kalan kapasite da küme için bir bütün olarak izlenir. 

Kapasite hem de hizmet düzeyinde tüketimi açısından ölçümleri cinsinden ifade edilir. Örneğin, ölçüm "ClientConnections" olabilir ve bir düğüm 32.768 "ClientConnections" için bir kapasiteye sahip olabilir. Diğer düğümlere diğer sınırlamaları olabilir. İlgili düğümde çalışan bir hizmet şu anda "ClientConnections." ölçümün 32.256 tüketiyor diyebilirsiniz

Çalışma zamanı sırasında Kalan kapasite kümedeki ve düğümlerdeki Küme Kaynak Yöneticisi izler. Kapasite izlemek için her hizmetin kullanım hizmetinin çalıştığı düğümün kapasiteden Küme Kaynak Yöneticisi çıkarır. Bu bilgileri kullanarak küme kaynak yöneticisi yerleştirin veya kapasite aşımı düğümlerini yapılmaz, böylece çoğaltmaları taşımak nereye ekleyeceğimi.

<center>

![Küme düğümlerini ve kapasite][Image7]
</center>

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

```PowerShell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

Küme bildiriminde tanımlanan kapasiteler görebilirsiniz. ClusterManifest.xml için bir örnek aşağıda verilmiştir:

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

Kapasitelerin ClusterConfig.json Azure'da barındırılan kümeleri için tek başına dağıtımlarında veya Template.json için tanımlanmış bir örnek aşağıda verilmiştir: 

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

Bir hizmet sık sık değişir dinamik olarak yükleyin. Söyleyin yineleme yükü "ClientConnections" 1024 2.048 için değiştirildi. Ardından çalıştığı üzerinde düğümü bu ölçümü için kalan tek 512 kapasitesine sahipti. Bu düğümde yeterli alan olmadığı için artık bu çoğaltma veya örneğinin yerleştirme, geçersiz. Küme Kaynak Yöneticisi, düğüm kapasitesi aşağıda geri almak vardır. Bu, bir veya daha fazla yineleme veya örnekleri, bu düğümden diğer düğümlere taşınmasını göre kapasite aşımı düğümü üzerindeki yükü azaltır. 

Küme Kaynak Yöneticisi, çoğaltmaları taşıma maliyeti en aza indirmek çalışır. Daha fazla bilgi edinebilirsiniz [taşıma maliyeti](service-fabric-cluster-resource-manager-movement-cost.md) ve yaklaşık [stratejiler ve kuralları yeniden Dengeleme](service-fabric-cluster-resource-manager-metrics.md).

## <a name="cluster-capacity"></a>Küme kapasitesi
Service Fabric Küme Kaynak Yöneticisi, genel kümenin çok dolu nasıl korunsun mu? Dinamik yük ile yok çok bunu yapabilirsiniz. Küme Kaynak Yöneticisi gereken eylemler bağımsız olarak kendi yük depo hizmeti olabilir. Sonuç olarak, bugün payı bolca kümenizle varsa bir depo yarın underpowered olabilir. 

Denetimleri Küme Kaynak Yöneticisi'nde, sorunları önlemeye yardımcı olur. Yapabileceğiniz ilk şey, kümenin dolmasına neden olan yeni iş yüklerini oluşturulmasını önlemek ' dir.

Diyelim ki bir durum bilgisi olmayan hizmet oluşturma ve onunla ilişkili bazı yük vardır. Hizmet hakkında "DiskSpaceInMb" ölçüm önemser. Hizmet, hizmetin her örneği için "DiskSpaceInMb" beş birimleri kullanacaktır. Hizmet üç örneklerini oluşturmak istiyorsunuz. 15 birimleri "DiskSpaceInMb" bile bu hizmet örnekleri oluşturmak için küme bulunması gereken anlamına gelir.

Küme Kaynak Yöneticisi sürekli kapasitesi ve her bir ölçüm kullanımını hesaplar kümedeki kalan kapasite belirleyebilirsiniz. Yeterli alan yoksa, Küme Kaynak Yöneticisi bir hizmet oluşturmak için çağrıyı reddeder.

Gereksinim yalnızca 15 birimleri kullanılabilir olduğundan, bu alan birçok farklı şekilde ayırabilirsiniz. Örneğin, olabilir bir kalan birim kapasitesi 15 farklı düğümlere ya da kapasite beş farklı düğümlere üç kalan birimi. Küme Kaynak Yöneticisi, böylece beş birimleri kullanılabilir üç düğümü şeyler düzenleyebilir, hizmet yerleştirir. Küme yeniden düzenleme küme dolmak veya mevcut hizmetlerden herhangi bir nedenden dolayı birleştirilmiş olamaz sürece genellikle mümkündür.

## <a name="buffered-capacity"></a>Arabelleğe alınan kapasite
Başka bir özellik kümesi Kaynak Yöneticisi'nin arabelleğe alınan kapasitesidir. Bazı genel düğüm kapasitesinden büyük bir kısmının ayırma sağlar. Bu kapasite arabelleği yalnızca Hizmetleri yükseltme ve düğüm hataları sırasında yerleştirmek için kullanılır. 

Arabelleğe alınan kapasite ölçüm tüm düğümler için genel olarak belirtilir. Kapasite çekme değeri, kümedeki sahip hata ve yükseltme etki alanlarının sayısı bir işlevdir. Daha fazla hata ve yükseltme etki alanları için arabelleğe alınan kapasite daha düşük bir sayı seçebilir anlamına gelir. Daha fazla etki alanı varsa, kümenizi yükseltme ve hataları sırasında kullanılamaz olarak küçük miktarlarda bekleyebilirsiniz. Yalnızca bir ölçüm için düğüm kapasitesi de belirlediyseniz arabelleğe alınan kapasiteyi belirten bir anlamı yoktur.

Arabelleğe alınan kapasite içinde ClusterManifest.xml belirtmek nasıl bir örnek aşağıda verilmiştir:

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

Arabelleğe alınan kapasiteyi ClusterConfig.json aracılığıyla tek başına dağıtımlarında veya Template.json için Azure'da barındırılan kümeleri belirtmek nasıl bir örnek aşağıda verilmiştir:

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

Küme bir ölçüm için arabelleğe alınan kapasite yetersiz olduğunda yeni hizmetler oluşturma başarısız olur. Arabellek korumak için yeni hizmetler oluşturulmasını önleme, kapasite aşımı gitmek düğümleri yükseltmeleri ve hataları neden olmadığını sağlar. Arabelleğe alınan kapasite isteğe bağlıdır, ancak bir ölçüm için bir kapasite tanımlayan herhangi bir küme içinde öneririz.

Küme Kaynak Yöneticisi, bu yük bilgiler sunar. Her bir ölçüm için bu bilgileri içerir: 
- Arabelleğe alınan kapasite ayarları.
- Toplam Kapasite.
- Geçerli kullanımı.
- Her ölçüm olup olmadığı kabul edilir veya dengeli.
- Standart sapma hakkındaki istatistiklerdir.
- En iyi olan düğümleri ve en az yük.  
  
Aşağıdaki kod, çıktının bir örneği gösterilmektedir:

```PowerShell
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
* Mimari ve bilgi akışını içinde Küme Kaynak Yöneticisi hakkında daha fazla bilgi için bkz: [Küme Kaynak Yöneticisi mimarisine genel bakış](service-fabric-cluster-resource-manager-architecture.md).
* Birleştirme ölçümleri tanımlama, yaymak yerine düğümlerde yük birleştirmek için bir yoludur. Birleştirme yapılandırma konusunda bilgi için bkz: [ölçümleri ve Service Fabric yük birleştirmenin](service-fabric-cluster-resource-manager-defragmentation-metrics.md).
* En baştan başlatmak ve [Service Fabric küme kaynak yöneticisi için giriş yapın](service-fabric-cluster-resource-manager-introduction.md).
* Küme Kaynak Yöneticisi nasıl yönetir ve kümedeki yük dengeleyen öğrenmek için bkz: [Service Fabric kümenizi Dengeleme](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png
