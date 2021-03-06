---
title: RBAC, Azure kaynakları için sorun giderme | Microsoft Docs
description: Azure kaynakları için rol tabanlı erişim denetimi (RBAC) ile ilgili sorunları giderin.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 59ece9c37a563efba6329a30c06c1b596b1a5d57
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058162"
---
# <a name="troubleshoot-rbac-for-azure-resources"></a>RBAC, Azure kaynakları için sorun giderme

Azure portalı ve rolleri erişimi sorunlarını giderme kullanırken beklenmesi gerekenler öğrenmek için bu makalede, Azure kaynakları için rol tabanlı erişim denetimi (RBAC) hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="problems-with-rbac-role-assignments"></a>RBAC rol atamalarıyla ilgili sorunlar

- Şirket Azure portalındaki bir rol ataması ekleyemiyor **erişim denetimi (IAM)** çünkü **Ekle** > **rol ataması Ekle** seçeneği devre dışı bırakıldı veya "nesne kimliğine sahip istemci eylemi gerçekleştirme yetkisi yok" izinleri hata aldığından, şu anda sahip rolü atanmış bir kullanıcı hesabıyla oturum açtığınız denetleyin `Microsoft.Authorization/roleAssignments/write` izni gibi [sahibi](built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) rol atamak için çalıştığınız kapsamında.
- Hata iletisi alırsanız "daha fazla rol ataması oluşturulabilir (kod: RoleAssignmentLimitExceeded)" hatasını alıyorsanız, rolleri gruplara atayarak rol atamalarının sayısını azaltmaya çalışın. Azure, abonelik başına en fazla **2000** rol atamasını destekler.

## <a name="problems-with-custom-roles"></a>Özel rollerle ilgili sorunlar

