---
title: IOT Uzaktan izleme ve bildirimler Azure Logic Apps ile | Microsoft Docs
description: Otomatik olarak algılanan anomalileri, posta kutunuz e-posta bildirimleri göndermek ve IOT hub'ınızda sıcaklık IOT izlemek için Azure Logic Apps kullanın.
author: rangv
manager: ''
keywords: IOT, IOT bildirimleri izleme sıcaklık IOT izleme
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: adda4e948c11f84517b1e8dd01e6cfe42155e1ca
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409450"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>IOT Uzaktan izleme ve IOT hub ve posta kutusu bağlanan Azure Logic Apps ile bildirimleri

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Azure Logic Apps, bir dizi adım şeklinde işlemleri otomatik hale getirmek için bir yol sağlar. Mantıksal uygulama, çeşitli hizmet ve protokolleri bağlanabilirsiniz. Bu bir tetikleyici ile gibi 'Hesabınız eklendiğinde', başlar ve Eylemler, biri 'anında iletme bildirimi gönderme' gibi bir birleşimi tarafından izlenen. Bu özellik Logic Apps mükemmel bir IOT çözüm IOT için izleme, uyarı, diğer kullanım senaryoları arasında anormallikleri için sağlama gibi yapar.

## <a name="what-you-learn"></a>Öğrenecekleriniz

IOT hub'ınıza ve Sıcaklık İzleme ve bildirimler için posta bağlanan bir mantıksal uygulama oluşturmayı öğrenin. Sıcaklık 30 C istemci uygulaması işaretler `temperatureAlert = "true"` iletide IOT hub'ınıza gönderir. Mantıksal uygulama bir e-posta bildirimi göndermek için ileti tetikler.

## <a name="what-you-do"></a>Neler

* Bir service bus ad alanı oluşturun ve bir kuyruk ekleyin.
* IOT hub'ınıza bir uç nokta ve yönlendirme kuralı ekleyin.
* Oluşturma, yapılandırma ve bir mantıksal uygulamayı test etme.

## <a name="what-you-need"></a>Ne gerekiyor

* Öğretici [Cihazınızı ayarlama](iot-hub-raspberry-pi-kit-node-get-started.md) tamamlandı, aşağıdaki gereksinimleri ele alınmaktadır:
  * Etkin bir Azure aboneliği.
  * Azure IOT hub, aboneliğiniz altında.
  * Azure IOT hub'ınıza ileti gönderen bir istemci uygulaması.

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a>Service bus ad alanı oluşturma ve bir kuyruk ekleyin

### <a name="create-a-service-bus-namespace"></a>Bir service bus ad alanı oluşturma

1. Üzerinde [Azure portalında](https://portal.azure.com/), tıklayın **kaynak Oluştur** > **Kurumsal tümleştirme** > **Service Bus**.
1. Şu bilgileri belirtin:

   **Ad**: service bus'ın adı.

   **Fiyatlandırma katmanı**: tıklayın **temel** > **seçin**. Temel katmanı, Bu öğretici için yeterlidir.

   **Kaynak grubu**: IOT hub'ınıza kullandığı aynı kaynak grubunu kullanın.

   **Konum**: IOT hub'ınıza kullandığı aynı konumu kullanın.
1. **Oluştur**’a tıklayın.

   ![Azure portalında bir service bus ad alanı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Hizmet veri yolu kuyruğu ekleme

1. Service bus ad alanı açın ve ardından **+ kuyruk**.
1. Sıra için bir ad girin ve ardından **Oluştur**.
1. Hizmet veri yolu kuyruğu'ı açın ve ardından **paylaşılan erişim ilkeleri** > **+ Ekle**.
1. Onay ilkesine bir ad girin **Yönet**ve ardından **Oluştur**.

   ![Azure portalında bir service bus kuyruğu ekleme](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-to-your-iot-hub"></a>IOT hub'ınıza bir uç nokta ve yönlendirme kuralı Ekle

### <a name="add-an-endpoint"></a>Bir uç nokta ekleme

1. IOT hub'ınızı açın, uç noktaları > + Ekle.
1. Aşağıdaki bilgileri girin:

   **Ad**: uç nokta adı.

   **Uç nokta türü**: seçin **hizmet veri yolu kuyruğu**.

   **Service Bus ad alanı**: oluşturduğunuz ad alanı seçin.

   **Service Bus kuyruğu**: oluşturduğunuz kuyruğu seçin.
1. **Tamam** düğmesine tıklayın.

   ![Azure portalındaki IOT hub'ınıza bir uç nokta ekleyin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Yönlendirme kuralı ekleme

1. IOT hub'ı tıklatın **yollar** > **+ Ekle**.
1. Aşağıdaki bilgileri girin:

   **Ad**: yönlendirme kuralı adı.

   **Veri kaynağı**: seçin **DeviceMessages**.

   **Uç nokta**: oluşturduğunuz uç noktayı seçin.

   **Sorgu dizesi**: girin `temperatureAlert = "true"`.
1. **Kaydet**’e tıklayın.

   ![Azure portalında bir yönlendirme kuralı Ekle](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Bir mantık uygulaması oluşturma ve yapılandırma

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

1. İçinde [Azure portalında](https://portal.azure.com/), tıklayın **kaynak Oluştur** > **Kurumsal tümleştirme** > **mantıksal uygulama**.
1. Aşağıdaki bilgileri girin:

   **Ad**: mantıksal uygulamanın adı.

   **Kaynak grubu**: IOT hub'ınıza kullandığı aynı kaynak grubunu kullanın.

   **Konum**: IOT hub'ınıza kullandığı aynı konumu kullanın.
1. **Oluştur**’a tıklayın.

### <a name="configure-the-logic-app"></a>Mantıksal uygulamayı yapılandırma

1. Logic Apps Tasarımcısı açılır mantıksal uygulamayı açın.
1. Logic Apps Tasarımcısı'nda tıklatın **boş mantıksal uygulama**.

   ![Azure portalında boş mantıksal uygulama ile başlayın](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. Tıklayın **Service Bus**.

   ![Azure portalında mantıksal uygulamanızı oluşturmaya başlamak için Service Bus'ı seçin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Tıklayın **Service Bus kuyruk (Otomatik Tamamlama) veya daha fazla ileti ulaştığında –**.
1. Service bus bağlantı oluşturun.
   1. Bir bağlantı adı girin.
   1. Service bus ad alanına tıklayın > service bus İlkesi > **Oluştur**.

      ![Azure portalında mantıksal uygulamanız için bir hizmet veri yolu bağlantı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Tıklayın **devam** hizmet veri yolu bağlantı oluşturulduktan sonra.
   1. Oluşturduğunuz sıranın seçin ve girin `175` için **en fazla ileti sayısı**

      ![Mantıksal uygulamanız için hizmet veri yolu bağlantı en fazla ileti sayısı belirtin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. Değişiklikleri kaydetmek için "Kaydet" düğmesine tıklayın.

1. Bir SMTP hizmeti bağlantısı oluşturun.
   1. Tıklayın **yeni adım** > **Eylem Ekle**.
   1. Tür `SMTP`, tıklayın **SMTP** hizmet arama sonucunda ve ardından **SMTP - e-posta Gönder**.

      ![Azure portalında mantıksal uygulamanızda bir SMTP bağlantısı oluşturun](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. Posta SMTP bilgilerini girebilir ve ardından **Oluştur**.

      ![Azure portalında mantıksal uygulamanızda SMTP bağlantı bilgilerini girin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      SMTP hakkında bilgi alın [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), ve [Yahoo Posta](https://help.yahoo.com/kb/SLN4075.html).
   1. E-posta adresinizi girin **gelen** ve **için**, ve `High temperature detected` için **konu** ve **gövdesi**.
   1. **Kaydet**’e tıklayın.

Mantıksal uygulamayı kaydettikten düzende çalışmasını andır.

## <a name="test-the-logic-app"></a>Mantıksal uygulamayı test etme

1. Cihazınızı dağıttığınız istemci uygulamayı başlatmak [Azure IOT hub'a bağlanma ESP8266](iot-hub-arduino-huzzah-esp8266-get-started.md).
1. SensorTag 30 c. için Geçici ortam sıcaklık artırın Örneğin, Şamdan, SensorTag etrafında açık.
1. Mantıksal uygulama tarafından gönderilen bir e-posta bildirim almanız gerekir.

   > [!NOTE]
   > E-posta gönderir, olduğundan emin olmak için gönderen kimliğini doğrulamak e-posta hizmet sağlayıcınıza gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ınıza ve Sıcaklık İzleme ve bildirimler için posta bağlanan bir mantıksal uygulama başarıyla oluşturdunuz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
