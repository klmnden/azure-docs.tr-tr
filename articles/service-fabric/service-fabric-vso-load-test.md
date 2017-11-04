---
title: "Yük testi uygulamanızı Visual Studio Team Services kullanarak | Microsoft Docs"
description: "Test stres testi uygulamak Azure Service Fabric uygulamalarınızı Visual Studio Team Services kullanarak öğrenin."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: fc743585-0d1b-483f-981d-493f4552ac07
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: e8e270ce865d4da3ee219958b308db2c1c89b11b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Yük, Visual Studio Team Services kullanarak uygulamanızı test etme
Bu makalede test stres testi uygulamak için Microsoft Visual Studio Yük Testi özellikleri kullanmak bir uygulama gösterilmektedir. Bir Azure Service Fabric durum bilgisi olan hizmet arka uç ve bir durum bilgisi olmayan web ön uç kullanır. Burada kullanılan örnek bir uçak konumu simulator uygulamasıdır. Bir uçak kimliği, ayrılma süresi ve hedef sağlayın. Uygulamanın arka uç isteklerini işleyen ve ön uç ölçütlerle eşleşen uçak haritada görüntüler.

Aşağıdaki diyagramda, test Service Fabric uygulaması gösterilmektedir.

![Örnek uçak konumu uygulama diyagramı][0]

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce aşağıdakileri yapmanız gerekir:

* Visual Studio Team Services hesabı edinin. Ücretsiz adresindeki edinebilirsiniz [Visual Studio Team Services](https://www.visualstudio.com).
* Alma ve Visual Studio 2013 veya Visual Studio 2015 yükleyin. Bu makalede Visual Studio 2015 Enterprise edition kullanır, ancak Visual Studio 2013 ve diğer sürümleri benzer şekilde çalışması gerekir.
* Hazırlama ortamında uygulamanıza dağıtın. Bkz: [Visual Studio kullanarak uzak bir küme için uygulamaların nasıl dağıtılacağı](service-fabric-publish-app-remote-cluster.md) bu hakkında bilgi için.
* Uygulamanızın kullanım deseniyle anlayın. Bu bilgiler, yük düzeni benzetimini yapmak için kullanılır.
* Yük testi için hedef anlayın. Bu, yorumlar ve yük testi sonuçlarını analiz yardımcı olur.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Oluşturma ve Web performansı ve yük testi projesi çalıştırma
### <a name="create-a-web-performance-and-load-test-project"></a>Bir Web performansı ve yük testi projesi oluşturma
1. Visual Studio 2015’i açın. Seçin **dosya** > **yeni** > **proje** açmak için menü çubuğunda **yeni proje** iletişim kutusu.
2. Genişletme **Visual C#** düğümü seçin **Test** > **Web performansı ve yük testi projesi**. Proje bir ad verin ve ardından **Tamam** düğmesi.

    ![Yeni Proje iletişim kutusunun ekran görüntüsü][1]

    Çözüm Gezgini'nde yeni bir Web performansı ve yük testi projesi görmeniz gerekir.

    ![Yeni Proje gösteren Çözüm Gezgini ekran görüntüsü][2]

### <a name="record-a-web-performance-test"></a>Web performans testini kaydetme
1. .Webtest projesini açın.
2. Seçin **ekleme kaydı** tarayıcınızda kayıt oturumu başlatmak için simge.

    ![Bir tarayıcıda kayıt Ekle simgesi ekran görüntüsü][3]

    ![Bir tarayıcıda kayıt düğmesinin Ekran görüntüsü][4]
3. Service Fabric uygulamaya göz atın. Kayıt Masası web istekleri göstermesi gerekir.

    ![Web isteklerini kaydı panelinde ekran görüntüsü][5]
4. Bir dizi gerçekleştirmelerini beklediğiniz eylemi gerçekleştirin. Bu eylemler kalıp olarak yük oluşturmak için kullanılır.
5. İşiniz bittiğinde seçin **durdurmak** durdurmanıza düğmesi.

    ![Durdur düğmesinin Ekran görüntüsü][6]

    Visual Studio'da .webtest projesi, bir dizi isteğin yakalanan. Dinamik parametreleri otomatik olarak değiştirilir. Bu noktada, test senaryonuz parçası olmayan herhangi bir ek, yinelenen bağımlılık isteğinin silebilirsiniz.
6. Projeyi kaydedin ve ardından **testi Çalıştır** web performans testini yerel olarak çalıştırın ve her şeyi emin olmak için komut doğru çalışır.

    ![Test çalışması komutu ekran görüntüsü][7]

### <a name="parameterize-the-web-performance-test"></a>Web performans testini Parametreleştirme
Kodlanmış web performans testine dönüştürme ve kod düzenleme web performans testini Parametreleştirme. Alternatif olarak, böylece test verilerine tekrarlanan bir veri listesine web performans testi bağlayabilirsiniz. Bkz: [oluşturma ve çalıştırma kodlanmış web performans testine](https://msdn.microsoft.com/library/ms182552.aspx) web performans dönüştürmek nasıl kullanılacağı hakkındaki ayrıntıları Kodlanmış testi için test etmek için. Bkz: [bir veri kaynağı bir web performans testine ekleme](https://msdn.microsoft.com/library/ms243142.aspx) bir web performans testine veri bağlama hakkında bilgi için.

Uçak kimliği oluşturulan GUID ile değiştirin ve farklı konumlara uçuşlar göndermek için daha fazla isteği eklemek için bu örnekte, biz web performans testi kodlanmış testine dönüştürmeniz.

### <a name="create-a-load-test-project"></a>Bir yük testi projesi oluşturma
Web performans testi ve ek belirtilen yük test ayarları ile birlikte birim testi tarafından açıklanan bir veya daha fazla senaryo, bir yük testi projesi oluşur. Aşağıdaki adımlar, bir yük testi projesi oluşturma gösterir:

1. Web performans ve yük testi proje kısayol menüsünden seçin **Ekle** > **yük testi**. İçinde **yük testi** Sihirbazı'nı seçin **sonraki** düğmesi test ayarlarını yapılandırın.
2. İçinde **yük düzeni** bölümünde, bir sabit kullanıcı yük veya birkaç kullanıcı ile başlar ve zaman içinde kullanıcıları artıran bir adım yük isteyip istemediğinizi seçin.

    İyi bir kullanıcı yük miktarı tahmin varsa ve geçerli sistem nasıl gerçekleştireceğini görmek istediğiniz seçin **sabit yük**. Amacınız öğrenmek için ise olup sistem gerçekleştirir sürekli olarak çeşitli yük altında seçin **Adım yük**.
3. İçinde **Test karışımı** bölümünde, seçin **Ekle** düğmesine tıklayın ve ardından yük testinde dahil etmek istediğiniz test seçin. Kullanabileceğiniz **dağıtım** sütun toplam testleri her test için Çalıştır yüzdesini belirtin.
4. İçinde **çalıştırma ayarları** bölümünde, yük test süresi belirtin.

   > [!NOTE]
   > **Test Yinelemeleri** seçenek, kullanılabilir yerel olarak Visual Studio kullanarak bir yük testi çalıştırdığınızda.
   >
   >
5. İçinde **konumu** bölümünü **çalıştırma ayarları**, yük testi istekleri üretilen burada konumu belirtin. Sihirbaz Team Services hesabınıza oturum açmanızı isteyebilir. Oturum açın ve ardından bir coğrafi konumu seçin. İşiniz bittiğinde seçin **son** düğmesi.
6. Yük testi oluşturulduktan sonra .loadtest projeyi açın ve ayarı çalıştırmak geçerli seçin **çalıştırma ayarları** > **çalıştırma Ayarları1 [etkin]**. Bu çalışma ayarlarında açar **özellikleri** penceresi.
7. İçinde **sonuçları** bölümünü **çalıştırma ayarları** Özellikler penceresi **zamanlama ayrıntıları depolama** ayarı olmalıdır **hiçbiri** , varsayılan değer olarak. Bu değeri değiştirmek **Tüm Bireysel Ayrıntılar** test sonuçlarını yükleme hakkında daha fazla bilgi almak için. Bkz: [yük testi](https://www.visualstudio.com/load-testing.aspx) Visual Studio Team Services'e bağlanmak ve yük testi çalıştırma hakkında daha fazla bilgi için.

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Visual Studio Team Services kullanarak yük testi çalıştırma
Seçin **yük testi Çalıştır** test çalışmasını başlatmak için komutu.

![Yük testi çalıştırma komutu ekran görüntüsü][8]

## <a name="view-and-analyze-the-load-test-results"></a>Görüntüleme ve yük testi sonuçlarını çözümleme
Performans bilgilerini grafiği çizilecek, yükü olarak ilerledikçe test. Aşağıdaki grafiğe benzer bir şey görmeniz gerekir.

![Yük testi sonuçlarını performans grafiği ekran görüntüsü][9]

1. Seçin **karşıdan rapor** sayfanın üstüne yakın bağlantı. Raporun yüklendikten sonra Seç **görüntülemek rapor** düğmesi.

    Üzerinde **grafik** sekmesini çeşitli performans sayaçları için grafikleri görebilirsiniz. Üzerinde **Özet** sekmesinde, genel test sonuçları görüntülenir. **Tabloları** sekmesini başarılı ve başarısız yük testleri toplam sayısını gösterir.
2. Sayı bağlantılar seçin **Test** > **başarısız** ve **hataları** > **sayısı** sütunları hatası ayrıntılarına bakın.

    **Ayrıntı** sekmesi başarısız istekler için sanal kullanıcı ve test senaryosu bilgilerini gösterir. Bu veriler birden çok senaryolar yük testi içeriyorsa, yararlı olabilir.

Bkz: [yük testi sonuçları çözümleme Yük Testi Çözümleyicisinin Grafik görünümünde](https://www.visualstudio.com/load-testing.aspx) yük görüntüleme hakkında daha fazla bilgi için test sonuçları.

## <a name="automate-your-load-test"></a>Yük testlerini otomatikleştirme
Visual Studio Team Services yük sınama yük testleri yönetmenize ve bir Team Services hesabında sonuçlarını analiz yardımcı olmak için API'ler sağlar. Bkz: [bulut yük test etme Rest API'leri](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
* [İzleme ve yerel makine geliştirme Kurulum Hizmetleri'nde tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
