---
title: Azure veri kutusu Edge işlem yönetimi | Microsoft Docs
description: Tetikleyici, modüller, işlem yapılandırmasını görüntüleme gibi Edge işlem ayarları yönetme, yapılandırma, Azure veri kutusu Edge üzerinde Azure Portalından kaldırmak açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/26/2019
ms.author: alkohli
ms.openlocfilehash: 58c4f42859f735a81a3e3edc801daff5d26194a0
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997657"
---
# <a name="manage-compute-on-your-azure-data-box-edge"></a>İşlem, Azure veri kutusu edge'de yönetme

Bu makalede, Azure veri kutusu Edge üzerinde işlem yönetme. İşlem Azure portal aracılığıyla yönetmenize veya yerel web kullanıcı Arabirimi. Modüller, Tetikleyiciler, yönetmek için Azure portalını kullanın ve işlem yapılandırması ve yerel web kullanıcı Arabirimi, işlem ayarlarını yönetmek için.

> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.


Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Tetikleyicileri yönetme
> * İşlem yapılandırmasını yönetme


## <a name="manage-triggers"></a>Tetikleyicileri yönetme

Bulut ortamınızda veya Cihazınızda üzerinde işlem gerçekleştirmek isteyebileceğiniz şeyler olaylardır. Örneğin, bir paylaşımında bir dosya oluşturulduğunda, bir olay olur. Tetikleyici olayları tetikleyebilir. Veri kutusu Edge için Tetikleyiciler, yanıt dosyası olayları veya bir zamanlama olabilir.

- **Dosya**: Bu tetikleyiciler gibi bir dosya değişikliği bir dosyanın dosya olaylara yanıt vermek için geçerlidir.
- **Zamanlanmış**: Bu tetikleyiciler, yanıt olarak başlangıç tarihi, başlangıç saati ve yineleme aralığı ile tanımladığınız bir planlamada olan.


### <a name="add-a-trigger"></a>Tetikleyici ekleyin

