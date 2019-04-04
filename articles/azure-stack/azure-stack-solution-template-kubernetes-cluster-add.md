---
title: Kubernetes için Azure Stack Marketini ekleyin | Microsoft Docs
description: Kubernetes için Azure Stack Marketini eklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: cf831c6f8faad1892291794bc43dc13e6a17eba1
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484857"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Kubernetes için Azure Stack Marketini Ekle

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu. Azure Stack bağlantısı kesilmiş senaryo preview tarafından şu anda desteklenmiyor.

Kullanıcılarınız için bir Market öğesi Kubernetes sunabilir. Kullanıcılarınızın ardından, Kubernetes içinde tek ve eşgüdümlü bir işlemle dağıtabilir.

Şu makaleye bakın dağıtmak ve tek başına bir Kubernetes kümesi için kaynakları sağlamak için bir Azure Resource Manager şablonu kullanarak. Başlamadan önce Azure Stack ve Azure genel Kiracı ayarlarını kontrol edin. Azure Stack hakkında gerekli bilgileri toplayın. Gerekli kaynakları kiracınız ve Azure Stack Marketini ekleyin. Bir Ubuntu sunucusu, özel komut dosyası ve Kubernetes küme Market öğeyi Market'te küme bağlıdır.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Bir plan, teklif ve bir abonelik oluşturun

Bir plan, teklif ve Kubernetes Market öğesi için bir abonelik oluşturun. Ayrıca, var olan bir planı kullanın ve sunar.

