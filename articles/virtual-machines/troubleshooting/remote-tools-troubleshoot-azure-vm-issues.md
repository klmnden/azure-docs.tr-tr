---
title: Azure VM sorunlarını gidermek için Uzak Araçlar'ı kullanma | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 01/11/2018
ms.author: delhan
ms.openlocfilehash: 513ce98703e67053ab0bcac3e6fc7a3e959f6870
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64717276"
---
# <a name="use-remote-tools-to-troubleshoot-azure-vm-issues"></a>Azure VM sorunlarını gidermek için Uzak Araçlar kullanın

Bir Azure sanal makine'de (VM) sorunları giderirken, Uzak Masaüstü Protokolü (RDP) kullanmak yerine bu makalede açıklanan uzak Araçlar'ı kullanarak sanal Makineye bağlanabilirsiniz.

## <a name="serial-console"></a>Seri Konsol

Kullanım [sanal makinenin seri konsol](serial-console-windows.md) uzak bir Azure sanal makinesinde komutlarını çalıştırmak.

## <a name="remote-cmd"></a>Uzak CMD

İndirme [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec). Aşağıdaki komutu çalıştırarak VM'ye bağlanın:

```cmd
psexec \\<computer>-u user -s cmd
```

>[!Note]
>* Komutu, aynı sanal ağda bulunan bir bilgisayarda çalıştırmanız gerekir.
>* Değiştirilecek DIP veya ana bilgisayar adı kullanılabilir \<bilgisayar >.
>* -S parametresi komut sistem hesabı (yönetici izni) kullanılarak çağrılır emin olur.
>* PsExec 135 ve 445 numaralı TCP bağlantı noktalarını kullanır. Bu nedenle, iki bağlantı noktası Güvenlik Duvarı'nı açık olması gerekir.

## <a name="run-commands"></a>Komutlar çalıştırın

Bkz: [çalıştırma PowerShell betiklerini Windows VM'nizi komutu Çalıştır](../windows/run-command.md) VM üzerinde betikleri çalıştırma için komutları çalıştırma özelliğini kullanma hakkında daha fazla bilgi için.

## <a name="customer-script-extension"></a>Müşteri betik uzantısını

Özel betik uzantısı özelliği, hedef sanal Makineye özel bir betik çalıştırmak için kullanabilirsiniz. Bu özelliği kullanmak için aşağıdaki koşullar karşılanmalıdır:

* VM bağlantısı vardır.

* Azure aracısı yüklü olduğundan ve VM üzerinde beklendiği gibi çalışmaktadır.

* Uzantı, sanal makinede daha önce yüklenmediyse.
 
  Uzantı komut kullanıldığı ilk kez ekleyecektir. Daha sonra bu özelliği kullanmak, uzantı zaten kullanılmış algılar ve yeni komut dosyası karşıya yüklemez.

Bir depolama hesabına betiğinizi ve kendi kapsayıcısı oluşturmak gerekir. Ardından, sanal makine bağlantısı olan bir bilgisayarda Azure PowerShell'de aşağıdaki betiği çalıştırın.

### <a name="for-v1-vms"></a>V1 VM'ler için

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>" 
$storageAccount = "<<STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for the blob in the storage
$vmName = "<<VM NAME>>" 
$vmCloudService = "<<CLOUD SERVICE>>" #Resource group/Cloud Service where the VM is hosted. I.E.: For "demo305.cloudapp.net" the cloud service is going to be demo305

#Setup the Azure Powershell module and ensure the access to the subscription
Import-Module Azure
Add-AzureAccount  #Ensure Login with account associated with subscription ID
Get-AzureSubscription -SubscriptionId $subscriptionID | Select-AzureSubscription

#Setup the access to the storage account and upload the script
$storageKey = (Get-AzureStorageKey -StorageAccountName $storageAccount).Primary
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)<
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
$vm = Get-AzureVM -ServiceName $vmCloudService -Name $vmName
Set-AzureVMCustomScriptExtension "CustomScriptExtension" -VM $vm -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName | Update-AzureVM
```

### <a name="for-v2-vms"></a>V2 VM'ler için

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>"
$storageAccount = "<<STORAGE ACCOUNT>>"
$storageRG = "<<RESOURCE GROUP OF THE STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for blob in storage
$vmName = "<<VM NAME>>" 
$vmResourceGroup = "<<RESOURCE GROUP>>"
$vmLocation = "<<DATACENTER>>" 
 
#Setup the Azure Powershell module and ensure the access to the subscription
Login-AzAccount #Ensure Login with account associated with subscription ID
Get-AzSubscription -SubscriptionId $subscriptionID | Select-AzSubscription

#Setup the access to the storage account and upload the script 
$storageKey = (Get-AzStorageAccountKey -ResourceGroupName $storageRG -Name $storageAccount).Value[0]
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
Set-AzVMCustomScriptExtension -Name "CustomScriptExtension" -ResourceGroupName $vmResourceGroup -VMName $vmName -Location $vmLocation -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName
```

## <a name="remote-powershell"></a>Uzak PowerShell

>[!Note]
>Bu seçenek kullanabilmesi için TCP bağlantı noktası 5986 (HTTPS) açık olması gerekir.
>
>ARM Vm'leri için ağ güvenlik grubu (NSG) üzerinde 5986 bağlantı noktasını açmanız gerekir. Daha fazla bilgi için bkz: güvenlik grupları. 
>
>RDFE VM'ler için özel olan bir uç nokta olmalıdır (5986) bağlantı noktası ve genel bağlantı noktası. Ardından, genel bir yönelik-port NSG üzerinde de açmanız gerekir.

### <a name="set-up-the-client-computer"></a>İstemci bilgisayarı ayarlama

