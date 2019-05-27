---
title: 'Öğretici: Azure Zaman Serisi Görüşleri ortamı oluşturma | Microsoft Docs'
description: Benzetilmiş aygıtlardan veri ile doldurulan bir zaman serisi görüşleri ortamı oluşturmayı öğrenin.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/26/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: b8b46db043113f29f559ad44855d19f0d6ca73c3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244151"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Öğretici: Azure Time Series Insights ortamı oluşturma

Bu öğreticide sanal cihazlardan veri ile doldurulan bir Azure zaman serisi görüşleri ortamı oluşturma işleminde size rehberlik eder. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Zaman serisi görüşleri ortamı oluşturun.
> * IOT hub'ı içeren bir cihaz benzetimi çözümü oluşturun.
> * Zaman serisi görüşleri ortamına IOT hub'a bağlayın.
> * Cihaz benzetimi zaman serisi görüşleri ortamına veri akışı için çalıştırın.
> * Sanal telemetri verileri doğrulayın.

## <a name="video"></a>Video

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Bir Azure IOT Çözüm Hızlandırıcısı, verileri oluşturmak ve zaman serisi görüşleri ile başlamak için kullanmayı öğrenin. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Azure oturum açma hesabınızı ayrıca aboneliğin bir üyesi olmanız gerekir **sahibi** rol. Daha fazla bilgi için [erişim, rol tabanlı erişim denetimi ve Azure portalını kullanarak yönetme](/azure/role-based-access-control/role-assignments-portal).

## <a name="overview"></a>Genel Bakış

