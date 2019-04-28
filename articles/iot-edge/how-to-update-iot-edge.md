---
title: Güncelleştirme IOT Edge cihazları - Azure IOT Edge sürümünde | Microsoft Docs
description: IOT Edge cihazları, güvenlik daemon'ı ve IOT Edge çalışma zamanı en son sürümünü çalıştırma güncelleştirme
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a3dd7f78362b5f5c99dc4a74fe0a32c4d26be5b7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125942"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme

IOT Edge hizmetinin sürümleri yeni sürümleri gibi güvenlik geliştirmeleri ve en son özellikler için IOT Edge cihazlarınıza güncelleştirmek isteyebilirsiniz. Bu makalede yeni bir sürümü kullanılabilir olduğunda, IOT Edge cihazlarınıza güncelleştirme hakkında bilgi sağlar. 

IOT Edge cihazı iki bileşenden yeni bir sürüme taşımak istiyorsanız güncelleştirilmesi gerekir. Cihaz üzerinde çalışan ve cihaz çalışma zamanı modülleri başlatıldığında güvenlik arka plan programı, davranıştır. Şu anda güvenlik arka plan programı CİHAZDAN yalnızca güncelleştirilebilir. IOT Edge hub'ı ve IOT Edge Aracısı modülleri çalışma zamanı, ikinci bileşendir. Dağıtımınızı nasıl yapısına bağlı olarak, çalışma zamanı CİHAZDAN veya uzaktan güncelleştirilebilir. 

Azure IOT Edge en son sürümünü bulmak için bkz: [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

>[!IMPORTANT]
>Bir Windows cihazda Azure IOT Edge çalıştırıyorsanız, aşağıdakilerden birini cihazınıza geçerliyse 1.0.5 sürümüne güncelleştirme: 
>* Windows için derleme 17763 Cihazınızı yükseltilmemiş. IOT Edge sürüm 1.0.5 Windows desteklemiyor 17763 şundan oluşturur.
>* Windows Cihazınızda Java veya Node.js modüllerini çalıştırın. Windows cihazınız için en son sürüme güncelleştirdikten bile sürüm 1.0.5 atlayın. 
>
>IOT Edge 1.0.5 sürümü hakkında daha fazla bilgi için bkz. [1.0.5 sürüm notları](https://github.com/Azure/azure-iotedge/releases/tag/1.0.5). Geliştirme araçlarınızı en son sürüme güncelleştirilmesini engelleme hakkında daha fazla bilgi için bkz. [IOT Geliştirici blogu](https://devblogs.microsoft.com/iotdev/).


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

Windows cihazlarda güvenlik arka plan programı kaldırıp için PowerShell betiğini kullanın. Yükleme betiği otomatik olarak en son sürümünü güvenlik arka plan programı çeker. 

Bir yönetici PowerShell oturumunda güvenlik arka plan programı kaldırın. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-SecurityDaemon
```

Çalışan `Uninstall-SecurityDaemon` komut hiçbir parametre olmadan yalnızca kaldırır güvenlik arka plan programı cihazınızdan iki çalışma zamanı kapsayıcı görüntülerinin yanı sıra. Config.yaml dosya Moby container altyapısı verileri yanı sıra cihaz tutulur. Bağlantı dizesi veya yeniden yükleme işlemi sırasında cihazınız için cihaz sağlama hizmeti bilgilerini sağlamaları gerekmez yapılandırma bilgileri anlamına gelir kalmasını sağlar. 

IOT Edge Cihazınızı Windows kapsayıcıları ya da Linux kapsayıcıları kullanıp bağlı olarak güvenlik daemon'ı yeniden yükleyin. Tümcecik değiştirin **\<Windows veya Linux\>** uygun bir kapsayıcı işletim sistemlerine sahip. Kullanım **- ExistingConfig** Cihazınızda mevcut config.yaml dosyasına işaret etmek için bayrak. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -ExistingConfig -ContainerOS <Windows or Linux>
```

Güvenlik arka plan programı belirli bir sürümünü yüklemek istiyorsanız, uygun iotedged windows.zip dosyası indirin [IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases). Ardından, `-OfflineInstallationPath` dosya konumuna işaret edecek şekilde parametresi. Daha fazla bilgi için [çevrimdışı yükleme](how-to-install-iot-edge-windows.md#offline-installation).

## <a name="update-the-runtime-containers"></a>Çalışma zamanı kapsayıcılarındaki güncelleştir

IOT Edge aracısı ve IOT Edge hub'ı kapsayıcıları güncelleştirme yolu, dağıtımınızdaki (1.0 gibi) sıralı etiketleri veya (1.0.2 gibi) belirli etiketlere kullanıp bağlıdır. 

Şu anda komutları kullanarak Cihazınızı IOT Edge hub'ı modülleri ve IOT Edge Aracısı sürümünü denetleme `iotedge logs edgeAgent` veya `iotedge logs edgeHub`. 

  ![Günlüklerde kapsayıcı sürümü Bul](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>IOT Edge etiketleri anlama

IOT Edge hub'ı görüntüler ve IOT Edge Aracısı ile ilişkili IOT Edge sürümü ile etiketlenir. Çalışma zamanı görüntülerle etiketleri kullanmak için iki farklı yolu vardır: 

* **Etiketler alınıyor** -yalnızca ilk iki değeri sürüm numarasının basamaklar eşleşen en son görüntü almak için kullanın. Örneğin, en son 1.0.x sürüme işaret edecek şekilde yeni bir sürüm olduğunda 1.0 güncelleştirilir. IOT Edge Cihazınızda kapsayıcı çalışma zamanı görüntü yeniden alınır, çalışma zamanı modülleri en son sürüme güncelleştirildi. Bu yaklaşım, geliştirme amacıyla önerilir. Etiketler alınıyor dağıtımların Azure portal varsayılan. 
* **Belirli etiketlere** -görüntü sürümü açıkça ayarlamak için sürüm numarası, tüm üç değerleri kullanın. Örneğin, 1.0.2, ilk sürümünden sonra değişmez. Güncelleştirmeye hazır olduğunuzda dağıtım bildirimi içinde yeni bir sürüm numarası bildirebilirsiniz. Bu yaklaşım, üretim için önerilir.

### <a name="update-a-rolling-tag-image"></a>Sıralı bir etiket görüntüsünü güncelleştir

Dağıtımınızda çalışırken etiketleri kullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0**) kapsayıcı çalışma zamanı görüntüsünün son sürümünü çekmek için Cihazınızda zorlama ayarını yapmanız gerekir. 

IOT Edge cihazınızın yerel görüntü sürümü silin. Tekrar bu adımı gerçekleştirmeniz gerekmez Windows makinelerde güvenlik arka plan kaldırma da çalışma zamanı görüntüleri kaldırır. 

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

Zorla kullanmanız gerekebilir `-f` görüntülerini kaldırmak için bayrak. 

IOT Edge hizmetine en son sürümlerini çalışma zamanı görüntüleri çekin ve otomatik olarak bunları Cihazınızda yeniden başlatın. 

### <a name="update-a-specific-tag-image"></a>Belirli bir etiketi görüntüsünü güncelleştir

Dağıtımınızdaki belirli etiketlere kullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0.2**) yapmak için ihtiyacınız olan sonra Dağıtım bildiriminizi etiketinde güncelleştirin ve değişiklikleri cihazınıza uygulanabilmesi. 

Azure portalında çalışma zamanı dağıtımı görüntüleri içinde bildirilen **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** bölümü. 

![Gelişmiş edge çalışma zamanı ayarlarını yapılandırma](./media/how-to-update-iot-edge/configure-runtime.png)

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