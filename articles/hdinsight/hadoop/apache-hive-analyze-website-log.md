---
title: Web sitesi günlüğü çözümlemesi - Azure HDInsight Hadoop ile Hive kullanma
description: Web sitesi günlüklerini çözümlemek için HDInsight ile Hive kullanma konusunda bilgi edinin. Bir HDInsight tablosuna giriş olarak bir günlük dosyası kullanın ve verileri sorgulamak için HiveQL kullanın.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/17/2016
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: a40aef8d0231fcfc0ae0f399440b1fb98367dd2d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595724"
---
# <a name="use-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a>Web sitesi günlüklerini çözümlemek için Windows tabanlı HDInsight ile Hive kullanma
Bir Web sitesi günlüklerini çözümlemek için HDInsight ile HiveQL kullanmayı öğrenin. Web sitesi günlüğü çözümlemesi benzer etkinliklere dayalı hedef kitlenizi segmentlere ayırın, site ziyaretçilerinin demografik bilgilere göre kategorilere ayırın ve bulmak için içeriği teslim bunlar görünümü, Web siteleri geliyorlar ve benzeri kullanılabilir.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı HDInsight kümeleri ile çalışır. HDInsight yalnızca Windows üzerinde HDInsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

Bu örnekte, bir HDInsight kümesi ziyaretlerin sıklığına ilişkin Web sitesi için dış Web sitelerinden bir gün içinde öngörü almak için Web sitesi günlük dosyalarını analiz etmek için kullanın. Ayrıca, kullanıcıların deneyimini Web sitesi hatalarının bir özetini de oluşturur. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

* Web sitesi günlük dosyalarını içeren bir Azure Blob depolama alanına bağlayın.
* Bu günlükleri sorgulamak için HIVE tabloları oluşturun.
* Verileri çözümlemek için HIVE sorguları oluşturun.
* Microsoft Excel için HDInsight (açık veritabanı bağlantısı (ODBC) analiz edilen verileri almak üzere kullanarak tarafından. bağlanmak için kullanın

![HDI. Samples.Website.Log.Analysis](./media/apache-hive-analyze-website-log/hdinsight-weblogs-sample.png)

## <a name="prerequisites"></a>Önkoşullar
* Azure HDInsight Hadoop kümesinde sağlanması gerekir. Yönergeler için [HDInsight küme sağlama](../hdinsight-hadoop-provision-linux-clusters.md).
* Excel 2010 yüklü veya Microsoft Excel 2013 yüklü olmalıdır.
* Olmalıdır [Microsoft Hive ODBC sürücüsünü](http://www.microsoft.com/download/details.aspx?id=40886) kovanından Excel'e veri aktarma için.

## <a name="to-run-the-sample"></a>Örneği çalıştırmak için
1. Gelen [Azure portalında](https://portal.azure.com/), ilerlemeyi (küme var. sabitlenmiş değilse), gelen örneği çalıştırmak istediğiniz küme kutucuğuna tıklayın.
2. Küme dikey penceresinden altında **hızlı bağlantılar**, tıklayın **küme Panosu**ve sonra **küme Panosu** dikey penceresinde tıklayın **HDInsight kümesi Pano**. Alternatif olarak, aşağıdaki URL'yi kullanarak Pano doğrudan açabilirsiniz:

         https://<clustername>.azurehdinsight.net

    İstendiğinde, yönetici kullanıcı adı ve parola kümesi sağlanırken kullanılan kullanarak kimlik doğrulaması.
3. Web açılan sayfasından tıklatın **alma başlatıldı galeri** sekmesinde altındaki **çözümleri örnek verilerle** kategorisi, tıklayın **Web sitesi günlüğü çözümlemesi** örnek.
4. Örneği tamamlamak için web sayfasında sağlanan yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örnek deneyin: [HDInsight ile Hive'ı kullanarak sensör verilerini çözümleme](apache-hive-analyze-sensor-data.md).

[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md
