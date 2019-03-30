---
title: 'Öğretici: Service Fabric tek başına istemcisini yükleme - Azure Service Fabric | Microsoft Docs'
description: Bu öğreticide, Service Fabric tek başına istemcisini önceki öğretici makalesinde oluşturduğunuz kümeye yüklemeyi öğreneceksiniz.
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
ms.openlocfilehash: 1bcb7f9fadc79db0db17336f07f6d58302433c1f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664038"
---
# <a name="tutorial-install-and-create-service-fabric-cluster"></a>Öğretici: Yükleme ve Service Fabric kümesi oluşturma

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici serisinde, AWS üzerinde barındırılan bir tek başına küme oluşturacak ve içine bir uygulama yükleyeceksiniz.

Bu öğretici, bir dizinin ikinci bölümüdür. Bu öğreticide, tek başına Service Fabric kümesi oluşturma adımları gösterilmektedir.

Serinin ikinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Service Fabric tek başına paketini indirme ve yükleme
> * Service Fabric kümesini oluşturma
> * Service Fabric kümesine bağlanma

## <a name="download-the-service-fabric-for-windows-server-package"></a>Windows Server paketi için Service Fabric indirme

Service Fabric, tek başına Service Fabric kümeleri oluşturmak için bir kurulum paketi sağlar.  Yerel bilgisayarınıza [kurulum paketini indirin](https://go.microsoft.com/fwlink/?LinkId=730690).  Başarıyla indirildikten sonra EC2 örneğinizin RDP bağlantısında kopyalayın ve masaüstüne yapıştırın.

Zip dosyasını seçip bağlam menüsünü açın ve **Tümünü Ayıkla** > **Ayıkla**’yı seçin.  Dosyaları ayıkladığınızda, masaüstünde zip dosya adı ile aynı olan bir klasör oluşturursunuz.

Daha ayrıntılı bilgi için [kurulum paketinin içeriğine](service-fabric-cluster-standalone-package-contents.md) bakın.

## <a name="set-up-your-configuration-file"></a>Yapılandırma dosyanızı ayarlama

Üç düğümlü Windows kümesi oluşturduğunuz için `ClusterConfig.Unsecure.MultiMachine.json` dosyasını değiştirmeniz gerekir.

Ardından, dosyanın 8, 15 ve 22. satırlarında oluşan üç ipAddress satırını her bir örneğin IP Adresleri ile güncelleştirin.

Düğümler güncelleştirildikten sonra şu şekilde görünür:

```json
        {
            "nodeName": "vm0",
            "ipAddress": "172.31.27.1",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        }
```

Sonra birkaç özelliği güncelleştirmeniz gerekir.  34. satırda, tanılama deposunun bağlantı dizesini değiştirmeniz gerekir ve sonrasında şekilde görünmelidir: `"connectionstring": "C:\\ProgramData\\SF\\DiagnosticsStore"`

Son olarak, yapılandırmanın `nodeTypes` bölümünde Windows’un kullanacağı kısa ömürlü bağlantı noktalarıyla eşlenecek yeni bir bölüm ekleyin.  Yapılandırma dosyası aşağıdaki gibi görünmelidir:

```json
"applicationPorts": {
    "startPort": "20001",
    "endPort": "20031"
},
"ephemeralPorts": {
    "startPort": "20606",
    "endPort": "20861"
},
"isPrimary": true
```

## <a name="validate-the-environment"></a>Ortamı doğrulama

Tek başına paketteki *TestConfiguration.ps1* betiği, bir kümenin belirli bir ortamda dağıtılıp dağıtılamayacağını doğrulamak için bir en iyi yöntem çözümleyicisi olarak kullanılır. [Dağıtım hazırlığı](service-fabric-cluster-standalone-deployment-preparation.md) belgesinde, ön koşullar ve ortam gereksinimleri listelenir. Geliştirme kümesini oluşturabileceğinizi doğrulamak için betiği çalıştırın:

```powershell
cd .\Desktop\Microsoft.Azure.ServiceFabric.WindowsServer.6.2.274.9494\
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
```

Aşağıdaki gibi bir çıktı görmeniz gerekir. Alttaki "Başarılı" alanı `True` olarak döndürülürse, sağlamlık denetimleri başarılı olmuştur ve giriş yapılandırmasına bağlı olarak küme dağıtılabilir görünür.

```powershell
Trace folder already exists. Traces will be written to existing trace folder: C:\Users\Administrator\Desktop\Microsoft.Azure.ServiceFabric.WindowsServer.6.2.274.9494\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 :
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
DataDrivesAvailable        : True
NoDomainController         : True
Passed                     : True
```

## <a name="create-the-cluster"></a>Kümeyi oluşturma

Küme yapılandırmanızı başarıyla doğruladıktan sonra *CreateServiceFabricCluster.ps1* betiğini çalıştırarak Service Fabric kümesini yapılandırma dosyasındaki sanal makinelere dağıtın.

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

Tümü işe yararsa, şuna benzer bir çıktı alırsınız:

```powershell
Your cluster is successfully created! You can connect and manage your cluster using Microsoft Azure Service Fabric Explorer or PowerShell. To connect through PowerShell, run 'Connect-ServiceFabricCluster [ClusterConnectionEndpoint]'.
```

> [!NOTE]
> Dağıtım izlemeleri, üzerinde CreateServiceFabricCluster.ps1 PowerShell betiğini çalıştırabileceğiniz VM/makineye yazılır. Bunlar, betiğin çalıştırıldığı dizine göre DeploymentTraces alt klasöründe bulunabilir. Service Fabric’in bir makineye doğru şekilde dağıtılıp dağıtılmadığını görmek için, FabricSettings bölümündeki küme yapılandırma dosyasında (varsayılan olarak c:\ProgramData\SF) ayrıntılı olarak açıklandığı gibi FabricDataRoot dizinindeki yüklü dosyaları bulun. Ayrıca, FabricHost.exe ve Fabric.exe işlemlerinin Görev Yöneticisi’nde çalıştığı görülebilir.
>
>

### <a name="bring-up-service-fabric-explorer"></a>Service Fabric Explorer’ı getirme

Service Fabric Explorer ile küme bağlanabilirsiniz artık http makinelerle birinden doğrudan ya da:\//localhost:19080/Explorer/index.html veya http ile uzaktan:\/ /<  *IPAddressofaMachine*>: 19080/Explorer/index.html.

## <a name="add-and-remove-nodes"></a>Düğüm ekleme ve kaldırma

İş gereksinimleriniz değiştikçe tek başına Service Fabric kümenize düğüm ekleyebilir veya kaldırabilirsiniz. Ayrıntılı adımlar için bkz. [Service Fabric tek başına kümesine düğüm ekleme veya kaldırma](service-fabric-cluster-windows-server-add-remove-nodes.md).

## <a name="next-steps"></a>Sonraki adımlar

Serinin ikinci kısmında, büyük miktarlarda rastgele verileri paralel şekilde bir depolama hesabına yüklemeyle ilgili aşağıda örnekleri verilen işlemleri öğrendiniz:

> [!div class="checklist"]
> * Bağlantı dizesini yapılandırma
> * Uygulama oluşturma
> * Uygulamayı çalıştırma
> * Bağlantı sayısını doğrulama

Oluşturduğunuz kümeye bir uygulama yüklemek için serinin üçüncü bölümüne ilerleyin.

> [!div class="nextstepaction"]
> [Service fabric kümesine uygulama yükleme](service-fabric-tutorial-standalone-install-an-application.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
