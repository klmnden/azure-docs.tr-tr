---
title: "Azure yığınında (Hizmet Yöneticisi ve Kiracı) kullanıcı başına kaynaklarına izinleri yönetme | Microsoft Docs"
description: "Hizmet Yöneticisi veya Kiracı RBAC izinleri yönetmeyi öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: fenila
editor: 
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: mabrigg
ms.reviewer: thomas.roettinger
ms.openlocfilehash: 0e50ea44ebb0b0a7285dab04666dd55cad480c6a
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="manage-role-based-access-control"></a>Rol tabanlı erişim denetimini yönetmek

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir kullanıcı Azure yığınında Okuyucu, sahibi veya katkıda bulunan her bir abonelik, kaynak grubu ya da hizmet örneği olabilir. Örneğin, kullanıcı bir abonelik bir okuyucu izinlerine sahip, ancak sanal makine yedi sahibi izinlerine sahip.

 - Okuyucu: Kullanıcı her şeyi görüntüleyebilir ancak değişiklik yapamazsınız.
 - Katkıda bulunan: Kullanıcı kaynaklara erişim dışında her şeyi yönetebilir.
 - Sahibi: Kullanıcı kaynaklara erişim dahil her şeyi yönetebilir.

## <a name="set-access-permissions-for-a-user"></a>Bir kullanıcı için erişim izinlerini ayarlayın

1. Yönetmek istediğiniz kaynak sahibi izinlerine sahip bir hesapla oturum açın.
2. Kaynak dikey penceresinde tıklayın **erişim** simgesi ![](media/azure-stack-manage-permissions/image1.png).
3. İçinde **kullanıcılar** dikey penceresinde tıklatın **rolleri**.
4. İçinde **rolleri** dikey penceresinde tıklatın **Ekle** kullanıcının izinlerini eklemek için.

## <a name="set-access-permissions-for-a-universal-group"></a>Bir evrensel grup için erişim izinlerini ayarlayın 

> [!Note]  
Yalnızca Active Directory uygulanabilir Hizmetleri (AD FS) federe.

1. Yönetmek istediğiniz kaynak sahibi izinlerine sahip bir hesapla oturum açın.
2. Kaynak dikey penceresinde tıklayın **erişim** simgesi ![](media/azure-stack-manage-permissions/image1.png).
3. İçinde **kullanıcılar** dikey penceresinde tıklatın **rolleri**.
4. İçinde **rolleri** dikey penceresinde tıklatın **Ekle** evrensel grup, Active Directory grubu için izinleri eklemek için.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack kiracısı ekleme](azure-stack-add-new-user-aad.md)

