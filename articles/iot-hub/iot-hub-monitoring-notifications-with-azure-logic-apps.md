---
title: IOT Uzaktan izleme ve bildirimler Azure Logic Apps ile | Microsoft Docs
description: Otomatik olarak algılanan anomalileri, posta kutunuz e-posta bildirimleri göndermek ve IOT hub'ınızda sıcaklık IOT izlemek için Azure Logic Apps kullanın.
author: robinsh
keywords: IOT, IOT bildirimleri izleme sıcaklık IOT izleme
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/19/2019
ms.author: robinsh
ms.openlocfilehash: 26637468f44e12f7ad66f907e0f6be3d907e578f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64719314"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>IOT Uzaktan izleme ve IOT hub ve posta kutusu bağlanan Azure Logic Apps ile bildirimleri

![Uçtan uca diyagramı](media/iot-hub-monitoring-notifications-with-azure-logic-apps/iot-hub-e2e-logic-apps.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) şirket içi iş akışlarınızı düzenleyin ve bulut Hizmetleri, size gibi kuruluşlar, bir veya daha fazla ve çeşitli protokoller üzerinden. Mantıksal uygulama, ardından koşullar ve yineleyiciler gibi yerleşik denetimlerini kullanarak sıralı bir veya daha fazla eylemler tarafından izlenen bir tetikleyici ile başlar. Bu esneklik, Logic Apps izleme IOT senaryoları için ideal bir IOT çözümü sağlar. Örneğin, telemetri verilerini IOT Hub uç noktasında bir CİHAZDAN geldiğini bir Azure depolama blobu, veri ambarı veri anomalileri uyar, bir cihaz bir hata bildirirse, bir teknisyen ziyaret zamanlamak için e-posta uyarıları göndermek için mantıksal uygulama iş akışlarını başlatabilir , ve benzeri.

## <a name="what-you-learn"></a>Öğrenecekleriniz

IOT hub'ınıza ve Sıcaklık İzleme ve bildirimler için posta bağlanan bir mantıksal uygulama oluşturmayı öğrenin.

Bir uygulama özelliği, cihaz üzerinde çalışan istemci kodu ayarlar `temperatureAlert`, her bir telemetri iletisi, IOT hub'ınıza gönderir. İstemci kodu bir sıcaklığı 30 C yukarıda algıladığında, bu özellik ayarlar `true`; Aksi takdirde özelliği ayarlar `false`.

Bu konu başlığında, IOT hub'ınızda yönlendirme ayarlama, ileti göndermek için ayarladığınız `temperatureAlert = true` Service Bus uç noktada gelen iletilerin tetikler ve bir e-posta bildirimi gönderen bir mantıksal uygulama için bir Service Bus uç noktası ve ayarlayın.

## <a name="what-you-do"></a>Neler

* Service Bus ad alanı oluşturma ve Service Bus kuyruğuna ekleyin.
* Özel uç nokta ve yönlendirme kuralı içeren Service Bus kuyruğuna bir sıcaklık uyarı iletileri yönlendirmek için IOT hub'ınızı ekleyin.
* Oluşturma, yapılandırma ve kullanma, Service Bus kuyruğundan ileti ve istenen alıcılara bildirim e-postaları göndermek için bir mantıksal uygulamayı test etme.

## <a name="what-you-need"></a>Ne gerekiyor

