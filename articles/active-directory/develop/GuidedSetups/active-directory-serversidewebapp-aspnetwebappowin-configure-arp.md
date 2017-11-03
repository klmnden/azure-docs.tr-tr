---
title: "Azure AD v2 ASP.NET Web sunucusu alma başlatıldı - Config | Microsoft Docs"
description: "Microsoft oturum açma Openıd Connect standardını kullanan geleneksel web tarayıcı tabanlı bir uygulama ile ASP.NET çözümünü uygulama"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 8a1650a65e7980f4a13fa4edc7918b0099bb5464
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>ASP.NET Web uygulamanız uygulamanın kayıt bilgileri ile yapılandırma

Bu adımda, projenizin SSL kullanacak şekilde yapılandırın ve uygulamanızın kayıt bilgilerini yapılandırmak için SSL URL'sini kullanın. Bundan sonra uygulama Ekle ' kayıt bilgileri çözümünüze *web.config*.

1.  Çözüm Gezgini'nde, proje seçip bakmak `Properties` (bir özellik penceresinde F4 tuşuna basın görmüyorsanız) penceresi
2.  Değişiklik `SSL Enabled` için`True`
3.  Değerinden kopyalama `SSL URL` yukarıda ve yapıştırın `Redirect URL` alan bu sayfanın üst kısmında ve ardından *güncelleştirme*:<br/><br/>![Proje Özellikleri](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
4.  Aşağıdakileri ekleyin `web.config` bölümünde kökün klasörde bulunan dosya `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
