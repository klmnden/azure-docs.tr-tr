---
title: Azure IoT Edge sorunlarını giderme | Microsoft Docs
description: Azure IoT Edge için genel sorunları çözümleme ve sorun giderme becerilerini öğrenme
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9ec396e8a1ad36e85e1291995345ca1de24668d0
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128069"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Azure IoT Edge için genel sorunlar ve çözümler

Ortamınızda Azure IoT Edge’i kullanma konusunda sorun yaşarsanız, sorun giderme ve çözümleme için kılavuz olarak bu makaleden yararlanın. 

## <a name="standard-diagnostic-steps"></a>Standart tanılama adımları 

Bir sorunla karşılaştığınızda, cihaza gelen ve cihazdan giden iletileri ve kapsayıcı günlüklerini gözden geçirerek IoT Edge cihazınızın durumu hakkında daha fazla bilgi edinin. Bilgi toplamak için bu bölümdeki komutları ve araçları kullanın. 

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>IOT kenar Güvenlik Yöneticisi'ni ve günlüklerinin durumunu kontrol edin:

Linux üzerinde:
- IOT kenar Güvenlik Yöneticisi'nin durumunu görüntülemek için:

   ```bash
   sudo systemctl status iotedge
   ```

- IOT kenar Güvenlik Yöneticisi'nin günlükleri görüntülemek için:

    ```bash
    sudo journalctl -u iotedge -f
    ```

- Ayrıntılı günlükleri IOT kenar Güvenlik Yöneticisi'nin görüntülemek için:

   - İotedge arka plan programı ayarlarını düzenleyin:

      ```bash
      sudo systemctl edit iotedge.service
      ```
   
   - Aşağıdaki satırları güncelleştirin:
    
      ```
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```
    
   - IOT kenar güvenlik arka plan programı yeniden başlatın:
    
      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

Windows'da:
- IOT kenar Güvenlik Yöneticisi'nin durumunu görüntülemek için:

   ```powershell
   Get-Service iotedge
   ```

- IOT kenar Güvenlik Yöneticisi'nin günlükleri görüntülemek için:

   ```powershell
   # Displays logs from today, newest at the bottom.
 
   Get-WinEvent -ea SilentlyContinue `
   -FilterHashtable @{ProviderName= "iotedged";
     LogName = "application"; StartTime = [datetime]::Today} |
   select TimeCreated, Message |
   sort-object @{Expression="TimeCreated";Descending=$false}
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>IOT kenar Güvenlik Yöneticisi çalışmıyor, yaml yapılandırma dosyanızı doğrulayın.

> [!WARNING]
> YAML dosyaları sekmeleri identation içeremez. 2 alanları kullanın.

Linux üzerinde:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Windows'da:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Sorunları için kapsayıcı günlükleri denetleyin

IOT kenar güvenlik arka plan programı çalışıyor sonra sorunlarını algılayacak şekilde kapsayıcıların günlüklerine bakın. Dağıttığınız kapsayıcılarla başlayın, sonra IoT Edge çalışma zamanını oluşturan şu kapsayıcılara bakın: Edge Aracısı ve Edge Hub’ı. Edge Aracısı günlükleri genellikle her bir kapsayıcının yaşam döngüsü hakkında bilgi sağlar. Edge Hub’ı günlükleri, mesajlaşma ve yönlendirme hakkında bilgi sağlar. 

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-edge-hub"></a>Edge hub'ı aracılığıyla giden iletiler görüntüleyin

Edge hub'ı aracılığıyla giden iletiler görüntüleyin ve edgeAgent ve edgeHub çalışma zamanı kapsayıcılardan cihaz özellikleri güncelleştirmeleri ayrıntılı günlükleri ile ilişkin Öngörüler toplayabilir. Bu kapsayıcılara ayrıntılı günlükleri etkinleştirmek için ayarlanmış `RuntimeLogLevel` ortam değişkeni: 

Linux üzerinde:
    
   ```cmd
   export RuntimeLogLevel="debug"
   ```
    
Windows'da:
    
   ```powershell
   [Environment]::SetEnvironmentVariable("RuntimeLogLevel", "debug")
   ```

