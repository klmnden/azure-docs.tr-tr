---
title: "Lisans kullanıcıların Azure Active Directory'de | Microsoft Docs"
description: "Kendinizin ve kullanıcılarınızın Azure Active Directory'de lisans öğrenin."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
custom: it-pro
ms.openlocfilehash: c4509cdb003687083d0456c1957b19cf35ee056a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="quickstart-license-users-in-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory'deki kullanıcı lisansı
Lisans tabanlı Azure AD, Azure kiracınızdaki bir Azure Active Directory (Azure AD) aboneliği etkinleştirerek iş Hizmetleri. Abonelik etkinleştirildikten sonra hizmet özellikleri Azure AD yöneticileri tarafından yönetilir ve lisanslı kullanıcılar tarafından kullanılır. Enterprise Mobility + güvenlik, Azure AD Premium veya Azure AD temel satın aldığınızda, Kiracı, geçerlilik süresi ve ön ödemeli lisansları dahil olmak üzere abonelikle güncelleştirilir. Atanan veya kullanılabilir lisans sayısı gibi abonelik bilgilerinizi altında Azure Portalı aracılığıyla kullanılabilir **Azure Active Directory** açarak **lisansları** döşeme. **Lisansları** dikey olan lisans anlaşmalarını yönetme için en iyi yerdir.

Bir aboneliği elde Ücretli özellikleri yapılandırmak için gereken her şeyi olsa da, hala özellikleri Ücretli Ücretli Azure AD için kullanıcı lisansları atamanız gerekir. Kimin erişiminiz veya bir Azure üzerinden, yönetilen kimin herhangi bir kullanıcı bir lisans özelliği Ücretli AD atanmış. Lisans atama, kullanıcı ve Azure AD Premium, Basic veya Enterprise Mobility + Security gibi bir satın alınan hizmet arasındaki bir eşlemedir.

Kullanabileceğiniz [grup tabanlı lisans atamasını](active-directory-licensing-whatis-azure-portal.md) aşağıdaki gibi kurallar ayarlamak için:
* Dizininizdeki tüm kullanıcılara otomatik olarak bir lisans alın
* Uygun iş unvanı herkesle bir lisans alır
* Kuruluştaki diğer yöneticiler kararını devredebilirsiniz (kullanarak [Self Servis grup](active-directory-accessmanagement-self-service-group-management.md))

