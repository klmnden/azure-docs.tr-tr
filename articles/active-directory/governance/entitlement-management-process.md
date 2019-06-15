---
title: Azure AD hak yönetimi (Önizleme) - Azure Active Directory işlemi ve e-posta bildirimleri iste
description: Erişim paketi ve Azure Active Directory hak yönetimi (Önizleme) e-posta bildirimleri gönderilirken isteği işlemi hakkında bilgi edinin.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/30/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: aede5e315141251026867f7028ebf989d44da4d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473035"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management-preview"></a>Azure AD hak yönetimi (Önizleme) işlemi ve e-posta bildirimleri iste

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bir kullanıcı erişim paket için bir istek gönderdiğinde, bu isteği sunmak için bir işlem başlatıldı. İşlem sırasında anahtar olayları gerçekleştiğinde azure AD hak yönetimi onaylayanlar ve istek sahipleri için ayrıca e-posta bildirimleri gönderir.

Bu makalede, istek işlemin yanı sıra, gönderilen e-posta bildirimleri açıklanır.

## <a name="request-process"></a>İstek işlemi

Bir erişim paketi erişmesi gereken bir kullanıcı, bir erişim isteği gönderebilirsiniz. İlke yapılandırmasına bağlı olarak, bir onay isteği gerektirebilir. Bir istek onaylandığında, her kaynak erişim paketindeki kullanıcı erişimi atamak bir işlem başlar. Aşağıdaki diyagramda işlemi ve farklı durumları hakkında genel bir bakış sunar.

![Onay işlemi diyagramı](./media/entitlement-management-process/request-process.png)

| Eyalet | Açıklama |
| --- | --- |
| Gönderildi | Kullanıcı bir istek gönderir. |
| Onay bekleniyor | İlke erişim paketi için onay gerektiriyorsa, bir isteği onay bekliyor taşır. |
| Süresi dolmuş | Hiçbir onaylayanlara onay isteği zaman aşımı süresi içinde bir isteği onaylıyorsanız, isteğin süresi dolar. Yeniden denemek için kullanıcı, isteği yeniden gönderin gerekecektir. |
| Reddedildi | Onaylayan bir isteği reddeder. |
| Onaylandı | Onaylayan bir isteğini onaylar. |
| Teslim etme | Kullanıcının **değil** edilmiş erişim paketteki tüm kaynaklara erişim atanmış. Bu dış kullanıcı ise, kullanıcı kaynak dizini erişilen henüz ve izinleri istemi kabul. |
| Teslim Edildi | Kullanıcı erişim paketteki tüm kaynaklara erişimi atanmıştır. |
| Genişletilmiş erişim | Uzantıları ilkede izin veriliyorsa, kullanıcı atama genişletilmiş. |
| Erişim süresi doldu | Kullanıcının erişim paketi için erişim süresi doldu. Erişim elde etmek için yeniden kullanıcı bir istek göndermeniz gerekir. |

## <a name="email-notifications"></a>E-posta bildirimleri

Onaylayan varsa, bir erişim isteği onaylamak, ihtiyacınız olduğunda ve bir erişim isteği tamamlandığında e-posta bildirimleri gönderilir. Bir istek sahibi ise isteğinizin durumunu belirten bir e-posta bildirimleri gönderilir. Bu e-posta bildirimleri, aşağıdaki diyagramda gösterildiği gönderilir.

![Hak Yönetimi e-posta işlemi](./media/entitlement-management-process/email-notifications.png)

Aşağıdaki tabloda, her biri bu e-posta bildirimleri hakkında daha fazla ayrıntı sağlar.

