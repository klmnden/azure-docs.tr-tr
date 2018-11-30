---
title: Azure Active Directory'de görev tarafından en az ayrıcalıklı rolleri temsil | Microsoft Docs
description: Azure Active Directory'de kimlik görevler için temsilci seçmek için roller
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 11/08/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: bd63e8379051864a19c473779625a925446185f2
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445030"
---
# <a name="administrator-roles-by-identity-task-in-azure-active-directory"></a>Azure Active Directory'de kimlik görev tarafından yönetici rolleri

Bu makalede, Azure Active Directory'de (Azure AD) en az ayrıcalıklı rolleri atayarak bir kullanıcının Yönetici izinlerini kısıtlamak için gereken bilgileri bulabilirsiniz. Özellik alanı ve görevi gerçekleştirebilirsiniz ek genel yönetici olmayan rollerin yanı sıra her bir görevi gerçekleştirmek için gereken en az ayrıcalıklı rol tarafından düzenlenen yönetici görevlerini bulabilirsiniz.

## <a name="application-proxy"></a>Uygulama proxy’si

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Uygulama proxy'si uygulamasını yapılandırma | Uygulama yöneticisi | 
Bağlayıcı grubu özelliklerini yapılandırma | Uygulama yöneticisi | 
Uygulama kaydı oluşturduğunuzda özelliği tüm kullanıcılar için devre dışı bırakıldı | Uygulama geliştirici | Bulut uygulaması Yöneticisi, uygulama Yöneticisi
Bağlayıcı grubu oluşturma | Uygulama yöneticisi | 
Bağlayıcı grubu silinemiyor | Uygulama yöneticisi | 
Uygulama ara sunucusunu devre dışı bırakma | Uygulama yöneticisi | 
Bağlayıcı hizmeti indir | Uygulama yöneticisi | 
Tüm yapılandırması okuma | Uygulama yöneticisi | 

## <a name="b2c"></a>B2C

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Azure AD B2C dizini oluşturmak | Tüm Konuk olmayan kullanıcılar ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
B2C uygulamaları oluşturma | Genel Yönetici | 
Kurumsal uygulamalar oluşturun | Bulut Uygulaması Yöneticisi | Uygulama Yöneticisi
Oluşturun, okuyun, güncelleştirin ve B2C ilkelerini Sil | Genel Yönetici | 
Oluşturma, okuma, güncelleştirme ve silme kimlik sağlayıcıları | Genel Yönetici | 
Oluşturma, okuma, güncelleştirme ve parola sıfırlama kullanıcı akışları silme | Genel Yönetici | 
Oluşturun, okuyun, güncelleştirin ve profil düzenleme kullanıcı akışları silme | Genel Yönetici | 
Oluşturun, okuyun, güncelleştirin ve oturum açma kullanıcı akışları silme | Genel Yönetici | 
Oluşturun, okuyun, güncelleştirin ve kaydolma kullanıcı akışını Sil |Genel Yönetici | 
Oluşturma, okuma, güncelleştirme ve kullanıcı özniteliklerine Sil | Genel Yönetici | 
Oluşturun, okuyun, güncelleştirin ve kullanıcıları Sil | Genel yönetici ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs))
Tüm yapılandırması okuma | Genel Yönetici | 
Okuma B2C denetim günlükleri | Genel yönetici ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs)) | 

## <a name="company-branding"></a>Şirket markası

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Şirket markası yapılandırma | Genel Yönetici | 
Tüm yapılandırması okuma | Dizin okuyucular | Varsayılan kullanıcı rolü ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="company-properties"></a>Şirket özellikleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Şirket özelliklerini yapılandırın | Genel Yönetici | 

## <a name="connect"></a>Bağlan

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Geçişli kimlik doğrulamasını | Genel Yönetici | 
Tüm yapılandırması okuma | Genel Yönetici | 
Sorunsuz çoklu oturum açma | Genel Yönetici | 

