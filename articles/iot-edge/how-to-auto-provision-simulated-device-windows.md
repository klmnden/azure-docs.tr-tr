---
title: DPS - Windows ile Azure IOT sınır cihazı otomatik sağlama | Microsoft Docs
description: Bir sanal cihaz Windows makinenizde otomatik cihaz cihaz sağlama hizmeti ile Azure IOT köşesi sağlama sınamak için kullanın.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0a973b248022cf3c0497f72bc2fcdd45a6527e65
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116288"
---
# <a name="create-and-provision-a-simulated-tpm-edge-device-on-windows"></a>Oluşturma ve Windows sanal bir TPM kenar cihaz sağlama

Azure IOT sınır cihazları otomatik-sağlanabilir kullanarak [aygıt hizmeti sağlama](../iot-dps/index.yml) kenar etkin olmayan cihazları'olduğu gibi. Otomatik sağlama işlemine değilseniz gözden [otomatik sağlama kavramları](../iot-dps/concepts-auto-provisioning.md) devam etmeden önce. 

Bu makalede, test etme otomatik sağlama aşağıdaki adımlarla benzetimli kenar aygıtta gösterir: 

* Bir örneği, IOT Hub cihaz sağlama hizmeti (DPS) oluşturun.
* Donanım güvenlik için Windows makinenizde ile bir sanal Güvenilir Platform Modülü (TPM) sanal bir cihaz oluşturma.
* Cihaz için ayrı bir kayıt oluşturun.
* IOT kenar çalışma zamanı yükleme ve cihazın IOT Hub'ına bağlanın.

## <a name="prerequisites"></a>Önkoşullar

* Bir Windows geliştirme makine. Bu makalede Windows 10 kullanır. 
* Etkin bir IOT Hub. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>IOT Hub cihaz sağlama hizmetini kurma

Azure IOT Hub cihaz sağlama hizmeti yeni bir örneğini oluşturun ve IOT hub'ınıza bağlayın. ' Ndaki yönergeleri izleyin [IOT Hub DPS ayarlamak](../iot-dps/quick-setup-auto-provision.md).

Cihaz sağlama hizmeti çalışır durumda sonra değerini kopyalayın **kimliği kapsam** genel bakış sayfasında. IOT kenar çalışma zamanı yapılandırdığınızda, bu değeri kullanın. 

## <a name="simulate-a-tpm-device"></a>TPM cihazının benzetimini yapma

Sanal bir TPM cihaz, Windows geliştirme makinenizde oluşturma. Alma **kayıt kimliği** ve **onay anahtarını** , cihazınız için ve dağıtım noktaları bir tek tek kayıt girişi oluşturmak için bunları kullanın. 

Dağıtım noktaları bir kayıt oluşturduğunuzda, bildirme fırsatına sahip bir **ilk cihaz çifti durumu**. Cihaz çiftine çözümünüzdeki bölge, ortam, konum veya aygıt türü gibi gereken herhangi bir ölçümü tarafından cihazları için etiketler ayarlayabilirsiniz. Bu etiketler oluşturmak için kullanılan [otomatik dağıtımları](how-to-deploy-monitor.md). 

Sanal cihazı oluşturmak için kullanmak istediğiniz SDK dili seçin ve tek tek kayıt oluşturana kadar adımları izleyin. 

Tek tek kayıt oluşturduğunuzda seçmeniz **etkinleştirmek** bu sanal makine olduğunu bildirmek için bir **IOT sınır cihazı**.

Sanal cihazı ve tek tek kayıt kılavuzları: 
* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Tek tek kayıt oluşturduktan sonra değeri kaydetmek **kayıt kimliği**. IOT kenar çalışma zamanı yapılandırdığınızda, bu değeri kullanın. 

## <a name="install-the-iot-edge-runtime"></a>IOT kenar çalışma zamanı yükleme

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenlerinden kapsayıcılarında çalıştırmak ve kod sınırında çalıştırabilmeniz için ek kapsayıcıları dağıtmak olanak sağlar. Windows çalıştıran cihazlarda ya da Windows kapsayıcıları ya da Linux kapsayıcıları kullanmayı da tercih edebilirsiniz. Kullanmak istediğiniz kapsayıcıları türünü seçin ve adımları izleyin. Otomatik, değil el ile sağlama IOT kenar çalışma zamanı yapılandırdığınızdan emin olun. 

Benzetimli TPM önceki bölümden çalıştıran cihazın IOT kenar çalışma zamanı yüklemek için yönergeleri izleyin. 

DPS bilmeniz **kimliği kapsam** ve cihaz **kayıt kimliği** Bu makaleler başlamadan önce. 

* [Windows kapsayıcıları](how-to-install-iot-edge-windows-with-windows.md)
* [Linux kapsayıcıları](how-to-install-iot-edge-windows-with-linux.md)

## <a name="create-a-tpm-environment-variable"></a>TPM ortam değişkeni oluşturun

Sanal cihazınız çalıştığı makinede değiştirme **iotedge** bir ortam değişkeni ayarlamak için hizmet kayıt defteri.

1. Gelen **Başlat** menüsünde, açık **regedit**. 
2. Gidin **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\iotedge**. 
3. Seçin **Düzenle** > **yeni** > **çok dizeli değer**. 
4. Bir ad girin **ortam**. 
5. Yeni değişkeni çift tıklatın ve değer verisini **IOTEDGE_USE_TPM_DEVICE ON =**. 
6. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın. 

## <a name="restart-the-iot-edge-runtime"></a>IOT kenar çalışma zamanı yeniden başlatın

Bu cihaz üzerinde yapılan tüm yapılandırma değişikliklerini seçer böylece IOT kenar çalışma zamanı yeniden başlatın. 

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

IOT kenar çalışma zamanı çalışıp çalışmadığını denetleyin. 

   ```bash
   sudo systemctl status iotedge
   ```

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Çalışma zamanı başarıyla başlatılırsa, IOT Hub'ına gidin ve yeni Cihazınızı otomatik olarak sağlanan ve modülleri IOT kenar çalıştırılmaya hazır olduğunu öğrenin. 

IOT kenar hizmetinin durumunu kontrol edin.

```powershell
Get-Service iotedge
```

Son 5 dakika Hizmeti günlüklerini inceleyin.

```powershell
# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false}
```

Çalışan modülleri listele.

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Aygıt hizmeti sağlama kayıt işlemini yeni cihazı hazırlama olarak cihaz çiftine etiketleri ve cihaz kimliği aynı anda ayarlamanıza olanak tanır. Bu değerler, tek bir cihazı veya otomatik cihaz Yönetimi'ni kullanarak cihaz gruplarını hedeflemek için kullanabilirsiniz. Bilgi nasıl [dağıtma ve izleme modüller ölçeklendirme Azure portalını kullanarak IOT kenar](how-to-deploy-monitor.md) veya [Azure CLI kullanma](how-to-deploy-monitor-cli.md)