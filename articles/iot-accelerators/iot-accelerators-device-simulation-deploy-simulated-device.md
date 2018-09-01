---
title: Özel sanal cihazlar - Azure IOT dağıtma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda, özel sanal cihazlar için cihaz benzetimi çözüm Hızlandırıcısını dağıtma işlemini göstermektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/14/2018
ms.topic: conceptual
ms.openlocfilehash: e51d0330c3ed804bff8cdb06265bb8c39141733f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345122"
---
# <a name="deploy-a-new-simulated-device"></a>Yeni bir simülasyon cihazı dağıtma

Uzaktan izleme ve cihaz benzetimi Çözüm Hızlandırıcıları her iki sanal cihazlarınızı tanımlamanıza olanak sağlar. Bu makalede bir özelleştirilmiş Soğutucu cihaz türü ve yeni bir ampul cihaz türü için cihaz benzetimi çözüm Hızlandırıcısını dağıtmayı gösterir.

Bu makaledeki adımları tamamladığınızdan varsayar [oluşturma ve test yeni bir simülasyon cihazı](iot-accelerators-remote-monitoring-create-simulated-device.md) nasıl yapılır Kılavuzu ve özelleştirilmiş Soğutucu ve yeni ampul cihaz türlerini tanımlayan dosyalarınız.

Bu nasıl yapılır kılavuzunda adımlarda size yol gösterecektir için:

1. Cihaz benzetimi çözüm Hızlandırıcısını sanal makinesinin dosya sistemine erişim için SSH kullanın.

1. Cihaz modelleri Docker kapsayıcısı dışında bir konumdan yüklemek için Docker'ı yapılandırın.

1. Özel cihaz modeli dosyalarını kullanarak bir simülasyon çalıştırın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için etkin bir Azure aboneliği gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda takip etmek için ihtiyacınız vardır:

- Dağıtılan bir örneğini [cihaz benzetimi Çözüm Hızlandırıcısı](https://www.azureiotsolutions.com/Accelerators#solutions/types/DS).
- Yerel bir **bash** çalıştırılacak Kabuk `ssh` ve `scp` komutları. Windows, yüklemek için kolay bir yol üzerinde **bash** yüklemektir [git](https://git-scm.com/download/win).
- Açıklanan olanlar gibi özel cihaz model dosyalarınızı [oluşturma ve test yeni bir simülasyon cihazı](iot-accelerators-remote-monitoring-create-simulated-device.md).

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="configure-docker"></a>Docker'ı yapılandırma

Bu bölümde, yapılandırdığınız cihaz modeli dosyalarından yüklemek için Docker **/tmp/devicemodels** klasör sanal makinede yerine Docker kapsayıcısı içinde. İçinde bu bölümdeki komutları çalıştırmanız bir **bash** Kabuk yerel makinenizde:

1. Yerel makinenizden azure'daki sanal makineye bağlanmak için SSH kullanın. Aşağıdaki komut, sanal makinenin genel IP adresini varsayar **vm vikxv** olduğu **104.41.128.108** --bu değeri, önceki bölümde, sanal makinenizin genel IP adresiyle değiştirin:

   ```sh
    ssh azureuser@104.41.128.108
    ```

    Sanal makine önceki bölümde belirlediğiniz parola ile oturum açmak için yönergeleri izleyin.

1. Kapsayıcının dışından cihaz modellerinden yüklemek için cihaz benzetimi hizmetini yapılandırın. İlk olarak Docker yapılandırma dosyasını açın:

    ```sh
    sudo nano /app/docker-compose.yml
    ```

    Bulmak için ayarları **devicesimulation** kapsayıcı ve düzenleme **birimleri** aşağıdaki kod parçacığında gösterildiği gibi ayarlama:

    ```yml
    # To mount custom device models, upload the files to the VM, edit and uncomment the following line:
          - /tmp/devicemodels:/app/webservice/data/devicemodels:ro
    # To mount a custom service configuration, create a custom file, edit and uncomment the following line:
    #     - /app/custom-device-simulation-appsettings.ini:/app/webservice/appsettings.ini:ro
    ```

    Değişiklikleri kaydedin.

1. JSON ve komut dosyaları depolamak için bir klasör oluşturun:

    ```sh
    sudo mkdir -p /tmp/devicemodels/scripts
    ```

    Tutun **bash** SSH oturumunuzun açık penceresiyle.

1. Özel cihaz model dosyalarınızı sanal makinesine kopyalayın. Başka bir programda bu komutu çalıştırmak **bash** Kabuk özel cihaz Modellerinizi oluşturulduğu makine üzerinde. İlk olarak, cihaz modeli JSON dosyalarını içeren yerel klasöre gidin. Aşağıdaki komutlar sanal makinenin genel IP adresidir varsayar **104.41.128.108** --bu değeri, sanal makinenizin genel IP adresiyle değiştirin. İstendiğinde, sanal makine parolası girin:

    ```sh
    scp *json azureuser@104.41.128.108:/tmp/devicemodels
    cd scripts
    scp *js azureuser@104.41.128.108:/tmp/devicemodels/scripts
    ```

1. Yeni cihaz modelini kullanmak için cihaz benzetimi Docker kapsayıcısı yeniden başlatın. Aşağıdaki komutları çalıştırın **bash** Kabuk ile sanal makineye SSH oturum aç:

    ```sh
    sudo /app/start.sh
    ```

    Çalışan Docker kapsayıcıları ve onların kapsayıcı kimlikleri durumunu görmek istiyorsanız, aşağıdaki komutu kullanın:

    ```sh
    docker ps
    ```

    Cihaz benzetimi kapsayıcısı günlüğünden görmek istiyorsanız, aşağıdaki komutu çalıştırın. Kapsayıcı kimliği, cihaz benzetimi kapsayıcı kimliği ile değiştirin:

    ```sh
    docker logs -f 5d3f3e78822e
    ```

## <a name="run-simulation"></a>Simülasyon çalıştırma

Artık, özel cihaz modelleri kullanarak bir simülasyon çalıştırabilirsiniz:

1. Cihaz benzetim web kullanıcı Arabiriminden başlatma [Microsoft Azure IOT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators#dashboard).

1. Web kullanıcı Arabirimi, yapılandırmak ve özel cihaz Modellerinizi birini kullanarak bir simülasyon çalıştırma için kullanın.

1. Bir simülasyon çalıştırdığınızda tıklayın **Azure portalındaki IOT Hub'ı görünümü ölçümleri** benzetim izlemek için. Alternatif olarak, cihazın trafiği bulut izlemek için aşağıdaki Azure CLI komutunu kullanabilirsiniz. IOT hub'ı adı hub adınız ile değiştirin:

    ```azurecli-interactive
    az iot hub monitor-events -n contoso-simulation9dc75
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha fazla incelemeyi planlıyorsanız, Cihaz Benzetimi çözüm hızlandırıcısını dağıtımda bırakın.

Çözüm Hızlandırıcısını artık ihtiyacınız kalmadığında, silmek [sağlanan çözümleri](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasında, seçerek ve ardından **Sil çözüm**.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, özel cihaz modelleri için cihaz benzetimi çözüm Hızlandırıcısını dağıtma nasıl oluşturulacağını gösterir. Hakkında daha fazla bilgi edinmek için önerilen sonraki adım olan [cihaz modeli şemasını](iot-accelerators-device-simulation-device-schema.md).
