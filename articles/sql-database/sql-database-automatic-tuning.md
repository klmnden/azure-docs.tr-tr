---
title: "Azure SQL veritabanı - otomatik ayarlama | Microsoft Docs"
description: "Azure SQL veritabanını SQL sorgusunun analiz eder ve otomatik olarak kullanıcı iş yüküne uyum sağlar."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: drasumic
editor: danimir
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 09/19/2017
ms.author: jovanpop
ms.openlocfilehash: 1e884754682ecab4cdf097bd75caa6fcf2e0a29c
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Azure SQL veritabanı'nda otomatik ayarlama

Azure SQL veritabanı otomatik ayarlama ve kararlı iş yüklerinin sürekli performans yapay zeka kullanılarak ayarlama aracılığıyla en yüksek performans sağlar.

Otomatik ayarlama yerleşik zekaya sürekli olarak bir veritabanı üzerinde yürütülen sorgularını izlemek için kullandığı bir tam olarak yönetilen bir hizmettir ve otomatik olarak kendi performansını artırır. Bu veritabanı iş yüklerini değiştirme ve ayarlama önerileri uygulamak için dinamik olarak uyarlama aracılığıyla sağlanır. Otomatik ayarlama yapay zeka aracılığıyla Azure ile ilgili tüm veritabanlarındaki yatay olarak öğrenir ve dinamik olarak ayarlamasını eylemlerini artırır. Uzun bir Azure SQL veritabanı otomatik üzerinde ayarlama ile çalışır, daha iyi gerçekleştirir.

Azure SQL veritabanı otomatik ayarlama kararlı ve yoğun iş yükleri gerçekleştirme sağlamak için etkinleştirebilirsiniz en önemli özelliklerden biri olabilir.

## <a name="what-can-automatic-tuning-do-for-you"></a>Otomatik ayarlama sizin için ne?

- Azure SQL veritabanlarının otomatik performans ayarlama
- Performans artışı otomatik doğrulanması
- Otomatik geri alma ve kendi kendine düzeltme
- Geçmiş günlük ayarlama
- Eylem T-SQL betikleri dağıtımlar için ayarlama
- Öngörülü iş yükü performansını izleme
- Yüz binlerce veritabanları yeteneğini ölçeğini genişletme
- DevOps kaynaklara ve toplam sahip olma maliyetini pozitif etkisi

## <a name="safe-reliable-and-proven"></a>Güvenli, güvenilir ve kanıtlanmış

Azure SQL veritabanları için uygulanan ayarlama işlemleri, en yoğun iş yükleri performans için tam olarak güvenlidir. Sistem, kullanıcı iş yükleri ile etkileşime değil dikkatli tasarlanmıştır. Otomatik ayarlama önerileri yalnızca düşük kullanımı zamanlarda uygulanır. Sistem iş yükü performansını korumak için otomatik ayarlama işlemleri geçici olarak devre dışı bırakabilirsiniz. Böyle bir durumda, Azure portalında "Sistem tarafından devre dışı" iletisi gösterilir. Otomatik ayarlama en yüksek kaynak önceliğe sahip iş yüklerini tahminini.

Otomatik ayarlama mekanizmaları olgun ve yüz binlerce Azure üzerinde çalışan veritabanları üzerinde perfected. Uygulanan otomatik ayarlama işlemleri pozitif geliştirme iş yükü performansı olduğundan emin olmak için otomatik olarak doğrulanır. Gerileyen performansı önerileri dinamik olarak algılandı ve derhal döndürüldü. Üzerinden ayarlama geçmiş oturum her Azure SQL veritabanına yapılan geliştirmeler ayarlama, düz bir izleme yoktur. 

Otomatik ayarlama works genel bir bakış ve tipik kullanım senaryoları için lütfen katıştırılmış video bakın:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-Automatic-Tuning/player]
>

