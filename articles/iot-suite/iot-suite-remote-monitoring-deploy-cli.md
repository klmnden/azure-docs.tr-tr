---
title: "Uzaktan izleme çözümü - Azure Java dağıtma | Microsoft Docs"
description: "Bu öğretici, CLI kullanarak Uzaktan izleme önceden yapılandırılmış çözümü sağlamak nasıl gösterir."
services: 
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
ms.openlocfilehash: 94c3db3286623264e9df7873962d10dd5cc662d4
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-using-the-cli"></a>CLI kullanarak Uzaktan izleme önceden yapılandırılmış çözümü dağıtma

Bu öğretici, önceden yapılandırılmış uzaktan izleme çözümünün nasıl hazırlanacağını gösterir. CLI kullanarak çözümü dağıtın. Bu seçenek bakın hakkında bilgi edinmek için azureiotsuite.com web tabanlı kullanıcı arabirimini kullanarak çözüm de dağıtabilirsiniz [önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](iot-suite-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Önkoşullar

Önceden yapılandırılmış Uzaktan izleme çözümü dağıtmak için etkin bir Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

CLI çalıştırmak için ihtiyacınız [Node.js](https://nodejs.org/) yerel makinenize yüklü.

## <a name="install-the-cli"></a>CLI'yi yükleme

CLI yüklemek için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>CLI için oturum açın

Önceden yapılandırılmış çözümü dağıtmadan önce CLI kullanarak Azure aboneliğinizde oturum gerekir:

```cmd/sh
pcs login
```

İzleyin ekran oturum açma işlemini tamamlamak için yönergeleri.

## <a name="deployment-options"></a>Dağıtım seçenekleri

Önceden yapılandırılmış çözümü dağıttığınızda, dağıtım işlemi yapılandırma birkaç seçeneğiniz vardır:

| Seçenek | Değerler | Açıklama |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | A _temel_ dağıtım, test ve gösterim için tasarlanmıştır, tek bir sanal makine için tüm mikro dağıtır. A _standart_ dağıtımı için birden çok sanal makine mikro dağıttığı üretim için tasarlanmıştır. A _yerel_ dağıtım mikro yerel makinenizde çalıştırmak için Docker kapsayıcısı yapılandırır ve bulutta Azure Hizmetleri, depolama ve Cosmos DB gibi kullanır. |
| Çalışma Zamanı | `dotnet`, `java` | Mikro dil uyarlamasını seçer. |

Yerel dağıtım kullanma hakkında bilgi edinmek için [Uzaktan izleme çözümü yerel olarak çalışan](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="deploy-the-preconfigured-solution"></a>Önceden yapılandırılmış çözümü dağıtma

### <a name="example-deploy-net-version"></a>Örnek: .NET sürüm dağıtın

Aşağıdaki örnek, önceden yapılandırılmış Uzaktan izleme çözümü temel, .NET sürümünü dağıtmanız gösterilmektedir:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Örnek: Java Sürüm dağıtın

Aşağıdaki örnek standart, Java sürümünü önceden yapılandırılmış Uzaktan izleme çözümü dağıtma gösterir:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>bilgisayarları komut seçenekleri

Çalıştırdığınızda `pcs` bir çözümü dağıtmak için komut için istenir:

- Çözümünüz için bir ad. Bu ad benzersiz olmalıdır.
- Kullanılacak Azure aboneliği.
- Bir konum.
- Sanal makineler için barındıran mikro kimlik bilgileri. Sorun giderme için sanal makinelere erişmek için bu kimlik bilgileri kullanabilirsiniz.

Zaman `pcs` komut tamamlandığında, yeni önceden yapılandırılmış çözüm dağıtımınızın URL'yi görüntüler. `pcs` Komut ayrıca bir dosya oluşturur `{deployment-name}-output.json` adı gibi ek bilgileri sizin için hazırlanmış olan IOT hub ile.

Komut satırı parametreleri hakkında daha fazla bilgi için çalıştırın:

```cmd/sh
pcs -h
```

CLI hakkında daha fazla bilgi için bkz: [CLI kullanma](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Önceden yapılandırılmış çözümünüzü yapılandırma
> * Önceden yapılandırılmış çözümü dağıtma
> * Önceden yapılandırılmış çözümü oturum açın

Uzaktan izleme çözümü dağıttıysanız, sonraki adım olacak [çözüm Panosu özelliklerini keşfedin](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->