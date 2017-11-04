---
title: "Özel rol tabanlı erişim denetimi rolleri oluşturmak ve azure'da iç ve dış kullanıcılara atamak | Microsoft Docs"
description: "İç ve dış kullanıcılar için PowerShell ve CLI kullanılarak oluşturulan özel RBAC Rolleri Ata"
services: active-directory
documentationcenter: 
author: andreicradu
manager: catadinu
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/10/2017
ms.author: a-crradu
ms.openlocfilehash: bb9b89d087cfb62efe63cf0ff600d7faa58a7b8b
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
## <a name="intro-on-role-based-access-control"></a>Giriş rol tabanlı erişim denetimi hakkında

Rol tabanlı erişim denetimi belirli kaynak kapsamları ortamlarında yöneten diğer kullanıcılara ayrıntılı rolleri atamak sahipleri, bir abonelik sağlayan bir Azure portal yalnızca özelliğidir.

Dış ortak çalışanları, satıcılar veya ortamınızdaki belirli kaynaklara ancak mutlaka tüm altyapının veya herhangi bir erişim gereken freelancers ile çalışma RBAC büyük kuruluşlarda ve SMB'ler için daha iyi güvenlik yönetimi sağlar Faturalama ilgili kapsamlar. Bir Azure aboneliğine sahip esnekliğini yönetici hesabı (Hizmet Yöneticisi rolü bir abonelik düzeyinde) tarafından yönetilen ve birden çok kullanıcı aynı abonelik altında ancak tüm yönetim haklarına sahip olmayan çalışmak için davet edildi RBAC sağlar . Bir yönetim ve fatura perspektifi, RBAC Özelliği Azure çeşitli senaryolarda kullanmak için saat ve yönetim verimli bir seçenek kanıtlar.

## <a name="prerequisites"></a>Ön koşullar
RBAC kullanarak Azure ortamında gerektirir:

* Azure aboneliği kullanıcıya (abonelik rolü) sahibi olarak atanmış bir tek başına sahip
* Azure aboneliğinin sahibi rolüne sahip
* Erişimi [Azure portalı](https://portal.azure.com)
* Kullanıcı abonelik için kaydedilmiş aşağıdaki kaynak sağlayıcıları bulunduğundan emin olun: **Microsoft.Authorization**. Kaynak sağlayıcıları kaydetme hakkında daha fazla bilgi için bkz: [Resource Manager sağlayıcıları, bölgeleri, API sürümleri ve şemaları](/azure-resource-manager/resource-manager-supported-services.md).
<!---Loc Comment: Link [Resource Manager providers, regions, API versions and schemas] is broken with an error message "404 - Content Not Found---->

> [!NOTE]
> Office 365 aboneliği veya Azure Active Directory lisansları (örneğin: Azure Active Directory'ye erişim) portal yok kalitesi RBAC kullanarak O365 sağlandı.

## <a name="how-can-rbac-be-used"></a>RBAC nasıl kullanılabileceğini
RBAC, Azure üç farklı kapsamlar adresindeki uygulanabilir. En düşük bir üst kapsamdan bunlar şu şekildedir:

* Abonelik (yüksek)
* Kaynak grubu
* Kaynak kapsamı (tek başına bir Azure kaynak kapsam hedeflenen izinleri sunumu düşük erişim düzeyi)

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>Abonelik kapsamında RBAC Rolleri Ata
RBAC kullanılan (ancak bunlarla sınırlı olmamak kaydıyla olduğunda) iki ortak örnekler vardır:

* Dış kullanıcılar kuruluşlardan sahip bazı kaynaklar ya da tüm abonelik yönetmek için (yönetici kullanıcının Azure Active Directory Kiracı parçası değil) davet
* Kullanıcılar (bunlar parçası olan kullanıcının Azure Active Directory Kiracı) kuruluş ancak farklı ekipleri ve ayrıntılı erişim tüm abonelik ya da belirli kaynak grupları veya kaynak kapsamlarda gereken grupları parçası içinde ile çalışma ortamı

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Azure Active Directory dışındaki bir kullanıcı için erişim izni ver abonelik düzeyinde
RBAC rolleri olanağı verilir yalnızca **sahipleri** aboneliği, bu nedenle yönetici kullanıcı, önceden atanmış bu rolü veya Azure aboneliği oluşturduğu bir kullanıcı adıyla oturum açmış olmanız gerekir.

Oturum Yöneticisi olarak açma sonra Azure portalından "Abonelikleri" ve seçtiğiniz istenen bir seçin.
![Azure portalında abonelik dikey penceresinde](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) yönetici kullanıcı Azure aboneliği satın aldıysa varsayılan olarak, kullanıcı olarak görünecek **Hesap Yöneticisi**, bu abonelik rol bırakılıyor. Azure aboneliği rolleri hakkında daha fazla bilgi için bkz: [abonelik ya da hizmetleri yönetmek ekleme veya değiştirme Azure yönetici rolleri](/billing/billing-add-change-azure-subscription-administrator.md).

Bu örnekte, kullanıcı "alflanigan@outlook.com" olan **sahibi** "ücretsiz deneme sürümü" AAD abonelikte Kiracı "varsayılan Kiracı Azure". Bu kullanıcı ilk Microsoft Account "Outlook" Azure aboneliği oluşturan olduğundan (Microsoft Account Outlook, Canlı vb. =) bu Kiracı eklenen diğer tüm kullanıcılar için varsayılan etki alanı adı olacaktır **"@alflaniganuoutlook.onmicrosoft.com"**. Tasarım gereği, yeni etki alanının sözdizimi Kiracı oluşturan kullanıcının kullanıcı adı ve etki alanı adını bir araya getirilmesi ve uzantı ekleyerek biçimlendirildiğinden **". onmicrosoft.com"**.
Ayrıca, kullanıcıların oturum Kiracı içinde bir özel etki alanı adıyla ekleme ve yeni bir kiracı için doğruladıktan sonra açma. Azure Active Directory kiracısı bir özel etki alanı adını doğrulama hakkında daha fazla ayrıntı için bkz: [bir özel etki alanı adı dizininize eklemek](/active-directory/active-directory-add-domain).

Bu örnekte, yalnızca etki alanı adı olan kullanıcılar "Varsayılan Kiracı Azure" dizinini içeren "@alflanigan.onmicrosoft.com".

Abonelik seçtikten sonra yönetici kullanıcı tıklatmalısınız **erişim denetimi (IAM)** ve ardından **yeni rol ekleme**.





![erişim denetimi IAM Özelliği Azure portalında](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![erişim denetimi IAM Özelliği Azure portalında yeni kullanıcı Ekle](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

Sonraki adım atanacak rol ve kullanıcı kim RBAC rolü atandı seçmektir. İçinde **rol** açılır menüsünde Yönetici kullanıcı Azure'da kullanılabilen yalnızca yerleşik RBAC rolleri görür. Her rol ve bunların atanabilir kapsamların açıklamalarını ayrıntılı için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](/active-directory/role-based-access-built-in-roles.md).
<!---Loc Comment: Link [Built-in roles for Azure Role-Based Access Control] is broken with an error message "404 - Content Not Found---->

Yönetici kullanıcı, ardından dış kullanıcı e-posta adresini eklemesi gerekir. Varolan Kiracı göstermemeyi dış kullanıcı için beklenen davranıştır bakın. Dış kullanıcı davet sonra kendisinin altında görünür olacak **abonelikleri > erişim denetimi (IAM)** hangi aboneliği kapsamında bir RBAC rolü atanmış olan tüm geçerli kullanıcı ile.





![Yeni RBAC rolü izinleri ekleyin](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![Abonelik düzeyinde RBAC rollerinin listesi](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

Kullanıcı "chessercarlton@gmail.com" olması için davet bir **sahibi** "Ücretsiz deneme" aboneliği için. Daveti gönderdikten sonra dış kullanıcı etkinleştirme bağlantısı ile bir e-posta onayı alacaksınız.
![e-posta daveti RBAC rolü için](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)

Kuruluş dış olmasının, yeni kullanıcı varolan öznitelikleri "Varsayılan Kiracı Azure" dizininde yok. Dış kullanıcı izin verdiği sonra bunlar oluşturulacak kendisine bir role atanmış olan aboneliği ile ilişkili olan dizin kaydedilecek.





![RBAC rolü için e-posta davet iletisi](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

Şu andan itibaren dış kullanıcı olarak Azure Active Directory Kiracı ve bu dış kullanıcı gösterir, hem Azure portalında hem de klasik Portalı'nda görüntülenebilir.





![Kullanıcılar dikey azure active Directory'yi Azure portalı](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![Kullanıcılar dikey azure active directory Klasik Azure portalı](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

İçinde **kullanıcılar** dış kullanıcılar tarafından tanımasını her iki portalları görünümünde:

* Azure portalında farklı simge türü
* Klasik Portalı'nda farklı kaynak belirleme noktası

Ancak, verme **sahibi** veya **katkıda bulunan** bir dış kullanıcı erişimi **abonelik** kapsamı, yönetici kullanıcının dizinine erişimi sürece izin vermiyor **Genel yönetici** verir. Kullanıcı proprieties içinde **kullanıcı türü** iki ortak parametreleri olan **üye** ve **Konuk** tanımlanabilir. Konuk bir dış kaynaktan dizine davet bir kullanıcı olsa da dizinde kayıtlı bir kullanıcı bir üyesidir. Daha fazla bilgi için bkz: [nasıl Azure Active Directory yöneticileri ekleyin B2B işbirliği kullanıcılar](/active-directory/active-directory-b2b-admin-add-users).
<!---Loc Comment: Link [How do Azure Active Directory admins add B2B collaboration users] is broken with an error message "404 - Content Not Found--->

> [!NOTE]
> Portalda kimlik bilgilerini girdikten sonra emin olun, dış kullanıcı oturum için açmak için doğru dizin seçer. Aynı kullanıcı birden fazla dizine erişiminiz ve üst taraftaki Azure portalında kullanıcı tıklayarak bunları birini seçin ve ardından açılır listeden uygun dizini seçin.

Dizinde Konuk olmasının, çalışırken dış kullanıcı Azure aboneliğine yönelik tüm kaynakları yönetebilir, ancak dizinine erişilemiyor.





![Azure active Directory'yi Azure portalına sınırlı erişim](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

Azure Active Directory ve Azure aboneliği bir üst-alt ilişkisi gibi diğer Azure kaynaklarına sahip (örneğin: sanal makineler, sanal ağlar, web uygulamaları, depolama vb.) ile Azure aboneliğiniz yok. Tüm ikinci oluşturulur, yönetilen ve bir Azure dizinine erişimi yönetmek için bir Azure aboneliği kullanılırken bir Azure aboneliği altında faturalandırılır. Daha fazla ayrıntı için bkz: [nasıl bir Azure aboneliğine Azure AD ile ilgili](/active-directory/active-directory-how-subscriptions-associated-directory).

Tüm rollerden yerleşik RBAC, **sahibi** ve **katkıda bulunan** katılımcı oluşturun ve yeni RBAC rollerini silme olmasına, fark ortamdaki tüm kaynaklar için tam yönetim erişimi sağlar . Diğer yerleşik roller ister **sanal makine Katılımcısı** yalnızca bağımsız olarak, adıyla belirtilen kaynaklarına tam yönetim erişimi teklif **kaynak grubu** içine oluşturulan.

Yerleşik RBAC rolü atama **sanal makine Katılımcısı** kullanıcı rolü atanmış bir abonelik düzeyinde anlamına gelir:

* Tüm sanal makineleri bakılmaksızın kendi dağıtım tarihi ve parçası olan kaynak gruplarını görüntüleyebilirsiniz
* Abonelikteki sanal makinelerin tam yönetim erişimi olan
* Başka bir kaynak türleri abonelikte görüntüleyemezsiniz
* Fatura perspektifi değişiklikler çalıştırılamıyor

> [!NOTE]
> RBAC, Azure portal yalnızca özelliği olan Klasik portal erişim izni vermez.

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Bir dış kullanıcı için bir yerleşik RBAC rolü atayın
Bu test, dış kullanıcı farklı bir senaryo için "alflanigan@gmail.com" olarak eklenen bir **sanal makine Katılımcısı**.




![sanal makine katkıda bulunan yerleşik rolü](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

Normal yerleşik bu rol ile dış bu kullanıcı için bakın ve yalnızca sanal makineler ve bitişik kaynak yöneticisi yalnızca kaynaklarını dağıtırken gereken yönetmek için bir davranıştır. Tasarım gereği, kısıtlı bu rolleri yalnızca Azure portalında oluşturulan karşılık düşen kaynaklarına erişimi sunmak, ne olursa olsun bazı hala Klasik portalda dağıtılabilir (örneğin: sanal makineler).





![azure portalında sanal makine Katılımcısı rolüne genel bakış](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Aynı dizinde bir kullanıcı için erişim izni ver abonelik düzeyinde
İşlem akışı, bir dış kullanıcı ekleme ile aynıdır, hem kullanıcı yanı sıra RBAC rolü verme yönetici açısından rolüne erişim verilmeden. Burada oturum açtıktan sonra abonelik içindeki tüm kaynak kapsamları panosunda kullanılabilir olacak şekilde davet edilen kullanıcı e-posta Davetleri almaz farktır.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>Kaynak grubu kapsamındaki RBAC Rolleri Ata
Bir RBAC rolü atama bir **kaynak grubu** kapsama sahip abonelik düzeyinde, her iki tür kullanıcı - harici veya dahili (aynı dizinde parçası) için rol atama için benzer bir işlem. RBAC rolü atanmış kullanıcılar olduğu kaynak grubu, erişimden atanmış yalnızca ortamlarında görmek için **kaynak grupları** Azure portalında simgesi.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>Kaynak kapsamındaki RBAC Rolleri Ata
Bir kaynak kapsamda Azure RBAC rolü atama abonelik düzeyinde ya da aynı iş akışı içinde her iki senaryoları için aşağıdaki kaynak grubu düzeyinde rol atama için benzer bir işlem var. RBAC rolü atanmış kullanıcılar yalnızca için ya da erişim, atanmış öğeleri yeniden görebilir **tüm kaynakları** sekmesini veya doğrudan kendi Pano.

RBAC hem kaynak grup kapsamı veya kaynak kapsamı için önemli bir özelliği doğru dizinine oturum açmak emin olmak kullanıcıları içindir.





![Azure portalında Directory oturum açma](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Bir Azure Active Directory grubu için RBAC Rolleri Ata
Üç farklı kapsamlar Azure RBAC kullanarak tüm senaryoları yönetme, dağıtma ve kişisel bir aboneliği yönetme gerek kalmadan atanmış bir kullanıcı olarak çeşitli kaynakları yönetme ayrıcalık sunar. Ne olursa olsun RBAC rolü abonelik, kaynak grubu veya kaynak kapsamı, tüm kullanıcıların erişimi sahip olduğu bir Azure aboneliği altında hakkında daha fazla atanmış kullanıcılar tarafından oluşturulan kaynakları faturalandırılır atanır. Bu şekilde, tüm Azure aboneliğiniz için yönetici izinleri faturalama kullanıcılar var. eksiksiz bir genel görünüm tüketimi, bakılmaksızın kimin kaynaklarını yönetme

Büyük kuruluşlar için yönetici kullanıcının ekipleri ve tüm bölümler için bu nedenle dikkate alarak, her bir kullanıcı için değil tek tek parçalı erişim vermek istediği perspektif dikkate Azure Active Directory grupları için aynı şekilde RBAC rolleri uygulanabilir. Bu isteğe bağlı olarak son derece saat ve yönetim etkin. Bu örnekte, göstermeye **katkıda bulunan** rol, bir kiracı abonelik düzeyinde gruplarının eklendi.





![AAD grupları için RBAC rolü Ekle](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

Bu grupları sağlanan ve yalnızca Azure Active Directory içinde yönetilen güvenlik gruplarıdır.

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-powershell"></a>PowerShell kullanarak destek istekleri açmak için özel bir RBAC rolü oluşturun
Azure'da kullanılabilen yerleşik RBAC roller ortamında kullanılabilir kaynaklara dayalı belirli izin düzeyleri emin olun. Ancak, bu rollerin hiçbiri yönetici kullanıcının gereksinimlerinize uygun değilse, daha fazla özel RBAC rolleri oluşturarak erişimi sınırlamak için bir seçenek yoktur.

Özel RBAC rolleri oluşturmak için bir yerleşik rolü, düzenlemek ve ortam geri almak için gerekir. Rolünün karşıya yükleme ve indirme PowerShell veya CLI kullanılarak yönetilir.

Abonelik düzeyinde ayrıntılı erişim vermek ve aynı zamanda davet edilen kullanıcı destek isteği açma esnekliğini tanımak özel bir rol oluşturarak Önkoşullar anlamak önemlidir.

Bu örneğin yerleşik rol **okuyucu** kullanıcılar tüm kaynak kapsamları görüntülemek üzere ancak bunları düzenleyin veya yeni kampanya oluşturmak erişim sağlayan destek isteği açma seçeneğini izin vermek için özelleştirildi.

Dışarı aktarma ilk eylemi **okuyucu** rol PowerShell'de tamamlanması gerekiyor yönetici olarak yükseltilmiş izinlerle çalıştı.

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Okuyucu RBAC rolü için PowerShell ekran görüntüsü](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

Ardından rolünün JSON şablonunu ayıklamanız gerekir.





![Özel okuyucu RBAC rolü için JSON şablonunu](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

Tipik bir RBAC rolü üç ana bölüm dışında oluşan **Eylemler**, **NotActions** ve **AssignableScopes**.

İçinde **eylem** bölümü olan bu rol için izin verilen tüm işlemler listelenir. Her eylemin kaynak sağlayıcısından atandığı anlamak önemlidir. Bu durumda, destek biletlerini oluşturmak için **Microsoft.Support** kaynak sağlayıcısı listelenmesi gerekir.

Kullanılabilir ve aboneliğinizde kayıtlı ilgili tüm kaynak sağlayıcıları kullanabilmek için PowerShell'i kullanabilirsiniz.
```
Get-AzureRMResourceProvider

```
Ayrıca, kaynak sağlayıcıları yönetmek tüm kullanılabilir için PowerShell cmdlet'leri kontrol edebilirsiniz.
    ![Kaynak sağlayıcısı yönetimi için PowerShell ekran görüntüsü](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)

Belirli bir RBAC rolü için tüm eylemleri kısıtlamak için kaynak sağlayıcıları bölümü altında listelenen **NotActions**.
Son olarak, RBAC rolü açık abonelik kimlikleri kullanıldığı içeren zorunludur. Abonelik kimlikleri altında listelenen **AssignableScopes**, aksi takdirde, aboneliğinizde rolünü içeri aktarmak için izin verilmez.

Oluşturma ve RBAC rolü özelleştirme sonra aktarılması gereken ortamını yedekleyin.

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

Bu örnekte, özel bu RBAC rolü için "okuyucu destek biletleri erişim düzeyi kullanıcının her şeyi abonelikte görüntülemek ve destek istekleri açmaya izin verme" adıdır.

> [!NOTE]
> Destek isteği açma eylemi izin verme yalnızca iki yerleşik RBAC rolleri **sahibi** ve **katkıda bulunan**. Destek istekleri açmak bir kullanıcı için tüm destek isteklerini bir Azure aboneliğine yönelik temel alınarak oluşturulur çünkü kendisine yalnızca abonelik kapsamında bir RBAC rolü atanmalıdır.

Bu yeni özel rolü aynı dizinden bir kullanıcıya atandı.





![Azure portalında içeri özel RBAC rolü ekran görüntüsü](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![kullanıcı aynı dizinde özel alınan RBAC rol atama işleminin ekran görüntüsü](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![Özel alınan RBAC rolü izinlerini ekran görüntüsü](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

Bu özel RBAC rolü sınırları gibi vurgulamak için örnek daha ayrıntılı:
* Yeni destek istekleri oluşturabilirsiniz
* Yeni kaynak kapsamlar oluşturulamıyor (örneğin: sanal makine)
* Yeni kaynak grupları oluşturulamıyor





![Özel RBAC rolü destek istekleri oluşturma ekran görüntüsü](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![ekran görüntüsü özel RBAC rolü VM'ler oluşturmak mümkün değil](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![ekran görüntüsü özel RBAC rolü yeni RGs oluşturmak mümkün değil](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-azure-cli"></a>Azure CLI kullanarak destek istekleri açmak için özel bir RBAC rolü oluşturun
Mac ve PowerShell erişmesini olmadan çalışan, Azure CLI Git yoludur.

Özel bir rol oluşturmak için aşağıdaki adımları, rol CLI kullanarak bir JSON şablonu indirilemiyor ancak CLI görüntülenebilir tek özel durumu ile aynıdır.

Bu örnek için t yerleşik rolü seçtiniz **yedekleme okuyucu**.

```

azure role show "backup reader" --json

```





![CLI ekran görüntüsü yedekleme okuyucu rolüne Göster](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

Bir JSON şablonu proprieties kopyaladıktan sonra Visual Studio rolünde düzenleme **Microsoft.Support** kaynak sağlayıcısı eklendi **Eylemler** böylece bu kullanıcı destek bölümler Okuyucu için yedekleme kasalarını olmasını etmeden istek sayısı. Burada bu rolü kullanılacak, abonelik kimliği eklemek gerekli olan yeniden **AssignableScopes** bölümü.

```

azure role create --inputfile <path>

```





![Özel RBAC rolü alma CLI ekran görüntüsü](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

Yeni rol Azure portalında kullanılabilir ve atamasını önceki örneklerde olduğu gibi aynı işlemidir.





![CLI 1.0 kullanılarak oluşturulan özel RBAC rolü Azure portal ekran görüntüsü](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

Son yapı 2017'dan sonra Azure bulut Kabuk genel kullanıma açıktır. Azure bulut Kabuk tamamlayan bir IDE ve Azure Portal ' dir. Bu hizmeti ile kimlik doğrulaması ve Azure içinde barındırılan ve tarayıcı tabanlı bir kabuk alın ve makinenize yüklü CLI yerine kullanın.





![Azure bulut Kabuğu](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
