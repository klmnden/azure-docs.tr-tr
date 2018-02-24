---
title: "Hızlı Başlangıç Azure IOT kenar + Windows | Microsoft Docs"
description: "Bir sanal sınır aygıtında analiz çalıştırarak Azure IOT kenar deneyin"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 803b0bbff12c8ce471c0bff5e22e24601b8ce07f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Hızlı Başlangıç:, ilk Windows cihaz IOT kenar modülüne Azure portalından dağıtma - Önizleme

Bu Hızlı Başlangıç, önceden oluşturulmuş kodu uzaktan IOT kenar cihazına dağıtmak için Azure IOT kenar bulut arabirimini kullanın. Bir modül dağıtabilirsiniz sonra bu görevi gerçekleştirmek için önce Windows Cihazınızı IOT sınır cihazı benzetimini yapmak için kullanın.

Etkin bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap] [ lnk-account] başlamadan önce.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, bir bilgisayarı veya Windows çalıştıran sanal makine bir nesnelerin interneti aygıt benzetimini yapmak için kullanmakta olduğunuz olduğunu varsayar. Bir sanal makinede Windows çalıştırıyorsanız, etkinleştirme [iç içe geçmiş sanallaştırma] [ lnk-nested] ve en az 2 GB bellek ayıramadı. 

1. Desteklenen bir Windows sürümünü kullandığınızdan emin olun:
   * Windows 10 
   * Windows Server
2. Yükleme [Windows için Docker] [ lnk-docker] ve emin olun çalıştığından.
3. Yükleme [Python 2.7 Windows] [ lnk-python] ve PIP komutunu kullanabilirsiniz emin olun.
4. IOT kenar denetim komut dosyasını karşıdan yüklemek için aşağıdaki komutu çalıştırın.

   ```cmd
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Azure IOT kenar Windows kapsayıcıları veya Linux kapsayıcıları çalıştırabilirsiniz. Windows kapsayıcılar kullanmak için çalıştırmanız gerekir:
>    * Windows 10 sonbaharda oluşturucuları güncelleştirmesi, veya
>    * Windows Server (yapı 16299) 1709 veya
>    * X64 tabanlı bir cihazda Windows IOT Core (yapı 16299)
>
> Windows IOT Core için yönergeleri izleyin [Windows IOT Core üzerinde IOT kenar çalışma zamanı yükleme][lnk-install-iotcore]. Aksi takdirde, sadece [Windows kapsayıcılar kullanmak için Docker yapılandırmak][lnk-docker-containers]ve isteğe bağlı olarak önkoşulların aşağıdaki powershell komutuyla doğrulayabilirsiniz:
>    ```powershell
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```

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

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>IOT kenar çalışma zamanı yapılandırma

IOT kenar çalışma zamanı, tüm IOT kenar aygıtlarda dağıtılır. İki modülden oluşur. İlk olarak, IOT kenar Aracısı dağıtımı ve IOT sınır cihazı modülleri izlenmesini kolaylaştırır. İkinci olarak, IOT kenar hub IOT sınır cihazı modülleri arasında ve cihaz IOT hub'ı arasındaki iletişim yönetir. 

Çalışma zamanı IOT kenar cihaz bağlantı dizenizi önceki bölümdeki yapılandırın.

```cmd
iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
```

Çalışma zamanı başlatın.

```cmd
iotedgectl start
```

Docker IOT kenar Aracısı'nı bir modül olarak çalışıp çalışmadığını denetleyin.

```cmd
docker ps
```

![Docker edgeAgent bakın](./media/tutorial-simulate-device-windows/docker-ps.png)

## <a name="deploy-a-module"></a>Bir modül dağıtma

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

Bu hızlı başlangıç yeni bir IOT sınır cihazı oluşturan ve IOT kenar çalışma zamanı yüklü. Ardından, cihaz için değişiklik yapmak zorunda kalmadan cihazda çalıştırmak için bir IOT kenar modülü göndermek için Azure portal kullanılır. Bu durumda, gönderilen modülü öğreticileri için kullanabileceğiniz çevresel veri oluşturur. 

Sanal cihazınız yeniden çalıştıran bilgisayarda komut istemi açın. Buluttan dağıtılan modülü IOT kenar aygıtınızda çalışır durumda olduğunu doğrulayın. 

```cmd
docker ps
```

![Cihazınızda üç modüller görünümü](./media/tutorial-simulate-device-windows/docker-ps2.png)

TempSensor modülünden buluta gönderilen iletiler görüntüleyin. 

```cmd
docker logs -f tempSensor
```

![Modülünüzün verileri görüntüleme](./media/tutorial-simulate-device-windows/docker-logs.png)

Cihaz kullanarak göndermeyi telemetriyi de görüntüleyebilirsiniz [IOT hub'ı explorer aracı][lnk-iothub-explorer]. 
## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz IOT Hub artık ihtiyacınız olduğunda kullanabileceğiniz [az IOT hub delete] [ lnk-delete] kaynak ve onunla ilişkili tüm aygıtları kaldırmak için komutu:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Sonraki adımlar

Bir IOT kenar modülünü IOT kenar cihazına dağıtmak öğrendiniz. Böylece veri kenarına çözümleyebilirsiniz şimdi farklı türlerdeki modüllerle, Azure Hizmetleri dağıtmayı deneyin. 

* [Bir modül olarak Azure işlevi dağıtma](tutorial-deploy-function.md)
* [Azure Stream Analytics’i modül olarak dağıtma](tutorial-deploy-stream-analytics.md)
* [Kendi kodunuzu bir modül olarak dağıtma](tutorial-csharp-module.md)


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
