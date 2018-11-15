---
title: Visual Studio için Data Lake Araçları'nı kullanarak Azure HDInsight'a bağlanma
description: Yükleme ve Azure HDInsight, Apache Hadoop kümelerine bağlanmak için Visual Studio için Data Lake Araçları'nı kullanın ve ardından Hive sorguları çalıştırma hakkında bilgi edinin.
keywords: hadoop araçları, hive sorgusu, visual studio, visual studio hadoop
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive, hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.openlocfilehash: 670de3f61047bcc8b168863f5981e41084225ec4
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634677"
---
# <a name="use-data-lake-tools-for-visual-studio-to-connect-to-azure-hdinsight-and-run-apache-hive-queries"></a>Bağlanmak için Azure HDInsight ve Apache Hive sorguları çalıştırmak için Visual Studio için Data Lake Araçları'nı kullanın

Apache Hadoop kümelerini bağlanmak için (Azure Data Lake ve Stream Analytics Tools for Visual Studio da denir) Visual Studio için Data Lake araçları kullanmayı öğrenin [Azure HDInsight](../hdinsight-hadoop-introduction.md) ve Hive sorguları göndermek. 

HDInsight'ı kullanma hakkında daha fazla bilgi için [HDInsight'a giriş](../hdinsight-hadoop-introduction.md) ve [HDInsight ile çalışmaya başlama](apache-hadoop-linux-tutorial-get-started.md) bölümlerine göz atın. 

