---
title: Kapasite planlama için Azure Search | Microsoft Docs
description: Azure Search, her bir kaynağın Faturalanabilir arama birimlerinde nerede fiyatlandırılır bölüm ve çoğaltma bilgisayar kaynakları ayarlayın.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/09/2017
ms.author: heidist
ms.openlocfilehash: 08ae64aa92d7262b462ad105aa8e776bdaef15c0
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="scale-resource-levels-for-query-and-indexing-workloads-in-azure-search"></a>Sorgu ve iş yüklerini Azure Search'te dizin oluşturma için ölçek kaynak düzeyleri
Çalıştırdıktan sonra [bir fiyatlandırma katmanı seçin](search-sku-tier.md) ve [bir arama hizmeti sağlamak](search-create-service-portal.md), isteğe bağlı olarak çoğaltma ya da hizmetiniz tarafından kullanılan bölümlerini sayısını artırmak için sonraki adımdır. Her katman fatura birimleri sabit sayıda sunar. Bu makalede, sorgu yürütme, dizin oluşturma ve depolama için gereksinimlerinizi dengeleyen bir en iyi yapılandırmasını elde etmek için bu birimleri tahsis açıklanmaktadır.

Kaynak yapılandırması, bir hizmetin ayarladığınızda kullanılabilir [temel katmana](http://aka.ms/azuresearchbasic) veya biri [standart katmanları](search-limits-quotas-capacity.md). Bu katmanlar adresindeki Hizmetleri için kapasite artışlarla satın *arama birimine* (SUs) burada her bölüm ve çoğaltma sayar bir SU. 

Daha az SUs sonuçları orantılı olarak daha düşük bir faturaya eklenecektir kullanma. Hizmet ayarlamak sürece faturalama için etkindir. Hizmet geçici olarak kullanmıyorsanız, faturalama önlemek için yalnızca hizmetin silinmesi ve gerektiğinde daha sonra yeniden oluşturulması yoludur.

> [!Note]
> Bir hizmetin silinmesi her şeyin üzerinde siler. Geri arama verilerini kalıcı ve Azure yedekleme için arama içinde hiçbir olanak yoktur. Yeni bir hizmet üzerinde varolan bir dizini yeniden dağıtmak için oluşturmak ve ilk olarak yüklemek için kullanılan program çalıştırmanız gerekir. 

## <a name="terminology-partitions-and-replicas"></a>Terminolojisi: bölümler ve çoğaltmalar
Bölümler ve çoğaltmalar bir arama hizmeti yedekleyen birincil kaynaklardır.

| Kaynak | Tanım |
|----------|------------|
|*Bölümler* | Dizin depolama ve g/ç okuma/yazma işlemleri (örneğin, yeniden oluşturma ve dizin yenilerken) sağlar.|
|*Çoğaltmaları* | Öncelikle Bakiye sorgu işlemleri yüklemek için kullanılan arama hizmeti örnekleri. Her çoğaltma her zaman bir dizinin bir kopya barındırır. 12 çoğaltmalar varsa, hizmette yüklenen her dizin 12 kopyalarını gerekir.|

> [!NOTE]
> Doğrudan yönetmek veya hangi dizinleri yineleme üzerinde çalışma yönetmek için bir yolu yoktur. Her çoğaltma her dizini bir kopyasını servis mimarisinin bir parçasıdır.
>

## <a name="how-to-allocate-partitions-and-replicas"></a>Bölümler ve çoğaltmalar tahsis etme
Azure Search'te bir hizmet başlangıçta en düşük düzeyde bir bölüm ve bir çoğaltma oluşan kaynakları tahsis edilir. Bunu destekleyen katmanları için daha fazla depolama ve g/ç gerekir veya daha büyük sorgu birimleri ya da daha iyi performans için daha fazla çoğaltmalarını eklerseniz bölümleri artırarak artımlı olarak hesaplama kaynaklarını ayarlayabilirsiniz. Tek bir hizmet (dizin oluşturma ve sorgular) tüm iş yüklerini işlemek için yeterli kaynak olması gerekir. Birden çok hizmet arasında iş yükleri ayırabilir olamaz.

Artırın veya çoğaltmaları ve bölümleri değiştirmek için Azure portalını kullanmanızı öneririz. Portal en fazla sınırları kalın izin verilen birleşimleri sınırları uygular:

1. Oturum [Azure portal](https://portal.azure.com/) ve arama hizmeti seçin.
2. İçinde **ayarları**, açık **ölçek** dikey ve artırmak veya bölümler ve çoğaltmalar sayısını azaltmak için kaydırma çubuklarını kullanın.

Bir komut dosyası veya kod tabanlı sağlama yaklaşım, gerekiyorsa [Yönetimi REST API](https://docs.microsoft.com/rest/api/searchmanagement/services) portalına alternatiftir.

Genellikle, hizmet işlemleri doğru sorgu iş yükleri özellikle eğilimi nedeniyle arama uygulamaları bölümler,'den daha fazla çoğaltmaları gerekir. Bölüm [yüksek kullanılabilirlik](#HA) nedenini açıklar.

> [!NOTE]
> Bir hizmet sağlandıktan sonra daha yüksek bir SKU yükseltilemez. Yeni katmanını bir arama hizmeti oluşturun ve dizinleri yeniden yüklemeniz gerekir. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) hizmet sağlama konusunda yardım için.
>
>

<a id="HA"></a>

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Kolay ve ölçeği oldukça hızlı olduğundan, genellikle bir bölüm ve bir başlayın veya iki çoğaltmaları ve ardından sorgu birimler olarak ölçek büyütme yapı öneririz. Sorgu iş yükleri öncelikle çoğaltmalar üzerinde çalıştırın. Daha fazla verimlilik veya yüksek kullanılabilirlik gerekiyorsa, ek çoğaltmaları büyük olasılıkla gerektirir.

Yüksek kullanılabilirlik için genel öneriler şunlardır:

* Salt okunur iş yüklerinin (sorgular) yüksek kullanılabilirlik için iki çoğaltma
* Okuma/yazma iş yüklerinin (sorgular ve tek tek belgeler eklendikçe, güncelleştirilmiş veya dizin) yüksek kullanılabilirlik için üç veya daha fazla çoğaltmaları

Azure arama için hizmet düzeyi sözleşmelerine (SLA), sorgu işlemleri ve ekleme, güncelleştirme veya silme belgeleri oluşur dizin güncelleştirmeleri hedefler.

Temel katman bir bölüm ve üç çoğaltmaları en üste. Dizin oluşturma ve sorgu işleme için isteğe bağlı dalgalanmalara hemen yanıtlamak için esneklik istiyorsanız, bir standart katmanı göz önünde bulundurun.

### <a name="index-availability-during-a-rebuild"></a>Bir yeniden oluşturma sırasında dizin kullanılabilirliği

Azure arama için yüksek kullanılabilirlik sorgular ve bir dizini yeniden oluşturma içermeyen dizin güncelleştirmeler için geçerlidir. Bir alan silin, veri türünü değiştirin veya alanı yeniden adlandırma, dizini yeniden oluşturmanız gerekir. Dizini yeniden oluşturmak için dizinini silmek, dizini yeniden oluşturun ve verileri yeniden yükleyin.

> [!NOTE]
> Dizin derlenmeden Azure Search dizini için yeni alanlar ekleyebilirsiniz. Yeni alanın değerini zaten dizindeki tüm belgeler için null olur.

Bir yeniden oluşturma sırasında dizin kullanılabilirliğini sağlamak için bir kopyasını bir dizin aynı hizmetin farklı bir adla veya farklı bir hizmet üzerinde aynı adda dizin kopyasını sahip ve ardından yeniden yönlendirme veya yük devretme mantığı kodunuzda sağlayın.

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma
Şu anda, olağanüstü durum kurtarma için yerleşik hiçbir mekanizması yoktur. Olağanüstü durum kurtarma hedefleri karşılamak için yanlış stratejisi, ekleme bölümleri veya çoğaltmaları olacaktır. En yaygın yaklaşım artıklık hizmet düzeyinde başka bir bölgede ikinci bir arama hizmeti ayarlayarak eklemektir. Kullanılabilirlik gibi bir dizini yeniden oluşturma sırasında birlikte yeniden yönlendirme veya yük devretme mantığı kodunuzdan gelmelidir.

## <a name="increase-query-performance-with-replicas"></a>Yinelemelerle sorgu performansı artırma
Sorgu gecikmesi ek çoğaltmaları gerekli olan bir göstergesidir. Genellikle, sorgu performansı iyileştirme bir ilk adım bu kaynağın daha fazla bilgi eklemektir. Çoğaltmaları ekledikçe dizini ek kopyalarını büyük sorgu iş yüklerini desteklemek ve yüklemek için çevrimiçi birden çok çoğaltma isteğini dengeleyin.

Sabit tahminleri / saniye (QPS) sorgularını sağladığımız olamaz: sorgu performansı rakip iş yükleri ve sorgu karmaşıklığına bağlıdır. Çoğaltmaları ekleme açıkça daha iyi performansla sonuçlanır rağmen sonuç kesinlikle doğrusal değil: üç çoğaltmaları ekleme garanti etmez Üçlü işleme.

QPS tahmin etme, iş yükleri için şu konularda rehberlik için bkz: [Azure Search performans ve en iyi duruma getirme konuları](search-performance-optimization.md).

## <a name="increase-indexing-performance-with-partitions"></a>Bölümleri olan dizin oluşturma performansı artırma
Gerçek zamanlı veri yenileme gerektiren arama uygulamalar orantılı olarak daha fazla bölüm çoğaltmaları daha gerekir. Bölümler ekleme okuma/yazma işlemleri çok sayıda işlem kaynakları arasında yayar. Bu aynı zamanda, daha fazla disk alanı ek dizinler ve belgeleri depolamak için sağlar.

Daha büyük dizinler sorguya daha uzun sürer. Bu nedenle, bölümler artımlı her artış çoğaltmaları küçük ancak orantılı bir artış gerektirdiğini görebilirsiniz. Sorgular ve sorgu toplu karmaşıklığı sorgu yürütme ne kadar hızlı açık içine öğeli.

## <a name="basic-tier-partition-and-replica-combinations"></a>Temel katman: bölüm ve çoğaltma birleşimler
Temel bir hizmet tam olarak bir bölüm olması ve üç çoğaltmaları kadar en için üç SUs sınırlamak. Çoğaltmaları yalnızca ayarlanabilir kaynaktır. En az iki çoğaltma sorguları yüksek kullanılabilirlik gerekir.

<a id="chart"></a>

## <a name="standard-tiers-partition-and-replica-combinations"></a>Standart katmanları: bölüm ve çoğaltma birleşimler
Bu tabloda, tüm standart katmanları için çoğaltmaları ve 36 SU sınırına bağlı bölümleri birleşimlerini desteklemek için gereken SUs gösterilmektedir.

|   | **1 bölümü** | **2 bölüm** | **3 bölümleri** | **4 bölümleri** | **6 bölümleri** | **12 bölümleri** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 çoğaltma** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 çoğaltma** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 çoğaltmaları** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 çoğaltmaları** |4 SU |8 SU |12 SU |16 SU |24 SU |Yok |
| **5 çoğaltmaları** |5 SU |10 SU |15 SU |20 SU |30 SU |Yok |
| **6 çoğaltmaları** |6 SU |12 SU |18 SU |24 SU |36 SU |Yok |
| **12 çoğaltmaları** |12 SU |24 SU |36 SU |Yok |Yok |Yok |

SUs, fiyatlandırma ve kapasite Azure Web sitesinde ayrıntılı olarak açıklanmıştır. Daha fazla bilgi için bkz: [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> Çoğaltmaları ve bölüm sayısı tam olarak 12 böler (özellikle, 1, 2, 3, 4, 6, 12). Böylece tüm bölümler eşit bölümlerinde yayılabilen Azure Search her dizin 12 parça önceden böler olmasıdır. Örneğin, üç bölüm hizmetiniz varsa ve dizin oluşturma, her bölüm dört parça dizinini içerecektir. Dizin bir Azure Search parça ne uygulama ayrıntılarını gelecekteki Sunumlarda değiştirilebilir. Sayı 12 Bugün olsa da, her zaman 12 gelecekte olması için bu sayıyı beklediğiniz döndürmemelidir.
>
>

## <a name="billing-formula-for-replica-and-partition-resources"></a>Çoğaltma ve bölüm kaynakları için faturalama formülü
Kaç tane SUs belirli birleşimleri için kullanılan hesaplama formülü çoğaltmaları ve bölümler, ürünüdür veya (R X P SU =). Örneğin, üç bölüm tarafından çarpılan üç çoğaltmaları fatura dokuz SUs.

SU başına maliyet, standart için temel daha düşük birim başına fatura oranı içeren katmanı tarafından belirlenir. Her katman üzerinde bulunabilir derecelendirir [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).
