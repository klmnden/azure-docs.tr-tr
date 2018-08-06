---
title: Azure IoT Edge için Hızlı Başlangıç + Linux | Microsoft Docs
description: Bu hızlı başlangıçta, önceden derlenmiş kodu uzaktan bir IoT Edge cihazına dağıtmayı öğrenin.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: dfcb764d75b7328d1234d47d82afdae8d6a0deef
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413023"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Hızlı Başlangıç: Bir Linux x64 cihazına ilk IoT Edge modülünüzü dağıtma

Azure IoT Edge, bulutun gücünü Nesnelerin İnterneti cihazlarınıza taşır. Bu hızlı başlangıçta, önceden derlenmiş kodu uzaktan bir IoT Edge cihazına dağıtmak için bulut arabirimini kullanmayı öğrenin.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. Bir IoT Hub oluşturma.
2. Bir IoT Edge cihazını IoT hub'ınıza kaydetme.
3. IoT Edge çalışma zamanını cihazınıza yükleme ve başlatma.
4. Bir IoT Edge cihazına uzaktan modül dağıtma.

![Hızlı başlangıç mimarisi][2]

Bu hızlı başlangıç, Linux bilgisayarınızı veya sanal makinenizi bir IoT Edge cihazına dönüştürür. Bu işlemin ardından Azure portalından cihazınıza bir modül dağıtabilirsiniz. Bu hızlı başlangıçta oluşturduğunuz modül; sıcaklık, nem ve basınç verileri üreten bir sensör simülasyonudur. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için simülasyon verilerini analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır. 

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][lnk-account] oluşturun.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıçtaki birçok adımı tamamlamak için Azure CLI kullanacaksınız. Azure IoT de ek işlevleri etkinleştirmek için bir uzantıya sahiptir. 

Azure IoT uzantısını cloud shell örneğine ekleyin.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```
   
## <a name="prerequisites"></a>Ön koşullar

Bulut kaynakları: 

* Bu hızlı başlangıçta kullandığınız tüm kaynakları yönetmek için kullanacağınız bir kaynak grubu. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

* IoT Edge cihazınız olacak bir Linux sanal makinesi. 

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username azureuser --generate-ssh-keys --size Standard_B1ms
   ```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Hızlı başlangıç adımlarına başlamak için Azure CLI ile IoT Hub'ınızı oluşturun. 

![IoT Hub oluşturun][3]

IoT Hub’ın ücretsiz düzeyi bu hızlı başlangıç için kullanılabilir. IoT Hub'ı daha önce kullandıysanız ve oluşturulmuş ücretsiz hub'ınız varsa bu IoT hub'ını kullanabilirsiniz. Her aboneliğin yalnızca bir ücretsiz IoT hub’ı olabilir. 

Aşağıdaki kod, **IoTEdgeResources** kaynak grubunda ücretsiz bir **F1** hub’ı oluşturur. *{hub_name}* değerini IoT hub'ınız için benzersiz bir adla değiştirin.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Aboneliğinizde zaten bir ücretsiz hub olduğu için hata alırsanız, SKU değerini **S1** olarak değiştirin. 

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT Hub'ına bir IoT Edge cihazı kaydedin.
![Cihaz kaydetme][4]

IoT hub'ınızla iletişim kurabilmesi amacıyla simülasyon cihazınız için bir cihaz kimliği oluşturun. IoT Edge cihazlarının davranışları ve yönetim özellikleri tipik IoT cihazlarından farklı olduğundan bunun en başından itibaren bir IoT Edge cihazı olduğunu bildirmiş olursunuz. 

1. Azure Cloud Shell'de aşağıdaki komutu girerek hub'ınızda **myEdgeDevice** adlı bir cihaz oluşturun.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

1. Fiziksel cihazınızla IoT Hub'daki kimliği arasında bağlantı oluşturan cihaz bağlantı dizesini alın. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Bağlantı dizesini kopyalayın ve kaydedin. Bu değeri bir sonraki bölümde IoT Edge çalışma zamanını yapılandırmak için kullanacaksınız. 


## <a name="install-and-start-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yükleme ve başlatma

Azure IoT Edge çalışma zamanını cihazınıza yükleyin ve başlatın. 
![Cihaz kaydetme][5]

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik daemon'u** bir Edge cihazı her başladığında çalışır ve IoT Edge aracısını çalıştırarak cihazı önyükler. **IoT Edge aracısı**, IoT Edge hub'ı dahil olmak üzere IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir. 

Aşağıdaki adımları, bu hızlı başlangıç için hazırladığınız Linux makinesinde veya VM’de izleyin. 

### <a name="register-your-device-to-use-the-software-repository"></a>Yazılım deposunu kullanmak için cihazınızı kaydetme

IoT Edge çalışma zamanını çalıştırmak için ihtiyacınız olan paketler, bir yazılım deposunda yönetilir. Bu depoya erişmek için IoT Edge cihazınızı yapılandırın. 

Bu bölümdeki adımlar **Ubuntu 16.04** çalıştıran cihazlara yöneliktir. Diğer Linux sürümlerinde yazılım deposuna erişmek için bkz. [Azure IoT Edge çalışma zamanını Linux (x64) üzerine yükleme](how-to-install-iot-edge-linux.md) veya [Azure IoT Edge çalışma zamanını Linux (ARM32v7/armhf) üzerine yükleme](how-to-install-iot-edge-linux-arm.md).

1. IoT Edge cihazı olarak kullandığınız makineye depo yapılandırmasını yükleyin.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Depoya erişmek için bir ortak anahtar yükleyin.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

IoT Edge çalışma zamanı, kapsayıcılardan oluşan bir kümedir ve IoT Edge cihazınıza dağıttığınız mantık, kapsayıcı olarak paketlenir. Cihazlarınızı bu bileşenlere hazır hale getirmek için bir kapsayıcı çalışma zamanı yükleyin.

**apt-get** komutunu kullanın.

   ```bash
   sudo apt-get update
   ```

Kapsayıcı çalışma zamanı olan **Moby**’yi yükleyin.

   ```bash
   sudo apt-get install moby-engine
   ```

