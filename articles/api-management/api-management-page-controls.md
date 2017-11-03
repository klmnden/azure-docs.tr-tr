---
title: "Azure API Management sayfası denetimleri | Microsoft Docs"
description: "Azure API Management'ta Geliştirici Portalı şablonlarındaki kullanıma sayfa denetimleri hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 03e0ac8d-64ff-4e9a-b029-d7be14fb31e3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 6aa7a25a9addceee78abe027fb3a19351940464e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-api-management-page-controls"></a>Azure API Management sayfası denetimleri
Azure API Management portal şablonları aşağıdaki denetimleri Geliştirici kullanmak için sağlar.  
  
 Bir denetimi kullanmak için Geliştirici Portalı şablonuna istediğiniz konuma yerleştirin. Gibi bazı denetimleri [uygulama eylemleri](#app-actions) denetlemek, parametreleri, aşağıdaki örnekte gösterildiği gibi sahiptir.  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 Parametreler için değerler olarak şablonu için veri modelinin parçası olarak geçirilir. Çoğu durumda, yalnızca düzgün çalışabilmesi için her denetim için sağlanan örnekte yapıştırabilirsiniz. Parametre değerleri hakkında daha fazla bilgi için denetim kullanılabilir her şablon için veri modeli bölümünde görebilirsiniz.  
  
 Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
## <a name="developer-portal-template-page-controls"></a>Geliştirici Portalı şablonuna sayfa denetimleri  
  
-   [Uygulama eylemleri](#app-actions)  
  
-   [temel oturum açma](#basic-signin)  
  
-   [disk belleği denetimi](#paging-control)  
  
-   [sağlayıcıları](#providers)  
  
-   [arama denetimi](#search-control)  
  
-   [Kaydolma](#sign-up)  
  
-   [Abone düğmesi](#subscribe-button)  
  
-   [aboneliği iptal etme](#subscription-cancel)  
  
##  <a name="app-actions"></a>Uygulama eylemleri  
 `app-actions` Denetimi Geliştirici Portalı'ndaki kullanıcı profili sayfasında uygulamalarla etkileşim için bir kullanıcı arabirimi sağlar.  
  
 ![uygulamanın &#45; Eylemler denetim](./media/api-management-page-controls/APIM-app-actions-control.png "APIM uygulama eylemleri denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|AppID|Uygulama kimliği.|  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `app-actions` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılır.  
  
-   [Uygulamalar](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a>temel oturum açma  
 `basic-signin` Denetimi, kullanıcı oturum açma oturum açma sayfası Geliştirici Portalı'ndaki bilgileri toplamak için bir denetim sağlar.  
  
 ![Basic &#45; oturum açma denetimi](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic oturum açma denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parametreler  
 yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `basic-signin` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılır.  
  
-   [Oturum Aç](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a>disk belleği denetimi  
 `paging-control` Sayfalama işlevselliğinin Geliştirici öğelerinin bir listesini görüntülemek portal sayfalarına sağlar.  
  
 ![Denetim disk belleği](./media/api-management-page-controls/APIM-paging-control.png "APIM disk belleği denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `paging-control` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılır.  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Sorun listesi](api-management-issue-templates.md#IssueList)  
  
-   [Ürün Listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a>sağlayıcıları  
 `providers` Denetim, oturum açma sayfasında Geliştirici Portalı'nda kimlik doğrulama sağlayıcıları seçimi için bir denetim sağlar.  
  
 ![sağlayıcıları Denetim](./media/api-management-page-controls/APIM-providers-control.png "APIM sağlayıcıları denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parametreler  
 yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `providers` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılır.  
  
-   [Oturum Aç](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a>arama denetimi  
 `search-control` Arama işlevini Geliştirici öğelerinin bir listesini görüntülemek portal sayfalarına sağlar.  
  
 ![Arama denetim](./media/api-management-page-controls/APIM-search-control.png "APIM arama denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `search-control` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılır.  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Ürün Listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a>Kaydolma  
 `sign-up` Denetim kayıt sayfasını Geliştirici Portalı'nda kullanıcı profili bilgilerini toplamak için bir denetim sağlar.  
  
 ![oturum &#45; yukarı denetim](./media/api-management-page-controls/APIM-sign-up-control.png "APIM kayıt denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parametreler  
 yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `sign-up` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılır.  
  
-   [Kaydol](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a>Abone düğmesi  
 `subscribe-button` Bir kullanıcı bir ürüne abone olmak için bir denetim sağlar.  
  
 ![Abone &#45; düğme denetimi](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM abone düğmesi denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parametreler  
 yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `subscribe-button` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılır.  
  
-   [Ürün](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a>aboneliği iptal etme  
 `subscription-cancel` Denetim Geliştirici Portalı'ndaki kullanıcı profili sayfasında ürün aboneliği iptal etmek için bir denetim sağlar.  
  
 ![Abonelik &#45; denetim iptal](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM aboneliği iptal denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|subscriptionId|İptal etmek için abonelik kimliği.|  
|CancelUrl|Aboneliği iptal URL'si.|  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `subscription-cancel` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılır.  
  
-   [Ürün](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Sonraki adımlar
Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](api-management-developer-portal-templates.md).