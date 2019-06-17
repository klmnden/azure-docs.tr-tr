---
title: Yerleşik Azure Güvenlik Merkezi için PowerShell kullanın ve ağınızı korumanıza | Microsoft Docs
description: Bu belge ekleme Azure Güvenlik Merkezi'nin PowerShell cmdlet'lerini kullanarak sürecinde yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/2/2018
ms.author: rkarlin
ms.openlocfilehash: 73043680ea7b8b63a329d0a457449b635b7b80f2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60703600"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>PowerShell kullanarak Azure Güvenlik Merkezi'nin ekleme otomatikleştirin

Azure Güvenlik Merkezi PowerShell modülünü kullanarak Azure iş yüklerinizi programlı olarak güvenliğini sağlayabilirsiniz.
PowerShell kullanarak, görevleri otomatikleştirme ve el ile gerçekleştirilen görevleri devralınan İnsan hatası kaçınmak sağlar. Onlarca, yüzlerce ve binlerce kaynakların her biri en baştan korunmalıdır – aboneliklerle gerektiren büyük ölçekli dağıtımlar bu durum özellikle faydalıdır.

PowerShell kullanarak ekleme Azure Güvenlik Merkezi, programlı olarak ekleme ve Azure kaynaklarınızı yönetimini otomatikleştirmek ve gerekli güvenlik denetimleri eklemek sağlar.

Bu makalede, değiştirilebilir ve ortamınızda Güvenlik Merkezi'ne gözatın aboneliklerinizde geri almak için kullanılan bir örnek PowerShell Betiği sağlanır. 

Bu örnekte Güvenlik Merkezi kimliği bir Abonelikteki etkinleştiririz: d07c0080-170c-4c24-861d-9c817742786c ve yüksek düzeyde koruma sağlayan Güvenlik Merkezi standart katmanı uygulayarak sağlayan önerilen ayarları uygula Gelişmiş tehdit koruması ve algılaması özellikleri:

1. Ayarlama [ASC standart koruma düzeyini](https://azure.microsoft.com/pricing/details/security-center/). 
 
2. Mevcut bir kullanıcı tanımlı çalışma (myWorkspace) Vm'lerinde topladığı – Bu örnekte, aboneliği ile ilişkili olduğu Microsoft Monitoring Agent gönderir Log Analytics çalışma alanını ayarlayın.

3. Güvenlik Merkezi'nin otomatik aracı, sağlamayı etkinleştirmek [Microsoft Monitoring Agent'ı dağıtır](security-center-enable-data-collection.md#auto-provision-mma).

5. Kuruluşun ayarlamak [ASC uyarıları ve önemli olayları güvenlik ilgili kişisi olarak CISO](security-center-provide-security-contact-details.md).

6. Ata Güvenlik Merkezi'nin [varsayılan güvenlik ilkeleri](tutorial-security-policy.md).

## <a name="prerequisites"></a>Önkoşullar

Güvenlik Merkezi cmdlet'leri çalıştırmadan önce bu adımlar gerçekleştirilmelidir:

1.  PowerShell'i yönetici olarak çalıştırın
2.  PowerShell'de aşağıdaki komutları çalıştırın:
      
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Install-Module -Name Az.Security -Force

## <a name="onboard-security-center-using-powershell"></a>PowerShell kullanarak Güvenlik Merkezi'ne ekleme

1.  Abonelikleriniz için Güvenlik Merkezi kaynak sağlayıcısını kaydedin:

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Register-AzResourceProvider -ProviderNamespace 'Microsoft.Security' 

2.  İsteğe bağlı: Abonelik kapsamı düzeyini (fiyatlandırma katmanı) ayarlayın (tanımlı değilse, fiyatlandırma katmanı ücretsiz olarak ayarlanır):

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Set-AzSecurityPricing -Name "default" -PricingTier "Standard"

3.  Aracıların rapor eder Log Analytics çalışma alanı yapılandırın. Zaten oluşturduğunuz aboneliğin Vm'leri rapor vereceği bir Log Analytics çalışma alanı olması gerekir. Birden çok abonelik aynı çalışma alanına raporlama yapacak tanımlayabilirsiniz. Tanımlı değilse, varsayılan çalışma alanı kullanılır.

        Set-AzSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  Microsoft Monitoring Agent'ı Azure sanal makinelerinize otomatik sağlama yüklemesini:
    
        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
    
        Set-AzSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > Azure sanal makinelerinizi otomatik olarak Azure Güvenlik Merkezi tarafından korunduğundan emin olmak otomatik sağlamayı etkinleştirmek için önerilir.
    >

5.  İsteğe bağlı: Güvenlik ilgili kişisinin bilgilerini ekleme, uyarılar ve bildirimler alıcılarını kullanılacak oluşturduğunuz abonelikler için tanımladığınız Güvenlik Merkezi tarafından önerilir:

        Set-AzSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertAdmin -NotifyOnAlert 

6.  Varsayılan Güvenlik Merkezi ilke girişimi atama:

        Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzPolicySetDefinition | where {$_.Properties.displayName -EQ '[Preview]: Enable Monitoring in Azure Security Center'}
        New-AzPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'

Başarıyla eklenen Azure Güvenlik Merkezi artık PowerShell ile!

Şimdi, program aracılığıyla abonelikler ve kaynaklar arasında yineleme yapmak için Otomasyon betikleri ile bu PowerShell cmdlet'lerini kullanabilirsiniz. Bu, zaman tasarrufu sağlar ve İnsan hatası olasılığını azaltır. Bu [örnek komut dosyası](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) başvuru olarak.






## <a name="see-also"></a>Ayrıca bkz.
Güvenlik Merkezi'ne ekleme otomatikleştirmek için PowerShell nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için şu makaleye bakın:

* [Az.Security](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Security/Commands.Security/help/Az.Security.md).

Güvenlik Merkezi hakkında daha fazla bilgi için şu makaleye bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
