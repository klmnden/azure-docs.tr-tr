---
title: Hortonworks korumalı alanı - Azure HDInsight ile Visual Studio için Data Lake araçları
description: Azure Data Lake araçları, yerel bir VM'de çalışan Hortonworks korumalı alanı ile Visual Studio için kullanmayı öğrenin. Bu araçlarla oluşturun ve korumalı alan ve görünüm iş çıktısının ve geçmiş Hive ve Pig işleri çalıştırma.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: hrasheed
ms.openlocfilehash: 3286ca3b9c85236ff322eb19324bc5ac7a904e22
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65605442"
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a>Hortonworks korumalı alanı ile Visual Studio için Azure Data Lake Araçları'nı kullanın

Azure Data Lake genel Apache Hadoop kümeleriyle çalışmaya yönelik araçlar içerir. Bu belgede, yerel bir sanal makinede çalışan Hortonworks korumalı alanı ile Data Lake araçları kullanmak için atılması gereken adımları sağlar.

Hortonworks korumalı alanı kullanarak Hadoop ile yerel olarak üzerinde geliştirme ortamınızı çalışmanıza olanak sağlar. Bir HDInsight kümesine, sonra çözüm geliştirmiş ve ölçekli olarak dağıtmak istediğiniz sonra taşıyabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Hortonworks üzerinde geliştirme ortamınızı bir sanal makinede çalışan korumalı alanı. Bu belge yazılan ve Oracle VirtualBox içinde çalışan korumalı alanı ile test. Korumalı alan ayarlama hakkında daha fazla bilgi için bkz. [Hortonworks sandbox ile kullanmaya başlayın.](hadoop/apache-hadoop-emulator-get-started.md) Belge.

* Visual Studio.

