---
title: Azure SQL veritabanı ile ölçek genişletme | Microsoft Docs
description: Yazılım olarak hizmet (SaaS) geliştiricileri bu araçları kullanarak bulutta kolayca esnek, ölçeklenebilir veritabanları oluşturabilirsiniz
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 59701c31e461bbd5d73ec708504139347f6075f2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241863"
---
# <a name="scaling-out-with-azure-sql-database"></a>Azure SQL Veritabanı ile ölçek genişletme
Out kullanarak Azure SQL veritabanlarını kolayca ölçeklendirebilirsiniz **esnek veritabanı** araçları. Bu araçlar ve özellikler, veritabanı kaynaklarını kullanmanıza olanak tanır **Azure SQL veritabanı** hizmet (SaaS) uygulamaları olarak işlem tabanlı iş yüklerinizi ve özellikle yazılım çözümleri oluşturun. Elastik veritabanı özellikleri oluşur:

* [Elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md): İstemci kitaplığı oluşturmak ve parçalı veritabanlarını korumak sağlayan bir özelliktir.  Bkz: [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md).
* [Elastik veritabanı bölme-birleştirme aracını](sql-database-elastic-scale-overview-split-and-merge.md): parçalı veritabanları arasında verileri taşır. Bu aracı, verileri çok kiracılı veritabanından bir tek kiracılı veritabanı (veya tersi) taşıma için kullanışlıdır. Bkz: [elastik veritabanı bölme-Birleştirme aracı Öğreticisi](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Elastik veritabanı işleri](elastic-jobs-overview.md): İşleri, çok sayıda Azure SQL veritabanlarını yönetmek için kullanın. Şema değişiklikleri, kimlik yönetimi, başvuru verilerini güncelleştirme, performans verileri toplama veya işlemleriyle Kiracı (müşteri) telemetrisi toplama gibi yönetim işlemlerini kolayca gerçekleştirin.
* [Elastik veritabanı sorgusu](sql-database-elastic-query-overview.md) (Önizleme): Birden çok veritabanını kapsayan bir Transact-SQL sorgusunu çalıştırmayı sağlar. Bu, Excel, Power BI, Tableau, vb. gibi raporlama araçları bağlantısı sağlar.
* [Elastik işlemler](sql-database-elastic-transactions-overview.md): Bu özellik, Azure SQL veritabanı'nda birkaç veritabanlarına yayılan işlemler çalıştırmanıza olanak tanır. Elastik veritabanı işlem ADO .NET kullanarak .NET uygulamaları için kullanılabilir ve tanıdık programlama deneyimi kullanarak ile tümleştirme [System.Transaction sınıfları](https://msdn.microsoft.com/library/system.transactions.aspx).

Aşağıdaki grafikte içeren bir mimari gösterilmektedir **elastik veritabanı özellikleri** veritabanları koleksiyonunu ile ilgili.

Bu grafikte renkleri veritabanı şemaları temsil eder. Aynı renge veritabanlarında aynı şemaya paylaşın.

1. Bir dizi **Azure SQL veritabanlarına** parçalama mimari kullanarak Azure'da barındırılır.
2. **Elastik veritabanı istemci Kitaplığı** parça yönetmek için kullanılır.
3. Bir alt kümesini veritabanlarını yerleştirerek bir **elastik havuz**. (Bkz [havuz nedir?](sql-database-elastic-pool.md)).
4. Bir **elastik veritabanı** tüm veritabanlarında zamanlanan veya geçici T-SQL betikleri çalıştırır.
5. **Bölme-birleştirme aracını** verileri bir parçadan veri taşımak için kullanılır.
6. **Esnek veritabanı sorgusu** , parça kümesi içindeki tüm veritabanlarına yayılan bir sorgu yazmanızı sağlar.
7. **Elastik işlemler** birkaç veritabanlarına yayılan işlemler çalıştırmanıza olanak tanır. 

![Elastik Veritabanı araçları][1]

