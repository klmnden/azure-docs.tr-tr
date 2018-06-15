---
title: Windows Azure Service Fabric Linux kümesi ayarlama | Microsoft Docs
description: Bu makalede, Windows geliştirme makinelerde çalışan Service Fabric Linux kümeleri ayarlama alınmaktadır. Bu platformlar geliştirme için yararlıdır.
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: mfussell
editor: ''
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/20/2017
ms.author: suhuruli
ms.openlocfilehash: ad0d383888c173ece5a7fbd3b0de690ed13074f7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206302"
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Windows Geliştirici makinenizde Linux Service Fabric kümesi ayarlayın

Bu belge Windows geliştirme makinelerde yerel bir Linux Service Fabric ayarlamak nasıl ele alınmaktadır. Yerel Linux kümesi ayarlama hızlı bir şekilde Linux kümeleri için hedeflenen uygulamaları test etmek kullanışlıdır ancak bir Windows makinesinde geliştirilir.

## <a name="prerequisites"></a>Önkoşullar
Linux tabanlı Service Fabric kümeleri Windows üzerinde yerel olarak çalıştırmayın. Yerel bir Service Fabric kümesi çalıştırmak için önceden yapılandırılmış bir Docker kapsayıcısı görüntü sağlanır. Başlamadan önce şunlar gereklidir:

* En az 4 GB RAM
* [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)'ın en son sürümü

>[!TIP]
> * Resmi Docker içinde belirtilen adımları takip edebilirsiniz [belgelerine](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions) Docker, Windows yüklemek için. 
> * Yüklemeyi tamamladıktan sonra, [burada](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine) anlatılan adımları izleyerek yüklemeyi doğru yaptığınızı onaylayın


## <a name="create-a-local-container-and-setup-service-fabric"></a>Yerel bir kapsayıcı oluşturma ve Service Fabric’i ayarlama
Yerel bir Docker kapsayıcısı ayarlama ayarlamak ve üzerini çalıştıran bir service fabric kümesi sağlamak için PowerShell içinde aşağıdaki adımları gerçekleştirin:

1. Docker merkez deposundan görüntü çekme:

    ```powershell
    docker pull microsoft/service-fabric-onebox
    ```

2. Ana bilgisayarınızda Docker daemon yapılandırmasını aşağıdakiyle güncelleştirin ve Docker daemon programını yeniden başlatın: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    Güncelleştirmek için tavsiye edilir - Docker simgesine Git yoludur > Ayarlar > arka plan programı > Gelişmiş ve orada güncelleştirme. Ardından, Docker arka plan programı değişikliklerin etkili olması yeniden başlatın. 

3. Görüntü ile bir Service Fabric One-box kapsayıcı örneği başlatın:

    ```powershell
    docker run -itd -p 19080:19080 --name sfonebox microsoft/service-fabric-onebox
    ```
    >[!TIP]
    > * Kapsayıcı örneğinize için bir ad belirterek, örneğinizi daha okunaklı bir biçimde işleyebilirsiniz. 
    > * Uygulamanız belirli bağlantı noktalarını dinliyorsa, ek -p etiketleri kullanılarak belirtilmelidir. Örneğin, uygulamanızın 8080 bağlantı noktasında dinleme, çalışma docker çalıştırırsanız - itd -p 19080:19080 -p 8080:8080--ad sfonebox microsoft/service-doku-onebox

4. Etkileşimli ssh modunda Docker kapsayıcısı oturumunu açın:

    ```powershell
    docker exec -it sfonebox bash
    ```

5. Gerekli bağımlılıkları getirecek olan ayar betiğini çalıştırın ve ondan sonra kümeyi kapsayıcı üzerinde başlatın.

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

6. 5. adım başarıyla tamamlandıktan sonra gidebilirsiniz ``http://localhost:19080`` , Windows ve, Service Fabric explorer görmeye olacaktır. Bu noktada, Windows Geliştirici makinenizden herhangi bir aracı kullanarak bu kümeye bağlanın ve Linux Service Fabric kümeleri için hedeflenen uygulamayı dağıtın. 

    > [!NOTE]
    > Eclipse eklentisi Windows üzerinde şu anda desteklenmemektedir. 

## <a name="next-steps"></a>Sonraki adımlar
* Kullanmaya başlama [Eclipse](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-eclipse)
* Diğer denetleyin [Java örnekleri](https://github.com/Azure-Samples/service-fabric-java-getting-started)


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png