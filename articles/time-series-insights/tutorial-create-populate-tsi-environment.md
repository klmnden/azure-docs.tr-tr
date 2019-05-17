---
title: 'Öğretici: Azure Zaman Serisi Görüşleri ortamı oluşturma | Microsoft Docs'
description: Sanal cihazlardan verilerle doldurulmuş bir Time Series Insights ortamı oluşturma hakkında bilgi edinin.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/26/2019
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 3c097ec74f45248a41c6dd4df80cbbd927a9b5ed
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827453"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Öğretici: Azure Time Series Insights ortamı oluşturma

Bu öğreticide, sanal cihazlar verilerle doldurulmuş bir zaman serisi görüşleri ortamı oluşturma işlemi boyunca size yol gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Time Series Insights ortamı oluşturma 
> * IoT Hub’ı içeren bir cihaz benzetimi çözümü oluşturma
> * Zaman serisi görüşleri ortamına IOT hub'a bağlama
> * Cihaz benzetimi için veri akışı zaman serisi görüşleri ortamına çalıştırın.
> * Sanal telemetri verilerini doğrulama

## <a name="video"></a>Video

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Bir Azure IOT Çözüm Hızlandırıcısı, verileri oluşturmak ve zaman serisi görüşleri ile başlamak için kullanmayı öğrenin. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

* Azure oturum açma hesabınızın ayrıca aboneliğin bir üyesi olmanız gerekir **sahibi** rol. Bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](/azure/role-based-access-control/role-assignments-portal) daha fazla ayrıntı için.

## <a name="overview"></a>Genel Bakış

Zaman serisi görüşleri ortamı, cihaz verileri burada toplanan ve depolanan girebiliriz. Zaman serisi görüşleri ortamına saklanmaya başlanan [Time Series Insights Gezgini](time-series-quickstart.md) ve [zaman serisi öngörüleri sorgu API'si](/rest/api/time-series-insights/ga-query-api) sorgulamak ve verileri çözümlemek için kullanılabilir. IOT Hub, tüm cihazlar tarafından (sanal veya fiziksel) güvenli bir şekilde bağlanın ve Azure bulut veri iletmek için kullanılan bağlantı noktasıdır. [Zaman serisi öngörüleri'ne genel bakış](time-series-insights-overview.md) Azure IOT Hub ayrıca bir zaman serisi görüşleri ortamına veri akışı için bir olay kaynağı görevi gören, notlar. Bu öğreticide bir [IOT Çözüm Hızlandırıcısı](/azure/iot-accelerators/) oluşturup örnek telemetri verilerini IOT hub'ına akış.

>[!TIP]
> IOT Çözüm Hızlandırıcıları, özel IOT çözümlerinin geliştirilmesini hızlandırmanızı sağlayan kurumsal sınıf, önceden yapılandırılmış çözümler sunar.

## <a name="create-an-environment"></a>Ortam oluşturma

İlk olarak, Azure aboneliğinizde bir zaman serisi görüşleri ortamı oluşturun:

