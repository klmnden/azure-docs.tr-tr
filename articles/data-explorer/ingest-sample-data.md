---
title: Azure veri Gezgini'ne örnek verileri alma
description: Azure veri Gezgini'ne (yükle) hava durumu ile ilgili örnek verileri alma hakkında bilgi edinin.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 2ebbe3434f032b38c33ec7b82e445532836f78c9
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758473"
---
# <a name="ingest-sample-data-into-azure-data-explorer"></a>Azure veri Gezgini'ne örnek verileri alma

Bu makalede, bir Azure Veri Gezgini veritabanına (yükle) örnek verileri alma işlemini göstermektedir. Vardır [veri almak için çeşitli yollar](ingest-data-overview.md); bu makalede, test amacıyla uygun bir temel yaklaşım odaklanır.

> [!NOTE]
> Tamamlanmışsa, bu veriler zaten [hızlı başlangıç: Azure Veri Gezgini Python kitaplığı kullanarak veri alma](python-ingest-data.md).

## <a name="prerequisites"></a>Önkoşullar

[Test kümesi ve veritabanı](create-cluster-database-portal.md)

## <a name="ingest-data"></a>Veriyi çekme

**StormEvents** örnek veri kümesi, [Ulusal Çevre Bilgileri Merkezleri](https://www.ncdc.noaa.gov/stormevents/)'nden gelen hava durumu verilerini içerir.

1. [https://dataexplorer.azure.com](https://dataexplorer.azure.com) adresinde oturum açın.

1. Uygulamanın sol üst köşesinden **Küme ekle**'yi seçin.

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

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure veri Gezgini'nde verileri Sorgulama](web-query-data.md)

> [!div class="nextstepaction"]
> [Sorgu yazma](write-queries.md)

> [!div class="nextstepaction"]
> [Azure Veri Gezgini veri alımı](ingest-data-overview.md)