---
title: Azure HDInsight GÇ Cache (Önizleme) kullanarak Apache Spark iş yüklerinin performansı
description: Azure HDInsight GÇ önbellek ve Apache Spark'ın performansını geliştirmek için kullanma hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 10/15/2018
ms.openlocfilehash: b77e7e9d5a68439e7f336ecb26e91031d80a7606
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64695215"
---
# <a name="improve-performance-of-apache-spark-workloads-using-azure-hdinsight-io-cache-preview"></a>Azure HDInsight GÇ Cache (Önizleme) kullanarak Apache Spark iş yüklerinin performansı

GÇ, verileri önbelleğe alma hizmeti için Azure HDInsight, Apache Spark işlerinin performansı artıran önbelleğidir. GÇ önbellek ile de çalışır [Apache TEZ](https://tez.apache.org/) ve [Apache Hive](https://hive.apache.org/) üzerinde çalışan iş yükleri [Apache Spark](https://spark.apache.org/) kümeleri. GÇ önbelleği RubiX adlı açık kaynak bir önbelleğe alma bileşen kullanır. RubiX, verilere bulut depolama sistemlerinde büyük veri analiz altyapıları ile kullanmak için yerel disk önbellektir. RubiX sistemleri, önbelleğe alma arasında benzersiz olduğundan Solid-State sürücüleri (SSD) yerine işletim bellek ayırma amacıyla önbelleğe almak için kullanır. GÇ önbellek hizmeti başlatır ve kümenin her çalışan düğümüne RubiX meta verileri sunucularını yönetir. Ayrıca, kümenin tüm hizmetleri RubiX önbelleğin saydam kullanım için yapılandırır.

Çoğu SSD'ler, bant genişliği saniye başına 1'den fazla GByte sağlar. İşletim sistemi bellek içi dosyası önbelleği tarafından tamamlanan bu bant genişliği, büyük veri işlem işleme altyapıları Apache Spark'ı yüklemek için yeterli bant genişliği sağlar. İşlem bellek seçeneği gibi yoğun bellek bağımlı görevlerini işlemek Apache Spark için kullanılabilir kalır. Bellek işletme özel kullanımda olan Apache Spark'ın en iyi kaynak kullanımını elde etmek sağlar.  

>[!Note]  
>GÇ önbellek şu anda bir önbellek bileşeni olarak RubiX kullanır, ancak bu hizmet sürümlerini gelecekte değişebilir. Lütfen GÇ önbellek arabirimleri kullanır ve herhangi bir bağımlılığın doğrudan RubiX mantığınız yakalayana.

## <a name="benefits-of-azure-hdinsight-io-cache"></a>Azure HDInsight GÇ önbellek avantajları

GÇ önbellek kullanarak Azure Blob depolamadan veri okuyan işleri için performans artışı sağlar.

GÇ Cache kullanırken performans artışı görmek için Spark işleri herhangi bir değişiklik gerekmez. GÇ önbellek devre dışı bırakıldığında, bu Spark kod veri uzaktan Azure Blob depolama alanından okuduğunuz: `spark.read.load('wasbs:///myfolder/data.parquet').count()`. GÇ önbellek etkinleştirildiğinde, aynı kod satırının önbelleğe alınmış bir GÇ Önbellek Okuma neden olur. Üzerinde aşağıdaki gerçekleştirilen okuma, verileri yerel olarak SSD okuyun. HDInsight kümesi üzerinde çalışan düğümleri ile yerel olarak bağlı, özel SSD sürücülerine bulunur. HDInsight GÇ önbellek bu yerel SSD önbelleğe almak için en düşük düzeyde gecikme süresi sağlar ve bant genişliğini en üst düzeye çıkarır kullanır.

## <a name="getting-started"></a>Başlarken

Azure HDInsight GÇ önbellek Önizleme'de varsayılan olarak devre dışı bırakılır. Apache Spark 2.3 çalışan 3.6 + Azure HDInsight Spark kümelerinde, g/ç önbellek kullanılabilir.  GÇ önbellek etkinleştirmek için aşağıdakileri yapın:

1. HDInsight kümenizi seçin [Azure portalında](https://portal.azure.com).

1. İçinde **genel bakış** seçme (küme seçtiğinizde varsayılan olarak açık) sayfası **Ambari giriş** altında **küme panoları**.

1. Seçin **GÇ önbellek** sol taraftaki hizmet.

1. Seçin **eylemleri** ve **etkinleştirme**.

    ![Ambari GÇ önbellek hizmetinde etkinleştirme](./media/apache-spark-improve-performance-iocache/ambariui-enable-iocache.png "Ambari GÇ önbellek hizmeti etkinleştirme")

1. Etkilenen tüm hizmetlerin kümede yeniden onaylayın.

>[!NOTE]  
> Etkin ilerleme çubuğu gösterir olsa da, diğer etkilenen hizmetleri yeniden başlatılana kadar g/ç önbellek gerçekten etkin değil.

## <a name="troubleshooting"></a>Sorun giderme
  
GÇ önbelleğini etkinleştirdikten sonra Spark işleri çalıştırma disk alanı hatalar alabilirsiniz. Spark ayrıca yerel disk depolama alanı karıştırma işlemleri sırasında veri depolamak için kullandığından bu hataları oluşur. Spark SSD alanı yetersiz GÇ önbelleği etkindir ve Spark deposu için alan azaltılmış bir kez çalıştırılabilir. Toplam SSD alanı yarısını GÇ önbelleği varsayılan olarak kullandığı alan miktarı. GÇ önbelleği için disk alanı kullanımı Ambari yapılandırılabilir. Disk alanı hataları alırsanız, g/ç önbelleği için kullanılan SSD alanı miktarını azaltmak ve hizmeti yeniden başlatın. GÇ önbelleği için ayrılan alanın değiştirmek için aşağıdaki adımları uygulayın:

1. Apache Ambari seçin **HDFS** sol taraftaki hizmet.

1. Seçin **yapılandırmaları** ve **Gelişmiş** sekmeler.

    ![Gelişmiş Yapılandırma HDFS Düzenle](./media/apache-spark-improve-performance-iocache/ambariui-hdfs-service-configs-advanced.png "Düzenle HDFS Gelişmiş Yapılandırma")

1. Aşağı kaydırıp genişletin **özel çekirdek-site** alan.

1. Özelliği bulun **hadoop.cache.data.fullness.percentage**.

1. Kutudaki değer değiştirin.

    ![G/ç önbellek tamlık yüzdesi Düzenle](./media/apache-spark-improve-performance-iocache/ambariui-cache-data-fullness-percentage-property.png "Düzenle g/ç önbellek tamlık yüzdesi")

1. Seçin **Kaydet** sağ üst köşedeki.

1. Seçin **yeniden** > **etkilenen tüm yeniden başlatma**.

    ![Etkilenen tüm yeniden başlatma](./media/apache-spark-improve-performance-iocache/ambariui-restart-all-affected.png "etkilenen tüm yeniden başlatma")

1. Seçin **yeniden başlatmayı Onayla tüm**.

Bu işe yaramazsa, g/ç önbellek devre dışı bırakın.

## <a name="next-steps"></a>Sonraki Adımlar

- Bu blog gönderisinde performans kıyaslamaları dahil olmak üzere, g/ç önbellek hakkında daha fazlasını okuyun: [Apache Spark işleri HDInsight GÇ Cache ile en fazla 9 x hızlandırmak elde edin](https://azure.microsoft.com/blog/apache-spark-speedup-with-hdinsight-io-cache/)