## <a name="connect-health"></a>Connect Health

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Ekleme veya silme Hizmetleri | Sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Eşitleme hata düzeltmeleri Uygula | Katkıda bulunan ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Sahip
Bildirimleri yapılandırma | Katkıda bulunan ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Sahip
Ayarları yapılandırma | Sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Eşitleme bildirimleri yapılandırma | Katkıda bulunan ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Sahip
Okuma ADFS güvenlik raporları | Güvenlik Okuyucu | Katkıda bulunan, sahibi
Tüm yapılandırması okuma | Okuyucu ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda bulunan, sahibi
Okuma Eşitleme hataları | Okuyucu ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda bulunan, sahibi
Okuma Eşitleme Hizmetleri | Okuyucu ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda bulunan, sahibi
Görünüm ölçümleri ve Uyarıları | Okuyucu ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda bulunan, sahibi
Görünüm ölçümleri ve Uyarıları | Okuyucu ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda bulunan, sahibi
Eşitleme hizmeti ölçümlerini görüntüleme ve uyarılar | Okuyucu ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda bulunan, sahibi


## <a name="custom-domain-names"></a>Özel etki alanı adları

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Etki alanlarını yönet | Genel Yönetici | 
Tüm yapılandırması okuma | Dizin okuyucular | Varsayılan kullanıcı rolü ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="domain-services"></a>Etki Alanı Hizmetleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Azure AD Domain Services örneği oluşturma | Genel Yönetici | 
Tüm Azure AD Domain Services görevlerini gerçekleştirme | Azure AD DC Administrators grubu ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-administer-domain#administrative-tasks-you-can-perform-on-a-managed-domain)) | 
Tüm yapılandırması okuma | AD DS hizmetini içeren bir Azure aboneliği üzerinde okuyucusu | 

## <a name="devices"></a>Cihazlar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Cihazı devre dışı bırak | Bulut cihazı yöneticisi | 
Cihazı etkinleştir | Bulut cihazı yöneticisi | 
Okuma temel yapılandırma | Varsayılan kullanıcı rolü ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Okuma Bitlocker anahtarları | Güvenlik Okuyucu | Parola Yöneticisi, Güvenlik Yöneticisi

## <a name="enterprise-applications"></a>Kurumsal uygulamalar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Herhangi bir temsilci atanan izin onayı | Bulut uygulaması yöneticisi | Uygulama yöneticisi
Microsoft Graph veya Azure AD Graph dahil değil uygulama izinleri için onay | Bulut uygulaması yöneticisi | Uygulama yöneticisi
Microsoft Graph veya Azure AD Graph uygulama izinlerini kabul edersiniz | Genel Yönetici | 
Kendi veri erişen uygulamalara izin vermesi | Varsayılan kullanıcı rolü ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Kuruluş uygulaması oluşturma | Bulut uygulaması yöneticisi | Uygulama yöneticisi
Uygulama proxy'si yönetme | Uygulama yöneticisi | 
Kullanıcı ayarlarını yönet | Genel Yönetici | 
Bir grubun veya bir uygulamanın okuma erişimi gözden geçirme | Güvenlik Okuyucu | Güvenlik Yöneticisi, Kullanıcı Yöneticisi
Tüm yapılandırması okuma | Varsayılan kullanıcı rolü ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Kurumsal uygulama Atamaları Güncelleştir | Kurumsal uygulama sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulaması Yöneticisi, uygulama Yöneticisi
Kurumsal uygulama sahipleri güncelleştir | Kurumsal uygulama sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulaması Yöneticisi, uygulama Yöneticisi
Kurumsal uygulama özelliklerini güncelleştir | Kurumsal uygulama sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulaması Yöneticisi, uygulama Yöneticisi
Kurumsal uygulama sağlama güncelleştir | Kurumsal uygulama sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulaması Yöneticisi, uygulama Yöneticisi
Kurumsal uygulama Self Servis update | Kurumsal uygulama sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulaması Yöneticisi, uygulama Yöneticisi
Çoklu oturum açma özellikleri güncelleştirme | Kurumsal uygulama sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulaması Yöneticisi, uygulama Yöneticisi

## <a name="groups"></a>Gruplar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Lisans ata | Kullanıcı hesabı yöneticisi | 
Grup oluştur | Kullanıcı hesabı yöneticisi | 
Oluşturmak, güncelleştirmek veya bir grubun veya bir uygulamanın erişim gözden geçirmesini silme | Kullanıcı hesabı yöneticisi | 
Grup kullanım süresini yönetme | Kullanıcı hesabı yöneticisi | 
Grup ayarlarını yönetme | Genel Yönetici | 
Tüm yapılandırma (dışında gizli üyeliği) okuyun | Dizin okuyucular | Varsayılan kullanıcı rolü ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Gizli okuma üyelik | Grup üyesi | Grup sahibi, parola Yöneticisi, Exchange Yöneticisi, SharePoint Yöneticisi, takımlar yönetici, kullanıcı hesabı yöneticisi
Gizli üyelikle gruplarının üyeliklerini okuyun | Yardım Masası Yöneticisi | Kullanıcı hesabı yöneticisi, takımlar yönetici
Lisans iptal et | Lisans yöneticisi | Kullanıcı hesabı yöneticisi
Grup üyeliği güncelleştir | Grup sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Kullanıcı hesabı yöneticisi
Güncelleştirme Grup sahipleri | Grup sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Kullanıcı hesabı yöneticisi
Güncelleştirme grubu özellikleri | Grup sahibi ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Kullanıcı hesabı yöneticisi

## <a name="identity-protection"></a>Kimlik Koruması

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Uyarı bildirimlerini yapılandırma| Güvenlik Yöneticisi | 
Yapılandırma ve etkinleştirme veya MFA ilkesini devre dışı bırak| Güvenlik Yöneticisi | 
Yapılandırma ve etkinleştirme veya devre dışı oturum açma riski İlkesi| Güvenlik Yöneticisi | 
Yapılandırma ve etkinleştirme veya devre dışı kullanıcı riski İlkesi | Güvenlik Yöneticisi | 
Haftalık özetler yapılandırın | Güvenlik Yöneticisi| 
Tüm risk olayları kapatılamadı | Güvenlik Yöneticisi | 
Güvenlik Açığı kapatmak veya düzeltmek | Güvenlik Yöneticisi | 
Tüm yapılandırması okuma | Güvenlik Okuyucu | 
Tüm risk olayları okuma | Güvenlik Okuyucu | 
Güvenlik açıklarını okuyun | Güvenlik Okuyucu | 

## <a name="licenses"></a>Lisanslar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Lisans ata | Lisans yöneticisi | Kullanıcı hesabı yöneticisi
Tüm yapılandırması okuma | Dizin okuyucular | Varsayılan kullanıcı rolü ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Lisans iptal et | Lisans yöneticisi | Kullanıcı hesabı yöneticisi
Deneyin veya abonelik satın alın | Faturalama yöneticisi | 


## <a name="monitoring---audit-logs"></a>İzleme - Denetim günlükleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Denetim günlüklerini okuma | Rapor okuyucu | Güvenlik okuyucusu, Güvenlik Yöneticisi

## <a name="monitoring---sign-ins"></a>İzleme - oturum açma işlemleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Oturum açma günlüklerini okuyun | Rapor okuyucu | Güvenlik okuyucusu, Güvenlik Yöneticisi

## <a name="multi-factor-authentication"></a>Multi-factor authentication

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Seçilen kullanıcılar tarafından oluşturulan mevcut tüm uygulama parolalarını sil | Genel Yönetici | 
Mfa'yı devre dışı bırak | Genel Yönetici | 
MFA’yı etkinleştirme | Genel Yönetici | 
MFA hizmet ayarlarını yönet | Genel Yönetici | 
Seçilen kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste  | Genel Yönetici | 
Çok öğeli kimlik doğrulamayı hatırlanan tüm cihazlarda geri yükle  | Genel Yönetici | 

## <a name="mfa-server"></a>MFA Sunucusu

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kullanıcı engelle/engelini kaldır | Genel Yönetici | 
Hesap kilitleme yapılandırın | Genel Yönetici | 
Önbelleğe alma kuralları yapılandırma | Genel Yönetici | 
Sahtekarlık Uyarısı'nı yapılandırma | Genel Yönetici
Bildirimleri yapılandırma | Genel Yönetici | 
Bir kerelik geçişi yapılandırma | Genel Yönetici | 
Telefon araması ayarları yapılandırma | Genel Yönetici | 
Sağlayıcılarını yapılandırma | Genel Yönetici | 
Sunucu ayarlarını yapılandırma | Genel Yönetici | 
Etkinlik raporunu okuyun | Genel Yönetici | 
Tüm yapılandırması okuma | Genel Yönetici | 
Sunucu durumu okuma | Genel Yönetici |  

