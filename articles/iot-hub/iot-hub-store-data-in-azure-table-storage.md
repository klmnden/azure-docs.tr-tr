---
title: IOT hub iletilerinizi Azure veri depolamaya kaydetme | Microsoft Docs
description: IOT Hub ileti yönlendirme, IOT hub iletilerinizi Azure blob depolamanızda kaydetmek için kullanın. IOT hub'a iletileri IOT cihazınızın gönderilen sensör verilerini gibi bilgileri içerir.
author: rangv
manager: ''
keywords: IOT veri depolama, IOT algılayıcı veri depolama
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 84355453a5cb8d8f42abdcbde5432651c9c035b0
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856299"
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>Algılayıcı verileri Azure blob depolama alanınızın içeren IOT hub iletilerini Kaydet

![Uçtan uca diyagramı](./media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Azure depolama hesabınız ve IOT hub iletilerini Azure Blob storage'da depolamak için bir Azure işlev uygulaması oluşturmayı öğrenin.

## <a name="what-you-do"></a>Neler

- Bir Azure Storage hesabı oluşturun.
- Depolama rota iletileri IOT hub'ınızı ayarlayın.

## <a name="what-you-need"></a>Ne gerekiyor

- [Cihazınızı ayarlama](iot-hub-raspberry-pi-kit-node-get-started.md) aşağıdaki gereksinimleri karşılamak için:
  - Etkin bir Azure aboneliği
  - Aboneliğinizde bir IOT hub 
  - IOT hub'ınıza ileti gönderen bir çalışan uygulama

## <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

1. İçinde [Azure portalında](https://portal.azure.com/), tıklayın **kaynak Oluştur** > **depolama** > **depolama hesabı**.

2. Depolama hesabı için gereken bilgileri girin:

   ![Azure portalında depolama hesabı oluşturma](./media/iot-hub-store-data-in-azure-table-storage/1_azure-portal-create-storage-account.png)

   * **Ad**: depolama hesabının adı. Adın genel olarak benzersiz olması gerekir.

   * **Hesap türü**: seçin `Storage (general purpose v1)`.

   * **Konum**: IOT hub'ınıza kullandığı aynı konumu seçin.

   * **Çoğaltma**: seçin `Locally-redundant storage (LRS)`.

   * **Performans**: seçin `Standard`.

   * **Güvenli aktarım gereklidir**: seçin `Disabled`.

   * **Abonelik**: Azure aboneliğinizi seçin.

   * **Kaynak grubu**: IOT hub'ınıza kullandığı aynı kaynak grubunu kullanın.

   * **Panoya sabitle**: Panodan IoT hub'ınıza kolay erişim için bu seçeneği belirleyin.

3. **Oluştur**’a tıklayın.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Depolama rota iletileri IOT hub'ınıza hazırlama

IOT hub'ı yönlendirme iletileri Azure depolama blobları olarak yerel olarak destekler. Azure IOT hub'ı özel uç noktaları hakkında daha fazla bilgi edinmek için başvurabilirsiniz [yerleşik IOT Hub uç noktaları listesi](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints#custom-endpoints).

### <a name="add-storage-as-a-custom-endpoint"></a>Depolama alanı özel bir uç noktası ekleme

1. Azure portalındaki IOT hub'ınıza gidin. 

2. Tıklayın **uç noktaları** > **ekleme**. 

3. Uç nokta adı ve seçin **Azure depolama kapsayıcısı** uç nokta türü olarak. 

4. Önceki bölümde oluşturduğunuz depolama hesabını seçmek için seçiciyi kullanın. Bir depolama kapsayıcısı oluşturmak ve onu seçin ve ardından tıklayın **Tamam**.

   ![IOT Hub'ında özel uç nokta oluşturma](./media/iot-hub-store-data-in-azure-table-storage/2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>Bir rota için rota veri depolama alanına ekleme

1. Tıklayın **yollar** > **Ekle** ve yol için bir ad girin. 

2. Seçin **cihaz iletilerini** veri kaynağı ve yeni depolama uç noktası oluşturulan rotadaki uç nokta olarak seçin. 

3. Girin `true` sorgu dizesi ardından **Kaydet**.

   ![IOT Hub'ında yönlendirme oluşturma](./media/iot-hub-store-data-in-azure-table-storage/3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Etkin yolu telemetri (isteğe bağlı) için bir rota ekleyin

Varsayılan olarak, yerleşik uç noktasına başka bir yolun eşleşmeyen tüm iletileri IOT hub'ı yönlendirir. Bu yana tüm telemetri iletilerini artık, depolama alanına iletileri yönlendiren kuralla eşleşmesi iletilerini yerleşik uç noktasına yazılacak için başka bir yol eklemeniz gerekir. Birden fazla uç nokta için iletileri yönlendirmek için ek ücret yoktur.

> [!NOTE]
> Size, telemetri iletilerini üzerinde işleme ek uygulamıyorsanız bu adımı atlayabilirsiniz.

1. Tıklayın **Ekle** yollar bölmesinden ve yol için bir ad girin. 

2. Seçin **cihaz iletilerini** veri kaynağı olarak ve **olayları** uç nokta olarak. 

3. Girin `true` sorgu dizesi ardından **Kaydet**.

  ![IOT hub'ına anında yolu yönlendirme oluşturma](./media/iot-hub-store-data-in-azure-table-storage/4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>İletinizi depolama kapsayıcınızda doğrulayın

1. Cihazınızı IOT hub'ınıza ileti göndermek için örnek uygulamayı çalıştırın.

2. [Azure Depolama Gezgini'ni indirip](http://storageexplorer.com/).

3. Depolama Gezgini'ni açın, **Azure hesabı Ekle** > **oturum**ve Azure hesabınızda oturum açın.

4. Azure aboneliğinize tıklayın > **depolama hesapları** > depolama hesabınız > **Blob kapsayıcıları** > kapsayıcı.

   Blob kapsayıcısında günlüğe IOT hub'ınıza, CİHAZDAN gönderilen iletilere görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bu öğreticide, bir depolama hesabı eklenir ve ardından depolama hesabına yazılması IOT Hub'ından iletiler için yönlendirme eklendi. Oluşturduğunuz kaynakları temizlemek için yönlendirme bilgilerini kaldırın ve ardından depolama hesabını silin. 

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. Tıklayın **kaynak grupları** ve kullandığınız kaynak grubunu seçin. Kaynak grubunda listesi görüntülenir. 

   > [!NOTE]
   > Kaynak grubunda bulunan tüm kaynakları kaldırmak istiyorsanız, tıklayın **Sil** kaynak grubunu silmek için yönergeleri izleyin. Kaynakları temizleme işlemini tamamladınız ve sonraki bölüme atlayabilirsiniz bu her şeyi bu kaynak grubunda kaldırır.

3. Bu öğreticide kullanılan IOT hub'ı tıklayın. 

4. IOT hub'ı bölmesinden **yollar**. Yönlendirme kuralı eklenir ve ardından tıklayın yanındaki onay kutusuna tıklayın **Sil**. Bu rota silmek istediğinizden eminseniz tıklayın sorulduğunda **Evet**.

   ![Yönlendirme kuralını Kaldır](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-routing.png)

   Yönlendirme bölmesini kapatın. Kaynak grubu bölmesine döndürülür.

5. IOT hub'ınızda tekrar tıklayın. 

6. IOT hub'ı bölmesinden **uç noktaları**. Depolama kapsayıcısı için eklediğiniz uç noktasını yanındaki onay kutusuna tıklayın ve ardından tıklayın **Sil**. Seçili uç noktasını silmek istediğinizden eminseniz tıklayın sorulduğunda **Evet**.

    ![Odebrat koncový bod](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-endpoint.png)

    Uç noktaları bölmesini kapatın. Kaynak grubu bölmesine döndürülür. 

7.  Bu öğretici için ayarladığınız depolama hesabına tıklayın. 

8.  Depolama hesabı bölmesinde tıklatın **Sil** depolama hesabını kaldırmak için. Yönlendirilirsiniz **depolama hesabını Sil** bölmesi.

   ![Depolama hesabı Kaldır](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-storageaccount.png)

8.  Depolama hesabı adını yazın, ardından tıklatın **Sil** bölmesinin alt kısmındaki. 

## <a name="next-steps"></a>Sonraki adımlar

Başarıyla Azure depolama hesabınızın ve yönlendirilmiş iletileri IOT Hub'ından o depolama hesabındaki bir blob kapsayıcısını oluşturdunuz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
