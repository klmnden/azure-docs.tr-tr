---
title: Kullanıcı akışı sürümlerinde Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de kullanılabilir olan kullanıcı Akışları'nın sürümleri hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: ed57a9fa3b041961ce220e8f10d9aed5e7bef60e
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511933"
---
# <a name="user-flow-versions-in-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanıcı akışı sürümlerinde

>[!IMPORTANT]
> Makalede listelenen herhangi bir kullanıcı akışı olarak tanımlanan sürece genel Önizleme aşamasında olduğu kabul edilir **önerilen**. Yalnızca, üretim uygulamaları için önerilen kullanıcı akışları kullanmanız gerekir.

Azure Active Directory (Azure AD) B2C kullanıcı akışlarında ortak kümesi yardımcı [ilkeleri](active-directory-b2c-reference-policies.md) müşteri kimlik deneyimi tam olarak açıklanmaktadır. Bu deneyimler, kaydolma, oturum açma parolasını sıfırlama veya profil düzenleme içerir. Azure AD B2C'de önerilen kullanıcı akışları hem Önizleme kullanıcı akışları koleksiyonundan seçebilirsiniz. 

Yeni kullanıcı akışları yeni sürümler olarak eklenir. Kullanıcı akışları kararlı haline geldiğinden, bunlar kullanmak için önerilen. Kullanıcı akışları olarak işaretlenmiş **önerilen** varsa bunlar baştan sona test. Kullanıcı akışları önerildiği şekilde işaretlenmiş kadar Önizleme'de kabul edilir. Herhangi bir üretim uygulaması için önerilen kullanıcı akışı kullanın, ancak uygun olduğunda yeni işlevselliğini test etmek için diğer sürümlerden seçin. Önerilen kullanıcı Akışları'nın eski sürümlerini kullanmamalısınız.

## <a name="v1"></a>V1

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama | Evet | Bir kullanıcının e-postasına doğruladıktan sonra yeni bir parola seçmenizi sağlar. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Parola karmaşıklık gereksinimleri](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Profil düzenleme | Evet | Kullanıcı özniteliklerinin yapılandırmak bir kullanıcı etkinleştirir. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](active-directory-b2c-reference-tokens.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li></ul> |
| Kaynak sahibi | Hayır | Doğrudan yerel uygulamaları (tarayıcı gerekli) oturum açmak bir yerel hesabı kullanıcıyla sağlar. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](active-directory-b2c-reference-tokens.md)</li><li>Belirteç uyumluluk ayarları</li></ul> |
| Oturum aç | Hayır | Bir kullanıcı, hesabında oturum açmak etkinleştirir. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Belirteç ömrü](active-directory-b2c-reference-tokens.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>Blok oturum açma</li><li>Parola sıfırlama zorla</li><li>(KMSI) içinde Oturumumu açık tut</ul><br>Bu kullanıcı akışını kullanıcı arabirimiyle özelleştiremezsiniz. |
| Kaydolma | Hayır | Kullanıcının bir hesap oluşturmasını sağlar. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Belirteç ömrü](active-directory-b2c-reference-tokens.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Parola karmaşıklık gereksinimleri](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Kaydolma ve oturum açma | Evet | Bir kullanıcı, hesabında oturum açın veya hesap oluşturmak etkinleştirir. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Belirteç ömrü](active-directory-b2c-reference-tokens.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Parola karmaşıklık gereksinimleri](active-directory-b2c-reference-password-complexity.md)</li></ul>|

## <a name="v2"></a>V2

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| V2 parola sıfırlama | Hayır | Bir kullanıcının e-postasına doğruladıktan sonra yeni bir parola seçmenizi sağlar. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Yaş geçidi](basic-age-gating.md)</li><li>[Parola karmaşıklık gereksinimleri](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| V2'de oturum açın | Hayır | Bir kullanıcı, hesabında oturum açmak etkinleştirir. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Belirteç ömrü](active-directory-b2c-reference-tokens.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Yaş geçidi](basic-age-gating.md)</li><li>Oturum açma sayfasını özelleştirme</li></ul> |
| V2 ' oturum | Hayır | Kullanıcının bir hesap oluşturmasını sağlar. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Belirteç ömrü](active-directory-b2c-reference-tokens.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Yaş geçidi](basic-age-gating.md)</li><li>[Parola karmaşıklık gereksinimleri](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Kaydolma ve oturum açma v2'de | Hayır | Hesap oluşturmak ya da kendi hesabında oturum açmasına olanak tanır. Bu kullanıcı akışını kullanarak, aşağıdakileri yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Yaş geçidi](basic-age-gating.md)</li><li>[Parola karmaşıklık gereksinimleri](active-directory-b2c-reference-password-complexity.md)</li></ul> |
