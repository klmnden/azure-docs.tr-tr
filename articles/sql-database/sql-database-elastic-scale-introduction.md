---
title: Azure SQL Database ile ölçek genişletme | Microsoft Docs
description: Bu araçları kullanarak bulutta bir hizmet (SaaS) geliştiriciler olarak yazılım kolayca esnek, ölçeklenebilir veritabanları oluşturabilirsiniz
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 4944c1c017dbb59b7880a73bce7d0a9b0d972b3f
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="scaling-out-with-azure-sql-database"></a>Azure SQL Database ile ölçek genişletme
Out kullanarak Azure SQL veritabanlarını kolayca ölçeklendirebilirsiniz **esnek veritabanı** araçları. Bu araçları ve özelliklerinin veritabanı kaynaklarını kullanmanıza olanak tanır **Azure SQL veritabanı** hizmet (SaaS) uygulamaları olarak işlem iş yükleri ve özellikle yazılım çözümleri oluşturmak için. Esnek veritabanı özellikleri oluşur:

* [Esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md): istemci kitaplığı oluşturun ve parçalı veritabanlarını korumanıza olanak sağlayan bir özelliktir.  Bkz: [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md).
* [Esnek veritabanı bölünmüş Birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md): parçalı veritabanları arasında verileri taşır. Bu, verileri bir çok kiracılı veritabanından bir tek Kiracı veritabanı (veya tersi) taşıma için kullanışlıdır. Bkz: [esnek veritabanı bölünmüş Birleştirme aracı Öğreticisi](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Esnek veritabanı iş](sql-database-elastic-jobs-overview.md) (Önizleme): Azure SQL veritabanları çok sayıda yönetmek için işlerini kullanın. Kolayca şema değişiklikleri, kimlik bilgileri yönetimi, başvuru veri güncelleştirmeleri, performans verileri toplama veya Kiracı (müşteri) telemetri koleksiyonunu işlemleriyle gibi yönetim işlemlerini gerçekleştirin.
* [Esnek veritabanı sorgusu](sql-database-elastic-query-overview.md) (Önizleme): birden çok veritabanı yayılan bir Transact-SQL sorgusu çalıştırmanıza olanak sağlar. Bu bağlantı Excel, Powerbı, Tableau, vb. gibi raporlama araçları sağlar.
* [Esnek işlemleri](sql-database-elastic-transactions-overview.md): Bu özellik, Azure SQL veritabanı birkaç veritabanlarında span işlemleri çalıştırmanıza izin verir. Esnek veritabanı işlemleri ADO .NET kullanarak .NET uygulamaları için kullanılabilir ve tanıdık programlama deneyimi kullanarak ile tümleştirmek [System.Transaction sınıfları](https://msdn.microsoft.com/library/system.transactions.aspx).

Aşağıdaki grafikte içeren bir mimari gösterilir **esnek veritabanı özelliklerini** veritabanları koleksiyonunu göre.

Bu grafikte renkleri veritabanı şemaları temsil eder. Veritabanları aynı rengi ile aynı şema paylaşır.

1. Bir dizi **Azure SQL veritabanlarını** parçalama mimarisi kullanarak Azure'da barındırılır.
2. **Esnek veritabanı istemci Kitaplığı** parça kümeyi yönetmek için kullanılır.
3. Bir alt veritabanlarının içine konur bir **esnek havuz**. (Bkz [bir havuzu nedir?](sql-database-elastic-pool.md)).
4. Bir **esnek veritabanı iş** karşı tüm veritabanları zamanlanan veya geçici T-SQL betikleri çalıştırır.
5. **Bölünmüş Birleştirme aracı** verilerini bir parça diğerine taşımak için kullanılır.
6. **Esnek veritabanı sorgusu** parça kümesindeki tüm veritabanları yayılan bir sorgu yazmanızı sağlar.
7. **Esnek işlemleri** , çeşitli veritabanları span işlemleri çalıştırmanızı sağlar. 

![Elastik Veritabanı araçları][1]

## <a name="why-use-the-tools"></a>Araçlar neden kullanılır?
Esneklik ve bulut uygulamaları için ölçek elde VM'ler için basit olmuştur ve blob depolama - yalnızca eklemek veya birimleri çıkarmak veya güç artırın. Ancak, durum bilgisi olan veri işleme ilişkisel veritabanları için bir sınama kaldığını. Bu senaryolarda zorluklar ortaya çıktı:

* Büyüyen ve İş yükünüzün ilişkisel veritabanı parçası için kapasite küçültme.
* Belirli bir alt - meşgul end-müşteri (Kiracı) gibi veri kümesini etkileyen kaynaklanabilecek etkin yönetme.

Geleneksel olarak, aşağıdaki gibi senaryoları büyük ölçekli veritabanı sunucuları uygulamayı desteklemek için yatırım tarafından giderilmiştir. Ancak, bu seçenek, tüm işlemler önceden tanımlanmış ticari donanımlarda nerede olacağını bulutta sınırlıdır. Bunun yerine, veri dağıtma ve aynı şekilde yapılandırılmış birçok veritabanı arasında işleme ("parçalama" bilinen bir genişleme deseni) alternatif geleneksel büyütme yaklaşımlar hem maliyet ve esneklik sağlar.

## <a name="horizontal-and-vertical-scaling"></a>Yatay ve dikey ölçekleme
Aşağıdaki şekilde esnek veritabanları Genişletilebilir temel yolu olan ölçeklendirme, yatay ve dikey boyutları gösterir.

![Yatay Dikey Ölçek genişletme karşılaştırması][2]

Yatay ölçekleme ekleme veya kapasite ya da genel performansı ayarlamak için veritabanları kaldırma anlamındadır. Bu aynı zamanda "ölçeklendirme" belirtilmiştir. Veri özdeş olarak yapılandırılmış veritabanları koleksiyonunda bölümlenmiş parçalama yatay ölçekleme uygulamak için genel bir yoludur.  

Dikey ölçekleme başvuruyor artırarak veya azaltarak tek bir veritabanının performans düzeyini için — bu "ölçeklendirmeyi." olarak da bilinir

Çoğu bulut ölçekli veritabanı uygulamaları bu iki stratejileri birleşimini kullanın. Örneğin, bir hizmet uygulaması olarak bir yazılım büyütür veya iş yükü tarafından gerektiği gibi kaynakları küçültür her bitiş müşterinin veritabanına izin vermek için yeni son müşterilere sağlamak için yatay ölçekleme ve dikey ölçeklendirme kullanabilir.

* Yatay ölçekleme yönetilir kullanarak [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md).
* Dikey ölçeklendirme gerçekleştirilir Hizmet katmanını değiştirmek için Azure PowerShell cmdlet'lerini kullanarak veya bir esnek havuz veritabanları koyarak.

## <a name="sharding"></a>Parçalama
*Parçalama* büyük miktarlarda özdeş olarak yapılandırılmış veri bağımsız veritabanı sayısı dağıtmak için bir tekniktir. Son müşterilere veya işletmeler için yazılım hizmet (SAAS) tekliflerini oluşturma bulut geliştiriciler ile özellikle yaygın olarak kullanılır. Bu son müşterilere "kiracılar" anılır. Parçalama birkaç nedenden dolayı için gerekli olabilir:  

* Toplam veri miktarını tek bir veritabanının kısıtlamaları içinde sığmayacak kadar büyük
* Tek veritabanı özelliklerine genel iş yükü işlem verimliliğini aşıyor
* Ayrı veritabanlarını her bir kiracı için gerektiği şekilde kiracılar birbirinden, fiziksel yalıtım gerektirebilir
* Bir veritabanı farklı bölümlerini uyumluluk, performans veya jeopolitik nedeniyle farklı coğrafyalara bulunması gerekebilir.

Dağıtılmış aygıtlardan veri alım gibi diğer senaryolarda parçalama geçici olarak düzenlenmiş bir veritabanları kümesi doldurmak için kullanılabilir. Örneğin, ayrı bir veritabanı her gün veya hafta ayrılabilir. Bu durumda, parçalama anahtarı (mevcut parçalı tabloları tüm satırlarda) tarihini temsil eden bir tamsayı olabilir ve söz konusu aralığını kapsayan veritabanlarının alt uygulama tarafından yönlendirilmesi gereken sorgular bir tarih aralığı için bilgileri alınıyor.

Bir uygulamadaki her işlem tek bir parçalama anahtarı değerine kısıtlanabilir zaman parçalama en iyi şekilde çalışır. Tüm işlemleri belirli bir veritabanına yerel sağlar.

## <a name="multi-tenant-and-single-tenant"></a>Çok kiracılı ve tek Kiracı
Bazı uygulamalar her bir kiracı için ayrı bir veritabanı oluşturmanın en kolay yaklaşım kullanın. Bu **tek bir kiracı parçalama düzeni** yalıtımı, yedekleme/geri yükleme yeteneği ve Kiracı bazda ölçeklendirme kaynak sağlar. Tek bir kiracı parçalama ile her veritabanı bir belirli Kiracı kimliği değeri (veya müşteri anahtar değeri ile) ilgili değildir ancak bu anahtarı her zaman verilerde bulunması gerekmez. Bu her istek için uygun veritabanı - yönlendirmek için uygulamanın sorumluluğundadır ve istemci kitaplığının bu kolaylaştırabilir.

![Tek bir kiracı çok kiracılı karşılaştırması][4]

Diğer senaryolar paketi birden çok kiracıya birlikte ayrı veritabanlarına yalıtma yerine veritabanları içine. Tipik bir budur **çok kiracılı parçalama düzeni** - ve uygulamanın çok sayıda küçük kiracılar yönetir gerçeğiyle güdümlü. Çok kiracılı parçalama içinde satır veritabanı tablolarındaki tüm Kiracı kimliği tanımlayan anahtar veya parçalama anahtar taşımak için tasarlanmıştır. Yeniden, uygulama katmanı uygun veritabanına bir kiracının isteği yönlendirme için sorumludur ve bu esnek veritabanı istemci kitaplığı tarafından desteklenebilir. Satır düzeyi güvenlik filtresine hangi satırların her Kiracı erişebilir - Ayrıntılar için bkz. Ayrıca, kullanılabilir [çok kiracılı uygulamalarla esnek veritabanı araçlarını ve satır düzeyi güvenlik](sql-database-elastic-tools-multi-tenant-row-level-security.md). Verileri veritabanları arasında yeniden dağıtma çok kiracılı parçalama deseni gerekli olabilir ve bu esnek veritabanı bölünmüş Birleştirme aracı tarafından sayesinde kolaylaşır. Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Database kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Birden çok from verileri taşımak için tek Kiracı veritabanları
Bir SaaS uygulaması oluştururken, müşteri adayları yazılım deneme sürümünü sunmak için normaldir. Bu durumda, verileri birden çok Kiracı veritabanı kullanmak için uygun maliyetli. Bir müşteri adaylarını olur, çünkü daha iyi performans sağlar ancak, tek Kiracı veritabanı daha iyidir. Müşteri verileri deneme süresi boyunca yarattıysanız kullanmak [bölünmüş Birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md) verileri birden çok kiracıdan yeni tek Kiracı veritabanına taşımak için.

## <a name="next-steps"></a>Sonraki adımlar
İstemci Kitaplığı gösteren bir örnek uygulama için bkz: [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md).

Araçları kullanmak için var olan veritabanlarını dönüştürmek için bkz: [genişletilecek varolan veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md).

Esnek havuz ayrıntılarını görmek için bkz: [esnek havuzlar için fiyat ve performans konuları](sql-database-elastic-pool.md), veya ile yeni bir havuz oluşturma [esnek havuzlar](sql-database-elastic-pool-manage-portal.md).  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

