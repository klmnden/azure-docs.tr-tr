---
title: "IOT hub iletilerinizi Azure veri depolama alanına Kaydet | Microsoft Docs"
description: "IOT hub iletilerinizi Azure blob depolama alanına kaydetmek için IOT hub'ı ileti yönlendirme'yi kullanın. IOT hub iletileri IOT aygıtınızdan gönderilen algılayıcı verileri gibi bilgileri içerir."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT veri depolama, IOT algılayıcı veri depolama"
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/04/2017
ms.author: xshi
ms.openlocfilehash: f6b334dbc9903d0080b74052062de7564aa4a993
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>Azure blob depolama alanınızın algılayıcı verileri içeren IOT hub iletileri kaydetme

![Uçtan uca diyagramı](media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bir Azure depolama hesabı ve blob depolama alanınızın IOT hub iletileri depolamak için bir Azure işlev uygulaması oluşturmayı öğrenin.

## <a name="what-you-do"></a>Neler

- Bir Azure Storage hesabı oluşturun.
- Depolama birimine iletileri yönlendirmek için IOT hub'ınızı hazırlayın.

## <a name="what-you-need"></a>Ne gerekiyor

- [Cihazınızı ayarlamak](iot-hub-raspberry-pi-kit-node-get-started.md) aşağıdaki gereksinimleri karşılamak için:
  - Etkin bir Azure aboneliği
  - Aboneliğinizdeki IOT hub'ı 
  - IOT hub'ına iletileri gönderir çalışan bir uygulama

## <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

1. İçinde [Azure portal](https://portal.azure.com/), tıklatın **kaynak oluşturma** > **depolama** > **depolama hesabı**  >  **Oluşturma**.

2. Depolama hesabı için gerekli bilgileri girin:

   ![Azure Portal'da depolama hesabı oluşturma](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Ad**: depolama hesabı adı. Adın genel olarak benzersiz olması gerekir.

   * **Kaynak grubu**: IOT hub'ınızı kullandığı aynı kaynak grubunu kullanın.

   * **Panoya sabitle**: Panodan IoT hub'ınıza kolay erişim için bu seçeneği belirleyin.

3. **Oluştur**’a tıklayın.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>IOT hub'ınıza iletileri yönlendirmek için depolama alanı hazırlama

IOT hub'ı, Azure depolama yönlendirme iletileri BLOB olarak yerel olarak destekler.

### <a name="add-storage-as-a-custom-endpoint"></a>Özel bir uç noktası olarak depolama ekleme

Azure portalında IOT hub'ınıza gidin. Tıklatın **uç noktaları** > **eklemek**. Uç nokta adı ve seçin **Azure depolama kapsayıcısının** uç nokta türü olarak. Önceki bölümde oluşturduğunuz depolama hesabını seçmek için seçiciyi kullanın. Depolama kapsayıcısı oluşturun ve onu seçin ve ardından tıklatın **Tamam**.

  ![IOT Hub ' özel bir uç noktası oluşturma](media\iot-hub-store-data-in-azure-table-storage\2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>Bir rota için rota veri depolama alanına ekleme

Tıklatın **yollar** > **Ekle** ve yol için bir ad girin. Seçin **cihaz iletilerini** veri kaynağı ve yeni depolama uç noktası oluşturuldu rotadaki uç noktası olarak seçin. Girin `true` sorgu dizesi olarak ardından **kaydetmek**.

  ![Bir rota IOT Hub oluşturma](media\iot-hub-store-data-in-azure-table-storage\3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Sık kullanılan yolu telemetri (isteğe bağlı) için bir rota ekleyin

Varsayılan olarak, IOT Hub, yerleşik uç noktasına başka bir yol ile eşleşmiyor tüm iletileri yönlendirir. Tüm telemetri iletilerini şimdi, depolama alanına iletileri yönlendiren kuralıyla eşleşen olduğundan, yerleşik uç noktasına yazılacak iletileri için başka bir yol eklemeniz gerekir. Birden çok Uç noktalara iletileri yönlendirmek için ek ücret yoktur.

> [!NOTE]
> Ek, telemetri iletilerini işleme yapıyorlarsa değil, bu adımı atlayabilirsiniz.

Tıklatın **Ekle** yollar bölmesinden ve yol için bir ad girin. Seçin **cihaz iletilerini** veri kaynağı olarak ve **olayları** uç nokta olarak. Girin `true` sorgu dizesi olarak ardından **kaydetmek**.

  ![Hot yolu yol IOT Hub oluşturma](media\iot-hub-store-data-in-azure-table-storage\4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Depolama kapsayıcısı iletinize doğrulayın

1. IOT hub'ınıza ileti göndermek için Cihazınızda örnek uygulamayı çalıştırın.

2. [Azure Storage Gezgini yükleyip](http://storageexplorer.com/).

3. Depolama Gezgini'ni açın, **Azure Hesap Ekle** > **oturum**ve Azure hesabınızda oturum açın.

4. Azure aboneliğiniz tıklayın > **depolama hesapları** > depolama hesabınız > **Blob kapsayıcıları** >, kapsayıcı.

   Blob kapsayıcısında oturum IOT hub'ınıza aygıtınızdan gönderilen iletileri görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Başarıyla Azure depolama hesabı ve yönlendirilmiş iletiler IOT Hub'ından o depolama hesabındaki bir blob kapsayıcısını oluşturdunuz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
