---
title: Azure IoT Edge için Hızlı Başlangıç + Windows | Microsoft Docs
description: Bir sanal edge cihazında analiz çalıştırarak Azure IoT Edge’i deneme
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 05/03/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 2fd16ab4ade61b1a08f93294051f4246e47839b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631743"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Hızlı Başlangıç: İlk IoT Edge modülünüzü Azure portalından bir Windows cihaza dağıtma - önizleme

Bu hızlı başlangıçta, önceden derlenmiş kodu uzaktan bir IoT Edge cihazına dağıtmak için Azure IoT Edge bulut arabirimini kullanın. Bu görevi gerçekleştirmek için ilk olarak bir IoT Edge cihazının benzetimi için Windows cihazınızı kullanın; daha sonra buna bir modül dağıtabilirsiniz.

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][lnk-account] oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide, bir Nesnelerin İnterneti cihazının benzetimi için Windows çalıştıran bir bilgisayar veya bir sanal makine kullanmakta olduğunuz varsayılır. Windows’u sanal bir makinede çalıştırıyorsanız, [iç içe sanallaştırmayı][lnk-nested] etkinleştirin ve en az 2 GB bellek ayırın. 

1. Desteklenen bir Windows sürümü kullandığınızdan emin olun:
   * Windows 10 
   * Windows Server
2. [Docker for Windows][lnk-docker] yükleyin ve çalıştığından emin olun.
3. [Windows’da Python][lnk-python] yükleyin ve PIP komutunu kullanabildiğinizden emin olun. Bu hızlı başlangıç, Python’ın 2.7.9 ve 3.5.4 veya üzeri sürümleri ile test edilmiştir.  
4. IoT Edge denetim betiğini indirmek için aşağıdaki komutu çalıştırın.

   ```cmd
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Azure IoT Edge, Windows kapsayıcılarını veya Linux kapsayıcılarını çalıştırabilir. Windows kapsayıcılarını kullanmak için aşağıdakileri çalıştırmanız gerekir:
>    * Windows 10 Fall Creators Update veya
>    * Windows Server 1709 (Derleme 16299) ya da
>    * x64 tabanlı bir cihazda Windows IoT Core (Derleme 16299)
>
> Windows IoT Core’da, [Windows IoT Core’a IoT Edge çalışma zamanını yükleme][lnk-install-iotcore] yönergelerini izleyin. Aksi takdirde, [Windows kapsayıcılarını kullanmak için Docker’ı yapılandırın][lnk-docker-containers] ve isteğe bağlı olarak, aşağıdaki powershell komutuyla önkoşullarınızı doğrulayın:
>    ```powershell
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```

## <a name="create-an-iot-hub-with-azure-cli"></a>Azure CLI ile IoT hub oluşturma

Azure aboneliğinizde bir IoT hub oluşturun. IoT Hub’ın ücretsiz düzeyi bu hızlı başlangıç için kullanılabilir. Geçmişte IoT Hub kullandıysanız ve halen oluşturulmuş bir ücretsiz hub’ınız varsa, bu bölümü atlayıp [IoT Edge cihazını kaydetme][anchor-register] bölümüne geçebilirsiniz. Her aboneliğin yalnızca bir ücretsiz IoT hub’ı olabilir. 

1. [Azure portalında][lnk-portal] oturum açın. 
1. **Cloud Shell** düğmesini seçin. 

   ![Cloud Shell düğmesi][1]

1. Bir kaynak grubu oluşturun. Aşağıdaki kod, **Batı ABD** bölgesinde **IoTEdge** adında bir kaynak grubu oluşturur:

   ```azurecli
   az group create --name IoTEdge --location westus
   ```

1. Yeni kaynak grubunuzda bir IoT hub oluşturun. Aşağıdaki kod, **IoTEdge** kaynak grubunda **MyIotHub** adında ücretsiz bir **F1** hub’ı oluşturur:

   ```azurecli
   az iot hub create --resource-group IoTEdge --name MyIotHub --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazını kaydetme

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yapılandırma

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. İki modülden oluşur. İlk olarak, IoT Edge aracısı, IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. İkinci olarak, IoT Edge hub, IoT Edge cihazındaki modüller ve cihaz ile IoT Hub arasındaki iletişimi yönetir. 

Çalışma zamanını önceki bölümdeki IoT Edge cihaz bağlantı dizesi ile yapılandırın.

```cmd
iotedgectl setup --connection-string "{device connection string}" --nopass
```

Çalışma zamanını başlatın.

```cmd
iotedgectl start
```

IoT Edge aracısının bir modül olarak çalıştığından emin olmak için Docker’ı denetleyin.

```cmd
docker ps
```

![Docker’da edgeAgent’a bakın](./media/tutorial-simulate-device-windows/docker-ps.png)

## <a name="deploy-a-module"></a>Modül dağıtma

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, cihazda bir değişiklik yapmak zorunda kalmadan çalışacak bir IoT Edge modülünü göndermek için Azure portalını kullandınız. Bu örnekte gönderdiğiniz modül öğreticiler için kullanabileceğiniz ortam verilerini oluşturmaktadır. 

Benzetimli cihazınızı çalıştıran bilgisayarda yeniden komut istemini açın. Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun. 

```cmd
docker ps
```

![Cihazınızda üç modül görüntüleme](./media/tutorial-simulate-device-windows/docker-ps2.png)

tempSensor modülünden buluta gönderilen iletileri görüntüleyin. 

```cmd
docker logs -f tempSensor
```

![Verileri modülünüzden görüntüleme](./media/tutorial-simulate-device-windows/docker-logs.png)

Ayrıca, [IoT Hub gezgini aracını][lnk-iothub-explorer] kullanarak cihazın gönderdiği telemetriyi görüntüleyebilirsiniz. 
## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz benzetimli cihazı, her bir modül için başlatılan Docker kapsayıcılarıyla birlikte kaldırmak istiyorsanız aşağıdaki komutu kullanın: 

```cmd
iotedgectl uninstall
```

Oluşturduğunuz IoT Hub’a artık ihtiyacınız olmadığında, kaynağı ve kaynakla ilişkilendirilmiş cihazları kaldırmak için [az iot hub delete][lnk-delete] komutunu kullanabilirsiniz:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Sonraki adımlar

Bir IoT Edge cihazına IoT Edge modülünün nasıl dağıtılacağını öğrendiniz. Şimdi edge’de verileri analiz edebilmeniz için modül olarak farklı türlerde Azure hizmetlerini dağıtmayı deneyin. 

* [Azure İşlevi’ni modül olarak dağıtma](tutorial-deploy-function.md)
* [Azure Stream Analytics’i modül olarak dağıtma](tutorial-deploy-stream-analytics.md)
* [Kendi kodunuzu modül olarak dağıtma](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png

<!-- Links -->
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-python]: https://www.python.org/downloads/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete
[lnk-install-iotcore]: how-to-install-iot-core.md

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
