---
title: Bir ölçek birimi düğüm bir Azure Stack tümleşik sisteminde Değiştir | Microsoft Docs
description: Bir Azure Stack tümleşik sistemi fiziksel ölçek birimi düğümde nasıl değiştirileceğini öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2018
ms.author: mabrigg
ms.openlocfilehash: 9d53aa879c39eb68597a402133a7ff16737f4f65
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53716319"
---
# <a name="replace-a-scale-unit-node-on-an-azure-stack-integrated-system"></a>Bir ölçek birimi düğüm bir Azure Stack tümleşik sisteminde değiştirin

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede bir Azure Stack tümleşik sisteminde (Ayrıca bir ölçek birimi düğüm olarak adlandırılır) fiziksel bir bilgisayarı değiştirme için genel süreç açıklanır. Gerçek bir ölçek birimi düğüm değiştirme adımları farklılık gösterir, orijinal ekipman üreticisi (OEM) donanım satıcınıza temel. Sisteme özgü ayrıntılı adımlar için satıcınızın alan bulunduğu yerde değiştirilebilen biriminin (FRU) belgelerine bakın.

Aşağıdaki akış diyagramı bir tüm ölçek birimi düğümü değiştirmek için genel FRU işlemi gösterilmektedir.

![Değiştir düğümü işlemi akış çizelgesi](media/azure-stack-replace-node/replacenodeflow.png)

* Bu eylem fiziksel donanım koşula göre gerekli olmayabilir.

> [!Note]  
> Kapatma işlemi başarısız olursa, durdurma işlemi tarafından izlenen boşaltma işlemi kullanmak için önerilir. Kullanılabilir düğüm işlemleri daha fazla ayrıntı görmek için  

## <a name="review-alert-information"></a>Uyarı bilgileri gözden geçirin

Bir ölçek birimi düğüm kapalı ise, aşağıdaki kritik uyarılar alırsınız:

- Düğüm ağ Denetleyicisi'ne bağlı değil
- Düğüm sanal makine yerleştirme için erişilemez
- Ölçek birimi düğümü çevrimdışı.

![Ölçek birimi için uyarıların listesi](media/azure-stack-replace-node/nodedownalerts.png)

Açarsanız **ölçek birimi düğümü çevrimdışı** uyarı, uyarı açıklaması erişilemez ölçek birimi düğümü içerir. OEM özel izleme çözümündeki donanım yaşam döngüsü konakta çalıştığından, ek uyarılar da alabilirsiniz.

![Düğümü çevrimdışı uyarının ayrıntıları](media/azure-stack-replace-node/nodeoffline.png)

## <a name="scale-unit-node-replacement-process"></a>Ölçek birimi düğüm değiştirme işlemini

Ölçek birimi düğüm değiştirme işleminin üst düzey bir genel aşağıdaki adımları sağlanır. Sisteme özgü ayrıntılı adımlar için OEM donanım satıcınızın FRU belgelerine bakın. OEM tarafından sağlanan belgelerinize başvuruda bulunmadan bu adımları izlemeyin.

1. Kullanım **kapatma** düzgün bir şekilde ölçek birimi düğüm kapatma eylemi. Bu eylem fiziksel donanım koşula göre gerekli olmayabilir. 

2. Olası içinde kapatma eylemi başarısız durumda, kullanın [boşaltma](azure-stack-node-actions.md#drain) ölçek birimi düğümü bakım moduna almak için eylem. Bu eylem fiziksel donanım koşula göre gerekli olmayabilir.

   > [!NOTE]  
   > Herhangi bir durumda, yalnızca bir düğüm kullanılabilir devre dışı ve aynı anda S2D bozmadan kapalı (depolama alanları doğrudan).

3. Ölçek birimi düğüm bakım modunda olduğunda, kullanmak [Durdur](azure-stack-node-actions.md#stop) eylem. Bu eylem fiziksel donanım koşula göre gerekli olmayabilir.

   > [!NOTE]  
   > Eylem kapatma çalışmıyor olası durumda da, temel kart yönetim denetleyicisine (BMC) web arabirimini kullanın.

4. Fiziksel bilgisayar değiştirin. Genellikle, bu OEM donanım satıcınız tarafından gerçekleştirilir.
5. Kullanım [onarım](azure-stack-node-actions.md#repair) Ölçek birimine yeni fiziksel bilgisayar eklemek için eylem.
6. Ayrıcalıklı uç noktasına kullanın [sanal disk onarma durumunu](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Yeni veri sürücüleri ile tam depolama Onar işi sistem yüküne bağlı olarak birkaç saat sürebilir ve kullanılan alanı.
7. Onarım işlemi tamamlandıktan sonra tüm etkin uyarıları otomatik olarak kapatılan doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Sistemin açık durumdayken, fiziksel disk değiştirme hakkında daha fazla bilgi için bkz: [disk değiştirme](azure-stack-replace-disk.md). 
- Kapatılmasına sisteme gerektiren bir donanım bileşenini değiştirme hakkında daha fazla bilgi için bkz: [bir donanım bileşenini değiştirme](azure-stack-replace-component.md).