| # | E-posta konusu | Gönderildiğinde | Gönderilen |
| --- | --- | --- | --- |
| 1 | Eylem gerekiyor: Gözden geçirme erişim isteğinden *[istek sahibi]* için *[access paketi]* tarafından *[date]* | Bir istek sahibi erişim paket için bir istek gönderdiğinde | Tüm onaylayanlar |
| 2 | Eylem gerekiyor: Gözden geçirme erişim isteğinden *[istek sahibi]* için *[access paketi]* tarafından *[date]* | X gün önce onay istek zaman aşımı | Tüm onaylayanlar |
| 3 | Durum bildirimi: *[istek sahibi]* kişinin erişim isteğini *[access paketi]* süresi doldu | Ne zaman onaylayan değil onaylamak veya istek süresi içinde bir erişim isteği reddet | İstek sahibi |
| 4 | Durum bildirimi: *[istek sahibi]* erişim isteğini *[access paketi]* tamamlandı | İlk onaylayan onaylar veya bir erişim isteğini reddederse ne zaman | Tüm onaylayanlar |
| 5 | Erişim engellendi *[access paketi]* | Ne zaman bir istek sahibi erişim paketine erişim reddedildi | İstek sahibi |
| 6 | Artık erişiminiz *[access paketi]*  | Ne zaman bir istek sahibi erişim paketindeki her bir kaynağa erişim izni verildi | İstek sahibi |
| 7 | Erişiminizi *[access paketi]* X gün sonra sona eriyor. | X gün önce erişim paket sahibinin erişimi sona erer | İstek sahibi |
| 8 | Erişiminizi *[access paketi]* süresi doldu | Bir erişim paketi sahibinin erişim süresi dolduğunda | İstek sahibi |

### <a name="access-request-emails"></a>Erişim isteği e-postaları

Bir istek sahibinin onay gerektirecek şekilde yapılandırılmış bir erişim paket için bir erişim isteği gönderdiğinde, ilkede yapılandırılan tüm onaylayanlar istek ayrıntılarını içeren bir e-posta bildirimi alır. Ayrıntıları sahibinin adını, kuruluş ekleyin, erişim, İş Gerekçesi isteği gönderildiğinde ve isteğin süresi dolar sağlanırsa, başlangıç ve bitiş tarihi. E-postayı burada onaylayanlar onaylayın veya erişim isteği reddedin bir bağlantı içerir. Aşağıda, bir istek sahibine erişim isteği gönderdiğinde, bir onaylayana gönderilen bir e-posta bildirimi verilmiştir.

![Gözden geçirme erişim isteği e-postası](./media/entitlement-management-shared/email-approve-request.png)

### <a name="approved-or-denied-emails"></a>Onaylanan veya reddedilen e-postaları

İstek sahipleri, kendi erişim isteği onaylanmış ve erişimi için kullanılabilir olduğunda ya da kendi erişim isteğinin reddedildiğini bildirilir. Onaylayan bir istek sahibi tarafından gönderilen bir erişim isteği aldığında, onaylayabilir ya da erişim isteği reddeder. İş Gerekçesi kendi kararı eklemek onaylayan gerekir.

Erişim isteği onaylandığında, hak yönetim erişim paketteki kaynakların her biri için istek sahibi erişim izni verme işlemini başlatır. İstek sahibi erişim paketindeki her bir kaynağa erişimi verildikten sonra bir e-posta bildirimi, erişimi İsteği Onaylandı ve artık erişim paketine sahiptirler istek sahibine gönderilir. Aşağıda, bir erişim paket erişim verildiğinde, bir istek sahibine gönderilen bir e-posta bildirimi verilmiştir.

Erişim isteği reddedildiğinde e-posta bildirimi istek sahibine gönderilir. Aşağıda, kullanıcıların erişim isteğinin reddedilmesi durumunda, bir istek sahibine gönderilen bir e-posta bildirimi verilmiştir.

### <a name="expired-access-request-emails"></a>Erişim isteği e-postaları süresi doldu

İstek sahipleri, kendi erişim isteği sona erdiğinde bildirilir. Bir istek sahibine erişim isteği gönderdiğinde, isteğin sonra süresi dolar bir istek süresi vardır. Bir onaylama ve reddetme karar gönderen hiçbir onaylayıcısı varsa, isteği onay bekliyor durumda kalmak devam eder. İstek yapılandırılmış zaman aşımı süresinin ulaştığında, istek süresi ve artık Onaylandı veya onaylayanlar tarafından reddedildi. Bu durumda, istek süresi dolmuş bir duruma geçer. Süresi dolmuş bir isteği artık Onaylandı veya reddedildi. İstek sahibine kendi erişim isteğinin süresi doldu ve erişim isteği yeniden göndermek ihtiyaç duydukları bir e-posta bildirimi gönderilir. Aşağıda, kullanıcıların erişim isteği sona erdiğinde, bir istek sahibine gönderilen bir e-posta bildirimi verilmiştir.

![Erişim isteği e-posta süresi doldu](./media/entitlement-management-process/email-expired-access-request.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Bir erişim paket erişim isteği](entitlement-management-request-access.md)
- [Onaylayın veya reddedin erişim istekleri](entitlement-management-request-approve.md)
