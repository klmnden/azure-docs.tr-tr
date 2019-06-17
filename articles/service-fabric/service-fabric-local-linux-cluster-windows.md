---
title: Windows üzerinde Azure Service Fabric Linux kümesi ayarlama | Microsoft Docs
description: Bu makalede, Windows geliştirme makineler üzerinde çalışan Service Fabric Linux kümelerinde ayarlama ele alınmaktadır. Bu platform geliştirme için yararlıdır.
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
ms.openlocfilehash: e700250a6ebcdb82f99c1b460a510811d7ceb96c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60719949"
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Windows Geliştirici makinenizde Linux Service Fabric kümesi ayarlama

Bu belge, Windows geliştirme makinelerinde yerel Linux Service Fabric ayarlama ele alınmaktadır. Yerel Linux kümesi ayarlama, hızlı bir şekilde Linux kümeleri için hedeflenen uygulamaları test etmek kullanışlıdır ancak bir Windows makinede geliştirilir.

## <a name="prerequisites"></a>Önkoşullar
Service Fabric Linux tabanlı kümeler, Windows üzerinde yerel olarak çalışmaz. Yerel bir Service Fabric kümesini çalıştırmak için önceden yapılandırılmış bir Docker kapsayıcı görüntüsü sağlanır. Başlamadan önce şunlar gereklidir:

* En az 4 GB RAM
* [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)'ın en son sürümü
* Docker, Linux modunda çalıştırılması gerekir

>[!TIP]
> * Resmi Docker belirtilen adımları takip edebilirsiniz [belgeleri](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions) , Windows Docker yüklemek için. 
> * Yüklemeyi tamamladıktan sonra, [burada](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine) anlatılan adımları izleyerek yüklemeyi doğru yaptığınızı onaylayın


## <a name="create-a-local-container-and-setup-service-fabric"></a>Yerel bir kapsayıcı oluşturma ve Service Fabric’i ayarlama
Yerel bir Docker kapsayıcısı ayarlamak ve üzerinde çalıştırılan bir service fabric kümesi için PowerShell'de aşağıdaki adımları uygulayın:


1. Ana bilgisayarınızda Docker daemon yapılandırmasını aşağıdakiyle güncelleştirin ve Docker daemon programını yeniden başlatın: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    Güncelleştirmek için tavsiye edilir - Docker simgesi Git yoludur > Ayarlar > Daemon > Gelişmiş ve orada güncelleştirin. Ardından, değişikliklerin etkili olması Docker Daemon programını yeniden başlatın. 

2. Service Fabric Görüntünüzü derlemek için yeni bir dizinde `Dockerfile` adlı bir dosya oluşturun:

    ```Dockerfile
    FROM microsoft/service-fabric-onebox
    WORKDIR /home/ClusterDeployer
    RUN ./setup.sh
    #Generate the local
    RUN locale-gen en_US.UTF-8
    #Set environment variables
    ENV LANG=en_US.UTF-8
    ENV LANGUAGE=en_US:en
    ENV LC_ALL=en_US.UTF-8
    EXPOSE 19080 19000 80 443
    #Start SSH before running the cluster
    CMD /etc/init.d/ssh start && ./run.sh
    ```

    >[!NOTE]
    >Kapsayıcınıza ek programlar veya bağımlılıklar eklemek için bu dosyayı uyarlayabilirsiniz.
    >Örneğin, `RUN apt-get install nodejs -y` komutu eklendiğinde, konuk yürütülebilir dosyaları olarak `nodejs` uygulamaları için destek sağlanır.
    
    >[!TIP]
    > Varsayılan olarak bu, görüntüyü Service Fabric’in en son sürümüyle çeker. Belirli düzeltmeler için lütfen [Docker Hub](https://hub.docker.com/r/microsoft/service-fabric-onebox/) sayfasını ziyaret edin

3. `Dockerfile` içinden yeniden kullanılabilir görüntünüzü derlemek için bir terminal açın ve doğrudan `Dockerfile` öğesini tutarak `cd` uygulayıp sonra şunu çalıştırın:

    ```powershell 
    docker build -t mysfcluster .
    ```
    
    >[!NOTE]
    >Bu işlem biraz zaman alır, ancak yalnızca bir kez gereklidir.

4. Şimdi aşağıdakini çalıştırarak her ihtiyaç duyduğunuzda hızla yerel bir Service Fabric kopyası başlatabilirsiniz:

    ```powershell 
    docker run --name sftestcluster -d -v //var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mysfcluster
    ```

    >[!TIP]
    >Kapsayıcı örneğiniz için bir ad belirterek, örneğinizin daha okunaklı bir biçimde işlenebilmesini sağlayın. 
    >
    >Uygulamanız belirli bağlantı noktalarını dinliyorsa, bağlantı noktaları ek `-p` etiketleri kullanılarak belirtilmelidir. Örneğin, uygulamanız 8080 bağlantı noktasını dinliyorsa, şuradaki `-p` etiketini ekleyin:
    >
    >`docker run -itd -p 19080:19080 -p 8080:8080 --name sfonebox microsoft/service-fabric-onebox`
    >

5. Kümenin başlatılması kısa bir süre sürer, aşağıdaki komutu kullanarak günlükleri görüntüleyebilir veya [http://localhost:19080](http://localhost:19080) küme durumunu görüntülemek için panoya atlayabilirsiniz:

    ```powershell 
    docker logs sftestcluster
    ```

6. Adım 5 başarıyla tamamlandıktan sonra gidebilirsiniz ``http://localhost:19080`` , Windows ve, Service Fabric gezginini görebilirsiniz olacaktır. Bu noktada, Windows Geliştirici makinenizde herhangi bir aracı kullanarak bu kümeye bağlanın ve Linux Service Fabric kümeleri için hedeflenen uygulamayı dağıtma. 

    > [!NOTE]
    > Eclipse eklentisi Windows üzerinde şu anda desteklenmemektedir. 

7. İşiniz bittiğinde, durdurun ve şu komutla kapsayıcıyı temizleme:

    ```powershell 
    docker rm -f sftestcluster
    ```

### <a name="known-limitations"></a>Bilinen Sınırlamalar 
 
 Mac’e yönelik kapsayıcıdaki yerel küme çalıştırmaya ilişkin bilinen sınırlandırmalar aşağıda verilmiştir: 
 
 * DNS hizmeti çalışmıyor ve desteklenmiyor [Sorun No. 132](https://github.com/Microsoft/service-fabric/issues/132)

## <a name="next-steps"></a>Sonraki adımlar
* Kullanmaya başlama [Eclipse](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-eclipse)
* Diğer [Java örnekleri](https://github.com/Azure-Samples/service-fabric-java-getting-started)


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
