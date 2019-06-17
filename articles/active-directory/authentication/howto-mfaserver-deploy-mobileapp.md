---
title: Azure MFA sunucusu mobil uygulama Web hizmeti - Azure Active Directory
description: MFA sunucusunu Microsoft Authenticator Uygulaması ile kullanıcılara anında iletme bildirimleri göndermek için yapılandırın.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2cbe10eb88550f04ead22a64fbcc2f17548af02d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057373"
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile mobil uygulama kimlik doğrulamasını etkinleştirme

Microsoft Authenticator uygulaması ek bir bant dışı doğrulama seçeneği sunar. Oturum açma sırasında kullanıcıya otomatik telefon çağrısı yapmak veya SMS göndermek yerine, Azure Multi-Factor Authentication, kullanıcının akıllı telefonu ya da tabletindeki Microsoft Authenticator uygulamasına anında iletme bildirimi gönderir. Kullanıcının oturum açma işlemini tamamlamak için uygulamada **Doğrula** seçeneğine dokunması (ya da PIN’i girip “Kimliği Doğrula” seçeneğine dokunması ) yeterlidir.

Şebeke sinyal gücünün güvenilir olmadığı durumlarda iki aşamalı doğrulama için bir mobil uygulama kullanmak tercih edilir. Uygulamayı bir OATH belirteci oluşturucu olarak kullanıyorsanız ağ veya İnternet bağlantısı gerekmez.

> [!IMPORTANT]
> 1 Temmuz 2019'dan itibaren Microsoft artık yeni dağıtımlar için MFA sunucusu sunacaktır. Bulut tabanlı Azure multi-Factor Authentication, kullanıcıların multi-Factor authentication gerektirmesine istediğiniz yeni müşteriler kullanmanız gerekir. MFA sunucusu 1 Temmuz'dan önce etkinleştirmiş olan mevcut müşteriler, Gelecekteki güncelleştirmelerin en son sürümü indirip zamanki etkinleştirme kimlik bilgileri oluştur mümkün olacaktır.

> [!IMPORTANT]
> Azure Multi-Factor Authentication Sunucusu v8.x veya üzeri yüklüyse aşağıdaki adımları gerçekleştirmeniz gerekmez. [Mobil uygulamayı yapılandırma](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server) bölümündeki adımlar izlenerek mobil uygulama kimlik doğrulaması ayarlanabilir.

## <a name="requirements"></a>Gereksinimler

Microsoft Authenticator uygulamasını kullanmak için Azure Multi-Factor Authentication Sunucusu v8.x veya üzerini çalıştırıyor olmanız gerekir

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırma

1. Multi-Factor Authentication Sunucusu konsolunda Kullanıcı Portalı simgesine tıklayın. Kullanıcıların kendi kimlik doğrulama yöntemlerini denetlemesine izin veriliyorsa, Ayarlar sekmesindeki **Kullanıcıların yöntemi seçmesine izin ver** bölümünden **Mobil Uygulama**’yı işaretleyin. Bu özellik etkinleştirilmeden, Mobil Uygulama için etkinleştirme işlemini tamamlamak üzere son kullanıcıların Yardım Masanızla iletişim kurması gerekir.
2. **Kullanıcıların Mobil Uygulamaları etkinleştirmesine izin ver** kutusunu işaretleyin.
3. **Kullanıcı Kaydına İzin Ver** kutusunu işaretleyin.
4. **Mobil Uygulama** simgesine tıklayın.
5. **Hesap adı** alanına bu hesabın mobil uygulamasında görüntülenecek şirket veya kuruluş adını girin.
   ![MFA Sunucusu yapılandırması Mobil Uygulama ayarları](./media/howto-mfaserver-deploy-mobileapp/mobile.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication ve üçüncü taraf VPN’ler ile gelişmiş senaryolar](howto-mfaserver-nps-vpn.md).
