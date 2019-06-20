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
ms.openlocfilehash: c1564fadef35a20d0d87db8439ae1cc3dc923318
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144091"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs"></a>Ayarlama veya sınıf Labs'de sanal makineler için parola sıfırlama
Bu makale, Vm'leri classroom Labs'de erişmek için ayar ve resettings parolaların farklı yol sunar. 

## <a name="lab-owners-teachers"></a>Laboratuvar sahibini (Öğretmenler)
Laboratuvar sahibi (Öğretmen) ayarlama / parola VM'ler için zaman (Laboratuvar Oluşturma Sihirbazı) Laboratuvar oluşturma veya (Pano üzerinde) laboratuvarı oluşturduktan sonra sıfırlama. 

### <a name="set-password-at-the-time-of-lab-creation"></a>Laboratuvar oluşturma sırasında parola ayarlayın
Laboratuvar sahibi (Öğretmen) bir parola için Vm'leri Laboratuvardaki ayarlayabilirsiniz **kimlik bilgilerini ayarla** Laboratuvar Oluşturma Sihirbazı'nın sayfa.

![Kimlik bilgilerini ayarlama](../media/tutorial-setup-classroom-lab/set-credentials.png)

Etkinleştirme/devre dışı bırakarak **tüm sanal makineler için kullanılan parolayı kullan** Laboratuvardaki tüm sanal makineler için aynı parolayı kullanın veya kendi Vm'leri için parolanın öğrencilerin izin vermek bu sayfada, bir Öğretmen seçeneği tercih edebilir. Varsayılan olarak, Ubuntu dışındaki tüm Windows ve Linux işletim sistemi görüntüleri için bu ayar etkinleştirilir. Bu ayar devre dışı bırakıldığında, Öğrenciler VM ilk kez bağlanmaya çalıştığınızda bir parola ayarlamanız istenir. 

Laboratuvar sahibi bu parolayı (gerekirse) üzerinde sıfırlayabilir **yapılandırma şablonu** Laboratuvar Oluşturma Sihirbazı'nın sayfa. 

![Tamamlanmış şablon yapılandırma sayfası](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)

Panoda Laboratuvar oluşturulduktan sonra Laboratuvar sahibi parolayı da sıfırlayabilirsiniz. 

### <a name="reset-password-on-the-dashboard"></a>Panoda parola sıfırlama

1. Laboratuvar kutucuğuna taşma menüsü (dikey üç nokta) seçip **parolayı Sıfırla**. 

    ![Giriş sayfasında menüyü parola sıfırlama](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)
1. Üzerinde **parola ayarlama** iletişim kutusu, bir parola girin ve seçin **parola ayarlama**.
    
    ![Küme parolası iletişim kutusu](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="lab-users-students"></a>Laboratuvar kullanıcıları (Öğrenciler)
Laboratuvar oluşturma sırasında Laboratuvar sahibi etkinleştirmek veya devre dışı bırakabileceğiniz **tüm sanal makineler için kullanılan parolayı kullan**. Bu seçenek etkinleştirilirse, Öğrenciler parolanızı sıfırlayamazsınız. Öğretmen tarafından ayarlanan aynı parolayı labs'teki tüm sanal makineler gerekir. 

Bu seçeneği devre dışıysa, kullanıcılar ilk kez VM'ye bağlanmaya çalışırken bir parola ayarlamanız gerekir. Kullanıcılar (Öğrenciler) seçtiğinizde **Connect** düğmesine Laboratuvar kutucuğa **sanal makinelerim** sayfasında, kullanıcı için VM parolasını ayarlamak için aşağıdaki iletişim kutusu görür: 

![Öğrenci için parola sıfırlama](../media/how-to-set-virtual-machine-passwords/student-set-password.png)

Öğrenci de ayarlayabilirsiniz parola taşma menüsü tıklayarak (**üç dikey noktayı**) Laboratuvar kutucuğa tıklayıp **parolayı Sıfırla**. 

## <a name="next-steps"></a>Sonraki adımlar
(Laboratuvar sahibi olarak) diğer Öğrenci kullanım seçenekleri hakkında bilgi almak için yapılandırma, şu makaleye bakın: [Öğrenci kullanım yapılandırma](how-to-configure-student-usage.md).
