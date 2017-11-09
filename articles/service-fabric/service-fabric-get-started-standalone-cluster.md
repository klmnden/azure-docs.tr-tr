---
title: "Tek başına Azure Service Fabric kümesi ayarlama | Microsoft Docs"
description: "Aynı bilgisayarda çalışan üç düğüme sahip tek başına bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra çok makineli bir küme oluşturmaya hazır hale gelirsiniz."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: 5438d8d366ef989d5ae29581477513f8c884c4b3
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a>İlk tek başına Service Fabric kümenizi oluşturma
Şirket içinde veya bulutta Windows Server 2012 R2 ya da Windows Server 2016 çalıştıran herhangi bir sanal makinede tek başına Service Fabric kümesi oluşturabilirsiniz. Bu hızlı başlangıç kılavuzu, yalnızca birkaç dakika içerisinde tek başına bir geliştirme kümesi oluşturmanıza yardımcı olur.  Kılavuzu tamamladığınızda, uygulama dağıtabileceğiniz tek bir bilgisayarda çalışan üç düğümlü bir kümeniz olur.

## <a name="before-you-begin"></a>Başlamadan önce
Service Fabric, tek başına Service Fabric kümeleri oluşturmak için bir kurulum paketi sağlar.  [Kurulum paketini indirin](http://go.microsoft.com/fwlink/?LinkId=730690).  Kurulum paketini, geliştirme kümesini kuracağınız bilgisayar veya sanal makinedeki bir klasöre çıkarın.  Kurulum paketinin içeriği [burada](service-fabric-cluster-standalone-package-contents.md) ayrıntılı olarak açıklanmıştır.

Kümeyi dağıtan ve yapılandıran küme yöneticisinin bilgisayarda yönetici ayrıcalıklarına sahip olması gerekir. Service Fabric’i bir etki alanı denetleyicisine yükleyemezsiniz.

## <a name="validate-the-environment"></a>Ortamı doğrulama
Tek başına paketteki *TestConfiguration.ps1* betiği, bir kümenin belirli bir ortamda dağıtılıp dağıtılamayacağını doğrulamak için bir en iyi yöntem çözümleyicisi olarak kullanılır. [Dağıtım hazırlığı](service-fabric-cluster-standalone-deployment-preparation.md) belgesinde, ön koşullar ve ortam gereksinimleri listelenir. Geliştirme kümesini oluşturabileceğinizi doğrulamak için betiği çalıştırın:

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-the-cluster"></a>Kümeyi oluşturma
Kurulum paketiyle birlikte birkaç örnek küme yapılandırma dosyası yüklenir. Tek bir bilgisayarda çalışan korumasız ve üç düğümlü bir küme olan *ClusterConfig.Unsecure.DevCluster.json*, en basit küme yapılandırmasıdır.  Diğer yapılandırma dosyalarında X.509 sertifikalarıyla ya da Windows güvenliğiyle korunan tek veya çok makineli kümeler açıklanır.  Bu öğreticide varsayılan yapılandırma ayarlarından herhangi birini değiştirmeniz gerekmez, ancak yapılandırma dosyasını inceleyip ayarları tanıyın.  **Düğümler** bölümünde, kümedeki üç düğüm açıklanmaktadır: ad, IP adresi, [düğüm türü, hata etki alanı ve yükseltme etki alanı](service-fabric-cluster-manifest.md#nodes-on-the-cluster).  **Özellikler** bölümü kümenin [güvenlik, güvenilirlik düzeyi, tanılama koleksiyonu ve düğüm türlerini](service-fabric-cluster-manifest.md#cluster-properties) tanımlar.

Bu küme güvenli değildir.  Herkes anonim olarak bağlanıp yönetim işlemleri gerçekleştirebileceğinden, üretim kümeleri her zaman X.509 sertifikaları veya Windows güvenliği kullanılarak güvenli hale getirilmelidir.  Güvenlik yalnızca küme oluşturma sırasında yapılandırılır ve küme oluşturulduktan sonra güvenliği etkinleştirmek mümkün değildir.  Service Fabric küme güvenliği hakkında daha fazla bilgi edinmek için [Küme güvenliğini sağlama](service-fabric-cluster-security.md) makalesini okuyun.  

Üç düğümlü geliştirme kümesini oluşturmak için bir yönetici PowerShell oturumundan *CreateServiceFabricCluster.ps1* betiğini çalıştırın:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Service Fabric çalışma zamanı paketi küme oluşturulurken otomatik olarak indirilir ve yüklenir.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma
Üç düğümlü geliştirme kümeniz artık çalışıyor. ServiceFabric PowerShell modülü çalışma zamanıyla birlikte yüklenir.  Aynı bilgisayardan ya da Service Fabric çalışma zamanına sahip uzak bir bilgisayardan kümenin çalıştığını doğrulayabilirsiniz.  [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet’i, kümeyle bir bağlantı kurar.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
Bir kümeye bağlanmayla ilgili diğer örnekler için bkz. [Güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md). Kümeye bağlandıktan sonra, kümedeki düğümlerin listesini ve her bir düğümün durum bilgilerini görüntülemek için [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet’ini kullanın. Her düğüm için **HealthState** değerinin *OK* olması gerekir.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-the-cluster-using-service-fabric-explorer"></a>Service Fabric Explorer’ı kullanarak kümeyi görselleştirme
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), kümenizi görselleştirmek ve uygulamaları yönetmek için iyi bir araçtır.  Service Fabric Explorer, kümede çalışan ve bir tarayıcıda [http://localhost:19080/Explorer](http://localhost:19080/Explorer) adresine giderek erişebileceğiniz bir hizmettir. 

Küme panosu, kümenize uygulama ve düğüm durumunun özetini de içeren bir genel bakış sağlar. Düğüm görünümü, kümenin fiziksel düzenini gösterir. Belirli bir düğümde, hangi uygulamalara kod dağıtıldığını denetleyebilirsiniz.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-the-cluster"></a>Kümeyi kaldırma
Bir kümeyi kaldırmak için paket klasöründen *RemoveServiceFabricCluster.ps1* PowerShell betiğini çalıştırın ve yolu JSON yapılandırma dosyasına geçirin. İsteğe bağlı olarak silme işleminin günlüğü için bir konum belirtebilirsiniz.

```powershell
# Removes Service Fabric cluster nodes from each computer in the configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

Service Fabric çalışma zamanını bilgisayardan kaldırmak için paket klasöründen aşağıdaki PowerShell betiğini çalıştırın.

```powershell
# Removes Service Fabric from the current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a>Sonraki adımlar
Artık bir tek başına geliştirme kümesi ayarladığınıza göre aşağıdaki makaleleri deneyebilirsiniz:
* [Çok makineli tek başına küme ayarlama](service-fabric-cluster-creation-for-windows-server.md) ve güvenliği etkinleştirme.
* [PowerShell kullanarak uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
