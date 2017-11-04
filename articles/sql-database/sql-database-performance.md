---
title: "İzleme ve performansı - Azure SQL veritabanı | Microsoft Docs"
description: "Azure SQL veritabanı geçerli sorgu performansını iyileştirebilir alanları tanımlamanıza yardımcı olması için performans araçları sağlar."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: a60b75ac-cf27-4d73-8322-ee4d4c448aa2
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 07/19/2016
ms.author: sstein
ms.openlocfilehash: 49b24619372d41ff25f815ca493ca7e6ce69ef9c
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="monitor-and-improve-performance"></a>İzleme ve performansı
Azure SQL veritabanı veritabanınızdaki olası sorunlar tanımlanmakta ve akıllı ayarlama Eylemler ve öneriler sağlayarak İş yükünüzün performansını geliştirebilirsiniz Eylemler önerir.

Veritabanı performansınızı gözden geçirmek için kullanın **performans** döşeme genel bakış sayfasında veya gidin "Destek + sorun giderme için" bölümüne:

   ![Performans görünümü](./media/sql-database-performance/entries.png)

İçindeki "Destek + sorun giderme" bölümüne, şu sayfaları kullanabilirsiniz:


1. [Performans genel bakış](#performance-overview) veritabanınızın performansını izlemek için. 
2. [Performans önerileri](#performance-recommendations) İş yükünüzün performansını artırabilir performans önerileri bulunamıyor.
3. [Sorgu performansı öngörüleri](#query-performance-insight) kaynak tüketen sorguları üst kaynak bulunamıyor.
4. [Otomatik ayarlama](#automatic-tuning) Azure SQL veritabanı otomatik olarak veritabanınızı en iyi duruma izin vermek için.

## <a name="performance-overview"></a>Performans genel bakış
Bu görünüm, veritabanınızın performansını özetini sağlar ve ve sorun gidermeyi performansla yardımcı olur. 

![Performans](./media/sql-database-performance/performance.png)

* **Önerileri** kutucuğu veritabanınız için öneriler ayarlama bir dökümünü sağlar (ilk üç önerileri gösterilen olup olmadığını daha fazla). Bu kutucuğa tıklamak, götürür size  **[performans önerileri](#performance-recommendations)**. 
* **Etkinlik ayarlama** kutucuğu devam eden ve veritabanınız için Eylemler ayarlama, etkinlik ayarlama geçmişini hızlı bir görünüme vererek tamamlanan bir özetini sağlar. Bu kutucuğa tıklamak veritabanınızın tam ayarlama geçmişini görüntülemek için götürür.
* **Otomatik ayarı** döşeme gösterir [otomatik ayarı yapılandırma](sql-database-automatic-tuning-enable.md) (veritabanınızı otomatik olarak uygulanan seçeneklerini ayarlama) veritabanı. Bu kutucuğa tıklayarak Otomasyon yapılandırması iletişim kutusunu açar.
* **Veritabanı sorguları** döşeme veritabanınız (kaynak tüketen sorguları genel DTU kullanımı ve üst kaynak) için sorgu performansı özetini gösterir. Bu kutucuğa tıklamak, götürür size  **[sorgu performansı öngörüleri](#query-performance-insight)**.

## <a name="performance-recommendations"></a>Performans önerileri
Bu sayfa akıllı sağlar [önerileri ayarlama](sql-database-advisor.md) veritabanınızın performansı geliştirebilir. Bu sayfada aşağıdaki önerileri türleri gösterilmektedir:

* Öneri oluşturmak veya bırakma için hangi dizinlerin.
* Şema sorunları veritabanında belirlendiğinde öneriler sunar.
* Sorguları parametreli sorgularından yararlanabilir kullanmayla ilgili öneriler.

![Performans](./media/sql-database-performance/recommendations.png)

Geçmişte uygulanan eylemler ayarlama, tam geçmişini de bulabilirsiniz.

Apply performans önerileri bulmayı öğrenin [bulmak ve performans önerileri uygulamak](sql-database-advisor-portal.md) makalesi.

## <a name="automatic-tuning"></a>Otomatik ayarlama
Azure SQL veritabanları otomatik olarak ayarlamak veritabanı performansını uygulayarak [performans önerileri](sql-database-advisor.md). Daha fazla bilgi edinmek için okuma [otomatik ayarlama makale](sql-database-automatic-tuning.md). Etkinleştirmek için okuma [otomatik ayarlamayı etkinleştirmek nasıl](sql-database-automatic-tuning-enable.md).

## <a name="query-performance-insight"></a>Sorgu Performansı Öngörüleri
[Sorgu performansı öngörüleri](sql-database-query-performance.md) sağlayarak veritabanı performans sorunlarını giderme daha az süre beklemesini sağlar:

* Veritabanları (DTU) kaynak tüketimi daha derin bir anlayış. 
* Potansiyel olarak performansı için ayarlanmış sorguları tüketen üst CPU. 
* Bir sorgu ayrıntıları detaya yeteneği. 

  ![Performans Panosu](./media/sql-database-query-performance/performance.png)

Bu sayfa hakkında daha fazla bilgi makalesinde Bul  **[sorgu performansı öngörüleri kullanmayı](sql-database-query-performance.md)**.

## <a name="additional-resources"></a>Ek kaynaklar
* [Tek veritabanları için Azure SQL Database performans rehberi](sql-database-performance-guidance.md)
* [Bir esnek havuz ne zaman kullanılmalı?](sql-database-elastic-pool-guidance.md)

