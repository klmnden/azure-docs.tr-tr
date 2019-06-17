---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 11/09/2018
ms.author: dobett
ms.openlocfilehash: c95bca125ea70cf32acad0d5ea67c3ad195ed704
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66146568"
---
## <a name="automatic-device-management"></a>Otomatik cihaz Yönetimi
Azure IOT hub otomatik cihaz Yönetimi döngülerini tamamına büyük cihaz filolarına yönetme yinelenen ve karmaşık görevlerinin birçoğunu otomatik hale getirir. İle otomatik cihaz yönetimi, bir dizi cihazda özelliklerine göre hedef, istenen yapılandırmasını tanımlamak ve IOT Hub'ın kapsama geldikleri her cihazları güncelleştirmek olanak tanır.  Oluşan [otomatik cihaz yapılandırmaları](../articles/iot-hub/iot-hub-auto-device-config.md) ve [IOT Edge otomatik dağıtımlar](../articles/iot-edge/how-to-deploy-monitor.md).

## <a name="iot-edge"></a>IoT Edge
Azure IOT Edge, Azure hizmetlerinin ve çözüme özel kodun şirket içi cihazlara bulut odaklı dağıtımını sağlar. Verileri buluta gönderilmeden önce IOT Edge cihazları diğer bilgi işlem gerçekleştirmek için cihazlar ve analiz verileri toplayabilirsiniz. Daha fazla bilgi için [Azure IOT Edge](https://docs.microsoft.com/azure/iot-edge/).

## <a name="iot-edge-agent"></a>IOT Edge Aracısı
IOT Edge çalışma zamanı dağıtma ve izleme modüllerini sorumlu bir parçası.

## <a name="iot-edge-device"></a>IoT Edge cihazı
IOT Edge cihazlarının IOT Edge çalışma zamanı yüklü olması ve olarak işaretlenmiş **IOT Edge cihazı** cihaz ayrıntıları. Bilgi edinmek için nasıl [üzerinde bir Linux sanal cihaza Azure IOT Edge dağıtma - Önizleme](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux).

## <a name="iot-edge-automatic-deployment"></a>IOT Edge otomatik dağıtım
IOT Edge otomatik dağıtım, IOT Edge cihazları, bir dizi IOT Edge modüllerini çalıştırmak için hedef kümesini yapılandırır. Her dağıtım, kendi hedef koşulu ile eşleşen tüm cihazlar Belirtilen modül kümesini çalıştırıyorsanız, hedef koşulu için değiştirilmiş veya oluşturulan bile yeni cihazlar sürekli olarak sağlar. Her IOT Edge cihazı yalnızca hedef koşulu da karşılayan en yüksek öncelikli dağıtımı alır. Daha fazla bilgi edinin [IOT Edge otomatik dağıtım](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).

## <a name="iot-edge-deployment-manifest"></a>IOT Edge dağıtım bildirimi
Bir modül kümesini dağıtmak için bir veya daha fazla IOT Edge cihazları modülü twin(s) kopyalanacak bilgileri içeren bir Json belgesi, yollar ve ilişkili modül istenen özellikleri.

## <a name="iot-edge-gateway-device"></a>IOT Edge ağ geçidi cihazı
Aşağı Akış cihaz ile IOT Edge cihazı. Aşağı Akış cihazın IOT Edge veya IOT Edge cihazı olabilir.

## <a name="iot-edge-hub"></a>IOT Edge hub'ı
IOT Edge Çalışma Zamanı Modülü modülü iletişim (doğru IOT Hub) yukarı ve aşağı akış (uzağa, IOT Hub) için sorumlu parçası iletişim. 

## <a name="iot-edge-leaf-device"></a>IOT Edge yaprak cihaz
Aşağı Akış cihaz ile IOT Edge cihazı. 

## <a name="iot-edge-module"></a>IOT Edge Modülü
IOT Edge modülü IOT Edge cihazlarına dağıtabileceğiniz bir Docker kapsayıcıdır. Bu, bir CİHAZDAN bir ileti almak, bir ileti dönüştürme ya da bir IOT hub'ına ileti gönderme gibi belirli bir görevi gerçekleştirir. Bu, diğer modüllerle iletişim kurar ve IOT Edge çalışma zamanına veri gönderir. [IOT Edge modülleri geliştirmek için Araçlar ve gereksinimleri anlamak](https://docs.microsoft.com/azure/iot-edge/module-development).

## <a name="iot-edge-module-identity"></a>IOT Edge modülü kimlik
Bir edge hub'ı veya IOT hub'ı ile kimlik doğrulaması için bir modül tarafından kullanılacak varlığı ve güvenlik kimlik bilgileri gerçekleşen IOT hub'ı modül kimliği kayıt defterinde kayıt.

## <a name="iot-edge-module-image"></a>IOT Edge modülü görüntüsü
IOT Edge çalışma zamanı tarafından modülü örnekleri oluşturmak için kullanılan bir docker görüntüsü.

## <a name="iot-edge-module-twin"></a>IOT Edge modül ikizi
Bir modül örneğinin için durum bilgilerini depolayan IOT hub'ında bir Json belgesi kalıcı.

## <a name="iot-edge-priority"></a>IOT Edge önceliği
IOT Edge iki dağıtım aynı cihazı hedeflediğinde, daha yüksek önceliğe sahip dağıtım uygulanır. İki dağıtım aynı önceliğe sahipse, oluşturma tarihi daha sonra olan dağıtım uygulanır. Daha fazla bilgi edinin [öncelik](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#priority).

## <a name="iot-edge-runtime"></a>IoT Edge çalışma zamanı
IOT Edge çalışma zamanı bir IOT Edge cihazında yüklü olmasını Microsoft dağıtan her şeyi içerir. Bu, Edge aracısı ve Edge hub'ı IOT Edge güvenlik arka plan programı içerir.

## <a name="iot-edge-set-modules-to-a-single-device"></a>IOT Edge modülleri için tek bir cihaz ayarlama
Bir cihaz üzerinde bir IOT Edge bildiriminin içeriği kopyalayan bir işlem ' modül ikizi. Genel bir temel alınan API'dir 'yapılandırma apply' yalnızca aldığı girdi olarak bir IOT Edge bildirimi.

## <a name="iot-edge-target-condition"></a>IOT Edge hedef koşulu
Örneğin dağıtımın hedef cihazlarını seçmek için cihaz ikizlerini etiketleri hakkında herhangi bir Boolean koşul bir IOT Edge dağıtımı, hedef durumdur **tag.environment prod =** . Hedef koşul gereksinimlerini karşılayan yeni cihazları eklemek veya artık yapmak cihazları kaldırmak için sürekli olarak değerlendirilir. Daha fazla bilgi edinin [hedef koşulu](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#target-condition)