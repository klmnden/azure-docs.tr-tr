---
title: Güncelleştirme IOT Edge cihazları - Azure IOT Edge sürümünde | Microsoft Docs
description: IOT Edge cihazları, güvenlik daemon'ı ve IOT Edge çalışma zamanı en son sürümünü çalıştırma güncelleştirme
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 8b8638d8fa428b1b867e3f126ac8b5cc992cc273
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53095163"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme

IOT Edge hizmetinin sürümleri yeni sürümleri gibi güvenlik geliştirmeleri ve en son özellikleri sağlamak için IOT Edge cihazlarınıza güncelleştirmek isteyebilirsiniz. Bu makalede yeni bir sürümü kullanılabilir olduğunda, IOT Edge cihazlarınıza güncelleştirme hakkında bilgi sağlar. 

IOT Edge cihazı iki bileşenden yeni bir sürüme taşımak istiyorsanız güncelleştirilmesi gerekir. İlk cihaz üzerinde çalışan ve cihaz çalışma zamanı başlatıldığında güvenlik arka plan programı var. Şu anda güvenlik arka plan programı CİHAZDAN yalnızca güncelleştirilebilir. Edge hub'ı ve Edge Aracısı modülleri çalışma zamanı, ikinci bileşendir. Dağıtımınızı nasıl yapısına bağlı olarak, çalışma zamanı CİHAZDAN veya uzaktan güncelleştirilebilir. 

Azure IOT Edge en son sürümünü bulmak için bkz: [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

## <a name="update-the-security-daemon"></a>Güvenlik daemon'ı güncelleştirme

IOT Edge güvenlik arka plan programı, IOT Edge cihazında paket Yöneticisini kullanarak güncelleştirilmesi gereken yerel bir bileşenidir. 

Komutunu kullanarak, Cihazınızda çalışan arka plan programı güvenlik sürümünü denetleme `iotedge version`. 

### <a name="linux-devices"></a>Linux cihazları

Linux cihazlarda güvenlik arka plan programı güncelleştirmek için apt-get veya uygun Paket Yöneticisi'ni kullanın. 

```bash
apt-get update
apt-get install libiothsm iotedge
```

### <a name="windows-devices"></a>Windows cihazları

Windows cihazlarda güvenlik arka plan programı kaldırıp için PowerShell betiğini kullanın. Yükleme betiği otomatik olarak en son sürümünü güvenlik arka plan programı çeker. Yeniden yükleme işlemi sırasında cihazınız için bağlantı dizesi sağlamanız gerekir. 

Bir yönetici PowerShell oturumunda güvenlik arka plan programı kaldırın. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
UnInstall-SecurityDaemon
```

IOT Edge Cihazınızı Windows kapsayıcıları ya da Linux kapsayıcıları kullanıp bağlı olarak güvenlik daemon'ı yeniden yükleyin. Tümcecik değiştirin **\<Windows veya Linux\>** kapsayıcı işletim sistemlerinden biri ile. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOS <Windows or Linux>
```

## <a name="update-the-runtime-containers"></a>Çalışma zamanı kapsayıcılarındaki güncelleştir

Edge aracısı ve Edge hub'ı kapsayıcıları güncelleştirme yolu, dağıtımınızdaki (1.0 gibi) sıralı etiketleri veya (1.0.2 gibi) belirli etiketlere kullanıp bağlıdır. 

IOT Edge aracısı ve Edge hub'ı modülleri şu anda komutlarını kullanarak Cihazınızda sürümünü denetleme `iotedge logs edgeAgent` veya `iotedge logs edgeHub`. 

  ![Günlüklerde kapsayıcı sürümü Bul](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>IOT Edge etiketleri anlama

Edge aracısı ve Edge hub'ı görüntüleri, ilişkili oldukları IOT Edge sürümü ile etiketlenir. Çalışma zamanı görüntülerle etiketleri kullanmak için iki farklı yolu vardır: 

* **Etiketler alınıyor** -yalnızca ilk iki değeri sürüm numarasının basamaklar eşleşen en son görüntü almak için kullanın. Örneğin, en son 1.0.x sürüme işaret edecek şekilde yeni bir sürüm olduğunda 1.0 güncelleştirilir. IOT Edge Cihazınızda kapsayıcı çalışma zamanı görüntü yeniden alınır, çalışma zamanı modülleri en son sürüme güncelleştirildi. Bu yaklaşım, geliştirme amacıyla önerilir. Etiketler alınıyor dağıtımların Azure portal varsayılan. 
* **Belirli etiketlere** -görüntü sürümü açıkça ayarlamak için sürüm numarası, tüm üç değerleri kullanın. Örneğin, 1.0.2, ilk sürümünden sonra değişmez. Güncelleştirmeye hazır olduğunuzda dağıtım bildirimi içinde yeni bir sürüm numarası bildirebilirsiniz. Bu yaklaşım, üretim için önerilir.

### <a name="update-a-rolling-tag-image"></a>Sıralı bir etiket görüntüsünü güncelleştir

Dağıtımınızda çalışırken etiketleri kullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0**) kapsayıcı çalışma zamanı görüntüsünün son sürümünü çekmek için Cihazınızda zorlama ayarını yapmanız gerekir. 

IOT Edge cihazınızın yerel görüntü sürümü silin. 

```cmd/sh
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

Zorla kullanmanız gerekebilir `-f` görüntülerini kaldırmak için bayrak. 

IOT Edge hizmetine en son sürümlerini çalışma zamanı görüntüleri çekin ve otomatik olarak bunları Cihazınızda yeniden başlatın. 

### <a name="update-a-specific-tag-image"></a>Belirli bir etiketi görüntüsünü güncelleştir

Dağıtımınızdaki belirli etiketlere kullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0.2**) yapmak için ihtiyacınız olan sonra Dağıtım bildiriminizi etiketinde güncelleştirin ve değişiklikleri cihazınıza uygulanabilmesi. 

Azure portalında çalışma zamanı dağıtımı görüntüleri içinde bildirilen **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** bölümü. 

[Gelişmiş edge çalışma zamanı ayarlarını yapılandırma](./media/how-to-update-iot-edge/configure-runtime.png)

Bir JSON dağıtım bildirimi içinde modül görüntüleri güncelleştirme **systemModules** bölümü. 

```json
"systemModules": {
  "edgeAgent": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-agent:1.0.2",
      "createOptions": ""
    }
  },
  "edgeHub": {
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0.2",
      "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
    }
  }
},
```

## <a name="next-steps"></a>Sonraki adımlar

En son görüntülemek [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

En son güncelleştirmeleri ve Duyuruda ile güncel kalın [nesnelerin interneti blogu](https://azure.microsoft.com/blog/topics/internet-of-things/) 