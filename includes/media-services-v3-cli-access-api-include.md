---
title: include dosyası
description: include dosyası
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 04/13/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: acb9bdf294dd66005df203f957c155540b658698
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
## <a name="access-the-media-services-api"></a>Media Services API’sine erişim

Azure AD hizmet sorumlusu kimlik doğrulamasını kullanarak Azure Media Services API’sine bağlanın. Aşağıdaki komut bir Azure AD uygulaması oluşturur ve hesaba bir hizmet sorumlusu ekler. Döndürülen değerleri aşağıdaki adımda gösterildiği gibi .NET uygulamanızı yapılandırmak için kullanacaksınız.

Betiği çalıştırmadan önce, `amsaccount` ve `amsResourceGroup` değerlerini bu kaynakları oluştururken seçtiğiniz adlarla değiştirebilirsiniz. `amsaccount`, hizmet sorumlusunu ekleyeceğiniz Azure Media Services hesabının adıdır. <br/>Ardından gelen komut, app.config dosyanıza yapıştırabileceğiniz bir xml döndüren `xml` seçeneğini kullanır. `xml` seçeneğini atlarsanız, yanıt `json` biçiminde olacak.

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```

Bu komut, aşağıdakine benzer bir yanıt oluşturur:

```xml
<add key="Region" value="West US 2" />
<add key="ResourceGroup" value="amsResourceGroup" />
<add key="AadEndpoint" value="https://login.microsoftonline.com" />
<add key="AccountName" value="amsaccount" />
<add key="SubscriptionId" value="111111111-0000-2222-3333-55555555555" />
<add key="ArmAadAudience" value="https://management.core.windows.net/" />
<add key="AadTenantId" value="2222222222-0000-2222-3333-6666666666666" />
<add key="AadSecret" value="33333333-0000-2222-3333-55555555555" />
<add key="AadClientId" value="44444444-0000-2222-3333-55555555555" />
<add key="ArmEndpoint" value="https://management.azure.com/" />
```

### <a name="configure-the-sample-app"></a>Örnek uygulamayı yapılandırma

Uygulamayı çalıştırıp Media Services API'lerine erişmek için App.config dosyasında doğru erişim değerlerini belirtmeniz gerekir. 

1. Visual Studio'yu açın.
2. Kopyaladığınız çözüme göz atın.
3. Çözüm Gezgini'nde *EncodeAndStreamFiles* projesini açın.
4. Bu projeyi başlangıç projesi olarak ayarlayın.
5. App.config dosyasını açın.
6. appSettings değerlerini önceki adımda aldığınız değerlerle değiştirin.

 ```xml
 <add key="Region" value="value" />
 <add key="ResourceGroup" value="value" />
 <add key="AadEndpoint" value="value" />
 <add key="AccountName" value="value" />
 <add key="SubscriptionId" value="value" />
 <add key="ArmAadAudience" value="value" />
 <add key="AadTenantId" value="value" />
 <add key="AadSecret" value="value" />
 <add key="AadClientId" value="value" />
 <add key="ArmEndpoint" value="value" />
 ```    
 
7. Çözümü oluşturmak için CTRL+Shift+B'ye basın.