Bir Storm kümesine bağlanma hakkında daha fazla bilgi için bkz. [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojisi geliştirme](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

Visual Studio için Data Lake Araçlarını hem Azure Data Lake Analytics’e hem de HDInsight’a erişmek için kullanabilirsiniz. Data Lake Araçları hakkında bilgi için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak ve Visual Studio için Data Lake Araçları’nı kullanmak üzere şunlar gerekir:

* Bir Azure HDInsight kümesi. Bir HDInsight kümesi oluşturmak için bkz. [Azure HDInsight’ta Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md). Etkileşimli Hive sorguları çalıştırmak için bir [HDInsight Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md) kümesi gerekir.
* Visual Studio 2017, 2015 ya da 2013’ün yüklü olduğu bir bilgisayar.
    
    > [!NOTE]
    > Şu anda Visual Studio için Data Lake Araçları’nın yalnızca İngilizce sürümü mevcuttur.
    > 
    > 

## <a name="install-or-update-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake Araçları’nı yükleme veya güncelleştirme

### <a name="install-data-lake-tools"></a>Data Lake Araçları’nı yükleme

Data Lake Araçları, Visual Studio 2017 için varsayılan olarak yüklenir. Visual Studio'nun önceki sürümleri için Data Lake Araçları’nı [Web Platformu Yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyebilirsiniz. Visual Studio sürümünüzle eşleşen Data Lake Araçları sürümünü seçin. 

### <a name="install-visual-studio"></a>Visual Studio yükleme

Visual Studio yüklü değilse, Visual Studio Community ve Azure SDK'sının en son sürümlerini yüklemek için [Web Platformu Yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)’ni kullanın:

![Visual Studio için Data Lake Araçları Web Platformu Yükleyicisinin Ekran Görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "Visual Studio için Data Lake Araçları'nı yüklemek üzere Web Platformu Yükleyicisi'ni kullanma")

### <a name="update-the-tools"></a>Araçları güncelleştirme

1. Visual Studio'yu açın.
2. **Araçlar** menüsünde **Uzantılar ve güncelleştirmeler**’i seçin.
3. **Güncelleştirmeler**’i genişletin ve **Azure Data Lake ve Stream Analytics Araçları**’nı seçin (yüklüyse).

> [!NOTE]
>
> Etkileşimli Sorgu kümelerine bağlanmak ve etkileşimli Hive sorguları çalıştırmak için yalnızca Data Lake Araçları sürüm 2.3.0.0 veya üzerini kullanabilirsiniz.

## <a name="connect-to-azure-subscriptions"></a>Azure aboneliklerine bağlanma
Visual Studio için Data Lake Araçları’nı kullanarak HDInsight kümelerinize bağlanabilir, bazı temel yönetim işlemlerini gerçekleştirebilir ve Hive sorguları çalıştırabilirsiniz.

> [!NOTE]
> Genel bir Hadoop kümesine bağlanma hakkında bilgi için bkz. [Visual Studio kullanarak Hive sorguları yazma ve gönderme](https://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).
> 
> 

Azure aboneliğinize bağlanmak için:

1. Visual Studio'yu açın.
2. **Görünüm** menüsünde **Sunucu Gezgini**'ni seçin.
3. Sunucu Gezgini’nde **Azure** seçeneğini ve ardından **HDInsight** seçeneğini genişletin.
   
   > [!NOTE]
   > **HDInsight Görev Listesi** penceresi açık olmalıdır. Pencereyi görmüyorsanız, **Görünüm** menüsünde **Diğer Pencereler**’i ve ardından **HDInsight Görev Listesi Penceresi**’ni seçin.  
   > 
   > 
4. Azure aboneliği kimlik bilgilerinizi girin ve ardından **Oturum Aç**’ı seçin. Kimlik doğrulaması sadece daha önce bu bilgisayarda Visual Studio’dan Azure aboneliğinize bağlanmadıysanız gerekir.
5. Sunucu Gezgini’nde, var olan HDInsight kümelerinin listesi görünür. Kümeniz yoksa Azure portalı, Azure PowerShell veya HDInsight SDK’yı kullanarak bir küme oluşturabilirsiniz. Daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).
   
   ![Sunucu Gezgini’nde Visual Studio için Data Lake Araçları küme listesinin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Sunucu Gezgini’nde Visual Studio için Data Lake Araçları küme listesi")
6. HDInsight kümesini genişletin. **Hive Veritabanları**, varsayılan depolama hesabı, bağlantılı depolama hesapları ve **Hadoop Hizmeti günlüğü** görüntülenir. Varlıkları daha da genişletebilirsiniz.

Azure aboneliğinize bağlandıktan sonra aşağıdaki görevleri gerçekleştirebilirsiniz.

Visual Studio'dan Azure portalına bağlanmak için:

1. Sunucu Gezgini'nde **Azure** > **HDInsight**’ı seçin.
2. Bir HDInsight kümesine sağ tıklayın ve sonra **Azure portalında Küme Yönetimi**’ni seçin.

Visual Studio ile ilgili soru sormak ve geri bildirim sağlamak için:

1. **Araçlar** menüsünde **HDInsight**’ı seçin.
2. Soru sormak için **MSDN Forumu**’nu seçin. Geri bildirimde bulunmak için **Geri Bildirim Gönder**’i seçin.

## <a name="explore-linked-resources"></a>Bağlantılı kaynakları araştırma
Sunucu Gezgini'nde, varsayılan depolama hesabını ve bağlantılı tüm depolama hesaplarını görebilirsiniz. Varsayılan depolama hesabını genişletirseniz, depolama hesabında kapsayıcıları görebilirsiniz. Varsayılan depolama hesabı ve varsayılan kapsayıcı işaretlenmiştir. Kapsayıcı içeriğini görüntülemek için kapsayıcıların herhangi birine sağ tıklayın.

![Sunucu Gezgini’nde Visual Studio için Data Lake Araçları bağlantılı kaynakları listeleme ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "Bağlantılı kaynakları listeleme")

Bir kapsayıcıyı açtıktan sonra aşağıdaki düğmeleri kullanarak blob’ları karşıya yükleyebilir, silebilir ve indirebilirsiniz:

![Sunucu Gezgini’nde Visual Studio için Data Lake Araçları blob işlemlerinin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "Sunucu Gezgini’nde blob yükleme, silme ve indirme")

## <a name="run-interactive-hive-queries"></a>Etkileşimli Hive sorguları çalıştırma
[Apache Hive](http://hive.apache.org), Hadoop üzerinde oluşturulmuş bir veri ambarı altyapısıdır. Hive veri özetleme, sorgular ve analiz için kullanılır. Visual Studio’dan Hive sorguları çalıştırmak üzere Visual Studio için Data Lake Araçları’nı kullanabilirsiniz. Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma](hdinsight-use-hive.md).

[Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md), Apache Hive 2.1 sürümünde [LLAP üzerinde Hive](https://cwiki.apache.org/confluence/display/Hive/LLAP) kullanır. Etkileşimli Sorgu büyük, depolanmış veri kümelerinde karmaşık veri ambarı stili sorgulara etkileşim katar. Etkileşimli Sorgu üzerinde Hive sorgularının çalıştırılması, geleneksel Hive toplu işlerine kıyasla çok daha hızlıdır. Daha fazla bilgi için bkz. [Hive toplu işleri çalıştırma](#run-hive-batch-jobs).

> [!NOTE]
>
> Etkileşimli Hive sorgularını yalnızca bir [HDInsight Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md) kümesine bağlandığınızda çalıştırabilirsiniz.

Ayrıca, Visual Studio için Data Lake Araçları’nı kullanarak bir Hive işinin içeriğini görebilirsiniz. Visual Studio için Data Lake Araçları bazı Hive işlerinin Yarn günlüklerini toplar ve yüzeye çıkarır.

### <a name="view-hivesampletable"></a>**hivesampletable** öğesini görüntüleme
Tüm HDInsight kümeleri hivesampletable adlı varsayılan bir örnek Hive tablosuna sahiptir. Hive tablosu, Hive tablolarını listeleme, tablo şemalarını görüntüleme ve Hive tablosundaki satırları listeleme işlemini tanımlar.

Hive tablolarını listelemek ve Hive tablo şemasını görüntülemek için:

1. Tablo şemasını görmek için **Sunucu Gezgini**'nde **Azure** > **HDInsight**’ı seçin. Kümenizi seçin ve ardından **Hive Veritabanları** > **Varsayılan** > **hivesampletable** öğesini seçin.
2. **hivesampletable** öğesine sağ tıklayın ve ardından satırları listelemek için **İlk 100 Satırı Görüntüle**’ye tıklayın. Bu, Hive ODBC sürücüsünü kullanarak aşağıdaki Hive sorgusunu çalıştırmaya eşdeğerdir:
   
     `SELECT * FROM hivesampletable LIMIT 100`
   
   Satır sayısını özelleştirebilirsiniz.
   
   ![HDInsight Hive Visual Studio şema sorgusunun ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Hive sorgu sonuçları")

### <a name="create-hive-tables"></a>Hive tabloları oluşturma
Bir Hive tablosu oluşturmak için GUI’yi ya da Hive sorgularını kullanabilirsiniz. Hive sorgularını kullanma hakkında daha fazla bilgi için bkz. [Hive sorgularını çalıştırma](#run.queries).

Hive tablosu oluşturmak için:

1. **Sunucu Gezgini**’nde **Azure** > **HDInsight Kümeleri**’ni seçin. HDInsight kümenizi ve ardından **Hive Veritabanları**’nı seçin.
2. **Varsayılan**’a sağ tıklayıp **Tablo Oluştur**’u seçin.
3. Tabloyu yapılandırın.  
4. Yeni Hive tablosu oluşturmak üzere işi göndermek için **Tablo Oluştur**’u seçin.
   
    ![HDInsight Visual Studio Araçları Tablo Oluştur penceresinin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Hive tablosu oluştur")

### <a name="run.queries"></a>Hive sorgularını çalıştırma ve doğrulama
Hive sorguları oluşturmak ve çalıştırmak için iki seçeneğiniz vardır:

* Geçici sorgular oluşturma
* Hive uygulaması oluşturma

Geçici sorgular oluşturmak, doğrulamak ve çalıştırmak için:

1. **Sunucu Gezgini**’nde **Azure** > **HDInsight Kümeleri**’ni seçin.
2. Sorguyu çalıştırmak istediğiniz yerde sağ tıklayın ve **Hive Sorgusu Yaz**’ı seçin.  
3. Hive sorgularını girin. 

    Hive düzenleyicisi IntelliSense’i destekler. Visual Studio için Data Lake Araçları, Hive betiğinizi düzenlerken uzak meta verilerin yüklenmesini destekler. Örneğin, **SELECT * FROM** yazarsanız IntelliSense önerilen tüm tablo adlarını listeler. Bir tablo adı belirtildiğinde, IntelliSense sütun adlarını listeler. Araçlar çoğu Hive DML deyimlerini, alt sorguları ve yerleşik UDF'leri destekler.
   
    ![HDInsight Visual Studio Araçları IntelliSense örnek 1’in ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![HDInsight Visual Studio Araçları IntelliSense örnek 2’nin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > IntelliSense yalnızca HDInsight araç çubuğunda seçilen kümelerin meta verilerini önerir.
   > 
   
4. (İsteğe bağlı) Betik söz dizimi hatalarını denetlemek için **Betiği Doğrula**’yı seçin.
   
    ![Visual Studio için Data Lake Araçları yerel doğrulamasının ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "Betik doğrulama")
5. **Gönder** veya **Gönder (Gelişmiş)** öğesini seçin. Gelişmiş gönderme seçeneğini belirlerseniz, betik için **İş Adı**, **Bağımsız Değişkenler**, **Ek Yapılandırmalar** ve **Durum Dizini**’ni yapılandırın:
   
    ![HDInsight Hadoop Hive sorgusunun ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Sorgu gönderme")
   
    İşi gönderdikten sonra **Hive İşi Özeti** penceresi görüntülenir.
   
    ![Bir HDInsight Hadoop Hive sorgusu özetinin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Hive iş özeti")
6. İş durumu **Tamamlandı** olarak değişinceye kadar, durumu güncelleştirmek için **Yenile** düğmesini kullanın.
7. **İş Sorgusu**, **İş Çıktısı**, **İş Günlüğü** veya **Yarn günlüğü**’nü görmek için alt kısımdaki bağlantıları seçin.

Hive çözümü oluşturmak ve çalıştırmak için:

1. **Dosya** menüsünde **Yeni**'yi ve ardından **Proje**'yi seçin.
2. Sol bölmede **HDInsight**’ı seçin. Orta bölmede seçin **Hive Uygulaması**’nı seçin. Özellikleri girip **Tamam**’ı seçin.
   
    ![HDInsight Visual Studio Araçları yeni Hive projesinin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Visual Studio’dan Hive uygulamaları oluşturma")
3. Betiği açmak için **Çözüm Gezgini**’nde **Script.hql** öğesine çift tıklayın.
4. Hive betiğini doğrulamak için **Betiği Doğrula** düğmesini seçin. Veya Hive düzenleyicisinde betiğe sağ tıklayabilir ve sonra açılır menüden **Betiği Doğrula**’yı seçebilirsiniz.

### <a name="view-hive-jobs"></a>Hive İşlerini Görüntüleme
Hive işleri için iş sorguları, iş çıktısı, iş günlükleri ve Yarn günlüklerini görüntüleyebilirsiniz. Daha fazla bilgi için yukarıdaki ekran görüntüsüne bakın.

Araçların en son sürümünde Yarn günlüklerini toplayarak ve görünmesini sağlayarak Hive işlerinizin içeriğini görebilirsiniz. Yarn günlüğü, performans sorunlarını araştırmanıza yardımcı olabilir. HDInsight'ın Yarn günlüklerini nasıl topladığı hakkında daha fazla bilgi için bkz. [HDInsight uygulama günlüklerine programlamayla erişme](../hdinsight-hadoop-access-yarn-app-logs.md).

Hive işlerini görüntülemek için:

1. **Sunucu Gezgini**’nde **Azure** seçeneğini ve ardından **HDInsight** seçeneğini genişletin.
2. HDInsight kümesine sağ tıklayın ve ardından **İşleri Görüntüle**’yi seçin. Küme üzerinde çalıştırılan Hive işlerinin listesi görüntülenir.  
3. Bir iş seçin. **Hive İş Özeti** penceresinde aşağıdakilerden birini seçin:
    - **İş Sorgusu**
    - **İş Çıktısı**
    - **İş Günlüğü**  
    - **Yarn günlüğü**
   
    ![HDInsight Visual Studio Araçları Hive İşlerini Görüntüle penceresinin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Hive işlerini görüntüle")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>HiveServer2 aracılığıyla daha hızlı Hive yürütme
> [!NOTE]
> Bu özellik yalnızca HDInsight 3.2 veya üzeri bir sürümdeki kümede çalışır.
 
Hive işlerini [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (Templeton olarak da bilinir) aracılığıyla göndermek için kullanılan Visual Studio için Data Lake Araçları. Hive işlerini göndermeye yönelik bu yöntemin, iş ayrıntılarını ve hata bilgilerini döndürmesi uzun zaman almıştır.

Bu performans sorununu çözmek üzere Visual Studio için Data Lake Araçları, RDP/SSH’yi atlar ve Hive işlerini doğrudan HiveServer2 aracılığıyla küme içinde yürütür.

Daha iyi performansa ek olarak, bu yöntemi kullanarak Apache Tez grafiklerinde ve görev ayrıntılarında Hive görüntüleyebilirsiniz.

HDInsight 3.2 veya sonraki sürümlerinde bir kümede **HiveServer2 aracılığıyla yürüt** düğmesi görünür:

![Visual Studio için Data Lake Araçları HiveServer2 aracılığıyla yürüt seçeneğinin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "HiveServer2 kullanarak Hive soruları yürüt")

Geri akışı yapılan günlükleri gerçek zamanlı olarak da görebilirsiniz. Hive sorgusu Tez'de yürütülürse iş grafiklerini de görebilirsiniz.

![Visual Studio için Data Lake Araçları HiveServer2 ile daha hızlı Hive yürütme ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "İş grafiklerini görüntüleme")

### <a name="execute-queries-via-hiveserver2-vs-submit-queries-via-webhcat"></a>HiveServer2 aracılığıyla sorgu yürütme ve WebHCat aracılığıyla sorgu gönderme karşılaştırması

Sorguları HiveServer2 aracılığıyla yürütme yönteminin birçok performans avantajı sunmasına rağmen bazı kısıtlamaları da vardır. Bazı kısıtlamalar nedeniyle bu yöntem, üretim kullanımına uygun değildir. 

Aşağıdaki tabloda sorguları HiveServer2 aracılığıyla yürütme ile sorguları WebHCat aracılığıyla gönderme arasındaki farklar gösterilmiştir:

|  | HiveServer2 aracılığıyla yürütme | WebHCat aracılığıyla gönderme |
| --- | --- | --- |
| Sorguları yürütme |WebHCat’te ek yükü ortadan kaldırır (TempletonControllerJob adlı bir MapReduce İşi çalıştırır). |Bir sorgu WebHCat aracılığıyla yürütülürse, WebHCat ek gecikme sağlayan bir MapReduce işi başlatır. |
| Geriye akış günlükleri |Neredeyse gerçek zamanlı olarak. |Yalnızca iş tamamlandığında, iş yürütme günlüklerini kullanılabilir. |
| İş geçmişini görüntüleme |Bir sorgu HiveServer2 aracılığıyla yürütülürse, buna ait iş geçmişi (iş günlüğü, iş çıktısı) korunmaz. Uygulamayı Yarn kullanıcı arabiriminde sınırlı bilgiyle görüntüleyebilirsiniz. |Bir sorgu WebHCat aracılığıyla yürütülürse, buna ait iş geçmişi (iş günlüğü, iş çıktısı) korunmaz. Visual Studio, HDInsight SDK veya PowerShell kullanarak iş geçmişini görüntüleyebilirsiniz. |
| Pencereyi kapatma |HiveServer2 aracılığıyla yürütme işlemi *zaman uyumludur*. Pencereler kapatılırsa sorgu yürütme işlemi iptal edilir. |WebHCat aracılığıyla gönderme işlemi *zaman uyumsuzdur*. Sorguyu WebHCat aracılığıyla gönderip Visual Studio'yu kapatabilirsiniz. İstediğiniz zaman geri dönüp sonuçlara bakabilirsiniz. |

### <a name="tez-hive-job-performance-graph"></a>Tez Hive işi performans grafiği
Visual Studio için Data Lake Araçları’nda Tez yürütme altyapısının çalıştığı Hive işleri için performans grafiklerini görebilirsiniz. Tez'i etkinleştirme hakkında daha fazla bilgi için bkz. [HDInsight'ta Hive kullanma](hdinsight-use-hive.md). 

Visual Studio'da bir Hive işi gönderdikten sonra, iş tamamlandığında Visual Studio grafiği gösterir. En son iş durumunu görüntülemek için **Yenile** düğmesini seçmeniz gerekebilir.

> [!NOTE]
> Bu özellik yalnızca HDInsight 3.2.4.593 veya üzeri bir sürümdeki kümede kullanılabilir. Özellik yalnızca tamamlanmış işler üzerinde çalışır. Ayrıca, bu özelliği kullanmak için işi WebHCat aracılığıyla göndermeniz gerekir. Sorguyu HiveServer2 aracılığıyla yürüttüğünüzde aşağıdaki görüntü gösterilir: 
> 
> ![Hadoop Hive Tez performans grafiğinin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "İş durumu")

Hive sorgunuzu daha iyi anlamanıza yardımcı olması için bu sürüme Hive İşleci görünümü eklenmiştir. Köşe içindeki tüm işleçleri görüntülemek için iş grafiğinin köşelerine çift tıklayın. Ayrıca, işleç hakkında daha fazla ayrıntı görmek için belirli bir işleci işaret edebilirsiniz.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Tez işlerinde Hive için Görev Yürütme Görünümü
Tez işlerinde Hive için Görev Yürütme Görünümünü kullanarak Hive işlerine göre yapılandırılmış ve görselleştirilmiş bilgiler alabilirsiniz. Bununla birlikte, daha fazla iş ayrıntısı alabilirsiniz. Performans sorunları oluşursa, sorun hakkında daha fazla bilgi almak için bu görünümü kullanabilirsiniz. Örneğin, her bir görevin nasıl çalıştığı hakkında bilgi ve her görev (veri okuma/yazma, zamanlama/başlangıç/bitiş zamanı vb.) hakkında ayrıntılı bilgi elde edebilirsiniz. İş yapılandırmalarını veya sistem mimarisini görselleştirilmiş bilgilere göre ayarlamak için bilgileri kullanın.

![Data Lake Visual Studio Araçları Görev Yürütme Görünümünün ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Görev Yürütme Görünümü")

## <a name="run-hive-batch-jobs"></a>Hive toplu işleri çalıştırma
Bir Hive betiğinin HDInsight kümesine göre test edilmesi (Etkileşimli Sorgu kümesi dışında) uzun süren bir işlem olabilir. İşlem birkaç dakika veya daha fazla sürebilir. Visual Studio için Data Lake Araçları, dinamik bir kümeye bağlanmadan Hive betiğini yerel olarak doğrulayabilir. Etkileşimli sorgular çalıştırma hakkında daha fazla bilgi için bkz. [Etkileşimli Hive sorguları çalıştırma](#run-interactive-hive-queries).

Belirli Hive işlerine ait Yarn günlüklerini toplayarak ve görünmesini sağlayarak Hive işinin içeriğini görmek istiyorsanız Visual Studio için Data Lake Araçları’nı kullanabilirsiniz.

Hive toplu işleri çalıştırma hakkında daha fazla bilgi almak için bkz. [Etkileşimli Hive sorguları çalıştırma](#run-interactive-hive-queries). Bu bölümdeki bilgiler, daha uzun süreli Hive toplu işlerini çalıştırma için geçerlidir.

## <a name="run-pig-scripts"></a>Pig betikleri çalıştırma
Visual Studio için Data Lake Araçları’nı kullanarak Pig betikleri oluşturup HDInsight kümelerine gönderebilirsiniz. İlk olarak, şablondan bir Pig projesi oluşturun. Ardından, betiği HDInsight kümelerine gönderin.

## <a name="feedback-and-known-issues"></a>Geri bildirim ve bilinen sorunlar
* Şu anda, HiveServer2 sonuçları ideal olmayan düz metin biçiminde gösterilmektedir. Microsoft bu sorunu gidermek için çalışmaktadır.
* Null değerlerle başlatılan sonuçların gösterilmediği bir sorun düzeltilmiştir. Bu sorun sizi engelliyorsa destek ekibine başvurun.
* Visual Studio tarafından oluşturulan HQL betiği kullanıcının yerel bölge ayarlarına bağlı olarak kodlanır. Betiği bir kümeye ikili dosya olarak yüklerseniz betik doğru şekilde yürütülmez.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Visual Studio’dan HDInsight kümelerine bağlanmak üzere Visual Studio için Data Lake Araçları paketini kullanmayı öğrendiniz. Ayrıca bir Hive sorgusu çalıştırmayı öğrendiniz. Daha fazla bilgi için şu makalelere bakın:

* [HDInsight'ta Hadoop Hive kullanma](hdinsight-use-hive.md)
* [HDInsight'ta Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight'ta Hadoop işlerini gönderme](submit-apache-hadoop-jobs-programmatically.md)
* [HDInsight'ta Hadoop ile Twitter verilerini çözümleme](../hdinsight-analyze-twitter-data.md)

