---
title: Güncelleştirme IOT Edge cihazları - Azure IOT Edge sürümünde | Microsoft Docs
description: IOT Edge cihazları, güvenlik daemon'ı ve IOT Edge çalışma zamanı en son sürümünü çalıştırma güncelleştirme
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/27/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 0c461da44d3d9075d66a68fe8994a4e970288fca
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543758"
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

Linux x64 cihazlarda güvenlik arka plan programı güncelleştirmek için apt-get veya uygun Paket Yöneticisi'ni kullanın. 

```bash
apt-get update
apt-get install libiothsm iotedge
```

Linux ARM32 cihazlarda içindeki adımları kullanın [yükleme Azure IOT Edge çalışma zamanı (ARM32v7/armhf) Linux'ta](how-to-install-iot-edge-linux-arm.md) güvenlik arka plan programı en son sürümünü yüklemek için. 

### <a name="windows-devices"></a>Windows cihazları

Windows cihazlarda güvenlik arka plan programı güncelleştirmek için PowerShell betiğini kullanın. Betik, güvenlik arka plan programı en son sürümünü otomatik olarak çeker. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

Güncelleştirme IoTEdge çalıştırarak güvenlik arka plan programı iki çalışma zamanı kapsayıcı görüntülerinin yanı sıra, bir CİHAZDAN kaldırır. (Windows kapsayıcıları kullanıyorsanız) config.yaml dosya Moby container altyapısı verileri yanı sıra cihaz tutulur. Bağlantı dizesi veya güncelleştirme işlemi sırasında yeniden cihazınız için cihaz sağlama hizmeti bilgilerini sağlamaları gerekmez yapılandırma bilgileri anlamına gelir kalmasını sağlar. 

Güvenlik arka plan programı belirli bir sürümünü yüklemek istiyorsanız, uygun Microsoft Azure IoTEdge.cab dosya indirme [IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases). Ardından, `-OfflineInstallationPath` dosya konumuna işaret edecek şekilde parametresi. Daha fazla bilgi için [çevrimdışı yükleme](how-to-install-iot-edge-windows.md#offline-installation).

## <a name="update-the-runtime-containers"></a>Çalışma zamanı kapsayıcılarındaki güncelleştir

IOT Edge aracısı ve IOT Edge hub'ı kapsayıcıları güncelleştirme yolu, dağıtımınızdaki (1.0 gibi) sıralı etiketleri veya belirli etiketlere (gibi 1.0.7) kullanıp bağlıdır. 

Şu anda komutları kullanarak Cihazınızı IOT Edge hub'ı modülleri ve IOT Edge Aracısı sürümünü denetleme `iotedge logs edgeAgent` veya `iotedge logs edgeHub`. 

  ![Günlüklerde kapsayıcı sürümü Bul](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>IOT Edge etiketleri anlama

IOT Edge hub'ı görüntüler ve IOT Edge Aracısı ile ilişkili IOT Edge sürümü ile etiketlenir. Çalışma zamanı görüntülerle etiketleri kullanmak için iki farklı yolu vardır: 

* **Etiketler alınıyor** -yalnızca ilk iki değeri sürüm numarasının basamaklar eşleşen en son görüntü almak için kullanın. Örneğin, en son 1.0.x sürüme işaret edecek şekilde yeni bir sürüm olduğunda 1.0 güncelleştirilir. IOT Edge Cihazınızda kapsayıcı çalışma zamanı görüntü yeniden alınır, çalışma zamanı modülleri en son sürüme güncelleştirildi. Bu yaklaşım, geliştirme amacıyla önerilir. Etiketler alınıyor dağıtımların Azure portal varsayılan. 
* **Belirli etiketlere** -görüntü sürümü açıkça ayarlamak için sürüm numarası, tüm üç değerleri kullanın. Örneğin, 1.0.7, ilk sürümünden sonra değişmez. Güncelleştirmeye hazır olduğunuzda dağıtım bildirimi içinde yeni bir sürüm numarası bildirebilirsiniz. Bu yaklaşım, üretim için önerilir.

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

Dağıtımınızdaki belirli etiketlere kullanıyorsanız (örneğin, mcr.microsoft.com/azureiotedge-hub:**1.0.7**) yapmak için ihtiyacınız olan sonra Dağıtım bildiriminizi etiketinde güncelleştirin ve değişiklikleri cihazınıza uygulanabilmesi. 

Azure portalında çalışma zamanı dağıtımı görüntüleri içinde bildirilen **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** bölümü. 

![Gelişmiş edge çalışma zamanı ayarlarını yapılandırma](./media/how-to-update-iot-edge/configure-runtime.png)

Bir JSON dağıtım bildirimi içinde modül görüntüleri güncelleştirme **systemModules** bölümü. 

```json
"systemModules": {
  "edgeAgent": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-agent:1.0.7",
      "createOptions": ""
    }
  },
  "edgeHub": {
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0.7",
      "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
    }
  }
},
```

## <a name="update-to-a-release-candidate-version"></a>Bir yayın adayı sürümüne güncelleştirin

Azure IOT Edge, IOT Edge hizmetinin yeni sürümleri düzenli olarak serbest bırakır. Her bir kararlı yayın önce Sürüm Adayı (RC) bir veya daha fazla yayın yok. RC sürümlerini yayımı için planlanan tüm özellikleri içerir, ancak olan kararlı bir sürüm için gerekli sınama ve doğrulama işlemleri aracılığıyla hala devam ediyor. Yeni bir özellik erken test etmek isterseniz, RC sürümünü yükleyin ve GitHub aracılığıyla geri bildirim sağlayın. 

Yayın Adayı sürümleri aynı numaralandırma kuralı sürümlerin izleyin, ancak sahip **-rc** artı sonuna artan bir sayı. Sürüm adayları aynı listesinde görebilirsiniz [Azure IOT Edge sürümleri](https://github.com/Azure/azure-iotedge/releases) kararlı sürümler olarak. Örneğin, bulma **1.0.7-rc1** ve **1.0.7-rc2**, iki sürüm önce gelen aday **1.0.7**. RC sürümleri ile işaretlenmiş atabilirsiniz **yayın öncesi** etiketler. 

Önizleme, yayın adayı sürümleri en son sürüm olarak dahil edilmez, normal yükleyicileri hedef. Bunun yerine, el ile test etmek istediğiniz RC sürümü için varlıklar hedef gerekir. IOT Edge cihaz işletim sistemine bağlı olarak, IOT Edge belirli bir sürüme güncelleştirmek için aşağıdaki bölümleri kullanın:

* [Linux X64](how-to-install-iot-edge-linux.md#install-a-specific-version)
* [Linux ARM32](how-to-install-iot-edge-linux-arm.md#install-a-specific-version)
* [Windows](how-to-install-iot-edge-windows.md#offline-installation)

## <a name="next-steps"></a>Sonraki adımlar

En son görüntülemek [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

En son güncelleştirmeleri ve Duyuruda ile güncel kalın [nesnelerin interneti blogu](https://azure.microsoft.com/blog/topics/internet-of-things/) 