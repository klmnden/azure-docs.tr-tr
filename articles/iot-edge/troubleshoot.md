---
title: "Azure IOT kenar sorunlarını giderme | Microsoft Docs"
description: "Sık karşılaşılan sorunları gidermeye ve sorun giderme becerilerinizi Azure IOT köşesi öğrenin"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3f61f0bf8234e747ae38146d1a5ea030e3163fa3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Ortak sorunlar ve çözümleri için Azure IOT kenar

Azure IOT kenar ortamınızda çalışan sorunlarla karşılaşırsanız, sorun giderme ve çözümleme için bu makaleyi kılavuz olarak kullanın. 

## <a name="standard-diagnostic-steps"></a>Standart tanılama adımları 

Bir sorunla karşılaştığınızda, kapsayıcı günlükleri ve ve aygıttan geçirmek iletilerini gözden geçirerek IOT kenar Cihazınızı durumu hakkında daha fazla bilgi. Araçlar ve komutlar bilgileri toplamak için bu bölümdeki kullanın. 

* Sorunları algılamak için docker kapsayıcıların günlüklerine bakın. Dağıtılan kapsayıcılarını ile başlayın, ardından IOT kenar çalışma zamanı yapmak kapsayıcıları bakın: kenar aracısı ve kenar Hub. Edge Aracısı günlükleri, genellikle her kapsayıcı yaşam döngüsü hakkında bilgi sağlar. Edge Hub günlükleri, Mesajlaşma ve yönlendirme bilgileri sağlar. 

   ```cmd
   docker logs <container name>
   ```

* Edge hub'ı aracılığıyla giden iletiler görüntüleyin ve Öngörüler cihaz özellikleri güncelleştirmeleri ayrıntılı günlükleriyle çalışma zamanı kapsayıcılardan toplama.

   ```cmd
   iotedgectl setup --runtime-log-level DEBUG
   ```

* Bağlantı sorunları yaşıyorsa, cihaz bağlantı dizenizi gibi kenar aygıt ortam değişkenlerini inceleyin:

   ```cmd
   docker exec edgeAgent printenv
   ```

Ayrıca, IOT hub'ı ve IOT sınır cihazları arasında gönderilen iletiler kontrol edebilirsiniz. Kullanarak bu iletilerin görüntüleme [Azure IOT Araç Seti](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) Visual Studio Code uzantısı. Daha fazla yardım için bkz: [ile Azure IOT geliştirirken kullanışlı aracı](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

Bilgi iletilerini ve günlükleri araştırdıktan sonra Azure IOT kenar çalışma zamanı yeniden başlatmayı deneyebilirsiniz:

   ```cmd
   iotedgectl restart
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>Yaklaşık bir dakika sonra kenar Aracısı durdurur

Edge Aracısı'nı başlatır ve yaklaşık bir dakika başarıyla çalışırsa, ardından durdurur. Günlükleri kenar Aracısı AMQP IOT Hub'ına bağlanmaya ve yaklaşık olarak 30 saniye daha sonra AMQP kullanarak websocket üzerinden bağlanma girişiminde gösterir. Başarısız olduğunda, kenar Aracısı çıkar. 

Örnek kenar Aracısı kaydeder:

```
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Kök nedeni
Ağ yapılandırmasının konak ağ, ağ ulaşmasını kenar Aracısı engelliyor. Aracı ilk AMQP (bağlantı noktası 5671) bağlanma girişiminde bulunur. Bu başarısız olursa, websockets (bağlantı noktası 443) çalışır.

İletişim için her modül için bir ağ IOT kenar çalışma zamanı ayarlar. Linux üzerinde bu ağ köprüsü ağdır. İsteğe bağlı olarak, Windows NAT kullanır Bu sorunu NAT ağı kullanmak Windows kapsayıcıları kullanma Windows cihazlarda daha yaygın bir durumdur. 

### <a name="resolution"></a>Çözüm
İnternet bu köprüsü/NAT ağa atanan IP adresleri için bir yol olduğundan emin olun. Bazen ana bilgisayarda VPN yapılandırması IOT kenar ağ geçersiz kılar. 

## <a name="edge-hub-fails-to-start"></a>Edge Hub başlatılamıyor

Başlatmak için sınır Hub başarısız oluyor ve aşağıdaki günlüklere ileti baskı siparişi: 

```
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Kök nedeni
Konak makinedeki başka bir işlem, bağlantı noktası 443 bağlı. Edge hub'ı bağlantı noktası 5671 eşler ve ağ geçidi senaryolarda için 443'ü kullanın. Başka bir işlem bu bağlantı noktası zaten bağlanmış Bu bağlantı noktası eşlemesi başarısız olur. 

### <a name="resolution"></a>Çözüm
Bul ve 443 numaralı bağlantı noktası kullanma işlemini durdurun. Bu işlem genellikle bir web sunucusudur.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>Edge Aracısı bir modülün görüntü (403) erişemiyor
Bir kapsayıcı çalışmaz ve kenar aracı günlüklerini 403 hatası gösterir. 

### <a name="root-cause"></a>Kök nedeni
Edge Aracısı'nı bir modülün görüntü erişim iznine sahip değil. 

### <a name="resolution"></a>Çözüm
Try çalışan `iotedgectl login` yeniden komutu.

## <a name="next-steps"></a>Sonraki adımlar
IOT kenar platform hata bulundu düşünüyorsunuz? Lütfen [bir sorun gönderme](https://github.com/Azure/iot-edge/issues) böylece biz geliştirmeye devam. 
