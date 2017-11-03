---
title: "Azure Active Directory'ye yeni kullanıcı ekleme | Microsoft Belgeleri"
description: "Azure Active Directory'ye yeni kullanıcı ekleme açıklanmaktadır."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 9b6a48220132bb8ea18ae5efca46ea2faf825806
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="quickstart-add-new-users-to-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory'ye yeni kullanıcı ekleme
Bu makalede Azure Active Directory'de (Azure AD), kuruluşunuzdaki yeni kullanıcı ekleme açıklayan bir Azure portalını kullanarak bir zamanda veya şirket içi Windows Server AD kullanıcı hesabı verilerinizi eşitleyerek. 

## <a name="add-cloud-based-users"></a>Bulut tabanlı kullanıcıları ekleme
1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **Azure Active Directory** ve ardından **kullanıcılar ve gruplar**.
3. Üzerinde **kullanıcılar ve gruplar**seçin **tüm kullanıcılar**ve ardından **yeni kullanıcı**.
   ![Ekle komutu seçme](./media/add-users-azure-active-directory/add-user.png)
4. Kullanıcı için Ayrıntılar gibi girin **adı** ve **kullanıcı adı**. Kullanıcı adı etki alanı adı kısmını ya da gereken ilk varsayılan etki alanı adı "[etki alanı adı].onmicrosoft.com" veya doğrulanmış, Federasyon olmayan olması [özel etki alanı adı](add-custom-domain.md) "contoso.com" gibi
5. Kopyalama veya aksi takdirde bu işlem tamamlandıktan sonra kullanıcıya sağlayabilir böylece oluşturulmuş kullanıcı parolası not edin.
6. İsteğe bağlı olarak, açın ve bilgileri doldurun **profil**, **grupları**, veya **dizin rolünü** kullanıcı için. Kullanıcı ve yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure AD'de yönetici rolü atama](active-directory-assign-admin-roles.md).
7. Üzerinde **kullanıcı**seçin **oluşturma**.
8. Böylece kullanıcı oturum açarak yeni kullanıcı için oluşturulmuş bir parola güvenli bir şekilde dağıtın.

> [!TIP]
> Ayrıca şirket içi Windows Server AD kullanıcı hesabı verileri eşitleyebilirsiniz. Microsoft'un kimlik çözümleri şirket içi ve bulut tabanlı özellikleri, kimlik doğrulama ve yetkilendirme konum bağımsız olarak tüm kaynaklar için tek bir kullanıcı kimliğini oluşturma span. Bu karma kimlik diyoruz. [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) şirket içi dizinlerinizi Azure Active Directory ile karma kimlik senaryoları için tümleştirmek için kullanılabilir. Bu sayede kullanıcılarınıza Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik sağlayabilirsiniz. 

## <a name="delete-users-from-azure-ad"></a>Azure AD kullanıcıları Sil
1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **kullanıcılar ve gruplar**.
3. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, listeden silmek istediğiniz kullanıcıyı seçin. 
4. Seçilen kullanıcı için dikey seçin **genel bakış**ve ardından komut çubuğunda **silmek**.
   ![Ekle komutu seçme](./media/add-users-azure-active-directory/delete-user.png)


### <a name="learn-more"></a>Daha fazla bilgi edinin 
* [Bir dış kullanıcı ekleme](active-directory-users-create-external-azure-portal.md)

* [Bir kullanıcı, Azure AD'de rol atama](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıç Azure AD Premium için yeni kullanıcı ekleme öğrendiniz. 

Azure portalından Azure AD'de yeni bir kullanıcı oluşturmak için aşağıdaki bağlantıyı kullanın.

> [!div class="nextstepaction"]
> [Kullanıcıları Azure AD’ye ekleme](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 