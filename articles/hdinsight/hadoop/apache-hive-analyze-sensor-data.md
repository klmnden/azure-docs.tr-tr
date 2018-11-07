---
title: Hive ve Hadoop - Azure HDInsight'ı kullanarak sensör verilerini çözümleme
description: Hive sorgu Konsolu kullanarak HDInsight (Hadoop) ile algılayıcı verilerini çözümlemeyi öğrenin ve ardından verileri Microsoft Excel'de PowerView ile görselleştirin.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 04/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: dfc4d930f185c36c3ba0c869494ba0e7dee64cac
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249413"
---
# <a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>HDInsight Hadoop Hive sorgu Konsolu kullanarak sensör verilerini çözümleme

Hive sorgu Konsolu kullanarak HDInsight (Hadoop) ile algılayıcı verilerini çözümlemeyi öğrenin ve ardından Power View'ı kullanarak verileri Microsoft Excel'de görselleştirin.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı HDInsight kümeleri ile çalışır. HDInsight yalnızca Windows üzerinde HDInsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).


Bu örnekte, Hive geçmiş verilerini işlemek ve ısıtma ve havalandırma sistemlerle sorunlarını belirlemek için kullanın. Özellikle, sistemleri belirlemek aşağıdaki görevleri gerçekleştirerek ayarlanan bir sıcaklığı güvenilir bir şekilde korumak mümkün değildir:

* HIVE tablolarını virgülle ayrılmış değer (CSV) dosyasına depolanmış veri oluşturun.
* Verileri çözümlemek için HIVE sorguları oluşturun.
* Analiz edilen verileri almak üzere Microsoft Excel için HDInsight bağlanmak için kullanın.
* Verileri görselleştirmek için Power View'ı kullanın.

![Çözüm mimarisi diyagramı](./media/apache-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Önkoşullar

* Bir HDInsight (Hadoop) kümesi: bkz [Hadoop kümeleri oluşturma HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) bir küme oluşturma hakkında daha fazla bilgi için.
* Microsoft Excel 2013

  > [!NOTE]
  > Microsoft Excel ile veri görselleştirme için kullanılan [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft Hive ODBC sürücüsü](https://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a>Örneği çalıştırmak için

1. Web tarayıcınızdan şu URL'ye gidin: 

         https://<clustername>.azurehdinsight.net

    `<clustername>` değerini HDInsight kümenizin adıyla değiştirin.

    İstendiğinde, yönetici kullanıcı adını ve bu küme sağlanırken kullanılan parola kullanarak kimlik doğrulaması.

2. Web açılan sayfasından tıklatın **alma başlatıldı galeri** sekmesinde altındaki **çözümleri örnek verilerle** kategorisi, tıklayın **algılayıcısı veri analizi** örnek.

    ![Başlatılan galeri görüntüsü alma](./media/apache-hive-analyze-sensor-data/getting-started-gallery.png)

3. Örneği tamamlamak için web sayfasında sağlanan yönergeleri izleyin.
