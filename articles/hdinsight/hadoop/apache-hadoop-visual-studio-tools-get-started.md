---
title: "Visual Studio için Data Lake Araçları'nı kullanarak Azure HDInsight'a bağlanma | Microsoft Docs"
description: "Azure HDInsight'ta Hadoop kümelerine bağlanmak ve Hive sorguları çalıştırmak üzere Visual Studio için Data Lake Araçları'nı yüklemeyi ve kullanmayı öğrenin."
keywords: "hadoop araçları, hive sorgusu, visual studio, visual studio hadoop"
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/22/2018
ms.author: jgao
ms.openlocfilehash: 0f3e8f00b1cee1141277f4dee21fabf993e69ba0
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="connect-to-azure-hdinsight-and-run-hive-queries-using-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake Araçları'nı kullanarak Azure HDInsight'a bağlanma ve Hive sorguları çalıştırma| Microsoft Docs

[Azure HDInsight](../hdinsight-hadoop-introduction.md)’ta Hadoop kümelerine bağlanmak ve Hive sorguları göndermek üzere Visual Studio için Data Lake Araçları’nı (aynı zamanda Azure Data Lake ve Stream Analytics Tools olarak adlandırılır) kullanmayı öğrenin. HDInsight'ı kullanma hakkında daha fazla bilgi için [HDInsight'a giriş](../hdinsight-hadoop-introduction.md) ve [HDInsight ile çalışmaya başlama](apache-hadoop-linux-tutorial-get-started.md) bölümlerine göz atın. Bir Storm kümesine bağlanma hakkında daha fazla bilgi için bkz. [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojisi geliştirme ](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

Visual Studio için Data Lake Araçları hem Data Lake Analytics’e hem de HDInsight’a erişmek için kullanılabilir.  Data Lake Araçları hakkında bilgi edinmek için bkz. [Öğretici: Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

**Önkoşullar**

Bu öğreticiyi tamamlamak ve Visual Studio’da Data Lake Araçları’nı kullanmak için şunlar gerekir:

* Azure HDInsight kümesi: Bir tane oluşturmak için bkz: [Azure HDInsight’ta Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md). Etkileşimli Hive sorguları çalıştırma hakkında bilgi edinmek için bir [HDInsight Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md) kümesi gerekir.
* Visual Studio 2013/2015/2017 çalıştıran bir iş istasyonu.
    
    > [!NOTE]
    > Şu anda, Visual Studio için Data Lake Araçları yalnızca İngilizce sürüm ile birlikte gelir.
    > 
    > 

## <a name="install-and-upgrade-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake Araçları’nı yükleme ve yükseltme

Data Lake Araçları, Visual Studio 2017 için varsayılan olarak yüklenir. Daha eski Visual Studio sürümleri için [Web Platformu Yükleyicisi](https://www.microsoft.com/web/downloads/)’ni kullanarak yükleyebilirsiniz. Visual Studio sürümünüzle eşleşen birini seçmeniz gerekir. Visual Studio yüklü değilse, en son Visual Studio Community ve Azure SDK'sını [Web Platformu Yükleyicisi](https://www.microsoft.com/web/downloads/)’ni kullanarak yükleyebilirsiniz:

![Visual Studio için Data Lake Araçları Web Platformu Yükleyicisi.](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "Visual Studio için Data Lake Araçları'nı yüklemek üzere Web Platformu Yükleyicisi'ni kullanın")

**Araçları güncelleştirmek için**
1. Visual Studio'yu açın.
2. **Araçlar** menüsünde **Uzantılar ve güncelleştirmeler**’e tıklayın.
3. **Güncelleştirmeler**’i genişletin ve varsa **Azure Data Lake ve Stream Analytics Araçları**’nı güncelleştirin.

> [!NOTE]
>
> Etkileşimli Sorgu kümelerine bağlanma ve etkileşimli Hive sorgularını çalıştırma işlemi yalnızca 2.3.0.0 veya sonraki sürümler tarafından desteklenir.

## <a name="connect-to-azure-subscriptions"></a>Azure aboneliklerine bağlanma
Visual Studio için Data Lake Araçları, HDInsight kümelerinizi bağlamanıza, bazı temel yönetim işlemlerini gerçekleştirmenize ve Hive sorguları çalıştırmanıza olanak sağlar.

> [!NOTE]
> Genel bir Hadoop kümesine bağlanma hakkında bilgi için bkz. [Visual Studio kullanarak Hive sorguları yazma ve gönderme](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).
> 
> 

**Azure aboneliğinize bağlanmak için**

1. Visual Studio'yu açın.
2. **Görünüm** menüsünde, **Sunucu Gezgini**’ne tıklayarak Sunucu Gezgini penceresini açın.
3. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.
   
   > [!NOTE]
   > **HDInsight Görev Listesi** penceresinin açık olduğuna dikkat edin. Görmüyorsanız, **Görünüm** menüsünde **Diğer Pencereler**’e tıklayın ve ardından **HDInsight Görev Listesi Penceresi**’ne tıklayın.  
   > 
   > 
4. Azure aboneliği kimlik bilgilerinizi girin ve ardından **Oturum Aç**’a tıklayın. Kimlik doğrulaması sadece daha önce bu istasyonunda Visual Studio’dan Azure aboneliğinize bağlanmadıysanız gerekir.
5. Sunucu Gezgini’nde, var olan HDInsight kümelerinin listesini görürsünüz. Kümeniz yoksa Azure portalı, Azure PowerShell veya HDInsight SDK’yı kullanarak bir küme oluşturabilirsiniz. Daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).
   
   ![Visual Studio için Data Lake Araçları Sunucu Gezgini küme listesi](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Visual Studio için Data Lake Araçları Sunucu Gezgini")
6. HDInsight kümesini genişletin. **Hive Veritabanları**, varsayılan depolama hesabı, bağlantılı depolama hesapları ve **Hadoop Hizmeti günlüğünü** görürsünüz. Varlıkları daha da genişletebilirsiniz.

Azure aboneliğinize bağlandıktan sonra aşağıdaki görevleri gerçekleştirebilirsiniz:

**Visual Studio'dan Azure portalına bağlanmak için**

* Sunucu Gezgini'nde, **Azure** > **HDInsight**’ı genişletin, HDInsight kümesine sağ tıklayın ve ardından **Kümeyi Azure portalında Yönet**’e tıklayın.

**Visual Studio ile ilgili soru sormak ve geri bildirim sağlamak için**

* **Araçlar** menüsünde, **HDInsight**’a ve ardından **MSDN Forumu**’na tıklayarak soru sorun ya da **Görüş bildirin**’e tıklayın.

## <a name="navigate-the-linked-resources"></a>Bağlı kaynaklara gitme
Sunucu Gezgini'nde, varsayılan depolama hesabını ve bağlı tüm depolama hesaplarını görebilirsiniz. Varsayılan depolama hesabını genişletirseniz, depolama hesabında kapsayıcıları görebilirsiniz. Varsayılan depolama hesabı ve varsayılan kapsayıcı işaretlenmiştir. Ayrıca içeriğini görüntülemek için kapsayıcılara sağ tıklayabilirsiniz.

![Visual Studio için Data Lake Araçları sunucu gezgini bağlı kaynakları listeleme](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "Bağlı kaynakları listeleme")

Bir kapsayıcıyı açtıktan sonra aşağıdaki düğmeleri kullanarak blob’ları karşıya yükleyebilir, silebilir ve indirebilirsiniz:

![Visual Studio için Data Lake Araçları sunucu gezgini blob işlemleri](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "Blob yükleme, silme ve indirme")

## <a name="run-interactive-hive-queries"></a>Etkileşimli Hive sorguları çalıştırma
[Apache Hive](http://hive.apache.org), veri özetleme, sorgu ve analiz sağlamaya yönelik, Hadoop'ta kurulu bir veri ambarı altyapısıdır. Visual Studio için Data Lake Araçları Visual Studio'dan Hive sorguları çalıştırmayı destekler. Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma](hdinsight-use-hive.md).

[Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md), Apache Hive 2.1’de [LLAP üzerinde LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) kullanır ve depolanmış büyük veri kümelerinde karmaşık veri ambarı stil sorgularınıza etkileşim katar. Etkileşimli Sorgu üzerinde Hive sorgularının çalıştırılması, geleneksel Hive toplu işlerine kıyasla çok daha hızlıdır.  Hive toplu işlerini çalıştırma hakkında daha fazla bilgi için bkz. [Hive toplu işleri çalıştırma](#run-hive-batch-jobs).

> [!note]
>
> Etkileşimli Hive sorgularının çalıştırılması yalnızca bir [HDInsight Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md) kümene bağlı olduğunuzda desteklenir.

Visual Studio için Data Lake Araçları ayrıca belirli Hive işlerine ait YARN günlüklerini toplayarak ve görünmesini sağlayarak Hive işinin içeriğini görmelerini sağlar.

### <a name="view-the-hivesampletable"></a>**hivesampletable** görüntüleme
Tüm HDInsight kümeleri *hivesampletable* adlı örnek bir Hive tablosuyla birlikte gelir  Hive tablolarını listeleme, tablo şemalarını görüntüleme ve Hive tablosundaki satırları listelemeyi size göstermek için bu Hive tablosu kullanılır.

**Hive tablolarını listelemek ve Hive tablo şemasını görüntülemek için**

1. **Sunucu Gezgini**’nde, tablo şemasını görmek için **Azure** > **HDInsight** > tercih ettiğiniz küme > **Hive Veritabanları** > **Varsayılan** > **hivesampletable** öğesini genişletin.
2. **hivesampletable** öğesine sağ tıklayın ve ardından satırları listelemek için **İlk 100 Satırı Görüntüle**’ye tıklayın. Bu, Hive ODBC sürücüsünü kullanarak aşağıdaki Hive sorgusunu çalıştırmaya eşdeğerdir:
   
     SELECT * FROM hivesampletable LIMIT 100
   
   Satır sayısını özelleştirebilirsiniz.
   
   ![Data Lake Araçları: HDInsight Hive Visual Studio şema sorgusu](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Hive sorgusu sonuçları")

### <a name="create-hive-tables"></a>Hive tabloları oluşturma
Bir Hive tablosu oluşturmak için GUI’yi kullanabilir ya da Hive sorgularını kullanabilirsiniz. Hive sorgularını kullanma hakkında daha fazla bilgi için bkz. [Hive sorgularını çalıştırma](#run.queries).

**Hive tablosu oluşturmak için**

1. **Sunucu Gezgini**’nde **Azure** > **HDInsight Kümeleri** HDInsight kümesi > **Hive Veritabanları**’nı genişletin ve sonra **varsayılan**’a sağ tıklayın ve **Tablo Oluştur**’a tıklayın.
2. Tabloyu yapılandırın.
3. Yeni Hive tablosu oluşturmak üzere işi göndermek için **Tablo Oluştur**’a tıklayın.
   
    ![Data Lake Araçları: HDInsight Visual Studio Araçları Hive tablosu oluşturma](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Hive tablosu oluşturma")

### <a name="run.queries"></a>Hive sorgularını çalıştırma ve doğrulama
Hive sorgularını çalıştırmak ve doğrulamak için iki yol vardır:

* Geçici sorgular oluşturma
* Hive uygulaması oluşturma

**Geçici sorgular oluşturmak, doğrulamak ve çalıştırmak için**

1. **Sunucu Gezgini**’nde **Azure** seçeneğini ve ardından **HDInsight Kümeleri** seçeneğini genişletin.
2. Sorguyu çalıştırmak istediğiniz yerde sağ tıklayın ve **Hive Sorgusu Yaz**’a tıklayın.
3. Hive sorgularını girin. Hive düzenleyicisinin IntelliSense’i desteklediğine dikkat edin. Visual Studio için Data Lake Araçları, Hive betiğinizi düzenlerken, uzak meta veri yüklenmesini destekler. Örneğin, "SELECT * FROM" yazdığınızda, IntelliSense önerilen tablo adlarını listeler. Bir tablo adı belirtildiğinde, sütun adları IntelliSense tarafından listelenir. Araçlar neredeyse tüm Hive DML deyimleri, alt sorguları ve yerleşik UDF'leri destekler.
   
    ![Data Lake Araçları: HDInsight Visual Studio Araçları IntelliSense](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![Data Lake Araçları: HDInsight Visual Studio Araçları IntelliSense](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > Yalnızca HDInsight Araç Çubuğunda seçilen kümelerin meta verileri önerilir.
   > 
   > 
4. (İsteğe bağlı) Betik söz dizimi hatalarını denetlemek için **Betiği Doğrula**’ya tıklayın.
   
    ![Data Lake Araçları: Visual Studio için Data Lake Araçları yerel doğrulama](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "Betik doğrulama")
5. **Gönder** veya **Gönder (Gelişmiş) gönderme** seçeneğine tıklayın. Gelişmiş gönderme seçeneği ile, betik için **İş Adı**, **Bağımsız Değişkenler**, **Ek Yapılandırmalar** ve **Durum Dizini**’ni yapılandırın:
   
    ![HDInsight Hadoop Hive sorgusu](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Sorgu gönderme")
   
    İşi gönderdikten sonra, gördüğünüz **Hive İşi Özeti** penceresini görürsünüz.
   
    ![Bir HDInsight Hadoop Hive sorgusunun özeti](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Hive iş özeti")
6. İş durumu **Tamamlandı** olarak değişinceye kadar, durumu güncelleştirmek için **Yenile** düğmesini kullanın.
7. Şunları görmek için alt kısımdaki bağlantılara tıklayın: **İş Sorgusu**, **İş Çıktısı**, **İş Günlüğü** veya **Yarn günlüğü**.

**Hive çözümü oluşturmak ve çalıştırmak için**

1. **DOSYA** menüsünde **Yeni**’ye ve sonra **Proje**’ye tıklayın.
2. Sol bölmede **HDInsight**’ı seçin, orta bölmede **Hive Uygulaması**’nı seçin, özellikleri girin ve **Tamam**’a tıklayın.
   
    ![Data Lake Araçları: HDInsight Visual Studio Araçları yeni Hive projesi](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Visual Studio'da Hive uygulamaları oluşturma")
3. **Çözüm Gezgini**’nde, açmak için **Script.hql** öğesine çift tıklayın.
4. Hive betiğini doğrulamak için, **Betiği Doğrula** düğmesine tıklayabilir veya Hive düzenleyicisinde betiğe sağ tıklayıp bağlam menüsünde **Betiği Doğrula**’ya tıklayabilirsiniz.

### <a name="view-hive-jobs"></a>Hive İşlerini Görüntüleme
Hive işleri için iş sorguları, iş çıktısı, iş günlükleri ve Yarn günlüklerini görüntüleyebilirsiniz. Daha fazla bilgi için önceki ekran görüntüsüne bakın.

Araçların en son sürümü YARN günlüklerini toplayarak ve görünmesini sağlayarak Hive işlerinizin içeriğini görmenizi sağlar. YARN günlüğü, performans sorunlarını araştırmanıza yardımcı olabilir. HDInsight'ın YARN günlüklerini nasıl topladığı hakkında daha fazla bilgi için bkz. [HDInsight Uygulama Günlüklerine Programlamayla Erişme](../hdinsight-hadoop-access-yarn-app-logs.md).

**Hive işlerini görüntülemek için**

1. **Sunucu Gezgini**’nde **Azure** seçeneğini ve ardından **HDInsight** seçeneğini genişletin.
2. HDInsight kümesine sağ tıklayın ve ardından **İşleri Görüntüle**’ye tıklayın. Küme üzerinde çalışan Hive işlerinin listesini görürsünüz.
3. Seçmek için iş listesindeki bir işe tıklayın ve sonra **Hive İşi Özet** penceresini kullanarak **İş Sorgusu**, **İş Çıktısı**, **İş Günlüğü** veya **Yarn günlüğü**’nü açın.
   
    ![Data Lake Araçları: HDInsight Visual Studio Araçları Hive işlerini görüntüleme](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Hive işlerini görüntüleme")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>HiveServer2 aracılığıyla daha hızlı Hive yürütme
> [!NOTE]
> Bu özellik yalnızca HDInsight kümesi sürüm 3.2 ve üstünde çalışır.
> 
> 

Hive işlerini [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (Templeton olarak da bilinir) aracılığıyla göndermek için kullanılan Data Lake Araçları. İş ayrıntılarını ve hata bilgilerini döndürmek uzun zaman aldı.
Bu performans sorununu çözmek için, Data Lake Araçları Hive işlerini doğrudan HiveServer2 aracılığıyla yürüterek RDP/SSH’yi atlar.
Daha iyi performansa ek olarak, kullanıcılar aynı zamanda Tez grafiklerinde Hive ve Görev ayrıntılarını görüntüleyebilir 

HDInsight kümesi sürüm 3.2 veya sonraki sürümlerinde **HiveServer2 aracılığıyla yürüt** düğmesi görebilirsiniz:

![Data Lake Visual Studio Araçları HiveServer2 aracılığıyla yürütme](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "HiveServer2 kullanarak Hive sorguları yürütme")

Ayrıca, gerçek zamanlı olarak geri akışlı günlükleri görebilir ve Hive sorgusu Tez’de yürütülürse iş grafiklerini görebilirsiniz.

![Data Lake Visual Studio Araçları hızlı yol Hive yürütme](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "İş grafiklerini görüntüleme")

**Sorguları HiveServer2 aracılığıyla yürütme ile Sorguları WebHCat aracılığıyla Gönderme arasındaki fark**

Sorguları HiveServer2 aracılığıyla yürütmenin birçok performans avantajı olmasına rağmen, bazı kısıtlamaları vardır. Bazı sınırlamaları üretim kullanımına uygun değildir. Aşağıdaki tabloda farklar gösterilmektedir:

|  | HiveServer2 aracılığıyla yürütme | WebHCat aracılığıyla gönderme |
| --- | --- | --- |
| Sorguları yürütme |WebHCat’te ek yükü ortadan kaldırır (“TempletonControllerJob” adlı bir MapReduce İşi çalıştıran). |Bir sorgu WebHCat aracılığıyla yürütüldüğü sürece, WebHCat ek gecikme sağlayan bir MapReduce işi başlatır. |
| Geriye akış günlükleri |Neredeyse gerçek zamanlı olarak. |Yalnızca iş tamamlandığında, iş yürütme günlüklerini kullanılabilir. |
| İş geçmişini görüntüleme |Bir sorgu HiveServer2 aracılığıyla yürütülürse, buna ait iş geçmişi (iş günlüğü, iş çıktısı) korunmaz. Uygulama YARN kullanıcı arabiriminde, sınırlı bilgiyle görüntülenebilir. |Bir sorgu WebHCat aracılığıyla yürütülürse, buna ait iş geçmişi (iş günlüğü, iş çıktısı) korunur ve Visual Studio/HDInsight SDK/PowerShell kullanarak görüntülenebilir. |
| Pencereyi kapatma |HiveServer2 aracılığıyla yürütme işlemi "zaman uyumlu" bir yöntem olduğundan, pencereleri açık tutmalısınız; pencereler kapatılırsa, sorgu yürütme işlemi iptal edilir. |WebHCat aracılığıyla yürütme işlemi "zaman uyumsuz" bir yöntemdir, bu nedenle sorguyu WebHCat aracılığıyla gönderebilir ve Visual Studio’yu kapatabilirsiniz. İstediğiniz zaman geri dönüp sonuçlara bakabilirsiniz. |

### <a name="tez-hive-job-performance-graph"></a>Tez Hive işi performans grafiği
Data Lake Araçları, Tez yürütme altyapısı tarafından çalıştırılan Hive işleri için performans grafikleri göstermeyi destekler. Tez'i etkinleştirme hakkında daha fazla bilgi için bkz. [HDInsight'ta Hive kullanma](hdinsight-use-hive.md). Visual Studio'da bir Hive işi gönderdikten sonra, iş tamamlandığında Visual Studio size grafiği gösterir.  En son iş durumunu almak için **Yenile** düğmesine tıklamanız gerekebilir.

> [!NOTE]
> Bu özellik yalnızca HDInsight kümesini 3.2.4.593 sürümünün üstü için geçerlidir ve sadece tamamlanan işler için kullanılabilir (işinizi WebHCat aracılığıyla gönderdiyseniz, bu grafik sorgunuzu ne zaman yürüttüğünüzü HiveServer2 aracılığıyla gösterir). 
> 
> 

![Hadoop Hive Tez performans grafiği](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "İş durumu")

Hive sorgunuzu daha iyi anlamanıza yardımcı olması için, araçların bu sürümüne Hive İşleci görünümü eklenmiştir. İş grafiğinin köşeleri içindeki tüm işleçleri görmek için köşe noktalarına çift tıklamanız yeterlidir. Bu işlece ilişkin diğer ayrıntıları görüntülemek için belirli bir işlecin üzerine de gelebilirsiniz.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Tez işlerinde Hive için Görev yürütme
Tez işlerinde Hive için Görev yürütme, Hive işleri için yapılandırılmış ve görselleştirilmiş bilgi ve diğer iş ayrıntılarını almak üzere kullanılabilir. Performans sorunları olduğunda, daha fazla bilgi almak için görünümü kullanabilirsiniz. Örneğin, her bir görevin nasıl çalıştığı ve her bir göre hakkında ayrıntılı bilgi (veri okuma/yazma, zamanlaması/başlangıç/bitiş zamanı vb.), böylece görselleştirilmiş bilgiler temelinde iş yapılandırmalarını veya sistem mimarisini ayarlayabilirsiniz.

![Data Lake Visual Studio Araçları görev yürütme görünümü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Görev yürütme görünümü")

## <a name="run-hive-batch-jobs"></a>Hive toplu işleri çalıştırma
[Apache Hive](http://hive.apache.org), veri özetleme, sorgu ve analiz sağlamaya yönelik, Hadoop'ta kurulu bir veri ambarı altyapısıdır. Visual Studio için Data Lake Araçları Visual Studio'dan Hive sorguları çalıştırmayı destekler. Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma](hdinsight-use-hive.md).

Hive betiğinin Etkileşimli Sorgu kümesi dışında bir HDInsight kümesine göre test edilmesi uzun süren bir işlemdir. Birkaç dakika veya daha fazla sürebilir. Visual Studio için Data Lake Araçları dinamik bir kümeye bağlanmadan Hive betiğini yerel olarak doğrulayabilir. Etkileşimli sorgular çalıştırma hakkında daha fazla bilgi için bkz. [Etkileşimli Hive sorguları çalıştırma](#run-interactive-hive-queries).

Visual Studio için Data Lake Araçları ayrıca belirli Hive işlerine ait YARN günlüklerini toplayarak ve görünmesini sağlayarak Hive işinin içeriğini görmelerini sağlar.

Hive toplu işleri çalıştırma hakkında daha fazla bilgi için [etkileşimli Hive sorguları çalıştırma](#run-interactive-hive-queries) bölümüne bakın. Bu bölümdeki bilgiler, daha uzun süreli Hive toplu işlerini çalıştırma için geçerlidir.

## <a name="run-pig-scripts"></a>Pig betikleri çalıştırma
Visual Studio için Data Lake Araçları, Pig betikleri oluşturmayı ve HDInsight kümelerine göndermeyi destekler. Kullanıcılar, şablonu kullanarak bir Pig proje şablonu oluşturabilir ve sonra betiği HDInsight kümelerine gönderebilir.

## <a name="feedbacks--known-issues"></a>Geribildirimler ve Bilinen sorunlar
* Şu anda, HiveServer2 sonuçlar ideal olmayan salt metin biçiminde görüntülenir. Microsoft bunu düzeltmek için çalışmaktadır.
* Sonuçlar NULL değerler ile başladıysa, şu anda sonuçlar gösterilmez. Bu sorunu düzelttik, ancak bu sorunda takıldıysanız destek ekibine başvurabilirsiniz.
* Visual Studio tarafından oluşturulan HQL betiği kullanıcının yerel bölge ayarlarına bağlı olarak kodlanır. Kullanıcı betiği ikili olarak kümeye yüklerse, doğru şekilde yürütülmeyebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Data Lake (HDInsight) Araçları paketini kullanarak Visual Studio’dan HDInsight kümelerine bağlanmayı ve Hive sorgusu çalıştırmayı öğrendiniz. Daha fazla bilgi için bkz.

* [HDInsight'ta Hadoop Hive kullanma](hdinsight-use-hive.md)
* [HDInsight'ta Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight'ta Hadoop işlerini gönderme](submit-apache-hadoop-jobs-programmatically.md)
* [HDInsight'ta Hadoop ile Twitter verilerini çözümleme](../hdinsight-analyze-twitter-data.md)

