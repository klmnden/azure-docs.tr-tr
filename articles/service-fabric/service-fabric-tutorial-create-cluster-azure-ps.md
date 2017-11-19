---
title: "Service Fabric kümesi oluşturma | Microsoft Docs"
description: "PowerShell kullanarak Azure'da bir Windows veya Linux Service Fabric kümesi oluşturmayı öğrenin"
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
ms.date: 10/03/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 217b9f2f0dfed5b095e1bac1c8146abf4753fadc
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-secure-cluster-in-azure-by-using-powershell"></a>PowerShell kullanarak Azure'da güvenli küme oluşturma
Bu makalede Azure Service Fabric kümeleri ve kapsayıcılar kullanarak bir .NET uygulaması buluta taşımak nasıl Göster eğitim serileri ilk getirilmiştir. Aşağıdaki adımlarda, Azure'da çalışan Service Fabric kümesi (Windows veya Linux) oluşturmayı öğrenin. İşlemi tamamladığınızda, uygulamaları dağıtabilir bulutta çalışan güvenli bir küme sahip.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Yükleme [Service Fabric SDK](service-fabric-get-started.md).
- Yükleme [Azure Powershell modülü sürümü 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). (Gerekirse, [Azure PowerShell'i yükleme](/powershell/azure/overview) veya [daha yeni bir sürüme güncelleştirmek](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0#update-azps).)


## <a name="create-a-service-fabric-cluster"></a>Service Fabric kümesi oluştur

Bu komut dosyası tek düğümlü oluşturur, Service Fabric Önizleme küme. Kendinden imzalı bir sertifika küme güvenliğini sağlar. Komut dosyası küme birlikte bir sertifika oluşturur ve ardından sertifika bir anahtar kasası yerleştirir. Tek düğümlü kümeler bir sanal makinenin ötesine ölçeklendirme olamaz ve önizleme kümeleri daha yeni sürümlerine yükseltme yapamazsınız.

Azure Service Fabric kümesi çalıştırarak sonucunda oluşan maliyet hesaplamak için [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/).
Service Fabric kümeleri oluşturma hakkında daha fazla bilgi için bkz: [Azure Kaynak Yöneticisi'ni kullanarak bir Service Fabric kümesi oluştur](service-fabric-cluster-creation-via-arm.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma
Bir PowerShell konsolu açın, Azure'da oturum açma ve kümede dağıtmak istediğiniz aboneliği seçin:

   ```PowerShell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```

## <a name="cluster-parameters"></a>Küme parametreleri

   Bu betiği aşağıdaki parametreleri ve kavramları kullanır. Parametreleri gereksinimlerinizi karşılayacak şekilde özelleştirin.

   | Parametre       | Açıklama | Önerilen Değer |
   | --------------- | ----------- | --------------- |
   | Konum | Küme dağıttığınız Azure bölgesi. | Örneğin, *westeurope*, *eastasia*, veya *eastus* |
   | Ad     | Oluşturmak istediğiniz küme adıdır. Ad 4-23 karakter olmalıdır ve yalnızca küçük harf, sayı ve kısa çizgi olabilir. | Örneğin, *bobs sfpreviewcluster* |
   | resourceGroupName   | Küme oluşturulacağı kaynak grubunun adı. | Örneğin, *myresourcegroup* |
   | VmSku  | Sanal makine SKU düğümleri için kullanın. | Geçerli bir sanal makine SKU |
   | İşletim Sistemi  | Sanal makine için düğümleri kullanmak için işletim sistemi. | Geçerli bir sanal makine işletim sistemi |
   | keyVaultName | Kümesi ile ilişkilendirmek üzere yeni anahtar kasasının adı. | Örneğin, *mykeyvault* |
   | ClusterSize | Sanal makine kümenizdeki sayısı (olabilir *1* veya *3-99*).| Önizleme küme için yalnızca bir sanal makine belirtin |
   | CertificateSubjectName | Oluşturulacak sertifika konu adı. | Biçimdedir:  *<name>* . *<location>* . cloudapp.azure.com |

### <a name="default-parameter-values"></a>Varsayılan parametre değerleri
**Sanal makine**: isteğe bağlı ayarlar. Bunları belirtmezseniz, yönetici kullanıcı adı için varsayılan olarak *vmadmin* ve PowerShell, bir sanal makine parola ister küme oluşturmadan önce.

**Bağlantı noktaları**: varsayılan bağlantı noktaları için 80 ve 8081. Yönergeler için aşağıdaki kullanarak ek bağlantı noktalarını belirtebilirsiniz [Service Fabric kümeleri bağlantı noktaları](https://docs.microsoft.com/en-us/azure/service-fabric/create-load-balancer-rule).

**Tanılama**: varsayılan olarak etkindir.

**DNS hizmeti**: varsayılan olarak etkin değildir.

**Ters proxy**: varsayılan olarak etkin değildir.

## <a name="create-the-cluster-with-your-parameters"></a>Parametrelerinizi ile küme oluşturma

Gereksinimlerinize uyan parametreleri karar verdikten sonra güvenli bir Service Fabric kümesi ve sertifikasını oluşturmak için aşağıdaki komutu çalıştırın.

Ek parametreler eklemek için bu komut dosyasını değiştirebilirsiniz. Küme oluşturma için parametreler hakkında daha fazla bilgi için bkz: [yeni AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/new-azurermservicefabriccluster.md) cmdlet'i.

>[!NOTE]
>Bu komutu çalıştırmadan önce küme sertifika burada depolayabileceğiniz bir klasör oluşturmalısınız.

```PowerShell

# Set the certificate variables. This creates and encrypts a password that Service Fabric will use.
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force

# You must create the folder where you want to store the certificate on your machine before you start this step.
$certfolder="c:\mycertificates\"

# Set the variables for common values. Change the values to fit your needs.
$clusterloc="WestUS"
$clustername = "mysfcluster"
$groupname="mysfclustergroup"       
$vmsku = "Standard_D2_v2"
$vaultname = "mykeyvault"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Set the number of cluster nodes. The possible values are 1 and 3-99.
$clustersize=1

# Create the Service Fabric cluster and its self-signed certificate. The OS you specify here lets you use this cluster with any applications that are also using containers.
New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
-ClusterSize $clustersize -CertificateSubjectName $subname `
-CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
-OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname
```

Oluşturma işlemi birkaç dakika sürebilir. Yapılandırma tamamlandıktan sonra Azure içinde oluşturulan küme bilgilerini çıkarır. Ayrıca bu parametre için belirtilen yol - CertificateOutputFolder dizinine küme sertifika kopyalar.

Not edin **ManagementEndpoint** gibi aşağıdaki URL'yi olabilir, kümeniz için URL: https://mycluster.westeurope.cloudapp.azure.com:19080.

## <a name="import-the-certificate"></a>Sertifika alma

Küme başarıyla oluşturulduğunda otomatik olarak imzalanan sertifikayı kullandığınızdan emin olmak için aşağıdaki komutu çalıştırın:

```PowerShell

# To connect to the cluster, install the certificate into the Personal (My) store of the current user on your computer.
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
-FilePath C:\mycertificates\mysfclustergroup20170531104310.pfx `
-Password $certpwd
```

Bu komut sertifikayı geçerli kullanıcının makinenizin yükler. Service Fabric Explorer erişmek ve kümenizi durumunu görüntülemek için bu sertifika gerekir.


## <a name="view-your-cluster-optional"></a>(İsteğe bağlı) kümenizi görüntüleyin

Küme ve içeri aktarılan sertifika aldıktan sonra kümeye bağlanın ve durumunu görüntüleyin. Service Fabric Explorer veya PowerShell ile bağlanmak için birden çok yolu vardır.

### <a name="service-fabric-explorer"></a>Service Fabric Explorer
Service Fabric Explorer ile kümenizi durumunu görüntüleyebilirsiniz. Bunu yapmak için Gözat **ManagementEndpoint** URL'sini küme ve makinenizde kaydedilen sertifikayı seçin.

>[!NOTE]
>Service Fabric Explorer açtığınızda, otomatik olarak imzalanan bir sertifika kullanıyorsanız gibi bir sertifika hatası görürsünüz. Kenar içinde tıklattığınızdan sahip **ayrıntıları**ve ardından **Web sayfasına gidin** bağlantı. Chrome'tıklatmak zorunda **Gelişmiş**ve ardından **devam** bağlantı.

### <a name="powershell"></a>PowerShell

Service Fabric PowerShell modülü, Service Fabric kümeleri, uygulamaları ve hizmetleri yönetmek için birçok cmdlet'leri sağlar. Kullanım [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet'ini güvenli kümeye bağlanın. Sertifika parmak izi ve bağlantı uç nokta ayrıntılarını önceki adımdaki çıktı bulunabilir.

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
-KeepAliveIntervalInSec 10 `
-X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-StoreLocation CurrentUser -StoreName My
```

Ayrıca, bağlı olduğunuzu ve kullanarak küme sağlıklı olup olmadığını denetleyebilirsiniz [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet'i.

```PowerShell
Get-ServiceFabricClusterHealth
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, PowerShell kullanarak güvenli bir Service Fabric küme oluşturmayı öğrendiniz.

Ardından, var olan bir uygulama dağıtma hakkında bilgi edinmek için aşağıdaki öğreticide ilerlemek:
> [!div class="nextstepaction"]
> [Varolan bir .NET uygulama Docker Compose ile dağıtma](service-fabric-host-app-in-a-container.md)

 
