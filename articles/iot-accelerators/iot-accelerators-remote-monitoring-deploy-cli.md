---
title: CLI - Azure'ı kullanarak Uzaktan izleme çözümü dağıtın | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda CLI kullanarak Uzaktan izleme çözüm Hızlandırıcısını sağlamak nasıl gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: ea96b2b996ea79efacdcda50c6370f25e26e0aa2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447021"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>CLI kullanarak Uzaktan izleme çözüm Hızlandırıcısını dağıtma

Bu nasıl yapılır kılavuzunda, Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir. CLI kullanarak çözümü dağıtın. Ayrıca bu seçeneği bakın hakkında bilgi edinmek için azureiotsolutions.com web tabanlı kullanıcı Arabirimi kullanarak çözümü dağıtabilirsiniz [Uzaktan izleme çözüm Hızlandırıcısını dağıtma](quickstart-remote-monitoring-deploy.md) hızlı başlangıç.

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını dağıtma için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).

CLI'yı çalıştırmak için ihtiyacınız [Node.js](https://nodejs.org/) yerel makinenizde yüklü.

## <a name="install-the-cli"></a>CLI'yi yükleme

CLI'yı yüklemek için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>CLI için oturum açın

Çözüm Hızlandırıcısını dağıtmadan önce CLI kullanarak Azure aboneliğinizde oturum açmanız gerekir:

```cmd/sh
pcs login
```

İzleyin ekran oturum açma işlemini tamamlamak için yönergeleri.

## <a name="deployment-options"></a>Dağıtım seçenekleri

Çözüm Hızlandırıcısını dağıttığınızda, dağıtım işlemi yapılandırma birkaç seçenek vardır:

| Seçenek | Değerler | Açıklama |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | A _temel_ dağıtım, test ve Tanıtımlar için tasarlanmıştır, tek bir sanal makine için tüm mikro Hizmetleri dağıtır. A _standart_ dağıtım çeşitli sanal makinelere mikro hizmetler dağıttığı üretim için tasarlanmıştır. A _yerel_ dağıtım mikro Hizmetleri yerel makinenizde çalıştırmak için bir Docker kapsayıcısı yapılandırır ve Azure bulut Hizmetleri, depolama ve Cosmos DB gibi kullanır. |
| Çalışma Zamanı | `dotnet`, `java` | Mikro hizmetler dil uygulamasını seçer. |

