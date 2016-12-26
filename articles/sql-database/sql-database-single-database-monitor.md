---
title: "Azure SQL Veritabanı’nda veritabanı performansını izleme | Microsoft Belgeleri"
description: "Azure araçlarını ve dinamik yönetim görünümlerini kullanarak veritabanınızı izleme seçenekleri hakkında bilgi edinin."
keywords: "veritabanı izleme, bulut veritabanı performansı"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: a2e47475-c955-4a8d-a65c-cbef9a6d9b9f
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/27/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: c0c9d107ff1642d66e96de5409863e4e894d1b6b


---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Azure SQL Database'de veritabanı performansını izleme
Azure SQL veritabanı performansını izlemeye, seçtiğiniz veritabanı performans düzeyiyle ilgili kaynak kullanımını izleyerek başlarsınız. İzleme, veritabanınızın gerekenden fazla kapasiteye sahip olup olmadığını veya veritabanınızda kaynak kullanımının üst sınıra ulaşması nedeniyle bir sorun olup olmadığını belirlemenize yardımcı olur. Böylece veritabanınızın performans düzeyinin ve [hizmet katmanının](sql-database-service-tiers.md) ayarlanma zamanının gelip gelmediğine karar verirsiniz. Veritabanınızı [Azure portalında](https://portal.azure.com) bulunan grafik araçlarını veya SQL [dinamik yönetim görünümlerini](https://msdn.microsoft.com/library/ms188754.aspx) kullanarak izleyebilirsiniz.

## <a name="monitor-databases-using-the-azure-portal"></a>Azure portalını kullanarak veritabanlarını izleme
[Azure portalında](https://portal.azure.com/), tek başına veritabanlarının kullanımını veritabanınızı seçip **İzleme** grafiğine tıklayarak izleyebilirsiniz. Bu işlem sonrasında bir **Ölçüm** penceresi görüntülenir. **Grafiği düzenle** düğmesine tıklayarak değişiklik yapabilirsiniz. Şu ölçümleri ekleyin:

* CPU yüzdesi
* DTU yüzdesi
* Veri G/Ç yüzdesi
* Veri boyutu yüzdesi

Bu ölçümleri ekledikten sonra **İzleme** grafiğinde bulunan **Ölçüm** penceresinde bu ölçümleri daha ayrıntılı şekilde görüntülemeye devam edebilirsiniz. Dört ölçümün tümü de veritabanınızın ortalama **DTU** kullanım yüzdesini gösterir. DTU'lar ile ilgili ayrıntılı bilgi için bkz. [hizmet katmanları](sql-database-service-tiers.md).

![Hizmet katmanına göre veritabanı performansını izleme.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Performans ölçümlerine ilişkin uyarıları da yapılandırabilirsiniz. **Ölçüm** penceresindeki **Uyarı ekle** düğmesine tıklayın. Uyarınızı yapılandırmak için sihirbazı takip edin. Ölçümlerin belirli bir eşiği aşması veya belirli bir eşiğin altına düşmesi halinde uyarı alabilirsiniz.

Örneğin, veritabanınızdaki bir iş yükünün artmasını bekliyorsanız bir e-posta uyarısı yapılandırarak veritabanınızın herhangi bir performans ölçümünde %80 sınırına ulaşması halinde uyarı alabilirsiniz. Bu uyarıyı, daha yüksek bir performans düzeyine ne zaman geçmeniz gerektiğini anlamak üzere erken bir uyarı olarak kullanabilirsiniz.

Performans ölçümleri, daha düşük bir performans düzeyine geçip geçemeyeceğinizi belirlemenize de yardımcı olabilir. Standart S2 veritabanını kullandığınızı ve tüm performans ölçümlerinin, veritabanının belirli bir zaman için ortalama %10'dan daha fazla kullanımda bulunmadığını gösterdiğini varsayın. Bu, veritabanının Standart S1'de de düzgün şekilde çalışabileceğini gösterir. Ancak daha düşük bir performans düzeyine geçmeye karar vermeden önce, ani değişiklik veya dalgalanma gösteren iş yüklerine dikkat edin.

## <a name="monitor-databases-using-dmvs"></a>DMV'leri kullanarak veritabanlarını izleme
Portalda kullanıma sunulan ölçümler aynı zamanda şu sistem görünümleri aracılığıyla da kullanılabilir: sunucunuzun mantıksal **asıl** veritabanında bulunan [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) ve kullanıcı veritabanında bulunan [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx). Daha uzun bir sürede daha az ayrıntılı veri izlemeniz gerekiyorsa **sys.resource_stats** görünümünü kullanın. Daha küçük bir zaman diliminde daha fazla ayrıntılı veri izlemeniz gerekiyorsa **sys.dm_db_resource_stats** görünümünü kullanın. Daha fazla bilgi için bkz. [Azure SQL Database Performans Rehberi](sql-database-performance-guidance.md#monitor-resource-use).

> [!NOTE]
> **sys.dm_db_resource_stats**, Web ve İşletme sürümü veritabanlarında (bu sürümler kullanımdan kaldırılmıştır) kullanıldığında boş bir sonuç kümesi getirir.
>
>

Elastik havuzlar için bu bölümde açıklanan olan tekniklerle havuzda bulunan tek veritabanlarını izleyebilirsiniz. Ancak havuzu bir bütün olarak da izleyebilirsiniz. Bilgi için bkz. [Elastik havuz izleme ve yönetme](sql-database-elastic-pool-manage-portal.md).



<!--HONumber=Dec16_HO2-->


