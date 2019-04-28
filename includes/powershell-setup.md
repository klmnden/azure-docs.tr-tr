---
services: virtual-machines
title: PowerShell ayarlama
author: JoeDavies-MSFT
solutions: ''
manager: timlt
editor: tysonn
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/12/2015
ms.author: rasquill
ms.openlocfilehash: b96e8e6e31817f6d261f41dbf3b3047dd49c29ba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485431"
---
## <a name="setting-up-powershell"></a>PowerShell ayarlama
Azure PowerShell kullanabilmeniz için şu adımları izleyin.

### <a name="verify-powershell-versions"></a>PowerShell sürümleri doğrulayın
Windows PowerShell kullanmadan önce Windows PowerShell sürüm 3.0 veya 4.0 olması gerekir. Windows PowerShell sürümünü bulmak için Windows PowerShell komut isteminde bu komutu yazın.

    $PSVersionTable

Bunun gibi bir şey görmeniz gerekir.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Doğrulayın değerini **PSVersion** 3.0 veya 4.0. Uyumlu bir sürümünü yüklemek için bkz: [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) veya [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855).

Ayrıca Azure PowerShell sürümü 0.8.0 olmalıdır veya üzeri. Azure PowerShell, Azure PowerShell komut isteminde bu komutu ile yüklü olan sürümü denetleyebilirsiniz.

    Get-Module azure | format-table version

Bunun gibi bir şey görmeniz gerekir.

    Version
    -------
    0.8.16.1

Yönergeler ve en son sürüme bir bağlantı için bkz. [yükleme ve yapılandırma, Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="set-your-azure-account-and-subscription"></a>Azure hesabınızı ve aboneliğinizi ayarlama
Azure aboneliğiniz yoksa, etkinleştirebilir, [MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya kaydolun bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).

Bir Azure PowerShell komut istemi açın ve bu komutla Azure'da oturum açın.

    Add-AzureAccount

Birden çok Azure aboneliğiniz varsa, bu komut Azure aboneliklerinizle listeleyebilirsiniz.

    Get-AzureSubscription

Aşağıdaki türde bilgiler alırsınız:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Azure PowerShell komut isteminde şu komutları çalıştırarak geçerli Azure aboneliğinizi ayarlayabilirsiniz. Dahil olmak üzere, tırnak işaretleri içindeki her şeyi değiştirin < ve > karakterleri, doğru ada sahip.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current    

Azure aboneliklerini ve hesaplarını hakkında daha fazla bilgi için bkz: [nasıl yapılır: Aboneliğinize bağlanma](/powershell/azureps-cmdlets-docs#Connect).

