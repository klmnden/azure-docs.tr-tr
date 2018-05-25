---
title: Azure Active Directory (Azure AD) denetim etkinliği başvurusu | Microsoft Docs
description: Azure Active Directory’de (Azure AD) denetim günlüklerinize kaydedilebilecek denetim etkinliklerine genel bakış elde edin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: be19130f7a2f45049cd7e08bb961ac6cb5aa14ff
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-ad-audit-activity-reference"></a>Azure AD denetim etkinliği başvurusu

Azure Active Directory’deki (Azure AD) raporlama özelliğiyle ortamınızın nasıl çalıştığını belirlemek için gereken bilgileri alabilirsiniz.

Azure AD'nin raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
    - **Denetim günlükleri** - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Azure AD içindeki kullanıcı, uygulama, grup, rol, ilke, kimlik doğrulama vb. herhangi bir kaynakta yapılan değişiklikler, denetim günlüklerinin örnekleridir.
- **Güvenlik** 
    - **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. Riskli oturum açma işlemleri.
    - **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. Riskli oldukları belirlenen kullanıcılar.

Bu makalede, denetim günlüklerinize kaydedilebilecek denetim etkinlikleri listelenir.
 


## <a name="access-reviews"></a>Erişim gözden geçirmeleri

|Denetim Kategorisi|Etkinlik|
|---|---|
|Dizin Yönetimi|Yönetim birimi ekleme|
|Dizin Yönetimi|Yönetim birimine üye ekleme|
|Dizin Yönetimi|Yönetim birimini silme|
|Dizin Yönetimi|Yönetim biriminden üyeyi kaldırma|
|Dizin Yönetimi|Yönetim birimini güncelleştirme|
|Dizin Yönetimi|Yönetim|
|Kullanıcı Yönetimi|Dizin işlemi|
|Kullanıcı Yönetimi|Dışarı Aktarma|
|Kullanıcı Yönetimi|İçeri Aktarma|
|Kullanıcı Yönetimi|Diğer|
|Kullanıcı Yönetimi|Emanet işleme|
|Kullanıcı Yönetimi|Eşitleme kuralı eylemi|
|Kullanıcı Yönetimi|Uygulama ekleme|
|Kullanıcı Yönetimi|Uygulamayı silme|
|Kullanıcı Yönetimi|Uygulamayı güncelleştirme|
|Kullanıcı Yönetimi|Uygulamada çoklu oturum açma modunu güncelleştirme|
|Kullanıcı Yönetimi|Otomatik Parola Geçişi|
|Kullanıcı Yönetimi|V2 uygulama izinleri ekleme|
|Kullanıcı Yönetimi|V1 uygulaması oluşturma|
|Kullanıcı Yönetimi|V2 uygulaması oluşturma|
|Kullanıcı Yönetimi|V1 uygulamasını silme|
|Kullanıcı Yönetimi|V2 uygulamasını silme|
|Kullanıcı Yönetimi|V2 uygulama iznini silme|
|Kullanıcı Yönetimi|V1 ve V2 uygulamalarını alma|
|Kullanıcı Yönetimi|V1 uygulaması alma|
|Kullanıcı Yönetimi|V1 uygulamaları alma|
|Kullanıcı Yönetimi|V2 uygulaması alma|
|Kullanıcı Yönetimi|V2 uygulamaları alma|




## <a name="account-provisioning"></a>Hesap sağlama

|Denetim Kategorisi|Etkinlik|
|---|---|
|Uygulama Yönetimi|V2 uygulama izinleri alma|
|Uygulama Yönetimi|Geçerli kiracıdaki V2 uygulama hizmet sorumlularını alma|
|Uygulama Yönetimi|V1 uygulamasını güncelleştirme|
|Uygulama Yönetimi|V2 uygulamasını güncelleştirme|
|Uygulama Yönetimi|V2 uygulama iznini güncelleştirme|
|Uygulama Yönetimi|OAuth2PermissionGrant ekleme|
|Uygulama Yönetimi|Hizmet sorumlusuna uygulama rol ataması ekleme|

## <a name="application-proxy"></a>Uygulama proxy’si

|Denetim Kategorisi|Etkinlik|
|---|---|
|Uygulama Yönetimi|Uygulama ekleme|
|Uygulama Yönetimi|Uygulamaya sahip ekleme|
|Uygulama Yönetimi|Hizmet sorumlusuna sahip ekleme|
|Uygulama Yönetimi|Hizmet sorumlusuna ilke ekleme|
|Dizin Yönetimi|Hizmet sorumlusu ekleme|
|Dizin Yönetimi|Hizmet sorumlusu kimlik bilgileri ekleme|
|Dizin Yönetimi|Uygulama onayı|
|Dizin Yönetimi|Uygulamayı silme|
|Dizin Yönetimi|Uygulamayı Kalıcı Olarak Silme|
|Dizin Yönetimi|OAuth2PermissionGrant’ı kaldırma|
|Dizin Yönetimi|Hizmet sorumlusundan uygulama rolü atamasını kaldırma|
|Dizin Yönetimi|Uygulamadan sahibi kaldırma|
|Kaynak|Hizmet sorumlusundan sahibi kaldırma|
|Kaynak|Hizmet sorumlusundan ilkeyi kaldırma|
|Kaynak|Hizmet sorumlusunu kaldırma|


