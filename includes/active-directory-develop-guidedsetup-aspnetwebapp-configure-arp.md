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
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 5940195207f85d8011f61336c0318e456c2a8a4c
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203829"
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
