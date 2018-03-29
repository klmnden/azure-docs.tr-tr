---
title: Hızlı Başlangıç Azure IOT kenar + Linux | Microsoft Docs
description: Bir sanal sınır aygıtında analiz çalıştırarak Azure IOT kenar deneyin
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 750f09c91a086b22df5e7557e4b6fc6a763499e2
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-or-mac-device---preview"></a>Hızlı Başlangıç: ilk IOT kenar modülünüzün bir Linux veya Mac aygıta dağıtmak - Önizleme

Azure IOT kenar bulut gücünü nesnelerin interneti aygıtlarınızı taşır. Bu konuda, bulut arabirimi önceden oluşturulmuş kodu uzaktan IOT kenar cihazına dağıtmak için nasıl kullanılacağını öğrenin.

Etkin bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap] [ lnk-account] başlamadan önce.

## <a name="prerequisites"></a>Önkoşullar

Bu Hızlı Başlangıç, bilgisayar veya sanal makine bir nesnelerin interneti aygıtı gibi kullanır. Bir IOT sınır cihazı makinenize etkinleştirmek için aşağıdaki hizmetleri gereklidir:

* Python PIP, IOT kenar çalışma zamanı yüklenemedi.
   * Linux: `sudo apt-get install python-pip`.
   * MacOS: `sudo easy_install pip`.
* IOT kenar modüllerini çalıştırmak için docker
   * [Linux için Docker yükleme] [ lnk-docker-ubuntu] ve çalışır durumda olduğundan emin olun. 
   * [Mac için Docker yükleme] [ lnk-docker-mac] ve çalışır durumda olduğundan emin olun. 

## <a name="create-an-iot-hub-with-azure-cli"></a>Azure CLI ile IOT hub oluşturma

IOT hub'ı, Azure aboneliğinizde oluşturun. IOT hub'ı ücretsiz düzeyi bu hızlı başlangıç için çalışır. IOT hub'ı geçmişte kullanmış olduğunuz ve oluşturulan boş bir hub'ı zaten varsa, bu bölüm atlayın ve üzerinde Git [bir IOT kenar kaydedilecek][anchor-register]. Her abonelik yalnızca bir ücretsiz IOT hub olabilir. 

1. [Azure portalında][lnk-portal] oturum açın. 
1. Seçin **bulut Kabuk** düğmesi. 

   ![Bulut Kabuk düğmesi][1]

1. Bir kaynak grubu oluşturun. Aşağıdaki kod adlı bir kaynak grubu oluşturur **IoTEdge** içinde **Batı ABD** bölge:

   ```azurecli
   az group create --name IoTEdge --location westus
   ```

1. IOT hub'ı, yeni kaynak grubu oluşturun. Aşağıdaki kod ücretsiz oluşturur **F1** adlı hub **MyIotHub** kaynak grubunda **IoTEdge**:

   ```azurecli
   az iot hub create --resource-group IoTEdge --name MyIotHub --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Bir IOT sınır cihazı kaydetme

IOT hub ile iletişim kurabilmesi için sanal cihazınız için bir cihaz kimliği oluşturma. IOT sınır cihazları davranır ve tipik IOT cihazları farklı bir biçimde yönetilebilir olduğundan, bunun bir IOT sınır cihazı başlangıçtan itibaren olmasını bildirin. 

1. Azure portalında IOT hub'ına gidin.
1. Seçin **IOT kenar (Önizleme)**.
1. Seçin **ekleme IOT sınır cihazı**.
1. Sanal cihazınız bir benzersiz cihaz kimliği verin
1. Seçin **kaydetmek** Cihazınızı eklemek için.
1. Yeni Cihazınızı aygıtları listesinden seçin. 
1. Değeri kopyalama **bağlantı dizesi--birincil anahtar** ve kaydedin. Sonraki bölümde IOT kenar çalışma zamanı yapılandırmak için bu değeri kullanılır. 

## <a name="install-and-start-the-iot-edge-runtime"></a>Yükleyin ve IOT kenar çalışma zamanı başlatın

IOT kenar çalışma zamanı, tüm IOT kenar aygıtlarda dağıtılır. İki modülden oluşur. İlk olarak, IOT kenar Aracısı dağıtımı ve IOT sınır cihazı modülleri izlenmesini kolaylaştırır. İkinci olarak, IOT kenar hub IOT sınır cihazı modülleri arasında ve cihaz IOT hub'ı arasındaki iletişim yönetir. 

IOT sınır cihazı çalıştırdığı makinede IOT kenar denetim komut dosyasını karşıdan yükleyin:
```bash
sudo pip install -U azure-iot-edge-runtime-ctl
```

Çalışma zamanı IOT kenar cihaz bağlantı dizenizi önceki bölümdeki yapılandırın:
```bash
sudo iotedgectl setup --connection-string "{device connection string}" --nopass
```

Çalışma zamanı'nı başlatın:
```bash
sudo iotedgectl start
```

Docker IOT kenar Aracısı'nı bir modül olarak çalışıp çalışmadığını kontrol edin:
```bash
sudo docker ps
```

![Docker edgeAgent bakın](./media/tutorial-simulate-device-linux/docker-ps.png)

## <a name="deploy-a-module"></a>Bir modül dağıtma

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

Bu hızlı başlangıç yeni bir IOT sınır cihazı oluşturan ve IOT kenar çalışma zamanı yüklü. Ardından, cihaz için değişiklik yapmak zorunda kalmadan cihazda çalıştırmak için bir IOT kenar modülü göndermek için Azure portal kullanılır. Bu durumda, gönderilen modülü öğreticileri için kullanabileceğiniz çevresel veri oluşturur. 

Sanal cihazınız yeniden çalıştıran bilgisayarda komut istemi açın. Buluttan dağıtılan modülü IOT kenar aygıtınızda çalışır durumda olduğunu doğrulayın:

```bash
sudo docker ps
```

![Cihazınızda üç modüller görünümü](./media/tutorial-simulate-device-linux/docker-ps2.png)

TempSensor modülünden buluta gönderilen iletiler görüntüleyin:

```bash
sudo docker logs -f tempSensor
```

![Modülünüzün verileri görüntüleme](./media/tutorial-simulate-device-linux/docker-logs.png)

Cihaz kullanarak göndermeyi telemetriyi de görüntüleyebilirsiniz [IOT hub'ı explorer aracı][lnk-iothub-explorer]. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Her modül için başlatılan Docker kapsayıcıları yanı sıra, oluşturulan sanal cihaz kaldırmak isterseniz, aşağıdaki komutu kullanın: 

```bash
sudo iotedgectl uninstall
```

Oluşturduğunuz IOT Hub artık ihtiyacınız olduğunda kullanabileceğiniz [az IOT hub delete] [ lnk-delete] kaynak ve onunla ilişkili tüm aygıtları kaldırmak için komutu:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Sonraki adımlar

Bir IOT kenar modülünü IOT kenar cihazına dağıtmak öğrendiniz. Böylece veri kenarına çözümleyebilirsiniz şimdi farklı türlerdeki modüllerle, Azure Hizmetleri dağıtmayı deneyin. 

* [Kendi kodunuzu bir modül olarak dağıtma](tutorial-csharp-module.md)
* [Bir modül olarak Azure işlevi dağıtma](tutorial-deploy-function.md)
* [Azure Stream Analytics’i modül olarak dağıtma](tutorial-deploy-stream-analytics.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-docker-mac]: https://docs.docker.com/docker-for-mac/install/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
