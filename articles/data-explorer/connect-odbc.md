---
title: Azure Veri Gezgini ODBC ile bağlanma
description: Bu nasıl yapılır makalesinde bir Azure Veri Gezgini ODBC bağlantı kurmanız sonra Tableau ile verileri görselleştirme için bu bağlantıyı kullanmak hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/21/2019
ms.openlocfilehash: d01c825e50e30e3545a0d47e432835c658d677af
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59043890"
---
# <a name="connect-to-azure-data-explorer-with-odbc"></a>Azure Veri Gezgini ODBC ile bağlanma

Açık veritabanı bağlantısı ([ODBC](/sql/odbc/reference/odbc-overview)) veritabanı erişimi için bir yaygın olarak kabul edilen uygulama programlama arabirimi (API). Özel bir bağlayıcı olmayan uygulamalardan Azure veri Gezgini'ne bağlanmak için ODBC kullanma.

Arka planda uygulamaları adlı veritabanı özel modüller uygulanan işlevleri ODBC arabiriminde çağırma *sürücüleri*. Azure Veri Gezgini, SQL Server iletişim protokolü kümesini destekler ([MS TDS](/azure/kusto/api/tds/)); bu nedenle, SQL Server için ODBC sürücüsü kullanabilirsiniz.

Bu makalede, Azure veri Gezgini'ne ODBC destekleyen herhangi bir uygulamadan bağlanabilmesi için SQL Server ODBC sürücüsü kullanmayı öğrenin. Sonra isteğe bağlı olarak Tableau Azure veri Gezgini'ne bağlanmak ve bir örnek kümeden verilerinizi getirin.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl tamamlamak için şunlara ihtiyacınız vardır:

* [SQL Server sürüm 17.2.0.1 için Microsoft ODBC sürücüsü veya üzeri](/sql/connect/odbc/download-odbc-driver-for-sql-server) işletim sisteminiz için.

* Tableau Örneğimizdeki takip etmek istiyorsanız, ayrıca gerekir:

  * Tableau Masaüstü, tam veya [deneme](https://www.tableau.com/products/desktop/download) sürümü.

  * StormEvents örnek veriler içeren bir kümesi. Daha fazla bilgi için [hızlı başlangıç: Bir Azure Veri Gezgini kümesi ile veritabanı oluşturma](create-cluster-database-portal.md) ve [örnek verileri Azure veri Gezgini'ne alma](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="configure-the-odbc-data-source"></a>ODBC veri kaynağını yapılandırma

SQL Server için ODBC sürücüsünü kullanarak bir ODBC veri kaynağını yapılandırmak için aşağıdaki adımları izleyin.

1. Windows Arama *ODBC veri kaynakları*, ODBC veri kaynakları Masaüstü uygulamasını açın.

1. **Add (Ekle)** seçeneğini belirleyin.

    ![Veri kaynağı ekleme](media/connect-odbc/add-data-source.png)

1. Seçin **SQL Server için ODBC sürücüsü 17** ardından **son**.

    ![Sürücü seçin](media/connect-odbc/select-driver.png)

1. Bağlantı ve bağlanmak istediğiniz kümeyi seçin, ardından bir ad ve açıklama girin **sonraki**. URL biçiminde olmalıdır küme  *\<ClusterName\>.\< Bölge\>. kusto.windows.net*.

    ![Sunucu seçin](media/connect-odbc/select-server.png)

1. Seçin **Active Directory tümleşik** ardından **sonraki**.

    ![Active Directory Tümleşik](media/connect-odbc/active-directory-integrated.png)

1. Örnek verilerle veritabanı seçip **sonraki**.

    ![Varsayılan veritabanını Değiştir](media/connect-odbc/change-default-database.png)

1. Sonraki ekranda tüm seçenekleri seçip varsayılan olarak bırakın **son**.

1. Seçin **Test veri kaynağı**.

    ![Test verisi kaynağı](media/connect-odbc/test-data-source.png)

1. Test seçip başarılı olduğunu doğrulamak **Tamam**. Test başarılı olmadıysa, önceki adımda belirttiğiniz değerleri kontrol edip kümeye bağlanmak için yeterli izinlere sahip olduğunuzdan emin olun.

    ![Test başarılı oldu](media/connect-odbc/test-succeeded.png)

## <a name="visualize-data-in-tableau-optional"></a>(İsteğe bağlı) Tableau verileri görselleştirin

ODBC yapılandırma bitirdikten sonra örnek verileri Tableau getirebilirsiniz.

1. Tableau Desktop'ta sol menüde **diğer veritabanları (ODBC)**.

    ![ODBC ile bağlanma](media/connect-odbc/connect-odbc.png)

1. İçin **DSN**, ODBC için oluşturduğunuz veri kaynağını seçin ve ardından seçin **oturum**.

    ![ODBC oturum açma](media/connect-odbc/odbc-sign-in.png)

1. İçin **veritabanı**, kümenizdeki örnek veritabanı gibi seçin *TestDatabase*. İçin **şema**seçin *dbo*ve **tablo**seçin *StormEvents* örnek tablo.

    ![Veritabanı ve Tablo Seç](media/connect-odbc/select-database-table.png)

1. Tableau, artık şema örnek veriler için de gösterir. Seçin **Şimdi Güncelleştir** Tableau verileri getirmek için.

    ![Verileri güncelleştirme](media/connect-odbc/update-data.png)

    Veriler içeri aktarıldığında, Tableau veri satırı aşağıdaki görüntüye benzer gösterir.

    ![Sonuç kümesi](media/connect-odbc/result-set.png)

1. Şimdi, Azure veri Gezgini'nde duruma verileri temel alan Tableau görselleştirmeler oluşturabilirsiniz. Daha fazla bilgi için [Tableau öğrenme](https://www.tableau.com/learn).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Veri Gezgini için sorgu yazma](write-queries.md)

[Öğretici: Azure Power BI veri Gezgini'nde verileri görselleştirin](visualize-power-bi.md)