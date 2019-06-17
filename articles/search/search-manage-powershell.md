---
title: PowerShell betikleri Az.Search modülü - Azure Search kullanma
description: Oluşturun ve PowerShell ile Azure Search Hizmeti yapılandırın. Hizmet ölçeği artırın veya azaltın, yönetici ve sorgu api anahtarlarından ve sorgu sistem bilgilerini yönetin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: heidist
ms.openlocfilehash: 8f07468ccff4431e1afdf66aedc72599ddc0c25b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60194283"
---
# <a name="manage-your-azure-search-service-with-powershell"></a>PowerShell ile Azure Search hizmetinizi yönetme
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [REST API](https://docs.microsoft.com/rest/api/searchmanagement/)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Windows, Linux üzerinde veya PowerShell cmdlet'leri ve betikleri çalıştırılabilir [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) oluşturmak ve Azure Search yapılandırmak için. **Az.Search** Azure PowerShell modülü genişletir] için tam eşlikli [Azure arama yönetimi REST API'lerini](https://docs.microsoft.com/rest/api/searchmanagement). Azure PowerShell ile ve **Az.Search**, aşağıdaki görevleri gerçekleştirebilirsiniz:

> [!div class="checklist"]
> * [Aboneliğinizdeki tüm arama hizmetleri listeleyin](#list-search-services)
> * [Belirli bir arama hizmeti hakkında bilgi edinin](#get-search-service-information)
> * [Oluşturma veya hizmet silme](#create-or-delete-a-service)
> * [Yönetici API anahtarlarını yeniden oluştur](#regenerate-admin-keys)
> * [Oluşturma veya sorgu api anahtarlarından silme](#create-or-delete-query-keys)
> * [Bir hizmet ölçeğini artırmayı veya düşüren, çoğaltmalar ve bölümler](#scale-replicas-and-partitions)

PowerShell, ad, bölge veya hizmetinizin katmanını değiştirmek için kullanılamaz. Bir hizmet oluşturulduğunda ayrılmış kaynakları tahsis edilir. Temel alınan donanım (konumu veya düğüm tür) değiştirme, yeni bir hizmet gerektirir. Araçlar veya yoktur API'leri içeriğin bir hizmetten diğerine aktarılması için. Tüm içerik yönetimi aracılığıyladır [REST](https://docs.microsoft.com/rest/api/searchservice/) veya [.NET](https://docs.microsoft.com/dotnet/api/?term=microsoft.azure.search) API'leri ve dizinleri taşımak istiyorsanız, yeniden oluşturun ve bunları yeni bir hizmet üzerinde yeniden yüklemek gerçekleştirmeniz gerekir. 

İçerik Yönetimi için ayrılmış bir PowerShell komut yok olsa da, REST veya .NET oluşturmak ve dizinleri yüklemek için çağıran PowerShell Betiği yazabilirsiniz. **Az.Search** modülü kendisi tarafından bu işlemleri sağlamaz.

PowerShell veya herhangi başka bir API'yi (yalnızca portalı) ile desteklenmeyen diğer görevler aşağıdakileri içerir:
+ [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md) için [AI zenginleştirilmiş dizinleme](cognitive-search-concept-intro.md). Bir bilişsel hizmet bir beceri kümesi, olmayan bir abonelik veya hizmet eklenir.
+ [Eklenti izleme çözümleri](search-monitor-usage.md#add-on-monitoring-solutions) veya [trafik analizi arama](search-traffic-analytics.md) Azure Search izlemesi için kullanılır.

<a name="check-versions-and-load"></a>

## <a name="check-versions-and-load-modules"></a>Sürümleri denetleyin ve modülleri yükleme

Bu makaledeki örneklerde, etkileşimli ve yükseltilmiş izinleri gerektirir. Azure PowerShell ( **Az** Modülü) yüklü olmalıdır. Daha fazla bilgi için [Azure PowerShell yükleme](/powershell/azure/overview).

### <a name="powershell-version-check-51-or-later"></a>PowerShell sürüm denetimi (5.1 veya üzeri)

Yerel PowerShell 5.1 veya üstünde, tüm desteklenen işletim sistemlerinde olmalıdır.

```azurepowershell-interactive
$PSVersionTable.PSVersion
```

### <a name="load-azure-powershell"></a>Azure PowerShell'i yükleme

Emin değilseniz olup **Az** yüklü, doğrulama adımı olarak aşağıdaki komutu çalıştırın. 

```azurepowershell-interactive
Get-InstalledModule -Name Az
```

Bazı sistemler otomatik yükle modülleri değil yapın. Bir hata iletisi alırsanız önceki komutta, modül yüklemeyi deneyin ve bu başarısız olursa bir adımı atladıysanız, görmek için yükleme yönergeleri için geri dönün.

```azurepowershell-interactive
Import-Module -Name Az
```

### <a name="connect-to-azure-with-a-browser-sign-in-token"></a>Oturum açma tarayıcı belirteciyle Azure'a bağlanma

Bir abonelikte PowerShell bağlanmak için portalı oturum açma kimlik bilgilerinizi kullanabilirsiniz. Alternatif olarak [etkileşimli olmayan hizmet sorumlusuyla kimlik doğrulaması](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

```azurepowershell-interactive
Connect-AzAccount
```

Birden çok Azure aboneliği tutarsanız, Azure aboneliğinizi ayarlama. Geçerli aboneliklerinizin bir listesini görmek için şu komutu çalıştırın.

```azurepowershell-interactive
Get-AzSubscription | sort SubscriptionName | Select SubscriptionName
```

Aboneliği belirtmek için aşağıdaki komutu çalıştırın. Aşağıdaki örnekte, abonelik addır `ContosoSubscription`.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName ContosoSubscription
```

<a name="list-search-services"></a>

## <a name="list-all-azure-search-services-in-your-subscription"></a>Aboneliğinizdeki tüm Azure Search hizmetlerini listelemek

Aşağıdaki komutları arasındadır [ **Az.Resources**](https://docs.microsoft.com/powershell/module/az.resources/?view=azps-1.4.0#resources), mevcut kaynaklar ve zaten, aboneliğinizde sağlanan hizmetler hakkında bilgi döndüren. Kaç arama hizmetleri önceden oluşturulan bilmiyorsanız, bu komutları bir seyahat portala kaydetme, bu bilgileri döndürür.

İlk komut, tüm arama hizmetlerinin döndürür.

```azurepowershell-interactive
Get-AzResource -ResourceType Microsoft.Search/searchServices | ft
```

Hizmetler listesinden belirli bir kaynak hakkında bilgi döndürür.

```azurepowershell-interactive
Get-AzResource -ResourceName <service-name>
```

Sonuç aşağıdaki çıktıya benzer olmalıdır.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : westus
ResourceId        : /subscriptions/<alpha-numeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="import-azsearch"></a>İçeri aktarma Az.Search

Gelen komutları [ **Az.Search** ](https://docs.microsoft.com/powershell/module/az.search/?view=azps-1.4.0#search) modülü kadar kullanılamaz.

```azurepowershell-interactive
Install-Module -Name Az.Search
```

### <a name="list-all-azsearch-commands"></a>Tüm Az.Search komutları listesi

Bir doğrulama adımı modülünde sağlanan komutların bir listesini döndürür.

```azurepowershell-interactive
Get-Command -Module Az.Search
```

Sonuç aşağıdaki çıktıya benzer olmalıdır.

```
CommandType     Name                                Version    Source
-----------     ----                                -------    ------
Cmdlet          Get-AzSearchAdminKeyPair            0.7.1      Az.Search
Cmdlet          Get-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          Get-AzSearchService                 0.7.1      Az.Search
Cmdlet          New-AzSearchAdminKey                0.7.1      Az.Search
Cmdlet          New-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          New-AzSearchService                 0.7.1      Az.Search
Cmdlet          Remove-AzSearchQueryKey             0.7.1      Az.Search
Cmdlet          Remove-AzSearchService              0.7.1      Az.Search
Cmdlet          Set-AzSearchService                 0.7.1      Az.Search
```

## <a name="get-search-service-information"></a>Arama hizmeti bilgilerini alma

Sonra **Az.Search** aktarılır ve çalıştırma, arama hizmetinizin içeren kaynak grubunu biliyorsanız [Get-AzSearchService](https://docs.microsoft.com/powershell/module/az.search/get-azsearchservice?view=azps-1.4.0) ad, bölge, katman ve çoğaltma gibi hizmet tanımı döndürülecek ve bölüm sayısı.

```azurepowershell-interactive
Get-AzSearchService -ResourceGroupName <resource-group-name>
```

Sonuç aşağıdaki çıktıya benzer olmalıdır.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : West US
Sku               : Standard
ReplicaCount      : 1
PartitionCount    : 1
HostingMode       : Default
ResourceId        : /subscriptions/<alphanumeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="create-or-delete-a-service"></a>Oluşturma veya hizmet silme

[**Yeni AzSearchService** ](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) için kullanılan [yeni arama hizmeti oluşturma](search-create-service-portal.md).

```azurepowershell-interactive
New-AzSearchService -ResourceGroupName "demo-westus" -Name "my-demo-searchapp" -Sku "Standard" -Location "West US" -PartitionCount 3 -ReplicaCount 3
``` 
Sonuç aşağıdaki çıktıya benzer olmalıdır.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Id                : /subscriptions/<alphanumeric-subscription-ID>/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 3
PartitionCount    : 3
HostingMode       : Default
Tags
```     

## <a name="regenerate-admin-keys"></a>Yönetici anahtarları yeniden oluştur

[**Yeni AzSearchAdminKey** ](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) yönetici geri almak için kullanılan [API anahtarları](search-security-api-keys.md). Her hizmet kimliği doğrulanmış erişim için iki yönetici anahtarı oluşturulur. Her istek anahtarları gereklidir. Her iki yönetici anahtarı bir arama hizmeti tam yazma erişimi herhangi bir bilgi almak veya oluşturma ve herhangi bir nesne silme olanağı vermek işlevsel olarak eşdeğerdir. Diğer değiştirme sırasında kullanabilirsiniz, böylece iki anahtar yok. 

Yalnızca teker teker olarak belirtilen yeniden oluşturabilirsiniz `primary` veya `secondary` anahtarı. Kesintisiz hizmet için birincil anahtarı alınması sırasında ikincil bir anahtar kullanmak için tüm istemci kodu güncelleştirmeniz unutmayın. İşlemleri uçuş sırasında anahtarları değiştirmekten kaçının.

İstemci kodu güncelleştirmeden anahtarlarını yeniden oluşturma durumunda, bekleyebileceğiniz gibi eski anahtarı kullanan istekler başarısız olur. Tüm yeni anahtarlarını yeniden kalıcı olarak size, hizmet dışı kilitlemez ve hizmet portal üzerinden erişmeye devam edebilirsiniz. Birincil ve ikincil anahtarları yeniden oluşturduktan sonra istemci kodu yeni anahtarları kullanacak şekilde güncelleştirebilirsiniz ve işlemleri uygun şekilde devam eder.

API anahtarları için değerleri, hizmet tarafından oluşturulur. Azure Search'ı kullanmak için özel anahtar sağlayamıyor. Benzer şekilde, yönetici API anahtarlarından için kullanıcı tanımlı adı yoktur. Anahtar başvuruları sabit dizeler ya da `primary` veya `secondary`. 

```azurepowershell-interactive
New-AzSearchAdminKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -KeyKind Primary
```

Sonuç aşağıdaki çıktıya benzer olmalıdır. Bir kerede yalnızca değiştirme olsa bile her iki anahtarı döndürülür.

```
Primary                    Secondary
-------                    ---------
<alphanumeric-guid>        <alphanumeric-guid>  
```

## <a name="create-or-delete-query-keys"></a>Oluşturma veya sorgu anahtarları silme

[**Yeni AzSearchQueryKey** ](https://docs.microsoft.com/powershell/module/az.search/new-azsearchquerykey?view=azps-1.4.0) sorgu oluşturmak için kullanılan [API anahtarları](search-security-api-keys.md) istemci uygulamalarından salt okunur erişim için Azure Search dizini. Sorgu anahtarları, arama sonuçlarını almak amacıyla belirli bir dizin için kimlik doğrulaması için kullanılır. Sorgu anahtarları, hizmette bir dizini, veri kaynağı ve dizin oluşturucu gibi diğer öğeleri salt okunur erişimi veremez.

Azure Search'ı kullanmak için bir anahtar sağlayamıyor. API anahtarları, hizmet tarafından oluşturulur.

```azurepowershell-interactive
New-AzSearchQueryKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -Name <query-key-name> 
```

## <a name="scale-replicas-and-partitions"></a>Ölçek çoğaltmalar ve bölümler

[**Set-AzSearchService** ](https://docs.microsoft.com/powershell/module/az.search/set-azsearchservice?view=azps-1.4.0) için kullanılan [çoğaltmalar ve bölümler azaltmak veya artırmak](search-capacity-planning.md) hizmetinizin içinde Faturalanabilen kaynaklar yeniden ayarlama gereksinimini. Çoğaltmalar veya bölümler artırma hem de faturanız için ekler sabit ve değişken ücretler. Ek işlem gücü için geçici bir gereksinimi varsa, çoğaltmalar ve bölümler iş yükünü işlemek için artırabilirsiniz. İzleme alan genel bakış portal sayfasında, sorgu gecikme süresi, sorgu başına saniye ve azaltma, geçerli kapasite yeterli olup olmadığını gösteren kutucukları sahiptir.

Uygulamanın kaynak ekleyip biraz sürebilir. Devam etmek var olan iş yükleri izin vermek için arka planda ayarlamalar kapasite oluşur. Ek kapasite, ek yapılandırma gerektirmeden hazır olduğunda hemen sonra gelen istekler için kullanılır. 

Kapasite kaldırma karışıklığa neden olabilir. Kapasite azaltma önce tüm dizin ve dizin oluşturucu işlerini durdurma bırakılan istekleri önlemek için önerilir. Bu uygun değilse, kademeli olarak kapasite azalır düşünebilirsiniz bir çoğaltma ve bölüm, yeni hedef düzeyleri ulaşıncaya kadar bir zaman.

Komutu gönderildikten sonra bunu sonlandıramaz bir yolu yoktur midway aracılığıyla. Komut sayıları düzeltilmesi önce işlemi tamamlanana kadar beklemeniz gerekir.

```azurepowershell-interactive
Set-AzSearchService -ResourceGroupName <resource-group-name> -Name <search-service-name> -PartitionCount 6 -ReplicaCount 6
```

Sonuç aşağıdaki çıktıya benzer olmalıdır.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 6
PartitionCount    : 6
HostingMode       : Default
Id                : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```


## <a name="next-steps"></a>Sonraki adımlar

Derleme bir [dizin](search-what-is-an-index.md), [dizin sorgulama](search-query-overview.md) portalı, REST API'leri veya .NET SDK kullanarak.

* [Azure portalında bir Azure Search dizini oluşturma](search-create-index-portal.md)
* [Diğer hizmetlerden veri yüklemek için bir dizin oluşturucu ' ayarlayın](search-indexer-overview.md)
* [Arama Gezgini'ni kullanarak Azure portalında bir Azure Search dizinini sorgulama](search-explorer.md)
* [.NET’te Azure Search kullanma](search-howto-dotnet-sdk.md)