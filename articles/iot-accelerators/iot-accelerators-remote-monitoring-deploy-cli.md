---
title: Azure Java Uzaktan izleme çözümü - dağıtın | Microsoft Docs
description: Bu öğreticide CLI kullanarak Uzaktan izleme çözüm Hızlandırıcısını sağlama gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/12/2018
ms.topic: conceptual
ms.openlocfilehash: 56f233afed8c403d19c9b668e98ecfec45470b64
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44721628"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>CLI kullanarak Uzaktan izleme çözüm Hızlandırıcısını dağıtma

Bu öğreticide Uzaktan izleme çözüm Hızlandırıcısını sağlama gösterilmektedir. CLI kullanarak çözümü dağıtın. Ayrıca bu seçeneği bakın hakkında bilgi edinmek için azureiotsuite.com web tabanlı kullanıcı Arabirimi kullanarak çözümü dağıtabilirsiniz [Uzaktan izleme çözüm Hızlandırıcısını dağıtma](quickstart-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını dağıtma için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

CLI'yı çalıştırmak için ihtiyacınız [Node.js](https://nodejs.org/) yerel makinenizde yüklü.

## <a name="install-the-cli"></a>CLI'yi yükleme

CLI'yı yüklemek için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>CLI için oturum açın

Çözüm Hızlandırıcısını dağıtmadan önce şu şekilde CLI kullanarak Azure aboneliğinizde oturum açmanız gerekir:

```cmd/sh
pcs login
```

İzleyin ekran oturum açma işlemini tamamlamak için yönergeleri.

## <a name="deployment-options"></a>Dağıtım seçenekleri

Çözüm Hızlandırıcısını dağıttığınızda, dağıtım işlemi yapılandırma birkaç seçenek vardır:

| Seçenek | Değerler | Açıklama |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | A _temel_ dağıtım, test ve Tanıtımlar için tasarlanmıştır, tek bir sanal makine için tüm mikro Hizmetleri dağıtır. A _standart_ dağıtım, birden çok sanal makine için mikro Hizmetleri dağıttığı üretim için tasarlanmıştır. A _yerel_ dağıtım mikro Hizmetleri yerel makinenizde çalıştırmak için bir Docker kapsayıcısı yapılandırır ve bulutta depolama ve Cosmos DB gibi Azure hizmetlerini kullanır. |
| Çalışma Zamanı | `dotnet`, `java` | Mikro hizmetler dil uygulamasını seçer. |

Yerel dağıtımı kullanma hakkında bilgi edinmek için [Uzaktan izleme çözümü yerel olarak çalışan](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="basic-vs-standard-deployments"></a>Temel vs. Standart dağıtım

### <a name="basic"></a>Temel
Temel dağıtım, çözümü vitrini doğru yöneliktir. Bu gösterimin maliyetini azaltmak için tüm mikro hizmetler tek bir sanal makinede dağıtılan; Bu, üretime hazır bir mimari dikkate alınmaz.

Ölçek ve genişletilebilirlik için yerleşik bir üretime hazır mimarisi özelleştirmek hazır olduğunuzda, bizim standart dağıtım seçeneği kullanılmalıdır.

Temel bir çözüm oluşturulması aşağıdaki Azure hizmetlerinin Azure aboneliğinize ücret halinde hazırlandığı neden olur: 

| Sayı | Kaynak                       | Tür         | İçin kullanılan |
|-------|--------------------------------|--------------|----------|
| 1     | [Linux sanal makinesi](https://azure.microsoft.com/services/virtual-machines/) | Standart D1 V2  | Mikro hizmet barındırma |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1 – standart katman | Cihaz yönetimi ve iletişim |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standart        | Yapılandırma verileri, kuralları, uyarılar ve diğer soğuk depolama |  
| 1     | [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standart        | VM ve kontrol noktaları akış depolama |
| 1     | [Web uygulaması](https://azure.microsoft.com/services/app-service/web/)        |                 | Ön uç web uygulamasını barındırma |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Kullanıcı kimliklerini ve güvenliği yönetme |
| 1     | [Azure Haritalar](https://azure.microsoft.com/services/azure-maps/)        | Standart                | Varlık konumları görüntüleme |
| 1     | [Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/)        |   3 birim              | Gerçek zamanlı analiz etkinleştirme |
| 1     | [Azure cihaz sağlama hizmeti](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Cihazları uygun ölçekte sağlama |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1 – 1 birim              | Depolama için iletileri veri ve etkinleştirir yakından telemetri analizi |



### <a name="standard"></a>Standart
Standart dağıtım Geliştirici özelleştirebilir ve genişletebilirsiniz ihtiyaçlarını karşılamak için üretime hazır dağıtımıdır. Güvenilirlik ve ölçek için uygulama mikro Hizmetleri Docker kapsayıcıları olarak oluşturulur ve bir orchestrator kullanılarak dağıtılan ([Kubernetes](https://kubernetes.io/) varsayılan olarak). Orchestrator, dağıtım, ölçeklendirme ve uygulama yönetimi için sorumludur.

Standart bir çözüm oluşturulması aşağıdaki Azure hizmetlerinin Azure aboneliğinize ücret halinde hazırlandığı neden olur:

| Sayı | Kaynak                                     | SKU / boyutu      | İçin kullanılan |
|-------|----------------------------------------------|-----------------|----------|
| 4     | [Linux Sanal Makineleri](https://azure.microsoft.com/services/virtual-machines/)   | Standart D2 V2  | 1 Yöneticisi ve artıklık ile mikro hizmetler barındırmak için 3 aracıları |
| 1     | [Azure kapsayıcı hizmeti](https://azure.microsoft.com/services/container-service/) |                 | [Kubernetes](https://kubernetes.io) orchestrator |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                     | S2 – standart katman | Cihaz yönetimi, komut ve Denetim |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standart        | Yapılandırma verileri ve cihaz telemetrisi kuralları, uyarıları ve iletileri gibi depolama |
| 5     | [Azure depolama hesapları](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standart        | 4 VM depolamasını ve akış kontrol noktaları için 1 |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Standart     | Application Gateway'de SSL üzerinden |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Kullanıcı kimliklerini ve güvenliği yönetme |
| 1     | [Azure Haritalar](https://azure.microsoft.com/services/azure-maps/)        | Standart                | Varlık konumları görüntüleme |
| 1     | [Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/)        |   3 birim              | Gerçek zamanlı analiz etkinleştirme |
| 1     | [Azure cihaz sağlama hizmeti](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Cihazları uygun ölçekte sağlama |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1 – 1 birim              | Depolama için iletileri veri ve etkinleştirir yakından telemetri analizi |

> Bu hizmetler bulunabilir fiyatlandırma bilgileri [burada](https://azure.microsoft.com/pricing). İçinde kullanım miktarlar ve aboneliğiniz için fatura ayrıntılarını bulunabilir [Azure portalı](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>Çözüm Hızlandırıcısını dağıtma

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

Çalıştırdığınızda `pcs` bir çözümü dağıtmak için komut için istenir:

- Çözümünüz için bir ad. Bu ad benzersiz olmalıdır.
- Kullanılacak Azure aboneliği.
- Bir konum.
- Sanal makineleri barındıran mikro Hizmetleri kimlik bilgileri. Bu kimlik bilgileri, sorun giderme için sanal makinelere erişmek için kullanabilirsiniz.

Zaman `pcs` komut tamamlandığında, yeni çözüm Hızlandırıcı dağıtımınızın URL'yi görüntüler. `pcs` Komut ayrıca bir dosya oluşturur `{deployment-name}-output.json` ile sizin için sağlanan IOT Hub'ının adı gibi ek bilgileri.

Komut satırı parametreleri hakkında daha fazla bilgi için çalıştırın:

```cmd/sh
pcs -h
```

CLI hakkında daha fazla bilgi için bkz: [clı'yi nasıl](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Çözüm hızlandırıcısını yapılandırma
> * Çözüm Hızlandırıcısını dağıtma
> * Çözüm Hızlandırıcısını için oturum açın

Uzaktan izleme çözümü dağıttıktan sonra sonraki adım olarak [çözüm panosunun özelliklerini keşfedin](./quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->