Bir tetikleyici oluşturmak için Azure portalında aşağıdaki adımları uygulayın.

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **Edge işlem > tetikleyici**. Seçin **+ tetikleyicisi Ekle** komut çubuğunda.

    ![Tetikleyici Ekle'yi seçin](media/data-box-edge-manage-compute/add-trigger-1.png)

2. İçinde **tetikleyicisi Ekle** dikey penceresinde, Tetikleyiciniz için benzersiz bir ad sağlayın.
    
    <!--Trigger names can only contain numbers, lowercase letters, and hyphens. The share name must be between 3 and 63 characters long and begin with a letter or a number. Each hyphen must be preceded and followed by a non-hyphen character.-->

3. Seçin bir **türü** tetikleyici için. Seçin **dosya** tetikleyici olduğunda dosya olaya yanıt olarak. Seçin **zamanlanmış** tanımlanmış bir zamanda başlatmak ve belirli bir yineleme aralıklarla çalıştırmak için tetikleyici istediğinizde. Seçiminize bağlı olarak, farklı bir dizi seçenek sunulur.

    - **Dosya tetikleyici** -açılan listeden bir bağlı paylaşım seçin. Tetikleyici bu paylaşımın dosya olayı tetiklendiğinde bir Azure işlevi çağırır.

        ![SMB paylaşımı ekleme](media/data-box-edge-manage-compute/add-file-trigger.png)

    - **Zamanlanan tetikleyici** - başlangıç tarih/saat belirtin ve yineleme aralığı saat, dakika ve saniye. Ayrıca, bir konu adını girin. Bir konu, cihaza dağıtılan bir modül için tetikleyici yönlendirmek için esneklik sunar.

        Bir örnek yol dizesi: `"route3": "FROM /* WHERE topic = 'topicname' INTO BrokeredEndpoint("modules/modulename/inputs/input1")"`.

        ![NFS paylaşımı ekleme](media/data-box-edge-manage-compute/add-scheduled-trigger.png)

4. Seçin **Ekle** tetikleyicisi oluşturmak için. Tetikleyici oluşturma sürüyor bir bildirimi gösterir. Tetikleyici oluşturulduktan sonra dikey penceresinde Yeni Tetikleyici yansıtacak şekilde güncelleştirir.
 
    ![Güncelleştirilmiş tetikleyici listesi](media/data-box-edge-manage-compute/add-trigger-2.png)

### <a name="delete-a-trigger"></a>Bir tetikleyiciyi silin

Bir tetikleyici silmek için Azure portalında aşağıdaki adımları uygulayın.

1. Tetikleyiciler listesinden silmek istediğiniz tetikleyiciyi seçin.

    ![Tetikleyici seçin](media/data-box-edge-manage-compute/add-trigger-1.png)

2. Sağ tıklayın ve ardından **Sil**.

    ![Sil'i seçin](media/data-box-edge-manage-compute/add-trigger-1.png)

3. Onayınız istendiğinde **Evet**’e tıklayın.

    ![Silmeyi onayla](media/data-box-edge-manage-compute/add-trigger-1.png)

Silinmesini yansıtacak şekilde Tetikleyicileri güncelleştirmeleri listesi.

## <a name="manage-compute-configuration"></a>İşlem yapılandırmasını yönetme

İşlem yapılandırmasını görüntülemek için var olan bir işlem yapılandırmasını kaldırmak veya yenilemek veri kutusu Ucunuzdaki için IOT cihaz ve IOT Edge cihazı için erişim anahtarlarını eşitlemek için işlem yapılandırması için Azure portalını kullanın.

### <a name="view-compute-configuration"></a>İşlem yapılandırmasını görüntüleme

Cihazınız için işlem yapılandırmasını görüntülemek için Azure portalında aşağıdaki adımları uygulayın.

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **Edge işlem > modülleri**. Seçin **görüntülemek işlem** komut çubuğunda.

    ![Görünüm işlem seçin](media/data-box-edge-manage-compute/view-compute-1.png)

2. İşlem yapılandırma Cihazınızda not edin. İşlem yapılandırıldığında, bir IOT hub'ı kaynak oluşturuldu. Bu IOT hub'ı kaynağı altında bir IOT cihaz ve bir IOT Edge cihazı yapılandırılır. IOT Edge Cihazınızda çalıştırmak için yalnızca Linux modülleri desteklenir.

    ![Yapılandırmayı görüntüle](media/data-box-edge-manage-compute/view-compute-2.png)


### <a name="remove-compute-configuration"></a>İşlem yapılandırmasını Kaldır

Cihazınız için var olan Edge işlem yapılandırmasını kaldırmak için Azure portalında aşağıdaki adımları uygulayın.

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **Edge işlem > başlama**. Seçin **kaldırmak işlem** komut çubuğunda.

    ![Remove işlem seçin](media/data-box-edge-manage-compute/remove-compute-1.png)

2. İşlem yapılandırmasını kaldırırsanız, işlem yeniden kullanmanız gerektiği durumlarda Cihazınızı yeniden yapılandırmanız gerekir. Onayınız istendiğinde seçin **Evet**.

    ![Remove işlem seçin](media/data-box-edge-manage-compute/remove-compute-2.png)

### <a name="sync-up-iot-device-and-iot-edge-device-access-keys"></a>IOT cihaz ve IOT Edge cihazı ayarlama erişim anahtarları Eşitle

İşlem, veri kutusu Edge üzerinde yapılandırdığınızda, bir IOT cihaz ve bir IOT Edge cihazı oluşturulur. Bu cihazlar otomatik olarak simetrik erişim tuşu atanır. Güvenlik açısından en iyisi, bu anahtarlar düzenli olarak IOT Hub hizmeti ile döndürülür.

Bu anahtarlarını döndürmek için oluşturduğunuz IOT Hub hizmetine gidin ve IOT cihaz veya IOT Edge cihazı seçin. Her cihaz, birincil erişim anahtarı ve bir ikincil erişim anahtarları vardır. Birincil erişim anahtarını ikincil erişim tuşunu atayın ve ardından birincil erişim anahtarını yeniden oluştur.

IOT cihaz ve IOT Edge cihaz anahtarları Döndürülmüş, en son erişim anahtarlarını almak için veri kutusu Ucunuzdaki yapılandırmasına yenilemek gerekir. Eşitleme, cihaz IOT cihaz ve IOT Edge cihazı için en son anahtarlarını edinmeniz yardımcı olur. Veri kutusu Edge yalnızca birincil erişim anahtarlarını kullanır.

Cihazınız için erişim anahtarlarını eşitleme için Azure portalında aşağıdaki adımları uygulayın.

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **Edge işlem > başlama**. Seçin **yenileme Yapılandırması** komut çubuğunda.

    ![Yenileme yapılandırması](media/data-box-edge-manage-compute/refresh-configuration-1.png)

2. Seçin **Evet** onaylamanız istendiğinde.

     ![Evet, istendiğinde seçin](media/data-box-edge-manage-compute/refresh-configuration-2.png)

3. Eşitleme tamamlandıktan sonra iletişim kutusunu kapatın.

## <a name="enable-a-network-interface-for-compute"></a>İşlem için bir ağ arabirimi etkinleştir

Veri kutusu Edge Cihazınızda çalışan bir modül erişim gerekebilir. Modül erişmek için harici olarak, Cihazınızda bir ağ arabirimi bir IP adresi atamak gerekir. Bunlar yönetebileceğiniz yerel web kullanıcı Arabirimi ayarlarını işlem.

Yerel Web kullanıcı Arabirimi, işlem ayarlarını yapılandırmak için aşağıdaki adımları uygulayın.

1. Yerel web kullanıcı Arabirimi, Git **yapılandırma > ayarları işlem**.  

2. **Etkinleştirme** işlem modüller cihazda bağlanmak için kullanmak istediğiniz ağ arabirimi. 

    - Statik IP adresleri kullanırken, ağ arabirimi için bir IP adresi girin.
    - DHCP kullanıyorsanız, IP adreslerini otomatik olarak atanır.

3. Seçin **Uygula** ayarları uygulamak için.

    ![İşlem ayarlarını etkinleştirme](media/data-box-edge-manage-compute/compute-settings-1.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla kullanıcıları yönetme](data-box-edge-manage-users.md) hakkında bilgi edinin.
