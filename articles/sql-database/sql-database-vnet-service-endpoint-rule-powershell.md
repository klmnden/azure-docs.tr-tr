---
title: "Sanal ağ hizmet uç noktalarına ve SQL kurallarında için PowerShell | Microsoft Docs"
description: "Oluşturma ve Azure SQL veritabanı için sanal hizmet uç noktaları yönetmek için PowerShell komut dosyaları sağlar."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: VNet Service endpoints
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/15/2017
ms.author: genemi
ms.openlocfilehash: 9f58ea34dad5d4436c13b64653040bd2a57c299e
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="use-powershell-to-create-a-virtual-service-endpoint-and-rule-for-azure-sql-database"></a>Azure SQL veritabanı için bir sanal Hizmeti uç noktası ve kuralı oluşturmak için PowerShell kullanma

Bu makalede sağlar ve aşağıdaki işlemleri yapar bir PowerShell komut dosyası açıklanmaktadır:

1. Bir Microsoft Azure oluşturur *sanal Hizmeti uç noktası* alt ağınız üzerinde.
2. Uç nokta oluşturmak için Azure SQL veritabanı sunucunuzun güvenlik duvarı ekler bir *sanal ağ kuralı*.

Bir kural oluşturmak için sözleri açıklanmıştır: [sanal hizmet uç noktaları Azure SQL veritabanı için][sql-db-vnet-service-endpoint-rule-overview-735r].

