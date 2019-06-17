---
title: Azure Analysis Services modellerine Azure Otomasyonu ile yenileme | Microsoft Docs
description: Azure Otomasyonu'nu kullanarak model yenileme kod öğrenin.
author: chrislound
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: chlound
ms.openlocfilehash: 4cae93cff594ad561973f8029ea7335dc4c60263
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66356991"
---
# <a name="refresh-with-azure-automation"></a>Azure Otomasyonu ile yenileme

Azure Otomasyonu ve PowerShell runbook'ları kullanarak, Azure Analysis tablosal Modellerinizi otomatik veri yenileme işlemleri gerçekleştirebilirsiniz.  

Bu makaledeki örnekte [PowerShell SqlServer modülleri](https://docs.microsoft.com/powershell/module/sqlserver/?view=sqlserver-ps).

Bir örnek PowerShell Runbook, bir model yenileme gösterir, bu makalenin sonraki bölümlerinde sağlanır.  

## <a name="authentication"></a>Kimlik Doğrulaması

Tüm çağrıları ile geçerli bir Azure Active Directory (OAuth 2) belirteci kimlik doğrulaması gerekir.  Bu makaledeki örnek, Azure Analysis Services için kimlik doğrulaması için bir hizmet sorumlusu (SPN) kullanır.

Bir hizmet sorumlusu oluşturma hakkında daha fazla bilgi edinmek için [Azure portalını kullanarak bir hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Aşağıdaki örnekte, Azure Analysis Services Güvenlik Duvarı'nı devre dışı varsayılır. Güvenlik Duvarı etkinse, isteği başlatanın genel IP adresini Güvenlik Duvarı'nda beyaz listeye alınması gerekecektir.

### <a name="install-sqlserver-modules-from-powershell-gallery"></a>SqlServer modülleri PowerShell Galerisi'nden yükleme.

1. Azure Otomasyon hesabınızdaki tıklayın **modülleri**, ardından **Galeriye Gözat**.

2. Arama çubuğunda arama **SqlServer**.

    ![Arama modülleri](./media/analysis-services-refresh-azure-automation/1.png)

3. SQL Server'ı seçin, ardından tıklayın **alma**.
 
    ![Modül İçeri Aktar](./media/analysis-services-refresh-azure-automation/2.png)

4. **Tamam**'ı tıklatın.
 
### <a name="create-a-service-principal-spn"></a>(SPN) hizmet sorumlusu oluşturma

Bir hizmet sorumlusu oluşturma hakkında bilgi edinmek için [Azure portalını kullanarak bir hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).

### <a name="configure-permissions-in-azure-analysis-services"></a>Azure Analysis Services'de izinlerini yapılandırma
 
Oluşturduğunuz hizmet asıl sunucuda Sunucu yönetici izinleri olmalıdır. Daha fazla bilgi için bkz. [sunucu yöneticisi rolüne hizmet sorumlusu ekleme](analysis-services-addservprinc-admins.md).

## <a name="design-the-azure-automation-runbook"></a>Azure Otomasyonu Runbook tasarlama

1. Otomasyon hesabı oluşturun bir **kimlik bilgilerini** hizmet sorumlusu güvenli bir şekilde depolamak için kullanılacak kaynak.

    ![kimlik bilgisi oluşturma](./media/analysis-services-refresh-azure-automation/6.png)

2. Ayrıntılar için kimlik bilgisi girin.  İçin **kullanıcı adı**, girin **SPN ClientID**, için **parola**, girin **SPN gizli**.

    ![kimlik bilgisi oluşturma](./media/analysis-services-refresh-azure-automation/7.png)

3. Otomasyon Runbook'u İçeri Aktar

    ![Runbook'u İçeri Aktar](./media/analysis-services-refresh-azure-automation/8.png)

4. Göz atın **yenileme Model.ps1** dosya, sağlayan bir **adı** ve **açıklama**ve ardından **Oluştur**.

    ![Runbook'u İçeri Aktar](./media/analysis-services-refresh-azure-automation/9.png)

5. Runbook oluşturulduğunda otomatik olarak düzenleme moduna geçer.  **Yayımla**’yı seçin.

    ![Publish Runbook](./media/analysis-services-refresh-azure-automation/10.png)

    > [!NOTE]
    > Oluşturulan kimlik bilgisi kaynağı kullanarak runbook tarafından daha önce listelene **Get-AutomationPSCredential** komutu.  Bu komut daha sonra geçirilir **Invoke-ProcessASADatabase** Azure Analysis Services kimlik doğrulaması gerçekleştirmek için PowerShell komutu.

6. Tıklayarak runbook'u test **Başlat**.

    ![Runbook başlatma](./media/analysis-services-refresh-azure-automation/11.png)

7. Doldurun **DATABASENAME**, **ANALYSISSERVER**, ve **REFRESHTYPE** parametreleri ve ardından **Tamam**. **WEBHOOKDATA** parametresi zorunlu değildir el ile bir Runbook çalıştırıldığında.

    ![Runbook başlatma](./media/analysis-services-refresh-azure-automation/12.png)

Runbook başarılı bir şekilde yürütüldü, aşağıdakine benzer bir çıktı alırsınız:

![Başarılı çalıştırma](./media/analysis-services-refresh-azure-automation/13.png)

## <a name="use-a-self-contained-azure-automation-runbook"></a>Kendi içinde bir Azure Otomasyonu Runbook'u kullanın

Runbook, Azure Analysis Services modeli yenileyerek bir zamanlamaya göre tetikleyecek şekilde yapılandırılabilir.

Bunu şu şekilde yapılandırılabilir:

1. Otomasyon Runbook'u tıklayın **zamanlamaları**, ardından **zamanlama Ekle**.
 
    ![Bir zamanlama oluşturun](./media/analysis-services-refresh-azure-automation/14.png)

2. Tıklayın **zamanlama** > **yeni bir zamanlama oluşturmak**ve ardından ayrıntıları girin.

    ![Zamanlamayı Yapılandır](./media/analysis-services-refresh-azure-automation/15.png)

3. **Oluştur**’a tıklayın.

4. Zamanlama için parametreleri doldurun. Bunlar Runbook'u tetikleyen her saat kullanılacaktır. **WEBHOOKDATA** parametre kalan boş bir zamanlama ile çalışırken.

    ![Parametreleri Yapılandır](./media/analysis-services-refresh-azure-automation/16.png)

5. **Tamam**'ı tıklatın.

## <a name="consume-with-data-factory"></a>Data Factory ile kullanma

Azure Data Factory kullanarak runbook kullanmak için öncelikle oluşturma bir **Web kancası** runbook. **Web kancası** bir Azure Data Factory web etkinliği çağrılabilen bir URL sağlar.

> [!IMPORTANT]
> Oluşturmak için bir **Web kancası**, runbook'un durumu olmalıdır **yayımlanan**.

1. Otomasyon Runbook'unuz tıklayın **Web kancaları**ve ardından **Web kancası Ekle**.

   ![Web kancası Ekle](./media/analysis-services-refresh-azure-automation/17.png)

2. Web kancasını bir ad ve bir süre sonu verin.  Ad yalnızca Otomasyon Runbook'u içinde Web kancasını tanımlar, URL'nin parçası form değil.

   >[!CAUTION]
   >Geri kez kapalı elde edemiyor olarak Sihirbazı kapatmadan önce URL'yi kopyalayın emin olun.
    
   ![Web kancası yapılandırma](./media/analysis-services-refresh-azure-automation/18.png)

    Web kancası parametreleri boş kalır.  Azure Data Factory web etkinliği yapılandırırken parametreleri web çağrısı gövdesine geçirilebilir.

3. Data Factory'de yapılandırma bir **web etkinliği**

### <a name="example"></a>Örnek

   ![Örnek Web etkinliği](./media/analysis-services-refresh-azure-automation/19.png)

**URL** Web kancasından oluşturulan URL.

**Gövdesi** aşağıdaki özellikleri içermesi gereken bir JSON belgesidir:


|Özellik  |Değer  |
|---------|---------|
|**AnalysisServicesDatabase**     |Azure Analysis Services veritabanının adı <br/> Örnek: AdventureWorksDB         |
|**AnalysisServicesServer**     |Azure Analysis Services sunucu adı. <br/> Örnek: https:\//westus.asazure.windows.net/servers/myserver/models/AdventureWorks/         |
|**DatabaseRefreshType**     |Yenileme gerçekleştirmek için türü. <br/> Örnek: Tam         |

Örnek JSON gövdesi:

```json
{
    "AnalysisServicesDatabaseName": "AdventureWorksDB",
    "AnalysisServicesServer": "asazure://westeurope.asazure.windows.net/MyAnalysisServer",
    "DatabaseRefreshType": "Full"
}
```

Bu parametreler, PowerShell Betiği runbook'ta tanımlanır.  Web Etkinliği yürütüldüğünde, geçirilen JSON WEBHOOKDATA yüktür.

Bu seri durumdan ve ardından Invoke-ProcesASDatabase PowerShell komutu tarafından kullanılan PowerShell parametreleri olarak depolanır.

![Seri durumdan çıkarılmış Web kancası](./media/analysis-services-refresh-azure-automation/20.png)

## <a name="use-a-hybrid-worker-with-azure-analysis-services"></a>Karma çalışanı Azure Analysis Services ile kullanma

Bir Azure sanal makinesi bir statik genel IP adresi ile Azure Otomasyon karma çalışanı olarak kullanılabilir.  Bu genel IP adresi, Azure Analysis Services Güvenlik Duvarı sonra eklenebilir.

> [!IMPORTANT]
> Sanal makinenin genel IP adresi statik olarak yapılandırıldığından emin olun.
>
>Azure Otomasyon karma çalışanlarını yapılandırma hakkında daha fazla bilgi edinmek için [veri merkezinde veya bulutta kaynaklarında karma Runbook çalışanı kullanarak otomatikleştirmek](../automation/automation-hybrid-runbook-worker.md#install-a-hybrid-runbook-worker).

Karma çalışanı yapılandırıldıktan sonra bölümünde açıklandığı gibi bir Web kancası oluşturma [Data Factory ile kullanma](#consume-with-data-factory).  Burada tek fark seçmektir **çalıştıracağınız** > **karma çalışanı** Web kancasını yapılandırırken seçeneği.

Örnek Web kancası karma çalışanı kullanarak:

![Örnek karma çalışan Web kancası](./media/analysis-services-refresh-azure-automation/21.png)

## <a name="sample-powershell-runbook"></a>Örnek PowerShell Runbook

Aşağıdaki kod parçacığı, bir PowerShell Runbook'ı kullanarak Azure Analysis Services model yenileme gerçekleştirmek nasıl bir örneğidir.

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData,

    [Parameter (Mandatory = $false)]
    [String] $DatabaseName,
    [Parameter (Mandatory = $false)]
    [String] $AnalysisServer,
    [Parameter (Mandatory = $false)]
    [String] $RefreshType
)

$_Credential = Get-AutomationPSCredential -Name "ServicePrincipal"

# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData)
{ 
    # Retrieve AAS details from Webhook request body
    $atmParameters = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)
    Write-Output "CredentialName: $($atmParameters.CredentialName)"
    Write-Output "AnalysisServicesDatabaseName: $($atmParameters.AnalysisServicesDatabaseName)"
    Write-Output "AnalysisServicesServer: $($atmParameters.AnalysisServicesServer)"
    Write-Output "DatabaseRefreshType: $($atmParameters.DatabaseRefreshType)"
    
    $_databaseName = $atmParameters.AnalysisServicesDatabaseName
    $_analysisServer = $atmParameters.AnalysisServicesServer
    $_refreshType = $atmParameters.DatabaseRefreshType
 
    Invoke-ProcessASDatabase -DatabaseName $_databaseName -RefreshType $_refreshType -Server $_analysisServer -ServicePrincipal -Credential $_credential
}
else 
{
    Invoke-ProcessASDatabase -DatabaseName $DatabaseName -RefreshType $RefreshType -Server $AnalysisServer -ServicePrincipal -Credential $_Credential
}
```


## <a name="next-steps"></a>Sonraki adımlar

[Örnekler](analysis-services-samples.md)  
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)
