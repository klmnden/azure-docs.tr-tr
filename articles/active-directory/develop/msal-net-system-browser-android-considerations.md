---
title: Xamarin Android konuları (Microsoft kimlik doğrulama kitaplığı .NET için) | Azure
description: Xamarin Android ile Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanırken hakkında belirli değerlendirmeler öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c57feb33967732481d78e0ddaba5e90f4f82f327
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544436"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Xamarin Android özgü MSAL.NET hakkında konuları
Bu makalede, sistem tarayıcı Xamarin Android ile Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanırken dikkate alınacak belirli noktalar açıklanmaktadır.

MSAL.NET 2.4.0-preview ile başlayarak, MSAL.NET Chrome dışındaki tarayıcıları destekler ve Chrome artık gerektiren kimlik doğrulaması için bir Android cihazında yüklü olmalıdır.

Özel sekmeler, bunlar gibi destekleyen tarayıcılar kullanmanızı öneririz:

| Özel sekmeler desteğiyle tarayıcılar | Paket adı |
|------| ------- |
|Chrome | com.android.chrome|
|Microsoft Edge | com.microsoft.emmx|
|Firefox | org.Mozilla.Firefox|
|Ecosia | com.ecosia.android|
|Kivi | com.kiwibrowser.browser|
|Brave | com.brave.browser|

Tarayıcı tabanlı test işlemlerimizi üzerinde özel sekmeler desteğiyle yanı sıra özel sekmeler desteklemeyen bazı tarayıcılar da kimlik doğrulaması için çalışır: Opera, Opera Mini InBrowser ve Maxthon. Daha fazla bilgi için okuma [test sonuçları için tablosu](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Android-system-browser#devices-and-browsers-tested).

## <a name="known-issues"></a>Bilinen sorunlar

- Kullanıcının cihazda etkin hiçbir tarayıcı varsa, MSAL.NET oluşturmaz bir `AndroidActivityNotFound` özel durum. 
  - **Risk azaltma**: Kullanıcı, bir tarayıcı (tercihen bir özel sekmeler desteğiyle) cihazlarında etkinleştirmelisiniz bildirin.

- (Örn. kimlik doğrulaması başarısız olursa kimlik doğrulaması başlatılır DuckDuckGo ile), MSAL.NET döndürür bir `AuthenticationCanceled MsalClientException`. 
  - **Kök sorunun**: Özel sekmeler destekli bir tarayıcıya cihazda etkin değil. Kimlik doğrulaması, kimlik doğrulamasını tamamlamak ekleyemediyse alternatif bir tarayıcı ile başlatıldı. 
  - **Risk azaltma**: Kullanıcı, bir tarayıcı (tercihen bir özel sekme desteğiyle) cihazlarında yüklemelisiniz bildirin.

## <a name="devices-and-browsers-tested"></a>Cihazlar ve tarayıcılar test
Aşağıdaki tabloda, sınanmış tarayıcılar ve cihazlar listeler.

| | Tarayıcı&ast;     |  Sonuç  | 
| ------------- |:-------------:|:-----:|
| Huawei / bir + | Chrome&ast; | Geçiş|
| Huawei / bir + | Edge&ast; | Geçiş|
| Huawei / bir + | Firefox&ast; | Geçiş|
| Huawei / bir + | Brave&ast; | Geçiş|
| Bir + | Ecosia&ast; | Geçiş|
| Bir + | Kivi&ast; | Geçiş|
| Huawei / bir + | Opera | Geçiş|
| Huawei | OperaMini | Geçiş|
| Huawei / bir + | InBrowser | Geçiş|
| Bir + | Maxthon | Geçiş|
| Huawei / bir + | DuckDuckGo | Kullanıcı iptal etti kimlik doğrulama|
| Huawei / bir + | UC tarayıcı | Kullanıcı iptal etti kimlik doğrulama|
| Bir + | Dolphin | Kullanıcı iptal etti kimlik doğrulama|
| Bir + | CM tarayıcı | Kullanıcı iptal etti kimlik doğrulama|
| Huawei / bir + | hiçbiri yüklü değil | Ex AndroidActivityNotFound|

&ast; Özel sekmeler destekler

## <a name="next-steps"></a>Sonraki adımlar
İçin kod parçacıkları sistem tarayıcı Xamarin Android ile kullanma hakkında ek bilgi okuyup bu [Kılavuzu](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid).  