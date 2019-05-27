---
title: RBAC kullanarak dış kullanıcılar için Azure kaynaklarına erişimi yönetme | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) kullanarak bir kuruluş için dış kullanıcılar için Azure kaynaklarına erişimi yönetmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/20/2018
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: d919453816436366c00dde506210a2ed38cc69b7
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65952215"
---
# <a name="manage-access-to-azure-resources-for-external-users-using-rbac"></a>RBAC kullanarak dış kullanıcılar için Azure kaynaklarına erişimi yönetme

Rol tabanlı erişim denetimi (RBAC) dış ortak çalışanlar, satıcılar, ortamınızda belirli kaynakların ancak mutlaka tüm erişmesi gereken freelancers ile çalışırken büyük kuruluşlar için ve SMB'ler için daha iyi güvenlik yönetimi sağlar. Altyapı veya herhangi bir faturalandırma ile ilgili kapsam. Bir Azure aboneliğine sahip olan esnekliği yönetici hesabı (Hizmet Yöneticisi rolü abonelik düzeyinde) tarafından yönetilen ve birden çok kullanıcı aynı abonelik altında ancak tüm yönetim haklarına sahip olmayan için çalışmaya davet RBAC sağlar .

> [!NOTE]
> Office 365 aboneliği veya Azure Active Directory lisansları (örneğin: Yönetim Merkezi için RBAC kullanarak uygun olmayan Microsoft 365'ten erişimi Azure Active Directory) sağlandı.

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>Abonelik kapsamında RBAC Rolleri Ata

RBAC kullanılan (ancak bunlarla sınırlı olmamak üzere olduğunda) iki yaygın örnekleri vardır:

* Dış kullanıcıların kuruluşların belirli kaynaklara ya da tüm abonelik yönetmek için (yönetici kullanıcının Azure Active Directory kiracısı parçası değil) davet
* Kuruluş (bunlar, kullanıcının Azure Active Directory kiracısı parçası olan) ancak farklı ekipler veya tam bir aboneliğe veya belirli bir kaynak grupları veya kaynak kapsamları ortamında ayrıntılı erişim gereken gruplarının parçası içinde kullanıcılar ile çalışma

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Azure Active Directory dışındaki bir kullanıcı için erişim verme abonelik düzeyinde

RBAC rollerini yalnızca verilebilir **sahipleri** abonelik. Bu role sahip bir kullanıcı, önceden atanmış veya Azure aboneliğini oluşturan bu nedenle, yönetici oturum açmanız gerekir.

Yönetici oturum açtıktan sonra Azure portalından "Abonelikler" ve ardından istediğiniz birini seçin.
![Azure portalında abonelik dikey](./media/role-assignments-external-users/0.png) yönetici kullanıcı Azure aboneliği satın aldıysanız, varsayılan olarak, kullanıcı olarak görünecek **Hesap Yöneticisi**, bu abonelik rol alınıyor. Azure aboneliği rolleri hakkında daha fazla bilgi için bkz. [ekleme veya değiştirme Azure aboneliği yöneticileri](../billing/billing-add-change-azure-subscription-administrator.md).

Bu örnekte, kullanıcı "alflanigan@outlook.com" olan **sahibi** "ücretsiz deneme sürümü" aboneliğinde AAD Kiracı "varsayılan Kiracı Azure". Bu kullanıcı ilk Microsoft Account "Outlook" ile Azure aboneliğini oluşturan olduğundan (Microsoft Account = Outlook, Canlı vb.) bu kiracıda eklenen tüm kullanıcılar için varsayılan etki alanı adı **"\@ alflaniganuoutlook.onmicrosoft.com"**. Tasarım gereği, yeni etki alanının sözdizimi Kiracı oluşturan kullanıcının kullanıcı adı ve etki alanı adını bir araya getirilmesi ve uzantı ekleyerek biçimlendirilmiş **". onmicrosoft.com"**.
Ayrıca, kullanıcılar oturum kiracıdaki özel etki alanı ekleme ve yeni Kiracı için doğruladıktan sonra oturum açabilir. Azure Active Directory kiracısında özel etki alanı doğrulama hakkında daha fazla bilgi için bkz. [dizininize özel etki alanı adı ekleme](../active-directory/fundamentals/add-custom-domain.md).

Bu örnekte, yalnızca kullanıcıların etki alanı adıyla "Varsayılan Kiracı Azure" dizinini içeren "\@alflanigan.onmicrosoft.com".

Abonelik seçtikten sonra yönetici kullanıcı tıklatmalısınız **erişim denetimi (IAM)** ardından **Yeni Rol Ekle**.

![Azure portalında erişim denetimi IAM özelliği](./media/role-assignments-external-users/1.png)

![erişim denetimi IAM Özelliği Azure portalında yeni kullanıcı ekleme](./media/role-assignments-external-users/2.png)

Sonraki adım, atanacak rol ve kullanıcı kim RBAC rolü atanmış seçeneğini belirlemektir. İçinde **rol** açılır menüsünde, yönetici kullanıcı, Azure'da kullanılabilen yalnızca yerleşik RBAC rolleri görür. Her rol ve bunların atanabilir kapsamlarla açıklamalarını ayrıntılı için bkz: [Azure kaynakları için yerleşik roller](built-in-roles.md).

Yönetici kullanıcı, daha sonra dış kullanıcının e-posta adresi eklemeniz gerekir. Beklenen davranış mevcut kiracıda değil gösterilecek dış kullanıcı içindir. Dış kullanıcıyı davet sonra bunlar altında görünür olacak **abonelikler > erişim denetimi (IAM)** abonelik kapsamında bir RBAC rolü atanmış olan tüm geçerli kullanıcılar ile.

![Yeni RBAC rolü için izinleri ekleme](./media/role-assignments-external-users/3.png)

![Abonelik düzeyinde RBAC rolleri listesi](./media/role-assignments-external-users/4.png)

Kullanıcı "chessercarlton@gmail.com" olarak davet bir **sahibi** "Ücretsiz deneme" aboneliği için. Dış kullanıcıyı davet gönderdikten sonra etkinleştirme bağlantısını içeren bir e-posta onayı alırsınız.
![e-posta davetiyesi RBAC rolü](./media/role-assignments-external-users/5.png)

Kuruluş dışı olduğundan, yeni kullanıcı herhangi bir mevcut özniteliği "Varsayılan Kiracı Azure" dizininde yok. Dış kullanıcı izin verdiği sonra oluşturulurlar abonelikle ilişkili dizine kaydedilmesi, bir role atanmış.

![RBAC rolü için e-posta davetiyesi iletisi](./media/role-assignments-external-users/6.png)

Dış kullanıcı olarak artık Azure Active Directory kiracısı ve bu dış kullanıcı gösterir, Azure portalında görüntülenebilir.

![Kullanıcılar dikey penceresinde azure active Directory'yi Azure portalı](./media/role-assignments-external-users/7.png)

İçinde **kullanıcılar** görünümü, dış kullanıcılar, Azure portalında farklı simge türü tarafından tanınan.

Ancak, vermiş **sahibi** veya **katkıda bulunan** bir dış kullanıcı erişimi **abonelik** kapsamı, yönetici kullanıcının dizine erişim sürece izin vermiyor **Genel yönetici** verir. Kullanıcı ekrana içinde **kullanıcı türü**, iki ortak parametreleri olan **üye** ve **Konuk** tanımlanabilir. Bir konuk bir kullanıcı bir dış kaynaktan dizine davet ederken dizinde kayıtlı bir kullanıcı bir üyesidir. Daha fazla bilgi için [nasıl Azure Active Directory yöneticileri ekleme B2B işbirliği kullanıcıları](../active-directory/active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Portalda kimlik bilgilerini girdikten sonra emin olun, doğru dizinde oturum açmak için dış kullanıcıyı seçer. Aynı kullanıcı birden çok dizini erişimi kullanıcı adı, Azure portalının sağ üst taraflarında bulunur tıklayarak bunları birini seçin ve ardından açılır listeden uygun dizini seçin.

Dizinde Konuk olan sırasında dış kullanıcı Azure aboneliği için tüm kaynakları yönetebilir, ancak dizinine erişilemiyor.

![Azure active Directory'yi Azure portalına kısıtlı erişim](./media/role-assignments-external-users/9.png)

Azure Active Directory ve Azure aboneliği diğer Azure kaynakları gibi bir üst-alt ilişkisi yok (örneğin: sanal makineler, sanal ağlar, web uygulamaları, depolama vb.) ile bir Azure aboneliğine sahip. Tüm ikinci oluşturulmuş, yönetilen ve Azure aboneliğinin bir Azure dizinine erişimi yönetmek için kullanılan bir Azure aboneliği altında faturalandırılırken. Daha fazla bilgi için [nasıl bir Azure aboneliği Azure AD ile ilgili](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

Tüm yerleşik RBAC rolleri gelen **sahibi** ve **katkıda bulunan** katkıda bulunan oluşturun ve yeni RBAC rollerini silme emin olmasına fark ortamındaki tüm kaynakların tam yönetim erişimi sunar. . Bir yerleşik roller ister **sanal makine Katılımcısı** yalnızca ada göre bakılmaksızın belirtilen kaynaklar için tam yönetim erişimi sunan **kaynak grubu** içinde oluşturulan.

Yerleşik RBAC rolü atama **sanal makine Katılımcısı** kullanıcı rolü atanmış bir abonelik düzeyinde anlamına gelir:

* Tüm sanal makineler bakılmaksızın kendi dağıtım tarihi ve bunlar parçası olan kaynak grupları görüntüleyebilir
* Abonelikte sanal makinelerin tam yönetim erişimi olan
* Diğer kaynak türlerini abonelikte görüntüleyemezsiniz.
* Faturalandırma açısından değişiklikleri çalışamaz

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Bir dış kullanıcı için bir yerleşik RBAC rolü atayın

Bu test, dış kullanıcı farklı bir senaryo için "alflanigan@gmail.com" olarak eklenen bir **sanal makine Katılımcısı**.

![sanal makine Katılımcısı yerleşik rolü](./media/role-assignments-external-users/11.png)

Bu yerleşik rolü bu dış kullanıcı için normal davranış görebilir ve yalnızca sanal makineler ve bitişik Resource Manager yalnızca kaynaklarını dağıtırken gereken yönetme sağlamaktır. Bu sınırlı roller, tasarım gereği, yalnızca Azure portalında oluşturulan düşen kaynaklarına erişim sunar.

![Azure portalında sanal makine katkıda bulunan rolüne genel bakış](./media/role-assignments-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Aynı dizinde bir kullanıcının erişim verme abonelik düzeyinde

İşlem akışını, bir dış kullanıcı ekleme ile aynıdır, hem kullanıcı yanı sıra, RBAC rolü verme yönetici açısından rolüne erişim verilmeden. Buradaki fark, Abonelikteki tüm kaynak kapsamlar oturum açtıktan sonra Panoda kullanılabilir olarak davet edilen kullanıcının tüm e-posta Davetleri almaz ' dir.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>Kaynak grubu kapsamındaki RBAC Rolleri Ata

Bir RBAC rolü atama bir **kaynak grubu** kapsamına sahip iki kullanıcı türleri - harici veya dahili (aynı dizine parçası) için abonelik düzeyinde rol atamak için özdeş bir işlem. RBAC rolü atanmış kullanıcılar olduğu kaynak grubu, erişim atanmış yalnızca kendi ortamlarında görmek için **kaynak grupları** Azure portalında simgesi.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>Kaynak kapsamda RBAC Rolleri Ata

Azure'da bir kaynak kapsamda bir RBAC rolü atama abonelik düzeyinde veya her iki senaryo için aynı iş akışını izleyerek kaynak grubu düzeyinde rol atamak için benzer bir işlem var. RBAC rolü atanmış kullanıcılar için ya da erişim, atanmış öğeler yeniden görebilir **tüm kaynakları** sekmesinde veya doğrudan panonun.

Hem kaynak grup kapsamı veya kaynak kapsamında RBAC için önemli bir yönüdür doğru dizinde mi oturum açtığınızdan emin olmak kullanıcılara yöneliktir.

![Azure portalında Directory oturum açma](./media/role-assignments-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>RBAC rolleri için bir Azure Active Directory grubuna atayın

Azure'da üç farklı kapsamlardaki RBAC kullanarak tüm senaryolar yöneten, dağıtan ve kişisel abonelik yönetimine gerek kalmadan atanmış bir kullanıcısı olarak çeşitli kaynakları yönetme yükselmesi sunar. Bakılmaksızın bir aboneliğe, kaynak grubuna ya da kaynak kapsamı için RBAC rolü atanmış, erişim için kullanıcıların sahip olduğu bir Azure aboneliği altında hakkında daha fazla atanmış kullanıcılar tarafından oluşturulan tüm kaynakları faturalandırılır. Bu şekilde, faturalama, tüm Azure aboneliğiniz için yönetici izinleri olan kullanıcıları var. eksiksiz bir genel bakış tüketimi, bakılmaksızın kimin kaynakları yönetme

Büyük kuruluşlar için RBAC rollerini yönetici kullanıcı teams veya tüm Departmanlar için bu nedenle dikkate alarak, her bir kullanıcı için değil ayrı ayrı ayrıntılı erişim vermek istediğini açısından ele Azure Active Directory grupları ile aynı şekilde uygulanabilir Bu isteğe bağlı olarak son derece zaman ve yönetim etkin. Bu örneği göstermek üzere **katkıda bulunan** rolü, abonelik düzeyinde kiracısındaki gruplardan birine eklendi.

![AAD grupları için RBAC rolü Ekle](./media/role-assignments-external-users/14.png)

Sağlanan ve yönetilen yalnızca Azure Active Directory içinde güvenlik gruplarını bu gruplarıdır.

