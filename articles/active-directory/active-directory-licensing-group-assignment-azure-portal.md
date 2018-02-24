---
title: Azure Active Directory'deki bir gruba lisans atamak | Microsoft Docs
description: "Azure Active Directory Grup lisans yoluyla kullanıcılara lisanslar atama"
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f24daaf5690eb1d9a0ab3b27a3626d03e6021d99
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Azure Active Directory'de Grup üyeliğiyle kullanıcılara lisansları atama

Bu makalede, Azure Active Directory'de (Azure AD) kullanıcı grubu ürün lisansları atamak ve bunlar doğru lisansa sahip emin doğrulama anlatılmaktadır.

Bu örnekte, Kiracı adlı bir güvenlik grubu içeren **ik departmanı**. Bu grubun tüm üyelerini İnsan Kaynakları departmanı (yaklaşık 1.000 kullanıcı) içerir. Tüm departman Office 365 Kurumsal E3 lisansları atamak istediğiniz. Departman kullanmaya başlamak için hazır olana kadar ürüne dahil Yammer Kurumsal hizmeti geçici olarak devre dışı bırakılmalıdır. Enterprise Mobility + güvenlik lisansları kullanıcılar aynı gruba dağıtmak istiyorsunuz.

> [!NOTE]
> Bazı Microsoft Hizmetleri tüm konumlarda kullanılabilir değil. Bir kullanıcıya bir lisans atanabilmesi için önce kullanıcı kullanım konumu özelliği belirtmek yönetici vardır.

> Grup lisans atama için belirtilen bir kullanım konumu olmayan tüm kullanıcılar dizininin konumunu devralır. Birden çok konumda kullanıcılar varsa, her zaman kullanım konumu, lisans atama sonucu sağlar (örneğin AAD Connect yapılandırması aracılığıyla) - Azure AD Kullanıcı oluşturma akışında parçası her zaman doğru olduğundan ve kullanıcıların almadığınız olarak ayarlamanızı öneririz izin verilmeyen konumlarda Hizmetleri.

## <a name="step-1-assign-the-required-licenses"></a>1. adım: gerekli lisansları atama

