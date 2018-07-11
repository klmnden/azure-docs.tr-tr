---
title: Windows'da Azure IoT Edge benzetimi | Microsoft Docs
description: Azure IoT Edge çalışma zamanını Windows'da benzetimli bir cihaza yükleme ve ilk modülünüzü dağıtma
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 06/08/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5bde54a65160c58d8bfba2f6c4c3b6a4317e46ed
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436451"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Hızlı Başlangıç: İlk IoT Edge modülünüzü Azure portalından bir Windows cihaza dağıtma - önizleme

Azure IoT Edge, tüm verilerinizi buluta göndermek zorunda kalmak yerine analiz ve veri işlemeyi cihazlarınızda gerçekleştirmenizi sağlar. IoT Edge öğreticileri farklı tür modülleri dağıtmayı gösterir, ancak önce test etmek için bir cihaz gerekir. 

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. IoT Hub'ı oluşturma
2. IoT Edge cihazı kaydetme
3. IoT Edge çalışma zamanını başlatma
4. Modül dağıtma

![Öğretici mimarisi][2]

Bu hızlı başlangıçta oluşturduğunuz modül; sıcaklık, nem ve basınç verileri üreten bir sensör simülasyonudur. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için simülasyon verilerini analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır. 

>[!NOTE]
>Windows üzerinde IoT Edge çalışma zamanı [genel önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) sürümündedir.

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][lnk-account] oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta, bir IoT cihazının benzetimi için Windows çalıştıran bir bilgisayar veya bir sanal makine kullanmakta olduğunuz varsayılır. Windows’u sanal bir makinede çalıştırıyorsanız, [iç içe sanallaştırmayı][lnk-nested] etkinleştirin ve en az 2 GB bellek ayırın. 

IoT Edge cihazı için kullandığınız makinede aşağıdaki önkoşulların hazır olduğundan emin olun:

1. Desteklenen bir Windows sürümü kullandığınızdan emin olun:
   * Windows 10 veya daha yenisi
   * Windows Server 2016 veya daha yenisi
