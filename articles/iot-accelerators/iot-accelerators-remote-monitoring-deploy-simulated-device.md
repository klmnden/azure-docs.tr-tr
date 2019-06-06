---
title: Özel sanal cihazlar - Azure IOT dağıtma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda, özel sanal cihazlar için Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/15/2018
ms.topic: conceptual
ms.openlocfilehash: 7cbab38db859935c9f4490d79a131d6c9a7e302b
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427574"
---
# <a name="deploy-a-new-simulated-device"></a>Yeni bir simülasyon cihazı dağıtma

Uzaktan izleme ve cihaz benzetimi Çözüm Hızlandırıcıları her iki sanal cihazlarınızı tanımlamanıza olanak sağlar. Bu makalede bir özelleştirilmiş Soğutucu cihaz türü ve yeni bir ampul cihaz türü için Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir.

Bu makaledeki adımları tamamladığınızdan varsayar [oluşturma ve test yeni bir simülasyon cihazı](iot-accelerators-remote-monitoring-create-simulated-device.md) nasıl yapılır Kılavuzu ve özelleştirilmiş Soğutucu ve yeni ampul cihaz türlerini tanımlayan dosyalarınız.

Bu nasıl yapılır kılavuzunda adımlarda size yol gösterecektir için:

1. Uzaktan izleme çözüm Hızlandırıcısını sanal makinesinin dosya sistemine erişim için SSH kullanın.

1. Cihaz modelleri Docker kapsayıcısı dışında bir konumdan yüklemek için Docker'ı yapılandırın.

1. Özel cihaz modeli dosyalarını kullanarak Uzaktan izleme çözüm Hızlandırıcısını çalıştırma.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için etkin bir Azure aboneliği gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda takip etmek için ihtiyacınız vardır:

- Dağıtılan bir örneğini [Uzaktan izleme çözüm hızlandırıcısının](https://www.azureiotsolutions.com/Accelerators#solutions/types/RM2).
- Yerel bir **bash** çalıştırılacak Kabuk `ssh` ve `scp` komutları. Windows, yüklemek için kolay bir yol üzerinde **bash** yüklemektir [git](https://git-scm.com/download/win).
- Açıklanan olanlar gibi özel cihaz model dosyalarınızı [oluşturma ve test yeni bir simülasyon cihazı](iot-accelerators-remote-monitoring-create-simulated-device.md).

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="configure-docker"></a>Docker'ı yapılandırma

Bu bölümde, yapılandırdığınız cihaz modeli dosyalarından yüklemek için Docker **/tmp/devicemodels** klasör sanal makinede yerine Docker kapsayıcısı içinde. İçinde bu bölümdeki komutları çalıştırmanız bir **bash** Kabuk yerel makinenizde:

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
    devicesimulation:
      image: azureiotpcs/device-simulation-dotnet:1.0.0
      networks:
        - default_net
      depends_on:
        - storageadapter
      environment:
        - PCS_KEYVAULT_NAME
        - PCS_AAD_APPID
        - PCS_AAD_APPSECRET
      # How one could mount custom device models
      volumes:
        - /tmp/devicemodels:/app/webservice/data/devicemodels:ro
    ```

    Değişiklikleri kaydedin.

1. Mevcut cihaz modeli dosyaları kapsayıcısından yeni bir konuma kopyalayın. İlk olarak, kapsayıcı kimliği için cihaz benzetimi kapsayıcı bulun:

    ```sh
    sudo docker ps
    ```

    Ardından cihaz modeli dosyaları kopyalayın **tmp** sanal makinede klasör. Aşağıdaki komut, kapsayıcı kimliği c378d6878407--bu değeri cihaz benzetimi kapsayıcı Kimliğinizle değiştirin varsayar:

    ```sh
    sudo docker cp c378d6878407:/app/webservice/data/devicemodels /tmp
    sudo chown -R azureuser /tmp/devicemodels/
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
    sudo docker ps
    ```

    Cihaz benzetimi kapsayıcısı günlüğünden görmek istiyorsanız, aşağıdaki komutu çalıştırın. Kapsayıcı kimliği, cihaz benzetimi kapsayıcı kimliği ile değiştirin:

    ```sh
    sudo docker logs -f 5d3f3e78822e
    ```

## <a name="run-simulation"></a>Simülasyon çalıştırma

Uzaktan izleme çözümünde özel cihaz Modellerinizi artık kullanabilirsiniz:

1. Uzaktan izleme panonuzdan başlatma [Microsoft Azure IOT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators#dashboard).

1. Kullanım **cihazları** sanal cihaz eklemek için sayfa. Yeni bir simülasyon cihazı eklediğinizde, yeni cihaz Modellerinizi seçmek kullanılabilir.

1. Cihaz telemetrisini görüntüleme ve cihaz yöntemleri çağırmak için panoyu kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha fazla keşfetmeye devam etmeyi planlıyorsanız, dağıtılan Uzaktan izleme çözüm Hızlandırıcısını bırakın.

Çözüm Hızlandırıcısını artık ihtiyacınız kalmadığında, silmek [sağlanan çözümleri](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasında, seçerek ve ardından **Sil çözüm**.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, özel cihaz modelleri için Uzaktan izleme çözüm Hızlandırıcısını dağıtma nasıl oluşturulacağını gösterir. Önerilen sonraki adıma öğrenmektir nasıl [gerçek bir cihaz, Uzaktan izleme çözümüne bağlama](iot-accelerators-connecting-devices-node.md).
