---
title: Ekleme ve özel etki alanı adlarını - Azure Active Directory doğrulama | Microsoft Docs
description: Yönetim kavramlar ve Azure Active Directory'de bir etki alanı adı yönetmek için bilgi belgeleri
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 908ae768ae471ab6f49452c99323c31d34772d45
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60472340"
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Azure Active Directory'de özel etki alanı adlarını yönetme

Bir etki alanı adı için çok sayıda dizin kaynaklarını tanımlayıcının önemli bir parçasıdır: bir grup adresi bir parçası olan bir kullanıcı için bir kullanıcı adı veya e-posta adresi bir parçasıdır ve bazen bir uygulama için uygulama kimliği URI'SİNİN bir bölümü olabilir. Azure Active Directory'de (Azure AD) bir kaynak tarafından kaynağı içeren dizine ait bir etki alanı adı ekleyebilirsiniz. Yalnızca bir genel yönetici, Azure AD'de etki alanlarını yönetebilir.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Azure AD dizininiz için birincil etki alanı adı ayarlayın

Dizininiz oluşturulurken 'contoso.onmicrosoft.com' gibi bir ilk etki alanı adı da birincil etki alanı adıdır. Yeni kullanıcı oluşturma, birincil etki alanı varsayılan etki alanı için yeni bir kullanıcı adıdır. Bir birincil etki alanı adı ayarlama Portalı'nda yeni kullanıcılar oluşturmak bir yöneticinin sürecini kolaylaştırır. Birincil etki alanı adını değiştirmek için:

