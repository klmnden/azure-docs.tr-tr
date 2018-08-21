---
title: Yerel aracı dağıtma ve görüntü sanal makineleri bir hizmet olarak Azure Stack doğrulama test | Microsoft Docs
description: Yerel aracı dağıtma ve görüntü sanal makineleri bir hizmet olarak Azure Stack doğrulama için test edin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 9d32c23f66563988d023df3bf6a33efa30237e57
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235493"
---
# <a name="deploy-the-local-agent-and-test-virtual-machines"></a>Yerel aracısı ve test sanal makineleri dağıtma

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Doğrulama donanımınız denetlemek için hizmet (VaaS) yerel aracı kullanmayı öğrenin. Yerel aracı doğrulama testlerini çalıştırmadan önce doğrulanması gereken Azure Stack çözüm dağıtılması gerekir.

> [!Note]  
> Yerel aracı çalıştığı bilgisayarın Internet'e erişimini kaybeder değil emin olmanız gerekir. Makine, yalnızca Azure Stack VaaS kullanma yetkisi olan kullanıcılar için erişilebilir olmalıdır.

Sanal makinelerinizi test etmek için:

1. Yerel aracı yükleme
2. Hataları, sisteme ekleme
3. Yerel aracı çalıştırın

## <a name="download-and-start-the-local-agent"></a>İndirin ve yerel Aracısı'nı başlatın

Azure Stack sistemi, ancak tüm Azure Stack Uç noktalara erişimi olan bir parçası değil, veri merkezinizdeki önkoşullarını karşılayan bir makine için aracıyı indirin.

### <a name="machine-prerequisites"></a>Makine önkoşulları

Makinenizde aşağıdaki ölçütleri karşıladığından emin olun:

- Tüm Azure Stack uç noktalarına erişimi
- .NET 4.6 ve PowerShell 5.0 yüklü
- En az 8 GB RAM
- En az 8 çekirdek işlemcileri
- En az 200 GB disk alanı
- İnternet'e kararlı ağ bağlantısı

Azure Stack, test altındaki sistemidir. Makine, Azure Stack parçası olmamalıdır veya Azure Stack bulut üzerinde barındırılan.

### <a name="download-and-install-the-agent"></a>Aracısını indirme ve yükleme

1. Testleri çalıştırmak için kullanacağınız makinede yükseltilmiş isteminde Windows PowerShell'i açın.
2. Yerel aracı indirmek için aşağıdaki komutu çalıştırın:

    ```PowerShell  
        Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.3.2.0.nupkg" -outfile "OnPremAgent.zip"
        Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent.3.2.0 -Force
        Set-Location VaaSOnPremAgent.3.2.0\lib\net46
    ````

3. Yerel aracı bağımlılıkları yüklemek için aşağıdaki komutu çalıştırın:

    ```PowerShell  
        $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-VaaSPrerequisites -AadTenantId <AadTenantId> `
        -ServiceAdminCreds <ServiceAdminCreds> `
        -ArmEndpoint https://adminmanagement.<ExternalFqdn> `
        -Region <Region>
    ````

    **Parametreler**

    | Parametre | Açıklama |
    | --- | --- |
    | aadServiceAdminUser | Azure AD kiracınız için genel yönetici kullanıcı. Örneğin olabilir, vaasadmin@contoso.onmicrosoft.com. |
    | aadServiceAdminPassword | Genel yönetici kullanıcı parolası. |
    | AadTenantId | Azure hesabı için Azure AD Kiracı kimlik doğrulama hizmet olarak kayıtlı. |
    | ExternalFqdn | Tam etki alanı adı yapılandırma dosyasından alabilirsiniz. Yönergeler için bkz [Test parametreleri için Azure Stack hizmet olarak doğrulama](azure-stack-vaas-parameters-test.md). |
    | Bölge | Azure AD kiracınızın bölge. |

Komut bir ortak görüntü deposu (PIR) görüntü (işletim sistemi VHD'si) ve kopyalama, bir Azure blob depolama alanından Azure Stack depolama indirir. 

![Önkoşulları indirin](media/installingprereqs.png)

> [!Note]  
> Bu görüntüleri indirirken yavaş ağ hızı karşılaşıyorsanız, bunları ayrı ayrı bir yerel paylaşıma indirin ve parametresini **- LocalPackagePath** *FileShareOrLocalPath*. Daha fazla rehberliğe PIR indirmenizin bölümünde bulabilirsiniz [tanıtıcı yavaş ağ bağlantısı](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) , [doğrulama hizmet olarak sorun giderme](azure-stack-vaas-troubleshoot.md).

## <a name="fault-injection"></a>Hata ekleme

Microsoft Azure Stack birden çok yazılım ve donanım hatalarının Tolerans ve esneklik için tasarlanmıştır. Hata ekleme sistemde hataları oranını artırır. Bu artış sistemi kapatmaya olayların sayısını azaltabilir, sorunları daha önce ortaya çıkarmaya yardımcı olur.

Hataları, sisteme ekleme için aşağıdaki komutları çalıştırın.

1. Windows PowerShell'i yükseltilmiş isteminde açın.

2. Şu komutu çalıştırın:

    ```PowerShell  
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-ServiceFabricSDK Install-ServiceFabricSDK
    ```

## <a name="run-the-agent"></a>Aracıyı çalıştırın

1. Windows PowerShell'i yükseltilmiş isteminde açın.

2. Şu komutu çalıştırın:

    ````PowerShell  
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ````

      **Parametreler**  
    
    | Parametre | Açıklama |
    | --- | --- |
    | VaaSUserId | Kullanıcı kimliği kullanılan VaaS portalda oturum açmak için (örneğin, UserName@Contoso.com) |
    | VaaSTenantId | Azure hesabı için Azure AD Kiracı kimlik doğrulama hizmet olarak kayıtlı. |

    > [!Note]  
    > Aracı çalıştırdığınızda, geçerli çalışma dizinine yürütülebilir, Görev Altyapısı ana konumu olmalıdır **Microsoft.VaaSOnPrem.TaskEngineHost.exe.**

Hataları bildirilen görmüyorsanız, yerel aracı başarılı oldu. Aşağıdaki örnek metni konsol penceresinde görünür.

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Başlatılan aracı](media/startedagent.png)

Bir aracıyı adlarıyla benzersiz olarak tanımlanır. Varsayılan olarak, burada başlatıldığından makinenin tam etki alanı adı (FQDN) kullanır. Herhangi bir pencere olarak yanlışlıkla tıklar önlemek için pencerenin en aza odağı değiştirilmesi, diğer tüm eylemler duraklatır.

## <a name="next-steps"></a>Sonraki adımlar

- [Yeni bir Azure Stack çözüm doğrula](azure-stack-vaas-validate-solution-new.md)  
- Yavaş veya aralıklı Internet bağlantısı varsa, PIR görüntü indirebilirsiniz. Daha fazla bilgi için [tanıtıcı yavaş ağ bağlantısı](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity).
- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).
