---
title: "SQL veri ambarı alt düzey istemcileri destekleyen veri denetimi için | Microsoft Docs"
description: "Verileri denetlemek için SQL Data Warehouse alt düzey istemci desteği hakkında bilgi edinin"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: dfe29ff3-dfeb-4309-83c0-c1a300f4f44e
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: a7ea6141285a0098339f1e071af2592dd4535c12
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>SQL Data Warehouse - denetime ve dinamik veri maskeleme için alt düzey istemci desteği
[Denetim](sql-data-warehouse-auditing-overview.md) TDS yeniden yönlendirmeyi destekleyen SQL istemcileri ile çalışır.

TDS 7.4 uygulayan herhangi bir istemci yeniden yönlendirme de desteklemelidir. Bu özel durumlar yeniden yönlendirme özelliğini tam olarak desteklenmez ve Node.JS hangi yeniden yönlendirmesi için Tedious uygulanmadı JDBC 4.0 içerir.

"Alt düzey istemciler için", yani hangi destek TDS sürüm 7.3 ve aşağıda - sunucunun FQDN bağlantı dizesi değiştirilmelidir:

Bağlantı dizesindeki özgün sunucunun FQDN: <*sunucu adı*>. database.windows.net

Bağlantı dizesindeki değiştirilmiş sunucu FQDN: <*sunucu adı*> .database. **güvenli**. windows.net

"Alt düzey istemciler" kısmi bir listesine içerir:

* .NET 4.0 ve aşağıdaki
* ODBC 10.0 ve aşağıdaki.
* JDBC (JDBC TDS 7.4 desteklerken, bu TDS yeniden yönlendirme özelliğini tam olarak desteklenmez)
* Can sıkıcı (için Node.JS)

**Açıklama:** yukarıdaki sunucunun FDQN değişikliği bir yapılandırma gereksinimini adım olmadan her veritabanı (geçici azaltma) de bir SQL Server düzeyi denetim ilkesi uygulamak için yararlı olabilir.     

