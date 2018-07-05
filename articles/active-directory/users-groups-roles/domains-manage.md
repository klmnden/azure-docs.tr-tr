---
title: Azure Active Directory'de özel etki alanı adlarını yönetme | Microsoft Docs
description: Yönetim kavramlar ve Azure Active Directory'de bir etki alanı adı yönetmek için bilgi belgeleri
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 11/14/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.openlocfilehash: b503ef4c5cd922d4f7b58940cfc98a83a78ae986
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450318"
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Azure Active Directory'de özel etki alanı adlarını yönetme
Bir etki alanı adı için çok sayıda dizin kaynaklarını tanımlayıcının önemli bir parçasıdır: bir grup adresi bir parçası olan bir kullanıcı için bir kullanıcı adı veya e-posta adresi bir parçasıdır ve uygulamanın uygulama kimliği URI'SİNİN bir bölümü olabilir. Azure Active Directory'de (Azure AD) bir kaynak zaten kaynağı içeren dizine göre ait olarak doğrulanmış bir etki alanı adı ekleyebilirsiniz. Yalnızca bir genel yönetici, Azure AD'deki etki alanı yönetim görevlerini gerçekleştirebilirsiniz.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Azure AD dizininiz için birincil etki alanı adı ayarlayın
Dizininiz oluşturulurken 'contoso.onmicrosoft.com' gibi bir ilk etki alanı adı da birincil etki alanı adıdır. Yeni kullanıcı oluşturma, birincil etki alanı varsayılan etki alanı için yeni bir kullanıcı adıdır. Bir birincil etki alanı adı ayarlama Portalı'nda yeni kullanıcılar oluşturmak bir yöneticinin sürecini kolaylaştırır. Birincil etki alanı adını değiştirmek için:

1. Oturum [Azure portalında](https://portal.azure.com) dizin için genel yönetici olan bir hesapla.
2. **Azure Active Directory**'yi seçin.
3. Seçin **özel etki alanı adları**.
     
   ![Kullanıcı yönetimini açma](./media/domains-manage/add-custom-domain.png)
4. Birincil etki alanı olmasını istediğiniz etki alanının adını seçin.
5. Seçin **birincil yap** komutu. Sorulduğunda Seçiminizi onaylayın.
   
   ![Bir etki alanı adı birincil yap](./media/domains-manage/make-primary-domain.png)

Dizininiz birleşik bir doğrulanmış özel etki alanı için birincil etki alanı adını değiştirebilirsiniz. Dizininiz için birincil etki alanı değiştirmek, var olan tüm kullanıcılar kullanıcı adlarını değiştirmez.

## <a name="add-custom-domain-names-to-your-azure-ad-tenant"></a>Azure AD kiracınız ile özel etki alanı ekleme
900 yönetilen etki alanı adları en çok ekleyebilirsiniz. Şirket içi Active Directory ile Federasyon için etki alanları yapılandırıyorsanız, size her dizinde 450 etki alanı adı ekleyebilirsiniz. Daha fazla bilgi için [Federasyon ve yönetilen etki alanı adları](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain-concepts#federated-and-managed-domain-names).

## <a name="add-subdomains-of-a-custom-domain"></a>Özel bir etki alanının alt etki alanlarını ekleme
'Europe.contoso.com' gibi bir üçüncü düzey etki alanı adı dizininize eklemek istiyorsanız, öncelikle ekleyin ve ikinci düzey etki alanı, contoso.com gibi doğrulamanız gerekir. Alt etki alanı, Azure AD tarafından otomatik olarak doğrulanır. Yeni eklediğiniz alt etki alanı doğrulandı görmek için etki alanları listelenir tarayıcıda sayfayı yenileyin.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>DNS kayıt şirketi için özel etki alanınızın adını değiştirirseniz yapmanız gerekenler
DNS kayıt şirketi için özel etki alanınızın adını değiştirirseniz, özel etki alanı adınızı Azure AD'ye kendi kesinti olmadan ek yapılandırma görevleri ve kullanmaya devam edebilirsiniz. Office 365, Intune veya Azure AD'de özel etki alanı adları, diğer hizmetler ile özel etki alanınızın adını kullanırsanız, bu hizmetlerin belgelerine başvurun.

## <a name="delete-a-custom-domain-name"></a>Özel etki alanı Sil
Kuruluşunuz artık bu etki alanı adı kullanıyorsa veya başka bir Azure AD ile bu etki alanı adı kullanmanız gerekiyorsa, özel etki alanı, Azure AD'den silebilirsiniz.

Özel etki alanı adını silmek için önce dizininizdeki hiçbir kaynak etki alanı adına dayanan emin olmalısınız. Dizininizden, bir etki alanı adı silinemiyor:

* Herhangi bir kullanıcı, bir kullanıcı adı, e-posta adresi veya etki alanı adını içeren bir proxy adresi vardır.
* Herhangi bir grubu, bir e-posta adresi veya etki alanı adını içeren bir proxy adresi vardır.
* Herhangi bir uygulama, Azure AD'de bir uygulama kimliği URI'si, etki alanı adı içerir sahiptir.

Değiştirme veya özel etki alanı adını silmeden önce Azure AD dizininizde herhangi bir kaynağa silmeniz gerekir.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Etki alanı adlarını yönetmek için PowerShell'i veya Graph API'sini kullanın
Azure Active Directory etki alanı adları için yönetim görevlerinin çoğunu Microsoft PowerShell kullanarak veya Azure AD Graph API'sini kullanarak program aracılığıyla tamamlanabilir.

* [PowerShell kullanarak Azure AD'de etki alanı adlarını yönetme](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Graph API'si kullanarak Azure AD'deki etki alanı adlarını yönetme](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Sonraki adımlar
* [Özel etki alanı adı ekleme](../fundamentals/add-custom-domain.md)

