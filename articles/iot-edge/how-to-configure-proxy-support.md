---
title: Azure IOT Edge cihazları ağ proxy'leri için yapılandırma | Microsoft Docs
description: Azure IOT Edge çalışma zamanı ve bir proxy sunucu üzerinden iletişim kurmak için herhangi bir internet'e yönelik IOT Edge modüllerini nasıl yapılandırılacağı.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 09/24/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6e6a1d2f758cabca41ac405a01de1f0d8bfd0a7b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037465"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>Bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma

IOT Edge cihazları, IOT Hub ile iletişim kurmak için HTTPS isteği gönderir. Cihazınızı bir ara sunucu kullanıyorsa bir ağa bağlıysa, sunucu üzerinden iletişim kurmak için IOT Edge çalışma zamanı'nı yapılandırmanız gerekir. Edge hub'ı yönlendirilmesini değil HTTP veya HTTPS isteklerini yapmaları durumunda proxy sunucuları tek tek bir IOT Edge modüllerinin da etkileyebilir. 

IOT Edge cihazı bir proxy sunucusu ile çalışmak için yapılandırma temel adımları izler: 

1. IOT Edge çalışma zamanı, Cihazınızda yükleyin. 
2. Docker daemon'ı ve IOT Edge arka plan programı, Cihazınızda proxy sunucusu kullanacak şekilde yapılandırın.
3. Cihazınızda config.yaml dosyasındaki edgeAgent özelliklerini yapılandırın.
4. Ortam değişkenlerini IOT Edge çalışma zamanı ve diğer IOT Edge modülleri dağıtım bildiriminde ayarlayın. 

## <a name="install-the-runtime"></a>Çalışma zamanını yükleme

Bir Linux cihaza IOT Edge çalışma zamanı'nı yüklüyorsanız, yükleme paketi erişmek için Ara sunucu üzerinden gitmesi için Paket Yöneticisi'ni yapılandırın. Örneğin, [apt-get bir http Ara sunucusunu kullanacak şekilde](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy). Paket Yöneticisi'ni yapılandırıldıktan sonra yönergeleri izleyin [yükleme Azure IOT Edge çalışma zamanı (ARM32v7/armhf) Linux'ta](how-to-install-iot-edge-linux-arm.md) veya [(x64) Linux üzerinde Azure IOT Edge çalışma zamanı yükleme](how-to-install-iot-edge-linux.md) zamanki. 

Bir Windows cihaza IOT Edge çalışma zamanı'nı yüklüyorsanız, yükleme paketi erişmek için Ara sunucu üzerinden gitmesi gerekir. Proxy bilgileri Windows ayarlarında yapılandırdığınız veya proxy bilgilerinizi doğrudan yükleme betiğini içerir. Aşağıdaki powershell betiğini kullanarak bir windows yükleme ilişkin bir örnektir `-proxy` bağımsız değişkeni:

```powershell
. {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows
```

Daha fazla bilgi ve yükleme seçenekleri için bkz [Windows kapsayıcıları ile kullanmak için Windows yükleme Azure IOT Edge çalışma zamanına](how-to-install-iot-edge-windows-with-windows.md) veya [Linux kapsayıcıları ile kullanmak için Windows yükleme Azure IOT Edge çalışma zamanına](how-to-install-iot-edge-windows-with-linux.md).

IOT Edge çalışma zamanı yüklendikten sonra Ara sunucu bilgileri ile yapılandırmak için aşağıdaki bölümü kullanın. 

## <a name="configure-the-daemons"></a>Daemon'ları yapılandırma

IOT Edge Cihazınızda çalışan Docker ve IOT Edge Daemon proxy sunucusu kullanacak şekilde yapılandırılması gerekir. Docker Daemon programını web istekleri, kapsayıcı kayıt defterleri için kapsayıcı görüntüleri çekmeniz hale getirir. IOT Edge arka plan programı, IOT Hub ile iletişim kurmak için web isteği yapar.

### <a name="docker-daemon"></a>Docker Daemon programını

