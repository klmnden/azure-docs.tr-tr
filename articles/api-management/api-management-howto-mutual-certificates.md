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
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 844a7ea1c2dd8f7dbb4984fc148575529ac154db
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36292868"
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Arka uç hizmetlerini kullanan istemci sertifikası kimlik doğrulaması Azure API Management'te güvenliğini sağlama

İstemci sertifikalarını kullanan bir API'nin arka uç hizmetine erişim güvenliğini sağlamak için API Management sağlar. Bu kılavuz, Azure portalında Azure API Management hizmet örneğindeki sertifikaların nasıl yönetileceğini göstermektedir. Ayrıca, arka uç hizmetine erişmek için bir sertifika kullanmak üzere bir API yapılandırmak nasıl açıklar.

API Management REST API kullanarak sertifikaların yönetilmesi hakkında daha fazla bilgi için bkz: <a href="https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-certificate-entity">Azure API Management REST API sertifikası varlık</a>.

## <a name="prerequisites"> </a>Önkoşulları

Bu kılavuz bir API arka uç hizmetine erişim için istemci sertifikası kimlik doğrulamasını kullanmak için API Management hizmet örneği yapılandırmak nasıl gösterir. Bu makaledeki adımları izlemeden önce arka uç hizmeti istemci sertifikası kimlik doğrulaması için yapılandırılmış olması gerekir ([Azure Web siteleri sertifika kimlik doğrulaması yapılandırmak için bu makalesine başvurun] [ to configure certificate authentication in Azure WebSites refer to this article]). API Management hizmeti yüklemek için sertifika ve parola erişmeniz gerekir.

## <a name="step1"> </a>Bir istemci sertifikasını karşıya yükle

![İstemci sertifikaları Ekle](media/api-management-howto-mutual-certificates/apim-client-cert.png)

Yeni bir istemci sertifikası yüklemek için aşağıdaki adımları izleyin. Henüz bir API Management hizmeti örneği oluşturmadıysanız, öğretici bkz [bir API Management hizmet örneği oluşturma][Create an API Management service instance].

1. Azure portalında Azure API Management hizmeti örneğine gidin.
2. Seçin **istemci sertifikalarını** menüsünde.
3. Tıklatın **+ Ekle** düğmesi.  
    ![İstemci sertifikaları Ekle](media/api-management-howto-mutual-certificates/apim-client-cert-add.png)  
4. Sertifika için göz atın, kendi Kimliğini ve parolasını girin.  
5. **Oluştur**’a tıklayın.

> [!NOTE]
> Sertifika olmalıdır **.pfx** biçimi. Otomatik olarak imzalanan sertifikalar izin verilir.

Sertifika yüklendikten sonra gösterir **istemci sertifikalarını**.  Birden çok sertifika varsa, aşağıdakileri yapmak için istenen sertifikanın parmak izini Not [bir API Ağ Geçidi kimlik doğrulaması için bir istemci sertifikası kullanacak şekilde yapılandırma][Configure an API to use a client certificate for gateway authentication].

> [!NOTE]
> Sertifika zinciri doğrulamasını devre dışı, örneğin, kullanırken otomatik olarak imzalanan bir sertifika açmak için bu SSS bölümünde açıklanan adımları izleyin [öğesi](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Bir istemci sertifikası silme

Bir sertifikayı silmek için bağlam menüsünü tıklatın **...**  seçip **silmek** sertifikanın yanındaki.

![İstemci sertifikaları sil](media/api-management-howto-mutual-certificates/apim-client-cert-delete.png)

Sertifika bir API tarafından kullanılıyorsa, bir uyarı ekranı görüntülenir. Sertifikayı silmek için sertifikayı kullanmak üzere yapılandırılmış tüm API'lerinin kaldırmanız gerekir.

![İstemci sertifikaları hatası Sil](media/api-management-howto-mutual-certificates/apim-client-cert-delete-failure.png)

## <a name="step2"> </a>Bir API Ağ Geçidi kimlik doğrulaması için bir istemci sertifikası kullanacak şekilde yapılandırma

1. Tıklatın **API'leri** gelen **API Management** soldaki menüde ve API için gidin.  
    ![İstemci sertifikalarını etkinleştirme](media/api-management-howto-mutual-certificates/apim-client-cert-enable.png)

2. İçinde **tasarım** sekmesinde, bir Kurşun Kalem simgesine tıklayın **arka uç** bölümü. 
3. Değişiklik **Ağ Geçidi kimlik** için **istemci sertifikası** ve aşağı açılır listeden sertifikanızı seçin.  
    ![İstemci sertifikalarını etkinleştirme](media/api-management-howto-mutual-certificates/apim-client-cert-enable-select.png)

4. **Kaydet**’e tıklayın. 

> [!WARNING]
> Bu değişikliği hemen etkili olur ve bu API'nin işlemlerini çağrıları arka uç sunucusunda kimlik doğrulaması için sertifika kullanır.


> [!TIP]
> Ağ Geçidi kimlik doğrulaması bir API için arka uç hizmeti için bir sertifika belirtildiğinde, bu API için ilkesinin bir parçası olur ve İlke Düzenleyicisi'nde görüntülenebilir.

## <a name="self-signed-certificates"></a>Otomatik olarak imzalanan sertifikalar

Otomatik olarak imzalanan sertifikalar kullanıyorsanız, API Management'ı arka uç sistemi ile iletişim kurmak sırayla sertifika zinciri doğrulamasını devre dışı bırakmak gerekir. Aksi takdirde, 500 hata kodunu döndürür. Bunu yapılandırmak için kullanabileceğiniz [ `New-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/new-azurermapimanagementbackend) (için yeni arka uç) veya [ `Set-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/set-azurermapimanagementbackend) (için varolan arka uç) PowerShell cmdlet'lerini ve `-SkipCertificateChainValidation` parametresi `True`.

```
$context = New-AzureRmApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzureRmApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

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
