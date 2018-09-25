---
title: Azure IOT Edge çevrimdışı özellikleri | Microsoft Docs
description: Nasıl IOT Edge cihazları ve modülleri çevrimdışı süre uzun süre çalışabilir ve IOT Edge çevrimdışı çok çalışması normal bir IOT cihazları nasıl olanak sağlayabileceğiniz anlayın.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f4afad753da4a314ade3fb7433c6be3e489e05b0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033694"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices-preview"></a>IOT Edge cihazları, modülleri ve alt cihazlar (Önizleme) genişletilmiş çevrimdışı özelliklerini anlama

Azure IOT Edge, IOT Edge cihazlarınıza genişletilmiş çevrimdışı işlemleri destekler ve alt kenar-olmayan cihazlarda çevrimdışı işlemleri çok sağlar. IOT Edge cihazı IOT Hub'ına bağlanmak için bir fırsat dolmadığı sürece, onu ve tüm alt aygıtların aralıklı işleviyle veya internet bağlantısı yok devam edebilirsiniz. 

>[!NOTE]
>IOT Edge için çevrimdışı destek konusu [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-it-works"></a>Nasıl çalışır?

Edge hub'ı, IOT Edge cihazı çevrimdışı moduna girer, üç rollerinde alır. İlk olarak, cihaz yeniden kadar bunları kaydeder ve Yukarı Akış çıkacak herhangi bir iletiyi depolar. İkinci olarak, bunlar çalışmaya devam edebilmesi için modüller ve alt cihazların kimliğini doğrulamak için IOT Hub adına hareket eder. Üçüncü olarak, IOT hub'ı aracılığıyla normalde çıkacak alt cihazlar arasındaki iletişimi sağlar. 

Aşağıdaki örnek, bir IOT Edge senaryo çevrimdışı modda nasıl çalıştığı gösterilmektedir:

1. **IOT Edge cihazı yapılandırın.**

   IOT Edge cihazları otomatik olarak etkinleştirilen çevrimdışı özellikleri vardır. Diğer IOT cihazlarına söz konusu özellik genişletmek için cihazlar IOT hub'ında arasında bir üst-alt ilişkisi bildirmeniz gerekir. 

2. **IOT Hub ile eşitleme.**

   En az bir kez IOT Edge çalışma zamanı yüklendikten sonra IOT Edge cihazı IOT Hub ile eşitlemek için çevrimiçi olması gerekir. Bu eşitleme, IOT Edge cihazı atanmış tüm alt cihazlar hakkında bilgi alır. IOT Edge cihazı de güvenli bir şekilde çevrimdışı işlemlerini etkinleştirmek için yerel önbelleği güncelleştirir ve telemetri iletilerini yerel depolama ayarlarını alır. 

3. **Çevrimdışı olur.**

   IOT Edge cihaz, dağıtılan modülleri ve tüm alt öğeleri IOT cihazları IOT Hub'ından bağlı değilken süresiz olarak çalışabilir. Modüller ve alt cihazları başlatma ve yeniden başlatma sırasında Edge hub'ı ile kimlik doğrulaması yaparak çevrimdışı. IOT Hub'ına Yukarı Akış ilişkili telemetri yerel olarak depolanır. İletişim alt IOT cihazları veya modülleri arasında doğrudan yöntemler veya iletileri korunur. 

4. **Bağlayın ve IOT Hub ile yeniden eşitleyin.**

   IOT Hub ile bağlantı kurulduktan sonra IOT Edge cihazı yeniden eşitler. Yerel olarak depolanan iletilerinize depolanmış aynı sırada teslim edilir. Modüller istenen ve bildirilen özellikleri ve cihazlar arasındaki farkları mutabık kılınır. IOT Edge cihazı IOT cihazları atanan alt kümesine herhangi bir değişiklik güncelleştirir.

## <a name="restrictions-and-limits"></a>Kısıtlamaları ve sınırlar

Bu makalede açıklanan genişletilmiş çevrimdışı özellikleri kullanılabilir [IOT Edge sürüm 1.0.2 veya üzeri](https://github.com/Azure/azure-iotedge/releases). Önceki sürümlerde çevrimdışı özelliklerinin bir alt kümesi. IOT Edge mevcut genişletilmiş çevrimdışı özellikleri olmayan cihazlar, çalışma zamanı sürümü değiştirerek yükseltilemez ancak bu özellikler sağlamak için yeni bir IOT Edge cihaz kimliği ile yapılandırılması gerekir. 

Genişletilmiş çevrimdışı desteği, IOT hub'ı kullanılabildiği, Doğu ABD ve Batı Avrupa tüm bölgelerde kullanılabilir. 

IOT Edge olmayan cihazlar yalnızca alt cihazlar olarak eklenebilir. 

IOT Edge cihazları ve atanan alt cihazlarını çalışabilmesi ilk, tek seferlik eşitleme sonrasında süresiz olarak çevrimdışı. Ancak, depolama iletilerin süresi (TTL) ayarı ve iletileri depolamak için kullanılabilir disk alanı bağlıdır. 

## <a name="set-up-an-edge-device"></a>Bir Edge cihazı ayarlama

Genişletilmiş çevrimdışı dönemleri sırasında gerçekleştirmek istediğiniz tüm IOT Edge cihaz MQTT iletişim kurmak için IOT Edge çalışma zamanı yapılandırın. 

Bir IOT Edge cihazı alt IOT cihazları, genişletilmiş çevrimdışı yeteneklerini genişletmek Azure portalında üst-alt ilişkileri bildirmeniz gerekir.

### <a name="set-the-upstream-protocol-to-mqtt"></a>Yukarı Akış Protokolü için MQTT ayarlayın

Edge hub'ı hem Yukarı Akış Protokolü olarak MQTT ile iletişim kurmak için Edge Aracısı'nı yapılandırın. Bu protokol, dağıtım bildiriminde ortam değişkenlerini kullanarak bildirilir. 

Azure portalında, Edge Aracısı modül tanımları ve Edge hub'ı seçerek erişebilirsiniz **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** modülleri bir dağıtım için ayarlarken düğmesi. Her iki modüller için adlı bir ortam değişkeni oluşturma **UpstreamProtocol** ve değerini ayarlamak **MQTT**. 

Dağıtım şablonu JSON'da, ortam değişkenleri, aşağıdaki örnekte gösterildiği gibi bildirilir: 

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}]}}}"
    },
    "env": {
        "UpstreamProtocol": {
            "value": "MQTT"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

### <a name="assign-child-devices"></a>Alt cihaz atama

Alt aygıtları aynı IOT Hub'ına kayıtlı herhangi bir kenar-olmayan cihaz olabilir. Üst-alt ilişkisi oluşturma yeni bir cihaz veya cihaz ayrıntıları sayfasına bir üst ya da IOT Edge cihazı veya alt IOT cihazı yönetebilir. 

   ![IOT Edge cihaz ayrıntıları sayfasından alt cihazları yönetme](./media/offline-capabilities/manage-child-devices.png)

Üst cihazları birden çok alt cihazlar olabilir, ancak alt cihaz, yalnızca bir üste sahip olabilir.

## <a name="optional-offline-settings"></a>İsteğe bağlı bir çevrimdışı ayarları

Daha sonra oluşturulan, tüm iletileri toplamak istediğiniz kadar süreyle çevrimdışı dönemler için cihazlarınızı bekliyorsanız tüm iletileri depolayabileceğiniz Edge hub'ı yapılandırın. İki değişiklikleri Edge hub'ına ileti için uzun süreli depolama olanağı yapabilirsiniz vardır. İlk ayarı yaşam süresini artırın ve ardından ileti depolama için ek disk alanı ekleyin. 

### <a name="time-to-live"></a>Yaşam süresi

Ayar yaşam süresi dolmadan önce teslim edilecek bir ileti bekleyebileceği süreyi (saniye cinsinden) ' dir. 7200 saniye (iki saat) varsayılandır. 

Bu ayar, modül ikizi içinde depolanan Edge hub'ın istenen bir özelliktir. İçinde Azure Portalı'nda yapılandırabilirsiniz **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** bölümünde veya doğrudan dağıtım bildirimi. 

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="additional-offline-storage"></a>Çevrimdışı ek depolama alanı

Varsayılan olarak, iletiler Edge hub'ın kapsayıcı dosya sistemi içinde saklanır. Bu depolama miktarını çevrimdışı gereksinimleriniz için yeterli değilse, IOT Edge cihazı yerel depolamada ayırabilirsiniz. Bir depolama kapsayıcısındaki işaret Edge hub'ı için bir ortam değişkenini oluşturmak için ihtiyacınız. Ardından, bu depolama klasörü konak makinedeki bir klasöre bağlamak için oluşturma seçenekleri kullanın. 

Azure portalında ortam değişkenlerini ve Edge hub'ı modülü oluşturma seçeneklerini yapılandırabilirsiniz **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** bölümü. Veya doğrudan dağıtım bildiriminde yapılandırabilirsiniz. 

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": "{\"HostConfig\":{\"Binds\":[\"C:\\\\HostStoragePath:C:\\\\ModuleStoragePath\"],\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}]}}}"
    },
    "env": {
        "storageFolder": {
            "value": "C:\\\\ModuleStoragePath"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Saydam bir ağ geçidi senaryolarınız için genişletilmiş çevrimdışı işlemlerinde etkinleştirme [Linux](how-to-create-transparent-gateway-linux.md) veya [Windows](how-to-create-transparent-gateway-windows.md) cihazlar. 