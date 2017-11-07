---
title: "Bir tek başına Service Fabric kümesi düğümlerine ekleyip | Microsoft Docs"
description: "Bir fiziksel veya şirket içi olarak Windows Server çalıştıran sanal makine veya tüm buluttaki bir Azure Service Fabric kümesi düğümlerine ekleyip öğrenin."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: 252dcdf0ff9e1fecd6665808bfe7978a4417018b
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Windows Server'da çalışan bir tek başına Service Fabric kümesi düğümlerine Ekle Kaldır
Sonra [Windows Server makinelerde tek başına Service Fabric kümesi oluşturulan](service-fabric-cluster-creation-for-windows-server.md)(iş) gereksinimlerinizi değiştirebilir ve, küme düğümlerine ekleyip gerekir. Bu makalede, bunu başarmak için ayrıntılı adımlar sağlanmaktadır. Lütfen Ekle/Kaldır düğümü işlevselliği yerel geliştirme kümelerinde desteklenmez unutmayın.

## <a name="add-nodes-to-your-cluster"></a>İçin küme düğümleri Ekle

1. Özetlenen adımları izleyerek, kümeye eklemek istediğiniz VM/makineyi hazırlama [planlama ve Service Fabric kümesi dağıtımınızı hazırlama](service-fabric-cluster-creation-for-windows-server.md)
2. Hangi hata etki alanı ve bu VM/makineye eklemek için kalacaklarını yükseltme etki alanını tanımlayın
3. Kümeye eklemek istediğiniz VM/makinede Uzak Masaüstü (RDP)
4. Kopyalama veya [Windows Server için Service Fabric tek başına paketini karşıdan](http://go.microsoft.com/fwlink/?LinkId=730690) VM/makineye ve paketin sıkıştırmasını açın
5. PowerShell yükseltilmiş ayrıcalıklarla çalıştırın ve sıkıştırması açılmış paket konumuna gidin
6. Çalıştırma *AddNode.ps1* komut dosyası eklemek için yeni düğümü açıklayan parametrelere sahip. Aşağıdaki örnek VM5 olarak adlandırılan yeni bir düğüm ekler, türüyle NodeType0 ve IP 182.17.34.52, UD1 ve fd halinde adresi: / dc1/r0. *ExistingClusterConnectionEndPoint* bağlantı uç noktası için bir düğüm zaten var olan küme IP adresini olabilen *herhangi* kümedeki düğüm.

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    Betik çalışması bittikten sonra yeni düğümü çalıştırarak eklenip eklenmediğini denetleyebilirsiniz [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet'i.

7. Kümedeki farklı düğümleri arasında tutarlılık sağlamak için bir yapılandırma yükseltme başlatması gerekir. Çalıştırma [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) en son yapılandırma dosyasını alın ve yeni eklenen düğümü "Düğümleri" bölümüne ekleyin. Ayrıca, her zaman kullanılabilir durumda aynı yapılandırmaya sahip bir küme redploy için gereken en son küme yapılandırmasını sağlamak için önerilir.

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
    Service Fabric Explorer üzerinde yükseltmesinin ilerleme durumunu izleyebilirsiniz. Alternatif olarak, çalıştırabilirsiniz [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a>Windows güvenliği kullanarak gMSA yapılandırılmış kümelerine düğümleri Ekle
Grup yönetilen hizmet Account(gMSA) ile (https://technet.microsoft.com/library/hh831782.aspx) yapılandırılmış kümeler için yapılandırma yükseltme kullanarak yeni bir düğüm eklenebilir:
1. Çalıştırma [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) herhangi bir son yapılandırma dosyasını alın ve "Düğümleri" bölümünde eklemek istediğiniz yeni düğümün ayrıntılarını eklemek için var olan düğümler. Yeni düğümü aynı yönetilen grup hesabının bir parçası olduğundan emin olun. Bu hesap tüm makinelerde yönetici olmanız gerekir.

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
    Service Fabric Explorer üzerinde yükseltmesinin ilerleme durumunu izleyebilirsiniz. Alternatif olarak, çalıştırabilirsiniz [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-node-types-to-your-cluster"></a>Düğüm türleri, kümeye ekleme
Yeni düğüm türü eklemek için "NodeTypes" bölümünde "Özellikler" altında ve bir yapılandırmaya başlamadan yeni düğüm türü içerecek şekilde yapılandırmanızı değiştirin kullanarak yükseltme [başlangıç ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps). Yükseltme işlemi tamamlandıktan sonra bu düğüm türü ile kümeye yeni düğümler ekleyebilirsiniz.

## <a name="remove-nodes-from-your-cluster"></a>Kümeden düğüm kaldırma
Aşağıdaki şekilde bir yapılandırma yükseltme kullanarak bir kümeden bir düğümü kaldırılabilir:

1. Çalıştırma [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) en son yapılandırma dosyasını almak için ve *kaldırmak* "Düğümleri" bölümünden düğümü.
"NodesToBeRemoved" parametresi "Kurulumu" bölümünde "FabricSettings" bölümün içine ekleyin. "Değeri" kaldırılmaları gerekir düğümleri düğüm adlarının virgülle ayrılmış bir listesi olmalıdır.

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
    Service Fabric Explorer üzerinde yükseltmesinin ilerleme durumunu izleyebilirsiniz. Alternatif olarak, çalıştırabilirsiniz [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

> [!NOTE]
> Düğümleri kaldırma birden fazla yükseltme başlatabilir. Bazı düğümler işaretlenir `IsSeedNode=”true”` etiketi ve küme sorgulayarak tanımlanabilir kullanarak bildirim `Get-ServiceFabricClusterManifest`. Çekirdek düğümleri gibi senaryolarda taşınacak gerekeceğinden bu düğümleri kaldırılmasını diğerlerinden daha uzun sürebilir. Küme en az 3 birincil düğüm türü düğümleri bulundurmanız gerekir.
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>Düğüm türleri, kümeden kaldırma
Düğüm türü başvuran tüm düğümleri varsa bir düğüm türü kaldırmadan önce çift gözden geçirin. Bu düğümler, karşılık gelen düğüm türü kaldırmadan önce kaldırın. Tüm ilgili düğümleri kaldırıldıktan sonra NodeType kümesi yapılandırmasından kaldırmak ve bir yapılandırmaya başlamadan kullanarak yükseltme [başlangıç ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).


### <a name="replace-primary-nodes-of-your-cluster"></a>Birincil düğüm kümenizin değiştirin
Birincil düğüm değiştirme ardına toplu olarak ekleme ve kaldırma yerine gerçekleştirilen bir düğüm olmalıdır.


## <a name="next-steps"></a>Sonraki adımlar
* [Tek başına Windows kümesi için yapılandırma ayarları](service-fabric-cluster-manifest.md)
* [Tek başına küme X509 kullanarak Windows güvenli sertifikaları](service-fabric-windows-cluster-x509-security.md)
* [Azure VM'ler Windows çalıştıran bir tek başına Service Fabric kümesi oluştur](service-fabric-cluster-creation-with-windows-azure-vms.md)

