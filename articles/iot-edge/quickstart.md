---
title: Hızlı Başlangıç, Windows üzerinde Azure IOT Edge cihazı oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta bir IOT Edge cihazı oluşturma ve ardından önceden oluşturulmuş kod Azure Portalı'ndan uzaktan dağıtma öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/02/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 941d5d8f356fbd1477b4559f1475511165c01341
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53340105"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Hızlı Başlangıç: İlk IOT Edge modülü Azure portalından bir Windows cihazına dağıtma - Önizleme

Bu hızlı başlangıçta, önceden derlenmiş kodu uzaktan bir IoT Edge cihazına dağıtmak için Azure IoT Edge bulut arabirimini kullanın. Bu görevi gerçekleştirmek için ilk olarak bir IoT Edge cihazının benzetimi için Windows cihazınızı kullanın; daha sonra buna bir modül dağıtabilirsiniz.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. Bir IoT Hub oluşturma.
2. Bir IoT Edge cihazını IoT hub'ınıza kaydetme.
3. IoT Edge çalışma zamanını cihazınıza yükleme ve başlatma.
4. IoT Edge cihazına uzaktan modül dağıtma ve IoT Hub'a telemetri verileri gönderme.

![Diyagram - cihaz ve buluta yönelik hızlı başlangıç mimarisi](./media/quickstart/install-edge-full.png)

Bu hızlı başlangıçta oluşturduğunuz modül; sıcaklık, nem ve basınç verileri üreten bir sensör simülasyonudur. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için simülasyon verilerini analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır.

>[!NOTE]
>Windows üzerinde IoT Edge çalışma zamanı [genel önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) sürümündedir.

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıçtaki birçok adımı tamamlamak için Azure CLI kullanacaksınız. Azure IoT de ek işlevleri etkinleştirmek için bir uzantıya sahiptir.

Azure IoT uzantısını cloud shell örneğine ekleyin.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>Önkoşullar

Bulut kaynakları:

* Bu hızlı başlangıçta kullandığınız tüm kaynakları yönetmek için kullanacağınız bir kaynak grubu.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

IoT Edge cihazı:

* IoT Edge cihazınız olacak bir Windows bilgisayar veya sanal makine. Desteklenen bir Windows sürümünü kullanın:
  * Windows 10 veya daha yenisi
  * Windows Server 2016 veya daha yenisi
* Bir Windows bilgisayar etkinleştirilmişse, bu karşıladığından emin olun [sistem gereksinimleri](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) Hyper-V için.
* Bir sanal makineyse etkinleştirme [iç içe sanallaştırma](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) ve en az 2 GB bellek ayrılamadı.
* [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)’u yükleyin ve çalıştığından emin olun.

> [!TIP]
> Docker kurulumu sırasında Windows veya Linux kapsayıcılarını kullanma seçeneği bulunur. Bu hızlı başlangıçta, IoT Edge çalışma zamanını Linux kapsayıcılarıyla kullanacak şekilde yapılandırma açıklanılır.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Hızlı başlangıç adımlarına başlamak için Azure CLI ile IoT hub'ınızı oluşturun.

![Diyagram - bulutta IOT hub'ı oluşturma](./media/quickstart/create-iot-hub.png)

IoT Hub’ın ücretsiz düzeyi bu hızlı başlangıç için kullanılabilir. IoT Hub'ı daha önce kullandıysanız ve oluşturulmuş ücretsiz hub'ınız varsa bu IoT hub'ını kullanabilirsiniz. Her aboneliğin yalnızca bir ücretsiz IoT hub’ı olabilir.

Aşağıdaki kod, **IoTEdgeResources** kaynak grubunda ücretsiz bir **F1** hub’ı oluşturur. *{hub_name}* değerini IoT hub'ınız için benzersiz bir adla değiştirin.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1
   ```

   Aboneliğinizde zaten bir ücretsiz hub olduğu için hata alırsanız, SKU değerini **S1** olarak değiştirin. IOT hub'ı adı kullanılamıyor, bir hata alırsanız, başka birisi zaten bu ada sahip bir hub'ı olduğu anlamına gelir. Yeni bir ad deneyin. 

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT Hub'ına bir IoT Edge cihazı kaydedin.
![Diyagram: bir IOT hub'ı kimlik ile bir cihaz kaydı](./media/quickstart/register-device.png)

IoT hub'ınızla iletişim kurabilmesi amacıyla simülasyon cihazınız için bir cihaz kimliği oluşturun. Cihaz kimliği bulutta kalır ve fiziksel cihazla cihaz kimliği arasında bağlantı kurmak için benzersiz bir bağlantı dizesi kullanılır.

IOT Edge cihazları sınıflardır ve tipik bir IOT cihazlarında farklı yönetilebilir olduğundan, bu kimlik ile IOT Edge cihazı için bildirmek `--edge-enabled` bayrağı. 

1. Azure Cloud Shell'de aşağıdaki komutu girerek hub'ınızda **myEdgeDevice** adlı bir cihaz oluşturun.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

   İlke anahtarları iothubowner hakkında bir hata alırsanız, cloud shell azure CLI IOT ext uzantısı'nın en son sürümünü çalıştırdığından emin olun. 

2. Fiziksel cihazınızla IoT Hub'daki kimliği arasında bağlantı oluşturan cihaz bağlantı dizesini alın.

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. Bağlantı dizesini kopyalayın ve kaydedin. Bu değeri bir sonraki bölümde IoT Edge çalışma zamanını yapılandırmak için kullanacaksınız.

## <a name="install-and-start-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yükleme ve başlatma

Azure IoT Edge çalışma zamanını IoT Edge cihazınıza yükleyin ve cihaz bağlantı dizesiyle yapılandırın.
![Diyagram - cihazda çalışma zamanını başlatma](./media/quickstart/start-runtime.png)

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik daemon'u** bir Edge cihazı her başladığında çalışır ve IoT Edge aracısını çalıştırarak cihazı önyükler. **IoT Edge aracısı**, IoT Edge hub'ı dahil olmak üzere IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir.

Çalışma zamanı yüklemesi sırasında cihaz bağlantı dizesi istenir. Azure CLI'den aldığınız dizeyi kullanın. Bu dize, fiziksel cihazınızı Azure'daki IoT Edge cihaz kimliğiyle ilişkilendirir.

Bu bölümdeki yönergeler, IoT Edge çalışma zamanını Linux kapsayıcılarla yapılandırmaktadır. Windows kapsayıcıları kullanmak istiyorsanız bkz. [Azure IoT Edge çalışma zamanını Windows kapsayıcılarla kullanmak üzere Windows'a yükleme](how-to-install-iot-edge-windows-with-windows.md).

### <a name="connect-to-your-iot-edge-device"></a>IOT Edge Cihazınızı bağlama

Tüm bu bölümdeki adımlarda, IOT Edge Cihazınızda gerçekleşir. IOT Edge cihazı olarak kendi makine kullanıyorsanız, bu bölümü atlayabilirsiniz. Bir sanal makine ya da ikincil donanım kullanıyorsanız, artık bu makineye bağlanmak istediğiniz. 

### <a name="download-and-install-the-iot-edge-service"></a>IoT Edge hizmetini indirme ve yükleme

PowerShell'i kullanarak IoT Edge çalışma zamanını indirin ve yükleyin. Cihazınızı yapılandırmak için IoT Hub'dan aldığınız cihaz bağlantı dizesini kullanın.

1. IoT Edge cihazınızda PowerShell'i yönetici olarak çalıştırın.

2. IoT Edge hizmetini cihazınıza indirin ve yükleyin.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Linux
   ```

3. **DeviceConnectionString** istendiğinde önceki bölümde kopyaladığınız dizeyi iletin. Bağlantı dizesinin etrafındaki tırnak işaretlerini dahil etmeyin.

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
    sort-object @{Expression="TimeCreated";Descending=$false} |
    format-table -autosize -wrap
   ```

3. IoT Edge cihazınızda çalışan tüm modülleri görüntüleyin. Hizmet ilk kez başlatıldığı için yalnızca **edgeAgent** modülünün çalıştığını göreceksiniz. edgeAgent modülü varsayılan olarak çalışır ve cihazınıza dağıtmak istediğiniz ek modülleri yüklemenize ve başlatmanıza yardımcı olur.

   ```powershell
   iotedge list
   ```

   ![Cihazınızda bir modülü görüntüleme](./media/quickstart/iotedge-list-1.png)

IoT Edge cihazınız yapılandırıldı. Bulutta dağıtılan modülleri çalıştırmak için hazır.

## <a name="deploy-a-module"></a>Modül dağıtma

Azure IoT Edge cihazınızı, IoT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Diyagram - modülü buluttan cihaza dağıtın](./media/quickstart/deploy-module.png)

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

Kullanarak IOT hub'ınıza gelen iletileri izlemek isterseniz [Visual Studio Code için Azure IOT hub'ı Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (eski adıyla Azure IOT Toolkit uzantısını). 

## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Edge öğreticilerine devam etmek istiyorsanız bu hızlı başlangıçta kaydettiğiniz ve ayarladığınız cihazı kullanabilirsiniz. Aksi halde, oluşturduğunuz Azure kaynaklarını silebilir ve IoT Edge çalışma zamanını cihazınızdan kaldırabilirsiniz.

### <a name="delete-azure-resources"></a>Azure kaynaklarını silme

Sanal makinenizi ve IoT hub’ınızı yeni bir kaynak grubunda oluşturduysanız, bu grubu ve ilişkili tüm kaynaklarını silebilirsiniz. Çifte denetim var. emin olmak için kaynak grubunun içeriğini kullanıcının tutmak istediğiniz bir şey. Tüm bir grubu silmek istemiyorsanız, bunun yerine bu kaynakları tek tek silebilirsiniz.

**IoTEdgeResources** grubunu kaldırın.

   ```azurecli-interactive
   az group delete --name IoTEdgeResources
   ```

### <a name="remove-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını kaldırma

Gelecekte test için IoT Edge cihazını kullanmayı planlıyorsanız ama tempSensor modülünün kullanımda olmadığında IoT hub'ınıza veri göndermesini durdurmak istiyorsanız, aşağıdaki komutu kullanarak IoT Edge hizmetini durdurun.

   ```powershell
   Stop-Service iotedge -NoWait
   ```

Testi yeniden başlatmaya hazır olduğunuzda hizmeti yeniden başlatabilirsiniz

   ```powershell
   Start-Service iotedge
   ```

Cihazınızdaki yüklemeleri kaldırmak istiyorsanız aşağıdaki komutları kullanın.  

IoT Edge çalışma zamanını kaldırın.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Uninstall-SecurityDaemon
   ```

IoT Edge çalışma zamanı kaldırıldığında, oluşturduğu kapsayıcılar durdurulur, ancak cihazınızda yer almaya devam eder. Tüm kapsayıcıları görüntüleyin.

   ```powershell
   docker ps -a
   ```

IoT Edge çalışma zamanı tarafından cihazınızda oluşturulan kapsayıcıları silin. Farklı bir ad verdiyseniz, tempSensor kapsayıcısının adını değiştirin.

   ```powershell
   docker rm -f tempSensor
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```
   
## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir IoT Edge cihazı oluşturdunuz ve cihaza kod dağıtmak için Azure IoT Edge bulut arabirimini kullandınız. Artık ortamı hakkında ham veri üreten bir test cihazınız var.

Azure IoT Edge'in bu verileri Edge'de iş içgörüsüne dönüştürmenize nasıl yardımcı olabileceğini öğrenmek için diğer öğreticilere devam etmeye hazırsınız.

> [!div class="nextstepaction"]
> [Bir Azure İşlevi kullanarak sensör verilerini filtreleme](tutorial-deploy-function.md)
