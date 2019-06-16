---
title: Tek başına Service Fabric küme düğümleri Ekle Kaldır | Microsoft Docs
description: Bir fiziksel veya sanal makine şirket içi olabilir Windows Server çalıştıran şirket içindeki veya buluttaki bir Azure Service Fabric küme düğümleri ekleyebilir veya kaldırabilirsiniz öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: 585d918026ca40bc1a04c55e2bac454492c55936
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60711042"
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Windows Server üzerinde çalışan bir tek başına Service Fabric küme düğümleri ekleyebilir veya kaldırabilirsiniz
Sonra [tek başına Service Fabric kümeniz Windows Server makinelerinde oluşturulan](service-fabric-cluster-creation-for-windows-server.md)(iş) ihtiyaçlarınızı değişebilir ve kümenize düğümleri ekleyebilir veya kaldırabilirsiniz gerekecektir. Bu makalede Bunu başarmak için ayrıntılı adımlar verilmektedir. Ekle/Kaldır düğüm işlev yerel geliştirme kümelerinde desteklenmiyor unutmayın.

## <a name="add-nodes-to-your-cluster"></a>Makinenizden küme düğümleri Ekle

1. Özetlenen adımları izleyerek kümenize eklemek istediğiniz VM/makinesini hazırlama [planlama ve Service Fabric kümesine dağıtımınızı hazırlama](service-fabric-cluster-creation-for-windows-server.md)
2. Hangi hata etki alanı ve yükseltme etki alanı bu VM/makineye eklemek için oluşturacağınız belirleyin
3. Kümeye eklemek istediğiniz VM/makinede Uzak Masaüstü (RDP)
4. Kopyalama veya [Windows Server için Service Fabric tek başına paketi indirmek](https://go.microsoft.com/fwlink/?LinkId=730690) VM/makineye ve paketin sıkıştırmasını açın
5. PowerShell'i yükseltilmiş ayrıcalıklarla çalıştırın ve sıkıştırması açılmış paketin konuma gidin
6. Çalıştırma *AddNode.ps1* komut dosyası eklemek için Yeni düğümün parametrelere sahip. Aşağıdaki örnekte VM5 adında yeni bir düğüm ekler, türüyle NodeType0 ve IP 182.17.34.52, UD1 ve fd adres: / dc1/r0. *ExistingClusterConnectionEndPoint* bir düğüm zaten var olan bir küme için bir bağlantı uç noktası IP adresini olabilen *herhangi* kümedeki düğüm.

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    Betiğin çalışması tamamlandıktan sonra yeni düğümü çalıştırarak eklenip eklenmediğini denetleyebilirsiniz [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet'i.

7. Farklı kümenin düğümleri arasında tutarlılık sağlamak için bir yapılandırma yükseltmeyi başlatmak gerekir. Çalıştırma [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) en son yapılandırma dosyasını alın ve yeni eklenen düğümü "Düğüm" bölümüne ekleyin. Ayrıca, her zaman aynı yapılandırmaya sahip bir kümeyi yeniden yapmanız durumunda kullanılabilir en son küme yapılandırmasını sağlamak için önerilir.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. Çalıştırma [başlangıç ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) yükseltmeye başlamak için.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    Service Fabric Explorer yükseltmesinin ilerleme durumunu izleyebilirsiniz. Alternatif olarak, çalıştırabileceğiniz [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a>Gmsa'yı kullanarak Windows Güvenlik ile yapılandırılan kümeler için düğümleri Ekle
Grup yönetilen hizmet Account(gMSA) ile yapılandırılan kümeler (https://technet.microsoft.com/library/hh831782.aspx), yapılandırma yükseltme kullanarak yeni bir düğüm eklenebilir:
1. Çalıştırma [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) herhangi birinde var olan düğümlerin en son yapılandırma dosyasını alın ve "Düğüm" bölümünde eklemek istediğiniz yeni düğümün ayrıntılarını ekleyin. Yeni düğüm aynı yönetilen grup hesabı bir parçası olduğundan emin olun. Bu hesap tüm makinelerinde Yönetici olmalıdır.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. Çalıştırma [başlangıç ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) yükseltmeye başlamak için.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
    ```
    Service Fabric Explorer yükseltmesinin ilerleme durumunu izleyebilirsiniz. Alternatif olarak, çalıştırabileceğiniz [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-node-types-to-your-cluster"></a>Düğüm türleri kümenize ekleyin
Yeni bir düğüm türü eklemek için "NodeType" altındaki "Özellikler" bölümüne ve bir yapılandırmaya başlamadan yeni bir düğüm türü eklemek için yapılandırmayı değiştirmek kullanarak yükseltme [başlangıç ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps). Yükseltme tamamlandıktan sonra bu düğüm türü ile kümenize yeni düğümler ekleyebilirsiniz.

## <a name="remove-nodes-from-your-cluster"></a>Kümeden düğüm kaldırma
Aşağıdaki şekilde bir yapılandırma yükseltme kullanarak bir kümeden bir düğümü kaldırılabilir:

1. Çalıştırma [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) en son yapılandırma dosyasını almak için ve *Kaldır* "Düğüm" bölümünden düğümü.
"Kurulum" bölümüne "FabricSettings" bölümü içinde "NodesToBeRemoved" parametresini ekleyin. "value" kaldırılması gerekir düğümlerinin düğümü adlarını virgülle ayrılmış listesi olmalıdır.

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. Çalıştırma [başlangıç ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) yükseltmeye başlamak için.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    Service Fabric Explorer yükseltmesinin ilerleme durumunu izleyebilirsiniz. Alternatif olarak, çalıştırabileceğiniz [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

> [!NOTE]
> Birden fazla yükseltme düğümleri kaldırma işlemi başlatabilir. Bazı düğümler ile işaretlenmiş `IsSeedNode=”true”` etiketi ve küme sorgulayarak tanımlanabilir kullanarak bildirim `Get-ServiceFabricClusterManifest`. Böyle senaryolarda taşınmasını çekirdek düğümleri olduğundan bu düğümleri kaldırılmasını diğerlerinden daha uzun sürebilir. Küme en az 3 birincil düğüm türü düğüm sürdürmeniz gerekir.
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>Düğüm türleri, kümeden kaldırma
Başvuran düğüm türü herhangi bir düğüm varsa, bir düğüm türü kaldırmadan önce çift gözden geçirin. Bu düğümler, ilgili düğüm türü kaldırmadan önce kaldırın. Tüm ilgili düğümleri kaldırdıktan sonra küme yapılandırmasından NodeType kaldırın ve bir yapılandırmaya başlamadan kullanarak yükseltme [başlangıç ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).


### <a name="replace-primary-nodes-of-your-cluster"></a>Birincil düğüm kümenizin değiştirin
Birincil düğüm değiştirme ardına kaldırarak ve ardından toplu olarak ekleme yerine gerçekleştirilen bir düğüm olmalıdır.


## <a name="next-steps"></a>Sonraki adımlar
* [Tek başına Windows Küme yapılandırma ayarları](service-fabric-cluster-manifest.md)
* [X509 kullanarak Windows üzerinde tek başına küme güvenliğini sağlama sertifikaları](service-fabric-windows-cluster-x509-security.md)
* [Windows çalıştıran Azure Vm'leriyle bir tek başına Service Fabric kümesi oluşturma](service-fabric-cluster-creation-with-windows-azure-vms.md)

