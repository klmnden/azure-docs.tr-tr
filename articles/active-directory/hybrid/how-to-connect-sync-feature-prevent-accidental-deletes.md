---
title: 'Azure AD Connect eşitleme: Yanlışlıkla silmeleri engelleme | Microsoft Docs'
description: Bu konu, Azure AD CONNECT'te engelle (yanlışlıkla silmeleri engelleme) yanlışlıkla silmeleri özelliğini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 07/12/2017
ms.date: 11/09/2018
ms.component: hybrid
ms.author: v-junlch
ms.openlocfilehash: b1244dd460196e5882caab0d4b526850da48d084
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60383414"
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect eşitleme: Yanlışlıkla silmeleri engelleme
Bu konu, Azure AD CONNECT'te engelle (yanlışlıkla silmeleri engelleme) yanlışlıkla silmeleri özelliğini açıklar.

Azure AD Connect yükleme önlediğinde yanlışlıkla silmeleri varsayılan olarak etkindir ve 500'den fazla siler dışa izin vermeyecek şekilde yapılandırılmış. Bu özellik, birçok kullanıcıyı ve nesneyi etkileyebilecek yanlışlıkla gerçekleştirilen yapılandırma değişikliklerinden ve şirket içi dizin değişikliklerinden koruma sağlamak için tasarlanmıştır.

## <a name="what-is-prevent-accidental-deletes"></a>Ne yanlışlıkla silmeleri engelleme olduğu
Dahil birçok siler gördüğünüzde yaygın senaryolar:

- Değişikliklerini [filtreleme](how-to-connect-sync-configure-filtering.md) tüm durumlarda [OU](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) veya [etki alanı](how-to-connect-sync-configure-filtering.md#domain-based-filtering) seçimi kaldırıldı.
- Bir kuruluş birimi içindeki tüm nesnelerin silinmesi.
- Bir kuruluş biriminin yeniden adlandırılması nedeniyle içindeki tüm nesnelerin eşitleme kapsamı dışında olarak değerlendirilmesi.

PowerShell ile varsayılan değer 500 nesnelerin değiştirilebilir kullanarak `Enable-ADSyncExportDeletionThreshold`, Azure Active Directory Connect ile yüklenen AD eşitleme modülü bir parçası değildir. Bu değer, kuruluşunuzun sığacak şekilde yapılandırmanız gerekir. Eşitleme Zamanlayıcısı 30 dakikada çalıştığından, değer 30 dakika içinde görülen siler sayısıdır.

Azure AD'ye aktarılacak aşamalı çok fazla silme varsa, dışarı aktarma durur ve böyle bir e-posta alırsınız:

![Yanlışlıkla silmeleri e-posta engelle](./media/how-to-connect-sync-feature-prevent-accidental-deletes/email.png)

> *Merhaba (teknik konular ilgili kişisi). Kimlik eşitleme hizmeti silme sayısı (kuruluş adı) için yapılandırılmış silme eşiği aşıldı (zaman) olduğunu algıladı. (Sayı) toplam nesneleri, bu kimlik eşitleme çalıştırma silinmek gönderildi. Bu karşılanmalı veya (sayı) nesneleri yapılandırılmış silme eşik değerini aştı. Onay vermenizi ihtiyacımız ilerlemeden önce bu silme işlenmesi gerekir. Lütfen önleme yanlışlıkla silinmekten bu e-posta iletisinde listelenen hata hakkında daha fazla bilgi için bkz.*
>
> 

Durum atabilirsiniz `stopped-deletion-threshold-exceeded` baktığınızda **Eşitleme Hizmeti Yöneticisi** dışarı aktarma profili için kullanıcı Arabirimi.
![Eşitleme Hizmeti Yöneticisi kullanıcı Arabirimi yanlışlıkla silmeleri engelleme](./media/how-to-connect-sync-feature-prevent-accidental-deletes/syncservicemanager.png)

Bu beklenmeyen, araştırma ve düzeltme girişimlerinde bulunun. Silinmek üzere hangi nesnelerin olduğunu görmek için aşağıdakileri yapın:

1. Başlangıç **eşitleme hizmeti** Başlat menüsünden.
2. Git **Bağlayıcılar**.
3. Bağlayıcı türü olan seçin **Azure Active Directory**.
4. Altında **eylemleri** sağa seçin **arama bağlayıcı alanı**.
5. Altında pencerede **kapsam**seçin **bağlı olduğundan** ve geçmişteki bir zamanı seçin. **Ara**'ya tıklayın. Bu sayfa, silinmek üzere tüm nesnelerin bir görünümünü sağlar. Her bir öğeye tıklayarak bu nesne hakkında ek bilgi alabilirsiniz. Ayrıca **sütun ayarı** kılavuzunda görünür olması için ek öznitelikler eklemek için.

![Bağlayıcı alanı arama](./media/how-to-connect-sync-feature-prevent-accidental-deletes/searchcs.png)

Ardından tüm silmeleri isterseniz, aşağıdakileri yapın:

1. Geçerli silme eşiği almak için PowerShell cmdlet'ini çalıştırın `Get-ADSyncExportDeletionThreshold`. Bir Azure AD genel yönetici hesabı ve parolasını belirtin. Varsayılan değer 500'dür.
2. Geçici olarak bu korumayı devre dışı bırakın ve bu siler geçtikleri, PowerShell cmdlet'ini çalıştırın: `Disable-ADSyncExportDeletionThreshold`. Bir Azure AD genel yönetici hesabı ve parolasını belirtin.
   ![Kimlik Bilgileri](./media/how-to-connect-sync-feature-prevent-accidental-deletes/credentials.png)
3. Azure Active Directory seçili durumdayken Bağlayıcısı ile bir eylem seçin **çalıştırma** seçip **dışarı**.
4. Korumayı yeniden etkinleştirmek için PowerShell cmdlet'ini çalıştırın: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`. 500 geçerli silme eşiği alınırken fark değeriyle değiştirin. Bir Azure AD genel yönetici hesabı ve parolasını belirtin.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

- [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

