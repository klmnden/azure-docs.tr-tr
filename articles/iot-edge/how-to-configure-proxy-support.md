---
title: Ağ Ara sunucuları - Azure IOT Edge cihazları yapılandırma | Microsoft Docs
description: Azure IOT Edge çalışma zamanı ve bir proxy sunucu üzerinden iletişim kurmak için herhangi bir internet'e yönelik IOT Edge modüllerini nasıl yapılandırılacağı.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 1c0da1a768b894f543b9089643622c31d6a8758d
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730155"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>Bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma

IOT Edge cihazları, IOT Hub ile iletişim kurmak için HTTPS isteği gönderir. Cihazınızı bir ara sunucu kullanıyorsa bir ağa bağlıysa, sunucu üzerinden iletişim kurmak için IOT Edge çalışma zamanı'nı yapılandırmanız gerekir. Bunlar IOT Edge hub'ı ile yönlendirilen olmayan HTTP veya HTTPS istekleri yapıyorsa proxy sunucuları tek tek bir IOT Edge modüllerinin da etkileyebilir. 

Bu makalede, yapılandırmak ve ardından bir proxy sunucusunun arkasında bir IOT Edge cihazı yönetmek için aşağıdaki dört adım adım yönergeler sağlanmıştır: 

1. **IOT Edge çalışma zamanı, Cihazınızda yükleyin.**

   Cihazınızı bu isteğinde bulunmak için proxy sunucu üzerinden iletişim kurması gereken şekilde IOT Edge yükleme betikleri, paketleri ve dosyaları internet'ten çekin. Ayrıntılı adımlar için bkz. [bir ara sunucu aracılığıyla çalışma zamanı yükleme](#install-the-runtime-through-a-proxy) bu makalenin. Windows cihazları için yükleme komut dosyası ayrıca sağlar bir [çevrimdışı yükleme](how-to-install-iot-edge-windows.md#offline-installation) seçeneği. 

   Bu adım, ilk önce onu ayarlarken, IOT Edge cihazında gerçekleştirilen tek seferlik bir işlemdir. IOT Edge çalışma zamanı güncelleştirdiğinizde aynı bağlantıları da gereklidir. 