## <a name="automated-password-rollover"></a>Otomatik parola geçişi

|Denetim Kategorisi|Etkinlik|
|---|---|
|Uygulama Yönetimi|Hizmet sorumlusu kimlik bilgilerini kaldırma|


## <a name="b2c"></a>B2C

|Denetim Kategorisi|Etkinlik|
|---|---|
|Uygulama Yönetimi|Uygulamayı geri yükleme|
|Uygulama Yönetimi|İzni iptal etme|
|Uygulama Yönetimi|Uygulamayı güncelleştirme|
|Uygulama Yönetimi|Dış gizli anahtarları güncelleştirme|
|Uygulama Yönetimi|Hizmet sorumlusunu güncelleştirme|
|Uygulama Yönetimi|Uygulamaya bir erişim belirteci verme|
|Uygulama Yönetimi|Uygulamaya bir yetkilendirme kodu verme|
|Uygulama Yönetimi|Uygulamaya bir id_token verme|
|Uygulama Yönetimi|Yerel hesap kimlik bilgilerini doğrulama|
|Uygulama Yönetimi|Kullanıcı kimlik doğrulamasını doğrulama|
|Uygulama Yönetimi|V2 uygulama izinleri ekleme|
|Uygulama Yönetimi|CPIM anahtar kapsayıcısına ASCII gizli anahtarına dayalı bir anahtar ekleme|
|Uygulama Yönetimi|CPIM anahtar kapsayıcısına bir anahtar ekleme|
|Uygulama Yönetimi|AdminPolicyDatas-SetResources|
|Uygulama Yönetimi|AdminUserJourneys-GetResources|
|Uygulama Yönetimi|AdminUserJourneys-RemoveResources|
|Kimlik Doğrulaması|AdminUserJourneys-SetResources|
|Kimlik Doğrulaması|IdentityProvider oluşturma|
|Kimlik Doğrulaması|V1 uygulaması oluşturma|
|Kimlik Doğrulaması|V2 uygulaması oluşturma|
|Kimlik Doğrulaması|Kiracıda özel etki alanları oluşturma|
|Yetkilendirme|Yeni bir AdminUserJourney oluşturma|
|Yetkilendirme|Yerelleştirilmiş kaynak json oluşturma|
|Yetkilendirme|Yeni Özel IDP oluşturma|
|Yetkilendirme|Yeni IDP oluşturma|
|Yetkilendirme|B2C dizin kaynağı oluşturma veya güncelleştirme|
|Yetkilendirme|İlke oluşturma|
|Yetkilendirme|trustFramework ilkesi oluşturma|
|Yetkilendirme|Yapılandırılabilir önek ile trustFramework ilkesi oluşturma|
|Yetkilendirme|Kullanıcı özniteliği oluşturma|
|Yetkilendirme|CreateTrustFrameworkPolicy|
|Yetkilendirme|Yeni bir AdminUserJourney oluşturur veya güncelleştirir|
|Yetkilendirme|IDP’yi silme|
|Yetkilendirme|IdentityProvider’ı silme|
|Yetkilendirme|V1 uygulamasını silme|
|Yetkilendirme|V2 uygulamasını silme|
|Yetkilendirme|V2 uygulama iznini silme|
|Yetkilendirme|B2C dizini kaynağını silme|
|Yetkilendirme|CPIM anahtar kapsayıcısını silme|
|Yetkilendirme|trustFramework ilkesini silme|
|Yetkilendirme|Kullanıcı özniteliğini silme|
|Yetkilendirme|B2C özelliğini etkinleştirme|
|Yetkilendirme|Bir abonelikteki B2C dizin kaynaklarını alma|
|Yetkilendirme|Özel IDP alma|
|Yetkilendirme|IDP alma|
|Yetkilendirme|V1 ve V2 uygulamalarını alma|
|Yetkilendirme|V1 uygulaması alma|
|Yetkilendirme|V1 uygulamaları alma|
|Yetkilendirme|V2 uygulaması alma|
|Yetkilendirme|V2 uygulamaları alma|
|Yetkilendirme|B2C dizin kaynağı alma|
|Yetkilendirme|Kiracıdaki özel etki alanlarının listesini alma|
|Yetkilendirme|Kullanıcı yolculuğu alma|
|Yetkilendirme|Kullanıcı yolculuğu için izin verilen uygulama taleplerini alma|
|Yetkilendirme|Kullanıcı yolculuğu için izin verilen otomatik olarak onaylanan talepler alma|
|Yetkilendirme|İlkenin izin verilen otomatik olarak onaylanan taleplerini alma|
|Yetkilendirme|Kullanılabilir çıktı talepleri listesini alma|
|Yetkilendirme|Kullanıcı yolculuğu için içerik tanımlarını alma|
|Yetkilendirme|Belirli bir yönetim akışı için idp’leri alma|
|Yetkilendirme|JWK içindeki anahtar kapsayıcısı etkin anahtar meta verilerini alma|
|Yetkilendirme|Tüm yönetim akışlarının listesini alma|
|Yetkilendirme|Tüm kullanıcılar için tüm yönetim akışlarının etiketlerinin listesini alma|
|Yetkilendirme|Bir kullanıcı için kiracıların listesini alma|
|Yetkilendirme|Yerel hesapların otomatik olarak onaylanan taleplerini alma|
|Yetkilendirme|Yerelleştirilmiş kaynak json alma|
|Yetkilendirme|Microsoft.AzureActiveDirectory kaynak sağlayıcısının işlemlerini alma|
|Yetkilendirme|İlkeleri alma|
|Yetkilendirme|İlke alma|
|Yetkilendirme|Bir kiracının kaynak özelliklerini alma|
|Yetkilendirme|Desteklenen IDP listesini alma|
|Yetkilendirme|Kullanıcı yolculuğunun desteklenen IDP listesini alma|
|Yetkilendirme|Kiracı Bilgilerini alma|
|Yetkilendirme|Kiracının izin verilen özelliklerini alma|
|Yetkilendirme|Kiracı tanımlı Özel IDP listesini alma|
|Yetkilendirme|Kiracı tanımlı IDP listesini alma|
|Yetkilendirme|Kiracı tanımlı yerel IDP listesini alma|
|Yetkilendirme|Kaynak oluşturma için bir kullanıcının kiracı ayrıntılarını alma|
|Yetkilendirme|Kiracı listesini alma|
|Yetkilendirme|tenantDomains alma|
|Yetkilendirme|CPIM için varsayılan desteklenen kültürü alma|
|Yetkilendirme|Bir yönetim akışının ayrıntılarını alma|
|Yetkilendirme|Bu kiracı için UserJourneys listesini alma|
|Yetkilendirme|CPIM için kullanılabilir desteklenen kültürler kümesini alma|
|Yetkilendirme|trustFramework ilkesini alma|
|Yetkilendirme|trustFramework ilkesini xml olarak alma|
|Yetkilendirme|Kullanıcı özniteliğini alma|
|Yetkilendirme|Kullanıcı özniteliklerini alma|
|Yetkilendirme|Kullanıcı yolculuğu listesini alma|
|Yetkilendirme|GetIEFPolicies|
|Yetkilendirme|GetIdentityProviders|
|Yetkilendirme|GetTrustFrameworkPolicy|
|Yetkilendirme|Bir CPIM anahtar kapsayıcısını jwk biçiminde alır|
|Yetkilendirme|Kiracıdaki anahtar kapsayıcılarının listesini alır|
|Yetkilendirme|Kiracı türünü alır|
|Yetkilendirme|MigrateTenantMetadata|
|Yetkilendirme|IdentityProvider için düzeltme eki uygulama|
|Yetkilendirme|PutTrustFrameworkPolicy|
|Yetkilendirme|PutTrustFrameworkpolicy|
|Yetkilendirme|Kullanıcı yolculuğunu kaldırma|
|Yetkilendirme|CPIM anahtar kapsayıcısı yedeklemesini geri yükleme|
|Yetkilendirme|V2 uygulama izinleri alma|
|Yetkilendirme|Geçerli kiracıdaki V2 uygulama hizmet sorumlularını alma|
|Yetkilendirme|Özel IDP’yi güncelleştirme|
|Yetkilendirme|IDP’yi güncelleştirme|
|Yetkilendirme|Yerel IDP’yi güncelleştirme|
|Yetkilendirme|V1 uygulamasını güncelleştirme|
|Yetkilendirme|V2 uygulamasını güncelleştirme|
|Yetkilendirme|V2 uygulama iznini güncelleştirme|
|Yetkilendirme|İlkeyi güncelleştirme|
|Yetkilendirme|Kullanıcı özniteliğini güncelleştirme|
|Yetkilendirme|CPIM şifrelenmiş anahtarını karşıya yükleme|
|Yetkilendirme|Kullanıcı Yetkilendirmesi: Kiracı featureset için API devre dışı bırakıldı|
|Yetkilendirme|Kullanıcı Yetkilendirmesi: Kullanıcıya 'Kiracı Yöneticisi' olarak erişim izni verildi|
|Yetkilendirme|Kullanıcı Yetkilendirmesi: Kullanıcıya 'Kimliği Doğrulanmış Kullanıcılar' erişim hakları verildi|
|Yetkilendirme|B2C özelliğinin etkinleştirildiğini doğrulama|
|Yetkilendirme|Özelliğin etkinleştirildiğini doğrulama|
|Yetkilendirme|Program oluşturma|
|Yetkilendirme|Program silme|
|Yetkilendirme|Program denetimini bağlama|
|Yetkilendirme|Azure AD Erişim Gözden Geçirmelerine Ekleme|
|Yetkilendirme|Program denetiminin bağlantısını kaldırma|
|Yetkilendirme|Program güncelleştirme|
|Yetkilendirme|Masaüstü Sso’yu devre dışı bırakma|
|Yetkilendirme|Belirli bir etki alanı için Masaüstü Sso’yu devre dışı bırakma|
|Yetkilendirme|Uygulama ara sunucusunu devre dışı bırakma|
|Yetkilendirme|Geçişli kimlik doğrulamasını devre dışı bırakma|
|Yetkilendirme|Masaüstü Sso’yu etkinleştirme|
|Dizin Yönetimi|Belirli bir etki alanı için Masaüstü Sso’yu etkinleştirme|
|Dizin Yönetimi|Uygulama ara sunucusunu etkinleştirme|
|Dizin Yönetimi|Geçişli kimlik doğrulamasını etkinleştirme|
|Dizin Yönetimi|Kiracıda özel etki alanları oluşturma|
|Dizin Yönetimi|B2C özelliğini etkinleştirme|
|Dizin Yönetimi|Kiracıdaki özel etki alanlarının listesini alma|
|Dizin Yönetimi|Bir kiracının kaynak özelliklerini alma|
|Dizin Yönetimi|Kiracı Bilgilerini alma|
|Dizin Yönetimi|Kiracının izin verilen özelliklerini alma|
|Dizin Yönetimi|tenantDomains alma|
|Anahtar|Kiracı türünü alır|
|Anahtar|B2C özelliğinin etkinleştirildiğini doğrulama|
|Anahtar|Özelliğin etkinleştirildiğini doğrulama|
|Anahtar|Şirkete iş ortağı ekleme|
|Anahtar|Doğrulanmamış etki alanı ekleme|
|Anahtar|Doğrulanmış etki alanı ekleme|
|Anahtar|Şirket oluşturma|
|Anahtar|Şirket ayarları oluşturma|
|Anahtar|Şirket ayarlarını silme|
|Anahtar|İş ortağını indirgeme|
|Anahtar|Dizin silindi|
|Diğer|Dizin kalıcı olarak silindi|
|Diğer|Dizin silinmek üzere zamanlandı|
|Kaynak|Şirketi iş ortağına yükseltme|
|Kaynak|Hak yönetimi özelliklerini temizleme|
|Kaynak|Şirketten iş ortağını kaldırma|
|Kaynak|Doğrulanmamış etki alanını kaldırma|
|Kaynak|Doğrulanmış etki alanını kaldırma|
|Kaynak|Şirket Bilgilerini Ayarlama|
|Kaynak|DirSync özelliğini ayarlama|
|Kaynak|DirSyncEnabled bayrağını ayarlama|
|Kaynak|İş Ortaklığı Ayarlama|
|Kaynak|Yanlışlıkla silme eşiğini ayarlama|
|Kaynak|Şirket tarafından izin verilen veri konumunu ayarlama|
|Kaynak|Şirket çok uluslu özelliğini etkin olarak ayarlama|
|Kaynak|Kiracıda dizin özelliğini ayarlama|
|Kaynak|Etki alanı kimlik doğrulaması ayarlama|
|Kaynak|Etki alanında federasyon ayarlarını belirleme|
|Kaynak|Parola ilkesi ayarlama|
|Kaynak|Rights management özelliklerini ayarlama|
|Kaynak|Şirketi güncelleştirme|
|Kaynak|Şirket ayarlarını güncelleştirme|
|Kaynak|Etki alanını güncelleştirme|
|Kaynak|Etki alanını doğrulama|
|Kaynak|E-posta ile doğrulanmış etki alanını doğrulama|
|Kaynak|Ekleme|
|Kaynak|Uyarı ayarlarını güncelleştirme|
|Kaynak|Haftalık özet ayarlarını güncelleştirme|
|Kaynak|Dizin için parola geri yazmayı devre dışı bırakma|
|Kaynak|Dizin için parola geri yazmayı etkinleştirme|
|Kaynak|Gruba uygulama rolü ataması ekleme|
|Kaynak|Grup ekleme|
|Kaynak|Gruba üye ekleme|
|Kaynak|Gruba sahip ekleme|
|Kaynak|Grup ayarları oluşturma|
|Kaynak|Grubu silme|
|Kaynak|Grup ayarlarını silme|
|Kaynak|Kullanıcılara grup tabanlı lisans uygulamayı sonlandırma|
|Kaynak|Grubu Kalıcı Olarak Silme|
|Kaynak|Gruptan uygulama rolü atamasını kaldırma|
|Kaynak|Gruptan üyeyi kaldırma|
|Kaynak|Gruptan sahibi kaldırma|
|Kaynak|Grubu geri yükleme|
|Kaynak|Grup lisansı ayarlama|
|Kaynak|Grubu kullanıcı tarafından yönetilecek şekilde ayarlama|
|Kaynak|Kullanıcılara grup tabanlı lisans uygulamaya başlama|
|Kaynak|Tetikleyici grubu lisans yeniden hesaplaması|
|Kaynak|Grubu güncelleştirme|
|Kaynak|Grup ayarlarını güncelleştirme|
|Kaynak|Üye Ekleme|
|Kaynak|Grup Oluşturma|
|Kaynak|Grubu Silme|
|Kaynak|Üye Kaldırma|
|Kaynak|Grubu Güncelleştirme|
|Kaynak|Onay bekleyen bir gruba katılma isteğini onaylama|
|Kaynak|Onay bekleyen bir gruba katılma isteğini iptal etme|
|Kaynak|Yaşam döngüsü yönetim ilkesi oluşturma|
|Kaynak|Onay bekleyen bir gruba katılma isteğini silme|
|Kaynak|Onay bekleyen bir gruba katılma isteğini reddetme|
|Kaynak|Grubu yenileme|
|Kaynak|Bir gruba katılma isteğinde bulunma|
|Kaynak|Dinamik grup özelliklerini ayarlama|
|Kaynak|Yaşam döngüsü yönetim ilkesini güncelleştirme|
|Kaynak|CPIM anahtar kapsayıcısına ASCII gizli anahtarına dayalı bir anahtar ekleme|
|Kaynak|CPIM anahtar kapsayıcısına bir anahtar ekleme|
|Kaynak|CPIM anahtar kapsayıcısını silme|
|Kaynak|Anahtar kapsayıcısını silme|
|Kaynak|JWK içindeki anahtar kapsayıcısı etkin anahtar meta verilerini alma|
|Kaynak|Anahtar kapsayıcısı meta verilerini alma|
|Kaynak|Bir CPIM anahtar kapsayıcısını jwk biçiminde alır|
|Kaynak|Kiracıdaki anahtar kapsayıcılarının listesini alır|
|Kaynak|CPIM anahtar kapsayıcısı yedeklemesini geri yükleme|
|Kaynak|Anahtar kapsayıcısını kaydetme|
|Kaynak|CPIM şifrelenmiş anahtarını karşıya yükleme|
|Kaynak|Uygulamaya bir yetkilendirme kodu verme|
|Kaynak|Uygulamaya bir id_token verme|


