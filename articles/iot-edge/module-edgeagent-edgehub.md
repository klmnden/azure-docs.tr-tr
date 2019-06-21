---
title: EdgeAgent ve EdgeHub istenen özellikler başvuru - Azure IOT Edge | Microsoft Docs
description: Belirli özellikleri ve değerlerini edgeAgent ve edgeHub modül ikizlerini için gözden geçirin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: e8a8170023c8f529894522e27a4c6231325089af
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190993"
---
# <a name="properties-of-the-iot-edge-agent-and-iot-edge-hub-module-twins"></a>IOT Edge aracısı ve IOT Edge hub'ı modül ikizlerini özellikleri

IOT Edge aracısı ve IOT Edge hub'ı, IOT Edge çalışma zamanını oluşturan iki modüllerdir. Her modülün gerçekleştirdiği hangi görevleri hakkında daha fazla bilgi için bkz. [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md). 

Bu makalede, istenen özellikleri ve çalışma zamanı modül ikizlerini bildirilen özellikleri sağlar. IOT Edge cihazlarında modülleri dağıtma hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve IOT Edge'de rotaları oluşturmak hakkında bilgi edinin](module-composition.md).

Modül ikizi şunları içerir: 

* **İstenen özellikleri**. Modül okuyabilirsiniz ve çözüm arka ucu, istenen özellikleri ayarlayabilirsiniz. Modülü, ayrıca istenen özelliklerinde değişiklik bildirimleri alabilirsiniz. İstenen özellikler, bildirilen özellikleriyle birlikte, modül yapılandırması veya koşulları eşitlemek için kullanılır.

* **Bildirilen özellikler**. Modül, bildirilen özellikleri ayarlayabilirsiniz ve çözüm arka ucu okuyun ve bunları sorgulayabilirsiniz. Bildirilen özellikler, istenen özellikleriyle birlikte, modül yapılandırması veya koşulları eşitlemek için kullanılır. 

## <a name="edgeagent-desired-properties"></a>İstenen EdgeAgent özellikleri

