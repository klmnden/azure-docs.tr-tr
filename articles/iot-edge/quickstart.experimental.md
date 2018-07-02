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
ms.openlocfilehash: 5863a8edbb20b2b0c231834259f1bb7b0423a8f6
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37034025"
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
   Invoke-WebRequest https://conteng.blob.core.windows.net/iotedged/iotedge.zip -o .\iotedge.zip
   Expand-Archive .\iotedge.zip C:\ProgramData\iotedge -f
   $env:Path += ";C:\ProgramData\iotedge"
   SETX /M PATH "$env:Path"
   ```

3. IoT Edge hizmetini oluşturun ve başlatın.

   ```powershell
   New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
   Start-Service iotedge
   ```

4. IoT Edge hizmetinin kullandığı bağlantı noktaları için güvenlik duvarı özel durumları ekleyin.

   ```powershell
   New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
   ```

5. **iotedge.reg** adlı yeni bir dosya oluşturun ve metin düzenleyiciyle açın. 

6. Aşağıdaki içeriği ekleyip dosyayı kaydedin. 

   ```input
   Windows Registry Editor Version 5.00
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
   "CustomSource"=dword:00000001
   "EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
   "TypesSupported"=dword:00000007
   ```

7. Dosya Gezgini'nde kaydettiğiniz dosyayı bulun ve çift tıklayarak Windows Kayıt Defteri'ne ekleyin. 

### <a name="configure-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yapılandırma 

Çalışma zamanını yeni bir cihaz kaydettiğinizde kopyaladığınız IoT Edge cihazı bağlantı dizenizle yapılandırın. Ardından çalışma zamanı ağını yapılandırın. 

1. `C:\ProgramData\iotedge\config.yaml` konumunda bulunan IoT Edge yapılandırma dosyasını açın. Dosya korumalı olduğundan önce Not Defteri gibi bir metin düzenleyiciyi yönetici olarak açıp ardından dosyayı açın. 

2. **Provisioning** (Sağlama) bölümünü bulun ve **device_connection_string** yerine IoT Edge cihazınızın ayrıntılarından kopyaladığınız dizeyi yazın. 

3. Yönetici PowerShell pencerenizde IoT Edge cihazınızın ana bilgisayar adını alın ve çıktıyı kopyalayın. 

   ```powershell
   hostname
   ```

4. Yapılandırma dosyasında **Edge device hostname** (Edge cihazı ana bilgisayar adı) bölümünü bulun. **hostname** değerini PowerShell'den kopyaladığınız değerle güncelleştirin.

5. Yönetici PowerShell pencerenizde IoT Edge cihazınızın IP adresini alın. 

   ```powershell
   ipconfig
   ```

6. Çıktının **vEthernet (DockerNAT)** bölümündeki **IPv4 Address** değerini kopyalayın. 

7. **IOTEDGE_HOST** adlı bir ortam değişkeni oluşturun, *\<ip_address\>* yerine IoT Edge cihazınızın IP Adresini yazın. 

   ```powershell
   [Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<ip_address>:15580")
   ```

8. `config.yaml` dosyasında **Connect settings** (Bağlantı ayarları) bölümünü bulun. **management_uri** ve **workload_uri** içindeki **\<GATEWAY_ADDRESS\>** değerini IP adresiniz ve bir önceki bölümde açtığınız bağlantı noktalarıyla değiştirin. 

   ```yaml
   connect: 
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

9. **Listen settings** (Dinleme ayarları) bölümünü bulun ve **management_uri** ile **workload_uri** için de aynı değerleri kullanın. 

   ```yaml
   listen:
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

10. **Moby Container Runtime settings** (Moby Kapsayıcısı Çalışma zamanı ayarları) bölümünü bulun. **network** satırının açıklamasını kaldırın ve değerin `nat` olarak ayarlandığından emin olun.

   ```yaml
   moby_runtime:
     uri: "npipe://./pipe/docker_engine"
     network: "nat"
   ```

11. Yapılandırma dosyasını kaydedin. 

12. PowerShell'de IoT Edge hizmetini yeniden başlatın.

   ```powershell
   Stop-Service iotedge
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
  select TimeCreated, Message | Sort-Object -Descending
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
