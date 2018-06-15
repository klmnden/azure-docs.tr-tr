---
title: SQL veri ambarı eşzamanlılık ve iş yükü yönetimi | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL Data Warehouse eşzamanlılık ve iş yükü yönetimi anlayın.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: ''
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 08/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: eaf2d43286dbaa52ada1430fbb7ce1e37f41c0d4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
ms.locfileid: "29959528"
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>SQL veri ambarı eşzamanlılık ve iş yükü yönetimi
Ölçekte, Microsoft Azure SQL Data Warehouse yardımcı tahmin edilebilir performans sağlamak üzere tutarlılık düzeyleri ve bellek ve CPU önceliği gibi kaynak ayırma denetler. Bu makale size nasıl her iki özellik uygulanan ve nasıl veri ambarında denetlenebilecek açıklayan eşzamanlılık ve iş yükü yönetimi kavramları tanıtır. SQL veri ambarı iş yükü yönetimi, çok kullanıcılı ortamlar desteklemek amacıyla tasarlanmıştır. Çoklu kiracı iş yükleri için tasarlanmamıştır.

## <a name="concurrency-limits"></a>Eşzamanlılık sınırları
SQL veri ambarı en fazla 1024 eşzamanlı bağlantı sağlar. Tüm 1.024 bağlantıları sorguları eşzamanlı olarak gönderebilirsiniz. Ancak, verimliliği en iyi duruma getirmek için SQL Data Warehouse her sorgu en az bellek ataması aldığından emin olmak için bazı sorguları sırası. Queuing sorgu yürütme sırasında oluşur. Eşzamanlılık sınırlara ulaşıldığında queuing sorgular tarafından SQL Data Warehouse etkin sorgular oldukça gereken bellek kaynaklara erişim elde etmenizi sağlayarak toplam verimliliği artırabilirsiniz.  

Eşzamanlılık sınırları iki kavramları tarafından yönetilir: *eş zamanlı sorguları* ve *eşzamanlılık yuvaları*. Bir sorgu yürütmek sorgu eşzamanlılık sınırı ve eşzamanlılık yuvası ayırma içinde yürütülmelidir.

