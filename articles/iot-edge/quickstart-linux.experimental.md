---
title: Hızlı Başlangıç, Linux üzerinde Azure IOT Edge cihazı oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta bir IOT Edge cihazı oluşturma ve ardından önceden oluşturulmuş kod Azure Portalı'ndan uzaktan dağıtma öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/14/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: dc9ad544bb974dded098a27855ff5f6b9885d879
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53556586"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Hızlı Başlangıç: Bir Linux x64 cihaza, ilk IOT Edge modülü dağıtma

Azure IoT Edge, bulutun gücünü Nesnelerin İnterneti cihazlarınıza taşır. Bu hızlı başlangıçta, önceden derlenmiş kodu uzaktan bir IoT Edge cihazına dağıtmak için bulut arabirimini kullanmayı öğrenin.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. Bir IoT Hub oluşturma.
2. Bir IoT Edge cihazını IoT hub'ınıza kaydetme.
3. IoT Edge çalışma zamanını cihazınıza yükleme ve başlatma.
4. Bir IoT Edge cihazına uzaktan modül dağıtma.

![Diyagram - cihaz ve buluta yönelik hızlı başlangıç mimarisi](./media/quickstart-linux/install-edge-full.png)

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

* IoT Edge cihazınız olacak bir Linux cihazı veya sanal makinesi. Bu da Microsoft tarafından sağlanan kullanılması önerilir [Azure IOT Edge üzerinde Ubuntu'da](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu) sanal makine, IOT Edge çalışma zamanı önceden. Aşağıdaki komutu kullanarak bu sanal makine oluşturun:

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```

   Yeni bir sanal makine oluşturduğunuzda, Not **Publicıpaddress**, oluşturma komut çıktısı bir parçası olarak sağlanır. Bu hızlı başlangıcın sonraki bölümlerinde sanal makineye bağlanmak için bu genel IP adresi kullanır.

* Azure IOT Edge çalışma zamanı yerel sisteminizde çalıştırmayı tercih ederseniz konumundaki yönergeleri [(x64) Linux üzerinde Azure IOT Edge çalışma zamanı yükleme](how-to-install-iot-edge-linux.md).

* Raspberry Pi gibi bir temel ARM32 cihazı kullanmak istiyorsanız, konumundaki yönergeleri [yükleme Azure IOT Edge çalışma zamanı (ARM32v7/armhf) Linux'ta](how-to-install-iot-edge-linux-arm.md).

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Bu hızlı başlangıçta, Azure CLI ile IOT hub'ı oluşturarak başlayın.

![Diyagram - bulutta IOT hub'ı oluşturma](./media/quickstart-linux/create-iot-hub.png)

IoT Hub’ın ücretsiz düzeyi bu hızlı başlangıç için kullanılabilir. IoT Hub'ı daha önce kullandıysanız ve oluşturulmuş ücretsiz hub'ınız varsa bu IoT hub'ını kullanabilirsiniz. Her aboneliğin yalnızca bir ücretsiz IoT hub’ı olabilir. 

Aşağıdaki kod, **IoTEdgeResources** kaynak grubunda ücretsiz bir **F1** hub’ı oluşturur. *{hub_name}* değerini IoT hub'ınız için benzersiz bir adla değiştirin.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Aboneliğinizde zaten bir ücretsiz hub olduğu için hata alırsanız, SKU değerini **S1** olarak değiştirin. IOT hub'ı adı kullanılamıyor, bir hata alırsanız, başka birisi zaten bu ada sahip bir hub'ı olduğu anlamına gelir. Yeni bir ad deneyin. 

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT hub'ına bir IoT Edge cihazı kaydedin.
![Diyagram: bir IOT hub'ı kimlik ile bir cihaz kaydı](./media/quickstart-linux/register-device.png)

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

3. JSON çıktısını bağlantı dizesini kopyalayın ve kaydedin. Bu değeri bir sonraki bölümde IoT Edge çalışma zamanını yapılandırmak için kullanacaksınız.

   ![CLI çıkışından bağlantı dizesi alma](./media/quickstart/retrieve-connection-string.png)

## <a name="connect-the-iot-edge-device-to-iot-hub"></a>IOT Edge cihazı IOT hub'a bağlama

Azure IoT Edge çalışma zamanını IoT Edge cihazınıza yükleyin ve başlatın. 
![Diyagram - cihazda çalışma zamanını başlatma](./media/quickstart-linux/start-runtime.png)

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik daemon'u** bir Edge cihazı her başladığında çalışır ve IoT Edge aracısını çalıştırarak cihazı önyükler. **IoT Edge aracısı**, IoT Edge hub'ı dahil olmak üzere IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir. 

Çalışma zamanı yapılandırması sırasında cihaz bağlantı dizesi sağlamanız gerekir. Azure CLI'den aldığınız dizeyi kullanın. Bu dize, fiziksel cihazınızı Azure'daki IoT Edge cihaz kimliğiyle ilişkilendirir. 

### <a name="set-the-connection-string-on-the-iot-edge-device"></a>IOT Edge cihazında bağlantı dizesini Ayarla

* Ubuntu sanal makinesi üzerinde Azure IOT Edge kullanıyorsanız, IOT Edge Cihazınızı uzaktan yapılandırmak için daha önce kopyaladığınız cihaz bağlantı dizesini kullanın:

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunShellScript --script '/etc/iotedge/configedge.sh "{device_connection_string}"'
   ```

   Kalan adımları için çıkış oluşturma komutu genel IP adresini alın. Genel IP adresini sanal makinenizin genel bakış sayfasında Azure Portalı'nda da bulabilirsiniz. Sanal makinenize bağlanmak için aşağıdaki komutu kullanın. Değiştirin **{Publicıpaddress}** makinenizin adresine sahip. 

   ```azurecli-interactive
   ssh azureuser@{publicIpAddress}
   ```

* IOT Edge yerel makinenize veya ARM32 cihaz üzerinde çalıştırıyorsanız, /etc/iotedge/config.yaml ve güncelleştirme bulunan yapılandırma dosyasını açın **device_connection_string** değişken değerle daha önce kopyaladığınız, ardından yeniden başlatın yaptığınız değişiklikleri uygulamak için IOT Edge güvenlik arka plan programı:

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
![Diyagram - modülü buluttan cihaza dağıtın](./media/quickstart-linux/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, cihazda bir değişiklik yapmak zorunda kalmadan çalışacak bir IoT Edge modülünü göndermek için Azure portalını kullandınız. Bu örnekte gönderdiğiniz modül öğreticiler için kullanabileceğiniz ortam verilerini oluşturmaktadır.

IOT Edge Cihazınızda bir komut istemi yeniden açın veya Azure CLI SSH bağlantısından kullanın. Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun:

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
