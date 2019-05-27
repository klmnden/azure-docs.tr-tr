---
title: Sertifika kimlik doğrulaması - Azure API Management istemcisini kullanarak arka uç hizmetlerini güvenli hale getirme | Microsoft Docs
description: Azure API Yönetimi'nde istemci sertifikası kimlik doğrulaması kullanarak arka uç hizmetlerini güvenli hale getirmeyi öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 13a2eb080c6822a8a6786be1952bc588fa8afd80
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66141532"
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Sertifika kimlik doğrulaması Azure API Management istemcisini kullanarak arka uç hizmetlerine güvenliğini sağlama

İstemci sertifikaları kullanarak güvenli erişim bir API için arka uç hizmeti için API yönetimi sağlar. Bu kılavuzda, Azure portalında Azure API Management hizmet örneği sertifikaları yönetme işlemi gösterilmektedir. Ayrıca, bir arka uç hizmetine erişmek için bir sertifika kullanmak üzere bir API yapılandırma açıklanmaktadır.

API Management REST API'sini kullanarak sertifikaların yönetilmesi hakkında daha fazla bilgi için bkz: <a href="https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-certificate-entity">Azure API Management REST API sertifikası varlık</a>.

## <a name="prerequisites"> </a>Önkoşulları

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu kılavuzda bir API için arka uç hizmetine erişmek için istemci sertifikası kimlik doğrulaması kullanmak için API Management hizmet örneğinizin yapılandırma gösterilmektedir. Bu makaledeki adımları izlemeden önce arka uç hizmetiniz, istemci sertifikası kimlik doğrulaması için yapılandırılmış olması gerekir ([Azure Web Siteleri'nde sertifika kimlik doğrulaması yapılandırmak için bu makaleye başvurun] [ to configure certificate authentication in Azure WebSites refer to this article]). API Management hizmeti için yüklemek için sertifika ve parola erişmeniz gerekir.

## <a name="step1"> </a>Bir istemci sertifikasını karşıya yükle

![İstemci sertifikaları Ekle](media/api-management-howto-mutual-certificates/apim-client-cert.png)

Yeni bir istemci sertifikası yüklemek için aşağıdaki adımları izleyin. Henüz bir API Management hizmet örneği oluşturmadıysanız, bkz [bir API Management hizmet örneği oluşturma][Create an API Management service instance].

1. Azure API Management hizmet örneğinizin Azure portalındaki gidin.
2. Seçin **istemci sertifikaları** menüsünde.
3. Tıklayın **+ Ekle** düğmesi.  
    ![İstemci sertifikaları Ekle](media/api-management-howto-mutual-certificates/apim-client-cert-add.png)  
4. Sertifika için göz atın, kendi Kimliğini ve parolasını belirtin.  
5. **Oluştur**’a tıklayın.

> [!NOTE]
> Sertifika olmalıdır **.pfx** biçimi. Otomatik olarak imzalanan sertifikalar izin verilir.

Sertifika karşıya yüklendikten sonra gösterir **istemci sertifikaları**.  Birden çok sertifika varsa için istenen sertifikanın parmak izini not alın [bir API Ağ Geçidi kimlik doğrulaması için bir istemci sertifikası kullanacak şekilde yapılandırma][Configure an API to use a client certificate for gateway authentication].

> [!NOTE]
> Otomatik olarak imzalanan bir sertifika kullanırken, örneğin, sertifika zinciri doğrulamasını devre dışı bırakmak için bu SSS'de açıklanan adımları izleyin. [öğesi](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Bir istemci sertifikasını Sil

Bir sertifikayı silmek için bağlam menüsünü tıklatın **...**  seçip **Sil** sertifikanın yanındaki.

![İstemci sertifikaları sil](media/api-management-howto-mutual-certificates/apim-client-cert-delete.png)

Ardından bir API tarafından sertifika ise, bir uyarı ekranı görüntülenir. Sertifikayı silmek için sertifikayı kullanmak üzere yapılandırılan tüm API'lerinden kaldırmanız gerekir.

![İstemci sertifikaları hatası Sil](media/api-management-howto-mutual-certificates/apim-client-cert-delete-failure.png)

## <a name="step2"> </a>Bir API Ağ Geçidi kimlik doğrulaması için bir istemci sertifikası kullanacak şekilde yapılandırma

1. Tıklayın **API'leri** gelen **API Management** sol taraftaki menüden ve API'ye gidin.  
    ![İstemci sertifikaları etkinleştir](media/api-management-howto-mutual-certificates/apim-client-cert-enable.png)

2. İçinde **tasarım** sekmesinde, üzerinde bir kalem simgesine tıklayın **arka uç** bölümü. 
3. Değişiklik **Ağ Geçidi kimlik** için **istemci sertifikası** ve açılan listeden, sertifika seçin.  
    ![İstemci sertifikaları etkinleştir](media/api-management-howto-mutual-certificates/apim-client-cert-enable-select.png)

4. **Kaydet**’e tıklayın. 

> [!WARNING]
> Bu değişiklik hemen etkili olur ve bu API'nin işlemlerini çağrıları arka uç sunucusunda kimlik doğrulaması için sertifika kullanır.


> [!TIP]
> Ağ Geçidi kimlik doğrulaması bir API için arka uç hizmeti için bir sertifika belirtildiğinde, bu API için ilkesinin bir parçası haline gelir ve ilke düzenleyicisinde görüntülenebilir.

## <a name="self-signed-certificates"></a>Otomatik olarak imzalanan sertifikalar

Otomatik olarak imzalanan sertifikalar kullanıyorsanız, API Management'ı arka uç sistemi ile iletişim kurmak için sırayla sertifika zinciri doğrulamasını devre dışı bırakmak gerekir. Aksi takdirde, 500 hata kodu döndürür. Bunu yapılandırmak için kullanabileceğiniz [ `New-AzApiManagementBackend` ](https://docs.microsoft.com/powershell/module/az.apimanagement/new-azapimanagementbackend) (için yeni arka uç) veya [ `Set-AzApiManagementBackend` ](https://docs.microsoft.com/powershell/module/az.apimanagement/set-azapimanagementbackend) (mevcut arka uç için) PowerShell cmdlet'leri ve `-SkipCertificateChainValidation` parametresi `True`.

```powershell
$context = New-AzApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
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

[Azure API Management REST API Certificate entity]: https://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[to configure certificate authentication in Azure WebSites refer to this article]: ../app-service/app-service-web-configure-tls-mutual-auth.md

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API to use a client certificate for gateway authentication]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps
