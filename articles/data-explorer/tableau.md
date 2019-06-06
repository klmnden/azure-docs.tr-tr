---
title: Tableau ile verileri görselleştirme için açık veritabanı bağlantısı (ODBC) bağlantı Azure Veri Gezgini'ni kullanın
description: Bu makalede, Azure Veri Gezgini bağlantı için bir açık veritabanı bağlantısı (ODBC) bağlantısı Tableau ile verileri görselleştirme için nasıl kullanılacağını öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: a0948ae35a5c23768df117979db819861ac64529
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499092"
---
# <a name="visualize-data-from-azure-data-explorer-in-tableau"></a>Tableau Azure veri Gezgini'nde verileri görselleştirin

 [Tableau](https://www.tableau.com/) iş zekası görsel analiz platformudur. Tableau Azure veri Gezgini'ne bağlanmak ve örnek kümeden verileri getirmek için SQL Server açık veritabanı bağlantısı (ODBC) sürücüsü kullanın. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede tamamlamak için şunlara ihtiyacınız vardır:

* [ODBC ile Azure Veri Gezgini bağlanma](connect-odbc.md) Tableau Azure veri Gezgini'ne bağlanmak için SQL Server ODBC sürücüsünü kullanarak. 

* Tam, tableau Masaüstü veya [deneme](https://www.tableau.com/products/desktop/download) sürümü.

* StormEvents örnek veriler içeren bir kümesi. Daha fazla bilgi için [bir Azure Veri Gezgini kümesi ile veritabanı oluşturma](create-cluster-database-portal.md) ve [örnek verileri Azure veri Gezgini'ne alma](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="visualize-data-in-tableau"></a>Tableau verileri görselleştirin 

ODBC yapılandırma işlemini bitirdiğinizde, örnek verileri Tableau getirebilirsiniz.

1. Tableau Desktop'ta sol menüde **diğer veritabanları (ODBC)** .

    ![ODBC ile bağlanma](media/tableau/connect-odbc.png)

1. İçin **DSN**, ODBC için oluşturduğunuz veri kaynağını seçin ve ardından seçin **oturum**.

    ![ODBC oturum açma](media/tableau/odbc-sign-in.png)

1. İçin **veritabanı**, kümenizdeki örnek veritabanı gibi seçin *TestDatabase*. İçin **şema**seçin *dbo*ve **tablo**seçin *StormEvents* örnek tablo.

    ![Veritabanı ve Tablo Seç](media/tableau/select-database-table.png)

1. Tableau, artık şema örnek veriler için de gösterir. Seçin **Şimdi Güncelleştir** Tableau verileri getirmek için.

    ![Verileri güncelleştirme](media/tableau/update-data.png)

    Veriler içeri aktarıldığında, Tableau veri satırı aşağıdaki görüntüye benzer gösterir.

    ![Sonuç kümesi](media/tableau/result-set.png)

1. Şimdi, Azure veri Gezgini'nde duruma verileri temel alan Tableau görselleştirmeler oluşturabilirsiniz. Daha fazla bilgi için [Tableau öğrenme](https://www.tableau.com/learn).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini için sorgu yazma](write-queries.md)