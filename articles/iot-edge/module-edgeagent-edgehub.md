---
title: EdgeAgent ve EdgeHub istenen özellikler başvuru - Azure IOT Edge | Microsoft Docs
description: Belirli özellikleri ve değerlerini edgeAgent ve edgeHub modül ikizlerini için gözden geçirin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a0834e5886a1a088486109f967baf357e375ad05
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100292"
---
# <a name="properties-of-the-edge-agent-and-edge-hub-module-twins"></a>Edge aracısı ve Edge hub'ı modül ikizlerini özellikleri

Edge aracısı ve Edge hub'ı, IOT Edge çalışma zamanını oluşturan iki modüllerdir. Her modülün gerçekleştirdiği hangi görevleri hakkında daha fazla bilgi için bkz. [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md). 

Bu makalede, istenen özellikleri ve çalışma zamanı modül ikizlerini bildirilen özellikleri sağlar. IOT Edge cihazlarında modülleri dağıtma hakkında daha fazla bilgi için bkz. [dağıtım ve izleme](module-deployment-monitoring.md).

## <a name="edgeagent-desired-properties"></a>İstenen EdgeAgent özellikleri

Edge Aracısı modül ikizi adlı `$edgeAgent` ve bir cihaz ve IOT hub'ı çalıştıran Edge Aracısı arasındaki iletişimi düzenler. İstenen özellikleri, bir dağıtım bildirimi tek cihaz veya ölçekli bir dağıtımının parçası olarak belirli bir cihazda uygulama işlemi sırasında ayarlanır. 

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| schemaVersion | "1.0" olması gerekir | Evet |
| Runtime.Type | "Docker" olması gerekir | Evet |
| runtime.settings.minDockerVersion | Bu dağıtım bildirimi tarafından gereken en düşük Docker sürümü için ayarlayın | Evet |
| runtime.settings.loggingOptions | Edge Aracısı kapsayıcı günlüğe kaydetme seçeneklerini içeren dizeleştirilmiş JSON. [Docker günlük seçenekleri](https://docs.docker.com/engine/admin/logging/overview/) | Hayır |
| runtime.settings.registryCredentials<br>. {registryId} .username | Kapsayıcı kayıt defteri kullanıcı adı. Azure Container Registry için kullanıcı kayıt defteri adı genellikle adıdır.<br><br> Kayıt defteri kimlik bilgilerini ortak olmayan modül görüntüleri için gerekli değildir. | Hayır |
| runtime.settings.registryCredentials<br>. {registryId} .password | Kapsayıcı kayıt defteri parolası. | Hayır |
| runtime.settings.registryCredentials<br>. {registryId} .address | Kapsayıcı kayıt defteri adresi. Azure Container Registry için genellikle adresidir *{registryname}.azurecr.io/Azure-vote-Front:v1*. | Hayır |  
| systemModules.edgeAgent.type | "Docker" olması gerekir | Evet |
| systemModules.edgeAgent.settings.image | Edge Aracısı görüntüsünü URI'si. Şu anda, Edge Aracısı'nı güncelleştirme kendisini mümkün değildir. | Evet |
| systemModules.edgeAgent.settings<br>.createOptions | Edge Aracısı container oluşturulması için seçenekleri içeren bir dizeleştirilmiş JSON. [Docker oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Hayır |
| systemModules.edgeAgent.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bu bildirimi bir dağıtım kullanarak uygulandığında, bu özellik, IOT Hub tarafından ayarlanır. Parçası olmayan bir dağıtım bildirimi. |
| systemModules.edgeHub.type | "Docker" olması gerekir | Evet |
| systemModules.edgeHub.status | "Çalışıyor" gerekir | Evet |
| systemModules.edgeHub.restartPolicy | "Her zaman" olması gerekir | Evet |
| systemModules.edgeHub.settings.image | Edge hub'ı görüntüsünü URI'si. | Evet |
| systemModules.edgeHub.settings<br>.createOptions | Edge hub'ı container oluşturulması için seçenekleri içeren bir dizeleştirilmiş JSON. [Docker oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Hayır |
| systemModules.edgeHub.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bu bildirimi bir dağıtım kullanarak uygulandığında, bu özellik, IOT Hub tarafından ayarlanır. Parçası olmayan bir dağıtım bildirimi. |
| modüller. {Moduleıd} .version | Bu modülün sürümünü temsil eden kullanıcı tanımlı bir dize. | Evet |
| modules.{moduleId}.type | "Docker" olması gerekir | Evet |
| modüller. {Moduleıd} .status | {"çalışıyor" \| "durduruldu"} | Evet |
| modules.{moduleId}.restartPolicy | {"hiçbir zaman" \| "üzerinde başarısız oldu-" \| "üzerinde-sağlıksız" \| "her zaman"} | Evet |
| modüller. {Moduleıd}.settings.image | Modülü görüntüsü URI. | Evet |
| modules.{moduleId}.settings.createOptions | Modül container oluşturulması için seçenekleri içeren bir dizeleştirilmiş JSON. [Docker oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Hayır |
| modüller. {Moduleıd}.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bu bildirimi bir dağıtım kullanarak uygulandığında, bu özellik, IOT Hub tarafından ayarlanır. Parçası olmayan bir dağıtım bildirimi. |

## <a name="edgeagent-reported-properties"></a>EdgeAgent bildirilen özellikler

Edge Aracısı özellikleri ana üç parça bilgi içeren rapor:

1. Son görülme istenen özellikleri uygulama durumu;
2. Edge aracısı tarafından raporlanan cihaz üzerinde şu anda çalışan modüllerin durumunu; ve
3. İstenen özellikleri kullanarak cihaz üzerinde şu anda çalışan bir kopyası.

Bu son bilgilerden biri, en son istenen özellikleri çalışma zamanı tarafından başarıyla uygulanmaz ve cihaz önceki bir dağıtım bildirimi hala çalışıyor durumunda yararlıdır.

> [!NOTE]
> Edge Aracısı'nın bildirilen özellikleri ile sorgulanabilir gibi yararlı [IOT Hub sorgu dili](../iot-hub/iot-hub-devguide-query-language.md) uygun ölçekte dağıtımların durumunu araştırmak için. Edge Aracısı özellikleri durumu kullanma hakkında daha fazla bilgi için bkz. [IOT Edge dağıtımlarını anlama tek tek cihazlarda veya uygun ölçekte](module-deployment-monitoring.md).

Aşağıdaki tabloda, istenen özelliklerden kopyalanır bilgileri içermez.

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu tamsayı en son sürümünü Edge aracısı tarafından işletildiğinde istenen özellikleri ifade eder. |
| lastDesiredStatus.code | Edge aracısı tarafından görülen son istenen özellikleri başvuran durum kodudur. İzin verilen değerler: `200` başarı `400` geçersiz yapılandırma `412` geçersiz şema sürümü `417` istenen özellikleri boş `500` başarısız oldu |
| lastDesiredStatus.description | Durum açıklaması metni |
| deviceHealth | `healthy` tüm modüllerin çalışma zamanı durum ya da ise `running` veya `stopped`, `unhealthy` Aksi takdirde |
| configurationHealth.{deploymentId}.health | `healthy` çalışma zamanı durumu {Deploymentıd} dağıtımı tarafından tüm modüllerin geçerli olduğunda `running` veya `stopped`, `unhealthy` Aksi takdirde |
| runtime.platform.OS | Cihaz üzerinde çalışan işletim Sisteminin raporlama |
| Runtime.Platform.Architecture | Cihazda raporlama CPU mimarisi |
| systemModules.edgeAgent.runtimeStatus | Edge Aracısı durumu: {"çalışıyor" \| "kötü"} |
| systemModules.edgeAgent.statusDescription | Edge Aracısı bildirilen durumunu metin açıklaması. |
| systemModules.edgeHub.runtimeStatus | Edge hub'ı geçerli durumu: {"çalışıyor" \| "durduruldu" \| "başarısız" \| "geri alma" \| "kötü"} |
| systemModules.edgeHub.statusDescription | Edge hub'ı sağlıksız, geçerli durumunu metin açıklaması. |
| systemModules.edgeHub.exitCode | Çıktı, çıkış kodu Edge hub'ı kapsayıcı tarafından bildirilen varsa |
| systemModules.edgeHub.startTimeUtc | Edge hub'ı en son başlatıldığı saat |
| systemModules.edgeHub.lastExitTimeUtc | Edge hub'ı en son ne zaman çıkıldı zaman |
| systemModules.edgeHub.lastRestartTimeUtc | Edge hub'ı en son ne zaman başlatıldığı saat |
| systemModules.edgeHub.restartCount | Kaç kez yeniden başlatma ilkesi bir parçası olarak bu modülü yeniden başlatıldı. |
| modüller. {Moduleıd} .runtimeStatus | Modül geçerli durumu: {"çalışıyor" \| "durduruldu" \| "başarısız" \| "geri alma" \| "kötü"} |
| modules.{moduleId}.statusDescription | Geçerli durumu iyi olmayan sistem durumunda modülün metin açıklaması. |
| modules.{moduleId}.exitCode | Çıktı, çıkış kodu modülü kapsayıcı tarafından bildirilen varsa |
| modüller. {Moduleıd} .startTimeUtc | Modül son başlatıldığı saat |
| modules.{moduleId}.lastExitTimeUtc | Zaman zaman modülü son çıkıldı |
| modules.{moduleId}.lastRestartTimeUtc | Zaman modülün en son ne zaman yeniden başlatıldı |
| modules.{moduleId}.restartCount | Kaç kez yeniden başlatma ilkesi bir parçası olarak bu modülü yeniden başlatıldı. |

## <a name="edgehub-desired-properties"></a>İstenen EdgeHub özellikleri

Edge hub'ı modül ikizi adlı `$edgeHub` ve Edge hub'ı bir cihaz ve IOT hub'ı çalıştıran arasındaki iletişimi düzenler. İstenen özellikleri, bir dağıtım bildirimi tek cihaz veya ölçekli bir dağıtımının parçası olarak belirli bir cihazda uygulama işlemi sırasında ayarlanır. 

| Özellik | Açıklama | Dağıtım bildiriminde gerekli |
| -------- | ----------- | -------- |
| schemaVersion | "1.0" olması gerekir | Evet |
| yollar. {Routetablename} | Bir Edge hub'ı yolu temsil eden bir dize. | `routes` Öğesi var ancak boş olabilir. |
| storeAndForwardConfiguration.timeToLiveSecs | Edge hub'a iletileri IOT hub'ı veya yerel modül bağlantısı kesildi durumunda bağlantısı kesilmiş yönlendirme uç noktaları, örneğin, tutar saniye cinsinden süre | Evet |

## <a name="edgehub-reported-properties"></a>EdgeHub bildirilen özellikler

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu tamsayı en son sürümünü Edge hub'ı tarafından işlenen istenen özellikleri ifade eder. |
| lastDesiredStatus.code | Edge hub'ı tarafından görülen son istenen özellikleri başvuran durum kodudur. İzin verilen değerler: `200` başarı `400` geçersiz yapılandırma `500` başarısız oldu |
| lastDesiredStatus.description | Durum açıklaması metni |
| istemciler. {cihaz veya modül kimliği} .status | Bu cihaz veya modül bağlantı durumu. Olası değerler {"bağlı" \| "bağlantısı"}. Yalnızca modül kimlikleri bağlantısı kesilmiş olabilir. Edge hub'ına bağlanmak için aşağı akış cihazlarının yalnızca bağlı olduğunda görünür. |
| istemciler. {cihaz veya modül kimliği} .lastConnectTime | Cihaz veya bağlı modülü son saat |
| istemciler. {cihaz veya modül kimliği} .lastDisconnectTime | Cihaz veya modül son kez bağlantısı kesildi |

## <a name="next-steps"></a>Sonraki adımlar

Bu özellikler dağıtım bildirimleri oluşturmak için kullanmayı öğrenin için bkz: [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).
