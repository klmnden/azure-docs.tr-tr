---
title: Azure API Management sayfası denetimleri | Microsoft Docs
description: Azure API Management'ta Geliştirici Portalı şablonlarındaki kullanıma sayfa denetimleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2017
ms.author: apimpm
ms.openlocfilehash: da68c9b7ebbb1880e35bd60b12db9f920f51e13c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-api-management-page-controls"></a>Azure API Management sayfası denetimleri
Azure API Management portal şablonları aşağıdaki denetimleri Geliştirici kullanmak için sağlar.  
  
Bir denetimi kullanmak için Geliştirici Portalı şablonuna istediğiniz konuma yerleştirin. Gibi bazı denetimleri [uygulama eylemleri](#app-actions) denetlemek, parametreleri, aşağıdaki örnekte gösterildiği gibi sahiptir:  
  
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
  
##  <a name="app-actions"></a> Uygulama eylemleri  
 `app-actions` Denetimi Geliştirici Portalı'ndaki kullanıcı profili sayfasında uygulamalarla etkileşim için bir kullanıcı arabirimi sağlar.  
  
 ![Uygulama&#45;Eylemler denetimi](./media/api-management-page-controls/APIM-app-actions-control.png "APIM uygulama eylemleri denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|AppID|Uygulama kimliği.|  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `app-actions` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılan:  
  
-   [Uygulamalar](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a> temel oturum açma  
 `basic-signin` Denetim Geliştirici Portalı'nda oturum açma sayfasındaki kullanıcı oturum açma bilgileri toplamak için bir denetim sağlar.  
  
 ![temel&#45;signın denetim](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic oturum açma denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `basic-signin` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılan:  
  
-   [Oturum Aç](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a> disk belleği denetimi  
 `paging-control` Sayfalama işlevselliğinin Geliştirici öğelerinin bir listesini görüntülemek portal sayfalarına sağlar.  
  
 ![Denetim disk belleği](./media/api-management-page-controls/APIM-paging-control.png "APIM disk belleği denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `paging-control` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılan:  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Sorun listesi](api-management-issue-templates.md#IssueList)  
  
-   [Ürün Listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a> sağlayıcıları  
 `providers` Denetim seçimi Geliştirici Portalı'nda oturum açma sayfasındaki kimlik doğrulama sağlayıcıları için bir denetim sağlar.  
  
 ![sağlayıcıları Denetim](./media/api-management-page-controls/APIM-providers-control.png "APIM sağlayıcıları denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `providers` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılan:  
  
-   [Oturum Aç](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a> arama denetimi  
 `search-control` Arama işlevini Geliştirici öğelerinin bir listesini görüntülemek portal sayfalarına sağlar.  
  
 ![Arama denetim](./media/api-management-page-controls/APIM-search-control.png "APIM arama denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `search-control` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılan:  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Ürün Listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a> Kaydolma  
 `sign-up` Denetim Geliştirici Portalı'ndaki kaydolma sayfasında kullanıcı profili bilgileri toplamak için bir denetim sağlar.  
  
 ![oturum&#45;yukarı denetim](./media/api-management-page-controls/APIM-sign-up-control.png "APIM kayıt denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `sign-up` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılan:  
  
-   [Kaydol](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a> Abone düğmesi  
 `subscribe-button` Bir kullanıcı bir ürüne abone olmak için bir denetim sağlar.  
  
 ![abone&#45;düğme denetim](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM abone düğmesi denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `subscribe-button` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılan:  
  
-   [Ürün](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a> aboneliği iptal etme  
 `subscription-cancel` Denetim Geliştirici Portalı'ndaki kullanıcı profili sayfasında ürün aboneliği iptal etmek için bir denetim sağlar.  
  
 ![Abonelik&#45;denetim iptal](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM aboneliği iptal denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|subscriptionId|İptal etmek için abonelik kimliği.|  
|CancelUrl|Abonelik URL iptal eder.|  
  
### <a name="developer-portal-templates"></a>Geliştirici Portalı şablonları  
 `subscription-cancel` Denetimi aşağıdaki Geliştirici Portalı şablonlarında kullanılan:  
  
-   [Ürün](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Sonraki adımlar
Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](api-management-developer-portal-templates.md).