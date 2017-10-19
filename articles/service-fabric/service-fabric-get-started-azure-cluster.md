---
title: "Azure Service Fabric kümesi ayarlama | Microsoft Docs"
description: "Hızlı başlangıç- Azure’da bir Windows veya Linux Service Fabric kümesi oluşturun."
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
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: de7fa7e6445e6eaf08bdcc8ae812611f20a98c34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Azure’da ilk Service Fabric kümenizi oluşturma
[Service Fabric kümesi](service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Bu hızlı başlangıç, [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) veya [Azure portalı](http://portal.azure.com) üzerinden yalnızca birkaç dakika içinde Windows veya Linux üzerinde çalışan beş düğümlü bir küme oluşturmanıza yardımcı olur.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="use-the-azure-portal"></a>Azure portalı kullanma

[http://portal.azure.com](http://portal.azure.com) sayfasından Azure portalda oturum açın.

### <a name="create-the-cluster"></a>Kümeyi oluşturma

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.
2. **Yeni** dikey penceresinden **İşlem**’i seçin ve ardından **İşlem** dikey penceresinden **Service Fabric Kümesi**’ni belirleyin.
3. Service Fabric **Temel Bilgileri** formunu doldurun. **İşletim sistemi** için küme düğümlerinin çalışmasını istediğiniz Windows veya Linux sürümünü seçin. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. **Kaynak grubu** için yeni bir tane oluşturun. Kaynak grubu, Azure kaynaklarının oluşturulup toplu olarak yönetildiği bir mantıksal kapsayıcıdır. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Küme kurulumu çıktısı][cluster-setup-basics]

4. **Küme yapılandırması** formunu doldurun.  **Düğüm türü sayısı** için "1" değerini girin.

5. **Düğüm türü 1 (Birincil)**’i seçip **Düğüm türü yapılandırma** formunu doldurun.  Düğüm türü adını girin ve [Dayanıklılık katmanı](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) için "Bronz" seçeneğini belirleyin.  VM boyutunu seçin.

    Düğüm türleri VM boyutunu, VM sayısını, özel uç noktaları ve aynı türdeki VM’lerin diğer ayarlarını tanımlar. Tanımlanan her düğüm türü, sanal makineleri küme olarak dağıtıp yönetmek için kullanılan ayrı bir sanal makine ölçek kümesi olarak ayarlanır. Her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir.  Birinci veya birincil düğüm türü, Service Fabric sistem hizmetlerinin barındırıldığı yerdir ve beş ya da daha fazla VM içermelidir.

    Herhangi bir üretim dağıtımı için [kapsite planlaması](service-fabric-cluster-capacity.md) önemli bir adımdır.  Ancak, bu hızlı başlangıçta uygulama çalıştırmadığınız için bir *DS1_v2 Standart* VM boyutu seçin.  [Güvenilirlik katmanı](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) için "Gümüş" seçeneğini belirleyin ve sanal makine ölçek kümesinin başlangıç kapasitesini 5 olarak ayarlayın.  

    Özel uç noktalar Azure Load Balancer’da bağlantı noktaları açtığı için, küme üzerinde çalışan uygulamalarla bağlantı kurabilirsiniz.  80 ve 8172 bağlantı noktalarını açmak için "80, 8172" girin.

    TCP/HTTP yönetim uç noktalarını, uygulama bağlantı noktası aralıklarını, [yerleştirme kısıtlamalarını](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) ve [kapasite özelliklerini](service-fabric-cluster-resource-manager-metrics.md) özelleştirmek için kullanılan **Gelişmiş ayarları yapılandır** kutusunu işaretlemeyin.    

    **Tamam**’ı seçin.

6. **Küme yapılandırması** formunda **Tanılama** ayarını **Açık** olarak belirleyin.  Bu hızlı başlangıçta herhangi bir [yapı ayarı](service-fabric-cluster-fabric-settings.md) özelliği girmeniz gerekmez.  Microsoft’un kümeyi çalıştıran yapı kodu sürümünü otomatik olarak güncelleştirmesi için **Fabric sürümü** menüsünde **Otomatik** yükseltme modunu seçin.  Yükseltmenin yapılacağı [desteklenen bir sürüm seçmek](service-fabric-cluster-upgrade.md) istiyorsanız modu **El ile** olarak ayarlayın. 

    ![Düğüm türü yapılandırması][node-type-config]

    **Tamam**’ı seçin.

7. **Güvenlik** formunu doldurun.  Bu hızlı başlangıç için **Güvenli olmayan**’ı seçin.  Ancak, güvenli olmayan bir kümeye herhangi bir kişi anonim olarak bağlanıp yönetim işlemlerini gerçekleştirebileceğinden, üretim iş yükleri için güvenli bir küme oluşturmanız önemle tavsiye edilir.  

    Sertifikalar, Service Fabric’te bir küme ile uygulamalarının çeşitli yönlerini güvenli hale getirmek üzere kimlik doğrulaması ve şifreleme sağlamak için kullanılır. Sertifikaların Service Fabric’te kullanılmasıyla ilgili daha fazla bilgi için bkz. [Service Fabric kümesi güvenlik senaryoları](service-fabric-cluster-security.md).  Azure Active Directory kullanarak kullanıcı kimlik doğrulamasını etkinleştirmek veya uygulama güvenliği için sertifika ayarlamak üzere [bir Resource Manager şablonundan küme oluşturun](service-fabric-cluster-creation-via-arm.md).

    **Tamam**’ı seçin.

8. Özeti gözden geçirin.  Girdiğiniz ayarlarla oluşturulmuş bir Resource Manager şablonu indirmek isterseniz, **Şablon ve parametreleri indir**’i seçin.  **Oluştur**’u seçerek kümeyi oluşturun.

    Oluşturma işleminin ilerleme durumunu bildirimler bölümünden görebilirsiniz. (Ekranınızın sağ üst köşesindeki durum çubuğunun yanında bulunan "Zil" simgesine tıklayın.) Kümeyi oluştururken **Başlangıç Panosuna Sabitle**’ye tıkladıysanız, **Service Fabric Kümesi Dağıtılıyor** öğesinin **Başlangıç** panosuna sabitlendiğini görürsünüz.

### <a name="connect-to-the-cluster-using-powershell"></a>PowerShell kullanarak kümeye bağlanma
PowerShell ile bağlanarak kümenin çalıştığını doğrulayın.  ServiceFabric PowerShell modülü, [Service Fabric SDK’sı](service-fabric-get-started.md) ile birlikte yüklenir.  [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet’i, kümeyle bir bağlantı kurar.   

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
Service Fabric kümesi, küme kaynağının yanı sıra diğer Azure kaynaklarından oluşur. Bu nedenle, bir Service Fabric kümesini tamamen silmek için onu oluşturan tüm kaynakları da silmeniz gerekir. Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir. Bir kümeyi veya bir kaynak grubundaki bazı (tümü değil) kaynakları silmenin diğer yolları için bkz. [Küme silme](service-fabric-cluster-delete.md)

Azure portalında bir kaynak grubunu silin:
1. Silmek istediğiniz Service Fabric kümesine gidin.
2. Küme temel bileşenleri sayfasında **Kaynak Grubu** adına tıklayın.
3. **Kaynak Grubu Temel Bileşenleri** sayfasında **Kaynak grubunu sil**’e tıklayın ve bu sayfadaki yönergeleri izleyerek kaynak grubu silme işlemini tamamlayın.
    ![Kaynak grubunu silme][cluster-delete]


## <a name="use-azure-powershell-to-deploy-a-secure-windows-cluster"></a>Azure PowerShell'i kullanarak güvenli bir Windows kümesi dağıtma
1. [Azure Powershell modülü sürüm 4.0 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) makinenize indirin.

2. Bir PowerShell penceresi açıp aşağıdaki komutu çalıştırın. 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    Aşağıdakine benzer bir çıktı görmeniz gerekir.

    ![ps-list][ps-list]

3. Azure’da oturum açıp kümeyi oluşturmak istediğiniz aboneliği seçin

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. Güvenli bir küme oluşturmak için aşağıdaki komutu çalıştırın. Parametreleri özelleştirmeyi unutmayın. 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also the name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    Komutun tamamlanması 10 dakika ile 30 dakika arasında sürer ve bu süre sonunda aşağıdakine benzer bir çıktı alırsınız. Çıktıda, sertifikayla ilgili bilgiler, sertifikanın yüklendiği anahtar kasası ve sertifikanın kopyalandığı yerel klasör bulunur. 

    ![ps-out][ps-out]

5. Tüm çıktıyı kopyalayın ve daha sonra başvurulması gerekeceğinden bir metin dosyasına kaydedin. Çıktıda aşağıdaki bilgileri not edin. 

    - **CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx
    - **CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10
    - **ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080
    - **ClientConnectionEndpointPort** : 19000

### <a name="install-the-certificate-on-your-local-machine"></a>Sertifikayı yerel makinenize yükleme
  
Kümeye bağlanmak için sertifikayı geçerli kullanıcının Kişisel (My) deposuna yüklemeniz gerekir. 

Aşağıdaki PowerShell komutunu çalıştırın

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\the name of the cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

Şimdi güvenli kümenize bağlanmaya hazırsınız.

### <a name="connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanma 

Güvenli bir kümeye bağlanmak için aşağıdaki PowerShell komutunu çalıştırın. Sertifika ayrıntıları, kümeyi oluşturmak için kullanılan bir sertifikayla eşleşmelidir. 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


Aşağıdaki örnekte, tamamlanmış parametreler gösterilmektedir: 

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

Remove-AzureRmResourceGroup -Name $RGname -Force

```
## <a name="use-azure-cli-to-deploy-a-secure-linux-cluster"></a>Azure CLI'yı kullanarak güvenli bir Linux kümesi dağıtma

1. Bilgisayarınıza [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest)'ı yükleyin.
2. Azure'da oturum açıp kümeyi oluşturmak istediğiniz aboneliği seçin.
   ```azurecli
   az login
   az account set --subscription <GUID>
   ```
3. Güvenli bir küme oluşturmak için [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) komutunu çalıştırın.

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
    # the domain that you use to access the Service Fabric cluster.  The certificate is downloaded locally.
    az sf cluster create --resource-group $ResourceGroupName --location $Location --certificate-output-folder . \
        --certificate-password $Password --certificate-subject-name $Subject --cluster-name $ClusterName \
        --cluster-size 5 --os UbuntuServer1604 --vault-name $VaultName --vault-resource-group $VaultGroupName \
        --vm-password $VmPassword --vm-user-name $VmUserName
    ```
    
### <a name="connect-to-the-cluster"></a>Kümeye bağlanma
Sertifikayı kullanarak kümeye bağlanmak için aşağıdaki CLI komutunu kullanın.  Kimlik doğrulaması için bir istemci sertifikası kullanıyorsanız sertifika bilgilerinin küme düğümlerine dağıtılmış olan bir sertifikayla eşleşmesi gerekir.  Otomatik olarak imzalanan sertifika için `--no-verify` seçeneğini kullanın.

```azurecli
az sf cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 --pem ./linuxcluster201709161647.pem --no-verify
```

Bağlı olup olmadığınızı ve kümenin sağlıklı olup olmadığını denetlemek için aşağıdaki komutu çalıştırın.

```azurecli
az sf cluster health
```

### <a name="connect-to-the-nodes-directly"></a>Düğümlere doğrudan bağlanma 

Bir Linux kümesindeki düğümlere bağlanmak için SSH kullanarak 3389 üzeri bir bağlantı noktası numarasıyla düğümlere bağlanabilirsiniz. Örneğin, önceden oluşturulmuş olan beş düğümlü bir küme için komutlar şu şekilde olacaktır:
```bash
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3389
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3390
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3391
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3392
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3393
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
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
