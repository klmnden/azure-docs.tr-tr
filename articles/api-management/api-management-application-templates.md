---
title: Azure API Yönetimi'nde uygulama şablonları | Microsoft Docs
description: Azure API Management'ta Geliştirici Portalı Uygulama sayfalarında içeriğini özelleştirmeyi öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: f3122c4d-e10e-4cdf-977b-36e8f4133fc8
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 732fdf3f9210a1484895e0b43e061b4bbc586b43
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60657960"
---
# <a name="application-templates-in-azure-api-management"></a>Azure API Yönetimi'nde uygulama şablonları
Azure API Management içeriklerini yapılandıran bir dizi kullanarak Geliştirici portal sayfalarının içeriğini özelleştirme becerisi sunuyor. Kullanarak [DotLiquid](http://dotliquidmarkup.org/) söz dizimi ve tercih ettiğiniz düzenleyiciyi gibi [tasarımcılarına yönelik DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), ve sağlanan bir dizi yerelleştirilmiş [dize kaynakları](api-management-template-resources.md#strings), [karakter Kaynakları](api-management-template-resources.md#glyphs), ve [sayfasında denetimleri](api-management-page-controls.md), sayfaların içeriğini bu şablonları kullanarak dilediğiniz şekilde yapılandırmak için harika esnekliğine sahip olursunuz.  
  
 Bu bölümdeki şablonları Geliştirici Portalı Uygulama sayfalarında içeriğini özelleştirmenizi sağlar.  
  
-   [Uygulama listesi](#ProductList)  
  
-   [Uygulama](#Application)  
  
> [!NOTE]
>  Örnek varsayılan şablonları aşağıdaki belgelerde bulunan, ancak sürekli geliştirmeler nedeniyle değiştirilebilir. İstenen bireysel şablonlara giderek Canlı varsayılan şablonları Geliştirici Portalı'nda görüntüleyebilirsiniz. Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="ProductList"></a> Uygulama listesi  
 **Uygulama listesi** şablon Geliştirici Portalı Uygulama listesinde sayfasının gövdesi özelleştirmenize olanak sağlar.  
  
 ![Uygulama listesi sayfasını Geliştirici Portal şablonları](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM uygulama listesi sayfasını Geliştirici Portal şablonları")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "AppStrings|WebApplicationsHeader" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if applications.size > 0 %}  
        <ul class="list-unstyled">  
        {% for app in applications %}  
            <li>  
            {% if app.application.icon.url != "" %}  
                <aside>  
                    <a href="/applications/details/{{app.application.id}}"><img src="{{app.application.icon.url}}" alt="App Icon" /></a>  
                </aside>  
            {% endif %}  
                <h3><a href="/applications/details/{{app.application.id}}">{{app.application.title}}</a></h3>  
                {{app.application.description}}  
            </li>     
        {% endfor %}  
        </ul>  
        <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a>Denetimler  
 `Product list` Şablon aşağıdaki kullanabilir [sayfasında denetimleri](api-management-page-controls.md).  
  
-   [sayfalama denetimi](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Paging`|[Disk belleği](api-management-template-data-model-reference.md#Paging) varlık.|Uygulama koleksiyonu için sayfalandırma bilgileri.|  
|`Applications`|Koleksiyonu [uygulama](api-management-template-data-model-reference.md#Application) varlıklar.|Geçerli kullanıcıya görünür olan uygulamalar.|  
|`CategoryName`|string|Uygulama kategorisi.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Applications": [  
        {  
            "Application": {  
                "Id": "5702b96fb16653124c8f9ba8",  
                "Title": "Contoso Calculator",  
                "Description": "A simple online calculator.",  
                "Url": null,  
                "Version": null,  
                "Requirements": "Free application with no requirements.",  
                "State": 2,  
                "RegistrationDate": "2016-04-04T18:59:00",  
                "CategoryId": 5,  
                "DeveloperId": "5702b5b0b16653124c8f9ba4",  
                "Attachments": [  
                    {  
                        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                        "Type": "Icon",  
                        "ContentType": "image/png"  
                    },  
                    {  
                        "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
                        "Type": "Screenshot",  
                        "ContentType": "image/png"  
                    }  
                ],  
                "Icon": {  
                    "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                    "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                    "Type": "Icon",  
                    "ContentType": "image/png"  
                }  
            },  
            "CategoryName": "Finance"  
        }  
    ]  
}  
```  
  
##  <a name="Application"></a> Uygulama  
 **Uygulama** şablon Geliştirici Portalı'nda uygulama sayfasının gövdesi özelleştirmenize olanak sağlar.  
  
 ![Uygulama sayfası Geliştirici Portal şablonları](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM uygulama sayfası Geliştirici Portal şablonları")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<h2>{{title}}</h2>  
{% if icon.url != "" %}  
<aside class="applications_aside">  
  <div class="image-placeholder">  
    <img src="{{icon.url}}" alt="Application Icon" />  
  </div>  
</aside>  
{% endif %}  
  
<article>  
  {% if url != "" %}  
    <a target="_blank" href="{{url}}">{{url}}</a>  
  {% endif %}  
  
  <p>{{description}}</p>  
  
  {% if requirements != null %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsRequirementsHeader" %}</h3>  
  <p>{{requirements}}</p>  
  {% endif %}  
  
  {% if attachments.size > 0 %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsScreenshotsHeader" %}</h3>  
    {% for screenshot in attachments %}  
      {% if screenshot.type != "Icon" %}  
      <a href="{{screenshot.url}}" data-lightbox="example-set">  
          <img src="/Developer/Applications/Thumbnail?url={{screenshot.url}}" alt='{% localized "AppDetailsStrings|WebApplicationsScreenshotAlt" %}' />  
      </a>  
      {% endif %}  
    {% endfor %}  
  {% endif %}  
</article>  
  
```  
  
### <a name="controls"></a>Denetimler  
 `Application` Şablon kullanımı, izin vermiyor [sayfasında denetimleri](api-management-page-controls.md).  
  
### <a name="data-model"></a>Veri modeli  
 [Uygulama](api-management-template-data-model-reference.md#Application) varlık.  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Id": "5702b96fb16653124c8f9ba8",  
    "Title": "Contoso Calculator",  
    "Description": "A simple online calculator.",  
    "Url": null,  
    "Version": null,  
    "Requirements": "Free application with no requirements.",  
    "State": 2,  
    "RegistrationDate": "2016-04-04T18:59:00",  
    "CategoryId": 5,  
    "DeveloperId": "5702b5b0b16653124c8f9ba4",  
    "Attachments": [  
        {  
            "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
            "Type": "Icon",  
            "ContentType": "image/png"  
        },  
        {  
            "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
            "Type": "Screenshot",  
            "ContentType": "image/png"  
        }  
    ],  
    "Icon": {  
        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
        "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
        "Type": "Icon",  
        "ContentType": "image/png"  
    }  
}  
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](api-management-developer-portal-templates.md).
