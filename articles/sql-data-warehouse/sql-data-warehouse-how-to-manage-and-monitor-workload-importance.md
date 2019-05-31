---
title: Yönetme ve izleme Azure SQL veri ambarı iş yükü önem | Microsoft Docs
description: Yönetme ve düzey önem istek izleme hakkında bilgi edinin.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: workload management
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: a39d71e0f8b8072cab6a3af9a2f0913973f303ee
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235935"
---
# <a name="manage-and-monitor-workload-importance-in-azure-sql-data-warehouse"></a>Yönetme ve Azure SQL veri ambarı iş yükü önem izleme

Yönetebilir ve istek düzeyi önemi DMVs ve katalog görünümlerini kullanarak Azure SQL veri ambarı izleyebilirsiniz.

## <a name="monitor-importance"></a>İzleyici önem derecesi

İzleme kullanarak yeni önem sütununa önem [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=azure-sqldw-latest) dinamik yönetim görünümünü.
Aşağıda izleme sorgu gönderme zamanı ve sorgular için başlangıç zamanı gösterir. Gönderme zamanı gözden geçirin ve başlangıç zamanı zamanlama önem nasıl etkilediği görmek için önem yanı sıra.

```sql
SELECT s.login_name, r.status, r.importance, r.submit_time, r.start_time
  FROM sys.dm_pdw_exec_sessions s
  JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
  WHERE r.resource_class is not null
ORDER BY r.start_time
```

Daha fazla zamanlama sorguların nasıl gönderildiğini içine aramak için Katalog görünümleri kullanın.

## <a name="manage-importance-with-catalog-views"></a>Önem derecesi Katalog görünümleri ile yönetme

Sys.workload_management_workload_classifiers Katalog görünümü, Azure SQL veri ambarı Örneğinizde sınıflandırıcılar ilgili bilgiler içerir. Dışlanacak eşlemek için kaynak sınıfları sistem tanımlı sınıflandırıcı aşağıdaki kodu yürütün:

```sql
SELECT *
  FROM sys.workload_management_workload_classifiers
  WHERE classifier_id > 12
```

Katalog görünümü [sys.workload_management_workload_classifier_details](/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql?view=azure-sqldw-latest), sınıflandırıcı oluşturulmasında kullanılan parametreler hakkında bilgi içerir.  ExecReportsClassifier üzerinde oluşturulan sorgu gösterir ```membername``` parametresi ExecutiveReports değerlerle için:

```sql
SELECT c.name,cd.classifier_type, classifier_value
  FROM sys.workload_management_workload_classifiers c
  JOIN sys.workload_management_workload_classifier_details cd
    ON cd.classifier_id = c.classifier_id
  WHERE c.name = 'ExecReportsClassifier'
```

![Sorgu sonuçları](./media/sql-data-warehouse-how-to-manage-and-monitor-workload-importance/wlm-query-results.png)

Sorun giderme misclassification basitleştirmek için iş yükü sınıflandırıcılar oluştururken kaynak sınıfı rolüne eşlemeleri kaldırmanız önerilir. Aşağıdaki kod mevcut bir kaynak sınıfı rol üyeliklerini döndürür. Her biri için sp_droprolemember çalıştırma ```membername``` karşılık gelen kaynak sınıfından döndürdü.
Bir iş yükü sınıflandırıcı bırakmadan önce bulunup bulunmadığını denetleme bir örnek aşağıda verilmiştir:

```sql
IF EXISTS (SELECT 1 FROM sys.workload_management_workload_classifiers WHERE name = 'ExecReportsClassifier')
  DROP WORKLOAD CLASSIFIER ExecReportsClassifier;
GO
```

## <a name="next-steps"></a>Sonraki adımlar
- Sınıflandırma hakkında daha fazla bilgi için bkz. [iş yükü sınıflandırma](sql-data-warehouse-workload-classification.md).
- Önem derecesi hakkında daha fazla bilgi için bkz. [iş yükü önem derecesi](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [İş yükü önem yapılandırma bölümüne Git ](sql-data-warehouse-how-to-configure-workload-importance.md)
