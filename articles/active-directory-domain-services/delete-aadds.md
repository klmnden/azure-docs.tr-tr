---
title: Azure Active Directory etki alanı hizmetleri devre dışı bırakma | Microsoft Docs
description: Azure Active Directory etki alanı Azure portalını kullanarak hizmetleri devre dışı bırak
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: mstephen
ms.openlocfilehash: 122caddd73219060689ac8f6e3ab6408ea99a422
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66246232"
---
# <a name="disable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory etki alanı Azure portalını kullanarak hizmetleri devre dışı bırak
Bu makalede Azure AD dizininiz için Azure Active Directory (AD) etki alanı hizmetleri devre dışı bırakmak için Azure portalını kullanmayı gösterir.

> [!WARNING]
> **Silme işlemi kalıcıdır ve geri alınamaz.**
> Dikkatli olun! Yönetilen etki alanı sildiğinizde:
>   * Yönetilen etki alanı için etki alanı denetleyicilerine XML'deki sağlanan ve sanal ağdan kaldırıldı.
>   * Yönetilen etki alanındaki verileri kalıcı olarak silinir. Bu özel OU'ları, GPO'ları, özel DNS kayıtları, hizmet sorumluları, yönetilen etki alanında oluşturduğunuz Gmsa'lar vb. içerir.
>   * Yönetilen etki alanına katılan makineler kendi güven ilişkisine sahip kaybedebilir ve etki alanına katılmamış olmanız gerekir.
>   * Kurumsal AD kimlik bilgilerini kullanarak bu makinelerde oturum açamazsınız. Yerel yönetici kimlik bilgileri makine için kullanın.
> Yönetilen etki alanı siliniyor Azure AD dizininizi silmeyin veya dizin olumsuz Aksi takdirde.

Azure AD Domain Services yönetilen etki alanınızı silmek için aşağıdaki adımları gerçekleştirin:
1. Gidin [Azure AD Domain Services Uzantısı](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında.
2. Yönetilen etki alanınıza adına tıklayın.

    ![Silmek için etki alanı seçin](./media/getting-started/domain-services-delete-select-domain.png)

3. Üzerinde **genel bakış** sayfasında **Sil** düğmesi.

    ![Etki alanını Sil](./media/getting-started/domain-services-delete-domain.png)

4. Silme işlemini onaylamak için yönetilen etki alanı DNS etki alanı adını yazın. Tıklayın **Sil** işiniz bittiğinde düğmesi.

    ![Etki alanı onayını Sil](./media/getting-started/domain-services-delete-domain-confirm.png)

Yönetilen etki alanı yaklaşık 15-20 dakika içinde silinir.

Göz önünde bulundurun [geri bildirim paylaşma](contact-us.md) özellikleri yardımcı olan anlamamıza yardımcı olmak için Azure AD Domain Services'ı gelecekte seçtiniz. Bu geri bildirim hizmetini kullanım örnekleri ve dağıtım gereksinimlerini daha iyi uyacak şekilde yardımcı olur.