* Tamamlamak [Raspberry Pi çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya bir cihaz öğreticileri; Örneğin, [node.js ile Raspberry Pi](iot-hub-raspberry-pi-kit-node-get-started.md). Bu, aşağıdaki gereksinimleri kapsar:

  * Etkin bir Azure aboneliği.
  * Azure IOT hub, aboneliğiniz altında.
  * Azure IOT hub'ınıza telemetri iletilerini gönderir, bir cihaz üzerinde çalışan bir istemci uygulaması.

## <a name="create-service-bus-namespace-and-queue"></a>Service Bus ad alanı ve Kuyruk oluşturma

Service Bus ad alanı ve kuyruğu oluşturun. Bu konuda daha sonra bir mantıksal uygulama tarafından seçilir ve bildirim e-posta göndermek için tetikleme burada Service Bus kuyruğuna bir sıcaklık uyarıda doğrudan iletiler için IOT hub'ınızdaki bir yönlendirme kuralını oluşturun.

### <a name="create-a-service-bus-namespace"></a>Service Bus ad alanı oluşturma

1. Üzerinde [Azure portalında](https://portal.azure.com/)seçin **+ kaynak Oluştur** > **tümleştirme** > **Service Bus**.

1. Üzerinde **ad alanı oluşturma** bölmesinde aşağıdaki bilgileri sağlayın:

   **Ad**: Service bus ad alanı adı. Ad Azure genelinde benzersiz olmalıdır.

   **Fiyatlandırma katmanı**: Seçin **temel** aşağı açılan listeden. Temel katmanı, Bu öğretici için yeterlidir.

   **Kaynak grubu**: IOT hub'ınıza kullandığı aynı kaynak grubunu kullanın.

   **Konum**: IOT hub'ınıza kullandığı aynı konumu kullanın.

   ![Azure portalında bir service bus ad alanı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1-create-service-bus-namespace-azure-portal.png)

1. **Oluştur**’u seçin. Sonraki adıma geçmeden önce dağıtımın bekleyin.

### <a name="add-a-service-bus-queue-to-the-namespace"></a>Service Bus kuyruğuna ad alanına ekleyin

1. Service Bus ad alanı'nı açın. Service Bus ad alanına almak için en kolay yolu seçmektir **kaynak grupları** kaynak bölmesinden kaynak grubunuzu seçin ve ardından kaynak listesinden Service Bus ad alanını seçin.

1. Üzerinde **Service Bus Namespace** bölmesinde **+ kuyruk**.

1. Sıra için bir ad girin ve ardından **Oluştur**. Kuyruk başarıyla oluşturulduğunda **kuyruk Oluştur** bölmeyi kapatır.

   ![Azure portalında bir service bus kuyruğu ekleme](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-queue.png)

1. Yeniden **Service Bus Namespace** bölmesi altında **varlıkları**seçin **kuyrukları**. Service Bus kuyruğuna açın ve ardından **paylaşılan erişim ilkeleri** >  **+ Ekle**.

1. Onay ilkesine bir ad girin **Yönet**ve ardından **Oluştur**.

   ![Azure portalında bir service bus kuyruğu İlkesi Ekle](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2-add-service-bus-queue-azure-portal.png)

## <a name="add-a-custom-endpoint-and-routing-rule-to-your-iot-hub"></a>IOT hub'ınıza bir özel uç nokta ve yönlendirme kuralı Ekle

IOT hub'ınıza Service Bus kuyruğu için özel bir uç nokta ekleyin ve burada, mantıksal uygulamanız tarafından seçilir bir sıcaklık uyarıda bu uç noktaya iletileri yönlendirmek için bir ileti yönlendirme kuralını oluşturun. Bir yönlendirme sorgu yönlendirme kuralını kullanan `temperatureAlert = "true"`, değerini temel alarak iletilerini iletecek şekilde `temperatureAlert` cihazda çalışan istemci kodu tarafından ayarlanan uygulama özelliği. Daha fazla bilgi için bkz. [ileti özelliklerine bağlı yönlendirme sorgusu iletisi](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#message-routing-query-based-on-message-properties).

### <a name="add-a-custom-endpoint"></a>Özel uç nokta Ekle

1. IOT hub'ınızı açın. IOT hub'ına almak için en kolay yolu seçmektir **kaynak grupları** kaynak bölmesinden kaynak grubunuzu seçin ve ardından IOT hub'ı kaynakları listesinden seçin.

1. Altında **Mesajlaşma**seçin **ileti yönlendirme**. Üzerinde **ileti yönlendirme** bölmesinde **özel uç noktalar** sekmesini seçip **+ Ekle**. Aşağı açılan listesinden **hizmet veri yolu kuyruğu**.

   ![Azure portalındaki IOT hub'ınıza bir uç nokta ekleyin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-iot-hub-custom-endpoint.png)

1. Üzerinde **bir service bus uç noktası ekleme** bölmesinde aşağıdaki bilgileri girin:

   **Uç nokta adı**: Uç nokta adı.

   **Service bus ad alanı**: Oluşturduğunuz ad alanı seçin.

   **Hizmet veri yolu kuyruğu**: Oluşturduğunuz kuyruk seçin.

   ![Azure portalındaki IOT hub'ınıza bir uç nokta ekleyin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3-add-iot-hub-endpoint-azure-portal.png)

1. **Oluştur**’u seçin. Uç nokta başarıyla oluşturulduktan sonra sonraki adıma geçin.

### <a name="add-a-routing-rule"></a>Yönlendirme kuralı ekleme

1. Yeniden **ileti yönlendirme** bölmesinde **yollar** sekmesini seçip **+ Ekle**.

1. Üzerinde **bir yol eklemek** bölmesinde aşağıdaki bilgileri girin:

   **Ad**: Yönlendirme kuralı adı.

   **Uç nokta**: Oluşturduğunuz uç noktayı seçin.

   **Veri kaynağı**: Seçin **cihaz Telemetri iletilerini**.

   **Yönlendirme sorgusu**: `temperatureAlert = "true"` yazın.

   ![Azure portalında bir yönlendirme kuralı Ekle](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4-add-routing-rule-azure-portal.png)

1. **Kaydet**’i seçin. Kapatabilirsiniz **ileti yönlendirme** bölmesi.

## <a name="create-and-configure-a-logic-app"></a>Bir mantık uygulaması oluşturma ve yapılandırma

Önceki bölümde, Service Bus kuyruğuna bir sıcaklık uyarı içeren iletileri yönlendirmek için IOT hub'ınızı ayarlayın. Artık, Service Bus kuyruğu izlemek ve kuyruğa bir ileti eklendiğinde bir e-posta bildirimi göndermek için bir mantıksal uygulama ayarlarsınız.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

1. Seçin **kaynak Oluştur** > **tümleştirme** > **mantıksal uygulama**.

1. Aşağıdaki bilgileri girin:

   **Ad**: Mantıksal uygulamanın adı.

   **Kaynak grubu**: IOT hub'ınıza kullandığı aynı kaynak grubunu kullanın.

   **Konum**: IOT hub'ınıza kullandığı aynı konumu kullanın.

   ![Azure portalında mantıksal uygulama oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-a-logic-app.png)

1. **Oluştur**’u seçin.

### <a name="configure-the-logic-app-trigger"></a>Mantıksal uygulama tetikleyicisini yapılandırın

1. Mantıksal uygulamayı açın. Mantıksal uygulamaya almak için en kolay yolu seçmektir **kaynak grupları** kaynak bölmesinden kaynak grubunuzu seçin ve ardından kaynak listesinden mantıksal uygulamanızı seçin. Mantıksal uygulama seçtiğinizde, Logic Apps Tasarımcısı açılır.

1. Logic Apps Tasarımcısı'nda, aşağı kaydırarak **şablonları** seçip **boş mantıksal uygulama**.

   ![Azure portalında boş mantıksal uygulama ile başlayın](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5-start-with-blank-logic-app-azure-portal.png)

1. Seçin **tüm** sekmesini seçip **Service Bus**.

   ![Azure portalında mantıksal uygulamanızı oluşturmaya başlamak için Service Bus'ı seçin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6-select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Altında **Tetikleyicileri**seçin **(Otomatik Tamamlama) kuyrukta veya daha fazla ileti ulaştığında**.

   ![Azure portalında mantıksal uygulamanızın Tetikleyici seçin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-service-bus-trigger.png)

1. Service bus bağlantı oluşturun.
   1. Bir bağlantı adı girin ve Service Bus ad alanınızı listeden seçin. Sonraki ekranda açılır.

      ![Azure portalında mantıksal uygulamanız için bir hizmet veri yolu bağlantı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-connection-1.png)

   1. Service bus İlkesi (RootManageSharedAccessKey) seçin. Ardından **Oluştur**.

      ![Azure portalında mantıksal uygulamanız için bir hizmet veri yolu bağlantı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7-create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Son ekranında, için **kuyruk adı**, açılan listeden oluşturulan kuyruk seçin. Girin `175` için **en fazla ileti sayısı**.

      ![Mantıksal uygulamanız için hizmet veri yolu bağlantı en fazla ileti sayısı belirtin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8-specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)

   1. Seçin **Kaydet** yaptığınız değişiklikleri kaydetmek için Logic Apps Tasarımcısı üst kısmındaki menüde.

### <a name="configure-the-logic-app-action"></a>Mantıksal uygulama eylemi yapılandırın

1. Bir SMTP hizmeti bağlantısı oluşturun.

   1. **Yeni adım**'ı seçin. İçinde **eylem seçin**seçin **tüm** sekmesi.

   1. Tür `smtp` arama kutusunda **SMTP** hizmet arama sonucunda ve ardından **e-posta Gönder**.

      ![Azure portalında mantıksal uygulamanızda bir SMTP bağlantısı oluşturun](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9-create-smtp-connection-logic-app-azure-portal.png)

   1. Posta kutunuz için SMTP bilgilerini girebilir ve ardından **Oluştur**.

      ![Azure portalında mantıksal uygulamanızda SMTP bağlantı bilgilerini girin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10-enter-smtp-connection-info-logic-app-azure-portal.png)

      SMTP hakkında bilgi alın [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), ve [Yahoo Posta](https://help.yahoo.com/kb/SLN4075.html).

      > [!NOTE]
      > Bağlantı kurmak için SSL'yi devre dışı gerekebilir. Bu durumda ve SSL bağlantı kurulduktan sonra yeniden etkinleştirmek istiyorsanız, isteğe bağlı bir adım, bu bölümün sonunda bakın.

   1. Gelen **yeni parametre Ekle** üzerindeki açılan **e-posta Gönder** adım, select **gelen**, **için**, **konu**ve **gövdesi**. ' A tıklayın veya seçim kutusunu kapatmak için ekran üzerinde herhangi bir yere dokunun.

      ![SMTP bağlantı e-posta alanları seçin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/smtp-connection-choose-fields.png)

   1. E-posta adresinizi girin **gelen** ve **için**, ve `High temperature detected` için **konu** ve **gövdesi**. Varsa **uygulama ve Bu akışta kullanılan bağlayıcılardan dinamik içerik ekleyin** iletişim kutusunu açar, seçin **Gizle** kapatmak için. Bu öğreticide dinamik içerik kullanmayın.

      ![Doldurma SMTP bağlantı e-posta alanları](media/iot-hub-monitoring-notifications-with-azure-logic-apps/fill-in-smtp-connection-fields.png)

   1. Seçin **Kaydet** SMTP bağlantıyı kaydetmek için.

1. (İsteğe bağlı) E-posta sağlayıcınız ile bağlantı kurmak ve yeniden etkinleştirmek SSL'yi devre dışı tablonuz varsa aşağıdaki adımları izleyin:

   1. Üzerinde **mantıksal uygulama** bölmesi altında **geliştirme araçları**seçin **API bağlantıları**.

   1. SMTP bağlantı API bağlantıları listesinden seçin.

   1. Üzerinde **smtp API bağlantısı** bölmesi altında **genel**seçin **Düzenle API bağlantısı**.

   1. Üzerinde **Düzenle API bağlantısı** bölmesinde **SSL'yi etkinleştir?** , e-posta hesabı için parolayı yeniden girin ve seçin **Kaydet**.

      ![Azure portalında mantıksal uygulamanızda SMTP API bağlantısını düzenleme](media/iot-hub-monitoring-notifications-with-azure-logic-apps/re-enable-smtp-connection-ssl.png)

Mantıksal uygulamanız artık Service Bus kuyruğundan sıcaklık uyarılar işlemek ve hesabınıza e-posta bildirimleri göndermek hazırdır.

## <a name="test-the-logic-app"></a>Mantıksal uygulamayı test etme

1. İstemci uygulamanın, Cihazınızda başlatın.

1. Fiziksel bir cihaz kullanıyorsanız, sıcaklığı 30 derece c aşana kadar dikkatle ısı algılayıcı yakın bir ısı kaynak Getir Çevrimiçi simülatör kullanıyorsanız, istemci kodu 30 c aşan telemetri iletilerini rastgele çıkarır

1. Mantıksal uygulama tarafından gönderilen e-posta bildirimleri almaya başlaması gerekir.

   > [!NOTE]
   > E-posta gönderir, olduğundan emin olmak için gönderen kimliğini doğrulamak e-posta hizmet sağlayıcınıza gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ınıza ve Sıcaklık İzleme ve bildirimler için posta bağlanan bir mantıksal uygulama başarıyla oluşturdunuz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