## <a name="why-use-the-tools"></a>Araçlar neden kullanmalısınız?
Esneklik ve bulut uygulamaları için ölçek elde edin VM ve blob depolama için basit olmamıştı - yalnızca ekleme veya çıkarma birimleri veya power artırın. Ancak, durum bilgisi olan veri işlemede ilişkisel veritabanları için bir sınama kaldığını. Bu senaryolarda zorluklar ortaya:

* Büyütme ve küçültme ilişkisel veritabanı bölümü iş yükü için kapasite.
* Belirli bir alt kümesini bir meşgul son müşteri (Kiracı) gibi veri - etkileyen kaynaklanabilecek etkin noktaları yönetme.

Geleneksel olarak, bu gibi senaryolarda büyük ölçekli veritabanı sunucuları uygulamayı desteklemek için yatırım tarafından çözüldü. Ancak, bu seçenek, tüm işlem önceden tanımlanmış ticari donanımlarda nerede olacağını bulutta sınırlıdır. Bunun yerine, veri dağıtımı ve aynı şekilde yapılandırılmış birçok veritabanı arasında işleme ("parçalama" bilinen bir ölçek genişletme düzeni) geleneksel ölçek büyütme yaklaşım açısından maliyeti ve esneklik bir alternatif sunar.

## <a name="horizontal-and-vertical-scaling"></a>Yatay ve dikey ölçeklendirme
Aşağıdaki şekilde elastik veritabanları ölçeklendirilebilir temel yolu olan ölçeklendirmenin, yatay ve dikey boyutları gösterir.

![Yatay ve dikey ölçeklendirme][2]

Yatay ölçeklendirme ekleyerek veya "ölçeklendirme" de bahsedilen kapasiteye ya da genel performansı ayarlamak için veritabanlarını kaldırarak başvuruyor. Parçalama, veri koleksiyonu aynı şekilde yapılandırılmış veritabanları arasında bölümlenen yatay ölçeklendirme uygulamak için genel bir yoludur.  

Dikey ölçeklendirme artan ya da tek bir veritabanının işlem boyutunu azaltarak, diğer adıyla "büyütme." anlamına gelir

Çoğu bulut ölçeğinde veritabanı uygulamalar, bu iki stratejiler birleşimini kullanın. Örneğin, bir hizmet uygulaması olarak bir yazılım yeni son müşteri sağlamak için yatay ölçekleme ve dikey ölçeklendirme büyütün ya da iş yüküne göre kaynakları daralmasına her son müşterinin veritabanı izin vermek için kullanabilir.

* Yatay ölçeklendirme yönetilir kullanarak [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md).
* Dikey ölçeklendirme gerçekleştirilir Hizmet katmanını değiştirmek için Azure PowerShell cmdlet'lerini kullanarak veya elastik havuzdaki veritabanları koyarak.

## <a name="sharding"></a>Parçalama
*Parçalama* büyük miktarlarda veri aynı şekilde yapılandırılmış bir dizi bağımsız veritabanları arasında dağıtmak için bir tekniktir. Bulut geliştiricilerine hizmet (SAAS) teklifi olarak, son müşterilere veya işletmeler için yazılım oluşturma ile özellikle yaygın olarak kullanılır. Bu son müşterilerin, genellikle "kiracıları" olarak da adlandırılır. Parçalama, birkaç nedenden için gerekli olabilir:  

* Toplam veri miktarı tek bir veritabanının kısıtlamaları içinde sığmayacak kadar büyük
* Tek veritabanı özelliklerine genel iş yükü işlem verimini aşıyor
* Her Kiracı için ayrı veritabanlarına gereken şekilde kiracılar fiziksel birbirlerinden, gerektirebilir
* Farklı bölümlerini bir veritabanı uyumluluk, performans veya jeopolitik nedeniyle farklı coğrafyalara yer açmanız gerekebilir.