> [!TIP]
> Tüm yapmanız gereken ise değerlendirmek veya sanal hizmet uç noktası eklemek için *türü adı* alt ağınız için SQL veritabanı için için daha ileri atlayabilirsiniz [PowerShell komut dosyasını doğrudan](#a-verify-subnet-is-endpoint-ps-100).

#### <a name="major-cmdlets"></a>Ana cmdlet'leri

Bu makalede adlı cmdlet vurgular **yeni AzureRmSqlServerVirtualNetworkRule**, böylece bir kural oluşturma Azure SQL veritabanı sunucunuzun erişim denetim listesine (ACL) alt ağ uç noktası ekler.

Aşağıdaki liste, diğer gösterilir *ana* aramanız için hazırlamak için çalıştırmalısınız cmdlet'leri **yeni AzureRmSqlServerVirtualNetworkRule**. Bu makalede, bu çağrıları ortaya [betik 3 "sanal ağ kuralı"](#a-script-30):

1. [AzureRmVirtualNetworkSubnetConfig yeni](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig): bir alt ağ nesnesi oluşturur.

2. [Yeni-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork): sanal ağınızı alt vermiş oluşturur.

3. [Set-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/Set-AzureRmVirtualNetworkSubnetConfig): sanal hizmet uç noktası, alt ağına atar.

4. [Set-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork): sanal ağınıza yapılan güncelleştirmeler devam ettirir.

5. **AzureRmSqlServerVirtualNetworkRule yeni**: bir uç nokta, alt ağ olduktan sonra alt ağınızı bir sanal ağ kuralı Azure SQL veritabanı sunucunuzun ACL ekler.

#### <a name="prerequisites-for-running-powershell"></a>PowerShell çalıştırma önkoşulları

- Zaten Azure'a gibi aracılığıyla oturum açabildiğinizden [Azure portal][http-azure-portal-link-ref-477t].
- PowerShell komut dosyaları zaten çalıştırabilirsiniz.

#### <a name="one-script-divided-into-four-chunks"></a>Tek bir betik dört parçalara bölünür

Bizim tanıtım PowerShell Betiği küçük komut dizisi ayrılmıştır. Bölme öğrenme kolaylaştırır ve esneklik sağlar. Komut dosyaları, belirtilen sırayla çalıştırılmalıdır. Komut dosyalarını çalıştırmak için şu anki saat yoksa bizim gerçek test çıkışı 4 betik sonra görüntülenir.






<a name="a-script-10" />

## <a name="script-1-variables"></a>Komut dosyası, 1: değişkenleri

Bu ilk PowerShell komut dosyası değişkenleri için değerleri atar. Sonraki komut dosyaları bu değişkenleri üzerinde bağlıdır.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırmadan önce değerleri isterseniz düzenleyebilirsiniz. Örneğin, bir kaynak grubu zaten varsa, kaynak grubu adı atanan değer olarak Düzenle isteyebilirsiniz.
>
>  Abonelik adı betiğe düzenlenmesi gerekir.

#### <a name="powershell-script-1-source-code"></a>PowerShell komut dosyası 1 kaynak kodu

```powershell
######### Script 1 ########################################
##   LOG into to your Azure account.                     ##
##   (Needed only one time per powershell.exe session.)  ##
###########################################################

$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Login-AzureRmAccount; }

###########################################################
##  Assignments to variables used by the later scripts.  ##
###########################################################

# You can edit these values, if necessary.

$SubscriptionName = 'yourSubscriptionName';
Select-AzureRmSubscription -SubscriptionName $SubscriptionName;

$ResourceGroupName = 'RG-YourNameHere';
$Region            = 'westcentralus';

$VNetName            = 'myVNet';
$SubnetName          = 'mySubnet';
$VNetAddressPrefix   = '10.1.0.0/16';
$SubnetAddressPrefix = '10.1.1.0/24';
$VNetRuleName        = 'myFirstVNetRule-ForAcl';

$SqlDbServerName         = 'mysqldbserver-forvnet';
$SqlDbAdminLoginName     = 'ServerAdmin';
$SqlDbAdminLoginPassword = 'ChangeYourAdminPassword1';

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql';  # Official type name.

Write-Host 'Completed script 1, the "Variables".';
```





<a name="a-script-20" />

## <a name="script-2-prerequisites"></a>Komut dosyası 2: Önkoşullar

Bu komut, uç nokta eylem olduğu sonraki komut dosyası için hazırlar. Bu komut dosyasını aşağıdaki oluşturur zaten mevcut ancak yalnızca öğeler listelenir. Bu öğe zaten var olduğundan emin olup olmadığını 2 betiği atlayabilirsiniz:

- Azure kaynak grubu
- Azure SQL veritabanı sunucusu

#### <a name="powershell-script-2-source-code"></a>PowerShell komut dosyası 2 kaynak kodu

```powershell
######### Script 2 ########################################
##   Ensure your Resource Group already exists.          ##
###########################################################

Write-Host "Check whether your Resource Group already exists.";

$gottenResourceGroup = $null;

$gottenResourceGroup = Get-AzureRmResourceGroup `
  -Name        $ResourceGroupName `
  -ErrorAction SilentlyContinue;

if ($null -eq $gottenResourceGroup)
{
    Write-Host "Creating your missing Resource Group - $ResourceGroupName.";

    $gottenResourceGroup = New-AzureRmResourceGroup `
      -Name $ResourceGroupName `
      -Location $Region;

    $gottenResourceGroup;
}
else { Write-Host "Good, your Resource Group already exists - $ResourceGroupName."; }

$gottenResourceGroup = $null;

###########################################################
## Ensure your Azure SQL Database server already exists. ##
###########################################################

Write-Host "Check whether your Azure SQL Database server already exists.";

$sqlDbServer = $null;

$sqlDbServer = Get-AzureRmSqlServer `
  -ResourceGroupName $ResourceGroupName `
  -ServerName        $SqlDbServerName `
  -ErrorAction       SilentlyContinue;

if ($null -eq $sqlDbServer)
{
    Write-Host "Creating the missing Azure SQL Database server - $SqlDbServerName.";

    Write-Host "Gather the credentials necessary to next create an Azure SQL Database server.";

    $sqlAdministratorCredentials = New-Object `
      -TypeName     System.Management.Automation.PSCredential `
      -ArgumentList `
        $SqlDbAdminLoginName, `
        $(ConvertTo-SecureString `
            -String      $SqlDbAdminLoginPassword `
            -AsPlainText `
            -Force `
         );

    if ($null -eq $sqlAdministratorCredentials)
    {
        Write-Host "ERROR, unable to create SQL administrator credentials.  Now ending.";
        return;
    }

    Write-Host "Create your Azure SQL Database server.";

    $sqlDbServer = New-AzureRmSqlServer `
      -ResourceGroupName $ResourceGroupName `
      -ServerName        $SqlDbServerName `
      -Location          $Region `
      -SqlAdministratorCredentials $sqlAdministratorCredentials;

    $sqlDbServer;
}
else { Write-Host "Good, your Azure SQL Database server already exists - $SqlDbServerName."; }

$sqlAdministratorCredentials = $null;
$sqlDbServer                 = $null;

Write-Host 'Completed script 2, the "Prerequisites".';
```






<a name="a-script-30" />

## <a name="script-3-create-an-endpoint-and-a-rule"></a>Betik 3: bir uç nokta ve bir kural oluşturun

Bu komut dosyasını bir alt ağ ile bir sanal ağ oluşturur. Komut dosyası atar sonra **Microsoft.Sql** alt ağınız için uç nokta türü. Son olarak betik böylece bir kural oluşturmak, SQL veritabanı sunucusu, erişim denetim listesine (ACL) alt ağınızı ekler.

#### <a name="powershell-script-3-source-code"></a>PowerShell komut dosyası 3 kaynak kodu

```powershell
######### Script 3 ########################################
##   Create your virtual network, and give it a subnet.  ##
###########################################################

Write-Host "Define a subnet '$SubnetName', to be given soon to a virtual network.";

$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Create a virtual network '$VNetName'." `
  "  Give the subnet to the virtual network that we created.";

$vnet = New-AzureRmVirtualNetwork `
  -Name              $VNetName `
  -AddressPrefix     $VNetAddressPrefix `
  -Subnet            $subnet `
  -ResourceGroupName $ResourceGroupName `
  -Location          $Region;

###########################################################
##   Create a Virtual Service endpoint on the subnet.    ##
###########################################################

Write-Host "Assign a Virtual Service endpoint 'Microsoft.Sql' to the subnet.";

$vnet = Set-AzureRmVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Persist the updates made to the virtual network > subnet.";

$vnet = Set-AzureRmVirtualNetwork `
  -VirtualNetwork $vnet;

$vnet.Subnets[0].ServiceEndpoints;  # Display the first endpoint.

###########################################################
##   Add the Virtual Service endpoint Id as a rule,      ##
##   into SQL Database ACLs.                             ##
###########################################################

Write-Host "Get the subnet object.";

$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

$subnet = Get-AzureRmVirtualNetworkSubnetConfig `
  -Name           $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Add the subnet .Id as a rule, into the ACLs for your Azure SQL Database server.";

$vnetRuleObject1 = New-AzureRmSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -VirtualNetworkSubnetId $subnet.Id;

$vnetRuleObject1;

Write-Host "Verify that the rule is in the SQL DB ACL.";

$vnetRuleObject2 = Get-AzureRmSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName;

$vnetRuleObject2;

Write-Host 'Completed script 3, the "Virtual-Netowrk-Rule".';
```






<a name="a-script-40" />

## <a name="script-4-clean-up"></a>Betik 4: Temizleyin

Bu son komut dosyası, önceki betikler tanıtım için oluşturulan kaynakları siler. Ancak, aşağıdaki silmeden önce betik onaylamanızı ister:

- Azure SQL veritabanı sunucusu
- Azure kaynak grubu

Komut dosyası 4 1 komut dosyası tamamlandıktan sonra istediğiniz zaman çalıştırabilirsiniz.

#### <a name="powershell-script-4-source-code"></a>PowerShell komut dosyası 4 kaynak kodu

```powershell
######### Script 4 ########################################
##   Clean-up phase A:  Unconditional deletes.           ##
##                                                       ##
##   1. The test rule is deleted from SQL DB ACL.        ##
##   2. The test endpoint is deleted from the subnet.    ##
##   3. The test virtual network is deleted.             ##
###########################################################

Write-Host "Delete the rule from the SQL DB ACL.";

Remove-AzureRmSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -ErrorAction            SilentlyContinue;

Write-Host "Delete the endpoint from the subnet.";

$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

Remove-AzureRmVirtualNetworkSubnetConfig `
  -Name $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Delete the virtual network (thus also deletes the subnet).";

Remove-AzureRmVirtualNetwork `
  -Name              $VNetName `
  -ResourceGroupName $ResourceGroupName `
  -ErrorAction       SilentlyContinue;

###########################################################
##   Clean-up phase B:  Conditional deletes.             ##
##                                                       ##
##   These might have already existed, so user might     ##
##   want to keep.                                       ##
##                                                       ##
##   1. Azure SQL Database server                        ##
##   2. Azure resource group                             ##
###########################################################

$yesno = Read-Host 'CAUTION !: Do you want to DELETE your Azure SQL Database server AND your Resource Group?  [yes/no]';
if ('yes' -eq $yesno)
{
    Write-Host "Remove the Azure SQL DB server.";
    
    Remove-AzureRmSqlServer `
      -ServerName        $SqlDbServerName `
      -ResourceGroupName $ResourceGroupName `
      -ErrorAction       SilentlyContinue;
    
    Write-Host "Remove the Azure Resource Group.";
    
    Remove-AzureRmResourceGroup `
      -Name        $ResourceGroupName `
      -ErrorAction SilentlyContinue;
}
else
{
    Write-Host "Skipped over the DELETE of SQL Database and resource group.";
}

