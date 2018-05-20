---
title: Azure dış kullanıcılar için rol atamalarını yönetme | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC), kuruluş için dış kullanıcılar için Azure'da yönetin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: kgremban
ms.assetid: ''
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/20/2018
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: dc0d91482637e3ade1147083b6d9c890e2798036
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="manage-role-assignments-for-external-users"></a>Dış kullanıcılar için rol atamalarını yönetme

Rol tabanlı erişim denetimi (RBAC) dış ortak çalışanları, satıcılar veya ortamınızdaki belirli kaynakların ancak mutlaka tüm erişmeniz freelancers ile çalışma büyük kuruluşlarda ve SMB'ler için daha iyi güvenlik yönetimi sağlar. Altyapı veya herhangi bir faturalama ilişkili kapsam. Bir Azure aboneliğine sahip esnekliğini yönetici hesabı (Hizmet Yöneticisi rolü bir abonelik düzeyinde) tarafından yönetilen ve birden çok kullanıcı aynı abonelik altında ancak tüm yönetim haklarına sahip olmayan çalışmak için davet edildi RBAC sağlar .

> [!NOTE]
> Office 365 aboneliği veya Azure Active Directory lisansları (örneğin: Azure Active Directory'ye erişim) Office 365 Yönetim Merkezi olmayan nitelemek için RBAC kullanarak gelen sağlandı.

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>Abonelik kapsamında RBAC Rolleri Ata

RBAC kullanılan (ancak bunlarla sınırlı olmamak kaydıyla olduğunda) iki ortak örnekler vardır:

* Dış kullanıcılar kuruluşlardan sahip bazı kaynaklar ya da tüm abonelik yönetmek için (yönetici kullanıcının Azure Active Directory Kiracı parçası değil) davet
* Kullanıcılar (bunlar, kullanıcının Azure Active Directory Kiracı parçasıdır) kuruluş ancak farklı ekipleri ve tüm abonelik için ya da belirli kaynak grupları veya ortamında kaynak kapsamı için ayrıntılı erişmesi gereken grupları parçası içinde ile çalışma

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Azure Active Directory dışındaki bir kullanıcı için erişim izni ver abonelik düzeyinde

RBAC rolleri olanağı verilir yalnızca **sahipleri** abonelik. Bu role sahip bir kullanıcı önceden atanmış veya Azure aboneliği oluşturduğu gibi bu nedenle, yönetici oturum açmanız gerekir.

Yönetici olarak oturum açtıktan sonra Azure portalından "Abonelikleri" ve seçtiğiniz istenen bir seçin.
![Azure portalında abonelik dikey penceresinde](./media/role-assignments-external-users/0.png) yönetici kullanıcı Azure aboneliği satın aldıysa varsayılan olarak, kullanıcı olarak görünecek **Hesap Yöneticisi**, bu abonelik rol bırakılıyor. Azure aboneliği rolleri hakkında daha fazla bilgi için bkz: [abonelik ya da hizmetleri yönetmek ekleme veya değiştirme Azure yönetici rolleri](../billing/billing-add-change-azure-subscription-administrator.md).

Bu örnekte, kullanıcı "alflanigan@outlook.com" olan **sahibi** "ücretsiz deneme sürümü" AAD abonelikte Kiracı "varsayılan Kiracı Azure". Bu kullanıcı ilk Microsoft Account "Outlook" Azure aboneliği oluşturan olduğundan (Microsoft Account Outlook, Canlı vb. =) bu Kiracı eklenen diğer tüm kullanıcılar için varsayılan etki alanı adı olacaktır **"@alflaniganuoutlook.onmicrosoft.com"**. Tasarım gereği, yeni etki alanının sözdizimi Kiracı oluşturan kullanıcının kullanıcı adı ve etki alanı adını bir araya getirilmesi ve uzantı ekleyerek biçimlendirildiğinden **". onmicrosoft.com"**.
Ayrıca, kullanıcıların bir kiracı özel etki alanı adı ekleme ve yeni bir kiracı için doğruladıktan sonra oturum. Azure Active Directory kiracısı bir özel etki alanı adını doğrulayın hakkında daha fazla bilgi için bkz: [bir özel etki alanı adı dizininize eklemek](/active-directory/active-directory-add-domain).

Bu örnekte, yalnızca etki alanı adı olan kullanıcılar "Varsayılan Kiracı Azure" dizinini içeren "@alflanigan.onmicrosoft.com".

Abonelik seçtikten sonra yönetici kullanıcı tıklatmalısınız **erişim denetimi (IAM)** ve ardından **yeni rol ekleme**.

![erişim denetimi IAM Özelliği Azure portalında](./media/role-assignments-external-users/1.png)

![erişim denetimi IAM Özelliği Azure portalında yeni kullanıcı Ekle](./media/role-assignments-external-users/2.png)

Sonraki adım atanacak rol ve kullanıcı kim RBAC rolü atandı seçmektir. İçinde **rol** açılır menüsünde, yönetici kullanıcı, Azure'da kullanılabilen yalnızca yerleşik RBAC rolleri görür. Her rol ve bunların atanabilir kapsamların açıklamalarını ayrıntılı için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](built-in-roles.md).

Yönetici kullanıcı, ardından dış kullanıcı e-posta adresini eklemesi gerekir. Varolan Kiracı göstermemeyi dış kullanıcı için beklenen davranıştır bakın. Dış kullanıcı davet sonra kendisinin altında görünür olacak **abonelikleri > erişim denetimi (IAM)** abonelik kapsamında bir RBAC rolü atanmış olan tüm geçerli kullanıcılar ile.

![Yeni RBAC rolü izinleri ekleyin](./media/role-assignments-external-users/3.png)

![Abonelik düzeyinde RBAC rollerinin listesi](./media/role-assignments-external-users/4.png)

Kullanıcı "chessercarlton@gmail.com" olması için davet bir **sahibi** "Ücretsiz deneme" aboneliği için. Daveti gönderdikten sonra dış kullanıcı etkinleştirme bağlantısı ile bir e-posta onayı alacaksınız.
![e-posta daveti RBAC rolü için](./media/role-assignments-external-users/5.png)

Kuruluş dış olmasının, yeni kullanıcı varolan öznitelikleri "Varsayılan Kiracı Azure" dizininde yok. Dış kullanıcı izin verdiği sonra bunlar oluşturulacak aboneliğe dizininde kaydedilecek kendisine bir role atanmıştır.

![RBAC rolü için e-posta davet iletisi](./media/role-assignments-external-users/6.png)

Şu andan itibaren dış kullanıcı olarak Azure Active Directory Kiracı ve bu dış kullanıcı gösterir Azure Portalı'nda görüntülenebilir.

![Kullanıcılar dikey azure active Directory'yi Azure portalı](./media/role-assignments-external-users/7.png)

İçinde **kullanıcılar** görünümü, dış kullanıcılar Azure portalında farklı simge türü tarafından tanınmıyor.

Ancak, verme **sahibi** veya **katkıda bulunan** bir dış kullanıcı erişimi **abonelik** kapsamı, yönetici kullanıcının dizinine erişimi sürece izin vermiyor **Genel yönetici** verir. Kullanıcı proprieties içinde **kullanıcı türü**, iki ortak parametreleri olan **üye** ve **Konuk** tanımlanabilir. Bir konuk bir dış kaynaktan dizine davet bir kullanıcı olsa da dizinde kayıtlı bir kullanıcı bir üyesidir. Daha fazla bilgi için bkz: [nasıl Azure Active Directory yöneticileri ekleyin B2B işbirliği kullanıcılar](../active-directory/active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Portalda kimlik bilgilerini girdikten sonra emin olun, dış kullanıcı oturum açmak için doğru dizin seçer. Aynı kullanıcı birden fazla dizine erişiminiz ve üst taraftaki Azure portalında kullanıcı tıklayarak bunları birini seçin ve ardından açılır listeden uygun dizini seçin.

Dizinde Konuk olmasının, çalışırken dış kullanıcı Azure aboneliğine yönelik tüm kaynakları yönetebilir, ancak dizinine erişilemiyor.

![Azure active Directory'yi Azure portalına sınırlı erişim](./media/role-assignments-external-users/9.png)

Azure Active Directory ve Azure aboneliği bir üst-alt ilişkisi gibi diğer Azure kaynaklarına sahip (örneğin: sanal makineler, sanal ağlar, web uygulamaları, depolama vb.) ile Azure aboneliğiniz yok. Tüm ikinci oluşturulan, yönetilen ve bir Azure dizinine erişimi yönetmek için bir Azure aboneliği kullanılırken bir Azure aboneliği altında faturalandırılır. Daha fazla bilgi için bkz: [nasıl bir Azure aboneliğine Azure AD ile ilgili](/active-directory/active-directory-how-subscriptions-associated-directory).

Tüm rollerden yerleşik RBAC, **sahibi** ve **katkıda bulunan** katılımcı oluşturun ve yeni RBAC rollerini silme olmasına, fark ortamdaki tüm kaynaklar için tam yönetim erişimi sağlar . Diğer yerleşik roller ister **sanal makine Katılımcısı** yalnızca bağımsız olarak, adıyla belirtilen kaynaklarına tam yönetim erişimi teklif **kaynak grubu** içine oluşturulan.

Yerleşik RBAC rolü atama **sanal makine Katılımcısı** kullanıcı rolü atanmış bir abonelik düzeyinde anlamına gelir:

* Tüm sanal makineleri bakılmaksızın kendi dağıtım tarihi ve parçası olan kaynak gruplarını görüntüleyebilirsiniz
* Abonelikteki sanal makinelerin tam yönetim erişimi olan
* Başka bir kaynak türleri abonelikte görüntüleyemezsiniz
* Fatura perspektifi değişiklikler çalıştırılamıyor

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Bir dış kullanıcı için bir yerleşik RBAC rolü atayın

Bu test, dış kullanıcı farklı bir senaryo için "alflanigan@gmail.com" olarak eklenen bir **sanal makine Katılımcısı**.

![sanal makine katkıda bulunan yerleşik rolü](./media/role-assignments-external-users/11.png)

Normal yerleşik bu rol ile dış bu kullanıcı için bakın ve yalnızca sanal makineler ve bitişik kaynak yöneticisi yalnızca kaynaklarını dağıtırken gereken yönetmek için bir davranıştır. Tasarım gereği, kısıtlı bu rolleri yalnızca Azure portalında oluşturulan karşılık düşen kaynaklarına erişimi sunar.

![Azure portalında sanal makine Katılımcısı rolüne genel bakış](./media/role-assignments-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Aynı dizinde bir kullanıcı için erişim izni ver abonelik düzeyinde

İşlem akışı, bir dış kullanıcı ekleme ile aynıdır, hem kullanıcı yanı sıra RBAC rolü verme yönetici açısından rolüne erişim verilmeden. Burada oturum açtıktan sonra abonelik içindeki tüm kaynak kapsamları panosunda kullanılabilir olacak şekilde davet edilen kullanıcı e-posta Davetleri almaz farktır.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>Kaynak grubu kapsamındaki RBAC Rolleri Ata

Bir RBAC rolü atama bir **kaynak grubu** kapsama sahip abonelik düzeyinde, her iki tür kullanıcı - harici veya dahili (aynı dizinde parçası) için rol atama için benzer bir işlem. RBAC rolü atanmış kullanıcılar olduğu kaynak grubu, erişimden atanmış yalnızca ortamlarında görmek için **kaynak grupları** Azure portalında simgesi.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>Kaynak kapsamındaki RBAC Rolleri Ata

Bir kaynak kapsamda Azure RBAC rolü atama abonelik düzeyinde ya da aynı iş akışı içinde her iki senaryoları için aşağıdaki kaynak grubu düzeyinde rol atama için benzer bir işlem var. RBAC rolü atanmış kullanıcılar yalnızca için ya da erişim, atanmış öğeleri yeniden görebilir **tüm kaynakları** sekmesini veya doğrudan kendi Pano.

RBAC hem kaynak grup kapsamı veya kaynak kapsamı için önemli bir özelliği doğru dizinine oturum açmak emin olmak kullanıcıları içindir.

![Azure portalında Directory oturum açma](./media/role-assignments-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Bir Azure Active Directory grubu için RBAC Rolleri Ata

Üç farklı kapsamlar Azure RBAC kullanarak tüm senaryoları yönetme, dağıtma ve kişisel bir aboneliği yönetme gerek kalmadan atanmış bir kullanıcı olarak çeşitli kaynakları yönetme ayrıcalık sunar. Ne olursa olsun RBAC rolü atanmış bir abonelik, kaynak grubu veya kaynak kapsamı için kullanıcıların erişimi sahip olduğu bir Azure aboneliği altında hakkında daha fazla atanmış kullanıcılar tarafından oluşturulan tüm kaynakları faturalandırılır. Bu şekilde, tüm Azure aboneliğiniz için yönetici izinleri faturalama kullanıcılar var. eksiksiz bir genel görünüm tüketimi, bakılmaksızın kimin kaynaklarını yönetme

Büyük kuruluşlar için yönetici kullanıcının ekipleri ve tüm bölümler için bu nedenle dikkate alarak, her bir kullanıcı için değil tek tek parçalı erişim vermek istediği perspektif dikkate Azure Active Directory grupları için aynı şekilde RBAC rolleri uygulanabilir. Bu isteğe bağlı olarak son derece saat ve yönetim etkin. Bu örnekte, göstermeye **katkıda bulunan** rol, bir kiracı abonelik düzeyinde gruplarının eklendi.

![AAD grupları için RBAC rolü Ekle](./media/role-assignments-external-users/14.png)

Sağlanan ve yalnızca Azure Active Directory içinde yönetilen güvenlik grupları bu gruplarıdır.

