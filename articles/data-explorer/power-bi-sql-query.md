---
title: Power BI'da bir SQL sorgusunu kullanarak Azure veri Gezgini'nde verileri görselleştirin
description: "Bu makalede, Power bı'da verileri görselleştirmek için üç seçenekten birini kullanmayı öğrenin: Azure Veri Gezgini kümesine göre bir SQL sorgusu."
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 13cec67a690239204bfc52d04b7cf03fd571148a
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399264"
---
# <a name="visualize-data-using-the-azure-data-explorer-connector-for-power-bi"></a>Power BI için Azure Veri Gezgini Bağlayıcısı'nı kullanarak verileri Görselleştir

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir veri keşfetme hizmetidir. Power BI, verilerinizi görselleştirmenizi ve sonuçları kuruluşunuzda paylaşmanızı sağlayan bir iş analizi çözümüdür.

Azure Veri Gezgini, Power bı'daki verilere bağlanmak için üç seçenek sunar: yerleşik Bağlayıcısı, Azure veri Gezgini'nde bir sorguyu içeri aktarmak veya bir SQL sorgusu kullanın. Bu makalede veri almak ve bir Power BI raporuna görselleştirme için bir SQL sorgusu kullanmayı gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede tamamlamak için şunlara ihtiyacınız vardır:

* Bağlanabilir, böylece Azure Active directory üyesi olan bir kuruluş e-posta hesabı [Azure Veri Gezgini Yardım kümesi](https://dataexplorer.azure.com/clusters/help/databases/samples).

* [Power BI Desktop](https://powerbi.microsoft.com/get-started/) (seçin **DOWNLOAD FREE**)

## <a name="get-data-from-azure-data-explorer"></a>Azure veri Gezgini'nde verileri alma

İlk olarak, Azure Veri Gezgini Yardım kümeye bağlanın ve ardından bir veri alt kümelerinin getirin *StormEvents* tablo. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

Azure Veri Gezgini ile genellikle yerel sorgu dili kullanır, ancak burada kullanacağınız SQL sorguları da destekler. Azure Veri Gezgini SQL sorgusu bir yerel sorguya sizin için çevirir.

1. Power BI Desktop'ta üzerinde **giriş** sekmesinde **Veri Al** ardından **daha fazla**.

    ![Verileri alma](media/power-bi-sql-query/get-data-more.png)

1. Arama *Azure SQL veritabanı*seçin **Azure SQL veritabanı** ardından **Connect**.

    ![Arama ve veri alma](media/power-bi-sql-query/search-get-data.png)

1. Üzerinde **SQL Server veritabanı** ekranında, formunu aşağıdaki bilgilerle doldurun.

    ![Veritabanı, tablo, sorgu seçenekleri](media/power-bi-sql-query/database-table-query.png)

    **Ayar** | **Değer** | **Alan açıklaması**
    |---|---|---|
    | Sunucusu | *help.kusto.windows.net* | Yardım kümesi URL'sini (olmadan *https://* ). Diğer kümeler için URL biçimindedir  *\<ClusterName\>.\< Bölge\>. kusto.windows.net*. |
    | Database | *Örnekler* | Bağlanmakta olduğunuz kümesi üzerinde barındırılan örnek veritabanı. |
    | Veri bağlantısı modu | *İçeri Aktar* | Power BI veri aldığında veya doğrudan veri kaynağına bağlanan belirler. Bu bağlayıcıyı kullanarak, iki seçenekten birini kullanabilirsiniz. |
    | Komut zaman aşımı | Boş bırakın | Bir zaman aşımı hatası fırlatmadan önce ne kadar sorgusu çalıştırır. |
    | SQL deyimi | Bu tablonun altındaki sorguyu Kopyala | Azure Veri Gezgini, bir yerel sorguya çevirir SQL deyimi. |
    | Diğer Seçenekler | Varsayılan değer olarak bırakın. | Seçenekler, Azure Veri Gezgini kümeleri için geçerli değildir. |
    | | | |

    ```SQL
    SELECT TOP 1000 *
    FROM StormEvents
    ORDER BY DamageCrops DESC
    ```

1. Yardım kümeyle bir bağlantı zaten yoksa, oturum açın. Bir Microsoft hesabıyla oturum açın ve ardından **Connect**.

    ![Oturum aç](media/power-bi-sql-query/sign-in.png)

1. Üzerinde **help.kusto.windows.net: Örnekleri** ekranındayken **yük**.

    ![Veri yükleme](media/power-bi-sql-query/load-data.png)

    Tablo, örnek verilere dayalı raporlar oluşturabileceğiniz ana penceresinde Power BI, rapor görünümünde açılır.

## <a name="visualize-data-in-a-report"></a>Bir rapordaki verileri görselleştirin

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, oluşturulan rapor artık ihtiyacınız kalmadığında Power BI Desktop (.pbix) dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar

[Power BI hizmetinde içeri aktarılan bir sorgu kullanarak verileri Görselleştir](power-bi-connector.md)