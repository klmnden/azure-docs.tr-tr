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
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a54d1ad3ab809f2a2f8df6ae0e30b1b061c2be1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60471340"
---
# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Azure Active Directory'de Grup üyeliği kullanıcıları için lisans atama

Bu makalede bir Azure Active Directory'de (Azure AD) kullanıcı grubu ürün lisansları atamak ve ardından bunların doğru lisansınız doğrulama size yol gösterir.

Bu örnekte, Kiracı adlı bir güvenlik grubu içeren **ik departmanı**. Bu grubun tüm üyelerinin İnsan Kaynakları departmanı (yaklaşık 1000 kullanıcı) içerir. Tüm departmanı için Office 365 Kurumsal E3 lisansı atamak istediğiniz. Departman kullanmaya başlamak hazır olana kadar üründe mevcut Kurumsal Yammer service geçici olarak devre dışı bırakılmalıdır. Ayrıca, Enterprise Mobility + Security Lisanslarımı aynı kullanıcı grubunu dağıtmak istiyorsanız.

> [!NOTE]
> Bazı Microsoft hizmetleri tüm konumlarda kullanılamaz. Bir kullanıcıya lisans atanabilmesi için önce yönetici kullanıcı kullanım konum özelliği belirtmesi gerekir.
> 
> Grup lisansı atama için kullanım konumu belirtilmemiş olmadan herhangi bir kullanıcı dizin konumunu devralır. Birden fazla konumda kullanıcılar varsa, her zaman kullanım konumu lisans ataması sonucu sağlar (örneğin aracılığıyla yapılandırması) AAD Connect - Azure AD'de kullanıcı oluşturma akışınızı parçası her zaman doğru olduğundan ve kullanıcıların almadığınız olarak ayarlamanızı öneririz izin verilmeyen bir konumda Hizmetleri.

## <a name="step-1-assign-the-required-licenses"></a>1. Adım: Gerekli lisansları atama