VM'ye uzaktan bağlanmak üzere PowerShell kullanmak için öncelikle bağlantısına izin vermek için istemci bilgisayar'kurmak olması. Bunu yapmak için uygun şekilde aşağıdaki komutu çalıştırarak VM PowerShell güvenilir konaklar listesine ekleyin.

Bir VM güvenilir konak listesine eklemek için:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName>
```

Birden çok VM güvenilir konak listesine eklemek için:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName1>,<ComputerName2>
```

Tüm bilgisayarlarda güvenilir konak listesine eklemek için:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value *
```

### <a name="enable-remoteps-on-the-vm"></a>VM'de RemotePS etkinleştir

Klasik VM'ler için aşağıdaki betiği çalıştırmak için özel betik uzantısı kullanın:

```powershell
Enable-PSRemoting -Force
New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My).Thumbprint
$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername""; CertificateThumbprint=""$thumbprint""}"
cmd.exe /C $command
```

ARM Vm'leri için EnableRemotePS betiği çalıştırmak için portaldan çalıştırma komutları kullanın:

![Çalıştır Komutu](./media/remote-tools-troubleshoot-azure-vm-issues/run-command.png)

### <a name="connect-to-the-vm"></a>VM’ye bağlanma

Bilgisayar konumu istemciye bağlı olarak aşağıdaki komutu çalıştırın:

* VNET veya dağıtım dışında

  * Klasik bir sanal makine için aşağıdaki komutu çalıştırın:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName  "<<CLOUDSERVICENAME.cloudapp.net>>" -port "<<PUBLIC PORT NUMBER>>" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

  * Bir ARM sanal makine için önce genel IP adresi için bir DNS adı ekleyin. Ayrıntılı adımlar için bkz. [tam etki alanı adını Azure portalında bir Windows VM için oluşturma](../windows/portal-create-fqdn.md). Ardından şu komutu çalıştırın:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName "<<DNSname.DataCenter.cloudapp.azure.com>>" -port "5986" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

* VNET veya dağıtım içinde aşağıdaki komutu çalıştırın:
  
  ```powershell
  $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
  Enter-PSSession -ComputerName  "<<HOSTNAME>>" -port 5986 -Credential (Get-Credential) -useSSL -SessionOption $Skip
  ```

>[!Note] 
>SkipCaCheck bayrak ayarlandığında, oturumu başlattığınızda, VM'ye sertifika alma gereksinimini atlar.

Bir betiği sanal makinede uzaktan çalıştırmak için Invoke-Command cmdlet'ini de kullanabilirsiniz:

```powershell
Invoke-Command -ComputerName "<<COMPUTERNAME>" -ScriptBlock {"<<SCRIPT BLOCK>>"}
```

## <a name="remote-registry"></a>Uzak Kayıt defteri

>[!Note]
>TCP bağlantı noktası 135'i veya 445, bu seçeneği kullanmak için açık olmalıdır.
>
>ARM Vm'leri için bağlantı noktası 5986 NSG açmanız gerekmez. Daha fazla bilgi için bkz: güvenlik grupları. 
>
>RDFE VM'ler için özel bir bağlantı noktası 5986 olan bir uç nokta ve bir genel bağlantı noktası olmalıdır. Bu NSG genel kullanıma yönelik bağlantı noktasını da açmanız gerekmez.

1. Aynı sanal ağ başka bir VM'den, Kayıt Defteri Düzenleyicisi (regedit.exe) açın.

2. Seçin **dosya** >**ağ kayıt bağlanmak**.

   ![Uzak bir seçeneği](./media/remote-tools-troubleshoot-azure-vm-issues/remote-registry.png) 

3. ' % S'hedef sanal Makineyi bulun tarafından **hostname** veya **dinamik IP** "Seçilecek nesne adını girin" kutusuna girerek (tercih).

   ![Uzak bir seçeneği](./media/remote-tools-troubleshoot-azure-vm-issues/input-computer-name.png) 
 
4. Hedef sanal makine için kimlik bilgilerini girin.

5. Gerekli kayıt defteri değişiklikleri yapın.

## <a name="remote-services-console"></a>Uzak Hizmetler konsolunu

>[!Note]
>TCP bağlantı noktaları 135 veya 445, bu seçeneği kullanmak için açık olmalıdır.
>
>ARM Vm'leri için bağlantı noktası 5986 NSG açmanız gerekmez. Daha fazla bilgi için bkz: güvenlik grupları. 
>
>RDFE VM'ler için özel bir bağlantı noktası 5986 olan bir uç nokta ve bir genel bağlantı noktası olmalıdır. Ardından, bu NSG genel kullanıma yönelik bağlantı noktasını da açmanız gerekir.

1. Bir örneği aynı sanal AĞDA bulunan başka bir VM'den açın **Services.msc**.

2. Sağ **hizmetler (yerel)** .

3. Seçin **başka bir bilgisayara bağlan**.

   ![Uzak hizmet](./media/remote-tools-troubleshoot-azure-vm-issues/remote-services.png)

4. ' % S'hedef sanal makine, dinamik IP girin.

   ![Giriş DIP](./media/remote-tools-troubleshoot-azure-vm-issues/input-ip-address.png)

5. Hizmetler için gerekli değişiklikleri yapın.

## <a name="next-steps"></a>Sonraki Adımlar

[PSSession girin](https://technet.microsoft.com/library/hh849707.aspx)

[Özel betik uzantısı için Klasik dağıtım modelini kullanarak Windows](../extensions/custom-script-classic.md)

PsExec parçasıdır [PSTools Suite](https://download.sysinternals.com/files/PSTools.zip).

PSTools paketi hakkında daha fazla bilgi için bkz. [PSTools Suite](https://docs.microsoft.com/sysinternals/downloads/pstools).