## <a name="organizational-relationships"></a>Kuruluş ilişkileri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kimlik sağlayıcılarını yönet | Genel Yönetici | 
Ayarları yönetme | Genel Yönetici | 
Kullanım koşullarını Yönet | Genel Yönetici | 
Tüm yapılandırması okuma | Genel Yönetici | 

## <a name="password-reset"></a>Parola sıfırlama

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kimlik doğrulama yöntemlerini yapılandırın | Genel Yönetici | 
Özelleştirmeyi Yapılandır | Genel Yönetici | 
Bildirimi yapılandırma | Genel Yönetici | 
Şirket içi tümleştirmeyi Yapılandır | Genel Yönetici | 
Parola sıfırlama özellikleri yapılandırma | Genel Yönetici | 
Kaydı yapılandır | Genel Yönetici | 
Tüm yapılandırması okuma | Güvenlik Yöneticisi Kullanıcı Yöneticisi | 

## <a name="privileged-identity-management"></a>Privileged Identity management

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kullanıcı rollerine atama | Ayrıcalıklı rol yöneticisi | 
Rol ayarlarını yapılandırma | Ayrıcalıklı rol yöneticisi | 
Denetim etkinlikleri görüntüle | Güvenlik okuyucusu | 
Görünüm rol üyelikleri | Güvenlik okuyucusu | 

## <a name="roles-and-administrators"></a>Roller ve yöneticiler

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Rol atamalarını yönetme | Ayrıcalıklı rol yöneticisi | 
Azure AD rolüne okuma erişimi gözden geçirme  | Güvenlik Okuyucu | Güvenlik Yöneticisi, ayrıcalıklı Rol Yöneticisi
Tüm yapılandırması okuma | Varsayılan kullanıcı rolü ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 

## <a name="security---authentication-methods"></a>Güvenlik - kimlik doğrulama yöntemleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kimlik doğrulama yöntemlerini yapılandırın | Genel Yönetici | 
Tüm yapılandırması okuma | Genel Yönetici | 

## <a name="security---conditional-access"></a>Güvenlik - koşullu erişim

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Mfa'yı yapılandırma güvenilen IP adresleri | Koşullu erişim yöneticisi | 
Özel denetimler oluşturma | Koşullu erişim yöneticisi | Güvenlik yöneticisi
Adlandırılmış konumlar oluşturma | Koşullu erişim yöneticisi | Güvenlik yöneticisi
İlkeleri oluşturma | Koşullu erişim yöneticisi | Güvenlik yöneticisi
Kullanım Koşulları oluşturma | Koşullu erişim yöneticisi | Güvenlik yöneticisi
VPN bağlantı sertifikası oluşturma | Koşullu erişim yöneticisi | Güvenlik yöneticisi
Klasik ilke Sil | Koşullu erişim yöneticisi | Güvenlik yöneticisi
Kullanım koşullarını silme | Koşullu erişim yöneticisi | Güvenlik yöneticisi
VPN bağlantısı sertifikasını Sil | Koşullu erişim yöneticisi | Güvenlik yöneticisi
Klasik ilke devre dışı bırak | Koşullu erişim yöneticisi | Güvenlik yöneticisi
Özel denetimleri yönetmek | Koşullu erişim yöneticisi | Güvenlik yöneticisi
Adlandırılmış konumları yönetin | Koşullu erişim yöneticisi | Güvenlik yöneticisi
Kullanım koşullarını Yönet | Koşullu erişim yöneticisi | Güvenlik yöneticisi
Tüm yapılandırması okuma | Güvenlik okuyucusu | Güvenlik yöneticisi
Adlandırılmış konumlar okuma | Güvenlik okuyucusu | Koşullu Erişim Yöneticisi, Güvenlik Yöneticisi

## <a name="security---identity-security-score"></a>Güvenlik - kimlik güvenlik puanı