Moby için CLI komutlarını yükleyin. 

   ```bash
   sudo apt-get install moby-cli
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>IoT Edge güvenlik daemon'unu yükleme ve yapılandırma

Güvenlik daemon'u sistem hizmeti olarak yüklenir ve bu sayede IoT Edge çalışma zamanı cihaz her başlatıldığında çalışır. Yükleme aynı zamanda güvenlik daemon'unun cihazın donanım güvenliğiyle etkileşim kurmasını sağlayan **hsmlib** uygulamasının bir sürümünü de içerir. 

1. IoT Edge Güvenlik Daemon'unu indirin ve yükleyin. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. IoT Edge yapılandırma dosyasını açın. Korumalı bir dosya olduğu için yükseltilmiş ayrıcalıklarla erişmeniz gerekir.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. IoT Edge cihazı bağlantı dizesini ekleyin. **device_connection_string** değişkenini bulun ve cihazınızı kaydettikten sonra değerini kopyaladığınız dizeyle güncelleştirin.

4. Dosyayı kaydedin ve kapatın. 

   `CTRL + X`, `Y`, `Enter`

4. IoT Edge güvenlik daemon'unu yeniden başlatın.

   ```bash
   sudo systemctl restart iotedge
   ```

5. Edge Güvenlik Daemon'unun sistem hizmeti olarak çalışıp çalışmadığını kontrol edin.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Edge Daemon'unun sistem hizmeti olarak çalıştığını görün](./media/quickstart-linux/iotedged-running.png)

   Aşağıdaki komutu çalıştırarak Edge Güvenlik Daemon'u günlük dosyalarını da görebilirsiniz:

   ```bash
   journalctl -u iotedge
   ```

6. Cihazınızda çalışan modülleri görüntüleyin. 

   >[!TIP]
   >İlk olarak `iotedge` komutlarını çalıştırmak için *sudo* kullanmanız gerekir. Makinenizde oturumunuzu kapatıp tekrar açarak izinleri güncelleştirdikten sonra `iotedge` komutlarını yükseltilmiş ayrıcalıklar olmadan çalıştırabilirsiniz. 

   ```bash
   sudo iotedge list
   ```

   ![Cihazınızda bir modülü görüntüleme](./media/quickstart-linux/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Modül dağıtma

Azure IoT Edge cihazınızı, IoT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Cihaz kaydetme][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, cihazda bir değişiklik yapmak zorunda kalmadan çalışacak bir IoT Edge modülünü göndermek için Azure portalını kullandınız. Bu örnekte gönderdiğiniz modül öğreticiler için kullanabileceğiniz ortam verilerini oluşturmaktadır. 

Benzetimli cihazınızı çalıştıran bilgisayarda yeniden komut istemini açın. Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun:

   ```bash
   sudo iotedge list
   ```
   Oturum kapatma ve açma döngüsünden sonra yukarıdaki komut için *sudo* kullanılması gerekmez.

   ![Cihazınızda üç modül görüntüleme](./media/quickstart-linux/iotedge-list-2.png)

tempSensor modülünden gönderilen iletileri görüntüleyin:

   ```bash
   sudo iotedge logs tempSensor -f 
   ```
Oturum kapatma ve açma döngüsünden sonra yukarıdaki komut için *sudo* kullanılması gerekmez.

![Verileri modülünüzden görüntüleme](./media/quickstart-linux/iotedge-logs.png)

Günlüğün son satırında `Using transport Mqtt_Tcp_Only` varsa sıcaklık sensörü modülü Edge Hub'ına bağlanmayı bekliyor olabilir. Modülü sonlandırıp Edge Aracısı tarafından yeniden başlatılmasını sağlayın. `sudo docker stop tempSensor` komutuyla sonlandırabilirsiniz.

[IoT Hub gezginini][lnk-iothub-explorer] veya [Visual Studio Code için Azure IoT Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) kullanarak cihazın gönderdiği telemetri verilerini de görüntüleyebilirsiniz. 


## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Edge öğreticilerine devam etmek istiyorsanız bu hızlı başlangıçta kaydettiğiniz ve ayarladığınız cihazı kullanabilirsiniz. Aksi halde, oluşturduğunuz Azure kaynaklarını silebilir ve IoT Edge çalışma zamanını cihazınızdan kaldırabilirsiniz. 

### <a name="delete-azure-resources"></a>Azure kaynaklarını silme

Sanal makinenizi ve IoT hub’ınızı yeni bir kaynak grubunda oluşturduysanız, bu grubu ve ilişkili tüm kaynaklarını silebilirsiniz. İlgili kaynak grubunda bulunan saklamak istediğiniz bir şey varsa, temizlemek istediğiniz kaynakları silmeniz yeterlidir. 

**IoTEdgeResources** grubunu kaldırın. 

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```

### <a name="remove-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını kaldırma

Cihazınızdaki yüklemeleri kaldırmak istiyorsanız aşağıdaki komutları kullanın.  

IoT Edge çalışma zamanını kaldırın.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

IoT Edge çalışma zamanı kaldırıldığında, oluşturduğu kapsayıcılar durdurulur, ancak cihazınızda yer almaya devam eder. Tüm kapsayıcıları görüntüleyin.

   ```bash
   sudo docker ps -a
   ```

IoT Edge çalışma zamanı tarafından cihazınızda oluşturulan kapsayıcıları silin. Farklı bir ad verdiyseniz, tempSensor kapsayıcısının adını değiştirin. 

   ```bash
   sudo docker rm -f tempSensor
   sudo docker rm -f edgeHub
   sudo docker rm -f edgeAgent
   ```

Kapsayıcı çalışma zamanını kaldırın.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç, tüm IoT Edge öğreticilerinin önkoşuludur. Azure IoT Edge'in bu verileri Edge'de iş içgörüsüne dönüştürmenize nasıl yardımcı olabileceğini öğrenmek için diğer öğreticilere devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir Azure İşlevi kullanarak sensör verilerini filtreleme](tutorial-deploy-function.md)



<!-- Images -->
[0]: ./media/quickstart-linux/cloud-shell.png
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png


<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
