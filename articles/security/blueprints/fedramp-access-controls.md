---
title: "FedRAMP Azure şeması Otomasyonu - erişim denetimi"
description: "Web uygulamaları için FedRAMP - erişim denetimi"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: f7e6cd8f-b2df-4db6-8332-de97d86c5281
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 4f2445f1166ae24afb1c6b2a81cf02176593fdaa
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
> [!NOTE]
> Bu denetimler NIST ve ABD tarafından tanımlanır Ticaret Bakanlığı NIST özel yayını 800-53 düzeltme 4 bir parçası olarak. NIST 800 53 düzeltme 4 yordamları ve yönergeler her denetim için test etme hakkında bilgi için lütfen bakın.
    
    

# <a name="access-control-ac"></a>Erişim denetimi (AC)

## <a name="nist-800-53-control-ac-1"></a>NIST 800 53 denetim AC-1

#### <a name="access-control-policy-and-procedures"></a>Erişim denetimi ilkesini ve yordamları

**AC-1** kuruluş geliştirir, belgeler ve için disseminates [atama: kuruluş tarafından tanımlanan personel ya da roller] adresleri amacı, kapsam, roller, sorumlulukları, yönetim taahhüt, koordinasyonu bir erişim denetimi İlkesi Kurumsal varlıklar ve uyumluluk arasında; ve erişim denetimi ilkesini ve ilişkili erişim denetimleri uyarlamasını kolaylaştırmak için yordamlar; gözden geçirir ve geçerli erişim denetim ilkesi güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı]; ve erişim denetim yordamları [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini ve yordamlar bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2a"></a>NIST 800 53 denetim AC-2.a

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.a** kuruluş tanımlar ve kuruluş görevler/iş işlevleri desteklemek için bilgi sistem hesapları aşağıdaki türlerini seçer: [atama: kuruluş tarafından tanımlanan bilgileri sistem hesap türleri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması kullanır ve aşağıdaki sistem hesabı türleri uygulayan: (çözümü dağıtmak ve Azure kaynaklarına erişimi yönetmek için kullanılan) Azure Active Directory Kullanıcıları, Windows işletim sistemi kullanıcıları (Active Directory tarafından yönetilen), SQL Server hizmet hesabı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2b"></a>NIST 800 53 denetim AC-2.b

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.b** kuruluş bilgileri sistem hesapları için hesap yöneticileri atar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri AC-02.a tanımlanan hesaplarını yöneticilerini atamak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2c"></a>NIST 800 53 denetim AC-2.c

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.c** grup ve rol üyeliğini koşullarını organizasyon oluşturur.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından denetlenen hesap türleri için (AC 02.a bakın) rol ve Grup Üyelik ölçütleri kurmak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2d"></a>NIST 800 53 denetim AC-2.d

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.d** bilgi sistemi, Grup ve rol üyeliğini ve erişim yetkili kullanıcıların kuruluş belirtir yetkilerini (yani, ayrıcalıklar) ve her hesap için (gerektiği gibi) diğer öznitelikleri.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bir yerleşik kuruluş düzeyinde hesabı yetkilendirme işlemine bağlıdır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2e"></a>NIST 800 53 denetim AC-2.e

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.e** Onaylar tarafından kuruluşunuza [atama: kuruluş tarafından tanımlanan personel ya da roller] bilgileri Sistem hesaplarını oluşturmak istekleri için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bir yerleşik kuruluş düzeyinde hesabı yetkilendirme işlemine bağlıdır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2f"></a>NIST 800 53 denetim AC-2.f

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.f** kuruluş oluşturur, sağlar, değiştirir, devre dışı bırakır ve ile uyumlu olarak bilgi sistem hesapları kaldırır [atama: kuruluş tarafından tanımlanan yordamları veya koşulları].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bir yerleşik kuruluş düzeyinde hesabı yönetim işlemine bağlıdır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2g"></a>NIST 800 53 denetim AC-2.g

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.g** kuruluş bilgileri sistem hesapları kullanımını izler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması OMS güvenlik ve denetim çözümün kimlik ve erişim Pano uygular. Bu panoyu bilgi sistem hesapları kullanımını izlemek hesap yöneticileri sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2h"></a>NIST 800 53 denetim AC-2.h

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.h** sonlandırıldı ya da aktarılan; kullanıcı hesapları artık gerekli; ne ve ne zaman kuruluş hesap yöneticileri uyarır bilmeniz gerek değişiklikleri veya tek tek bilgi sistem kullanımı.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetim yordamları bir hesap artık gerekli olmadığında uygun Hesap Yöneticisi'ni bildirmek için bir işlem oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2i"></a>NIST 800 53 denetim AC-2.i

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.i** kuruluş geçerli erişim yetkilendirme; hedeflenen sistem kullanımı; ve kuruluş veya ilişkili görevler/iş işlevleri gerektirdiği gibi diğer öznitelikleri göre bilgi sisteme erişim yetkisi verir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetim yordamları erişim Yetkilendirme işlemi oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2j"></a>NIST 800 53 denetim AC-2.j

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.j** kuruluş hesabı Yönetimi gereksinimleri ile uyumluluk için hesapları incelemeleri [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri hesapları tüm kuruluş gereksinimleriyle uyumlu olup olmadıklarını belirlemek için gerekli sıklığı gözden geçirme müşteri tarafından denetlenen hesapları sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-2k"></a>NIST 800 53 denetim AC-2.k

#### <a name="account-management"></a>Hesap Yönetimi

**AC 2.k** kuruluşun kişiler gruptan kaldırıldığında paylaşılan grup hesabının kimlik bilgilerini (dağıtıldıysa) vermeye yönelik bir proses oluşturur.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetim yordamları grup hesabının kimlik bilgilerini yönetmek için bir işlem oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-1"></a>NIST 800 53 denetim AC-2 (1)

#### <a name="account-management--automated-system-account-management"></a>Hesap Yönetimi | Otomatik Sistem hesabı Yönetim

**AC-2 (1)** kuruluş bilgilerini system hesaplarının yönetimini desteklemek için otomatik mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması OMS güvenlik ve denetim çözümün kimlik ve erişim Pano uygular. Bu panoyu bilgi sistem hesapları kullanımını izlemek hesap yöneticileri etkinleştirin. OMS alışılmadık etkinliği şüpheli veya diğer önceden tanımlanmış olaylarından uyarıları göndermek üzere yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-2"></a>NIST 800 53 denetim AC-2 (2)

#### <a name="account-management--removal-of-temporary--emergency-accounts"></a>Hesap Yönetimi | Geçici / Acil Durum hesapları kaldırılması

**AC-2 (2)** bilgi sistemi otomatik olarak [seçim: kaldırır; devre dışı bırakır] sonra geçici ve Acil Durum hesapları [atama: süre her hesap türü için kuruluşunuz tarafından tanımlanan].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması, geçici veya Acil Durum hesapları dağıtmayı değil. El ile devre dışı bırakılırsa, dağıtılan etki alanı denetleyicisi otomatik olarak tüm etkin olmayan hesaplar 35 gün sonra devre dışı bırakır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-3"></a>NIST 800 53 denetim AC-2 (3)

#### <a name="account-management--disable-inactive-accounts"></a>Hesap Yönetimi | Etkin olmayan hesaplar devre dışı bırak

**AC-2 (3)** bilgileri sistem otomatik olarak sonra etkin olmayan hesaplar devre dışı bırakır [atama: kuruluş tanımlı süre].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan etki alanı denetleyicisi, işlem yapılmadan geçen süre 35 gün sonra tüm kullanıcı hesapları devre dışı bırakmak için yapılandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-4"></a>NIST 800 53 denetim AC-2 (4)

#### <a name="account-management--automated-audit-actions"></a>Hesap Yönetimi | Otomatik denetim eylemleri

**AC-2 (4)** bilgi sistemi otomatik olarak hesabı oluşturulması, değiştirilmesi, etkinleştirme, devre dışı bırakma ve kaldırma eylemleri denetler ve bildirir ve [atama: kuruluş tarafından tanımlanan personel ya da roller].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Aşağıdaki sistem hesap türleri bu Azure şeması uygulayan: Azure Active Directory kullanıcılarını, Windows işletim sistemi kullanıcılar, SQL Server hizmet hesabı. Azure Active Directory hesabı yönetim eylemleri bir olayı Azure etkinlik günlüğü oluşturur; İşletim sistemi düzeyinde bir hesap yönetimi eylemleri sistem günlüğüne bir olay oluşturur. Bu günlükler günlük analizi tarafından toplanan ve OMS deposunda saklanır. OMS önceden tanımlanmış olaylar meydana geldiğinde uyarıları göndermek üzere yapılandırılabilir.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-5"></a>NIST 800 53 denetim AC-2 (5)

#### <a name="account-management--inactivity-logout"></a>Hesap Yönetimi | Etkin olmama oturum kapatma

**AC-2 (5)** kuruluşun kullanıcılar ne zaman oturum gerektirir [atama: zaman aralığı kuruluşunuz tarafından tanımlanan beklenen etkinlik veya ne zaman oturum kapatma açıklaması].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini bir süre (veya diğer etkenlere) etkin olmayan durumda düşündüğünüz yükleyen kullanıcılar oturum kapatma bir ilke oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-7a"></a>NIST 800 53 AC-2 (7) bir denetim

#### <a name="account-management--role-based-schemes"></a>Hesap Yönetimi | Rol tabanlı düzenleri

**AC-2 (7) bir** kuruluş kurar ve ayrıcalıklı kullanıcı hesaplarına izin verilen bilgi sistem erişimi ve ayrıcalıkları rollere düzenler bir rol tabanlı erişim düzeni uygun olarak yönetir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Aşağıdaki sistem hesap türleri bu Azure şeması uygulayan: Azure Active Directory kullanıcılarını, Windows işletim sistemi kullanıcılar, SQL Server hizmet hesabı. Azure Active Directory hesap ayrıcalığı kullanıcıları rollere atama tarafından rol tabanlı erişim denetimini kullanarak uygulanır; Active Directory hesap ayrıcalığı kullanıcıları güvenlik gruplarına atayarak rol tabanlı erişim denetimini kullanarak uygulanır. Bu rol tabanlı düzenleri iş gereksinimlerini karşılamak üzere müşteri tarafından genişletilebilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-7b"></a>NIST 800 53 AC-2 (7) .b denetleme

#### <a name="account-management--role-based-schemes"></a>Hesap Yönetimi | Rol tabanlı düzenleri

**AC-2 (7) .b** kuruluş ayrıcalıklı rol atamaları izler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması OMS güvenlik ve denetim çözümün kimlik ve erişim Pano uygular. Bu panoyu bilgi sistem hesapları kullanımını izlemek hesap yöneticileri sağlar. Bu çözüm, ayrıcalıklı rol atamaları bildirmek için sorgulanabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-7c"></a>NIST 800 53 AC-2 (7) .c denetleme

#### <a name="account-management--role-based-schemes"></a>Hesap Yönetimi | Rol tabanlı düzenleri

**AC-2 (7) .c** kuruluş alır [atama: kuruluş tarafından tanımlanan Eylemler] ne zaman ayrıcalıklı rol atamaları artık uygun.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, ayrıcalıklı rol atamaları artık uygun olduğunda, eylem müşteri tarafından denetlenen hesaplarında almak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-9"></a>NIST 800 53 denetim AC-2 (9)

#### <a name="account-management--restrictions-on-use-of-shared--group-accounts"></a>Hesap Yönetimi | Paylaşılan / grup hesaplarını kullanma kısıtlamaları

**AC-2 (9)** kuruluş yalnızca karşılayan paylaşılan grup hesapları kullanımına izin verir [atama: paylaşılan grup hesapları oluşturma için kuruluş tanımlanan koşullar].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Hiç paylaşılan grubu hesabı bu Azure şeması tarafından dağıtılan kaynak üzerinde etkindir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-10"></a>NIST 800 53 denetim AC-2 (10)

#### <a name="account-management--shared--group-account-credential-termination"></a>Hesap Yönetimi | Paylaşılan / Grup hesap kimlik bilgisi sonlandırma

**AC-2 (10)** üyeleri grubu ayrıldığında bilgi sistemi paylaşılan grup hesabının kimlik bilgilerini sonlandırır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Hiç paylaşılan grubu hesabı bu Azure şeması tarafından dağıtılan kaynak üzerinde etkindir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-11"></a>NIST 800 53 denetim AC-2 (11)

#### <a name="account-management--usage-conditions"></a>Hesap Yönetimi | Kullanım koşulları

**AC-2 (11)** bilgi sistemi zorlar [atama: kuruluş tarafından tanımlanan koşullar ve/veya kullanım koşulları] için [atama: kuruluş tarafından tanımlanan bilgileri sistem hesapları].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi Active Directory'de oluşturulan ve gün saat kısıtlamaları veya diğer hesap kullanım koşulları uygulamak için yapılandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-12a"></a>NIST 800 53 AC-2 (12) bir denetim

#### <a name="account-management--account-monitoring--atypical-usage"></a>Hesap Yönetimi | İzleme / alışılmadık kullanım hesabı

**AC-2 (12) bir** kuruluş için bilgi sistem hesapları izler [atama: alışılmadık kullanımı kuruluş tanımlı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması OMS güvenlik ve denetim çözümün kimlik ve erişim Pano uygular. Bu panoya erişim denemesi dağıtılan kaynaklara karşı izlemek hesap yöneticileri sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-12b"></a>NIST 800 53 AC-2 (12) .b denetleme

#### <a name="account-management--account-monitoring--atypical-usage"></a>Hesap Yönetimi | İzleme / alışılmadık kullanım hesabı

**AC-2 (12) .b** kuruluş bilgilerini system hesaplarının alışılmadık kullanım raporları [atama: kuruluş tarafından tanımlanan personel ya da roller].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması OMS güvenlik ve denetim çözümün kimlik ve erişim Pano uygular. Bu panoya erişim denemesi dağıtılan kaynaklara karşı izlemek hesap yöneticileri etkinleştirin. Bu çözüm, alışılmadık etkinliği şüpheli veya diğer önceden tanımlanmış olaylarından uyarıları göndermek üzere yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-2-13"></a>NIST 800 53 denetim AC-2 (13)

#### <a name="account-management--disable-accounts-for-high-risk-individuals"></a>Hesap Yönetimi | Yüksek riskli kişiler için hesapları devre dışı bırak

**AC-2 (13)** kuruluş içindeki önemli bir riski taşıyor kullanıcıların hesaplarını devre dışı bırakır [atama: kuruluş tanımlı süre] riskin bulma.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini ve yordamları hesapları kuruluş için önemli bir riski taşıyor kullanıcılar için devre dışı bırakmak için koşullar oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-3"></a>NIST 800 53 denetim AC-3

#### <a name="access-enforcement"></a>Erişim zorlama

**AC-3** bilgileri sistem bilgileri ve sistem kaynakları geçerli erişim denetim ilkelerini uygun şekilde mantıksal erişimi için onaylanan yetkilerini zorlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması roller, kullanıcılar güvenlik gruplarına atayarak Active Directory Kullanıcıları atayarak Azure Active Directory tarafından zorlanan rol tabanlı erişim denetimini kullanarak mantıksal erişimi yetkilerini zorunlu kılan ve Windows işletim sistemi düzeyinde denetler. Azure Active Directory rolleri kullanıcılara atanan veya grupları mantıksal kaynak, Grup veya abonelik düzeyinde Azure içindeki kaynaklara erişimi denetler. Active Directory güvenlik grupları, işletim sistemi düzeyinde kaynakları ve işlevleri mantıksal erişimi denetler. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-4"></a>NIST 800 53 denetim AC-4

#### <a name="information-flow-enforcement"></a>Bilgi akışını zorlama

**AC-4** bilgileri sistem sistemi içinde ve temel birbirine sistemleri arasındaki bilgi akışını denetlemek için onaylanan yetkilerini zorlar [atama: kuruluş tarafından tanımlanan bilgileri akış denetimi ilkeleri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması ağ güvenlik grupları, kaynaklar dağıtılır, alt ağlar için uygulama ağ geçidi, uygulanan kullanımı ile bilgi akışını kısıtlamaları zorunlu kılar ve yük dengeleyici. Ağ güvenlik grupları, onaylanan kurallara göre kaynakları arasındaki bilgi akışını denetlenir emin olun. Uygulama ağ geçidi ve yük dengeleyici, belirli kaynaklara trafiği yönlendirme onaylanan rollerine dinamik olarak bağlı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-4-8"></a>NIST 800 53 denetim AC-4 (8)

#### <a name="information-flow-enforcement--security-policy-filters"></a>Bilgi akışını zorlama | Güvenlik İlkesi filtreleri

**AC-4 (8)** bilgileri akış denetimi kullanarak bilgi sistemi zorlar [atama: kuruluş tanımlanan güvenlik ilke filtrelerini] temel akış denetimi kararlarını olarak [atama: kuruluş tarafından tanımlanan bilgileri akışları].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde bilgi akış denetimi zorlama için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-4-21"></a>NIST 800 53 denetim AC-4 (21)

#### <a name="information-flow-enforcement--physical--logical-separation-of-information-flows"></a>Bilgi akışını zorlama | Mantıksal / fiziksel ayrımı bilgi akışları

**AC-4 (21)** mantıksal veya fiziksel olarak kullanarak bilgi akışları bilgi sistemi ayıran [atama: kuruluş tarafından tanımlanan mekanizmaları ve/veya teknikleri] gerçekleştirmek için [atama: kuruluş tarafından tanımlanan gerekli tür tarafından ayırmaları Bilgi].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde bilgi akışları ayırmak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-5a"></a>NIST 800 53 denetim AC-5.a

#### <a name="separation-of-duties"></a>Görev ayrımını

**AC 5.a** kuruluş ayıran [atama: kuruluş tarafından tanımlanan görevlerini kişilerin].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri müşteri tarafından denetlenen hesaplarında görev ayrımını için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-5b"></a>NIST 800 53 denetim AC-5.b

#### <a name="separation-of-duties"></a>Görev ayrımını

**AC 5.b** kuruluş, görevlerin ayrılmasını kişilerin belgeleri.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri müşteri tarafından denetlenen hesaplarında görev ayrımını belgelemesinden sorumlu değildir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-5c"></a>NIST 800 53 denetim AC-5.c

#### <a name="separation-of-duties"></a>Görev ayrımını

**AC 5.c** kuruluş, görevlerin ayrılmasını desteklemek için bilgi sistem erişim yetkilerini tanımlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması görevlerini kuruluş gereksinimlerine göre ayırmak için yapılandırılan rol tabanlı erişim denetimi uygular. Azure Active Directory hesap ayrıcalığı kullanıcıları rollere atama tarafından rol tabanlı erişim denetimini kullanarak uygulanır; Active Directory hesap ayrıcalığı kullanıcıları güvenlik gruplarına atayarak rol tabanlı erişim denetimini kullanarak uygulanır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-6"></a>NIST 800 53 denetim AC-6

#### <a name="least-privilege"></a>En az ayrıcalık

**AC-6** kuruluş kuruluş görevler uygun olarak atanan görevleri gerçekleştirmek gerekli olan yetkili erişimi kullanıcılar (veya kullanıcılar adına hareket işlemleri) için izin vererek, en az ayrıcalık ilkesini uygular ve İş işlevleri.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması kullanıcıların yalnızca açıkça atanan ayrıcalıkları sınırlamak için rol tabanlı erişim denetimi uygular. Azure Active Directory hesap ayrıcalığı kullanıcıları rollere atama tarafından rol tabanlı erişim denetimini kullanarak uygulanır; Active Directory hesap ayrıcalığı kullanıcıları güvenlik gruplarına atayarak rol tabanlı erişim denetimini kullanarak uygulanır.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-6-1"></a>NIST 800 53 denetim AC-6 (1)

#### <a name="least-privilege--authorize-access-to-security-functions"></a>En az ayrıcalık | Güvenlik işlevleri erişim yetkisi

**AC-6 (1)** kuruluş açıkça erişimini yetkilendirir [atama: (donanım, yazılım ve bellenim dağıtılan) güvenlik kuruluşunuz tarafından tanımlanan işlevler ve güvenlik ilgili bilgileri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetim yordamları güvenlik işlevlere erişim içeren erişim Yetkilendirme işlemi oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-6-2"></a>NIST 800 53 denetim AC-6 (2)

#### <a name="least-privilege--non-privileged-access-for-nonsecurity-functions"></a>En az ayrıcalık | Güvenlikle ilgili olmayan işlevler için ayrıcalıklı olmayan erişim

**AC-6 (2)** kuruluş gerektiren bilgi sistem hesapları veya rolleri erişimi olan kullanıcıları [atama: kuruluş tanımlanan güvenlik işlevleri veya güvenlik ilgili bilgileri], ayrıcalıklı olmayan hesaplar veya rolleri kullanın olduğunda güvenlikle ilgili olmayan işlevleri erişme.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini ayrıcalıklı olmayan hesaplar güvenlikle ilgili olmayan işlevleri erişirken kullanılacak kullanıcı gerektirebilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-6-3"></a>NIST 800 53 denetim AC-6 (3)

#### <a name="least-privilege--network-access-to-privileged-commands"></a>En az ayrıcalık | Ağ erişimi ayrıcalıklı komutları

**AC-6 (3)** kuruluş ağ erişimini yetkilendirir [atama: kuruluş tarafından tanımlanan ayrıcalıklı komutları] yalnızca [atama: kuruluş tanımlı işlem ihtiyaçlarını çekici] ve bu tür erişim için stratejinin belgeleri bilgi sistemi güvenlik planı.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini bir ağ üzerinden erişilebilir ayrıcalıklı komutları tanımlayabilir. Not: Müşteriler fiziksel Azure altyapısına erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-6-5"></a>NIST 800 53 denetim AC-6 (5)

#### <a name="least-privilege--privileged-accounts"></a>En az ayrıcalık | Ayrıcalıklı hesaplar

**AC-6 (5)** kuruluş için bilgi sistemde ayrıcalıklı hesapların kısıtlayan [atama: kuruluş tarafından tanımlanan personel ya da roller].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini ayrıcalıklı hesapların kullanımı kısıtlamalarını tanımlayabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-6-7a"></a>NIST 800 53 AC-6 (7) bir denetim

#### <a name="least-privilege--review-of-user-privileges"></a>En az ayrıcalık | Kullanıcı ayrıcalıkları, gözden geçirme

**AC-6 (7) bir** kuruluş incelemeler [atama: kuruluş tarafından tanımlanan sıklığı] atanan ayrıcalıkları [atama: kuruluş tanımlı roller veya kullanıcı sınıfları] böyle ayrıcalıklarına gerek doğrulamak için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, kullanıcı ayrıcalıkları müşteri tarafından denetlenen planını gözden geçirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-6-7b"></a>NIST 800 53 AC-6 (7) .b denetleme

#### <a name="least-privilege--review-of-user-privileges"></a>En az ayrıcalık | Kullanıcı ayrıcalıkları, gözden geçirme

**AC-6 (7) .b** kuruluş yeniden atar veya ayrıcalıkları, gerekirse, doğru kuruluş görev iş gereksinimleriniz yansıtacak şekilde kaldırır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yeniden atama veya uygun olduğunda müşteri tarafından denetlenen hesapları için ayrıcalıklarının kaldırılması sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-6-8"></a>NIST 800 53 denetim AC-6 (8)

#### <a name="least-privilege--privilege-levels-for-code-execution"></a>En az ayrıcalık | Kod yürütmeyi ayrıcalık düzeylerini

**AC-6 (8)** bilgi sistemi engeller [atama: kuruluş tanımlı yazılım] yazılım yürütme kullanıcıları daha yüksek ayrıcalık düzeyde yürütülmesini.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması kullanıcıların yalnızca açıkça atanan ayrıcalıkları sınırlamak için rol tabanlı erişim denetimi uygular. Sanal makine işletim sistemi düzeyinde korumaları yazılım yürütme kullanıcıların daha yüksek bir ayrıcalık düzeyinde çalıştırmak yazılım izin vermez. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-6-9"></a>NIST 800 53 denetim AC-6 (9)

#### <a name="least-privilege--auditing-use-of-privileged-functions"></a>En az ayrıcalık | Ayrıcalıklı işlevler kullanımını denetleme

**AC-6 (9)** bilgi sistemi ayrıcalıklı işlevleri yürütmesini denetler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması günlük analizi hizmeti OMS uygular. Dağıtılan VM'ler ve Azure tanılama depolama hesaplarıdır ayrıcalıklı işlevleri yürütmesini denetlenir günlük analizi sağlamak için bağlı kaynaklar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-6-10"></a>NIST 800 53 denetim AC-6 (10)

#### <a name="least-privilege--prohibit-non-privileged-users-from-executing-privileged-functions"></a>En az ayrıcalık | Ayrıcalıklı olmayan kullanıcılar ayrıcalıklı işlevler yürütmek engelle

**AC-6 (10)** devre dışı, atlamak, dahil etmek için ayrıcalıklı işlevler yürütülmesini ayrıcalıklı olmayan kullanıcılar bilgi sistemi engeller ya da uygulanan güvenlik önlemlerinin/önlemler değiştirme.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması kullanıcıların yalnızca açıkça atanan ayrıcalıkları sınırlamak için rol tabanlı erişim denetimi uygular.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-7a"></a>NIST 800 53 denetim AC-7.a

#### <a name="unsuccessful-logon-attempts"></a>Başarısız oturum açma denemeleri

**AC 7.a** bilgi sistemi sınırı zorlar [atama: kuruluş tanımlı sayı] ardışık geçersiz oturum açma girişimi sırasında bir kullanıcı tarafından bir [atama: kuruluş tanımlı süre].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure portal sınırları ardışık geçersiz oturum açma kullanıcılar tarafından çalışır. Bir Grup İlkesi, bu Azure şeması tarafından dağıtılan tüm sanal makineleri için işletim sistemi düzeyinde uygulanır. İlke ardışık geçersiz oturum açma girişimlerinin en çok üç 15 dakikalık süre içinde kullanıcılar tarafından sınırlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-7b"></a>NIST 800 53 denetim AC-7.b

#### <a name="unsuccessful-logon-attempts"></a>Başarısız oturum açma denemeleri

**AC 7.b** bilgi sistemi otomatik olarak [seçim: hesap/düğümü için kilitler bir [atama: kuruluş tanımlı süre]; bir yönetici tarafından yayımlanan kadar hesap/düğüm kilitler; gecikmeler [göre bir sonraki oturum açma istemini Atama: kuruluş tarafından tanımlanan gecikme algoritması]] zaman başarısız deneme sayısı aşıldı.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Kullanıcılar tarafından ardışık geçersiz oturum açma denemesinden sonra Azure portalı hesapları kilitler. Bir Grup İlkesi, bu Azure şeması tarafından dağıtılan tüm sanal makineleri için işletim sistemi düzeyinde uygulanır. İlke hesapları üç saat sonra kullanıcılar tarafından üç ardışık geçersiz oturum açma girişimleri için kilitler. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-7-2"></a>NIST 800 53 denetim AC-7 (2)

#### <a name="unsuccessful-logon-attempts--purge--wipe-mobile-device"></a>Başarısız oturum açma girişimlerinin | Temizleme / mobil aygıtı

**AC-7 (2)** bilgi sistemi temizler/temizleme bilgilerinden [atama: mobil cihazları kuruluşun tanımlı] göre [atama: kuruluş tanımlı gereksinimleri/teknikleri temizleme/silme] sonra [atama: Kuruluş tanımlı sayı] aygıt ardışık, başarısız oturum açma girişimleri.

**Sorumlulukları:**`Not Applicable`

|||
|---|---|
| **Müşteri** | Mobil cihazlar Azure üzerinde dağıtılan sistemler kapsamında değildir. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure mobil cihazlar Azure sınırları içinde izin vermiyor. Bu nedenle, bu denetim Microsoft Azure için geçerli değildir. |


 ## <a name="nist-800-53-control-ac-8a"></a>NIST 800 53 denetim AC-8.a

#### <a name="system-use-notification"></a>Sistem kullanımı bildirimi

**AC 8.a** kullanıcılara bilgi sistemi görüntülediği [atama: kuruluş tarafından tanımlanan sistem bildirim iletisi veya başlık kullanın] güvenlik ve gizlilik bildirimlerini geçerli federal ile tutarlı sağlayan sistem erişim vermeden önce yasaları, Executive siparişler, yönergeleri, ilkeleri, düzenlemeler, standartları ve Kılavuzu ve bir ABD erişen kullanıcı durumları Kamu bilgi sistemi; bilgi sistem kullanımı, kayıtlı ve denetim tabi izlenmesi; yetkisiz bilgi sistemi yasaklanmış ve suçlunun tabidir ve hukuki kullanımıdır; ve izleme ve kaydetme onay bilgi sistemi kullanımını gösterir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi oturum açma öncesinde kullanıcılara görüntülenen bir sistem kullanımı bildirimi uygular. Not: Örnek sistem kullanımı bildirim Azure şeması uygular. Müşteri kuruluş ve/veya yasal gövde gereksinimlerini karşılamak için bu metni düzenlemeniz gerekir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-8b"></a>NIST 800 53 denetim AC-8.b

#### <a name="system-use-notification"></a>Sistem kullanımı bildirimi

**AC 8.b** kullanıcılar kullanım koşulları kabul etmek ve oturum açın veya daha fazla bilgi sistemi erişmek için açık eylemleri kadar bilgi sistemi bildirim iletisi veya ekran başlığında korur.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi oturum açma öncesinde kullanıcılara görüntülenen bir sistem kullanımı bildirimi uygular. Kullanıcı oturum açma için bildirim kabul gerekir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-8c"></a>NIST 800 53 denetim AC-8.c

#### <a name="system-use-notification"></a>Sistem kullanımı bildirimi

**AC 8.c** bilgileri sistem genel olarak erişilebilir sistemleri için sistem kullanım bilgilerini görüntüler [atama: kuruluş tanımlanan koşullar], daha fazla erişim vermeden önce başvuruları varsa, izleme için görüntüler kaydı veya denetleme genellikle bu etkinlikleri engelle bu sistemlere gizlilik konaklamaları ile tutarlı; ve yetkili kullandığı sisteminin açıklamasını içerir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bir sistem kullanımı bildirimi tüm genel olarak erişilebilir müşteri tarafından dağıtılan kaynaklara görüntülemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-10"></a>NIST 800 53 denetim AC-10

#### <a name="concurrent-session-control"></a>Eşzamanlı oturum denetimi

**AC 10** bilgi sistemi her eşzamanlı oturum sayısını sınırlar [atama: kuruluş tanımlı bir hesabı ve/veya hesap türü] için [atama: kuruluş tanımlı sayı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bir işletim sistemi İlkesi, bu Azure şeması tarafından dağıtılan sanal makineleri için uygulanır. İlke eşzamanlı oturum kısıtlamaları (iki oturumları) uygular. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-11a"></a>NIST 800 53 denetim AC-11.a

#### <a name="session-lock"></a>Oturum kilidi

**AC 11.a** bilgi sistemi, bir oturumun kilidi sonra başlatarak sisteme başka erişimi engeller [atama: kuruluş tanımlı süre] yapılmadıktan veya bir kullanıcı isteği aldıklarında.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi RDP oturumları için bir etkinlik kilidi uygular. Kullanıcılar, kilit el ile başlatabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-11b"></a>NIST 800 53 denetim AC-11.b

#### <a name="session-lock"></a>Oturum kilidi

**AC 11.b** kullanıcı erişimi kurulan tanımlama ve kimlik doğrulama yordamları kullanarak yeniden kurar kadar bilgi sistemi oturum kilidi korur.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi RDP oturumları için bir etkinlik kilidi uygular. Kullanıcılar oturum kilidini açmak için sağlamalarını gerekir.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-11-1"></a>NIST 800 53 denetim AC-11 (1)

#### <a name="session-lock--pattern-hiding-displays"></a>Oturum kilidi | Desen gizleme görüntüler

**AC-11 (1)** bilgi sistemi gizlendiğinden, oturumun kilidi herkes tarafından izlenen bir görüntü ile görüntüleme hakkında bilgi daha önce görünür.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi RDP oturumları için bir etkinlik kilidi uygular. Oturum kilidi önceden görülebilir bilgi gizler. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-12"></a>NIST 800 53 denetim AC-12

#### <a name="session-termination"></a>Oturum sonlandırma

**AC-12** bilgileri sistem bir kullanıcı oturumu sonra otomatik olarak sona erdirmeden [atama: kuruluş tanımlanan koşullar veya tetikleyici olay oturumu bağlantıyı kes gerektiren].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan Windows sanal makineleri için Uzak Masaüstü Oturumu Ana Bilgisayar Yapılandırması kuruluş oturum sonlandırma gereksinimlerini karşılamak için yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-12-1a"></a>NIST 800 53 denetim AC-12 (1) bir

#### <a name="session-termination--user-initiated-logouts--message-displays"></a>Oturum sonlandırma | Kullanıcı tarafından başlatılan Logouts / iletisi görüntüler

**AC-12 (1) bir** bilgi sistemi bir oturum kapatma özelliği kullanıcı tarafından başlatılan iletişim için kimlik doğrulaması erişmek için kullanılan her oturumları sağlar [atama: kuruluş tarafından tanımlanan bilgileri kaynakları].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure portal ve sanal makine bu Azure şeması tarafından dağıtılan işletim sistemleri, bir oturum kapatma başlatmak kullandığı etkinleştirin. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-12-1b"></a>NIST 800 53 denetim AC-12 (1) .b

#### <a name="session-termination--user-initiated-logouts--message-displays"></a>Oturum sonlandırma | Kullanıcı tarafından başlatılan Logouts / iletisi görüntüler

**AC-12 (1) .b** bilgi sistemi kimliği doğrulanmış iletişimler oturumları güvenilir sonlandırılması belirten kullanıcılara bir açık oturum kapatma iletisi görüntüler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure portal ve sanal makine bu Azure şeması tarafından dağıtılan işletim sistemleri, bir oturum kapatma başlatmak kullandığı etkinleştirin. Oturum kapatma işlemi kullanıcılar oturum sonlandırıldı sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-14a"></a>NIST 800 53 denetim AC-14.a

#### <a name="permitted-actions-without-identification-or-authentication"></a>İzin verilen eylemleri kimliği veya kimlik doğrulaması olmadan

**AC 14.a** kuruluş tanımlayan [atama: kuruluş tarafından tanımlanan kullanıcı eylemlerini] bilgi sistemde kimliği veya kuruluş görevler/iş işlevleri ile tutarlı kimlik doğrulaması olmadan gerçekleştirilebilir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri Kimliği veya (örn., örneğin genel olarak erişilebilir bir web sayfası görüntüleme) kimlik doğrulaması olmadan müşteri tarafından dağıtılan kaynaklar üzerinde gerçekleştirilen eylemler tanımlamaktan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-14b"></a>NIST 800 53 denetim AC-14.b

#### <a name="permitted-actions-without-identification-or-authentication"></a>İzin verilen eylemleri kimliği veya kimlik doğrulaması olmadan

**AC 14.b** kuruluş belgeler ve destekleyici stratejinin güvenlik planında bilgileri sistem için kimlik veya kimlik doğrulaması gerektirmeyen kullanıcı eylemlerini sağlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri Kimliği veya müşteri tarafından dağıtılan kaynaklarda kimlik doğrulaması gerektirmeyen kullanıcı eylemlerini belgelerine sağlamaktan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-17a"></a>NIST 800 53 denetim AC-17.a

#### <a name="remote-access"></a>Uzaktan Erişim

**AC 17.a** kuruluş oluşturur ve kullanım kısıtlamaları, yapılandırma/bağlantı gereksinimleri ve izin verilen uzaktan erişim her tür uygulama kılavuzunu belgeleri.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini uzaktan erişim kullanım kısıtlamaları tanımlayabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-17b"></a>NIST 800 53 denetim AC-17.b

#### <a name="remote-access"></a>Uzaktan Erişim

**AC 17.b** kuruluş bu tür bağlantıları vermeden önce bilgi sisteme uzaktan erişim yetkisi verir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetim yordamları bir uzaktan erişim Yetkilendirme işlemi oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-17-1"></a>NIST 800 53 denetim AC-17 (1)

#### <a name="remote-access--automated-monitoring--control"></a>Uzaktan erişim | İzleme otomatik / denetimi

**AC-17 (1)** bilgi sistemi izler ve Uzaktan erişim yöntemleri denetler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması bir müşteri uygulanan web uygulaması ve bir jumpbox aracılığıyla Uzak Masaüstü bağlantısı üzerinden Azure Portalı aracılığıyla bilgi sisteme uzaktan erişim sağlar. Azure portal ve Uzaktan Yardım oturumları aracılığıyla erişir denetlenir ve OMS izlenebilir. Müşteri web uygulaması için gereken uzaktan erişim denetimleri uygulamalıdır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-17-2"></a>NIST 800 53 denetim AC-17 (2)

#### <a name="remote-access--protection-of-confidentiality--integrity-using-encryption"></a>Uzaktan erişim | Gizlilik Koruması / bütünlüğü şifreleme kullanma

**AC-17 (2)** bilgi sistemi, uzaktan erişim oturumlarının bütünlüğü ve gizliliği korumak için şifreleme mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure Azure portalı, Uzak Masaüstü Bağlantısı ve web uygulama ağ geçidi, şeması tarafından dağıtılan kaynaklara uzaktan erişim güvenli TLS kullanarak. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-17-3"></a>NIST 800 53 denetim AC-17 (3)

#### <a name="remote-access--managed-access-control-points"></a>Uzaktan erişim | Yönetilen erişim denetim noktaları

**AC-17 (3)** aracılığıyla tüm uzaktan erişim bilgileri sistem yolları [atama: kuruluş tanımlı sayı] yönetilen ağ erişim denetim noktaları.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Uzaktan erişim bu Azure şeması tarafından dağıtılan notional web uygulaması için bir uygulama ağ geçidi üzerinden ' dir. Tüm diğer kaynaklara uzaktan erişim jumpbox ' dir. Genel olarak erişilebilen herhangi bir uç vardır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-17-4a"></a>NIST 800 53 AC 17 (4) bir denetim

#### <a name="remote-access--privileged-commands--access"></a>Uzaktan erişim | Ayrıcalıklı komutları / erişim

**AC-17 (4) bir** kuruluş ayrıcalıklı komutlar ve yalnızca için uzaktan erişim aracılığıyla güvenlik ilgili bilgilere erişim yürütülmesi yetkilendirir [atama: kuruluşunuzun ihtiyaçlarını tanımlı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini Uzaktan erişilebilir ve bir stratejinin içeren ayrıcalıklı komutları tanımlayabilir. Not: Müşteriler, Azure altyapı için hiçbir doğrudan ağ erişimi. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-17-4b"></a>NIST 800 53 AC 17 (4) .b denetleme

#### <a name="remote-access--privileged-commands--access"></a>Uzaktan erişim | Ayrıcalıklı komutları / erişim

**AC-17 (4) .b** bilgi sistemi güvenlik planı stratejinin gibi erişim için kuruluş belgeleri.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini Uzaktan erişilebilir ve bir stratejinin içeren ayrıcalıklı komutları tanımlayabilir. Not: Müşteriler, Azure altyapı için hiçbir doğrudan ağ erişimi. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-17-9"></a>NIST 800 53 denetim AC-17 (9)

#### <a name="remote-access--disconnect--disable-access"></a>Uzaktan erişim | Bağlantısını kes / erişimini devre dışı bırakma

**AC-17 (9)** kuruluş süratle çağrı yapın bağlantısını kesmek veya bilgi sistem içinde uzaktan erişimi devre dışı yeteneği sağlar [atama: kuruluş tanımlı süre].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması Azure portalı, bir jumpbox aracılığıyla Uzak Masaüstü Bağlantısı ve bir web uygulaması aracılığıyla bilgi sisteme uzaktan erişim sağlar. Bir Azure Active Directory hesabı devre dışı veya kaldırılmış Azure portal erişim hemen kesilir. Benzer şekilde, bir sanal makine işletim sistemi düzeyinde bir hesap devre dışı veya kaldırılmış jumpbox aracılığıyla uzak masaüstü erişimi hemen kesilir. Müşteriler, uzaktan erişim uygulanmalı web uygulamasının bağlantısını kesin. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-18a"></a>NIST 800 53 denetim AC-18.a

#### <a name="wireless-access"></a>Kablosuz erişim

**AC 18.a** kullanım kısıtlamaları, yapılandırma/bağlantı gereksinimleri ve uygulama kılavuzunu kablosuz erişim için kuruluş oluşturur.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında kablosuz erişimi yoktur. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure kullanım kısıtlamaları, yapılandırma/bağlantı gereksinimleri ve ağ güvenliği açıkça Microsoft Azure ortamında kablosuz kullanımını önleyen standardı, aracılığıyla kablosuz erişim için uygulama kılavuzunu oluşturur. |


 ## <a name="nist-800-53-control-ac-18b"></a>NIST 800 53 denetim AC-18.b

#### <a name="wireless-access"></a>Kablosuz erişim

**AC 18.b** kuruluş bu tür bağlantıları vermeden önce bilgi sisteme kablosuz erişim yetkisi verir.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında kablosuz erişimi yoktur. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure Microsoft Azure veri merkezleri içinde kablosuz erişim izin vermiyor. |


 ### <a name="nist-800-53-control-ac-18-1"></a>NIST 800 53 denetim AC-18 (1)

#### <a name="wireless-access--authentication-and-encryption"></a>Kablosuz erişim | Kimlik doğrulama ve şifreleme

**AC-18 (1)** kablosuz erişim için kimlik doğrulaması kullanarak sistem bilgileri sistem korur [seçimi (bir veya daha fazla): kullanıcılar; aygıtları] ve şifreleme.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında kablosuz erişimi yoktur. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure Microsoft Azure ortamında kablosuz erişimi izin vermiyor. |


 ### <a name="nist-800-53-control-ac-18-3"></a>NIST 800 53 denetim AC-18 (3)

#### <a name="wireless-access--disable-wireless-networking"></a>Kablosuz erişim | Kablosuz ağ devre dışı bırak

**AC-18 (3)** kullanım için tasarlanmamıştır, kuruluş devre dışı bırakır, kablosuz dahili olarak bilgi sistem bileşenleri verme ve dağıtım öncesinde içinde katıştırılmış yetenekleri ağ.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında kablosuz erişimi yoktur. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure Microsoft Azure ortamında kablosuz erişimi izin vermiyor. |


 ### <a name="nist-800-53-control-ac-18-4"></a>NIST 800 53 denetim AC-18 (4)

#### <a name="wireless-access--restrict-configurations-by-users"></a>Kablosuz erişim | Kullanıcılar tarafından yapılandırmaları kısıtla

**AC-18 (4)** kuruluş tanımlar ve açıkça bağımsız olarak kablosuz ağ özellikleri yapılandırmak için izin verilen kullanıcıların yetkilendirir.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında kablosuz erişimi yoktur. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure Microsoft Azure ortamında kablosuz erişimi izin vermiyor. |


 ### <a name="nist-800-53-control-ac-18-5"></a>NIST 800 53 denetim AC-18 (5)

#### <a name="wireless-access--antennas--transmission-power-levels"></a>Kablosuz erişim | Antenler / İletim güç düzeyleri

**AC-18 (5)** kuruluş radyo antenler seçer ve kullanılabilir sinyalleri kuruluş denetlenen sınırları dışında aldığınızı olasılığını azaltmak için iletim güç düzeylerini yanı sıra de.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında kablosuz erişimi yoktur. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure Microsoft Azure ortamında kablosuz erişimi izin vermiyor. |


 ## <a name="nist-800-53-control-ac-19a"></a>NIST 800 53 denetim AC-19.a

#### <a name="access-control-for-mobile-devices"></a>Mobil cihazlar için erişim denetimi

**AC 19.a** kuruluş kullanım kısıtlamaları, yapılandırma gereksinimleri, bağlantı gereksinimleri ve kuruluş tarafından denetlenen mobil aygıtlar için Uygulama Kılavuzu oluşturur.

**Sorumlulukları:**`Not Applicable`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri tarafından denetlenen mobil aygıt yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure mobil cihazlar Azure sınırları içinde izin vermiyor. Bu nedenle, bu denetim Microsoft Azure için geçerli değildir. |


 ## <a name="nist-800-53-control-ac-19b"></a>NIST 800 53 denetim AC-19.b

#### <a name="access-control-for-mobile-devices"></a>Mobil cihazlar için erişim denetimi

**AC 19.b** kuruluş kuruluş bilgi systems mobil aygıtlara bağlantı yetkilendirir.

**Sorumlulukları:**`Not Applicable`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri tarafından denetlenen mobil aygıt yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure mobil cihazlar Azure sınırları içinde izin vermiyor. Bu nedenle, bu denetim Microsoft Azure için geçerli değildir. |


 ### <a name="nist-800-53-control-ac-19-5"></a>NIST 800 53 denetim AC-19 (5)

#### <a name="access-control-for-mobile-devices--full-device--container-based--encryption"></a>Mobil cihazlar için erişim denetimi | Tam aygıt / kapsayıcı tabanlı şifreleme

**AC-19 (5)** kuruluş kullanır [seçim: tam cihaz şifreleme; kapsayıcı şifrelemesi] bilgi bütünlüğü ve gizliliği korumak için [atama: kuruluş tarafından tanımlanan mobil cihazları].

**Sorumlulukları:**`Not Applicable`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri tarafından denetlenen mobil aygıt yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure mobil cihazlar Azure sınırları içinde izin vermiyor. Bu nedenle, bu denetim Microsoft Azure için geçerli değildir. |


 ## <a name="nist-800-53-control-ac-20a"></a>NIST 800 53 denetim AC-20.a

#### <a name="use-of-external-information-systems"></a>Dış bilgi Systems kullanımı

**AC 20.a** hüküm ve koşullar, işletim yapamaz, diğer kuruluşlarla kurulan herhangi bir güven ilişkisine tutarlı kuruluş kurar ve/veya dış bilgi systems, izin verme koruma yetkili kişiler Dış bilgi sistemlerden bilgi sisteme erişmek için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini kullanımına ilişkin sağlama bulut hizmet tekliflerinin FedRAMP altında bulunabilir. Azure FedRAMP birleşik yetkilendirme Panosu (JAB tarafından) (P-ATO) çalışması için geçici bir yetkilendirme Azure bulut hizmetlerine edinme kullanımına etkinleştirme devlet dairesi tarafından verildi. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-20b"></a>NIST 800 53 denetim AC-20.b

#### <a name="use-of-external-information-systems"></a>Dış bilgi Systems kullanımı

**AC 20.b** hüküm ve koşullar, işletim yapamaz, diğer kuruluşlarla kurulan herhangi bir güven ilişkisine tutarlı kuruluş kurar ve/veya dış bilgi systems, izin verme koruma yetkili kişiler işlem, saklamak veya dış bilgi systems kullanarak kuruluş denetlenen bilgi aktarmak için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini kullanımına ilişkin sağlama bulut hizmet tekliflerinin FedRAMP altında bulunabilir. Azure FedRAMP birleşik yetkilendirme Panosu (JAB tarafından) (P-ATO) çalışması için geçici bir yetkilendirme Azure bulut hizmetlerine edinme kullanımına etkinleştirme devlet dairesi tarafından verildi. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-20-1"></a>NIST 800 53 denetim AC-20 (1)

#### <a name="use-of-external-information-systems--limits-on-authorized-use"></a>Dış bilgi Systems kullanımını | Yetkili kullanım sınırları

**AC-20 (1)** kuruluş bilgi sisteme erişmek için işlem, saklamak veya yalnızca kuruluş doğrularken kuruluş denetlenen bilgi iletimi için bir dış bilgi sistemi kullanmaya yetkili kişiler izin verir. gerekli güvenlik denetimleri kuruluşunuzun bilgi güvenlik ilkesi ve güvenlik planı belirtildiği gibi dış sistemdeki uyarlamasını; veya dış bilgi sistemi barındırma kuruluş varlıkla onaylanan bilgileri sistem bağlantı veya işleme anlaşmaları korur.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin Kurumsal düzeyde bilgi teknolojileri grubu, kuruluşunuzun bilgi güvenlik gereksinimlerine sahip bulut hizmeti sağlayıcısı uyumluluğunu doğrulamak ve ilişkili bulut hizmet teklifleri kullanmak için kuruluş çapında onayı vermek olabilirsiniz. Azure FedRAMP birleşik yetkilendirme Panosu (JAB tarafından) (P-ATO) çalışması için geçici bir yetkilendirme verildi. Azure FedRAMP güvenlik denetimini ve diğer gereksinimler ile uyumluluğunu doğrulamak için üçüncü taraf FedRAMP onaylı değerlendirme kuruluşu (3PAO) tarafından uygunluk. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ac-20-2"></a>NIST 800 53 denetim AC-20 (2)

#### <a name="use-of-external-information-systems--portable-storage-devices"></a>Dış bilgi Systems kullanımını | Taşınabilir depolama aygıtları

**AC-20 (2)** kuruluş [seçim: kısıtlar; yasaklar] dış bilgi sistemlerde yetkili kişiler tarafından kuruluş denetlenen taşınabilir depolama aygıtlarının kullanımını.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft, Microsoft Azure ortamı içindeki müşteri tarafından denetlenen taşınabilir depolama aygıtları izin vermiyor. |


 ## <a name="nist-800-53-control-ac-21a"></a>NIST 800 53 denetim AC-21.a

#### <a name="information-sharing"></a>Bilgi paylaşma

**AC 21.a** kuruluş bilgi paylaşımı ortağına atanmış erişim kimlik doğrulamalarını bilgilerini erişim kısıtlamalarını eşleşip eşleşmediğini belirlemek yetkili kullanıcıların etkinleştirerek paylaşımı kolaylaştıran [atama: Kullanıcı kendi takdirine bağlı gerekli olduğu durumlarda paylaşımı kuruluş tarafından tanımlanan bilgileri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetimi ilkesini bilgi paylaşımı ilgili hükümleri içerebilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-21b"></a>NIST 800 53 denetim AC-21.b

#### <a name="information-sharing"></a>Bilgi paylaşma

**AC 21.b** kuruluş kullanır [atama: kuruluş tarafından tanımlanan otomatik mekanizmaları veya el ile işlemler] bilgi paylaşımı işbirliği kararları vermekte yol kullanıcılara yardımcı olmak için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bir kuruluş düzeyi bilgi karar destek yetenek paylaşımı üzerinde dayanabileceği. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-22a"></a>NIST 800 53 denetim AC-22.a

#### <a name="publicly-accessible-content"></a>Genel olarak erişilebilir içerik

**AC 22.a** kuruluş genel olarak erişilebilir bilgi sistemi üzerine bilgi göndermek için yetkili kişiler belirler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetim yordamları genel olarak erişilebilir bilgileri postalamak için yetkilendirilmiş kişiler belirleyebilirsiniz. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-22b"></a>NIST 800 53 denetim AC-22.b

#### <a name="publicly-accessible-content"></a>Genel olarak erişilebilir içerik

**AC 22.b** kuruluş genel olarak erişilebilir bilgi özel bilgiler içermediğinden emin olmak için yetkili kişiler eğitir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri genel olarak erişilebilir bilgileri postalamak için yetkilendirilmiş kişiler için kuruluş düzeyi eğitim bağlı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-22c"></a>NIST 800 53 denetim AC-22.c

#### <a name="publicly-accessible-content"></a>Genel olarak erişilebilir içerik

**AC 22.c** kuruluş ortak olmayan bilgileri dahil olmadığından emin olmak için genel olarak erişilebilir bilgi sisteme nakil önce bilgi önerilen içeriğini gözden geçirir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetim yordamları genel olarak erişilebilir bir sistem postalama için önerilen içerik için bir gözden geçirme işlemi oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ac-22d"></a>NIST 800 53 denetim AC-22.d

#### <a name="publicly-accessible-content"></a>Genel olarak erişilebilir içerik

**AC 22.d** kuruluş ortak olmayan bilgileri için genel olarak erişilebilir bilgi sistemde içerik incelemeleri [atama: kuruluş tarafından tanımlanan sıklığı] bulunan gibi bilgileri kaldırır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde erişim denetim yordamları periyodik olarak İnceleme genel olarak erişilebilir sistemlere gönderilen içerik için bir işlem oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |

