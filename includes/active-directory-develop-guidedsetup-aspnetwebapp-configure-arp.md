---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: f3048daa7d597d92041733cdf5c6f299cebc2f73
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50098056"
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>ASP.NET Web uygulamanızı uygulama kayıt bilgileriyle yapılandırın

Bu adımda, SSL kullanacak şekilde projenizi yapılandırmak ve ardından uygulamanızın kayıt bilgileri yapılandırmak için SSL URL'sini kullanın. Bundan sonra uygulamayı eklemek ' kayıt bilgilerini çözümünüze *web.config*.

1. Çözüm Gezgini'nde projeyi seçin ve bakmak `Properties` (bir Özellikler penceresinde, F4 tuşuna görmüyorsanız) penceresi
2. Değişiklik `SSL Enabled` için `True`
3. Değeri Şuradan Kopyala: `SSL URL` yukarıda yapıştırın `Redirect URL` alan bu sayfanın üst kısmındaki'a tıklayın *güncelleştirme*:<br/><br/>![Proje Özellikleri](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
4. Aşağıdakileri eklemek `web.config` bölümünde kökünün klasörde bulunan dosya `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
