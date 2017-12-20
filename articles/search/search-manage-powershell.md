---
title: "Powershell komut dosyalarıyla Azure Search yönetme | Microsoft Docs"
description: "PowerShell komut dosyalarıyla Azure Search hizmetinizi yönetme. Oluşturma veya güncelleştirme bir Azure Search hizmeti ve Azure Search yönetici anahtarları Yönet"
services: search
documentationcenter: 
author: seansaleh
manager: mblythe
editor: 
tags: azure-resource-manager
ms.assetid: 9b3dc1f2-3619-4235-ba1f-d2d6f5c45dd5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: powershell
ms.date: 08/15/2016
ms.author: seasa
ms.openlocfilehash: aa51c846efef12461ec382274199bc049c42aaa3
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="manage-your-azure-search-service-with-powershell"></a>PowerShell ile Azure Search hizmetinizi yönetme
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> 
> 

Bu konu Azure arama hizmetleri için yönetim görevlerinin çoğunu gerçekleştirmek için PowerShell komutlarını açıklar. Bir arama hizmeti oluşturma, ölçeklendirme ve API anahtarını yönetme size yol gösterir.
Bu komutlar kullanılabilir yönetim seçenekleri paralel [Azure Search Yönetimi REST API'si](http://msdn.microsoft.com/library/dn832684.aspx).

## <a name="prerequisites"></a>Ön koşullar
* Azure PowerShell 1.0 veya daha büyük olmalıdır. Yönergeler için bkz: [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/overview).
* Aşağıda açıklandığı gibi PowerShell'de Azure aboneliğinizde oturum açmanız gerekir.

İlk olarak, şunları yapmalısınız bu komutla Azure oturum açma:

    Login-AzureRmAccount

Microsoft Azure oturum açma iletişim kutusunda Azure hesabınızı ve kendi parolasını e-posta adresini belirtin.

Alternatif olarak [bir hizmet sorumlusu ile etkileşimli oturum açma](../azure-resource-manager/resource-group-authenticate-service-principal.md).

Birden çok Azure aboneliğiniz varsa, Azure aboneliğinizin ayarlamanız gerekir. Geçerli Aboneliklerin listesini görmek için bu komutu çalıştırın.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Aboneliği belirtmek için aşağıdaki komutu çalıştırın. Aşağıdaki örnekte, abonelik addır `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Yardımcı olması için komutları çalışmaya başlama
    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search"

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1

    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # View your resource
    $resource

    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key

    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource

    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource

## <a name="next-steps"></a>Sonraki Adımlar
Hizmetiniz oluşturulur, sonraki adımlar alabilir: derleme bir [dizin](search-what-is-an-index.md), [dizin sorgu](search-query-overview.md)ve son olarak oluşturun ve Azure Search kullanan kendi arama uygulamasını Yönet.

* [Azure portalda Azure Search dizini oluşturma](search-create-index-portal.md)
* [Azure Portalı'nda arama Gezgini kullanarak Azure Search dizini sorgulama](search-explorer.md)
* [Veri hizmetlerinden yüklemek için bir dizin oluşturucu ayarlayın](search-indexer-overview.md)
* [Azure Search .NET ile kullanma](search-howto-dotnet-sdk.md)
* [Azure Search trafiğini analiz](search-traffic-analytics.md)