1. Oturum [Azure portalında](https://portal.azure.com) dizin için genel yönetici olan bir hesapla.
2. **Azure Active Directory**'yi seçin.
3. **Özel etki alanı adları**'nı seçin.
  
   ![Kullanıcı yönetimi sayfasına açma](./media/domains-manage/add-custom-domain.png)
4. Birincil etki alanı olmasını istediğiniz etki alanının adını seçin.
5. Seçin **birincil yap** komutu. Sorulduğunda Seçiminizi onaylayın.
  
   ![Bir etki alanını birincil ad](./media/domains-manage/make-primary-domain.png)

Federasyon olmayan tüm doğrulanmış özel etki alanında olmasını dizininiz için birincil etki alanı adını değiştirebilirsiniz. Dizininiz için birincil etki alanı değiştirme, var olan tüm kullanıcılar için kullanıcı adı değişmez.

## <a name="add-custom-domain-names-to-your-azure-ad-tenant"></a>Azure AD kiracınız ile özel etki alanı ekleme

900 yönetilen etki alanı adları ekleyebilir. Şirket içi Active Directory ile Federasyon için etki alanları yapılandırıyorsanız, her dizin için en fazla 450 etki alanı adları ekleyebilir.

## <a name="add-subdomains-of-a-custom-domain"></a>Özel bir etki alanının alt etki alanlarını ekleme

'Europe.contoso.com' gibi bir üçüncü düzey etki alanı adı dizininize eklemek istiyorsanız, öncelikle ekleyin ve ikinci düzey etki alanı, contoso.com gibi doğrulamanız gerekir. Alt etki alanı, Azure AD tarafından otomatik olarak doğrulanır. Eklediğiniz bir alt etki alanı doğrulanır görmek için tarayıcıda etki alanı listesini yenileyin.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>DNS kayıt şirketi için özel etki alanınızın adını değiştirirseniz yapmanız gerekenler

DNS kaydedicilerin değiştirirseniz, Azure AD'de hiçbir ek yapılandırma görevleri vardır. Kesinti olmadan Azure AD ile etki alanı adını kullanmaya devam edebilirsiniz. Office 365, Intune veya Azure AD'de özel etki alanı adları, diğer hizmetler ile özel etki alanınızın adını kullanırsanız, bu hizmetleri belgelerine bakın.

## <a name="delete-a-custom-domain-name"></a>Özel etki alanı Sil

Kuruluşunuz artık bu etki alanı adı kullanıyorsa veya başka bir Azure AD ile bu etki alanı adı kullanmanız gerekiyorsa, özel etki alanı, Azure AD'den silebilirsiniz.

Özel etki alanı adını silmek için önce dizininizdeki hiçbir kaynak etki alanı adına dayanan emin olmalısınız. Dizininizden, bir etki alanı adı silinemiyor:

* Herhangi bir kullanıcı, bir kullanıcı adı, e-posta adresi veya etki alanı adını içeren bir proxy adresi vardır.
* Herhangi bir grubu, bir e-posta adresi veya etki alanı adını içeren bir proxy adresi vardır.
* Herhangi bir uygulama, Azure AD'de bir uygulama kimliği URI'si, etki alanı adı içerir sahiptir.

Değiştirme veya özel etki alanı adını silmeden önce Azure AD dizininizde herhangi bir kaynağa silmeniz gerekir.

### <a name="forcedelete-option"></a>ForceDelete seçeneği

Yapabilecekleriniz **ForceDelete** bir etki alanı adını [Azure AD yönetim merkezini](https://aad.portal.azure.com) veya bu adı kullanıyor [Microsoft Graph API](https://docs.microsoft.com/graph/api/domain-forcedelete?view=graph-rest-beta). Bu seçenekler, zaman uyumsuz bir işlem kullanın ve özel etki alanı adından gibi tüm başvurularını güncelleştir "user@contoso.com"gibi ilk varsayılan etki alanı adı için"user@contoso.onmicrosoft.com." 

Çağrılacak **ForceDelete** Azure portalında, etki alanı adı için 1000'den az başvuruları vardır, ve Exchange sağlama hizmeti olduğu tüm başvuruları güncelleştirilemiyor veya kaldırıldı sağlamalısınız [ Exchange yönetici merkezini](https://outlook.office365.com/ecp/). Bu, Exchange Mail-Enabled güvenlik grupları ve dağıtılmış listeleri içerir; Daha fazla bilgi için [posta etkin güvenlik grupları kaldırma](https://technet.microsoft.com/library/bb123521(v=exchg.160).aspx#Remove%20mail-enabled%20security%20groups). Ayrıca, **ForceDelete** aşağıdakilerden biri doğruysa işlemi başarılı olmaz:

* Office 365 etki alanı abonelik hizmetleri aracılığıyla bir etki alanı satın
* Başka bir müşterinin Kiracı adına yönetim iş ortağı olan

Aşağıdaki eylemleri bir parçası olarak gerçekleştirilir **ForceDelete** işlemi:

* UPN, EmailAddress ve başvuruları özel etki alanı adına sahip kullanıcıların ProxyAddress ilk varsayılan etki alanı adını yeniden adlandırır.
* Özel etki alanı adı başvurular gruplarının EmailAddress ilk varsayılan etki alanı adını yeniden adlandırır.
* Özel etki alanı adı başvurular uygulamalarının identifierUris ilk varsayılan etki alanı adını yeniden adlandırır.

Hata olduğunda döndürülür:

* Yeniden adlandırılacak nesneleri sayısı 1000'den büyük
* Yeniden adlandırılacak ve uygulamalardan birinin bir çok kiracılı uygulamasıdır

### <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Neden etki alanı silme işlemi, bu etki alanı adına yönetilen Exchange gruplarını sahip olduğunu belirten bir hata ile başarısız oluyor?** <br>
**C:** Bugün, posta güvenlik grupları ve dağıtılmış listeleri gibi bazı grupları Exchange tarafından sağlanan ve el ile temizlenmesi gereken [Exchange yönetici Merkezi (EAC)](https://outlook.office365.com/ecp/). Var kalan ProxyAddresses üzerinde özel bir etki alanı adını kullanır ve başka bir etki alanı adını el ile güncelleştirilmesi gerekir. 

**S: Yönetici oturum\@contoso.com ancak "contoso.com" etki alanı adı silinemiyor?**<br>
**C:** Kullanıcı hesabı adını silmeye çalıştığınız özel etki alanı adına başvuramaz. Genel yönetici hesabını ilk varsayılan etki alanı adını kullandığından emin olun (. onmicrosoft.com) gibi admin@contoso.onmicrosoft.com. Oturum açın. farklı bir genel yönetici hesabı gibi admin@contoso.onmicrosoft.com veya başka bir özel etki alanı adı "hesabı olduğu fabrikam.com" gibi admin@fabrikam.com.

**S: Görmek ve Delete etki alanı düğmesi tıkladım `In Progress` silme işlemi için durumu. Ne kadar sürer? Başarısız olursa ne olur?**<br>
**C:** Etki alanı silme işlemi, etki alanı adı için tüm başvuruları yeniden adlandırır bir zaman uyumsuz bir arka plan görevdir. Bir veya iki dakika içinde tamamlanır. Etki alanı silme başarısız olursa olmadığından emin olun:

* Etki alanı adı appIdentifierURI ile yapılandırılan uygulamalar
* Tüm posta etkin bir grup özel etki alanı adına başvuran
* Etki alanı adı için 1000'den fazla başvuruları

Koşullardan biri karşılandığında henüz olduğunu fark ederseniz el ile başvuruları temizlemek ve etki alanı yeniden silmeyi deneyin.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Etki alanı adlarını yönetmek için PowerShell'i veya Graph API'sini kullanın

Azure Active Directory etki alanı adları için yönetim görevlerinin çoğunu Microsoft PowerShell kullanarak veya Azure AD Graph API'sini kullanarak program aracılığıyla tamamlanabilir.

* [PowerShell kullanarak Azure AD'de etki alanı adlarını yönetme](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Graph API'si kullanarak Azure AD'deki etki alanı adlarını yönetme](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Sonraki adımlar

* [Özel etki alanı adı ekleme](/azure/active-directory/fundamentals/add-custom-domain?context=azure/active-directory/users-groups-roles/context/ugr-context)
* [Exchange posta etkin güvenlik grupları Exchange Yönetim merkezinde Azure AD'de özel etki alanı üzerinde Kaldır](https://technet.microsoft.com/library/bb123521(v=exchg.160).aspx#Remove%20mail-enabled%20security%20groups)
* [Microsoft Graph API ile özel etki alanı ForceDelete](https://docs.microsoft.com/graph/api/domain-forcedelete?view=graph-rest-beta)