## <a name="core-directory"></a>Çekirdek dizin

|Denetim Kategorisi|Etkinlik|
|---|---|
|Yönetim Birimi Yönetimi|Tek bir risk olayı türünü indirme|
|Yönetim Birimi Yönetimi|Yöneticileri ve haftalık özet katılımı durumunu indirme|
|Yönetim Birimi Yönetimi|Tüm risk olayı türlerini indirme|
|Yönetim Birimi Yönetimi|Ücretsiz kullanıcı risk olaylarını indirme|
|Yönetim Birimi Yönetimi|Riskli olduğu belirlenen kullanıcıları indirme|
|Uygulama Yönetimi|İşlenen toplu davetler|
|Uygulama Yönetimi|Karşıya yüklenen toplu davetler|
|Uygulama Yönetimi|İlkeye sahip ekleme|
|Uygulama Yönetimi|İlke ekleme|
|Uygulama Yönetimi|İlkeyi silme|
|Uygulama Yönetimi|İlke kimlik bilgilerini kaldırma|
|Uygulama Yönetimi|İlkeyi güncelleştirme|
|Uygulama Yönetimi|MFA kayıt ilkesini ayarlama|
|Uygulama Yönetimi|Oturum açma riski ilkesini ayarlama|
|Uygulama Yönetimi|Kullanıcı riski ilkesini ayarlama|
|Uygulama Yönetimi|Kullanım Koşullarını Kabul Etme|
|Uygulama Yönetimi|Kullanım Koşulları Oluşturma|
|Uygulama Yönetimi|Kullanım Koşullarını Reddetme|
|Uygulama Yönetimi|Kullanım Koşullarını Silme|
|Uygulama Yönetimi|Kullanım Koşullarını Düzenleme|
|Uygulama Yönetimi|Kullanım Koşullarını Yayımlama|
|Uygulama Yönetimi|Kullanım Koşullarını Yayımdan Kaldırma|
|Uygulama Yönetimi|Uygulama SSL sertifikası ekleme|
|Uygulama Yönetimi|SSL bağlamasını silme|
|Uygulama Yönetimi|Bağlayıcıyı kaydetme|
|Uygulama Yönetimi|AdminPolicyDatas-RemoveResources|
|Uygulama Yönetimi|AdminPolicyDatas-SetResources|
|Uygulama Yönetimi|AdminUserJourneys-GetResources|
|Dizin Yönetimi|AdminUserJourneys-RemoveResources|
|Dizin Yönetimi|AdminUserJourneys-SetResources|
|Dizin Yönetimi|IdentityProvider oluşturma|
|Dizin Yönetimi|Yeni bir AdminUserJourney oluşturma|
|Dizin Yönetimi|Yerelleştirilmiş kaynak json oluşturma|
|Dizin Yönetimi|Yeni Özel IDP oluşturma|
|Dizin Yönetimi|Yeni IDP oluşturma|
|Dizin Yönetimi|B2C dizin kaynağı oluşturma veya güncelleştirme|
|Dizin Yönetimi|İlke oluşturma|
|Dizin Yönetimi|trustFramework ilkesi oluşturma|
|Dizin Yönetimi|Yapılandırılabilir önek ile trustFramework ilkesi oluşturma|
|Dizin Yönetimi|Kullanıcı özniteliği oluşturma|
|Dizin Yönetimi|CreateTrustFrameworkPolicy|
|Dizin Yönetimi|IDP’yi silme|
|Dizin Yönetimi|IdentityProvider’ı silme|
|Dizin Yönetimi|B2C dizini kaynağını silme|
|Dizin Yönetimi|trustFramework ilkesini silme|
|Dizin Yönetimi|Kullanıcı özniteliğini silme|
|Dizin Yönetimi|Bir kaynak grubundaki B2C dizin kaynaklarını alma|
|Dizin Yönetimi|Bir abonelikteki B2C dizin kaynaklarını alma|
|Dizin Yönetimi|Özel IDP alma|
|Dizin Yönetimi|IDP alma|
|Dizin Yönetimi|B2C dizin kaynağı alma|
|Dizin Yönetimi|Kullanıcı yolculuğu alma|
|Dizin Yönetimi|Kullanıcı yolculuğu için izin verilen uygulama taleplerini alma|
|Dizin Yönetimi|Kullanıcı yolculuğu için izin verilen otomatik olarak onaylanan talepler alma|
|Dizin Yönetimi|İlkenin izin verilen otomatik olarak onaylanan taleplerini alma|
|Dizin Yönetimi|Kullanılabilir çıktı talepleri listesini alma|
|Dizin Yönetimi|Kullanıcı yolculuğu için içerik tanımlarını alma|
|Dizin Yönetimi|Belirli bir yönetim akışı için idp’leri alma|
|Dizin Yönetimi|Tüm yönetim akışlarının listesini alma|
|Dizin Yönetimi|Tüm kullanıcılar için tüm yönetim akışlarının etiketlerinin listesini alma|
|Grup Yönetimi|Bir kullanıcı için kiracıların listesini alma|
|Grup Yönetimi|Yerel hesapların otomatik olarak onaylanan taleplerini alma|
|Grup Yönetimi|Yerelleştirilmiş kaynak json alma|
|Grup Yönetimi|Microsoft.AzureActiveDirectory kaynak sağlayıcısının işlemlerini alma|
|Grup Yönetimi|İlkeleri alma|
|Grup Yönetimi|İlke alma|
|Grup Yönetimi|Desteklenen IDP listesini alma|
|Grup Yönetimi|Kullanıcı yolculuğunun desteklenen IDP listesini alma|
|Grup Yönetimi|Kiracı tanımlı Özel IDP listesini alma|
|Grup Yönetimi|Kiracı tanımlı IDP listesini alma|
|Grup Yönetimi|Kiracı tanımlı yerel IDP listesini alma|
|Grup Yönetimi|Kaynak oluşturma için bir kullanıcının kiracı ayrıntılarını alma|
|Grup Yönetimi|CPIM için varsayılan desteklenen kültürü alma|
|Grup Yönetimi|Bir yönetim akışının ayrıntılarını alma|
|Grup Yönetimi|Bu kiracı için UserJourneys listesini alma|
|Grup Yönetimi|CPIM için kullanılabilir desteklenen kültürler kümesini alma|
|Grup Yönetimi|trustFramework ilkesini alma|
|Grup Yönetimi|trustFramework ilkesini xml olarak alma|
|Grup Yönetimi|Kullanıcı özniteliğini alma|
|İlke Yönetimi|Kullanıcı özniteliklerini alma|
|İlke Yönetimi|Kullanıcı yolculuğu listesini alma|
|İlke Yönetimi|GetIEFPolicies|
|İlke Yönetimi|GetIdentityProviders|
|İlke Yönetimi|GetTrustFrameworkPolicy|
|Kaynak|MigrateTenantMetadata|
|Kaynak|Kaynakları taşıma|
|Kaynak|IdentityProvider için düzeltme eki uygulama|
|Kaynak|PutTrustFrameworkPolicy|
|Kaynak|PutTrustFrameworkpolicy|
|Kaynak|Kullanıcı yolculuğunu kaldırma|
|Kaynak|Özel IDP’yi güncelleştirme|
|Kaynak|IDP’yi güncelleştirme|
|Kaynak|Yerel IDP’yi güncelleştirme|
|Kaynak|B2C dizini kaynağını güncelleştirme|
|Kaynak|İlkeyi güncelleştirme|
|Kaynak|Abonelik durumunu güncelleştirme|
|Rol Yönetimi|Kullanıcı özniteliğini güncelleştirme|
|Rol Yönetimi|Kaynakları taşımayı doğrulama|
|Rol Yönetimi|Cihaz ekleme|
|Rol Yönetimi|Cihaz yapılandırması ekleme|
|Rol Yönetimi|Cihaza kayıtlı sahip ekleme|
|Rol Yönetimi|Cihaza kayıtlı kullanıcılar ekleme|
|Rol Yönetimi|Cihazı silme|
|Rol Yönetimi|Cihaz yapılandırmasını silme|
|Rol Yönetimi|Cihaz artık uyumlu değil|
|Rol Yönetimi|Cihaz artık yönetilen cihaz değil|
|Kullanıcı Yönetimi|Cihazdan kayıtlı sahibi kaldırma|
|Kullanıcı Yönetimi|Cihazdan kayıtlı kullanıcıları kaldırma|
|Kullanıcı Yönetimi|Cihazı güncelleştirme|
|Kullanıcı Yönetimi|Cihaz yapılandırmasını güncelleştirme|
|Kullanıcı Yönetimi|Role uygun üye ekleme|
|Kullanıcı Yönetimi|Role üye ekleme|
|Kullanıcı Yönetimi|Rol tanımına rol ataması ekleme|
|Kullanıcı Yönetimi|Şablondan rol ekleme|
|Kullanıcı Yönetimi|Role kapsamlı üye ekleme|
|Kullanıcı Yönetimi|Rolden uygun üyeyi kaldırma|
|Kullanıcı Yönetimi|Rolden üyeyi kaldırma|
|Kullanıcı Yönetimi|Rol tanımından rol atamasını kaldırma|
|Kullanıcı Yönetimi|Rolden kapsamlı üyeyi kaldırma|
|Kullanıcı Yönetimi|Rolü güncelleştirme|
|Kullanıcı Yönetimi|AccessReview_Review|
|Kullanıcı Yönetimi|AccessReview_Update|
|Kullanıcı Yönetimi|ActivationAborted|
|Kullanıcı Yönetimi|ActivationApproved|
|Kullanıcı Yönetimi|ActivationCanceled|
|Kullanıcı Yönetimi|ActivationRequested|
|Kullanıcı Yönetimi|Eklendi|
|Kullanıcı Yönetimi|Ata|


