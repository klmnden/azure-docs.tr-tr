---
title: Visual Studio için Azure Data Lake Araçları’nı yükleme
description: Bu makalede, Visual Studio için Azure Data Lake araçları yüklemek açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 05/03/2018
ms.openlocfilehash: 3269d38b691ec4dd9573a241c89dadc285787143
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60408160"
---
# <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake Araçları’nı yükleme.

Azure Data Lake Analytics hesapları oluşturmak, [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde işler tanımlamak ve Data Lake Analytics hizmetine iş göndermek için Visual Studio’nun nasıl kullanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* **Visual Studio**: Express dışında tüm sürümler desteklenir.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **.NET için Microsoft Azure SDK** 2.7.1 sürümü veya sonraki sürümleri.  [Web platformu yükleyicisini](https://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
* **Data Lake Analytics** hesabı. Hesap oluşturmak için bkz. [Azure portalı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio-2017"></a>Visual Studio 2017 için Azure Data Lake Araçları’nı yükleme

Visual Studio için Azure Data Lake Araçları, Visual Studio 2017 15.3 veya üzeri sürümlerde desteklenir. Araç, Visual Studio Yükleyicisi’ndeki **Veri depolama ve işleme** ve **Azure Geliştirme** iş yüklerinin parçasıdır. Visual Studio yüklemenizin parçası olarak bu iki iş yükünden birini etkinleştirin.  

Etkinleştirme **veri depolama ve işleme** gösterildiği gibi iş yükü: ![Veri depolama ve işleme iş yükünü etkinleştirin](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-01.png)

Etkinleştirme **Azure geliştirme** gösterildiği gibi iş yükü: ![Azure geliştirme iş yükünü etkinleştirin](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-02.png)

## <a name="install-azure-data-lake-tools-for-visual-studio-2013-and-2015"></a>Visual Studio 2013 ve 2015 için Azure Data Lake Araçları’nı yükleme

[İndirme Merkezi'nden](https://aka.ms/adltoolsvs) Visual Studio için Azure Data Lake Araçları’nı indirip yükleyin. Yükleme işleminden sonra şunları kontrol edin:
* **Sunucu Gezgini** > **Azure** düğümü, **Data Lake Analytics** düğümü içerir. 
* **Araçlar** menüsünde **Data Lake** öğesi vardır.


## <a name="next-steps"></a>Sonraki Adımlar
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* Köşe yürütme görünümünü kullanma hakkında bilgi için bkz: [Visual Studio için Data Lake araçlarına köşe yürütme görünümünü kullanma](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)
