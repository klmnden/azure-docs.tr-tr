---
title: "Windows Service Fabric kümesi oluşturma | Microsoft Docs"
description: "Windows Service Fabric kümesi PowerShell kullanarak mevcut bir Azure sanal ağı uygulamasına dağıtmayı öğrenin."
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
ms.date: 11/02/2017
ms.author: ryanwi
ms.openlocfilehash: 1ac5ca34e412aeb8b24e657abfe8eca04943799d
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Azure sanal ağda bir doku Windows hizmeti kümesini dağıtma
Bu öğretici bir dizi birini bir parçasıdır. Mevcut bir Azure sanal ağı (VNET) Windows Service Fabric kümesine dağıtmak ve PowerShell kullanarak alt net öğreneceksiniz. İşlemi tamamladığınızda, uygulamaları dağıtabileceğiniz bulutta çalıştıran bir kümeye sahip.  Azure CLI kullanarak bir Linux kümesi oluşturmak için bkz: [Azure'da güvenli bir Linux kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * PowerShell kullanarak Azure'da bir sanal ağ oluşturma
> * Bir anahtar kasası oluşturun ve bir sertifika yükleyin
> * Azure PowerShell'de güvenli bir Service Fabric kümesi oluştur
> * Küme bir X.509 sertifikası ile güvenli
> * PowerShell kullanarak kümeye bağlanma
> * Bir küme Kaldır

Bu öğretici serisinde öğrenin nasıl yapılır:
> [!div class="checklist"]
> * Azure üzerinde güvenli bir küme oluşturun
> * [API Management Service Fabric ile dağıtma](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Yükleme [Service Fabric SDK ve PowerShell Modülü](service-fabric-get-started.md)
- Yükleme [Azure Powershell modülü sürümü 4.1 veya üzeri](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

Aşağıdaki yordamlar, beş düğümlü Service Fabric kümesi oluşturun. Service Fabric kümesi Azure kullanımda çalıştırarak ücrete maliyeti hesaplamak için [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/).

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Azure'da oturum açın ve aboneliğinizi seçin
Bu kılavuz, Azure PowerShell'i kullanır. Yeni bir PowerShell oturumu başlattığınızda, Azure hesabınızda oturum açın ve Azure komutları çalıştırmadan önce aboneliğinizi seçin.
 
Azure hesabınızda oturum açmak için aşağıdaki komutu çalıştırın, aboneliğinizi seçin:

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Dağıtımınız için yeni bir kaynak grubu oluşturun ve bir ad ve bir konum girin.

```powershell
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc
```

## <a name="deploy-the-network-topology"></a>Ağ topolojisi dağıtmak
Ardından, ağ topolojisini için API Management ve Service Fabric kümesi dağıtılacak ayarlayın. [Network.json] [ network-arm] Resource Manager şablonu yapılandırılmış bir sanal ağ (VNET) ve ayrıca bir alt ağ ve ağ güvenlik grubu (NSG) ve bir alt ağ ve NSG Service Fabric için API Management oluşturmak için . Daha fazla hakkında sanal ağlar, alt ağları ve Nsg'ler öğrenin [burada](../virtual-network/virtual-networks-overview.md).

[Network.parameters.json] [ network-parameters-arm] parametreler dosyası alt ağları ve Service Fabric ve API Management dağıtmak Nsg'ler adlarını içerir.  API Management dağıtıldığı [öğretici aşağıdaki](service-fabric-tutorial-deploy-api-management.md). Bu kılavuz, parametre değerlerini değiştirilmesi gerekmez. Service Fabric Resource Manager şablonları bu değerleri kullanın.  Değerler burada değiştirildiyse, onları bu öğreticide kullanılan diğer Resource Manager şablonları olarak değiştirmeniz gerekir ve [dağıtmak API Management Öğreticisi](service-fabric-tutorial-deploy-api-management.md). 

Aşağıdaki Resource Manager şablonu ve parametre dosyasını karşıdan yükleyin:
- [Network.JSON][network-arm]
- [Network.Parameters.JSON][network-parameters-arm]

Ağ Kurulumu Resource Manager şablonu ve parametre dosyalarını dağıtmak için aşağıdaki PowerShell komutunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile C:\winclustertutorial\network.json -TemplateParameterFile C:\winclustertutorial\network.parameters.json -Verbose
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>Service Fabric kümesi dağıtma
Ağ kaynaklarını dağıtma tamamladıktan sonra sonraki adım bir Service Fabric kümesi sanal ağ alt ağındaki ve Service Fabric kümesi için belirlenmiş NSG dağıtmaktır. Var olan bir VNET ve alt ağ (Bu makalede daha önce dağıttığınız) için bir küme dağıtımı Resource Manager şablonu gerektirir.  Bu öğretici seri için şablon sanal ağ, alt ağ ve bir önceki adımda ayarladığınız NSG adları kullanmak üzere önceden yapılandırılmıştır.  

Aşağıdaki Resource Manager şablonu ve parametre dosyasını karşıdan yükleyin:
- [Cluster.JSON][cluster-arm]
- [Cluster.Parameters.JSON][cluster-parameters-arm]

Güvenli bir küme oluşturmak için bu şablonu kullanın.  Küme düğümü, düğümü iletişimi güvenli hale getirmek ve bir yönetim istemcisi için küme yönetim uç noktalarının kimliğini doğrulamak için kullanılan bir X.509 sertifikası sertifikasıdır.  Küme sertifika ayrıca bir SSL HTTPS yönetim API'si ve Service Fabric Explorer için HTTPS üzerinden sağlar. Azure anahtar kasası, Azure Service Fabric kümeleri sertifikalarını yönetmek için kullanılır.  Bir küme Azure'da dağıtıldığında, Azure kaynak sağlayıcısı Service Fabric kümeleri oluşturmak için sorumlu sertifikaları anahtar Kasası'nı çeker ve VM'ler kümede yükler. 

Küme sertifikası veya test amacıyla bir sertifika yetkilisinden (CA) bir sertifika kullanmak, otomatik olarak imzalanan bir sertifika oluşturun. Küme sertifika gerekir:

- özel anahtarı içerir.
- bir kişisel bilgi değişimi (.pfx) dosyası verilebilir anahtar değişimi için oluşturulamıyor.
- Service Fabric kümesi erişmek için kullandığınız etki alanı ile eşleşen bir konu adına sahip. Bu eşleştirme, kümenin HTTPS yönetim uç noktaları ve Service Fabric Explorer için SSL sağlamak için gereklidir. İçin bir sertifika yetkilisinden (CA) bir SSL sertifikası alınamıyor. cloudapp.azure.com etki alanı. Özel etki alanı adı, kümeniz için edinmeniz gerekir. Bir CA'dan bir sertifika isteme, sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adı eşleşmelidir.

Boş dolgu *konumu*, *clusterName*, *adminUserName*, ve *Admınpassword* parametrelerinde  *Cluster.Parameters.JSON* dağıtımınız için dosya.  Bırakın *certificateThumbprint*, *certificateUrlValue*, ve *sourceVaultValue* parametreleri otomatik olarak imzalanan bir sertifika oluşturmak için boş.  Daha önce bir anahtar Kasası'na yüklenen var olan bir sertifikayı kullanmak istiyorsanız, bu parametre değerlerini doldurun.

Aşağıdaki komut dosyası kullanan [yeni AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) cmdlet'i ve azure'da yeni bir küme dağıtmak için şablon. Cmdlet ayrıca Azure'da yeni bir anahtar kasası oluşturur, anahtar Kasası'na yeni bir otomatik olarak imzalanan sertifika ekler ve sertifika dosyasını yerel olarak indirir. Diğer parametreleri kullanarak, varolan bir sertifikayı ve/veya anahtar kasası belirtebilirsiniz [yeni AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) cmdlet'i.

```powershell
# Variables.
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster"
$vaultname = "clusterkeyvault111"
$vaultgroupname="clusterkeyvaultgroup111"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile 'C:\winclustertutorial\cluster.json' `
-ParameterFile 'C:\winclustertutorial\cluster.parameters.json' -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname -CertificateSubjectName $subname
```

## <a name="connect-to-the-secure-cluster"></a>Güvenli kümeye bağlanın
Service Fabric SDK ile birlikte yüklenen Service Fabric PowerShell modülünü kullanarak kümeye bağlanın.  İlk olarak, geçerli kullanıcının, bilgisayarınızdaki kişisel (My) deposuna sertifikası yükleyin.  Aşağıdaki PowerShell komutunu çalıştırın:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Şimdi güvenli kümenize bağlanmaya hazırsınız.

**Service Fabric** PowerShell Modülü Service Fabric kümeleri, uygulamaları ve hizmetleri yönetmek için birçok cmdlet'leri sağlar.  Kullanım [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet'ini güvenli kümeye bağlanın. Sertifika parmak izi ve bağlantı uç nokta ayrıntılarını önceki adımdaki çıktı bulunur.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Bağlı ve küme sağlıklı olduğundan emin olun kullanarak [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet'i.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğretici serisinde diğer makaleleri oluşturduğunuz küme kullanın. Sonraki makalede açın hemen taşıma değilseniz, küme ve Ücret oluşmasını önlemek için anahtar kasası silmek isteyebilirsiniz. Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir.

Kaynak grubu ve kullanarak tüm küme kaynakları silmek [Remove-AzureRMResourceGroup cmdlet'i](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).  Aynı zamanda anahtar kasası içeren kaynak grubunu silin.

```powershell
Remove-AzureRmResourceGroup -Name $groupname -Force
Remove-AzureRmResourceGroup -Name $vaultgroupname -Force
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * PowerShell kullanarak Azure'da bir sanal ağ oluşturma
> * Bir anahtar kasası oluşturun ve bir sertifika yükleyin
> * PowerShell kullanarak Azure'da güvenli bir Service Fabric kümesi oluştur
> * Küme bir X.509 sertifikası ile güvenli
> * PowerShell kullanarak kümeye bağlanma
> * Bir küme Kaldır

Ardından, API Management Service Fabric ile dağıtma hakkında bilgi edinmek için aşağıdaki öğreticiyi ilerleyin.
> [!div class="nextstepaction"]
> [API Yönetimi dağıtma](service-fabric-tutorial-deploy-api-management.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json
