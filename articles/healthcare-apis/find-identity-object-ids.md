---
title: Kimlik, kimlik doğrulama - Azure API FHIR için nesne kimliklerini bulun.
description: Bu makalede, Azure API FHIR kimlik doğrulamasını yapılandırmak için gerekli kimlikleri kimlik nesneyi bulmak açıklanmaktadır
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: b120b21a89a7105a25ba610402da99f9dce4b7e1
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824302"
---
# <a name="find-identity-object-ids-for-authentication-configuration"></a>Kimlik doğrulama yapılandırması için nesne kimliklerini kimliği bulunamadı

Bu makalede, izin verilen kimlik nesne kimlikleri listesi için Azure API için FHIR yapılandırmak için gereken nesne kimlikleri kimliği bulma öğreneceksiniz.

Tam olarak yönetilen Azure API'si FHIR&reg; hizmeti erişim kimlik önceden tanımlanmış bir listesi için nesne kimliklerini izin verecek şekilde yapılandırıldı. Bir uygulama veya kullanıcıya FHIR API erişmeye çalışırken, bir taşıyıcı belirteç sunulmalıdır. Bu taşıyıcı belirteç belirli talepler (alanlar) sahip olacaktır. FHIR API'sine erişim vermek için belirteç doğru veren içermelidir (`iss`), İzleyici (`aud`) ve bir nesne kimliği (`oid`) izin verilen nesne kimlikleri listesi. Bir nesne kimliği, bir kullanıcının nesne kimliği veya bir hizmet sorumlusu Azure Active Directory'de ' dir.

## <a name="configure-list-of-allowed-object-ids"></a>İzin verilen nesne kimlikleri listesi yapılandırma

FHIR örneği için yeni bir Azure API oluşturduğunuzda, izin verilen nesne kimlikleri listesini yapılandırabilirsiniz:

![İzin verilen nesne kimlikleri yapılandırın](media/quickstart-paas-portal/configure-allowed-oids.png)

Bu nesne kimlikleri, belirli kullanıcılar veya Azure Active Directory Hizmet sorumluları için kimlikleri ya da olabilir.

## <a name="find-user-object-id-using-powershell"></a>PowerShell kullanarak kullanıcı nesne kimliği bulunamıyor

Kullanıcı adına sahip bir kullanıcı varsa `myuser@consoso.com`, kullanıcıların bulabilirsiniz `ObjectId` aşağıdaki PowerShell komutunu kullanarak:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

veya Azure CLI'yı kullanabilirsiniz:

```azurecli-interactive
az ad user show --upn-or-object-id myuser@consoso.com | jq -r .objectId
```

## <a name="find-service-principal-object-id-using-powershell"></a>PowerShell kullanarak hizmet sorumlusu nesne kimliği bulunamadı

Kayıtlı olduğunu varsayın bir [hizmeti istemci uygulaması](register-service-azure-ad-client-app.md) ve FHIR için Azure API'sine erişmesini sağlar, bu hizmeti istemcisi izin vermek istiyorsanız, aşağıdaki PowerShell komutu ile hizmet sorumlusu nesne kimliği için istemci bulabilirsiniz:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "AppId eq 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'").ObjectId
```

Burada `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX` hizmeti istemci uygulaması kimliği. Alternatif olarak, `DisplayName` hizmet istemcisi:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "DisplayName eq 'testapp'").ObjectId
```

Azure CLI kullanıyorsanız kullanabilirsiniz:

```azurecli-interactive
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX | jq -r .objectId
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kimlik nesnesinin FHIR örneği için bir Azure API'sine erişim izni olan kimlik yapılandırmak için gerekli kimlikleri bulmak öğrendiniz. Sonraki tam olarak yönetilen bir Azure API için FHIR dağıtın:
 
>[!div class="nextstepaction"]
>[Açık kaynak FHIR sunucusu dağıtma](fhir-paas-portal-quickstart.md)