---
title: include dosyası
description: include dosyası
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/09/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: 524bc1d3a19ad8ecfc8d0d210e735d6a9ae0058b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34678317"
---
### <a name="set-up-pagination"></a>Sayfa numaralandırma ayarlayın

Bazı bağlayıcılar ve birden çok öğe alma eylemlerini için sonuçlarınızı bağlayıcı'nın varsayılan sayfa boyutunu aşabilir. Bu durumda, eylem sonuçlarını yalnızca ilk sayfasına döndürür. Örneğin, varsayılan sayfa boyutu **SQL Server - Get satırları** eylem 2048'dir, ancak başka ayarlarına göre farklılık gösterebilir. Tüm kayıtları almak emin olmak için Aç **sayfalandırma** Bu eylem için ayarlama. Bu ayar mantıksal uygulamanızı sahip bağlayıcı için geri kalan kayıtları isteyin, ancak işlem sona erdiğinde tüm sonuçları tek bir ileti döndürür. 

Burada belirli eylemler için sayfalandırma açabilirsiniz yalnızca bazı bağlayıcılar şunlardır: 

* <a href="https://docs.microsoft.com/connectors/db2/" target="_blank">DB2</a>
* <a href="https://docs.microsoft.com/connectors/dynamicscrmonline/" target="_blank">Dynamics 365 CRM Online</a>
* <a href="https://docs.microsoft.com/connectors/excel/" target="_blank">Excel</a>
* <a href="https://docs.microsoft.com/connectors/oracle/" target="_blank">Oracle Veritabanı</a>
* <a href="https://docs.microsoft.com/connectors/sharepointonline/" target="_blank">SharePoint</a>
* <a href="https://docs.microsoft.com/connectors/sql/" target="_blank">SQL Server</a> 

Örneği için **satırları Al** eylem:

1. Eylem sayfalandırma destekleyip desteklemediğini öğrenmek için eylemin açmak **ayarları**. 

   !["Ayarlar" eylemini, açın](./media/connectors-pagination-bulk-data-transfer/sql-action-settings.png)

2. Eylem sayfalandırma destekliyorsa, değiştirme **sayfalandırma** setting from **kapalı** için **üzerinde**. Eylem en az bir sonuç kümesi bulduğundan emin olmak için için bir değer belirtin **sınırı**.

   ![Eylem sonuçlarını en az sayıda döndürme belirtin](./media/connectors-pagination-bulk-data-transfer/sql-action-settings-pagination.png)

3. Hazır olduğunuzda, seçin **Bitti**.