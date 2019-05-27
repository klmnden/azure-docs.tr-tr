---
title: Linux için Azure IOT Edge üzerinde Windows yükleme | Microsoft Docs
description: Windows 10, Windows Server ve Windows IOT Core Linux kapsayıcıları için Azure IOT Edge yükleme yönergeleri
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: kgremban
ms.openlocfilehash: b7386cbbe18d7e05c2fbffb96f6214b468956192
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151697"
---
# <a name="use-iot-edge-on-windows-to-run-linux-containers"></a>IOT Edge, Linux kapsayıcıları çalıştırmak için Windows üzerinde kullanın.

IOT Edge modüllerini kullanarak bir Windows makinede Linux cihazlarda test edin. 

Bir üretim senaryosunda, Windows cihazları, yalnızca Windows kapsayıcıları çalıştırmanız gerekir. Ancak, ortak bir geliştirme senaryosu bir Windows bilgisayar IOT Edge modülleri Linux cihazlar için oluşturulacak kullanmaktır. IOT Edge çalışma zamanı Windows için Linux kapsayıcıları için çalıştırmanıza izin veren **test ve geliştirme** amaçlar. 

Bu makalede, Windows x64 (AMD/Intel) Linux kapsayıcıları kullanarak Azure IOT Edge çalışma zamanı yükleme adımlarını listeler sistem. Tüm yükleme parametreleriyle ilgili ayrıntılar dahil olmak üzere IOT Edge çalışma zamanı yükleyicisi hakkında daha fazla bilgi için bkz. [Windows üzerinde Azure IOT Edge çalışma zamanı yükleme](how-to-install-iot-edge-windows.md).

## <a name="prerequisites"></a>Önkoşullar

Bu bölümde, Windows cihazınız, IOT Edge desteği olup olmadığını gözden geçirin ve yüklemeden önce kapsayıcı altyapısı için hazırlamak için kullanın. 

### <a name="supported-windows-versions"></a>Desteklenen Windows sürümleri

Linux kapsayıcıları ile Azure IOT Edge, aşağıdaki Windows sürümleri üzerinde çalıştırabilirsiniz: 
* Windows 10 Yıldönümü Güncelleştirmesi (derleme 14393) veya daha yeni
* Windows Server 2016 veya daha yenisi

