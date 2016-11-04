---
title: Azure Portal'ı kullanarak Azure Search Dizininizi sorgulama | Microsoft Docs
description: Azure Portal'ın Arama Gezgini'ninde arama sorgusu gönderin.
services: search
documentationcenter: ''
author: ashmaka

ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka

---
# Azure Portal'ı kullanarak Azure Search dizininizi sorgulama
> [!div class="op_single_selector"]
> * [Genel Bakış](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Bu kılavuz, Azure Portal'da Azure Search dizininizi nasıl sorgulayacağınızı gösterecektir.

Bu kılavuzda başlamadan önce, [Azure Search dizini oluşturmuş](search-what-is-an-index.md) ve [bunu verilerle doldurmuş](search-what-is-data-import.md) olmanız gerekir.

## I. Azure Search dikey pencerenize gitme
1. [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)'ın sol tarafındaki menüde "Tüm kaynaklar"a tıklama
2. Azure Search hizmetinizi seçme

## II. Aramak istediğiniz dizini seçme
1. "Dizinler" kutucuğunda aramak istediğiniz dizini seçin.

![](./media/search-explorer/pick-index.png)

## III. "Arama Gezgini" kutucuğuna tıklama
![](./media/search-explorer/search-explorer-tile.png)

## III. Aramayı başlatma
1. Azure Search dizininizi aramak için "*Sorgu dizesi*" alanına yazmaya başlayın ve ardından "**Ara**"'ya basın.
   
   * Arama Gezgini'ni kullanırken bir [sorgu parametreleri](https://msdn.microsoft.com/library/dn798927.aspx)'den herhangi birini belirtebilirsiniz
2. "*Sonuçlar*" bölümünde, sorgunun sonuçları, Azure Search REST API'sine arama istekleri gönderdiğinizde HTTP Yanıt Gövdesi içinde aldığınız ham JSON'da temsil edilir.
3. Azure Search REST API'sine HTTP isteği göndermek için, sorgu dizesi uygun istek URL'si içine otomatik olarak ayrıştırılır.

![](./media/search-explorer/search-bar.png)

<!--HONumber=Sep16_HO3-->


