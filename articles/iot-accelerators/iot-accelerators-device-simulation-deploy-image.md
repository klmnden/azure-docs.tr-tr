---
title: Özel bir cihaz benzetimi görüntü - Azure'ı dağıtma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda Azure'a özel bir Docker görüntüsü cihaz benzetimi çözüm dağıtmayı öğrenin.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/06/2018
ms.author: dobett
ms.openlocfilehash: c1f321f452b65016c11cb66d08ebab108509cc62
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61448411"
---
# <a name="deploy-a-custom-device-simulation-docker-image"></a>Bir özel cihaz benzetimi docker görüntüsü dağıtma

Özel özellikler eklemek için cihaz benzetimi çözüm değiştirebilirsiniz. Örneğin, [protokol arabellekleri kullanarak telemetri serileştirmek](iot-accelerators-device-simulation-protobuf.md) makale telemetri göndermek için protokol Arabellekleri'ni (Protobuf) kullanan çözüme özel bir cihaz eklemek nasıl gösterir. Değişikliklerinizi yerel olarak test ettikten sonra sonraki adıma değişikliklerinizi azure'da cihaz benzetimi Örneğinize dağıtmaktır. Bu görevi tamamlamak için değiştirilen hizmeti içeren bir Docker görüntüsü oluşturup gerekir.

Nasıl, bu kılavuzdaki adımları Yardım-How-to-göstermek için:

1. Geliştirme ortamınızı hazırlama
1. Yeni bir Docker görüntüsü oluştur
1. Cihaz benzetimi, yeni Docker görüntünüzü kullanmak için yapılandırma
1. Yeni görüntüyü kullanarak bir simülasyon çalıştırma

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için gerekir:

* Dağıtılmış [cihaz benzetimi](quickstart-device-simulation-deploy.md) örneği.
* Docker. İndirme [Docker Community Edition](https://www.docker.com/products/docker-engine#/download) platformunuz için.
* A [Docker Hub hesabı](https://hub.docker.com/) karşıya yüklersiniz Docker görüntülerinizi. Docker Hub hesabınıza adlı ortak bir depo oluşturma **cihaz benzetimi**.
* Değiştirilebilir ve test edilmiş bir [cihaz benzetimi çözüm](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) yerel makinenizde. Örneğin, çözüme değiştirebilirsiniz [protokol arabellekleri kullanarak telemetri serileştirmek](iot-accelerators-device-simulation-protobuf.md).
* Bir kabuk SSH çalıştırabilirsiniz. Git için Windows yüklerseniz, kullanabileceğiniz **bash** th yüklemesinin bir parçası olan Kabuğu. Ayrıca, [Azure Cloud Shell](https://shell.azure.com/).

Bu makaledeki yönergeler, Windows kullanmakta olduğunuz varsayılır. Başka bir işletim sistemi kullanıyorsanız, bazı dosya yolları ve komutları ortamınıza uyacak şekilde ayarlamanız gerekebilir.

## <a name="create-a-new-docker-image"></a>Yeni bir Docker görüntüsü oluşturma

Kendi değişiklikleri ve cihaz benzetimi hizmeti dağıtmak için derleme ve dağıtım betiklerini düzenlemeniz gerekir **scripts\docker** kapsayıcılar, docker hub hesabınıza yüklemek için klasör

### <a name="modify-the-docker-scripts"></a>Docker komut dosyalarını değiştirme

Docker değiştirme **build.cmd**, **publish.cmd**, ve **run.cmd** içindeki komutlar **scripts\docker** Docker hub'ınızla klasörü Havuz bilgileri. Bu adımlarda adlı ortak bir depoya oluşturduğunuz varsayılır **cihaz benzetimi**:

`DOCKER_IMAGE={your-docker-hub-username}/device-simulation`

Güncelleştirme **docker-compose.yml** aşağıdaki gibi:

`image: {your-docker-hub-username}/device-simulation`

### <a name="configure-the-solution-to-include-any-new-files"></a>Tüm yeni dosyaları eklenip eklenmeyeceğini çözümünüzü yapılandırın

Yeni cihaz modeli dosyalarını eklediyseniz, açıkça çözüme dahil etmek gerekir. Bir girdiyi **services/services.csproj** dahil etmek her dosya için. Örneğin, tamamlandı, [protokol arabellekleri kullanarak telemetri serileştirmek](iot-accelerators-device-simulation-protobuf.md) yapılır, aşağıdaki girişleri ekleyin:

```xml
<None Update="data\devicemodels\assettracker-01.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
<None Update="data\devicemodels\scripts\assettracker-01-state.js">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
```

### <a name="generate-new-docker-images-and-push-to-docker-hub"></a>Yeni Docker görüntüleri oluşturun ve Docker hub'a gönderme

Yeni Docker görüntüsünü Docker hub'a docker hub hesabınızı kullanarak yayımlama:

1. Bir komut istemi açın ve yerel cihaz benzetimi depo kopyasına gidin.

1. Gidin **docker** klasörü:

    ```cmd
    cd scripts\docker
    ```

1. Docker görüntüsünü oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd
    build.cmd
    ```

1. Docker görüntüsünü Docker hub'a deponuza yayımlamak için aşağıdaki komutu çalıştırın. Docker için Docker hub'ı kimlik bilgilerinizle oturum açın:

    ```cmd
    docker login
    publish.cmd
    ```

<!-- TODO fix heading levels working include -->

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="update-the-service"></a>Güncelleştirme hizmeti

Özel görüntü kullanmak için cihaz benzetimi kapsayıcısını güncelleştirmek için aşağıdaki adımları tamamlayın:

* Cihaz benzetimi örneğinizin barındırma sanal makineye bağlanmak için SSH kullanın. IP adresi ve parola önceki bölümde Not yapılan kullanın:

    ```sh
    ssh azureuser@{your vm ip address}
    ```

* Gidin **/app** dizini:

    ```sh
    cd /app
    ```

* Düzen **docker-compose.yml** dosyası:

    ```sh
    sudo nano docker-compose.yml
    ```

    Değiştirme **görüntü** özel işaret edecek şekilde **cihaz benzetimi** Docker Hub deponuzu karşıya görüntü:

    ```yml
    image: {your-docker-hub-username}/device-simulation
    ```

    Yaptığınız değişiklikleri kaydedin.

* Mikro hizmetleri yeniden başlatmak için aşağıdaki komutu çalıştırın:

    ```sh
    sudo start.sh
    ```

## <a name="run-your-simulation"></a>Benzetim çalıştırın

Artık, özelleştirilmiş cihaz benzetimi çözümünüzü kullanarak bir simülasyon çalıştırabilirsiniz:

1. Cihaz benzetim web kullanıcı Arabiriminden başlatma [Microsoft Azure IOT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators#dashboard).

1. Web kullanıcı Arabirimi, yapılandırmak ve bir simülasyon çalıştırma için kullanın. Daha önce tamamlanırsa [protokol arabellekleri kullanarak telemetri serileştirmek](iot-accelerators-device-simulation-protobuf.md), özel cihaz modelinizi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Özel bir cihaz benzetimi görüntüsünü dağıtmayı öğrendiniz. artık edinmek isteyebilir nasıl [mevcut bir IOT hub ile cihaz benzetimi çözüm Hızlandırıcı kullanmak](iot-accelerators-device-simulation-choose-hub.md).