Azure SQL veritabanı otomatik ayarlama, çekirdek mantığını SQL Server otomatik ayarlama altyapısı ile paylaşıyor. Yerleşik zekaya mekanizmasını ek teknik bilgiler için bkz: [SQL Server otomatik ayarlama](https://docs.microsoft.com/en-us/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="use-automatic-tuning"></a>Otomatik ayarlama kullanın

Otomatik ayarlama, aboneliğinizde el ile etkinleştirilmesi gerekir. Azure portalını kullanarak otomatik ayarlamayı etkinleştirmek için bkz: [otomatik ayarlamayı etkinleştirmek](sql-database-automatic-tuning-enable.md).

Otomatik ayarlama otonom şekilde otomatik olarak ayarlama önerileri, performans artışı otomatik doğrulama dahil olmak üzere uygulama üzerinden çalışır. 

Daha fazla denetim için öneriler ayarlama, otomatik uygulama kapatılabilir ve öneriler ayarlama el ile Azure Portalı aracılığıyla uygulanabilir. Yalnızca otomatik ayarlama önerileri görüntülemek ve komut dosyaları ve araçları tercih ettiğiniz el ile uygulamak için çözüm kullanmak da mümkündür. 

## <a name="automatic-tuning-options"></a>Otomatik ayarlama seçenekleri

Azure SQL veritabanı'nda kullanılabilir otomatik ayarlama seçenekleri şunlardır:
 1. **CREATE INDEX** İş yükünüzün performansını artırabilir, dizinler oluşturur ve sorguların performansını artırmak doğrular dizinleri tanımlar.
 2. **DROP INDEX** yedekli ve yinelenen dizinler ve uzun süre içinde kullanılmadı dizinleri tanımlar.
 3. **ZORLA son iyi planı** , önceki iyi planı yavaş yürütme planı kullanarak SQL sorguları tanımlar ve bilinen son iyi plan gerileyen plan yerine kullanılır.

Azure SQL veritabanı tanımlayan **CREATE INDEX**, **DROP INDEX**, ve **ZORLA son iyi planlama** veritabanınızı en iyi duruma getirebilirsiniz ve bunları Azure Portalı'nda görünecek öneriler. Konumundaki değiştirilmelidir dizinleri tanımlaması hakkında daha fazla bilgi bulmak [dizin önerileri Azure Portalı'nda bulmak](sql-database-advisor-portal.md). Ya da önerileri Portalı'nı kullanarak el ile uygulayabilirsiniz veya otomatik olarak önerileri uygulamak, değişiklikten sonra iş yükünü izlemek için Azure SQL veritabanı sağlar ve önerisi, İş yükünüzün performansını geliştirilmiş doğrulayın.

Seçenekleri ayarlama otomatik bağımsız olarak açmak veya kapatmak veritabanı başına açılabilir veya bunlar kullanılabilir mantıksal sunucusunda yapılandırılan ve sunucudan ayarlarını devralır her veritabanı üzerinde uygulanabilir. Otomatik sunucusunda seçeneklerini ayarlama ve sunucu veritabanlarında ayarlarını devralmak yapılandırma çok sayıda veritabanı üzerinde otomatik ayarlama seçenekleri yönetimini basitleştirir olduğundan otomatik ayarlama yapılandırma yöntemi önerilir.

## <a name="next-steps"></a>Sonraki adımlar

- Otomatik İş yükünüzün yönetmek için Azure SQL veritabanında ayarlamayı etkinleştirmek için bkz: [otomatik ayarlamayı etkinleştirmek](sql-database-automatic-tuning-enable.md).
- El ile gözden geçirin ve öneriler ayarlama otomatik uygulamak için bkz: [bulmak ve performans önerileri uygulamak](sql-database-advisor-portal.md).
- Otomatik ayarlamak için kullanılan yerleşik zekaya hakkında daha fazla bilgi için bkz: [yapay zeka ayarladığını Azure SQL veritabanlarını](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Azure SQL Database ve SQL server 2017'de otomatik ayarlama çalıştığı hakkında daha fazla bilgi edinmek için [SQL Server otomatik ayarlama](https://docs.microsoft.com/en-us/sql/relational-databases/automatic-tuning/automatic-tuning).
