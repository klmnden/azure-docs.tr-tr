---
title: "Azure AD Etki Alanı Hizmetleri: Azure sanal ağı için DNS ayarlarını güncelleştirme| Microsoft Belgeleri"
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
ms.date: 09/21/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 81c0564390c57c7ee001ce5a574ab670faf898b4


---
# <a name="azure-ad-domain-services-update-dns-settings-for-the-azure-virtual-network"></a>Azure AD Etki Alanı Hizmetleri - Azure sanal ağı için DNS ayarlarını güncelleştirme
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Görev 4: Azure sanal ağı için DNS ayarlarını güncelleştirme
Önceki yapılandırma görevlerinde dizininiz için Azure AD Etki Alanı Hizmetlerini başarıyla etkinleştirdiniz. Sonraki göreviniz sanal ağınızdaki bilgisayarların bu hizmetlere bağlanabilmesini ve bu hizmetleri kullanabilmesini sağlamaktır. Sanal ağınızdaki DNS sunucusu ayarlarını, sanal ağda Azure AD Etki Alanı Hizmetlerinin kullanılabilir olduğu iki IP adresini işaret edecek şekilde güncelleştirin.

> [!NOTE]
> Azure AD Etki Alanı Hizmetleri'ni dizininiz için etkinleştirdikten sonra, dizinin **Yapılandır** sekmesinde görüntülenen Azure AD Etki Alanı Hizmetleri'nin IP adreslerini not edin.
> 
> 

Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz sanal ağın DNS sunucusu ayarını güncelleştirmek için aşağıdaki yapılandırma adımlarını uygulayın.

1. **Klasik Azure portalına** ([https://manage.windowsazure.com](https://manage.windowsazure.com)) gidin.
2. Sol bölmede **Ağlar** düğümünü seçin.
   
    ![Sanal ağlar düğümü](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. **Sanal Ağlar** sekmesinde, Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz sanal ağı seçerek özelliklerini görüntüleyin.
4. **Configure (Yapılandır)** sekmesine tıklayın.
   
    ![Sanal ağlar düğümü](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. **DNS sunucuları** bölümünde, Azure AD Etki Alanı Hizmetleri'nin IP adreslerini girin.
6. Dizininizin **Yapılandır** sekmesindeki **Etki Alanı Hizmetleri**'nde görüntülenen her iki IP adresini de girdiğinizden emin olun.
7. Bu sanal ağa ait DNS sunucusu ayarlarını kaydetmek için sayfanın alt kısmındaki görev bölmesinde **Kaydet**’e tıklayın.
   
   ![Sanal ağın DNS sunucusu ayarlarını güncelleştirin.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
> Sanal ağın DNS sunucusu ayarları güncelleştirildikten sonra, ağdaki sanal makinelerin güncelleştirilen DNS yapılandırmasını almaları biraz sürebilir. Bir sanal makine etki alanına bağlanamıyorsa sanal makinedeki DNS ayarlarının yenilenmesini zorlamak için DNS önbelleğini boşaltabilirsiniz (ör. 'ipconfig /flushdns') sanal makine üzerinde. Bu komut, sanal makinedeki DNS ayarlarını yenilenmeye zorlar.
> 
> 

## <a name="task-5-enable-password-synchronization-to-azure-ad-domain-services"></a>Görev 5 - Azure AD Etki Alanı Hizmetleri için parola eşitlemeyi etkinleştirme
Bir sonraki yapılandırma görevi, [Azure AD Etki Alanı Hizmetleri için parola eşitlemeyi etkinleştirmedir](active-directory-ds-getting-started-password-sync.md).




<!--HONumber=Nov16_HO2-->