- Özel rol öğreticileri kullanarak özel bir rol oluşturmak için adımları gerekirse bkz [Azure PowerShell](tutorial-custom-role-powershell.md) veya [Azure CLI](tutorial-custom-role-cli.md).
- Mevcut bir özel rolü güncelleştirme bulamıyorsanız, şu anda sahip rolü atanmış bir kullanıcı hesabıyla oturum açtığınız denetleyin `Microsoft.Authorization/roleDefinition/write` izni gibi [sahibi](built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator).
- Özel rolü silemiyor ve "Role başvuran mevcut rol atamaları var (kod: RoleDefinitionHasAssignments)" hata iletisiyle karşılaşıyorsanız, özel rolü kullanan rol atamaları mevcuttur. Bu rol atamalarını kaldırın ve özel rolü silmeyi tekrar deneyin.
- Yeni bir özel rol oluşturmaya çalıştığınızda "Rol tanımı sınırı aşıldı. Daha fazla hiçbir rol tanımları oluşturulabilir (kod: RoleDefinitionLimitExceeded) "yeni bir özel rol oluşturmaya çalıştığınızda, kullanılmayan herhangi bir özel rolü silme. Azure kadar destekler **5000** bir kiracıdaki özel roller. (Azure Kamu, Azure Almanya ve Azure Çin 21Vianet gibi özel Bulutlar için 2000 özel rol sınırı vardır.)
- "İstemci bağlı abonelik bulunamadı ancak kapsamında '/ subscriptions / {subscriptionıd}', 'Microsoft.Authorization/roleDefinitions/write' eylemini gerçekleştirme izni olan" benzer bir hata alırsanız bir özel rol güncelleştirmeye çalıştığınızda, denetleyin olup olmadığını bir veya daha fazla [atanabilir kapsamlarla](role-definitions.md#assignablescopes) kiracıda silinmiş. Kapsam silinmişse, şu anda bir self servis çözüm olmadığı için destek bileti oluşturun.

## <a name="recover-rbac-when-subscriptions-are-moved-across-tenants"></a>Abonelikler kiracılar arasında taşınırken RBAC koşullarını kurtarma

- Abonelik aktarımı adımlar ihtiyacınız varsa bkz farklı bir Azure AD kiracısına [bir Azure aboneliğinin sahipliğini başka bir hesaba](../billing/billing-subscription-transfer.md).
- Bir aboneliği farklı bir Azure AD kiracısına aktardığınızda tüm rol atamaları kaynak Azure AD kiracısından kalıcı olarak silinir ve hedef Azure AD kiracısına geçirilmez. Rol atamalarınızı hedef kiracıda yeniden oluşturmanız gerekir. Ayrıca, Azure kaynakları için yönetilen kimlikleri el ile yeniden oluşturmanız gerekir. Daha fazla bilgi için [SSS ve bilinen sorunlar ile yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/known-issues.md).
- Azure AD'yi olduğunda kiracılar arasında taşındıktan sonra genel yönetici ve bir aboneliğe erişiminiz yoksa, kullanın **Azure kaynakları için Access management** geçici geçiş [erişiminiziYükselt](elevate-access-global-admin.md) aboneliğe erişim elde etmek için.

## <a name="issues-with-service-admins-or-co-admins"></a>Hizmet yöneticileri veya ortak yöneticilerle ilgili sorunlar

- Hizmet Yöneticisi veya ortak yönetici ile ilgili sorunlar yaşıyorsanız bkz [ekleme veya değiştirme Azure aboneliği yöneticileri](../billing/billing-add-change-azure-subscription-administrator.md) ve [Klasik Abonelik Yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yönetici rolleri](rbac-and-directory-admin-roles.md).

## <a name="access-denied-or-permission-errors"></a>Erişim engellendi veya izin hataları

- "Nesne kimliğine sahip istemcinin kapsam üzerinde işlemi gerçekleştirme yetkisi yok (kod: AuthorizationFailed)" izin hatasını kaynak oluşturmaya çalıştığınızda alıyorsanız, seçilen kapsamda kaynak için yazma iznine sahip bir rolün atanmış olduğu kullanıcı hesabıyla oturum açmış olduğunuzdan emin olun. Örneğin bir kaynak grubundaki sanal makineleri yönetmek için kaynak grubunda (veya üst kapsamda) [Sanal Makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) rolüne sahip olmanız gerekir. Yerleşik rollerin izinlerinin yer aldığı liste için bkz. [Azure kaynakları için yerleşik roller](built-in-roles.md).
- "Bir destek isteği oluşturmak için izniniz yok" izinleri hata alırsanız, oluşturulacak veya güncelleştirilecek bir destek bileti çalıştığınızda şu anda sahip rolü atanmış bir kullanıcı hesabıyla oturum açtığınız denetleyin `Microsoft.Support/supportTickets/write` gibiizni[Destek isteği Katılımcısı](built-in-roles.md#support-request-contributor).

## <a name="role-assignments-without-a-security-principal"></a>Bir güvenlik sorumlusu olmadan rol atamaları

Rol atamalarınızı Azure PowerShell kullanarak listelediğinizde, boş bir atamaları görebileceğiniz `DisplayName` ve `ObjectType` bilinmeyen olarak ayarlayın. Örneğin, [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) aşağıdakine benzer bir rol ataması döndürür:

```azurepowershell
RoleAssignmentId   : /subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111
DisplayName        :
SignInName         :
RoleDefinitionName : Storage Blob Data Contributor
RoleDefinitionId   : ba92f5b4-2d11-453d-a403-e96b0029c9fe
ObjectId           : 33333333-3333-3333-3333-333333333333
ObjectType         : Unknown
CanDelegate        : False
```

Benzer şekilde, Azure CLI'yı kullanarak, rol atamalarını listeleme, boş bir atamaları görebilirsiniz `principalName`. Örneğin, [az rol ataması listesi](/cli/azure/role/assignment#az-role-assignment-list) aşağıdakine benzer bir rol ataması döndürür:

```azurecli
{
    "canDelegate": null,
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222",
    "name": "22222222-2222-2222-2222-222222222222",
    "principalId": "33333333-3333-3333-3333-333333333333",
    "principalName": "",
    "roleDefinitionId": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
    "roleDefinitionName": "Storage Blob Data Contributor",
    "scope": "/subscriptions/11111111-1111-1111-1111-111111111111",
    "type": "Microsoft.Authorization/roleAssignments"
}
```

Bu rol atamaları bir güvenlik sorumlusu (kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik) rol atama ve daha sonra bu güvenlik sorumlusunun silmek ortaya çıkar. Bu rol atamaları Azure portalında görüntülenmez ve bunları bırakmak için bir sorun değildir. Ancak, isterseniz, bu rolü atamalarını kaldırabilirsiniz.

Bu rol atamalarını kaldırmak için [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) veya [az rol atamasını Sil](/cli/azure/role/assignment#az-role-assignment-delete) komutları.

PowerShell'de, denerseniz nesne Kimliğini ve rol tanımı adı ve birden fazla rol ataması'nı kullanarak rol atamalarını kaldırmak için parametrelerinizi eşleşir, hata iletisi alırsınız: "Rol ataması için sağlanan bilgileri eşlenmiyor". Hata iletisi örneği aşağıda gösterilmiştir:

```Example
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor"

Remove-AzRoleAssignment : The provided information does not map to a role assignment.
At line:1 char:1
+ Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : CloseError: (:) [Remove-AzRoleAssignment], KeyNotFoundException
+ FullyQualifiedErrorId : Microsoft.Azure.Commands.Resources.RemoveAzureRoleAssignmentCommand
```

Bu hata iletisini alırsanız, ayrıca belirttiğinizden emin olun `-Scope` veya `-ResourceGroupName` parametreleri.

```Example
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor" - Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="rbac-changes-are-not-being-detected"></a>RBAC değişikliklerin algılanmaz

Azure Resource Manager bazen yapılandırmaları ve performansı artırmak için verileri önbelleğe alır. Rol atamaları silmesini veya yaratmasını, değişikliklerin etkili olması için 30 dakikaya kadar sürebilir. Azure portalı, Azure PowerShell veya Azure CLI kullanıyorsanız kapatıp açtıktan rol ataması değişikliklerinizi yenilemeye zorlayabilirsiniz. REST API çağrıları ile rol atamasını değişiklikler yapıyorsanız, erişim belirtecinizin yenileyerek yenilemeye zorlayabilirsiniz.

## <a name="web-app-features-that-require-write-access"></a>Yazma erişimi gerektiren bir web uygulaması özellikleri

Tek bir web uygulaması için kullanıcı salt okunur erişim sağlıyorsa, beklediğiniz değil, bazı özellikler devre dışı bırakılır. Aşağıdaki yönetim özelliklerini gerektiren **yazma** (Katkıda bulunan veya sahip) bir web uygulamasına erişim ve salt okunur senaryoda kullanılamaz.

* Komutlar (örneğin, başlatma, durdurma, vs.)
* Genel yapılandırma, Ölçek ayarları, Yedekleme ayarlarını ve izleme ayarları gibi ayarlarını değiştirme
* Yayımlama kimlik bilgileri ve diğer gizli dizileri uygulama ayarlarının ve bağlantı dizeleri gibi erişme
* Akış günlükleri
* Tanılama günlüklerini yapılandırma
* Konsolu (komut istemi)
* Etkin ve son dağıtımları (için yerel git sürekli dağıtım)
* Tahmini harcama
* Web testleri
* Sanal ağ (yalnızca, bir sanal ağda yazma erişimine sahip bir kullanıcı tarafından önceden yapılandırılmış bir okuyucu için görünür).

Bu kutucuk erişemiyorsanız, web uygulamasına katkıda bulunan erişimi için yöneticinizden gerekir.

## <a name="web-app-resources-that-require-write-access"></a>Yazma erişimi gerektiren bir web uygulama kaynakları

Web apps, Etkileşim özelliği birkaç farklı kaynak varlığı karmaşık. Tipik bir kaynak grubu birkaç Web siteleri şu şekildedir:

![Web uygulaması kaynak grubu](./media/troubleshooting/website-resource-model.png)

Yalnızca web uygulaması, Azure portalında Web sitesi dikey penceresinde işlevselliğinin erişimi devre dışı vermeniz bir sonucu.

Bu öğeleri gerektiren **yazma** erişim **App Service planı** sitenize karşılık gelir:  

* Web uygulamasını görüntüleme (ücretsiz veya standart) katmanı fiyatlandırma  
* Ölçek yapılandırma (örnekleri, sanal makine boyutu, otomatik ölçeklendirme ayarları sayısı)  
* Kotalar (depolama, bant genişliği, CPU)  

Bu öğeleri gerektiren **yazma** tam erişim **kaynak grubu** sitenizi içeren:  

* SSL sertifikaları ve bağlamaları (SSL sertifikaları, coğrafi konum ve aynı kaynak grubundaki siteler arasında paylaşılabilir)  
* Uyarı kuralları  
* Otomatik ölçeklendirme ayarları  
* Application ınsights bileşenleri  
* Web testleri  

## <a name="virtual-machine-features-that-require-write-access"></a>Yazma erişimi gerektiren sanal makine özellikleri

Benzer şekilde web uygulamaları, sanal makine dikey penceresinde bazı özellikler sanal makineye veya kaynak grubundaki diğer kaynaklara yazma erişimi gerektirir.

Sanal makineler, etki alanı adları, sanal ağlar, depolama hesapları ve uyarı kuralları ilgilidir.

Bu öğeleri gerektiren **yazma** erişim **sanal makine**:

* Uç Noktalar  
* IP adresleri  
* Diskler  
* Uzantılar  

Bunlar gerektirir **yazma** hem erişim **sanal makine**ve **kaynak grubu** (etki alanı adı ile birlikte) BT'nin zamanı:  

* Kullanılabilirlik kümesi  
* Yük dengeli küme  
* Uyarı kuralları  

Bu kutucuk erişemiyorsanız, kaynak grubuna katkıda bulunan erişimi için yöneticinizden isteyin.

## <a name="azure-functions-and-write-access"></a>Azure işlevleri ve yazma erişimi

Bazı özellikleri [Azure işlevleri](../azure-functions/functions-overview.md) yazma erişimi gerektirir. Okuyucu rolü atanmış bir kullanıcı, örneğin, bunlar bir işlev uygulaması işlevlerinde görüntülemek mümkün olmayacaktır. Portal görüntüler **(erişim yok)** .

![Uygulama erişimi işlevi](./media/troubleshooting/functionapps-noaccess.png)

Bağlanabilmesi **Platform özellikleri** sekmesine ve ardından **tüm ayarlar** bazı ayarları görüntülemek için ilgili bir işlev uygulaması (bir web app ile benzer), ancak bu ayarlardan herhangi birini değiştiremezler.

## <a name="next-steps"></a>Sonraki adımlar
* [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-portal.md)
* [RBAC değişiklikler Azure kaynakları için etkinlik günlüklerini görüntüleme](change-history-report.md)

