---
title: 'Azure Active Directory etki alanı Hizmetleri: Azure sanal ağı için DNS ayarlarını güncelleştirme | Microsoft Docs'
description: Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: iainfou
ms.openlocfilehash: ef0c25227e98d066d75c9adb62af3f602e35db23
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67474151"
---
# <a name="enable-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services’ı etkinleştirme

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Görev 4: Azure sanal ağı için DNS ayarlarını güncelleştirme
Önceki yapılandırma görevlerinde dizininiz için Azure Active Directory Domain Services başarıyla etkinleştirdiniz. Ardından, sanal ağ içindeki bilgisayarları bu hizmetlere bağlanacak ve bu hizmetleri kullanacak şekilde etkinleştirin. Bu makalede, sanal ağınızdaki DNS sunucusu ayarlarını, sanal ağda Azure Active Directory Domain Services’in kullanılabilir olduğu iki IP adresini işaret edecek şekilde güncelleştirin.

Azure Active Directory Domain Services'i etkinleştirdiğiniz sanal ağın DNS sunucusu ayarını güncelleştirmek için aşağıdaki adımları tamamlayın:


1. **Genel Bakış** sekmesinde, yönetilen etki alanınız tamamen hazır hale getirildikten sonra gerçekleştirilecek **gerekli yapılandırma adımları** kümesi bulunur. İlk yapılandırma adımı, **sanal ağınız için DNS sunucusu ayarlarını güncelleştirmektir**.

    ![Domain Services - Genel bakış sekmesi](./media/getting-started/domain-services-provisioned-overview.png)

    > [!TIP]
    > Bu yapılandırma adımını görmüyor musunuz? Sanal ağınızın DNS sunucusu ayarlarının güncel olması durumunda, 'Sanal ağınızın DNS sunucusu ayarlarını güncelleştirin' kutucuğunu Genel Bakış sekmesinde görmezsiniz.
    >
    >

2. Sanal ağın DNS sunucusu ayarlarını güncelleştirmek için **Yapılandır** düğmesine tıklayın.

> [!NOTE]
> Ağ üzerindeki sanal makineler, yeni DNS ayarlarını yalnızca yeniden başlatma işleminin ardından alabilir. Sanal makinelerin güncelleştirilen DNS ayarlarını doğrudan almasını istiyorsanız portal, PowerShell veya CLI aracılığıyla bir yeniden başlatma tetikleyin.
>
>

## <a name="next-step"></a>Sonraki adım
[Görev 5: Azure Active Directory Domain Services ile parola karma eşitlemesini etkinleştirme](active-directory-ds-getting-started-password-sync.md)