## <a name="identity-protection"></a>Kimlik koruması

|Denetim Kategorisi|Etkinlik|
|---|---|
|Dizin Yönetimi|Yükselt|
|Dizin Yönetimi|Kaldırıldı|
|Dizin Yönetimi|Rol Ayarı değişiklikleri|
|Diğer|ScanAlertsNow|
|Diğer|Kaydol|
|Diğer|Yetkiyi Kaldır|
|Diğer|UpdateAlertSettings|
|Diğer|UpdateCurrentState|
|İlke Yönetimi|Erişim gözden geçirmesi sona erdi|
|İlke Yönetimi|İstek onayına onaylayan ekleme|
|İlke Yönetimi|Erişim gözden geçirmesine gözden geçiren ekleme|
|Kullanıcı Yönetimi|Erişim gözden geçirmesi uygulama|
|Kullanıcı Yönetimi|Erişim gözden geçirmesi oluşturma|


## <a name="invited-users"></a>Davetli kullanıcılar

|Denetim Kategorisi|Etkinlik|
|---|---|
|Diğer|İstek onayı oluşturma|
|Diğer|Erişim gözden geçirmesini silme|
|Kullanıcı Yönetimi|Erişim gözden geçirmesinden gözden geçireni kaldırma|
|Kullanıcı Yönetimi|Gözden geçirme sonucunu uygulamayı isteme|
|Kullanıcı Yönetimi|Gözden Geçirmeyi Durdurmayı İsteme|
|Kullanıcı Yönetimi|Uygulama atamasını gözden geçirme|
|Kullanıcı Yönetimi|Grup üyeliğini gözden geçirme|
|Kullanıcı Yönetimi|Rbac Rolü üyeliğini gözden geçirme|