* Eş zamanlı sorgular sorguları aynı anda yürütme gerçekleşiyor. SQL veri ambarı büyük DWU boyutlarına en fazla 32 eş zamanlı sorguları destekler.
* Eşzamanlılık yuvaları üzerinde DWU göre ayrılır. Her 100 DWU 4 eşzamanlılık yuvası sağlar. Örneğin, bir DW100 4 eşzamanlılık yuvası ayırır ve 40 DW1000 ayırır. Bir veya daha fazla eşzamanlılık yuvaları, bağımlı her sorgu tüketir [kaynak sınıfı](#resource-classes) sorgunun. Smallrc kaynak sınıfında çalışan sorguları bir eşzamanlılık yuvası kullanabilir. Sorguları daha yüksek bir kaynak sınıfında çalıştırma başka eşzamanlılık yuva kullanabilir.

Aşağıdaki tabloda, eş zamanlı sorgular ve çeşitli DWU boyutlarda eşzamanlılık yuvaları için sınırları açıklanmaktadır.

### <a name="concurrency-limits"></a>Eşzamanlılık sınırları
| DWU | En fazla eş zamanlı sorgular | Ayrılmış eşzamanlılık yuvaları |
|:--- |:---:|:---:|
| DW100 |4 |4 |
| DW200 |8 |8 |
| DW300 |12 |12 |
| DW400 |16 |16 |
| DW500 |20 |20 |
| DW600 |24 |24 |
| DW1000 |32 |40 |
| DW1200 |32 |48 |
| DW1500 |32 |60 |
| DW2000 |32 |80 |
| DW3000 |32 |120 |
| DW6000 |32 |240 |

Bu eşikler biri karşılandığında yeni sorgular sıraya ve bir ilk giren ilk çıkar özelliğine sahip temelinde yürütülür.  Sıraya alınan sorguları bir sorguları sonlandırır ve sorgular ve yuva sayısını sınırlar denk olarak yayımlanmıştır. 

> [!NOTE]  
> *Seçin* sorguları özel olarak üzerinde yürütme (Dmv'leri) dinamik yönetim görünümleri veya Katalog görünümleri değil yönetilir herhangi bir eşzamanlılık sınırları tarafından. Sistem sorguları üzerinde yürütme sayısından bağımsız olarak izleyebilirsiniz.
> 
> 

## <a name="resource-classes"></a>Kaynak sınıfları
Kaynak sınıfları, bir sorguya verilen işlemci döngülerini ve bellek ayırmayı denetlemenize yardımcı olur. Veritabanı rolleri biçiminde bir kullanıcı için iki tür kaynak sınıfı atayabilirsiniz. İki tür kaynak sınıfı aşağıdaki gibidir:
1. Dinamik kaynak sınıflar (**smallrc, mediumrc, largerc, xlargerc**) bir değişken geçerli DWU bağlı olarak bellek miktarı ayırın. Bu, daha büyük bir DWU'ya ölçeklendirme, sorgularınızı otomatik olarak daha fazla bellek elde anlamına gelir. 
2. Statik kaynak sınıflar (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) (DWU yeterli belleğe sahip olması koşuluyla) bellek geçerli DWU bakılmaksızın aynı miktarda ayırın. Bu büyük Dwu üzerinde size daha fazla sorguları her kaynak sınıfında eşzamanlı olarak çalıştırabileceği anlamına gelir.

Kullanıcılar **smallrc** ve **staticrc10** daha az miktarda belleğe verilir ve daha yüksek eşzamanlılık avantajından yararlanabilirsiniz. Kullanıcılar buna karşılık, atanan **xlargerc** veya **staticrc80** büyük miktarlarda bellek, verilir ve bu nedenle daha az sorgularını çalışabilir.

Varsayılan olarak, her kullanıcı küçük kaynak sınıfı üyesidir **smallrc**. Yordam `sp_addrolemember` kaynak sınıfı artırmak için kullanılır ve `sp_droprolemember` kaynak sınıfı azaltmak için kullanılır. Örneğin, bu komut için loaduser kaynak sınıfının artırır **largerc**:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a>Kaynak sınıfları getirmiyor sorguları

Birkaç daha büyük bir bellek ayırma faydalanmaz sorgu türleri vardır. Sistem, kaynak sınıf ayırma yoksayar ve her zaman bu sorgular küçük bir kaynak sınıfında bunun yerine çalıştırın. Bu sorguları her zaman küçük kaynak sınıfında çalıştırırsanız, olduğunda eşzamanlılık yuvaları baskısı altında ve gerekli olandan daha fazla yuvaları tüketmemesidir çalıştırabilirsiniz. Bkz: [kaynak sınıf özel durumları](#query-exceptions-to-concurrency-limits) daha fazla bilgi için.

## <a name="details-on-resource-class-assignment"></a>Kaynak sınıf ataması ilgili ayrıntılar


Kaynak sınıfı birkaç daha fazla bilgi:

* *Rol alter* izni kaynak sınıfı, bir kullanıcının değiştirmek için gereklidir.
* Bir kullanıcı bir veya daha yüksek kaynak sınıfların eklemesi mümkün olsa da, dinamik kaynak sınıfları statik kaynak sınıfları önceliklidir. Diğer bir deyişle, bir kullanıcı hem de atanmışsa **mediumrc**(dinamik) ve **staticrc80**(statik) **mediumrc** uygulanır kaynak sınıfı.
 * Bir kullanıcı belirli bir kaynak sınıfı türü (birden fazla dinamik kaynak sınıfı veya birden çok statik kaynak sınıfı) birden fazla kaynak sınıfında atandığında, en yüksek kaynak sınıfı uygulanır. Diğer bir deyişle, bir kullanıcı hem mediumrc hem de largerc atanırsa, daha yüksek kaynak sınıfı (largerc) uygulanır. Ve bir kullanıcı hem de atanmışsa **staticrc20** ve **statirc80**, **staticrc80** ayardaki değil.
* Sistem yönetici kullanıcı kaynak sınıfının değiştirilemez.

Ayrıntılı bir örnek için bkz: [değiştirme kullanıcı kaynak sınıfı örneği](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Bellek ayırma
Artıları ve eksileri kullanıcının kaynak sınıfı artırmak için vardır. Bir kullanıcı için bir kaynak sınıfı artırılması, kendi sorguları daha hızlı sorgu yürütebilir olduğu anlamına gelebilir daha fazla bellek erişmenizi sağlar.  Ancak, daha yüksek kaynak sınıfları ayrıca çalıştırabilirsiniz eş zamanlı sorguları sayısını azaltın. Büyük miktarlarda bellek tek bir sorgu için ayırma ya da bellek ayırmaları, aynı anda çalıştırmak için gereken diğer sorgular izin verme arasındaki dengelemeyi budur. Bir kullanıcı bir sorgu için bellek yüksek ayırmaları verilirse, diğer kullanıcıların bir sorguyu çalıştırmak için aynı belleğin erişimi yok.

Aşağıdaki tabloda, her dağıtım noktasına DWU ve kaynak sınıfı tarafından ayrılan bellek eşler.

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a>Dinamik kaynak sınıfları (MB) için dağıtım başına bellek ayırma
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |100 |100 |200 |400 |
| DW200 |100 |200 |400 |800 |
| DW300 |100 |200 |400 |800 |
| DW400 |100 |400 |800 |1,600 |
| DW500 |100 |400 |800 |1,600 |
| DW600 |100 |400 |800 |1,600 |
| DW1000 |100 |800 |1,600 |3,200 |
| DW1200 |100 |800 |1,600 |3,200 |
| DW1500 |100 |800 |1,600 |3,200 |
| DW2000 |100 |1,600 |3,200 |6,400 |
| DW3000 |100 |1,600 |3,200 |6,400 |
| DW6000 |100 |3,200 |6,400 |12,800 |

Aşağıdaki tabloda, her dağıtım noktasına DWU ve statik kaynak sınıfı tarafından ayrılan bellek eşler. Daha yüksek kaynak sınıfları genel DWU sınırları vermenizin azaltılmış kendi bellek gerektiğini unutmayın.

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a>Statik kaynak sınıfları (MB) için dağıtım başına bellek ayırma
| DWU | staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |100 |200 |400 |400 |400 |400 |400 |400 |
| DW200 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW300 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW400 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW500 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW600 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW1000 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW1200 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW1500 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW2000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |6,400 |
| DW3000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |6,400 |
| DW6000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |12,800 |

Önceki tablosundan bir DW2000 üzerinde çalışan bir sorgu görebilirsiniz **xlargerc** kaynak sınıfı 6.400 MB her 60 dağıtılmış veritabanlarının içindeki bellek erişimi olması.  SQL veri ambarı'nda 60 dağıtımları vardır. Bu nedenle, bir sorguda belirtilen kaynak sınıfı için toplam bellek ayırmayı hesaplamak için yukarıdaki değerleri 60 çarpılacağı.

### <a name="memory-allocations-system-wide-gb"></a>Bellek ayırma sistem genelinde (GB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |6 |6 |12 |23 |
| DW200 |6 |12 |23 |47 |
| DW300 |6 |12 |23 |47 |
| DW400 |6 |23 |47 |94 |
| DW500 |6 |23 |47 |94 |
| DW600 |6 |23 |47 |94 |
| DW1000 |6 |47 |94 |188 |
| DW1200 |6 |47 |94 |188 |
| DW1500 |6 |47 |94 |188 |
| DW2000 |6 |94 |188 |375 |
| DW3000 |6 |94 |188 |375 |
| DW6000 |6 |188 |375 |750 |

Bu sistem genelinde bellek ayırmaları tablodan xlargerc kaynak sınıfında DW2000 üzerinde çalışan bir sorgu 375 GB bellek toplam ayrılır görebilirsiniz (6.400 MB * 60 dağıtımları / 1.024 GB'lık dönüştürmek için) SQL veri ambarı tamamen üzerinden.

Aynı hesaplama statik kaynak sınıflar için geçerlidir.
 
## <a name="concurrency-slot-consumption"></a>Eşzamanlılık yuvası tüketim  
SQL veri ambarını daha yüksek kaynak sınıflarda çalışmakta olan sorgulara daha fazla bellek verir. Bellek sabit bir kaynaktır.  Bu nedenle, sorgu ayrılan daha fazla bellek, daha az sayıda eş zamanlı sorguları çalıştırabilirsiniz. Aşağıdaki tabloda tüm eşzamanlılık yuvaları DWU tarafından kullanılabilir ve her kaynak sınıfı tarafından tüketilen yuvaların sayısını gösterir tek bir görünümde önceki kavramlarını reiterates.  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a>Ayırma ve dinamik kaynak sınıfları için eşzamanlılık yuva tüketim  
| DWU | En fazla eş zamanlı sorgular | Ayrılmış eşzamanlılık yuvaları | Smallrc tarafından kullanılan yuvaları | Mediumrc tarafından kullanılan yuvaları | Largerc tarafından kullanılan yuvaları | Xlargerc tarafından kullanılan yuvaları |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |4 |4 |1 |1 |2 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |
| DW400 |16 |16 |1 |4 |8 |16 |
| DW500 |20 |20 |1 |4 |8 |16 |
| DW600 |24 |24 |1 |4 |8 |16 |
| DW1000 |32 |40 |1 |8 |16 |32 |
| DW1200 |32 |48 |1 |8 |16 |32 |
| DW1500 |32 |60 |1 |8 |16 |32 |
| DW2000 |32 |80 |1 |16 |32 |64 |
| DW3000 |32 |120 |1 |16 |32 |64 |
| DW6000 |32 |240 |1 |32 |64 |128 |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a>Ayırma ve statik kaynak sınıfları için eşzamanlılık yuva tüketim  
| DWU | En fazla eş zamanlı sorgular | Ayrılmış eşzamanlılık yuvaları |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |4 |4 |1 |2 |4 |4 |4 |4 |4 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW400 |16 |16 |1 |2 |4 |8 |16 |16 |16 |16 |
| DW500 | 20| 20| 1| 2| 4| 8| 16| 16| 16| 16|
| DW600 | 24| 24| 1| 2| 4| 8| 16| 16| 16| 16|
| DW1000 | 32| 40| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1200 | 32| 48| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1500 | 32| 60| 1| 2| 4| 8| 16| 32| 32| 32|
| DW2000 | 32| 80| 1| 2| 4| 8| 16| 32| 64| 64|
| DW3000 | 32| 120| 1| 2| 4| 8| 16| 32| 64| 64|
| DW6000 | 32| 240| 1| 2| 4| 8| 16| 32| 64| 128|

Bu tablodan en fazla 32 eş zamanlı sorgular ve 40 eşzamanlılık yuvaları toplam DW1000 ayırır gibi SQL veri ambarı çalıştıran görebilirsiniz. Tüm kullanıcılar smallrc çalıştırıyorsanız, her sorgu 1 eşzamanlılık yuva kullanılmasına neden olur olduğundan 32 eş zamanlı sorguları izin. Tüm kullanıcılar bir DW1000 mediumrc içinde çalışmakta olan, her sorgu 47 GB sorgu başına bir toplam bellek ayırma için dağıtım başına 800 MB ayrılması ve eşzamanlılık 5 kullanıcılar sınırlı olacaktır (40 eşzamanlılık yuvaları / 8 yuvası mediumrc kullanıcı başına).

## <a name="selecting-proper-resource-class"></a>Doğru kaynak sınıfını seçme  
Bir kaynak sınıfı için kendi kaynak sınıflarını değiştirmek yerine kullanıcıların kalıcı olarak atamak iyi bir uygulamadır. Örneğin, kümelenmiş columnstore tabloları yükleri daha fazla bellek tahsis zaman daha yüksek kaliteli dizinler oluşturun. Yükleri daha yüksek bellek erişimi olduğundan emin olun için özellikle verileri yüklemek için bir kullanıcı oluşturun ve kalıcı olarak bu kullanıcı için daha yüksek bir kaynak sınıfı atayın.
Birkaç burada izlemek için en iyi yöntemler vardır. Yukarıda belirtildiği gibi SQL DW iki tür kaynak sınıfı türlerini destekler: statik kaynak sınıfları ve dinamik kaynak sınıfları.
### <a name="loading-best-practices"></a>En iyi uygulamaları yükleme
1.  Beklentileri normal veri miktarı ile yükleri varsa, statik kaynak sınıfı iyi bir seçimdir. Daha sonra daha fazla hesaplama gücüne alma yukarı ölçeklendirilirken veri ambarı yük kullanıcı daha fazla bellek tüketmesine değil gibi daha fazla eşzamanlı sorguları out-of--box, çalıştırmanız mümkün olacaktır.
2.  Beklentileri bazı durumlar büyük yükleri varsa, dinamik kaynak sınıfı iyi bir seçimdir. Daha sonra daha fazla hesaplama gücüne alma yukarı ölçeklendirilirken yük kullanıcı daha fazla bellek out-of--box, bu nedenle daha hızlı gerçekleştirmek yük izin vererek alır.

İşlenen veri miktarına ve yüklenen tablo yapısı üzerinde yüklerini verimli bir şekilde işlemek için gereken bellek bağlıdır. Örneğin, CCI tablolara veri yükleniyor CCI rowgroups en iyi hale getirme ulaşmasını sağlamak için bazı bellek gerektirir. Daha fazla ayrıntı için bkz: Columnstore dizinleri - veri Yükleme Kılavuzu.

En iyi uygulama, biz yükler için en az 200 MB bellek kullanmak için öneriyoruz.

### <a name="querying-best-practices"></a>En iyi yöntemler sorgulama
Sorguları kendi karmaşıklığına bağlı olarak farklı gereksinimleri vardır. Sorgu başına bellek artırma veya eşzamanlılık artırmayı sorgu gereksinimlerine bağlı olarak genel üretilen işi artırmak için geçerli iki yollarıdır.
1.  Beklentileri normal, karmaşık sorgular varsa (örneğin, günlük ve haftalık raporlar üretmek için) ve eşzamanlılık yararlanmak gerekmez, dinamik kaynak sınıfı iyi bir seçimdir. Sistem işlemek için daha fazla veri varsa, veri ambarı ölçeklendirme bu nedenle otomatik olarak daha fazla bellek sorgu çalıştıran kullanıcının sağlar.
2.  Beklentileri değişkeni ya da diurnal eşzamanlılık desenleri varsa (örneği için veritabanı web UI kapsamlı erişilebilir sorgulanan varsa), bir statik kaynak sınıf iyi bir seçimdir. Daha sonra veri ambarına yukarı ölçeklendirilirken statik kaynak sınıfıyla ilişkili kullanıcıyı otomatik olarak daha fazla eşzamanlı sorguları çalıştırmak mümkün olacaktır.

Sorgunuz ihtiyacına bağlı olarak seçilmesi uygun bellek ataması Önemsiz olmayan, aynıdır tablo şemalarını ve çeşitli birleştirme, seçim ve Grup koşulları yapısını sorgulanan, veri miktarı gibi pek çok etkene bağlıdır. Genel açısından daha fazla bellek ayırma daha hızlı tamamlamak için sorgular izin verir, ancak Genel eşzamanlılık azaltmak. Eşzamanlılık sorunu değilse, aşırı bellek ayırma zarar değil. Üretilen iş ince ayar yapmak için kaynak sınıflarının çeşitli özellikleri çalışırken gerekli olabilir.

Belirli bir SLO kaynak sınıfı ve bellek yoğun CCI tablosu üzerinde işlem bölümlenmemiş CCI belirtilen kaynak sınıfı en yakın en iyi kaynak sınıfı başına eşzamanlılık ve bellek grant ekleneceğini aşağıdaki depolanan yordamı kullanabilirsiniz:

#### <a name="description"></a>Açıklama:  
Bu saklı yordam amacı şöyledir:  
1. Kullanıcının anlamasına yardımcı olmak için belirli bir SLO kaynak sınıfı başına eşzamanlılık ve bellek verin. Kullanıcı NULL hem şema hem de tablename için bunun için örnekte gösterildiği gibi sağlaması gerekir.  
2. Kullanıcının yardımcı olmak için bellek intensed için en yakın iyi kaynak sınıfı (yük, kopyalama tablo yeniden dizin, vb.) CCI tablosu üzerinde işlem dışı bölümlenmiş CCI belirtilen kaynak sınıfı, tahmin edin. Saklı yordam, bunun için gerekli bellek ataması öğrenmek için tablo şemasını kullanır.

#### <a name="dependencies--restrictions"></a>Bağımlılıklar ve sınırlamalar:
- Bu saklı yordam bölümlenmiş cci tablo bellek gereksinimini hesaplamak üzere tasarlanmamıştır.    
- Bu saklı yordam Ekle/CTAS-seçin seçme bölümü bellek gereksinimini dikkate almaz ve basit bir seçim olmasını varsayar.
- Bu oturumda bu saklı yordam oluşturulduğu kullanılacak şekilde bu saklı yordam bir geçici tablo kullanır.    
- Bu değişiklikler, ardından bu saklı yordam düzgün çalışmayan ve bu saklı yordam geçerli tekliflerini (örn. donanım yapılandırması, DMS config) bağlıdır.  
- Değişiklikler, ardından bu saklı yordam düzgün çalışmayan ve bu saklı yordam varolan sunulan eşzamanlılık sınırı bağlıdır.  
- Bu saklı yordam mevcut kaynak sınıfı tekliflerini bağlıdır ve, ardından bu değişip değişmediğini proc wuold çalışmaz doğru bir şekilde depolanır.  

>  [!NOTE]  
>  Saklı yordam olabilir sonra iki durumda sağlanan parametrelerle yürüttükten sonra çıktı almıyorsanız. <br />1. Her iki DW parametresi geçersiz SLO değeri içeriyor <br />2. VEYA yoksa hiçbir eşleşen kaynak sınıfı CCI işlemi için tablo adı sağlandı. <br />Örneğin, DW100 400 MB ve tablo şemasını 400 MB gereksinim geçilecek geniş ise en yüksek bellek ataması kullanılabilir olur.
      
#### <a name="usage-example"></a>Kullanım örneği:
Sözdizimi:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU: Ya geçerli DWU DW DB'den ayıklamak veya desteklenen tüm DWU 'DW100' biçiminde sağlamak için bir NULL parametresi sağlayın
2. @SCHEMA_NAME: Tablo şema adını belirtin
3. @TABLE_NAME: İlgilendiğiniz bir tablo adı sağlayın

Bu saklı yordam yürütme örnekler:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

Yukarıdaki örneklerde kullanılan tablo1 aşağıdaki gibi oluşturulabilir:  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-the-stored-procedure-definition"></a>Saklı yordam tanımı aşağıda verilmiştir:
```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(   @DWU VARCHAR(7),
      @SCHEMA_NAME VARCHAR(128),
       @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) to hold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="query-importance"></a>Sorgu önem derecesi
SQL veri ambarı iş yükü gruplarını kullanarak kaynak sınıfları uygular. Çeşitli DWU boyutları arasında kaynak sınıfları davranışını denetleyen sekiz iş yükü grupları toplam vardır. SQL veri ambarı tüm DWU için yalnızca dört sekiz iş yükü gruplarını kullanır. Her iş yükü grubu dört kaynak sınıflardan birine atanmış olduğundan bu mantıklı: smallrc, mediumrc, largerc, veya xlargerc. Bu iş yükü grupları bazıları için daha yüksek ayarlandığını iş yükü grupları anlama önemi olan *önem*. Önem derecesi CPU için kullanılan planlama. Yüksek öncelikli olarak çalıştırılan sorguların olandan Orta önem düzeyine sahip üç kat daha fazla CPU döngülerini alırsınız. Bu nedenle, eşzamanlılık yuvası eşlemeleri CPU Öncelik belirler. Bir sorgu 16 veya daha fazla yuvaları tüketir yüksek önem olarak çalışır.

Aşağıdaki tabloda, her iş yükü grubu için önem eşlemeler gösterilmektedir.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>İş yükü grubu eşlemelere eşzamanlılık yuvaları ve önem derecesi
| İş yükü grupları | Eşzamanlılık yuvası eşleme | MB / dağıtım | Önem derecesi eşleme |
|:--- |:---:|:---:|:--- |
| SloDWGroupC00 |1 |100 |Orta |
| SloDWGroupC01 |2 |200 |Orta |
| SloDWGroupC02 |4 |400 |Orta |
| SloDWGroupC03 |8 |800 |Orta |
| SloDWGroupC04 |16 |1,600 |Yüksek |
| SloDWGroupC05 |32 |3,200 |Yüksek |
| SloDWGroupC06 |64 |6,400 |Yüksek |
| SloDWGroupC07 |128 |12,800 |Yüksek |

Gelen **ayırma ve kullanımını eşzamanlılık yuva** grafiği, bir DW500 1, 4, 8 ya da 16 eşzamanlılık yuvası smallrc, mediumrc, largerc ve xlargerc, sırasıyla kullandığını görebilirsiniz. Bu değerleri her kaynak sınıfı için önem bulmak için yukarıdaki grafikte bakabilirsiniz.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Önem derecesi için kaynak sınıfların DW500 eşleme
| Kaynak sınıfı | İş yükü grubu | Kullanılan eşzamanlılık yuvaları | MB / dağıtım | Önem derecesi |
|:--- |:--- |:---:|:---:|:--- |
| smallrc |SloDWGroupC00 |1 |100 |Orta |
| mediumrc |SloDWGroupC02 |4 |400 |Orta |
| largerc |SloDWGroupC03 |8 |800 |Orta |
| xlargerc |SloDWGroupC04 |16 |1,600 |Yüksek |
| staticrc10 |SloDWGroupC00 |1 |100 |Orta |
| staticrc20 |SloDWGroupC01 |2 |200 |Orta |
| staticrc30 |SloDWGroupC02 |4 |400 |Orta |
| staticrc40 |SloDWGroupC03 |8 |800 |Orta |
| staticrc50 |SloDWGroupC03 |16 |1,600 |Yüksek |
| staticrc60 |SloDWGroupC03 |16 |1,600 |Yüksek |
| staticrc70 |SloDWGroupC03 |16 |1,600 |Yüksek |
| staticrc80 |SloDWGroupC03 |16 |1,600 |Yüksek |

Bellek kaynağı ayırma ayrıntılı farklılıkları kaynak İdarecisi perspektifinden bakmak ya da etkin ve geçmiş kullanımı iş yükü gruplarının sorunlarını giderirken çözümlemek için aşağıdaki DMV sorgusu kullanabilirsiniz.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                        AS node_type
    ,pn.pdw_node_id                    AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024                AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                    AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                        AS group_max_dop
    ,wg.effective_max_dop                AS group_effective_max_dop
    ,wg.total_request_count                AS group_total_request_count
    ,wg.total_queued_request_count            AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT    pool_name
,        pool_max_mem_MB
,        group_name
,        group_importance
,        (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,        node_name
,        node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,    group_request_max_memory_grant_pcnt
,    group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Eşzamanlılık sınırları dikkate sorguları
Sorguların çoğu kaynak sınıfları tarafından yönetilir. Bu sorgular, her iki eş zamanlı sorgu ve eşzamanlılık yuvası eşikleri sığması gerekir. Bir kullanıcı, bir sorgu eşzamanlılık yuvası modelden dışlanacak seçemezsiniz.

Yinelemek için aşağıdaki deyimleri kaynak sınıfları dikkate:

* INSERT-SELECT
* GÜNCELLEŞTİRME
* DELETE
* (Kullanıcı tablosu sorgulanırken) seçin
* ALTER INDEX YENİDEN OLUŞTURMA
* ALTER INDEX REORGANIZE KOMUTUNU
* ALTER TABLE YENİDEN OLUŞTURMA
* DİZİN OLUŞTURMA
* KÜMELENMİŞ COLUMNSTORE DİZİNİ OLUŞTURMA
* TABLE AS SELECT (CTAS) OLUŞTURMA
* Veri yükleme
* Veri Taşıma hizmeti (DMS) tarafından gerçekleştirilen veri taşıma işlemleri

## <a name="query-exceptions-to-concurrency-limits"></a>Eşzamanlılık sınırları için sorgu özel durumlar
Bazı sorgular kullanıcı atandığı kaynak sınıfı getirmiyor. Belirli bir komut için gerekli bellek kaynakları genellikle komutu meta veri işlemi olduğundan düşük olduğunda bu özel durumlar eşzamanlılık sınırları için yapılır. Hiçbir zaman bunları gerekir sorgular için büyük bellek ayırmaları önlemek için bu özel durumlar amacı gerekir. Bu durumda, varsayılan küçük kaynak sınıfı (smallrc) kullanıcıya atanan gerçek kaynak sınıfı bağımsız olarak her zaman kullanılır. Örneğin, `CREATE LOGIN` smallrc içinde her zaman çalışır. Bu işlemi gerçekleştirmek için gereken kaynakları çok düşük olduğundan sorgu eşzamanlılık yuvası modele dahil edilecek algılama yapmaz.  Bu sorguları da 32 kullanıcı eşzamanlılık sınırı sınırlı değildir, bu sorguları sınırsız sayıda 1.024 oturumlarının oturum sınırı çalıştırabilirsiniz.

Aşağıdaki deyimleri kaynak sınıfları getirmiyor:

* DROP TABLE veya oluşturma
* ALTER TABLE... ANAHTAR, bölme veya bölüm birleştirme
* ALTER INDEX DEVRE DIŞI BIRAK
* DROP INDEX
* OLUŞTURMA, güncelleştirme ya da DROP STATISTICS
* TRUNCATE TABLE
* ALTER YETKİLENDİRME
* OTURUM AÇMA OLUŞTURUN
* CREATE, ALTER veya DROP USER
* OLUŞTURMA, değiştirme veya bırakma yordamı
* OLUŞTURMA veya DROP VIEW
* DEĞER EKLEME
* Sistem görünümleri ve Dmv'leri seçin
* AÇIKLANMAKTADIR
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <a name="changing-user-resource-class-example"></a> Bir kullanıcı kaynak sınıfı örneği değiştirme
1. **Oturum açma oluşturun:** bir bağlantı açmak, **ana** veritabanı, SQL veri ambarı veritabanını barındıran SQL Server'da ve aşağıdaki komutları yürütün.
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > Azure SQL Data Warehouse kullanıcılar için ana veritabanındaki bir kullanıcı oluşturmak için iyi bir fikirdir. Master kullanıcı oluşturma, kullanıcının bir veritabanı adı belirtmeden SSMS gibi araçları kullanarak oturum açma izin verir.  Ayrıca SQL server üzerinde tüm veritabanlarını görüntülemek için Nesne Gezgini kullanmalarını sağlar.  Oluşturma ve kullanıcıları yönetme hakkında daha fazla ayrıntı için bkz: [SQL veri ambarı veritabanında güvenli][Secure a database in SQL Data Warehouse].
   > 
   > 
2. **SQL veri ambarı kullanıcı oluşturun:** bir bağlantı açmak **SQL Data Warehouse** veritabanı ve aşağıdaki komutu yürütün.
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. **İzinleri verin:** aşağıdaki örnek verir `CONTROL` üzerinde **SQL Data Warehouse** veritabanı. `CONTROL` adlı veritabanı düzeyi SQL Server'daki db_owner eşdeğerdir.
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```
4. **Kaynak sınıfı artırmak:** daha yüksek bir iş yükü yönetim rolü için bir kullanıcı eklemek için aşağıdaki sorguyu kullanın.
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. **Kaynak sınıfı azaltın:** bir kullanıcı bir iş yükü yönetim rolünden kaldırmak için aşağıdaki sorguyu kullanın.
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > Smallrc bir kullanıcıyı kaldırmak mümkün değildir.
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a>Sıraya alınan sorgu algılama ve diğer Dmv'leri
Kullanabileceğiniz `sys.dm_pdw_exec_requests` bir eşzamanlılık sırada bekleyen sorguları tanımlamak için DMV. Sorgular bir eşzamanlılık yuva durumu için bekleyen **askıya**.

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

İş yükü yönetim rolleri ile görüntülenebilir `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Aşağıdaki sorguda her kullanıcının atandığı rolü gösterir.

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL veri ambarı türleri bekleyin aşağıdakileri içerir:

* **LocalQueriesConcurrencyResourceType**: dışında eşzamanlılık yuvası framework sit sorgular. DMV sorgular ve sistem işlevleri gibi `SELECT @@VERSION` yerel sorgular gösterilebilir.
* **UserConcurrencyResourceType**: içinde eşzamanlılık yuvası framework sit sorgular. Son kullanıcı tabloları sorguları bu kaynak türü kullanırsınız örnekleri temsil eder.
* **DmsConcurrencyResourceType**: veri taşıma işlemleri kaynaklanan bekler.
* **BackupConcurrencyResourceType**: Bu bekleme bir veritabanı yedekleniyor olduğunu gösterir. Bu kaynak türü için maksimum değeri 1'dir. Aynı anda birden çok yedekleme istenmiş, diğerleri sıraya koyar.

`sys.dm_pdw_waits` DMV, bir isteği bekliyor kaynakları görmek için kullanılabilir.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,    SESSION_ID()            AS Current_session
,    s.[status]            AS Session_status
,    s.[login_name]
,    s.[query_count]
,    s.[client_id]
,    s.[sql_spid]
,    r.[command]            AS Request_command
,    r.[label]
,    r.[status]            AS Request_status
,    r.[submit_time]
,    r.[start_time]
,    r.[end_compile_time]
,    r.[end_time]
,    DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,    DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,    DATEDIFF(ms,r.[end_compile_time],r.[end_time])        AS Request_execution_time_ms
,    r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID();
```

`sys.dm_pdw_resource_waits` DMV yalnızca belirli bir sorgu tarafından kullanılan kaynak bekler gösterir. Kaynak bekleme süresi yalnızca sorgu CPU üzerine zamanlamak temel alınan SQL sunucuları için gereken süredir sinyal bekleme süresi aksine sağlanması kaynaklar için bekleyen süreyi ölçer.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID();
```

`sys.dm_pdw_wait_stats` DMV bekler geçmiş eğilim analizi için kullanılabilir.

```sql
SELECT    w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Sonraki adımlar
Veritabanı kullanıcıları yönetme ve güvenlik hakkında daha fazla bilgi için bkz: [SQL veri ambarı veritabanında güvenli][Secure a database in SQL Data Warehouse]. Ne kadar büyük kaynak sınıfları hakkında daha fazla bilgi kümelenmiş columnstore dizini kalitesini artırmak, bkz [segment kalitesini artırmak için dizinleri yeniden oluşturma].

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[segment kalitesini artırmak için dizinleri yeniden oluşturma]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
