---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: f0cc888eaf3724737e9c868c69a641094a19348c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60297746"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Bir Windows Masaüstü uygulamasından Microsoft Graph API çağırma

Bu kılavuz, nasıl bir Microsoft kimlik Platformu (eski adıyla Azure AD olarak adlandırılır) geliştiriciler v2.0 uç noktası için bir yerel Windows Masaüstü .NET (uygulama erişim belirteci alma ve Microsoft Graph API'sini veya erişim gerektiren başka bir API çağrısının XAML) belirteçleri gösterir.

Kılavuzu tamamladıktan sonra uygulamanızın kişisel hesaplar (outlook.com, live.com ve diğerleri dahil) kullanan korumalı bir API'yi çağırmak mümkün olacaktır. Uygulama ayrıca iş ve Okul hesapları herhangi bir şirket veya Azure Active Directory kullanan kuruluş.  

> [!NOTE]
> Visual Studio 2015 güncelleştirme 3 veya Visual Studio 2017 Kılavuzu gerektirir. Bu sürümlerden birini yok mu? [Visual Studio 2017'yi ücretsiz olarak indirin](https://www.visualstudio.com/downloads/).

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?

![Örnek uygulama tarafından bu öğreticileri çalışır nasıl oluşturulacağını gösterir](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.svg)

Bu kılavuz ile oluşturduğunuz örnek uygulama, Microsoft Graph API'sini veya Microsoft kimlik platformu uç noktasından belirteçleri kabul eden bir Web API'si sorgular bir Windows masaüstü uygulaması sağlar. Bu senaryoda, yetkilendirme üst bilgisi ile HTTP istekleri için bir belirteç ekler. Microsoft Authentication Library (MSAL), belirteç edinme ve yenileme işlemini gerçekleştirir.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Web API'leri korumalı erişim belirteci alma işleme

Örnek uygulama, kullanıcının kimliği doğrulandıktan sonra Microsoft Graph API'si veya Microsoft kimlik platformu tarafından geliştiriciler için güvenli bir Web API'si sorgulamak için kullanılabilecek bir belirteç alır.

Belirli kaynaklara erişmesine izin vermek için bir belirteç gibi Microsoft Graph API'leri gerektirir. Örneğin, bir belirteç kullanıcı profilini okuyun, bir kullanıcının takvime erişip veya e-posta göndermek için gereklidir. Uygulamanız API kapsamları belirterek bu kaynaklara erişmek için MSAL kullanarak bir erişim belirteci isteyebilirsiniz. Bu erişim belirteci, ardından korumalı kaynağa karşı yapılan her çağrı için HTTP yetkilendirme üst bilgisi eklenir.

MSAL önbelleğe alma ve erişim belirteçleri, yenileme yönetir, uygulamanız gerekmez.

## <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuz, aşağıdaki NuGet paketlerini kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft kimlik doğrulama kitaplığı (MSAL.NET)|
