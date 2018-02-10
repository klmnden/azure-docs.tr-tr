---
title: "Azure bulut Kabuğu (Önizleme) hızlı başlangıç PowerShell'de | Microsoft Docs"
description: "PowerShell bulut kabuğu için hızlı başlangıç"
services: Azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: damaerte
ms.openlocfilehash: 71ae70c13b4de87593345fd957a773741294b49c
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="quickstart-for-powershell-in-azure-cloud-shell-preview"></a>PowerShell Azure bulut Kabuğu (Önizleme) için hızlı başlangıç

Bu belge PowerShell bulut Kabuğu'nda kullanmak nasıl ayrıntıları [Azure portal](https://aka.ms/PSCloudPreview).

> [!NOTE]
> A [Azure bulut Kabuğu'nda Bash](quickstart.md) hızlı başlangıç kullanılabilir de.

## <a name="start-cloud-shell"></a>Bulut Kabuğu'nu başlatın

1. Tıklayın **bulut Kabuk** Azure portal'ın üst gezinti çubuğu düğmesinden

  ![](media/quickstart-powershell/shell-icon.png)

2. PowerShell ortamı açılan listeden seçin ve Azure sürücüde olur`(Azure:)`

  ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>PowerShell komutlarını çalıştırın

Normal PowerShell komutlarını bulut Kabuğu'nda aşağıdaki gibi çalıştırın:

```PowerShell
PS Azure:\> Get-Date
Monday, September 25, 2017 08:55:09 AM

PS Azure:\> Get-AzureRmVM -Status

ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Azure kaynaklarını gidin

 1. Aboneliklerinizi listesi

    ``` PowerShell
    PS Azure:\> dir
    ```

 2. `cd`tercih edilen aboneliğinizi

    ``` PowerShell
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. Geçerli abonelik altında tüm Azure kaynaklarını görüntüleme
 
    Tür `dir` Azure kaynaklarınızı birden çok görünüm listesi.
 
    ``` PowerShell
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

### <a name="allresources-view"></a>AllResources görünümü 
Tür `dir` altında `AllResources` Azure kaynaklarınızı görüntülemek için dizin.
    
    PS Azure:\MySubscriptionName> dir AllResources

### <a name="explore-resource-groups"></a>Kaynak grupları keşfedin

 Gidebilirsiniz `ResourceGroups` dizin ve belirli bir kaynak grubu içindeki sanal makineleri bulabilirsiniz.

``` PowerShell
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271

```
> [!NOTE]
> Yazdığınızda ikinci kez karşılaşabilirsiniz `dir`, bulut Kabuk çok daha hızlı öğeleri görüntülemek kullanabilirsiniz.
> Alt öğeler daha iyi bir kullanıcı deneyimi için bellekte önbelleğe alınan olmasıdır.
Bununla birlikte, her zaman kullanabilirsiniz `dir -Force` yeni veri almak için.

### <a name="navigate-storage-resources"></a>Depolama kaynaklarını gidin
    
İçine girerek `StorageAccounts` kolayca gezinmenizi depolama kaynaklarınıza klasörü
    
``` PowerShell 
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files


Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>


```

Bağlantı dizesi ile Azure dosya paylaşımı bağlamak için aşağıdaki komutu kullanabilirsiniz.
        
``` PowerShell
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>


```

Ayrıntılar için bkz [bir Azure dosya paylaşımı bağlayabilir ve Windows paylaşıma erişim][azmount].

Azure dosya paylaşımı altındaki dizinleri şu şekilde de gidebilirsiniz:

            
``` PowerShell
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1

    
```

### <a name="interact-with-virtual-machines"></a>Sanal makineleri ile etkileşim

Tüm sanal makineler geçerli abonelik altında bulabilirsiniz `VirtualMachines` dizin.
    
``` PowerShell
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running


```

#### <a name="invoke-powershell-script-across-remote-vms"></a>PowerShell Betiği uzak VM'ler üzerindeki çağırma

 > [!WARNING]
 > Lütfen [Azure VM'ler, uzaktan yönetimi sorunlarını giderme](troubleshooting.md#powershell-resolutions).

  Bir VM MyVM1, sahip olduğunuz varsayılarak kullanalım `Invoke-AzureRmVMCommand` uzak makinede PowerShell betik bloğu çağırmak için.

  ``` Powershell
  Invoke-AzureRmVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```
  Ayrıca ilk VirtualMachines dizinine gidin ve çalıştırın `Invoke-AzureRmVMCommand` gibi.

  ``` PowerShell
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzureRmVMCommand -Scriptblock{Get-ComputerInfo}
  ```
  Aşağıdakine benzer bir çıktı görürsünüz:

  ``` PowerShell
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

#### <a name="interactively-log-on-to-a-remote-vm"></a>Uzak bir VM etkileşimli oturum açma

Kullanabileceğiniz `Enter-AzureRmVM` için Azure'da çalışan VM etkileşimli olarak oturum açın.

  ``` PowerShell
  Enter-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

Ayrıca gidebilirsiniz `VirtualMachines` ilk ve çalışma dizinini `Enter-AzureRmVM` gibi

  ``` PowerShell
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzureRmVM
 ```

### <a name="discover-webapps"></a>WebApps Bul

İçine girerek `WebApps` kolayca gezinmenizi web apps kaynaklarınızı klasörü

``` PowerShell
PS Azure:\MySubscriptionName> dir .\WebApps\

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Stopped  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US



# You can use Azure cmdlets to Start/Stop your web apps
PS Azure:\MySubscriptionName\WebApps> Start-AzureRmWebApp -Name mywebapp1 -ResourceGroupName MyResourceGroup1

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

[Win32 OpenSSH](https://github.com/PowerShell/Win32-OpenSSH) PowerShell bulut Kabuğu'nda kullanılabilir.
Sunucular veya VM'ler SSH kullanarak kimlik doğrulaması için bulut Kabuğu'nda genel-özel anahtar çifti oluşturmalı ve ortak anahtara yayımlayın `authorized_keys` uzak makinede gibi `/home/user/.ssh/authorized_keys`.

> [!NOTE]
> SSH ortak olmayan özel anahtarlarını kullanarak oluşturabileceğiniz `ssh-keygen` ve onlara yayımlamak `$env:USERPROFILE\.ssh` bulut Kabuğu'nda.

### <a name="using-a-custom-profile-to-persist-git-and-ssh-settings"></a>GIT ve SSH ayarlarını sürdürmek için özel bir profil kullanma

Oturumları bağlı kalmaz beri oturum kapatma, kaydetme, `$env:USERPROFILE\.ssh` klasörüne `CloudDrive` veya Bulut Kabuk başlatıldığında bir simgesel oluşturun.
Aşağıdaki kod parçacığında simgesel CloudDrive için oluşturmak için profile.ps1 ekleyin.

``` PowerShell
# Check if the .ssh folder exists
if( -not (Test-Path $home\CloudDrive\.ssh)){
    mkdir $home\CloudDrive\.ssh
}

# .ssh path relative to this script
$script:sshFolderPath = Join-Path $PSScriptRoot .ssh

# Create a symlink to .ssh in user's $home
if(Test-Path $script:sshFolderPath){
   if(-not (Test-Path (Join-Path $HOME .ssh ))){
        New-Item -ItemType SymbolicLink -Path $HOME -Name .ssh -Value $script:sshFolderPath
   }
}

```

### <a name="using-ssh"></a>SSH kullanma

Yönergeleri izleyerek [burada](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-powershell) AzureRM cmdlet'lerini kullanarak yeni bir VM yapılandırması oluşturmak için.
Önce çağırma içine `New-AzureRmVM` dağıtım başlangıcı, SSH ortak anahtarını VM yapılandırmasına ekleyin.
Yeni oluşturulan VM ortak anahtar içerecek `~\.ssh\authorized_keys` konumu, böylelikle kimlik bilgisi serbest SSH oturumu VM etkinleştirme.

``` PowerShell

# Create VM config object - $vmConfig using instructions on linked page above

# Generate SSH keys in Cloud Shell
ssh-keygen -t rsa -b 2048 -f $HOME\.ssh\id_rsa 

# Ensure VM config is updated with SSH keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmConfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

# Create a virtual machine
New-AzureRmVM -ResourceGroupName <yourResourceGroup> -Location <vmLocation> -VM $vmConfig

# SSH to the VM
ssh azureuser@MyVM.Domain.Com

```


## <a name="list-available-commands"></a>Liste kullanılabilir komutlar

Altında `Azure` sürücü, yazın `Get-AzureRmCommand` bağlamı özgü Azure komutları almak için.

Alternatif olarak, her zaman kullanabilirsiniz `Get-Command *azurerm* -Module AzureRM.*` kullanılabilir Azure komutları bulunamıyor.

## <a name="install-custom-modules"></a>Özel modüllerini yükleyin

Çalıştırabilirsiniz `Install-Module` modüllerden yüklemek için [PowerShell Galerisi][gallery].

## <a name="get-help"></a>Get-Help

Tür `Get-Help` Azure bulut Kabuğu'nda PowerShell hakkında bilgi almak için.

``` PowerShell
PS Azure:\> Get-Help
```

Belirli bir komut için hala yapabileceğiniz `Get-Help` cmdlet tarafından izlenen.

``` PowerShell
PS Azure:\> Get-Help Get-AzureRmVM
```

## <a name="use-azure-files-to-store-your-data"></a>Azure dosyaları verilerinizi depolamak için kullanın

Bir komut dosyası deyin oluşturabilirsiniz `helloworld.ps1`ve kaydetmesi, `CloudDrive` Kabuk oturumlarında kullanmak için.

``` PowerShell
cd C:\users\ContainerAdministrator\CloudDrive
PS C:\users\ContainerAdministrator\CloudDrive> vim .\helloworld.ps1
# Add the content, such as 'Hello World!'
PS C:\users\ContainerAdministrator\CloudDrive> .\helloworld.ps1
Hello World!
```

Bulut Kabuğu'nda PowerShell kullandığınızda, sonraki sefer `helloworld.ps1` dosya var altında `CloudDrive` , Azure dosya paylaşımı bağladığı klasör.

## <a name="use-custom-profile"></a>Özel profil kullan

PowerShell oluşturarak PowerShell ortamınızın özelleştirebilirsiniz profillerini - `profile.ps1` veya `Microsoft.PowerShell_profile.ps1`. Altında kaydetmek `CloudDrive` böylece bulut Kabuğu'nu başlatın zaman her PowerShell oturumunda yüklenebilir.

Profil oluşturma, başvurmak için [hakkında profilleri][profile].

## <a name="use-git"></a>Git kullanın

Oluşturmanıza gerek bulut Kabuğu'nda bir Git deposuna kopyalamak için bir [kişisel erişim belirteci] [ githubtoken] ve kullanıcı adı olarak kullanın. Bir kez, belirteç, kopya deposu gibi vardır:

 ``` PowerShell
  git clone https://<your-access-token>@github.com/username/repo.git

```
Bulut Kabuk oturumlarında oturumu veya oturum zaman aşımına uğramadan devam etmeyen olduğundan, Git yapılandırma dosyası sonraki oturum açma sırasında mevcut olmaz. Kalıcı, Git config sağlamak için .gitconfig kaydedin, `CloudDrive` ve kopyalama veya Bulut Kabuk başlatıldığında bir simgesel oluşturun. Aşağıdaki kod parçacığında, profile.ps1 içinde bir simgesel oluşturulacağı `CloudDrive`.

 ``` PowerShell
 
# .gitconfig path relative to this script
$script:gitconfigPath = Join-Path $PSScriptRoot .gitconfig

# Create a symlink to .gitconfig in user's $home
if(Test-Path $script:gitconfigPath){

    if(-not (Test-Path (Join-Path $home .gitconfig ))){
         New-Item -ItemType SymbolicLink -Path $home -Name .gitconfig -Value $script:gitconfigPath
    }
}

```
## <a name="exit-the-shell"></a>Kabuktan çıkış yapma

Tür `exit` oturumu sona erdirmek için.

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