Görev | En az ayrıcalıklı rol | Ek roller | 
---- | --------------------- | ----------------
Tüm yapılandırması okuma | Güvenlik okuyucusu | Güvenlik yöneticisi
Okuma güvenlik puanı | Güvenlik okuyucusu | Güvenlik yöneticisi
Olay durumunu güncelleştirme | Güvenlik yöneticisi | 

## <a name="security---risky-sign-ins"></a>Güvenlik - riskli oturum açma işlemleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Tüm yapılandırması okuma | Güvenlik Okuyucu | 
Riskli oturum açma işlemleri oku | Güvenlik Okuyucu | 

## <a name="security---users-flagged-for-risk"></a>Güvenlik - risk için işaretlenmiş kullanıcılar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Tüm olayları kapatabilirsiniz. | Güvenlik Yöneticisi | 
Tüm yapılandırması okuma | Güvenlik Okuyucu | 
Riskli olduğu belirlenen kullanıcıların okuma | Güvenlik Okuyucu | 

## <a name="users"></a>Kullanıcılar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Dizin rolü için kullanıcı ekleme | Ayrıcalıklı rol yöneticisi | 
Kullanıcıyı gruba ekleyin | Kullanıcı hesabı yöneticisi | 
Lisans ata | Lisans yöneticisi | Kullanıcı hesabı yöneticisi
Konuk kullanıcı oluşturma | Konuk davet eden | Kullanıcı hesabı yöneticisi
Kullanıcı oluştur | Kullanıcı hesabı yöneticisi | 
Kullanıcıları Sil | Kullanıcı hesabı yöneticisi | 
Sınırlı yöneticilerin (belgelerine bakın), yenileme belirteçleri geçersiz kıl | Kullanıcı hesabı yöneticisi | 
Yenileme belirteçleri (belgelerine göz atın) yönetici olmayan biri geçersiz | Parola yöneticisi | Kullanıcı hesabı yöneticisi
Ayrıcalıklı yöneticilerin (belgelerine bakın), yenileme belirteçleri geçersiz kıl | Genel Yönetici | 
Okuma temel yapılandırma | Varsayılan kullanıcı rolü ([belgelerine bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions) | 
Sınırlı yöneticilerin (belgelerine göz atın) parola sıfırlama | Kullanıcı hesabı yöneticisi | 
(Belgelerine göz atın) yönetici olmayanlar parolasını sıfırlama | Parola yöneticisi | Kullanıcı hesabı yöneticisi
Ayrıcalıklı yöneticilerin, parola sıfırlama | Genel Yönetici | 
Lisans iptal et | Lisans yöneticisi | Kullanıcı hesabı yöneticisi
Kullanıcı asıl adı dışındaki tüm özelliklerini güncelleştir | Kullanıcı hesabı yöneticisi | 
Kullanıcı asıl adı (belgelerine göz atın) sınırlı yöneticilerin güncelleştir | Kullanıcı hesabı yöneticisi | 
Kullanıcı asıl adı (belgelerine göz atın) ayrıcalıklı yöneticilerin özellikte güncelleştir | Genel Yönetici | 
Kullanıcı ayarlarını güncelleştirme | Genel Yönetici | 


## <a name="support"></a>Destek

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Destek bileti gönderin | Hizmet Yöneticisi | Uygulama Yöneticisi, yönetici, bulut uygulaması Yöneticisi, uyumluluk Yöneticisi, Dynamics 365 Yönetici fatura, Masaüstü Analytics yönetici, Exchange Yöneticisi, parola Yöneticisi, Information Protection Yönetici, Intune Yöneticisi, Skype Kurumsal Yöneticisi, Power BI Yöneticisi, ayrıcalıklı kimlik yöneticisi, SharePoint Yöneticisi, takımlar iletişimleri Yöneticisi, takımlar yönetici, yönetici kullanıcı, Workplace Analytics Yöneticisi

## <a name="next-steps"></a>Sonraki adımlar

* [Atama veya azure AD yönetici rollerini kaldırın](directory-manage-roles-portal.md)
* [Azure AD yönetici rollerini başvurusu](directory-assign-admin-roles.md)
