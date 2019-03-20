---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: 33e2ac136ae68ee0c0ce0109a6f6934727d3a6c5
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203743"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Bir Android uygulamasından Microsoft Graph'i çağırmaya ve kullanıcılarının oturumunu

Bu öğreticide, bir Android uygulaması oluşturmak ve Microsoft kimlik platformuyla tümleştirebilir öğreneceksiniz. Özellikle, bu uygulama bir kullanıcının oturumunu, Microsoft Graph API'sini çağırmak için erişim belirteci almak ve Microsoft Graph API için temel bir istekte.  

Kılavuzu tamamladıktan sonra uygulamanızın oturum açma işlemleri kişisel Microsoft hesabı (outlook.com, live.com ve diğerleri dahil) kabul eder ve herhangi bir şirket veya Azure Active Directory kullanan kuruluş iş veya Okul hesapları.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?

![Örnek uygulama tarafından bu öğreticileri çalışır nasıl oluşturulacağını gösterir](media/active-directory-develop-guidedsetup-android-intro/android-intro-updated.png)

Bu örnek uygulamasında kullanıcılarının oturumunu ve onların adına veri alın.  Bu veriler, yetkilendirme gerektirir ve Microsoft kimlik platformu tarafından korunmuş bir uzak API'ye (Bu durumda Microsoft Graph API) aracılığıyla erişilir.

Daha ayrıntılı belirtmek gerekirse:

* Uygulamanızı bir web sayfası, kullanıcının oturum açmak için başlatılır.
* Uygulamanızı Microsoft Graph API'si için bir erişim belirteci verilir.
* HTTP isteği Web API'sine erişim belirtecinin dahil edilir.
* Microsoft Graph yanıta işler.

Bu örnek, koordine ve kimlik doğrulaması ile yardımcı için Microsoft Authentication kitaplığı için Android (MSAL) kullanır. MSAL otomatik olarak yenileme belirteçleri, cihazdaki diğer uygulamalar arasında SSO sunun, hesapları yönetebilir ve koşullu erişim durumlarında çoğu yardımcı.

## <a name="prerequisites"></a>Önkoşullar

* Bu Kılavuzlu Kurulum Android Studio 3.0 kullanır.
* Android 21 veya üzeri gereklidir (25 + önerilir).
* Google Chrome veya özel sekmeler kullanan bir web tarayıcısı Android için MSAL bu sürümü için gereklidir.

## <a name="library"></a>Kitaplık

Bu kılavuz, aşağıdaki kimlik doğrulama kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft kimlik doğrulama kitaplığı (MSAL)|
