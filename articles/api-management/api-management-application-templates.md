---
title: "Uygulama Şablonları Azure API Management | Microsoft Docs"
description: "Azure API Management'ta Geliştirici Portalı'nda uygulama sayfaları içeriğini özelleştirmeyi öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: f3122c4d-e10e-4cdf-977b-36e8f4133fc8
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 40a4d159afff4db78e27f89b448e21b1d94cd378
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="application-templates-in-azure-api-management"></a>Azure API Management'te uygulama şablonları
Azure API Management Geliştirici portal sayfalarına içeriklerini yapılandırma şablonları kümesini kullanarak içeriği özelleştirme yeteneği sağlar. Kullanarak [DotLiquid](http://dotliquidmarkup.org/) sözdizimi ve düzenleyiciyi, gibi [DotLiquid tasarımcıları için](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), ve sağlanan bir dizi yerelleştirilmiş [dize kaynakları](api-management-template-resources.md#strings), [karakter kaynakları](api-management-template-resources.md#glyphs), ve [sayfa denetimleri](api-management-page-controls.md), bu şablonları kullanarak uygun gördüğünüz şekilde sayfaların yapılandırmak için büyük esneklik vardır.  
  
 Bu bölümdeki şablonları Geliştirici Portalı'nda uygulama sayfaları içeriğini özelleştirmenize olanak sağlar.  
  
-   [Uygulama listesi](#ProductList)  
  
-   [Uygulama](#Application)  
  
> [!NOTE]
>  Örnek varsayılan şablonları aşağıdaki belgelerde yer alır ancak değişikliği sürekli geliştirmeler nedeniyle tabidir. İstenen tek tek şablonları giderek Geliştirici Portalı'nda Canlı varsayılan şablonları görüntüleyebilirsiniz. Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
##  <a name="ProductList"></a>Uygulama listesi  
 **Uygulama listesini** şablonu Geliştirici Portalı'nda Uygulama listesi sayfası gövdesi özelleştirmenizi sağlar.  
  
 ![Uygulama listesi sayfasında Geliştirici Portalı şablonları](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM uygulama listesi sayfasında Geliştirici Portalı şablonları")  
  
### <a name="default-template"></a>Varsayılan şablonu  
  
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
 `Product list` Şablonu, aşağıdaki kullanabilir [sayfasında denetimleri](api-management-page-controls.md).  
  
-   [disk belleği denetimi](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Sayfalama|[Disk belleği](api-management-template-data-model-reference.md#Paging) varlık.|Uygulamaları koleksiyonu için disk belleği bilgileri.|  
|Uygulamalar|Koleksiyonu [uygulama](api-management-template-data-model-reference.md#Application) varlıklar.|Geçerli kullanıcı için görünür olan uygulamalar.|  
|CategoryName|Dize|Uygulama kategorisi.|  
  
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
  
##  <a name="Application"></a>Uygulama  
 **Uygulama** şablonu Geliştirici Portalı'nda uygulama sayfası gövdesi özelleştirmenizi sağlar.  
  
 ![Uygulama sayfası Geliştirici Portalı şablonları](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM uygulama sayfasında Geliştirici Portalı şablonları")  
  
### <a name="default-template"></a>Varsayılan şablonu  
  
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
 `Application` Şablonu herhangi kullanılmasına izin verme [sayfasında denetimleri](api-management-page-controls.md).  
  
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
Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](api-management-developer-portal-templates.md).