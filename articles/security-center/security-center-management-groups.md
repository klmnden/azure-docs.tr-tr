---
title: Azure Güvenlik Merkezi, Kiracı genelinde görünürlük elde edin | Microsoft Docs
description: Azure Güvenlik Merkezi'nde Kiracı genelinde görünürlük elde etme hakkında bilgi edinin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: b85c0e93-9982-48ad-b23f-53b367f22b10
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2018
ms.author: rkarlin
ms.openlocfilehash: 7e26dc37c5c4f85e3db634bd961bf9308e418a03
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59045773"
---
# <a name="gain-tenant-wide-visibility-for-azure-security-center"></a>Azure Güvenlik Merkezi, Kiracı genelinde görünürlük elde edin
Bu makale Azure Güvenlik Merkezi sağladığı en üst düzeye çeşitli eylemler yaparak başlamanıza yardımcı olur. Bu eylemler gerçekleştirme birden çok güvenlik ilkeleri uygulayarak, kuruluşunuzun güvenlik duruşunu uygun ölçekte tüm Azure Active Directory kiracınız ve etkili bir şekilde bağlanan Azure aboneliklerini yönetme hakkında daha fazla görünürlük elde etmenizi sağlar Abonelikler aggregative bir biçimde.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="management-groups"></a>Yönetim grupları
Azure yönetim erişimi, ilkeleri ve abonelikleri gruplarında raporlama verimli bir şekilde yönetme olanağı sağlar, hem de kök yönetim grubu üzerinde eylemler gerçekleştirerek tüm Azure Emlak etkili bir şekilde yönetin. Her Azure AD kiracısı bir üst düzey yönetim grubunun kök yönetim grubu adı verilir. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Bu grup, genel ilkeler ve RBAC atamaları dizin düzeyinde uygulanmasına olanak sağlar. 

