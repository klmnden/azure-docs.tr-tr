---
title: "Azure'da Windows Service Fabric Kümesi oluşturma | Microsoft Docs"
description: "PowerShell kullanarak mevcut bir Azure sanal ağına Windows Service Fabric kümesi dağıtmayı öğrenin."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/22/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 4aee1b0ded7a26df802ca2f05d6e93c153fa0476
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Azure sanal ağına Service Fabric Windows kümesi dağıtma
Bu öğretici, bir serinin birinci bölümüdür. PowerShell ile bir şablon kullanarak bir [Azure sanal ağına (VNET)](../virtual-network/virtual-networks-overview.md) ve [ağ güvenlik grubuna](../virtual-network/virtual-networks-nsg.md) Windows çalıştıran bir Service Fabric kümesi dağıtmayı öğrenirsiniz. Öğreticiyi tamamladığınızda, bulutta çalışan ve uygulama dağıtabileceğiniz bir kümeniz olur.  Azure CLI kullanarak Linux kümesi oluşturmak için bkz. [Azure’da güvenli bir Linux kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

Bu öğreticide bir üretim senaryosu açıklanır.  Test amaçlı olarak hızlıca küçük bir küme oluşturmak istiyorsanız bkz. [Üç düğümlü test kümesi oluşturma](./scripts/service-fabric-powershell-create-test-cluster.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * PowerShell kullanarak Azure’da VNet oluşturma
> * Anahtar kasası oluşturma ve karşıya sertifika yükleme
> * Azure PowerShell’de güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * PowerShell kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure’da güvenli bir küme oluşturma
> * [Bir kümenin ölçeğini daraltma veya genişletme](/service-fabric-tutorial-scale-cluster.md)
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Service Fabric ile API Management dağıtma](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
- [Service Fabric SDK’sını ve PowerShell modülünü](service-fabric-get-started.md) yükleyin
- [Azure Powershell modülü sürüm 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) yükleyin

Aşağıdaki yordamlarda beş düğümlü bir Service Fabric kümesi oluşturulur. Azure’da Service Fabric kümesi çalıştırmaktan kaynaklanan maliyetleri hesaplamak için [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)’nı kullanın.

## <a name="key-concepts"></a>Önemli kavramlar
[Service Fabric kümesi](service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Kümeler binlerce makine içerecek şekilde ölçeklendirilebilir. Bir kümenin parçası olan makine veya VM’lere düğüm denir. Her düğüme bir düğüm adı (bir dize) atanır. Düğümlerin yerleşim özellikleri gibi özellikleri vardır.

Düğüm türü, kümedeki bir grup sanal makinenin boyutunu, sayısını ve özelliklerini tanımlar. Tanımlanan her düğüm türü, bir sanal makine koleksiyonunu küme halinde yönetmek için kullandığınız bir Azure işlem kaynağı olan [sanal makine ölçek kümesi](/azure/virtual-machine-scale-sets/) olarak ayarlanır. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Düğüm türleri, bir düğüm kümesinin "ön uç" veya "arka uç" şeklindeki rolünün tanımlanması için kullanılır.  Kümenizde birden çok düğüm türü olabilir, ancak üretim kümeleri için birincil düğüm türünde en az beş VM (veya test kümeleri için en az üç VM) olmalıdır.  [Service Fabric sistem hizmetleri](service-fabric-technical-overview.md#system-services), birincil düğüm türündeki düğümlere yerleştirilir.

Kümenin güvenliği bir küme sertifikası ile sağlanır. Küme sertifikası, düğümler arası iletişimin güvenliğini sağlamak ve bir yönetim istemcisinde küme yönetimi uç noktalarının kimliğini doğrulamak için kullanılan bir X.509 sertifikasıdır.  Küme sertifikası ayrıca, HTTPS üzerinden HTTPS yönetim API’si ve Service Fabric Explorer için SSL sağlar. Otomatik olarak imzalanan sertifikalar, test kümeleri için kullanışlıdır.  Üretim kümeleri için küme sertifikası olarak bir sertifika yetkilisinden (CA) alınan bir sertifika kullanın.

Küme sertifikası:

- Özel anahtar içermelidir.
- Bir Kişisel Bilgi Değişimi (.pfx) dosyasına aktarılabilen anahtar değişimi için oluşturulmuş olmalıdır.
- Service Fabric kümesine erişmek için kullandığınız etki alanıyla eşleşen bir konu adına sahip olmalıdır. Kümenin HTTPS yönetim uç noktalarına ve Service Fabric Explorer’a yönelik SSL sağlanması için bu eşleşme gereklidir. Bir sertifika yetkilisinden (CA) .cloudapp.azure.com etki alanı için SSL sertifikası edinemezsiniz. Kümeniz için özel bir etki alanı adı edinmeniz gerekir. CA’dan sertifika istediğinizde sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adıyla eşleşmelidir.

Azure’da Service Fabric kümelerine ait sertifikaları yönetmek için Azure Key Vault kullanılır.  Azure’da bir küme dağıtıldığında, Azure Service Fabric kümeleri oluşturmaktan sorumlu Azure kaynak sağlayıcısı sertifikaları Key Vault’tan çeker ve küme VM’lerine yükler.

Bu öğreticide tek bir küme türündeki beş düğüme sahip bir küme dağıtılır. Bununla birlikte, tüm küme dağıtımları için [kapasite planlaması](service-fabric-cluster-capacity.md) önemli bir adımdır. Bu süreç kapsamında dikkat etmeniz gerekenler şunlardır:

- Kümeniz için gerekli düğüm sayısı ve türleri 
- Her düğüm türünün özellikleri (örneğin boyut, birincil, İnternet’e yönelik ve VM sayısı)
- Kümenin güvenilirlik ve dayanıklılık özellikleri

## <a name="download-and-explore-the-template"></a>Şablonu indirin ve keşfedin
Aşağıdaki Resource Manager şablonu dosyalarını indirin:
- [vnet-cluster.json][template]
- [vnet-cluster.parameters.json][parameters]

[vnet-cluster.json][template] tarafından aşağıdakiler dahil çeşitli kaynaklar dağıtılır. 

### <a name="service-fabric-cluster"></a>Service Fabric kümesi
Aşağıdaki özelliklere sahip bir Windows kümesi dağıtılır:
- tek bir düğüm türü 
- birincil düğüm türünde beş düğüm (şablon parametrelerinden yapılandırılabilir)
- İşletim Sistemi: Windows Server 2016 Datacenter with Containers (şablon parametrelerinden yapılandırılabilir)
- sertifikanın güvenliğinin sağlanması (şablon parametrelerinden yapılandırılabilir)
- [ters proxy](service-fabric-reverseproxy.md) etkin
- [DNS hizmeti](service-fabric-dnsservice.md) etkin
- Bronz [dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) (şablon parametrelerinde yapılandırılabilir)
- Gümüş [güvenilirlik düzeyi](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) (şablon parametrelerinden yapılandırılabilir)
- istemci bağlantısı uç noktası: 19000 (şablon parametrelerinde yapılandırılabilir)
- HTTP ağ geçidi uç noktası: 19080 (şablon parametrelerinde yapılandırılabilir)

### <a name="azure-load-balancer"></a>Azure yük dengeleyici
Bir yük dengeleyici dağıtılır ve aşağıdaki bağlantı noktaları için araştırmalarla kurallar ayarlanır:
- istemci bağlantı uç noktası: 19000
- HTTP ağ geçidi uç noktası: 19080 
- uygulama bağlantı noktası: 80
- uygulama bağlantı noktası: 443
- Service Fabric ters proxy’si: 19081

Başka bir uygulama bağlantı noktası gerekiyorsa Microsoft.Network/loadBalancers kaynağı ile Microsoft.Network/networkSecurityGroups kaynağını trafiğin geçmesine izin verecek şekilde ayarlamanız gerekir.

### <a name="virtual-network-subnet-and-network-security-group"></a>Sanal ağ, alt ağ ve ağ güvenlik grubu
Sanal ağ, alt ağ ve ağ güvenlik grubunun adları şablon parametrelerinde bildirilir.  Sanal ağ ve alt ağın adres alanları da şablon parametrelerinde bildirilir:
- sanal ağ adres alanı: 172.16.0.0/20
- Service Fabric alt ağ adres alanı: 172.16.2.0/23

Ağ güvenlik grubunda aşağıdaki gelen trafik kuralları etkindir. Şablon değişkenlerini değiştirerek bağlantı noktası değerlerini değiştirebilirsiniz.
- ClientConnectionEndpoint (TCP): 19000
- HttpGatewayEndpoint (HTTP/TCP): 19080
- SMB : 445
- Internodecommunication - 1025, 1026, 1027
- Kısa ömürlü bağlantı noktası aralığı – 49152-65534 (en az 256 bağlantı noktası gerekir)
- Uygulamanın kullanımına yönelik bağlantı noktaları: 80 ve 443
- Uygulama bağlantı noktası aralığı – 49152-65534 (hizmetler arası iletişim için kullanılır ve diğerlerinden farklı olarak Yük dengeleyicide açılmaz)
- Diğer tüm bağlantı noktalarını engelleyin

Başka bir uygulama bağlantı noktası gerekiyorsa Microsoft.Network/loadBalancers kaynağı ile Microsoft.Network/networkSecurityGroups kaynağını trafiğin geçmesine izin verecek şekilde ayarlamanız gerekir.

## <a name="set-template-parameters"></a>Şablon parametrelerini ayarlama
[vnet-cluster.parameters.json][parameters] parametre dosyası tarafından kümenin ve ilişkili kaynakların dağıtılması için kullanılan birçok değer bildirilir. Dağıtımınız için değiştirmeniz gerekebilecek bazı parametreler:

|Parametre|Örnek değer|Notlar|
|---|---||
|adminUserName|vmadmin| Küme VM’leri için yönetici kullanıcı adı. |
|adminPassword|Password#1234| Küme VM’leri için yönetici parolası.|
|clusterName|mysfcluster123| Kümenin adı. |
|location|southcentralus| Kümenin konumu. |
|certificateThumbprint|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika parmak izi değerini girin. Örneğin: "6190390162C988701DB5676EB81083EA608DCCF3"</p>. | 
|certificateUrlValue|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır. </p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika URL’sini girin. Örneğin: "https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346".</p>|
|sourceVaultValue||<p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için kaynak kasa değerini girin. Örneğin: "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT".</p>|


<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Sanal ağı ve kümeyi dağıtma
Ardından, ağ topolojisini ayarlayın ve Service Fabric kümesini dağıtın. [vnet-cluster.json][template] Resource Manager şablonu bir sanal ağın (VNET) yanı sıra Service Fabric için bir alt ağ ve ağ güvenlik grubu (NSG) da oluşturur. Şablon tarafından sertifika güvenliği etkin bir küme de dağıtılır.  Üretim kümeleri için küme sertifikası olarak bir sertifika yetkilisinden (CA) alınan bir sertifika kullanın. Test kümelerinin güvenliğinin sağlanması için otomatik olarak imzalanan bir sertifika kullanılabilir.

### <a name="create-a-cluster-using-an-existing-certificate"></a>Mevcut bir sertifikayı kullanarak küme oluşturma
Aşağıdaki betik, [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) cmdlet’ini ve bir şablonu kullanarak Azure’da yeni bir küme dağıtır. Ayrıca, cmdlet tarafından Azure’da yeni bir anahtar kasası oluşturulur ve sertifikanız karşıya yüklenir. 

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # must match the clusterName parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\vnet-linuxcluster.json" `
-ParameterFile "$templatepath\vnet-linuxcluster.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-using-a-new-self-signed-certificate"></a>Yeni ve otomatik olarak imzalanan bir sertifika ile küme oluşturma
Aşağıdaki betik, [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) cmdlet’ini ve bir şablonu kullanarak Azure’da yeni bir küme dağıtır. Ayrıca, cmdlet tarafından Azure’da yeni bir anahtar kasası oluşturulup bu kasaya otomatik olarak imzalanan yeni bir sertifika eklenir ve sertifika dosyası yerel olarak indirilir. 

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\vnet-linuxcluster.json" `
-ParameterFile "$templatepath\vnet-linuxcluster.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname -CertificateSubjectName $subname

```

## <a name="connect-to-the-secure-cluster"></a>Güvenli kümeye bağlanma
Service Fabric SDK’sı ile birlikte yüklenen Service Fabric PowerShell modülünü kullanarak kümeye bağlanın.  İlk olarak sertifikayı bilgisayarınızda geçerli kullanıcının Kişisel (My) deposuna yükleyin.  Aşağıdaki PowerShell komutunu çalıştırın:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Şimdi güvenli kümenize bağlanmaya hazırsınız.

**Service Fabric** PowerShell modülü, Service Fabric kümelerini, uygulamalarını ve hizmetlerini yönetmek için birçok cmdlet sağlar.  Güvenli kümeye bağlanmak için [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet'ini kullanın. Sertifika parmak izi ve bağlantı uç noktası ayrıntıları önceki adımda elde edilen çıktıdan bulunabilir.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

[Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet'ini kullanarak bağlı olduğunuzu ve kümenin iyi durumda olduğunu doğrulayın.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğretici serisindeki diğer makalelerde, oluşturduğunuz küme kullanılır. Hemen bir sonraki makaleye geçmeyecekseniz ücretlendirmeden kaçınmak için kümeyi ve anahtar kasasını silmek isteyebilirsiniz. Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir.

[Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet’ini kullanarak kaynak grubunu ve tüm küme kaynaklarını silin.  Anahtar kasasını içeren kaynak grubunu da silin.

```powershell
Remove-AzureRmResourceGroup -Name $groupname -Force
Remove-AzureRmResourceGroup -Name $vaultgroupname -Force
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * PowerShell kullanarak Azure’da VNet oluşturma
> * Anahtar kasası oluşturma ve karşıya sertifika yükleme
> * PowerShell kullanarak Azure’da güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * PowerShell kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Ardından, kümenizi nasıl ölçeklendirebileceğinizi öğrenmek üzere aşağıdaki öğreticiye geçin.
> [!div class="nextstepaction"]
> [Küme Ölçeklendirme](service-fabric-tutorial-scale-cluster.md)


[template]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-cluster.json
[parameters]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-cluster.parameters.json
