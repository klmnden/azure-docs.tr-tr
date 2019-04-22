---
title: Azure IOT hub'ınıza – Web uygulamalarını gelen algılayıcı verilerini gerçek zamanlı veri Görselleştirme | Microsoft Docs
description: Algılayıcıdan toplanan ve IOT hub'ına gönderilen sıcaklık ve nem verileri görselleştirmek için Microsoft Azure App Service'in Web Apps özelliğini kullanın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: robinsh
ms.openlocfilehash: 070f37a969411cfc4caf5f2d2b089ccfae759ca2
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59683209"
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a>Azure App Service'in Web Apps özelliğini kullanarak Azure IOT hub'ınıza gelen gerçek zamanlı algılayıcı verilerini Görselleştirme

![Uçtan uca diyagramı](./media/iot-hub-live-data-visualization-in-web-apps/1_iot-hub-end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu öğreticide, bir web uygulamasında barındırılan bir web uygulaması çalıştırarak IOT hub'ınızın aldığı gerçek zamanlı algılayıcı verilerini görselleştirme konusunda bilgi edinin. Power BI'ı kullanarak IOT hub'ınızdaki verileri görselleştirmek denemek istiyorsanız bkz [kullanım Power BI'ı, Azure IOT hub'ı gerçek zamanlı sensör verilerini görselleştirmek için](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Neler

* Azure portalında bir web uygulaması oluşturun.
* IOT hub'ınıza bir tüketici grubu ekleyerek veri erişim için hazırlanın.
* Algılayıcı verileri IOT hub'ından okumak için web uygulaması yapılandırırsınız.
* Web uygulaması tarafından barındırılan bir web uygulaması yükleyin.
* IOT hub'ınıza gerçek zamanlı sıcaklık ve nem verileri görmek için web uygulamasını açın.

## <a name="what-you-need"></a>Ne gerekiyor

* Tamamlamak [Raspberry Pi çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya bir cihaz öğreticileri; Örneğin, [node.js ile Raspberry Pi](iot-hub-raspberry-pi-kit-node-get-started.md). Bu, aşağıdaki gereksinimleri kapsar:

  * Etkin bir Azure aboneliği
  * Aboneliğinizde bir IOT hub
  * IOT hub'ınıza ileti gönderen bir istemci uygulaması

* [Git indir](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

1. İçinde [Azure portalında](https://portal.azure.com/), tıklayın **kaynak Oluştur** > **Web + mobil** > **Web uygulaması**.

2. Benzersiz iş adını girin, aboneliğinizi doğrulamak, bir kaynak grubunu ve konumu, select belirtin **panoya Sabitle**ve ardından **Oluştur**.

   Kaynak grubunuzun aynı konumu seçin öneririz. 
   
   ![Web uygulaması oluşturma](./media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a>IOT hub'ınızdaki verileri okumak için web uygulamasını yapılandırma

1. Yalnızca sağlanan web uygulamasını açın.

2. Tıklayın **uygulama ayarları**ve sonra **uygulama ayarları**, aşağıdaki anahtar/değer çifti ekleyin:

   | Anahtar                                   | Değer                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Azure CLI üzerinden alınan                                      |
   | Azure.IoT.IoTHub.ConsumerGroup        | IOT hub'ınıza eklediğiniz tüketici grubunun adı  |
   | WEBSITE_NODE_DEFAULT_VERSION          | 8.9.4                                                        |

   ![Ayarları, anahtar/değer çiftleri ile web uygulamanıza ekleme](./media/iot-hub-live-data-visualization-in-web-apps/3_web-app-settings-key-value-azure.png)

3. Tıklayın **uygulama ayarları**altında **genel ayarlar**, iki durumlu **Web yuvaları** seçeneğini ve ardından **Kaydet**.

   ![Web yuvaları seçeneğini değiştirir](./media/iot-hub-live-data-visualization-in-web-apps/4_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a>Web uygulaması tarafından barındırılan bir web uygulaması yükleme

GitHub üzerinde IOT hub'ınıza gerçek zamanlı sensör verileri görüntüleyen bir web uygulaması kullanılabilir yaptık. Tek yapmak için ihtiyacınız olan bir Git deposu ile çalışacak, web uygulamasını Github'dan indirin ve ardından barındırmak için web uygulaması için Azure'a yükleme için web uygulamasını yapılandırın.

1. Web uygulamasında tıklayın **dağıtım seçenekleri** > **Kaynağı Seç** > **yerel Git deposu**ve ardından **Tamam**.

   ![Yerel Git deposu kullanmak için web uygulaması dağıtımınızı yapılandırma](./media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Tıklayın **dağıtım kimlik bilgileri**, bir kullanıcı adı ve parolası Azure Git deposuna bağlanın ve ardından oluşturma **Kaydet**.

3. Tıklayın **genel bakış**ve değerini not edin **Git kopyası URL'si**.

   ![Web uygulamanızın Git kopya URL'si Al](./media/iot-hub-live-data-visualization-in-web-apps/6_web-app-git-clone-url-azure.png)

4. Bir komut veya yerel bilgisayarınızdaki terminal penceresi açın.

5. Web uygulamasını Github'dan indirin ve barındırmak için web uygulaması için Azure'a yükleyin. Bunu yapmak için aşağıdaki komutları çalıştırın:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > \<Git kopya URL'si\> bulunan Git deposunun URL'si **genel bakış** web uygulamasının sayfası.

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>IOT hub'ınıza gerçek zamanlı sıcaklık ve nem verileri görmek için web uygulamasını açın.

Üzerinde **genel bakış** sayfası, web uygulamanızı web uygulamasını açmak için URL'yi tıklayın.

![Web uygulamanızın URL'sini alma](./media/iot-hub-live-data-visualization-in-web-apps/7_web-app-url-azure.png)

IOT hub'ından gerçek zamanlı sıcaklık ve nem veri görmeniz gerekir.

![Gerçek zamanlı sıcaklık ve nem gösteren bir web uygulaması sayfası](./media/iot-hub-live-data-visualization-in-web-apps/8_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> Örnek uygulamanın, Cihazınızda çalıştığından emin olun. Boş bir grafik alırsınız, altında öğreticileri başvurabilirsiniz [Cihazınızı ayarlama](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ından gerçek zamanlı sensör verilerini görselleştirmek için web uygulamanızı başarıyla kullandınız.

Azure IOT Hub'ından verileri görselleştirmek alternatif bir yolu için bkz: [kullanım Power BI'ı, IOT hub'ınıza gerçek zamanlı sensör verilerini görselleştirmek için](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]