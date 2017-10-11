---
title: "Web sitesi günlüğü analizi - Azure Hdınsight Hadoop ile Hive kullanma | Microsoft Docs"
description: "Web sitesi günlüklerini çözümlemek için Hdınsight ile Hive kullanma öğrenin. Bir Hdınsight tabloya giriş olarak bir günlük dosyası kullanmak ve verileri sorgulamak için HiveQL kullanma."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: e1cdb786bb6049980aafc0213abf53013e342618
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a>Web sitesi günlüklerini çözümlemek için Windows tabanlı Hdınsight ile Hive kullanma
Bir Web sitesi günlüklerini çözümlemek için Hdınsight ile HiveQL kullanmayı öğrenin. Web sitesi günlüğü analizi benzer etkinliklere dayalı kitlenizi segmentlere, site ziyaretçilerini demografisine göre kategorilere ayırmak ve bulmak için içeriği teslim bunlar görünümü, Web siteleri geliyor ve benzeri kullanılabilir.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı Hdınsight kümeleri ile çalışır. Hdınsight yalnızca Windows'da Hdınsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Bu örnekte, ziyaretlerin sıklığını Web sitesine dış Web sitelerinden bir gün içinde bir anlayış almak için Web sitesinin günlük dosyalarını çözümlemek için Hdınsight kümesi kullanır. Ayrıca, kullanıcının karşılaştığı Web sitesi hatalarının özetini oluşturacaksınız. Şunları öğreneceksiniz nasıl yapılır:

* Web sitesinin günlük dosyalarını içeren ve Azure Blob Depolama birimine bağlayın.
* HIVE tablolarını Bu günlükleri sorgu oluşturun.
* Verileri çözümlemek için HIVE sorguları oluşturun.
* Hdınsight'a (açık veritabanı bağlantısı (ODBC) çözümlenen verileri almak üzere kullanarak. bağlanmak için Microsoft Excel kullanın

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a>Ön koşullar
* Azure Hdınsight Hadoop kümesinde sağlanması gerekir. Yönergeler için bkz: [sağlama Hdınsight kümeleri][hdinsight-provision].
* Excel 2010 yüklü veya Microsoft Excel 2013 yüklü olmalıdır.
* Bilmeniz gereken [Microsoft Hive ODBC sürücüsü](http://www.microsoft.com/download/details.aspx?id=40886) kovanından Excel'e veri almak için.

## <a name="to-run-the-sample"></a>Örneği çalıştırmak için
1. Gelen [Azure Portal](https://portal.azure.com/), (küme var. sabitlenmiş varsa) Sabitle gelen istediğiniz örneği çalıştırmak küme kutucuğa tıklayın.
2. Küme dikey penceresinden altında **hızlı bağlantılar**, tıklatın **küme Panosu**ve sonra **küme Panosu** dikey penceresinde tıklatın **Hdınsight küme Panosu**. Alternatif olarak, aşağıdaki URL'yi kullanarak Pano doğrudan açabilirsiniz:

         https://<clustername>.azurehdinsight.net

    İstendiğinde, yönetici kullanıcı adını ve küme hazırlama sırasında kullanılan parola kullanarak kimlik doğrulaması.
3. Web açan sayfasından tıklatın **alma başlatıldı galeri** sekmesi ve ardından **örnek verilerle çözümleri** kategorisi, tıklatın **Web sitesi günlüğü analizi** örnek.
4. Örnek tamamlamak için web sayfasında sağlanan yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örnek deneyin: [Hdınsight ile Hive kullanma algılayıcı verilerini çözümleme](hdinsight-hive-analyze-sensor-data.md).

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