Dağıtılmış cihazlardan veri alma gibi diğer senaryolarda parçalama zamansal olarak düzenlenen bir veritabanları kümesi doldurmak için kullanılabilir. Örneğin, ayrı bir veritabanı, her gün veya hafta ayrılabilir. Bu durumda, parçalama anahtarı (mevcut parçalı tablonun tüm satırlarını) tarihini temsil eden bir tamsayı olabilir ve söz konusu aralığını kapsayan veritabanlarının alt uygulama tarafından yönlendirilmesi gereken sorgular bir tarih aralığındaki bilgileri alınıyor.

Tek bir değere bir parçalama anahtarı bir uygulamada her işlem kısıtlanabilir olduğunda parçalama en iyi şekilde çalışır. Bu, tüm işlemler için belirli bir veritabanı yerel olmasını sağlar.

## <a name="multi-tenant-and-single-tenant"></a>Çok kiracılı ve tek kiracılı
Bazı uygulamalar, her Kiracı için ayrı bir veritabanı oluşturmanın en kolay yaklaşım kullanır. Bu yaklaşım **tek kiracılı parçalama düzeni** yalıtım, yedekleme/geri yükleme özelliği ve kiracının ayrıntı düzeyinde ölçeklendirme kaynak sağlar. Tek kiracılı parçalama ile her veritabanı bir belirli Kiracı kimliği değeri (veya müşteri anahtar değeri ile) ilişkilidir, ancak bu anahtara her zaman verilerinde mevcut olması gerekmez. Bunu her istek için uygun veritabanı - yönlendirmek için uygulamanın sorumluluğundadır ve istemci kitaplığının bu kolaylaştırabilir.

![Tek bir kiracı ile çok kiracılı karşılaştırması][4]

Diğer senaryoları paketi birden çok kiracının birlikte halinde bunları ayrı veritabanlarına yalıtma yerine veritabanları. Bu tipik bir desendir **çok kiracılı parçalama düzeni** - ve uygulama çok sayıda küçük Kiracı yönetir olgusu odaklı. Çok kiracılı parçalama veritabanı tablolarını satırlarda tüm Kiracı kimliği tanımlayan anahtar veya parçalama anahtarı yürütmek için tasarlanmıştır. Tekrar uygun veritabanına bir kiracının istek yönlendirme için uygulama katmanı sorumludur ve bu elastik veritabanı istemci kitaplığı tarafından desteklenebilir. Satır düzeyi güvenlik filtresine hangi satırların her Kiracı erişebilir - Ayrıntılar için bkz. Ayrıca, kullanılabilir [esnek veritabanı araçlarını ve satır düzeyi güvenlik ile çok kiracılı uygulamaları](sql-database-elastic-tools-multi-tenant-row-level-security.md). Veritabanları arasında verileri yeniden dağıtma ile çok kiracılı parçalama düzeni gerekli olabilir ve elastik veritabanı bölme-Birleştirme aracı tarafından sağlanır. Elastik havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Birden çok veri taşıma tek kiracılı veritabanlarına
Bir SaaS uygulaması oluştururken, ileriki müşterilerini yazılım deneme sürümü teklifi normaldir. Bu durumda, uygun maliyetli bir çok kiracılı veritabanı verileri için kullanın. Bir müşteri adaylarını olur, çünkü daha iyi performans sağlar ancak, tek kiracılı veritabanı daha iyidir. Müşteri verileri deneme süresi boyunca oluşturduysanız kullanın [bölme-birleştirme aracını](sql-database-elastic-scale-overview-split-and-merge.md) verileri birden çok kiracıdan yeni tek kiracılı veritabanına taşımak için.

## <a name="next-steps"></a>Sonraki adımlar
İstemci Kitaplığı gösteren bir örnek uygulama için bkz: [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md).

Araçlarını kullanmak için mevcut veritabanlarını dönüştürün için bkz: [ölçeğini genişletmek için mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md).

Esnek havuz ayrıntılarını görmek için bkz: [elastik havuzlar için fiyat ve performans hususları](sql-database-elastic-pool.md), veya ile yeni bir havuz oluşturma [elastik havuzlar](sql-database-elastic-pool-manage-portal.md).  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