2. **Docker daemon'ı ve IOT Edge arka plan programı, Cihazınızda yapılandırın.**

   IOT Edge, cihazdaki ikisi için de ara sunucu üzerinden web isteklerinde bulunmak için gereken iki Daemon'ları kullanır. IOT Edge arka plan programı, IOT Hub ile iletişim için sorumludur. Moby arka plan programı kapsayıcı yönetiminden sorumlu, bu nedenle kapsayıcı kayıt defterleri ile iletişim kurar. Ayrıntılı adımlar için bkz. [Daemon'ları yapılandırma](#configure-the-daemons) bu makalenin. 

   Bu adım, ilk önce onu ayarlarken, IOT Edge cihazında gerçekleştirilen tek seferlik bir işlemdir.

3. **Cihazınızda config.yaml dosyasındaki IOT Edge aracı özelliklerini yapılandırın.**

   IOT Edge arka plan programı edgeAgent modülü başlangıçta başlatır, ancak ardından edgeAgent modülü dağıtım bildirimi IOT Hub'ından alınıyor ve tüm diğer modülleri başlangıç sorumludur. IOT Edge Aracısı IOT Hub'ına ilk bağlantıyı kurmak edgeAgent modülü ortam değişkenlerini cihazda el ile yapılandırın. İlk bağlantının ardından edgeAgent modülü uzaktan yapılandırabilirsiniz. Ayrıntılı adımlar için bkz. [IOT Edge Aracısı'nı yapılandırma](#configure-the-iot-edge-agent) bu makalenin.

   Bu adım, ilk önce onu ayarlarken, IOT Edge cihazında gerçekleştirilen tek seferlik bir işlemdir.

4. **Tüm modül gelecekteki dağıtımlar için proxy sunucusu üzerinden iletişim kuran herhangi bir modül için ortam değişkenlerini ayarlayın.**

   IOT Edge Cihazınızı ayarlamak ve proxy sunucusu üzerinden IOT Hub'ına bağlı sonra tüm modül gelecekteki dağıtımlar bağlantıyı korumak için gerekir. Ayrıntılı adımlar için bkz. [yapılandırma dağıtım bildirimleri](#configure-deployment-manifests) bu makalenin. 

   Bu adım, cihazın özelliği, Ara sunucu üzerinden iletişim kurmak için her yeni modül veya dağıtım güncelleştirme korur, böylece uzaktan gerçekleştirilen devam eden bir işlemdir. 

## <a name="know-your-proxy-url"></a>Proxy URL'nizi bildirin

Bu makaledeki adımları başlamadan önce proxy URL'sini de bilmeniz gerekir.

Ara sunucu URL'leri şu biçimde olması: **Protokolü**://**proxy_host**:**proxy_port**.

* **Protokolü** HTTP veya HTTPS. Docker Daemon programını kapsayıcı kayıt defteri ayarlarınıza bağlı olarak her iki protokolü kullanabilirsiniz ancak arka plan programı ve çalışma zamanı IOT Edge kapsayıcıları her zaman HTTPS kullanmalıdır.

* **Proxy_host** bir proxy sunucusunun adresidir. Ara sunucunuz kimlik doğrulaması gerektiriyorsa, kimlik bilgilerinizi proxy konağını bir parçası olarak aşağıdaki biçimde sağlayabilir: **kullanıcı**:**parola**\@**proxy_host** .

* **Proxy_port** proxy ağ trafiğini yanıt ağ bağlantı noktası.

## <a name="install-the-runtime-through-a-proxy"></a>Bir ara sunucu aracılığıyla çalışma zamanını yükleme

IOT Edge Cihazınızı Windows veya Linux'ta çalışan olsun, proxy sunucusu üzerinden yükleme paketleri erişmeniz gerekir. İşletim sisteminize bağlı olarak, IOT Edge çalışma zamanı bir ara sunucu üzerinden yüklemek için adımları izleyin. 

### <a name="linux"></a>Linux

Bir Linux cihaza IOT Edge çalışma zamanı'nı yüklüyorsanız, yükleme paketi erişmek için Ara sunucu üzerinden gitmesi için Paket Yöneticisi'ni yapılandırın. Örneğin, [apt-get bir http Ara sunucusunu kullanacak şekilde](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy). Paket Yöneticisi'ni yapılandırıldıktan sonra yönergeleri izleyin [yükleme Azure IOT Edge çalışma zamanı (ARM32v7/armhf) Linux'ta](how-to-install-iot-edge-linux-arm.md) veya [(x64) Linux üzerinde Azure IOT Edge çalışma zamanı yükleme](how-to-install-iot-edge-linux.md) zamanki.

### <a name="windows"></a>Windows

Bir Windows cihaza IOT Edge çalışma zamanı'nı yüklüyorsanız, proxy sunucusu üzerinden iki kez gitmeniz gerekiyor. İlk bağlantı Yükleyici komut dosyasını yükler ve gerekli bileşenleri yüklemek için yükleme sırasında ikinci bir bağlantı olur. Proxy bilgileri Windows ayarlarını yapılandırın veya proxy bilgilerinizi doğrudan PowerShell komutları içerir. 

Aşağıdaki adımları kullanarak bir windows yükleme örneği gösteren `-proxy` bağımsız değişkeni:

1. Invoke-WebRequest komut yükleyicisi betiği erişmek için Ara sunucu bilgileri gerekir. Daha sonra dağıtım IoTEdge komutu yükleme dosyalarının indirileceği proxy bilgisi gerekir. 

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Deploy-IoTEdge -proxy <proxy URL>
   ```

2. Initialize-IoTEdge komut ikinci adım, Ara sunucu bilgilerini yalnızca Invoke-WebRequest gerektirir. proxy sunucusu üzerinden konulunca gerekmez.

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Initialize-IoTEdge

If you have complicated credentials for the proxy server that can't be included in the URL, use the `-ProxyCredential` parameter within `-InvokeWebRequestParameters`. For example,

```powershell
$proxyCredential = (Get-Credential).GetNetworkCredential()
. {Invoke-WebRequest -proxy <proxy URL> -ProxyCredential $proxyCredential -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -InvokeWebRequestParameters @{ '-Proxy' = '<proxy URL>'; '-ProxyCredential' = $proxyCredential }
```

Ara sunucu parametreleri hakkında daha fazla bilgi için bkz. [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest). Çevrimdışı yükleme dahil olmak üzere Windows yükleme seçenekleri hakkında daha fazla bilgi için bkz. [Windows yükleme Azure IOT Edge çalışma zamanına](how-to-install-iot-edge-windows.md).

## <a name="configure-the-daemons"></a>Daemon'ları yapılandırma

IOT Edge, IOT Edge cihaz üzerinde çalışan iki Daemon bağımlıdır. Moby arka plan programı, web istekleri kapsayıcı kayıt defterleri için kapsayıcı görüntüleri çekmeniz hale getirir. IOT Edge arka plan programı, IOT Hub ile iletişim kurmak için web isteği yapar.

Moby hem IOT Edge Daemon'ları için devam eden cihazın işlevselliğini proxy sunucusu kullanacak şekilde yapılandırılması gerekir. Bu adım, ilk cihaz kurulumu sırasında IOT Edge cihazında yerini alır. 

### <a name="moby-daemon"></a>Moby arka plan programı

Docker üzerinde Moby kurulu olduğundan, ortam değişkenleriyle Moby daemon'ı yapılandırmak için Docker belgelerine bakın. Çoğu kapsayıcı kayıt defterleri (DockerHub ve Azure kapsayıcı kayıt defterleri dahil) ayarlamalısınız parametredir HTTPS istekleri desteklediğimizden **erişmek**. Aktarım Katmanı Güvenliği (TLS) desteklemeyen bir kayıt defterinden görüntüleri çekmek sonra ayarlamalısınız **HTTP_PROXY** parametresi. 

IOT Edge cihaz işletim sistemi için makaleyi seçin: 

* [Linux'ta Docker daemon'ı yapılandırma](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
    * Linux cihazlarda Moby daemon Docker adı tutar.
* [Windows Docker daemon yapılandırma](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon#proxy-configuration)
    * Windows cihazlarda Moby daemon iotedge moby çağrılır. Docker Masaüstü hem de Moby bir Windows cihazında paralel olarak çalıştırmak mümkün olduğundan farklı bir adlarıdır. 

### <a name="iot-edge-daemon"></a>IOT Edge arka plan programı

IOT Edge arka plan programı Moby arka plan programı için benzer şekilde yapılandırılır. İşletim sisteminize bağlı hizmeti için bir ortam değişkenini ayarlamak için aşağıdaki adımları kullanın. 

IOT Edge arka plan programı, IOT Hub'ına istekleri göndermek için her zaman HTTPS kullanır.

#### <a name="linux"></a>Linux

IOT Edge daemon'ı yapılandırmak için terminalde Düzenleyicisi. 

```bash
sudo systemctl edit iotedge
```

Aşağıdaki metni girin değiştirerek  **\<proxy URL'si >** proxy sunucu adresi ve bağlantı noktası. Ardından, kaydedin ve çıkın. 

```ini
[Service]
Environment="https_proxy=<proxy URL>"
```

IOT Edge için yeni yapılandırma yerden devam edebiliyorduk service manager'ı yenileyin.

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

## <a name="configure-the-iot-edge-agent"></a>IOT Edge Aracısı'nı yapılandırma

IOT Edge Aracısı'nı, üzerinde herhangi bir IOT Edge cihazı başlatmak için ilk bir modüldür. IOT Edge config.yaml dosyanın içindeki bilgileri temel alan ilk kez başlatılır. IOT Edge Aracısı'nı, ardından, diğer modüllerin neler olması gerektiğini bildirin ve dağıtım bildirimlerini almak için IOT Hub'ına bağlanır cihazda dağıtılabilir.

Bu adım, cihaz ilk kurulum sırasında bir kez IOT Edge cihazında yerini alır. 

1. IOT Edge Cihazınızda config.yaml dosyasını açın. Linux sistemlerinde, bu dosya şu konumdadır **/etc/iotedge/config.yaml**. Windows sistemlerde, bu dosya şu konumdadır **C:\ProgramData\iotedge\config.yaml**. Yapılandırma dosyası korunur ve ona erişmek için yönetici ayrıcalıkları gerekir. Linux sistemleri `sudo` tercih edilen bir metin Düzenleyicisi'nde dosyasını açmadan önce komutu. Windows, yönetici olarak Not Defteri gibi bir metin düzenleyicisinde açın ve ardından dosyasını açın. 

2. Config.yaml dosyasında **Edge Aracısı modülü spec** bölümü. IOT Edge Aracısı tanımını içeren bir **env** ortam değişkenlerini ekleyebileceğiniz parametresi. 

3. Env parametresi için yer tutucular ve yeni bir satıra yeni bir değişken ekleyin, küme parantezleri kaldırın. YAML içinde girintileri iki boşluk olduğunu unutmayın. 

   ```yaml
   https_proxy: "<proxy URL>"
   ```

4. IOT Edge çalışma zamanı, IOT Hub'kurmak, varsayılan olarak AMQP kullanır. Bazı Ara sunucuları AMQP bağlantı noktalarını engelleyin. Bu durumda, daha sonra ayrıca edgeAgent WebSocket üzerinden AMQP kullanmak için yapılandırmanız gerekir. İkinci bir ortam değişkenine ekleyin.

   ```yaml
   UpstreamProtocol: "AmqpWs"
   ```

   ![ortam değişkenleriyle edgeAgent tanımı](./media/how-to-configure-proxy-support/edgeagent-edited.png)

5. Config.yaml için değişiklikleri kaydedin ve düzenleyiciyi kapatın. IOT Edge değişikliklerin etkili olması için yeniden başlatın. 

   * Linux: 

      ```bash
      sudo systemctl restart iotedge
      ```

   * Windows:

      ```powershell
      Restart-Service iotedge
      ```

## <a name="configure-deployment-manifests"></a>Dağıtım bildirimleri yapılandırma  

IOT Edge Cihazınızı proxy sunucunuz ile çalışmak üzere yapılandırıldı sonra ileride dağıtım bildirimleri ortam değişkenleri bildirmek devam etmeniz gerekir. Azure portalı sihirbazını kullanarak dağıtım bildirimleri düzenleyebilir ya da bir dağıtım düzenleyerek bildirim JSON dosyası. 

Her zaman iki çalışma zamanı modülü ve edgeAgent edgeHub, IOT Hub ile bağlantı sağlamak için proxy sunucu üzerinden iletişim kurmak için yapılandırın. Proxy bilgileri edgeAgent modülünden kaldırırsanız, bağlantıyı yeniden kurmak için tek cihazda config.yaml dosyasını düzenleyerek önceki bölümde açıklandığı gibi yoludur. 

İnternet'e bağlanan diğer IOT Edge modülleri, çok proxy sunucu üzerinden iletişim kurması için yapılandırılması gerekir. Ancak, kendi iletileri edgeHub aracılığıyla yönlendirmek veya yalnızca iletişim modül cihazdaki diğer modüllerle proxy sunucusu ayrıntılarının gerekmez. 

Bu adım, IOT Edge cihazı ömrü devam ediyor. 

### <a name="azure-portal"></a>Azure portal

Kullanırken **modülleri ayarlama** cihazları IOT Edge dağıtımlarını oluşturmak için Sihirbazı her bir modüle sahip bir **ortam değişkenlerini** proxy sunucu bağlantılarını yapılandırmak için kullanabileceğiniz bir bölüm. 

IOT Edge hub'ı modülleri ve IOT Edge Aracısı'nı yapılandırmak için seçin **Gelişmiş Edge çalışma zamanı ayarları Yapılandır** Sihirbazı'nın ilk adımında. 

![Gelişmiş Edge Çalışma Zamanı ayarlarını yapılandırma](./media/how-to-configure-proxy-support/configure-runtime.png)

Ekleme **erişmek** ortam değişkeni IOT Edge aracısı ve IOT Edge hub'ı modül tanımları. Dahil ettiyseniz **UpstreamProtocol** IOT Edge Cihazınızda config.yaml dosyasında ortam değişkeni ekleyin, IOT Edge Aracısı modülü tanımına çok. 

![Erişmek ortam değişken Ayarla](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

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

Dahil ettiyseniz **UpstreamProtocol** IOT Edge Cihazınızda confige.yaml dosyasında ortam değişkeni ekleyin, IOT Edge Aracısı modülü tanımına çok. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Rolleri hakkında daha fazla bilgi [IOT Edge çalışma zamanı](iot-edge-runtime.md).

Yükleme ve yapılandırma hataları ile ilgili sorunları giderme [genel sorunlar ve çözümleri için Azure IOT Edge](troubleshoot.md)

