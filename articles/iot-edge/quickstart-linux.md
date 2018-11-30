---
title: Azure IoT Edge için Hızlı Başlangıç + Linux | Microsoft Docs
description: Bu hızlı başlangıçta, önceden derlenmiş kodu uzaktan bir IoT Edge cihazına dağıtmayı öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/14/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 4e53d0d492213373794821e14d4c08ec9db2ad5c
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495451"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Hızlı Başlangıç: Bir Linux x64 cihazına ilk IoT Edge modülünüzü dağıtma

Azure IoT Edge, bulutun gücünü Nesnelerin İnterneti cihazlarınıza taşır. Bu hızlı başlangıçta, önceden derlenmiş kodu uzaktan bir IoT Edge cihazına dağıtmak için bulut arabirimini kullanmayı öğrenin.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. Bir IoT Hub oluşturma.
2. Bir IoT Edge cihazını IoT hub'ınıza kaydetme.
3. IoT Edge çalışma zamanını cihazınıza yükleme ve başlatma.
4. Bir IoT Edge cihazına uzaktan modül dağıtma.

![Hızlı başlangıç mimarisi](./media/quickstart-linux/install-edge-full.png)

Bu hızlı başlangıç, Linux bilgisayarınızı veya sanal makinenizi bir IoT Edge cihazına dönüştürür. Bu işlemin ardından Azure portalından cihazınıza bir modül dağıtabilirsiniz. Bu hızlı başlangıçta oluşturduğunuz modül; sıcaklık, nem ve basınç verileri üreten bir sensör simülasyonudur. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için simülasyon verilerini analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır.

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
   az group create --name IoTEdgeResources --location westus2
   ```

IoT Edge cihazı:

* IoT Edge cihazınız olacak bir Linux cihazı veya sanal makinesi. Azure'da bir sanal makine oluşturmak istiyorsanız aşağıdaki komutu kullanarak hızlı bir başlangıç yapabilirsiniz:

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```

   Yeni bir sanal makine oluşturduğunuzda, Not **Publicıpaddress**, oluşturma komut çıktısı bir parçası olarak sağlanır. Bu hızlı başlangıcın sonraki bölümlerinde sanal makineye bağlanmak için bu genel IP adresi kullanın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Hızlı başlangıç adımlarına başlamak için Azure CLI ile IoT hub'ınızı oluşturun.

![IoT Hub oluşturun](./media/quickstart-linux/create-iot-hub.png)

IoT Hub’ın ücretsiz düzeyi bu hızlı başlangıç için kullanılabilir. IoT Hub'ı daha önce kullandıysanız ve oluşturulmuş ücretsiz hub'ınız varsa bu IoT hub'ını kullanabilirsiniz. Her aboneliğin yalnızca bir ücretsiz IoT hub’ı olabilir. 

Aşağıdaki kod, **IoTEdgeResources** kaynak grubunda ücretsiz bir **F1** hub’ı oluşturur. *{hub_name}* değerini IoT hub'ınız için benzersiz bir adla değiştirin.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Aboneliğinizde zaten bir ücretsiz hub olduğu için hata alırsanız, SKU değerini **S1** olarak değiştirin. IOT hub'ı adı kullanılamıyor, bir hata alırsanız, başka birisi zaten bu ada sahip bir hub'ı olduğu anlamına gelir. Yeni bir ad deneyin. 

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT hub'ına bir IoT Edge cihazı kaydedin.
![Cihaz kaydetme](./media/quickstart-linux/register-device.png)

IoT hub'ınızla iletişim kurabilmesi amacıyla simülasyon cihazınız için bir cihaz kimliği oluşturun. Cihaz kimliği bulutta kalır ve fiziksel cihazla cihaz kimliği arasında bağlantı kurmak için benzersiz bir bağlantı dizesi kullanılır. 

IOT Edge cihazları sınıflardır ve tipik bir IOT cihazlarında farklı yönetilebilir olduğundan, bu kimlik ile IOT Edge cihazı için bildirmek `--edge-enabled` bayrağı. 

