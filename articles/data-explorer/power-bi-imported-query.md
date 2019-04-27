---
title: "Hızlı Başlangıç: İçeri aktarılan bir sorgu Power BI ile Azure veri Gezgini'nde verileri Görselleştirme "
description: "Bu hızlı başlangıçta, Power bı'da verileri görselleştirmek için üç seçenekten birini kullanmayı öğrenin: Azure veri Gezgini'nde bir sorgu alma."
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 11/14/2018
ms.openlocfilehash: d14de1f25cc432cb2a9fed2149bd0870aa3ce16a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60828996"
---
# <a name="quickstart-visualize-data-using-a-query-imported-into-power-bi"></a>Hızlı Başlangıç: Power BI'a aktarılan bir sorgu kullanarak verileri Görselleştir

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir veri keşfetme hizmetidir. Power BI, verilerinizi görselleştirmenizi ve sonuçları kuruluşunuzda paylaşmanızı sağlayan bir iş analizi çözümüdür.

Azure Veri Gezgini, Power bı'daki verilere bağlanmak için üç seçenek sunar: yerleşik Bağlayıcısı, Azure veri Gezgini'nde bir sorguyu içeri aktarmak veya bir SQL sorgusu kullanın. Bu hızlı başlangıçta bir sorguyu her içeri veri almak ve bir Power BI raporuna görselleştirme gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlara ihtiyacınız vardır:

* Bağlanabilir, böylece Azure Active directory üyesi olan bir kuruluş e-posta hesabı [Azure Veri Gezgini Yardım kümesi](https://dataexplorer.azure.com/clusters/help/databases/samples).

* [Power BI Desktop](https://powerbi.microsoft.com/get-started/) (seçin **DOWNLOAD FREE**)

* [Azure Veri Gezgini masaüstü uygulaması](/azure/kusto/tools/kusto-explorer)

## <a name="get-data-from-azure-data-explorer"></a>Azure veri Gezgini'nde verileri alma

İlk olarak, Azure Veri Gezgini masaüstü uygulamasında bir sorgu oluşturun ve Power BI kullanmak için dışarı aktarın. Ardından, Azure Veri Gezgini Yardım kümeye bağlanın ve verilerin bir alt getirin *StormEvents* tablo. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

1. Bir tarayıcıda Git [ https://help.kusto.windows.net/ ](https://help.kusto.windows.net/) Azure Veri Gezgini Masaüstü uygulamasını başlatmak için.

1. İçinde bir masaüstü uygulaması, aşağıdaki sorguyu çalıştırın sağ sorgu penceresine kopyalayın.

    ```Kusto
    StormEvents
    | sort by DamageCrops desc
    | take 1000
    ```

    Sonuç kümesi ilk birkaç satırı aşağıdaki görüntüye benzer olmalıdır.

    ![Sorgu sonuçları](media/power-bi-imported-query/query-results.png)

1. Üzerinde **Araçları** sekmesinde **sorgulamak için Power BI** ardından **Tamam**.

    ![Sorgu dışarı aktarma](media/power-bi-imported-query/export-query.png)

1. Power BI Desktop'ta üzerinde **giriş** sekmesinde **Veri Al** ardından **boş sorgu**.

    ![Verileri alma](media/power-bi-imported-query/get-data.png)

1. Güç sorgu Düzenleyicisi'nde, üzerinde **giriş** sekmesinde **Gelişmiş Düzenleyici**.

1. İçinde **Gelişmiş Düzenleyici** penceresi, dışarı aktardığınız sorgu seçip yapıştırma **Bitti**.

    ![Yapıştırma sorgu](media/power-bi-imported-query/paste-query.png)

1. Ana Power Query Düzenleyicisi penceresinde, seçin **kimlik bilgilerini Düzenle**. Seçin **kuruluş hesabı**, oturum açın ve ardından seçin **Connect**.

    ![Kimlik bilgilerini Düzenle](media/power-bi-imported-query/edit-credentials.png)

1. Üzerinde **giriş** sekmesinde **Kapat & Uygula**.

    ![Kapat ve uygula](media/power-bi-imported-query/close-apply.png)

## <a name="visualize-data-in-a-report"></a>Bir rapordaki verileri görselleştirin

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu Hızlı Başlangıç için oluşturduğunuz rapor artık ihtiyacınız kalmadığında Power BI Desktop (.pbix) dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Power BI hizmetinde içeri aktarılan bir sorgu kullanarak verileri Görselleştir](power-bi-sql-query.md)