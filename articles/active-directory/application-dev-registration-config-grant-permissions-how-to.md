---
title: Özel olarak geliştirilmiş bir uygulamada için izinlerin nasıl | Microsoft Docs
description: Azure AD portalı veya bir URL parametresi kullanarak özel olarak geliştirilmiş uygulamanız için izinlerin nasıl
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 75beeb35b740bb126fff905f4cfa5a0b455e025e
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365252"
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Nasıl özel olarak geliştirilmiş bir uygulamada izinler

Uygulamanızı sıd'lerde onay vermek istediğiniz veya değil onaylı bir hata bir uygulamaya çalışan, aşağıdaki adımları deneyin.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Uygulamanız için yönetici onayı gerçekleştirme

Bu, kuruluşunuzdaki tüm kullanıcılar için uygulama izin verme etkisi vardır.

1. Gidin **uygulama kayıtları** dikey olarak bir **genel yönetici**, ardından uygulamayı seçin.

2. Seçin **gerekli izinler**ve son olarak isabet **izinler** dikey penceresinin üstünde düğme.

Alternatif olarak, bir isteği oluşturabilirsiniz *login.microsoftonline.com* ile uygulama yapılandırmalarını ve üzerinde ekleme *& komut istemi Yönetim =\_onay*. Yönetici kimlik bilgilerinizle oturum sonra uygulamayı tüm kullanıcılar için izin verildi.

## <a name="how-to-force-user-consent-for-your-application"></a>Uygulamanız için kullanıcı onay zorlama

* Kimlik doğrulama istekleri ekleme *& komut istemi onay =* son kullanıcıların kimlik doğrulaması her zaman onay gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

[Onay ve AzureAD uygulamaları ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

[Uygulama onayı ve AzureAD v2.0 için kullanarak yakınsanmış](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
