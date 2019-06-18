---
title: Sorun giderme - Azure IOT Edge | Microsoft Docs
description: Bu makalede standart tanılama yetenekleri için Azure IOT Edge, bileşen durumu ve günlükleri alma gibi öğrenin ve yaygın sorunları çözme
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 00147002317f15345f01c88e81973837d16e6669
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65797618"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Azure IoT Edge için genel sorunlar ve çözümler

Ortamınızda Azure IoT Edge’i kullanma konusunda sorun yaşarsanız, sorun giderme ve çözümleme için kılavuz olarak bu makaleden yararlanın.

## <a name="run-the-iotedge-check-command"></a>'Kontrol Et komutu' iotedge çalıştırın

IOT Edge sorunlarını giderirken, ilk adımınız kullanılacak olmalıdır `check` koleksiyonu testleri yapılandırma ve bağlantıyla ilgili yaygın sorunları gerçekleştiren komutu. `check` Komutu kullanılabilir [1.0.7 yayın](https://github.com/Azure/azure-iotedge/releases/tag/1.0.7) ve daha sonra.

Çalıştırabileceğiniz `check` gibi komutunu ya da dahil `--help` bayrağı seçeneklerinin tam bir listesi görmek için:

* Linux üzerinde:

  ```bash
  sudo iotedge check
  ```

* Windows'da:

  ```powershell
  iotedge check
  ```

Tür denetimler tarafından aracı olarak sınıflandırılabilir:

* Yapılandırması şunları denetler: Uç cihazlarına bulut ile ilgili sorunlar da dahil olmak üzere, bağlanmasını engelleyebilir ayrıntıları inceler *config.yaml* ve kapsayıcı altyapısı.
* Bağlantıyı denetler: IOT Edge çalışma zamanı konak cihaz üzerindeki bağlantı noktalarına erişebilen ve tüm IOT Edge bileşenleri, IOT Hub'ına bağlanabilir doğrular.
* Üretim Hazırlık Denetimleri: Cihaz sertifika yetkilisi (CA) sertifikaları ve modül günlük dosyası yapılandırmasını durumu gibi önerilen üretim en iyi yöntemler arar.

Tanılama denetimleri tam bir listesi için bkz. [yerleşik işlevsellik sorunlarını giderme](https://github.com/Azure/iotedge/blob/master/doc/troubleshoot-checks.md).

## <a name="standard-diagnostic-steps"></a>Standart tanılama adımları

Bir sorunla karşılaşırsanız, kapsayıcı günlükleri ve iletileri için ve CİHAZDAN gözden geçirerek IOT Edge cihazınızın durumu hakkında daha fazla öğrenebilirsiniz. Bilgi toplamak için bu bölümdeki komutları ve araçları kullanın.

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>IOT Edge Güvenlik Yöneticisi'ni ve onun günlüklerini durumunu denetleyin

Linux üzerinde:
- IOT Edge Güvenlik Yöneticisi'nin durumunu görüntülemek için:

   ```bash
   sudo systemctl status iotedge
   ```

- IOT Edge Güvenlik Yöneticisi'nin günlükleri görüntülemek için:

    ```bash
    sudo journalctl -u iotedge -f
    ```

- Ayrıntılı günlükleri, IOT Edge Güvenlik Yöneticisi'ni daha fazla bilgi görüntülemek için:

   - İotedge arka plan programı ayarları düzenleyin:

      ```bash
      sudo systemctl edit iotedge.service
      ```
   
   - Aşağıdaki satırları güncelleştirin:
    
      ```
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```
    
   - IOT Edge güvenlik Daemon'unu yeniden başlatın:
    
      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

Windows'da:
- IOT Edge Güvenlik Yöneticisi'nin durumunu görüntülemek için:

   ```powershell
   Get-Service iotedge
   ```

- IOT Edge Güvenlik Yöneticisi'nin günlükleri görüntülemek için:

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>IOT Edge Güvenlik Yöneticisi çalışmıyor, yaml yapılandırma dosyanızı doğrulayın.

> [!WARNING]
> YAML dosyaları sekmeler girinti içeremez. Bunun yerine 2 alanları kullanın.

Linux üzerinde:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Windows'da:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Kapsayıcı günlüklerini sorunlar için denetleyin

IOT Edge güvenlik arka plan programı çalışır duruma geçtikten sonra sorunları algılamak için kapsayıcılarının günlüklerine bakın. Dağıttığınız kapsayıcılarla başlayın ve IOT Edge çalışma zamanını oluşturan kapsayıcılara bakın: edgeAgent ve edgeHub. IOT Edge Aracısı günlükleri genellikle her bir kapsayıcının yaşam döngüsü hakkında bilgi de sağlar. IOT Edge hub'ı günlükleri, Mesajlaşma ve yönlendirme hakkında bilgi sağlar. 

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-iot-edge-hub"></a>IOT Edge hub'ı aracılığıyla giden iletileri görüntüleyin

IOT Edge hub'ı aracılığıyla giden iletileri görüntüleyin ve çalışma zamanı kapsayıcılarındaki ayrıntılı günlükleri Öngörüler toplayın. Bu kapsayıcıların ayrıntılı günlüklerini etkinleştirmek için ayarlanmış `RuntimeLogLevel` yaml yapılandırma dosyanızdaki. Dosyayı açmak için:

Linux üzerinde:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Windows'da:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

Varsayılan olarak, `agent` öğesi, aşağıdaki örnekteki gibi görünür:

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.0
       auth: {}
   ```

Değiştirin `env: {}` ile:

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

   > [!WARNING]
   > YAML dosyaları sekmeler identation içeremez. Bunun yerine 2 alanları kullanın.

Dosyayı kaydedin ve IOT Edge Güvenlik Yöneticisi'ni yeniden başlatın.

IoT Hub ile IoT Edge cihazları arasında gönderilmekte olan iletileri de denetleyebilirsiniz. Kullanarak bu iletileri görüntüleyin [Azure IOT hub'ı Araç Seti](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) Visual Studio Code uzantısı (eski adıyla Azure IOT Toolkit uzantısını). Daha fazla bilgi için [Azure IOT ile geliştirme sürecinde kullanışlı araç](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

### <a name="restart-containers"></a>Kapsayıcılar'ı yeniden başlatın
Bilgi iletilerini ve günlükleri araştırdıktan sonra kapsayıcıları yeniden başlatmayı deneyebilirsiniz:

```
iotedge restart <container name>
```

IOT Edge çalışma zamanı kapsayıcılarındaki yeniden başlatın:

```
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>IOT Edge Güvenlik Yöneticisi'ni yeniden başlatın

Sorunu hala kalıcı yapma, IOT Edge Güvenlik Yöneticisi'ni yeniden başlatmayı deneyebilirsiniz.

Linux üzerinde:

   ```cmd
   sudo systemctl restart iotedge
   ```

Windows'da:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

## <a name="iot-edge-agent-stops-after-about-a-minute"></a>IOT Edge Aracısı yaklaşık bir dakika sonra durdurulur

EdgeAgent modülü başlar ve başarıyla için yaklaşık bir dakika çalıştırılır ve sonra durdurulur. Günlükleri, IOT Edge Aracısı AMQP üzerinden IOT Hub'ına bağlanmayı dener ve sonra WebSocket üzerinden AMQP kullanarak bağlanma girişiminde gösterir. Başarısız olduğunda, IOT Edge Aracısı çıkış yapar. 

Örnek edgeAgent günlükleri:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Kök neden
Konak ağı üzerindeki bir ağ yapılandırması, IOT Edge Aracısı ağa ulaşmasını engelliyordur. Aracı ilk olarak AMQP (5671 numaralı bağlantı noktası) üzerinden bağlanma girişiminde bulunur. Bağlantı başarısız olursa Websockets'i (443 numaralı bağlantı noktası) dener.

IoT Edge çalışma zamanı, her bir modül için iletişim kurulacak bir ağ ayarlar. Linux’ta bu ağ bir köprü ağıdır. Windows’da NAT kullanır. Bu sorun, NAT ağını kullanan Windows kapsayıcılarının kullanıldığı Windows cihazlarında daha yaygın olarak görülür. 

### <a name="resolution"></a>Çözüm
Bu köprüye/NAT ağına atanan IP adresleri için bir İnternet rotası olduğundan emin olun. Bazen konaktaki VPN yapılandırması, IoT Edge ağını geçersiz kılar. 

## <a name="iot-edge-hub-fails-to-start"></a>IOT Edge hub'u başlatılamıyor

EdgeHub modülü başarısız başlatılamıyor ve günlüklere şu iletiyi yazıyor: 

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Kök neden
Konak makinedeki başka bir işlem, 443 numaralı bağlantı noktasına bağlıdır. IOT Edge hub'ı bağlantı noktası 5671 eşler ve 443 üzerinden ağ geçidi senaryolarında kullanmak. Başka bir işlem zaten bu bağlantı noktasına bağlıysa bu bağlantı noktası eşlemesi başarısız olur. 

### <a name="resolution"></a>Çözüm
443 numaralı bağlantı noktasını kullanan işlemi bulup durdurun. Bu işlem genellikle bir web sunucusudur.

## <a name="iot-edge-agent-cant-access-a-modules-image-403"></a>IOT Edge Aracısı (403) bir modülün görüntüsüne erişemiyor
Bir kapsayıcı çalışmazsa ve edgeAgent günlükleri 403 hatasını gösteriyor. 

### <a name="root-cause"></a>Kök neden
IOT Edge Aracısı bir modülün görüntüsüne erişme izinleri yoktur. 

### <a name="resolution"></a>Çözüm
Kayıt defteri kimlik bilgileriniz, dağıtım bildiriminde doğru belirtildiğinden emin olun

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>IOT Edge güvenlik arka plan programı, geçersiz bir ana bilgisayar adı ile başarısız oluyor

Komut `sudo journalctl -u iotedge` başarısız oluyor ve aşağıdaki iletiyi yazdırmaz: 

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

### <a name="root-cause"></a>Kök neden
IOT Edge çalışma zamanı yalnızca ana bilgisayar adları 64 karakterden kısa destekleyebilir. Fiziksel makineler genellikle uzun konak adı yok, ancak bir sanal makineye daha yaygın bir sorundur. Ana bilgisayar adları, Azure'da barındırılan Windows sanal makineleri için otomatik olarak oluşturulan, özellikle uzun olma eğilimindedir. 

### <a name="resolution"></a>Çözüm
Bu hatayı gördüğünüzde, sanal makinenin DNS adını yapılandırarak ve ardından Kurulum komutu, konak adı olarak DNS adını ayarlar çözebilirsiniz.

1. Azure portalında, sanal makinenizin Genel Bakış sayfasına gidin. 
2. Seçin **yapılandırma** DNS adı altında. Sanal makineniz yapılandırılan DNS adı zaten varsa, yeni bir yapılandırma gerekmez. 

   ![Sanal makinenin DNS adını yapılandırma](./media/troubleshoot/configure-dns.png)

3. İçin bir değer girin **DNS ad etiketi** seçip **Kaydet**.
4. Biçiminde olmalıdır yeni bir DNS adı kopyalamanız  **\<DNSnamelabel\>.\< vmlocation\>. cloudapp.azure.com**.
5. Sanal makinenin içinde IOT Edge çalışma zamanı, DNS adı ile ayarlamak için aşağıdaki komutu kullanın:

   - Linux üzerinde:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   - Windows'da:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="stability-issues-on-resource-constrained-devices"></a>Kaynaktaki kararlılık sorunlarını cihazları kısıtlı 
Özellikle, ağ geçidi olarak kullanıldığında Raspberry Pi gibi kısıtlı cihazlarda kararlılık sorunlarla karşılaşabilirsiniz. Edge hub'ı modülü bellek durumlar dışında belirtileri içerir, aşağı akış cihazları bağlanılamıyor veya cihazın telemetri gönderme birkaç saat sonra durdurur.

### <a name="root-cause"></a>Kök neden
IOT Edge hub, IOT Edge çalışma zamanı bir parçası olan varsayılan olarak performans için iyileştirilmiştir ve büyük boyutta bellek ayırmaya çalışır. Bu iyileştirme kısıtlanmış edge cihazlar için ideal değildir ve kararlılık sorunlara neden olabilir.

### <a name="resolution"></a>Çözüm
IOT Edge hub'ı için bir ortam değişkenini ayarlamak **OptimizeForPerformance** için **false**. Bunu yapmanın iki yolu vardır:

Kullanıcı Arabiriminde: 

Portalda gidin **cihaz ayrıntıları** > **modülleri ayarlama** > **Gelişmiş Edge çalışma zamanı ayarları Yapılandır**. Adlı Edge hub'ı modül için bir ortam değişkenini oluşturmak *OptimizeForPerformance* ayarlanmış *false*.

![OptimizeForPerformance false olarak ayarlayın.](./media/troubleshoot/optimizeforperformance-false.png)

**VEYA**

Dağıtım bildirimi içinde:

```json
  "edgeHub": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
      "createOptions": <snipped>
    },
    "env": {
      "OptimizeForPerformance": {
          "value": "false"
      }
    },
```
## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>IOT Edge üzerinde Windows arka plan programının günlüklerini alınamıyor
Kullanırken bir EventLogException alırsanız `Get-WinEvent` Windows üzerinde kayıt defteri girişlerini denetleyin.

### <a name="root-cause"></a>Kök neden
`Get-WinEvent` PowerShell komutu tarafından belirli bir günlükleri bulmak için mevcut olması için bir kayıt defteri girişi dayanır `ProviderName`.

### <a name="resolution"></a>Çözüm
IOT Edge arka plan programı bir kayıt defteri anahtarını ayarlayın. Oluşturma bir **iotedge.reg** dosya aşağıdaki içeriğe ve alma Windows kayıt defterine çift veya kullanarak `reg import iotedge.reg` komutu:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="iot-edge-module-fails-to-send-a-message-to-the-edgehub-with-404-error"></a>IOT Edge modülü ileti göndermek için edgeHub 404 hatası ile başarısız olur.

IOT Edge modülü başarısız bir 404 ile edgeHub bir ileti göndermek için özel bir `Module not found` hata. IOT Edge arka plan programı günlüklere şu iletiyi yazdırmaz: 

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 ) 
```

### <a name="root-cause"></a>Kök neden
IOT Edge arka plan programı, güvenlik nedenleriyle edgeHub bağlanan tüm modüller için işlem kimliği zorlar. Bir modül tarafından gönderilen tüm iletiler modülü ana işlem Kimliğinden geldiğini doğrular. Başlangıçta kurulan olandan farklı bir işlem Kimliğinden modülü tarafından ileti gönderiliyor ise bir 404 hatası ileti iletinin reddeder.

### <a name="resolution"></a>Çözüm
Aynı işlem kimliği her zaman özel bir IOT Edge modülü tarafından ileti göndermek için edgeHub için kullanılmadığından emin olun. Örneği için emin olun `ENTRYPOINT` yerine `CMD` Docker dosyanızda beri komutu `CMD` bir işlem, modül kimliği ve başka bir işlem kimliği için ise ana program çalışırken bash komut önünü açacak `ENTRYPOINT` önünü açacak bir tek bir işlem kimliği.


## <a name="firewall-and-port-configuration-rules-for-iot-edge-deployment"></a>IOT Edge dağıtımı için güvenlik duvarı ve bağlantı noktası yapılandırma kuralları
Azure IOT Edge, IOT hub'ı Desteklenen protokoller kullanarak Azure bulut bir şirket içi sunucusundan iletişim sağlar, bkz: [iletişim protokolü seçme](../iot-hub/iot-hub-devguide-protocols.md). Gelişmiş güvenlik için Azure IOT Edge ile Azure IOT Hub arasındaki iletişim kanallarını her zaman giden olacak şekilde yapılandırılır. Bu yapılandırma dayanır [hizmet destekli iletişim düzeni](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/), keşfetmek kötü amaçlı bir varlık için saldırı yüzeyini en aza. Gelen iletişim istekleri, yalnızca Azure IOT Edge cihazına iletileri göndermek için Azure IOT hub'ı gereken yere belirli senaryolar için gereklidir. Bulut-cihaz iletilerini TLS güvenli kanalı kullanılarak korunur ve daha fazla X.509 sertifikaları ve TPM cihaz modülleri kullanılarak güvenli hale getirilebilir. Azure IOT Edge Güvenlik Yöneticisi bu iletişimin nasıl olabileceğini yöneten kurulduktan bkz [IOT Edge Güvenlik Yöneticisi](../iot-edge/iot-edge-security-manager.md).

IOT Edge modülleri dağıtılır ve Azure IOT Edge çalışma zamanı güvenliğini sağlamak için Gelişmiş yapılandırma sağlar, ancak temel alınan makine ve ağ yapılandırmasına yine de bağlıdır. Bu nedenle, uygun bir ağ sağlamak zorunludur ve güvenlik duvarı kuralları iletişim buluta güvenli edge için ayarlanır. Temel alınan sunucuları için yapılandırma güvenlik duvarı kuralları, Azure IOT Edge çalışma zamanı barındırıldığı bir kılavuz olarak aşağıdaki tabloda kullanılabilir:

|Protocol|Bağlantı noktası|gelen|Giden|Rehber|
|--|--|--|--|--|
|MQTT|8883|ENGELLENEN (varsayılan)|ENGELLENEN (varsayılan)|<ul> <li>Giden (giden) MQTT iletişim protokolü olarak kullanılırken açık olacak şekilde yapılandırın.<li>MQTT için 1883 IOT Edge tarafından desteklenmiyor. <li>Gelen (gelen) bağlantıları engellenmesi gerekir.</ul>|
|AMQP|5671|ENGELLENEN (varsayılan)|Açık (varsayılan)|<ul> <li>IOT Edge için varsayılan iletişim protokolü. <li> Azure IOT Edge, diğer Desteklenen protokoller için yapılandırılmadı veya AMQP istenen iletişim protokolü açık olarak yapılandırılmış olmalıdır.<li>AMQP için 5672, IOT Edge tarafından desteklenmiyor.<li>Azure IOT Edge kullanan farklı bir IOT Hub protokol desteklendiğinde, bu bağlantı noktası engelleyin.<li>Gelen (gelen) bağlantıları engellenmesi gerekir.</ul></ul>|
|HTTPS|443|ENGELLENEN (varsayılan)|Açık (varsayılan)|<ul> <li>Giden (giden) açık olmasını sağlama IOT Edge için 443 numaralı yapılandırın. Bu yapılandırma, el ile komut dosyaları veya Azure IOT cihaz sağlama hizmeti (DPS) kullanılırken gereklidir. <li>Gelen (gelen) bağlantının açık olmalıdır yalnızca belirli senaryoları için: <ul> <li>  Yöntem isteği gönderebilir yaprak cihazlar ile saydam bir ağ geçidi varsa. Bu durumda, bağlantı noktası 443'ü dış ağlara bağlanmak için IoTHub ya da Azure IOT Edge üzerinden IoTHub hizmetleri sağlamak için açık olması gerekmez. Bu nedenle gelen kuralı yalnızca iç ağdan gelen (gelen) açmak için kısıtlı olabilir. <li> Cihaz (C2D) senaryoları için daha fazla istemci için.</ul><li>HTTP için 80, IOT Edge tarafından desteklenmiyor.<li>HTTP olmayan protokolleri (örneğin, AMQP veya MQTT), Kurumsal yapılandırılamıyorsa; iletileri WebSockets üzerinden gönderilebilir. 443 numaralı bağlantı noktası için iletişim WebSocket durumlarda kullanılır.</ul>|

## <a name="edge-agent-module-continually-reports-empty-config-file-and-no-modules-start-on-the-device"></a>Cihazda boş yapılandırma raporları 'file' ve hiçbir modül sürekli olarak Edge Aracısı modülü Başlat

Cihaz dağıtımda tanımlı modülleri başlangıç sorun vardır. Yalnızca edgeAgent çalıştırıyor, ancak sürekli olarak '... boş yapılandırma dosyası' raporlama.

### <a name="potential-root-cause"></a>Olası kök nedeni
Varsayılan olarak, IOT Edge modülleri, kendi yalıtılmış kapsayıcıyı ağında başlatır. Cihaz bu özel ağda DNS ad çözümlemesi ile ilgili sorun yaşıyor olabilirsiniz.

### <a name="resolution"></a>Çözüm

**1. seçenek: DNS sunucusunu kapsayıcısında altyapısı ayarları ayarlayın.**

Ortamınız için DNS sunucusu altyapısı tarafından başlatılan tüm kapsayıcı modülleri uygulanacak kapsayıcı altyapısı ayarları belirtin. Adlı bir dosya oluşturun `daemon.json` kullanmak için DNS sunucusu belirtme. Örneğin:

```
{
    "dns": ["1.1.1.1"]
}
```

Yukarıdaki örnekte, bir ortak olarak erişilebilen DNS hizmeti için DNS sunucusu ayarlar. Sınır cihazı, bu IP, ortamından erişemiyorsanız erişilebilen DNS sunucusu adresi ile değiştirin.

Bir yerde `daemon.json` platformunuz için doğru konumda: 

| Platform | Location |
| --------- | -------- |
| Linux | `/etc/docker` |
| Windows kapsayıcıları ile Windows konağı | `C:\ProgramData\iotedge-moby\config` |

Konum zaten varsa `daemon.json` ekleyin **dns** için anahtar ve dosyayı kaydedin.

*Güncelleştirmelerin etkili olabilmesi kapsayıcı altyapısını yeniden başlatma*

| Platform | Komut |
| --------- | -------- |
| Linux | `sudo systemctl restart docker` |
| Windows (yönetici Powershell) | `Restart-Service iotedge-moby -Force` |

**2. seçenek: IOT Edge dağıtımındaki modülün başına DNS sunucusunu ayarlayın**

DNS sunucusu için her modülün ayarlayabilirsiniz *createOptions* IOT Edge dağıtımındaki. Örneğin:

```
"createOptions": {
  "HostConfig": {
    "Dns": [
      "x.x.x.x"
    ]
  }
}
```

Bunun için ayarladığınızdan emin olun *edgeAgent* ve *edgeHub* modüller de. 

## <a name="next-steps"></a>Sonraki adımlar
IoT Edge platformunda bir hata bulduğunuzu düşünüyor musunuz? [Sorun bildir](https://github.com/Azure/iotedge/issues) böylece biz geliştirmeye devam. 

Başka sorularım varsa, oluşturun bir [destek isteği](https://portal.azure.com/#create/Microsoft.Support) Yardım. 