* [.NET için Azure SDK'sı](https://azure.microsoft.com/downloads/) 2.7.1 veya üzeri.

* [Visual Studio için Azure Data Lake Araçları](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="configure-passwords-for-the-sandbox"></a>Korumalı alan için parolaları yapılandırın

Hortonworks korumalı alanı çalıştığından emin olun. Ardından adımları [Hortonworks korumalı alanı içinde kullanmaya başlayın](hadoop/apache-hadoop-emulator-get-started.md#set-sandbox-passwords) belge. Bu adımları, SSH için parolayı yapılandırmak `root` hesabı ve Apache Ambari `admin` hesabı. Bu parolalar, korumalı alan Visual Studio'dan bağlandığınızda kullanılır.

## <a name="connect-the-tools-to-the-sandbox"></a>Korumalı alan araçları bağlanma

1. Visual Studio'yu açın, **görünümü**ve ardından **Sunucu Gezgini**.

2. Gelen **Sunucu Gezgini**, sağ **HDInsight** giriş tıklayın ve ardından **HDInsight öykünücüsü Bağlan**.

    ![Vurgulanan Gezgini'nin ekran görüntüsü Server ile HDInsight öykünücüsü Bağlan](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. Gelen **HDInsight öykünücüsü Bağlan** iletişim kutusunda, Ambari için yapılandırdığınız parolayı girin.

    ![Parola metin kutusu vurgulanmış iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    Devam etmek için **İleri**’yi seçin.

4. Kullanım **parola** için yapılandırdığınız parolayı girmek için alan `root` hesabı. Diğer alanları varsayılan değerde bırakın.

    ![Parola metin kutusu vurgulanmış iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    Devam etmek için **İleri**’yi seçin.

5. Doğrulama hizmetleri tamamlanması için bekleyin. Bazı durumlarda, doğrulama başarısız ve yapılandırmasını güncelleştirmek için ister. Doğrulama başarısız olursa, seçin **güncelleştirme**, yapılandırma ve doğrulama hizmeti tamamlanması için bekleyin.

    ![Güncelleştirme düğmesi vurgulanan iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]  
    > Güncelleştirme işlemi, Visual Studio için Data Lake araçları tarafından beklenen Hortonworks korumalı alanı yapılandırmasını değiştirmek için Ambari kullanır.

6. Doğrulama tamamlandıktan sonra seçin **son** yapılandırmayı tamamlamak için.
    ![Son düğmesi vurgulanan iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)

     >[!NOTE]  
     > Geliştirme ortamı ve sanal makineye ayrılan bellek miktarını hızına bağlı olarak, uygulamanın yapılandırma ve doğrulama hizmetleri için birkaç dakika sürebilir.

Bu adımları uyguladıktan sonra artık sahip olduğunuz bir **HDInsight yerel küme** Sunucu Gezgini giriş altında **HDInsight** bölümü.

## <a name="write-an-apache-hive-query"></a>Apache Hive sorgusu Yaz

Hive, yapılandırılmış verilerle çalışmaya yönelik bir SQL benzeri bir sorgu dili (HiveQL) sağlar. Yerel kümede isteğe bağlı sorguları çalıştırma hakkında bilgi edinmek için aşağıdaki adımları kullanın.

1. İçinde **Sunucu Gezgini**, daha önce eklemiş olduğunuz yerel küme için girişe sağ tıklayın ve ardından **Hive sorgusu yaz**.

    ![Gezgini'nin ekran görüntüsü sunucusu ile vurgulanmış bir Hive sorgusu Yaz](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    Yeni bir sorgu penceresi görüntülenir. Hızla yapabilirsiniz buradan yazma ve sorgu yerel kümeye gönderme.

2. Yeni Sorgu penceresinde aşağıdaki komutu girin:

        select count(*) from sample_08;

    Sorguyu çalıştırmak için seçin **Gönder** pencerenin üst kısmındaki. Diğer değerleri bırakın (**Batch** ve sunucu adı) varsayılan değerlerinde.

    ![Gönder düğmesi ile sorgu penceresinin ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    Aşağı açılan menüsünün yanındaki kullanabilirsiniz **Gönder** seçilecek **Gelişmiş**. Gelişmiş seçenekleri, işi gönderdiğinizde, ek seçenekler sağlar olanak tanır.

    ![Ekran görüntüsü, betiği Gönder iletişim kutusu](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. Sorgu gönderdikten sonra iş durumu görüntülenir. Hadoop tarafından işlendiği iş durumunu işle ilgili bilgileri görüntüler. **İş durumu** işinin durumunu sağlar. Durumu düzenli olarak güncelleştirilir ve durumu el ile yenilemek için yenile simgesini kullanın.

    ![İş görünümü ekran iletişim kutusunda vurgulanan iş durumu](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    Sonra **iş durumu** değişikliklerini **tamamlandı**, yönlendirilmiş Çevrimsiz graf (DAG) görüntülenir. Bu şema tarafından Tez Hive sorgusu işlenirken belirlendi yürütme yolunu açıklar. Tez Hive için varsayılan yürütme altyapısı yerel kümedeki ' dir.

    > [!NOTE]  
    > Linux tabanlı HDInsight kümeleri kullanılırken Apache Tez ayrıca varsayılandır. Varsayılan olarak Windows tabanlı HDInsight değil. Bunu kullanmak için satırın eklemelisiniz `set hive.execution.engine = tez;` Hive sorgunuzu başlangıcına.

    Kullanım **iş çıktısı** çıkışı görüntülemek için bağlantı. Bu durumda, 823, sample_08 tablodaki satır sayısını gösterir. İşle ilgili tanılama bilgilerini kullanarak görüntüleyebileceğiniz **iş günlüğü** ve **YARN günlüğünü indirmek** bağlantıları.

4. Ayrıca Hive işlerini etkileşimli olarak değiştirerek çalıştırabileceğiniz **Batch** alanı **etkileşimli**. Ardından **yürütme**.

    ![Vurgulanan etkileşimli ekran görüntüsü ve yürütme düğmeleri](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    Etkileşimli bir sorgu işleme sırasında üretilen çıkış günlüğü akışları **HiveServer2 çıkış** penceresi.

    > [!NOTE]  
    > Kullanılabilir aynı bilgilerdir **iş günlüğü** bir işi tamamlandıktan sonra bağlantı.

    ![Çıkış günlüğü görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a>Bir Hive projesi oluşturma

Ayrıca, birden çok Hive betiklerini içeren bir proje oluşturabilirsiniz. İlgili betikleri veya bir sürüm denetim sisteminde betikleri depolamak istediğiniz proje kullanırsınız.

1. Visual Studio'da **dosya**, **yeni**, ardından **proje**.

2. Projelerin listesinden genişletin **şablonları**, genişletme **Azure Data Lake**ve ardından **HIVE (HDInsight)** . Şablonlar listesinden **örnek Hive**. Bir ad ve konum girin ve ardından **Tamam**.

    ![Yeni Proje ekran penceresiyle Azure Data Lake, HIVE, örnek Hive ve Tamam'ı vurgulanmış](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

**Örnek Hive** projesini içeren iki komut **WebLogAnalysis.hql** ve **SensorDataAnalysis.hql**. Bu komut dosyalarını aynı kullanarak gönderebilirsiniz **Gönder** pencerenin üstünde düğme.

## <a name="create-an-apache-pig-project"></a>Apache Pig projesi oluşturun

Pig, Hive, yapılandırılmış verilerle çalışmak için SQL benzeri bir dil sağlarken, verilerinizde dönüşümler gerçekleştirerek çalışır. Pig, bir işlem hattı dönüşümlerinin geliştirmenize olanak veren bir dili (Pig Latin) sağlar. Yerel küme ile pig kullanma için bu adımları izleyin:

1. Visual Studio'yu açın ve seçin **dosya**, **yeni**, ardından **proje**. Projelerin listesinden genişletin **şablonları**, genişletme **Azure Data Lake**ve ardından **Pig (HDInsight)** . Şablonlar listesinden **Pig uygulama**. Konum, bir ad girin ve ardından **Tamam**.

    ![Yeni Proje ekran penceresiyle Azure Data Lake, Pig, Pig uygulama ve Tamam'ı vurgulanmış](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. Aşağıdaki metni içeriğini girin **script.pig** bu projeyle oluşturulan dosya.

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    Pig, Hive farklı bir dil kullanırken, işlerin nasıl çalıştırdığınız her iki dil arasında tutarlı aracılığıyla **Gönder** düğmesi. Yanındaki açılan seçerek **Gönder** için Pig bir Gelişmiş Gönder iletişim kutusu görüntüler.

    ![Ekran görüntüsü, betiği Gönder iletişim kutusu](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. İş durumunu ve çıkışını de görüntülenir, Hive sorgusu ile aynı.

    ![Tamamlanmış bir Pig işi ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a>İşleri görüntüle

Data Lake araçları kolayca, Hadoop üzerinde çalışan işleri hakkında bilgileri görüntülemenizi sağlar. Yerel küme üzerinde çalıştırılır işleri görmek için aşağıdaki adımları kullanın.

1. Gelen **Sunucu Gezgini**yerel kümeye sağ tıklayın ve ardından **işleri görüntüle**. Kümesine gönderilen işler listesi görüntülenir.

    ![Gezgini'nin ekran görüntüsü sunucusu ile vurgulanmış işleri görüntüle](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. İşleri listesinden bir iş ayrıntılarını görüntülemek için seçin.

    ![Ekran görüntüsü, iş, tarayıcı vurgulanmış işlerden biri](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    Çıkışı görüntülemek ve bilgi günlük bağlantılar dahil olmak üzere bir Hive veya Pig sorgu çalıştırdıktan sonra gördüğünüz üzere görüntülenen bilgileri benzerdir.

3. Ayrıca, değiştirebilir ve buradan işi yeniden gönderin.

## <a name="view-hive-databases"></a>Hive veritabanları görüntüleyin

1. İçinde **Sunucu Gezgini**, genişletme **HDInsight yerel küme** girişi ve ardından **Hive veritabanları**. **Varsayılan** ve **xademo** veritabanları alan yerel kümede görüntülenir. Bir veritabanını genişletmek, veritabanındaki tabloları gösterir.

    ![Sunucu Gezgini ekran veritabanları ile genişletilmiş](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. Bir tablo genişleterek bu tablonun sütunlarını görüntüler. Verileri hızlıca görüntülemek için bir tablo sağ tıklatın ve seçin **ilk 100 satırı görüntüle**.

    ![Gezgini'nin ekran görüntüsü sunucu Genişletilmiş Tablo ve Görünüm ilk 100 satırı seçili](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a>Veritabanı ve tablo özellikleri

Bir veritabanı veya tablo özelliklerini görüntüleyebilirsiniz. Seçme **özellikleri** Özellikler penceresinde seçilen öğenin ayrıntılarını görüntüler. Örneğin, aşağıdaki ekran görüntüsünde gösterilen bilgiler bakın:

![Ekran görüntüsü, Özellikler penceresi](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a>Bir tablo oluşturma

Bir tablo oluşturmak için bir veritabanına sağ tıklayın ve ardından **Create Table**.

![Gezgini'nin ekran görüntüsü sunucusu ile vurgulanmış Tablo Oluştur](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

Ardından, formu kullanarak tablo oluşturabilirsiniz. Aşağıdaki ekran görüntüsünde sayfanın en tablo oluşturmak için kullanılan ham HiveQL görebilirsiniz.

![Bir tablo oluşturmak için kullanılan biçiminde ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Hortonworks korumalı alanı işin öğrenme](https://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Apache Hadoop Öğreticisi - HDP ile çalışmaya başlama](https://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
