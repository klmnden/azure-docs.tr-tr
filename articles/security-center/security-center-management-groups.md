---
title: Azure Güvenlik Merkezi için Kiracı çapında görünürlük kazanmak | Microsoft Docs
description: Azure Güvenlik Merkezi'nde Kiracı çapında görünürlük elde hakkında bilgi edinin.
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
ms.date: 06/22/2018
ms.author: terrylan
ms.openlocfilehash: 2c95b06ce34b850d1bfaf60e47d6e5fede148a38
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025947"
---
# <a name="gain-tenant-wide-visibility-for-azure-security-center"></a>Azure Güvenlik Merkezi için Kiracı çapında görünürlük kazanmak
Bu makalede Azure Güvenlik Merkezi sağlar yararlarını en üst düzeye çeşitli eylemler yaparak başlamanıza yardımcı olur. Bu eylemler gerçekleştirme birden çok güvenlik ilkelerini uygulayarak görünürlük tüm Azure Active Directory kiracınızın ve etkili bir şekilde bağlı Azure Aboneliklerini yönetmek, kuruluşunuzun güvenlik tutumunu ölçekte görmenizi sağlar Abonelikler aggregative bir biçimde.

## <a name="management-groups"></a>Yönetim grupları
Azure Yönetim grupları verimli bir şekilde erişim, ilkeleri ve abonelikleri gruplarında raporlama yönetme olanağı sağlar, aynı zamanda kök yönetim grubunda eylemleri gerçekleştirerek tüm Azure varlıklarının etkili bir şekilde yönetmek. Her Azure AD kiracısı kök yönetim grubu olarak adlandırılan tek bir üst düzey yönetim grubu verilir. Bu kök yönetim grubunun tüm Yönetim gruplarını sağlamak için hiyerarşiye oluşturulur ve abonelikleri Katlama. Bu grup, genel ilkeler ve dizin düzeyinde uygulanması için RBAC atamalarını sağlar. 

