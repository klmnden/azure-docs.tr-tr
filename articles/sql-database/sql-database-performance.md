---
title: İzleme ve performansı - Azure SQL veritabanı | Microsoft Docs
description: Azure SQL veritabanı, geçerli sorgu performansını iyileştirebilir alanları tanımlamanıza yardımcı olması için performans araçları sağlar.
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: jrasnik, carlrab
manager: digimobile
origin.date: 01/25/2019
ms.date: 02/25/2019
ms.openlocfilehash: adbccd5f9cfd5ddd1912e304f800f3ebe04912c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584837"
---
# <a name="monitor-and-improve-performance"></a>İzleme ve performansı geliştirin

Azure SQL veritabanı, veritabanı olası sorunları tanımlar ve akıllı ayarlama işlemleri ve öneriler sağlayarak İş yükünüzün performansını iyileştirebilir Eylemler önerir.

Veritabanı performansınızı gözden geçirmek için **performans** döşeme genel bakış sayfasında veya gidin "Destek + sorun giderme aşağı" bölümünde:

   ![Performans görünümü](./media/sql-database-performance/entries.png)

İçinde "Destek + sorun giderme" bölümünde sayfalarda kullanabilirsiniz:


1. [Performansa genel bakış](#performance-overview) veritabanınızın performansını izlemeniz gerekir. 
2. [Performans önerileri](#performance-recommendations) İş yükünüzün performansını iyileştirebilir performans önerileri bulunacak.
3. [Sorgu performansı İçgörüleri](#query-performance-insight) üst kaynak kullanan sorgular bulunamadı.
4. [Otomatik ayarlama](#automatic-tuning) Azure SQL veritabanı otomatik olarak veritabanınızı iyileştirin izin vermek için.

## <a name="performance-overview"></a>Performansa genel bakış

Bu görünüm, veritabanınızın performansını özetini sağlar ve performansı ayarlamaya ve sorun gidermeye yardımcı olur. 

![Performans](./media/sql-database-performance/performance.png)

* **Önerileri** kutucuğu önerilerinde veritabanınız için bir dökümünü sağlar (ilk üç önerileri gösterilir varsa daha fazla). Bu kutucuğa tıklamak  **[performans önerileri](#performance-recommendations)**. 
* **Ayarlama etkinliği** kutucuğu, devam eden ve tamamlanmış Eylemler, veritabanınızın ayarlama, size ayarlama etkinliği geçmişi hızlıca görüntülemenizi sağlayan bir özetini sağlar. Bu kutucuğa tıklandığında veritabanınızın tam ayarlama geçmişi görünümü alır.
* **Otomatik ayarlama** döşeme gösterir [otomatik ayarlama yapılandırmasını](sql-database-automatic-tuning-enable.md) (veritabanınızı otomatik olarak uygulanacağını da seçeneklerini ayarlama) veritabanı. Bu kutucuğa tıklayarak Otomasyon yapılandırma iletişim kutusu açılır.
* **Veritabanı sorguları** kutucuğu (kaynak kullanan sorgular genel DTU kullanımını ve üst) veritabanı sorgu performansı özetini gösterir. Bu kutucuğa tıklamak  **[sorgu performansı İçgörüleri](#query-performance-insight)**.

## <a name="performance-recommendations"></a>Performans önerileri

Bu sayfa, akıllı sağlar [önerilerinde](sql-database-advisor.md) veritabanınızın performansını geliştirebilir. Öneriler aşağıdaki türde, bu sayfada gösterilir:

* Hangi dizin oluşturma veya drop ilgili öneriler.
* Veritabanında şema sorunlar belirlendiğinde öneriler sunar.
* Sorguları parametreli sorgularından yararlanabilir öneriler.

![Performans](./media/sql-database-performance/recommendations.png)

Geçmişte uygulanan eylemler ayarlama, tüm geçmişi de bulabilirsiniz.

Performans önerilerini Uygula bulmayı öğrenin [bulun ve performans önerilerini uygulama](sql-database-advisor-portal.md) makalesi.

## <a name="automatic-tuning"></a>Otomatik ayarlama

Azure SQL veritabanları otomatik olarak ayarla veritabanı performansı uygulayarak [performans önerileri](sql-database-advisor.md). Daha fazla bilgi edinmek için [otomatik ayarlama makale](sql-database-automatic-tuning.md). Bunu etkinleştirmek için okuma [otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md).

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri

[Sorgu performansı İçgörüleri](sql-database-query-performance.md) veritabanı performans sorunlarını giderme sağlayarak daha az süre beklemesini sağlar:

* Veritabanları (DTU) kaynak tüketiminizi ayrıntılı Öngörüler. 
* En çok CPU kullanan potansiyel olarak daha iyi performans için ayarlanmış sorguları. 
* Bir sorgunun ayrıntılarına detaya gitme olanağı. 

  ![Performans Panosu](./media/sql-database-query-performance/performance.png)

Bu sayfa hakkında daha fazla bilgi makalesinde Bul  **[sorgu performansı İçgörüleri'ni kullanmayı](sql-database-query-performance.md)**.

## <a name="additional-resources"></a>Ek kaynaklar

* [Tek veritabanları için Azure SQL Database performans rehberi](sql-database-performance-guidance.md)
* [Elastik havuz ne zaman kullanılmalıdır?](sql-database-elastic-pool-guidance.md)

