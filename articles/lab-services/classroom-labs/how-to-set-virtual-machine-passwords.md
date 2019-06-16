---
title: Azure Lab Services, sanal makineler için parolaları ayarlama | Microsoft Docs
description: Azure Lab Services'ın classroom Labs'de sanal makineler (VM) için parolaları ve öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 3701c69ff97d840f6cba175df8f02bc55ece498b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67123199"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs"></a>Ayarlama veya sınıf Labs'de sanal makineler için parola sıfırlama
Bu makale, Vm'leri classroom Labs'de erişmek için ayar ve resettings parolaların farklı yol sunar. 

## <a name="lab-owners-teachers"></a>Laboratuvar sahibini (Öğretmenler)
Laboratuvar sahibi (Öğretmen) bir parola için Vm'leri Laboratuvardaki ayarlayabilirsiniz **kimlik bilgilerini ayarla** Laboratuvar Oluşturma Sihirbazı'nın sayfa.

![Kimlik bilgilerini ayarlama](../media/tutorial-setup-classroom-lab/set-credentials.png)

Ardından, Laboratuvar sahibi parolayı (gerekirse) üzerinde sıfırlayabilir **yapılandırma şablonu** Laboratuvar Oluşturma Sihirbazı'nın sayfa. 

![Tamamlanmış şablon yapılandırma sayfası](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)

Panoda Laboratuvar oluşturulduktan sonra Laboratuvar sahibi parolayı da sıfırlayabilirsiniz. 

![Giriş sayfasında menüyü parola sıfırlama](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)

Ardından, yeni parolayı girin **parola ayarlama** sayfasında ve seçin **parola ayarlama**. 

![Giriş sayfasında menüyü parola sıfırlama](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="lab-users-students"></a>Laboratuvar kullanıcıları (Öğrenciler)
Laboratuvar oluşturma zamanında. Aynı zamanda, Laboratuvar sahibi etkinleştirme veya kendi parolalarını VM'ler için ayarlanacak sağlayarak Öğrenciler devre dışı bırakabilirsiniz. Laboratuvar sahibi (Öğretmen) bu seçeneği etkinleştirirse, Laboratuvar kullanıcı (Öğrenci) Öğrenci VM ilk kez oturum açtığında, VM için parola ayarlama seçeneğine sahip olur. Üzerinde bir **Windows VM**, basın **CTRL + ALT + END**seçip **parolasını değiştirme**. 

## <a name="next-steps"></a>Sonraki adımlar
(Laboratuvar sahibi olarak) diğer Öğrenci kullanım seçenekleri hakkında bilgi almak için yapılandırma, şu makaleye bakın: [Öğrenci kullanım yapılandırma](how-to-configure-student-usage.md).
