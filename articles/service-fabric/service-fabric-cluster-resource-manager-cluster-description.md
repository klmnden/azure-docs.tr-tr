---
title: Küme Kaynak Yöneticisi küme açıklaması | Microsoft Docs
description: Service Fabric kümesi, hata etki alanlarını, yükseltme etki alanları, düğüm özellikleri ve düğüm kapasitesi için Küme Kaynak Yöneticisi'ni belirterek açıklayan.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 396f1d3d8c69ba3204d16f06d49656fd138a1126
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="describing-a-service-fabric-cluster"></a>Service fabric kümesi açıklayan
Service Fabric kümesi Kaynak Yöneticisi, bir küme açıklamak için çeşitli mekanizmalar sağlar. Çalışma zamanı sırasında Küme Kaynak Yöneticisi'ni kümede çalışan hizmetler yüksek düzeyde kullanılabilirliğini sağlamak için bu bilgileri kullanır. Bu önemli kurallar zorlarken bu aynı zamanda kümedeki kaynak tüketimini en iyi duruma dener.

## <a name="key-concepts"></a>Önemli kavramlar
Küme Kaynak Yöneticisi'ni bir küme açıklayan çeşitli özellikleri destekler:

* Hata etki alanları
* Yükseltme etki alanları
* Düğüm özellikleri
* Düğüm kapasitesi

## <a name="fault-domains"></a>Hata etki alanları
Hata etki alanı herhangi Eşgüdümlü hatası alanıdır. (Sürücü hatalara bozuk NIC bellenimi için güç kaynağı hatalardan için kendi çeşitli nedenlerle üzerinde başarısız olacağından bu yana) tek bir makineye bir hata etki alanıdır. Elektrik kesintisinden veya tek bir konumda tek bir kaynak paylaşımı makineler olduğu gibi aynı Ethernet anahtara bağlı makine aynı hata etki alanında yok. Donanım hatalarının çakışmasına doğal olduğundan, hata etki alanlarını kendiliğinden hiyerarşik ve Service Fabric URI'ler olarak temsil edilir.

Service Fabric güvenle Hizmetleri yerleştirmek için bu bilgileri kullandığından hata etki alanlarının doğru şekilde ayarlandığından emin önemlidir. Service Fabric Git bir hizmet hata (bazı bileşeninin hatadan kaynaklanıyor) etki alanı kaybedilmesine neden olacağı şekilde Hizmetleri yerleştirmek istediğiniz değil. Azure ortamı Service Fabric ortamı tarafından sağlanan hata etki alanı bilgileri doğru sizin adınıza kümedeki düğümleri yapılandırmak için kullanır. Küme ayarlama zaman tanımlanan için Service Fabric tek başına, hata etki alanları 

