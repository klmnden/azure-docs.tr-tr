---
title: Azure Güvenlik Merkezi, Kiracı genelinde görünürlük elde edin | Microsoft Docs
description: Azure Güvenlik Merkezi'nde Kiracı genelinde görünürlük elde etme hakkında bilgi edinin.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: b85c0e93-9982-48ad-b23f-53b367f22b10
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2018
ms.author: terrylan
ms.openlocfilehash: 800ec83b3599dba716e7a4a015b9b8c1745a0975
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144576"
---
# <a name="gain-tenant-wide-visibility-for-azure-security-center"></a>Azure Güvenlik Merkezi, Kiracı genelinde görünürlük elde edin
Bu makale Azure Güvenlik Merkezi sağladığı en üst düzeye çeşitli eylemler yaparak başlamanıza yardımcı olur. Bu eylemler gerçekleştirme birden çok güvenlik ilkeleri uygulayarak, kuruluşunuzun güvenlik duruşunu uygun ölçekte tüm Azure Active Directory kiracınız ve etkili bir şekilde bağlanan Azure aboneliklerini yönetme hakkında daha fazla görünürlük elde etmenizi sağlar Abonelikler aggregative bir biçimde.

## <a name="management-groups"></a>Yönetim grupları
Azure yönetim erişimi, ilkeleri ve abonelikleri gruplarında raporlama verimli bir şekilde yönetme olanağı sağlar, hem de kök yönetim grubu üzerinde eylemler gerçekleştirerek tüm Azure Emlak etkili bir şekilde yönetin. Her Azure AD kiracısı bir üst düzey yönetim grubunun kök yönetim grubu adı verilir. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Bu grup, genel ilkeler ve RBAC atamaları dizin düzeyinde uygulanmasına olanak sağlar. 

