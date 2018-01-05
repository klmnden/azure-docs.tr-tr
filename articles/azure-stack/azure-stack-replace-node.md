---
title: "Bir Azure tümleşik yığını sistemi bir ölçek birimi düğümünde Değiştir | Microsoft Docs"
description: "Bir Azure tümleşik yığını sistem fiziksel ölçek birimi düğümde Değiştir öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: f9434689-ee66-493c-a237-5c81e528e5de
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: mabrigg
ms.openlocfilehash: 468af385833395963ef8acad905b99a9b7e6b8fa
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="replace-a-scale-unit-node-on-an-azure-stack-integrated-system"></a>Bir Azure tümleşik yığını sistemi bir ölçek birimi düğümünde değiştirin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede bir fiziksel bilgisayara değiştirmek için genel işlem (olarak da adlandırılan bir *ölçek birimi düğümü*) bir Azure yığında sistem tümleşik. Gerçek ölçek birimi düğümü değiştirme adımları farklılık gösterir, özgün donanım üreticisi (OEM) donanım satıcınıza temel. Sisteme özgü ayrıntılı adımlar için satıcınızın alan değiştirilebilen biriminin (FRU) belgelerine bakın.

Aşağıdaki akış diyagramı tüm ölçek birimi düğümü değiştirmek için genel FRU işlemi gösterilmektedir.

![Değiştir düğümü işlemi için akış çizelgesi](media/azure-stack-replace-node/replacenodeflow.png)

* Bu eylem donanım fiziksel koşula göre gerekli olmayabilir.

## <a name="review-alert-information"></a>Uyarı bilgileri gözden geçirin

Bir ölçek birimi düğümü kapalı ise, aşağıdaki kritik uyarılar alırsınız:

- Düğüm ağ denetleyicisine bağlı değil
- Sanal makine yerleştirme için erişilemez düğümü
- Ölçek birimi düğümü çevrimdışı

![Ölçek birimi için uyarıların listesi](media/azure-stack-replace-node/nodedownalerts.png)

Açarsanız **ölçek birimi düğüm çevrimdışıysa** uyarı, uyarı açıklaması erişilemez ölçek birimi düğümü içerir. Donanım yaşam döngüsü konakta çalıştığından OEM özgü izleme çözümde ek uyarıları da alabilirsiniz.

![Düğüm çevrimdışı uyarı ayrıntıları](media/azure-stack-replace-node/nodeoffline.png)

## <a name="scale-unit-node-replacement-process"></a>Ölçek birimi düğümü değiştirme işlemini

Aşağıdaki adımlar, bir üst düzey genel bakış ölçek birimi düğümü değiştirme işlemini sağlanır. Sisteme özgü ayrıntılı adımlar için OEM donanım satıcısının FRU belgelerine bakın. OEM tarafından sağlanan belgelerinize bakarak olmadan adımları izlemeyin.

1. Kullanım [boşaltma](azure-stack-node-actions.md#scale-unit-node-actions) ölçek birimi düğümü bakım moduna eylem. Bu eylem donanım fiziksel koşula göre gerekli olmayabilir.

   > [!NOTE]
   > Herhangi bir durumda, yalnızca tek bir düğüme boşaltmış ve değiştirebilirsiniz aynı anda S2D bozmadan kapalı (depolama alanları doğrudan).

2. Düğüme hala açık olduğundan kullanırsanız [kapatmak](azure-stack-node-actions.md#scale-unit-node-actions) eylem. Bu eylem donanım fiziksel koşula göre gerekli olmayabilir.
 
   > [!NOTE]
   > Eylem kapatma işe yaramazsa olası durumda da, temel kart yönetim denetleyicisi (BMC) web arabirimi kullanın.

1. Fiziksel bilgisayar değiştirin. Genellikle, bu OEM donanım satıcınız tarafından gerçekleştirilir.
2. Kullanım [onarım](azure-stack-node-actions.md#scale-unit-node-actions) ölçek birimi için yeni fiziksel bilgisayar eklemek için eylem.
3. Ayrıcalıklı uç noktasına kullanmak [sanal disk onarım durumunu denetleme](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Yeni bir veri sürücüsü ile tam depolama onarım iş sistem yükü bağlı olarak birkaç saat sürebilir ve alan tüketilen.
4. Onarım işlemi tamamlandıktan sonra tüm etkin uyarıları otomatik olarak kapatılan doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Hot Swap fiziksel disk değiştirme hakkında daha fazla bilgi için bkz: [disk değiştirme](azure-stack-replace-disk.md). 
- Olmayan hot Swap donanım bileşeni değiştirme hakkında daha fazla bilgi için bkz: [donanım bileşeni Değiştir](azure-stack-replace-component.md).