> [!WARNING]
> Service Fabric sağlanan hata etki alanı bilgileri doğru olması önemlidir. Örneğin, Service Fabric kümenin düğümleri beş fiziksel ana bilgisayarda çalışan 10 sanal makinelerde çalıştırılan varsayalım. Bu durumda, olsa bile 10 sanal makineyi, vardır yalnızca 5 farklı (üst düzey) hata etki alanları. Aynı fiziksel ana bilgisayar paylaşımı fiziksel ana bilgisayarları başarısız olursa VM'ler Eşgüdümlü hatasıyla karşılaşan beri aynı kök hata etki paylaşmak VM'ler neden olur.  
>
> Service Fabric hata etki alanı değil değiştirmek için bir düğümün bekliyor. Sanal makinelerin yüksek kullanılabilirliğini gibi sağlamaya başka mekanizmalar [HA-VMs](https://technet.microsoft.com/en-us/library/cc967323.aspx) saydam VM'ler geçişini bir ana bilgisayardan diğerine kullandıkları Service Fabric ile çakışmalarına neden olabilir. Bu mekanizmaların etmeyin yeniden yapılandırın veya VM içinde çalışan kod bildirin. Bu nedenle, oldukları **desteklenmiyor** Service Fabric çalıştırmak için ortam olarak kümeleri. Service Fabric işe yalnızca yüksek oranda kullanılabilirlik teknolojisi olmalıdır. Dinamik VM geçiş gibi mekanizmaları SAN'lar, veya diğerleri gerekli değildir. Service Fabric, bu mekanizmaların ile birlikte kullandıysanız _azaltmak_ uygulama kullanılabilirliği ve güvenilirliği ek karmaşıklık tanıtmak için hata merkezi kaynakları ekleyin ve güvenilirlik kullanma ve Service Fabric de çakışan kullanılabilirlik stratejisi. 
>
>

Aşağıdaki grafikte hata etki alanları için katkıda ve tüm farklı hataya neden etki alanlarını listelemek tüm varlıklar rengi. Bu örnekte, veri merkezleri ("DC"), kabin ("R") ve dikey pencerelerde ("B") sahip. Alanlara, her dikey birden fazla sanal makine barındırıyorsa, hata etki alanı hiyerarşideki başka bir katmana olabilir.

<center>
![Hata etki alanlarını düzenlenmiş düğümler][Image1]
</center>

Çalışma zamanı sırasında Service Fabric kümesi Kaynak Yöneticisi hata etki alanlarını kümedeki göz önünde bulundurur ve düzenleri planları. Ayrı hata etki alanı; bu nedenle durum bilgisi olan çoğaltmaları veya belirli bir hizmeti için durum bilgisiz örnek dağıtılır. Hata etki alanları arasında hizmet dağıtma hiyerarşinin herhangi bir düzeyde hata etki alanı başarısız olduğunda hizmetin kullanılabilirliğini tehlikeye değil sağlar.

Service Fabric'ın Küme Kaynak Yöneticisi'ni kaç Katmanlar hata etki alanı hiyerarşisinde vardır önemli değil. Bununla birlikte, hiyerarşideki herhangi bir kısmının kaybı içinde çalışan hizmetler etkilemez emin olmak çalışır. 

Aynı sayıda her hata etki alanı hiyerarşisinde derinlik düzeyini düğüm varsa en iyisidir. Hata etki alanı "Ağaç" kümenizdeki dengesiz ise, bu hizmetleri en iyi ayırma bulmak için Küme Kaynak Yöneticisi için zorlaştıran. İmbalanced hata etki alanlarını düzenleri, bazı etki alanlarına etkisi diğer etki alanlarındaki'den fazla hizmetlerin kullanılabilirliğini kaybı anlamına gelir. Sonuç olarak, Küme Kaynak Yöneticisi'ni arasında iki amacı kaldırılır: Hizmetleri üzerlerinde koyarak "kalın" Bu etki alanında makineler kullanmak istediği ve böylece bir etki alanı kaybı sorunlara neden olmayan hizmetleri diğer etki alanlarında yerleştirmek istiyor. 

İmbalanced etki alanları ne gibi görünüyor? Aşağıdaki diyagramda, iki farklı küme düzenleri gösterir. İlk örnekte düğümleri hata etki alanları arasında eşit olarak dağıtılır. İkinci örnekte, bir hata etki alanı, diğer bir hata etki alanları daha pek çok daha fazla düğüm yok. 

<center>
![İki farklı küme düzenleri][Image2]
</center>

Azure üzerinde hata etki alanı bir düğümü içeren seçim sizin için yönetilir. Ancak, sağlamanız düğüm sayısına bağlı olarak, hala hata etki alanları ile diğerlerinden daha fazla düğüm bunlara şunun. Örneğin, kümedeki beş hata etki alanlarını sahip ancak yedi düğümler için belirli bir NodeType sağlamasını söyleyin. Daha fazla düğüm ile bu durumda, ilk iki hata etki alanlarını sonlanır. Yalnızca birkaç örnekleri ile daha fazla NodeTypes dağıtmaya devam ederseniz, sorunu daha zayıf alır. Bu nedenle, her düğümde bulunan düğüm sayısı önerilir birden çok hata etki alanları sayısı türüdür.

## <a name="upgrade-domains"></a>Yükseltme etki alanları
Yükseltme etki alanları, küme düzenini anlamak Service Fabric küme Resource Manager yardımcı olan başka bir özelliğidir. Yükseltme etki alanları aynı anda yükseltilir düğümlerinin kümelerini tanımlayın. Yükseltme etki alanlarının Küme Kaynak Yöneticisi anlamak ve yükseltme gibi yönetim işlemlerini düzenlemek yardımcı olur.

Yükseltme etki alanlarının çok hata etki alanları gibi ancak birkaç önemli farklılıklar sahip değildir. İlk olarak, Eşgüdümlü donanım hataları alanlarının hata etki alanlarını tanımlayın. Yükseltme etki alanları, diğer yandan, ilke tarafından tanımlanır. Kaç tane, bu ortamı tarafından dikte yerine istediğinize karar verin alın. Sayıda yükseltme düğümleri yaptığınız gibi etki alanları olabilir. Başka bir hata etki alanları ve yükseltme etki alanları arasında yükseltme etki alanları hiyerarşik olmayan farktır. Bunun yerine, bunlar gibi basit bir etiketin daha fazla. 

Aşağıdaki diyagramda, üç yükseltme etki alanları üç hata etki alanlarında şeritli gösterir. Ayrıca, her farklı hata ve yükseltme etki alanları bittiği bir olası yerleşimini üç farklı çoğaltmalar için bir durum bilgisi olan hizmet gösterir. Bu yerleştirme hata etki alanı hizmeti yükseltmesi sırasında ortasında kaybı sağlar ve kod ve verilerin bir kopyasını çözümlenmedi.  

<center>
![Hata ve yükseltme etki alanları ile yerleştirme][Image3]
</center>

Artıları ve eksileri sahip çok sayıda yükseltme etki alanları için vardır. Daha fazla yükseltme etki alanı yükseltme her adımı daha ayrıntılı ve bu nedenle daha az sayıda düğümleri veya hizmetleri etkiler anlamına gelir. Sonuç olarak, daha az Hizmetleri aynı anda taşımak daha az karmaşası sisteme giriş var. Bu hizmetin daha az yükseltme sırasında sunulan herhangi bir sorundan etkilenmez beri güvenilirliğini, artırmak eğilimindedir. Daha fazla yükseltme etki alanları ayrıca diğer düğümlerdeki yükseltme etkisini işlemek için daha az arabellek gerektiği anlamına gelir. Örneğin, beş yükseltme etki alanları varsa, her düğüm trafiğinizi % 20 işleme kabaca. Bu yükseltme etki alanı için bir yükseltme aşağı olması gerekiyorsa, bu Yük genellikle bir yere gitmek gerekir. Dört kalan yükseltme etki alanları sahip olduğundan, her yaklaşık %5 toplam trafik için yeriniz olması gerekir. Daha fazla yükseltme etki alanları kümedeki düğümler üzerinde daha az arabellek ihtiyaç duyacağınız anlamına gelir. Örneğin, 10 yükseltme etki alanları yerine sahip olmadığını göz önünde bulundurun. Bu durumda, her UD yalnızca yaklaşık %10 toplam trafik işleme. Bir yükseltme adımları, küme aracılığıyla her etki alanı yalnızca yaklaşık %1.1 toplam trafik için yeriniz olması gerekir. Daha fazla yükseltme etki alanları genellikle, daha az ayrılmış kapasite gerektiğinden düğümleriniz yüksek kullanımı, çalıştırmanızı sağlar. Aynı hata etki alanları için geçerlidir.  

Birden çok etki alanı yükseltme sonucunda dezavantajı, yükseltmeler daha uzun sürer eğilimindedir ' dir. Service Fabric yükseltme etki alanı tamamlandıktan ve bir sonraki yükseltmeye başlamadan önce bir denetim gerçekleştirmediğini saat kısa bir süre bekler. Bu gecikmeler yükseltmeye devam etmeden önce yükseltme tarafından sunulan algılama sorunları etkinleştirin. Kolaylığını kabul edilebilir olduğundan aynı anda hizmeti çok fazla etkilemesini hatalı değişiklikleri önler.

Çok az yükseltme etki alanına sahip birçok negatif yan etkileri – tek tek her yükseltme etki alanı aşağı ve büyük bölümünü yükseltiliyor genel kapasitenizi kullanılamıyor. Örneğin, yalnızca üç yükseltme etki alanı varsa, aynı anda yaklaşık 1/3 genel hizmet veya küme kapasite sürüyor. İş yükünü işlemek için kümenizi geri kalanı yeterli kapasiteye sahip olmanız bu yana çok hizmetinizin aşağı aynı anda sahip arzu değil. Bu arabellek anlamına gelir normal işlemi sırasında aksi durumdakinden daha az yüklenen düğümleri olan koruma. Bu, hizmetiniz çalıştırma maliyetini artırır.

Toplam sayısına hatası veya yükseltme etki alanlarının bir ortam ya da nasıl birbiriyle kısıtlamaları gerçek bir sınır yoktur. Birkaç ortak desenler vardır Bununla:

- Hata etki alanları ve yükseltme etki alanları 1:1 eşlenmiş
- Bir yükseltme etki alanı her düğüm (fiziksel veya sanal işletim sistemi örneği)
- Burada hata etki alanları ve yükseltme etki alanı bir matris genellikle çizgileri çalışan makineler ile form "şeritli" veya "matris" modeli

<center>
![Hata ve yükseltme etki alanı düzenleri][Image4]
</center>

Hangi düzenin seçmek için hiçbir en iyi yanıt, her bazı Artıları ve eksileri vardır. Örneğin, 1FD:1UD modeli ayarlamak basit bir işlemdir. 1 yükseltme etki alanı düğümü modeli başına hangi kişiler için kullanılan gibi çoğu'dir. Yükseltme sırasında her düğüm bağımsız olarak güncelleştirilir. Bu, nasıl küçük makineler kümesini el ile geçmişte yükseltilen için benzer. 

Burada FDs ve UDs bir tablo oluşturur ve düğümler çapraz başlangıç yerleştirilir FD/UD matris en yaygın modelidir. Bu, varsayılan olarak Azure Service Fabric kümelerinde kullanılan modelidir. Birçok düğümleri içeren kümeler için her şeyi yukarıdaki yoğun matris düzeni gibi bakan sonlandırır.

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Hata ve yükseltme etki alanı kısıtlamaları ve bunun sonucunda oluşan davranışı
### <a name="default-approach"></a>*Varsayılan yaklaşımı*
Varsayılan olarak, Küme Kaynak Yöneticisi'ni hata ve yükseltme etki alanları arasında dengeli Hizmetleri tutar. Bu olarak modellendi bir [kısıtlaması](service-fabric-cluster-resource-manager-management-integration.md). Hata ve yükseltme etki alanı kısıtlaması durumları: "verili hizmet bölüm için hiçbir zaman olması gerektiğini fark aynı düzeyde iki tüm etki alanları arasında hizmet nesneleri (durum bilgisiz hizmet örneği veya durum bilgisi olan hizmet çoğaltmaları) sayısı birden büyük hiyerarşisi". Bu kısıtlamanın "maksimum fark" garantisi sağlar varsayalım. Hata ve yükseltme etki alanı kısıtlaması belirli taşır ve yukarıda belirtilen kural ihlal düzenlemeleri engeller. 

Bir örneğe bakalım. Altı düğümü, beş hata etki alanları ve beş yükseltme etki alanları ile yapılandırılmış bir kümeye sahip olduğunuzu varsayalım.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

*Yapılandırma 1*

Şimdi biz bir TargetReplicaSetSize ile (veya Instancecount bir durum bilgisi olmayan bir hizmet için) bir hizmet beş oluşturduğunuzu varsayalım. Çoğaltmaları kara N1 N5 üzerinde. Aslında, N6 hiçbir zaman kaç Hizmetleri bu oluşturduğunuz gibi olsun kullanılır. Ancak neden? Geçerli düzen ve N6 seçilirse ne olacağını arasındaki farkı bakalım.

İşte, biz var düzeni ve çoğaltmaları hata ve yükseltme etki alanı başına toplam sayısı:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

*Düzen 1*


Bu düzen, hata etki alanı ve yükseltme etki alanı başına düğüm bakımından dengelenir. Ayrıca, hata ve yükseltme etki alanı başına çoğaltmaların sayısı bakımından dengelenir. Her etki alanı düğümleri aynı sayıda ve çoğaltmaları aynı sayıda sahiptir.

Şimdi, N6 yerine N2 kullandık neler olacağını konumundaki bakalım. Nasıl çoğaltmaları sonra dağıtılmış?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

*Düzen 2*


Bu düzen bizim "maksimum fark" garantisi hata etki alanı kısıtlaması tanımını ihlal ediyor. FD1 bir maksimum fark büyük olduğu iki, toplam FD0 FD1 arasındaki fark yapmadan sıfır sahipken FD0 iki çoğaltması yok. Küme Kaynak Yöneticisi'ni kısıtlamayı ihlal olduğundan, bu düzenleme izin vermiyor. Benzer şekilde biz N2 ve N6 (yerine N1 ve N2) kaldırdıysa biz alın:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

*Düzen 3*


Bu düzen, hata etki alanlarını bakımından dengelenir. UD1 iki sahipken UD0 sıfır çoğaltmalarına sahip olduğundan Bununla birlikte, artık bu etki alanı yükseltme kısıtlaması ihlal. Bu nedenle, bu düzeni ayrıca geçersiz ve küme kaynak yönetici tarafından çekilmesi olmaz.

Bu yaklaşım durum bilgisi olan çoğaltmaları veya durum bilgisiz örneklerinin dağıtım için en iyi olası hataya dayanıklılık sağlar. Bir durumda bir etki alanı aşağı gittiğinde çoğaltmaları/örnekleri en az sayıda kaybolur. 

Öte yandan, bu yaklaşım çok sıkı ve kümenin tüm kaynakları kullanmasına izin verme. Belirli bir küme yapılandırmaları için belirli düğümler kullanılamaz. Bu hizmet, hizmetlerinizi yerleştirme değil uyarı iletilerini kaynaklanan doku neden olabilir. Önceki örnekte, küme düğümleri bazıları olamaz (verilen örnekte N6) kullanılır. Bu kümeye (N7 – N10) düğümleri eklemek olsa bile, çoğaltmaları/örnekleri yalnızca N1 – hata ve yükseltme etki alanı kısıtlamaları nedeniyle N5 üzerinde konulabilir. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | |N10 |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | |N9 |N5 |

*Yapılandırma 2*


### <a name="alternative-approach"></a>*Alternatif bir yaklaşım*

Küme Kaynak Yöneticisi'ni en düşük düzeyde güvenlik hala güvence altına almak sırasında yerleştirme sağlayan hata ve yükseltme etki alanı kısıtlaması başka bir sürümünü destekler. Alternatif hata ve yükseltme etki alanı kısıtlaması şu şekilde belirtilebilir: "verili hizmet bölümü için etki alanları arasında çoğaltma dağıtım bölüm çekirdek kayıp saptanmamış emin olmanız gerekir". Bu kısıtlamanın "çekirdek güvenli" garanti sağlayan varsayalım. 

> [!NOTE]
>Durum bilgisi olan bir hizmet için tanımlarız *çekirdek kayıp* çoğaltmalarını çoğunu aşağı olduğunda aynı anda bir durumda. Örneğin, TargetReplicaSetSize beş ise, tüm üç çoğaltmaları kümesini çekirdek temsil eder. Benzer şekilde, TargetReplicaSetSize 6 ise, dört çoğaltmaları çekirdek için gereklidir. Normal şekilde çalışmaya devam edebilmesi bölüm istiyorsa, her iki durumda da ikiden fazla çoğaltmaları aşağı aynı anda olabilir. Durum bilgisiz hizmeti için bu tür hiçbir şey yoktur *çekirdek kayıp* normalde örnekleri çoğunu bile kesilirse aynı anda functionate için durum bilgisi olmayan hizmetler conitnue olarak. Bu nedenle, biz metnin geri kalanında durum bilgisi olan hizmetler üzerinde odaklanır.
>

Önceki örnekte edelim. Kısıtlama "çekirdek güvenli" sürümü ile tüm üç verilen düzenleri geçerli olacaktır. Bu durum, bölüm olurdu olsa bile FD0 üçüncü düzeninde hatası ikinci düzeni veya UD1 hala (çoğaltmalarını çoğunu hala yukarı olur) çekirdeğini gerekir çünkü. Bu kısıtlamanın sürümüyle N6 neredeyse her zaman göstermesi.

"Çekirdek güvenli" yaklaşım "maksimum fark" yaklaşım neredeyse her küme topolojisi geçerli çoğaltma dağıtımları daha kolay olduğundan daha fazla esneklik sağlar. Bazı hatalar diğerlerinden da kötüsü olduğundan ancak, bu yaklaşımın en iyi hataya dayanıklılık özellikleri garanti edemez. En kötü Durum senaryosu, çoğaltmaların çoğunluğu bir etki alanı ve bir ek çoğaltma hatası kaybolabilir. Örneğin, 5 çoğaltmaları veya örnekleri ile çekirdek kaybetmenize gerekli 3 hataları yerine, yalnızca iki hatalarıyla Çoğunluk şimdi kaybedebilirsiniz. 

### <a name="adaptive-approach"></a>*Uyarlamalı yaklaşımı*
Her iki yaklaşım güçlü ve zayıf olduğu için Biz bu iki stratejileri birleştirir Uyarlamalı bir yaklaşım ekledik.

> [!NOTE]
>Bu Service Fabric sürüm 6.2 ile başlayarak varsayılan davranışı olacaktır. 
>
Uyarlamalı yaklaşım, varsayılan olarak "maksimum fark" mantığını kullanır ve yalnızca gerekli olduğunda "çekirdek güvenli" mantığı geçer. Küme Kaynak Yöneticisi'ni otomatik olarak hangi stratejiyi nasıl küme ve Hizmetleri yapılandırılan en bakarak gerekli olduğunu rakamlar. Belirli bir hizmeti için: *TargetReplicaSetSize hata etki alanlarının sayısı ve yükseltme etki alanlarının sayısı tarafından tam bölünebilir ise **ve** düğüm sayısı (hata etki alanları sayısı) küçük veya eşit: * ( sayısı, yükseltme etki alanları), Küme Kaynak Yöneticisi'ni bu hizmet için "temel çekirdek" mantığı kullanmalıdır.* Küme Kaynak Yöneticisi'ni durum bilgisi olmayan hizmetler için ilgili olmaması çekirdek kaybına rağmen durum bilgisiz ve durum bilgisi olan hizmetler için bu yaklaşım kullanacağını aklınızda size aittir.

Önceki örnekte edelim ve bir küme şimdi (küme hala beş hata etki alanları ve beş yükseltme etki alanları ve bu küme kalır beş barındırılan bir hizmete TargetReplicaSetSize yapılandırılır) 8 düğümü sahip olduğunu varsayın. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | | |N5 |

*Yapılandırma 3*

Tüm gerekli koşulların karşılanıp çünkü Küme Kaynağı Yöneticisi hizmeti dağıtma içinde "çekirdek dayalı" mantığı kullanın. Bu N6 – N8 kullanımını etkinleştirir. Bir olası bir hizmet dağıtım, bu durumda gibi görünebilir:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | | | |0 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |2 |1 |1 |0 |1 |- |

*Düzen 4*

Hizmetinizin TargetReplicaSetSize dört ile (örneğin) azaltılmıştır, Küme Kaynak Yöneticisi'ni değişiklik dikkat edin ve TargetReplicaSetSize artık FDs ve UDs sayısına göre bölünebilen olmadığından "maksimum fark" mantığı kullanarak Sürdür. Sonuç olarak, belirli çoğaltma hareketleri, böylece hata etki alanı ve yükseltme etki alanı mantığı "maksimum fark" sürümünü ihlal edilmediğini N1 N5 düğümlerde dört çoğaltmaları kalan dağıtmak amacıyla meydana gelir. 

Geri dördüncü düzeni ve beş, TargetReplicaSetSize aranıyor. N1 kümeden kaldırılırsa, yükseltme etki alanlarının sayısı dört ile eşit olur. Yeniden UDs sayısı eşit hizmetin TargetReplicaSetSize artık bölmek değil olarak "maksimum fark" mantığı kullanarak küme Kaynak Yöneticisi'ni başlatır. Sonuç olarak, böylece hata ve yükseltme etki alanı kısıtlaması değil ihlal üzerinde N4 güden çoğaltma yeniden oluşturulduğunda R1 vardır.

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |Yok |Yok |Yok |Yok |Yok |Yok |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | |R1 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

*Düzen 5*

## <a name="configuring-fault-and-upgrade-domains"></a>Hata ve yükseltme etki alanlarını yapılandırma
Hata etki alanları ve yükseltme etki alanları tanımlama yapılır otomatik olarak Azure Service Fabric dağıtımları barındırılan. Service Fabric alır ve Azure ortamı bilgileri kullanır.

Kendi kümenizi oluşturmakta olduğunuz (veya belirli bir topolojisi geliştirme çalıştırmak istediğiniz varsa), hata etki alanı ve etki alanı yükseltme bilgileri kendiniz sağlayabilir. Bu örnekte, üç "veri merkezleri" (her üç rafları) yayılan dokuz düğümlü bir yerel geliştirme küme tanımlayın. Bu küme üç yükseltme bu üç veri merkezi arasında şeritli etki alanı da sahiptir. Yapılandırma örneği aşağıdadır: 

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
> Kümeler Azure Resource Manager aracılığıyla tanımlarken, hata etki alanları ve yükseltme etki alanlarının Azure tarafından atanır. Bu nedenle, Azure Resource Manager şablonu düğüm türleri ve sanal makine ölçek kümeleri tanımında hata etki alanı veya etki alanı yükseltme bilgileri içermez.
>

## <a name="node-properties-and-placement-constraints"></a>Düğüm özellikleri ve yerleştirme kısıtlamaları
Bazen (aslında, çoğu zaman), belirli iş yükleri yalnızca belirli türde bir kümedeki düğümler üzerinde çalıştığından emin olmak istediğiniz oluşturacağız. Örneğin, diğerleri olmayabilir ancak bazı iş yükü GPU veya SSD gerektirebilir. Neredeyse her n katmanlı mimarisi ölçeklendiriyor belirli iş yükleri için donanım hedefleme harika bir örnektir. Belirli makineler ön uç veya uygulamanın tarafı hizmet veren API hizmet ve istemcilerin veya Internet'e gösteriliyor. Genellikle, farklı donanım kaynakları olan farklı makinelerde işlem ya da depolama katmanları iş işleyin. Çoğunlukla bunlar _değil_ doğrudan istemcileri veya Internet'e açık. Service Fabric burada belirli donanım yapılandırmalarını temel çalıştırmak için belirli iş yükleri gereken durumlar vardır bekliyor. Örneğin:

* bir Service Fabric ortamına varolan n katmanlı uygulama "kaldırılmış ve gölgeye" olarak adlandırılmıştır
* bir iş yükü performansı, ölçek ve güvenlik yalıtımı nedeniyle belirli donanım üzerinde çalıştırmak istiyor
* Bir iş yükü İlkesi ya da kaynak tüketimi nedeniyle diğer iş yüklerini üzerinden olmalıdır

Bu tür yapılandırmaları desteklemek için Service Fabric düğümlere uygulanan etiketleri birinci sınıf kavramı vardır. Bu etiketler adlı **düğümü özellikleri**. **Kısıtlamalarından** olan bir veya daha fazla düğüm özelliklerini seçin, tek tek hizmetlerine bağlı deyimleri. Kısıtlamalarından Hizmetleri çalıştırdığı tanımlar. Kısıtlamalar kümesini Genişletilebilir - herhangi bir anahtar/değer çifti çalışabilirsiniz. 

<center>
![Düzen farklı iş yükleri küme][Image5]
</center>

### <a name="built-in-node-properties"></a>Düğüm özellikleri yerleşik
Service Fabric kullanıcının tanımlamadan gerek kalmadan otomatik olarak kullanılabilecek bazı varsayılan düğüm özelliklerini tanımlar. Her düğümde tanımlanan varsayılan Özellikler **NodeType** ve **NodeName**. Bu nedenle örneğin yerleştirme kısıtlama olarak yazabilirsiniz `"(NodeType == NodeType03)"`. Genellikle en yaygın olarak uygulanacak NodeType özelliklerinin kullanılmasını buldunuz. Bir makine türüyle 1:1 karşılık gelen beri yararlı olur. Her makine türü geleneksel n katmanlı uygulama iş yükü türünü karşılık gelir.

<center>
![Yerleştirme kısıtlamaları ve düğüm özellikleri][Image6]
</center>

## <a name="placement-constraint-and-node-property-syntax"></a>Yerleştirme kısıtlaması ve düğüm özellik sözdizimi 
Düğüm özelliğinde belirtilen değeri veya uzun imzalı bir string, bool, olabilir. Hizmet ifadede bir yerleştirme adlı *kısıtlaması* hizmeti kümedeki burada çalışabilir kısıtlar beri. Kısıtlama kümedeki farklı bir düğüme özelliklerindeki çalışır herhangi bir Boole ifadesini olabilir. Bu boolean deyimlerinde geçerli seçiciler şunlardır:

1) belirli ifadeleri oluşturmak için koşullu denetimleri

| Bildirim | Sözdizimi |
| --- |:---:|
| "eşit" | "==" |
| "eşit değil" | "!=" |
| "büyüktür" | ">" |
| "değerinden büyük veya eşit" | ">=" |
| "küçüktür" | "<" |
| "küçük veya eşit" | "<=" |

2) gruplandırma ve mantıksal işlemleri için Boole ifadeleri

| Bildirim | Sözdizimi |
| --- |:---:|
| "ve" | "&&" |
| "veya" | "&#124;&#124;" |
| "değil" | "!" |
| "grubu olarak tek bir deyimde" | "()" |

Temel kısıtlama deyimleri bazı örnekleri aşağıda verilmiştir.

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Yalnızca genel yerleştirme kısıtlaması deyimi burada "True" olarak değerlendirilir düğümler üzerinde yerleştirilen hizmetine sahip olabilirsiniz. Tanımlanmış bir özellik olmayan düğümleri içeren bu özellik her yerleştirme kısıtlamayla eşleşmiyor.

Verilen düğüm türü için aşağıdaki düğüm özellikleri tanımlanmadı varsayalım:

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

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan. 

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

Hizmet yerleşimi oluşturabilirsiniz *kısıtlamaları* gösterildiği gibi bir hizmet için:

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

Tüm düğümleri NodeType01 geçerliyse, bu düğüm türü kısıtlaması ile öğesini de seçebilirsiniz "(NodeType == NodeType01)".

Bir hizmetin kısıtlamalarından cool çalışmalarıdır bunlar dinamik olarak çalışma zamanı sırasında güncelleştirilmesi biridir. Bu nedenle gerekiyorsa, kümedeki bir hizmet gezinme, ekleyip kaldırabilirsiniz gereksinimleri, vb. Service Fabric bile bu tip değişiklikler yapıldığında hizmeti açık ve kullanılabilir kaldığından emin olma mvc'deki.

C# ' TA:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Kısıtlamalarından her farklı adlı hizmet örneği için belirtilir. Güncelleştirmeleri her zaman yerini alan (üzerine yaz) ne daha önce belirtildi.

Küme tanımı bir düğümde özelliklerini tanımlar. Bir düğümün özelliklerini değiştirme küme yapılandırması yükseltme gerektirir. Bir düğümün özellikleri yükseltme yeni özelliklerini raporlamak için yeniden başlatma için etkilenen her düğüme gerektirir. Bunlar çalışırken yükseltme Service Fabric tarafından yönetilir.

## <a name="describing-and-managing-cluster-resources"></a>Açıklayan ve küme kaynaklarını yönetme
Tüm orchestrator en önemli işlerden biri kümedeki kaynak tüketimini yönetmenize yardımcı olmaktır. Küme kaynaklarını yönetme farklı şunları anlamına gelebilir. İlk olarak, var. makineler aşırı yüklü olmadığını sağlamaktır. Bu makineler bunlar işleyebileceğinden daha fazla hizmet çalıştırmıyor emin olma anlamına gelir. İkinci olarak, Yük Dengeleme ve Hizmetleri verimli bir şekilde çalışmasını için kritik olan en iyi duruma getirme yoktur. Maliyet etkili veya performans hassas hizmet teklifleri başkalarının soğuk durumdayken etkin olması bazı düğümler izin veremez. Kaynak çakışması ve performansın etkin düğümleri sağlama ve soğuk düğümleri harcanan kaynakları ve artan maliyetleri temsil eder. 

Service Fabric kaynakları olarak temsil eden `Metrics`. Service Fabric tanımlamak istediğiniz mantıksal veya fiziksel kaynak ölçümleridir. "WorkQueueDepth" veya "MemoryInMb" gibi şeyleri ölçümleri örnekleridir. Düğümlerde Service Fabric yönetebilecek fiziksel kaynakları hakkında bilgi için bkz: [kaynak İdaresi](service-fabric-resource-governance.md). Özel ölçümleri ve kullanımları yapılandırma hakkında daha fazla bilgi için bkz: [bu makalede](service-fabric-cluster-resource-manager-metrics.md)

Ölçümleri yerleşimi kısıtlamaları ve düğüm özellikleri farklıdır. Düğüm, düğümlerin kendilerini statik tanımlayıcıları özelliklerdir. Ölçümleri düğümünüz kaynaklar ve bir düğümde çalıştırdığınızda hizmetlerini kullanma açıklanmaktadır. Bir düğüm özelliği "HasSSD" olabilir ve true veya false ayarlayabilirsiniz. Kullanılabilir alan miktarını, SSD ve ne kadar hizmetler tarafından kullanılan "DriveSpaceInMb" gibi bir ölçü olacaktır. 

Gibi kısıtlamalarından ve düğüm özellikleri için Service Fabric kümesi Kaynak Yöneticisi ölçümleri adlarını anlamı anlamıyor olduğunu dikkate almak önemlidir. Ölçüm adları yalnızca dizelerdir. Birim belirsiz olabilir, oluşturduğunuz ölçüm adları bir parçası olarak bildirmek için iyi bir uygulamadır.

## <a name="capacity"></a>Kapasite
Tüm kaynak etkinleştirdiyseniz *Dengeleme*, Service Fabric'ın Küme Kaynak Yöneticisi hala sağlamak hiçbir düğüm kapasitesi sonuçlandı olduğunu. Kapasite taşmaları yönetme küme çok dolu veya iş yükünün düğümlerinden büyük olduğu sürece mümkündür. Kapasitesi ise başka bir *kısıtlaması* Küme Kaynak Yöneticisi'ni düğüm bir kaynağın ne kadarının anlamak için kullanır. Kapasite kalan da küme için bir bütün olarak izlenir. Kapasite ve hizmet düzeyinde tüketim ölçümleri cinsinden ifade edilir. Bu nedenle örneğin ölçüm "ClientConnections" olabilir ve belirli bir düğümün 32768 "ClientConnections" için bir kapasiteye sahip. Diğer düğümlere o düğümü üzerinde çalıştıran bazı hizmet deyin, şu anda "ClientConnections" ölçüsünün 32256 kullanan diğer sınırlamaları olabilir.

Çalışma zamanı sırasında Kalan kapasite kümedeki ve düğümlerdeki Küme Kaynak Yöneticisi'ni izler. Kapasite izlemek üzere Küme Kaynağı Yöneticisi her hizmetin kullanımı hizmetin çalıştığı düğümün kapasiteden çıkarır. Bu bilgilerle Service Fabric kümesi Kaynak Yöneticisi yerleştirin veya düğüm kapasitesi geçmez, böylece çoğaltmaları taşımak nereye tahmin.

<center>
![Küme düğümlerini ve kapasite][Image7]
</center>

C# ' TA:

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

Küme bildiriminde tanımlanan kapasiteleri görebilirsiniz:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan. 

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

Yaygın olarak bir hizmetin değişiklikleri dinamik olarak yükleyin. "ClientConnections" yineleme yükünü 1024'ten 2048 değişti, ancak ardından çalıştığı üzerinde düğüm yalnızca bu ölçüm için kalan 512 kapasitesi olduğu söyleyin. Bu düğüm üzerinde yeterli alan olmadığından şimdi bu çoğaltma veya örneğinin yerleştirme, geçersiz. İlkenin etkisini gösterip ve düğüm kapasitesi aşağıda geri almak Küme Kaynak Yöneticisi'ni vardır. Bir veya daha fazla çoğaltmalar veya örnekleri bu düğümden başka düğümlere taşıyarak kapasite aşımı düğümü üzerindeki yükü azaltır. Çoğaltmaları taşırken, bu hareketleri maliyetini en aza indirmek Küme Kaynak Yöneticisi'ni çalışır. Taşıma maliyeti ele alınmıştır [bu makalede](service-fabric-cluster-resource-manager-movement-cost.md) Küme Kaynak Yöneticisi hakkında daha fazla yeniden dengelenmesi stratejilerini ve kurallar açıklanmaktadır [burada](service-fabric-cluster-resource-manager-metrics.md).

## <a name="cluster-capacity"></a>Küme kapasite
Service Fabric kümesi Kaynak Yöneticisi çok tam engeller genel küme nasıl tutmak? İyi yok çok dinamik yük ile bunu yapabilirsiniz. Küme Kaynak Yöneticisi tarafından gerçekleştirilen eylemler bağımsız olarak kendi yük depo Hizmetleri olabilir. Sonuç olarak, yarın ünlü olduğunda kümenizle yeterli boş alan bugün underpowered olabilir. Bu, içinde sorunları önlemek için baked bazı denetimler vardır belirtti. Yapabiliriz ilk şey, kümenin dolmasına neden olur yeni iş yüklerine oluşturulmasını engellemek ' dir.

Durum bilgisi olmayan bir hizmet oluşturmak ve onunla ilişkili bazı yük sahip söyleyin. Hizmet hakkında "DiskSpaceInMb" ölçüm cares varsayalım. Ayrıca hizmet, her örneği için "DiskSpaceInMb" beş birimleri tüketmeye geçiyor olduğunu düşünelim. Üç hizmet örneklerini oluşturmak istiyorum. Harika! Bu nedenle "Küme bize bile sırada bulunması DiskSpaceInMb" 15 birimleri ihtiyacımız anlamına bu hizmet örnekleri oluşturmak mümkün olmayacaktır. Küme Kaynak Yöneticisi'ni sürekli kapasitesi ve her ölçümü tüketimini hesaplar kümedeki kalan kapasite belirleyebilirsiniz. Yeterli alan yoksa, Küme Kaynak Yöneticisi oluşturma hizmeti çağrısı reddeder.

Gereksinim yalnızca 15 birimler kullanılabilir olmadığından, bu alanı birçok farklı yolu ayrılamadı. Örneğin, olabilir bir kalan birim kapasitesi 15 farklı düğümlerde ya da üç kalan birim kapasitesi beş farklı düğümlerde. Bu yüzden beş birimler kullanılabilir üç düğümlerde küme Kaynak Yöneticisi'ni şeyler düzenleyebilirsiniz hizmeti yerleştirir. Küme yeniden düzenleme küme dolmak veya mevcut Hizmetleri herhangi bir nedenden dolayı birleştirilmiş olamaz sürece genellikle mümkündür.

## <a name="buffered-capacity"></a>Arabelleğe alınan kapasite
Arabelleğe alınan kapasite başka bir küme kaynağı Yöneticisi'nin özelliğidir. Bazı genel düğüm kapasitesi kısmının ayırma sağlar. Bu kapasite arabellek yalnızca Hizmetleri yükseltme ve düğüm hataları sırasında yerleştirmek için kullanılır. Arabelleğe alınan kapasite ölçüm tüm düğümler için başına genel olarak belirtilir. Ayrılmış kapasiteyi çekme değeri, hata ve yükseltme kümedeki sahip etki alanları sayısı bir işlevdir. Daha fazla hata ve yükseltme etki alanları anlamına gelir için arabelleğe alınan kapasite daha düşük bir sayı seçebilirsiniz. Daha fazla etki alanı varsa, yükseltmeleri ve hataları sırasında kullanılamaz hale gelmesine kümenizi daha küçük miktarda bekleyebilirsiniz. Düğüm kapasitesi bir ölçüm için ayrıca belirttiyseniz arabelleğe alınan kapasite belirtme yalnızca anlamlıdır.

Arabelleğe alınan kapasite belirleme konusunda bir örneği burada verilmiştir:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan:

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

Küme bir ölçüm için arabelleğe alınan kapasite dışında olduğunda yeni hizmetler oluşturma başarısız olur. Arabellek korumak için yeni hizmetler oluşturulmasını önleme yükseltmeleri ve hataları kapasite aşımı gitmek düğümleri neden yok sağlar. Arabelleğe alınan kapasite isteğe bağlıdır, ancak bir ölçüm için bir kapasite tanımlayan herhangi bir küme önerilir.

Küme Kaynak Yöneticisi'ni bu yük bilgilerini gösterir. Her ölçüm için bu bilgileri içerir: 
  - arabelleğe alınan kapasite ayarları
  - Toplam Kapasite
  - Geçerli tüketim
  - Dengeli veya her ölçümü olup olmadığı kabul edilir
  - Standart sapma hakkındaki istatistiklerdir
  - en olan düğümler ve en az yükleme  
  
Aşağıdaki örnek, çıktı olarak bakın:

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
* Mimari ve bilgi akışını içinde Küme Kaynak Yöneticisi hakkında daha fazla bilgi için kullanıma [bu makalede ](service-fabric-cluster-resource-manager-architecture.md)
* Birleştirme ölçümleri tanımlama yayılmak yerine düğümlerde yük birleştirmek için bir yoludur. Birleştirme yapılandırma konusunda bilgi edinmek için bkz [bu makalede](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* En baştan başlatın ve [bir giriş için Service Fabric kümesi Resource Manager Al](service-fabric-cluster-resource-manager-introduction.md)
* Küme Kaynak Yöneticisi'ni yönetir ve yük devretme kümesinde dengeleyen hakkında bilgi almak için makalesine kontrol [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png
