---
title: Azure Veri Kataloğu önkoşulları
description: Azure veri Kataloğu ile çalışmaya başlamak için gereken önkoşulları hakkında bilgi edinin.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 788e7a453e9879d9c901448659ff1c8cfa7201ff
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57548328"
---
# <a name="azure-data-catalog-prerequisites"></a>Azure Veri Kataloğu önkoşulları

Birkaç şey Azure veri Kataloğu ayarlamadan önce halletmeniz gerekir. Endişelenmeyin, bu işlem uzun sürmez.

## <a name="azure-subscription"></a>Azure aboneliği
Veri Kataloğu'nu ayarlamak için sahibi veya bir Azure aboneliğinin ortak sahibi olmalıdır.

Azure Abonelikleri, veri Kataloğu gibi bulut hizmeti kaynaklarına erişimi düzenlemenize yardımcı olur. Abonelikler nasıl kaynak kullanımı bildirilen faturalandırılır ve ödendiği denetlemenize yardımcı. Her abonelikte, abonelikleri ve planları, departman, proje, bölgesel ofise vb. göre farklı olabilir bir ayrı faturalandırma ve ödeme Kurulum olabilir. Her bulut hizmetine bir aboneliği aittir ve, veri Kataloğu'nu önce bir aboneliğe sahip olmanız gerekir. Daha fazla bilgi için bkz. [Hesapları, abonelikleri ve yönetici rollerini yönetme](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Veri Kataloğu'nu ayarlamak için bir Azure Active Directory (Azure AD) kullanıcı hesabıyla oturum açmanız gerekir.

Azure AD işletmenizin kimlik ve erişimi hem bulutta hem de şirket içinde yönetmesi için kolay bir yöntem sağlar. Kullanıcılar için çoklu oturum açma için herhangi bir buluttaki kullanabilir tek bir iş veya Okul hesabı ve şirket içi web uygulaması. Veri Kataloğu, oturum açma kimliğini doğrulamak için Azure AD kullanır. Daha fazla bilgi için bkz. [Azure Active Directory nedir?](../active-directory/fundamentals/active-directory-whatis.md).

> [!NOTE]
> Kullanarak [Azure portalında](https://portal.azure.com/), oturum açmak kişisel bir Microsoft hesabı ya da bir Azure Active Directory ile oturum iş veya Okul hesabı. Azure portalını kullanarak veri Kataloğu ayarlamak için veya [veri Kataloğu portalı](https://www.azuredatacatalog.com), bir kişisel hesap bir Azure Active Directory hesabıyla oturum açmanız gerekir.
>
>

## <a name="active-directory-policy-configuration"></a>Active Directory ilke yapılandırması
Veri Kataloğu portalında oturum açabildiğiniz, ancak veri kaynağı kayıt aracında oturum açmaya çalıştığında bir durum karşılaşabilirsiniz, açmasını engelleyen bir hata iletisiyle karşılaşırsınız. Bu sorun davranış yalnızca şirket ağında olduğunuz veya yalnızca gelen şirket ağının dışından bağlantı kurduğunuz olduğunda ortaya çıkabilir gerçekleşebilir.

Veri kaynağı kayıt aracını, Active Directory, kullanıcı kimlik bilgilerini doğrulamak için form kimlik doğrulaması kullanır. Bir Active Directory Yöneticisi başarıyla oturum açmanıza yardımcı olmak için genel kimlik doğrulama ilkesini form kimlik doğrulamasını etkinleştirmeniz gerekir.

Genel kimlik doğrulama ilkesinde kimlik doğrulama yöntemleri ayrı olarak intranet ve extranet bağlantıları için aşağıdaki ekran görüntüsünde gösterildiği gibi etkinleştirilebilir. Bağlantı kurduğunuz ağ için form kimlik doğrulaması etkin değilse oturum açma hataları oluşabilir.

 ![Active Directory genel kimlik doğrulama İlkesi](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [kimlik doğrulama ilkelerini yapılandırma](https://technet.microsoft.com/library/dn486781.aspx).
