---
title: " Otomasyon runbook'ları ile Azure VM uyarıları düzeltmek | Microsoft Docs"
description: "Bu makalede, Azure sanal makine uyarıları Azure Otomasyon çalışma kitabı ile tümleştirmek ve sorunları otomatik olarak düzeltmek gösterilmiştir"
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: tysonn
ms.assetid: 1f7baa7f-7283-4a4f-9385-3f5cd1062c7f
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: csand;magoedte
ms.openlocfilehash: 18cccc88ab74235722e2f4886671fc483ab67da8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a>Azure otomasyonu senaryosu - Azure VM uyarıları Düzelt
Azure Otomasyonu ve Azure sanal makineleri Otomasyon runbook'ları çalıştırmak için sanal makine (VM) uyarıları yapılandırmanıza olanak sağlayan yeni bir özellik yayımlandı. Bu yeni özellik otomatik olarak yeniden başlatma veya durdurma VM gibi VM uyarılara yanıt olarak standart düzeltme gerçekleştirmenize olanak sağlar.

Daha önce VM uyarı kuralı oluşturma sırasında yapabileceksiniz [Otomasyonu Web kancası belirtin](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) bir runbook uyarıyı tetikleyen her runbook'u çalıştırmak için. Ancak, bu runbook oluşturma, runbook için Web kancası oluşturma ve ardından kopyalama ve Web kancası uyarı kuralı oluşturma sırasında yapıştırma işlemlerini yapmak için gereklidir. Bu yeni sürüm ile işlemi, doğrudan bir runbook bir listeden uyarı kuralı oluşturma sırasında seçebilir ve runbook çalışan bir Otomasyon hesabı seçin ya da kolayca bir hesap oluşturmak için çok daha kolaydır.

Bu makalede, bir Azure VM uyarısı ayarla ve uyarıyı tetikleyen getirildiğinde çalışmak için bir Otomasyon runbook yapılandırma ne kadar kolay olduğunu gösteriyoruz. Örnek senaryolar, bellek kullanımı bir uygulama bellek sızıntısına VM'ye nedeniyle bazı eşiğini aştığında bir VM'yi yeniden başlatırken veya CPU kullanıcı süresi %1 son bir saat boyunca ve kullanımda olmadığında bir VM durdurma içerir. Ayrıca bir hizmet sorumlusu Otomasyon hesabınızda otomatik oluşturulmasını Azure uyarı düzeltme runbook'larda kullanımını nasıl basitleştirir açıklayacağız.

## <a name="create-an-alert-on-a-vm"></a>Bir VM üzerinde bir uyarı oluşturabilir.
Eşiğini sağlandığında bir runbook'u başlatmak için bir uyarı yapılandırmak için aşağıdaki adımları gerçekleştirin.

> [!NOTE]
> Bu sürümle birlikte, yalnızca V2 sanal makineler ve destek için Klasik VM'ler yakında eklenecek destekliyoruz.  
> 
> 

1. Azure portalında oturum açın ve tıklatın **sanal makineleri**.  
2. Sanal makinelerinizi birini seçin.  
3. VM ekranında içinde **izleme** 'yi tıklatın **uyarı kuralları**.
4. Üzerinde **uyarı kuralları** bölmesinde tıklatın **uyarı Ekle**.

Bu açılır **uyarı kuralı eklemek** burada uyarı için koşulları yapılandırmanızı ve bir ya da tüm bu seçenekler arasında seçin sayfası: e-posta göndermek için başka bir sisteme uyarı iletmek ve/veya çalıştırmak için bir Web kancası kullanın bir Otomasyon runbook yanıt denemek sorunu düzeltmek.

## <a name="configure-a-runbook"></a>Bir runbook yapılandırma
VM uyarı eşiği karşılandığında çalıştırmak için bir runbook yapılandırmak için seçin **Otomasyon Runbook'u**. İçinde **runbook yapılandırma** bölmesinde runbook'un çalıştırılmasına ve runbook'u çalıştırmak için Otomasyon hesabı seçebilir.