## <a name="microsoft-identity-manager-mim"></a>Microsoft Identity Manager (MIM)

|Denetim Kategorisi|Etkinlik|
|---|---|
|Grup Yönetimi|İstek onayı isteğini gözden geçirme|
|Grup Yönetimi|Erişim Gözden Geçirmesini Güncelleştirme|
|Grup Yönetimi|Erişim gözden geçirmesi posta bildirimi ayarlarını güncelleştirme|
|Grup Yönetimi|Erişim gözden geçirmesi yineleme sayısı ayarını güncelleştirme|
|Grup Yönetimi|Erişim gözden geçirmesi yineleme gün sayısını güncelleştirme|
|Kullanıcı Yönetimi|Erişim gözden geçirmesi yineleme bitiş türü ayarını güncelleştirme|
|Kullanıcı Yönetimi|Erişim gözden geçirmesi yineleme türü ayarını güncelleştirme|



## <a name="privileged-identity-management"></a>Privileged Identity Management

|Denetim Kategorisi|Etkinlik|
|---|---|
|Rol Yönetimi|Erişim gözden geçirmesi anımsatıcı ayarlarını güncelleştirme|
|Rol Yönetimi|İstek onayını güncelleştirme|
|Rol Yönetimi|Kullanıcıya uygulama rolü atama izni ekleme|
|Rol Yönetimi|Kullanıcı ekle|
|Rol Yönetimi|Kullanıcılar için güçlü kimlik doğrulaması telefon uygulaması ayrıntılarını ekleme|
|Rol Yönetimi|Kullanıcı lisansını değiştirme|
|Rol Yönetimi|Kullanıcı parolasını değiştirme|
|Rol Yönetimi|Federasyon kullanıcısını yönetilen kullanıcıya dönüştürme|
|Rol Yönetimi|Kullanıcı için uygulama parolası oluşturma|
|Rol Yönetimi|Kullanıcı için uygulama parolasını silme|
|Rol Yönetimi|Kullanıcıyı silme|
|Rol Yönetimi|Hesabı devre dışı bırak|
|Rol Yönetimi|Güçlü Kimlik Doğrulamasını Etkinleştirme|
|Rol Yönetimi|Kullanıcıyı Kalıcı Olarak Silme|
|Rol Yönetimi|Kullanıcıdan uygulama rolü atamasını kaldırma|
|Rol Yönetimi|Kullanıcılar için güçlü kimlik doğrulaması telefon uygulaması ayrıntılarını kaldırma|



