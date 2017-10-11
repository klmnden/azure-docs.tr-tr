---
title: "Uygulama kayıt portalı Yardım konuları | Microsoft Docs"
description: "Microsoft uygulama kayıt Portalı'nda çeşitli özellikleri açıklaması."
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: c60499c425a7fd800f7ca9a5bac1fed5af73b801
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="app-registration-reference"></a>Uygulama Kayıt başvurusu
Bu belgede bağlamı ve Microsoft uygulama kayıt Portalı'nda bulunan çeşitli özelliklerin açıklamaları sağlanmaktadır [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Uygulamalarım
Bu listedeki tüm Azure AD v2.0 uç noktası ile kullanılması için kaydedilen uygulamalar içerir.  Bu uygulamalar Microsoft hesabı ve Azure Active Directory'den iş/Okul hesapları hem kişisel hesapları olan kullanıcıların oturum seçeneğine sahipsiniz.  Azure AD v2.0 uç hakkında daha fazla bilgi için bkz: bizim [v2.0 genel bakış](active-directory-appmodel-v2-overview.md).  Bu uygulamalar Microsoft hesabınızın kimlik doğrulama uç ile tümleştirmek için de kullanılabilir `https://login.live.com`.

## <a name="live-sdk-applications"></a>Canlı SDK uygulamaları
Bu listedeki tüm uygulamalarınızın yalnızca Microsoft hesabı ile kullanmak için kayıtlı içerir.  Azure Active Directory doğabilecek ile kullanmak için etkinleştirilmemiş.  MSA Geliştirici portalında ile önceden kaydedilmiş tüm uygulamaları bulabileceğiniz budur `https://account.live.com/developers/applications`.  Daha önce sırasında gerçekleştirilen tüm işlevleri `https://account.live.com/developers/applications` şimdi bu yeni portalında gerçekleştirilen `https://apps.dev.microsoft.com`.  Daha fazla Microsoft hesabı uygulamalarınızı hakkında sorularınız varsa lütfen bizimle iletişime geçin.

## <a name="application-secrets"></a>Uygulama parolaları
Uygulama parolaları gerçekleştirmek, uygulamanın güvenilir izin kimlik bilgileri olan [istemci kimlik doğrulaması](http://tools.ietf.org/html/rfc6749#section-2.3) Azure AD ile.  OAuth ve Openıd Connect içinde bir uygulama parolaları yaygın olarak adlandırılır bir `client_secret`.  Bir web adreslenebilir konumdaki güvenlik belirtecini alır herhangi bir uygulama v2.0 Protokolü (kullanarak bir `https` düzeni) bir uygulama gizli anahtarı kendi Azure ad güvenlik belirtecini kullanım bağlı tanımlamak için kullanmanız gerekir.  Ayrıca, herhangi bir yerel istemci üzerinde bu recieves belirteçler bir aygıt bir uygulama gizli anahtarı kullanarak istemci kimlik doğrulaması gerçekleştirmek için güvenli olmayan ortamlarda gizli depolama önerilmemektedir alınamaz.

Her uygulama verilen herhangi bir noktada iki geçerli uygulama parolaları içerebilir.  İki gizli tutarak, uygulamanızın tüm ortamları boyunca düzenli anahtarı geçiş işlemini gerçekleştirmek için ablilty sahip.  Yeni parolayı uygulamanıza tamamen geçirdikten sonra eski parola silin ve yeni bir sağlama.

Şu anda uygulama parolaları yalnızca iki tür uygulama kayıt Portalı'nda izin verilir.  Seçme **yeni bir parola oluşturmak** oluşturur ve uygulamanızda kullanabilirsiniz ilgili veri deposundaki bir paylaşılan gizlilik depolar.  Seçme **oluştur yeni bir anahtar çifti** indirmiş ve Azure AD için istemci kimlik doğrulaması için kullanılan yeni bir ortak/özel anahtar çifti oluşturur.

## <a name="profile"></a>Profil
Uygulama kayıt portalı profil bölümü, oturum açma sayfasında uygulamanız için özelleştirmek için kullanılabilir.  Şu anda oturum açma sayfasının uygulama logosu, hizmet URL'si ve gizlilik bildirimini koşulları değiştirebilirsiniz.  Logo; saydam, 48 x 48 veya 50 x 50 piksel boyutunda, 15 KB veya daha küçük bir GIF, PNG veya JPEG dosyası olmalıdır.  Deneyin değerleri değiştirme ve sonuçta elde edilen oturum açma sayfasında görüntüleme!

## <a name="live-sdk-support"></a>Canlı SDK'sı desteği
"Live SDK desteği" etkinleştirdiğinizde, hem Azure AD ile oluşturduğunuz herhangi bir uygulama parolaları sağlanacak ve Microsoft Account verileri depolar.  Bu, doğrudan Microsoft Account hizmetini ile (login.live.com) tümleştirmek için uygulamanıza olanak tanır.  Microsoft Account doğrudan (Azure AD v2.0 uç aksine) kullanarak bir uygulama oluşturmak istiyorsanız, Live SDK desteği etkin olduğundan emin olun.

Live SDK desteğini devre dışı bırakma olun uygulama gizli anahtarı yalnızca Azure AD verileri yazılmış depolar.  Azure AD veri deposu FISMA uyumluluk gibi belirli standartları karşılamak üzere onu izin Kurumsal düzeyde düzenlemeleri içerir.  Live SDK desteğini etkinleştirirseniz, uygulamanızın bazı Bu standartlar ile uyumluluğu elde.

Her zaman sadece Azure AD v2.0 uç kullanmayı planlıyorsanız, Canlı SDK'sı desteği güvenli bir şekilde devre dışı bırakabilirsiniz.

