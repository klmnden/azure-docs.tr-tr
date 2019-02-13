---
title: Bir grup - Azure Active Directory lisansları atama | Microsoft Docs
description: Azure Active Directory Grup lisanslama yoluyla kullanıcılara lisansları atama
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 92fc46dd3fe3c6526a9a85fd13ec7297bf270976
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208903"
---
# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Azure Active Directory'de Grup üyeliği kullanıcıları için lisans atama

Bu makalede bir Azure Active Directory'de (Azure AD) kullanıcı grubu ürün lisansları atamak ve ardından bunların doğru lisansınız doğrulama size yol gösterir.

Bu örnekte, Kiracı adlı bir güvenlik grubu içeren **ik departmanı**. Bu grubun tüm üyelerinin İnsan Kaynakları departmanı (yaklaşık 1000 kullanıcı) içerir. Tüm departmanı için Office 365 Kurumsal E3 lisansı atamak istediğiniz. Departman kullanmaya başlamak hazır olana kadar üründe mevcut Kurumsal Yammer service geçici olarak devre dışı bırakılmalıdır. Ayrıca, Enterprise Mobility + Security Lisanslarımı aynı kullanıcı grubunu dağıtmak istiyorsanız.

> [!NOTE]
> Bazı Microsoft hizmetleri tüm konumlarda kullanılamaz. Bir kullanıcıya lisans atanabilmesi için önce yönetici kullanıcı kullanım konum özelliği belirtmesi gerekir.

> Grup lisansı atama için kullanım konumu belirtilmemiş olmadan herhangi bir kullanıcı dizin konumunu devralır. Birden fazla konumda kullanıcılar varsa, her zaman kullanım konumu lisans ataması sonucu sağlar (örneğin aracılığıyla yapılandırması) AAD Connect - Azure AD'de kullanıcı oluşturma akışınızı parçası her zaman doğru olduğundan ve kullanıcıların almadığınız olarak ayarlamanızı öneririz izin verilmeyen bir konumda Hizmetleri.

## <a name="step-1-assign-the-required-licenses"></a>1. Adım: Gerekli lisansları atama