Aşağıdaki eylemlerden herhangi birini gerçekleştirirken kök yönetim grubu otomatik olarak oluşturulur: 
1. Azure Yönetim grupları giderek kullanmak için kabul **Yönetim grupları** içinde [Azure portalında](https://portal.azure.com).
2. Bir API çağrısı aracılığıyla bir yönetim grubu oluşturun.
3. PowerShell ile bir yönetim grubu oluşturun.

Yönetim gruplarına ayrıntılı bir genel bakış için bkz: [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../azure-resource-manager/management-groups-overview.md) makalesi.

## <a name="create-a-management-group-in-the-azure-portal"></a>Azure portalını kullanarak bir yönetim grubu oluşturma
Yönetim gruplarına abonelikleri düzenleyebilir ve idare ilkelerini uygulamak yönetim gruplarına. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan ilkeleri devralır. Yönetim grupları için yerleşik Güvenlik Merkezi gerekli değildir, ancak kök yönetim grubu oluşmasını en az bir yönetim grubu oluşturmanız önerilir. Grup oluşturulduktan sonra Azure AD kiracınıza altındaki tüm abonelikler için bağlanır. Yönergeler için PowerShell ve daha fazla bilgi için bkz. [kaynak ve Kuruluş Yönetimi yönetim grupları oluşturma](../azure-resource-manager/management-groups-create.md).

 
1. [Azure Portal](http://portal.azure.com)’da oturum açın.
2. Seçin **tüm hizmetleri** > **Yönetim grupları**.
3. Ana sayfasında göze seçin **yeni yönetim grubu.** 

    ![Ana Grup](./media/security-center-management-groups/main.png) 
4.  Yönetim grubu kimliği alanını doldurun. 
    - **Yönetim grubu Tanıtıcısı** bu yönetim grubunda komutları göndermek için kullanılan dizini benzersiz tanımlayıcısı. Bu tanımlayıcı, Azure sistem genelinde bu grubu tanımlamak için kullanılan oluşturulduktan sonra düzenlenebilir değildir. 
    - Görünen ad alanı, Azure portalında görüntülenen addır. Yönetim oluştururken ayrı görünen ad isteğe bağlı bir alan olan Grup ve herhangi bir zamanda değiştirilebilir.  

      ![Oluştur](./media/security-center-management-groups/create_context_menu.png)  
5.  Seçin **Kaydet**

### <a name="view-management-groups-in-the-azure-portal"></a>Azure portalında Yönetim grupları görüntüleyin
1. Oturum [Azure portalında](http://portal.azure.com).
2. Yönetim gruplarını görüntülemek için seçin **tüm hizmetleri** Azure ana menüsünden altında.
3. Altında **genel**seçin **Yönetim grupları**.

    ![Bir yönetim grubu oluşturma](./media/security-center-management-groups/all-services.png)

## <a name="grant-tenant-level-visibility-and-the-ability-to-assign-policies"></a>Kiracı düzeyinde görünürlüğe ve ilkeleri atama olanağı verin

Azure AD kiracısına kayıtlı tüm abonelikleri güvenlik duruşunu içine görünürlük elde etmek için yeterli Okuma izinleri olan bir RBAC rolü kök yönetim grubu üzerinde atanacak gereklidir.

### <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>İçin Azure Active Directory'de genel yönetici erişimini yükseltme
Bir Azure Active Directory Kiracı Yöneticisi, Azure abonelikleri doğrudan erişimi yok. Ancak, bir dizin Yöneticisi olarak, kendilerini erişimine sahip bir rol için yükseltme hakkı sahiptirler. RBAC rollerini atayabilmeniz için kök yönetim grubu düzeyinde kullanıcı erişimi Yöneticisi kendisini yükseltmek bir Azure AD Kiracı Yöneticisi gerekir. PowerShell yönergeleri ve ek bilgi için bkz. [için Azure Active Directory'de genel yönetici erişimini yükseltme](../role-based-access-control/elevate-access-global-admin.md). 


1. Oturum [Azure portalında](https://portal.azure.com) veya [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com).

2. Gezinti listesinde **Azure Active Directory** ve ardından **özellikleri**.

   ![Azure AD özellikleri - ekran görüntüsü](./media/security-center-management-groups/aad-properties.png)

3. Altında **genel yönetici Azure aboneliklerini ve Yönetim gruplarını yönetebilir**, anahtar kümesine **Evet**.

   ![Genel yönetici Azure aboneliklerini ve Yönetim gruplarını - ekran görüntüsü yönetebilir.](./media/security-center-management-groups/aad-properties-global-admin-setting.png)

   - Anahtar ayarlandığında **Evet**, Azure RBAC kök kapsamda kullanıcı erişimi yöneticisi rolü genel yönetici hesabınızın (şu anda oturum açmış kullanıcı olarak) eklenir (`/`), size erişim görünümünü ve raporunu için hangi verir Tüm Azure abonelikleri Azure AD kiracınız ile ilişkili.

   - Anahtar ayarlandığında **Hayır**, Azure RBAC kullanıcı erişimi yöneticisi rolü genel yönetici hesabınızın (şu anda oturum açmış kullanıcı olarak) kaldırılır. Azure AD kiracısı ile ilişkili tüm Azure abonelikleri göremez ve görüntüleyebilir ve yalnızca erişim için verilmiş Azure aboneliklerini yönetme.

4. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

    - Bu ayar, genel bir özellik değildir ve yalnızca şu anda oturum açmış kullanıcı için geçerlidir.

5. Yükseltilmiş erişim sağlamak için gereken görevleri yapın. İşiniz bittiğinde, anahtar kümesi geri **Hayır**.

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

### <a name="assign-rbac-roles-to-users"></a>RBAC rolleri, kullanıcılara ata
Kiracı Yöneticisi erişim yükselten sonra kök yönetim grubu düzeyinde ilgili kullanıcılar için RBAC rolü atayabilirsiniz. Önerilen rol atamak için [ **okuyucu**](../role-based-access-control/built-in-roles.md#reader). Bu rol, Kiracı düzeyi görünürlük sağlamak için gereklidir. Atanan rolü, tüm Yönetim gruplarını ve kök yönetim grubundaki abonelikleri otomatik olarak yayılır. RBAC rolleri hakkında daha fazla bilgi için bkz. [kullanılabilir roller](../active-directory/users-groups-roles/directory-assign-admin-roles.md#available-roles). 

1. [Azure PowerShell](/powershell/azure/install-azurerm-ps)'i yükleyin.
2. Aşağıdaki komutları çalıştırın: 

    ```azurepowershell
    # Install Management Groups Powershell module
    Install-Module AzureRM.Resources
    
    # Login to Azure as a Global Administrator user
    Login-AzureRmAccount
    ```

3. İstendiğinde, genel yönetici kimlik bilgilerinizle oturum açın. 

    ![Oturum açma komut istemi ekran görüntüsü](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Aşağıdaki komutu çalıştırarak, okuyucu rol izinleri verin:

    ```azurepowershell
    # Add Reader role to the required user on the Root Management Group
    # Replace "user@domian.com” with the user to grant access to
    New-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/"
    ```
5. Rolü kaldırmak için aşağıdaki komutu kullanın: 

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/" 
    ```

<!-- Currently, PowerShell method only 6/26/18

1. Sign in to the [Azure portal](https://portal.azure.com). 
2. To view management groups, select **All services** under the Azure main menu then select **Management Groups**.
3.  Select a management group and click **details**.

    ![Management Groups details screenshot](./media/security-center-management-groups/management-group-details.PNG)
 
4. Click **Access control (IAM)** then **Add**.
5. Select the role to assign and the user, then click **Save**.  
   
   ![Add Security Reader role screenshot](./media/security-center-management-groups/asc-security-reader.png)
-->

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

