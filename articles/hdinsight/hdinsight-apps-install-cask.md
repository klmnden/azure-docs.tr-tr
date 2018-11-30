---
title: Yayımlanan uygulama - Datameer - Azure HDInsight yükleme
description: Yükleme ve Datameer üçüncü taraf Hadoop uygulamasını kullanın.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: ef61ee9f15253c6a270cd4089625776a458df2ee
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52499332"
---
# <a name="install-published-application---cask-data-application-platform-cdap"></a>Yayımlanan uygulama - Cask Data Application Platform (CDAP) yükleme

Bu makalede, yüklemek ve çalıştırmak açıklanır [CDAP](http://cask.co/products/cdap/) yayımlanan [Apache Hadoop](https://hadoop.apache.org/) Azure HDInsight uygulaması. HDInsight uygulama platformu için genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için bkz. yayımlanan uygulamalar [üçüncü taraf Apache Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-cdap"></a>CDAP hakkında

Hadoop uygulamaları geliştirmek zor olabilir.  Tümleştirmek için biraz zaman alabilir Hadoop teknoloji uzantılar, geniş ve büyüyen bir sayısı yoktur. Veri akışı izleme ve ölçümleri toplamaya ayrı bir çözümde oluşturmak gerekebilir.

### <a name="how-does-cdap-help"></a>CDAP nasıl yardımcı oldu mu?

Cask Data Application Platform (CDAP), büyük veriler için bir tümleştirme platformudur. CDAP odaklanmak uygulamaları oluşturmak yerine temel alınan altyapı sağlar.

Üst düzey kavramlarını ve geliştiricilerinin aşina soyutlama CDAP kullanır. Bu soyutlama, iç sistemleri karmaşıklığını gizleyebilir ve çözümleri kullanılırlığı teşvik edin.

CDAP uzantı adlandırılan [Cask Hydrator](http://cask.co/products/hydrator/) geliştirin ve veri işlem hatlarını yönetmek için bir kullanıcı arabirimi sağlar. Veri komut zinciri çeşitli oluşur * görevleri eklentileri, veri alma, dönüştürme, analiz ve çalıştırma sonrası işlemleri ister.

Her CDAP eklentinin iyi tanımlanmış bir arabirim sahiptir; böylece değerlendirme farklı teknolojiler uygulama geri kalanını touch gerek kalmadan başka bir, bir eklenti değiştirme adımlarından oluşur.

CDAP *işlem hatları* bir üst düzey resimsel uygulamanızda veri akışı sağlar. Bu görselleştirme, veri alımı, veri dönüşümlerini ve analizleri, üzerinden uçtan uca akışını gösterir ve son olarak bir dış veri depolayın.

Aşağıdaki örnek bir veri işlem hattı, gerçek zamanlı twitter verilerini alır ve ardından önceden tanımlanmış ölçütlere göre bazı tweetleri filtreler. Veri işlem hattı ham tweet verileri dönüştürür ve veri daha okunabilir bir biçimine ardından tweetleri bir değerler kümesi göre gruplandırır ve sonuçları bir HBase Yazar depolar.

![CDAP işlem hattı](./media/hdinsight-apps-install-cask/pipeline.png)

Bu uçtan uca işlem hattı kullanılarak oluşturulan **Cask Hydrator UI**, her aşama arasında bağlantı eklenti arabirimi ve sürükle ve bırak işlevlerini kullanma. Yalıtmak ve her eklentinin işlevlerini bağımsız olarak değiştirin. CDAP kullanarak, benzer bir işlem hatları oluşturulabilen ve saat olarak doğrulandı. Tipik Hadoop dünyasında, bu tür çözümler oluşturmak, birkaç gün sürebilir.

CDAP ayrıca adlı bir uzantı sağlar [Cask İzleyicisi](http://cask.co/products/tracker/) görsel olarak izleme verilerinin için akışlar uygulama. Cask İzleyicisi ekler *veri yönetimi* sisteme veri varlıklarını izlerse uygulama genelinde yönetilebilmeleri için vmm'nin. Her veri noktasının rengini kökenini izlemek, ilgili ölçümleri toplamak ve denetim işlemi boyunca veri kaydı.

Yukarıdaki işlem hattı, verileri nasıl geçiyor, bir gösterim şu şekildedir:

![CDAP İzleyicisi](./media/hdinsight-apps-install-cask/tracker.png)

## <a name="prerequisites"></a>Önkoşullar

Bu uygulamayı yeni bir HDInsight kümesi veya mevcut bir kümeye yüklemek için aşağıdaki yapılandırmaya sahip olmalıdır:

* Küme katmanı: standart
* Küme türü: HBase
* Küme sürümü: 3.4, 3.5

## <a name="install-the-cdap-published-application"></a>Yayımlanan uygulamayı yükleme CDAP

Bu ve diğer kullanılabilir ISV uygulamalarını yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-cdap"></a>CDAP başlatın

1. Yükleme sonrasında CDAP Azure portalında kümenizden giderek başlatın **ayarları** seçip bölmesinde **uygulamaları** altında **genel** kategorisi. Yüklü uygulamalar bölmesi tüm yüklü uygulamalar listelenir.

    ![Yüklü CDAP uygulama](./media/hdinsight-apps-install-cask/cdap-app.png)

2. CDAP seçtiğinizde, web sayfası ve HTTP uç noktası ve ayrıca SSH uç noktası yolu için bir bağlantı görürsünüz. Web sayfası bağlantıyı seçin.

3. İstendiğinde, Küme Yöneticisi kimlik bilgilerinizi girin.

    ![Kimlik Doğrulaması](./media/hdinsight-apps-install-cask/auth.png)

4. Oturum açtıktan sonra Cask CDAP GUI giriş sayfasını görürsünüz.

    ![Cask GUI giriş sayfası](./media/hdinsight-apps-install-cask/gui.png)

5. CDAP arabirimi keşfetmek için tıklatın **Cask Pazar** menü bağlantısı sayfanın en üstünde.

    ![Cask Market bağlantısı](./media/hdinsight-apps-install-cask/cask-market.png)

6. Seçin **erişim günlük örnek** listeden.

    ![Erişim günlüğü örneği](./media/hdinsight-apps-install-cask/market-log-sample.png)

7. Tıklayın **yük** onaylamak için.

    ![Yükle'yi tıklatın](./media/hdinsight-apps-install-cask/market-load.png)

8. Bir görünüm dahil edilmiş örnek veri görüntülenir. Seçin **sonraki**.

    ![Günlük örnek - görünümünü veri erişimi](./media/hdinsight-apps-install-cask/market-view-data.png)

9. Seçin **Stream** hedef türü bir hedef ad girin ve ardından **son**.

    ![Erişim günlüğü örneği - hedefi seçin](./media/hdinsight-apps-install-cask/market-destination.png)

10. Datapack başarıyla yüklendikten sonra seçin **Stream ayrıntıları**.

    ![Datapack başarıyla karşıya yüklendi](./media/hdinsight-apps-install-cask/market-view-details.png)

11. Ad alanı için meta verileri etkinleştirmek için seçin **etkinleştirme** erişim günlüğü ayrıntıları sayfasındaki kullanımı sekmesinde.

    ![-Yüklendi - erişim günlüğü örnek meta verilerini etkinleştir](./media/hdinsight-apps-install-cask/log-loaded.png)

12. Meta veri etkinleştirildikten sonra denetim iletisi bilgileri görüntüleyen bir grafik görürsünüz.

    ![Meta veri erişim günlüğü örneği - etkin](./media/hdinsight-apps-install-cask/log-metadata.png)

13. Günlük verileri araştırmak için seçin **Araştır** sayfanın en üstünde simgesi.

    ![Erişim günlüğü örneği - keşfedin](./media/hdinsight-apps-install-cask/log-explore.png)

14. Örnek SQL sorgusu görürsünüz. İstenen ve select gibi değiştirmekten çekinmeyin **yürütme**.

    ![Günlük örnek erişim - veri kümesi bir sorgu ile keşfedin](./media/hdinsight-apps-install-cask/log-query.png)

15. Sorgu tamamlandıktan sonra seçin **görünümü** Eylemler sütunundaki altında simgesi.

    ![Erişim günlüğü örneği - tamamlanmış görünüm sorgu](./media/hdinsight-apps-install-cask/log-query-view.png)

16. Sorgu sonuçları görürsünüz.

    ![Erişim günlüğü örneği - sorgu sonuçları](./media/hdinsight-apps-install-cask/log-query-results.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Cask belgeleri](http://cask.co/resources/documentation/).
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): HDInsight için yayımlanmamış bir HDInsight uygulamasının nasıl dağıtılacağını öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirin](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [HDInsight içinde boş kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümeleri erişmek ve test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.
