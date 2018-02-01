---
title: "Yükleme yayımlanan uygulama - Dataiku DDS - Azure Hdınsight | Microsoft Docs"
description: "Yükleyin ve Dataiku DDS üçüncü taraf Hadoop uygulama kullanın."
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 835f433e0bf79e8bc4d9b30963196f65bc53cd0e
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="install-published-application---dataiku-dds"></a>Yayımlanan uygulama - Dataiku DDS yükleyin

Bu makalede yüklemek ve çalıştırmak nasıl [Dataiku DDS](https://www.dataiku.com/) Azure hdınsight'ta Hadoop uygulama yayımlanır. Hdınsight uygulaması platformu genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için yayımlanan uygulamalar bkz [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-dataiku-dss"></a>Dataiku DSS hakkında

Dataiku [veri bilimi Studio (DSS)](https://www.dataiku.com/dss/features/connectivity/), yapı ve analitik çözümler sunmak veri bilimcilerine sağlar işbirliği veri bilimi platformudur. Bir Hdınsight olarak DSS sunumu uygulama, büyük veri çözümleri oluşturmak ve bunları Kurumsal düzeyde ve ölçek çalıştırmak için veri bilimi kullanmanızı sağlar.

DSS hazırlığı, veri alımı ile başlayan ve işleme eksiksiz bir analitik çözümü uygulamak için kullanabilirsiniz. DSS çözüm de eğitim ve uygulama makine öğrenimi modellerini, Görselleştirme ve ardından faaliyete geçirmeye yönelik içerir.

Hadoop veya Spark kümeleri kullanarak Hdınsight'ta DSS yükleyebilirsiniz. Yeni küme oluşturma sırasında var olan çalışan kümelerinde veya DSS yükleyebilirsiniz. DSS verileri okumak için bir bağlayıcı olarak Azure Blob storage kullanarak da destekler.

DSS, MapReduce veya Spark işlerinin sonra oluşturabilirsiniz projeleri oluşturmak üzere kullanabilirsiniz. İsteğe bağlı küme ölçeklendirmek için bu işleri, hdınsight'ta normal MapReduce veya Spark iş olarak çalıştırılır.

## <a name="prerequisites"></a>Önkoşullar

Yeni bir Hdınsight kümesi veya varolan bir kümenin bu uygulamayı yüklemek için aşağıdaki yapılandırmaya sahip olmalısınız:

* Küme tier(s): standart ve Premium
* Küme türleri: Hadoop, Spark
* Küme sürüm(ler): 3.4, 3.5

## <a name="install-the-dataiku-dss-published-application"></a>Uygulama yükleme Dataiku DSS yayımlanır

Bu ve diğer kullanılabilir ISV uygulamaları yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-dataiku-dss"></a>Dataiku DSS başlatın

1. Yükleme sonrasında, DSS Azure portalında, kümeden giderek başlatabilirsiniz **ayarları** tıklatarak bölmesinde **uygulamaları** altında **genel** kategorisi. Yüklü uygulamalar bölmesinde yüklü uygulamalar listelenir.

    ![Yüklü Dataiku DSS uygulama](./media/hdinsight-apps-install-dataiku/app.png)

2. Hdınsight üzerinde DSS seçtiğinizde, bir web sayfası ve SSH uç noktası yolu bağlantısına bakın. Web sayfası bağlantıyı seçin.

3. İlk kez başlatıldığında üzerinde yeni bir Dataiku hesap ücretsiz oluşturmak veya var olan bir hesapla oturum açmak için bir form ile sunulur. Ücretsiz bir 2 haftalık deneme sürümünü Başlat seçeneğine de [Enterprise Edition](https://www.dataiku.com/dss/editions/). Bu noktadan bir lisans anahtarı için Enterprise Edition girerek veya Community Edition'ı kullanarak ile devam etmeden seçeneğiniz vardır.

4. Seçilen lisans seçeneğinizi tamamladıktan sonra oturum açma formu ile sunulur. Oturum açma formu önce görüntülenen varsayılan kimlik bilgilerini girin.

Aşağıdaki adımlar, basit bir örnek sağlar.

1. [Örnek siparişleri CSV Yükle](https://doc.dataiku.com/tutorials/data/101/haiku_shirt_sales.csv).

2. DSS panodan seçin  **+**  (Yeni Proje) yeni bir proje oluşturmak üzere sol menüdeki bağlantısında.

    ![Yeni proje bağlantısı](./media/hdinsight-apps-install-dataiku/new-project.png)

3. Yeni Proje biçiminde yazın bir **adı**. **Proje anahtarı** önerilen değeri ile otomatik olarak doldurulur. Bu durumda, "Siparişler" girin. Tıklatın **oluşturma**.

    ![Yeni Proje formu](./media/hdinsight-apps-install-dataiku/new-project-form.png)

4. Seçin **+ alma bilgisayarınızı ilk veri KÜMESİNİ** yeni proje sayfanıza.

    ![Karşıya Dosya Yükleme](./media/hdinsight-apps-install-dataiku/import-dataset.png)

5. Seçin **dosyalarınızı karşıya** altında **dosyaları** dataset listesi. Karşıya yükleme iletişim kutusuyla sunulur. Dosya Ekle'a tıklayın, `haiku_shirt_sales.csv` dosyası indirilir ve doğrulama.

6. Dosya için DSS yüklenir. DSS CSV biçimi doğru Önizleme düğmesini tıklatarak algılanırsa denetleyin.

    ![Karşıya Dosya Yükleme](./media/hdinsight-apps-install-dataiku/preview.png)

7. İçeri aktarma neredeyse mükemmeldir. CSV dosyası sekmesi ayırıcı kullanıyor. Özellikleri ve gözlemleri temsil eden satırlar adlı sütunları bir tablo biçiminde veridir görebilirsiniz. Görünüşe göre dosya üstbilgisi ve verileri arasında boş bir satıra içermediğini hatasıdır. Bu hatayı düzeltmek için girin `1` içinde **Atla sonraki satırları** alan.

    ![Kaydet](./media/hdinsight-apps-install-dataiku/skip-lines.png)

8. Yeni veri kümesi bir ad verin. Girin **haiku_shirt_sales** ekranın üstünde alanında seçip **oluşturma** düğmesi.

9. Araştırma başlayabileceğiniz verilerinizin Tablo görünümüne bakın. Her bir sütunun Dataiku Bilim Studio içindeki bir veri türü algıladı görmelisiniz _mavi_ - bu durum, metin, sayı veya tarih (ayrýþtýrýlmamýþ). Bir ölçer kendisi için değerleri yazın (kırmızı) eşleştirilecek olmadığı veya (boş) eksik sütun oranını gösterir. Bu örnek kümesinde departman boş değerler ve geçersiz veriler var.

    ![Tablo görünümü](./media/hdinsight-apps-install-dataiku/viewing-dataset.png)

## <a name="data-manipulation"></a>Veri Yönetimi

Gerçek veri neredeyse her zaman düzensiz ve onu düzgünce nadiren paketlenmiştir. Verileri genellikle temizleme komut dosyaları ve ilişkili iş mantığı zinciri gerektirir. Dataiku DSS sağlar ayrılmış bir **Laboratuvar** bu görevi daha kullanıcı dostu olmasını sağlamak için aracı.

1. Tıklayın **Laboratuvar** sağ üst köşedeki.

    ![Laboratuvar düğmesi](./media/hdinsight-apps-install-dataiku/lab-button.png)

2. Laboratuvar penceresi açılır. Laboratuvar tekrarlayarak içine başka almak için veri kümesi üzerinde çalıştığınız ' dir. Bu öğretici görsel çözümleme en boy gösterir. Seçin **yeni** görsel çözümleme aşağıdaki düğmesine. Çözümleme için bir ad belirtmeniz istenir. Şimdi varsayılan adı bırakın ve ardından **oluşturma**.

    ![Laboratuvar oluşturma](./media/hdinsight-apps-install-dataiku/create-lab.png)

3. Seçin **hızlı sütunları istatistiği** sayfanın sağ üst köşesinde bulunan düğmesi.

    ![Hızlı sütunları istatistikleri](./media/hdinsight-apps-install-dataiku/quick-column-stats.png)

4. Veri türleri ve değerleri zaman çizelgesi tabanlı grafikleri altında görüntülenen istatistiklerini gördüğünüz **sütunları Hızlı Görünüm** bölmesi.

    ![Sütunları Hızlı Görünüm](./media/hdinsight-apps-install-dataiku/columns-quick-view.png)

Şimdi örnek verileri kullanarak DSS da gözden geçirebilirsiniz. Temizleme ve verilerle çalışmak ve yeni görselleştirmeler oluşturun.

Ayrıntılı öğreticileri için okuma [öğrenin Dataiku DSS](https://www.dataiku.com/learn/).

## <a name="next-steps"></a>Sonraki adımlar

* [Dataiku DSS belgelerine](https://doc.dataiku.com/dss/latest/).
* [Özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir Hdınsight uygulamasının Hdınsight'a nasıl yükleneceğini öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı Hdınsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [Hdınsight'ta boş kenar düğümünü kullanmak](hdinsight-apps-use-edge-node.md): Hdınsight kümeleri erişmek ve test etmek ve Hdınsight uygulamalarını barındırmak için bir boş kenar düğümüne kullanmayı öğrenin.
