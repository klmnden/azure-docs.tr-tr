---
title: 'Azure AD Connect eşitleme: yanlışlıkla silmeleri engelleme | Microsoft Docs'
description: Bu konuda Azure AD Connect engelle (yanlışlıkla silmeleri engelleme) yanlışlıkla silmeleri özelliğinde açıklanmaktadır.
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
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: ecec08dcfa251b94ccfd09e87a8499dc03164ea9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34595079"
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect Eşitleme: Yanlışlıkla Silmeleri Engelleme
Bu konuda Azure AD Connect engelle (yanlışlıkla silmeleri engelleme) yanlışlıkla silmeleri özelliğinde açıklanmaktadır.

Azure AD Connect yükleme engellediğinizde yanlışlıkla silmeleri varsayılan olarak etkindir ve bir verme 500'den fazla siler ile izin vermeyecek şekilde yapılandırılmış. Bu özellik, çok sayıda kullanıcı ve diğer nesneleri etkileyecek şirket içi dizininize yanlışlıkla yapılandırma değişiklikleri ve değişiklikleri korumak için tasarlanmıştır.

## <a name="what-is-prevent-accidental-deletes"></a>Ne yanlışlıkla silmeleri engelleme olduğu
Dahil birçok siler gördüğünüzde yaygın senaryolar:

* Değişikliklerini [filtreleme](active-directory-aadconnectsync-configure-filtering.md) tüm burada [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) veya [etki alanı](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) seçildiyse.
* Bir OU'daki tüm nesneler silinir.
* İçindeki tüm nesneleri eşitleme için kapsam dışına olduğu kabul edilir şekilde bir OU adlandırılır.

Varsayılan değer 500 nesnelerin PowerShell ile değiştirilebilir kullanarak `Enable-ADSyncExportDeletionThreshold`, Azure Active Directory Connect ile yüklenen AD eşitleme modülü bir parçası olduğu. Bu değer, kuruluşunuzun sığacak şekilde yapılandırmanız gerekir. Eşitleme Zamanlayıcı 30 dakikada bir çalışır olduğundan, değeri 30 dakika içinde görülen siler sayısıdır.

Azure AD'ye aktarılacak hazırlanan çok fazla silme varsa, verme durdurur ve böyle bir e-posta alırsınız:

![E-posta yanlışlıkla silmeleri engelleme](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Merhaba (teknik kişi). (Zaman) kimlik eşitleme hizmeti silme sayısı (kuruluş adı) için yapılandırılmış silme eşiği aşıldı algıladı. (Sayı) toplam nesneleri çalıştırmak bu kimlik eşitleme silinmek gönderildi. Bu karşıladığında veya (sayı) nesneleri yapılandırılmış silme eşik değerini aştı. Onaylama vermeniz ihtiyacımız ilerlemeden önce bu silme işlemlerini işlenmesi gerektiğini. Lütfen önleme yanlışlıkla silmeleri bu e-posta iletisinde listelenen hata hakkında daha fazla bilgi için bkz.*
>
> 

Durum de görebilirsiniz `stopped-deletion-threshold-exceeded` baktığınızda **Eşitleme Hizmeti Yöneticisi'ni** verme profili için kullanıcı Arabirimi.
![Eşitleme Hizmeti Yöneticisi kullanıcı Arabirimi yanlışlıkla silmeleri engelleme](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Beklenmeyen olduysa, araştırın ve düzeltici eylemleri gerçekleştirin. Silinmek üzere hangi nesnelerin olduğunu görmek için aşağıdakileri yapın:

1. Başlat **eşitleme hizmeti** Başlat menüsünden.
2. Git **Bağlayıcılar**.
3. Bağlayıcı türü olan seçin **Azure Active Directory**.
4. Altında **Eylemler** sağa seçin **arama bağlayıcı alanı**.
5. Altında pencerede **kapsam**seçin **bağlantısı kesilmiş beri** ve geçmişteki bir zamana seçin. Tıklatın **arama**. Bu sayfa silinmek üzere tüm nesnelerin bir görünümünü sağlar. Her öğe tıklayarak nesnesi hakkında ek bilgi alabilirsiniz. Tıklatarak **sütun ayarı** kılavuzda görünür olması için ek öznitelikler eklemek için.

![Bağlayıcı alanı arama](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Tüm siler isterseniz, aşağıdakileri yapın:

1. Geçerli silme eşiği almak için PowerShell cmdlet'ini çalıştırın `Get-ADSyncExportDeletionThreshold`. Bir Azure AD genel yönetici hesabı ve parolası belirtin. Varsayılan değer 500'dür.
2. Geçici olarak bu korumayı devre dışı bırakın ve Git, PowerShell cmdlet'ini çalıştırın bu siler sağlar: `Disable-ADSyncExportDeletionThreshold`. Bir Azure AD genel yönetici hesabı ve parolası belirtin.
   ![Kimlik Bilgileri](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
3. Azure Active Directory seçiliyken Bağlayıcısı ile bir eylem seçin **çalıştırmak** seçip **verme**.
4. Korumayı yeniden etkinleştirmek için PowerShell cmdlet'ini çalıştırın: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`. 500 geçerli silme eşiği alınırken fark değerle değiştirin. Bir Azure AD genel yönetici hesabı ve parolası belirtin.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