1. Oturum [Yönetim Portalı.](https://adminportal.local.azurestack.external)

1. Temel plan bir plan oluşturun. Yönergeler için [Azure Stack'te plan oluşturma](azure-stack-create-plan.md).

1. Bir teklif oluşturun. Yönergeler için [Azure Stack'te teklif oluşturma](azure-stack-create-offer.md).

1. Seçin **sunar**ve oluşturduğunuz teklif bulun.

1. Seçin **genel bakış** teklif dikey penceresinde.

1. Seçin **durumunu değiştir**. Seçin **genel**.

1. Seçin **+ kaynak Oluştur** > **sunar ve planları** > **abonelik** bir abonelik oluşturmak için.

    a. Girin bir **görünen ad**.

    b. Girin bir **kullanıcı**. Kiracınız ile ilişkili Azure AD hesabını kullanın.

    c. **Sağlayıcısı açıklaması**

    d. Ayarlama **dizin Kiracı** Azure AD kiracınız için Azure Stack için. 

    e. Seçin **teklif**. Oluşturduğunuz teklif adını seçin. Abonelik kimliği not edin

## <a name="create-a-service-principal-and-credentials-in-ad-fs"></a>AD FS'de bir hizmet sorumlusu ve kimlik bilgileri oluşturma

Kimlik Yönetimi hizmetiniz için Active Directory Federasyon Hizmetleri'nde (AD FS) kullanıyorsanız, bir hizmet sorumlusu kullanıcıların bir Kubernetes kümesini dağıtırken oluşturmak gerekir.

1. Oluşturun ve hizmet sorumlusu oluşturmak için kullanılan otomatik olarak imzalanan bir sertifika verin. 

    - Şu bilgilere ihtiyacınız vardır:

       | Değer | Açıklama |
       | ---   | ---         |
       | Parola | Sertifika için yeni bir parola girin. |
       | Yerel sertifika yolu | Sertifika yolu ve dosya adını girin. Örneğin, `c:\certfilename.pfx` |
       | Sertifika adı | Sertifika adını girin. |
       | Sertifika depolama konumu |  Örneğin, `Cert:\LocalMachine\My` |

    - PowerShell ile yükseltilmiş istemi açın. Değerlerinizi güncelleştirilmiş parametrelerle birlikte aşağıdaki betiği çalıştırın:

        ```powershell  
        # Creates a new self signed certificate 
        $passwordString = "<password>"
        $certlocation = "<local certificate path>.pfx"
        $certificateName = "CN=<certificate name>"
        $certStoreLocation="<certificate store location>"
        
        $params = @{
        CertStoreLocation = $certStoreLocation
        DnsName = $certificateName
        FriendlyName = $certificateName
        KeyLength = 2048
        KeyUsageProperty = 'All'
        KeyExportPolicy = 'Exportable'
        Provider = 'Microsoft Enhanced Cryptographic Provider v1.0'
        HashAlgorithm = 'SHA256'
        }
        
        $cert = New-SelfSignedCertificate @params -ErrorAction Stop
        Write-Verbose "Generated new certificate '$($cert.Subject)' ($($cert.Thumbprint))." -Verbose
        
        #Exports certificate with password in a .pfx format
        $pwd = ConvertTo-SecureString -String $passwordString -Force -AsPlainText
        Export-PfxCertificate -cert $cert -FilePath $certlocation -Password $pwd
        ```

2.  PowerShell oturumunuzda, görüntülenen yeni sertifika kimliği Not `1C2ED76081405F14747DC3B5F76BB1D83227D824`. Kimliğinde hizmet sorumlusu oluştururken kullanılacak.

    ```powershell  
    VERBOSE: Generated new certificate 'CN=<certificate name>' (1C2ED76081405F14747DC3B5F76BB1D83227D824).
    ```

3. Hizmet sorumlusu sertifikasını kullanarak oluşturun.

    - Şu bilgilere ihtiyacınız vardır:

       | Değer | Açıklama                     |
       | ---   | ---                             |
       | ERCS IP | ASDK normalde ayrıcalıklı uç noktadır `AzS-ERCS01`. |
       | Uygulama adı | Uygulama hizmet sorumlusu için basit bir ad girin. |
       | Sertifika depolama konumu | Bilgisayarınızdaki sertifika depoladığınız yolu. Bu depolama konumu belirtilir ve sertifika kimliği ilk adımda oluşturulur. Örneğin, `Cert:\LocalMachine\My\1C2ED76081405F14747DC3B5F76BB1D83227D824` |

       İstendiğinde, ayrıcalık uç noktaya bağlanmak için aşağıdaki kimlik bilgilerini kullanın. 
        - Kullanıcı adı: CloudAdmin hesabı, belirttiğiniz biçimde <Azure Stack domain>\cloudadmin. (ASDK için kullanıcı azurestack\cloudadmin adıdır.)
        - Parola: AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.

    - Değerlerinizi güncelleştirilmiş parametrelerle birlikte aşağıdaki betiği çalıştırın:

        ```powershell  
        #Create service principal using the certificate
        $privilegedendpoint="<ERCS IP>"
        $applicationName="<application name>"
        $certStoreLocation="<certificate location>"
        
        # Get certificate information
        $cert = Get-Item $certStoreLocation
        
        # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
        $creds = Get-Credential

        # Creating a PSSession to the ERCS PrivilegedEndpoint
        $session = New-PSSession -ComputerName $privilegedendpoint -ConfigurationName PrivilegedEndpoint -Credential $creds

        # Get Service principal Information
        $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name "$using:applicationName" -ClientCertificates $using:cert}

        # Get Stamp information
        $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }

        # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. This is read from the AzureStackStampInformation output of the ERCS VM.
        $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

        # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. This is read from the AzureStackStampInformation output of the ERCS VM.
        $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

        # TenantID for the stamp. This is read from the AzureStackStampInformation output of the ERCS VM.
        $TenantID = $AzureStackInfo.AADTenantID

        # Register an AzureRM environment that targets your Azure Stack instance
        Add-AzureRMEnvironment `
        -Name "AzureStackUser" `
        -ArmEndpoint $ArmEndpoint

        # Set the GraphEndpointResourceId value
        Set-AzureRmEnvironment `
        -Name "AzureStackUser" `
        -GraphAudience $GraphAudience `
        -EnableAdfsAuthentication:$true
        Add-AzureRmAccount -EnvironmentName "azurestackuser" `
        -ServicePrincipal `
        -CertificateThumbprint $ServicePrincipal.Thumbprint `
        -ApplicationId $ServicePrincipal.ClientId `
        -TenantId $TenantID

        # Output the SPN details
        $ServicePrincipal
        ```

    - Aşağıdaki kod parçacığında gibi hizmet sorumlusu ayrıntıları bakın

        ```Text  
        ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
        ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
        Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
        ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
        PSComputerName        : azs-ercs01
        RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
        ```

## <a name="add-an-ubuntu-server-image"></a>Ubuntu server resim ekleme

Ubuntu Server aşağıda Market'te ekleyin:

1. Oturum [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. Seçin **tüm hizmetleri**ve ardından altındaki **Yönetim** kategorisi, select **Market Yönetim**.

1. Seçin **+ Azure'dan Ekle**.

1. `Ubuntu Server` yazın.

1. Sunucu en yeni sürümünü seçin. Tam sürümünü denetleyin ve en yeni sürümüne sahip olduğunuzdan emin olun:
    - **Yayımcı**: Canonical
    - **Teklif**: UbuntuServer
    - **Sürüm**: 16.04.201806120 (veya en son sürüm)
    - **SKU**: 16.04-LTS

1. Seçin **indirin.**

## <a name="add-a-custom-script-for-linux"></a>Linux için özel bir komut dosyası Ekle

Kubernetes marketten ekleyin:

1. Açık [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. Seçin **tüm hizmetleri** altındaki **Yönetim** kategorisi, select **Market Yönetim**.

1. Seçin **+ Azure'dan Ekle**.

1. `Custom Script for Linux` yazın.

1. Aşağıdaki profil komut dosyasını seçin:
   - **Teklif**: Linux 2.0 için özel betik
   - **Sürüm**: 2.0.6 (veya en son sürüm)
   - **Yayımcı**: Microsoft Corp

     > [!Note]  
     > Linux için özel betik birden fazla sürümünü listelenebilir. Öğenin son sürümü eklemeniz gerekir.

1. Seçin **indirin.**


## <a name="add-kubernetes-to-the-marketplace"></a>Kubernetes Market'te Ekle

1. Açık [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. Seçin **tüm hizmetleri** altındaki **Yönetim** kategorisi, select **Market Yönetim**.

1. Seçin **+ Azure'dan Ekle**.

1. `Kubernetes` yazın.

1. `Kubernetes Cluster` öğesini seçin.

1. Seçin **indirin.**

    > [!note]  
    > Bu Market öğesi Market'te görünmesi için beş dakika sürebilir.

    ![Kubernetes](user/media/azure-stack-solution-template-kubernetes-deploy/marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Güncelleştirme veya Kubernetes kaldırma 

Kubernetes öğesi güncelleştirilirken, önceki öğeyi Market'te kaldırırsınız. Market'te Kubernetes güncelleştirme eklemek için bu makalede verilen yönergeleri izleyin.

Kubernetes öğeyi kaldırmak için:

1. PowerShell ile Azure Stack operatör bağlanın. Yönergeler için bkz [Azure Stack operatör olarak PowerShell ile bağlanma](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-admin).

2. Galerideki geçerli Kubernetes kümesi öğeyi bulur.

    ```powershell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. Geçerli öğenin adını gibi unutmayın `Microsoft.AzureStackKubernetesCluster.0.3.0`

4. Öğeyi kaldırmak için aşağıdaki PowerShell cmdlet'ini kullanın:

    ```powershell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Bir Kubernetes için Azure Stack dağıtma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

[Azure stack'teki hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md)
