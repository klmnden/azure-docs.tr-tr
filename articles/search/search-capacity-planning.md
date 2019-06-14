---
title: Ölçek bölümleri ve çoğaltmalarını sorgu ve dizin oluşturma - Azure arama
description: Azure Search, her kaynak Faturalanabilir arama birimleri cinsinden burada fiyatlandırılır bölüm ve çoğaltma bilgisayar kaynaklarını ayarlayın.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6879dd975f97ba2746165e87a135e5d90e8b229f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60308766"
---
# <a name="scale-partitions-and-replicas-for-query-and-indexing-workloads-in-azure-search"></a>Bölümleri ve çoğaltmalarını sorgu ve iş yüklerini Azure Search'te dizin oluşturma için ölçeklendirme
Çalıştırdıktan sonra [bir fiyatlandırma katmanı seçin](search-sku-tier.md) ve [bir arama hizmeti sağlama](search-create-service-portal.md), isteğe bağlı olarak çoğaltmalar veya bölümler hizmetiniz tarafından kullanılan sayısını artırmak için sonraki adımdır. Her katman, sabit bir faturalandırma birimi sayısı sunar. Bu makalede dengeleyen sorgu yürütme, dizin oluşturma ve depolama gereksinimlerinize en uygun yapılandırmayı elde etmek için bu birimleri ayırma açıklanmaktadır.

Kaynak yapılandırması, bir hizmetin ayarlarken kullanılabilir [temel katman](https://aka.ms/azuresearchbasic) veya biri [standart veya depolama için iyileştirilmiş Katmanlar](search-limits-quotas-capacity.md). Bu katmanda Hizmetleri için kapasite artışlarla satın alınır *arama birimleri* (su) burada her bölüm ve çoğaltma sayılır bir SU. 

Daha az SUs sonuç orantılı olarak daha düşük bir fatura kullanma. Hizmet ayarlandıysa sürece faturalandırma için geçerli olur. Hizmet geçici olarak kullanmıyorsanız, faturaları önlemek için yalnızca hizmetin silinmesi ve gerektiğinde daha sonra yeniden oluşturma yoludur.

> [!Note]
> Hizmet silme her şeyin üzerinde siler. Yedekleme için Azure Search içinde hiçbir olanak yoktur ve geri arama verilerini kalıcı. Yeni bir hizmet üzerinde mevcut bir dizini yeniden dağıtmak için oluşturma ve özgün yükleme için kullanılan program çalıştırmanız gerekir. 

## <a name="terminology-replicas-and-partitions"></a>Terimler: çoğaltmalar ve bölümler
Çoğaltmalar ve bölümler bir arama hizmeti yedekleyen birincil kaynaklardır.

| Resource | Tanım |
|----------|------------|
|*Bölümler* | Dizin depolaması ve g/ç için okuma/yazma işlemleri (örneğin, yeniden oluşturma ve dizin yenilerken) sağlar.|
|*Çoğaltmalar* | Öncelikle Bakiye sorgu işlemleri yüklemek için kullanılan arama hizmeti örnekleri. Her çoğaltma her zaman bir dizinin bir kopya barındırır. 12 çoğaltmalar varsa, hizmete yüklenen her dizin 12 kopyalarını gerekir.|

> [!NOTE]
> Doğrudan yönetmek veya yineleme üzerinde hangi dizinleri çalışma yönetmek için hiçbir yolu yoktur. Her çoğaltma üzerindeki her dizinin bir kopyasını servis mimarisinin bir parçasıdır.
>


## <a name="how-to-allocate-replicas-and-partitions"></a>Çoğaltmalar ve bölümler ayırma
Azure Search'te hizmet başlangıçta en düşük düzeyde bir bölüm ve bir çoğaltma oluşan kaynak tahsis edilir. Bu destek katmanları için daha fazla depolama ve g/ç gerekir ya da daha yüksek sorgu hacimleri ya da daha iyi performans için daha fazla çoğaltmalar ekleyerek bölüm sayısını artırarak artımlı olarak hesaplama kaynaklarını ayarlayabilirsiniz. Tek bir hizmet (dizin oluşturma ve sorguları) tüm iş yüklerini işlemek için yeterli kaynak olması gerekir. Birden çok hizmet arasında iş yükleri alt bölümlere olamaz.

Artırmak veya çoğaltmalar ve bölümler ayrılması değiştirmek için Azure portalını kullanmanızı öneririz. Portal sınırlarını kalın izin verilen birleşimleri barındırabileceğiniz zorlar. Bir betik veya kod tabanlı sağlama yaklaşımı gerekiyorsa [Azure PowerShell](search-manage-powershell.md) veya [Yönetimi REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/services) alternatif çözümler.

Genellikle, özellikle hizmet işlemleri sorgu iş yükleri doğru güçlü eğilimi nedeniyle arama uygulamaları bölümler, daha çok kopya gerekir. Bölüm [yüksek kullanılabilirlik](#HA) nedenini açıklar.

1. Oturum [Azure portalında](https://portal.azure.com/) ve arama hizmeti seçin.
2. İçinde **ayarları**açın **ölçek** sayfanıza çoğaltmalar ve bölümler. 

   Aşağıdaki ekran görüntüsünde, sağlanan bir bölüm ve çoğaltma ile standart bir hizmet gösterilmektedir. Formül altındaki kaç arama birimi kullanılan (1) gönderildiğini belirtir. Birim fiyatı 100 ABD doları (gerçek fiyat değil) varsa, bu hizmetin çalışıyor aylık maliyeti ortalama 100 ABD Doları olacaktır.

   ![Ölçek sayfası geçerli değerlerini gösteren](media/search-capacity-planning/1-initial-values.png "geçerli değerlerini gösteren ölçek sayfası")

3. Artırmak veya bölüm sayısını azaltmak için kaydırıcıyı kullanın. Formül altındaki kaç arama birimi kullanıldığını gösterir.

   Bu örnek, kapasite, yinelemeler ile iki katına çıkar ve her bölümler. Arama birimi sayısı dikkat edin. (2 x 2) bölümleri ile çarpılmış çoğaltmaları fatura formüldür artık dört olmasıdır. Kapasite Katlama hizmeti çalıştırmanın maliyeti birden iki katına çıkar. 100 ABD Doları arama birimi maliyeti ise yeni aylık fatura şimdi 400 olacaktır.

   Her bir katman birimi maliyetleri başına geçerli için ziyaret [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/).

   ![Çoğaltmalar ve bölümler ekleyin](media/search-capacity-planning/2-add-2-each.png "çoğaltmalar ve bölümler ekleyin")

3. Tıklayın **Kaydet** değişiklikleri onaylamak için.

   ![Ölçek ve faturalandırma değişiklikleri onaylayın](media/search-capacity-planning/3-save-confirm.png "ölçek ve faturalandırma değişiklikleri onaylayın")

   Kapasite değişiklikleri tamamlanması birkaç saat sürebilir. İşlemi başladıktan sonra hiçbir gerçek zamanlı izleme için çoğaltma ve bölüm ayarlamalar yoktur iptal edemezsiniz. Ancak, değişiklikler yapılıyor çalışırken aşağıdaki ileti görünür kalır.

   ![Durum iletisi portalında](media/search-capacity-planning/4-updating.png "portalında durumu iletisi")


> [!NOTE]
> Hizmet sağlandıktan sonra daha yüksek bir SKU yükseltilemez. Yeni katmanında bir arama hizmeti oluşturma ve dizinlerinizi yeniden yüklemeniz gerekir. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sağlama hizmeti ile ilgili Yardım için.
>
>

<a id="chart"></a>

## <a name="partition-and-replica-combinations"></a>Bölüm ve çoğaltma birleşimleri

Temel bir hizmet, tam olarak bir bölüm ve üç kopyaya kadar için en fazla üç SUs sınırlayın. Çoğaltmalar yalnızca ayarlanabilir kaynaktır. Sorgular üzerinde yüksek kullanılabilirlik için en az iki çoğaltmaları gerekir.

Tüm standart ve depolama için iyileştirilmiş arama hizmetleri, çoğaltmalar ve bölümler, 36 SU sınırına tabi aşağıdaki birleşimlerini kabul edilebilir. 

|   | **Bölüm 1** | **2 bölüm** | **3 bölümleri** | **4 bölüm** | **6 bölümleri** | **12 bölümleri** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 kopya** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 kopya** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 çoğaltma** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 kopya** |4 SU |8 SU |12 SU |16 SU |24 SU |Yok |
| **5 çoğaltma** |5 SU |10 SU |15 SU |20 SU |30 SU |Yok |
| **6 çoğaltma** |6 SU |12 SU |18 SU |24 SU |36 SU |Yok |
| **12 çoğaltmalar** |12 SU |24 SU |36 SU |Yok |Yok |Yok |

SUs, fiyatlandırma ve kapasite Azure Web sitesinde ayrıntılı olarak açıklanmıştır. Daha fazla bilgi için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> Çoğaltmalar ve bölümler sayısı tam olarak 12 böler (özellikle, 1, 2, 3, 4, 6, 12). Böylece tüm bölümler eşit bölümlerinde yayılabilen Azure Search her dizin 12 parçalara önceden böler olmasıdır. Örneğin, üç bölüm hizmetiniz varsa ve dizin oluşturma, her bölüm dizini dört parçalar içerir. Dizin bir Azure Search parçalar ne bir uygulama ayrıntısı gelecekte değişebilir serbest bırakır. Bugün kaç 12 olsa da her zaman 12 gelecekte olacak şekilde bu sayıyı beklediğiniz olmamalıdır.
>


<a id="HA"></a>

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Kolay ve hızlı göreceli olarak ölçeklendirilebilecek şekilde olduğundan, genellikle bir bölüm ve bir başlayın veya iki çoğaltmalar ve ardından sorgu birimler olarak ölçeği artırma yapı öneririz. Sorgu iş yükleri, öncelikle yinelemede çalıştırın. Daha fazla üretilen işi veya yüksek kullanılabilirlik gerekiyorsa, Ek çoğaltmalar büyük olasılıkla gerektirir.

Yüksek kullanılabilirlik için genel öneriler şunlardır:

* Salt okunur iş yüklerinin (sorgular) yüksek kullanılabilirlik için iki çoğaltma
* Okuma/yazma iş yüklerinin (sorgular ayrıca tek tek belgeler eklendi, güncelleştirildi veya dizin) yüksek kullanılabilirlik için üç veya daha fazla çoğaltma

Azure Search için hizmet düzeyi sözleşmeleri (SLA), sorgu işlemleri ve ekleme, güncelleştirme veya belgelerinin silinmesini bileşiminden dizin güncelleştirmelerini hedeflenir.

Temel katman bir bölüm ve üç kopyaya en üste. Dizin oluşturma ve sorgu aktarım hızı için isteğe bağlı dalgalanmaların hemen yanıt esnekliğini istiyorsanız, standart katmanlardan birine göz önünde bulundurun.  Depolama gereksinimleriniz, sorgu aktarım hızı çok daha hızlı büyüyor bulursanız, depolama için iyileştirilmiş katmanlardan birine göz önünde bulundurun.

### <a name="index-availability-during-a-rebuild"></a>Yeniden derleme sırasında dizin kullanılabilirlik

Azure Search için yüksek kullanılabilirlik sorgular ve bir dizini yeniden oluşturma içermeyen bir dizin güncelleştirmeleri için geçerlidir. Bir alan silin, bir veri türünü değiştirmek veya alanı yeniden adlandırma, dizini yeniden oluşturmanız gerekir. Bir dizini yeniden oluşturmak için dizini silmek, dizini yeniden oluşturun ve verileri yeniden yükleyin.

> [!NOTE]
> Dizinini yeniden oluşturmayı olmadan bir Azure Search dizinine yeni alanlar ekleyebilirsiniz. Yeni alan değeri dizindeki tüm belgeler için null olacaktır.

Yeniden derleme sırasında dizin kullanılabilirliği sürdürmek için bir kopyasını bir dizin aynı hizmetin farklı bir adla veya farklı bir hizmet üzerinde aynı ada sahip bir dizinin bir kopyasını ve daha sonra kodunuzda yeniden yönlendirme veya yük devretme mantığı sağlar.

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma
Şu anda, olağanüstü durum kurtarma için yerleşik bir mekanizma yoktur. Olağanüstü durum kurtarma hedefiyle toplantı için yanlış stratejisi, bölümleri veya çoğaltmaları eklenirken olacaktır. En yaygın bir yaklaşım ise başka bir bölge içinde ikinci bir arama hizmeti ayarlayarak hizmet düzeyinde yedeklilik eklemektir. Gibi bir dizini yeniden oluşturma sırasında kullanılabilirlik ile yeniden yönlendirme veya yük devretme mantığı kodunuzdan gelmelidir.

## <a name="increase-query-performance-with-replicas"></a>Yinelemelerle sorgu performansını artırma
Sorgunun gecikme süresi, Ek çoğaltmalar gerekli olup olmadığını bir göstergesidir. Genel olarak, sorgu performansını geliştirmeye yönelik ilk adımdır bu kaynağın daha fazla bilgi eklemektir. Çoğaltmalar ekleyerek gibi dizin ek kopya büyük sorgu iş yüklerini desteklemek için ve yüklemek için çevrimiçi üzerinde birden fazla çoğaltma istekleri dengeleyin.

Sabit tahminleri / saniye (QPS) sorguları sunamıyoruz: sorgu performansı üzerinde sorgu ve birbiriyle rekabet içindeki iş yükleri karmaşıklığına bağlıdır. Çoğaltmalar ekleyerek NET bir şekilde daha iyi performansla sonuçlanır olsa da, sonuç kesinlikle doğrusal değildir: üç kopyaya ekleme Üçlü aktarım hızı garantilemez.

İş yükleriniz için QPS tahmin kılavuzluk etmesi için bkz [Azure Search, performans ve iyileştirme konuları](search-performance-optimization.md).

## <a name="increase-indexing-performance-with-partitions"></a>Bölümleri olan dizin oluşturma performansı artırma
Gerçek zamanlı veri yenileme gerektiren uygulamalarda Ara orantılı olarak daha fazla bölüm çoğaltmaları daha gerekir. Bölümleri ekleme çok sayıda işlem kaynakları okuma/yazma işlemi yayar. Ayrıca, daha fazla disk alanı ek dizinler ve belgeler depolamak için tanır.

Daha büyük dizinler için sorgu daha uzun sürer. Bu nedenle, bölümler artımlı her artış çoğaltmaları daha küçük ancak orantılı bir artış gerektiğini fark edebilirsiniz. Sorguları ve sorgu karmaşıklığı, sorgu yürütme nasıl hızlı bir şekilde açık içine faktörü.


## <a name="next-steps"></a>Sonraki adımlar

[Azure arama için bir fiyatlandırma katmanı seçin](search-sku-tier.md)
