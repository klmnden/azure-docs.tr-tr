---
title: 'Öğretici: Azure Zaman Serisi Görüşleri ortamı oluşturma | Microsoft Docs'
description: Sanal cihazlardan verilerle doldurulmuş bir Time Series Insights ortamı oluşturma hakkında bilgi edinin.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 12/05/2018
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: c974e011e6f101eab617a370b24306f80c249b90
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64718409"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Öğretici: Azure Time Series Insights ortamı oluşturma

Bu öğreticide, sanal cihazlardan verilerle doldurulmuş bir Time Series Insight (TSI) ortamı oluşturma adımları gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * TSI ortamı oluşturma 
> * IoT Hub’ı içeren bir cihaz benzetimi çözümü oluşturma
> * TSI ortamını IoT hub’ına bağlama
> * TSI ortamına veri akışı yapmak için cihaz benzetimi çalıştırma
> * Sanal telemetri verilerini doğrulama

## <a name="video"></a>Video

### <a name="in-this-video-we-show-you-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-that-can-be-used-to-get-started-with-time-series-insights-br"></a>Bu videoda, Time Series Insights’ı kullanmaya başlamak için kullanılabilecek veriler oluşturmak için Azure IoT Çözüm Hızlandırıcısı’nın nasıl kullanılacağı gösterilmektedir. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

Azure oturum açma hesabının aynı zamanda aboneliğin Sahip rolüne üye olması gerekir. Ayrıntılar için [RBAC ve Azure portalı kullanarak erişimi yönetme](/azure/role-based-access-control/role-assignments-portal)

## <a name="overview"></a>Genel Bakış

TSI ortamı, cihaz verilerinin toplanıp depolandığı yerdir. TSI ortamına depolandıktan sonra, [TSI Gezgini](time-series-quickstart.md) ve [TSI Sorgu API’sini](/rest/api/time-series-insights/ga-query-api) kullanarak verileri sorgulayıp çözümleyebilirsiniz.

Sanal veya fiziksel tüm cihazlar gibi IoT Hub da Azure bulutuna güvenli bir şekilde bağlanmak ve veri aktarmak için cihazlar tarafından kullanılan bağlantı noktasıdır. [TSI’ya Genel Bakış](time-series-insights-overview.md) bölümünde ele alındığı gibi, IoT Hub aynı zamanda TSI ortamına veri akışı yapmaya yarayan bir olay kaynağı olarak görev yapar. 

Bu öğreticide ayrıca örnek telemetri verileri oluşturup IoT Hub’a akışını yapmak için bir [IoT çözüm hızlandırıcısı](/azure/iot-accelerators/) kullanılmaktadır. IoT çözüm hızlandırıcıları, özel IoT çözümleri geliştirmeyi hızlandırmanızı sağlayan, önceden yapılandırılmış kurumsal düzeyde çözümler sunar. 

## <a name="create-a-tsi-environment"></a>TSI ortamı oluşturma

İlk olarak, Azure aboneliğinizde bir TSI ortamı oluşturun:

1. Azure abonelik hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.  
2. Üstteki menüden **+ Kaynak oluştur**'u seçin.  
3. **Nesnelerin İnterneti** kategorisini, ardından **Time Series Insights**’ı seçin.  
   
   [![Time Series Insights ortam kaynağını seçin](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

4. **Time Series Insights ortamı** sayfasında gerekli parametreleri doldurun:
   
   Parametre|Açıklama
   ---|---
   **Ortam adı** | TSI ortamı için benzersiz bir ad seçin. Bu ad TSI Gezgini ve Sorgu API’si tarafından kullanılır.
   **Abonelik** | Abonelikler, Azure kaynaklarına yönelik kapsayıcılardır. TSI ortamını oluşturmak istediğiniz aboneliği seçin.
   **Kaynak grubu** | Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. Mevcut bir kaynak grubu seçin veya TSI ortam kaynağı için yeni bir tane oluşturun.
   **Konum** | TSI ortamınız için bir veri merkezi bölgesi seçin. Daha fazla bant genişliği maliyeti ve gecikme süresi olmaması için, TSI ortamının diğer IoT kaynakları ile aynı bölgede tutulması idealdir.
   **Fiyatlandırma SKU'su** | Gerekli aktarım hızını seçin. En düşük maliyet ve başlangıç kapasitesi için S1’i seçin.
   **Kapasite** | Kapasite; giriş oranına, depolama kapasitesine ve seçili SKU ile ilişkili maliyete uygulanan çarpandır.  Oluşturduktan sonra ortam kapasitesini değiştirebilirsiniz. En düşük maliyet için 1 kapasitesini seçin. 

   İşlem bittiğinde sağlama işlemini başlatmak için **Oluştur**'a tıklayın.

   ![Time Series Insights ortam kaynağı oluşturma](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)

5. Bir dakikadan kısa sürmesi gereken dağıtım işleminin tamamlanmasını izlemek için **Bildirimler** panelini denetleyebilirsiniz:  

   ![Time Series Insights ortamının dağıtımı başarılı oldu](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)

## <a name="create-a-device-simulation"></a>Cihaz benzetimi oluşturma

Ardından, TSI ortamınızı doldurmak için test verileri oluşturacak cihaz benzetimi çözümünü oluşturun:

1. Ayrı bir pencerede/sekmede https://www.azureiotsolutions.com sayfasına gidin, aynı Azure aboneliği hesabını kullanarak oturum açın ve "Cihaz Benzetimi" hızlandırıcısını seçin:

   ![Cihaz Benzetimi hızlandırıcısını çalıştırma](media/tutorial-create-populate-tsi-environment/sa-main.png)

2. **Cihaz Benzetimi çözümü oluşturun** sayfasına gerekli parametreleri girin:

   Parametre|Açıklama
   ---|---
   **Çözüm adı** | Yeni bir kaynak grubu oluşturmak için kullanılan benzersiz değer. Listelenen Azure kaynakları oluşturulur ve kaynak grubuna atanır.
   **Abonelik** | Önceki bölümde TSI ortamınızın oluşturulması için kullanılan aynı aboneliği belirtin.
   **Bölge** | Önceki bölümde TSI ortamınızın oluşturulması için kullanılan aynı bölgeyi belirtin. 
   **İsteğe bağlı Azure Kaynaklarını dağıtma** | Sanal cihazlar tarafından verileri bağlamak/akış yapmak için kullanılacağından **IoT Hub** seçeneğinin işaretini kaldırmayın.

   İşiniz bittiğinde, yaklaşık 6-7 dakika içinde çözümün Azure kaynaklarını sağlamak için **Çözüm oluştur**’a tıklayın:

   ![Cihaz benzetimi çözümünü sağlama](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)

3. Sağlama tamamlandıktan sonra yeni çözümünüzün üzerindeki "Sağlanıyor..." metni "Hazır" olarak değişir:

   >[!IMPORTANT]
   > **Başlat** düğmesine henüz tıklamayın! Ancak daha sonra döneceğiniz için bu web sayfasını açık tutun.

   ![Cihaz benzetimi çözümünü sağlama tamamlandı](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)

4. Şimdi Azure portalına geri dönüp aboneliğinizde yeni oluşturulan kaynakları inceleyin. Portalın **Kaynak grupları** sayfasında, son adımda sağlanan **Çözüm adı** kullanılarak yeni bir kaynak grubu oluşturulduğunu fark edeceksiniz. Ayrıca, cihaz benzetimi çözümünü desteklemek için tüm kaynakların oluşturulduğuna dikkat edin:

   [![Cihaz benzetimi çözüm kaynakları](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-tsi-environment-to-the-iot-hub"></a>TSI ortamını IoT hub’ına bağlama 

Bu noktada, her biri kendi kaynak grubunda bulunan iki kaynak kümesi oluşturmayı öğrendiniz: 
- Boş bir TSI ortamı 
- Bir çözüm hızlandırıcısı tarafından oluşturulan, IoT hub’ı dahil cihaz benzetimi çözüm kaynakları

Sanal cihazların cihaz verilerini akışla aktarmak için bir IoT hub'ına bağlanması gerektiğini unutmayın. Verileri TSI ortamına aktarmak için hem IoT hub’ınızda hem de TSI ortamınızda yapılandırma değişiklikleri yapmanız gerekir. 

### <a name="iot-hub-configuration-define-a-consumer-group"></a>IoT hub’ı yapılandırma: tüketici grubu tanımlama

IoT hub’ı, diğer aktörlerle işlev paylaşmak için çeşitli uç noktalar sağlar. "Olaylar" uç noktası, bir IoT hub örneğine aktarılırken diğer uygulamaların verileri kullanması için bir yol sağlar. Özellikle, "tüketici grupları" uygulamaların verileri dinlemesi ve IoT Hub'dan çekmesi için bir mekanizma sağlar. 

Bundan sonra, cihaz benzetimi çözümünün IoT hub "Olayları" uç noktası üzerinde yeni bir "tüketici grubu" özelliği tanımlayın. 

1. Azure portalında, cihaz benzetimi çözümü için oluşturduğunuz kaynak grubunun **Genel bakış** sayfasına gidin ve sonra IoT Hub kaynağını seçin:

   [![Cihaz benzetimi çözümü kaynak grubu](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Ayrıca, çözüm için oluşturulan IoT Hub kaynağının **Adını** daha sonra başvurmak üzere not edin.

2. Sayfayı aşağı kaydırıp **Uç noktalar** sayfasını seçin, ardından **Olaylar** uç noktasını seçin. Uç noktanın **Özellikler** sayfasında, "$Default" tüketici grubu altında uç noktanız için benzersiz bir ad girin, sonra **Kaydet**’e tıklayın:

   [![Cihaz benzetimi çözümü IoT hub uç noktaları](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="tsi-environment-configuration-define-an-event-source"></a>TSI ortamı yapılandırması: olay kaynağı tanımlama

Şimdi yeni IoT hub "tüketici grubu" olay uç noktasını, bir "olay kaynağı" olarak TSI ortamına bağlayın.

1. TSI ortamı için oluşturduğunuz kaynak grubunun **Genel Bakış** sayfasına gidin, ardından TSI ortamını seçin:

   [![TSI ortamı kaynak grubu ve ortamı](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

2. TSI ortamı sayfasında **Olay Kaynakları**’nı seçin, ardından **+ Ekle**’ye tıklayın:

   ![TSI ortamına genel bakış](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)

3. Gerekli parametreleri **Yeni olay kaynağı** sayfasına girin: 

   Parametre|Açıklama
   ---|---
   **Olay kaynağı adı** | Olay kaynağını adlandırmak için kullanılan benzersiz bir değer gerektirir.
   **Kaynak** | "IoT Hub" öğesini seçin.
   **İçeri aktarma seçeneği** | Varsayılan "Mevcut aboneliklerdeki IoT hub’ını kullan" seçeneğini kullanın. Bu seçenek, sonraki açılır listenin mevcut aboneliklerle doldurulmasına neden olur.
   **Abonelik** | TSI ortamını ve Cihaz Benzetimi kaynaklarını oluşturduğunuz aboneliğin aynısını seçin.
   **Iot hub adı** | Varsayılan olarak daha önce not ettiğiniz IoT hub'ının adı olmalıdır. Değilse, doğru IoT hub'ını seçin.
   **Iot hub ilke adı** | Varsayılan "iothubowner" değerini değiştirmeyin.
   **Iot hub tüketici grubu** | Varsayılan olarak daha önce oluşturduğunuz IoT hub'ı tüketici grubunun adı olmalıdır. Değilse, doğru tüketici grubu adını seçin. 
   **Olay serileştirme biçimi** | Varsayılan "JSON" değerini değiştirmeyin.
   **Zaman damgası özellik adı** | "timestamp" olarak belirtin.

   Bittiğinde, olay kaynağını eklemek için **Oluştur**’a tıklayın. Kaynak grubu **Genel Bakış** sayfasına geri döndüğünüzde, TSI ortam kaynağınızla birlikte yeni bir "Time Series Insights olay kaynağı" görürsünüz.

   ![TSI ortamı yeni olay kaynağı](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)

## <a name="run-a-device-simulation-to-stream-data-into-the-tsi-environment"></a>TSI ortamına veri akışı yapmak için cihaz benzetimi çalıştırma

Tüm yapılandırma işi tamamlandıktan sonra, sanal cihazlardaki örnek verilerle TSI ortamının doldurulması gerekir.

[Cihaz benzetimi bölümü oluşturma](#create-a-device-simulation) bölümünden hatırlayabileceğiniz gibi, çözümü desteklemek için hızlandırıcı tarafından birkaç Azure kaynağı oluşturulmuştur. Daha önce ele alınan IoT hub’ı ile birlikte, sanal cihaz telemetrisi oluşturmak ve aktarmak için bir Azure App Service web uygulaması oluşturulmuştur.

1. [Çözüm hızlandırıcıları panonuza](https://www.azureiotsolutions.com/Accelerators#dashboard) geri dönün. Gerekirse, bu öğreticide kullanmakta olduğunuz Azure hesabını kullanarak yeniden oturum açın. Şimdi "Cihaz Benzetimi" çözümünüzün altındaki **Başlat** düğmesine tıklayabilirsiniz:

     ![Çözüm hızlandırıcıları panosu](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)

2. Cihaz benzetimi web uygulaması bu noktada başlar ve ilk yüklemeden sonra birkaç saniye sürebilir. Ayrıca, web uygulamasına "Oturumunuzu açma ve profilinizi okuma" izni vermek için onayınız istenir. Bu izin, uygulamanın, çalışmasını desteklemek için gereken kullanıcı profili bilgilerini almasına olanak tanır:

     ![Cihaz benzetimi web uygulaması onayı](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)

3. **Benzetim ayarı** sayfası yüklendikten sonra gerekli parametreleri girin: 

   Parametre|Açıklama
   ---|---
   **Hedef IoT Hub'ı** | "Önceden sağlanan IoT Hub’ı kullan" öğesini seçin.
   **Cihaz modeli** | "Soğutucu" öğesini seçin.
   **Cihaz sayısı**  | **Tutar** altına 1000 girin.
   **Telemetri sıklığı** | 10 saniye girin.
   **Benzetim süresi** | **Sonlandır:** öğesini seçip 5 dakika girin.

   Bittiğinde **Benzetimi Başlat**’a tıklayın. Benzetim toplam 5 dakika çalışır ve 1000 sanal cihazdan 10 saniyede bir veri oluşturur.  

   ![Cihaz benzetimi ayarı](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)

4. Benzetim çalışırken **Toplam ileti sayısı** ve **Saniyede ileti sayısı** alanlarının yaklaşık 10 saniyede bir güncelleştirildiğini fark edeceksiniz. Benzetim yaklaşık 5 dakika sonra sona erer ve **Benzetim ayarı** sayfasına geri dönersiniz.

   ![Cihaz benzetimi çalışıyor](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)

## <a name="verify-the-telemetry-data"></a>Telemetri verilerini doğrulama

Bu son bölümde, telemetri verilerinin TSI ortamında oluşturulup depolandığını doğrulayacaksınız. Verileri doğrulamak için, telemetri verilerini sorgulamak ve çözümlemek için kullanılan Time Series Insights gezgininden yararlanın.

1. TSI ortamının kaynak grubu **Genel Bakış** sayfasına geri dönün, ardından tekrar TSI ortamını seçin:

   [![TSI ortamı kaynak grubu ve ortamı](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

2. TSI ortamı **Genel Bakış** sayfasında **Time Series Insights gezgin URL’sine** tıklayarak TSI gezginini açın:

   [![TSI gezgini](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

3. TSI gezgini yüklenir ve Azure portalı hesabınızı kullanarak kimlik doğrulaması yapar. İlk görüntülemeden sonra grafik alanında TSI ortamının sanal telemetri verileri ile doldurulduğunu görebilirsiniz. Daha dar bir zaman aralığı filtrelemek için sol üst köşedeki açılan listeyi seçin. Sonra, cihaz simülasyonunun süresine yayılacak kadar geniş bir zaman aralığı girin. Ardından arama büyütecine tıklayın:

   [![TSI gezgini zaman aralığı filtresi](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

4. Zaman aralığının daraltılması, grafiğin farklı veri aktarımı serilerini IoT hub ve TSI ortamına yakınlaştırmasını sağlar. Ayrıca, bulunan toplam olay sayısını gösteren sağ üst köşedeki **Akış tamamlandı** metnine dikkat edin. Grafikteki çizim ayrıntı düzeyini denetlemek için **Aralık boyutu** kaydırıcısını da sürükleyebilirsiniz:

   [![TSI gezgini zaman aralığı filtrelenmiş görünümü](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

5. Son olarak, bir bölgeye sol tıklayarak aralığı filtreleyebilir, ardından sağ tıklayıp **Olayları keşfedin**’i kullanarak tablo biçimindeki **Olaylar** görünümünde olay ayrıntılarını görüntüleyebilirsiniz:

   [![TSI gezgini zaman aralığı filtrelenmiş görünümü ve olayları](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide, TSI ortamını ve cihaz benzetimi çözümünü desteklemek üzere birkaç çalışan Azure hizmeti oluşturulur. Bu öğretici serisinden çıkmak ve/veya seriyi daha sonra tamamlamak isterseniz, gereksiz maliyetlerin doğmasını önlemek için tüm kaynakları silmeniz önerilir. 

Azure portalında soldaki menüden:

1. **Kaynak grupları** simgesine tıklayın, ardından TSI Ortamı için oluşturduğunuz kaynak grubunu seçin. Sayfanın üst kısmında **Kaynak grubunu sil**’e tıklayın, kaynak grubunun adını girin, ardından **Sil**’e tıklayın. 
2. **Kaynak grupları** simgesine tıklayın, ardından cihaz simülasyonu çözüm hızlandırıcısı tarafından oluşturulan kaynak grubunu seçin. Sayfanın üst kısmında **Kaynak grubunu sil**’e tıklayın, kaynak grubunun adını girin, ardından **Sil**’e tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * TSI ortamı oluşturma 
> * IoT Hub’ı içeren bir cihaz benzetimi çözümü oluşturma
> * TSI ortamını IoT hub’ına bağlama
> * TSI ortamına veri akışı yapmak için cihaz benzetimi çalıştırma
> * Sanal telemetri verilerini doğrulama

Artık kendi TSI ortamınızı oluşturmayı bildiğinize göre, TSI ortamındaki verileri kullanan bir web uygulaması oluşturmayı öğrenin:

> [!div class="nextstepaction"]
> [Azure Time Series Insights tek sayfalı web uygulaması oluşturma](tutorial-create-tsi-sample-spa.md)