2. [Docker for Windows][lnk-docker] yükleyin ve çalıştığından emin olun.
3. Docker'ı [Linux kapsayıcılarını](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) kullanacak şekilde yapılandırma

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Hızlı başlangıç adımlarına başlamak için Azure portalında IoT Hub'ınızı oluşturun.
![IoT Hub'ını oluşturma][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT Hub'ına bir IoT Edge cihazı kaydedin.
![Cihaz kaydetme][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yükleme ve başlatma

Azure IoT Edge çalışma zamanını IoT Edge cihazınıza yükleyin ve başlatın. 
![Cihaz kaydetme][5]

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik daemon'u** bir Edge cihazı her başladığında çalışır ve IoT Edge aracısını çalıştırarak cihazı önyükler. **IoT Edge aracısı**, IoT Edge hub'ı dahil olmak üzere IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir. 

>[!NOTE]
>Yükleme betiği geliştirme aşamasında olduğundan bu bölümdeki yükleme adımları şimdilik el ile gerçekleştirilmektedir. 

Bu bölümdeki yönergeler, IoT Edge çalışma zamanını Linux kapsayıcılarla yapılandırmaktadır. Windows kapsayıcıları kullanmak istiyorsanız bkz. [Azure IoT Edge çalışma zamanını Windows kapsayıcılarla kullanmak üzere Windows'a yükleme](how-to-install-iot-edge-windows-with-windows.md).

### <a name="download-and-install-the-iot-edge-service"></a>IoT Edge hizmetini indirme ve yükleme

1. IoT Edge cihazınızda PowerShell'i yönetici olarak çalıştırın.

2. IoT Edge hizmet paketini indirin.

  ```powershell
  Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
  Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
  Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
  rmdir C:\ProgramData\iotedge\iotedged-windows
  $env:Path += ";C:\ProgramData\iotedge"
  SETX /M PATH "$env:Path"
  ```

3. vcruntime bileşenini yükleyin.

  ```powershell
  Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
  .\vc_redist.exe /quiet /norestart
  ```

4. IoT Edge hizmetini oluşturun ve başlatın.

   ```powershell
   New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
   Start-Service iotedge
   ```

5. IoT Edge hizmetinin kullandığı bağlantı noktaları için güvenlik duvarı özel durumları ekleyin.

   ```powershell
   New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
   ```

6. **iotedge.reg** adlı yeni bir dosya oluşturun ve metin düzenleyiciyle açın. 

7. Aşağıdaki içeriği ekleyip dosyayı kaydedin. 

   ```input
   Windows Registry Editor Version 5.00
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
   "CustomSource"=dword:00000001
   "EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
   "TypesSupported"=dword:00000007
   ```

8. Dosya Gezgini'nde kaydettiğiniz dosyayı bulun ve çift tıklayarak Windows Kayıt Defteri'ne ekleyin. 

### <a name="configure-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yapılandırma 

Çalışma zamanını yeni bir cihaz kaydettiğinizde kopyaladığınız IoT Edge cihazı bağlantı dizenizle yapılandırın. Ardından çalışma zamanı ağını yapılandırın. 

1. `C:\ProgramData\iotedge\config.yaml` konumunda bulunan IoT Edge yapılandırma dosyasını açın. Dosya korumalı olduğundan önce Not Defteri gibi bir metin düzenleyiciyi yönetici olarak açıp ardından dosyayı açın. 

2. **Provisioning** (Sağlama) bölümünü bulun ve **device_connection_string** yerine IoT Edge cihazınızın ayrıntılarından kopyaladığınız dizeyi yazın. 

3. Yönetici PowerShell pencerenizde IoT Edge cihazınızın ana bilgisayar adını alın ve çıktıyı kopyalayın. 

   ```powershell
   hostname
   ```

4. Yapılandırma dosyasında **Edge device hostname** (Edge cihazı ana bilgisayar adı) bölümünü bulun. **hostname** değerini PowerShell'den kopyaladığınız değerle güncelleştirin.

3. Yönetici PowerShell pencerenizde IoT Edge cihazınızın IP adresini alın. 

   ```powershell
   ipconfig
   ```

4. Çıktının **vEthernet (DockerNAT)** bölümündeki **IPv4 Address** değerini kopyalayın. 

5. **IOTEDGE_HOST** adlı bir ortam değişkeni oluşturun, *\<ip_address\>* yerine IoT Edge cihazınızın IP Adresini yazın. 

  ```powershell
  [Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<ip_address>:15580")
  ```

  Ortam değişkenini yeniden başlatma işlemlerinden sonra kalıcı hale getirin.

  ```powershell
  SETX /M IOTEDGE_HOST "http://<ip_address>:15580"
  ```

6. `config.yaml` dosyasında **Connect settings** (Bağlantı ayarları) bölümünü bulun. **management_uri** ve **workload_uri** değerlerini IP adresiniz ve bir önceki bölümde açtığınız bağlantı noktalarıyla değiştirin. **\<GATEWAY_ADDRESS\>** yerine IP adresinizi yazın. 

   ```yaml
   connect: 
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

7. **Listen settings** (Dinleme ayarları) bölümünü bulun ve **management_uri** ile **workload_uri** için de aynı değerleri kullanın. 

   ```yaml
   listen:
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

8. **Moby Container Runtime settings** (Moby Kapsayıcısı Çalışma Zamanı ayarları) bölümünü bulun ve **network** değerinin `nat` olarak ayarlandığından emin olun.

9. Yapılandırma dosyasını kaydedin. 

10. PowerShell'de IoT Edge hizmetini yeniden başlatın.

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

### <a name="view-the-iot-edge-runtime-status"></a>IoT Edge çalışma zamanı durumunu görüntüleme

Çalışma zamanının başarıyla yüklendiğinden ve yapılandırıldığından emin olun.

1. IoT Edge hizmetinin durumunu kontrol edin.

   ```powershell
   Get-Service iotedge
   ```

2. Hizmetle ilgili sorunları gidermeniz gerekirse hizmet günlüklerini alın. 

   ```powershell
   # Displays logs from today, newest at the bottom.

   Get-WinEvent -ea SilentlyContinue `
    -FilterHashtable @{ProviderName= "iotedged";
      LogName = "application"; StartTime = [datetime]::Today} |
    select TimeCreated, Message |
    sort-object @{Expression="TimeCreated";Descending=$false}
   ```

3. IoT Edge cihazınızda çalışan tüm modülleri görüntüleyin. Hizmet ilk kez başlatıldığı için yalnızca **edgeAgent** modülünün çalıştığını göreceksiniz. edgeAgent modülü varsayılan olarak çalışır ve cihazınıza dağıtmak istediğiniz ek modülleri yüklemenize ve başlatmanıza yardımcı olur. 

   ```powershell
   iotedge list
   ```

   ![Cihazınızda bir modülü görüntüleme](./media/quickstart/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Modül dağıtma

Azure IoT Edge cihazınızı, IoT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Cihaz kaydetme][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, cihazda bir değişiklik yapmak zorunda kalmadan çalışacak bir IoT Edge modülünü göndermek için Azure portalını kullandınız. Bu örnekte gönderdiğiniz modül öğreticiler için kullanabileceğiniz ortam verilerini oluşturmaktadır. 

Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun. 

```powershell
iotedge list
```

   ![Cihazınızda üç modül görüntüleme](./media/quickstart/iotedge-list-2.png)

tempSensor modülünden buluta gönderilen iletileri görüntüleyin. 

```powershell
iotedge logs tempSensor -f
```

  ![Verileri modülünüzden görüntüleme](./media/quickstart/iotedge-logs.png)

[IoT Hub gezginini][lnk-iothub-explorer] veya [Visual Studio Code için Azure IoT Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) kullanarak IoT Hub'ınıza gönderilen iletileri de görüntüleyebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta yapılandırdığınız simülasyon cihazını IoT Edge öğreticilerini test etmek için kullanabilirsiniz. tempSensor modülünün IoT hub'ınıza veri göndermesini durdurmak isterseniz aşağıdaki komutu kullanarak IoT Edge hizmetini durdurun ve cihazınızda oluşturulmuş olan kapsayıcıları silin. Makinenizi tekrar bir IoT Edge cihazı olarak kullanmak istediğinizde hizmeti başlatmayı unutmayın. 

   ```powershell
   Stop-Service iotedge -NoWait
   docker rm -f $(docker ps -aq)
   ```

Oluşturduğunuz IoT Hub’a artık ihtiyacınız olmadığında, kaynağı ve kaynakla ilişkilendirilmiş cihazları kaldırmak için Azure portalını kullanabilirsiniz. IoT hub'ınızın genel bakış sayfasına gidip **Sil**'i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir IoT Edge cihazı oluşturdunuz ve cihaza kod dağıtmak için Azure IoT Edge bulut arabirimini kullandınız. Artık ortamı hakkında ham veri üreten bir test cihazınız var. 

Azure IoT Edge'in bu verileri Edge'de iş içgörüsüne dönüştürmenize nasıl yardımcı olabileceğini öğrenmek için diğer öğreticilere devam etmeye hazırsınız.

> [!div class="nextstepaction"]
> [Bir Azure İşlevi kullanarak sensör verilerini filtreleme](tutorial-deploy-function.md)

<!-- Images -->
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md
[lnk-account]: https://azure.microsoft.com/free
