---
title: IOT Uzaktan izleme ve Azure Logic Apps ile bildirimleri | Microsoft Docs
description: Azure mantıksal uygulamaları, IOT hub'ına sıcaklık IOT izlemek için kullanın ve otomatik olarak algılanan tüm daha fazla bilgi için posta kutunuza e-posta bildirimleri gönder.
services: iot-hub
documentationcenter: ''
author: rangv
manager: timlt
tags: ''
keywords: IOT, IOT bildirimleri izleme IOT Sıcaklık İzleme
ms.assetid: 43043067-2e1f-42c9-953d-e2dce8fd86df
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: e54c36d0cfbaedb93db86ad6ce5f99b288b63c9c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>IOT Uzaktan izleme ve IOT hub ve posta kutusu bağlanma Azure Logic Apps ile bildirimleri

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Azure mantıksal uygulamaları bir dizi adımı süreçlerini otomatikleştirmek için bir yol sağlar. Bir mantıksal uygulama çeşitli hizmetler ve protokoller bağlanabilirsiniz. Bir tetikleyici ile gibi 'Bir hesap eklendiğinde', başladıktan ve 'bir anında iletme bildirimi gönderme' gibi işlemleri birleştirilmesiyle gelmelidir. Bu özellik Logic Apps mükemmel bir IOT çözüm IOT için izleme, diğer kullanım senaryoları arasında anormallikleri için uyarı kaldığını gibi kolaylaştırır.

## <a name="what-you-learn"></a>Öğrenecekleriniz

IOT hub'ınızı ve Sıcaklık İzleme ve bildirimler için posta bağlayan bir mantıksal uygulama oluşturmayı öğrenin. Sıcaklığı 30 C olduğunda, istemci uygulaması işaretler `temperatureAlert = "true"` , IOT hub'ınıza gönderdiği iletisi. İleti bir e-posta bildirimi göndermek için mantıksal uygulama tetikler.

## <a name="what-you-do"></a>Neler

* Hizmet veri yolu ad alanı oluşturun ve bir kuyruk ekleyin.
* Bir uç nokta ve yönlendirme kuralı IOT hub'ınızı ekleyin.
* Oluşturma, yapılandırma ve bir mantıksal uygulama test.

## <a name="what-you-need"></a>Ne gerekiyor

* Öğretici [Cihazınızı](iot-hub-raspberry-pi-kit-node-get-started.md) , aşağıdaki gereksinimleri ele alınmaktadır tamamlandı:
  * Etkin bir Azure aboneliği.
  * Azure IOT hub'ı aboneliğinizdeki.
  * Azure IOT hub'ına iletileri gönderen bir istemci uygulaması.

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a>Hizmet veri yolu ad alanı oluşturmak ve bir kuyruk ekleyin

### <a name="create-a-service-bus-namespace"></a>Hizmet veri yolu ad alanı oluşturma

