---
title: SQL veri ambarı sınıflandırma | Microsoft Docs
description: Eşzamanlılık, önem derecesi, yönetmek ve işlem kaynakları için sorgular Azure SQL veri ambarı için sınıflandırma kullanma yönergeleri.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 888a64de29178834fc47199a033eb6bc62858e57
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617759"
---
# <a name="sql-data-warehouse-workload-classification-preview"></a>SQL veri ambarı iş yükü sınıflandırma (Önizleme)

Bu makalede, gelen istekleri kaynak sınıfı ve önemi atamak SQL veri ambarı iş yükü sınıflandırma işlemi açıklanmaktadır.

> [!Note]
> İş yükü Sınıflandırma, SQL veri ambarı Gen2'te önizlemesi için kullanılabilir. 9 Nisan 2019 veya sonraki bir sürüm tarihi yapılarla Önizleme iş yükü yönetimi sınıflandırma ve önem derecesi içindir.  Kullanıcılar, iş yükü yönetimi için test derlemeleri bu tarihten önceki kullanmaktan kaçınmanız gerekir.  Derleme iş yükü yönetimi özelliğine sahip olup olmadığını belirlemek için çalıştırdığınızda @ seçin@version SQL veri ambarı Örneğinize bağlandığında.

## <a name="classification"></a>Sınıflandırma

> [!Video https://www.youtube.com/embed/QcCRBAhoXpM]

İş yükü yönetimi sınıflandırma iş yükü ilkeleri atama aracılığıyla isteklerine uygulanmasını sağlayan [kaynak sınıfları](resource-classes-for-workload-management.md#what-are-resource-classes) ve [önem](sql-data-warehouse-workload-importance.md).

Veri ambarı iş yükleriniz sınıflandırmak için birçok yolu olsa da, basit ve en yaygın yükleme ve sorgu sınıflandırılmasıdır. INSERT, update ve delete deyimleri ile veri yükleme.  Verileri seçer kullanarak sorgulayın. Veri depolama çözümü, genellikle daha fazla kaynak ile daha yüksek bir kaynak sınıfı atamak gibi yük etkinliği için bir iş yükü İlkesi gerekir. Etkinlikleri yüklenemiyor kıyasla daha düşük önem gibi bir sorgu için farklı iş yükü ilke uygulayabilirsiniz.

Ayrıca, yükleme ve sorgu iş yüklerinizi subclassify. Subclassification iş yüklerinizi daha fazla denetim verir. Örneğin, sorgu iş yükleri küpün yenileme, Pano sorguları veya geçici sorgular oluşabilir. Her biri farklı kaynak sınıfları veya önem ayarı olan bu sorgu iş yükleri sınıflandırabilirsiniz. Yük subclassification da yararlanabilirsiniz. Büyük dönüştürmeleri için daha büyük kaynak sınıfları atanabilir. Yüksek önem derecesi, hava durumu verileri önce yükleyici veya sosyal veri akışı anahtar satış verilerini sağlamak için kullanılabilir.

Tüm ifadeler, bunlar kaynakları gerektirmez veya önem yürütme etkilemek için gereken şekilde sınıflandırılır.  DBCC komutları, başlangıç, işleme ve ROLLBACK TRANSACTION deyimlerini yok olarak sınıflandırılır.

## <a name="classification-process"></a>Sınıflandırma işlemi

SQL veri ambarı'nda sınıflandırma gerçekleştirilir bugün kullanıcılara atanmış kullanarak karşılık gelen bir kaynak sınıfına sahip bir role atayarak [sp_addrolemember](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql). Bir kaynak sınıfı için bir oturum açma ötesinde istekleri niteleyen olanağı, bu özellik ile sınırlıdır. Sınıflandırma için daha zengin bir yöntem ile kullanıma sunuldu [iş YÜKÜ SINIFLANDIRICI oluşturma](/sql/t-sql/statements/create-workload-classifier-transact-sql) söz dizimi.  Bu söz dizimi ile SQL veri ambarı kullanıcı isteklerini önem ve bir kaynak sınıfı atayabilirsiniz.  

> [!NOTE]
> Sınıflandırma, istek başına üzerinde değerlendirilir. Tek bir oturumda birden çok istek farklı şekilde sınıflandırılabilir.

## <a name="classification-precedence"></a>Sınıflandırma önceliği

Sınıflandırma işleminin bir parçası olarak, öncelik, hangi kaynak sınıfına atanır belirlemek için yerinde olduğundan. Bir veritabanı kullanıcı tabanlı sınıflandırma rolü üyeliği daha önceliklidir. Sınıflandırıcı oluşturursanız, mediumrc kaynak sınıfına Kullanıcıa veritabanı kullanıcısı eşler. Ardından, (Kullanıcıa üyesi olduğu) RoleA veritabanı rolü largerc kaynak sınıfına eşleyin. Veritabanı kullanıcısı mediumrc kaynak sınıfına eşleyen bir sınıflandırıcı largerc kaynak sınıfına RoleA veritabanı rolüne eşleyen bir sınıflandırıcı önceliklidir.

Bir kullanıcı bir veya birden çok sınıflandırıcılar eşleşen atanmış farklı kaynak sınıfları ile birden fazla rolün üyesi ise, kullanıcının en yüksek ataması sınıfı verilir.  Bu davranış, var olan kaynak sınıfı atama davranışı ile tutarlıdır.

## <a name="system-classifiers"></a>Sistem sınıflandırıcı

İş yükü sınıflandırması sistem iş yükü sınıflandırıcı vardır. Sistem sınıflandırıcılar, normal öncelikli olarak kaynak sınıfı kaynak ayırmalar için mevcut kaynak sınıfı rol üyeliklerini eşleyin. Sistem sınıflandırıcılar bırakılamaz. Sistem sınıflandırıcılar görüntülemek için çalıştırabileceğiniz sorgu aşağıda:

```sql
SELECT * FROM sys.workload_management_workload_classifiers where classifier_id <= 12
```

## <a name="mixing-resource-class-assignments-with-classifiers"></a>Kaynak sınıf ödevleri sınıflandırıcılar ile karıştırma

Sistem sınıflandırıcılar sizin adınıza oluşturulur, iş yükü sınıflandırmayla geçirmek için kolay bir yol sağlar. Yeni sınıflandırıcılar önem derecesiyle oluşturmak başlangıç olarak sınıflandırma önceliğe sahip kaynak sınıfı rolüne eşlemeleri kullanarak misclassification için neden olabilir.

Aşağıdaki senaryoyu göz önünde bulundurun:

Var olan veri ambarı •an DBAUser largerc kaynak sınıfı rolüne atanmış bir veritabanı kullanıcısı vardır. Kaynak sınıf atamasını sp_addrolemember ile yapılmaktaydı.
•Hedef veri ambarı iş yükü yönetimi ile güncelleştirilmiştir.
Yeni sınıflandırma söz dizimini (aynı DBAUser üyesi) DBARole bunları mediumrc ve yüksek önem derecesi için eşleme için oluşturulan bir sınıflandırıcı sahip veritabanı rolü test • tıklatın.
•When DBAUser oturum açması ve bir sorgu çalıştırır, sorgu için largerc atanır. Bir kullanıcının bir rol üyeliği göre öncelikli olduğundan.

Sorun giderme misclassification basitleştirmek için iş yükü sınıflandırıcılar oluştururken kaynak sınıfı rolüne eşlemeleri kaldırmanız önerilir.  Aşağıdaki kod mevcut bir kaynak sınıfı rol üyeliklerini döndürür.  Çalıştırma [sp_droprolemember](/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql) her üye adı için karşılık gelen kaynak sınıfından döndürdü.

```sql
SELECT  r.name AS [Resource Class]
,       m.name AS membername
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r ON rm.role_principal_id = r.principal_id
JOIN    sys.database_principals AS m ON rm.member_principal_id = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc','staticrc10','staticrc20','staticrc30','staticrc40','staticrc50','staticrc60','staticrc70','staticrc80');

--for each row returned run
sp_droprolemember ‘[Resource Class]’, membername
```

## <a name="next-steps"></a>Sonraki adımlar

SQL veri ambarı iş yükü sınıflandırma ve önemi hakkında daha fazla bilgi için bkz. [iş yükü sınıflandırıcı oluşturma](quickstart-create-a-workload-classifier-tsql.md) ve [SQL veri ambarı önem](sql-data-warehouse-workload-importance.md). Bkz: [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) sorgular ve atanan önem görüntülemek için.
