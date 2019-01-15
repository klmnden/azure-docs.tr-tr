---
title: Azure Stack'te (Hizmet Yöneticisi ve Kiracı) kullanıcı başına kaynaklarıyla ilgili izinleri yönetme | Microsoft Docs
description: Hizmet Yöneticisi veya Kiracı olarak RBAC izinlerinin yönetmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/14/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: 26ce7139b856fc2f8d7d2cad549b3dd3c0f5e406
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54304695"
---
# <a name="manage-role-based-access-control"></a>Rol tabanlı erişim denetimini yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te bir kullanıcı, okuyucu, sahibi veya katkıda bulunan her bir aboneliğe, kaynak grubuna ya da hizmet örneği için olabilir. Örneğin, bir kullanıcı abonelik bir okuyucu izinlere sahip, ancak yedi sanal makine için sahip izinleriniz.

 - Okuyucu: Kullanıcı, her şeyi görüntüleyebilir ancak değişiklik yapamazsınız.
 - Katkıda bulunan: Kullanıcı, kaynaklara erişim dışında her şeyi yönetebilir.
 - Sahibi: Kullanıcı, kaynaklara erişim dahil her şeyi yönetebilir.

## <a name="set-access-permissions-for-a-user"></a>Bir kullanıcı için erişim izinlerini ayarlayın

1. Yönetmek istediğiniz kaynağa sahip izinlerine sahip bir hesapla oturum açın.
2. Kaynak dikey penceresinde **erişim** simgesi ![](media/azure-stack-manage-permissions/image1.png).
3. İçinde **kullanıcılar** dikey penceresinde tıklayın **rolleri**.
4. İçinde **rolleri** dikey penceresinde tıklayın **Ekle** kullanıcı izinlerini eklemek için.

## <a name="set-access-permissions-for-a-universal-group"></a>Bir evrensel grubu için erişim izinlerini ayarlayın 

> [!Note]  
Active Directory yalnızca şunlara Hizmetleri (AD FS) federe.

1. Yönetmek istediğiniz kaynağa sahip izinlerine sahip bir hesapla oturum açın.
2. Kaynak dikey penceresinde **erişim** simgesi ![](media/azure-stack-manage-permissions/image1.png).
3. İçinde **kullanıcılar** dikey penceresinde tıklayın **rolleri**.
4. İçinde **rolleri** dikey penceresinde tıklayın **Ekle** evrensel grup, Active Directory grup için izinler eklemek için.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack kiracısı ekleme](azure-stack-add-new-user-aad.md)