1. Üzerinde [Azure portal](https://portal.azure.com/), tıklatın **kaynak oluşturma** > **Kurumsal tümleştirme** > **Service Bus**.
1. Aşağıdaki bilgileri sağlayın:

   **Ad**: hizmet veri yolu adı.

   **Fiyatlandırma katmanı**: tıklatın **temel** > **seçin**. Temel katman, Bu öğretici için yeterlidir.

   **Kaynak grubu**: IOT hub'ınızı kullandığı aynı kaynak grubunu kullanın.

   **Konum**: IOT hub'ınızı kullandığı aynı konum kullanın.
1. **Oluştur**’a tıklayın.

   ![Azure Portalı'nda hizmet veri yolu ad alanı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Hizmet veri yolu kuyruğu ekleme

1. Hizmet veri yolu ad alanı açın ve ardından **+ sıraya**.
1. Sıra için bir ad girin ve ardından **oluşturma**.
1. Hizmet veri yolu kuyruğu açın ve ardından **paylaşılan erişim ilkeleri** > **+ Ekle**.
1. Onay ilkesi için bir ad girin **Yönet**ve ardından **oluşturma**.

   ![Azure portalında bir hizmet veri yolu kuyruğu ekleme](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-to-your-iot-hub"></a>Bir uç nokta ve yönlendirme kuralı IOT hub'ınızı ekleyin

### <a name="add-an-endpoint"></a>Bir uç nokta ekleme

1. IOT hub'ınızı açın, uç noktaları > + Ekle.
1. Aşağıdaki bilgileri girin:

   **Ad**: uç noktanın adı.

   **Uç nokta türü**: seçin **hizmet veri yolu kuyruğu**.

   **Hizmet veri yolu ad alanı**: oluşturduğunuz ad alanını seçin.

   **Service Bus kuyruğuna**: oluşturduğunuz kuyruğu seçin.
1. **Tamam**’a tıklayın.

   ![Azure portalında IOT hub'ınız için bir uç nokta ekleyin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Yönlendirme kuralı Ekle

1. IOT hub'ı tıklatın **yollar** > **+ Ekle**.
1. Aşağıdaki bilgileri girin:

   **Ad**: yönlendirme kuralı adı.

   **Veri kaynağı**: seçin **DeviceMessages**.

   **Uç nokta**: oluşturduğunuz uç nokta seçin.

   **Sorgu dizesi**: girin `temperatureAlert = "true"`.
1. **Kaydet**’e tıklayın.

   ![Azure portalında bir yönlendirme kuralı Ekle](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Oluşturma ve bir mantıksal uygulama yapılandırma

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

1. İçinde [Azure portal](https://portal.azure.com/), tıklatın **kaynak oluşturma** > **Kurumsal tümleştirme** > **mantıksal uygulama**.
1. Aşağıdaki bilgileri girin:

   **Ad**: mantıksal uygulama adı.

   **Kaynak grubu**: IOT hub'ınızı kullandığı aynı kaynak grubunu kullanın.

   **Konum**: IOT hub'ınızı kullandığı aynı konum kullanın.
1. **Oluştur**’a tıklayın.

### <a name="configure-the-logic-app"></a>Mantıksal uygulama yapılandırma

1. Logic Apps tasarımcıya açar mantığı uygulamasını açın.
1. Logic Apps Tasarımcısı'nda tıklayın **boş mantıksal uygulama**.

   ![Azure portalında boş mantıksal uygulama ile başlayın](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. Tıklatın **Service Bus**.

   ![Azure portalda mantıksal uygulamanızı oluşturmaya başlamak için Service Bus seçin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Tıklatın **Service Bus – bir veya daha fazla ileti (Otomatik Tamamlama) kuyrukta geldiğinde**.
1. Hizmet veri yolu bağlantı oluşturun.
   1. Bir bağlantı adı girin.
   1. Hizmet veri yolu ad alanına tıklayın > hizmet veri yolu İlkesi > **oluşturma**.

      ![Azure portalda mantıksal uygulamanız için bir hizmet veri yolu bağlantı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Tıklatın **devam** hizmet veri yolu bağlantı oluşturulduktan sonra.
   1. Oluşturduğunuz kuyruk seçip girin `175` için **en fazla ileti sayısı**

      ![Hizmet veri yolu bağlantı için en fazla ileti sayısı mantıksal uygulamanızı belirtin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. Değişiklikleri kaydetmek için "Kaydet" düğmesini tıklatın.

1. SMTP hizmeti bağlantısı oluşturun.
   1. Tıklatın **yeni adım** > **Eylem Ekle**.
   1. Tür `SMTP`, tıklatın **SMTP** hizmet arama sonucunda ve ardından **SMTP - e-posta Gönder**.

      ![Bir SMTP bağlantı Azure portalda mantıksal uygulamanızı oluşturun](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. Posta SMTP bilgilerini girin ve ardından **oluşturma**.

      ![Azure portalda mantıksal uygulamanızı SMTP bağlantı bilgilerini girin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      SMTP bilgilerini al [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), ve [Yahoo Posta](https://help.yahoo.com/kb/SLN4075.html).
   1. E-posta adresinizi girin **gelen** ve **için**, ve `High temperature detected` için **konu** ve **gövde**.
   1. **Kaydet**’e tıklayın.

Kaydettiğinizde mantıksal uygulama çalışma sıradadır.

## <a name="test-the-logic-app"></a>Mantıksal uygulamayı test etme

1. Cihazınızı dağıttığınız istemci uygulaması başlangıç [Azure IOT Hub'ına bağlanmak ESP8266](iot-hub-arduino-huzzah-esp8266-get-started.md).
1. 30 c. SensorTag geçici ortam sıcaklık artırın Örneğin, Şamdan, SensorTag geçici açık.
1. Mantıksal uygulama tarafından gönderilen bir e-posta bildirim almanız gerekir.

   > [!NOTE]
   > E-posta hizmet sağlayıcınıza e-postayı gönderen, olduğundan emin olmak için gönderenin kimliğini doğrulamak gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ınızı ve Sıcaklık İzleme ve bildirimler için posta bağlayan bir mantıksal uygulama başarıyla oluşturdunuz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
