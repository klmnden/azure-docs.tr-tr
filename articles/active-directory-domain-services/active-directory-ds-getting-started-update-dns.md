---
title: "Azure Active Directory Domain Services: Azure sanal ağı için DNS ayarlarını güncelleştirme | Microsoft Docs"
description: "Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 8bee2a25f196d645b27f30f21305b1550e44e07a
ms.contentlocale: tr-tr
ms.lasthandoff: 06/30/2017


---
<a id="update-dns-settings-for-the-azure-virtual-network" class="xliff"></a>

# Azure sanal ağı için DNS ayarlarını güncelleştirme
<a id="task-4-update-dns-settings-for-the-azure-virtual-network" class="xliff"></a>

## Görev 4: Azure sanal ağı için DNS ayarlarını güncelleştirme
Önceki yapılandırma görevlerinde dizininiz için Azure Active Directory Domain Services başarıyla etkinleştirdiniz. Sonraki göreviniz sanal ağınızdaki bilgisayarların bu hizmetlere bağlanabilmesini ve bu hizmetleri kullanabilmesini sağlamaktır. Bu makalede, sanal ağınızdaki DNS sunucusu ayarlarını, sanal ağda Azure Active Directory Domain Services’in kullanılabilir olduğu iki IP adresini işaret edecek şekilde güncelleştirin.

> [!NOTE]
> Azure Active Directory Domain Services'i dizin için etkinleştirdikten sonra, dizinin **Yapılandır** sekmesinde görüntülenen Azure Active Directory Domain Services'in IP adreslerini not edin.
>
>

Azure Active Directory Domain Services'i etkinleştirdiğiniz sanal ağın DNS sunucusu ayarını güncelleştirmek için aşağıdaki adımları uygulayın:

1. [Klasik Azure portalı](https://manage.windowsazure.com)'na gidin.
2. Sol bölmede **Ağlar**’ı seçin.  
    **Ağlar** penceresi açılır.

    ![Sanal ağlar penceresi](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. **Sanal Ağlar** sekmesinde, Azure Active Directory Domain Services'i etkinleştirdiğiniz sanal ağı seçerek özelliklerini görüntüleyin.
4. **Configure (Yapılandır)** sekmesine tıklayın.

    ![Sanal ağlar penceresi](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. **DNS sunucuları** bölümünde, dizininizin **Yapılandır** sekmesindeki **Domain Services** kısmında görüntülenen her iki IP adresini de girdiğinizden emin olun.
6. Bu sanal ağa ait DNS sunucusu ayarlarını kaydetmek için pencerenin alt kısmındaki görev bölmesinde **Kaydet**’e tıklayın.

   ![Sanal ağın DNS sunucusu ayarlarını güncelleştirme](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  Ağ üzerindeki sanal makineler, yeni DNS ayarlarını yalnızca yeniden başlatma işleminin ardından alabilir. Sanal makinelerin güncelleştirilen DNS ayarlarını doğrudan almasını istiyorsanız portal, PowerShell veya CLI aracılığıyla bir yeniden başlatma tetikleyin.
>
>

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
Görev 5: [Azure Active Directory Domain Services ile parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)

