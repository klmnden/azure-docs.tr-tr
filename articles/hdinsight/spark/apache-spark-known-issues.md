---
title: Azure hdınsight'ta Apache Spark kümesi ile ilgili sorunları giderme | Microsoft Docs
description: Azure Hdınsight ve bunlar nasıl Apache Spark kümeleri ile ilgili sorunlar hakkında bilgi edinin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 610c4103-ffc8-4ec0-ad06-fdaf3c4d7c10
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: 664c97117de793209007843fa23c98f52c2b079d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>Hdınsight'ta Apache Spark kümesi için bilinen sorunlar

Bu belge bilinen sorunlar Hdınsight Spark genel Önizleme için izler.  

## <a name="livy-leaks-interactive-session"></a>Etkileşimli oturum Livy sızdırıyor
Livy bir etkileşimli oturum hala etkin (Ambari veya headnode 0 sanal makine yeniden başlatma nedeniyle) yeniden başlatıldıktan sonra bir etkileşimli iş oturumu sızmış. Sonuç olarak, yeni işleri kabul edilen durumunda kalmış olabilir.

**Risk Azaltma:**

Sorunu çözmek için aşağıdaki yordamı kullanın:

1. SSH headnode içine. Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Livy başlatılan etkileşimli işleri uygulama kimliklerini bulmak için aşağıdaki komutu çalıştırın. 
   
        yarn application –list
   
    İşlerini hiçbir açık adı belirtilen Livy etkileşimli oturum ile başlatılmış varsayılan proje adları Livy olacaktır. İş adı Jupyter not defteri tarafından başlatılan Livy oturumu için remotesparkmagics_ * ile başlatır. 
3. Bu işleri sonlandırmak için aşağıdaki komutu çalıştırın. 
   
        yarn application –kill <Application ID>

Yeni iş çalışmaya başlar. 

## <a name="spark-history-server-not-started"></a>Spark geçmişi sunucu başlatılmadı
Spark geçmişi sunucu bir küme oluşturulduktan sonra otomatik olarak başlatılan bir dosya değil.  

**Risk Azaltma:** 

El ile geçmişi sunucunun Ambari başlatın.

## <a name="permission-issue-in-spark-log-directory"></a>Spark günlük dizini izin sorunu
kullanarak bir iş gönderme spark-gönderdiğinizde hdiuser şu hatayı alır:

```
java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied)
```
Ve sürücü günlüğüne yazılır. 

**Risk Azaltma:**

1. Hdiuser Hadoop grubuna ekleyin. 
2. 777 izinleri üzerinde /var/log/spark Küme oluşturulduktan sonra sağlayın. 
3. Bir dizin 777 izinlerine sahip olması için Ambari kullanarak spark günlük konumunu güncelleştirin.  
4. Çalışma spark-gönderme sudo.  

## <a name="spark-phoenix-connector-is-not-supported"></a>Spark Phoenix bağlayıcı desteklenmez

Hdınsight Spark kümeleri Spark Phoenix Bağlayıcısı'nı desteklemez.

**Risk Azaltma:**

Bunun yerine Spark HBase Bağlayıcısı'nı kullanmanız gerekir. Yönergeler için bkz: [Spark HBase bağlayıcıyı kullanmak üzere nasıl](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).

## <a name="issues-related-to-jupyter-notebooks"></a>Jupyter not defterleri için ilgili sorunlar
Jupyter not defterleri için ilgili bazı bilinen sorunlar aşağıda verilmiştir.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Dizüstü bilgisayarlarla dosya adları ASCII olmayan karakterler
ASCII olmayan karakterler Jupyter not defteri adlarında kullanmayın. ASCII olmayan dosya adı varsa, Jupyter UI aracılığıyla bir dosyayı karşıya yüklemeyi denerseniz, herhangi bir hata iletisi başarısız olur. Jupyter dosyayı karşıya yüklemeyi izin vermez ancak onu görünen bir hata ya da durum oluşturmaz.

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Daha büyük boyutta not defterlerini yüklenirken hata oluştu
Bir hata görebilirsiniz **`Error loading notebook`** daha büyük boyutta not defterlerini yükleme.  

**Risk Azaltma:**

Bu hata alırsanız, bozuk veya kayıp verilerinizi gelmez.  Yine diskte, dizüstü bilgisayarlarda `/var/lib/jupyter`, ve bunlara erişmek için kümeye SSH kullanabilirsiniz. Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

SSH kullanarak kümeye bağlandıktan sonra not defterlerinizi kümenizden (SCP veya WinSCP kullanarak) yerel makinenize not defterindeki önemli verilerin kaybını önlemek için yedek olarak kopyalayabilirsiniz. Bağlantı noktası ağ geçidi üzerinden geçmeden Jupyter erişmek için 8001, headnode içine SSH tüneli kullanabilirsiniz.  Buradan, dizüstü bilgisayarınızı çıktısını temizleyin ve not defterinin boyutu en aza indirmek için yeniden kaydedin.

Bu hata gelecekte tekrarlamasını önlemek için bazı en iyi uygulamaları izlemelisiniz:

* Not defteri boyutunu küçük tutmaya önemlidir. Geri Jupyter için gönderilen bir Spark işleriniz çıktı not defterinde kalıcıdır.  Bunu Jupyter ile en iyi uygulama genel çalışan kaçınmaktır `.collect()` üzerinde büyük RDD'ın veya dataframes; bir RDD'ın içeriğini peek istiyorsanız, bunun yerine, çalışan göz önünde bulundurun `.take()` veya `.sample()` böylelikle, çıkış çok büyük açılmıyor.
* Bir not defteri kaydettiğinizde, ayrıca, Temizle tüm boyutunu azaltmak için hücreleri çıktı.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Not defteri başlangıç beklenenden uzun sürüyor
Jupyter Not Defteri kullanarak Spark Sihirli ilk kod deyiminde birden fazla bir dakika sürebilir.  

**Açıklama:**

Birinci kod hücresini çalıştırdığınızda çünkü bu gerçekleşir. Arka planda bu başlatır oturum yapılandırması ve Spark, SQL ve Hive bağlamları ayarlayın. Sonra bu içerikler ayarlayın, ilk ifadesini çalıştırmak ve bu izlenim gösteren deyimi tamamlanması uzun zaman aldı.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Oturum oluşturma Jupyter not defteri zaman aşımına uğradı
Spark kümesi kaynaklar yetersiz olduğunda, Jupyter Not Defteri, Spark ve PySpark tekrar oturum oluşturulmaya çalışılırken zaman aşımı olur. 

**Azaltıcı Etkenler:** 

1. Bazı kaynaklar tarafından Spark kümenizdeki boşaltmak:
   
   * Diğer Spark not defterlerini kapatın ve durdurmak menüsüne giderek veya dizüstü bilgisayar explorer'ın kapanması tıklatarak durduruluyor.
   * YARN diğer Spark uygulamalardan durduruluyor.
2. Başlatılması çalıştığınız dizüstü bilgisayar yeniden başlatın. Yeterli kaynaklara oturum şimdi oluşturmak için kullanılabilir olması gerekir.

## <a name="see-also"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

