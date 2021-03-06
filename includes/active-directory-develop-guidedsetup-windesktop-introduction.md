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
ms.openlocfilehash: 301abe95b245603e5414eef84ce74cdc8de01d19
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509936"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Bir Windows Masaüstü uygulamasından Microsoft Graph API çağırma

Bu kılavuz, bir erişim belirteci yerel bir Windows Masaüstü .NET (XAML) uygulama Microsoft Graph API'sini çağırmak için nasıl kullandığını gösterir. Uygulama geliştiriciler v2.0 uç noktası için Microsoft kimlik platformu erişim belirteçlerini gerektiren diğer API'ler de erişebilirsiniz. Bu platform, eski adıyla Azure adlı AD.

Kılavuzu tamamladıktan sonra uygulamanızın kişisel hesaplar (outlook.com, live.com ve diğerleri dahil) kullanan korumalı bir API'yi çağırmak mümkün olacaktır. Uygulama ayrıca iş ve Okul hesapları herhangi bir şirket veya Azure Active Directory kullanan kuruluş.  

> [!NOTE]
> Visual Studio 2015 güncelleştirme 3, Visual Studio 2017 veya Visual Studio 2019 Kılavuzu gerektirir. Bu sürümden herhangi birinde yok mu? [Visual Studio 2019 ücretsiz olarak indirin](https://www.visualstudio.com/downloads/).

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.svg)

Bu kılavuz ile oluşturduğunuz örnek uygulama, Microsoft Graph API'sini veya Microsoft kimlik platformu uç noktasından belirteçleri kabul eden bir Web API'si sorgular bir Windows masaüstü uygulaması sağlar. Bu senaryoda, yetkilendirme üst bilgisi ile HTTP istekleri için bir belirteç ekler. Microsoft Authentication Library (MSAL), belirteç edinme ve yenileme işlemini gerçekleştirir.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Web API'leri korumalı erişim belirteci alma işleme

Örnek uygulama, kullanıcının kimliği doğrulandıktan sonra Microsoft Graph API'si veya Microsoft kimlik platformu tarafından geliştiriciler için güvenli bir Web API'si sorgulamak için kullanabileceğiniz bir belirteç alır.

Belirli kaynaklara erişmesine izin vermek için bir belirteç gibi Microsoft Graph API'leri gerektirir. Örneğin, bir belirteç kullanıcı profilini okuyun, bir kullanıcının takvime erişip veya e-posta göndermek için gereklidir. Uygulamanız API kapsamları belirterek bu kaynaklara erişmek için MSAL kullanarak bir erişim belirteci isteyebilirsiniz. Bu erişim belirteci, ardından korumalı kaynağa karşı yapılan her çağrı için HTTP yetkilendirme üst bilgisi eklenir.

MSAL önbelleğe alma ve erişim belirteçleri, yenileme yönetir, uygulamanız gerekmez.

## <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuz, aşağıdaki NuGet paketlerini kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft kimlik doğrulama kitaplığı (MSAL.NET)|
