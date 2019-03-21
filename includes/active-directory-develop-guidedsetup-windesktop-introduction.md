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
ms.date: 03/20/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: bb28862ad6452eab3130eeb2dc0b4c269839d306
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203231"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Bir Windows Masaüstü uygulamasından Microsoft Graph API çağırma

Bu kılavuz nasıl yerel bir Windows Masaüstü .NET (XAML) uygulama erişim belirteci almak ve Microsoft Graph API'si veya bir Azure Active Directory v2.0 uç noktasından erişim belirteçlerini gerektiren diğer API'leri çağırmak gösterir.

Kılavuzu tamamladıktan sonra uygulamanızın kişisel hesaplar (outlook.com, live.com ve diğerleri dahil) kullanan korumalı bir API'yi çağırmak mümkün olacaktır. Uygulama ayrıca iş ve Okul hesapları herhangi bir şirket veya Azure Active Directory kullanan kuruluş.  

> [!NOTE]
> Visual Studio 2015 güncelleştirme 3 veya Visual Studio 2017 Kılavuzu gerektirir. Bu sürümlerden birini yok mu? [Visual Studio 2017'yi ücretsiz olarak indirin](https://www.visualstudio.com/downloads/).

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?

![Örnek uygulama tarafından bu öğreticileri çalışır nasıl oluşturulacağını gösterir](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks-updated.png)

Bu kılavuz ile oluşturduğunuz örnek uygulama, Microsoft Graph API'si veya bir Azure Active Directory v2.0 uç noktasından belirteçleri kabul eden bir Web API'si sorgular bir Windows masaüstü uygulaması sağlar. Bu senaryoda, yetkilendirme üst bilgisi ile HTTP istekleri için bir belirteç ekler. Microsoft Authentication Library (MSAL), belirteç edinme ve yenileme işlemini gerçekleştirir.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Web API'leri korumalı erişim belirteci alma işleme

Örnek uygulama, kullanıcının kimliği doğrulandıktan sonra Microsoft Graph API'si veya Azure Active Directory v2 ile güvenli bir Web API'si sorgulamak için kullanılabilecek bir belirteç alır.

Belirli kaynaklara erişmesine izin vermek için bir belirteç gibi Microsoft Graph API'leri gerektirir. Örneğin, bir belirteç kullanıcı profilini okuyun, bir kullanıcının takvime erişip veya e-posta göndermek için gereklidir. Uygulamanız API kapsamları belirterek bu kaynaklara erişmek için MSAL kullanarak bir erişim belirteci isteyebilirsiniz. Bu erişim belirteci, ardından korumalı kaynağa karşı yapılan her çağrı için HTTP yetkilendirme üst bilgisi eklenir.

MSAL önbelleğe alma ve erişim belirteçleri, yenileme yönetir, uygulamanız gerekmez.

## <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuz, aşağıdaki NuGet paketlerini kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft kimlik doğrulama kitaplığı (MSAL)|
