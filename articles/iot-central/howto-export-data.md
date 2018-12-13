---
title: Azure IOT Central verilerinizi dışarı aktarma | Microsoft Docs
description: Azure IOT Central uygulamanızdan veri dışarı aktarma
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: cba0bad2e81ffddedfc4ca04e82e17e4286b389b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53312128"
---
# <a name="export-your-data-in-azure-iot-central"></a>Azure IOT Central verilerinizi dışarı aktarma

*Bu konu, Yöneticiler için geçerlidir.*

Bu makalede sürekli veri dışa aktarma Özelliği Azure IOT Central verilerinizi kendi değerlerinizle dışarı aktarmak için nasıl kullanılacağını açıklar **Azure Blob Depolama**, **Azure Event Hubs**, ve **Azure Service Bus** örnekleri. Dışarı aktarabilirsiniz **ölçümleri**, **cihazları**, ve **cihaz şablonları** sıcak yol ve Durgun yol analizi için kendi hedef. Microsoft Power BI hizmetinde uzun vadeli eğilim analizi çalıştırmak için Blob depolama alanına verileri dışarı aktarma veya olay hub'larına ve Service Bus dönüştürmek ve neredeyse gerçek zamanlı Azure Logic Apps veya Azure işlevleri ile verilerinizi büyütmek için verileri dışarı aktarma.

> [!Note]
> Verileri sürekli dışarı aktarma üzerinde etkinleştirdiğinizde, ileriye doğru o andan itibaren yalnızca verileri alın. Şu anda, verileri sürekli dışarı aktarma kapalıydı ne zaman bir kez verileri alınamıyor. Daha fazla geçmiş verileri korumak için verileri sürekli dışarı aktarma üzerinde erken açın.

## <a name="prerequisites"></a>Önkoşullar

- IOT Central uygulamanızda yönetici olmanız gerekir

## <a name="export-to-blob-storage"></a>BLOB depolamaya dışarı aktarma

