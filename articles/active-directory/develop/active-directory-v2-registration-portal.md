---
title: Uygulama kayıt portalı Yardım konuları | Microsoft Docs
description: Microsoft uygulama kayıt Portalı'nda çeşitli özellikleri açıklaması.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: celested
ms.reviewer: lenalepa
ms.custom: aaddev
ms.openlocfilehash: 9d38f6e6d6b9fa47b1cd1497820f7ff887954ad5
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="app-registration-reference"></a>Uygulama Kayıt başvurusu
Bu belgede bağlamı ve Microsoft uygulama kayıt Portalı'nda bulunan çeşitli özelliklerin açıklamaları sağlanmaktadır [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/).

## <a name="my-applications"></a>Uygulamalarım
Bu listedeki tüm Azure AD v2.0 uç noktası ile kullanılması için kaydedilen uygulamalar içerir. Bu uygulamaların hem kişisel Microsoft hesapları hem de Azure Active Directory'den iş/Okul hesapları olan kullanıcılar oturum seçeneğine sahipsiniz. Azure AD v2.0 uç hakkında daha fazla bilgi için bkz: [v2.0 genel bakış](active-directory-appmodel-v2-overview.md). Bu uygulamalar Microsoft hesabınızın kimlik doğrulama uç ile tümleştirmek için de kullanılabilir `https://login.live.com`.

## <a name="live-sdk-applications"></a>Canlı SDK uygulamaları
Bu listedeki tüm uygulamalarınızın yalnızca Microsoft hesabı ile kullanmak için kayıtlı içerir. Azure Active Directory ile kullanmak için etkinleştirilmemiş. MSA Geliştirici portalında ile daha önce kaydedilen herhangi bir uygulama nerede budur `https://account.live.com/developers/applications`. Daha önce sırasında gerçekleştirilen tüm işlevleri `https://account.live.com/developers/applications` şimdi bu yeni portalında gerçekleştirilen `https://apps.dev.microsoft.com`. Microsoft hesabı uygulamalar hakkında daha fazla sorunuz varsa, bizimle iletişime geçin.

## <a name="application-secrets"></a>Uygulama Sırları
Uygulama parolaları gerçekleştirmek, uygulamanın güvenilir izin kimlik bilgileri olan [istemci kimlik doğrulaması](http://tools.ietf.org/html/rfc6749#section-2.3) Azure AD ile. OAuth ve Openıd Connect içinde bir uygulama gizli anahtarı yaygın olarak adlandırılır bir `client_secret`. Bir web adreslenebilir konumdaki güvenlik belirtecini alır herhangi bir uygulama v2.0 Protokolü (kullanarak bir `https` düzeni) bir uygulama gizli anahtarı kendi Azure ad güvenlik belirtecini kullanım bağlı tanımlamak için kullanmanız gerekir. Ayrıca, bir cihazda belirteçleri alan herhangi bir yerel istemci uygulama gizli anahtarı kullanarak istemci kimlik doğrulaması gerçekleştirmek için Yasak. Bu, güvenli olmayan ortamlarda gizli depolama zorlaştırır.

Her uygulama herhangi bir anda iki geçerli uygulama parolaları içerebilir. İki gizli tutarak, uygulamanızın tüm ortamları boyunca düzenli anahtarı geçiş işlemini gerçekleştirmek için özelliğine sahiptir. Yeni parolayı uygulamanıza tamamen geçirdikten sonra eski parola silin ve yeni bir sağlama.

Şu anda uygulama parolaları yalnızca iki tür uygulama kayıt Portalı'nda izin verilir. Seçme **yeni bir parola oluşturmak** oluşturur ve uygulamanızda kullanabilirsiniz ilgili veri deposundaki bir paylaşılan gizlilik depolar. Seçme **oluştur yeni bir anahtar çifti** indirmiş ve Azure AD için istemci kimlik doğrulaması için kullanılan yeni bir ortak/özel anahtar çifti oluşturur. Seçme **ortak anahtarı karşıya** kendi ortak/özel anahtar çifti kullanmanıza olanak sağlar.
Bir ortak anahtarı içeren bir sertifikayı karşıya yüklemek için gereklidir.

## <a name="profile"></a>Profil
Uygulama kayıt portalı profil bölümü, uygulamanız için oturum açma sayfası özelleştirmek için kullanılabilir. Şu anda oturum açma sayfasının uygulama logosu, alter koşullarını hizmet URL'si ve gizlilik bildirimi URL'si. Logo; saydam, 48 x 48 veya 50 x 50 piksel boyutunda, 15 KB veya daha küçük bir GIF, PNG veya JPEG dosyası olmalıdır. Deneyin değerlerini değiştirme ve sonuçta elde edilen oturum açma sayfası görüntüleme!

## <a name="live-sdk-support"></a>Canlı SDK'sı desteği
"Live SDK desteği" etkinleştirdiğinizde, hem Azure AD ile oluşturduğunuz herhangi bir uygulama parolaları sağlanacak ve Microsoft Account verileri depolar. Bu, doğrudan Microsoft Account hizmetini ile (login.live.com) tümleştirmek uygulamanızı sağlar. Microsoft Account doğrudan (Azure AD v2.0 uç aksine) kullanarak bir uygulama oluşturmak istiyorsanız, Live SDK desteği etkin olduğundan emin olun.

Live SDK desteğini devre dışı bırakma sağlar uygulama gizli anahtarı yalnızca Azure AD verileri yazılmış depolar. Azure AD veri deposu FISMA uyumluluk gibi belirli standartları karşılamak üzere onu izin Kurumsal düzeyde düzenlemeleri içerir. Live SDK desteğini etkinleştirirseniz, uygulamanızın bazı Bu standartlar ile uyumluluğu elde.

Her zaman sadece Azure AD v2.0 uç kullanmayı planlıyorsanız, Canlı SDK'sı desteği güvenli bir şekilde devre dışı bırakabilirsiniz.

