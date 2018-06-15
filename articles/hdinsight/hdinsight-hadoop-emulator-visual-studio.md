---
title: Data Lake araçları Hortonworks Sandbox - Azure Hdınsight ile Visual Studio için | Microsoft Docs
description: Azure Data Lake araçları Visual Studio için yerel bir VM'de çalıştıran Hortonworks sandbox ile nasıl kullanacağınızı öğrenin. Bu araçları ile oluşturun ve Hive veya Pig işleri korumalı alan ve görünüm iş çıktısı ve geçmiş çalıştırın.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: e3434c45-95d1-4b96-ad4c-fb59870e2ff0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: larryfr
ms.openlocfilehash: a4c1f5a8100d5d4017e56ef129aa4f4826746868
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33886740"
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a>Hortonworks Sandbox ile Visual Studio için Azure Data Lake araçları kullanın

Azure Data Lake genel Hadoop kümeleri ile çalışma araçları içerir. Bu belge, yerel bir sanal makinede çalışan Data Lake araçları Hortonworks Sandbox ile kullanmak için gerekli adımları sağlar.

Hortonworks Sandbox kullanarak Hadoop ile yerel olarak geliştirme ortamınızı üzerinde çalışmanıza olanak sağlar. Bir çözümü geliştirilmiştir ve ölçekte dağıtmak istediğiniz sonra bir Hdınsight kümesine taşıyabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Hortonworks geliştirme ortamınızı bir sanal makinede çalışan korumalı alan. Bu belge yazılmış ve Oracle VirtualBox çalıştıran korumalı alan ile test. Korumalı alan ayarlama hakkında daha fazla bilgi için bkz: [Hortonworks sandbox ile başlayın.](hadoop/apache-hadoop-emulator-get-started.md) Belge.

* Visual Studio 2013, Visual Studio 2015 ve Visual Studio 2017 (herhangi bir sürümünü).

