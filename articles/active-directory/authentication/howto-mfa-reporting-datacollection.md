---
title: Azure multi-Factor Authentication kullanıcı veri toplama
description: Hangi bilgilerin kullanıcıların kimliğini doğrulamak amacıyla Azure multi-Factor Authentication tarafından kullanılır?
services: multi-factor-authentication
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.service: multi-factor-authentication
ms.workload: identity
ms.topic: article
ms.date: 05/01/2018
ms.openlocfilehash: d28bb4b8e171ef6189f81acc337088b3c5499ccf
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655309"
---
# <a name="azure-multi-factor-authentication-user-data-collection"></a>Azure multi-Factor Authentication kullanıcı veri toplama

Bu belge, kaldırmak istediğiniz durumunda Azure çok faktörlü kimlik doğrulama sunucusu (MFA) ve Azure MFA (bulut tabanlı) tarafından toplanan kullanıcı bilgileri bulmak açıklanmaktadır.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Toplanan bilgiler

MFA sunucusu, NPS genişletme ve Windows Server 2016 Azure MFA AD FS bağdaştırıcısı toplamak ve 90 gün için aşağıdaki bilgileri depolar.

Kimlik doğrulama girişimlerini (Raporlama ve sorun giderme için kullanılır):

- Zaman damgası
- Kullanıcı adı
- Ad
- Soyadı
- E-posta adresi
- Kullanıcı Grubu
- Kimlik doğrulama yöntemi (telefon araması, SMS iletisi, mobil uygulama, OATH belirteci)
- Telefon araması modunu (standart, PIN)
- SMS mesajı yönünü (tek yönlü, çift yönlü)
- SMS mesajı modunu (OTP, OTP + PIN)
- Mobil uygulama modunu (standart, PIN)
- OATH belirteci modu (standart, PIN)
- Kimlik Doğrulama Türü
- Uygulama Adı
- Birincil çağrısı ülke kodu
- Birincil çağrısı telefon numarası
- Birincil çağrısı uzantısı
- Kimliği doğrulanmış birincil çağrısı
- Birincil arama sonucu
- Yedekleme çağrısı ülke kodu
- Yedekleme çağrısı telefon numarası
- Yedekleme çağrısı uzantısı
- Kimliği doğrulanmış yedekleme çağrısı
- Yedekleme arama sonucu
- Genel kimlik doğrulaması
- Genel sonucu
- Sonuçlar
- Kimliği Doğrulandı
- Sonuç
- IP adresi başlatılıyor
- Cihazlar
- Cihaz belirteci
- Cihaz Türü
- Mobil uygulama sürümü
- İşletim Sistemi Sürümü
- Sonuç
- Bildirim için kullanılan denetimi

Etkinleştirme (Microsoft Authenticator mobil uygulamasında bir hesabı etkinleştirmeniz çalışır):
- Kullanıcı adı
- Hesap Adı
- Zaman damgası
- Etkinleştirme kodu sonucu alın
- Başarı etkinleştir
- Hata etkinleştirme
- Etkinleştirme durumu sonucu
- Aygıt adı
- Cihaz Türü
- Uyg Sürümü
- OATH belirtecinin etkin

Bloklar (engellenmiş durumunu belirlemek için kullanılan ve raporlama için):

- Blok zaman damgası
- Kullanıcı adına göre engelle
- Kullanıcı adı
- Ülke Kodu
- Telefon Numarası
- Biçimlendirilmiş telefon numarası
- Dahili numara
- Temiz uzantısı
- Engellendi
- Engelleme Nedeni
- Tamamlama zaman damgası
- Tamamlama nedeni
- Hesap Kilitli
- Sahtekarlık Uyarısı
- Engellenmemiş sahtekarlık Uyarısı
- Dil

Atlar (Raporlama için kullanılan):

- Zaman damgası atla
- Atlama (Saniye)
- Kullanıcı adına göre atlama
- Kullanıcı adı
- Ülke Kodu
- Telefon Numarası
- Biçimlendirilmiş telefon numarası
- Dahili numara
- Temiz uzantısı
- Atlama Nedeni
- Tamamlama zaman damgası
- Tamamlama nedeni
- Kullanılan atlama

(Kullanıcı değişiklikleri MFA sunucusu veya AAD eşitleme için kullanılan) değişiklikleri:

- Zaman damgası değiştirme
- Kullanıcı adı
- Yeni Ülke Kodu
- Yeni Telefon Numarası
- Yeni Dahili Numara
- Yeni yedek ülke kodu
- Yeni yedek telefon numarası
- Yeni yedekleme uzantısı
- Yeni PIN
- PIN Değişikliği Gerekiyor
- Eski cihaz belirteci
- Yeni Cihaz Belirteci

## <a name="gather-data-from-mfa-server"></a>MFA sunucudan veri toplayın

MFA sunucusu sürümü 8.0 veya üzerini aşağıdaki süreci kullanıcılar için tüm verileri dışarı aktarmak Yöneticiler sağlar:

- MFA sunucunuza oturum açma, gidin **kullanıcılar** sekmesinde, söz konusu kullanıcı seçin ve tıklatın **Düzenle** düğmesi. Kullanıcı kendi geçerli MFA ayarları sağlamak için her sekmesinin ekran görüntüleri (Alt-PrtScn) alabilir.
- MFA sunucusunun komut satırından yüklemenizi göre yolu değiştirerek aşağıdaki komutu çalıştırın `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` JSON üretmek için dosya biçimlendirilmiş.
- Yöneticiler ayrıca belirli bir kullanıcının toplanan tüm MFA bulut hizmeti bilgilerini dışarı aktarmak için bir seçenek olarak Web hizmeti SDK GetUserGdpr işlemi kullanın veya daha büyük bir raporlama çözümü dahil.
- Arama `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` ve tüm yedeklemeler için "<username>" (tırnak işaretleri dahil) kullanıcı kaydını tüm örneklerini eklenen veya değiştirilen bulunamıyor.
   - Bu kayıtları sınırlı (ancak değil ortadan) kaldırarak **"Oturum kullanıcı değişiklikleri"** MFA sunucusu UX günlüğe kaydetme bölümü, günlük dosyaları sekmesi içinde.
   - Syslog yapılandırdıysanız ve **"Oturum kullanıcı değişiklikleri"** günlük girişlerini syslog yerine toplanabilir sonra MFA sunucusu UX içinde günlüğe kaydetme bölümü, Syslog sekmesi denetlenir.
- Günlük dosyalarını kimlik doğrulama denemesi, işletimsel ve MultiFactorAuthGdpr.exe verme veya Web hizmeti SDK'sını kullanarak sağlanan bilgilere yinelenen olarak kabul edilir ilgili diğer oluşumları MultiFactorAuthSvc.log ve diğer MFA sunucusu kullanıcı adı GetUserGdpr.

## <a name="delete-data-from-mfa-server"></a>MFA sunucusundan verilerini sil

MFA sunucusunun komut satırından yüklemenizi göre yolu değiştirerek aşağıdaki komutu çalıştırın `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` tüm MFA bulut hizmeti bu kullanıcı için toplanan bilgiler silinecek.

- Dışa aktarma işlemine dahil verileri gerçek zamanlı olarak silinmez, ancak tamamen kaldırılacak işletimsel veya yinelenen veri 30 güne kadar sürebilir.
- Yöneticiler ayrıca belirli bir kullanıcının toplanan tüm MFA bulut hizmeti bilgilerini silmek için bir seçenek olarak Web hizmeti SDK DeleteUserGdpr işlemi kullanın veya daha büyük bir raporlama çözümü dahil.

## <a name="gather-data-from-nps-extension"></a>NPS uzantısını verileri toplayın

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview.) dışa aktarma için bir istek yapma.

- MFA bilgileri saat veya tamamlamak için gün sürebilir verme içinde bulunur.
- Kullanıcı adı kimlik doğrulama/AzureMfa/AuthNOptCh, AuthZ/AzureMfa/AuthZAdminCh ve AuthZ/AzureMfa/AuthZOptCh olay günlüklerini oluşumları işletimsel ve dışa aktarma sağlanan bilgilere yinelenen olarak kabul edilir.

## <a name="delete-data-from-nps-extension"></a>NPS uzantısını verileri silme

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview.) tüm MFA bulut hizmeti bu kullanıcı için toplanan bilgiler silmek için Kapat hesabı için bir istek yapma.

- Tamamen kaldırmak veri 30 güne kadar sürebilir.

## <a name="gather-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure MFA AD FS bağdaştırıcısı verileri toplayın

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview.) dışa aktarma için bir istek yapma. 

- MFA bilgileri saat veya tamamlamak için gün sürebilir verme içinde bulunur.
- AD FS izleme/Debug olay günlüklerindeki (etkinse) kullanıcıadı oluşumları işletimsel ve dışa aktarma sağlanan bilgilere yinelenen olarak kabul edilir.

## <a name="delete-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure MFA AD FS bağdaştırıcısı verilerini sil

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview.) tüm MFA bulut hizmeti bu kullanıcı için toplanan bilgiler silmek için Kapat hesabı için bir istek yapma.

- Tamamen kaldırmak veri 30 güne kadar sürebilir.

## <a name="gather-data-for-azure-mfa"></a>Azure MFA için veri toplama

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview.) dışa aktarma için bir istek yapma.

- MFA bilgileri saat veya tamamlamak için gün sürebilir verme içinde bulunur.

## <a name="delete-data-for-azure-mfa"></a>Azure MFA için verilerini sil

Kullanım [Microsoft gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview.) tüm MFA bulut hizmeti bu kullanıcı için toplanan bilgiler silmek için Kapat hesabı için bir istek yapma.

- Tamamen kaldırmak veri 30 güne kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

[MFA sunucusu raporlama](howto-mfa-reporting.md)