Docker Daemon programını ortam değişkenleriyle yapılandırmak için Docker belgelerine bakın. Çoğu kapsayıcı kayıt defterleri (DockerHub ve Azure kapsayıcı kayıt defterleri dahil) ayarlamalısınız değişkendir HTTPS istekleri desteklediğimizden **erişmek**. Aktarım Katmanı Güvenliği (TLS) desteklemeyen bir kayıt defterinden görüntüleri çekmek sonra ayarlamalısınız **HTTP_PROXY**. 

Docker sürümünüzü makalenin seçin: 

* [Docker](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
* [Windows için docker](https://docs.docker.com/docker-for-windows/#proxies)

### <a name="iot-edge-daemon"></a>IOT Edge arka plan programı

IOT Edge arka plan programı, Docker Daemon programını benzer şekilde yapılandırılır. IOT Edge, IOT Hub'ına gönderir tüm istekler için HTTPS kullanır. İşletim sisteminize bağlı hizmeti için bir ortam değişkenini ayarlamak için aşağıdaki adımları kullanın. 

#### <a name="linux"></a>Linux

IOT Edge daemon'ı yapılandırmak için terminalde Düzenleyicisi. 

```bash
sudo systemctl edit iotedge
```

Aşağıdaki metni girin değiştirerek  **\<proxy URL'si >** proxy sunucu adresi ve bağlantı noktası. Ardından, kaydedin ve çıkın. 

```text
[Service]
Environment="https_proxy=<proxy URL>"
```

Hizmet Yöneticisi'ni iotedge yeni yapılandırmasını almak için yenileyin.

```bash
sudo systemctl daemon-reload
```

IOT Edge değişikliklerin etkili olması için yeniden başlatın.

```bash
sudo systemctl restart iotedge
```

Ortam değişkeninize oluşturuldu ve yeni yapılandırmayı yüklenip yüklenmediğini doğrulayın. 

```bash
systemctl show --property=Environment iotedge
```

#### <a name="windows"></a>Windows

Yönetici olarak bir PowerShell penceresi açın ve yeni ortam değişkeniyle kayıt defterini düzenlemek için aşağıdaki komutu çalıştırın. Değiştirin  **\<proxy URL'si >** proxy sunucu adresi ve bağlantı noktası. 

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\iotedge /v Environment /t REG_MULTI_SZ /d https_proxy=<proxy URL>
```

IOT Edge değişikliklerin etkili olması için yeniden başlatın.

```powershell
Restart-Service iotedge
```

## <a name="configure-the-edge-agent"></a>Edge Aracısı'nı yapılandırma

Edge Aracısı'nı, üzerinde herhangi bir IOT Edge cihazı başlatmak için ilk bir modüldür. IOT Edge config.yaml dosyanın içindeki bilgileri temel alan ilk kez başlatılır. Edge Aracısı'nı, ardından, diğer modüllerin neler olması gerektiğini bildirin ve dağıtım bildirimlerini almak için IOT Hub'ına bağlanır cihazda dağıtılabilir.

IOT Edge Cihazınızda config.yaml dosyasını açın. Linux sistemlerinde, bu dosya şu konumdadır **/etc/iotedge/config.yaml**. Windows sistemlerde, bu dosya şu konumdadır **C:\ProgramData\iotedge\config.yaml**. Yapılandırma dosyası korunur ve ona erişmek için yönetici ayrıcalıkları gerekir. Linux sistemlerinde kullanarak olan anlamına `sudo` tercih edilen bir metin Düzenleyicisi'nde dosyasını açmadan önce komutu. Windows üzerinde yönetici olarak çalıştırmak için Not Defteri gibi bir metin düzenleyicisinde açmak ve ardından dosyayı açmayı anlamına gelir. 

Config.yaml dosyasında **Edge Aracısı modülü spec** bölümü. Edge Aracısı tanımını içeren bir **env** ortam değişkenlerini ekleyebileceğiniz parametresi. 

![edgeAgent tanımı](./media/how-to-configure-proxy-support/edgeagent-unedited.png)

Env parametresi için yer tutucular ve yeni bir satıra yeni bir değişken ekleyin, küme parantezleri kaldırın. YAML içinde girintileri iki boşluk olduğunu unutmayın. 

```yaml
https_proxy: "<proxy URL>"
```

IOT Edge çalışma zamanı, IOT Hub'kurmak, varsayılan olarak AMQP kullanır. Bazı Ara sunucuları AMQP bağlantı noktalarını engelleyin. Bu durumda, daha sonra ayrıca edgeAgent WebSocket üzerinden AMQP kullanmak için yapılandırmanız gerekir. İkinci bir ortam değişkenine ekleyin.

```yaml
UpstreamProtocol: "AmqpWs"
```

![ortam değişkenleriyle edgeAgent tanımı](./media/how-to-configure-proxy-support/edgeagent-edited.png)

Config.yaml için değişiklikleri kaydedin ve düzenleyiciyi kapatın. IOT Edge değişikliklerin etkili olması için yeniden başlatın. 

* Linux: 

   ```bash
   sudo systemctl restart iotedge
   ```

* Windows:

   ```powershell
   Restart-Service iotedge
   ```

## <a name="configure-deployment-manifests"></a>Dağıtım bildirimleri yapılandırma  

IOT Edge Cihazınızı proxy sunucunuz ile çalışmak üzere yapılandırıldı sonra ayrıca tüm gelecekteki dağıtım bildirimleri ortam değişkenleri bildirmek gerekir. İki çalışma zamanı modülü ve edgeAgent edgeHub, her zaman olmalıdır IOT Hub ile iletişim sağlamak için yapılandırılmış bir proxy sunucu. Bir proxy sunucu üzerinden iletişim kurmak için herhangi bir IOT Edge modülü yapılandırabilirsiniz, ancak diğer modüllerle cihazda, kendi iletileri edgeHub aracılığıyla yönlendirmek veya yalnızca iletişim modüller için gerekli değildir. 

Azure portalını kullanarak ya da el ile bir JSON dosyası düzenleme dağıtım bildirimleri oluşturabilirsiniz. 

### <a name="azure-portal"></a>Azure portal

Kullanırken **modülleri ayarlama** cihazları IOT Edge dağıtımlarını oluşturmak için Sihirbazı her bir modüle sahip bir **ortam değişkenlerini** proxy sunucu bağlantılarını yapılandırmak için kullanabileceğiniz bir bölüm. 

Edge aracısı ve Edge hub'ı modülleri yapılandırmak için seçin **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** Sihirbazı'nın ilk adımında. 

![Gelişmiş Edge Çalışma Zamanı ayarlarını yapılandırma](./media/how-to-configure-proxy-support/configure-runtime.png)

Ekleme **erişmek** ortam değişkeni Edge aracısı ve Edge hub'ı modül tanımları. Dahil ettiyseniz **UpstreamProtocol** IOT Edge Cihazınızda config.yaml dosyasında ortam değişkeni ekleyin, Edge Aracısı modülü tanımına çok. 

![Ortam değişkenlerini belirleme](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

Bir dağıtım bildirimine eklediğiniz tüm diğer modülleri aynı düzeni uygular. Modül adı ve görüntü ayarlandığı sayfasında, bir ortam değişkenleri bölümü yoktur.

### <a name="json-deployment-manifest-files"></a>JSON dağıtım bildirim dosyaları

Visual Studio code'da veya JSON dosyalarını el ile oluşturarak şablonları kullanarak cihazları IOT Edge dağıtımlarını oluşturursanız, doğrudan her modül tanımı için ortam değişkenlerini ekleyebilirsiniz. 

Aşağıdaki JSON biçimi kullanın: 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    }
}
```

Ortam değişkenleri dahil, modül tanımınızı edgeHub aşağıdaki örnekteki gibi görünmelidir:

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": ""
    },
    "env": {
        "https_proxy": {
            "value": "https://proxy.example.com:3128"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Dahil ettiyseniz **UpstreamProtocol** IOT Edge Cihazınızda confige.yaml dosyasında ortam değişkeni ekleyin, Edge Aracısı modülü tanımına çok. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Rolleri hakkında daha fazla bilgi [IOT Edge çalışma zamanı](iot-edge-runtime.md).

Yükleme ve yapılandırma hataları ile ilgili sorunları giderme [genel sorunlar ve çözümleri için Azure IOT Edge](troubleshoot.md)

