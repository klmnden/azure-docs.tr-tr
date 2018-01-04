---
title: "Azure Active Directory'de kaynaklarına erişimi yönetmek için grupları kullanma | Microsoft Docs"
description: "Grupları Azure Active Directory'de kullanıcı erişimini şirket içi ve bulut uygulamaları ve kaynakları yönetmek için nasıl kullanılacağını."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 714120d0-cdf9-465d-afee-39bef591c6b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: aaccc501526d313a572692ff8f2f5c9da38849d3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="manage-access-to-resources-with-azure-active-directory-groups"></a>Azure Active Directory grupları ile kaynaklara erişimi yönetme
Azure Active Directory (Azure AD), güçlü bir şirket içi ve bulut uygulamalarını ve Office 365 ve Microsoft olmayan SaaS uygulamaları dünyasına gibi Microsoft Çevrimiçi hizmetler gibi kaynakları erişimi yönetmek için özellikler kümesi sağlayan kapsamlı bir kimlik ve erişim yönetimi çözüm olduğu. Bu makalede genel bir bakış sağlar, ancak başlatmak istiyorsanız Azure AD kullanarak gruplar şu anda,'ndaki yönergeleri izleyin [Azure AD'deki güvenlik gruplarını yönetme](active-directory-groups-create-azure-portal.md). Azure Active Directory'deki grupları yönetmek için PowerShell nasıl kullanabileceğiniz görmek istiyorsanız, daha fazla bilgiyi [Grup Yönetimi için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

> [!NOTE]
> Azure Active Directory'yi kullanmak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, şunları yapabilirsiniz [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/pricing/free-trial/).
>
>

Azure AD içinde bulunan önemli özelliklerin kaynaklara erişimi yönetme olanağı biridir. Bu kaynaklar dizin ya da SaaS uygulamaları, Azure Hizmetleri ve SharePoint siteleri veya şirket içi kaynaklar gibi dizin dışı olan kaynaklar rolleri aracılığıyla nesneleri yönetmek için izinleri durumunda olduğu gibi dizinin parçası olabilir. Bir kullanıcı bir kaynağa erişim hakları atanabilir dört yolu vardır:

1. Doğrudan atama

    Kullanıcıların doğrudan bir kaynağa, kaynak sahibi tarafından atanabilir.
2. Grup üyeliği

    Bir grup bir kaynağa kaynak sahibi tarafından ve bunu yaparak bu Grup kaynağına erişim izni üyeleri verme atanabilir. Grup üyeliğini sonra Grup sahibi tarafından yönetilebilir. Etkili bir şekilde kaynak sahibi kendi kaynak grubunun sahibi için kullanıcılar atama izni verilir.
3. Kural tabanlı

    Kaynak sahibi, hangi kullanıcıların bir kaynağa erişim atanmalıdır ifade etmek için bir kuralı kullanabilirsiniz. Bunu yaparak, kaynak sahibi etkili bir şekilde kuralda kullanılan öznitelikler için yetkili kaynağı kaynaklarına erişimi yönetme hakkı atar ve kural sonucunu, kural ve değerleri, belirli kullanıcılar için kullanılan öznitelikleri bağlıdır. Kaynak sahibi hala kural yönetir ve hangi öznitelikleri ve değerleri kendi kaynağına erişim sağlayıp belirler.
4. Dış yetkilisi

    Bir kaynağa erişimi bir dış kaynaktan elde edilir; Örneğin, bir şirket içi dizin gibi yetkili bir kaynak veya WorkDay gibi bir SaaS uygulaması eşitlenen bir grup. Kaynak sahibi kaynağa erişim sağlamak için Grup atar ve dış kaynak grubunun üyeleri yönetir.

   ![Erişim Yönetimi diyagramı genel bakış](./media/active-directory-access-management-groups/access-management-overview.png)

## <a name="watch-a-video-that-explains-access-management"></a>Erişim Yönetimi açıklayan bir videoyu izleyin
Bu konuda daha açıklayan kısa bir video izleyebilirsiniz:

**Azure AD: Gruplar için dinamik üyelik giriş**

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]
>
>

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Nasıl erişim yönetimini Azure Active Directory çalışma?
Azure AD merkezinde erişim yönetimi çözümü güvenlik grubudur. Kaynaklara erişimi yönetmek için bir güvenlik grubu kullanma hedeflenen kullanıcı grubuna bir kaynağa erişim sağlamak esnek ve kolay anlaşılır bir yol sağlayan bir iyi bilinen, bir örnektir. Kaynak sahibi (veya dizinin Yöneticisi) sağ sahip oldukları kaynakları belirli bir erişim sağlamak için bir grup atayabilir. Grubun üyelerini erişim sağlanması ve kaynak sahibine bir bölüm Yöneticisi'ni veya bir Yardım Masası Yöneticisi gibi başka birinin bir gruba üye listesini yönetmek için sağa devredebilirsiniz.

![Azure Active Directory erişim yönetimi diyagramı](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Bir grubun sahibi, ayrıca bu grubun Self Servis isteklerine için kullanılabilir yapabilirsiniz. Bunu yaparken, bir son kullanıcı aramak ve Grup bulma ve katılmak için bir istek grubu ile yönetilen kaynaklara erişim izni etkili bir şekilde aramayı hale getirebilirsiniz. Böylece katılma istekleri otomatik olarak onaylanır veya grubun sahibi tarafından onayı iste grubun sahibi grubu oluşturan ayarlayabilirsiniz. Bir kullanıcı bir gruba katılması için bir istekte bulunduğunda katılma isteğini grubun sahiplerini iletilir. Sahiplerinden biri isteği onaylarsa, istekte bulunan kullanıcı bildirimi ve kullanıcı grubuna katılır. Sahiplerinden biri isteği reddeder, istekte bulunan Kullanıcı bildirim ancak grubuna katılmamış.

## <a name="getting-started-with-access-management"></a>Access management ile çalışmaya başlama
Başlamaya hazır mısınız? Azure AD grupları ile yapabileceğiniz temel görevlerden bazıları çıkışı denemelisiniz. Kuruluşunuzdaki farklı kaynaklar için farklı kişi grupları için özel erişim sağlamak için bu özellikleri kullanın. Temel ilk adımlar listesi aşağıda listelenmektedir.

* [Dinamik bir grup üyeliklerini yapılandırmak için basit bir kural oluşturma](active-directory-groups-create-azure-portal.md)
* [SaaS uygulamalarına erişimi yönetmek için bir grup kullanma](active-directory-accessmanagement-group-saasapps.md)
* [Bir grup için son kullanıcı Self Servis kullanıma](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect kullanan Azure şirket içi gruba eşitleniyor](active-directory-aadconnect.md)
* [Bir grubun sahiplerini yönetme](active-directory-accessmanagement-managing-group-owners.md)

## <a name="next-steps"></a>Sonraki adımlar
Erişim Yönetimi temelleri anladım, burada ek bazı gelişmiş özelliklerin Azure Active Directory içinde kullanılabilir uygulama ve kaynaklarına erişimi yönetmek için.

* [Gelişmiş kurallar oluşturmak için öznitelikleri kullanma](active-directory-groups-dynamic-membership-azure-portal.md)
* [Azure AD içinde güvenlik gruplarını yönetme](active-directory-groups-create-azure-portal.md)
* [Azure AD içinde ayrılmış grupları ayarlama](active-directory-accessmanagement-dedicated-groups.md)
* [Gruplar için grafik API'si başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-cmdlets.md)