Yerel dağıtım seçeneğinin nasıl kullanılacağını öğrenmek için bkz: [Uzaktan izleme çözümü yerel olarak çalışan](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="basic-and-standard-deployments"></a>Temel ve standart dağıtım

Bu bölümde, temel ve standart bir dağıtım arasındaki temel farklılıklar özetlenmiştir.

### <a name="basic"></a>Temel

Temel bir dağıtımdan yapabileceğiniz [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) veya CLI kullanarak.

Temel dağıtım, çözümü sergilemeye yöneliktir. Maliyetleri azaltmak için tüm mikro hizmetler tek bir sanal makine dağıtılır. Bu dağıtım bir üretim ortamına hazır mimarisi kullanmaz.

Temel dağıtımı aşağıdaki hizmetleri Azure aboneliğinize oluşturur:

| Count | Resource                       | Tür         | İçin kullanılan |
|-------|--------------------------------|--------------|----------|
| 1     | [Linux sanal makinesi](https://azure.microsoft.com/services/virtual-machines/) | Standard D1 V2  | Mikro hizmet barındırma |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1 – standart katman | Cihaz yönetimi ve iletişim |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standart        | Yapılandırma verileri, kuralları, uyarılar ve diğer soğuk depolama |  
| 1     | [Azure Depolama Hesabı](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standart        | VM ve kontrol noktaları akış depolama |
| 1     | [Web uygulaması](https://azure.microsoft.com/services/app-service/web/)        |                 | Ön uç web uygulamasını barındırma |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Kullanıcı kimliklerini ve güvenliği yönetme |
| 1     | [Azure Haritalar](https://azure.microsoft.com/services/azure-maps/)        | Standart                | Varlık konumları görüntüleme |
| 1     | [Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/)        |   3 adet              | Gerçek zamanlı analiz etkinleştirme |
| 1     | [Azure cihaz sağlama hizmeti](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Cihazları uygun ölçekte sağlama |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1 – 1 birim              | Depolama için iletileri veri ve etkinleştirir yakından telemetri analizi |

### <a name="standard"></a>Standart

Yalnızca CLI'yı kullanarak standart bir dağıtım yapabilirsiniz.

Bir standart dağıtım, bir geliştirici özelleştirmek ve genişletmek üretime hazır bir dağıtımıdır. Ölçek ve genişletilebilirlik için yerleşik bir üretime hazır mimarisi özelleştirmek hazır olduğunuzda standart dağıtım seçeneğini kullanın. Uygulama mikro Hizmetleri Docker kapsayıcıları olarak oluşturulur ve Azure Kubernetes hizmeti kullanılarak dağıtılabilir. Kubernetes orchestrator dağıtır, ölçekler ve mikro hizmetler yönetir.

Standart bir dağıtım aşağıdaki hizmetleri Azure aboneliğinize oluşturur:

| Sayı | Resource                                     | SKU / boyutu      | İçin kullanılan |
|-------|----------------------------------------------|-----------------|----------|
| 1     | [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service)| Bir tam olarak yönetilen Kubernetes kapsayıcı düzenleme hizmeti, 3 aracılara varsayılanları kullanın|
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                     | S2 – standart katman | Cihaz yönetimi, komut ve Denetim |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standart        | Yapılandırma verileri depolama ve kuralları, uyarıları ve iletileri gibi cihaz telemetrisi |
| 5     | [Azure depolama hesapları](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standart        | 4 VM depolamasını ve akış kontrol noktaları için 1 |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Standart     | Application Gateway'de SSL üzerinden |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Kullanıcı kimliklerini ve güvenliği yönetme |
| 1     | [Azure Haritalar](https://azure.microsoft.com/services/azure-maps/)        | Standart                | Varlık konumları görüntüleme |
| 1     | [Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/)        |   3 adet              | Gerçek zamanlı analiz etkinleştirme |
| 1     | [Azure cihaz sağlama hizmeti](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Cihazları uygun ölçekte sağlama |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1 – 1 birim              | Depolama için iletileri veri ve etkinleştirir yakından telemetri analizi |

> [!NOTE]
> Bulabilirsiniz fiyatlandırma bilgileri için bu hizmetlere [ https://azure.microsoft.com/pricing ](https://azure.microsoft.com/pricing). Kullanım ve faturalandırma aboneliğinizde ayrıntılarını bulabilirsiniz [Azure portalı](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>Çözüm Hızlandırıcısını dağıtma

Dağıtım örnekleri:

### <a name="example-deploy-net-version"></a>Örnek: .NET sürümünü dağıtma

Aşağıdaki örnek, temel, .NET sürümü, Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Örnek: Java sürümü dağıtma

Aşağıdaki örnek, standart, Java sürümü Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>bilgisayarları komut seçenekleri

Çalıştırdığınızda `pcs` bir çözümü dağıtmak için komut için sorulur:

- Çözümünüz için bir ad. Bu ad benzersiz olmalıdır.
- Kullanılacak Azure aboneliği.
- Bir konum.
- Sanal makineleri barındıran mikro Hizmetleri kimlik bilgileri. Bu kimlik bilgileri, sorun giderme için sanal makinelere erişmek için kullanabilirsiniz.

Zaman `pcs` komut tamamlandığında, yeni çözüm hızlandırıcınız URL'sini görüntüler. `pcs` Komut ayrıca bir dosya oluşturur `{deployment-name}-output.json` oluşturulduğu IOT Hub'ının adı gibi bilgileri içerir.

Komut satırı parametreleri hakkında daha fazla bilgi için çalıştırın:

```cmd/sh
pcs -h
```

CLI hakkında daha fazla bilgi için bkz: [clı'yi nasıl](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Çözüm hızlandırıcısını yapılandırma
> * Çözüm Hızlandırıcısını dağıtma
> * Çözüm Hızlandırıcısını için oturum açın

Uzaktan izleme çözüm dağıttığınıza göre sonraki adım olarak [çözüm panosunun özelliklerini keşfedin](./quickstart-remote-monitoring-deploy.md).

<!-- Next how-to guides in the sequence -->
