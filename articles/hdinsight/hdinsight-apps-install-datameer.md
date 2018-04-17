---
title: Yükleme yayımlanan uygulama - Datameer - Azure Hdınsight | Microsoft Docs
description: Yükleyin ve Datameer üçüncü taraf Hadoop uygulama kullanın.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 5008056ae2274d058706649f286b91b71feadc27
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="install-published-application---datameer"></a>Yayımlanan uygulama - Datameer yükleyin

Bu makalede yüklemek ve çalıştırmak nasıl [Datameer](https://www.datameer.com/) Azure hdınsight'ta Hadoop uygulama yayımlanır. Hdınsight uygulaması platformu genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için yayımlanan uygulamalar bkz [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-datameer"></a>Datameer hakkında

Datameer Hadoop platform, genişletme Azure Hdınsight özellikleri var olan ve hızlı tümleştirme, hazırlık ve yapılandırılmış ve yapılandırılmamış veri analizi sağlayarak yerel bir uygulamadır. Datameer 70'den fazla kaynakları ve biçimler erişebilirsiniz: yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış. Doğrudan veri karşıya yükleyebilir veya isteğe bağlı olarak veri çekmek için kendi benzersiz veri bağlantıları kullanın. Datameer'ın Self Servis işlevleri ve bilinen elektronik tablo arabirimi büyük veri teknolojisi karmaşıklığını azaltır ve saat için Insight hızlandırır. Elektronik Tablo arabirimi en iyi duruma getirilmiş için Hadoop işlerini sonra çevrilmesi bildirim temelli formüller girmek için basit bir mekanizma sağlar. Datameer ve iş zekası (BI) ve Excel yetenekleri ile bulutta Hadoop hızlı bir şekilde kullanabilirsiniz. Daha fazla bilgi için bkz: [Datameer belgelerine](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).

## <a name="prerequisites"></a>Önkoşullar

Yeni bir Hdınsight kümesi veya varolan bir kümenin bu uygulamayı yüklemek için aşağıdaki yapılandırmaya sahip olmalısınız:

* Küme katmanı: standart
* Küme türü: Hadoop
* Küme sürümü: 3.4

## <a name="install-the-datameer-published-application"></a>Uygulama yükleme Datameer yayımlanır

Bu ve diğer kullanılabilir ISV uygulamaları yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-datameer"></a>Datameer başlatma

1. Yükleme sonrasında, Datameer Azure portalında, kümeden giderek başlatabilirsiniz **ayarları** tıklatarak bölmesinde **uygulamaları** altında **genel** kategorisi . Yüklü uygulamalar bölmesinde yüklü uygulamalar listelenir.

    ![Yüklü Datameer uygulama](./media/hdinsight-apps-install-datameer/datameer-app.png)

2. Datameer seçtiğinizde, bir web sayfası ve SSH uç noktası yolu bağlantısına bakın. Web sayfası bağlantıyı seçin.

3. İlk kez başlatıldığında üzerinde iki lisans seçenek vardır: ya da bir 14 günlük ücretsiz deneme, veya varolan bir lisans etkinleştirin.

    ![Lisans seçenekleri](./media/hdinsight-apps-install-datameer/license.png)

4. Seçilen lisans seçeneğinizi tamamladıktan sonra oturum açma formu ile sunulur. Oturum açma formu önce görüntülenen varsayılan kimlik bilgilerini girin. Oturum açtıktan sonra devam etmek için yazılım lisans sözleşmesini kabul edin.

    ![Oturum Aç](./media/hdinsight-apps-install-datameer/login.png)

Aşağıdaki adımlar, "Hello World" Tanıtımı gösterir.

1. [Örnek CSV Yükle](https://datameer.box.com/s/wzzw27za3agic4yjj8zrn6vfrph0ppnf).

2. Tıklatın **+** oturum üstünde Datameer Pano ve tıklatın **dosyayı karşıya yüklemeyi**.

    ![Karşıya Dosya Yükleme](./media/hdinsight-apps-install-datameer/upload.png)

3. Karşıya yükleme iletişim kutusunda, bulun ve seçin **Hello World.csv** indirdiğiniz dosya. Emin olun **dosya türü** CSV'ye ayarlamak / TSV. **İleri**’yi seçin. Sürekli **sonraki** Sihirbazın sonuna ulaşana kadar.

    ![Karşıya Dosya Yükleme](./media/hdinsight-apps-install-datameer/upload-browse.png)

4. Dosya adı **Hello World** altında yeni bir klasör. Yeni klasör "Demo" yeniden adlandırın. **Kaydet**’i seçin.

    ![Kaydet](./media/hdinsight-apps-install-datameer/save.png)

5. Tıklatın **+** seçin ve bir kez daha imzalamak **çalışma kitabı** verileri için yeni bir çalışma kitabı oluşturulamadı.

    ![Çalışma kitabı ekleme](./media/hdinsight-apps-install-datameer/add-workbook.png)

6. Genişletme **veri** klasörünü **FileUploads**, sonra **Demo** "Hello World" Dosya kaydedilirken oluşturduğunuz klasör. Seçin **Hello World** dosyaların listesini oluşturur ve ardından **eklemek veri**.

    ![Kaydet](./media/hdinsight-apps-install-datameer/select-file.png)

7. Bir elektronik tablo arabiriminde yüklenen veriler bakın. Bir veri alt kümesini seçmek için **filtre** araç çubuğu düğmesi.

    ![Filtre düğmesi](./media/hdinsight-apps-install-datameer/filter-button.png)

8. Filtre Uygula iletişim kutusunda seçin **Şehir** sütun, **eşittir** işleci ve türü **Chicago** filtre metni kutusuna. Denetleyin **yeni sayfa oluşturma filtrede** onay kutusunu seçip **Create Filter**.

    ![Filtre Uygula](./media/hdinsight-apps-install-datameer/apply-filter.png)

9. Tıklayarak çalışma kitabını kaydedin **dosya**, ardından **kaydetmek**. "Hello World çalışma kitabı" gibi bir ad sağlayın.

10. Seçenekleri nasıl ve ne zaman çalışacağını çalışma kitabı ile sunulur. Şimdilik, tüm seçeneklerin varsayılan değerlerinde bırakın ve ardından denetleyin **başlangıç hesaplama işlemi hemen sonra Kaydet**seçip **kaydetmek**.

    ![Çalışma kitabını kaydedin](./media/hdinsight-apps-install-datameer/save-workbook.png)

11. Datameer güçlü görsel araçlar sağlar. Verileri görüntülemek için bir bilgi grafiği oluşturun. Seçin **+** oturum üzerinde panonun sonra seçin **bilgi grafiği**.

    ![Bilgi grafiği ekleyin](./media/hdinsight-apps-install-datameer/infographic-button.png)

12. Çubuk grafiği pencere öğesi 1. adım Aşağıdaki diyagramda gösterildiği gibi sol tarafta, pencere öğeleri listesinden sürükleyin. Ardından, sağdaki veri tarayıcı altında bulunan veri klasöründe gezinin, (2. adım) filtreyle eklediğiniz çalışma ardından, çalışma kitabı'nı genişletin. Sürükleme **adı** sütun çubuk grafiği ve içine bırak üstündeki üzerinde **etiket** hedef çalışma kitabının ad sütunu grafiğin etiket alanı (3. adım) olarak ayarlayın.

    ![Bilgi grafiği](./media/hdinsight-apps-install-datameer/infographic.png)

13. Geçerlilik süresi grafiğin Y ekseni ayarlamak için sürükleyin **yaş** grafiğin sütununa **veri** alan.

    ![Bilgi grafiği](./media/hdinsight-apps-install-datameer/infographic-age.png)

Tebrikler! Herhangi bir kod yazmak zorunda kalmadan, verilerinizin görselleştirme oluşturduğunuzu düşünün. Şimdi Çeşitlemeler ve ek görselleştirmeleri da gözden geçirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Datameer belgeleri](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).
* [Özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir Hdınsight uygulamasının Hdınsight'a nasıl yükleneceğini öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı Hdınsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [Hdınsight'ta boş kenar düğümünü kullanmak](hdinsight-apps-use-edge-node.md): Hdınsight kümeleri erişmek ve test etmek ve Hdınsight uygulamalarını barındırmak için bir boş kenar düğümüne kullanmayı öğrenin.
