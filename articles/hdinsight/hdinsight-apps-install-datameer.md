---
title: Yayımlanan uygulama - Datameer - Azure HDInsight yükleme
description: Yükleme ve Datameer üçüncü taraf Apache Hadoop uygulamasını kullanın.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 97d99aa59c490cf2dcdd4a69f32411a051942d36
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037813"
---
# <a name="install-published-application---datameer"></a>Yayımlanan uygulama - Datameer yükleme

Bu makalede, yüklemek ve çalıştırmak açıklanır [Datameer](https://www.datameer.com/) Azure HDInsight üzerinde Apache Hadoop uygulama yayımlanır. HDInsight uygulama platformu için genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için bkz. yayımlanan uygulamalar [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-datameer"></a>Datameer hakkında

Datameer, Hadoop platform, genişletme var olan Azure HDInsight özellikleri ve hızlı tümleştirme, hazırlama ve yapılandırılmış ve yapılandırılmamış veri analizini sağlayan bir yerel bir uygulamadır. Datameer erişim 70'ten fazla kaynakları ve biçimler: yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış. Doğrudan veri yükleme veya isteğe bağlı olarak veri çekmek için kendi benzersiz veri bağlantıları kullanın. Self Servis Datameer'ın işlevsellik ve bilinen elektronik tablo arabirimi büyük veri teknoloji karmaşıklığını azaltır ve zaman öngörülere ve hızlandırır. Elektronik Tablo arabirimi en iyi duruma getirilmiş için Hadoop işlerini çevrilir bildirim temelli formüllerini girmek için basit bir mekanizma sağlar. Datameer ve iş zekası (BI) ve Excel becerileri ile bulutta Hadoop hızlı bir şekilde kullanabilirsiniz. Daha fazla bilgi için [Datameer belgeleri](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).

## <a name="prerequisites"></a>Önkoşullar

Bu uygulamayı yeni bir HDInsight kümesi veya mevcut bir kümeye yüklemek için aşağıdaki yapılandırmaya sahip olmalıdır:

* Küme katmanı: standart
* Küme türü: Hadoop
* Küme sürümü: 3.4

## <a name="install-the-datameer-published-application"></a>Yayımlanan uygulamayı yükleme Datameer

Bu ve diğer kullanılabilir ISV uygulamalarını yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-datameer"></a>Datameer başlatın

1. Yükleme sonrasında Datameer Azure portalında kümenizden giderek başlatabilirsiniz **ayarları** bölmesinde, ardından **uygulamaları** altında **genel** kategorisi . Yüklü uygulamalar bölmesi, yüklü uygulamalar listelenir.

    ![Yüklü Datameer uygulama](./media/hdinsight-apps-install-datameer/datameer-app.png)

2. Datameer seçtiğinizde, web sayfası ve SSH uç noktası yolu için bir bağlantı görürsünüz. Web sayfası bağlantıyı seçin.

3. İlk başlatmada iki lisans seçenek vardır: ya da bir 14 günlük ücretsiz deneme, ya da mevcut bir lisansınız etkinleştirin.

    ![Lisans seçenekleri](./media/hdinsight-apps-install-datameer/license.png)

4. Seçilen lisans seçeneğinizi tamamladıktan sonra bir oturum açma formu ile sunulur. Oturum açma formu önce görüntülenen varsayılan kimlik bilgilerini girin. Oturum açtıktan sonra devam etmek için yazılım lisans sözleşmesini kabul edin.

    ![Oturum Aç](./media/hdinsight-apps-install-datameer/login.png)

Aşağıdaki adımlar, bir "Hello World" örnek gösterir.

1. [CSV örneği indirin](https://datameer.box.com/s/wzzw27za3agic4yjj8zrn6vfrph0ppnf).

2. Tıklayın **+** Datameer Pano üzerinde oturum açın ve tıklayın **dosyayı karşıya yüklemeyi**.

    ![Karşıya Dosya Yükleme](./media/hdinsight-apps-install-datameer/upload.png)

3. Karşıya yükleme iletişim kutusunda, Gözat ve Seç **Hello World.csv** indirdiğiniz dosya. Emin **dosya türü** CSV'ye ayarlama / TSV. **İleri**’yi seçin. Sürekli **sonraki** Sihirbazın sonuna ulaşana kadar.

    ![Karşıya Dosya Yükleme](./media/hdinsight-apps-install-datameer/upload-browse.png)

4. Dosya adı **Hello World** altında yeni bir klasör. Yeni klasör "Tanıtım" yeniden adlandırın. **Kaydet**’i seçin.

    ![Kaydet](./media/hdinsight-apps-install-datameer/save.png)

5. Tıklayın **+** seçin ve daha sonra oturum **çalışma kitabı** verileri için yeni bir çalışma kitabı oluşturmanız gerekir.

    ![Çalışma kitabı Ekle](./media/hdinsight-apps-install-datameer/add-workbook.png)

6. Genişletin **veri** klasöründe **FileUploads**, ardından **tanıtım** "Hello World" dosyasını kaydederken oluşturduğunuz klasör. Seçin **Hello World** dosyaların listesini oluşturur ardından tıklatın **ekleme veri**.

    ![Kaydet](./media/hdinsight-apps-install-datameer/select-file.png)

7. Bir elektronik tablo arabirimi yüklenen verileri görürsünüz. Bir veri alt kümesini seçmek için **filtre** araç çubuğu düğmesi.

    ![Filtre düğmesi](./media/hdinsight-apps-install-datameer/filter-button.png)

8. Filtre Uygula iletişim kutusunda, seçmek **Şehir** sütun **eşittir** işleci ve türü **Chicago** filtre metin kutusuna. Denetleme **filtre oluştur yeni bir e-tablosunda** onay kutusunu seçip **oluşturma filtresi**.

    ![Filtre Uygula](./media/hdinsight-apps-install-datameer/apply-filter.png)

9. Çalışma kitabı Kaydet'e tıklayarak **dosya**, ardından **Kaydet**. "Hello World çalışma" gibi bir ad sağlayın.

10. Seçenekler nasıl ve ne zaman çalıştırılacağını çalışma kitabı ile sunulur. Şimdilik, tüm seçenekleri varsayılan değerlerinde bırakın ve ardından denetleyin **başlangıç hesaplama işlemi hemen sonra Kaydet**seçip **Kaydet**.

    ![Çalışma kitabını kaydedin](./media/hdinsight-apps-install-datameer/save-workbook.png)

11. Datameer, güçlü görselleştirme araçları sunar. Verileri görüntülemek için bir bilgi grafiği oluşturun. Seçin **+** Pano üzerinde oturum açın ve ardından **bilgi grafiği**.

    ![Bilgi grafiği ekleme](./media/hdinsight-apps-install-datameer/infographic-button.png)

12. 1. adım Aşağıdaki diyagramda gösterildiği gibi bir çubuk grafik pencere öğesi sol tarafta, pencere öğesi listesinden sürükleyin. Ardından, sağdaki veri tarayıcı altında veri klasör gidin, sonra (Adım 2) filtreyle eklediğiniz çalışma kitabınızı genişletin. Sürükleme **adı** sütun üst çubuk grafik halinde açılır ve üzerinde **etiket** çalışma kitabının adını sütun grafiğin etiket alanı (3. adım) ayarlamak için hedef.

    ![Bilgi grafiği](./media/hdinsight-apps-install-datameer/infographic.png)

13. Yaş grafiğin Y ekseni olarak ayarlamak için sürükleyin **yaş** grafiğin sütununa **veri** alan.

    ![Bilgi grafiği](./media/hdinsight-apps-install-datameer/infographic-age.png)

Tebrikler! Herhangi bir kod yazmaya gerek kalmadan verilerinizi görselleştirme oluşturdunuz. Şimdi çözümlenmeyebileceği ve ek görselleştirmeler keşfedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Datameer belgeleri](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): HDInsight için yayımlanmamış bir HDInsight uygulamasının nasıl dağıtılacağını öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirin](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [HDInsight içinde boş kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümeleri erişmek ve test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.
