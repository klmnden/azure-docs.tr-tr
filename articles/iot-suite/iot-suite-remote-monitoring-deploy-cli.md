---
title: Uzaktan izleme çözümü - Azure Java dağıtma | Microsoft Docs
description: Bu öğretici, CLI kullanarak Uzaktan izleme Çözüm Hızlandırıcısı sağlama gösterir.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: f158af573475964eca1ff168ecf3eadc58c2a394
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33774236"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>CLI kullanarak Uzaktan izleme Çözüm Hızlandırıcısı dağıtma

Bu öğretici, Uzaktan izleme Çözüm Hızlandırıcısı sağlama gösterir. CLI kullanarak çözümü dağıtın. Bu seçenek bakın hakkında bilgi edinmek için azureiotsuite.com web tabanlı kullanıcı arabirimini kullanarak çözüm de dağıtabilirsiniz [Uzaktan izleme Çözüm Hızlandırıcısı dağıtmak](iot-suite-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme Çözüm Hızlandırıcısı dağıtmak için etkin bir Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

CLI çalıştırmak için ihtiyacınız [Node.js](https://nodejs.org/) yerel makinenize yüklü.

## <a name="install-the-cli"></a>CLI'yi yükleme

CLI yüklemek için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>CLI için oturum açın

Çözüm Hızlandırıcısı dağıtmadan önce CLI kullanarak Azure aboneliğinizde oturum gerekir:

```cmd/sh
pcs login
```

İzleyin ekran oturum açma işlemini tamamlamak için yönergeleri.

## <a name="deployment-options"></a>Dağıtım seçenekleri

Çözüm Hızlandırıcısı dağıttığınızda, dağıtım işlemi yapılandırma birkaç seçeneğiniz vardır:

| Seçenek | Değerler | Açıklama |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | A _temel_ dağıtım, test ve gösterim için tasarlanmıştır, tek bir sanal makine için tüm mikro dağıtır. A _standart_ dağıtımı için birden çok sanal makine mikro dağıttığı üretim için tasarlanmıştır. A _yerel_ dağıtım mikro yerel makinenizde çalıştırmak için Docker kapsayıcısı yapılandırır ve bulutta Azure Hizmetleri, depolama ve Cosmos DB gibi kullanır. |
| Çalışma Zamanı | `dotnet`, `java` | Mikro dil uyarlamasını seçer. |

Yerel dağıtım kullanma hakkında bilgi edinmek için [Uzaktan izleme çözümü yerel olarak çalışan](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="basic-vs-standard-deployments"></a>Temel vs. Standart dağıtımları

### <a name="basic"></a>Temel
Temel dağıtım çözümü birtakım sergileyen doğru sağlamıştır. Bu Tanıtım maliyetini azaltmak için tüm mikro tek bir sanal makinede dağıtılan; Bu, üretime hazır mimarisi dikkate alınmaz.

Ölçek ve genişletilebilirlik için yerleşik bir üretime hazır mimarisi özelleştirmek hazır olduğunuzda bizim standart dağıtım seçeneği kullanılmalıdır.

Temel bir çözüm oluşturma, Azure aboneliğinizin maliyetle içine sağlanacak aşağıdaki Azure hizmetlerinin neden olur: 

| Sayı | Kaynak                       | Tür         | İçin kullanılır |
|-------|--------------------------------|--------------|----------|
| 1     | [Linux sanal makine](https://azure.microsoft.com/services/virtual-machines/) | Standart D1 V2  | Mikro barındırma |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1 – temel katmanı | Aygıt Yönetimi ve iletişim |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standart        | Yapılandırma verileri, cihaz telemetrisi kuralları, uyarılar ve iletileri gibi depolama |  
| 1     | [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standart        | VM ve kontrol noktaları akış için depolama |
| 1     | [Web uygulaması](https://azure.microsoft.com/services/app-service/web/)        |                 | Ön uç web uygulaması barındırma |

### <a name="standard"></a>Standart
Standart dağıtım Geliştirici özelleştirebilir ve kendi gereksinimlerini karşılayacak şekilde genişletmek bir üretime hazır dağıtımıdır. Güvenilirlik ve ölçek, uygulama mikro Docker kapsayıcılar olarak oluşturulur ve bir orchestrator kullanılarak dağıtılan ([Kubernetes](https://kubernetes.io/) varsayılan olarak). Orchestrator, dağıtım, ölçeklendirme ve uygulama yönetimi için sorumludur.

Standart bir çözüm oluşturulurken, Azure aboneliğinizin maliyetle içine sağlanacak aşağıdaki Azure hizmetlerini neden olur:

| Sayı | Kaynak                                     | SKU / boyut      | İçin kullanılır |
|-------|----------------------------------------------|-----------------|----------|
| 4     | [Linux Sanal Makineleri](https://azure.microsoft.com/services/virtual-machines/)   | Standart D2 V2  | 1 ana ve mikro hizmetler ile artıklık barındırmak için 3 aracıları |
| 1     | [Azure kapsayıcı hizmeti](https://azure.microsoft.com/services/container-service/) |                 | [Kubernetes](https://kubernetes.io) orchestrator |
| 1     | [Azure IOT Hub] [https://azure.microsoft.com/services/iot-hub/]                     | S1 – temel katmanı | Aygıt Yönetimi, komut ve Denetim |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standart        | Yapılandırma verileri, cihaz telemetrisi kuralları, uyarılar ve iletileri gibi depolama |
| 5     | [Azure depolama hesapları](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standart        | VM depolama ve akış denetim noktaları için 1 için 4 |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Standart     | SSL üzerinden uygulama ağ geçidi |

> Bu hizmetler bulunabilir fiyatlandırma bilgileri [burada](https://azure.microsoft.com/pricing). Kullanım miktarlarına ve fatura ayrıntılarına aboneliğiniz için bulunabilir [Azure Portal](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>Çözüm Hızlandırıcısı dağıtma

### <a name="example-deploy-net-version"></a>Örnek: .NET sürüm dağıtın

Aşağıdaki örnek, Uzaktan izleme Çözüm Hızlandırıcısı .NET sürümü, basic dağıtmak gösterilmektedir:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Örnek: Java Sürüm dağıtın

Aşağıdaki örnek, standart, Uzaktan izleme Çözüm Hızlandırıcısı Java sürümünü dağıtmak gösterilmektedir:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>bilgisayarları komut seçenekleri

Çalıştırdığınızda `pcs` bir çözümü dağıtmak için komut için istenir:

- Çözümünüz için bir ad. Bu ad benzersiz olmalıdır.
- Kullanılacak Azure aboneliği.
- Bir konum.
- Sanal makineler için barındıran mikro kimlik bilgileri. Sorun giderme için sanal makinelere erişmek için bu kimlik bilgileri kullanabilirsiniz.

Zaman `pcs` komut tamamlandığında, yeni Çözüm Hızlandırıcısı dağıtımınızın URL'yi görüntüler. `pcs` Komut ayrıca bir dosya oluşturur `{deployment-name}-output.json` adı gibi ek bilgileri sizin için hazırlanmış olan IOT hub ile.

Komut satırı parametreleri hakkında daha fazla bilgi için çalıştırın:

```cmd/sh
pcs -h
```

CLI hakkında daha fazla bilgi için bkz: [CLI kullanma](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Çözüm Hızlandırıcısı yapılandırın
> * Çözüm Hızlandırıcısı dağıtma
> * Çözüm Hızlandırıcısı oturum açın

Uzaktan izleme çözümü dağıttıysanız, sonraki adım olacak [çözüm Panosu özelliklerini keşfedin](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->