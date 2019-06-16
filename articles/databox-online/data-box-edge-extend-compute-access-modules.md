---
title: Azure veri kutusu Edge üzerinde işlem ağa erişim modülleri yönetme | Microsoft Docs
description: Modülleri bir dış IP üzerinden erişmek için veri kutusu Edge üzerinde işlem ağı genişletin işlemini açıklamaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/17/2019
ms.author: alkohli
ms.openlocfilehash: 907647725dd6795b3b6482476de7442fbbf66114
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65917243"
---
# <a name="enable-compute-network-on-your-azure-data-box-edge"></a>Azure veri kutusu Ucunuzdaki işlem ağda etkinleştir

Bu makalede, Azure veri kutusu Edge'de çalışan modüller cihazda etkin işlem ağ nasıl erişebileceğiniz açıklanmıştır.

Ağı yapılandırmak için aşağıdaki adımları atmanız:

- Veri kutusu Edge cihazınıza işlem için bir ağ arabiriminde etkinleştir
- Veri kutusu Ucunuzdaki işlem ağda erişim için bir modül Ekle
- Modül etkin ağ arabirimine erişip doğrulayın

Bu öğreticide, bir Web sunucusu uygulaması modül senaryoyu göstermek amacıyla kullanacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapmanız gerekir:

- Veri kutusu Edge cihazı ile cihaz kurulumu tamamlandı.
- Tamamladınız **işlem yapılandırma** olarak başına adım [Öğreticisi: Azure veri kutusu Edge ile verileri dönüştürün](data-box-edge-deploy-configure-compute-advanced.md#configure-compute) Cihazınızda. Cihazınız, ilişkili bir IOT hub'ı kaynak, bir IOT cihaz ve bir IOT Edge cihazı olmalıdır.

## <a name="enable-network-interface-for-compute"></a>Ağ arabirimi için işlem etkinleştir

Cihazınızda modülleri dış bir ağ üzerinden erişmek için Cihazınızda bir ağ arabirimi bir IP adresi atamak gerekir. Bunlar yönetebileceğiniz yerel web kullanıcı Arabirimi ayarlarını işlem.

Yerel Web kullanıcı Arabirimi, işlem ayarlarını yapılandırmak için aşağıdaki adımları uygulayın.

1. Yerel web kullanıcı Arabirimi, Git **yapılandırma > ayarları işlem**.  

2. **Etkinleştirme** cihazda çalıştıracaksınız bir işlem modülü bağlanmak için kullanmak istediğiniz ağ arabirimi.

    - Statik IP adresleri kullanırken, ağ arabirimi için bir IP adresi girin.
    - DHCP kullanıyorsanız, IP adreslerini otomatik olarak atanır. Bu örnek, DHCP kullanır.

    ![1 işlem ayarlarını etkinleştirme](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-1.png)

3. Seçin **Uygula** ayarları uygulamak için. DHCP kullanıyorsanız, ağ arabirimine atanan IP adresini not edin.

    ![İşlem ayarlarını etkinleştirme](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-2.png)

## <a name="add-webserver-app-module"></a>Web sunucusu uygulaması Modül Ekle

Veri kutusu Edge Cihazınızda bir Web sunucusu uygulaması modül eklemek için aşağıdaki adımları uygulayın.

1. İlişkili veri kutusu Edge Cihazınızı IOT hub'ı kaynağına gidin ve ardından **IOT Edge cihazı**.
2. Veri kutusu Edge cihazınıza ile ilişkili IOT Edge cihazı seçin. Üzerinde **cihaz ayrıntıları**seçin **modülleri ayarlama**. Üzerinde **modül eklemek**seçin **+ Ekle** seçip **IOT Edge Modülü**.
3. İçinde **özel IOT Edge modülleri** dikey penceresinde:

    1. Belirtin bir **adı** dağıtmak istiyorsanız, Web sunucusu uygulaması modülü için.
    2. Sağlayan bir **görüntü URI'si** modülü resmi. Sağlanan ad ve etiket ile eşleşen bir modül alınır. Bu durumda, `nginx:stable` genel kullanıma (stable etiketli) bir kararlı nginx görüntüsü çeker [Docker deposunda](https://hub.docker.com/_/nginx/).
    3. İçinde **kapsayıcı oluşturma seçenekleri**, aşağıdaki örnek kodu yapıştırın:  

        ```
        {
            "HostConfig": {
                "PortBindings": {
                    "80/tcp": [
                        {
                            "HostPort": "8080"
                        }
                    ]
                }
            }
        }
        ```

        Bu yapılandırma üzerinde işlem ağ IP kullanarak modülü erişmenize olanak tanır *http* üzerinde bir TCP bağlantı noktası 8080 (80 olan varsayılan Web sunucusu bağlantı noktası ile).

        ![IOT Edge Özel Modül dikey penceresindeki bağlantı noktası bilgilerini belirtin](media/data-box-edge-extend-compute-access-modules/module-information.png)

    4. **Kaydet**’i seçin.

## <a name="verify-module-access"></a>Modül erişimi doğrulama

1. Modül başarıyla dağıtıldıktan ve çalıştığını doğrulayın. Üzerinde **cihaz ayrıntıları** sayfasında **modülleri** sekmesinde modülü çalışma zamanı durumunu olmalıdır **çalıştıran**.  
2. Web sunucusu uygulaması modülüne bağlayın. Bir tarayıcı penceresi açıp türü:

    `http://<compute-network-IP-address>:8080`

    Web sunucusu uygulaması çalıştığını görmelisiniz.

    ![Belirtilen bağlantı noktası üzerinden modülü bağlantıyı doğrulama](media/data-box-edge-extend-compute-access-modules/verify-connect-module-1.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla kullanıcıları yönetme](data-box-edge-manage-users.md) hakkında bilgi edinin.