* [.NET için Azure SDK](https://azure.microsoft.com/downloads/) 2.7.1 veya üzeri.

* [Visual Studio için Azure Data Lake Araçları](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="configure-passwords-for-the-sandbox"></a>Korumalı alan için parolaları yapılandırın

Hortonworks Sandbox çalıştığından emin olun. Ardından adımları [Hortonworks korumalı alanda başlamak](hadoop/apache-hadoop-emulator-get-started.md#set-sandbox-passwords) belge. Bu adımları SSH için parolayı yapılandırmak `root` hesabı ve Ambari `admin` hesabı. Bu parolalar, Visual Studio'dan korumalı alanda bağlandığınızda kullanılır.

## <a name="connect-the-tools-to-the-sandbox"></a>Korumalı alan için Araçlar Bağlan

1. Visual Studio'ni açın, **Görünüm**ve ardından **Sunucu Gezgini**.

2. Gelen **Sunucu Gezgini**, sağ **Hdınsight** giriş ve ardından **Hdınsight öykünücüsünde Bağlan**.

    ![Ekran görüntüsü, Sunucu Gezgini, Hdınsight öykünücüsünde Bağlan ile vurgulanan](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. Gelen **Hdınsight öykünücüsünde Bağlan** iletişim kutusunda, Ambari için yapılandırdığınız parolayı girin.

    ![Vurgulanan parola metin kutusu ile iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    Devam etmek için **İleri**’yi seçin.

4. Kullanım **parola** için yapılandırılmış parola girmesini alan `root` hesabı. Diğer alanları varsayılan değeri bırakın.

    ![Vurgulanan parola metin kutusu ile iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    Devam etmek için **İleri**’yi seçin.

5. Bitirmek için Hizmetleri doğrulamasının tamamlanmasını bekleyin. Bazı durumlarda, doğrulama başarısız ve yapılandırmasını güncelleştirmek ister. Doğrulama başarısız olursa, seçin **güncelleştirme**ve yapılandırma ve doğrulama hizmeti tamamlanması için bekleyin.

    ![Güncelleştirme düğmesi vurgulanan iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > Güncelleştirme işlemi, Visual Studio için Data Lake araçları tarafından beklenenle için Hortonworks Sandbox yapılandırmasını değiştirmek için Ambari kullanır.

6. Doğrulama tamamlandıktan sonra seçin **son** yapılandırmayı tamamlamak için.
    ![Bitiş düğmesi vurgulanan iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)

     >[!NOTE]
     > Geliştirme ortamınızı ve sanal makineye ayrılan bellek miktarını hızına bağlı olarak yapılandırın ve Hizmetleri doğrulamak için birkaç dakika sürebilir.

Bu adımları uyguladıktan sonra artık elinizde bir **Hdınsight yerel küme** Sunucu Gezgini girişi altında **Hdınsight** bölümü.

## <a name="write-a-hive-query"></a>Bir Hive sorgusu Yaz

Hive yapılandırılmış verilerle çalışmak için bir SQL benzeri sorgu dili (HiveQL) sağlar. Yerel küme karşı isteğe bağlı sorguları çalıştırmak öğrenmek için aşağıdaki adımları kullanın.

1. İçinde **Sunucu Gezgini**, daha önce eklediğiniz yerel küme girişini sağ tıklatın ve ardından **Hive sorgusu yaz**.

    ![Ekran görüntüsü, Sunucu Gezgini, yazma vurgulanmış Hive sorgusu ile](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    Yeni bir sorgu penceresi görüntülenir. Burada hızla yapabilecekleriniz yazma ve yerel küme için bir sorgu gönderin.

2. Yeni Sorgu penceresinde aşağıdaki komutu girin:

        select count(*) from sample_08;

    Sorguyu çalıştırmak için seçin **gönderme** pencerenin üstündeki. Diğer değerleriyle bırakın (**toplu** ve sunucu adı) varsayılan değerleri.

    ![Gönder düğmesi vurgulanan Sorgu penceresinin ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    Yanında aşağı açılan menüsünü kullanabilirsiniz **gönderme** seçmek için **Gelişmiş**. Gelişmiş seçenekleri, iş gönderdiğinizde ek seçenekler sağlar olanak tanır.

    ![Gönderme betik ekran iletişim kutusu](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. Sorgu gönderdikten sonra iş durumu görüntülenir. İş durumu Hadoop tarafından işlenen iş hakkındaki bilgileri görüntüler. **İş durumu** işinin durumunu sağlar. Durumu düzenli aralıklarla güncelleştirilir veya durumu el ile yenilemek için Yenile simgesine kullanabilirsiniz.

    ![İş vurgulanmış durumu ile iş görünümünde ekran iletişim kutusu](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    Sonra **iş durumu** değişikliklerini **tamamlandı**, yönlendirilmiş Çevrimsiz grafik (DAG) görüntülenir. Bu diyagramda tarafından Tez Hive sorgusu işlenirken belirlendi yürütme yolu açıklanmaktadır. Tez Hive için varsayılan yürütme altyapısı yerel kümedeki ' dir.

    > [!NOTE]
    > Linux tabanlı Hdınsight kümelerini kullanırken Tez de varsayılandır. Varsayılan Windows tabanlı Hdınsight üzerinde değil. Kullanmak için satırın eklemelisiniz `set hive.execution.engine = tez;` Hive sorgunuzu başlangıcına.

    Kullanım **iş çıktısı** çıkışı görüntülemek için bağlantı. Bu durumda, bu 823, sample_08 tablodaki satır sayısını gösterir. Kullanarak işle ilgili tanılama bilgileri görüntüleyebilirsiniz **iş günlüğü** ve **karşıdan YARN günlüğü** bağlantılar.

4. Ayrıca Hive işleri etkileşimli olarak değiştirerek çalıştırabilirsiniz **toplu** alanı **etkileşimli**. Ardından **yürütme**.

    ![Vurgulanan etkileşimli ekran ve yürütme düğmeleri](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    Etkileşimli bir sorgu için işleme sırasında oluşturulan çıktı günlüğünü akışları **HiveServer2 çıkış** penceresi.

    > [!NOTE]
    > Kullanılabilir aynı bilgilerdir **iş günlüğü** bir işi tamamlandıktan sonra bağlantı.

    ![Çıktı oturum ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a>Bir Hive projesi oluşturma

Ayrıca, birden çok Hive betiği içeren bir proje oluşturabilirsiniz. Komut dosyaları ilgili ya da bir sürüm denetim sisteminde komut dosyalarını depolamak istediğiniz zaman bir proje kullanın.

1. Visual Studio'da seçin **dosya**, **yeni**ve ardından **proje**.

2. Projeleri listesinden genişletin **şablonları**, genişletin **Azure Data Lake**ve ardından **HIVE (Hdınsight)**. Şablonları listesinden seçin **örnek Hive**. Bir ad ve konum girin ve ardından **Tamam**.

    ![Yeni Proje penceresinin ekran penceresiyle Azure Data Lake, HIVE, örnek Hive ve Tamam'ı vurgulanmış](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

**Örnek Hive** projeyi içeren iki komut **WebLogAnalysis.hql** ve **SensorDataAnalysis.hql**. Bu komut dosyaları aynı kullanarak gönderebilirsiniz **gönderme** pencerenin üstündeki düğmesi.

## <a name="create-a-pig-project"></a>Pig proje oluşturma

Hive yapılandırılmış verilerle çalışmak için SQL benzeri bir dille sağlarken, Pig verileri dönüşümleri gerçekleştirerek çalışır. Pig bir ardışık düzen Dönüşümlerin geliştirmek izin veren bir dil (Pig Latin) sağlar. Yerel kümeyle pig kullanmak için şu adımları izleyin:

1. Visual Studio'yu açın ve seçin **dosya**, **yeni**ve ardından **proje**. Projeleri listesinden genişletin **şablonları**, genişletin **Azure Data Lake**ve ardından **Pig (Hdınsight)**. Şablonları listesinden seçin **Pig uygulama**. Konum, bir ad girin ve ardından **Tamam**.

    ![Yeni Proje penceresinin ekran penceresiyle Azure Data Lake, Pig, Pig uygulama ve Tamam'ı vurgulanmış](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. İçeriği aşağıdaki metni girin **script.pig** bu proje ile oluşturulmuş dosyası.

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

    Pig, Hive farklı bir dil kullanıyor, ancak işler çalışma şeklini iki diller arasında tutarlıdır aracılığıyla **gönderme** düğmesi. Yanındaki açılan seçme **gönderme** Pig için Gelişmiş Gönder iletişim kutusu görüntüler.

    ![Gönderme betik ekran iletişim kutusu](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. İş durumunu ve çıkışını de görüntülenir, Hive sorgusu ile aynı.

    ![Tamamlanan Pig işi ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a>İşleri görüntüle

Data Lake araçları de kolayca Hadoop üzerinde çalışan işleri hakkındaki bilgileri görüntülemek olanak sağlar. Yerel kümede çalışan işleri görmek için aşağıdaki adımları kullanın.

1. Gelen **Sunucu Gezgini**yerel kümeye sağ tıklayın ve ardından **işleri görüntüle**. Kümesine gönderilen işler listesi görüntülenir.

    ![Ekran görüntüsü, Sunucu Gezgini, ile vurgulanan işleri görüntüle](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. İşlerini listesinden bir iş ayrıntılarını görüntülemek için seçin.

    ![Ekran görüntüsü, iş tarayıcıyla, vurgulanmış işlerden biri](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    Görüntülenen bilgiler, çıktı görüntülemek ve bilgileri günlüğe kaydetmek için bağlantılar dahil olmak üzere bir Hive veya Pig sorgu çalıştırdıktan sonra gördüğünüz için benzer.

3. Ayrıca, değiştirebilir ve buradan işi yeniden gönderin.

## <a name="view-hive-databases"></a>Hive veritabanları görüntüleyin

1. İçinde **Sunucu Gezgini**, genişletin **Hdınsight yerel küme** girişi genişletin ve ardından **Hive veritabanları**. **Varsayılan** ve **xademo** veritabanları yerel kümedeki görüntülenir. Bir veritabanını genişletmeden veritabanının içindeki tabloları gösterir.

    ![Sunucu Gezgini ekran veritabanları ile genişletilmiş](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. Bir tablo genişletme tablosunun sütunlarını görüntüler. Verileri hızlı bir şekilde görüntülemek için bir tabloyu sağ tıklatın ve seçin **ilk 100 satırı görüntüle**.

    ![Ekran görüntüsü, Sunucu Gezgini, Genişletilmiş Tablo ve Görünüm ilk 100 seçilen satırı ile](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a>Veritabanı ve tablo özellikleri

Bir veritabanı veya tablo özelliklerini görüntüleyebilirsiniz. Seçme **Özellikler** Özellikler penceresinde seçili öğe için ayrıntılarını görüntüler. Örneğin, aşağıdaki ekran görüntüsünde gösterilen bilgiler bakın:

![Ekran görüntüsü, Özellikler penceresi](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a>Bir tablo oluşturma

Bir tablo oluşturmak için bir veritabanına sağ tıklayın ve ardından **Create Table**.

![Ekran görüntüsü, Sunucu Gezgini, ile vurgulanan Tablo Oluştur](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

Daha sonra bir form kullanarak tablosu oluşturabilirsiniz. Aşağıdaki ekran görüntüsünde alt kısmındaki tablo oluşturmak için kullanılan ham HiveQL görebilirsiniz.

![Bir tablo oluşturmak için kullanılan formu ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Hortonworks Sandbox ipleri öğrenme](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop Öğreticisi - HDP ile çalışmaya başlama](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
