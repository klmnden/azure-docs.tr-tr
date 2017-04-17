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
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: abb27292d4b5533fe6f3d66d6921fea8c82f18dd
ms.lasthandoff: 04/12/2017


---
# <a name="update-dns-settings-for-the-azure-virtual-network"></a>Azure sanal ağı için DNS ayarlarını güncelleştirme
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Görev 4: Azure sanal ağı için DNS ayarlarını güncelleştirme
Önceki yapılandırma görevlerinde dizininiz için Azure Active Directory Domain Services başarıyla etkinleştirdiniz. Sonraki göreviniz sanal ağınızdaki bilgisayarların bu hizmetlere bağlanabilmesini ve bu hizmetleri kullanabilmesini sağlamaktır. Bu makalede, sanal ağınızdaki DNS sunucusu ayarlarını, sanal ağda Azure Active Directory Domain Services’in kullanılabilir olduğu iki IP adresini işaret edecek şekilde güncelleştirin.

> [!NOTE]
> Azure Active Directory Domain Services'i dizin için etkinleştirdikten sonra, dizinin **Yapılandır** sekmesinde görüntülenen Azure Active Directory Domain Services'in IP adreslerini not edin.
>
>

Azure Active Directory Domain Services’i etkinleştirdiğiniz sanal ağın DNS sunucusu ayarını güncelleştirmek için aşağıdaki işlemleri yapın:

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
> Sanal ağın DNS sunucusu ayarlarını güncelleştirdikten sonra, ağdaki sanal makinelerin güncelleştirilen DNS yapılandırmasını almaları biraz sürebilir. Bir sanal makine etki alanına bağlanamıyorsa, sanal makinedeki DNS önbelleğini boşaltabilirsiniz ('ipconfig /flushdns'). Bu komut, sanal makinedeki DNS ayarlarını yenilenmeye zorlar.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Görev 5: [Azure Active Directory Domain Services ile parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)

