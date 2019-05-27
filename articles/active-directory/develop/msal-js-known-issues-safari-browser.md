---
title: Bilinen sorunlar (JavaScript için Microsoft kimlik doğrulama kitaplığı) tarayıcılarda | Azure
description: Microsoft kimlik doğrulama kitaplığı (MSAL.js) JavaScript için kullanırken bilinen sorunlar Safari tarayıcısı ile bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb89b1ef4dbbef234fba3152d7f85bbadfbdc64a
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873893"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>Safari tarayıcısını MSAL.js ile bilinen sorunları 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Safari 12 ve ITP 2.0 sessiz belirteci yenileme

12 Apple iOS ve MacOS 10.14 işletim sistemleri dahil sürümü [12 Safari tarayıcı](https://developer.apple.com/safari/whats-new/). Güvenlik ve gizlilik amacı doğrultusunda, Safari 12 içeren [akıllı izleme önleme 2.0](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/). Bu temelde ayarlanan üçüncü taraf tanımlama bilgilerini silmek tarayıcı neden olur. ITP 2.0 ayrıca üçüncü taraf tanımlama bilgileri olarak kimlik sağlayıcıları tarafından ayarlanan tanımlama bilgilerini işler.

### <a name="impact-on-msaljs"></a>MSAL.js etkisi

MSAL.js kapsamında sessiz belirteç edinme ve yenileme gerçekleştirmek için gizli bir Iframe kullanan `acquireTokenSilent` çağırır. Azure AD tarafından ayarlanmış tanımlama bilgileri tarafından temsil edilen kimliği doğrulanmış kullanıcı oturumuna erişim izni olan Iframe sessiz belirteci isteklerini dayanır. ITP 2.0 bu tanımlama bilgilerine erişimi engelleme, MSAL.js sessiz bir şekilde almak ve belirteçleri yenileme başarısız olur ve sonuçlanır `acquireTokenSilent` hataları.

Var. Bu sorun için çözüm bu noktada ve biz standartları topluluk seçeneklerle Değerlendiriyorlar.

### <a name="work-around"></a>Geçici çözüm

Varsayılan olarak, üzerinde Safari tarayıcısı ITP ayar etkindir. Giderek bu ayarı devre dışı bırakabilirsiniz **tercihleri** -> **gizlilik** ve işaretini **siteler arası izleme önlemek** seçeneği.

![Safari ayarı](./media/msal-js-known-issue-safari-browser/safari.png)

İşlemek ihtiyacınız olacak `acquireTokenSilent` başarısızlıkları ile etkileşimli kullanıcının oturum açmasını ister belirteci çağrısı.
Yinelenen oturum açma işlemleri önlemek için işlenecek uygulayabilirsiniz yaklaşım, `acquireTokenSilent` hatası ve kullanıcının etkileşimli çağrısı ile devam etmeden önce Safari'de ITP ayarı devre dışı bırakma seçeneği sağlar. Ayarı devre dışı bırakıldıktan sonra sonraki sessiz belirteci yenileme başarılı olması gerekir.
