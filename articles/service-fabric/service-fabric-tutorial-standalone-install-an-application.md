---
title: 'Öğretici: Tek başına Service Fabric kümenize uygulama yükleme - Azure Service Fabric | Microsoft Docs'
description: Bu öğreticide, tek başına Service Fabric kümesine uygulama yükleme konusunda bilgi edinin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2018
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 5bc326bbc16ef93d484425f26b6f8226150c77c6
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66302431"
---
# <a name="tutorial-deploy-an-application-on-your-service-fabric-standalone-cluster"></a>Öğretici: Service Fabric tek başına kümenizde bir uygulamayı dağıtma

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici serisinde, AWS üzerinde barındırılan tek başına küme oluşturma ve bir uygulamasına dağıtırsınız.

Bu öğretici, bir serinin üçüncü bölümüdür.  Service Fabric tek başına kümeler ortamınızı seçin ve "Herhangi bir işletim sistemi, herhangi bir buluttaki" yaklaşımımız ile Service Fabric'i bir parçası olarak küme oluşturma seçeneğini sunar. Bu öğreticide, bu tek başına kümeyi barındırmak için gereken AWS altyapısını oluşturma işlemi gösterilmektedir.

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Örnek uygulamayı indirme
> * Kümeye dağıtma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* [Visual Studio 2019 yükleme](https://www.visualstudio.com/) yükleyip **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleri.
* [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

### <a name="deploy-the-app-to-the-service-fabric-cluster"></a>Uygulamayı Service Fabric kümesine dağıtma

Uygulama indirildikten sonra, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

1. Visual Studio’yu açın

2. **Dosya** > **Aç**’ı seçin

3. Git deposunu kopyaladığınız klasöre gidin ve Voting.sln dosyasını seçin

4. Çözüm Gezgini'nde `Voting` uygulama projesine sağ tıklayın ve **Yayımla**’yı seçin

5. **Bağlantı Uç Noktası** açılır listesini seçin ve kümenizdeki düğümlerden birinin genel DNS Adını girin.  Örneğin, `ec2-34-215-183-77.us-west-2.compute.amazonaws.com:19000`

6. Tercih ettiğiniz tarayıcıyı açın ve küme adresini girin (bu uygulamanın 8080 numaralı bağlantı noktasında dağıttığı bağlantı uç noktası - örneğin, ec2-34-215-183-77.us-west-2.compute.amazonaws.com:8080).

    ![Kümeden API Yanıtı](./media/service-fabric-tutorial-standalone-cluster/deployed-app.png)

## <a name="next-steps"></a>Sonraki adımlar

Serinin üçüncü bölümünde, kümenize bir uygulamanın nasıl dağıtılacağı öğrendiniz:

> [!div class="checklist"]
> * Örnek uygulamayı indirme
> * Kümeye dağıtma

Kümenizi temizlemek için serinin dördüncü bölümüne ilerleyin.

> [!div class="nextstepaction"]
> [Kaynaklarınızı temizleme](service-fabric-tutorial-standalone-clean-up.md)