1. Oturum [ **Azure portalında** ](https://portal.azure.com) yönetici hesabı. Lisansları yönetmek için hesabın bir genel yönetici rolü veya kullanıcı hesabı yöneticisi olması gerekir.

2. Seçin **tüm hizmetleri** sol gezinti bölmesi ve seçip **Azure Active Directory**. Bu bölme Sık Kullanılanlara Ekle veya portal panosuna sabitleyin.

3. Üzerinde **Azure Active Directory** bölmesinde **lisansları** bakın ve kiracıdaki tüm lisanslanabilir ürünler yöneteceğiniz yeri bir bölme açmak için.

4. Altında **tüm ürünleri**, Office 365 Kurumsal E3 ve Enterprise Mobility + Security ürün adlarını seçerek seçin. Atama başlatmak için **atama** bölmenin üstünde.

   ![Tüm ürünlerin lisans atama](./media/licensing-groups-assign/all-products-assign.png)

5. Üzerinde **Ata lisans** bölmesinde tıklayın **kullanıcılar ve gruplar** açmak için **kullanıcılar ve gruplar** bölmesi. Grup adı için arama *ik departmanı*grubu seçin ve ardından onaylamak emin **seçin** bölmesinin alt kısmındaki.

   ![Grup seçin](./media/licensing-groups-assign/select-a-group.png)

6. Üzerinde **Ata lisans** bölmesinde tıklayın **atama seçenekleri (isteğe bağlı)**, daha önce seçilen iki ürünü de dahil tüm hizmet planları görüntüler. Bulma **Yammer Kurumsal** gidip **kapalı** ürün lisansının bu hizmet devre dışı bırakmak için. Tıklayarak onaylayın **Tamam** kısmındaki **atama seçenekleri**.

   ![Atama seçenekleri](./media/licensing-groups-assign/assignment-options.png)

7. Atamayı tamamlamak için **Lisans ata** bölmesinin en altında bulunan **Ata**'ya tıklayın.

8. Bildirim durumu ve işleminin sonucunu gösteren sağ üst köşesinde görüntülenir. (Örneğin, nedeniyle önceden var olan lisans grubunda) grubuna ataması tamamlanamadı, hatanın ayrıntılarını görüntülemek için bildirime tıklayın.

Biz, ik departmanı grubu için bir lisans şablonu artık belirttiniz. Bu grubun tüm mevcut üyelerin işlemek için Azure AD'de bir arka plan işlemi başlatıldı. Bu ilk işlem, geçerli Grup boyutuna bağlı olarak biraz zaman alabilir. Sonraki adım, işlemin tamamlandığını doğrulayın ve daha fazla dikkat sorunlarını gidermek için gerekip gerekmediğini belirlemek açıklar.

> [!NOTE]
> Alternatif bir konumdan aynı atama başlayabilirsiniz: **Kullanıcılar ve gruplar** Azure AD'de. Git **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm grupları**. Ardından grubun bulun, seçin ve Git **lisansları** sekmesi. **Atama** bölmenin en üstünde düğme lisans ataması bölmesini açar.

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>2. Adım: İlk atama tamamlandığını doğrulama

1. Git **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm grupları**. Ardından bulun **ik departmanı** lisansları atanmış olan grup.

2. Üzerinde **ik departmanı** grubu bölmesinde **lisansları**. Bu, tam olarak kullanıcıya lisans atandı ve içine bak gereken herhangi bir hata varsa hızla doğrulamanıza olanak sağlar. Aşağıdaki bilgiler kullanılabilir:

   - Gruba atanmış olan ürün lisansı listesi. Etkin belirli hizmetler göstermek ve değişiklik yapmak için bir giriş seçin.

   - Gruba (değişiklikler işlenir veya işleme için tüm kullanıcı üyeleri bitip bitmediğini) yapılan en son lisans değişiklikleri durumu.

   - Lisans atanamadı olduğundan bir hata durumunda onlara olan kullanıcılar hakkında bilgiler.

   ![Atama seçenekleri](./media/licensing-groups-assign/assignment-errors.png)

3. Daha ayrıntılı bilgi lisansı altında işleme hakkında **Azure Active Directory** > **kullanıcılar ve gruplar** > *grup adı*  >  **Denetim günlükleri**. Aşağıdaki etkinlikleri dikkat edin:

   - Etkinlik: **Kullanıcılara grup tabanlı lisans uygulama Başlat**. Sistem grupta lisans atama değişikliği alır ve tüm kullanıcı üyelerine uygulama başlatır, bu günlüğe kaydedilir. Yapılan değişiklikler hakkında bilgiler içerir.

   - Etkinlik: **Kullanıcılara grup tabanlı lisans uygulamayı sonlandırma**. Bu gruptaki tüm kullanıcılar işleme sistem sona erdiğinde günlüğe kaydedilir. Bu, kaç kullanıcının başarıyla işlendi ve kaç kullanıcının Grup lisansları atanamadı bir özetini içerir.

   [Bu bölümü okuyun](licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) grup tabanlı lisanslama tarafından yapılan değişiklikleri çözümlemek için denetim günlüklerini nasıl kullanılabileceği hakkında daha fazla bilgi için.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>3. Adım: İçin lisans sorunlarını denetleyin ve çözümleyin

1. Git **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm grupları**ve bulma **ik departmanı** Lisans atanmış olan grup.
2. Üzerinde **ik departmanı** grubu bölmesinde **lisansları**. Bölmenin en üstünde bir bildirim için lisansı atanamıyor 10 kullanıcı olduğunu gösterir. Tıklayarak, bu grup için bir lisans hatası durumunda tüm kullanıcıları içeren bir liste açılır.
3. **Başarısız atamalar** sütun söyler bize her iki ürün lisansları kullanıcılara atanan uygulanamadı. **İlk başarısızlık nedeni** hatanın nedenini sütun içerir. Bu durumda sahip **çakışan hizmet planları**.

   ![Başarısız atamalar](./media/licensing-groups-assign/failed-assignments.png)

4. Açmak için bir kullanıcı seçin **lisansları** bölmesi. Bu bölme, kullanıcıya atanmış olan tüm lisansları gösterir. Bu örnekte, devralındığı Office 365 Kurumsal E1 lisans kullanıcının **bilgi noktası kullanıcılar** grubu. Bu sistem, uygulamayı denedi E3 lisansı çakışıyor **ik departmanı** grubu. Sonuç olarak, bu gruptaki lisanslar hiçbiri kullanıcıya atandı.

   ![Bir kullanıcı için lisansları görüntüle](./media/licensing-groups-assign/user-license-view.png)

5. Bu çakışmayı çözmek için kullanıcıyı kaldırmak **bilgi noktası kullanıcılar** grubu. Azure AD değişikliği işledikten sonra **ik departmanı** lisansları doğru şekilde atanır.

   ![Doğru atanmış lisansı](./media/licensing-groups-assign/license-correctly-assigned.png)

## <a name="next-steps"></a>Sonraki adımlar

Grupları aracılığıyla lisans yönetimi için ayarlama özelliği hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Grup tabanlı Azure Active Directory lisansı nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](licensing-groups-resolve-problems.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](licensing-groups-migrate-users.md)
* [Kullanıcılar Azure Active Directory'de Grup tabanlı lisanslama kullanarak ürün lisansları arasında geçirme](licensing-groups-change-licenses.md)
* [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](../active-directory-licensing-group-advanced.md)
* [Azure Active Directory'de Grup tabanlı lisanslama için PowerShell örnekleri](licensing-ps-examples.md)
