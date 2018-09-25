---
title: Azure veri Gezgini'ne örnek verileri alma
description: Azure veri Gezgini'ne (yükle) hava durumu ile ilgili örnek verileri alma hakkında bilgi edinin.
services: data-explorer
author: mgblythe
ms.author: mblythe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 7eb0e48a5b66775ac97ed0cab751db0ef367f667
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964624"
---
# <a name="ingest-sample-data-into-azure-data-explorer"></a>Azure veri Gezgini'ne örnek verileri alma

Bu makalede, bir Azure Veri Gezgini veritabanına (yükle) örnek verileri alma işlemini göstermektedir. Vardır [veri almak için çeşitli yollar](ingest-data-overview.md); bu makalede, test amacıyla uygun bir temel yaklaşım odaklanır.

> [!NOTE]
> Tamamlanmışsa, bu veriler zaten [hızlı başlangıç: Azure Veri Gezgini Python kitaplığı kullanarak veri alma](python-ingest-data.md).

## <a name="prerequisites"></a>Önkoşullar

[Bir test kümesi ve veritabanı](create-cluster-database-portal.md)

## <a name="ingest-data"></a>Veriyi çekme

**StormEvents** örnek veri kümesini hava durumu ile ilgili verileri içeren [ortam bilgilerini Ulusal merkezine](https://www.ncdc.noaa.gov/stormevents/).

1. Oturum [ https://dataexplorer.azure.com ](https://dataexplorer.azure.com).

1. Sol üst köşesinde uygulamanın, seçin **Ekle küme**.

1. İçinde **Ekle küme** iletişim kutusunda, küme URL'nizi girin `https://<ClusterName>.<Region>.kusto.windows.net/`, ardından **Ekle**.

1. Aşağıdaki komutta yapıştırın ve seçin **çalıştırma**.

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)

    .ingest into table StormEvents h'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
    ```

1. Alma işlemi tamamlandıktan sonra aşağıdaki sorguyu yapıştırın, sorgu penceresinde seçip **çalıştırma**.

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```
    Sorgu, aşağıdaki sonuçları alınan örnek verileri döndürür.

    ![Sorgu sonuçları](media/ingest-sample-data/query-results.png)

## <a name="next-steps"></a>Sonraki adımlar

[Sorguları yazma](write-queries.md)

[Azure Veri Gezgini veri alımı](ingest-data-overview.md)