Depolama hesabınıza bir kez dakikada ölçümleri, cihazları ve cihaz şablonları verileri son dosyasına dışarı aktardığınız beri toplu değişiklikler içeren her bir dosya ile aktarılır. Dışarı aktarılan veriler [Apache AVRO](https://avro.apache.org/docs/current/index.html) biçimi.

Daha fazla bilgi edinin [Blob depolamaya aktarmak](howto-export-data-blob-storage.md).

## <a name="export-to-event-hubs-and-service-bus"></a>Olay hub'ları ve hizmet veri yolu dışarı aktarma

Ölçümler, cihazları ve cihaz şablonları verileri, olay hub'ı veya Service Bus kuyruğuna veya konusuna dışarı aktarılır. Dışarı aktarılan ölçümleri verileri neredeyse gerçek zamanlı olarak ulaşır ve iletinin tamamen IOT Central için gönderilen cihazlarınızı yalnızca ölçüleri değerlerini içerir. Dışarı aktarılan cihazlar verileri toplu olarak dakikada ulaşır ve değişiklikleri tüm cihazların ayarları ve özellikleri içerir ve dışarı aktarılan cihaz şablonları tüm cihaz şablonlarına yapılan değişiklikleri içerir.


Daha fazla bilgi edinin [Event Hubs ve Service Bus](howto-export-data-event-hubs-service-bus.md).

## <a name="set-up-export-destination"></a>Dışarı aktarma hedef ayarlayın

Vermek için mevcut bir depolama/olay hub'ları / Service Bus sahip değilseniz, şu adımları izleyin:

### <a name="create-storage-account"></a>Depolama hesabı oluşturma

1. Oluşturma bir [Azure portalında yeni depolama hesabı](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Daha fazla bilgi [Azure depolama belgeleri](https://aka.ms/blobdocscreatestorageaccount).
2. Hesap türü seçin **genel amaçlı** veya **Blob Depolama**.
3. Bir abonelik seçin. 

    > [!Note] 
    > Artık olan diğer abonelikler için verileri dışarı aktarabilirsiniz **aynı** bir Kullandıkça Öde IOT Central uygulamanız için. Bu durumda bir bağlantı dizesi kullanarak bağlanır.

4. Depolama hesabınızdaki bir kapsayıcı oluşturun. Depolama hesabınıza gidin. Altında **Blob hizmeti**seçin **Blob'lara göz at**. Seçin **+ kapsayıcı** üst yeni bir kapsayıcı oluşturun.

### <a name="create-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

1. Oluşturma bir [Azure portalında yeni Event Hubs ad alanı](https://ms.portal.azure.com/#create/Microsoft.EventHub). Daha fazla bilgi [Azure Event Hubs belgeleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).
2. Bir abonelik seçin. 

    > [!Note] 
    > Artık olan diğer abonelikler için verileri dışarı aktarabilirsiniz **aynı** bir Kullandıkça Öde IOT Central uygulamanız için. Bu durumda bir bağlantı dizesi kullanarak bağlanır.
3. Event Hubs ad alanınız içinde bir olay hub'ı oluşturun. Ad alanınıza gidin ve seçin **+ olay hub'ı** en üstünde bir olay hub'ı örneği oluşturulamadı.

### <a name="create-service-bus-namespace"></a>Service Bus ad alanı oluşturma

1. Oluşturma bir [Azure portalında yeni hizmet veri yolu ad alanı](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5) . Daha fazla bilgi [Azure Service Bus belgeleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal).
2. Bir abonelik seçin. 

    > [!Note] 
    > Artık olan diğer abonelikler için verileri dışarı aktarabilirsiniz **aynı** bir Kullandıkça Öde IOT Central uygulamanız için. Bu durumda bir bağlantı dizesi kullanarak bağlanır.

3. Service Bus ad alanınıza gidin ve seçin **+ kuyruk** veya **+ konu** en üstünde bir kuyruk veya konuda vermek için oluşturulacak.

## <a name="set-up-continuous-data-export"></a>Verileri sürekli dışarı aktarma ayarlayın

Verileri dışarı aktarmak için bir depolama/olay hub'ları / Service Bus hedef olduğuna göre verileri sürekli dışarı aktarma ' için bu adımları izleyin. 

1. IOT Central uygulamanız için oturum açın.

2. Sol menüde **verileri sürekli dışarı aktarma**.

    > [!Note]
    > Verileri sürekli dışarı aktarma sol taraftaki menüde görmüyorsanız, yöneticinin uygulamanızda değildir. Verileri dışarı aktarma ' için yöneticinin konuşun.

    ![Yeni değerinde olay hub'ı oluşturma](media/howto-export-data/export_menu.PNG)

3. Tıklayın **+ yeni** sağ üst köşesindeki düğme. Birini **Azure Blob Depolama**, **Azure Event Hubs**, veya **Azure Service Bus** dışarı aktarma hedefi olarak. 

    > [!NOTE] 
    > Dışarı aktarmalar uygulama başına en fazla sayısı beştir. 

    ![Yeni verileri sürekli dışarı aktarma oluştur](media/howto-export-data/export_new.PNG)

4. Aşağı açılan liste kutusunda, **depolama hesabı/Event Hubs ad alanı/Service Bus ad alanı**. Son seçenek, listeden seçebilirsiniz **bir bağlantı dizesi girin**. 

    > [!NOTE] 
    > Depolama hesapları/Event Hubs ad alanlarını/hizmet veri yolu ad alanları yalnızca göreceksiniz **IOT Central uygulamanız ile aynı abonelikte**. Bu abonelik dışında bir hedefe dışarı aktarmak istiyorsanız seçin **bir bağlantı dizesi girin** ve 5. adıma bakın.

    > [!NOTE] 
    > 7 günlük deneme uygulamaları, verileri sürekli yapılandırmak için tek yolu dışarı aktarmak için bir bağlantı dizesidir. 7 günlük deneme uygulamalar, ilişkili Azure aboneliği olmadığı için budur.

    ![Yeni değerinde olay hub'ı oluşturma](media/howto-export-data/export_create.PNG)

5. (İsteğe bağlı) Seçerseniz, **bir bağlantı dizesi girin**, bağlantı dizenizi yapıştırmak için yeni kutusu görünür. Bağlantı dizesini almak için:
    - Depolama hesabı, Azure portalında depolama hesabı'na gidin.
        - Altında **ayarları**, tıklayın **erişim anahtarları**
        - Key1 bağlantı dizesini veya key2 bağlantı dizesini kopyalayın.
    - Olay hub'ları veya Service Bus, Azure Portalı'nda ad alanına gidin.
        - Altında **ayarları**, tıklayın **paylaşılan erişim ilkeleri**
        - Varsayılan seçin **RootManageSharedAccessKey** veya yeni bir tane oluşturun
        - Birincil veya ikincil bağlantı dizesini kopyalayın
 
6. Aşağı açılan liste kutusundan bir kapsayıcı/olay hub'ı / kuyruk veya konuda seçin.

7. Altında **dışarı aktarmak için veri**, her tür ayarlayarak dışarı aktarmak için veri türü belirtin **üzerinde**.

6. Verileri sürekli dışarı aktarma üzerinde etkinleştirmek için emin **verileri dışarı aktarma** olduğu **üzerinde**. **Kaydet**’i seçin.

  ![Yapılandırma verileri sürekli dışarı aktarma](media/howto-export-data/export_list.PNG)

7. Birkaç dakika sonra verilerinizi, seçtiğiniz hedef olarak görünür.

## <a name="next-steps"></a>Sonraki adımlar

Verileriniz dışarı aktarma artık bildiğinize göre sonraki adıma devam edin:

> [!div class="nextstepaction"]
> [Azure Blob depolama alanına verileri dışarı aktarma](howto-export-data-blob-storage.md)

> [!div class="nextstepaction"]
> [Azure Event Hubs'a ve Azure Service Bus için verileri dışarı aktarma](howto-export-data-event-hubs-service-bus.md)

> [!div class="nextstepaction"]
> [Nasıl verilerinizi Power bı'da görselleştirin](howto-connect-powerbi.md)
