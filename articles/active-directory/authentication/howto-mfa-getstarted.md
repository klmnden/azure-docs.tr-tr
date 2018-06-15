---
title: Bulutta Azure MFA’yı kullanmaya başlama | Microsoft Docs
description: Bu, bulutta nasıl Microsoft Azure MFA kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 06/24/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 0a822d55e8d7bd0d503eb7d77f96dc9e60e1a4ba
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33882877"
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Bulutta Azure Multi-Factor Authentication kullanmaya başlama
Bu makalede bulutta Azure Multi-Factor Authentication kullanmaya nasıl başlayacağınız gösterilmektedir.

> [!NOTE]
> Aşağıdaki belgeler kullanıcıların **Azure portalı** kullanarak nasıl etkinleştirileceğine ilişkin bilgi sağlar. O365 kullanıcıları için Azure Multi-Factor Authentication kurulumuna ilişkin bilgileri arıyorsanız, bkz. [Office 365 için multi-factor authentication kurulumu.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![Bulutta MFA](./media/howto-mfa-getstarted/mfa_in_cloud.png)

## <a name="prerequisite"></a>Önkoşul
[Azure aboneliği için kaydolun](https://azure.microsoft.com/pricing/free-trial/) - Zaten bir Azure aboneliğiniz yoksa, birisi için kaydolmalısınız. Yeni başlıyor ve Azure MFA’yı henüz kullanmaya başlıyorsanız, deneme aboneliğini kullanabilirsiniz.

## <a name="enable-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication’ı etkinleştirme
Kullanıcıların Azure Multi-Factor Authentication içeren lisansları olduğu sürece, Azure MFA’yı etkinleştirmek için hiçbir işlem yapmanız gerekmez. Her kullanıcı için iki aşamalı doğrulama istemeye başlayabilirsiniz. Azure MFA'yı etkinleştiren lisanslar şunlardır:
- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

Bu üç lisanstan birine sahip değilseniz veya tüm kullanıcılarınızı kapsamaya yetecek sayıda lisansınız yoksa, bu da sorun değildir. Yalnızca ek bir adım uygulamanız ve dizininizde [Multi-Factor Auth Sağlayıcısı oluşturmanız](concept-mfa-authprovider.md) gerekir.

## <a name="turn-on-two-step-verification-for-users"></a>Kullanıcılar için iki aşamalı doğrulamayı açma

Azure MFA’yı kullanmaya başlamak için [Bir kullanıcı veya grup için iki aşamalı doğrulama gerektirme](howto-mfa-userstates.md) bölümünde listelenen yordamlardan birini kullanın. Tüm oturum açma işlemleri için iki aşamalı doğrulama uygulamayı seçebilir ya da yalnızca önem verdiğiniz durumlarda iki aşamalı kimlik doğrulaması uygulamak için koşullu erişim ilkeleri oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Bulutta Azure Multi-Factor Authentication özelliğini ayarladığınıza göre, şimdi dağıtımınızı yapılandırıp ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication’ı yapılandırma](howto-mfa-mfasettings.md).

