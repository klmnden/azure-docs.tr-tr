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
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: celested
ms.reviewer: lenalepa
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9b77f2a403bd4f410665d00bc69b3b1bcf0c3aaa
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65139165"
---
# <a name="app-registration-reference"></a>Uygulama kayıt başvurusu
Bu belgede bağlam sağlar ve çeşitli özelliklerin açıklamaları bulunan [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/).

> [!NOTE]
> Artık kaydetme ve yönetme yakınsanmış ve Azure AD uygulamalarında destekleyeceğiz [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/) Mayıs 2019 başlatılıyor. Mevcut uygulamalarınızı yönetmek ve kullanarak yeni uygulamalar kaydetme öneririz [uygulama kayıtları](https://aka.ms/appregistrations) Azure portalında karşılaşırsınız.

## <a name="my-applications-or-converged-applications"></a>Uygulamalarım veya hiper yakınsanmış uygulamalar
Bu liste tüm uygulamalarınızı Azure AD v2.0 uç noktası ile kullanılması için kaydedilen içerir. Bu uygulamaların kullanıcıların hem kişisel Microsoft hesapları hem de iş/Okul hesapları Azure Active Directory ile oturum açma olanağı vardır. Azure AD v2.0 uç noktası hakkında daha fazla bilgi için bkz: [v2.0 genel bakış](active-directory-appmodel-v2-overview.md). Bu uygulamalar Microsoft hesabı kimlik doğrulaması uç nokta ile tümleştirmek için de kullanılabilir `https://login.live.com`.

## <a name="azure-ad-only-applications"></a>Yalnızca Azure AD'yi kullanan uygulamalar
Bu liste tüm uygulamalarınızı Azure AD v1.0 uç noktası ile kullanılması için kaydedilen içerir. Bu uygulamalar, kullanıcılar Azure Active Directory'den iş/Okul hesaplarıyla oturum açma olanağı yeterlidir. Bu liste kullanılarak kaydedilmiş uygulamaları içerir **uygulama kayıtları** deneyimini [Azure portalı](https://portal.azure.com).

## <a name="live-sdk-applications"></a>Live SDK uygulamaları
Bu liste tüm uygulamalarınızı, yalnızca Microsoft hesabı ile kullanmak için kayıtlı içerir. Azure Active Directory ile kullanmak için etkinleştirilmedi. MSA Geliştirici portalında ile daha önce kaydolan tüm uygulamaları nerede budur `https://account.live.com/developers/applications`. Daha önce sırasında gerçekleştirilen tüm işlevleri `https://account.live.com/developers/applications` artık bu yeni portalda gerçekleştirilebilir `https://apps.dev.microsoft.com`.

## <a name="application-secrets"></a>Uygulama Sırları
Uygulama gizli dizilerini gerçekleştirmek, uygulamanızın güvenilir izin veren kimlik bilgileri olan [istemci kimlik doğrulaması](https://tools.ietf.org/html/rfc6749#section-2.3) Azure AD ile. OAuth ve Openıd Connect bir uygulama gizli anahtarı yaygın olarak adlandırılır bir `client_secret`. Bir web adreslenebilir konumda bir güvenlik belirteci alan herhangi bir uygulama v2.0 protokolündeki (kullanarak bir `https` düzeni) bu güvenlik belirtecinin kullanım sırasında Azure AD'ye kendisini tanımlamak için bir uygulama gizli anahtarı kullanmanız gerekir. Ayrıca, belirteçleri bir cihazda alan herhangi bir yerel istemcisi istemci kimlik doğrulaması gerçekleştirmek için bir uygulama gizli anahtarı kullanarak Yasak. Bu, gizli dizileri güvenli olmayan ortamlarda depolanmasını gerçekleştirilmesini önler.

Her uygulama, belirli bir zamanda iki geçerli uygulama gizli dizilerini içerebilir. İki gizli dizileri tutarak, uygulamanızın tüm ortam genelinde düzenli anahtar geçişi gerçekleştirme imkanına sahip olursunuz. Uygulamanızın yeni bir gizli dizi tamamen geçiş yaptıktan sonra eski gizli anahtarı silme ve yeni bir tane sağlayın.

Şu anda yalnızca iki tür uygulama gizli dizilerinin uygulama kayıt Portalı'nda izin verilir. Seçme **yeni parola oluştur** oluşturur ve paylaşılan gizlilik uygulamanızda kullanabileceğiniz ilgili veri deposunda saklar. Seçme **yeni anahtar çifti oluşturma** indirmiş ve Azure AD'ye istemci kimlik doğrulaması için kullanılan yeni bir ortak/özel anahtar çifti oluşturur. Seçme **ortak anahtarı karşıya** kendi ortak/özel anahtar çifti kullanmanıza olanak tanır.
Bir ortak anahtar içeren bir sertifikayı karşıya yüklemek için gereklidir.

## <a name="profile"></a>Profil
Uygulama kayıt portalı profili bölümünü, uygulamanız için oturum açma sayfasını özelleştirmek için kullanılabilir. Şu anda oturum açma sayfasının uygulama logosu, alter koşulları hizmet URL'sini ve gizlilik bildirimi URL'si. Logo; saydam, 48 x 48 veya 50 x 50 piksel boyutunda, 15 KB veya daha küçük bir GIF, PNG veya JPEG dosyası olmalıdır. Deneyin değiştirerek ve sonuçta elde edilen oturum açma sayfası görüntüleme!

## <a name="live-sdk-support"></a>Canlı SDK desteği
"Canlı SDK desteği" etkinleştirdiğinizde, hem Azure AD ile oluşturduğunuz herhangi bir uygulama gizli dizilerini sağlanacak ve Microsoft Account veri depoları. Bu, uygulamanızın doğrudan Microsoft Account hizmetini ile (login.live.com) tümleştirme sağlar. Microsoft Account doğrudan (Azure AD v2.0 uç noktası aksine) kullanarak bir uygulama oluşturmak istiyorsanız, Canlı SDK desteği etkin olduğundan emin olun.

Canlı SDK desteği devre dışı bırakma sağlar uygulama gizli anahtarı yalnızca Azure AD verisine yazılan depolayın. Azure AD veri deposu FISMA uyumluluğu gibi belirli standartlarını karşılayacak şekilde sağlayan kurumsal düzeyde düzenlemeler içerir. Canlı SDK desteği etkinleştirirseniz, uygulamanızın bazı Bu standartlar ile uyumluluk elde değil.

Yalnızca Azure AD v2.0 uç noktası kullanmayı planlıyorsanız, Canlı SDK desteği güvenli bir şekilde devre dışı bırakabilirsiniz.

