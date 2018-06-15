---
title: Azure Active Directory'de özel etki alanı adlarını yönetme | Microsoft Docs
description: Yönetim Kavramları ve Azure Active Directory'de bir etki alanı adı yönetmek için nasıl yapılır?
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
ms.openlocfilehash: 81c2371d5dbb17399071c80ff4e8b81813ed014c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33762403"
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Azure Active Directory'de özel etki alanı adlarını yönetme
Bir etki alanı adı tanımlayıcının birçok dizin kaynaklar için önemli bir parçasıdır: kullanıcı, bir grup adresi parçası için bir kullanıcı adı veya e-posta adresi bir parçasıdır ve bir uygulama için uygulama kimliği URI'SİNİN parçası olabilir. Azure Active Directory'de (Azure AD) bir kaynak, kaynak içeren dizine ait olarak zaten doğrulanmış bir etki alanı adı ekleyebilirsiniz. Yalnızca genel yönetici Azure AD etki alanı yönetimi görevlerini gerçekleştirebilir.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Azure AD dizininiz için birincil etki alanı adı ayarlama
Dizininizi oluşturulduğunda 'contoso.onmicrosoft.com,' gibi bir ilk etki alanı adı da birincil etki alanı adıdır. Yeni bir kullanıcı oluşturduğunuzda, birincil etki alanı varsayılan etki alanı için yeni bir kullanıcı adıdır. Bir birincil etki alanı adı ayarlama Portalı'nda yeni kullanıcılar oluşturmak bir yöneticinin işlemini kolaylaştırır. Birincil etki alanı adını değiştirmek için:

1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. **Azure Active Directory**'yi seçin.
3. Seçin **özel etki alanı adları**.
     
   ![Açılış kullanıcı yönetimi](./media/active-directory-domains-manage-azure-portal/add-custom-domain.png)
4. Birincil etki alanı olmasını istediğiniz etki alanının adını seçin.
5. Seçin **birincil olun** komutu. Sorulduğunda Seçiminizi onaylayın.
   
   ![Bir etki alanı adı birincil olun](./media/active-directory-domains-manage-azure-portal/make-primary-domain.png)

Birleşik olmadığı herhangi doğrulanmış özel etki alanı olmasını dizininiz için birincil etki alanı adını değiştirebilirsiniz. Dizininiz için birincil etki alanı değiştirme var olan tüm kullanıcılar kullanıcı adlarını değiştirmez.

## <a name="add-custom-domain-names-to-your-azure-ad-tenant"></a>Azure AD kiracınız için özel etki alanı adlarını Ekle
900 yönetilen etki alanı adlarının maksimum kadar ekleyebilirsiniz. Şirket içi Active Directory ile Federasyon, etki alanları yapılandırıyorsanız, her dizinde 450 etki alanı adlarının maksimum ekleyebilirsiniz. Daha fazla bilgi için bkz: [federe ve yönetilen etki alanı adları](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain-concepts#federated-and-managed-domain-names).

## <a name="add-subdomains-of-a-custom-domain"></a>Özel bir etki alanının alt etki alanlarını ekleme
'Europe.contoso.com' gibi bir üçüncü düzey etki alanı adı dizininize eklemek istiyorsanız, önce ekleyin ve contoso.com gibi ikinci düzey etki alanı doğrulamanız gerekir. Alt etki alanı otomatik olarak Azure AD tarafından doğrulanır. Yeni eklediğiniz alt etki alanı doğrulandı görmek için etki alanları listelenir tarayıcıda sayfayı yenileyin.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Özel etki alanı adınız için DNS kayıt şirketi değiştirirseniz yapmanız gerekenler
Özel etki alanı adınız için DNS kayıt şirketi değiştirirseniz, Azure AD ile kendini kesinti olmadan ek yapılandırma görevleri ve özel etki alanı adınızı kullanmaya devam edebilirsiniz. Office 365, Intune veya Azure AD içinde özel etki alanı adları kullanan diğer hizmetler ile özel etki alanı adınızı kullanırsanız, bu hizmetlerin belgelerine bakın.

## <a name="delete-a-custom-domain-name"></a>Özel etki alanı silme
Kuruluşunuz artık bu etki alanı adını kullanıyorsa ya da bu etki alanı adı başka bir Azure AD ile kullanmanız gerekiyorsa, özel etki alanı adı, Azure AD'den silebilirsiniz.

Bir özel etki alanı adı silmek için önce dizininizdeki hiçbir kaynak etki alanı adını kullanan emin olmalısınız. Bir etki alanı adı, varsa dizininizden silinemiyor:

* Herhangi bir kullanıcı, bir kullanıcı adı, e-posta adresi veya etki alanı adını içeren proxy adresi vardır.
* Herhangi bir grup, bir e-posta adresi veya etki alanı adını içeren proxy adresi vardır.
* Azure ad herhangi bir uygulama, bir uygulama etki alanı adını içeren kimliği URI sahiptir.

Değiştirme veya özel etki alanı adını silmeden önce herhangi bir kaynağa Azure AD dizininizi silmeniz gerekir.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Etki alanı adlarını yönetmek için PowerShell veya grafik API'sini kullanın
Azure Active Directory etki alanı adları için yönetim görevlerinin çoğunu Microsoft PowerShell veya program aracılığıyla Azure AD Graph API kullanarak tamamlanabilir.

* [Azure AD içinde etki alanı adlarını yönetmek için PowerShell kullanma](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Azure AD içinde etki alanı adlarını yönetmek için grafik API'si kullanma](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Sonraki adımlar
* [Özel etki alanı adı ekleme](add-custom-domain.md)

