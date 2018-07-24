---
title: Haziran 2018 tarihinden itibaren Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 5f4d39c6aa1a5c2c30e84fbf26535fe3ee7799a6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216521"
---
# <a name="whats-new-in-azure-sql-data-warehouse-july-2018"></a>Azure SQL veri ambarı'nda yenilikler nelerdir? Temmuz 2018
Azure SQL veri ambarı, sürekli olarak iyileştirmeler alır. Bu makalede, Temmuz 2018'de sunulan değişiklikler ve yeni özellikleri açıklar.


## <a name="finer-granularity-for-cross-region-and-server-restores"></a>Çapraz bölge ve sunucu geri yüklemeler için daha iyi tanecikli
Bölgeler ve her 24 saatte gerçekleşen coğrafi olarak yedekli yedeklemeleri seçmek yerine herhangi bir geri yükleme noktası kullanarak sunucuları arasında şimdi geri yükleyebilirsiniz. Bölge ve sunucu geri yükleme, her iki kullanıcı tanımlı ya da otomatik geri yükleme noktaları daha iyi tanecikli ek veri koruma için etkinleştirilmesi için desteklenir. Daha fazla geri yükleme noktaları ile kullanılabilir veri Ambarınızı bölgeler arasında geri yüklerken mantıksal olarak tutarlı olur emin olabilirsiniz.

![Command - Azure SQL veri ambarını geri yükleme](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6ac23972-9ec0-4502-ab10-7b6bc1a3d947.png)
![geri yükleme seçeneklerinizi - Azure SQL veri ambarı](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6c63bd0e-9c52-414d-b4be-d3bd3774ee08.png)

Daha fazla bilgi için [hızlandırılmış ve esnek geri yükleme noktaları](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) blog gönderisi.

## <a name="20-minute-restorations"></a>20 dakika geri yüklemeler
SQL veri ambarı artık sunan herhangi bir veri ambarı'nın bir geri yükleme olur **20 dakikadan** veritabanı boyutundan bağımsız olarak aynı bölge içinde. Geri yükleme işlemi, aynı veya farklı bir mantıksal sunucu aynı bölge içinde olup olmadığını geri zaman geçerlidir. Ayrıca, anlık görüntü işlemi, geri yükleme noktası oluşturmak için gereken süreyi azaltmak için iyileştirilmiştir. Alt içinde geliştirilmesi performans düzeyleri (alt DWU ayarları) olan bir *2 x azaltma* anlık görüntü oluşturma zamanında.

Daha fazla bilgi için [hızlandırılmış ve esnek geri yükleme noktaları](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) blog gönderisi.

## <a name="custom-restoration-configurations"></a>Özel bir geri yükleme yapılandırmaları
Şimdi, Azure portalında geri yüklerken performans düzeyinizi (DWU) değiştirebilirsiniz. Ayrıca, bir yükseltilmiş 2. nesil veri ambarı'na geri yükleyebilirsiniz. Gen 2 örneğine geri yükleyerek etkisi önce Gen2, artık değerlendirebilirsiniz [Gen1 veri ambarınıza yükseltme](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation).

![Özel bir geri yükleme yapılandırması - Azure SQL veri ambarı](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/f4c410c7-8515-409c-a983-0976792b8628.png)

## <a name="spdescribeundeclaredparameters"></a>SP_DESCRIBE_UNDECLARED_PARAMETERS
[Sp_describe_undeclared_parameters](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql) saklı yordam genellikle araçları tarafından parametreleri Transact-SQL toplu işindeki hakkındaki meta verileri almak için kullanılır. Yordam Bu parametre için çıkarılan tür bilgisi batch'le her parametre için bir satır döndürür. 

```sql
EXEC sp_describe_undeclared_parameters
    @tsql = 
    N'SELECT
        object_id, name, type_desc
      FROM
        sys.indexes
      WHERE
        object_id = @id;'
```

Sonuç kümesi hakkında meta veriler içeren `@id` parametre (kısmi sonuç gösterilir)
```
parameter_ordinal | name | suggested_system_type_id | suggested_system_type_name
--------------------------------------------------------------------------------
1                 | @id  | 56                       | int
```