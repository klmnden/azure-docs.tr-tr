---
title: Azure IOT EdgeAgent ve EdgeHub başvurusu | Microsoft Docs
description: Belirli özellikler ve değerlerinin edgeAgent ve edgeHub modülü çiftlerini için gözden geçirin
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0b9e7421bb09e619b4a820910db5faa9edfcc5d5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632916"
---
# <a name="properties-of-the-edge-agent-and-edge-hub-module-twins"></a>Edge aracısı ve kenar hub modülü çiftlerini özellikleri

Edge aracısı ve kenar hub IOT kenar çalışma zamanı yapmak iki modüllerdir. Her modülü gerçekleştirir hangi görevleri hakkında daha fazla bilgi için bkz: [Azure IOT kenar çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md). 

Bu makalede, istenen özellikler ve Çalışma Zamanı Modülü çiftlerini bildirilen özelliklerini sağlar. Bkz: [dağıtım ve izleme] [ lnk-deploy] IOT sınır cihazları modülleri dağıtma hakkında daha fazla bilgi için.

## <a name="edgeagent-desired-properties"></a>İstenen EdgeAgent özellikleri

Edge Aracısı modülü twin adlı `$edgeAgent` ve bir cihaz ve IOT hub'ı çalıştıran kenar Aracısı arasındaki iletişimi düzenler. İstenen özellikleri, bir dağıtım bildirimi tek aygıt veya ölçekli dağıtımının bir parçası belirli bir cihazda uygularken ayarlanır. 

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| schemaVersion | "1.0" olması gerekir | Evet |
| Runtime.Type | "Docker" olması gerekir | Evet |
| runtime.settings.minDockerVersion | Bu dağıtım listesi tarafından gereken en düşük Docker sürüm için ayarlayın | Evet |
| runtime.settings.loggingOptions | Edge Aracısı kapsayıcısı için günlüğe kaydetme seçeneklerini içeren stringified JSON. [Docker günlük seçenekleri][lnk-docker-logging-options] | Hayır |
| systemModules.edgeAgent.type | "Docker" olması gerekir | Evet |
| systemModules.edgeAgent.settings.image | Edge Aracısı görüntüsü URI'si. Şu anda, kenar Aracısı kendisini güncelleştirmek mümkün değil. | Evet |
| systemModules.edgeAgent.settings.createOptions | Edge Aracısı kapsayıcı oluşturma seçeneklerini içeren stringified JSON. [Docker oluşturma seçenekleri][lnk-docker-create-options] | Hayır |
| systemModules.edgeAgent.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bir dağıtım kullanarak bu bildirimi uygulandığında bu IOT Hub tarafından ayarlanır. Parçası olmayan bir dağıtım bildirimi. |
| systemModules.edgeHub.type | "Docker" olması gerekir | Evet |
| systemModules.edgeHub.status | "Çalışıyor olması gerektiğini" | Evet |
| systemModules.edgeHub.restartPolicy | "Her zaman" olması gerekir | Evet |
| systemModules.edgeHub.settings.image | Edge hub görüntüsü URI'si. | Evet |
| systemModules.edgeHub.settings.createOptions | Edge hub kapsayıcı oluşturma seçeneklerini içeren stringified JSON. [Docker oluşturma seçenekleri][lnk-docker-create-options] | Hayır |
| systemModules.edgeHub.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bir dağıtım kullanarak bu bildirimi uygulandığında bu IOT Hub tarafından ayarlanır. Parçası olmayan bir dağıtım bildirimi. |
| modüller. {Moduleıd} .version | Bu modül sürümü temsil eden kullanıcı tanımlı bir dize. | Evet |
| modules.{moduleId}.type | "Docker" olması gerekir | Evet |
| modules.{moduleId}.restartPolicy | {"hiçbir zaman" \| "üzerinde başarısız oldu-" \| "üzerinde-sağlıksız" \| "her zaman"} | Evet |
| modüller. {Moduleıd}.settings.image | Modül görüntü URI'si. | Evet |
| modules.{moduleId}.settings.createOptions | Modül kapsayıcı oluşturma seçeneklerini içeren stringified JSON. [Docker oluşturma seçenekleri][lnk-docker-create-options] | Hayır |
| modüller. {Moduleıd}.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bir dağıtım kullanarak bu bildirimi uygulandığında bu IOT Hub tarafından ayarlanır. Parçası olmayan bir dağıtım bildirimi. |

## <a name="edgeagent-reported-properties"></a>Özellikler EdgeAgent bildirdi

Edge Aracısı özellikleri üç ana parça bilgi dahil bildirdi:

1. Uygulama son görülen istenen özelliklerinin durumunu;
2. Şu anda kenar aracı tarafından raporlanan cihaz üzerinde çalışan modüllerin durumunu; ve
3. Şu anda cihazda çalışan istenen özelliklerinin kopyası.

Bu son parça bilgi son istenen özellikler çalışma zamanı tarafından başarıyla uygulanmaz ve aygıt hala bir önceki dağıtım bildirimi çalışıyor durumda yararlıdır.