IOT Edge en son sürümüne nelerin dahil olduğunu hakkında daha fazla bilgi için bkz. [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

IOT Edge bir sanal makineye yüklemek isterseniz, iç içe sanallaştırmayı etkinleştirmek ve en az 2 GB bellek ayrılamadı. İç içe sanallaştırma nasıl etkinleştirmeniz hiper yöneticide kullanımınıza bağlı olarak farklılık gösterir. Hyper-V için 2. nesil sanal makineler varsayılan olarak etkin iç içe sanallaştırmayı. VMWare için sanal makinenizde özelliği etkinleştirmek için bir geçiş yoktur. 

### <a name="prepare-the-container-engine"></a>Kapsayıcı altyapısını hazırlama 

Azure IOT Edge dayanır bir [OCI uyumlu](https://www.opencontainers.org/) container altyapısı. Windows ve Linux kapsayıcıları makinesidir IOT Edge yükleme bir Windows kapsayıcı çalışma zamanı içerir, ancak kendi çalışma zamanı, IOT Edge yüklemeden önce Linux kapsayıcıları için sağlamanız gereken bir Windows üzerinde çalışan arasındaki en büyük yapılandırma fark. 

Geliştirme ve test kapsayıcıları Linux cihazlar için bir Windows makinesine ayarlamak için kullanabileceğiniz [Docker Masaüstü](https://www.docker.com/docker-windows) kapsayıcı altyapınız olarak. Docker'ı yükleyip şekilde yapılandırmanız gerekir [Linux kapsayıcılarını](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) IOT Edge yüklemeden önce.  

IOT Edge Cihazınızı bir Windows bilgisayar olduğunda bu karşıladığından emin olun [sistem gereksinimleri](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) Hyper-V için.

## <a name="install-iot-edge-on-a-new-device"></a>IOT Edge yeni bir cihaza yükleme

>[!NOTE]
>Azure IOT Edge yazılım paketlerini (lisans dizininde) paketleri bulunan lisans koşullarına tabidir. Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

Bir PowerShell Betiği indirir ve Azure IOT Edge güvenlik daemon'ı yükler. Güvenlik daemon daha sonra diğer modüllerin uzak dağıtımları sağlayan IOT Edge Aracısı ilk iki çalışma zamanı modüllerinin başlatır. 

IOT Edge çalışma zamanı, bir cihaz üzerinde ilk kez yüklediğinizde, bir IOT hub'ından kimliğe sahip cihazı sağlamak gerekir. Tek bir IOT Edge cihazı IOT hub tarafından sağlanan cihaz bağlantı dizesini kullanarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti otomatik olarak ayarlamak için birçok cihaz olduğunda kullanışlı olan cihazları sağlamak için kullanabilirsiniz. 

Daha fazla farklı bir yükleme seçeneklerini ve parametrelerini makaledeki hakkında [Windows üzerinde Azure IOT Edge çalışma zamanı yükleme](how-to-install-iot-edge-windows.md). Docker yüklü ve yapılandırılmış Linux kapsayıcıları için Masaüstü aldıktan sonra ana yükleme fark Linux ile bildirme **- ContainerOs** parametresi. Örneğin: 

1. Henüz kaydolmadıysanız cihaz bağlantı dizesi alma ve yeni bir IOT Edge cihazı kaydedin. Bu bölümde daha sonra kullanmak üzere bağlantı dizesini kopyalayın. Bu adımı aşağıdaki araçları kullanarak tamamlayabilirsiniz:

   * [Azure portal](how-to-register-device-portal.md)
   * [Azure CLI](how-to-register-device-cli.md)
   * [Visual Studio Code](how-to-register-device-vscode.md)

2. PowerShell'i yönetici olarak çalıştırın.

   >[!NOTE]
   >IOT Edge, PowerShell (x86) yüklemek için bir AMD64 PowerShell oturumu kullanın. Kullanmakta olduğunuz hangi oturum türü emin değilseniz, aşağıdaki komutu çalıştırın:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. **Dağıt IoTEdge** komutu, Windows makinenizde desteklenen bir sürümü, kapsayıcıları özelliğini açar ve sonra (Linux kapsayıcıları için kullanılmaz) moby çalışma zamanı ve IOT Edge çalışma zamanı yükler denetler. Windows kapsayıcıları için komut varsayılan değerleri İstenen kapsayıcı işletim sistemi olarak bu nedenle Linux bildirin. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Linux
   ```

4. Bu noktada, IOT Core cihazları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server cihazları yeniden başlatmanızı isteyebilir. Bu durumda, cihazınız artık yeniden başlatın. PowerShell'i yönetici olarak yeniden çalıştırın, cihaz hazır olduktan sonra.

5. **Başlatma IoTEdge** komut, IOT Edge çalışma zamanı makinenizde yapılandırır. Komut, varsayılan olarak bir cihaz bağlantı dizesiyle el ile sağlama. Linux kapsayıcısı istenen işletim sistemi olarak yeniden bildirin. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ContainerOs Linux
   ```

6. İstendiğinde, 1. adımda aldığınız cihaz bağlantı dizesi sağlayın. Cihaz bağlantı dizesini fiziksel cihaz IOT hub cihaz kimliği ile ilişkilendirir. 

   Cihaz bağlantı dizesi aşağıdaki biçimi alır ve tırnak işaretleri içermemesi gerekir: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

IoT Edge hizmetinin durumunu kontrol edin. Olarak listelenmesi gerekir.  

```powershell
Get-Service iotedge
```

Son 5 dakika Hizmeti günlüklerini inceleyin. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Çalışan modülleri listeleyin. Çalışan olduğunu gördüğünüz yalnızca modülü yeni bir yüklemeden sonra **edgeAgent**. Çalıştırdıktan sonra [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md), diğerleri görürsünüz. 

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Yüklü olan çalışma zamanı ile sağlanan bir IOT Edge cihazına sahip olduğunuza göre şunları yapabilirsiniz [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md).

IOT Edge düzgün yüklerken sorunlarla karşılaşıyorsanız, kullanıma [sorun giderme](troubleshoot.md) sayfası.

Var olan bir yüklemesini IOT Edge en yeni sürüme güncelleştirmek için bkz: [IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
