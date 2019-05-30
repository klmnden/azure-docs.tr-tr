---
title: Azure Red Hat OpenShift için Azure AD kiracısı oluşturma | Microsoft Docs
description: Microsoft Azure Red Hat OpenShift kümenizi barındırmak için bir Azure Active Directory (Azure AD) kiracısı oluşturmayı açıklanmıştır.
author: tylermsft
ms.author: twhitney
ms.service: container-service
manager: jeconnoc
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: 04d710f4d60b776f8059d87ea4d009bed6f7f8ba
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65551696"
---
# <a name="create-an-azure-ad-tenant-for-azure-red-hat-openshift"></a>Azure Red Hat OpenShift için Azure AD kiracısı oluşturma

Microsoft Azure Red Hat OpenShift gerektiren bir [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) kümenizi oluşturmak, Kiracı. A *Kiracı* bir kuruluş ya da uygulama geliştiricisi bir ilişki ile Microsoft kaydolarak Azure, Microsoft Intune veya Microsoft 365 oluşturduklarında aldığı Azure AD adanmış örneğidir. Her Azure AD kiracısı farklıdır ve ayrı diğer Azure ad Kiracı ve kendi iş ve Okul kimlikleri ve uygulama kayıtları vardır.

Azure AD kiracısı zaten yoksa, oluşturmak için bu yönergeleri izleyin.

## <a name="create-a-new-azure-ad-tenant"></a>Yeni Azure AD kiracısı oluşturma

Bir kiracı oluşturmak için:

1. Oturum [Azure portalında](https://portal.azure.com/) Azure Red Hat OpenShift kümenizle ilişkilendirmek istediğiniz hesabı kullanarak.
2. Açık [Azure Active Directory dikey](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) yeni bir kiracı oluşturmak için (diğer adıyla yeni *Azure Active Directory*).
3. Sağlayan bir **kuruluş adı**.
4. Sağlayan bir **ilk etki alanı adı**. Bu olacaktır *onmicrosoft.com* eklenmiş. Değeri yeniden kullanabileceğiniz *kuruluş adı* burada.
5. Bir ülke veya Kiracı oluşturulacağı bölgeyi seçin.
6. **Oluştur**’a tıklayın.
7. Azure AD kiracınıza oluşturulduktan sonra seçin **yeni dizininizi yönetmek için buraya tıklayın** bağlantı. Yeni Kiracı adınızın-sağ üst kısmındaki Azure portalında görüntülenmesi gerekir:  

    ![Portalın sağ üst köşede Kiracı adını gösteren ekran görüntüsü][tenantcallout]  

8. Not *Kiracı kimliği* böylece daha sonra Azure Red Hat OpenShift kümenizi oluşturmak nereye belirtebilirsiniz. Portalda yeni kiracınız için artık Azure Active Directory genel bakış dikey penceresini görmeniz gerekir. Seçin **özellikleri** ve değeri kopyalayın, **dizin kimliği**. Bu değer anılacaktır `TENANT` içinde [Azure Red Hat OpenShift küme oluşturma](tutorial-create-cluster.md) öğretici.

[tenantcallout]: ./media/howto-create-tenant/tenant-callout.png

## <a name="resources"></a>Kaynaklar

Kullanıma [Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/) hakkında daha fazla bilgi için [Azure AD kiracılarıyla](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Hizmet sorumlusu oluşturma, bir istemci gizli anahtarı ve kimlik doğrulama geri çağırma URL'si oluşturun ve Azure Red Hat OpenShift kümenizdeki uygulamaları test etmek için yeni bir Active Directory kullanıcı oluşturma hakkında bilgi edinin.

[Bir Azure AD uygulama nesnesi ve kullanıcı oluşturma](howto-aad-app-configuration.md)