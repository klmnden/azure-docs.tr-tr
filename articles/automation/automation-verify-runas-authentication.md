---
title: "Azure Otomasyonu hesap yapılandırmasını doğrulama | Microsoft Docs"
description: "Bu makalede Otomasyon hesabınızın doğru şekilde ayarlandığını onaylama işlemi açıklanmaktadır."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: magoedte
ms.openlocfilehash: 55f5d5524019ac63565e5ddd1f47dbdd65f05065
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="test-azure-automation-run-as-account-authentication"></a>Azure Otomasyonu Farklı Çalıştır hesabı kimlik doğrulamasını test etme
Bir Otomasyon hesabı başarıyla oluşturulduktan sonra, yeni oluşturduğunuz veya güncelleştirdiğiniz Azure Farklı Çalıştır hesabını kullanarak Azure Resource Manager veya Azure klasik dağıtımında kimlik doğrulamasını başarıyla yapabildiğinizi onaylamak üzere basit bir test gerçekleştirebilirsiniz.    

## <a name="automation-run-as-authentication"></a>Otomasyon Farklı Çalıştır kimlik doğrulaması
Aşağıdaki örnek kodları Farklı Çalıştır hesabını kullanarak kimlik doğrulamayı denetlemek için bir [PowerShell runbook’u oluşturmak](automation-creating-importing-runbook.md) ve özel runbook’larınızda Otomasyon hesabınızda kimlik doğrulamak ve Resource Manager kaynaklarınızı yönetmek için kullanabilirsiniz.   

    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get the connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

        "Logging in to Azure..."
        Add-AzureRmAccount `
           -ServicePrincipal `
           -TenantId $servicePrincipalConnection.TenantId `
           -ApplicationId $servicePrincipalConnection.ApplicationId `
           -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
       if (!$servicePrincipalConnection)
       {
          $ErrorMessage = "Connection $connectionName not found."
          throw $ErrorMessage
      } else{
          Write-Error -Message $_.Exception
          throw $_.Exception
      }
    }

    #Get all ARM resources from all resource groups
    $ResourceGroups = Get-AzureRmResourceGroup 

    foreach ($ResourceGroup in $ResourceGroups)
    {    
       Write-Output ("Showing resources in resource group " + $ResourceGroup.ResourceGroupName)
       $Resources = Find-AzureRmResource -ResourceGroupNameContains $ResourceGroup.ResourceGroupName | Select ResourceName, ResourceType
       ForEach ($Resource in $Resources)
       {
          Write-Output ($Resource.ResourceName + " of type " +  $Resource.ResourceType)
       }
       Write-Output ("")
    } 

Runbook’ta kimlik doğrulaması için kullanılan cmdlet’in (**Add-AzureRmAccount**) *ServicePrincipalCertificate* parametre kümesini kullandığına dikkat edin.  Kimlik bilgilerini değil, hizmet asıl sertifikasını kullanarak kimlik doğrulamasını yapar.  

Farklı Çalıştır hesabınızı doğrulamak için bir [runbook’u çalıştırdığınızda](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) bir [runbook işi](automation-runbook-execution.md) oluşturulur, İş dikey penceresi gösterilir ve iş durumu **İş Özeti** kutucuğunda gösterilir. İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  Runbook işi tamamlandığında **Tamamlandı** durumunu görmeniz gerekir.

Runbook’un ayrıntılı sonuçlarını görmek için **Çıktı** kutucuğuna tıklayın.  **Çıktı** dikey penceresinde kimlik doğrulamasının başarıyla yapıldığını ve aboneliğinizde olan tüm kaynak gruplarındaki kaynakların bir listesinin döndürüldüğünü görürsünüz.  

Runbook’larınız için kodları kullanırken `#Get all ARM resources from all resource groups` açıklaması ile başlayan kod bloğunu kaldırmayı unutmayın.

## <a name="classic-run-as-authentication"></a>Klasik Farklı Çalıştır kimlik doğrulaması
Aşağıdaki örnek kodları Klasik Farklı Çalıştır hesabını kullanarak kimlik doğrulamayı denetlemek için bir [PowerShell runbook’u oluşturmak](automation-creating-importing-runbook.md) ve özel runbook’larınızda kimlik doğrulamak ve klasik dağıtım modelinde kaynaklarınızı yönetmek için kullanabilirsiniz.  

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID
    
    #Get all VMs in the subscription and return list with name of each
    Get-AzureVM | ft Name

Farklı Çalıştır hesabınızı doğrulamak için bir [runbook’u çalıştırdığınızda](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) bir [runbook işi](automation-runbook-execution.md) oluşturulur, İş dikey penceresi gösterilir ve iş durumu **İş Özeti** kutucuğunda gösterilir. İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  Runbook işi tamamlandığında **Tamamlandı** durumunu görmeniz gerekir.

Runbook’un ayrıntılı sonuçlarını görmek için **Çıktı** kutucuğuna tıklayın.  **Çıktı** dikey penceresinde kimlik doğrulamasının başarıyla yapıldığını ve aboneliğinizde olan Azure VM’lerinin VMName’lerini içeren bir listesinin döndürüldüğünü görürsünüz.  

Runbook’larınız için kodları kullanırken **Get-AzureVM** cmdlet’ini kaldırmayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook’um](automation-first-runbook-textual-powershell.md).
* Grafik Yazma hakkında daha fazla bilgi için bkz. [Azure Otomasyonu’nda grafik yazma](automation-graphical-authoring-intro.md).
