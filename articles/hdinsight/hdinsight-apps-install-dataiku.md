---
title: Yayımlanan uygulama - Dataiku DDS - Azure HDInsight yükleme
description: Yükleyip Dataiku DDS üçüncü taraf Apache Hadoop uygulamasını kullanabilirsiniz.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 5d2f803414db3bd118f57d317cfc32c8f22fcd5a
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2018
ms.locfileid: "53653264"
---
# <a name="install-published-application---dataiku-dds"></a>Yayımlanan uygulama - Dataiku DDS yükleme

Bu makalede, yüklemek ve çalıştırmak açıklanır [Dataiku DDS](https://www.dataiku.com/) yayımlanan [Apache Hadoop](https://hadoop.apache.org/) Azure HDInsight uygulaması. HDInsight uygulama platformu için genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için bkz. yayımlanan uygulamalar [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-dataiku-dss"></a>Dataiku DSS hakkında

Dataiku [Data Science Studio'yu (DSS)](https://www.dataiku.com/dss/features/connectivity/), oluşturmak ve analitik çözümler sunmak veri bilimcilerine sağlayan işbirliğine dayalı bir veri bilimi platformu olan. Bir HDInsight olarak DSS sunarak uygulama, büyük veri çözümleri oluşturmanıza ve bunları kurumsal sınıf ve ölçek çalıştırmak için veri bilimi kullanmanızı sağlar.

DSS, hazırlama, veri alımı ile başlayan ve işleme eksiksiz bir analiz çözümü uygulamak için kullanabilirsiniz. DSS çözüm, eğitim ve uygulama makine öğrenimi modelleri, Görselleştirme ve sonra faaliyete de içerebilir.

DSS HDInsight kullanarak yükleyebileceğiniz [Apache Hadoop](https://hadoop.apache.org/) veya [Apache Spark](https://spark.apache.org/) kümeleri. Yeni bir küme oluştururken, mevcut çalışan kümelerinde veya DSS yükleyebilirsiniz. DSS, veri okumak için bir bağlayıcı olarak Azure Blob depolamayı kullanarak da destekler.

DSS sonra oluşturabilirsiniz projeleri oluşturmak için kullanabileceğiniz [MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) veya Spark işleri. İsteğe bağlı küme ölçeklendirebilirsiniz. böylece bu işleri, HDInsight, normal MapReduce veya Spark işleri olarak yürütülür.

## <a name="prerequisites"></a>Önkoşullar

Bu uygulamayı yeni bir HDInsight kümesi veya mevcut bir kümeye yüklemek için aşağıdaki yapılandırmaya sahip olmalıdır:

* Küme katmanları: Standart, Premium
* Küme türleri: Hadoop, Spark
* Küme sürümleri: 3.4, 3.5

## <a name="install-the-dataiku-dss-published-application"></a>Yayımlanan uygulamayı yükleme Dataiku DSS

Bu ve diğer kullanılabilir ISV uygulamalarını yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Apache Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-dataiku-dss"></a>Dataiku DSS başlatın

1. Yükleme sonrasında DSS Azure portalında kümenizden giderek başlatabilirsiniz **ayarları** bölmesinde, ardından **uygulamaları** altında **genel** kategorisi. Yüklü uygulamalar bölmesi, yüklü uygulamalar listelenir.

    ![Dataiku DSS yüklü uygulama](./media/hdinsight-apps-install-dataiku/app.png)

2. HDInsight üzerinde DSS seçtiğinizde, web sayfası ve SSH uç noktası yolu bir bağlantısına bakın. Web sayfası bağlantıyı seçin.

3. İlk başlatma sırasında ücretsiz olarak yeni bir Dataiku hesabı oluşturun veya var olan bir hesap için oturum açmak için bir form ile sunulur. Ücretsiz 2 haftalık deneme sürümünü Başlat seçeneğini de [Enterprise Edition](https://www.dataiku.com/dss/editions/). Bu noktadan itibaren bir lisans anahtarı girmek için Enterprise Edition veya Community Edition'ı kullanarak doğurduğu seçeneğiniz vardır.

4. Seçilen lisans seçeneğinizi tamamladıktan sonra bir oturum açma formu ile sunulur. Oturum açma formu önce görüntülenen varsayılan kimlik bilgilerini girin.

Aşağıdaki adımlar, basit bir örnek sağlar.

1. [Örnek siparişleri CSV indirme](https://doc.dataiku.com/tutorials/data/101/haiku_shirt_sales.csv).

2. DSS Panoda **+** (Yeni Proje) yeni bir proje oluşturmak için sol menüdeki bağlantı.

    ![Yeni proje bağlantısı](./media/hdinsight-apps-install-dataiku/new-project.png)

3. Yeni Proje biçiminde yazın bir **adı**. **Proje anahtarı** önerilen değeri ile otomatik olarak doldurulur. Bu durumda, "Siparişler" girin. Tıklayın **Oluştur**.

    ![Yeni Proje formu](./media/hdinsight-apps-install-dataiku/new-project-form.png)

4. Seçin **+ İçeri Aktarma Sihirbazı ilk veri KÜMESİNİ** yeni proje sayfanızın.

    ![Karşıya Dosya Yükleme](./media/hdinsight-apps-install-dataiku/import-dataset.png)

5. Seçin **dosyalarınızın karşıya yüklenmesinin** altında **dosyaları** veri kümesi listesi. Karşıya yükleme iletişim kutusu ile sunulur. Bir dosya Ekle'ye tıklayın, seçin `haiku_shirt_sales.csv` dosyası indirilir ve doğrulayın.

6. DSS için dosya karşıya yüklendi. DSS CSV biçimi doğru Önizleme düğmesine tıklayarak algılanıp denetleyin.

    ![Karşıya Dosya Yükleme](./media/hdinsight-apps-install-dataiku/preview.png)

7. İçeri aktarma neredeyse mükemmeldir. CSV dosyası sekmesi ayırıcısı kullanıyor. Veri özellikleri ve gözlemler temsil eden satırlar adlı sütun başlıkları bir tablo biçiminde olduğunu görebilirsiniz. Görünüşe göre üst bilgi ve veriler arasında boş bir satır dosyasında yer alan, bir hatadır. Bu hatayı düzeltmek için girin `1` içinde **sonraki satırları atla** alan.

    ![Kaydet](./media/hdinsight-apps-install-dataiku/skip-lines.png)

8. Yeni veri kümesi bir ad verin. Girin **haiku_shirt_sales** ekranın en üstünde alanını seçip **Oluştur** düğmesi.

9. Araştırma başlayabileceğiniz verilerinizi tablolu bir görünümünü görürsünüz. Her sütun için Dataiku Science Studio içindeki bir veri türü algıladı görmelisiniz _mavi_ - bu durum, metin, sayı veya tarih (neanalyzovaný). Ölçer sütun olduğu için değerleri türü (kırmızı) eşleştirmek için görünen veya (boş) eksik oranını gösterir. Bu örnek veri kümesinde bölüm hem boş değerler hem de geçersiz veriler var.

    ![Tablo görünümü](./media/hdinsight-apps-install-dataiku/viewing-dataset.png)

## <a name="data-manipulation"></a>Veri işleme

Neredeyse her zaman gerçek veri kullanmak istemiyor ve bunun düzgünce nadiren paketlenmiştir. Genellikle verileri temizleme komut dosyaları ve ilişkili iş mantığı zinciri gerektirir. Dataiku DSS sağlar, ayrılmış bir **Laboratuvar** bu görevi daha kolay hale getirmek için aracı.

1. Tıklayarak **Laboratuvar** sağ üst köşedeki.

    ![Laboratuvar düğmesi](./media/hdinsight-apps-install-dataiku/lab-button.png)

2. Laboratuvar penceresi açılır. Laboratuvar çalıştırmalarınızı içine başka almak için veri kümeniz üzerinde çalıştığınız olur. Bu öğreticide Visual analizi en boy gösterilmektedir. Seçin **yeni** görsel analiz aşağıda düğmesi. Çözümleme için bir ad belirtmeniz istenir. Şimdilik varsayılan adı bırakın ve ardından tıklayın **Oluştur**.

    ![Laboratuvar oluşturma](./media/hdinsight-apps-install-dataiku/create-lab.png)

3. Seçin **hızlı sütun istatistikleri** sayfanın sağ üst köşesindeki düğmesi.

    ![Hızlı sütun istatistikleri](./media/hdinsight-apps-install-dataiku/quick-column-stats.png)

4. Veri türleri ve zaman çizelgesi tabanlı grafikler altında görüntülenen değerleri için istatistikleri gördüğünüz **sütunları Hızlı Görünüm** bölmesi.

    ![Sütunları Hızlı Bakış](./media/hdinsight-apps-install-dataiku/columns-quick-view.png)

Şimdi, örnek verileri kullanarak DSS keşfedebilirsiniz. Temizleme ve verilerle çalışacak ve yeni görselleştirmeler oluşturun.

Detaylı eğitimler için okuma [öğrenin Dataiku DSS](https://www.dataiku.com/learn/).

## <a name="next-steps"></a>Sonraki adımlar

* [Dataiku DSS belgeleri](https://doc.dataiku.com/dss/latest/).
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): HDInsight için yayımlanmamış bir HDInsight uygulamasının nasıl dağıtılacağını öğrenin.
* [HDInsight uygulamaları yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi'nde yayımlama konusunda bilgi edinin.
* [MSDN: Bir HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirin](hdinsight-hadoop-customize-cluster-linux.md): Ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [HDInsight içinde boş kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümeleri erişmek ve test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.
