---
title: Erişim ve Azure Active Directory - yeni bir kiracı oluşturmak için hızlı başlangıç | Microsoft Docs
description: Azure Active Directory bulma ve kuruluşunuz için yeni bir kiracı oluşturma hakkında yönergeler.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f970e9d43bc6dd67fe55ca9cdcfcae716c9f166a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110614"
---
# <a name="quickstart-create-a-new-tenant-in-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory’de yeni bir kiracı oluşturma
Kuruluşunuz için yeni bir kiracı oluşturulması da dahil olmak üzere, Azure Active Directory (Azure AD) portalı kullanarak tüm yönetim görevlerinizi gerçekleştirebilirsiniz. 

Bu hızlı başlangıçta, Azure portala ve Azure Active Directory’ye nasıl erişeceğinizi ve kuruluşunuz için temel bir kiracının nasıl oluşturulacağını öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Genel yönetici hesabını kullanarak kuruluşunuzun [Azure portalda](https://portal.azure.com/) oturum açın.

![Azure AD seçeneği ile Azure portal ekran](media/active-directory-access-create-new-tenant/azure-ad-portal.png)

## <a name="create-a-new-tenant-for-your-organization"></a>Kuruluşunuz için yeni bir kiracı oluşturma
Azure portalda oturum açtıktan sonra kuruluşunuz için yeni bir kiracı oluşturabilirsiniz. Yeni kiracınız kuruluşunuzu temsil eder ve şirket içindeki ve dışındaki kullanıcılarınız için belirli bir Microsoft bulut hizmetleri örneğini yönetmenize yardımcı olur.

### <a name="to-create-a-new-tenant"></a>Yeni kiracı oluşturmak için
1. Seçin **kaynak Oluştur**seçin **kimlik**ve ardından **Azure Active Directory**.

    **Dizin oluştur** sayfası görüntülenir.

    ![Azure Active Directory Oluştur sayfası](media/active-directory-access-create-new-tenant/azure-ad-create-new-tenant.png)

2.  **Dizin oluştur** sayfasına aşağıdaki bilgileri girin:
    
    - **Kuruluş adı** kutusuna _Contoso_ yazın.

    - **İlk etki alanı adı** kutusuna _Contoso_ yazın.

    - **Ülke veya bölge** kutusunda _Amerika Birleşik Devletleri_ seçeneğini değiştirmeden bırakın.

3. **Oluştur**’u seçin.

contoso.onmicrosoft.com etki alanıyla yeni kiracınız oluşturulur.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak kiracıyı silebilirsiniz:

- **Azure Active Directory** seçeneğini belirleyin ve sonra **Contoso - Genel Bakış** sayfasında **Dizini sil**’i seçin.

    Kiracı ve ilişkili bilgileri silinir.

    ![Vurgulanan silme directory düğmesiyle genel bakış sayfası](media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)

## <a name="next-steps"></a>Sonraki adımlar
- Etki alanı adlarını değiştirme veya ekleme; bkz. [Azure Active Directory’ye özel etki alanı adı ekleme](add-custom-domain.md)

- Kullanıcı ekleme; bkz. [Yeni kullanıcı ekleme veya silme](add-users-azure-active-directory.md)

- Gruplar ve üyeler ekleme; bkz. [Temel bir grup oluşturma ve üyeler ekleme](active-directory-groups-create-azure-portal.md)

- Hakkında bilgi edinin [Privileged Identity Management'ı kullanarak rol tabanlı erişim](../../role-based-access-control/pim-azure-resource.md) ve [koşullu erişim](../../role-based-access-control/conditional-access-azure-management.md) kuruluşunuzun uygulama ve kaynak erişimi yönetmenize yardımcı olmak için.

- [Temel lisans bilgileri, terminoloji ve ilişkili özellikleri](active-directory-whatis.md) dahil olmak üzere Azure AD hakkında bilgi edinin.