Aşağıdaki eylemlerden herhangi birini yaptığınızda kök yönetim grubu otomatik olarak oluşturulur: 
1. Azure Yönetim grupları giderek kullanmak için kabul **Yönetim grupları** içinde [Azure portal](https://portal.azure.com).
2. Bir API çağrısı aracılığıyla bir yönetim grubu oluşturun.
3. PowerShell ile bir yönetim grubu oluşturun.

Yönetim grupları ayrıntılı bir genel bakış için bkz: [kaynaklarınızı Azure Yönetim grupları ile düzenleme](../azure-resource-manager/management-groups-overview.md) makalesi.

## <a name="create-a-management-group-in-the-azure-portal"></a>Azure portalında bir yönetim grubu oluşturma
Abonelik Yönetimi gruplar halinde düzenlemek ve yönetim gruplarına idare ilkelerini uygulayın. Bir yönetim grubundaki tüm abonelikleri otomatik olarak yönetim grubuna uygulanan ilkeler devralır. Yönetim grupları için yerleşik Güvenlik Merkezi gerekli değildir, ancak kök yönetim grubu oluşmasını en az bir yönetim grubu oluşturmanız önerilir. Grup oluşturulduktan sonra Azure AD kiracınıza altındaki tüm abonelikleri bağlanır. PowerShell ve daha fazla bilgi için yönergeler için bkz: [kaynak ve Kuruluş Yönetimi yönetim grupları oluşturmak](../azure-resource-manager/management-groups-create.md).

 
1. [Azure Portal](http://portal.azure.com)’da oturum açın.
2. Seçin **tüm hizmetleri** > **Yönetim grupları**.
3. Ana sayfada seçin **yeni yönetim grubu.** 

    ![Ana grubu](./media/security-center-management-groups/main.png) 
4.  Yönetim grubu kimliği alanını doldurun. 
    - **Yönetim grubu Tanıtıcısı** bu yönetim grubu komutlarını göndermek için kullanılan dizini benzersiz tanımlayıcısı değil. Bu grup tanımlamak için Azure sistem genelinde kullanıldığı şekilde bu tanımlayıcı oluşturulduktan sonra düzenlenebilir değil. 
    - Görünen ad alanı Azure portalı içinde görüntülenen addır. Yönetim oluştururken, isteğe bağlı bir alan ayrı görünen adı olduğu grup ve herhangi bir zamanda değiştirilebilir.  

      ![Oluştur](./media/security-center-management-groups/create_context_menu.png)  
5.  Seçin **Kaydet**

### <a name="view-management-groups-in-the-azure-portal"></a>Azure portalındaki Yönetim grupları görüntüle
1. Oturum [Azure portal](http://portal.azure.com).
2. Yönetim gruplarını görüntülemek için seçin **tüm hizmetleri** Azure ana menü altında.
3. Altında **genel**seçin **Yönetim grupları**.

    ![Bir yönetim grubu oluştur](./media/security-center-management-groups/all-services.png)

## <a name="grant-tenant-level-visibility-and-the-ability-to-assign-policies"></a>Kiracı düzeyinde görünürlüğe ve ilkeleri atama olanağı vermek

Azure AD kiracısı'nda kayıtlı tüm abonelikleri güvenlik duruşunu içine görünürlük elde etmek için yeterli Okuma izinlerine sahip bir RBAC rolü kök yönetim grubunda atanacak gereklidir.

### <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Azure Active Directory'de genel bir yöneticinin erişimini yükseltme
Azure Active Directory Kiracı yönetici Azure abonelikleri için doğrudan erişime sahip değil. Ancak, bir dizinin Yöneticisi olarak, bunlar kendilerini erişimine sahip bir role yükseltmesine hakkına sahip olmanız. Azure AD Kiracı yönetici RBAC rol atamanıza kendisini kullanıcı erişimi Yöneticisi kök yönetim grubu düzeyinde yükseltebilirler gerekiyor. PowerShell yönergeleri ve ek bilgi için bkz: [Azure Active Directory'de genel bir yöneticinin erişimini yükseltme](../role-based-access-control/elevate-access-global-admin.md). 


1. Oturum [Azure portal](https://portal.azure.com) veya [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com).

2. Gezinti listesinde tıklayın **Azure Active Directory** ve ardından **özellikleri**.

   ![Azure AD özellikleri - ekran görüntüsü](./media/security-center-management-groups/aad-properties.png)

3. Altında **genel yönetici Azure abonelikleri ve Yönetim gruplarını yönetebilir**, anahtar kümesine **Evet**.

   ![Genel yönetici Azure abonelikleri ve Yönetim grupları - ekran görüntüsü yönetebilir](./media/security-center-management-groups/aad-properties-global-admin-setting.png)

   - Anahtar ayarlandığında **Evet**, genel Yönetici hesabınızla (şu anda oturum açmış kullanıcı) Azure RBAC kök kapsamda kullanıcı erişimi Yöneticisi rolüne eklenir (`/`), size erişim Görünüm ve rapor üzerinde hangi verir Azure AD Kiracı ile ilişkilendirilen tüm Azure abonelikleri.

   - Anahtar ayarlandığında **Hayır**, genel Yönetici hesabınızla (şu anda oturum açmış kullanıcı) Azure RBAC kullanıcı erişimi Yöneticisi rolünde kaldırılır. Azure AD Kiracı ile ilişkilendirilen tüm Azure abonelikleri göremez ve görüntülemek ve yalnızca erişim için verilmiş Azure Aboneliklerini yönetmek.

4. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

    - Bu ayar, genel bir özellik değildir ve yalnızca şu anda oturum açmış kullanıcı için geçerlidir.

5. Yükseltilmiş erişim sağlamak için gereken görevler yapın. İşiniz bittiğinde, anahtar kümesi geri **Hayır**.

### <a name="assign-rbac-roles-to-users"></a>Kullanıcılara RBAC roller atama
Kiracı Yöneticisi erişim yükselten sonra kök yönetim grubu düzeyinde ilgili kullanıcılar için RBAC rolü atayabilirsiniz. Atamak için önerilen rolü [ **okuyucu**](../role-based-access-control/built-in-roles.md#reader). Bu rol, Kiracı düzeyinde görünürlüğe sağlamak için gereklidir. Atanan rolü otomatik olarak tüm yönetim gruplarına ve kök yönetim grubundaki abonelikleri yayılır. RBAC rolleri hakkında daha fazla bilgi için bkz: [kullanılabilir roller](../active-directory/active-directory-assign-admin-roles-azure-portal.md#available-roles).

1. [Azure PowerShell](/powershell/azure/install-azurerm-ps)'i yükleyin.
2. Aşağıdaki komutları çalıştırın: 

    ```azurepowershell
    # Install Management Groups Powershell module
    Install-Module AzureRM.Resources
    
    # Login to Azure as a Global Administrator user
    Login-AzureRmAccount
    ```

3. İstendiğinde, genel yönetici kimlik bilgilerinizle oturum açın. 

    ![Komut istemi ekran görüntüsünde oturum](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Okuyucu rolü aşağıdaki komutu çalıştırarak izinleri:

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
Kullanıcılar için RBAC rolleri üzere atadıktan sonra Kiracı yönetici kendi kullanıcı erişimi yöneticisi rolünden kaldırmanız gerekir.

1. Oturum [Azure portal](https://portal.azure.com) veya [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com).

2. Gezinti listesinde tıklayın **Azure Active Directory** ve ardından **özellikleri**.

3. Altında **genel yönetici Azure abonelikleri ve Yönetim gruplarını yönetebilir**, anahtar kümesine **Hayır**.

4. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

### <a name="open-or-refresh-security-center"></a>Açın veya Güvenlik Merkezi yenileyin
RBAC rolleri atadıktan sonra açın veya Azure AD kiracınıza tüm abonelikleri görünürlüğe sahip doğrulamak için Azure Güvenlik Merkezi yenileyin. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Seçin **tüm hizmetleri** Azure ana menüsündeki seçip **Güvenlik Merkezi**.
3. İçinde **genel bakış**, bir abonelik kapsamı grafik yoktur. 
    ![Abonelik kapsamı grafik ekran görüntüsü](./media/security-center-management-groups/security-center-subscription-coverage.png)
4. Tıklayın **kapsamı** kapsamdaki Aboneliklerin listesini görmek için. 
    ![Abonelik kapsamı listesi ekran görüntüsü](./media/security-center-management-groups/security-center-coverage.png)

## <a name="adding-subscriptions-to-a-management-groups"></a>Yönetim grupları için abonelikler ekleniyor
Abonelikler, oluşturduğunuz yönetim grubuna ekleyebilirsiniz. Bu adımları Kiracı çapında görünürlük ve genel İlkesi ve erişim yönetimi kazanmak için zorunlu değildir.

1. Altında **Yönetim grupları**, aboneliğinize eklemek için bir yönetim grubu seçin.

    ![Aboneliğe eklemek için bir yönetim grubu seçin](./media/security-center-management-groups/management-group-subscriptions.png)

2. Seçin **ekleme varolan**.

    ![Var olanı ekle](./media/security-center-management-groups/add-existing.png)

3. Abonelik altında girin **mevcut kaynak ekleyin** tıklatıp **kaydetmek**.

4. Tüm abonelikleri kapsamda ekleyene kadar 1 ile 3 arasındaki adımları yineleyin.

 > [!NOTE]
 > Yönetim grupları, abonelikleri ve alt Yönetim gruplarını içerebilir. Üst yönetim grubuna bir RBAC rolü bir kullanıcıya atadığınızda, erişim alt yönetim grubunun abonelik tarafından devralınır. Üst yönetim grup ilkeleri da gruplar tarafından devralınır. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi için Kiracı çapında görünürlük geçirmesine öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md)

> [!div class="nextstepaction"]
> [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md)