Aşağıdaki eylemlerden herhangi birini gerçekleştirirken kök yönetim grubu otomatik olarak oluşturulur: 
1. Azure Yönetim grupları giderek kullanmak için kabul **Yönetim grupları** içinde [Azure portalında](https://portal.azure.com).
2. Bir API çağrısı aracılığıyla bir yönetim grubu oluşturun.
3. PowerShell ile bir yönetim grubu oluşturun.

Yönetim gruplarına ayrıntılı bir genel bakış için bkz: [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../azure-resource-manager/management-groups-overview.md) makalesi.

## <a name="create-a-management-group-in-the-azure-portal"></a>Azure portalını kullanarak bir yönetim grubu oluşturma
Yönetim gruplarına abonelikleri düzenleyebilir ve idare ilkelerini uygulamak yönetim gruplarına. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan ilkeleri devralır. Yönetim grupları için yerleşik Güvenlik Merkezi gerekli değildir, ancak kök yönetim grubu oluşmasını en az bir yönetim grubu oluşturmanız önerilir. Grup oluşturulduktan sonra Azure AD kiracınıza altındaki tüm abonelikler için bağlanır. Yönergeler için PowerShell ve daha fazla bilgi için bkz. [kaynak ve Kuruluş Yönetimi yönetim grupları oluşturma](../azure-resource-manager/management-groups-create.md).

 
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **Yönetim grupları**.
3. Ana sayfasında göze seçin **yeni yönetim grubu.** 

    ![Ana Grup](./media/security-center-management-groups/main.png) 
4.  Yönetim grubu kimliği alanını doldurun. 
    - **Yönetim grubu Tanıtıcısı** bu yönetim grubunda komutları göndermek için kullanılan dizini benzersiz tanımlayıcısı. Bu tanımlayıcı, Azure sistem genelinde bu grubu tanımlamak için kullanılan oluşturulduktan sonra düzenlenebilir değildir. 
    - Görünen ad alanı, Azure portalında görüntülenen addır. Yönetim oluştururken ayrı görünen ad isteğe bağlı bir alan olan Grup ve herhangi bir zamanda değiştirilebilir.  

      ![Oluştur](./media/security-center-management-groups/create_context_menu.png)  
5.  Seçin **Kaydet**

### <a name="view-management-groups-in-the-azure-portal"></a>Azure portalında Yönetim grupları görüntüleyin
1. Oturum [Azure portalında](https://portal.azure.com).
2. Yönetim gruplarını görüntülemek için seçin **tüm hizmetleri** Azure ana menüsünden altında.
3. Altında **genel**seçin **Yönetim grupları**.

    ![Yönetim grubu oluşturma](./media/security-center-management-groups/all-services.png)

## <a name="grant-tenant-level-visibility-and-the-ability-to-assign-policies"></a>Kiracı düzeyinde görünürlüğe ve ilkeleri atama olanağı verin

Azure AD kiracısına kayıtlı tüm abonelikleri güvenlik duruşunu içine görünürlük elde etmek için yeterli Okuma izinleri olan bir RBAC rolü kök yönetim grubu üzerinde atanacak gereklidir.

### <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>İçin Azure Active Directory'de genel yönetici erişimini yükseltme
Bir Azure Active Directory Kiracı Yöneticisi, Azure abonelikleri doğrudan erişimi yok. Ancak, bir dizin Yöneticisi olarak, kendilerini erişimine sahip bir rol için yükseltme hakkı sahiptirler. RBAC rollerini atayabilmeniz için kök yönetim grubu düzeyinde kullanıcı erişimi Yöneticisi kendisini yükseltmek bir Azure AD Kiracı Yöneticisi gerekir. PowerShell yönergeleri ve ek bilgi için bkz. [için Azure Active Directory'de genel yönetici erişimini yükseltme](../role-based-access-control/elevate-access-global-admin.md). 


1. Oturum [Azure portalında](https://portal.azure.com) veya [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com).

2. Gezinti listesinde **Azure Active Directory** ve ardından **özellikleri**.

   ![Azure AD özellikleri - ekran görüntüsü](./media/security-center-management-groups/aad-properties.png)

3. Altında **Azure kaynakları için Access management**, anahtar kümesine **Evet**.

   ![Genel yönetici Azure aboneliklerini ve Yönetim gruplarını - ekran görüntüsü yönetebilir.](./media/security-center-management-groups/aad-properties-global-admin-setting.png)

   - Anahtar Evet olarak ayarlarsanız, Azure RBAC (/) kök kapsamda kullanıcı erişimi yöneticisi rolü atanır. Bu, tüm Azure abonelikleri ve bu Azure AD dizini ile ilişkili yönetim gruplarını rol atama izni verir. Bu anahtar yalnızca, Azure AD'de genel Yönetici rolüne atanan kullanıcılar için kullanılabilir.

   - Anahtar Hayır olarak ayarlanırsa, kullanıcı erişimi yöneticisi rolü Azure RBAC kullanıcı hesabınızdan kaldırıldı. Artık tüm Azure abonelikleri ve bu Azure AD dizini ile ilişkili olan Yönetim grupları roller atayabilirsiniz. Görüntüleyebilir ve yalnızca Azure aboneliklerini ve yönetim gruplarına erişim için verilmiş yönetin.

4. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

    - Bu ayar, genel bir özellik değildir ve yalnızca şu anda oturum açmış kullanıcı için geçerlidir.

5. Yükseltilmiş erişim sağlamak için gereken görevleri gerçekleştirin. İşiniz bittiğinde, anahtar kümesi geri **Hayır**.


### <a name="assign-rbac-roles-to-users"></a>RBAC rolleri, kullanıcılara ata
Tüm abonelikler için görünürlük kazanmak için Kiracı yöneticileri kök yönetim grubu düzeyinde kendilerine dahil, Kiracı genelinde görünürlük vermek istediğiniz tüm kullanıcılara uygun RBAC rolü atamanız gerekir. Önerilen rol atamak için kullanılan **Güvenlik Yöneticisi** veya **güvenlik okuyucusu**. Genellikle, güvenlik yöneticisi rolü güvenlik okuyucusu Kiracı düzeyinde görünürlük sağlamak için yeterli olacaktır ancak kök düzeyi, ilkeleri uygulamak için gereklidir. Bu rolleri tarafından verilen izinler hakkında daha fazla bilgi için bkz: [Güvenlik Yöneticisi yerleşik rol açıklamasını](../role-based-access-control/built-in-roles.md#security-admin) veya [güvenlik okuyucusu yerleşik rol açıklamasını](../role-based-access-control/built-in-roles.md#security-reader).


#### <a name="assign-rbac-roles-to-users-through-the-azure-portal"></a>Azure portalı üzerinden kullanıcılara RBAC rollerini atayın: 

1. [Azure Portal](https://portal.azure.com) oturum açın. 
1. Yönetim gruplarını görüntülemek için seçin **tüm hizmetleri** Azure ana menüsünde seçip **Yönetim grupları**.
1.  Bir yönetim grubu seçip tıklayın **ayrıntıları**.

    ![Yönetim grupları ayrıntıları ekran görüntüsü](./media/security-center-management-groups/management-group-details.PNG)
 
1. Tıklayın **erişim denetimi (IAM)** ardından **rol atamaları**.

1. Tıklayın **rol ataması Ekle**.

1. Atamak için rol ve kullanıcı seçin ve ardından tıklayın **Kaydet**.  
   
   ![Güvenlik okuyucusu rolü ekran görüntüsü Ekle](./media/security-center-management-groups/asc-security-reader.png)


#### <a name="assign-rbac-roles-to-users-with-powershell"></a>Kullanıcılara PowerShell ile RBAC rollerini atayın: 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. [Azure PowerShell](/powershell/azure/install-az-ps)'i yükleyin.
2. Aşağıdaki komutları çalıştırın: 

    ```azurepowershell
    # Login to Azure as a Global Administrator user
    Connect-AzAccount
    ```

3. İstendiğinde, genel yönetici kimlik bilgilerinizle oturum açın. 

    ![Oturum açma komut istemi ekran görüntüsü](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Aşağıdaki komutu çalıştırarak, okuyucu rol izinleri verin:

    ```azurepowershell
    # Add Reader role to the required user on the Root Management Group
    # Replace "user@domian.com” with the user to grant access to
    New-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/"
    ```
5. Rolü kaldırmak için aşağıdaki komutu kullanın: 

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/" 
    ```

### <a name="open-or-refresh-security-center"></a>Açın veya Güvenlik Merkezi yenileyin
Erişim yükseltilmiş sonra açın veya Azure AD kiracınıza tüm abonelikler görünürlük olduğunu doğrulamak için Azure Güvenlik Merkezi yenileyin. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Güvenlik Merkezi'nde görüntülemek istediğiniz abonelik Seçici içinde tüm abonelikleri seçtiğinizden emin olun.

    ![Abonelik Seçici ekran görüntüsü](./media/security-center-management-groups/subscription-selector.png)

1. Seçin **tüm hizmetleri** Azure ana menüsünde seçip **Güvenlik Merkezi**.
2. İçinde **genel bakış**, bir abonelik kapsamı grafik yok.

    ![Abonelik kapsamı grafiği ekran görüntüsü](./media/security-center-management-groups/security-center-subscription-coverage.png)

3. Tıklayarak **kapsamı** kapsamdaki Aboneliklerin listesini görmek için. 

    ![Abonelik kapsamı listesi ekran görüntüsü](./media/security-center-management-groups/security-center-coverage.png)

### <a name="remove-elevated-access"></a>Yükseltilmiş erişimi Kaldır 
Kullanıcılar için RBAC rollerini kendilerine atandıktan sonra Kiracı Yöneticisi kendi kullanıcı erişimi yöneticisi rolü ' kaldırmanız gerekir.

1. Oturum [Azure portalında](https://portal.azure.com) veya [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com).

2. Gezinti listesinde **Azure Active Directory** ve ardından **özellikleri**.

3. Altında **genel yönetici Azure aboneliklerini ve Yönetim gruplarını yönetebilir**, anahtar kümesine **Hayır**.

4. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.



## <a name="adding-subscriptions-to-a-management-groups"></a>Bir yönetim grupları için abonelikler ekleniyor
Abonelikler, oluşturduğunuz yönetim grubuna ekleyebilirsiniz. Bu adımlar, Kiracı genelinde görünürlük ve genel İlkesi ve erişim yönetimi kazanmak için zorunlu değildir.

1. Altında **Yönetim grupları**, aboneliğinize eklemek için bir yönetim grubu seçin.

    ![Aboneliği eklemek için bir yönetim grubu seçin](./media/security-center-management-groups/management-group-subscriptions.png)

2. Seçin **ekleme mevcut**.

    ![Var olanı ekle](./media/security-center-management-groups/add-existing.png)

3. Abonelik altında girin **var olan bir kaynak ekleyin** tıklatıp **Kaydet**.

4. Kapsamı içinde tüm abonelikleri ekleyene kadar 1 ile 3 arasındaki adımları yineleyin.

   > [!NOTE]
   > Yönetim grupları hem abonelik hem de alt Yönetim grupları içerebilir. Üst yönetim grubuna RBAC rolü bir kullanıcıya atadığınızda, erişim alt yönetim grubunun abonelik tarafından devralınır. Üst yönetim grubu ayarlama ilkeleri de alt gruplar tarafından devralınır. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi için bir kiracı genelinde görünürlük elde öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md)

> [!div class="nextstepaction"]
> [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md)

