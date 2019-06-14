---
title: Azure Analysis Services diğer ad sunucusu adlarını | Microsoft Docs
description: Sunucu adları oluşturup kullanacağınızı açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 66e6b4713591f099769543a75dcddec34f3d2e2b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60534326"
---
# <a name="alias-server-names"></a>Diğer sunucu adları

Sunucu diğer adı kullanarak, kullanıcılar ile daha kısa bir Azure Analysis Services sunucunuza bağlanabilir *diğer* yerine sunucu adı. Bir istemci uygulamasından bağlanırken, diğer ad olarak bir uç noktayı kullanarak belirtilen **bağlantı: / /** Protokolü biçimi. Uç nokta, daha sonra bağlanmak için gerçek sunucu adını döndürür.

Diğer sunucu adları için uygundur:

- Kullanıcıları etkilemeden sunucular arasında geçişini modeller. 
- Kolay sunucu adları hatırlamanız kullanıcılar için daha kolay. 
- Günün farklı zamanlarda farklı sunuculara doğrudan kullanıcılara. 
- Kullanıcıları Azure Traffic Manager kullanırken gibi coğrafi olarak yakın, örnekleri farklı bölgelerde. 

Geçerli bir Azure Analysis Services sunucu adı döndürür herhangi bir HTTPS uç noktası, diğer ad olarak hizmet verebilir. Uç nokta bağlantı noktası 443 üzerinden HTTPS desteklemesi gerekir ve bağlantı noktası URI'de belirtilmemelidir.

![Bağlantı biçimi kullanarak diğer adı](media/analysis-services-alias/aas-alias-browser.png)

Bir istemciden bağlanırken, diğer sunucu adını kullanarak girilen **bağlantı: / /** Protokolü biçimi. Örneğin, Power BI Desktop'ta:

![Power BI Desktop bağlantısı](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Bir diğer ad oluştur

Diğer uç nokta oluşturmak için geçerli bir Azure Analysis Services sunucu adı döndüren herhangi bir yöntemi kullanabilirsiniz. Örneğin, gerçek sunucu içeren Azure Blob depolama alanındaki bir dosyaya bir başvuru adı veya oluşturma ve bir ASP.NET Web Forms uygulaması yayımlama.

Bu örnekte, Visual Studio'da ASP.NET Web Forms uygulaması oluşturulur. Ana Sayfa başvurusu ve kullanıcı denetimini Default.aspx sayfasından kaldırılır. Default.aspx içeriğini yalnızca aşağıdaki sayfa yönergesi verilmiştir:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

Default.aspx.cs Page_Load olayında, Azure Analysis Services sunucu adı döndürülecek Response.Write() yöntemi kullanır.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Ayrıca bkz.

[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Power BI Desktop'tan bağlanma](analysis-services-connect-pbi.md)
