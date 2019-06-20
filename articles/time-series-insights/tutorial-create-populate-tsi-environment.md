---
title: 'Öğretici: Azure Zaman Serisi Görüşleri ortamı oluşturma | Microsoft Docs'
description: Benzetilmiş aygıtlardan veri ile doldurulan bir zaman serisi görüşleri ortamı oluşturmayı öğrenin.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 06a450c47c7264bdecb663c9f71e3a9753df5e1e
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273408"
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

Zaman serisi görüşleri ortamı, cihaz verileri burada toplanan ve depolanan girebiliriz. Bir kez depolanır; [Azure Time Series Insights gezgininin](time-series-quickstart.md) ve [zaman serisi öngörüleri sorgu API'si](/rest/api/time-series-insights/ga-query-api) sorgulamak ve verileri çözümlemek için kullanılabilir.

Azure IOT hub'ı güvenli bir şekilde bağlanın ve Azure bulut veri iletmek için kullanılan tüm cihazlar tarafından (sanal veya fiziksel) öğreticide olay kaynağıdır.

Bu öğreticide ayrıca bir [IOT Çözüm Hızlandırıcısı](https://www.azureiotsolutions.com) oluşturup örnek telemetri verilerini IOT hub'ına akış.

>[!TIP]
> [IOT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com) özel IOT çözümlerinin geliştirilmesini hızlandırmanızı sağlayan kullanabileceğiniz kurumsal sınıf, önceden yapılandırılmış çözümler sunar.

## <a name="create-a-device-simulation"></a>Cihaz benzetimi oluşturma

İlk olarak, zaman serisi görüşleri ortamınıza doldurmak için test verilerini oluşturur cihaz benzetimi çözümü oluşturun.

1. Ayrı penceresi veya sekmesinde, Git [azureiotsolutions.com](https://www.azureiotsolutions.com). Aynı Azure aboneliği hesabını kullanarak oturum açın ve seçin **cihaz benzetimi** Hızlandırıcı.

   [![Cihaz benzetimi Hızlandırıcı çalıştırın](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Gerekli Parametreler girin **cihaz benzetimi oluşturma çözümü** sayfası.

   Parametre|Açıklama
   ---|---
   **Dağıtım adı** | Bu benzersiz bir değer, yeni bir kaynak grubu oluşturmak için kullanılır. Listelenen Azure kaynakları oluşturulur ve kaynak grubuna atanır.
   **Azure aboneliği** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı aboneliği belirtin.
   **Dağıtım seçenekleri** | Seçin **yeni IOT hub'ı sağlama** yeni bir IOT hub'ı Bu öğreticide özel oluşturmak için.
   **Azure konum** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı bölge belirtin.

   İşlemi tamamladığınızda, seçin **çözüm oluşturma** çözümün Azure kaynakları sağlamak için. Bu, bu işlemin tamamlanması 20 dakika sürebilir.

   [![Cihaz benzetimi çözümü sağlanamadı](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Sağlama tamamlandıktan sonra yeni çözümünüzü yukarıda metin değişiklikleri **sağlama** için **hazır**.

   >[!IMPORTANT]
   > Seçmeyin **başlatma** henüz! Buna daha sonra geri çünkü bu web sayfasını açık tutun.

   [![Cihaz benzetimi çözüm tam sağlama](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Şimdi, Azure portalında yeni oluşturulan kaynakları inceleyin. Üzerinde **kaynak grupları** sayfasında, yeni bir kaynak grubu kullanılarak oluşturulmuş fark **çözüm adı** son adımda sağlanan. Cihaz benzetimi için oluşturulan kaynakların not edin.

   [![Cihaz benzetimi kaynakları](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="create-an-environment"></a>Ortam oluşturma

İkinci olarak, Azure aboneliğinizde bir zaman serisi görüşleri ortamı oluşturun.

1. Oturum [Azure portalında](https://portal.azure.com) Azure abonelik hesabınızı kullanarak. 
1. Üstteki menüden **+ Kaynak oluştur**'u seçin. 
1. Seçin **nesnelerin interneti** kategori tıklayın ve ardından **Time Series Insights**. 

   [![Time Series Insights ortam kaynağını seçin](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. Üzerinde **zaman serisi görüşleri ortamına** sayfasında, gerekli parametreleri doldurun.

   Parametre|Açıklama
   ---|---
   **Ortam adı** | Zaman serisi görüşleri ortamı için benzersiz bir ad seçin. Adlar Time Series Insights Gezgini tarafından kullanılır ve [sorgu API'leri](https://docs.microsoft.com/rest/api/time-series-insights/ga-query).
   **Abonelik** | Abonelikler, Azure kaynaklarına yönelik kapsayıcılardır. Zaman serisi görüşleri ortamı oluşturmak için bir abonelik seçin.
   **Kaynak grubu** | Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. Mevcut bir kaynak grubu seçin veya zaman serisi görüşleri ortamı kaynak için yeni bir tane oluşturun.
   **Location** | Zaman serisi görüşleri ortamınız için bir veri merkezi bölgesini seçin. Ek gecikme önlemek için diğer IOT kaynaklarıyla aynı bölgede zaman serisi görüşleri ortamı oluşturun.
   **Katmanı** | Gerekli aktarım hızını seçin. Seçin **S1**.
   **Kapasite** | Seçilen SKU ile ilişkili depolama kapasitesi ve giriş oranı için uygulanan çarpan kapasitesidir. Kapasiteyi oluşturulduktan sonra değiştirebilirsiniz. Kapasite seçin **1**.

   İşiniz bittiğinde seçin **gözden geçir + Oluştur** sonraki adıma devam etmek için.

   [![Zaman serisi görüşleri ortamı kaynak oluştur](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Artık, çözüm Hızlandırıcı tarafından oluşturulan IOT hub'ına zaman serisi görüşleri ortamına bağlantı. Ayarlama **bir hub'ını seçin** için `Select existing`. Ardından çözüm Hızlandırıcı tarafından ayarlarken oluşturduğunuz IOT hub'ı seçin **IOT hub'ı adı**.

   [![Zaman serisi görüşleri ortamına oluşturulan IOT hub'a bağlama](media/tutorial-create-populate-tsi-environment/ap-create-resource-iot-hub.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-iot-hub.png#lightbox)

1. Denetleme **bildirimleri** paneli dağıtım tamamlanmasını izlemek için. 

   [![Zaman serisi görüşleri ortamına dağıtım başarılı](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="run-device-simulation-to-stream-data"></a>Cihaz benzetimi için veri akışı çalıştırın.

Örnek verileri ile zaman serisi görüşleri ortamına dağıtım ve ilk yapılandırmasından sonra tam doldurma [sanal cihazlar Hızlandırıcı tarafından oluşturulan](#create-a-device-simulation).

IOT hub ile birlikte bir Azure App Service web uygulaması oluşturma ve sanal cihaz telemetrisi iletmek için oluşturuldu.

1. [Çözüm hızlandırıcıları panonuza](https://www.azureiotsolutions.com/Accelerators#dashboard) geri dönün. Tekrar, gerekirse, bu öğreticide kullandığınız aynı Azure hesabını kullanarak oturum açın. Seçebileceğiniz artık **başlatma** "Cihaz benzetimi" çözümünüzü altında.

     [![Çözüm Hızlandırıcıları Panosu](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. Cihaz benzetimi web uygulaması, web uygulaması "oturumunuzu açma ve profilinizi okuma" izni isteyerek başlar izni. Bu izin, uygulamanın çalışmasını desteklemek için gereken kullanıcı profili bilgilerini almak uygulama izin verir.

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

Bu öğreticide zaman serisi görüşleri ortamı ve cihaz benzetimi çözümünü desteklemek için birden fazla çalışan Azure hizmetini oluşturur. Bunları kaldırmak için Azure portalına gidin.

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
