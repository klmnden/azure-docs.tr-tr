---
title: Otomatik sağlama Windows cihazlarıyla DPS - Azure IOT Edge | Microsoft Docs
description: Otomatik cihaz, cihaz sağlama hizmeti ile Azure IOT Edge için sağlama test etmek için Windows makinenizde bir simülasyon cihazı kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/09/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 01247dfc0046ef722d70fe48f7ab8ee63f685962
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65153566"
---
# <a name="create-and-provision-a-simulated-tpm-edge-device-on-windows"></a>Windows üzerinde sanal bir TPM Edge cihazı oluşturma ve sağlama

Azure IOT Edge cihazları otomatik-sağlanabilir kullanarak [cihaz sağlama hizmeti](../iot-dps/index.yml) edge etkin olmayan cihazlar'olduğu gibi. Otomatik sağlama işlemine bilmiyorsanız gözden [otomatik sağlama kavramlarını](../iot-dps/concepts-auto-provisioning.md) devam etmeden önce.

Bu makalede, test etmek otomatik sağlama aşağıdaki adımlarla sanal bir kenar cihazda gösterilmektedir:

* Bir örnek, IOT Hub cihaz sağlama hizmeti (DPS) oluşturun.
* Donanım güvenlik için Windows makinenizde ile bir sanal Güvenilir Platform Modülü (TPM) bir simülasyon cihazı oluşturun.
* Cihazı için bireysel bir kayıt oluşturun.
* IOT Edge çalışma zamanını yükleyin ve cihazı IOT hub'a bağlayın.

## <a name="prerequisites"></a>Önkoşullar

* Bir Windows geliştirme makinesi. Bu makalede, Windows 10 kullanır.
* Etkin bir IOT Hub.

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>IOT Hub cihazı sağlama hizmetini ayarlama

Azure'da yeni bir IOT Hub cihazı sağlama hizmeti örneğini oluşturun ve IOT hub'ınıza bağlayın. ' Ndaki yönergeleri takip edebilirsiniz [IOT hub'ı DPS ' ayarlamak](../iot-dps/quick-setup-auto-provision.md).

Cihaz sağlama hizmeti çalışıyor sonra değerini kopyalayın **kimlik kapsamı** genel bakış sayfasında. IOT Edge çalışma zamanı yapılandırdığınızda bu değeri kullanın.

## <a name="simulate-a-tpm-device"></a>TPM cihazının benzetimini yapma

Windows geliştirme makinenizde sanal bir TPM cihazı oluşturma. Alma **kayıt kimliği** ve **onay anahtarını** , cihazınız için ve DPS'de bir bireysel kayıt girişi oluşturma için bunları kullanın.

DPS'de bir kayıt oluşturduğunuzda, bildirme fırsatına sahip bir **ilk cihaz İkizi durumu**. Cihaz ikizinde bölge, ortam, konuma veya cihaz türü gibi çözümünüzdeki gereken herhangi bir ölçümü tarafından cihazları için etiketler ayarlayabilirsiniz. Bu etiketleri oluşturmak için kullanılan [otomatik dağıtımlar](how-to-deploy-monitor.md).

Sanal cihazı oluşturmak için kullanmak istediğiniz SDK dil seçin ve bireysel kayıt oluşturana kadar adımları izleyin.

Bireysel kayıt oluşturduğunuzda **etkinleştirme** Windows geliştirme makinenizde sanal TPM cihazı olduğunu bildirmek için bir **IOT Edge cihazı**.

Sanal cihazı ve bireysel kayıt kılavuzları:

* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Bireysel kayıt oluşturduktan sonra değerini kaydedin **kayıt kimliği**. IOT Edge çalışma zamanı yapılandırdığınızda bu değeri kullanın.

## <a name="install-the-iot-edge-runtime"></a>IOT Edge çalışma zamanını yükleme

Önceki bölümde tamamladıktan sonra yeni cihazınızın IOT hub'ınızda bir IOT Edge cihazı olarak listelendiğini görmeniz gerekir. Şimdi, cihazınızın IOT Edge çalışma zamanı yüklemeniz gerekir.

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarında çalıştırmak ve kod ucuna çalıştırabilmeniz için cihaza ek kapsayıcıları dağıtma olanak sağlar.  

Sanal TPM önceki bölümden çalıştıran cihazın IOT Edge çalışma zamanı yüklemek için yönergeleri izleyin. Otomatik değil el ile sağlama için IOT Edge çalışma zamanı yapılandırdığınızdan emin olun.

DPS'niz bilmeniz **kimlik kapsamı** ve cihaz **kayıt kimliği** IOT Edge Cihazınızda yüklemeden önce.

[Yükleme ve IOT Edge otomatik olarak sağlama](how-to-install-iot-edge-windows.md#option-2-install-and-automatically-provision)

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Çalışma zamanı başarıyla başlatıldı, IOT Hub'ına gidin ve IOT Edge modülleri, cihazınıza dağıtmaya başlayın. Aşağıdaki komutlar, çalışma zamanı yüklü ve başarıyla başlatıldı doğrulamak için Cihazınızda kullanın.  

IoT Edge hizmetinin durumunu kontrol edin.

```powershell
Get-Service iotedge
```

Son 5 dakika Hizmeti günlüklerini inceleyin.


```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Çalışan modülleri listeleyin.

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Cihaz sağlama hizmeti kayıt işlemi, yeni cihaz sağlama gibi cihaz kimliği ve cihaz ikizi etiketleri aynı anda belirlemenizi sağlar. Bu değerleri ayrı ayrı cihazlar ya da otomatik cihaz Yönetimi'ni kullanarak cihaz grupları hedeflemek için kullanabilirsiniz. Bilgi edinmek için nasıl [dağıtma ve izleme IOT Edge modülleri, ölçeklendirme Azure portalını kullanarak](how-to-deploy-monitor.md) veya [Azure CLI kullanarak](how-to-deploy-monitor-cli.md)
