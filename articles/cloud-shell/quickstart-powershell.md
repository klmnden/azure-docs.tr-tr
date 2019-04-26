---
title: Azure Cloud Shell hızlı başlangıç PowerShell'de | Microsoft Docs
description: Hızlı Başlangıç için Cloud shell'de PowerShell
services: Azure
documentationcenter: ''
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: damaerte
ms.openlocfilehash: 1fc9883e0ea35c384c3bfc83e76b8eded48cbcba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60199542"
---
# <a name="quickstart-for-powershell-in-azure-cloud-shell"></a>Hızlı Başlangıç için Azure Cloud shell'de PowerShell

Cloud Shell içinde PowerShell kullanma ayrıntılı [Azure portalında](https://portal.azure.com/).

> [!NOTE]
> A [Azure Cloud Shell'de Bash](quickstart.md) hızlı başlangıç, ayrıca kullanılabilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="start-cloud-shell"></a>Cloud Shell'i Başlat

1. Tıklayarak **Cloud Shell** düğmesi Azure portalının üst gezinti çubuğundan

   ![](media/quickstart-powershell/shell-icon.png)

2. Açılan listeden bir PowerShell ortamı seçin ve Azure sürücüde olur `(Azure:)`

   ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>PowerShell komutlarını çalıştırın

Normal PowerShell komutlarını Cloud Shell'de aşağıdaki gibi çalıştırın:

```azurepowershell-interactive
PS Azure:\> Get-Date

# Expected Output
Friday, July 27, 2018 7:08:48 AM

PS Azure:\> Get-AzVM -Status

# Expected Output
ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Azure kaynaklarında gezinme

 1. Tüm aboneliklerinizden listesinde `Azure` sürücü

    ```azurepowershell-interactive
    PS Azure:\> dir
    ```

 2. `cd` tercih edilen aboneliğinize

    ```azurepowershell-interactive
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. Geçerli abonelik altındaki tüm Azure kaynaklarını görüntüleyin

    Tür `dir` birden çok görünüm, Azure kaynaklarınızın listesi.

    ```azurepowershell-interactive
    PS Azure:\MySubscriptionName> dir

        Directory: azure:\MySubscriptionName

    Mode Name
    ---- ----
    +    AllResources
    +    ResourceGroups
    +    StorageAccounts
    +    VirtualMachines
    +    WebApps
    ```

### <a name="allresources-view"></a>AllResources görüntüle

Tür `dir` altında `AllResources` Azure kaynaklarınızı görüntülemek için dizin.

```azurepowershell-interactive
PS Azure:\MySubscriptionName> dir AllResources
```

### <a name="explore-resource-groups"></a>Kaynak grupları keşfedin

 Gidebilirsiniz `ResourceGroups` dizin ve belirli bir kaynak grubu içindeki sanal makine bulabilirsiniz.

```azurepowershell-interactive
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271
```

> [!NOTE]
> Bu ikinci kez yazdığınızda görebilirsiniz `dir`, Cloud Shell'i çok daha hızlı öğeleri görüntüleyebilir.
> Alt öğeler daha iyi bir kullanıcı deneyimi için bellekte önbelleğe olmasıdır.
Ancak, her zaman kullanabilirsiniz `dir -Force` yeni veri almak için.

### <a name="navigate-storage-resources"></a>Depolama kaynaklarını gidin

İçine girerek `StorageAccounts` dizin, tüm depolama kaynaklarını kolayca gidebilirsiniz

```azurepowershell-interactive
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files

Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>
```

Bağlantı dizesi ile Azure dosya paylaşımını bağlamak için aşağıdaki komutu kullanabilirsiniz.

```azurepowershell-interactive
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>
```

Ayrıntılar için bkz [bir Azure dosya paylaşımını bağlama ve Windows içinde paylaşıma erişme][azmount].

Azure dosyaları paylaşım altındaki dizinleri şu şekilde da gidebilirsiniz:

```azurepowershell-interactive
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1
```

### <a name="interact-with-virtual-machines"></a>Sanal makineleri ile etkileşim kurma

Tüm sanal makineler geçerli abonelik altında bulabilirsiniz `VirtualMachines` dizin.

```azurepowershell-interactive
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running
```

#### <a name="invoke-powershell-script-across-remote-vms"></a>Uzak sanal makinelerde PowerShell betiğini Çağır

 > [!WARNING]
 > Lütfen [Azure sanal makinelerini uzaktan yönetimi sorunlarını giderme](troubleshooting.md#troubleshooting-remote-management-of-azure-vms).

  Bir VM MyVM1, sahip olduğunuz varsayılarak kullanalım `Invoke-AzVMCommand` uzak makinede bir PowerShell komut dosyası bloğu çağırmak için.

  ```azurepowershell-interactive
  Invoke-AzVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```

  Ayrıca ilk VirtualMachines dizine gidin ve çalıştırma `Invoke-AzVMCommand` gibi.

  ```azurepowershell-interactive
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzVMCommand -Scriptblock {Get-ComputerInfo}

  # You will see output similar to the following:

  PSComputerName                                          : 65.52.28.207
  RunspaceId                                              : 2c2b60da-f9b9-4f42-a282-93316cb06fe1
  WindowsBuildLabEx                                       : 14393.1066.amd64fre.rs1_release_sec.170327-1835
  WindowsCurrentVersion                                   : 6.3
  WindowsEditionId                                        : ServerDatacenter
  WindowsInstallationType                                 : Server
  WindowsInstallDateFromRegistry                          : 5/18/2017 11:26:08 PM
  WindowsProductId                                        : 00376-40000-00000-AA947
  WindowsProductName                                      : Windows Server 2016 Datacenter
  WindowsRegisteredOrganization                           :
   ...
  ```

#### <a name="interactively-log-on-to-a-remote-vm"></a>Etkileşimli olarak uzak bir VM'de oturum açma

Kullanabileceğiniz `Enter-AzVM` Azure'da çalışan bir VM'ye etkileşimli olarak oturum.

  ```azurepowershell-interactive
  PS Azure:\> Enter-AzVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

Ayrıca gidebilirsiniz `VirtualMachines` dizinin ilk ve çalışma `Enter-AzVM` gibi

  ```azurepowershell-interactive
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzVM
 ```

### <a name="discover-webapps"></a>Web uygulamaları Bul

İçine girerek `WebApps` dizin, web apps kaynaklarınızı kolayca gidebilirsiniz

```azurepowershell-interactive
PS Azure:\MySubscriptionName> dir .\WebApps\

    Directory: Azure:\MySubscriptionName\WebApps

Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Stopped  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US

# You can use Azure cmdlets to Start/Stop your web apps
PS Azure:\MySubscriptionName\WebApps> Start-AzWebApp -Name mywebapp1 -ResourceGroupName MyResourceGroup1

Name           State    ResourceGroup        EnabledHostNames                   Location
----           -----    -------------        ----------------                   --------
mywebapp1      Running  MyResourceGroup1     {mywebapp1.azurewebsites.net ...   West US

# Refresh the current state with -Force
PS Azure:\MySubscriptionName\WebApps> dir -Force

    Directory: Azure:\MySubscriptionName\WebApps

Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Running  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US
```

## <a name="ssh"></a>SSH

Sunucular veya VM'ler SSH kullanarak kimlik doğrulaması için Cloud Shell'de ortak-özel anahtar çiftini oluşturmak ve yayımlamak için ortak anahtar `authorized_keys` uzak makinede gibi `/home/user/.ssh/authorized_keys`.

> [!NOTE]
> SSH özel ortak anahtarları kullanarak oluşturabileceğiniz `ssh-keygen` yayımlayın `$env:USERPROFILE\.ssh` Cloud shell'de.

### <a name="using-ssh"></a>SSH kullanma

Yönergeleri izleyerek [burada](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-powershell) Azure PowerShell cmdlet'lerini kullanarak yeni bir VM yapılandırması oluşturmak için.
Çağırmadan önce içine `New-AzVM` dağıtımı devre dışı istiyorsanız SSH ortak anahtarı VM yapılandırmasına ekleyin.
Yeni oluşturulan VM içindeki ortak anahtar içerecek `~\.ssh\authorized_keys` konumu, böylelikle sanal makineye SSH oturumu kimlik bilgisi gerektirmeyen etkinleştirme.

```azurepowershell-interactive
# Create VM config object - $vmConfig using instructions on linked page above

# Generate SSH keys in Cloud Shell
ssh-keygen -t rsa -b 2048 -f $HOME\.ssh\id_rsa 

# Ensure VM config is updated with SSH keys
$sshPublicKey = Get-Content "$HOME\.ssh\id_rsa.pub"
Add-AzVMSshPublicKey -VM $vmConfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

# Create a virtual machine
New-AzVM -ResourceGroupName <yourResourceGroup> -Location <vmLocation> -VM $vmConfig

# SSH to the VM
ssh azureuser@MyVM.Domain.Com
```

## <a name="list-available-commands"></a>Kullanılabilir komutlar listelenmektedir

Altında `Azure` sürücü, tip `Get-AzCommand` bağlam özgü Azure komutları almak için.

Alternatif olarak, her zaman kullanabilirsiniz `Get-Command *az* -Module Az.*` kullanılabilir Azure komutları bulunacak.

## <a name="install-custom-modules"></a>Özel modüller yükleme

Çalıştırabileceğiniz `Install-Module` modülleri yüklemek için [PowerShell Galerisi][gallery].

## <a name="get-help"></a>Get-Help

Tür `Get-Help` Azure Cloud Shell'de PowerShell hakkında bilgi almak için.

```azurepowershell-interactive
Get-Help
```

Belirli bir komut için bunu hala yapabilirsiniz `Get-Help` cmdlet'i tarafından izlenen.

```azurepowershell-interactive
Get-Help Get-AzVM
```

## <a name="use-azure-files-to-store-your-data"></a>Verilerinizi depolamak için Azure dosyaları'nı kullanma

Örneğin bir komut dosyası oluşturabilirsiniz `helloworld.ps1`ve kaydedin, `clouddrive` shell oturumlarında kullanılacak.

```azurepowershell-interactive
cd $HOME\clouddrive
# Create a new file in clouddrive directory
New-Item helloworld.ps1
# Open the new file for editing
code .\helloworld.ps1
# Add the content, such as 'Hello World!'
.\helloworld.ps1
Hello World!
```

Cloud Shell'de PowerShell kullanırken başlattığınızda `helloworld.ps1` altında dosya var `$HOME\clouddrive` , Azure dosyaları paylaşımına bağlandığı dizin.

## <a name="use-custom-profile"></a>Özel profilini kullanın

PowerShell oluşturarak PowerShell ortamınızın özelleştirebilirsiniz profil - `profile.ps1` (veya `Microsoft.PowerShell_profile.ps1`).
Bunun altında kaydetmek `$profile.CurrentUserAllHosts` (veya `$profile.CurrentUserAllHosts`), böylece Cloud Shell oturumunda her PowerShell'de yüklenebilir.

Profil oluşturma, başvurmak için [profilleri hakkında][profile].

## <a name="use-git"></a>Git'i kullanın

Cloud shell'de bir Git deposuna kopyalamak için oluşturmak gereken bir [kişisel erişim belirteci] [ githubtoken] ve kullanıcı adı olarak kullanın. Belirteç, kopya depoyu gibi olduğunda:

```azurepowershell-interactive
  git clone https://<your-access-token>@github.com/username/repo.git
```

## <a name="exit-the-shell"></a>Kabuktan çıkış yapma

Tür `exit` oturumu sona erdirmek için.

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
