---
title: "Azure Portal&quot;ı kullanarak Azure Search Dizininizi sorgulama | Microsoft Belgeleri"
description: "Azure Portal&quot;ın Arama Gezgini&quot;ninde arama sorgusu gönderin."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a23372112e17703a3399e1bdc9eaf73b85a1f80d


---
# <a name="query-your-azure-search-index-using-the-azure-portal"></a>Azure Portal'ı kullanarak Azure Search dizininizi sorgulama
> [!div class="op_single_selector"]
> * [Genel Bakış](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Bu kılavuz, Azure Portal'da Azure Search dizininizi nasıl sorgulayacağınızı gösterecektir.

Bu kılavuzda başlamadan önce, [Azure Search dizini oluşturmuş](search-what-is-an-index.md) ve [bunu verilerle doldurmuş](search-what-is-data-import.md) olmanız gerekir.

## <a name="i-go-to-your-azure-search-blade"></a>I. Azure Search dikey pencerenize gitme
1. [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)'ın sol tarafındaki menüde "Tüm kaynaklar"a tıklama
2. Azure Search hizmetinizi seçme

## <a name="ii-select-the-index-you-would-like-to-search"></a>II. Aramak istediğiniz dizini seçme
1. "Dizinler" kutucuğunda aramak istediğiniz dizini seçin.

![](./media/search-explorer/pick-index.png)

## <a name="iii-click-on-the-search-explorer-tile"></a>III. "Arama Gezgini" kutucuğuna tıklama
![](./media/search-explorer/search-explorer-tile.png)

## <a name="iii-start-searching"></a>III. Aramayı başlatma
1. Azure Search dizininizi aramak için "*Sorgu dizesi*" alanına yazmaya başlayın ve ardından "**Ara**"'ya basın.
   
   * Arama Gezgini'ni kullanırken bir [sorgu parametreleri](https://msdn.microsoft.com/library/dn798927.aspx)'den herhangi birini belirtebilirsiniz
2. "*Sonuçlar*" bölümünde, sorgunun sonuçları, Azure Search REST API'sine arama istekleri gönderdiğinizde HTTP Yanıt Gövdesi içinde aldığınız ham JSON'da temsil edilir.
3. Azure Search REST API'sine HTTP isteği göndermek için, sorgu dizesi uygun istek URL'si içine otomatik olarak ayrıştırılır.

![](./media/search-explorer/search-bar.png)




<!--HONumber=Nov16_HO2-->


