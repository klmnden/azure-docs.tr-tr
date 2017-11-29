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
ms.date: 09/25/2017
ms.author: damaerte
ms.openlocfilehash: 913bd917ae7c2b44df097ead9c3e35841338905c
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
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

```Powershell
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

    ``` Powershell
    PS Azure:\> dir
    ```

 2. `cd`tercih edilen aboneliğinizi

    ``` Powershell
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

  Bir VM MyVM1, sahip olduğunuz varsayılarak kullanalım `Invoke-AzureRmVMCommand` uzak makinede PowerShell scriptblock çağırmak için.

  ``` Powershell
  Invoke-AzureRmVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```
  Ayrıca ilk virtualMachines dizinine gidin ve çalıştırın `Invoke-AzureRmVMCommand` gibi.

  ``` Powershell
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzureRmVMCommand -Scriptblock{Get-ComputerInfo}
  ```
  Aşağıdakine benzer bir çıktı görürsünüz:

  ``` Powershell
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

  ``` Powershell
  Enter-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

Ayrıca gidebilirsiniz `virtualMachines` ilk ve çalışma dizinini `Enter-AzureRmVM` gibi

  ``` Powershell
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

## <a name="list-available-commands"></a>Liste kullanılabilir komutlar

Altında `Azure` sürücü, yazın `Get-AzureRmCommand` bağlamı özgü Azure komutları almak için.

Alternatif olarak, her zaman kullanabilirsiniz `Get-Command *azurerm* -Module AzureRM.*` kullanılabilir Azure komutları bulunamıyor.

## <a name="install-custom-modules"></a>Özel modüllerini yükleyin

Çalıştırabilirsiniz `Install-Module` modüllerden yüklemek için [PowerShell Galerisi][gallery].

## <a name="get-help"></a>Get-Help

Tür `Get-Help` Azure bulut Kabuğu'nda PowerShell hakkında bilgi almak için.

``` Powershell
PS Azure:\> Get-Help
```

Belirli bir komut için Get-Help cmdlet'i tarafından izlenen hala yapabilirsiniz.

``` Powershell
PS Azure:\> Get-Help Get-AzureRmVM
```

## <a name="use-azure-files-to-store-your-data"></a>Azure dosyaları verilerinizi depolamak için kullanın

Bir komut dosyası deyin oluşturabilirsiniz `helloworld.ps1`ve kaydetmesi, `CloudDrive` Kabuk oturumlarında kullanmak için.

``` Powershell
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

Oluşturmanıza gerek bulut Kabuğu'nda bir git deposuna kopyalamak için bir [kişisel erişim belirteci] [ githubtoken] ve kullanıcı adı olarak kullanın. Bir kez, belirteç, kopya deposu gibi vardır:

 ``` PowerShell
  git clone https://<your-access-token>@github.com/username/repo.git

```
Bulut Kabuk oturumlarında oturumu veya oturum zaman aşımına uğramadan devam etmeyen olduğundan, Git yapılandırma dosyası sonraki oturum açma sırasında mevcut olmaz. Kalıcı, Git config sağlamak için .gitconfig kaydedin, `CloudDrive` ve kopyalama veya Bulut Kabuk başlatıldığında bir simgesel oluşturun. Aşağıdaki kod parçacığında, profile.ps1 içinde bir simgesel oluşturulacağı `CloudDrive`.

 ``` PowerShell
 
# .gitconfig path relative to this script
$script:gitconfigPath = Join-Path $PSScriptRoot .gitconfig

# Create a symlink to .gitconfig in user's $home
if(Test-Path $script:gitconfigPath){

    if(-not (Test-Path (Join-Path $Home .gitconfig ))){
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
