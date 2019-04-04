---
title: Yerel aracı dağıtma | Microsoft Docs
description: Azure Stack doğrulama için yerel aracı bir hizmet olarak dağıtın.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: cfea454b20b010148eba063ec724e55134944ac3
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482938"
---
# <a name="deploy-the-local-agent"></a>Yerel aracı dağıtma

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Doğrulama Hizmeti (VaaS) yerel aracı, doğrulama testlerini çalıştırmak için kullanmayı öğrenin. Yerel aracı doğrulama testlerini çalıştırmadan önce dağıtılmalıdır.

> [!Note]  
> Yerel aracı üzerinde çalıştığı makinenin internet'e giden erişimini kaybeder değil emin olun. Bu makine VaaS adına kiracınızı kullanmak için yetkili kullanıcılar tarafından erişilebilir olmalıdır.

Yerel aracı dağıtmak için:

1. Yerel aracıyı yükleyin.
2. Sağlamlık denetimleri gerçekleştirin.
3. Yerel aracı çalıştırın.

## <a name="download-and-start-the-local-agent"></a>İndirin ve yerel Aracısı'nı başlatın

Veri merkezinizde önkoşulları karşıladığını ve tüm Azure Stack uç noktalarına erişimi olan bir makine için aracıyı indirin. Bu makine Azure Stack sisteminin parçası olmamalıdır veya Azure Stack bulut üzerinde barındırılan.

### <a name="machine-prerequisites"></a>Makine önkoşulları

Makinenizde aşağıdaki ölçütleri karşıladığından emin olun:

- Tüm Azure Stack uç noktalarına erişimi
- .NET 4.6 ve PowerShell 5.0 yüklü
- En az 8 GB RAM
- En az 8 çekirdek işlemcileri
- En az 200 GB disk alanı
- İnternet'e kararlı ağ bağlantısı

### <a name="download-and-install-the-agent"></a>Aracısını indirme ve yükleme

1. Testleri çalıştırmak için kullanacağınız makinede yükseltilmiş isteminde Windows PowerShell'i açın.
2. Yerel aracı indirmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.latest.nupkg" -outfile "OnPremAgent.zip"
    Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent -Force
    Set-Location VaaSOnPremAgent\lib\net46
    ```

3. Yerel aracı bağımlılıkları yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

    **Parametreler**

    | Parametre | Açıklama |
    | --- | --- |
    | aadServiceAdminUser | Azure AD kiracınız için genel yönetici kullanıcı. Örneğin olabilir, vaasadmin@contoso.onmicrosoft.com. |
    | aadServiceAdminPassword | Genel yönetici kullanıcı parolası. |
    | AadTenantId | Azure hesabı için Azure AD Kiracı kimlik doğrulama hizmet olarak kayıtlı. |
    | ExternalFqdn | Tam etki alanı adı yapılandırma dosyasından alabilirsiniz. Yönergeler için bkz [hizmet olarak Azure Stack doğrulama iş akışı ortak parametrelerinin](azure-stack-vaas-parameters.md). |
    | Bölge | Azure AD kiracınızın bölge. |

Komut bir ortak görüntü deposu (PIR) görüntü (işletim sistemi VHD'si) ve kopyalama, bir Azure blob depolama alanından Azure Stack depolama indirir.

![Önkoşulları indirin](media/installingprereqs.png)

> [!Note]
> Bu görüntüleri indirirken yavaş ağ hızı karşılaşıyorsanız, bunları ayrı ayrı bir yerel paylaşıma indirin ve parametresini **- LocalPackagePath** *FileShareOrLocalPath*. Daha fazla rehberliğe PIR indirmenizin bölümünde bulabilirsiniz [tanıtıcı yavaş ağ bağlantısı](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) , [hizmet olarak sorun giderme doğrulama](azure-stack-vaas-troubleshoot.md).

## <a name="checks-before-starting-the-tests"></a>Testlere başlamadan önce denetimleri

Uzak operations testleri çalıştırın. Testleri çalıştıran makine Azure Stack uç noktalarına erişime sahip olmalıdır, aksi takdirde testleri çalışmaz. VaaS yerel aracı kullanıyorsanız, aracı çalıştıracağınız makinenin kullanın. Aşağıdaki denetimleri çalıştırarak makinenizi Azure Stack Uç noktalara erişimi olduğunu denetleyebilirsiniz:

1. Taban URI erişilebildiğini kontrol edin. Bir komut istemi veya bash kabuğu ve aşağıdaki komutu çalıştırın komutuyla değiştirerek `<EXTERNALFQDN>` ortamınızın dış FQDN ile:

    ```bash
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Bir web tarayıcısı açın ve gidin `https://adminportal.<EXTERNALFQDN>` MAS portalı erişilebildiğini denetlemek için.

3. Azure AD kullanarak oturum açar, test geçiş oluştururken girdiğiniz yönetici adı ve parola değerlerini hizmeti.

4. Sistem durumu denetleyin **Test AzureStack** açıklandığı gibi PowerShell cmdlet'i [Azure Stack için bir doğrulama sınamasını çalıştırmanızı](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test). Herhangi bir test başlatmadan önce tüm uyarıları ve hataları düzeltin.

## <a name="run-the-agent"></a>Aracıyı çalıştırın

1. Windows PowerShell'i yükseltilmiş isteminde açın.

2. Şu komutu çalıştırın:

    ```powershell
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ```

      **Parametreler**  

    | Parametre | Açıklama |
    | --- | --- |
    | VaaSUserId | Kullanıcı kimliği kullanılan VaaS portalda oturum açmak için (örneğin, kullanıcı adı\@Contoso.com) |
    | VaaSTenantId | Azure hesabı için Azure AD Kiracı kimlik doğrulama hizmet olarak kayıtlı. |

    > [!Note]  
    > Aracı çalıştırdığınızda, geçerli çalışma dizinine yürütülebilir, Görev Altyapısı ana konumu olmalıdır **Microsoft.VaaSOnPrem.TaskEngineHost.exe.**

Hataları bildirilen görmüyorsanız, yerel aracı başarılı oldu. Aşağıdaki örnek metni konsol penceresinde görünür.

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Başlatılan aracı](media/startedagent.png)

Bir aracıyı adlarıyla benzersiz olarak tanımlanır. Varsayılan olarak, burada başlatıldığından makinenin tam etki alanı adı (FQDN) kullanır. Tüm yanlışlıkla seçer penceresinde önlemek için pencerenin en aza odağı değiştirilmesi, diğer tüm eylemler duraklatır.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet olarak doğrulama sorunlarını giderme](azure-stack-vaas-troubleshoot.md)
- [Hizmet temel kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md)
- [Hızlı Başlangıç: Doğrulama, ilk test zamanlamak için bir servis portalı kullanın.](azure-stack-vaas-schedule-test-pass.md)