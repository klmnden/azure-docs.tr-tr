---
title: Azure işlevleri Event Grid yerel hata ayıklama
description: Bir Event Grid olayı tarafından tetiklenen Azure işlevleri yerel olarak hata ayıklamayı öğrenin
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 10/18/2018
ms.author: cshoe
ms.openlocfilehash: 96d88fafd6824ed85f1d91bab59374b3490a55b2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60428325"
---
# <a name="azure-function-event-grid-trigger-local-debugging"></a>Azure işlevi olay Kılavuzu tetikleyicisi yerel hata ayıklama

Bu makalede, bir depolama hesabı ile gerçekleşen bir Azure Event Grid olay işleyen bir yerel işlev hata ayıklamak gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar

- Oluşturun veya mevcut bir işlev uygulamasını kullanma
- Oluşturun veya mevcut bir depolama hesabı kullanın
- İndirme [ngrok](https://ngrok.com/) yerel işlevinizi çağırmak Azure izin vermek için

## <a name="create-a-new-function"></a>Yeni bir işlev oluşturma

İşlev uygulamanızı Visual Studio'da açın ve Çözüm Gezgini'nde proje adının üzerine sağ tıklatıp **Ekle > Yeni Azure işlevi**.

İçinde *yeni Azure işlevi* penceresinde **Event Grid tetikleyicisinin** tıklatıp **Tamam**.

![Yeni işlev oluşturma](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-add-function.png)

İşlev oluşturulduktan sonra kopya URL dosyasının en üstüne kılınmıştır ve kod dosyası açın. Bu konum, Event Grid tetikleyicisinin yapılandırırken kullanılır.

![Konuma kopyalayın](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-copy-location.png)

Ardından, ile başlayan satırında bir kesme noktası ayarlamak `log.LogInformation`.

![kesme noktası Ayarla](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-set-breakpoint.png)


Ardından, **F5 tuşuna basarak** hata ayıklama oturumu başlatmak için.

## <a name="allow-azure-to-call-your-local-function"></a>Azure yerel işlevinizi çağırmak izin ver

Makinenizde hata ayıklaması yapılan bir işlev uygulamasına ayırmak için Azure bulutta yerel işlevinizi ile iletişim kurmak bir yol etkinleştirmeniz gerekir.

[Ngrok](https://ngrok.com/) yardımcı programı, Azure makinenizde çalışan bir işlevi çağırmak bir yol sağlar. Başlangıç *ngrok* aşağıdaki komutu kullanarak:

```bash
ngrok http -host-header=localhost 7071
```
Yardımcı program ayarlama gibi komut penceresi aşağıdaki ekran görüntüsüne benzer görünmelidir:

![Ngrok Başlat](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-ngrok.png)

Kopyalama **HTTPS** oluşturulan URL *ngrok* çalıştırılır. Bu değer, event grid olay uç nokta yapılandırma sırasında kullanılır.

## <a name="add-a-storage-event"></a>Bir depolama olay Ekle

Azure portalını açın ve bir depolama hesabına gidin ve tıklayarak **olayları** seçeneği.

![Depolama hesabı olay Ekle](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-add-event.png)

İçinde *olayları* penceresinde tıklayarak **olay aboneliği** düğmesi. İçinde *bile abonelik* penceresinde tıklayarak *uç noktası türü* açılan seçip **Web kancası**.

![Abonelik türü seçin](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-type.png)

Uç nokta türü yapılandırıldıktan sonra tıklayarak **bir uç nokta seçin** uç nokta değerini yapılandırmak için.

![Uç nokta türü seçin](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-endpoint.png)

*Abone uç noktası* değeri oluşur üç farklı değerleri. HTTPS tarafından oluşturulan URL önekidir *ngrok*. Sona eklenen işlev adı ile işlevi kod dosyasında bulunan URL'den URL geri kalanında gelir. İşlev kod dosyası URL'den başlayarak *ngrok* URL'sini değiştirir `http://localhost:7071` ve değiştirir ad işlevi `{functionname}`.

Aşağıdaki ekran görüntüsünde, son URL nasıl görünmesi gerektiğini göstermektedir:

![Uç nokta seçimi](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-endpoint-selection.png)

Uygun değere girdikten sonra tıklayın **seçimini Onayla**.

> [!IMPORTANT]
> Her başlattığınızda *ngrok*, HTTPS URL'sini yeniden oluşturulur ve değerini değiştirir. Bu nedenle, kullanıma, işlevinizi aracılığıyla azure'a her zaman yeni bir olay aboneliği oluşturmalısınız *ngrok*.

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme

Artık bir Event Grid olayı işlemek için yerel bir işlev tetiklemek için depolama hesabınıza bir dosyayı karşıya yükleyebilirsiniz. 

Açık [Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) bağlanın, depolama hesabı. 

- Genişletin **Blob kapsayıcıları** 
- Sağ tıklayıp **Blob kapsayıcısı Oluştur**.
- Kapsayıcı adı **test**
- Seçin *test* kapsayıcı
- Tıklayın **karşıya** düğmesi
- Tıklayın **dosyaları karşıya yükle**
- Bir dosya seçin ve blob kapsayıcısına yükleyin

## <a name="debug-the-function"></a>Hata ayıklama işlevi

Event Grid, yeni bir dosya depolama kapsayıcısına karşıya tanır sonra yerel işlevde kesme noktası isabet.

![Ngrok Başlat](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-breakpoint.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynakları temizlemek için Sil **test** depolama hesabınızdaki kapsayıcı.

## <a name="next-steps"></a>Sonraki adımlar

- [Karşıya yüklenen görüntüleri yeniden boyutlandırmayı Event Grid kullanarak otomatikleştirme](../event-grid/resize-images-on-storage-blob-upload-event.md)
- [Azure işlevleri için olay Kılavuzu tetikleyicisi](./functions-bindings-event-grid.md)