> [!TIP]
> Gelişmiş senaryolar ve Office 365 senaryoları, lisans gruplarına lisans atama hakkında ayrıntılı bilgi için dahil olmak üzere bkz [Azure Active Directory'de Grup üyeliğiyle kullanıcılara lisanslar atama](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="assign-licenses-to-users-and-groups"></a>Kullanıcılar ve gruplar için lisans atama
Etkin bir aboneliğiniz kullanarak ilk lisans kendinize atamak ve tüm aboneliğinizde yer alan beklenen özellikler gördüğünüzden emin olmak için tarayıcınızı yenileyin. Azure AD özelliklerini erişmek isteyen kullanıcıların lisans atamak için ücretli sonraki adımdır. Kullanıcı grupları yerine kişiler lisansları atamak için lisans atamak için kolay bir yol olduğu. Bir gruba lisans atadığınızda, tüm Grup üyeleri lisansı atanır. Kullanıcılar eklendiğinde veya gruptan kaldırıldığında, uygun lisans otomatik olarak atanan kaldırıldı veya. 

> [!NOTE]
> Bazı Microsoft Hizmetleri tüm konumlarda kullanılabilir değil. Yönetici bir kullanıcıya bir lisans atanabilmesi için önce belirtmelidir **kullanım konumu** kullanıcı için özellik. Bu özelliği altında ayarlayabilirsiniz **kullanıcı** &gt; **profil** &gt; **ayarları** Azure portalında. Grup lisans atamasını kullanırken, kullanım konumu belirtilmezse herhangi bir kullanıcı dizininin konumunu devralır.

Altında bir lisans atamak için **Azure Active Directory** &gt; **lisansları** &gt; **tüm ürünleri**, bir veya daha fazla ürünleri seçin ve ardından seçin **Ata** komut çubuğunda.

![Bir lisans atamak için seçin](media/license-users-groups/select-license-to-assign.png)

Kullanabileceğiniz **kullanıcılar ve gruplar** dikey birden çok kullanıcı veya grup seçin ya da hizmet planları üründeki devre dışı bırakmak için. Arama kutusuna üstte kullanıcı ve grup adları aramak için kullanın.

![Bir kullanıcı veya grup için lisans atamasını seçin](media/license-users-groups/select-user-for-license-assignment.png)

Bir gruba lisans atadığınızda, lisans grubu boyutuna bağlı olarak tüm kullanıcılar devralırlar önce biraz zaman alabilir. İşleme durumunu denetleyebilirsiniz **grup** dikey altında **lisansları** döşeme.

![Lisans atama durumu](media/license-users-groups/license-assignment-status.png)

Atama hataları sırasında Azure AD lisans atamasını gerçekleşebileceğini, ancak Azure AD yönetirken görece olarak daha ender ve Enterprise Mobility + güvenlik ürünleri. Olası atama hataları sınırlıdır:
- Atama çakışma: ne zaman bir kullanıcı daha önce atanma geçerli lisansı ile uyumsuz bir lisans. Bu durumda, yeni lisans atama geçerli kaldırılması gerekir.
- Kullanılabilir lisans aşıldı: atanan gruplarındaki kullanıcılar kullanılabilir lisans sayısını aştığında, bir hata nedeniyle eksik lisansları atamak için bir kullanıcının atama durumu yansıtır.

### <a name="azure-ad-b2b-collaboration-licensing"></a>Azure AD B2B işbirliği lisanslama

B2B işbirliği Azure AD hizmetlerinde erişim sağlamak için Azure AD kiracınıza içine Konuk kullanıcıları davet sağlar ve Azure kaynaklarını kullanılabilir yapın.  

B2B kullanıcıları davet ve bunları Azure AD'de bir uygulamaya atamak için ücretsizdir. En fazla 10 Konuk kullanıcı ve 3 temel rapor başına uygulamalardır ayrıca B2B işbirliği kullanıcılar için boş. Konuk kullanıcı ortağının Azure AD kiracısında atanmış uygun lisans varsa, bunlar sizin içinde de lisansına sahip olması.

Gerekli değildir, ancak Azure AD özelliklerini Ücretli erişim sağlamak istiyorsanız, bu B2B Konuk kullanıcılar lisansına sahip olması gerekir ile Azure AD lisansları uygun. Davet bir kiracı ile lisans Ücretli bir Azure AD B2B işbirliği Kiracı için davet ek bir beş Konuk kullanıcılar kullanıcı hakları atayabilirsiniz. Senaryolar ve bilgi için bkz: [Kılavuzu lisans B2B işbirliği](active-directory-b2b-licensing.md).

## <a name="view-assigned-licenses"></a>Görünüm atanan lisansları

Atanan ve kullanılabilir lisans Özet görünümünü altında görüntülenen **Azure Active Directory** &gt; **lisansları** &gt; **tüm ürünleri**.

![Özet görünümü lisans](media/license-users-groups/view-license-summary.png)

Atanan kullanıcıların ve grupların ayrıntılı bir liste, belirli bir ürün seçildiğinde kullanılabilir. **Lisanslı kullanıcı** listesi şu anda bir lisans ve lisans doğrudan kullanıcıya atanmış olup olmadığını veya bir gruptan devralınan varsa kullanan tüm kullanıcıları gösterir.

![Lisans ayrıntıları görüntüle](media/license-users-groups/view-license-detail.png)

Benzer şekilde, **lisans grupları** liste olduğu lisansları atanan tüm grupları gösterir. Bir kullanıcı veya açmak için grup seçin **lisansları** bu nesne için atanmış tüm lisanslar gösteren dikey.

## <a name="remove-a-license"></a>Bir lisans Kaldır

Bir lisans kaldırmak için kullanıcı veya gruba gidin ve açmak **lisansları** döşeme. Lisans seçin ve tıklayın **kaldırmak**.

![Bir lisans Kaldır](media/license-users-groups/remove-license.png)

Bir gruptan bir kullanıcı tarafından devralınmış lisansları doğrudan kaldırılamaz. Bunun yerine, kullanıcı, bunlar lisans devralan grubundan kaldırın.


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıç kullanıcılar ve gruplar Azure AD dizininde lisansları atama öğrendiniz. 

Azure portalından Azure AD'de abonelik lisans anlaşmalarını yapılandırmak için aşağıdaki bağlantıyı kullanın.

> [!div class="nextstepaction"]
> [Azure AD lisansları atama](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Overview) 