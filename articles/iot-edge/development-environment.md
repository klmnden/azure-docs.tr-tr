---
title: Azure IOT Edge geliştirme ortamı | Microsoft Docs
description: IOT Edge modülleri oluşturma yardımcı olacak birinci taraf geliştirme araçları ve desteklenen sistemleri hakkında bilgi edinin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a6fc2af0cbe770ee787da757966bbc1647717e5a
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66302675"
---
# <a name="prepare-your-development-and-test-environment-for-iot-edge"></a>IOT Edge için geliştirme ve test ortamı hazırlama

Azure IOT Edge, var olan iş mantığındaki ucuna yüksekte işleyen cihazlarda taşır. Uygulamalarınızı ve iş yüklerini çalıştırmak için hazırlamak üzere [IOT Edge modülleri](iot-edge-modules.md), kapsayıcıları olarak derlenmesine gerek. Bu makalede bir IOT Edge çözümü başarıyla oluşturabilmeniz, geliştirme ortamınızı yapılandırma konusunda bir Rehber sağlanır. Geliştirme ortamınızı ayarlayın tamamladıktan sonra şunları öğrenebilirsiniz nasıl [kendi IOT Edge modülleri geliştirme](module-development.md).

IOT Edge çözümlerle dikkate alınması gereken en az iki makine yok. IOT Edge cihazı (veya cihaz) biridir kendisine ait IOT Edge modülü çalıştırır. Diğer derleme, test etme ve modüllerini dağıtmak için kullandığınız geliştirme makinedir. Bu makalede öncelikli olarak geliştirme makinenizde odaklanır. Test amacıyla iki makine aynı olabilir. IOT Edge geliştirme makinenizde çalıştırın ve modülleri dağıtalım.

## <a name="operating-system"></a>İşletim sistemi

Azure IOT Edge çalıştıran belirli bir dizi üzerinde [desteklenen işletim sistemleri](support.md). IOT Edge için geliştirmeye yönelik bir kapsayıcı altyapısı çalıştırabilirsiniz çoğu işletim sistemi kullanabilirsiniz. Kapsayıcı altyapısı modüllerinizi kapsayıcıları olarak oluşturun ve bunları bir kapsayıcı kayıt defterine gönderin için geliştirme makinesindeki bir gereksinimdir. 

