---
title: Sanal ağ uç noktaları ve tek ve havuza alınmış Azure SQL veritabanlarına yönelik kuralları için PowerShell | Microsoft Docs
description: Oluşturma ve Azure SQL veritabanı ve SQL veri ambarı için sanal hizmet uç noktaları yönetmek için PowerShell komut dosyaları sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: PowerShell
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: genemi, vanto
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 6713182003a280c1d53e904209159b55b4ad01c6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60331154"
---
# <a name="powershell--create-a-virtual-service-endpoint-and-vnet-rule-for-sql"></a>PowerShell:  SQL sanal hizmet uç noktası ve sanal ağ kuralı oluşturma

*Sanal ağ kuralları* olduğunu denetleyen bir güvenlik duvarı olup olmadığını tek veritabanlarının ve azure'daki esnek havuz için veritabanı sunucusu [SQL veritabanı](sql-database-technical-overview.md) veya veritabanlarınızı [SQL veri Ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) belirli alt ağları sanal ağlardaki gönderildiği iletişimi kabul eder.

> [!IMPORTANT]
> Bu makale, Azure SQL server ve Azure SQL sunucusu üzerinde oluşturulmuş olan hem SQL veritabanı ve SQL veri ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır. Bu makale *değil* geçerli bir **yönetilen örnek** Azure SQL veritabanı'nda dağıtım çünkü kendisiyle ilişkilendirilmiş bir hizmet uç noktası yok.

Bu makale sağlar ve aşağıdaki işlemleri yapar bir PowerShell Betiği açıklanmaktadır:

1. Microsoft Azure oluşturur *sanal hizmet uç noktası* alt ağınız üzerinde.
2. Uç nokta oluşturmak için Azure SQL veritabanı sunucunuzun güvenlik duvarını ekler bir *sanal ağ kuralı*.

Bir kural oluşturmak için motivasyonlardan açıklanmıştır: [Azure SQL veritabanı için hizmet uç noktaları sanal][sql-db-vnet-service-endpoint-rule-overview-735r].

