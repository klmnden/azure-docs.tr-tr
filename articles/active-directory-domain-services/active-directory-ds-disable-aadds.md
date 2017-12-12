---
title: "Azure Active Directory etki alanı hizmetleri devre dışı bırakma | Microsoft Docs"
description: "Azure Active Directory etki alanı Azure portalını kullanarak hizmetleri devre dışı bırak"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: maheshu
ms.openlocfilehash: f61f6df85e47bec32e147990d956a4409429a60c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="disable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory etki alanı Azure portalını kullanarak hizmetleri devre dışı bırak
Bu makalede Azure portalında Azure AD dizininiz için Azure Active Directory (AD) etki alanı Hizmetleri'ni devre dışı bırakmak için nasıl kullanılacağı gösterilmektedir.

> [!WARNING]
> **Silme işlemi kalıcıdır ve geri alınamaz.**
> Dikkatli ilerleyin! Yönetilen etki alanı sildiğinizde:
  * Yönetilen etki alanı için etki alanı denetleyicileri XML'deki sağlanan ve sanal ağdan kaldırıldı.
  * Yönetilen etki alanı veriler kalıcı olarak silinir. Bu özel OU'ları, GPO'ları, özel DNS kayıtları, hizmet asıl adı, yönetilen etki alanında oluşturulan bir Gmsa'lar vb. içerir.
  * Yönetilen etki alanına katılan makineler kendi etki alanı ile güven ilişkisi kaybeder ve etki alanına katılmamış olması gerekir.
  * Kurumsal AD kimlik bilgilerini kullanarak bu makinelerin oturum açamaz. Yerel yönetici kimlik bilgileri makine için kullanın.
Yönetilen etki alanı silindiğinde Azure AD dizininizi silmeyin veya dizin olumsuz Aksi takdirde.
>

Azure AD etki alanı Hizmetleri yönetilen etki alanını silmek için aşağıdaki adımları gerçekleştirin:
1. Gidin [Azure AD etki alanı Hizmetleri Uzantısı](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında.
2. Yönetilen etki alanınızın adını tıklatın.

    ![Silmek için etki alanını seçin](./media/getting-started/domain-services-delete-select-domain.png)

3. Üzerinde **genel bakış** sayfasında, **silmek** düğmesi.

    ![Etki alanını sil](./media/getting-started/domain-services-delete-domain.png)

4. Silmeyi onaylamak için yönetilen etki alanının DNS etki alanı adını yazın. ' I tıklatın **silmek** işiniz bittiğinde düğmesine tıklayın.

    ![Etki alanı onayını Sil](./media/getting-started/domain-services-delete-domain-confirm.png)

Yönetilen etki alanı yaklaşık 15-20 dakika içinde silinir.

Göz önünde bulundurun [geri bildirim paylaşımı](active-directory-ds-contact-us.md) özellikleri yardımcı olacak anlamanıza yardımcı olmak için Azure AD etki alanı Hizmetleri gelecekte seçtiniz. Bu geri bildirim hizmeti kullanım örnekleri ve dağıtım gereksinimlerini daha iyi uyacak şekilde gelişmesi yardımcı olur.
