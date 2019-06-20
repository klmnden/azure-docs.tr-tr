---
title: Azure SQL veri ambarı'nda iş yükü önem yapılandırma | Microsoft Docs
description: İstek düzeyi önem kurmayı öğrenin.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: workload management
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 10a1fe2bff43b6799f47fc0245802ce578ef8f8f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165522"
---
# <a name="configure-workload-importance-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda iş yükü önem yapılandırın

SQL veri ambarı'nda önem ayarı, sorguların zamanlama etkilemek sağlar. Sorguları daha yüksek önem derecesi daha düşük önem derecesiyle önce sorguları çalıştırmak için zamanlanır. Sorguları önemi atamak için iş yükü sınıflandırıcı oluşturmanız gerekir.

## <a name="create-a-workload-classifier-with-importance"></a>Öncelikli olarak iş yükü sınıflandırıcı oluşturma

Genellikle bir veri ambarı senaryosunda sorguları hızlı bir şekilde çalıştırmak için gereken kullanıcılar sahiptir.  Kullanıcının, raporları veya kullanıcı çalıştırmak için gereken yöneticiler şirketin Analistin bir geçici sorgu çalıştırmanın olabilir olabilir. Bir sorguya önemi atamak için bir iş yükü sınıflandırıcı oluşturursunuz.  Aşağıdaki örnekler yeni kullanın [iş yükü sınıflandırıcı oluşturma](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) iki sınıflandırıcı oluşturmak için söz dizimi.  Membername, tek bir kullanıcı veya grup olabilir. Bireysel kullanıcı sınıflandırmaları rol sınıflandırmaları daha önceliklidir. Var olan veri ambarı kullanıcıları bulmak için şunu çalıştırın:

```sql
Select name from sys.sysusers
```

Bir iş yükü sınıflandırıcı oluşturmak için daha yüksek önem derecesi olan bir kullanıcı için çalıştırın:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
                   ,MEMBERNAME        = 'name'  
                   ,IMPORTANCE        =  above_normal);  

```

Geçici sorgular çalıştırma daha düşük önem derecesiyle çalıştıran kullanıcı için bir iş yükü sınıflandırıcı oluşturmak için:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
                   ,MEMBERNAME        = 'name'  
                   ,IMPORTANCE        =  below_normal);  
```

## <a name="next-steps"></a>Sonraki Adımlar
- İş yükü yönetimi hakkında daha fazla bilgi için bkz: [iş yükü sınıflandırma](sql-data-warehouse-workload-classification.md)
- Önem derecesi hakkında daha fazla bilgi için bkz. [iş yükü önem derecesi](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [Yönet gidin ve iş yükü önem izleyin](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