Write-Host 'Completed script 4, the "Clean-Up".';
```






<a name="a-actual-output" />

## <a name="actual-output-from-scripts-1-through-4"></a>Komut dosyaları 1-4 arası gerçek çıktısı

Bizim test çalışması çıktısını ardından, bir kısaltma biçiminde görüntülenir. Gerçekte PowerShell betikleri şimdi çalıştırmak istiyor musunuz durumda çıktı faydalı olabilir.

```
[C:\WINDOWS\system32\]
0 >> C:\Demo\PowerShell\sql-database-vnet-service-endpoint-powershell-s1-variables.ps1
Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]: yes


Environment           : AzureCloud
Account               : xx@microsoft.com
TenantId              : 11111111-1111-1111-1111-111111111111
SubscriptionId        : 22222222-2222-2222-2222-222222222222
SubscriptionName      : MySubscriptionName
CurrentStorageAccount : 



[C:\WINDOWS\system32\]
0 >> C:\Demo\PowerShell\sql-database-vnet-service-endpoint-powershell-s2-prerequisites.ps1
Check whether your Resource Group already exists.
Creating your missing Resource Group - RG-YourNameHere.


ResourceGroupName : RG-YourNameHere
Location          : westcentralus
ProvisioningState : Succeeded
Tags              : 
ResourceId        : /subscriptions/22222222-2222-2222-2222-222222222222/resourceGroups/RG-YourNameHere

