---
title: BitLocker'ı Azure sanal makinesinde önyükleme hatalarını giderme | Microsoft Docs
description: Azure VM'de BitLocker önyükleme hatalarını giderme hakkında bilgi edinin
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/25/2019
ms.author: genli
ms.openlocfilehash: 95dff39cc0803bc29ab69f5bc78952f89e423473
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762404"
---
# <a name="bitlocker-boot-errors-on-an-azure-vm"></a>BitLocker'ı Azure sanal makinesinde önyükleme hataları

 Bu makalede, Microsoft Azure'da Windows sanal makinesi (VM) başlattığınızda karşılaşabileceğiniz BitLocker hataları açıklanır.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="symptom"></a>Belirti

 Bir Windows VM başlamaz. Ne zaman iade ekran görüntüleri [önyükleme tanılaması](../windows/boot-diagnostics.md) penceresinde, aşağıdaki hata iletilerinden birini görürsünüz:

- BitLocker anahtarı içeren USB sürücüsüne takın.

- Kilitli! Yeniden kullanmaya başlamak için kurtarma anahtarını girin (klavye düzeni: Bilgisayarınıza gizliliğinizi korumak için kilitli olduğu için çok fazla kez oturum açma bilgilerini ABD) yanlış girildi. Kurtarma anahtarını almak için şu adrese gidin https://windows.microsoft.com/recoverykeyfaq başka bir PC veya mobil Cihazınızda. Bu anahtar gerektiği durumlarda XXXXXXX kimliğidir. Veya bilgisayarınızı sıfırlayabilir.

- Bu sürücü [] tuşuna Ekle yazarken parola görmek için kilidini açmak için parolayı girin.
- Kurtarma anahtarınızı yük, Kurtarma anahtarını USB cihazından girin.

## <a name="cause"></a>Nedeni

VM şifreli disk şifresini çözmek için BitLocker kurtarma anahtarı (BEK) dosyası bulamadığında bu sorun oluşabilir.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için durdurmak ve VM'yi serbest bırakın ve yeniden başlatın. Bu işlem, Azure Key Vault'tan BEK dosyasını alın ve ardından şifreli diskte yerleştirin VM'nin zorlar. 

Bu yöntem, çözümleme sorunu varsa, BEK dosyayı el ile geri yüklemek için aşağıdaki adımları izleyin:

1. Etkilenen sanal makinenin sistem diskinin anlık yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).
2. [Bir kurtarma VM'si sistem diski](troubleshoot-recovery-disks-portal-windows.md) BitLocker tarafından şifrelenir. Bunu çalıştırmak için gerekli [yönetme-bde](https://docs.microsoft.com/windows-server/administration/windows-commands/manage-bde) yalnızca BitLocker şifrelenmiş sanal makinede kullanılabilir komutu.

    Yönetilen disk eklediğinizde, bir "şifreleme ayarları içerir ve bu nedenle veri diski olarak kullanılamaz" hata iletisini alabilirsiniz. Bu durumda, diski yeniden denemek için aşağıdaki betiği çalıştırın:

    ```Powershell
    $rgName = "myResourceGroup"
    $osDiskName = "ProblemOsDisk"

    New-AzDiskUpdateConfig -EncryptionSettingsEnabled $false |Update-AzDisk -diskName $osDiskName -ResourceGroupName $rgName

    $recoveryVMName = "myRecoveryVM" 
    $recoveryVMRG = "RecoveryVMRG" 
    $OSDisk = Get-AzDisk -ResourceGroupName $rgName -DiskName $osDiskName;

    $vm = get-AzVM -ResourceGroupName $recoveryVMRG -Name $recoveryVMName 

    Add-AzVMDataDisk -VM $vm -Name $osDiskName -ManagedDiskId $osDisk.Id -Caching None -Lun 3 -CreateOption Attach 

    Update-AzVM -VM $vm -ResourceGroupName $recoveryVMRG
    ```
     Bir blob görüntüsünden geri yüklenen VM'ye yönetilen disk eklenemiyor.

3. Disk bağlandıktan sonra bazı Azure PowerShell betikleri çalıştırabilmek için kurtarma VM'sini Uzak Masaüstü Bağlantısı olun. Sahip olduğunuzdan emin olun [Azure PowerShell'in en son sürümünü](https://docs.microsoft.com/powershell/azure/overview) kurtarma sanal makinesinde yüklü.

4. Yükseltilmiş (yönetici olarak çalıştır) Azure PowerShell oturumu açın. Azure aboneliği için oturum açmak için aşağıdaki komutları çalıştırın:

    ```Powershell
    Add-AzAccount -SubscriptionID [SubscriptionID]
    ```

5. BEK dosyasının adı denetlemek için aşağıdaki betiği çalıştırın:

    ```powershell
    $vmName = "myVM"
    $vault = "myKeyVault"
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.ContentType -match 'BEK')} `
            | Sort-Object -Property Created `
            | ft  Created, `
                @{Label="Content Type";Expression={$_.ContentType}}, `
                @{Label ="Volume"; Expression = {$_.Tags.VolumeLetter}}, `
                @{Label ="DiskEncryptionKeyFileName"; Expression = {$_.Tags.DiskEncryptionKeyFileName}}
    ```

    Örnek çıktı verilmiştir. Bağlı disk BEK dosya adını bulun. Bu durumda, F bağlı disk sürücü harfi olduğu ve EF7B2F5A - 50 C 6-4637-9F13-7F599C12F85C BEK dosyasıdır varsayılır. BEK.

    ```
    Created             Content Type Volume DiskEncryptionKeyFileName               
    -------             ------------ ------ -------------------------               
    4/5/2018 7:14:59 PM Wrapped BEK  C:\    B4B3E070-836C-4AF5-AC5B-66F6FDE6A971.BEK
    4/7/2018 7:21:16 PM Wrapped BEK  F:\    EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    4/7/2018 7:26:23 PM Wrapped BEK  G:\    70148178-6FAE-41EC-A05B-3431E6252539.BEK
    4/7/2018 7:26:26 PM Wrapped BEK  H:\    5745719F-4886-4940-9B51-C98AFABE5305.BEK
    ```

    İki yinelenen birimler görürseniz, daha yeni bir zaman damgası birimi kurtarma VM tarafından kullanılan geçerli BEK dosyasıdır.

    Varsa **içerik türü** değer **sarmalanmış BEK**Git [anahtar şifreleme anahtarı (KEK) senaryoları](#key-encryption-key-scenario).

    Sürücü için BEK dosyasının adı olduğuna göre gizli dosya adı oluşturmanız gerekir. Sürücünün kilidini açmak için BEK dosyası. 

6.  BEK dosya kurtarma diski indirin. Aşağıdaki örnek BEK dosyasını C:\BEK klasörüne kaydeder. Emin olun `C:\BEK\` yolun var olduğundan betikler çalıştırmadan önce.

    ```powershell
    $vault = "myKeyVault"
    $bek = " EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK"
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $bek
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = "C:\BEK\DiskEncryptionKeyFileName.BEK"
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7.  BEK dosyası kullanarak bağlı disk kilidini açmak için aşağıdaki komutu çalıştırın:

    ```powershell
    manage-bde -unlock F: -RecoveryKey "C:\BEK\EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    ```
    Bu örnekte, ekli işletim sistemi diski F. emin doğru sürücü harfini kullandığınız sürücüdür. 

    - Disk başarıyla BEK anahtarı kullanarak kilidi durumunda. Biz BItLocker sorunun çözülmesi için göz önünde bulundurabilirsiniz. 

    - BEK anahtarını kullanarak disk açılmıyorsa kullanabileceğiniz geçici olarak BitLocker aşağıdaki komutu çalıştırarak kapatmak için koruma askıya alma
    
        ```powershell
        manage-bde -protectors -disable F: -rc 0
        ```      
    - Dytem diski kullanarak sanal Makineyi yeniden oluşturmak için kullanacaksanız, sürücü şifresini tam olarak gerekir. Bunu yapmak için aşağıdaki komutu çalıştırın:

        ```powershell
        manage-bde -off F:
        ```
8.  Kurtarma VM'sini diskten ayırma ve ardından sistemi diski olarak etkilenen VM disk yeniden ekleyin. Daha fazla bilgi için [işletim sistemi diskini bir kurtarma sanal Makinesine ekleyerek bir Windows sanal makinesinin sorunlarını giderme](troubleshoot-recovery-disks-windows.md).

### <a name="key-encryption-key-scenario"></a>Anahtar şifreleme anahtarı senaryosu

Anahtar şifreleme anahtarı senaryo için aşağıdaki adımları izleyin:

1. Oturum açmış kullanıcı hesabını anahtar kasası erişim ilkeleri "sarmalanmış halden" izin gerekiyor emin **kullanıcı | Anahtar izinleri | Şifreleme işlemleri | Anahtarı kaydırma**.
2. Kaydetmek için aşağıdaki komut bir. PS1 dosyası:

    ```powershell
    #Set the Parameters for the script
    param (
            [Parameter(Mandatory=$true)]
            [string] 
            $keyVaultName,
            [Parameter(Mandatory=$true)]
            [string] 
            $kekName,
            [Parameter(Mandatory=$true)]
            [string]
            $secretName,
            [Parameter(Mandatory=$true)]
            [string]
            $bekFilePath,
            [Parameter(Mandatory=$true)]
            [string] 
            $adTenant
            )
    # Load ADAL Assemblies. The following script assumes that the Azure PowerShell version you installed is 1.0.0. 
    $adal = "${env:ProgramFiles}\WindowsPowerShell\Modules\Az.Accounts\1.0.0\PreloadAssemblies\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
    $adalforms = "${env:ProgramFiles}\WindowsPowerShell\Modules\Az.Accounts\1.0.0\PreloadAssemblies\Microsoft.IdentityModel.Clients.ActiveDirectory.Platform.dll"
    [System.Reflection.Assembly]::LoadFrom($adal)
    [System.Reflection.Assembly]::LoadFrom($adalforms)

    # Set well-known client ID for AzurePowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2" 
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURI = "https://vault.azure.net"
    # Set Authority to Azure AD Tenant
    $authority = "https://login.windows.net/$adtenant"
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $authResult = $authContext.AcquireTokenAsync($resourceAppIdURI, $clientId, $redirectUri, $platformParameters).result
    # Generate auth header 
    $authHeader = $authResult.CreateAuthorizationHeader()
    # Set HTTP request headers to include Authorization header
    $headers = @{'x-ms-version'='2014-08-01';"Authorization" = $authHeader}

    ########################################################################################################################
    # 1. Retrieve wrapped BEK
    # 2. Make KeyVault REST API call to unwrap the BEK
    # 3. Convert the Base64Url string returned by KeyVault unwrap to Base64 string 
    # 4. Convert Base64 string to bytes and write to the BEK file
    ########################################################################################################################

    #Get wrapped BEK and place it in JSON object to send to KeyVault REST API
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $secretName
    $wrappedBekSecretBase64 = $keyVaultSecret.SecretValueText
    $jsonObject = @"
    {
    "alg": "RSA-OAEP",
    "value" : "$wrappedBekSecretBase64"
    }
    "@

    #Get KEK Url
    $kekUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name $kekName).Key.Kid;
    $unwrapKeyRequestUrl = $kekUrl+ "/unwrapkey?api-version=2015-06-01";

    #Call KeyVault REST API to Unwrap 
    $result = Invoke-RestMethod -Method POST -Uri $unwrapKeyRequestUrl -Headers $headers -Body $jsonObject -ContentType "application/json" -Debug

    #Convert Base64Url string returned by KeyVault unwrap to Base64 string
    $base64UrlBek = $result.value;
    $base64Bek = $base64UrlBek.Replace('-', '+');
    $base64Bek = $base64Bek.Replace('_', '/');
    if($base64Bek.Length %4 -eq 2)
    {
        $base64Bek+= '==';
    }
    elseif($base64Bek.Length %4 -eq 3)
    {
        $base64Bek+= '=';
    }

    #Convert base64 string to bytes and write to BEK file
    $bekFileBytes = [System.Convert]::FromBase64String($base64Bek);
    [System.IO.File]::WriteAllBytes($bekFilePath,$bekFileBytes)
    ```
3. Parametrelerini ayarlayın. Betik BEK anahtarı oluşturmak için KEK gizli dizi ve kurtarma sanal makinesinde yerel bir klasöre kaydedin.

4. Betik başladığında şu çıktıyı görürsünüz:

        GAC    Version        Location                                                                              
        ---    -------        --------                                                                              
        False  v4.0.30319     C:\Program Files\WindowsPowerShell\Modules\AzureRM.profile\4.0.0\Microsoft.Identity...
        False  v4.0.30319     C:\Program Files\WindowsPowerShell\Modules\AzureRM.profile\4.0.0\Microsoft.Identity...

    Betik tamamlandığında aşağıdaki çıktıyı görürsünüz:

        VERBOSE: POST https://myvault.vault.azure.net/keys/rondomkey/<KEY-ID>/unwrapkey?api-
        version=2015-06-01 with -1-byte payload
        VERBOSE: received 360-byte response of content type application/json; charset=utf-8


5. BEK dosyası kullanarak bağlı disk kilidini açmak için aşağıdaki komutu çalıştırın:

    ```powershell
    manage-bde -unlock F: -RecoveryKey "C:\BEK\EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    ```
    Bu örnekte, ekli işletim sistemi diski F. emin doğru sürücü harfini kullandığınız sürücüdür. 

    - Disk başarıyla BEK anahtarı kullanarak kilidi durumunda. Biz BItLocker sorunun çözülmesi için göz önünde bulundurabilirsiniz. 

    - BEK anahtarını kullanarak disk açılmıyorsa kullanabileceğiniz geçici olarak BitLocker aşağıdaki komutu çalıştırarak kapatmak için koruma askıya alma
    
        ```powershell
        manage-bde -protectors -disable F: -rc 0
        ```      
    - Dytem diski kullanarak sanal Makineyi yeniden oluşturmak için kullanacaksanız, sürücü şifresini tam olarak gerekir. Bunu yapmak için aşağıdaki komutu çalıştırın:

        ```powershell
        manage-bde -off F:
        ```

6. Kurtarma VM'sini diskten ayırma ve ardından sistemi diski olarak etkilenen VM disk yeniden ekleyin. Daha fazla bilgi için [işletim sistemi diskini bir kurtarma sanal Makinesine ekleyerek bir Windows sanal makinesinin sorunlarını giderme](troubleshoot-recovery-disks-windows.md).