1. Azure Cloud Shell'de aşağıdaki komutu girerek hub'ınızda **myEdgeDevice** adlı bir cihaz oluşturun.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

   İlke anahtarları iothubowner hakkında bir hata alırsanız, cloud shell azure CLI IOT ext uzantısı'nın en son sürümünü çalıştırdığından emin olun. 

2. Fiziksel cihazınızla IoT Hub'daki kimliği arasında bağlantı oluşturan cihaz bağlantı dizesini alın. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. Bağlantı dizesini kopyalayın ve kaydedin. Bu değeri bir sonraki bölümde IoT Edge çalışma zamanını yapılandırmak için kullanacaksınız. 

## <a name="install-and-start-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yükleme ve başlatma

Azure IoT Edge çalışma zamanını IoT Edge cihazınıza yükleyin ve başlatın. 
![Cihaz kaydetme](./media/quickstart-linux/start-runtime.png)

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik daemon'u** bir Edge cihazı her başladığında çalışır ve IoT Edge aracısını çalıştırarak cihazı önyükler. **IoT Edge aracısı**, IoT Edge hub'ı dahil olmak üzere IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir. 

Çalışma zamanı yapılandırması sırasında cihaz bağlantı dizesi sağlamanız gerekir. Azure CLI'den aldığınız dizeyi kullanın. Bu dize, fiziksel cihazınızı Azure'daki IoT Edge cihaz kimliğiyle ilişkilendirir. 

### <a name="connect-to-your-iot-edge-device"></a>IOT Edge Cihazınızı bağlama

Tüm bu bölümdeki adımlarda, IOT Edge Cihazınızda gerçekleşir. IOT Edge cihazı olarak kendi makine kullanıyorsanız, bu bölümü atlayabilirsiniz. Bir sanal makine ya da ikincil donanım kullanıyorsanız, artık bu makineye bağlanmak istediğiniz. 

Bu hızlı başlangıçta bir Azure sanal makinesinde oluşturduysanız tarafından oluşturma komut çıktısı genel IP adresini alın. Genel IP adresini sanal makinenizin genel bakış sayfasında Azure Portalı'nda da bulabilirsiniz. Sanal makinenize bağlanmak için aşağıdaki komutu kullanın. Değiştirin **{Publicıpaddress}** makinenizin adresine sahip. 

```azurecli-interactive
ssh azureuser@{publicIpAddress}
```

### <a name="register-your-device-to-use-the-software-repository"></a>Yazılım deposunu kullanmak için cihazınızı kaydetme

IoT Edge çalışma zamanını çalıştırmak için ihtiyacınız olan paketler, bir yazılım deposunda yönetilir. Bu depoya erişmek için IoT Edge cihazınızı yapılandırın. 

Bu bölümdeki adımlar **Ubuntu 16.04** çalıştıran x64 cihazlara yöneliktir. Linux veya cihaz mimarileri diğer sürümlerinde yazılım deposuna erişmek için bkz: [(x64) Linux üzerinde Azure IOT Edge çalışma zamanı yükleme](how-to-install-iot-edge-linux.md) veya [Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md).

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

1. **apt-get** komutunu kullanın.

   ```bash
   sudo apt-get update
   ```

2. Kapsayıcı çalışma zamanı olan **Moby**’yi yükleyin.

   ```bash
   sudo apt-get install moby-engine
   ```

3. Moby için CLI komutlarını yükleyin. 

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

2. IoT Edge yapılandırma dosyasını açın. Erişim için yükseltilmiş ayrıcalıklar kullanmak zorunda korumalı bir dosyadır.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. IoT Edge cihazı bağlantı dizesini ekleyin. **device_connection_string** değişkenini bulun ve cihazınızı kaydettikten sonra değerini kopyaladığınız dizeyle güncelleştirin. Bu bağlantı dizesi, fiziksel cihazınızı Azure'da oluşturduğunuz cihaz kimliğiyle ilişkilendirir.

4. Dosyayı kaydedin ve kapatın. 

   `CTRL + X`, `Y`, `Enter`

5. Yaptığınız değişikliklerin uygulanması için IoT Edge güvenlik daemon'unu yeniden başlatın.

   ```bash
   sudo systemctl restart iotedge
   ```

>[!TIP]
>`iotedge` komutlarını çalıştırmak için yükseltilmiş ayrıcalıklara ihtiyacınız olacaktır. Makinenizdeki oturumu kapattıktan sonra IoT Edge çalışma zamanını yükleyip oturum açtığınızda izinleriniz otomatik olarak güncelleştirilir. O zamana kadar komutların önüne **sudo** eklemenize gerekir. 

### <a name="view-the-iot-edge-runtime-status"></a>IoT Edge çalışma zamanı durumunu görüntüleme

Çalışma zamanının başarıyla yüklendiğinden ve yapılandırıldığından emin olun.

1. Edge Güvenlik Daemon'unun sistem hizmeti olarak çalışıp çalışmadığını kontrol edin.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Edge Daemon'unun sistem hizmeti olarak çalıştığını görün](./media/quickstart-linux/iotedged-running.png)

2. Hizmetle ilgili sorunları gidermeniz gerekirse hizmet günlüklerini alın. 

   ```bash
   journalctl -u iotedge
   ```

3. Cihazınızda çalışan modülleri görüntüleyin. 

   ```bash
   sudo iotedge list
   ```

   ![Cihazınızda bir modülü görüntüleme](./media/quickstart-linux/iotedge-list-1.png)

IoT Edge cihazınız yapılandırıldı. Bulutta dağıtılan modülleri çalıştırmak için hazır. 

## <a name="deploy-a-module"></a>Modül dağıtma

Azure IoT Edge cihazınızı, IoT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Cihaz kaydetme](./media/quickstart-linux/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, cihazda bir değişiklik yapmak zorunda kalmadan çalışacak bir IoT Edge modülünü göndermek için Azure portalını kullandınız. Bu örnekte gönderdiğiniz modül öğreticiler için kullanabileceğiniz ortam verilerini oluşturmaktadır.

IoT Edge cihazınızda komut istemini yeniden açın. Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun:

   ```bash
   sudo iotedge list
   ```

   ![Cihazınızda üç modül görüntüleme](./media/quickstart-linux/iotedge-list-2.png)

tempSensor modülünden gönderilen iletileri görüntüleyin:

   ```bash
   sudo iotedge logs tempSensor -f
   ```

![Verileri modülünüzden görüntüleme](./media/quickstart-linux/iotedge-logs.png)

Günlüğün son satırında `Using transport Mqtt_Tcp_Only` varsa sıcaklık sensörü modülü Edge Hub'ına bağlanmayı bekliyor olabilir. Modülü sonlandırıp Edge Aracısı tarafından yeniden başlatılmasını sağlayın. `sudo docker stop tempSensor` komutuyla sonlandırabilirsiniz.

Kullanarak IOT hub'ınıza gelen iletileri izlemek isterseniz [Visual Studio Code için Azure IOT Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Edge öğreticilerine devam etmek istiyorsanız bu hızlı başlangıçta kaydettiğiniz ve ayarladığınız cihazı kullanabilirsiniz. Aksi halde, oluşturduğunuz Azure kaynaklarını silebilir ve IoT Edge çalışma zamanını cihazınızdan kaldırabilirsiniz.

### <a name="delete-azure-resources"></a>Azure kaynaklarını silme

Sanal makinenizi ve IoT hub’ınızı yeni bir kaynak grubunda oluşturduysanız, bu grubu ve ilişkili tüm kaynaklarını silebilirsiniz. Çifte denetim var. emin olmak için kaynak grubunun içeriğini kullanıcının tutmak istediğiniz bir şey. Tüm bir grubu silmek istemiyorsanız, bunun yerine bu kaynakları tek tek silebilirsiniz.

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
   sudo apt-get remove --purge moby-cli
   sudo apt-get remove --purge moby-engine
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç, tüm IoT Edge öğreticilerinin önkoşuludur. Azure IoT Edge'in bu verileri Edge'de iş içgörüsüne dönüştürmenize nasıl yardımcı olabileceğini öğrenmek için diğer öğreticilere devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir Azure İşlevi kullanarak sensör verilerini filtreleme](tutorial-deploy-function.md)