Check whether your Azure SQL Database server already exists.
Creating the missing Azure SQL Database server - mysqldbserver-forvnet.
Gather the credentials necessary to next create an Azure SQL Database server.
Create your Azure SQL Database server.

ResourceGroupName        : RG-YourNameHere
ServerName               : mysqldbserver-forvnet
Location                 : westcentralus
SqlAdministratorLogin    : ServerAdmin
SqlAdministratorPassword : 
ServerVersion            : 12.0
Tags                     : 
Identity                 : 

Completed script 2, the "Prerequisites".



[C:\WINDOWS\system32\]
0 >> C:\Demo\PowerShell\sql-database-vnet-service-endpoint-powershell-s3-vnet-rule.ps1
Define a subnet 'mySubnet', to be given soon to a virtual network.
Create a virtual network 'myVNet'.   Give the subnet to the virtual network that we created.
WARNING: The output object type of this cmdlet will be modified in a future release.
Assign a Virtual Service endpoint 'Microsoft.Sql' to the subnet.
Persist the updates made to the virtual network > subnet.

Get the subnet object.
Add the subnet .Id as a rule, into the ACLs for your Azure SQL Database server.
ProvisioningState Service       Locations      
----------------- -------       ---------      
Succeeded         Microsoft.Sql {westcentralus}
                                               
Verify that the rule is in the SQL DB ACL.
                                               
Completed script 3, the "Virtual-Network-Rule".



[C:\WINDOWS\system32\]
0 >> C:\Demo\PowerShell\sql-database-vnet-service-endpoint-powershell-s4-clean-up.ps1
Delete the rule from the SQL DB ACL.

Delete the endpoint from the subnet.


Delete the virtual network (thus also deletes the subnet).
CAUTION !: Do you want to DELETE your Azure SQL Database server AND your Resource Group?  [yes/no]: yes
Remove the Azure SQL DB server.

ResourceGroupName        : RG-YourNameHere
ServerName               : mysqldbserver-forvnet
Location                 : westcentralus
SqlAdministratorLogin    : ServerAdmin
SqlAdministratorPassword : 
ServerVersion            : 12.0
Tags                     : 
Identity                 : 

Remove the Azure Resource Group.
True
Completed script 4, the "Clean-Up".
```

Bizim ana PowerShell komut dosyasının sonuna budur.





<a name="a-verify-subnet-is-endpoint-ps-100" />

## <a name="verify-your-subnet-is-an-endpoint"></a>Bir uç nokta, alt ağı olduğunu doğrulayın

Zaten atanmış bir alt ağa sahip **Microsoft.Sql** türü adı olduğu zaten sanal hizmet uç noktası anlamına gelir. Kullanabileceğinizi [Azure portal] [ http-azure-portal-link-ref-477t] uç noktasından bir sanal ağ kuralı oluşturulamadı.

Ya da alt ağınızı olup emin değilseniz olabilir **Microsoft.Sql** türü adı. Bu eylemleri uygulamak için aşağıdaki PowerShell betiğini çalıştırabilirsiniz:

1. Alt ağınız sahip olup olmadığını onaylaması **Microsoft.Sql** türü adı.
2. İsteğe bağlı olarak, mevcut ise tür adı atayın.
    - Komut dosyası, ister *onaylayın*etmeksizin türü geçerlidir önce adı.

#### <a name="phases-of-the-script"></a>Komut dosyası aşamaları

PowerShell Betiği aşamaları şunlardır:

1. PS oturumu yalnızca bir kez gereken Azure hesabınızda oturum açın.  Değişkenleri atayın.
2. Sanal ağınız için ve alt ağınız için arama yapın.
3. Alt ağınız olarak etiketlenir **Microsoft.Sql** uç sunucu türü?
4. Sanal hizmet uç noktası türü adının eklemek **Microsoft.Sql**, alt ağınız üzerinde.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırmadan önce komut dosyasının üstüne yakın $-değişkenlerini atanan değerler düzenlemeniz gerekir.

#### <a name="direct-powershell-source-code"></a>Doğrudan PowerShell kaynak kodu

Komut dosyasını güncelleştirmez herhangi bir şey varsa Evet yanıt sürece bu PowerShell olduğundan, onay ister. Komut dosyası türü adı ekleyebilirsiniz **Microsoft.Sql** alt ağınız için. Ancak, alt ağ türü adı eksikse komut dosyası Ekle çalışır.

```powershell
### 1. LOG into to your Azure account, needed only once per PS session.  Assign variables.

