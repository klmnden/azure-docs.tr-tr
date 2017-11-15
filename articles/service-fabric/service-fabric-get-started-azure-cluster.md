---
title: "Azure Service Fabric kümesi ayarlama | Microsoft Docs"
description: "Bu hızlı başlangıç, Azure’da bir Windows veya Linux Service Fabric kümesi oluşturmanıza yardımcı olur."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2017
ms.author: ryanwi
ms.openlocfilehash: caf76bb739fa92982c511c8e3e6c6aaf2bf9d6c1
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Azure’da ilk Service Fabric kümenizi oluşturma
[Azure Service Fabric kümesi](service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Bu hızlı başlangıç, [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) veya [Azure portalı](http://portal.azure.com) üzerinden yalnızca birkaç dakika içinde Windows veya Linux üzerinde çalışan beş düğümlü bir küme oluşturmanıza yardımcı olur. Bu görev için Azure CLI de kullanabilirsiniz.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="use-the-azure-portal"></a>Azure portalı kullanma

[http://portal.azure.com](http://portal.azure.com) sayfasından Azure portalında oturum açın.

### <a name="create-the-cluster"></a>Kümeyi oluşturma

1. Azure portalının sol üst köşesindeki **Yeni** öğesini seçin.
2. **Service Fabric** araması yapın ve döndürülen sonuçlarda **Service Fabric Kümesi**'ni seçin. Ardından **Oluştur**’u seçin.
3. Service Fabric **Temel Bilgileri** formunu doldurun. **İşletim sistemi** için küme düğümlerinin çalışmasını istediğiniz Windows veya Linux sürümünü seçin. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. **Kaynak grubu** için yeni bir tane oluşturun. Kaynak grubu, Azure kaynaklarının oluşturulup toplu olarak yönetildiği bir mantıksal kapsayıcıdır. İşiniz bittiğinde **Tamam**’ı seçin.

    ![Küme ayarı çıktısının ekran görüntüsü][cluster-setup-basics]

4. **Küme yapılandırması** formunu doldurun. **Düğüm türü sayısı** için **1** değerini girin.

5. **Düğüm türü 1 (Birincil)**’i seçip **Düğüm türü yapılandırma** formunu doldurun. Düğüm türü adını girin ve [Dayanıklılık katmanı](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) için **Bronz** seçeneğini belirleyin. VM boyutunu seçin.

    Düğüm türleri VM boyutunu, VM sayısını, özel uç noktaları ve aynı türdeki VM’lerin diğer ayarlarını tanımlar. Tanımlanan her düğüm türü, sanal makineleri küme olarak dağıtıp yönetmek için kullanılan ayrı bir sanal makine ölçek kümesi olarak ayarlanır. Her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Birinci veya birincil düğüm türü, Service Fabric sistem hizmetlerinin barındırıldığı yerdir. Bu düğüm beş ya da daha fazla VM içermelidir.

    Herhangi bir üretim dağıtımı için [kapsite planlaması](service-fabric-cluster-capacity.md) önemli bir adımdır. Ancak, bu hızlı başlangıçta uygulama çalıştırmadığınız için bir *DS1_v2 Standart* VM boyutu seçin. [Güvenilirlik katmanı](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) için **Gümüş** seçeneğini belirleyin ve sanal makine ölçek kümesinin başlangıç kapasitesini 5 olarak belirtin.  

    Özel uç noktalar Azure Load Balancer’da bağlantı noktaları açtığı için, küme üzerinde çalışan uygulamalarla bağlantı kurabilirsiniz.  80 ve 8172 bağlantı noktalarını açmak için **80, 8172** girin.

    TCP/HTTP yönetim uç noktalarını, uygulama bağlantı noktası aralıklarını, [yerleştirme kısıtlamalarını](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) ve [kapasite özelliklerini](service-fabric-cluster-resource-manager-metrics.md) özelleştirmek için kullanılan **Gelişmiş ayarları yapılandır** kutusunu seçmeyin.    
    
    ![Düğüm türü yapılandırmasının ekran görüntüsü][node-type-config]

    **Tamam**’ı seçin.

6. **Küme yapılandırması** formunda **Tanılama** ayarını **Açık** olarak belirleyin. Bu hızlı başlangıçta herhangi bir [yapı ayarı](service-fabric-cluster-fabric-settings.md) özelliği girmeniz gerekmez.  Microsoft’un kümeyi çalıştıran yapı kodu sürümünü otomatik olarak güncelleştirmesi için **Fabric sürümü** menüsünde **Otomatik** yükseltme modunu seçin.  Yükseltmenin yapılacağı [desteklenen bir sürüm seçmek](service-fabric-cluster-upgrade.md) istiyorsanız modu **El ile** olarak ayarlayın.     

    **Tamam**’ı seçin.

7. **Güvenlik** formunu doldurun. Bu hızlı başlangıç için **Güvenli olmayan**’ı seçin. Genel olarak, üretim iş yükleri için güvenli bir küme oluşturmanız gerekir. Güvenli olmayan bir kümeye herkes anonim olarak bağlanabilir ve yönetim işlemleri yapabilir.  

   Service Fabric bir küme ile uygulamalarının çeşitli yönlerini güvenli hale getirmek üzere kimlik doğrulaması ve şifreleme sağlamak için sertifikaları kullanır. Daha fazla bilgi için bkz. [Service Fabric kümesi güvenlik senaryoları](service-fabric-cluster-security.md). Azure Active Directory kullanarak kullanıcı kimlik doğrulamasını etkinleştirmek veya uygulama güvenliği için sertifika ayarlamak üzere bkz. [bir Resource Manager şablonundan küme oluşturma](service-fabric-cluster-creation-via-arm.md).

    **Tamam**’ı seçin.

8. Özeti gözden geçirin. Girdiğiniz ayarlarla derlenmiş bir Azure Resource Manager şablonu indirmek isterseniz, **Şablon ve parametreleri indir**’i seçin. **Oluştur**’u seçerek kümeyi oluşturun.

    Oluşturma işleminin ilerleme durumunu bildirimler bölümünden görebilirsiniz. (Ekranınızın sağ üst köşesindeki durum çubuğunun yanında bulunan "Zil" simgesini seçin.) Kümeyi oluştururken **Başlangıç Panosuna Sabitle**’yi seçtiyseniz, **Service Fabric Kümesi Dağıtılıyor** öğesinin **Başlangıç** panosuna sabitlendiğini görürsünüz.

### <a name="connect-to-the-cluster-by-using-powershell"></a>PowerShell kullanarak kümeye bağlanma
PowerShell ile bağlanarak kümenin çalıştığını doğrulayın. Service Fabric PowerShell modülü, [Service Fabric SDK’sı](service-fabric-get-started.md) ile birlikte yüklenir. [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet’i, kümeyle bir bağlantı kurar.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
Bir kümeye bağlanmayla ilgili diğer örnekler için bkz. [Güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md). Kümeye bağlandıktan sonra, kümedeki düğümlerin listesini ve her bir düğümün durum bilgilerini görüntülemek için [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet’ini kullanın. Her düğüm için **HealthState** değerinin *OK* olması gerekir.

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-the-cluster"></a>Kümeyi kaldırma
Service Fabric kümesi, küme kaynağının yanı sıra diğer Azure kaynaklarından oluşur. Bir Service Fabric kümesini tamamen silmek için onu oluşturan tüm kaynakları da silmeniz gerekir. Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir. Bir kümeyi veya bir kaynak grubundaki bazı (tümü değil) kaynakları silmenin diğer yolları için bkz. [Küme silme](service-fabric-cluster-delete.md).

Azure portalında bir kaynak grubunu silin:
1. Silmek istediğiniz Service Fabric kümesine göz atın.
2. Küme temel bileşenleri sayfasında **Kaynak Grubu** adını seçin.
3. **Kaynak Grubu Temel Bileşenleri** sayfasında **Kaynak grubunu sil**’i seçin. Sonra bu sayfadaki yönergeleri izleyerek kaynak grubu silme işlemini tamamlayın.
    ![Kaynak grubunu sil seçeneğinin vurgulandığı Kaynak Grubu Temel Bileşenleri sayfasının ekran görüntüsü][cluster-delete]


## <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Kümeyi oluşturmanın başka bir yolu ise PowerShell kullanmaktır. Bunu yapmak için:

1. [Azure Powershell modülü sürüm 4.0 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) bilgisayarınıza indirin.

2. Güvenliği X.509 sertifikasıyla sağlanan beş düğümlü bir Service Fabric kümesi oluşturmak için [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/new-azurermservicefabriccluster) cmdlet'ini çalıştırın. Bu komut otomatik olarak imzalanan bir sertifika oluşturur ve bunu yeni bir anahtar kasasına yükler. Sertifika aynı zamanda bir yerel dizine de kopyalanır. Küme düğümleri üzerinde çalışan Windows veya Linux sürümünü seçmek için *-OS* parametresini ayarlayın. Parametreleri gereken şekilde özelleştirin. 

    ```powershell
    #Provide the subscription Id
    $subscriptionId = 'yourSubscriptionId'

    # Certificate variables.
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $certfolder="c:\mycertificates\"

    # Variables for VM admin.
    $adminuser="vmadmin"
    $adminpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 

    # Variables for common values
    $clusterloc="SouthCentralUS"
    $clustername = "mysfcluster"
    $groupname="mysfclustergroup"       
    $vmsku = "Standard_D2_v2"
    $vaultname = "mykeyvault"
    $subname="$clustername.$clusterloc.cloudapp.azure.com"

    # Set the number of cluster nodes. Possible values: 1, 3-99
    $clustersize=5 

    # Set the context to the subscription ID where the cluster will be created
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Create the Service Fabric cluster.
    New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
    -ClusterSize $clustersize -VmUserName $adminuser -VmPassword $adminpwd -CertificateSubjectName $subname `
    -CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
    -OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname
    ```

    Komutun tamamlanması 10 dakika ile 30 dakika arasında sürebilir. Çıktıda, sertifikayla ilgili bilgiler, sertifikanın yüklendiği anahtar kasası ve sertifikanın kopyalandığı yerel klasör bulunur.     

3. Tüm çıktıyı kopyalayın ve bir metin dosyasına kaydedin (daha sonra başvurmanız gerekecek). Çıktıda aşağıdaki bilgileri not edin: 

    - CertificateSavedLocalPath
    - CertificateThumbprint
    - ManagementEndpoint
    - ClientConnectionEndpointPort

### <a name="install-the-certificate-on-your-local-machine"></a>Sertifikayı yerel makinenize yükleme
  
Kümeye bağlanmak için sertifikayı geçerli kullanıcının Kişisel (My) deposuna yükleyin. 

Aşağıdakileri çalıştırın:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\<certificatename>.pfx `
        -Password $certpwd
```

Şimdi güvenli kümenize bağlanmaya hazırsınız.

### <a name="connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanma 

Güvenli bir kümeye bağlanmak için [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet'ini çalıştırın. Sertifika ayrıntıları, kümeyi oluşturmak için kullanılan bir sertifikayla eşleşmelidir. 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <ManagementEndpoint>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <CertificateThumbprint> `
          -FindType FindByThumbprint -FindValue <CertificateThumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

Aşağıdaki örnekte, örnek parametreler gösterilmektedir: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Bağlı olup olmadığınızı ve kümenin sağlıklı olup olmadığını denetlemek için aşağıdaki komutu çalıştırın.

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="remove-the-cluster"></a>Kümeyi kaldırma
Küme, küme kaynağının yanı sıra diğer Azure kaynaklarından oluşur. Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir. 

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```
## <a name="use-azure-cli"></a>Azure CLI kullanma
Kümeyi oluşturmanın bir diğer yolu CLI kullanmaktır. Bunu yapmak için:

1. Bilgisayarınıza [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest)'ı yükleyin.
2. Azure'da oturum açıp kümeyi oluşturmak istediğiniz aboneliği seçin.
   ```azurecli
   az login
   az account set --subscription <GUID>
   ```
3. Güvenliği X.509 sertifikasıyla sağlanan beş düğümlü bir Service Fabric kümesi oluşturmak için [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) komutunu çalıştırın. Bu komut otomatik olarak imzalanan bir sertifika oluşturur ve bunu yeni bir anahtar kasasına yükler. Sertifika aynı zamanda bir yerel dizine de kopyalanır. Küme düğümleri üzerinde çalışan Windows veya Linux sürümünü seçmek için *-os* parametresini ayarlayın. Parametreleri gereken şekilde özelleştirin.

    ```azurecli
    #!/bin/bash

    # Variables
    ResourceGroupName="aztestclustergroup" 
    ClusterName="aztestcluster" 
    Location="southcentralus" 
    Password="q6D7nN%6ck@6" 
    Subject="aztestcluster.southcentralus.cloudapp.azure.com" 
    VaultName="aztestkeyvault" 
    VaultGroupName="testvaultgroup"
    VmPassword="Mypa$$word!321"
    VmUserName="sfadminuser"

    # Create resource groups
    az group create --name $ResourceGroupName --location $Location 
    az group create --name $VaultGroupName --location $Location

    # Create secure five node Linux cluster. Creates a key vault in a resource group
    # and creates a certficate in the key vault. The certificate's subject name must match 
    # the domain that you use to access the Service Fabric cluster. The certificate is downloaded locally.
    az sf cluster create --resource-group $ResourceGroupName --location $Location --certificate-output-folder . \
        --certificate-password $Password --certificate-subject-name $Subject --cluster-name $ClusterName \
        --cluster-size 5 --os UbuntuServer1604 --vault-name $VaultName --vault-resource-group $VaultGroupName \
        --vm-password $VmPassword --vm-user-name $VmUserName
    ```
    
### <a name="connect-to-the-cluster"></a>Kümeye bağlanma
Sertifikayı kullanarak kümeye bağlanmak için aşağıdaki CLI komutunu kullanın.  Kimlik doğrulaması için bir istemci sertifikası kullanıyorsanız sertifika bilgilerinin küme düğümlerine dağıtılmış olan bir sertifikayla eşleştiğinden emin olun. Otomatik olarak imzalanan sertifika için `--no-verify` seçeneğini kullanın.

```azurecli
az sf cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 --pem ./linuxcluster201709161647.pem --no-verify
```

Bağlı olup olmadığınızı ve kümenin sağlıklı olup olmadığını denetlemek için aşağıdaki komutu çalıştırın.

```azurecli
az sf cluster health
```

### <a name="connect-to-the-nodes-directly"></a>Düğümlere doğrudan bağlanma 

Bir Linux kümesindeki düğümlere bağlanmak için güvenli kabuk (SSH) komutunu kullanabilirsiniz. 3389’dan sonraki bir bağlantı noktası numarasını belirtin. Örneğin, önceden oluşturulmuş olan beş düğümlü bir küme için komutlar şu şekilde olacaktır:
```bash
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3389
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3390
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3391
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3392
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3393
```

### <a name="remove-the-cluster"></a>Kümeyi kaldırma
Küme, küme kaynağının yanı sıra diğer Azure kaynaklarından oluşur. Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir. 

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar
Artık bir geliştirme kümesi ayarladığınıza göre aşağıdakileri deneyebilirsiniz:
* [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)
* [Küme kaldırma](service-fabric-cluster-delete.md) 
* [PowerShell kullanarak uygulama dağıtma](service-fabric-deploy-remove-applications.md)
* [CLI kullanarak uygulama dağıtma](service-fabric-application-lifecycle-sfctl.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