IOT Edge Aracısı modül ikizi adlı `$edgeAgent` ve cihaz ve IOT hub'ı üzerinde çalışan IOT Edge Aracısı arasındaki iletişimi düzenler. İstenen özellikleri, bir dağıtım bildirimi tek cihaz veya ölçekli bir dağıtımının parçası olarak belirli bir cihazda uygulama işlemi sırasında ayarlanır. 

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| schemaVersion | "1.0" olması gerekir | Evet |
| Runtime.Type | "Docker" olması gerekir | Evet |
| runtime.settings.minDockerVersion | Bu dağıtım bildirimi tarafından gereken en düşük Docker sürümü için ayarlayın | Evet |
| runtime.settings.loggingOptions | IOT Edge Aracısı kapsayıcı günlüğe kaydetme seçeneklerini içeren dizeleştirilmiş JSON. [Docker günlük seçenekleri](https://docs.docker.com/engine/admin/logging/overview/) | Hayır |
| runtime.settings.registryCredentials<br>. {registryId} .username | Kapsayıcı kayıt defteri kullanıcı adı. Azure Container Registry için kullanıcı kayıt defteri adı genellikle adıdır.<br><br> Kayıt defteri kimlik bilgilerini ortak olmayan modül görüntüleri için gerekli değildir. | Hayır |
| runtime.settings.registryCredentials<br>. {registryId} .password | Kapsayıcı kayıt defteri parolası. | Hayır |
| runtime.settings.registryCredentials<br>. {registryId} .address | Kapsayıcı kayıt defteri adresi. Azure Container Registry için genellikle adresidir *{kayıt defteri adı}.azurecr.io/Azure-vote-Front:v1*. | Hayır |  
| systemModules.edgeAgent.type | "Docker" olması gerekir | Evet |
| systemModules.edgeAgent.settings.image | IOT Edge Aracısı görüntüsünü URI'si. Şu anda, IOT Edge Aracısı'nı güncelleştirme kendisini mümkün değildir. | Evet |
| systemModules.edgeAgent.settings<br>.createOptions | IOT Edge Aracısı container oluşturulması için seçenekleri içeren bir dizeleştirilmiş JSON. [Docker oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Hayır |
| systemModules.edgeAgent.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bir dağıtım kullanarak bildirim uygulandığında, IOT hub'ı bu özelliği ayarlar. Parçası olmayan bir dağıtım bildirimi. |
| systemModules.edgeHub.type | "Docker" olması gerekir | Evet |
| systemModules.edgeHub.status | "Çalışıyor" gerekir | Evet |
| systemModules.edgeHub.restartPolicy | "Her zaman" olması gerekir | Evet |
| systemModules.edgeHub.settings.image | IOT Edge hub'ı görüntüsünü URI'si. | Evet |
| systemModules.edgeHub.settings<br>.createOptions | IOT Edge hub'ı container oluşturulması için seçenekleri içeren bir dizeleştirilmiş JSON. [Docker oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Hayır |
| systemModules.edgeHub.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bir dağıtım kullanarak bildirim uygulandığında, IOT hub'ı bu özelliği ayarlar. Parçası olmayan bir dağıtım bildirimi. |
| modüller. {Moduleıd} .version | Bu modülün sürümünü temsil eden kullanıcı tanımlı bir dize. | Evet |
| modules.{moduleId}.type | "Docker" olması gerekir | Evet |
| modüller. {Moduleıd} .status | {"çalışıyor" \| "durduruldu"} | Evet |
| modules.{moduleId}.restartPolicy | {"hiçbir zaman" \| "hata" \| "üzerinde-sağlıksız" \| "her zaman"} | Evet |
| modüller. {Moduleıd}.settings.image | Modülü görüntüsü URI. | Evet |
| modules.{moduleId}.settings.createOptions | Modül container oluşturulması için seçenekleri içeren bir dizeleştirilmiş JSON. [Docker oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Hayır |
| modüller. {Moduleıd}.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bir dağıtım kullanarak bildirim uygulandığında, IOT hub'ı bu özelliği ayarlar. Parçası olmayan bir dağıtım bildirimi. |

## <a name="edgeagent-reported-properties"></a>EdgeAgent bildirilen özellikler

IOT Edge Aracısı özellikleri ana üç parça bilgi içeren rapor:

1. Son görülme istenen özellikleri uygulama durumu;
2. Şu anda IOT Edge aracı tarafından raporlanan cihaz üzerinde çalışan modüllerin durumunu; ve
3. İstenen özellikleri kullanarak cihaz üzerinde şu anda çalışan bir kopyası.

Bu son bilgilerden biri, bir kopyasını geçerli istenen özellikleri cihazın en son istenen özellikleri uygulanmış veya önceki bir dağıtım bildirimi hala çalışıyor bildirmek kullanışlıdır.

> [!NOTE]
> İle sorgulanabilir olarak bildirilen özellikleri IOT Edge Aracısı'nın yararlıdır [IOT Hub sorgu dili](../iot-hub/iot-hub-devguide-query-language.md) uygun ölçekte dağıtımların durumunu araştırmak için. IOT Edge Aracısı özellikleri durumu kullanma hakkında daha fazla bilgi için bkz. [IOT Edge dağıtımlarını anlama tek tek cihazlarda veya uygun ölçekte](module-deployment-monitoring.md).

Aşağıdaki tabloda, istenen özelliklerden kopyalanır bilgileri içermez.

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu tamsayı en son istenen özellikleri IOT Edge aracısı tarafından işletildiğinde sürümünü gösterir. |
| lastDesiredStatus.code | Bu durum kodu IOT Edge aracısı tarafından görülen son istenen özellikleri gösterir. İzin verilen değerler: `200` Başarı `400` geçersiz yapılandırma `412` geçersiz şema sürümü `417` istenen özellikleri boş `500` başarısız oldu |
| lastDesiredStatus.description | Durum açıklaması metni |
| deviceHealth | `healthy` tüm modüllerin çalışma zamanı durum ya da ise `running` veya `stopped`, `unhealthy` Aksi takdirde |
| configurationHealth.{deploymentId}.health | `healthy` çalışma zamanı durumu {Deploymentıd} dağıtımı tarafından tüm modüllerin geçerli olduğunda `running` veya `stopped`, `unhealthy` Aksi takdirde |
| runtime.platform.OS | Cihaz üzerinde çalışan işletim Sisteminin raporlama |
| Runtime.Platform.Architecture | Cihazda raporlama CPU mimarisi |
| systemModules.edgeAgent.runtimeStatus | IOT Edge Aracısı durumu: {"çalışıyor" \| "kötü"} |
| systemModules.edgeAgent.statusDescription | IOT Edge aracı durumu metin açıklaması. |
| systemModules.edgeHub.runtimeStatus | IOT Edge hub'ı durumu: {"çalışıyor" \| "durduruldu" \| "başarısız" \| "geri alma" \| "kötü"} |
| systemModules.edgeHub.statusDescription | IOT Edge hub'ı, sağlıksız durum metin açıklaması. |
| systemModules.edgeHub.exitCode | Kapsayıcı çıkarsa, IOT Edge hub'ı kapsayıcı tarafından bildirilen çıkış kodu |
| systemModules.edgeHub.startTimeUtc | IOT Edge hub'ı en son başlatıldığı saat |
| systemModules.edgeHub.lastExitTimeUtc | Zaman zaman IOT Edge hub'ı en son çıkıldı |
| systemModules.edgeHub.lastRestartTimeUtc | IOT Edge hub'ı en son ne zaman başlatıldığı saat |
| systemModules.edgeHub.restartCount | Kaç kez yeniden başlatma ilkesi bir parçası olarak bu modülü yeniden başlatıldı. |
| modüller. {Moduleıd} .runtimeStatus | Modül durumu: {"çalışıyor" \| "durduruldu" \| "başarısız" \| "geri alma" \| "kötü"} |
| modules.{moduleId}.statusDescription | Durumu iyi olmayan sistem durumunda modülün metin açıklaması. |
| modules.{moduleId}.exitCode | Kapsayıcı çıkılıyorsa modülü kapsayıcı tarafından bildirilen çıkış kodu |
| modüller. {Moduleıd} .startTimeUtc | Modül son başlatıldığı saat |
| modules.{moduleId}.lastExitTimeUtc | Zaman zaman modülü son çıkıldı |
| modules.{moduleId}.lastRestartTimeUtc | Zaman modülün en son ne zaman yeniden başlatıldı |
| modules.{moduleId}.restartCount | Kaç kez yeniden başlatma ilkesi bir parçası olarak bu modülü yeniden başlatıldı. |

## <a name="edgehub-desired-properties"></a>İstenen EdgeHub özellikleri

Modül ikizi IOT Edge hub'ı adlı `$edgeHub` ve IOT Edge hub'ı bir cihaz ve IOT hub'ı çalıştıran arasındaki iletişimi düzenler. İstenen özellikleri, bir dağıtım bildirimi tek cihaz veya ölçekli bir dağıtımının parçası olarak belirli bir cihazda uygulama işlemi sırasında ayarlanır. 

| Özellik | Açıklama | Dağıtım bildiriminde gerekli |
| -------- | ----------- | -------- |
| schemaVersion | "1.0" olması gerekir | Evet |
| yollar. {Routetablename} | Bir IOT Edge hub'ı yolu temsil eden bir dize. Daha fazla bilgi için [bildirmek yollar](module-composition.md#declare-routes). | `routes` Öğesi var ancak boş olabilir. |
| storeAndForwardConfiguration.timeToLiveSecs | Saniye cinsinden, IOT Edge hub'ı yönlendirme uç noktalarından olup kestiyseniz iletileri tutar. IOT hub'ı veya yerel bir modül. Değer pozitif bir tamsayı olabilir. | Evet |

## <a name="edgehub-reported-properties"></a>EdgeHub bildirilen özellikler

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu tamsayı en son istenen özellikleri IOT Edge hub'ı tarafından işlenen sürümünü gösterir. |
| lastDesiredStatus.code | IOT Edge hub'ı tarafından görülen son istenen özellikleri başvuran durum kodu. İzin verilen değerler: `200` Başarı `400` geçersiz yapılandırma `500` başarısız oldu |
| lastDesiredStatus.description | Durum metin açıklaması. |
| istemciler. {cihaz veya modül kimliği} .status | Bu cihaz veya modül bağlantı durumu. Olası değerler {"bağlı" \| "bağlantısı"}. Yalnızca modül kimlikleri bağlantısı kesilmiş olabilir. IOT Edge hub'ına bağlanmak için aşağı akış cihazlarının yalnızca bağlı olduğunda görünür. |
| istemciler. {cihaz veya modül kimliği} .lastConnectTime | Cihaz veya bağlı modülü son zaman. |
| istemciler. {cihaz veya modül kimliği} .lastDisconnectTime | Son zaman, cihaz veya modül bağlantısı kesildi. |

## <a name="next-steps"></a>Sonraki adımlar

Bu özellikler dağıtım bildirimleri oluşturmak için kullanmayı öğrenin için bkz: [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).