$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Login-AzureRmAccount; }

# Assignments to variables used by the later scripts.
# You can EDIT these values, if necessary.

$SubscriptionName  = 'yourSubscriptionName';
Select-AzureRmSubscription -SubscriptionName "$SubscriptionName";

$ResourceGroupName   = 'yourRGName';
$VNetName            = 'yourVNetName';
$SubnetName          = 'yourSubnetName';
$SubnetAddressPrefix = 'Obtain this value from the Azure portal.'; # Looks roughly like: '10.0.0.0/24'

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql';  # Do NOT edit. Is official value.

### 2. Search for your virtual network, and then for your subnet.

# Search for the virtual network.
$vnet = $null;
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

if ($vnet -eq $null)
{
    Write-Host "Caution: No virtual network found by the name '$VNetName'.";
    Return;
}

$subnet = $null;
for ($nn=0; $nn -lt $vnet.Subnets.Count; $nn++)
{
    $subnet = $vnet.Subnets[$nn];
    if ($subnet.Name -eq $SubnetName)
    { break; }
    $subnet = $null;
}

if ($subnet -eq $null)
{
    Write-Host "Caution: No subnet found by the name '$SubnetName'";
    Return;
}

### 3. Is your subnet tagged as 'Microsoft.Sql' endpoint server type?

$endpointMsSql = $null;
for ($nn=0; $nn -lt $subnet.ServiceEndpoints.Count; $nn++)
{
    $endpointMsSql = $subnet.ServiceEndpoints[$nn];
    if ($endpointMsSql.Service -eq $ServiceEndpointTypeName_SqlDb)
    {
        $endpointMsSql;
        break;
    }
    $endpointMsSql = $null;
}

if ($endpointMsSql -ne $null)
{
    Write-Host "Good: Subnet found, and is already tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'.";
    Return;
}
else
{
    Write-Host "Caution: Subnet found, but not yet tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'.";

    # Ask the user for confirmation.
    $yesno = Read-Host 'Do you want the PS script to apply the endpoint type name to your subnet?  [yes/no]';
    if ('no' -eq $yesno) { Return; }
}

### 4. Add a Virtual Service endpoint of type name 'Microsoft.Sql', on your subnet.

$vnet = Set-AzureRmVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

# Persist the subnet update.
$vnet = Set-AzureRmVirtualNetwork `
  -VirtualNetwork $vnet;

for ($nn=0; $nn -lt $vnet.Subnets.Count; $nn++)
{ $vnet.Subnets[0].ServiceEndpoints; }  # Display.
```

#### <a name="actual-output"></a>Gerçek çıkış

Aşağıdaki bloğu bizim gerçek geribildirim (düzenlemelerle yüzeysel) görüntüler.

```powershell
<# Our output example (with cosmetic edits), when the subnet was already tagged:

Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]: no


Environment           : AzureCloud
Account               : xx@microsoft.com
TenantId              : 11111111-1111-1111-1111-111111111111
SubscriptionId        : 22222222-2222-2222-2222-222222222222
SubscriptionName      : MySubscriptionName
CurrentStorageAccount : 


ProvisioningState : Succeeded
Service           : Microsoft.Sql
Locations         : {westcentralus}

Good: Subnet found, and is already tagged as an endpoint of type 'Microsoft.Sql'.
#>
```




<!-- Link references: -->

[sql-db-vnet-service-endpoint-rule-overview-735r]: sql-database-vnet-service-endpoint-rule-overview.md

[http-azure-portal-link-ref-477t]: https://portal.azure.com/