## <a name="self-service-group-management"></a>Self servis grup yönetimi

|Denetim Kategorisi|Etkinlik|
|---|---|
|Grup Yönetimi|Kullanıcı parolasını sıfırlama|
|Grup Yönetimi|Kullanıcıyı geri yükleme|
|Grup Yönetimi|Kullanıcı parolasını değiştirmeye zorlamayı ayarlama|
|Grup Yönetimi|Kullanıcı yöneticisini ayarlama|
|Grup Yönetimi|Kullanıcılar için kimlik doğrulaması belirteci meta verilerini etkin olarak ayarlama|
|Grup Yönetimi|StsRefreshTokenValidFrom Zaman Damgasını Güncelleştirme|
|Grup Yönetimi|Dış gizli anahtarları güncelleştirme|
|Grup Yönetimi|Kullanıcıyı güncelleştirme|
|Grup Yönetimi|Yönetici geçici bir parola oluşturur|


## <a name="self-service-password-management"></a>Self servis parola yönetimi

|Denetim Kategorisi|Etkinlik|
|---|---|
|Dizin Yönetimi|Yöneticiler, kullanıcının parolasını sıfırlamasını zorunlu tutar|
|Dizin Yönetimi|Uygulamaya dış kullanıcı atama|
|Kullanıcı Yönetimi|E-posta gönderilmedi, kullanıcının aboneliği kaldırıldı|
|Kullanıcı Yönetimi|Dış kullanıcıyı davet etme|
|Kullanıcı Yönetimi|Dış kullanıcı davetini kullanma|
|Kullanıcı Yönetimi|Virüslü kiracı oluşturma|
|Kullanıcı Yönetimi|Virüslü kullanıcı oluşturma|
|Kullanıcı Yönetimi|Kullanıcı Parolası Kaydı|
|Kullanıcı Yönetimi|Kullanıcı Parolası Sıfırlama|
|Kullanıcı Yönetimi|Self servis parola sıfırlaması engellendi|


## <a name="terms-of-use"></a>Kullanım koşulları

|Denetim Kategorisi|Etkinlik|
|---|---|
|İlke Yönetimi|Parolayı değiştirme (self servis)|
|İlke Yönetimi|Parola sıfırlama (yönetici tarafından)|
|İlke Yönetimi|Parola sıfırlama (self servis)|
|İlke Yönetimi|Self servis parola sıfırlama akış etkinliği ilerleme durumu|
|İlke Yönetimi|Self servis parola sıfırlama akış etkinliği ilerleme durumu|
|İlke Yönetimi|Kullanıcı hesabının kilidini açma (self servis)|
|İlke Yönetimi|Self servis parola sıfırlama için kaydolan kullanıcı|




## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki genel bakışlar için:

- Raporlama, bkz. [Azure Active Directory raporlama](active-directory-reporting-azure-portal.md).

- Denetim etkinliği raporları, bkz. [Azure Active Directory portalında etkinlik raporlarını denetleme](active-directory-reporting-activity-audit-logs.md). 

