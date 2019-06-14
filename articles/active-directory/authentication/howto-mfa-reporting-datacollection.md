---
title: Azure multi-Factor Authentication kullanıcı veri toplama - Azure Active Directory
description: Hangi bilgilerin kullanıcıların tanımasına yardımcı olmak için Azure multi-Factor Authentication tarafından kullanılır?
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
ms.openlocfilehash: e2b8d68cc348ce8e157c7d58424eaebb06940335
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60359050"
---
# <a name="azure-multi-factor-authentication-user-data-collection"></a>Azure multi-Factor Authentication kullanıcı veri toplama

Bu belgede, kaldırmak istediğiniz durumunda Azure multi-Factor Authentication (MFA sunucusu) ve Azure MFA (bulut tabanlı) tarafından toplanan kullanıcı bilgileri bulmak açıklanmaktadır.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Toplanan bilgiler

MFA sunucusu, NPS uzantısı ile Windows Server 2016 Azure MFA AD FS bağdaştırıcısı toplamak ve 90 gün için aşağıdaki bilgileri depolar.

Kimlik doğrulama girişimlerini (Raporlama ve sorun giderme için kullanılır):

- Zaman damgası
- Kullanıcı adı
- Ad
- Soyadı
- E-posta adresi
- Kullanıcı grubu
- Kimlik doğrulama yöntemini (telefon görüşmesi, metin iletisi, mobil uygulama, OATH belirteci)
- Telefon araması modunu (standart, PIN)
- SMS mesajı yönünü (tek yönlü, çift yönlü)
- SMS mesajı modunu (OTP, OTP + PIN)
- Mobil uygulama modunu (standart, PIN)
- OATH belirteci modu (standart, PIN)
- Kimlik doğrulaması türü
- Uygulama Adı
- Birincil arama ülke kodu
- Birincil arama telefon numarası
- Birincil arama uzantısı
- Birincil arama kimliği
- Birincil arama sonucu
- Yedekleme çağrısı ülke kodu
- Yedekleme çağrısı telefon numarası
- Yedekleme çağrısı uzantısı
- Kimliği doğrulanmış yedekleme çağrısı
- Yedekleme araması sonucu
- Genel kimlik doğrulaması
- Genel sonucu
- Sonuçlar
- Kimlik doğrulaması
- Sonuç
- IP adresi başlatılıyor
- Cihazlar
- Cihaz belirteci
- Cihaz Türü
- Mobil uygulama sürümü
- İşletim Sistemi Sürümü
- Sonuç
- Bildirim kullanılan denetle

Etkinleştirme (Microsoft Authenticator mobil uygulamasında bir hesabı etkinleştirmek için girişim sayısı):
- Kullanıcı adı
- Hesap Adı
- Zaman damgası
- Etkinleştirme kodu sonucunu Al
- Etkinleştirme başarılı
- Etkinleştirme hatası
- Etkinleştirme durumu sonucu
- Cihaz adı
- Cihaz Türü
- Uygulama Sürümü
- OATH belirtecinin etkin

Bloklar (engellenmiş durumda belirlemek için kullanılan ve raporlama için):

- Blok zaman damgası
- Kullanıcı adına göre engelle
- Kullanıcı adı
- Ülke kodu
- Telefon numarası
- Biçimlendirilmiş telefon numarası
- Dahili numara
- Temiz uzantısı
- Engellendi
- Engelleme nedeni
- Tamamlama zaman damgası
- Tamamlama nedeni
- Hesap Kilitli
- Sahtekarlık Uyarısı
- Sahtekarlık uyarısı Engellenmiyor
- Dil

Atlar (Raporlama için kullanılan):

- Zaman damgası atlama
- Saniye cinsinden geçiş süresi
- Kullanıcı adı tarafından atlama
- Kullanıcı adı
- Ülke kodu
- Telefon numarası
- Biçimlendirilmiş telefon numarası
- Dahili numara
- Temiz uzantısı
- Bypass Reason
- Tamamlama zaman damgası
- Tamamlama nedeni
- Kullanılan atlama

Değişiklikler (MFA sunucusu ya da AAD kullanıcı değişiklikleri eşitlemek için kullanılır):

- Zaman damgası değiştirme
- Kullanıcı adı
- Yeni ülke kodu
- Yeni telefon numarası
- Yeni Uzantı yükle
- Yeni yedek ülke kodu
- Yeni yedek telefon numarası
- Yeni yedekleme uzantısı
- Yeni PIN
- PIN değişikliği gerekiyor
- Eski cihaz belirteci
- Yeni cihaz belirteci

## <a name="gather-data-from-mfa-server"></a>MFA sunucudan veri toplayın

MFA sunucusu 8.0 veya üzeri bir sürüm için Yöneticiler, kullanıcılar için tüm verileri dışarı aktarmak aşağıdaki işlem sağlar:

- MFA sunucunuz için oturum açın, gitmek **kullanıcılar** sekmesinde, söz konusu kullanıcıyı seçin ve tıklayın **Düzenle** düğmesi. Her sekme, kullanıcının geçerli MFA ayarlarını sağlamak için ekran görüntülerini (Alt-PrtScn) alın.
- MFA sunucusu, komut satırından yüklemenizi göre yolu değiştirerek aşağıdaki komutu çalıştırın `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` bir JSON üretmek için biçimlendirilmiş dosyası.
- Yöneticiler ayrıca Web hizmeti SDK'sı GetUserGdpr işlem, belirli bir kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini dışarı aktarmak için bir seçenek olarak kullanın veya daha büyük bir raporlama çözümü dahil edilip derecelendirilir.
- Arama `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` ve tüm yedeklemeler için "\<kullanıcı adı >" (tırnak işaretleri aramaya dahil) tüm örneklerini kullanıcı kaydını eklenmesi veya değiştirilmesi bulunacak.
   - Bu kayıtlar sınırlı (ancak değil ortadan) kaldırarak **"Oturum, kullanıcı değişiklikleri"** MFA sunucusu kullanıcı Deneyimini, günlüğe kaydetme bölümü, günlük dosyaları sekmesi içinde.
   - Syslog yapılandırdıysanız ve **"Oturum, kullanıcı değişiklikleri"** günlük girişlerini syslog yerine toplanabilir sonra MFA sunucusu UX ile günlüğe kaydetme bölümü, Syslog sekmesi, denetlenir.
- Günlük dosyaları kimlik doğrulama girişimleri, işletimsel ve MultiFactorAuthGdpr.exe dışarı aktarma veya Web hizmeti SDK'sını kullanarak sağlanan bilgilere yinelenen olarak kabul edilir ilgili diğer örneklerini MultiFactorAuthSvc.log ve diğer MFA sunucusu kullanıcı adı GetUserGdpr.

## <a name="delete-data-from-mfa-server"></a>MFA Sunucusu'ndan verilerini sil

MFA sunucusu, komut satırından yüklemenizi göre yolu değiştirerek aşağıdaki komutu çalıştırın `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` tüm MFA bulut hizmeti bu kullanıcı için toplanan bilgilerin silinemedi.

- Gerçek zamanlı olarak veri dışa aktarma işlemine dahil silinir, ancak tamamen kaldırılacak işletimsel veya yinelenen verileri 30 güne kadar sürebilir.
- Yöneticiler ayrıca Web hizmeti SDK'sı DeleteUserGdpr işlem, belirli bir kullanıcı için toplanan tüm MFA bulut hizmeti bilgileri silmek için bir seçenek olarak kullanın veya daha büyük bir raporlama çözümü dahil edilip derecelendirilir.

## <a name="gather-data-from-nps-extension"></a>NPS uzantısından verileri toplayın

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) dışarı aktarma için bir istekte bulunmak için.

- MFA bilgiler tamamlamak için saatler veya günler sürebilir verme dahil edilir.
- Kullanıcı adı AuthN/AzureMfa/AuthNOptCh AzureMfa/AuthZ/AuthZAdminCh ve AzureMfa/AuthZ/AuthZOptCh olay günlükleri, oluşumunu işletimsel ve dışa aktarma, sağlanan bilgileri için yinelenen olarak kabul edilir.

## <a name="delete-data-from-nps-extension"></a>NPS uzantısından verilerini sil

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) hesabı Kapat tüm MFA bulut hizmeti bu kullanıcı için toplanan bilgilerin silmek için istekte bulunmak için.

- Bu tamamen kaldırılması için verileri 30 güne kadar sürebilir.

## <a name="gather-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure MFA AD FS bağdaştırıcısı verileri toplayın

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) dışarı aktarma için bir istekte bulunmak için. 

- MFA bilgiler tamamlamak için saatler veya günler sürebilir verme dahil edilir.
- Kullanıcı adı (etkinse) AD FS izleme/Debug olay günlükleri, oluşumunu işletimsel ve dışa aktarma, sağlanan bilgileri için yinelenen olarak kabul edilir.

## <a name="delete-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure MFA AD FS bağdaştırıcısı verilerini sil

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) hesabı Kapat tüm MFA bulut hizmeti bu kullanıcı için toplanan bilgilerin silmek için istekte bulunmak için.

- Bu tamamen kaldırılması için verileri 30 güne kadar sürebilir.

## <a name="gather-data-for-azure-mfa"></a>Azure MFA için veri toplama

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) dışarı aktarma için bir istekte bulunmak için.

- MFA bilgiler tamamlamak için saatler veya günler sürebilir verme dahil edilir.

## <a name="delete-data-for-azure-mfa"></a>Azure MFA için verileri silme

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) hesabı Kapat tüm MFA bulut hizmeti bu kullanıcı için toplanan bilgilerin silmek için istekte bulunmak için.

- Bu tamamen kaldırılması için verileri 30 güne kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

[MFA sunucusu raporlama](howto-mfa-reporting.md)
