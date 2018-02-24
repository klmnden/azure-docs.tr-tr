---
title: "Azure IOT hub'ınızı – Web Apps algılayıcı verilerini gerçek zamanlı veri Görselleştirme | Microsoft Docs"
description: "Algılayıcı toplanan ve IOT hub'ına gönderilen sıcaklık ve nem verileri görselleştirmek için Microsoft Azure App Service Web Apps özelliğini kullanın."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "gerçek zamanlı veri görselleştirme, dinamik veri görselleştirme algılayıcı verileri Görselleştirme"
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 54a2defd6bfe2396e24584c686698d3215893cfd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a>Azure App Service Web Apps özelliğini kullanarak Azure IOT hub'ınızı gerçek zamanlı algılayıcı verilerini görselleştirmek

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu öğreticide, bir web uygulaması üzerinde barındırılan bir web uygulaması çalıştırarak IOT hub'ınızın aldığı gerçek zamanlı algılayıcı verilerini görselleştirmek öğrenin. Power BI kullanarak IOT hub'ınızı verileri görselleştirmek denemek istiyorsanız bkz [kullanım Power BI'ı, Azure IOT hub'ı gerçek zamanlı algılayıcı verilerini görselleştirmek için](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Neler

- Azure portalında bir web uygulaması oluşturun.
- IOT hub'ınızı bir tüketici grubu ekleyerek veri erişimi için hazırlanın.
- IOT hub'ından algılayıcı verileri okumak için web uygulaması yapılandırın.
- Web uygulaması tarafından barındırılan bir web uygulamasını karşıya yükleyin.
- IOT hub'ınızı gerçek zamanlı sıcaklık ve nem verileri görmek için web uygulamasını açın.

## <a name="what-you-need"></a>Ne gerekiyor

- [Cihazınızı ayarlamak](iot-hub-raspberry-pi-kit-node-get-started.md), aşağıdaki gereksinimleri ele alınmaktadır:
  - Etkin bir Azure aboneliği
  - Aboneliğinizdeki IOT hub'ı
  - IOT hub'ına iletileri gönderen bir istemci uygulaması
- [Git indir](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

1. İçinde [Azure portal](https://portal.azure.com/), tıklatın **kaynak oluşturma** > **Web + mobil** > **Web uygulaması**.
2. Benzersiz iş adını girin, aboneliğinizi doğrulamak, bir kaynak grubu ve select bir konum belirtin **panoya Sabitle**ve ardından **oluşturma**.

   Aynı konumu, kaynak grubu seçmenizi öneririz. Bunun yapılması, işlem hızı yardımcı olur ve veri aktarımı maliyeti azaltır.

   ![Web uygulaması oluşturma](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a>IOT hub'ından veri okumak için web uygulamasını yapılandırma

1. Yalnızca sağlanan web uygulamasını açın.
2. Tıklatın **uygulama ayarları**ve ardından, **uygulama ayarları**, aşağıdaki anahtar/değer çiftleri ekleyin:

   | Anahtar                                   | Değer                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Iothub Gezgini'nden elde                                |
   | Azure.IoT.IoTHub.ConsumerGroup        | IOT hub'ınızı ekleyin tüketici grubu adı  |

   ![Anahtar/değer çiftleri ile web uygulamanızın ayarlarını ekleyin](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. Tıklatın **uygulama ayarları**altında **genel ayarları**, iki durumlu **Web yuvaları** seçeneğini ve ardından **kaydetmek**.

   ![Web yuvaları seçeneğini Değiştir](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a>Web uygulaması tarafından barındırılan bir web uygulaması karşıya yükle

Github'da, IOT hub'ınızı gerçek zamanlı algılayıcı verileri görüntüleyen bir web uygulaması kullanılabilir yaptık. Yapmanız gereken tek şey bir Git deposu ile iş web uygulaması Github'dan indirin ve ardından barındırmak için web uygulaması için Azure'a yüklemeniz için web uygulaması yapılandırın.

1. Web uygulamasında tıklatın **dağıtım seçenekleri** > **Kaynağı Seç** > **yerel Git deposu**ve ardından **Tamam**.

   ![Yerel Git deposu kullanmak için web uygulama dağıtımı yapılandırma](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Tıklatın **dağıtım kimlik bilgileri**, bir kullanıcı adı ve parola Azure Git deponuza bağlamak ve ardından oluşturmak **kaydetmek**.

3. Tıklatın **genel bakış**ve değeri Not **Git kopyası URL'si**.

   ![Web uygulamanızın Git kopya URL'si](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. Bir komut veya yerel bilgisayarınızda terminal penceresi açın.

5. Web uygulaması Github'dan indirin ve barındırmak için web uygulaması için Azure'a yükleyin. Bunu yapmak için aşağıdaki komutları çalıştırın:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > \<Git kopyalama URL'si\> bulunan Git deposu URL'si **genel bakış** sayfa web uygulaması.

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>IOT hub'ınızı gerçek zamanlı sıcaklık ve nem verileri görmek için web uygulamasını açın

Üzerinde **genel bakış** sayfasında web uygulamanız web uygulaması açmak için URL'yi tıklatın.

![Web uygulamanızın URL'sini alma](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

IOT hub'ından gerçek zamanlı sıcaklık ve nem veri görmeniz gerekir.

![Gerçek zamanlı sıcaklık ve nem gösteren web uygulama sayfası](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> Örnek uygulama Cihazınızda çalıştığından emin olun. Boş bir grafik alırsınız istemiyorsanız, altında öğreticileri başvurabilirsiniz [Cihazınızı](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="next-steps"></a>Sonraki adımlar
IOT hub'ından gerçek zamanlı algılayıcı verilerini görselleştirmek için web uygulamanızı başarıyla kullandıysanız.

Azure IOT Hub verilerini görselleştirmek için alternatif bir yol için bkz: [IOT hub'ınızı gerçek zamanlı algılayıcı verilerini görselleştirmek için kullanım Power BI](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