![Otomasyon runbook yapılandırma ve yeni bir Otomasyon hesabı oluşturma](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> Bu sürüm için hizmeti sağlayan – üç runbook'lardan VM yeniden başlatma, durdurma VM veya kaldırma VM seçebilirsiniz (silme).  Diğer runbook'lar veya kendi runbook'ları biri seçebilme gelecekteki bir sürümde kullanılabilir.
> 
> 

![Aralarından seçim yapabileceğiniz runbook'ları](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

Üç kullanılabilir runbook'ları, biri seçtikten sonra **Otomasyon hesabı** aşağı açılan listesinde görünür ve runbook'u olarak çalışacak bir Otomasyon hesabı seçin. Runbook'ları bağlamında çalıştırmanız gerekir bir [Otomasyon hesabı](automation-security-overview.md) Azure aboneliğinizde olmasıdır. Önceden oluşturulmuş veya sizin için oluşturulan yeni bir Otomasyon hesabı olabilir bir Otomasyon hesabı seçebilirsiniz.

Sağlanan runbook'ları bir hizmet sorumlusu kullanarak Azure kimlik doğrulaması. Runbook, var olan Otomasyon hesaplarından birini çalıştırmayı seçerseniz, otomatik olarak hizmet asıl sizin için oluşturuyoruz. Yeni bir Otomasyon hesabı oluşturmayı seçerseniz, ardından otomatik olarak hesabı ve hizmet sorumlusu oluşturuyoruz. Her iki durumda da, iki varlıklar da Otomasyon hesabı – adlı bir sertifika varlığı oluşturulan **AzureRunAsCertificate** ve adlı bir bağlantı varlığı **AzureRunAsConnection**. Runbook'ları kullanmanızı **AzureRunAsConnection** bir VM'ye karşı yönetim eylemi gerçekleştirmek için Azure kimlik doğrulaması için.

> [!NOTE]
> Hizmet sorumlusu abonelik kapsamında oluşturulan ve katkıda bulunan rolü atanır. Bu rol, Azure Vm'leri yönetmek için sırayla Otomasyon runbook'ları çalıştırma izni olması hesabı gereklidir.  Automaton hesabı ve/veya hizmet sorumlusu oluşturma tek seferlik bir olaydır. Oluşturulduktan sonra diğer Azure VM uyarılar için runbook'ları çalıştırmak için bu hesabı kullanabilirsiniz.
> 
> 

Tıkladığınızda **Tamam** uyarı yapılandırılır ve yeni bir Otomasyon hesabı oluşturma seçeneğini seçtiyseniz, birlikte hizmet sorumlusu oluşturulur.  Bu işlemin tamamlanması birkaç saniye alabilir.  

![Yapılandırılmakta Runbook](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

Yapılandırma tamamlandıktan sonra runbook adı bkz **uyarı kuralı eklemek** sayfası.

![Yapılandırılmış Runbook](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

Tıklatın **Tamam** içinde **uyarı kuralı eklemek** sayfası.  Uyarı kuralı oluşturulur ve sanal makine çalışır durumda ise etkinleştirin.

### <a name="enable-or-disable-a-runbook"></a>Etkinleştirmek veya devre dışı bir runbook
Bir uyarı için yapılandırılmış bir runbook'unuz varsa, bu runbook yapılandırma kaldırmadan devre dışı bırakabilirsiniz. Bu, çalışan uyarı tutmak ve belki de uyarı kuralları bazılarını test ve ardından daha sonra runbook'u yeniden etkinleştirmek sağlar.

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a>Azure uyarı ile çalışan bir runbook'u oluşturma
Bir Azure uyarı kuralının bir parçası bir runbook seçtiğinizde, runbook mantığı kendisine geçirilen uyarı verileri yönetmek için de olmalıdır.  Bir runbook bir uyarı kuralı yapılandırıldığında, bir Web kancası için runbook oluşturulur; Bu Web kancası sonra runbook uyarıyı tetikleyecek her zaman başlatmak için kullanılır.  Web kancası URL'si için HTTP POST isteği runbook'u başlatmak için gerçek çağrıdır. POST isteğini gövdesini uyarıyla ilgili yararlı özellikleri içeren bir JSON biçiminin nesnesi içerir.  Aşağıda görüldüğü gibi uyarı verileri Subscriptionıd, resourceGroupName, resourceName ve resourceType gibi ayrıntıları içerir.

### <a name="example-of-alert-data"></a>Uyarı veri örneği
```
{
    "WebhookName": "AzureAlertTest",
    "RequestBody": "{
    \"status\":\"Activated\",
    \"context\": {
        \"id\":\"/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/microsoft.insights/alertrules/AlertTest\",
        \"name\":\"AlertTest\",
        \"description\":\"\",
        \"condition\": {
            \"metricName\":\"CPU percentage guest OS\",
            \"metricUnit\":\"Percent\",
            \"metricValue\":\"4.26337916666667\",
            \"threshold\":\"1\",
            \"windowSize\":\"60\",
            \"timeAggregation\":\"Average\",
            \"operator\":\"GreaterThan\"},
        \"subscriptionId\":\<subscriptionID> \",
        \"resourceGroupName\":\"TestResourceGroup\",
        \"timestamp\":\"2016-04-24T23:19:50.1440170Z\",
        \"resourceName\":\"TestVM\",
        \"resourceType\":\"microsoft.compute/virtualmachines\",
        \"resourceRegion\":\"westus\",
        \"resourceId\":\"/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\",
        \"portalLink\":\"https://portal.azure.com/#resource/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\"
        },
    \"properties\":{}
    }",
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "<webhookURL>"
    }
}
```

Otomasyon Web kancası hizmeti HTTP POST aldığında uyarı verileri ayıklar ve WebhookData runbook giriş parametresi runbook'ta geçirir.  WebhookData parametresini kullanın ve uyarı verileri ayıklamak ve uyarıyı tetikleyen Azure kaynak yönetmek için kullanmak üzere nasıl oluşturulduğunu gösteren bir örnek runbook aşağıdadır.

### <a name="example-runbook"></a>Örnek runbook
```
#  This runbook restarts an ARM (V2) VM in response to an Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that the alert status is 'Activated' (alert condition went from false to true)
    # and not 'Resolved' (alert condition went from true to false)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get the info needed to identify the VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is the expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate to Azure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart the VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant to be started from an Azure alert only."
}
```

## <a name="summary"></a>Özet
Azure VM temelinde bir uyarı yapılandırdığınızda artık kolayca uyarıyı tetikleyen olduğunda otomatik olarak düzeltme eylemi gerçekleştirmek için bir Otomasyon runbook yapılandırma olanağına sahip. Bu sürüm için yeniden başlatma, durdurma veya uyarı senaryonuza bağlı olarak bir VM silmek runbook'lardan seçebilirsiniz. Bu uyarıyı tetikleyen olduğunda otomatik olarak gerçekleştirilir eylemleri (bildirim, sorun giderme, düzeltme) kontrol burada senaryoları etkinleştirme sadece başlangıçtır.

## <a name="next-steps"></a>Sonraki Adımlar
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için, bkz. [Azure Automation runbook türleri](automation-runbook-types.md)