> [!NOTE]
> İle sorgulanabilir gibi kenar aracının bildirilen özelliklerini yararlı [IOT hub'ı sorgu dili] [ lnk-iothub-query] ölçekte dağıtımların durumunu incelemek için. Başvurmak [dağıtımları] [ lnk-deploy] bu özelliğinin nasıl kullanılacağı hakkında daha fazla bilgi için.

Aşağıdaki tabloda, istenen özelliklerinden kopyalandığında bilgileri içermez.

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu tamsayı son kenar aracı tarafından işlenen istenen özellik sürümünü gösterir. |
| lastDesiredStatus.code | Bu sınır aracı tarafından görülen son istenen özelliklerine başvuran durum kodudur. İzin verilen değerler: `200` başarılı, `400` geçersiz yapılandırma `412` geçersiz şema sürümüne `417` İstenen özelliklerde boş `500` başarısız oldu |
| lastDesiredStatus.description | Durum metin açıklaması |
| DeviceHealth | `healthy` tüm modülleri çalışma zamanı durumunu ya da ise `running` veya `stopped`, `unhealthy` Aksi takdirde |
| configurationHealth.{deploymentId}.health | `healthy` {Deploymentıd} dağıtımı tarafından ayarlanmış olan tüm modülleri çalışma zamanı durumunu ya da ise `running` veya `stopped`, `unhealthy` Aksi takdirde |
| runtime.platform.OS | Aygıtta çalışan işletim sistemi raporlama |
| Runtime.Platform.Architecture | Cihazda raporlama CPU mimarisi |
| systemModules.edgeAgent.runtimeStatus | Edge Aracısı'nın bildirilen durum: {"çalışır" \| "sağlıksız"} |
| systemModules.edgeAgent.statusDescription | Edge aracının bildirilen durumunu metin açıklaması. |
| systemModules.edgeHub.runtimeStatus | Edge hub'ın geçerli durum: {"çalışır" \| "durduruldu" \| "başarısız" \| "geri Çekilme" \| "sağlıksız"} |
| systemModules.edgeHub.statusDescription | Edge hub sağlıksız varsa geçerli durumunu metin açıklaması. |
| systemModules.edgeHub.exitCode | Çıktı, çıkış kodu kenar hub kapsayıcı tarafından bildirilen varsa |
| systemModules.edgeHub.startTimeUtc | Edge hub son başlatıldığı zaman |
| systemModules.edgeHub.lastExitTimeUtc | Zaman zaman kenar hub'ı son çıkıldı |
| systemModules.edgeHub.lastRestartTimeUtc | Zaman kenar hub en son ne zaman yeniden başlatıldı |
| systemModules.edgeHub.restartCount | Bu modül yeniden başlatma ilkesi bir parçası olarak yeniden sayısı. |
| modüller. {Moduleıd} .runtimeStatus | Modülün geçerli durum: {"çalışır" \| "durduruldu" \| "başarısız" \| "geri Çekilme" \| "sağlıksız"} |
| modules.{moduleId}.statusDescription | Sağlıksız durumunda modülünü geçerli durumunu metin açıklaması. |
| modules.{moduleId}.exitCode | Çıktı, çıkış kodu modülü kapsayıcı tarafından bildirilen varsa |
| modüller. {Moduleıd} .startTimeUtc | Modül son başlatıldığı zaman |
| modules.{moduleId}.lastExitTimeUtc | Zaman zaman modülü son çıkıldı |
| modules.{moduleId}.lastRestartTimeUtc | Zaman modülü en son ne zaman yeniden başlatıldı |
| modules.{moduleId}.restartCount | Bu modül yeniden başlatma ilkesi bir parçası olarak yeniden sayısı. |

## <a name="edgehub-desired-properties"></a>İstenen EdgeHub özellikleri

Edge hub'ına yönelik modülü twin adlı `$edgeHub` ve bir cihaz ve IOT hub'ı çalıştıran kenar hub arasındaki iletişimi düzenler. İstenen özellikleri, bir dağıtım bildirimi tek aygıt veya ölçekli dağıtımının bir parçası belirli bir cihazda uygularken ayarlanır. 

| Özellik | Açıklama | Dağıtım bildiriminde gerekli |
| -------- | ----------- | -------- |
| schemaVersion | "1.0" olması gerekir | Evet |
| yollar. {Routetablename} | Bir sınır hub yolunu temsil eden dize. | `routes` Öğesi var ancak boş olabilir. |
| storeAndForwardConfiguration.timeToLiveSecs | Edge hub IOT hub'ı veya yerel modül bağlantısı kesilmiş iletileri bağlantısı kesilmiş yönlendirme uç noktaları, örneğin, durumunda tutar saniye cinsinden zaman | Evet |

## <a name="edgehub-reported-properties"></a>Özellikler EdgeHub bildirdi

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu tamsayı son kenar hub tarafından işlenen istenen özellik sürümünü gösterir. |
| lastDesiredStatus.code | Bu sınır hub tarafından görülen son istenen özelliklerine başvuran durum kodudur. İzin verilen değerler: `200` başarılı, `400` geçersiz yapılandırma `500` başarısız oldu |
| lastDesiredStatus.description | Durum metin açıklaması |
| istemciler. {aygıt veya modül kimliği} .status | Bu cihaz ya da modül bağlantı durumu. Olası değerler {"bağlı" \| "bağlantısı kesildi"}. Yalnızca modülü kimlikleri bağlantısı kesik bir durumda olabilir. Edge hub'a bağlanan Aşağı Akış aygıtları yalnızca bağlıyken görünür. |
| istemciler. {aygıt veya modül kimliği} .lastConnectTime | Aygıt ya da bağlı modülü son saat |
| istemciler. {aygıt veya modül kimliği} .lastDisconnectTime | Cihaz veya modülü son zamanı bağlantısı kesildi |

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım bildirimleri oluşturmak için bu özellikleri kullanmayı öğrenmek için bkz: [nasıl IOT kenar modülleri, yapılandırılmış, yeniden ve kullanılabilecek anlamak](module-composition.md).

<!--links -->
[lnk-deploy]: module-deployment-monitoring.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
