---
title: Azure HDInsight, Apache Spark kümesi ile ilgili sorunları giderme
description: Azure HDInsight ve bunlar nasıl Apache Spark kümeleri ile ilgili sorunlar hakkında bilgi edinin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: hrasheed
ms.openlocfilehash: 51a0ee6f2d928d79e60ca9976d7651c70867a41f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64717598"
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>HDInsight üzerinde Apache Spark kümesi için bilinen sorunlar

Bu belgede, HDInsight Spark genel önizlemesi için tüm bilinen sorunları izler.  

## <a name="apache-livy-leaks-interactive-session"></a>Etkileşimli oturum Apache Livy sızıntıları
Zaman [Apache Livy](https://livy.incubator.apache.org/) yeniden başlatılır (gelen [Apache Ambari](https://ambari.apache.org/) veya 0 baş düğümüne sanal makine yeniden başlatma nedeniyle) etkileşimli bir oturum ile hala canlı, etkileşimli işi oturumu sızmış. Sonuç olarak, yeni işleri kabul edilen durumunda takılabilir.

**Azaltma:**

Sorunu çözmek için aşağıdaki yordamı kullanın:

1. SSH baş içine. Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Lıvy başlatılan etkileşimli işleri uygulama kimliklerini bulmak için aşağıdaki komutu çalıştırın. 
   
        yarn application –list
   
    Varsayılan proje adları, işleri Livy etkileşimli oturumla hiçbir açık adı belirtilen başlatılmış Livy olacaktır. Livy oturumu tarafından başlatıldı için [Jupyter not defteri](https://jupyter.org/), iş adı başlar remotesparkmagics_ * ile. 
3. Bu işleri sonlandırmak için aşağıdaki komutu çalıştırın. 
   
        yarn application –kill <Application ID>

Yeni işleri çalıştırmaya başlayın. 

## <a name="spark-history-server-not-started"></a>Spark geçmiş sunucusu başlatılmadı
Spark geçmiş sunucusu bir küme oluşturulduktan sonra otomatik olarak başlatılan bir dosya değil.  

**Azaltma:** 

El ile Ambari'den geçmişi sunucuyu başlatın.

## <a name="permission-issue-in-spark-log-directory"></a>İzin sorunu Spark günlük dizini
kullanarak bir iş gönderme spark-submit, hdiuser şu hatayı alır:

```
java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied)
```
Ve hiçbir sürücüsü günlüğüne yazılır. 

**Azaltma:**

1. Hdiuser Hadoop grubuna ekleyin. 
2. 777 izinler, Küme oluşturulduktan sonra üzerinde /var/log/spark sağlayın. 
3. Bir dizin 777 izinlerine sahip olması için Ambari kullanarak spark günlük konumu güncelleştirin.  
4. Çalıştırma spark-submit sudo.  

## <a name="spark-phoenix-connector-is-not-supported"></a>Phoenix Spark Bağlayıcısı desteklenmiyor

HDInsight Spark kümeleri, Phoenix Spark Bağlayıcısı'nı desteklemez.

**Azaltma:**

Bunun yerine HBase Spark Bağlayıcısı'nı kullanmanız gerekir. Yönergeler için bkz. [Spark HBase bağlayıcı kullanma konusuna](https://web.archive.org/web/20190112153146/ https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).

## <a name="issues-related-to-jupyter-notebooks"></a>Jupyter not defterleri için ilgili sorunlar
Jupyter not defterleri için ilgili bazı bilinen sorunlar aşağıda verilmiştir.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Not Defterleri ile dosya adlarını ASCII olmayan karakterler
Jupyter not defteri adlarında ASCII olmayan karakterler kullanmayın. ASCII olmayan dosya adı varsa, Jupyter UI aracılığıyla bir dosyayı karşıya yüklemeyi denerseniz, herhangi bir hata iletisi başarısız oluyor. Jupyter, dosyayı karşıya yüklemeyi izin vermez ancak görünen bir hata ya da oluşturmaz.

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Not defterlerini daha büyük boyuttaki yüklenirken hata oluştu
Bir hata görebilirsiniz **`Error loading notebook`** yükü olduğunda daha büyük boyutta olan dizüstü bilgisayarlar.  

**Azaltma:**

Bu hata alırsanız, bozuk veya kayıp verilerinizi gelmez.  Yine de, disk üzerinde not defterlerinizi olan `/var/lib/jupyter`, ve bunlara erişmek için kümeye SSH kullanabilirsiniz. Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

SSH kullanarak kümeye bağlandıktan sonra not defterlerinizi kümenizden (SCP veya WinSCP kullanarak) yerel makinenize not defterinde önemli verilerin kaybını önlemek için yedek olarak kopyalayabilirsiniz. Ardından, baş düğüm ağ geçidi üzerinden geçmeden Jupyter erişmeye 8001 numaralı bağlantı noktasında içine SSH tüneli kullanabilirsiniz.  Burada, not defterinizin çıktısını temizleyin ve not defterinin boyutu en aza indirmek için yeniden kaydedin.

Bu hata, gelecekte gerçekleşmesini önlemeye yönelik bazı en iyi uygulamaları izlemelisiniz:

* Dizüstü bilgisayar boyutu küçük tutmak önemlidir. Jupyter için geri gönderilir, Spark işleri çıktısını not defterinde kalıcı hale getirilir.  Bunu Jupyter en iyi uygulama genel çalışmasını önlemek içindir `.collect()` üzerinde büyük RDD'ın veya veri çerçevelerini; bir RDD'ın içeriğini Özet istiyorsanız, bunun yerine, çalışan göz önünde bulundurun `.take()` veya `.sample()` böylece çıkışınızı çok büyük elde edemez.
* Bir not defteri kaydettiğinizde, ayrıca, clear tüm hücreleri boyutunu azaltmak için çıktı.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Not defteri başlangıç beklenenden uzun sürüyor
Spark Sihri kullanarak Jupyter not defteri ilk kod deyiminde birden fazla bir dakika sürebilir.  

**Açıklama:**

Birinci kod hücresini çalıştırdığınızda çünkü bu gerçekleşir. Arka planda bu başlatır oturum yapılandırması ve Spark, SQL ve Hive bağlamları ayarlayın. Sonra şu bağlamlarda ayarlayın, ilk deyimi çalıştırın ve bu izlenimi gösteren deyim tamamlanması uzun sürdü.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter not defteri zaman aşımı oturumu oluşturma
Spark kümesi kaynaklar yetersiz olduğunda, Jupyter not defterine Spark ve PySpark çekirdekleri oturum oluşturulmaya çalışılırken zaman aşımı olur. 

**Risk azaltma işlemleri:** 

1. Bazı kaynaklar tarafından Spark kümenizde boşaltmak:
   
   * Kapat ve durdurmak menüsüne giderek veya not defteri Gezgini'nde kapatma tıklayarak diğer Spark not defterlerini durduruluyor.
   * YARN diğer Spark uygulamalardan durduruluyor.
2. Başlatma açmaya not defteri yeniden başlatın. Yeterli kaynaklara artık oturum oluşturmak için kullanılabilir olması gerekir.

## <a name="see-also"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

