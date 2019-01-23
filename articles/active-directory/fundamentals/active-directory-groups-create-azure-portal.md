---
title: Temel bir grup oluşturma ve üye - Azure Active Directory'ye ekleme | Microsoft Docs
description: Azure Active Directory'yi kullanarak temel bir grup oluşturma hakkında yönergeler.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/22/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.openlocfilehash: 50900bb58fdec909a5922e0e33873a97ce2a72dc
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54448831"
---
# <a name="create-a-basic-group-and-add-members-using-azure-active-directory"></a>Temel bir grup oluşturma ve Azure Active Directory'yi kullanarak üye ekleme
Azure Active Directory (Azure AD) portalını kullanarak temel bir grup oluşturabilirsiniz. Bu makalenin amaçları doğrultusunda, kaynak sahibi (yönetici) tarafından tek bir kaynağa temel bir grup eklenir ve bu grup, o kaynağa erişmesi gereken belirli üyeleri (çalışanlar) içerir. Dinamik üyelikler ve kural oluşturma da dahil olmak üzere daha karmaşık senaryolar için bkz. [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).

## <a name="create-a-basic-group-and-add-members"></a>Temel bir grup oluşturma ve üye ekleme
Temel bir grup oluşturabilir ve aynı anda üyelerinizi ekleyebilirsiniz.

### <a name="to-create-a-basic-group-and-add-members"></a>Temel bir grup oluşturmak ve üye eklemek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**’yi, **Gruplar**’ı ve ardından **Yeni grup**’u seçin.

    ![Gruplar ile birlikte gösterilen Azure AD](media/active-directory-groups-create-azure-portal/group-full-screen.png)

3. **Grup** sayfasında gerekli bilgileri doldurun.

    ![Örnek bilgileriyle doldurulmuş şekilde yeni grup sayfası](media/active-directory-groups-create-azure-portal/new-group-blade.png)

    - **Grup türü (gerekli).** Önceden tanımlanmış bir grup türü seçin. Buna aşağıdakiler dahildir:
        
        - **Güvenlik**. Bir kullanıcı grubu için paylaşılan kaynaklara üye ve bilgisayar erişimini yönetmek için kullanılır. Örneğin, belirli bir güvenlik ilkesi için bir güvenlik grubu oluşturabilirsiniz. Böylece, her bir üyeye ayrı ayrı izin eklemek zorunda kalmadan aynı anda tüm üyelere bir dizi izin verebilirsiniz. Kaynaklara erişimi yönetme hakkında daha fazla bilgi için bkz. [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md).
        
        - **Office 365**. Üyelerin paylaşılan posta kutusuna, takvime, takvime, dosyalara, SharePoint sitesine vb.’ye erişmesini sağlayarak işbirliği fırsatları sunar. Bu seçenek, kuruluşunuzun dışındaki kişilerin de gruba erişmesini sağlar. Office 365 Grupları hakkında daha fazla bilgi için bkz. [Office 365 Grupları hakkında bilgi edinin](https://support.office.com/article/learn-about-office-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2).

    - **Grup adı (gerekli).** Grup için unutmayacağınız ve mantıklı olan bir ad ekler.

    - **Grup açıklaması.** Grubunuza isteğe bağlı bir açıklama ekler.

    - **Üyelik türü (gerekli).** Önceden tanımlanmış bir üyelik türü seçin. Buna aşağıdakiler dahildir:

        - **Atanan.** Bu grubun üyesi olacak ve benzersiz izinlere sahip olacak şekilde belirli kullanıcıları eklemenize olanak sağlar. Bu makalenin amaçları doğrultusunda, bu seçeneği kullanıyoruz.

        - **Dinamik kullanıcı.** Otomatik olarak üyeler eklemek ve kaldırmak için dinamik grup kuralları kullanmanıza olanak sağlar. Bir üyenin öznitelikleri değişirse sistem, üyenin kural gereksinimlerini karşıladığını mı (eklendiğini) yoksa artık kural gereksinimlerini karşılamadığını mı (kaldırıldığını) görmek amacıyla dizin için dinamik grup kurallarınıza bakar.

        - **Dinamik cihaz.** Otomatik olarak cihazlar eklemek ve kaldırmak için dinamik grup kuralları kullanmanıza olanak sağlar. Bir cihazın öznitelikleri değişirse sistem, cihazın kural gereksinimlerini karşıladığını mı (eklendiğini) yoksa artık kural gereksinimlerini karşılamadığını mı (kaldırıldığını) görmek amacıyla dizin için dinamik grup kurallarınıza bakar.

        >[!Important]
        >Ya cihazlar ya da kullanıcılar için bir dinamik grup oluşturabilirsiniz, her ikisi için oluşturamazsınız. Ayrıca cihaz sahiplerinin özniteliklerine göre de bir cihaz grubu oluşturamazsınız. Cihaz üyeliği kuralları yalnızca cihaz ilişkilendirmesine başvurabilir. Kullanıcılar ve cihazlar için dinamik bir grup oluşturma hakkında daha fazla bilgi için bkz. [dinamik bir grup oluşturun ve durumunu denetlemek](../users-groups-roles/groups-create-rule.md).

4. **Oluştur**’u seçin.

    Grubunuz oluşturulur ve üyeler eklemeniz için hazır olur.

5. **Grup** sayfasından **Üyeler** alanını seçin ve sonra **Üyeleri seç** sayfasından grubunuza eklenecek üyeleri aramaya başlayın.

    ![Grup oluşturma işlemi sırasında grubunuz için üyeleri seçme](media/active-directory-groups-create-azure-portal/select-members-create-group.png)

6. Üye ekleme işleminiz bittiğinde **Seç** öğesini seçin.

    **Gruba Genel Bakış** sayfası, şimdi gruba eklenen üye sayısını gösterecek şekilde güncelleştirilir.

    ![Üye sayısı vurgulanmış şekilde Gruba Genel Bakış sayfası](media/active-directory-groups-create-azure-portal/group-overview-blade-number-highlight.png)

## <a name="next-steps"></a>Sonraki adımlar
Şimdi bir grup ve en az bir kullanıcı eklediğinize göre artık aşağıdakileri yapabilirsiniz:

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Grup üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)

- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-create-rule.md)

- [Grup ayarlarınızı düzenleme](active-directory-groups-settings-azure-portal.md)

- [Grupları kullanarak kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Grupları kullanarak SaaS uygulamalarına erişimi yönetme](../users-groups-roles/groups-saasapps.md)

- [PowerShell komutlarını kullanarak grupları yönetme](../users-groups-roles/groups-settings-v2-cmdlets.md)

- [Azure Active Directory’ye bir Azure aboneliğini ekleme veya ilişkilendirme](active-directory-how-subscriptions-associated-directory.md)
