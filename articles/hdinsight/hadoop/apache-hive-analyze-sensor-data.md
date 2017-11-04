---
title: "Hive ve Hadoop - Azure Hdınsight kullanarak algılayıcı verilerini çözümleme | Microsoft Docs"
description: "Hive sorgusu konsol Hdınsight (Hadoop) kullanarak algılayıcı verilerini çözümlemeyi öğrenin ve ardından PowerView ile Microsoft Excel verilerini görselleştirin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a8ac160c-1cef-45d9-bf36-7beb5a439105
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: d4d216cd1d8de7c86f71d0181270607e12f761cf
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Hdınsight'ta Hadoop Hive sorgusu Konsolu kullanarak algılayıcı verilerini çözümleme

Hive sorgusu konsol Hdınsight (Hadoop) kullanarak algılayıcı verilerini çözümlemeyi öğrenin ve Power View'ı kullanarak Microsoft Excel verilerini görselleştirin.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı Hdınsight kümeleri ile çalışır. Hdınsight yalnızca Windows'da Hdınsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).


Bu örnekte, Hive geçmiş verileri işlemek ve ısıtma sistemleriyle sorunlarını belirlemek için kullanın. Özellikle, sistemleri belirlemek aşağıdaki görevleri gerçekleştirerek ayarlı sıcaklığı güvenilir bir şekilde korumak mümkün değildir:

* HIVE tablolarını virgülle ayrılmış değer (CSV) dosyasında depolanan verileri sorgulayamadı oluşturun.
* Verileri çözümlemek için HIVE sorguları oluşturun.
* Çözümlenen verileri almak üzere Hdınsight'a bağlanmak için Microsoft Excel kullanın.
* Verileri görselleştirmek için Power View'ı kullanın.

![Çözüm mimarisi diyagramı](./media/apache-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Ön koşullar

* Hdınsight (Hadoop) kümesi: bkz [Hdınsight'ta oluşturmak Hadoop kümeleri](../hdinsight-hadoop-provision-linux-clusters.md) bir küme oluşturma hakkında daha fazla bilgi için.
* Microsoft Excel 2013

  > [!NOTE]
  > Microsoft Excel ile verileri görselleştirme için kullanıldığından [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft Hive ODBC sürücüsü](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a>Örneği çalıştırmak için

1. Web tarayıcınızdan aşağıdaki URL'sine gidin: 

         https://<clustername>.azurehdinsight.net

    `<clustername>` değerini HDInsight kümenizin adıyla değiştirin.

    İstendiğinde, yönetici kullanıcı adını ve bu küme hazırlama sırasında kullanılan parola kullanarak kimlik doğrulaması.

2. Web açan sayfasından tıklatın **alma başlatıldı galeri** sekmesi ve ardından **örnek verilerle çözümleri** kategorisi, tıklatın **algılayıcı verilerini çözümleme** örnek.

    ![Başlatılan galeri görüntüsü alma](./media/apache-hive-analyze-sensor-data/getting-started-gallery.png)

3. Örnek tamamlamak için web sayfasında sağlanan yönergeleri izleyin.