1. Oturum [ **Azure portal** ](https://portal.azure.com) bir yönetici hesabıyla. Lisansları yönetmek için hesabın bir genel yönetici rolü veya kullanıcı hesabının yönetici olması gerekir.

2. Seçin **tüm hizmetleri** sol gezinti bölmesinde ve ardından **Azure Active Directory**. Bu bölme Sık Kullanılanlara Ekle veya portal panosuna sabitleyin.

3. Üzerinde **Azure Active Directory** bölmesinde, **lisansları** burada bakın ve kiracıdaki tüm lisanslanabilir ürünlerini yönetmek bölmesini açmak için.

4. Altında **tüm ürünleri**, Office 365 Kurumsal E3 ve Enterprise Mobility + Security ürün adları işaretleyerek seçin. Atama başlatmak için **atamak** bölmesinin üstünde.

   ![Tüm ürünlerin lisans atayın](media/active-directory-licensing-group-assignment-azure-portal/all-products-assign.png)

5. Üzerinde **Ata lisans** bölmesinde tıklatın **kullanıcılar ve gruplar** açmak için **kullanıcılar ve gruplar** bölmesi. Grup adı arayın *ik departmanı*grubu seçin ve sonra onaylamak emin **seçin** bölmesinin altındaki.

   ![Grup seçin](media/active-directory-licensing-group-assignment-azure-portal/select-a-group.png)

6. Üzerinde **Ata lisans** bölmesinde tıklatın **atama seçenekleri (isteğe bağlı)**, daha önce seçilen iki ürünü de dahil tüm hizmet planlarını görüntüler. Bul **Yammer Kurumsal** ve açıp **devre dışı** bu ürünün lisans hizmetinden devre dışı bırakmak için. Tıklayarak onaylayın **Tamam** alt kısmındaki **atama seçenekleri**.

   ![Atama seçenekleri](media/active-directory-licensing-group-assignment-azure-portal/assignment-options.png)

7. Atama tamamlamak için **Ata lisans** bölmesinde tıklatın **atamak** bölmenin altındaki.

8. Bir bildirim durumunu ve işleminin sonucunu gösteren sağ üst köşesinde görüntülenir. Grubuna atama (örneğin, nedeniyle önceden var olan lisans grubunda) tamamlanamadı, hatanın ayrıntıları görüntülemek için bildirime tıklatın.

Biz şimdi ik departmanı grubu için bir lisans şablonu belirlediniz. Bu grup tüm var olan üyelerine işlemek için Azure AD'de bir arka plan işlemi başlatıldı. Bu ilk işlemi grubun geçerli boyutuna bağlı olarak biraz zaman alabilir. Sonraki adım, işlemin tamamlandığını doğrulayın ve daha fazla dikkat sorunları gidermek için gerekip gerekmediğini belirlemek açıklar.

> [!NOTE]
> Alternatif bir konumdan aynı atama başlatabilirsiniz: **kullanıcılar ve gruplar** Azure AD'de. Git **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm grupları**. Grup bulmak, seçmek ve Git **lisansları** sekmesi. **Atamak** düğmesi bölmenin en üstünde, lisans atama bölmesini açar.

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>2. adım: ilk atama tamamlandığını doğrulama

1. Git **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm grupları**. Ardından bulmak **ik departmanı** lisansı atanmış olan grup.

2. Üzerinde **ik departmanı** Grup bölmesinde, **lisansları**. Bu, hızlı bir şekilde lisanslarını kullanıcılara tam olarak atanmışsa ve içine aramak için gereken herhangi bir hata varsa doğrulamanıza olanak tanır. Aşağıdaki bilgiler sağlanır:

   - Gruba atanmış olan ürün lisansları listesi. Etkinleştirilmiş belirli hizmetleri göstermek için ve değişiklik yapmak için bir girişi seçin.

   - Gruba (değişiklikleri işlenmekte olan veya tüm kullanıcı üyeler için işlemeyi bitirdikten varsa) yapılan en son lisans değişiklikleri durumu.

   - Lisansları atanamıyor çünkü bir hata durumunda onlara kullanıcılar hakkında bilgi.

   ![Atama seçenekleri](media/active-directory-licensing-group-assignment-azure-portal/assignment-errors.png)

3. Daha ayrıntılı lisans altında işleme hakkında bilgi **Azure Active Directory** > **kullanıcılar ve gruplar** > *grup adı*  >  **Denetim günlüklerini**. Aşağıdaki etkinlikler dikkat edin:

   - Etkinliği: **göre grubundaki lisans kullanıcılara uygulama Başlat**. Sistem grubu lisans atamasını değişiminde seçer ve tüm kullanıcı üyelerine uygulama başladığında bu günlüğe kaydedilir. Yapılan değişiklikler hakkında bilgi içerir.

   - Etkinliği: **göre grubundaki lisans kullanıcılara uygulama son**. Sistem gruptaki tüm kullanıcılar işlemeyi tamamladığında bu günlüğe kaydedilir. Kaç kullanıcı başarıyla işlendi ve kaç kullanıcı grubu lisansları atanamıyor özetini içerir.

   [Bu bölümde okuma](./active-directory-licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) grup tabanlı lisans tarafından yapılan değişiklikleri analiz etmek için denetim günlüklerini nasıl kullanılabileceği hakkında daha fazla bilgi edinmek için.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>3. adım: lisans sorunlarını denetleyin ve çözümleyin

1. Git **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm grupları**ve Bul **ik departmanı** lisansı atanmış olan grup.
2. Üzerinde **ik departmanı** Grup bölmesinde, **lisansları**. Bölmenin en üstünde bildirim lisansları için atanamıyor 10 kullanıcıları gösterir. Tıklatarak bu grup için bir lisans hatası durumunda tüm kullanıcıların listesini açar.
3. **Başarısız atamalar** sütun söyler bize her iki ürün lisansları kullanıcılara atanamıyor. **Üst başarısızlık nedeni** sütun hatanın nedenini içerir. Bu durumda, sahip **çakışan hizmet planları**.

   ![Başarısız atamalar](media/active-directory-licensing-group-assignment-azure-portal/failed-assignments.png)

4. Açmak için bir kullanıcı seçin **lisansları** bölmesi. Bu bölme kullanıcıya atanmış olan tüm lisanslar gösterir. Bu örnekte, kullanıcının devralındığı Office 365 Kurumsal E1 lisans sahip **bilgi noktası kullanıcılar** grubu. Bu sistem, uygulamayı denedi E3 lisans çakışıyor **ik departmanı** grubu. Sonuç olarak, bu grubun lisanslarını hiçbiri kullanıcıya atandı.

   ![Bir kullanıcı için Görünüm lisansları](media/active-directory-licensing-group-assignment-azure-portal/user-license-view.png)

5. Bu çakışmayı çözmek için kullanıcıyı kaldırmak **bilgi noktası kullanıcılar** grubu. Azure AD değişikliği işledikten sonra **ik departmanı** lisansları doğru atanır.

   ![Lisansı doğru atandı](media/active-directory-licensing-group-assignment-azure-portal/license-correctly-assigned.png)

## <a name="next-steps"></a>Sonraki adımlar

Grupları üzerinden lisans yönetimi için ayarlama özelliği hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Grup tabanlı Azure Active Directory lisanslaması nedir?](active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'deki bir gruba lisans sorunlarını tanımlama ve](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Azure Active Directory'de Grup tabanlı lisans için tek tek lisanslı kullanıcıları geçirme](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory grup tabanlı ilave senaryolar lisanslama](active-directory-licensing-group-advanced.md)