Geliştirme makinenizde Azure IOT Edge çalıştıramayan hakkında bilgi edinmek için bu makaledeki devam [test araçları](#testing-tools) yardımcı olan, test ve yerel olarak hata ayıklama. 

İşletim sistemi, geliştirme makinenizin IOT Edge cihazınızın işletim sistemi eşleşmesi gerekmez. Ancak, kapsayıcı işletim sistemi geliştirme makineniz ve IOT Edge cihazı arasında tutarlı olması gerekir. Örneğin, bir Windows makinede modülleri geliştirmek ve bunları bir Linux cihazına dağıtın. Linux cihazına modüllerini oluşturmak için Linux kapsayıcıları çalıştırmak Windows makine gerekir. 

## <a name="container-engine"></a>Kapsayıcı altyapısı

Merkezi IOT Edge, uzaktan, iş ve bulut mantığına cihazlara kapsayıcılarına paketleyerek dağıtabilirsiniz kavramdır. Kapsayıcılar oluşturmak için geliştirme makinenizde kapsayıcı altyapısının olması gerekir. 

IOT Edge cihazlarına üretimde için yalnızca desteklenen kapsayıcı Moby altyapısıdır. Ancak, Docker gibi açık kapsayıcı girişim ile uyumlu herhangi bir kapsayıcı altyapısı IOT Edge modül görüntüleri oluşturma yeteneğine sahip değildir. 

## <a name="development-tools"></a>Geliştirme araçları

IOT Edge çözümlerini geliştirmek amacıyla eklenti uzantıları, hem Visual Studio ve Visual Studio Code sahip. Bu uzantılar, yeni IOT Edge senaryoları oluşturup yardımcı olmak için dile özgü şablonları sağlar. Visual Studio ve Visual Studio Code Yardım, kod, derleme, dağıtma ve IOT Edge çözümlerinizi hata ayıklamak için Azure IOT Edge uzantıları. Uzantıları otomatik olarak her yeni modül eklenmesiyle dağıtım bildirim şablonu güncelleştirme ve birden çok modül içeren tüm bir IOT Edge çözüm oluşturabilirsiniz. Uzantılar sayesinde, IOT cihazlarınızdan Visual Studio veya Visual Studio Code içinde de yönetebilirsiniz. Modülleri bir aygıta dağıtmak, durumunu izlemek ve IOT Hub geldikçe iletileri görüntüleyin. Her iki uzantıları kullanma [IOT EdgeHub geliştirme aracı](#iot-edgehub-dev-tool) yerel çalıştırma ve modüllerin de geliştirme makinenizdeki hata ayıklamayı etkinleştirmek için. 

Diğer düzenleyiciler veya clı'dan geliştirmek isterseniz, Azure IOT Edge Geliştirici araç komutları sağlar, böylece geliştirip komut satırından test edebilirsiniz. Yeni IOT Edge senaryoları oluşturun, modül görüntüleri oluşturun, simülatör'de modüllerini çalıştırın ve IOT hub'a gönderilen iletileri izlemek. 

### <a name="visual-studio-code-extension"></a>Visual Studio Code uzantısı

Visual Studio Code için Azure IOT Edge uzantısını IOT Edge modülü şablonları programlama dillerinin C üzerinde oluşturulmuş sağlar C#, Java, Node.js ve Python yanı sıra Azure işlevlerinde C#. 

Daha fazla bilgi ve indirmek için bkz: [Visual Studio Code için Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

IOT Edge uzantıları yanı sıra geliştirmek için ek uzantıları yüklemek yararlı. Örneğin, kullanabileceğiniz [Visual Studio Code için Docker desteğini](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) , görüntüler, kapsayıcılar ve kayıt defterleri yönetme. Ayrıca, tüm büyük desteklenen dilleri modülleri geliştirirken, yardımcı olabilecek Visual Studio Code için uzantılarına sahiptir. 

#### <a name="prerequisites"></a>Önkoşullar

Bazı diller ve hizmetler için modül şablonları geliştirme makinenizde Visual Studio Code ile proje klasörleri oluşturmak gerekli olan önkoşulları vardır.

| Modülü şablonu | Önkoşul |
| --------------- | ------------ |
| Azure İşlevleri | [.NET core 2.1 SDK'sı](https://www.microsoft.com/net/download) |
| C | [Git](https://git-scm.com/) |
| C# | [.NET core 2.1 SDK'sı](https://www.microsoft.com/net/download) |
| Java | <ul><li>[Java SE Geliştirme Seti 10](https://aka.ms/azure-jdks) <li> [JAVA_HOME ortam değişkenini ayarla](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) <li> [Maven](https://maven.apache.org/)</ul> |
| Node.js | <ul><li>[Node.js](https://nodejs.org/) <li> [Yeoman](https://www.npmjs.com/package/yo) <li> [Azure IOT Edge Node.js modül Oluşturucu](https://www.npmjs.com/package/generator-azure-iot-edge-module)</ul> |
| Python |<ul><li> [Python](https://www.python.org/downloads/) <li> [pip](https://pip.pypa.io/en/stable/installing/#installation) <li> [Cookiecutter](https://cookiecutter.readthedocs.io/en/latest/installation.html) <li> [Git](https://git-scm.com/) </ul> |

### <a name="visual-studio-20172019-extension"></a>Visual Studio 2017/2019 uzantısı

Visual Studio için Azure IOT Edge araçları bir IOT Edge modülü şablonu olanakları sağlayın. C# ve C. 

Daha fazla bilgi ve indirmek için bkz: [Visual Studio 2017 için Azure IOT Edge Araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) veya [Visual Studio 2019 için Azure IOT Edge Araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools).

### <a name="iot-edge-dev-tool"></a>IOT Edge Geliştirici aracı

Azure IOT Edge geliştirme aracını, komut satırı becerileriyle ile IOT Edge geliştirmeyi basitleştirir. Bu araç, geliştirmek, hata ayıklama ve test modüller için CLI komutlarını sağlar. Makinenizde bağımlılıkları el ile yükledikten veya IOT Edge geliştirme kapsayıcı kullanarak IOT Edge geliştirme araç geliştirme sisteminizde ile çalışır. 

Daha fazla bilgi ve kullanmaya başlamak için bkz: [IOT Edge geliştirme aracı wiki](https://github.com/Azure/iotedgedev/wiki).

## <a name="testing-tools"></a>Test araçları

IOT Edge cihazları benzetimini yapmak veya modülleri daha verimli bir şekilde hata ayıklamaya yardımcı olmak için birkaç test araçları mevcut. Bölümleri tek tek her aracı daha belirgin olarak tanımlamak ve üst düzey bir karşılaştırma araçları arasında aşağıdaki tabloda gösterilmektedir. 

Yalnızca IOT Edge çalışma zamanı, üretim dağıtımları için desteklenir, ancak aşağıdaki araçları benzetimini yapmak veya IOT Edge cihazları geliştirme ve test etme amacıyla kolayca oluşturmanıza olanak sağlar. Bu araçlar, birbirini dışlayan değildir, ancak tam geliştirme deneyimi için birlikte çalışabilir. 

| Aracı | Olarak da bilinir | Desteklenen platformlar | En iyi kullanım alanı: |
| ---- | ------------- | ------------------- | --------- |
| IOT EdgeHub geliştirme aracı  | iotedgehubdev | Windows, Linux, MacOS | Modüller hata ayıklamak için bir cihazı benzetme |
| IOT Edge geliştirme kapsayıcı | microsoft/iotedgedev | Windows, Linux, MacOS | Bağımlılıkları yüklemeden geliştirme. |
| Bir kapsayıcıda IOT Edge çalışma zamanı | iotedgec | Windows, Linux, MacOS, ARM | Çalışma zamanı desteği olmayan bir cihazda test etme. |
| IOT Edge cihaz kapsayıcı | toolboc/azure-IOT-edge-cihaz-container | Windows, Linux, MacOS, ARM | Birçok IOT Edge cihazları uygun ölçekte bir senaryo testi. |

### <a name="iot-edgehub-dev-tool"></a>IOT EdgeHub geliştirme aracı

Azure IOT EdgeHub geliştirme aracını yerel bir geliştirme ve hata ayıklama deneyimi sağlar. IOT Edge modülleri IOT Edge çalışma zamanı olmadan oluşturabilirsiniz böylece geliştirin, test etmek, çalıştırmak ve IOT Edge modülleri ve çözümler yerel olarak hata ayıklama aracı yardımcı başlangıcı. Görüntüleri bir kapsayıcı kayıt defterine gönderin ve test etmek için bir cihaz dağıtmak gerekmez.

IOT EdgeHub geliştirme aracı dağıtımınızla birlikte Visual Studio ve Visual Studio Code uzantılarını yanı sıra IOT Edge geliştirme aracı ile çalışacak şekilde tasarlanmıştır. Bu iç döngü geliştirme hem de dış döngü test destekler, böylece çok DevOps araçlarıyla tümleşir. 

Daha fazla bilgi ve yüklemek için bkz: [Azure IOT EdgeHub geliştirme aracı](https://pypi.org/project/iotedgehubdev/).

### <a name="iot-edge-dev-container"></a>IOT Edge geliştirme kapsayıcı

Azure IOT Edge geliştirme IOT Edge geliştirme için ihtiyaç duyduğunuz tüm bağımlılıkları olan bir Docker kapsayıcısı kapsayıcıdır. Bu kapsayıcı kullanmaya başlamak kolaylaştırır hangi dili ile geliştirme de dahil olmak üzere istediğiniz C#, Python, Node.js ve Java. Yüklemek için ihtiyacınız olan Docker veya Moby, geliştirme makinenize kapsayıcıyı çekmek için bir kapsayıcı altyapısı. 

Daha fazla bilgi için [Azure IOT Edge geliştirme kapsayıcı](https://hub.docker.com/r/microsoft/iotedgedev/).

### <a name="iot-edge-runtime-in-a-container"></a>Bir kapsayıcıda IOT Edge çalışma zamanı

Cihaz bağlantı dizenizi bir ortam değişkeni olarak alan tam bir çalışma zamanı IOT Edge çalışma zamanında bir kapsayıcı sağlar. Bu kapsayıcı, IOT Edge modülleri ve çalışma zamanı yerel olarak, MacOS gibi desteklemeyebilir bir sistemde senaryoları test olanak tanır. Dağıttığınız tüm modüller, çalışma zamanı kapsayıcısı dışında başlatılacak. IOT Edge cihaz kapsayıcı çalışma zamanı ve dağıtılan modüllerin de aynı kapsayıcı içinde bulunmasını istiyorsanız, bunun yerine göz önünde bulundurun.

Daha fazla bilgi için [Azure IOT Edge çalıştıran bir kapsayıcıda](https://github.com/Azure/iotedgedev/tree/master/docker/runtime).

### <a name="iot-edge-device-container"></a>IOT Edge cihaz kapsayıcı

IOT Edge cihazı bir tam IOT Edge cihazı, bir kapsayıcı altyapısıyla herhangi bir makinede başlatılacak hazır kapsayıcıdır. Cihaz kapsayıcı, IOT Edge çalışma zamanı ve kapsayıcı altyapısı içerir. Her kapsayıcı tam olarak işlevsel bir kendi kendine sağlama IOT Edge cihazı örneğidir. Modül için bir ağ yolu var olduğu sürece cihaz kapsayıcısı modüllerini, uzaktan hata ayıklamayı destekler. Cihaz kapsayıcı çok sayıda IOT Edge cihazları ölçekli senaryolar veya Azure işlem hatları test etmek için hızlı bir şekilde oluşturmak için uygundur. Ayrıca, kubernetes helm aracılığıyla dağıtımı destekler. 

Daha fazla bilgi için [Azure IOT Edge cihaz kapsayıcı](https://github.com/toolboc/azure-iot-edge-device-container).

## <a name="devops-tools"></a>DevOps araçları

Kapsamlı üretim senaryoları için ölçekli çözümleri geliştirmek üzere hazır olduğunuzda, otomasyon, izleme ve kolaylaştırılmış yazılım Mühendisliği işlemleri gibi modern DevOps ilkeleri yararlanın. IOT Edge, Azure DevOps, Azure DevOps projeleri ve Jenkins gibi DevOps araçlarını destekleyecek uzantılar vardır. Mevcut bir işlem hattını özelleştirebilir veya CircleCI veya Travis CI gibi farklı bir DevOps aracını kullanmak istiyorsanız, IOT Edge geliştirme aracında bulunan CLI özellikleri ile bunu yapabilirsiniz.

Daha fazla bilgi, yönergeler ve örnekler için şu sayfalara bakın:
* [Sürekli tümleştirme ve sürekli dağıtım için Azure IOT Edge](how-to-ci-cd.md)
* [Azure DevOps projeleri ile IOT Edge için CI/CD işlem hattı oluşturma](how-to-devops-project.md)
* [Azure IOT Edge Jenkins eklentisi](https://plugins.jenkins.io/azure-iot-edge)
* [IOT Edge DevOps GitHub deposu](https://github.com/toolboc/IoTEdge-DevOps)
