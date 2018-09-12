---
title: Bir ölçek birimi düğüm bir Azure Stack tümleşik sisteminde Değiştir | Microsoft Docs
description: Bir Azure Stack tümleşik sistemi fiziksel ölçek birimi düğümde nasıl değiştirileceğini öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: f9434689-ee66-493c-a237-5c81e528e5de
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.openlocfilehash: 1b37b150dad4951a4ade81f226b515ce9cae9053
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377063"
---
# <a name="replace-a-scale-unit-node-on-an-azure-stack-integrated-system"></a>Bir ölçek birimi düğüm bir Azure Stack tümleşik sisteminde değiştirin

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede bir fiziksel bilgisayar için genel süreç (olarak da adlandırılan bir *ölçek birimi düğüm*) üzerinde bir Azure Stack tümleşik sistemi. Gerçek bir ölçek birimi düğüm değiştirme adımları farklılık gösterir, orijinal ekipman üreticisi (OEM) donanım satıcınıza temel. Sisteme özgü ayrıntılı adımlar için satıcınızın alan bulunduğu yerde değiştirilebilen biriminin (FRU) belgelerine bakın.

Aşağıdaki akış diyagramı bir tüm ölçek birimi düğümü değiştirmek için genel FRU işlemi gösterilmektedir.

![Değiştir düğümü işlemi akış çizelgesi](media/azure-stack-replace-node/replacenodeflow.png)

* Bu eylem fiziksel donanım koşula göre gerekli olmayabilir.

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

1. Kullanım [boşaltma](azure-stack-node-actions.md#scale-unit-node-actions) ölçek birimi düğümü bakım moduna almak için eylem. Bu eylem fiziksel donanım koşula göre gerekli olmayabilir.

   > [!NOTE]
   > Herhangi bir durumda, yalnızca bir düğüm kullanılabilir boşaltılır ve aynı anda S2D bozmadan kapalı (depolama alanları doğrudan).

2. Düğüme hala çalışır durumda kullanırsanız [gücünün kapatılmasını](azure-stack-node-actions.md#scale-unit-node-actions) eylem. Bu eylem fiziksel donanım koşula göre gerekli olmayabilir.
 
   > [!NOTE]
   > Eylem kapatma çalışmıyor olası durumda da, temel kart yönetim denetleyicisine (BMC) web arabirimini kullanın.

1. Fiziksel bilgisayar değiştirin. Genellikle, bu OEM donanım satıcınız tarafından gerçekleştirilir.
2. Kullanım [onarım](azure-stack-node-actions.md#scale-unit-node-actions) Ölçek birimine yeni fiziksel bilgisayar eklemek için eylem.
3. Ayrıcalıklı uç noktasına kullanın [sanal disk onarma durumunu](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Yeni veri sürücüleri ile tam depolama Onar işi sistem yüküne bağlı olarak birkaç saat sürebilir ve kullanılan alanı.
4. Onarım işlemi tamamlandıktan sonra tüm etkin uyarıları otomatik olarak kapatılan doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Sık erişimli çıkarılabilen bir fiziksel disk değiştirme hakkında daha fazla bilgi için bkz. [disk değiştirme](azure-stack-replace-disk.md). 
- Bir olmayan hot çıkarılabilen bir donanım bileşenini değiştirme hakkında daha fazla bilgi için bkz: [bir donanım bileşenini değiştirme](azure-stack-replace-component.md).
