---
title: "Azure veri Kataloğu önkoşulları | Microsoft Docs"
description: "Azure veri Kataloğu ile çalışmaya başlamak gereken önkoşulları hakkında bilgi edinin."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/01/2017
ms.author: maroche
ms.openlocfilehash: cf32ef4c80fa0ee68ce3dc1289467a419aab39c9
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="azure-data-catalog-prerequisites"></a>Azure Veri Kataloğu önkoşulları

Birkaç Azure veri Kataloğu ayarlamadan önce ilgilenebilmek gerekir. Endişelenmeyin, bu işlem uzun sürmez.

## <a name="azure-subscription"></a>Azure aboneliği
Veri Kataloğu'nu ayarlamak için sahibi veya bir Azure aboneliğinin ortak sahibi olmalıdır.

Azure Abonelikleri, veri Kataloğu gibi bulut hizmeti kaynaklarına erişimi düzenlemenize yardımcı olur. Abonelikleri nasıl kaynak kullanımı bildirilen faturalandırılır ve ödendiği denetlemenize yardımcı. Abonelikler ve departman, proje, bölgesel ofise vb. göre farklılık planları alacak şekilde her abonelik bir ayrı faturalandırma ve ödeme ayarına sahip olabilir. Her bir bulut hizmeti aboneliği aittir ve, veri Kataloğu'nu ayarlamak önce bir aboneliğe sahip olmanız gerekir. Daha fazla bilgi için bkz. [Hesapları, abonelikleri ve yönetici rollerini yönetme](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Veri Kataloğu'nu ayarlamak için bir Azure Active Directory (Azure AD) kullanıcı hesabıyla oturum açtığınızdan gerekir.

Azure AD işletmenizin kimlik ve erişimi hem bulutta hem de şirket içinde yönetmesi için kolay bir yöntem sağlar. Kullanıcılar için çoklu oturum açma herhangi buluta kullanabilir tek bir iş veya Okul hesabı ve şirket içi web uygulaması. Veri Kataloğu, oturum açma kimliğini doğrulamak için Azure AD kullanır. Daha fazla bilgi için bkz: [Azure Active Directory nedir?](../active-directory/active-directory-whatis.md).

> [!NOTE]
> Kullanarak [Azure portal](http://portal.azure.com/), oturum kişisel bir Microsoft hesabı veya bir Azure Active Directory oturum iş veya Okul hesabı. Veri kataloğu kullanarak Azure portalında ayarlamak için veya [veri Kataloğu portalını](http://www.azuredatacatalog.com), bir kişisel hesap bir Azure Active Directory hesabıyla oturum açması gerekir.
>
>

## <a name="active-directory-policy-configuration"></a>Active Directory ilke yapılandırması
Bir durum, veri Kataloğu portalında oturum açabildiğiniz, ancak veri kaynağı kayıt aracında oturum açmaya çalıştığınızda karşılaşabileceğiniz, açmasını engelleyen bir hata iletisiyle karşılaşırsınız. Bu sorunu davranış yalnızca şirket ağında olduğunuz veya yalnızca gelen şirket ağının dışından bağlantı kurduğunuz olduğunda ortaya çıkabilir gerçekleşebilir.

Veri kaynağı kayıt aracını, Active Directory, kullanıcı kimlik bilgilerini doğrulamak için form kimlik doğrulaması kullanır. Active Directory Yöneticisi başarıyla oturum açmanıza yardımcı olmak için genel kimlik doğrulama ilkesini form kimlik doğrulamasını etkinleştirmeniz gerekir.

Genel kimlik doğrulama ilkesinde kimlik doğrulama yöntemlerini ayrı olarak intranet ve extranet bağlantıları için aşağıdaki ekran görüntüsünde gösterildiği gibi etkinleştirilebilir. Bağlantı kurduğunuz ağ için form kimlik doğrulaması etkin değilse oturum açma hataları oluşabilir.

 ![Active Directory genel kimlik doğrulama İlkesi](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [kimlik doğrulaması ilkelerini yapılandırma](https://technet.microsoft.com/library/dn486781.aspx).
