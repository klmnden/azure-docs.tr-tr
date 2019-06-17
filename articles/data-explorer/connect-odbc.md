---
title: Azure Veri Gezgini ODBC ile bağlanma
description: Bu makalede, Azure Veri Gezgini bir açık veritabanı bağlantısı (ODBC) bağlantı kurmanız öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 02ae9673f1dc402ee1500b466d7e259263ef3262
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66494840"
---
# <a name="connect-to-azure-data-explorer-with-odbc"></a>Azure Veri Gezgini ODBC ile bağlanma

Açık veritabanı bağlantısı ([ODBC](/sql/odbc/reference/odbc-overview)) veritabanı erişimi için bir yaygın olarak kabul edilen uygulama programlama arabirimi (API). Özel bir bağlayıcı olmayan uygulamalardan Azure veri Gezgini'ne bağlanmak için ODBC kullanma.

Arka planda uygulamaları adlı veritabanı özel modüller uygulanan işlevleri ODBC arabiriminde çağırma *sürücüleri*. Azure Veri Gezgini, SQL Server iletişim protokolü kümesini destekler ([MS TDS](/azure/kusto/api/tds/)), SQL Server için ODBC sürücüsü kullanabilirsiniz.

Bu makalede, Azure veri Gezgini'ne ODBC destekleyen herhangi bir uygulamadan bağlanabilmesi için SQL Server ODBC sürücüsü kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdakiler gerekir:

* [SQL Server sürüm 17.2.0.1 için Microsoft ODBC sürücüsü veya üzeri](/sql/connect/odbc/download-odbc-driver-for-sql-server) işletim sisteminiz için.

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

    ![Active Directory ile tümleşik](media/connect-odbc/active-directory-integrated.png)

1. Örnek verilerle veritabanı seçip **sonraki**.

    ![Varsayılan veritabanını Değiştir](media/connect-odbc/change-default-database.png)

1. Sonraki ekranda tüm seçenekleri seçip varsayılan olarak bırakın **son**.

1. Seçin **Test veri kaynağı**.

    ![Test verisi kaynağı](media/connect-odbc/test-data-source.png)

1. Test seçip başarılı olduğunu doğrulamak **Tamam**. Test başarılı olmadıysa, önceki adımda belirttiğiniz değerleri kontrol edip kümeye bağlanmak için yeterli izinlere sahip olduğunuzdan emin olun.

    ![Test başarılı oldu](media/connect-odbc/test-succeeded.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Tableau Azure veri Gezgini'ne bağlanma](tableau.md)