Zaman serisi görüşleri ortamı, cihaz verileri burada toplanan ve depolanan girebiliriz. Veriler depolandıktan sonra [Azure Time Series Insights gezgininin](time-series-quickstart.md) ve [zaman serisi öngörüleri sorgu API'si](/rest/api/time-series-insights/ga-query-api) sorgulamak ve verileri çözümlemek için kullanılabilir. Azure IOT Hub, tüm cihazlar tarafından (sanal veya fiziksel) güvenli bir şekilde bağlanın ve Azure bulut veri iletmek için kullanılan bağlantı noktasıdır. [Time Series Insights genel bakış](time-series-insights-overview.md) Azure IOT Hub ayrıca bir zaman serisi görüşleri ortamına veri akışı için bir olay kaynağı görevi gören, notlar. Bu öğreticide bir [IOT Çözüm Hızlandırıcısı](/azure/iot-accelerators/) oluşturup örnek telemetri verilerini IOT hub'ına akış.

>[!TIP]
> IOT Çözüm Hızlandırıcıları, özel IOT çözümlerinin geliştirilmesini hızlandırmanızı sağlayan kullanabileceğiniz kurumsal sınıf, önceden yapılandırılmış çözümler sunar.

## <a name="create-an-environment"></a>Ortam oluşturma

İlk olarak, Azure aboneliğinizde bir zaman serisi görüşleri ortamı oluşturun.

1. Oturum [Azure portalında](https://portal.azure.com) Azure abonelik hesabınızı kullanarak. 
1. Üstteki menüden **+ Kaynak oluştur**'u seçin. 
1. Seçin **nesnelerin interneti** kategori tıklayın ve ardından **Time Series Insights**. 

   [![Time Series Insights ortam kaynağını seçin](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. Üzerinde **zaman serisi görüşleri ortamına** sayfasında, gerekli parametreleri doldurun.

   Parametre|Açıklama
   ---|---
   **Ortam adı** | Zaman serisi görüşleri ortamı için benzersiz bir ad seçin. Adlar, Time Series Insights Gezgini ve sorgu API'leri tarafından kullanılır.
   **Abonelik** | Abonelikler, Azure kaynaklarına yönelik kapsayıcılardır. Zaman serisi görüşleri ortamı oluşturmak için bir abonelik seçin.
   **Kaynak grubu** | Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. Mevcut bir kaynak grubu seçin veya zaman serisi görüşleri ortamı kaynak için yeni bir tane oluşturun.
   **Konum** | Zaman serisi görüşleri ortamınız için bir veri merkezi bölgesini seçin. Ek bant genişliği maliyetlerini ve gecikme süresini önlemek için zaman serisi görüşleri ortamına diğer IOT kaynaklar aynı bölgede bulundurun.
   **Fiyatlandırma SKU'su** | Gerekli aktarım hızını seçin. Düşük maliyet ve başlangıç kapasitesi için `S1`.
   **Kapasite** | Kapasite; giriş oranına, depolama kapasitesine ve seçili SKU ile ilişkili maliyete uygulanan çarpandır. Kapasiteyi oluşturulduktan sonra değiştirebilirsiniz. En düşük maliyeti, kapasitesi 1'i seçin.

   İşiniz bittiğinde seçin **Oluştur** sağlama işlemini başlatın.

   [![Zaman serisi görüşleri ortamı kaynak oluştur](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Denetleme **bildirimleri** paneli dağıtım tamamlanmasını izlemek için. 

   [![Zaman serisi görüşleri ortamına dağıtım başarılı](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="create-a-device-simulation"></a>Cihaz benzetimi oluşturma

Ardından, zaman serisi görüşleri ortamınıza doldurmak için test verilerini oluşturur cihaz benzetimi çözümü oluşturun.

1. Ayrı penceresi veya sekmesinde, Git [azureiotsolutions.com](https://www.azureiotsolutions.com). Aynı Azure aboneliği hesabını kullanarak oturum açın ve seçin **cihaz benzetimi** Hızlandırıcı.

   [![Cihaz benzetimi Hızlandırıcı çalıştırın](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Gerekli Parametreler girin **cihaz benzetimi oluşturma çözümü** sayfası.

   Parametre|Açıklama
   ---|---
   **Çözüm adı** | Bu benzersiz bir değer, yeni bir kaynak grubu oluşturmak için kullanılır. Listelenen Azure kaynakları oluşturulur ve kaynak grubuna atanır.
   **Abonelik** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı aboneliği belirtin.
   **Bölge** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı bölge belirtin.
   **İsteğe bağlı Azure Kaynaklarını dağıtma** | Bırakın **IOT hub'ı** işaretli. Sanal cihazlar bağlanmak veya veri akışı için kullanın.

   İşlemi tamamladığınızda, seçin **çözüm oluşturma** çözümün Azure kaynakları sağlamak için. İşlem, bu işlemi tamamlamak için 6-7 dakika sürebilir.

   [![Cihaz benzetimi çözümü sağlanamadı](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Sağlama tamamlandıktan sonra yeni çözümünüzü yukarıda metin değişiklikleri **sağlama** için **hazır**.

   >[!IMPORTANT]
   > Seçmeyin **başlatma** henüz! Buna daha sonra geri çünkü bu web sayfasını açık tutun.

   [![Cihaz benzetimi çözüm tam sağlama](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Şimdi Azure portalına geri dönün ve yeni oluşturulan kaynakları denetleyin. Portalda **kaynak grupları** sayfasında, yeni bir kaynak grubu kullanılarak oluşturulmuş fark **çözüm adı** son adımda sağlanan. Ayrıca cihaz benzetimi çözümünü desteklemek için oluşturulan tüm kaynakları dikkat edin.

   [![Cihaz benzetimi çözüm kaynakları](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-environment-to-the-iot-hub"></a>IOT hub'ına ortama bağlanın

Bu noktada, her biri kendi kaynak grubunda bulunan iki kaynak kümesi oluşturmayı öğrendiniz:

- Boş bir zaman serisi görüşleri ortamı.
- Çözüm Hızlandırıcısını tarafından oluşturulan bir IOT hub'ı içeren cihaz benzetimi çözüm kaynaklar.

Sanal cihazların cihaz verilerini akışla aktarmak için bir IoT hub'ına bağlanması gerektiğini unutmayın. Zaman serisi görüşleri ortamına veri akışı için hem IOT hub'ınıza hem de zaman serisi görüşleri ortamı için yapılandırma değişiklikleri yapmanız gerekir.

### <a name="iot-hub-configuration-define-a-consumer-group"></a>IOT hub'ı yapılandırma: Bir tüketici grubu tanımlayın

IOT hub'ı diğer aktörler işlevsellik paylaşmak için çeşitli uç noktaları sağlar. "Olaylar" uç noktası, bir IOT hub örneğine yeniden düşüklüğü yaşanacaktır verileri kullanan diğer uygulamalar için bir yol sağlar. Özellikle, "tüketici grupları" uygulamalar ve IOT hub'ından veri çekmek için bir mekanizma sağlar.

Ardından, yeni bir tanımladığınız **tüketici grubu** özelliği IOT hub'ı cihaz benzetimi çözümün **olaylar uç noktasına**.

1. Azure portalında Git **genel bakış** cihaz benzetimi çözümü oluşturduğunuz kaynak grubunun sayfası. IOT hub'ı kaynağını seçin.

   [![Cihaz benzetimi çözümü kaynak grubu](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Not **adı** çözümü için oluşturulan IOT hub'ı kaynak. Buna daha sonra başvurmanız.

1. Aşağı kaydırın ve select **uç noktaları** sayfasında ve ardından **olayları** uç noktası. Uç nokta üzerinde **özellikleri** sayfasında, "$Default" tüketici grubundaki uç noktanız için benzersiz bir ad girin. **Kaydet**’i seçin.

   [![Cihaz benzetimi çözümü IoT hub uç noktaları](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="environment-configuration-define-an-event-source"></a>Ortam yapılandırma: bir olay kaynağı tanımlayın

Artık, yeni IOT hub'ı bağlantı **tüketici grubu** olay uç noktası zaman serisi görüşleri ortamına bir **olay kaynağı**.

1. Git **genel bakış** zaman serisi görüşleri ortamı için oluşturduğunuz kaynak grubunun sayfası. Zaman serisi görüşleri ortamı seçin.

   [![Zaman serisi görüşleri ortamı kaynak grubu ve ortam](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

1. Zaman serisi görüşleri ortamı sayfasında **olay kaynakları**. Ardından **+ Ekle**.

   [![Zaman serisi görüşleri ortamına genel bakış](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png#lightbox)

1. Gerekli Parametreler girin **yeni olay kaynağı** sayfası.

   Parametre|Açıklama
   ---|---
   **Olay kaynağı adı** | Olay kaynağını adlandırmak için kullanılan benzersiz bir değer gerektirir.
   **Kaynak** | Seçin **IOT hub'ı**.
   **İçeri aktarma seçeneği** | Varsayılan seçin `Use IoT hub from available subscriptions`. Bu seçenek kullanılabilir abonelikler ile doldurulacak sonraki açılır listede neden olur.
   **Abonelik** | Cihaz benzetimi kaynaklar ve zaman serisi görüşleri ortamı oluşturulan aynı aboneliği seçin.
   **Iot hub adı** | Varsayılan olarak daha önce not ettiğiniz IoT hub'ının adı olmalıdır. Değilse, doğru IoT hub'ını seçin.
   **Iot hub ilke adı** | Seçin **iothubowner**.
   **Iot hub tüketici grubu** | Varsayılan olarak daha önce oluşturduğunuz IoT hub'ı tüketici grubunun adı olmalıdır. Değilse, doğru tüketici grubu adını seçin.
   **Olay serileştirme biçimi** | Varsayılan haline getirilen değeri olarak bırakın `JSON`.
   **Zaman damgası özellik adı** | Olarak belirttiğiniz `timestamp`.

   İşlemi tamamladığınızda, seçin **Oluştur** olay kaynağı ekleyin. Kaynak grubunu döndüğünüzde **genel bakış** sayfasında, zaman serisi görüşleri ortamı kaynak birlikte yeni bir "Zaman serisi görüşleri olay kaynağı" kaynak görürsünüz.

   [![Zaman serisi görüşleri ortamı yeni olay kaynağı](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png#lightbox)

## <a name="run-device-simulation-to-stream-data"></a>Cihaz benzetimi için veri akışı çalıştırın.

Tüm yapılandırma işi tamamlandığında, zaman serisi görüşleri ortamına benzetilmiş aygıtlardan örnek verilerle doldurmak için zaman var.

Geri çağırma [cihaz benzetimi bölüm oluşturmak](#create-a-device-simulation), çeşitli Azure kaynaklarını çözümünü desteklemek için Hızlandırıcı tarafından oluşturulmuş. Daha önce ele alınan IoT hub’ı ile birlikte, sanal cihaz telemetrisi oluşturmak ve aktarmak için bir Azure App Service web uygulaması oluşturulmuştur.

1. [Çözüm hızlandırıcıları panonuza](https://www.azureiotsolutions.com/Accelerators#dashboard) geri dönün. Tekrar, gerekirse, bu öğreticide kullandığınız aynı Azure hesabını kullanarak oturum açın. Seçebileceğiniz artık **başlatma** "Cihaz benzetimi" çözümünüzü altında.

     [![Çözüm Hızlandırıcıları Panosu](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. Bu noktasını ve cihaz benzetimi web uygulama başlatılır, ilk yükleme sırasında birkaç saniye sürebilir. Ayrıca web uygulaması "oturumunuzu açma ve profilinizi okuma" izni izin istenir izni. Bu izin, uygulamanın çalışmasını desteklemek için gereken kullanıcı profili bilgilerini almak uygulama izin verir.

     [![Cihaz benzetimi web uygulama onay](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Sonra **benzetimi Kurulum** sayfa yüklemelerinin, gerekli parametreleri girin.

   Parametre|Açıklama
   ---|---
   **Hedef IoT Hub'ı** | Seçin **kullanın, IOT hub'ı önceden sağlanan**.
   **Cihaz modeli** | Seçin **Soğutucu**.
   **Cihaz sayısı**  | Girin `1000` altında **tutarı**.
   **Telemetri sıklığı** | Girin `10` saniye.
   **Benzetim süresi** | Seçin **biten:** girin `5` dakika.

   İşlemi tamamladığınızda, seçin **benzetimi Başlat**. Benzetim toplamda 5 dakika boyunca çalışır. 1000 sanal cihazlardan veri 10 saniyede oluşturur. 

   [![Cihaz benzetimi Kurulumu](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Benzetim çalışırken dikkat **toplam iletileri** ve **saniye başına ileti** alanları güncelleştirin, yaklaşık her 10 saniye. Benzetim yaklaşık 5 dakika sonra sona erer ve size geri döndürür **benzetimi Kurulum**.

   [![Çalıştıran cihaz benzetimi](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>Telemetri verilerini doğrulama

Bu son bölümde telemetri verilerini oluşturulur ve zaman serisi görüşleri ortamına depolanan olduğunu doğrulayın. Verileri doğrulamak için, telemetri verilerini sorgulamak ve çözümlemek için kullanılan Time Series Insights gezgininden yararlanın.

1. İade zaman serisi görüşleri ortamının kaynak grubuna **genel bakış** sayfası. Zaman serisi görüşleri ortamı seçin.

   [![Zaman serisi görüşleri ortamı kaynak grubu ve ortam](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. Zaman serisi görüşleri ortamına **genel bakış** sayfasında **Time Series Insights Gezgini URL** Time Series Insights gezginini açın.

   [![Time Series Insights Gezgini](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. Time Series Insights gezgininin yükler ve Azure portalı hesabınıza kullanarak kimliğini doğrular. Başlangıç görünümü üzerinde zaman serisi görüşleri ortamına sanal telemetri verilerle doldurulmuş olan grafik alanında görebilirsiniz. Dar bir zaman aralığı filtrelemek için sol üst köşedeki aşağı açılan seçin. Cihaz benzetimi süresi span kadar büyük bir zaman aralığı girin. Daha sonra arama Büyüteç'i seçin.

   [![Zaman serisi görüşleri Gezgini zaman aralığı filtresi](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. Zaman aralığı daraltma ayrı artışları IOT hub'ı ve zaman serisi görüşleri ortamına veri aktarımı için yakınlaştırmanız grafiği sağlar. Ayrıca **akış tamamlandı** metin sağ üst köşede, bulunan olaylarının toplam sayısı gösterilmektedir. Ayrıca sürükleyebilirsiniz **aralık boyutu** kaydırıcı çizim ayrıntı grafiğine denetlemek için.

   [![Zaman serisi görüşleri Gezgini zaman aralığı filtrelenmiş görünüm](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Son olarak, bir aralık filtrelemek için bir bölge sol. Ardından sağ tıklayın ve kullanmak **olayları keşfet** Olay Ayrıntıları sekmeli gösterilecek **olayları** görünümü.

   [![Görünüm ve olayları zaman serisi görüşleri Gezgini zaman aralığı filtrelendi](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide zaman serisi görüşleri ortamı ve cihaz benzetimi çözümünü desteklemek için birden fazla çalışan Azure hizmetini oluşturur. İptal veya Bu öğretici serisinin sayesinde yalnızca işinize erteleme istiyorsanız, gereksiz yinelenen maliyetler oluşmasını önlemek için tüm kaynakları silin.

Azure portalında sol taraftaki menüden:

1. Seçin **kaynak grupları** simgesi. Ardından, zaman serisi görüşleri ortamı için oluşturduğunuz kaynak grubunu seçin. Sayfanın üst kısmında seçin **kaynak grubunu Sil**, kaynak grubunun adını girin ve seçin **Sil**.

1. Seçin **kaynak grupları** simgesi. Ardından cihaz benzetimi çözüm Hızlandırıcı tarafından oluşturulan kaynak grubunu seçin. Sayfanın üst kısmında seçin **kaynak grubunu Sil**, kaynak grubunun adını girin ve seçin **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Zaman serisi görüşleri ortamı oluşturun.
> * IOT hub'ı içeren bir cihaz benzetimi çözümü oluşturun.
> * Zaman serisi görüşleri ortamına IOT hub'a bağlayın.
> * Cihaz benzetimi zaman serisi görüşleri ortamına veri akışı için çalıştırın.
> * Sanal telemetri verileri doğrulayın.

Kendi Time Series Insights ortamınızı oluşturmaya nasıl artık bildiğinize göre zaman serisi görüşleri ortamından veri kullanan bir web uygulaması oluşturmayı öğrenin:

> [!div class="nextstepaction"]
> [Azure Time Series Insights tek sayfalı web uygulaması oluşturma](tutorial-create-tsi-sample-spa.md)