1. Azure abonelik hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.  
1. Üstteki menüden **+ Kaynak oluştur**'u seçin.  
1. **Nesnelerin İnterneti** kategorisini, ardından **Time Series Insights**’ı seçin.  

   [![Time Series Insights ortam kaynağını seçin](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. **Time Series Insights ortamı** sayfasında gerekli parametreleri doldurun:

   Parametre|Açıklama
   ---|---
   **Ortam adı** | Zaman serisi görüşleri ortamı için benzersiz bir ad seçin. Time Series Insights Gezgini ve sorgu API'leri tarafından kullanılan adı.
   **Abonelik** | Abonelikler, Azure kaynaklarına yönelik kapsayıcılardır. Zaman serisi görüşleri ortamı oluşturmak için bir abonelik seçin.
   **Kaynak grubu** | Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. Mevcut bir kaynak grubu seçin veya zaman serisi görüşleri ortamı kaynak için yeni bir tane oluşturun.
   **Konum** | Zaman serisi görüşleri ortamınız için bir veri merkezi bölgesini seçin. Ek bant genişliği maliyetlerini ve gecikme süresini önlemek için diğer IOT kaynaklarıyla aynı bölgede zaman serisi görüşleri ortamına tutmak en iyisidir.
   **Fiyatlandırma SKU'su** | Gerekli aktarım hızını seçin. Düşük maliyet ve başlangıç kapasitesi için `S1`.
   **Kapasite** | Kapasite; giriş oranına, depolama kapasitesine ve seçili SKU ile ilişkili maliyete uygulanan çarpandır.  Kapasiteyi oluşturulduktan sonra değiştirebilirsiniz. En düşük maliyeti, kapasitesi 1'i seçin.

   İşlem bittiğinde sağlama işlemini başlatmak için **Oluştur**'a tıklayın.

   [![Zaman serisi görüşleri ortamı kaynak oluştur](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Denetleme **bildirimleri** paneli dağıtım tamamlanmasını izlemek için.  

   [![Zaman serisi görüşleri ortamına dağıtım başarılı](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="create-a-device-simulation"></a>Cihaz benzetimi oluşturma

Ardından, zaman serisi görüşleri ortamınıza doldurmak için test verilerini oluşturacak cihaz benzetimi çözümü oluşturun:

1. Ayrı bir pencere/sekmesinde, Git [azureiotsolutions.com](https://www.azureiotsolutions.com). Aynı Azure aboneliği hesabını kullanarak oturum açın ve seçin **cihaz benzetimi** Hızlandırıcı.

   [![Cihaz benzetimi Hızlandırıcı çalıştırın](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Gerekli Parametreler girin **cihaz benzetimi oluşturma çözümü** sayfası.

   Parametre|Açıklama
   ---|---
   **Çözüm adı** | Yeni bir kaynak grubu oluşturmak için kullanılan benzersiz değer. Listelenen Azure kaynakları oluşturulur ve kaynak grubuna atanır.
   **Abonelik** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı aboneliği belirtin.
   **Bölge** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı bölge belirtin.
   **İsteğe bağlı Azure Kaynaklarını dağıtma** | Sanal cihazlar tarafından verileri bağlamak/akış yapmak için kullanılacağından **IoT Hub** seçeneğinin işaretini kaldırmayın.

   İşiniz bittiğinde tıklayın **çözüm oluşturma** çözümün Azure kaynakları sağlamak için. Bu, bu işlemi tamamlamak için 6-7 dakika sürebilir.

   [![Cihaz benzetimi çözümü sağlanamadı](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Sağlama tamamlandıktan sonra yeni çözümünüzü yukarıda metin değiştirilecek **sağlama...**  için **hazır**:

   >[!IMPORTANT]
   > **Başlat** düğmesine henüz tıklamayın! Ancak daha sonra döneceğiniz için bu web sayfasını açık tutun.

   [![Cihaz benzetimi çözüm tam sağlama](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Şimdi Azure portalına geri dönüp aboneliğinizde yeni oluşturulan kaynakları inceleyin. Portalın **Kaynak grupları** sayfasında, son adımda sağlanan **Çözüm adı** kullanılarak yeni bir kaynak grubu oluşturulduğunu fark edeceksiniz. Ayrıca, cihaz benzetimi çözümünü desteklemek için tüm kaynakların oluşturulduğuna dikkat edin:

   [![Cihaz benzetimi çözüm kaynakları](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-environment-to-the-iot-hub"></a>IOT hub'ına ortama bağlanın

Bu noktada, her biri kendi kaynak grubunda bulunan iki kaynak kümesi oluşturmayı öğrendiniz:

- Boş bir zaman serisi görüşleri ortamı.
- Çözüm Hızlandırıcısını tarafından oluşturulan bir IOT hub'ı dahil olmak üzere cihaz benzetimi çözüm kaynaklar.

Sanal cihazların cihaz verilerini akışla aktarmak için bir IoT hub'ına bağlanması gerektiğini unutmayın. Zaman serisi görüşleri ortamına veri akışı için IOT hub ve zaman serisi görüşleri ortamı için yapılandırma değişiklikleri yapmanız gerekir.

### <a name="iot-hub-configuration-define-a-consumer-group"></a>IoT hub’ı yapılandırma: tüketici grubu tanımlama

IoT hub’ı, diğer aktörlerle işlev paylaşmak için çeşitli uç noktalar sağlar. "Olaylar" uç noktası, bir IoT hub örneğine aktarılırken diğer uygulamaların verileri kullanması için bir yol sağlar. Özellikle, "tüketici grupları" uygulamaların verileri dinlemesi ve IoT Hub'dan çekmesi için bir mekanizma sağlar.

Daha yeni bir tanımlayın **tüketici grubu** özelliği, IOT hub'ı cihaz benzetimi çözümün **olaylar uç noktasına**.

1. Azure portalında, cihaz benzetimi çözümü için oluşturduğunuz kaynak grubunun **Genel bakış** sayfasına gidin ve sonra IoT Hub kaynağını seçin:

   [![Cihaz benzetimi çözümü kaynak grubu](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Ayrıca, çözüm için oluşturulan IoT Hub kaynağının **Adını** daha sonra başvurmak üzere not edin.

1. Sayfayı aşağı kaydırıp **Uç noktalar** sayfasını seçin, ardından **Olaylar** uç noktasını seçin. Uç noktanın **Özellikler** sayfasında, "$Default" tüketici grubu altında uç noktanız için benzersiz bir ad girin, sonra **Kaydet**’e tıklayın:

   [![Cihaz benzetimi çözümü IoT hub uç noktaları](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="environment-configuration-define-an-event-source"></a>Ortam yapılandırma: bir olay kaynağı tanımlayın

Artık yeni IOT hub'ı bağlantı **tüketici grubu** zaman serisi görüşleri ortamınıza olay uç noktası olarak bir **olay kaynağı**.

1. Git **genel bakış** sayfasında kaynak grubunun, zaman serisi görüşleri ortamınız için oluşturulmuş, ardından zaman serisi görüşleri ortamı seçin:

   [![Zaman serisi görüşleri ortamı kaynak grubu ve ortam](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

1. Zaman serisi görüşleri ortamı sayfasında **olay kaynakları**, ardından **+ Ekle**.

   [![Zaman serisi görüşleri ortamına genel bakış](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png#lightbox)

1. Gerekli Parametreler girin **yeni olay kaynağı** sayfası.

   Parametre|Açıklama
   ---|---
   **Olay kaynağı adı** | Olay kaynağını adlandırmak için kullanılan benzersiz bir değer gerektirir.
   **Kaynak** | Seçin **IOT hub'ı**.
   **İçeri aktarma seçeneği** | Varsayılan seçin `Use IoT hub from available subscriptions`. Bu seçenek, sonraki açılır listenin mevcut aboneliklerle doldurulmasına neden olur.
   **Abonelik** | Cihaz benzetimi kaynaklar ve zaman serisi görüşleri ortamı oluşturulan aynı aboneliği seçin.
   **Iot hub adı** | Varsayılan olarak daha önce not ettiğiniz IoT hub'ının adı olmalıdır. Değilse, doğru IoT hub'ını seçin.
   **Iot hub ilke adı** | Seçin **iothubowner**.
   **Iot hub tüketici grubu** | Varsayılan olarak daha önce oluşturduğunuz IoT hub'ı tüketici grubunun adı olmalıdır. Değilse, doğru tüketici grubu adını seçin.
   **Olay serileştirme biçimi** | Varsayılan haline getirilen değeri olarak bırakın `JSON`.
   **Zaman damgası özellik adı** | Olarak belirttiğiniz `timestamp`.

   Bittiğinde, olay kaynağını eklemek için **Oluştur**’a tıklayın. Kaynak grubunu döndüğünüzde **genel bakış** sayfasında, zaman serisi görüşleri ortamı kaynak birlikte yeni bir "Zaman serisi görüşleri olay kaynağı" kaynak görürsünüz.

   [![Zaman serisi görüşleri ortamı yeni olay kaynağı](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png#lightbox)

## <a name="run-device-simulation-to-stream-data"></a>Cihaz benzetimi için veri akışı çalıştırın.

Tüm yapılandırma işlemleri, tam zaman serisi görüşleri ortamına benzetilmiş aygıtlardan örnek verilerle doldurmak için zaman var.

[Cihaz benzetimi bölümü oluşturma](#create-a-device-simulation) bölümünden hatırlayabileceğiniz gibi, çözümü desteklemek için hızlandırıcı tarafından birkaç Azure kaynağı oluşturulmuştur. Daha önce ele alınan IoT hub’ı ile birlikte, sanal cihaz telemetrisi oluşturmak ve aktarmak için bir Azure App Service web uygulaması oluşturulmuştur.

1. [Çözüm hızlandırıcıları panonuza](https://www.azureiotsolutions.com/Accelerators#dashboard) geri dönün. Gerekirse, bu öğreticide kullanmakta olduğunuz Azure hesabını kullanarak yeniden oturum açın. Tıklayabilirsiniz artık **başlatma** düğmesi "Cihaz benzetimi" çözümünüzü altında.

     [![Çözüm Hızlandırıcıları Panosu](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. Cihaz benzetimi web uygulaması bu noktada başlar ve ilk yüklemeden sonra birkaç saniye sürebilir. Ayrıca, web uygulamasına "Oturumunuzu açma ve profilinizi okuma" izni vermek için onayınız istenir. Bu izin, uygulamanın çalışmasını desteklemek için gereken kullanıcı profili bilgilerini almak uygulama izin verir.

     [![Cihaz benzetimi web uygulama onay](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Bir kez **benzetimi Kurulum** sayfa yüklemelerinin, gerekli parametreleri girin.

   Parametre|Açıklama
   ---|---
   **Hedef IoT Hub'ı** | Seçin **kullanın, IOT hub'ı önceden sağlanan**.
   **Cihaz modeli** | Seçin **Soğutucu**.
   **Cihaz sayısı**  | Girin `1000` altında **tutarı**.
   **Telemetri sıklığı** | Girin `10` saniye.
   **Benzetim süresi** | Seçin **biten:** girin `5` dakika.

   Bittiğinde **Benzetimi Başlat**’a tıklayın. Benzetim toplam 5 dakika çalışır ve 1000 sanal cihazdan 10 saniyede bir veri oluşturur.  

   [![Cihaz benzetimi Kurulumu](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Benzetim çalışırken **Toplam ileti sayısı** ve **Saniyede ileti sayısı** alanlarının yaklaşık 10 saniyede bir güncelleştirildiğini fark edeceksiniz. Benzetim yaklaşık 5 dakika sonra sona erer ve **Benzetim ayarı** sayfasına geri dönersiniz.

   [![Çalıştıran cihaz benzetimi](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>Telemetri verilerini doğrulama

Bu son bölümde telemetri verilerini oluşturulur ve zaman serisi görüşleri ortamına depolanan olduğunu doğrulayın. Verileri doğrulamak için, telemetri verilerini sorgulamak ve çözümlemek için kullanılan Time Series Insights gezgininden yararlanın.

1. İade zaman serisi görüşleri ortamının kaynak grubuna **genel bakış** sayfası. Zaman serisi görüşleri ortamı seçin.

   [![Zaman serisi görüşleri ortamı kaynak grubu ve ortam](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. Zaman serisi görüşleri ortamına **genel bakış** sayfasında **Time Series Insights Gezgini URL** Time Series Insights gezginini açın.

   [![Time Series Insights Gezgini](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. Time Series Insights Gezgini yükleyin ve Azure portalı hesabınıza kullanarak kimlik doğrulaması. Başlangıç görünümü üzerinde zaman serisi görüşleri ortamına gerçekten sanal telemetri verilerle doldurulmuş olan grafik alanında görebilirsiniz. Daha dar bir zaman aralığı filtrelemek için sol üst köşedeki açılan listeyi seçin. Sonra, cihaz simülasyonunun süresine yayılacak kadar geniş bir zaman aralığı girin. Sınıf büyütme arama'ye tıklayın.

   [![Zaman serisi görüşleri Gezgini zaman aralığı filtresi](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. Zaman aralığı daraltma ayrı artışları IOT hub'ı ve zaman serisi görüşleri ortamına veri aktarımı için yakınlaştırmanız grafiği sağlar. Ayrıca, bulunan toplam olay sayısını gösteren sağ üst köşedeki **Akış tamamlandı** metnine dikkat edin. Ayrıca sürükleyebilirsiniz **aralık boyutu** kaydırıcı çizim ayrıntı grafiğine denetlemek için.

   [![Zaman serisi görüşleri Gezgini zaman aralığı filtrelenmiş görünüm](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Son olarak, aynı zamanda bir aralık filtre, sonra sağ tıklayın ve kullanmak için bir bölge sol **olayları keşfet** Olay Ayrıntıları sekmeli gösterilecek **olayları** görünümü.

   [![Görünüm ve olayları zaman serisi görüşleri Gezgini zaman aralığı filtrelendi](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide zaman serisi görüşleri ortamı ve cihaz benzetimi çözümünü desteklemek için birden fazla çalışan Azure hizmetini oluşturur. Bu öğretici serisinden çıkmak ve/veya seriyi daha sonra tamamlamak isterseniz, gereksiz maliyetlerin doğmasını önlemek için tüm kaynakları silmeniz önerilir.

Azure portalında soldaki menüden:

1. Tıklayın **kaynak grupları** simgesine ve ardından zaman serisi görüşleri ortamı için oluşturduğunuz kaynak grubunu seçin. Sayfanın üst kısmında **Kaynak grubunu sil**’e tıklayın, kaynak grubunun adını girin, ardından **Sil**’e tıklayın.

1. **Kaynak grupları** simgesine tıklayın, ardından cihaz simülasyonu çözüm hızlandırıcısı tarafından oluşturulan kaynak grubunu seçin. Sayfanın üst kısmında tıklayın **kaynak grubunu Sil**, kaynak grubunun adını girin ve ardından tıklayın **Sil**

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Time Series Insights ortamı oluşturma 
> * IoT Hub’ı içeren bir cihaz benzetimi çözümü oluşturma
> * Zaman serisi görüşleri ortamına IOT hub'a bağlama
> * Cihaz benzetimi için veri akışı zaman serisi görüşleri ortamına çalıştırın.
> * Sanal telemetri verilerini doğrulama

Kendi Time Series Insights ortamınızı oluşturmaya nasıl artık bildiğinize göre zaman serisi görüşleri ortamından veri kullanan bir web uygulaması oluşturmayı öğrenin:

> [!div class="nextstepaction"]
> [Azure Time Series Insights tek sayfalı web uygulaması oluşturma](tutorial-create-tsi-sample-spa.md)
