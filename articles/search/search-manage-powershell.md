---
title: Powershell betikleri - Azure Search ile Azure Search Hizmeti yönetme
description: PowerShell betikleri ile Azure arama hizmetinizi yönetin. Oluşturun veya bir Azure Search Hizmeti güncelleştirin ve Azure Search yönetici anahtarları yönetme
author: HeidiSteen
manager: cgronlun
tags: azure-resource-manager
services: search
ms.service: search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 08/15/2016
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 86f8eebb8e174b4a4d4dbdc9def516e23b79a131
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56591661"
---
# <a name="manage-your-azure-search-service-with-powershell"></a>PowerShell ile Azure Search hizmetinizi yönetme
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> 
> 

Bu konuda, Azure Search Hizmetleri için birçok yönetim görevlerini gerçekleştirmek için PowerShell komutlarını açıklanmaktadır. Bir arama hizmeti oluşturma, ölçeklendirme ve kendi API anahtarlarını yönetme aracılığıyla yol gösterir.
Bu komutları kullanılabilir yönetim seçenekleri paralel [Azure arama yönetimi REST API'si](https://docs.microsoft.com/rest/api/searchmanagement).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Azure PowerShell olması gerekir. Yükleme yönergeleri için bkz. [Azure PowerShell yükleme](/powershell/azure/overview).
* PowerShell'de, aşağıda açıklandığı gibi Azure aboneliğinizde oturum açmanız gerekir.

İlk olarak, yapmanız gerekenler şu komutla Azure'da oturum aç:

    Connect-AzAccount

Microsoft Azure oturum açma iletişim kutusunda Azure hesabınızı ve kendi parola e-posta adresini belirtin.

Alternatif olarak [etkileşimli olmayan bir hizmet sorumlusu ile oturum açma](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

Birden çok Azure aboneliğiniz varsa, Azure aboneliğinizi ayarlama gerekir. Geçerli aboneliklerinizin bir listesini görmek için şu komutu çalıştırın.

    Get-AzSubscription | sort SubscriptionName | Select SubscriptionName

Aboneliği belirtmek için aşağıdaki komutu çalıştırın. Aşağıdaki örnekte, abonelik addır `ContosoSubscription`.

    Select-AzSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Size yardımcı olan komutlar çalışmaya başlama
    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzResourceProvider -ProviderNamespace "Microsoft.Search"

    # Create a new search service
    # This command will return once the service is fully created
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1

    # Get information about your new service and store it in $resource
    $resource = Get-AzResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # View your resource
    $resource

    # Get the primary admin API key
    $primaryKey = (Invoke-AzResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key

    # View your query key
    $queryKey

    # Delete query key
    Remove-AzResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzResource

    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzResource

## <a name="next-steps"></a>Sonraki Adımlar
Hizmetiniz oluşturulur, şu adımları izleyin: derleme bir [dizin](search-what-is-an-index.md), [dizin sorgulama](search-query-overview.md)ve son olarak oluşturun ve Azure Search kullanan kendi arama uygulaması yönetin.

* [Azure portalında bir Azure Search dizini oluşturma](search-create-index-portal.md)
* [Arama Gezgini'ni kullanarak Azure portalında bir Azure Search dizinini sorgulama](search-explorer.md)
* [Kurulum hizmetlerinden verileri yüklemek için bir dizin oluşturucu](search-indexer-overview.md)
* [.NET’te Azure Search kullanma](search-howto-dotnet-sdk.md)
* [Azure Search trafiğiniz analiz edin](search-traffic-analytics.md)

