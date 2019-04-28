---
title: Azure IOT Edge cihazları çevrimdışı - çalışması | Microsoft Docs
description: IOT Edge cihazları ve modülleri uzun süre için internet bağlantısı olmadan nasıl çalışabilir ve IOT Edge çevrimdışı çok çalışması normal bir IOT cihazları nasıl olanak sağlayabileceğiniz anlayın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: e82c842ec8fce703c48c98eaf09ea5c8d91be9be
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60998626"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices-preview"></a>IOT Edge cihazları, modülleri ve alt cihazlar (Önizleme) genişletilmiş çevrimdışı özelliklerini anlama

Azure IOT Edge, IOT Edge cihazlarınıza genişletilmiş çevrimdışı işlemleri destekler ve alt kenar-olmayan cihazlarda çevrimdışı işlemleri çok sağlar. IOT Edge cihazı IOT Hub'ına bağlanmak için bir fırsat dolmadığı sürece, onu ve tüm alt aygıtların aralıklı işleviyle veya internet bağlantısı yok devam edebilirsiniz. 

>[!NOTE]
>IOT Edge için çevrimdışı destek konusu [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-it-works"></a>Nasıl çalışır?

IOT Edge cihazı çevrimdışı moduna girer, IOT Edge hub'ı üç rollerine götürür. İlk olarak, cihaz yeniden kadar bunları kaydeder ve Yukarı Akış çıkacak herhangi bir iletiyi depolar. İkinci olarak, bunlar çalışmaya devam edebilmesi için modüller ve alt cihazların kimliğini doğrulamak için IOT Hub adına hareket eder. Üçüncü olarak, IOT hub'ı aracılığıyla normalde çıkacak alt cihazlar arasındaki iletişimi sağlar. 

Aşağıdaki örnek, bir IOT Edge senaryo çevrimdışı modda nasıl çalıştığı gösterilmektedir:

1. **IOT Edge cihazı yapılandırın.**

   IOT Edge cihazları otomatik olarak etkinleştirilen çevrimdışı özellikleri vardır. Diğer IOT cihazlarına söz konusu özellik genişletmek için cihazlar IOT hub'ında arasında bir üst-alt ilişkisi bildirmeniz gerekir. 

2. **IOT Hub ile eşitleme.**

   En az bir kez IOT Edge çalışma zamanı yüklendikten sonra IOT Edge cihazı IOT Hub ile eşitlemek için çevrimiçi olması gerekir. Bu eşitleme, IOT Edge cihazı atanmış tüm alt cihazlar hakkında bilgi alır. IOT Edge cihazı de güvenli bir şekilde çevrimdışı işlemlerini etkinleştirmek için yerel önbelleği güncelleştirir ve telemetri iletilerini yerel depolama ayarlarını alır. 

3. **Çevrimdışı olur.**

   IOT Edge cihaz, dağıtılan modülleri ve tüm alt öğeleri IOT cihazları IOT Hub'ından bağlı değilken süresiz olarak çalışabilir. Modüller ve alt cihazları başlatma ve yeniden başlatma sırasında IOT Edge hub'ı ile kimlik doğrulaması yaparak çevrimdışı. IOT Hub'ına Yukarı Akış ilişkili telemetri yerel olarak depolanır. İletişim alt IOT cihazları veya modülleri arasında doğrudan yöntemler veya iletileri korunur. 

4. **Bağlayın ve IOT Hub ile yeniden eşitleyin.**

   IOT Hub ile bağlantı kurulduktan sonra IOT Edge cihazı yeniden eşitler. Yerel olarak depolanan iletilerinize depolanmış aynı sırada teslim edilir. Modüller istenen ve bildirilen özellikleri ve cihazlar arasındaki farkları mutabık kılınır. IOT Edge cihazı IOT cihazları atanan alt kümesine herhangi bir değişiklik güncelleştirir.

## <a name="restrictions-and-limits"></a>Kısıtlamaları ve sınırlar

Bu makalede açıklanan genişletilmiş çevrimdışı özellikleri kullanılabilir [IOT Edge 1.0.4 sürümü veya üzeri](https://github.com/Azure/azure-iotedge/releases). Önceki sürümlerde çevrimdışı özelliklerinin bir alt kümesi. IOT Edge mevcut genişletilmiş çevrimdışı özellikleri olmayan cihazlar, çalışma zamanı sürümü değiştirerek yükseltilemez ancak bu özellikler sağlamak için yeni bir IOT Edge cihaz kimliği ile yapılandırılması gerekir. 

Genişletilmiş çevrimdışı destek IOT hub'ı kullanılabildiği, tüm bölgelerde kullanılabilir **dışında** Doğu ABD.

IOT Edge olmayan cihazlar yalnızca alt cihazlar olarak eklenebilir. 

IOT Edge cihazları ve atanan alt cihazlarını çalışabilmesi ilk, tek seferlik eşitleme sonrasında süresiz olarak çevrimdışı. Ancak, depolama iletilerin süresi (TTL) ayarı ve iletileri depolamak için kullanılabilir disk alanı bağlıdır. 

## <a name="set-up-an-iot-edge-device"></a>Bir IOT Edge cihazı ayarlama

Bir IOT Edge cihazı alt IOT cihazları, genişletilmiş çevrimdışı yeteneklerini genişletmek Azure portalında üst-alt ilişkileri bildirmeniz gerekir.

### <a name="assign-child-devices"></a>Alt cihaz atama

Alt aygıtları aynı IOT Hub'ına kayıtlı herhangi bir kenar-olmayan cihaz olabilir. Üst-alt ilişkisi oluşturma yeni bir cihaz veya cihaz ayrıntıları sayfasına bir üst ya da IOT Edge cihazı veya alt IOT cihazı yönetebilir. 

   ![IOT Edge cihaz ayrıntıları sayfasından alt cihazları yönetme](./media/offline-capabilities/manage-child-devices.png)

Üst cihazları birden çok alt cihazlar olabilir, ancak alt cihaz, yalnızca bir üste sahip olabilir.

### <a name="specifying-dns-servers"></a>DNS sunucusu belirtme 

Sağlamlığını artırmak için ortamınızda kullanılan DNS sunucusu adresleri belirtmeniz önerilir. Örneğin, Linux üzerinde güncelleştirme **/etc/docker/daemon.json** (dosya oluşturmanız gerekebilir) eklemek için:

```json
{
    "dns": ["1.1.1.1"]
}
```

Yerel bir DNS sunucusu kullanıyorsanız, her 1.1.1.1 de yerel DNS sunucusunun IP adresi ile değiştirin. Değişikliklerin etkili olması docker hizmeti yeniden başlatın.


## <a name="optional-offline-settings"></a>İsteğe bağlı bir çevrimdışı ayarları

Cihazlarınızı çevrimdışı uzun dönemlerde oluşturan tüm iletileri toplanacak bekliyorsanız, böylece tüm iletileri depolayabilirsiniz IOT Edge hub'ı yapılandırın. İki değişiklikleri IOT Edge hub'ına ileti için uzun süreli depolama olanağı yapabilirsiniz vardır. İlk olarak, ayar yaşam süresini artırın. Ardından, ileti depolama için ek disk alanı ekleyin. 

### <a name="time-to-live"></a>Yaşam süresi

Ayar yaşam süresi dolmadan önce teslim edilecek bir ileti bekleyebileceği süreyi (saniye cinsinden) ' dir. 7200 saniye (iki saat) varsayılandır. 

Bu ayar, modül ikizi içinde depolanan IOT Edge hub'ın istenen bir özelliktir. İçinde Azure Portalı'nda yapılandırabilirsiniz **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** bölümünde veya doğrudan dağıtım bildirimi. 

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

İletileri, IOT Edge hub'ın kapsayıcı dosya sistemi varsayılan olarak depolanır. Bu depolama miktarını çevrimdışı gereksinimleriniz için yeterli değilse, IOT Edge cihazı yerel depolamada ayırabilirsiniz. Bir depolama kapsayıcısındaki işaret IOT Edge hub'ı için bir ortam değişkenini oluşturun. Ardından, bu depolama klasörü konak makinedeki bir klasöre bağlamak için oluşturma seçenekleri kullanın. 

Azure portalında ortam değişkenlerini ve IOT Edge hub'ı modülü oluşturma seçeneklerini yapılandırabilirsiniz **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** bölümü. Veya doğrudan dağıtım bildiriminde yapılandırabilirsiniz. 

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": {
            "HostConfig": {
                "Binds": ["<HostStoragePath>:<ModuleStoragePath>"],
                "PortBindings": {
                    "8883/tcp": [{"HostPort":"8883"}],
                    "443/tcp": [{"HostPort":"443"}],
                    "5671/tcp": [{"HostPort":"5671"}]
                }
            }
        }
    },
    "env": {
        "storageFolder": {
            "value": "<ModuleStoragePath>"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Değiştirin `<HostStoragePath>` ve `<ModuleStoragePath>` konak ve modül depolama ile yolu; hem konak hem modülü depolama yolu mutlak bir yol olmalıdır. Oluşturma seçeneklerinde konak ve modül depolama yollarını birbirine bağlayın. Ardından, modül depolama yolunu işaret eden bir ortam değişkeni oluşturun.  

Örneğin, `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]` dizin anlamına gelir **/etc/iotedge/storage** konağınız üzerinde sistem dizine eşlenmiş **/iotedge/depolama/** kapsayıcısı üzerinde. Veya Windows sistemleri için başka bir örnek `"Binds":["C:\\temp:C:\\contemp"]` dizin anlamına gelir **C:\\temp** konağınız üzerinde sistem dizine eşlenmiş **C:\\contemp** kapsayıcısı üzerinde. 

Oluşturma seçenekleri hakkında daha fazla ayrıntı bulabilirsiniz [docker docs](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="next-steps"></a>Sonraki adımlar

Genişletilmiş çevrimdışı işlemlerinde etkinleştirmek, [saydam bir ağ geçidi](how-to-create-transparent-gateway.md) senaryoları.