> [!TIP]
> Değerlendirme veya sanal hizmet uç noktası eklemek için tek ihtiyacınız olan, *tür adı* alt ağınız için SQL veritabanı için daha önceden atlayabilirsiniz [PowerShell Betiği doğrudan](#a-verify-subnet-is-endpoint-ps-100).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

## <a name="major-cmdlets"></a>Ana cmdlet'leri

Bu makalede vurgular **yeni AzSqlServerVirtualNetworkRule** alt uç nokta erişim denetim listesine (ACL), Azure SQL veritabanı sunucusu böylece bir kural oluşturma ekler cmdlet'i.

Aşağıdaki listede, diğer bir dizi gösterilir *ana* çağrınız hazırlamak için çalıştırılması gereken cmdlet'leri **yeni AzSqlServerVirtualNetworkRule**. Bu makalede, bu çağrıları ortaya [betik 3 "sanal ağ kuralı"](#a-script-30):

1. [Yeni AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig): Bir alt ağ nesnesini oluşturur.
2. [Yeni AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork): Sanal ağınızın alt veren oluşturur.
3. [Set-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/Set-azVirtualNetworkSubnetConfig): Sanal hizmet uç noktası, alt ağına atar.
4. [Set-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/Set-azVirtualNetwork): Sanal ağınıza yapılan güncelleştirmeleri kalıcıdır.
5. [New-AzSqlServerVirtualNetworkRule](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlservervirtualnetworkrule): Bir uç nokta, alt ağ olduktan sonra alt ağınızın bir sanal ağ kuralı Azure SQL veritabanı sunucunuzun ACL ekler.
   - Bu cmdlet parametresi sunar **- IgnoreMissingVNetServiceEndpoint**, başlangıç, Azure RM PowerShell modülü 5.1.1 sürümü.

## <a name="prerequisites-for-running-powershell"></a>PowerShell çalıştırma önkoşulları

- Zaten Azure'a gibi aracılığıyla oturum [Azure portalında][http-azure-portal-link-ref-477t].
- Zaten PowerShell betiklerini çalıştırabilirsiniz.

> [!NOTE]
> Hizmet uç noktaları sanal ağ/sunucunuza, aksi takdirde VNet güvenlik duvarı kuralı oluşturma başarısız olur eklemek istediğiniz alt ağ için açık olduğundan emin olun.

## <a name="one-script-divided-into-four-chunks"></a>Bir betik dört parçalara bölünür.

Bizim tanıtım PowerShell Betiği, daha küçük betikleri dizisine ayrılmıştır. Bölme işlemi öğrenme kolaylaştırır ve esneklik sağlar. Betikler, gösterilen sırada çalıştırılması gerekir. Betikleri çalıştırma süresi artık yoksa, bizim gerçek test çıkışı 4 betik sonra görüntülenir.

<a name="a-script-10" />

### <a name="script-1-variables"></a>1. kod: Değişkenler

İlk bu PowerShell Betiği, değerleri değişkenlere atar. Sonraki komut, şu değişkenlerde bağlıdır.

> [!IMPORTANT]
> Bu betiği çalıştırmadan önce isterseniz bu değerleri düzenleyebilirsiniz. Örneğin, bir kaynak grubu zaten varsa, kaynak grubu adınız olarak atanan değer düzenlemek isteyebilirsiniz.
>
> Abonelik adınız, betiğe düzenlenmelidir.

### <a name="powershell-script-1-source-code"></a>PowerShell komut dosyası 1 kaynak kodu

```powershell
######### Script 1 ########################################
##   LOG into to your Azure account.                     ##
##   (Needed only one time per powershell.exe session.)  ##
###########################################################

$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Connect-AzAccount; }

###########################################################
##  Assignments to variables used by the later scripts.  ##
###########################################################

# You can edit these values, if necessary.

$SubscriptionName = 'yourSubscriptionName';
Select-AzSubscription -SubscriptionName $SubscriptionName;

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

### <a name="script-2-prerequisites"></a>2. kod: Önkoşullar

Bu betik, uç nokta eylem olduğu sonraki komut için hazırlar. Bu betik, aşağıdaki oluşturur zaten mevcut ancak yalnızca öğeler listelenir. Bu öğe zaten var olduğundan emin olması durumunda, betik 2 atlayabilirsiniz:

- Azure kaynak grubu
- Azure SQL veritabanı sunucusu

### <a name="powershell-script-2-source-code"></a>PowerShell komut dosyası 2 kaynak kodu

```powershell
######### Script 2 ########################################
##   Ensure your Resource Group already exists.          ##
###########################################################

Write-Host "Check whether your Resource Group already exists.";

$gottenResourceGroup = $null;

$gottenResourceGroup = Get-AzResourceGroup `
  -Name        $ResourceGroupName `
  -ErrorAction SilentlyContinue;

if ($null -eq $gottenResourceGroup)
{
    Write-Host "Creating your missing Resource Group - $ResourceGroupName.";

    $gottenResourceGroup = New-AzResourceGroup `
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

$sqlDbServer = Get-AzSqlServer `
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

    $sqlDbServer = New-AzSqlServer `
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

## <a name="script-3-create-an-endpoint-and-a-rule"></a>3. betik: Bir uç nokta ve kuralı oluşturma

Bu betik, bir alt ağ ile sanal ağ oluşturur. Sonra da betik atar **Microsoft.Sql** alt ağınız için uç nokta türü. Son olarak betik alt ağınızın bir kuralın böylece SQL veritabanı sunucunuza erişim denetim listesine (ACL) ekler.

### <a name="powershell-script-3-source-code"></a>PowerShell Betiği 3 kaynak kodu

```powershell
######### Script 3 ########################################
##   Create your virtual network, and give it a subnet.  ##
###########################################################

Write-Host "Define a subnet '$SubnetName', to be given soon to a virtual network.";

$subnet = New-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Create a virtual network '$VNetName'." `
  "  Give the subnet to the virtual network that we created.";

$vnet = New-AzVirtualNetwork `
  -Name              $VNetName `
  -AddressPrefix     $VNetAddressPrefix `
  -Subnet            $subnet `
  -ResourceGroupName $ResourceGroupName `
  -Location          $Region;

###########################################################
##   Create a Virtual Service endpoint on the subnet.    ##
###########################################################

Write-Host "Assign a Virtual Service endpoint 'Microsoft.Sql' to the subnet.";

$vnet = Set-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Persist the updates made to the virtual network > subnet.";

$vnet = Set-AzVirtualNetwork `
  -VirtualNetwork $vnet;

$vnet.Subnets[0].ServiceEndpoints;  # Display the first endpoint.

###########################################################
##   Add the Virtual Service endpoint Id as a rule,      ##
##   into SQL Database ACLs.                             ##
###########################################################

Write-Host "Get the subnet object.";

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

$subnet = Get-AzVirtualNetworkSubnetConfig `
  -Name           $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Add the subnet .Id as a rule, into the ACLs for your Azure SQL Database server.";

$vnetRuleObject1 = New-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -VirtualNetworkSubnetId $subnet.Id;

$vnetRuleObject1;

Write-Host "Verify that the rule is in the SQL DB ACL.";

$vnetRuleObject2 = Get-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName;

$vnetRuleObject2;

Write-Host 'Completed script 3, the "Virtual-Network-Rule".';
```

<a name="a-script-40" />

## <a name="script-4-clean-up"></a>4. betik: Temizleme

Bu son kod, önceki betikler gösterimi için oluşturulan kaynakları siler. Ancak, betik aşağıdaki silmeden önce sizden onay ister:

- Azure SQL veritabanı sunucusu
- Azure Kaynak Grubu

Betik 4 1 betik tamamlandıktan sonra istediğiniz zaman çalıştırabilirsiniz.

### <a name="powershell-script-4-source-code"></a>PowerShell Betiği 4 kaynak kodu

```powershell
######### Script 4 ########################################
##   Clean-up phase A:  Unconditional deletes.           ##
##                                                       ##
##   1. The test rule is deleted from SQL DB ACL.        ##
##   2. The test endpoint is deleted from the subnet.    ##
##   3. The test virtual network is deleted.             ##
###########################################################

Write-Host "Delete the rule from the SQL DB ACL.";

Remove-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -ErrorAction            SilentlyContinue;

Write-Host "Delete the endpoint from the subnet.";

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

Remove-AzVirtualNetworkSubnetConfig `
  -Name $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Delete the virtual network (thus also deletes the subnet).";

Remove-AzVirtualNetwork `
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

    Remove-AzSqlServer `
      -ServerName        $SqlDbServerName `
      -ResourceGroupName $ResourceGroupName `
      -ErrorAction       SilentlyContinue;

    Write-Host "Remove the Azure Resource Group.";

    Remove-AzResourceGroup `
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

## <a name="actual-output-from-scripts-1-through-4"></a>1-4 arası betikler gerçek çıktısı

Bizim test çalıştırması çıkışı, ardından, kısaltılmış bir biçimde görüntülenir. PowerShell betikleri artık çalıştırdığı istemediğiniz durumlarda çıktı faydalı olabilir.

```cmd
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

Bu bizim ana PowerShell Betiği sonudur.

<a name="a-verify-subnet-is-endpoint-ps-100" />

## <a name="verify-your-subnet-is-an-endpoint"></a>Bir uç nokta, alt ağı olduğunu doğrulayın

Zaten atanmış bir alt ağa sahip olabileceğiniz **Microsoft.Sql** , zaten bir sanal hizmet uç noktası yani, tür adı. Kullanabileceğinizi [Azure portalında] [ http-azure-portal-link-ref-477t] uç noktasından bir sanal ağ kuralı oluşturun.

Veya, alt ağınızın olup kullanacağınızdan emin olabilirsiniz **Microsoft.Sql** tür adı. Bu eylemler için aşağıdaki PowerShell betiğini çalıştırabilirsiniz:

1. Alt ağınız olup olmadığını belirlemek **Microsoft.Sql** tür adı.
2. İsteğe bağlı olarak, yoksa, tür adı atayın.
    - Betik isteyen *onaylayın*, devamsızlık türü uygulanmadan önce adı.

### <a name="phases-of-the-script"></a>Betik aşamaları

PowerShell betiğini aşamaları şunlardır:

1. PS oturumu yalnızca bir kez gereken Azure hesabınızda oturum açın.  Değişkenleri atayın.
2. Sanal ağınızın ve alt ağınız için arama yapın.
3. Alt ağınız olarak etiketlenmiş **Microsoft.Sql** uç sunucu türü?
4. Tür adı, sanal hizmet uç noktası ekleme **Microsoft.Sql**, alt ağınız üzerinde.

> [!IMPORTANT]
> Bu betiği çalıştırmadan önce $-değişkenleri kodun üstüne yakın bir yere atanan değerleri düzenlemeniz gerekir.

### <a name="direct-powershell-source-code"></a>Doğrudan PowerShell kaynak kodu

Bu PowerShell Betiği güncelleştirilmez herhangi bir şey varsa Evet'i yanıt sürece olduğundan, onay ister. Tür adı betiği ekleyebilirsiniz **Microsoft.Sql** alt ağınız için. Ancak, yalnızca tür adı alt ağınız yoksa betik Ekle çalışır.

```powershell
### 1. LOG into to your Azure account, needed only once per PS session.  Assign variables.

$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Connect-AzAccount; }

# Assignments to variables used by the later scripts.
# You can EDIT these values, if necessary.

$SubscriptionName  = 'yourSubscriptionName';
Select-AzSubscription -SubscriptionName "$SubscriptionName";

$ResourceGroupName   = 'yourRGName';
$VNetName            = 'yourVNetName';
$SubnetName          = 'yourSubnetName';
$SubnetAddressPrefix = 'Obtain this value from the Azure portal.'; # Looks roughly like: '10.0.0.0/24'

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql';  # Do NOT edit. Is official value.

### 2. Search for your virtual network, and then for your subnet.

# Search for the virtual network.
$vnet = $null;
$vnet = Get-AzVirtualNetwork `
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

$vnet = Set-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

# Persist the subnet update.
$vnet = Set-AzVirtualNetwork `
  -VirtualNetwork $vnet;

for ($nn=0; $nn -lt $vnet.Subnets.Count; $nn++)
{ $vnet.Subnets[0].ServiceEndpoints; }  # Display.
```

### <a name="actual-output"></a>Gerçek çıkış

Aşağıdaki blok gerçek geri bildirim bölümümüzde (ile yüzeysel düzenlemeleri) görüntüler.

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
