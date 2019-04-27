---
title: Azure API Management sayfasında denetimlerinin | Microsoft Docs
description: Sayfa denetimleri kullanılmak üzere Azure API Management'ta Geliştirici portal şablonları hakkında bilgi edinin.
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
ms.openlocfilehash: d87293d89e4009512494bf47f9742ea5901f909a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60656991"
---
# <a name="azure-api-management-page-controls"></a>Azure API Management sayfa denetimleri
Azure API Management portal şablonları aşağıdaki denetimleri kullanılmak üzere Geliştirici sağlar.  
  
Bir denetim kullanmak için Geliştirici Portalı şablonuna istediğiniz konuma yerleştirin. Gibi bazı denetimleri [uygulama eylemlerini](#app-actions) denetlemek, parametreleri, aşağıdaki örnekte gösterildiği gibi sahiptir:  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 Parametreler için değerleri şablon için veri modelinin bir parçası olarak geçirilir. Çoğu durumda, yalnızca düzgün çalışabilmesi için her denetim için verilen örnekte yapıştırabilirsiniz. Parametre değerleri hakkında daha fazla bilgi için bir denetim kullanılabilir her şablon için veri modeli bölümünde görebilirsiniz.  
  
 Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
## <a name="developer-portal-template-page-controls"></a>Geliştirici Portalı şablonuna sayfa denetimleri  
  
-   [Uygulama eylemleri](#app-actions)  
-   [temel oturum aç](#basic-signin)  
-   [sayfalama denetimi](#paging-control)  
-   [sağlayıcıları](#providers)  
-   [arama denetimi](#search-control)  
-   [Kaydolma](#sign-up)  
-   [Abone düğmesi](#subscribe-button)  
-   [aboneliği iptal etme](#subscription-cancel)  
  
##  <a name="app-actions"></a> Uygulama eylemleri  
 `app-actions` Denetim uygulamalar kullanıcı profili sayfasında Geliştirici Portalı ile etkileşim kurmak için bir kullanıcı arabirimi sağlar.  
  
 ![Uygulama&#45;Eylemler denetimi](./media/api-management-page-controls/APIM-app-actions-control.png "APIM uygulama eylemleri denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|appId|Uygulama kimliği.|  
  
### <a name="developer-portal-templates"></a>Geliştirici portal şablonları  
 `app-actions` Denetimi aşağıdaki Geliştirici Portalı şablonlarda kullanılabilir:  
  
-   [Uygulamalar](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a> temel oturum aç  
 `basic-signin` Denetimi, geliştirici portalında oturum açma sayfasındaki kullanıcı oturum açma bilgilerini toplamak için bir denetim sağlar.  
  
 ![temel&#45;signın denetimi](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM temel signın denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portal şablonları  
 `basic-signin` Denetimi aşağıdaki Geliştirici Portalı şablonlarda kullanılabilir:  
  
-   [Oturum Aç](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a> sayfalama denetimi  
 `paging-control` Sayfalama işlevselliğinin Geliştirici öğeleri listesini görüntüleyen portal sayfalarına sağlar.  
  
 ![Denetim sayfalama](./media/api-management-page-controls/APIM-paging-control.png "APIM sayfalama denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portal şablonları  
 `paging-control` Denetimi aşağıdaki Geliştirici Portalı şablonlarda kullanılabilir:  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Sorun listesi](api-management-issue-templates.md#IssueList)  
  
-   [Ürün Listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a> sağlayıcıları  
 `providers` Denetimi, geliştirici portalında oturum açma sayfasında kimlik doğrulama sağlayıcıları seçimi için bir denetim sağlar.  
  
 ![providers denetimi](./media/api-management-page-controls/APIM-providers-control.png "APIM providers denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portal şablonları  
 `providers` Denetimi aşağıdaki Geliştirici Portalı şablonlarda kullanılabilir:  
  
-   [Oturum Aç](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a> arama denetimi  
 `search-control` Arama işlevini Geliştirici öğeleri listesini görüntüleyen portal sayfalarına sağlar.  
  
 ![arama denetimi](./media/api-management-page-controls/APIM-search-control.png "APIM arama denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portal şablonları  
 `search-control` Denetimi aşağıdaki Geliştirici Portalı şablonlarda kullanılabilir:  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Ürün Listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a> Kaydolma  
 `sign-up` Denetim Geliştirici Portalı'ndaki kaydolma sayfasında kullanıcı profili bilgilerini toplamak için bir denetim sağlar.  
  
 ![oturum&#45;denetimi yukarı](./media/api-management-page-controls/APIM-sign-up-control.png "APIM kayıt denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portal şablonları  
 `sign-up` Denetimi aşağıdaki Geliştirici Portalı şablonlarda kullanılabilir:  
  
-   [Kaydolma](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a> Abone düğmesi  
 `subscribe-button` Bir kullanıcı bir ürüne abone için bir denetim sağlar.  
  
 ![abone&#45;düğme denetimi](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM abone düğme denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portal şablonları  
 `subscribe-button` Denetimi aşağıdaki Geliştirici Portalı şablonlarda kullanılabilir:  
  
-   [Ürün](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a> aboneliği iptal etme  
 `subscription-cancel` Denetimi, kullanıcı profili sayfasında Geliştirici Portalı bir ürün aboneliği iptal etmek için bir denetim sağlar.  
  
 ![Abonelik&#45;iptal denetimi](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM aboneliği iptal denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|subscriptionId|İptal etmek için abonelik kimliği.|  
|CancelUrl|Abonelik URL'si iptal eder.|  
  
### <a name="developer-portal-templates"></a>Geliştirici portal şablonları  
 `subscription-cancel` Denetimi aşağıdaki Geliştirici Portalı şablonlarda kullanılabilir:  
  
-   [Ürün](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Sonraki adımlar
Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](api-management-developer-portal-templates.md).