IoT Hub ile IoT Edge cihazları arasında gönderilmekte olan iletileri de denetleyebilirsiniz. Visual Studio Code için [Azure IoT Araç Seti](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) uzantısını kullanarak bu iletileri görüntüleyin. Daha fazla yardım için bkz. [Azure IoT ile geliştirme sürecinde kullanışlı araç](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

### <a name="restart-containers"></a>Kapsayıcıları yeniden başlatın
Bilgi iletilerini ve günlükleri araştırdıktan sonra kapsayıcıları yeniden başlatmayı deneyebilirsiniz:

```
iotedge restart <container name>
```

IOT kenar çalışma zamanı kapsayıcıları yeniden başlatın:

```
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>IOT kenar Güvenlik Yöneticisi'ni yeniden başlatın

Sorun hala kalıcı ise IOT kenar Güvenlik Yöneticisi'ni yeniden başlatmayı deneyebilirsiniz.

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

## <a name="edge-agent-stops-after-about-a-minute"></a>Edge Aracısı yaklaşık bir dakika sonra durdurulur

Edge Aracısı başlatılıp yaklaşık bir dakika çalıştırılır ve sonra durdurulur. Günlükler, Edge Aracısı’nın AMQP üzerinden IoT Hub’a bağlanmaya çalıştığını ve yaklaşık 30 saniye sonra websocket üzerinden AMQP kullanarak bağlanma girişiminde bulunduğunu belirtir. Bu başarısız olduğunda Edge Aracısı çıkış yapar. 

Örnek Edge Aracısı günlükleri:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Kök neden
Konak ağı üzerindeki bir ağ yapılandırması, Edge Aracısı’nın ağa ulaşmasını engelliyordur. Aracı ilk olarak AMQP (5671 numaralı bağlantı noktası) üzerinden bağlanma girişiminde bulunur. Bu başarısız olursa websockets’i (443 numaralı bağlantı noktası) dener.

IoT Edge çalışma zamanı, her bir modül için iletişim kurulacak bir ağ ayarlar. Linux’ta bu ağ bir köprü ağıdır. Windows’da NAT kullanır. Bu sorun, NAT ağını kullanan Windows kapsayıcılarının kullanıldığı Windows cihazlarında daha yaygın olarak görülür. 

### <a name="resolution"></a>Çözüm
Bu köprüye/NAT ağına atanan IP adresleri için bir İnternet rotası olduğundan emin olun. Bazen konaktaki VPN yapılandırması, IoT Edge ağını geçersiz kılar. 

## <a name="edge-hub-fails-to-start"></a>Edge Hub’u başlatılamıyor

Edge Hub’ı başlatılamıyor ve günlüklere şu iletiyi yazıyor: 

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Kök neden
Konak makinedeki başka bir işlem, 443 numaralı bağlantı noktasına bağlıdır. Edge Hub'ı, ağ geçidi senaryolarında kullanmak üzere 5671 ve 443 numaralı bağlantı noktalarını eşler. Başka bir işlem zaten bu bağlantı noktasına bağlıysa bu bağlantı noktası eşlemesi başarısız olur. 

### <a name="resolution"></a>Çözüm
443 numaralı bağlantı noktasını kullanan işlemi bulup durdurun. Bu işlem genellikle bir web sunucusudur.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>Edge Aracısı bir modülün görüntüsüne erişemiyor (403)
Bir kapsayıcı çalıştırılamıyor ve Edge Aracısı günlükleri, 403 hatasını gösteriyor. 

### <a name="root-cause"></a>Kök neden
Edge Aracısı'nın bir modülün görüntüsüne erişme izinleri yoktur. 

### <a name="resolution"></a>Çözüm
Kayıt defteri kimlik bilgilerinizi Dağıtım bildiriminizi doğru belirtildiğinden emin olun

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>IOT kenar güvenlik arka plan programı geçersiz bir ana bilgisayar adı ile başarısız oluyor

Komut `sudo journalctl -u iotedge` başarısız oluyor ve aşağıdaki iletiyi yazdırır: 

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

### <a name="root-cause"></a>Kök neden
IOT kenar çalışma zamanı yalnızca ana bilgisayar adları 64 karakterden kısa destekleyebilir. Bu genellikle fiziksel makineler için bir sorun değildir, ancak bir sanal makinede çalışma zamanı ayarlarken ortaya çıkabilir. Otomatik olarak oluşturulan ana bilgisayar adları, Azure'da barındırılan Windows sanal makineleri için özel olarak, uzun olma eğilimindedir. 

### <a name="resolution"></a>Çözüm
Bu hatayı gördüğünüzde, sanal makinenin DNS adı yapılandırarak ve Kurulum komut ana bilgisayar adı olarak DNS adı ayarlama çözebilirsiniz.

1. Azure portalında sanal makinenize Genel Bakış sayfasına gidin. 
2. Seçin **yapılandırma** DNS adı altında. Sanal makineniz zaten yapılandırılmış bir DNS adı varsa, yeni bir yapılandırma gerekmez. 

   ![DNS adı yapılandırma](./media/troubleshoot/configure-dns.png)

3. İçin bir değer girin **DNS ad etiketi** seçip **kaydetmek**.
4. Biçiminde olmalıdır yeni bir DNS adı kopyalamanız  **\<DNSnamelabel\>.\< vmlocation\>. cloudapp.azure.com**.
5. Sanal makinenin içinde IOT kenar çalışma zamanı, DNS adı ile ayarlamak için aşağıdaki komutu kullanın:

   - Linux üzerinde:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   - Windows'da:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="next-steps"></a>Sonraki adımlar
IoT Edge platformunda bir hata bulduğunuzu düşünüyor musunuz? Lütfen gelişmeye devam edebilmemiz için [bir sorun gönderin](https://github.com/Azure/iotedge/issues). 