1. Oturum [ **Azure AD yönetim merkezini** ](https://aad.portal.azure.com) lisans yönetici hesabıyla. Lisansları yönetmek için bir Lisans Yöneticisi, Kullanıcı Yöneticisi veya genel yönetici hesabı olmalıdır.

2. Seçin **lisansları** bakın ve kiracıdaki tüm lisanslanabilir ürünler yöneteceğiniz yeri bir bölme açmak için.

4. Altında **tüm ürünleri**, ürün adlarını seçerek Office 365 Kurumsal E5 ve Enterprise Mobility + Security E3'ı seçin. Atama başlatmak için **atama** bölmenin üstünde.

   ![Ürünler, lisansları atamak için seçin](./media/licensing-groups-assign/all-products-assign.png)
  
5. Üzerinde **Ata lisans** bölmesinde **kullanıcılar ve gruplar** kullanıcıları ve grupları listesini açmak için.

6. Bir kullanıcı veya grup seçin ve ardından **seçin** seçiminizi onaylamak için bölmenin altındaki düğmesi.

7. Üzerinde **Ata lisans** bölmesinde tıklayın **atama seçenekleri**, daha önce seçilen iki ürünü de dahil tüm hizmet planları görüntüler. Bulma **Yammer Kurumsal** gidip **kapalı** ürün lisansının bu hizmet devre dışı bırakmak için. Tıklayarak onaylayın **Tamam** kısmındaki **lisans seçenekleri**.

   ![lisansları için hizmet planları seçin](./media/licensing-groups-assign/assignment-options.png)
  
8. Atamayı tamamlamak için **Lisans ata** bölmesinin en altında bulunan **Ata**'ya tıklayın.

9. Bildirim durumu ve işleminin sonucunu gösteren sağ üst köşesinde görüntülenir. (Örneğin, nedeniyle önceden var olan lisans grubunda) grubuna ataması tamamlanamadı, hatanın ayrıntılarını görüntülemek için bildirime tıklayın.

Bir gruba lisans atadığınızda, Azure AD, bu grubun tüm mevcut üyelerin işler. Bu işlem ile grup boyutunu değişen biraz zaman alabilir. Sonraki adım, işlemin tamamlandığını doğrulayın ve daha fazla dikkat sorunlarını gidermek için gerekip gerekmediğini belirlemek açıklar.

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>2. Adım: İlk atama tamamlandığını doğrulama

1. Git **Azure Active Directory** > **grupları**. Lisans atanmış olan grubu seçin.

2. Grup bölmeden **lisansları**. Bu, tam olarak kullanıcıya lisans atandı ve içine bak gereken herhangi bir hata varsa hızla doğrulamanıza olanak sağlar. Aşağıdaki bilgiler kullanılabilir:

   - Gruba atanmış olan ürün lisansı listesi. Etkin belirli hizmetler göstermek ve değişiklik yapmak için bir giriş seçin.

   - Gruba (değişiklikler işlenir veya işleme için tüm kullanıcı üyeleri bitip bitmediğini) yapılan en son lisans değişiklikleri durumu.

   - Lisans atanamadı olduğundan bir hata durumunda onlara olan kullanıcılar hakkında bilgiler.

   ![Lisans hataları ve lisans durumu](./media/licensing-groups-assign/assignment-errors.png)

3. Daha ayrıntılı bilgi lisansı altında işleme hakkında **Azure Active Directory** > **kullanıcılar ve gruplar** > *grup adı*  >  **Denetim günlükleri**. Aşağıdaki etkinlikleri dikkat edin:

   - Etkinlik: **Kullanıcılara grup tabanlı lisans uygulama Başlat**. Sistem grupta lisans atama değişikliği alır ve tüm kullanıcı üyelerine uygulama başlatır, bu günlüğe kaydedilir. Yapılan değişiklikler hakkında bilgiler içerir.

   - Etkinlik: **Kullanıcılara grup tabanlı lisans uygulamayı sonlandırma**. Bu gruptaki tüm kullanıcılar işleme sistem sona erdiğinde günlüğe kaydedilir. Bu, kaç kullanıcının başarıyla işlendi ve kaç kullanıcının Grup lisansları atanamadı bir özetini içerir.

   [Bu bölümü okuyun](licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) grup tabanlı lisanslama tarafından yapılan değişiklikleri çözümlemek için denetim günlüklerini nasıl kullanılabileceği hakkında daha fazla bilgi için.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>3. Adım: İçin lisans sorunlarını denetleyin ve çözümleyin

1. Git **Azure Active Directory** > **grupları**ve lisansları atanmış olan Grup bulunamıyor.
2. Grup bölmeden **lisansları**. Bölmenin en üstünde bir bildirim için lisansı atanamıyor 10 kullanıcı olduğunu gösterir. Bu grup için lisans bir hata durumunda tüm kullanıcıların listesini görmek için açın.
3. **Başarısız atamalar** sütun söyler bize her iki ürün lisansları kullanıcılara atanan uygulanamadı. **İlk başarısızlık nedeni** hatanın nedenini sütun içerir. Bu durumda sahip **çakışan hizmet planları**.

   ![Lisans atanamadı](./media/licensing-groups-assign/failed-assignments.png)

4. Açmak için bir kullanıcı seçin **lisansları** bölmesi. Bu bölme, kullanıcıya atanmış olan tüm lisansları gösterir. Bu örnekte, devralındığı Office 365 Kurumsal E1 lisans kullanıcının **bilgi noktası kullanıcılar** grubu. Bu sistem, uygulamayı denedi E3 lisansı çakışıyor **ik departmanı** grubu. Sonuç olarak, bu gruptaki lisanslar hiçbiri kullanıcıya atandı.

   ![Bir kullanıcı için tüm lisans çakışmaları görüntülemek](./media/licensing-groups-assign/user-license-view.png)

5. Bu çakışmayı çözmek için kullanıcıyı kaldırmak **bilgi noktası kullanıcılar** grubu. Azure AD değişikliği işledikten sonra **ik departmanı** lisansları doğru şekilde atanır.

   ![Lisansları burada doğru atanan](./media/licensing-groups-assign/license-correctly-assigned.png)

## <a name="next-steps"></a>Sonraki adımlar

Grupları aracılığıyla lisans yönetimi için ayarlama özelliği hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Grup tabanlı Azure Active Directory lisansı nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](licensing-groups-resolve-problems.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](licensing-groups-migrate-users.md)
* [Kullanıcılar Azure Active Directory'de Grup tabanlı lisanslama kullanarak ürün lisansları arasında geçirme](licensing-groups-change-licenses.md)
* [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](../active-directory-licensing-group-advanced.md)
* [Azure Active Directory'de Grup tabanlı lisanslama için PowerShell örnekleri](licensing-ps-examples.md)
