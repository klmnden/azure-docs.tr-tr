<properties
    pageTitle="Azure SQL Database'de veritabanı performansını izleme | Microsoft Azure"
    description="Azure araçlarını ve dinamik yönetim görünümlerini kullanarak veritabanınızı izleme seçenekleri hakkında bilgi edinin."
    keywords="veritabanı izleme, bulut veritabanı performansı"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>


# Azure SQL Database'de veritabanı performansını izleme
Azure SQL veritabanı performansını izlemeye, seçtiğiniz veritabanı performans düzeyiyle ilgili kaynak kullanımını izleyerek başlarsınız. İzleme, veritabanınızın gerekenden fazla kapasiteye sahip olup olmadığını veya veritabanınızda kaynak kullanımının üst sınıra ulaşması nedeniyle bir sorun olup olmadığını belirlemenize yardımcı olur. Böylece veritabanınızın performans düzeyinin ve [hizmet katmanının](sql-database-service-tiers.md) ayarlanma zamanının gelip gelmediğine karar verirsiniz. Veritabanınızı [Azure portalında](https://portal.azure.com) bulunan grafik araçlarını veya SQL [dinamik yönetim görünümlerini](https://msdn.microsoft.com/library/ms188754.aspx) kullanarak izleyebilirsiniz.

## Azure portalını kullanarak veritabanlarını izleme

[Azure portalında](https://portal.azure.com/), tek veritabanlarının kullanımını veritabanınızı seçip **İzleme** grafiğine tıklayarak izleyebilirsiniz. Bu işlem sonrasında bir **Ölçüm** penceresi görüntülenir. **Grafiği düzenle** düğmesine tıklayarak değişiklik yapabilirsiniz. Şu ölçümleri ekleyin:

- CPU yüzdesi
- DTU yüzdesi
- Veri G/Ç yüzdesi
- Veri boyutu yüzdesi

Bu ölçümleri ekledikten sonra **İzleme** grafiğinde bulunan **Ölçüm** penceresinde bu ölçümleri daha ayrıntılı şekilde görüntülemeye devam edebilirsiniz. Dört ölçümün tümü de veritabanınızın ortalama **DTU** kullanım yüzdesini gösterir. DTU'lar ile ilgili ayrıntılı bilgi için bkz. [hizmet katmanları](sql-database-service-tiers.md).

![Hizmet katmanına göre veritabanı performansını izleme.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Performans ölçümlerine ilişkin uyarıları da yapılandırabilirsiniz. **Ölçüm** penceresindeki **Uyarı ekle** düğmesine tıklayın. Uyarınızı yapılandırmak için sihirbazı takip edin. Ölçümlerin belirli bir eşiği aşması veya belirli bir eşiğin altına düşmesi halinde uyarı alabilirsiniz.

Örneğin, veritabanınızdaki bir iş yükünün artmasını bekliyorsanız bir e-posta uyarısı yapılandırarak veritabanınızın herhangi bir performans ölçümünde %80 sınırına ulaşması halinde uyarı alabilirsiniz. Bu uyarıyı, daha yüksek bir performans düzeyine ne zaman geçmeniz gerektiğini anlamak üzere erken bir uyarı olarak kullanabilirsiniz.

Performans ölçümleri, daha düşük bir performans düzeyine geçip geçemeyeceğinizi belirlemenize de yardımcı olabilir. Standart S2 veritabanını kullandığınızı ve tüm performans ölçümlerinin, veritabanının belirli bir zaman için ortalama %10'dan daha fazla kullanımda bulunmadığını gösterdiğini varsayın. Bu, veritabanının Standart S1'de de düzgün şekilde çalışabileceğini gösterir. Ancak daha düşük bir performans düzeyine geçmeye karar vermeden önce, ani değişiklik veya dalgalanma gösteren iş yüklerine dikkat edin.

## DMV'leri kullanarak veritabanlarını izleme

Portalda kullanıma sunulan ölçümler aynı zamanda şu sistem görünümleri aracılığıyla da kullanılabilir: sunucunuzun mantıksal **asıl** veritabanında bulunan [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) ve kullanıcı veritabanında bulunan [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx). Daha uzun bir sürede daha az ayrıntılı veri izlemeniz gerekiyorsa **sys.resource_stats** görünümünü kullanın. Daha küçük bir zaman diliminde daha fazla ayrıntılı veri izlemeniz gerekiyorsa **sys.dm_db_resource_stats** görünümünü kullanın. Daha fazla bilgi için bkz. [Azure SQL Database Performans Rehberi](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats).

>[AZURE.NOTE] **sys.dm_db_resource_stats**, Web ve İşletme sürümü veritabanlarında (bu sürümler kullanımdan kaldırılmıştır) kullanıldığında boş bir sonuç kümesi getirir.

Esnek veritabanı havuzları için bu bölümde açıklanan olan tekniklerle havuzda bulunan tek veritabanlarını izleyebilirsiniz. Ancak havuzu bir bütün olarak da izleyebilirsiniz. Bilgi için bkz. [Esnek veritabanı havuzunu izleme ve yönetme](sql-database-elastic-pool-manage-portal.md).



<!--HONumber=Sep16_HO3-->


