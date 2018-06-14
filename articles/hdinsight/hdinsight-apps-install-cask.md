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
ms.openlocfilehash: 9eef1760b7cee3bbdf33122514669b38b0b4d9db
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31400852"
---
# <a name="install-published-application---cask-data-application-platform-cdap"></a>Yayımlanan uygulama - Cask veri uygulama Platformu (CDAP) yükleyin

Bu makalede yüklemek ve çalıştırmak nasıl [CDAP](http://cask.co/products/cdap/) Azure hdınsight'ta Hadoop uygulama yayımlanır. Hdınsight uygulaması platformu genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için yayımlanan uygulamalar bkz [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-cdap"></a>CDAP hakkında

Hadoop uygulamaları geliştirme zor olabilir.  Tümleştirmek için biraz zaman alabilir Hadoop teknoloji uzantıları büyük ve artan bir sayısı yoktur. Veri akışı izleme ve ölçümleri toplama ayrı bir çözüm oluşturmaya gerek duyabilirsiniz.

### <a name="how-does-cdap-help"></a>CDAP nasıl yardımcı olur?

Cask veri uygulama Platformu (CDAP) bir büyük veri tümleştirme platformudur. CDAP odaklanmak uygulamaları oluşturmak yerine altyapının sağlar.

Üst düzey kavramlarını ve geliştiricilerinin aşina soyutlamalar CDAP kullanır. Bu soyutlama dahili sistem karmaşıklığını gizleme ve çözümleri kullanılırlığı teşvik edin.

CDAP uzantı adlandırılan [Cask Hydrator](http://cask.co/products/hydrator/) geliştirmek ve veri ardışık yönetmek için bir kullanıcı arabirimi sağlar. Veri ardışık çeşitli oluşan * görevleri eklentileri gibi veri alma, dönüştürme, analiz ve çalıştırma sonrası işlemleri.

Böylece farklı teknolojiler değerlendirme uygulamanın geri kalanına touch gerek kalmadan başka bir bir eklenti yerine, yalnızca bir konudur her CDAP eklentisi iyi tanımlanmış bir arabirimine sahiptir.

CDAP *ardışık düzen* veri uygulamanızdaki bir üst düzey resimsel akışını sağlar. Bu görselleştirmenin veri dönüşümleri ve çözümlemeler, aracılığıyla alım verilerden uçtan uca akışını gösterir ve son olarak bir dış veri depolamak.

Aşağıdaki örnek veri ardışık gerçek zamanlı twitter verileri alır ve ardından önceden tanımlanmış ölçütlere göre bazı tweet'leri çıkışı filtreler. Veri ardışık ham tweet verileri dönüştüren ve daha okunabilir bir biçime veri sonra tweet'leri değer kümesini göre gruplandırır ve sonuçları bir HBase Yazar projeleri depolar.

![CDAP ardışık düzen](./media/hdinsight-apps-install-cask/pipeline.png)

Bu uçtan uca ardışık düzen kullanılarak oluşturulan **Cask Hydrator UI**, her aşamanın arasında bağlantı oluşturmalarını eklentisi arabirimi ve sürükle ve bırak işlevlerini kullanarak. Yalıtmak ve her eklentinin işlevselliğini bağımsız olarak değiştirin. CDAP kullanarak, benzer ardışık düzen oluşturulur ve saat olarak doğrulandı. Tipik Hadoop dünyasında bu tür çözümler oluşturmak birkaç gün sürebilir.

CDAP de adlı bir uzantı sağlar [Cask İzleyicisi](http://cask.co/products/tracker/) görsel olarak izleme verilerine şekliyle üzerinden uygulama akar. Cask İzleyicisi ekler *veri İdaresi* sisteme böylece veri varlıklarını uygulama genelinde resmi olarak yönetilir. Her veri noktasının çizgileri izlemek, ilgili ölçümleri toplamak ve denetim işlemi boyunca veri kaydı.

Veri yukarıdaki ardışık düzeninde nasıl boyunca çizim şöyledir:

![CDAP İzleyicisi](./media/hdinsight-apps-install-cask/tracker.png)

## <a name="prerequisites"></a>Önkoşullar

Yeni bir Hdınsight kümesi veya varolan bir kümenin bu uygulamayı yüklemek için aşağıdaki yapılandırmaya sahip olmalısınız:

* Küme katmanı: standart
* Küme türü: HBase
* Küme sürümü: 3.4, 3.5

## <a name="install-the-cdap-published-application"></a>Uygulama yükleme CDAP yayımlanır

Bu ve diğer kullanılabilir ISV uygulamaları yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-cdap"></a>CDAP başlatma

1. Yükleme sonrasında, CDAP Azure portalında, kümeden giderek başlatın **ayarları** bölmesinde, ardından seçerek **uygulamaları** altında **genel** kategorisi. Yüklü uygulamalar bölmesinde tüm yüklü uygulamalar listelenir.

    ![Yüklü CDAP uygulama](./media/hdinsight-apps-install-cask/cdap-app.png)

2. CDAP seçtiğinizde, bir web sayfası ve HTTP uç noktası ve ayrıca SSH uç noktası yolu bağlantısına bakın. Web sayfası bağlantıyı seçin.

3. İstendiğinde, Küme Yöneticisi kimlik bilgilerinizi girin.

    ![Kimlik Doğrulaması](./media/hdinsight-apps-install-cask/auth.png)

4. Oturum açtıktan sonra Cask CDAP GUI giriş sayfasına bakın.

    ![Cask GUI giriş sayfası](./media/hdinsight-apps-install-cask/gui.png)

5. CDAP arabirimi keşfetmek için tıklatın **Cask Pazar** menü bağlantısı sayfanın en üstünde.

    ![Cask Pazar bağlantı](./media/hdinsight-apps-install-cask/cask-market.png)

6. Seçin **erişim günlüğü örneği** listeden.

    ![Erişim günlüğü örneği](./media/hdinsight-apps-install-cask/market-log-sample.png)

7. Tıklatın **yük** onaylamak için.

    ![Yükle'yi tıklatın](./media/hdinsight-apps-install-cask/market-load.png)

8. Dahil edilmiş örnek veri görünümü görüntülenir. Seçin **sonraki**.

    ![Günlük örnek - görünüm veri erişimi](./media/hdinsight-apps-install-cask/market-view-data.png)

9. Seçin **akış** hedef türü olarak hedef adı girin ve ardından **son**.

    ![Erişim günlüğü örneği - hedefi seçin](./media/hdinsight-apps-install-cask/market-destination.png)

10. Datapack başarıyla yüklendikten sonra seçin **akış ayrıntıları görüntüle**.

    ![Datapack başarıyla karşıya yüklendi](./media/hdinsight-apps-install-cask/market-view-details.png)

11. Ad alanı için meta veri etkinleştirmek için seçin **etkinleştirmek** kullanım sekmesinde erişim günlüğüne Ayrıntıları sayfasında.

    ![-Yüklendi - erişim günlüğü örneği meta verilerini etkinleştir](./media/hdinsight-apps-install-cask/log-loaded.png)

12. Meta veri etkinleştirildikten sonra denetim iletisi bilgileri görüntüleyen bir grafik görürsünüz.

    ![Günlük örnek - etkin meta veri erişimi](./media/hdinsight-apps-install-cask/log-metadata.png)

13. Günlük verileri araştırmak için seçin **Araştır** sayfanın en üstünde simgesi.

    ![Erişim günlük örnek - keşfedin](./media/hdinsight-apps-install-cask/log-explore.png)

14. Bir örnek SQL sorgu bakın. İstenen sonra select olarak değiştirmekten çekinmeyin **yürütme**.

    ![Günlük örnek erişim - bir sorgu ile dataset keşfedin](./media/hdinsight-apps-install-cask/log-query.png)

15. Sorgu tamamlandıktan sonra seçin **Görünüm** Eylemler sütununun altında simgesi.

    ![Erişim günlüğü örneği - tamamlandı görünümü sorgusu](./media/hdinsight-apps-install-cask/log-query-view.png)

16. Sorgu sonuçlarına bakın.

    ![Erişim günlüğü örneği - sorgu sonuçları](./media/hdinsight-apps-install-cask/log-query-results.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Cask belgeleri](http://cask.co/resources/documentation/).
* [Özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir Hdınsight uygulamasının Hdınsight'a nasıl yükleneceğini öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı Hdınsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [Hdınsight'ta boş kenar düğümünü kullanmak](hdinsight-apps-use-edge-node.md): Hdınsight kümeleri erişmek ve test etmek ve Hdınsight uygulamalarını barındırmak için bir boş kenar düğümüne kullanmayı öğrenin.
