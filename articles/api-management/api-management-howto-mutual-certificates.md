---
title: Arka uç hizmetlerini kullanan istemci güvenli sertifika kimlik doğrulaması - Azure API Management | Microsoft Docs
description: Azure API Management'te istemci sertifikası kimlik doğrulaması kullanarak arka uç hizmetlerini güvence altına alma hakkında bilgi edinin.
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
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: c3060765022cabcb877041927886b59d6725c7cf
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Arka uç hizmetlerini kullanan istemci sertifikası kimlik doğrulaması Azure API Management'te güvenliğini sağlama
API Management istemci sertifikalarını kullanan güvenli bir API'nin arka uç hizmetine erişim olanağı sağlar. Bu kılavuz, API yayımcı portalına sertifikaları yönetme ve arka uç hizmetine erişmek için bir sertifika kullanmak üzere bir API yapılandırma gösterir.

API Management REST API kullanarak sertifikaların yönetilmesi hakkında daha fazla bilgi için bkz: <a href="https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-certificate-entity">Azure API Management REST API sertifikası varlık</a>.

## <a name="prerequisites"> </a>Önkoşulları
Bu kılavuz bir API arka uç hizmetine erişim için istemci sertifikası kimlik doğrulamasını kullanmak için API Management hizmet örneği yapılandırmak nasıl gösterir. Bu konudaki adımları izlemeden önce arka uç hizmeti istemci sertifikası kimlik doğrulaması için yapılandırılmış olması gerekir ([Azure Web siteleri sertifika kimlik doğrulaması yapılandırmak için bu makalesine başvurun][to configure certificate authentication in Azure WebSites refer to this article]), ve sertifika ve parola için API Management yayımcı Portalı'nda yüklemeyle sertifikanın erişebilir.

## <a name="step1"> </a>Bir istemci sertifikasını karşıya yükle
Kullanmaya başlamak için API Management hizmetiniz için Azure Portal'da **Yayımcı portalı**’na tıklayın. Bu sizi API Management yayımcı portalına götürür.

![API yayımcı portalı][api-management-management-console]

> Henüz bir API Management hizmeti örneği oluşturmadıysanız, bkz: [bir API Management hizmet örneği oluşturma][Create an API Management service instance].
> 
> 

Tıklatın **güvenlik** gelen **API Management** sol ve tıklatın menüsünde **istemci sertifikalarını**.

![İstemci sertifikaları][api-management-security-client-certificates]

Yeni sertifikayı karşıya yüklemek için tıklayın **karşıya yükleme sertifika**.

![Sertifikayı karşıya yükleme][api-management-upload-certificate]

Sertifikanızı göz atın ve sonra sertifikanın parolasını girin.

> Sertifika olmalıdır **.pfx** biçimi. Otomatik olarak imzalanan sertifikalar izin verilir.
> 
> 

![Sertifikayı karşıya yükleme][api-management-upload-certificate-form]

Tıklatın **karşıya** sertifikayı karşıya yüklemek için.

> Sertifika parolası şu anda doğrulanır. Yanlış ise, bir hata iletisi görüntülenir.
> 
> 

![Sertifika karşıya yüklendi][api-management-certificate-uploaded]

Sertifika yüklendikten sonra göründüğü **istemci sertifikalarını** sekmesi. Birden fazla sertifika varsa, konu not ya da parmak izi sertifikalarını kullanmak için bir API aşağıdakileri anlatıldığı gibi yapılandırırken sertifikayı seçmek için kullanılan son dört karakterini olun [kullanmak için bir API'nin bir Ağ Geçidi kimlik doğrulaması için istemci sertifikası] [ Configure an API to use a client certificate for gateway authentication] bölümü.

> Sertifika zinciri doğrulamasını devre dışı, örneğin, kullanırken otomatik olarak imzalanan bir sertifika açmak için bu SSS bölümünde açıklanan adımları izleyin [öğesi](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).
> 
> 

## <a name="step1a"> </a>Bir istemci sertifikası silme
Bir sertifikayı silmek için tıklatın **silmek** istenen sertifikanın yanındaki.

![Sertifikayı sil][api-management-certificate-delete]

Tıklatın **Evet, silmeden** onaylamak için.

![Silmeyi onayla][api-management-confirm-delete]

Sertifika bir API tarafından kullanılıyorsa, bir uyarı ekranı görüntülenir. Sertifikayı silmek için sertifika kullanmak üzere yapılandırılmış tüm API'lerinin kaldırmanız gerekir.

![Silmeyi onayla][api-management-confirm-delete-policy]

## <a name="step2"> </a>Bir API Ağ Geçidi kimlik doğrulaması için bir istemci sertifikası kullanacak şekilde yapılandırma
Tıklatın **API'leri** gelen **API Management** , soldaki menüde istenen API adına tıklayın ve'ı tıklatın **güvenlik** sekmesi.

![API Güvenlik][api-management-api-security]

Seçin **istemci sertifikalarını** gelen **kimlik bilgileriyle** aşağı açılan liste.

![İstemci sertifikaları][api-management-mutual-certificates]

İstediğiniz sertifikayı seçin **istemci sertifikası** aşağı açılan liste. Birden fazla sertifika varsa doğru sertifikayı belirlemek için konu veya önceki bölümünde belirtildiği gibi parmak izi son dört karakterini bakabilirsiniz.

![Sertifika seçin][api-management-select-certificate]

Tıklatın **kaydetmek** API için yapılandırma değişikliği kaydetmek için.

> Bu değişikliği hemen etkili olur ve bu API'nin işlemlerini çağrıları arka uç sunucusunda kimlik doğrulaması için sertifika kullanır.
> 
> 

![API Değişiklikleri Kaydet][api-management-save-api]

> Ağ Geçidi kimlik doğrulaması bir API için arka uç hizmeti için bir sertifika belirtildiğinde, bu API için ilkesinin bir parçası olur ve İlke Düzenleyicisi'nde görüntülenebilir.
> 
> 

![Sertifika İlkesi][api-management-certificate-policy]

## <a name="self-signed-certificates"></a>Otomatik olarak imzalanan sertifikalar

Otomatik olarak imzalanan sertifikalar kullanıyorsanız, API Management'ı arka uç sistemi ile iletişim kurmak sırayla sertifika zinciri doğrulamasını devre dışı bırakmanız gerekir, aksi takdirde 500 hata kodunu döndürür. Bunu yapılandırmak için kullanabileceğiniz [ `New-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/new-azurermapimanagementbackend) (için yeni arka uç) veya [ `Set-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/set-azurermapimanagementbackend) (için varolan arka uç) PowerShell cmdlet'lerini ve `-SkipCertificateChainValidation` parametresi `True`.

```
$context = New-AzureRmApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzureRmApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[Azure API Management REST API Certificate entity]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[to configure certificate authentication in Azure WebSites refer to this article]: ../app-service/app-service-web-configure-tls-mutual-auth.md

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API to use a client certificate for gateway authentication]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps



