---
title: Bir Azure Stack ölçek birimi düğümde bir donanım bileşenini değiştirme | Microsoft Docs
description: Bir Azure Stack tümleşik sisteminde bir donanım bileşenini değiştirme hakkında bilgi edinin.
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
ms.date: 09/10/2018
ms.author: mabrigg
ms.openlocfilehash: df9470813f3f9c3bff58882879c06e7b7b0fc15b
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379613"
---
# <a name="replace-a-hardware-component-on-an-azure-stack-scale-unit-node"></a>Bir Azure Stack ölçek birimi düğümde bir donanım bileşenini değiştirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede olmayan kullanılan donanım bileşenleri değiştirmek için genel işlem açıklanır. Gerçek değiştirme adımları farklılık orijinal ekipman üreticisi (OEM) donanım satıcınıza temel. Azure Stack tümleşik sisteme özgü ayrıntılı adımlar için satıcınızın alan bulunduğu yerde değiştirilebilen biriminin (FRU) belgelerine bakın.

Olmayan sık kullanılan bileşenleri şunları içerir:

- CPU *
- Bellek *
- Ana/temel kart yönetim denetleyicisine (BMC) / video kartı
- Disk denetleyici/ana veri yolu bağdaştırıcısı (HBA) / devre kartı
- Ağ bağdaştırıcısı (NIC)
- İşletim sistemi disk *
- Veri sürücüleri (sık erişimli değiştirme, örneğin PCI-e eklentisi kartları desteklemeyen sürücü) *

* Bu bileşenler, sık erişimli takas destekliyor olabilir, ancak satıcı uygulamasına göre değişebilir. Ayrıntılı adımlar için OEM satıcınızın FRU belgelerine bakın.

Aşağıdaki akış diyagramı olmayan hot çıkarılabilen bir donanım bileşenini değiştirme genel FRU işlemini gösterir.

![Akış bileşeni değişiklik akışını gösteren diyagram](media/azure-stack-replace-component/replacecomponentflow.PNG)

* Bu eylem fiziksel donanım koşula göre gerekli olmayabilir.

** Olup OEM donanım satıcınıza bileşenini değiştirme ve üretici yazılımı değişiklik yapacaktır güncelleştirmeleri gerçekleştirir, destek sözleşmeniz temel.

## <a name="review-alert-information"></a>Uyarı bilgileri gözden geçirin

Azure Stack durumunu ve izleme sistemi, ağ bağdaştırıcıları ve depolama alanları doğrudan tarafından denetlenen veri sürücüleri durumunu izleyin. Diğer donanım bileşenlerinin izlemiyor. Diğer tüm donanım bileşenleri için donanım yaşam döngüsü konakta çalışan çözüm izleme satıcıya özgü donanım uyarılar oluşturulur.  

## <a name="component-replacement-process"></a>Bileşen değiştirme işlemi

Aşağıdaki adımlar, bileşeni değiştirme işlemi üst düzey bir genel bakış sağlar. OEM tarafından sağlanan FRU belgelerinize başvuruda bulunmadan bu adımları izlemeyin.

1. Kullanım [boşaltma](azure-stack-node-actions.md#scale-unit-node-actions) ölçek birimi düğümü bakım moduna almak için eylem. Bu eylem fiziksel donanım koşula göre gerekli olmayabilir.

   > [!NOTE]
   > Herhangi bir durumda, yalnızca bir düğüm kullanılabilir boşaltılır ve aynı anda S2D bozmadan kapalı (depolama alanları doğrudan).

2. Ölçek birimi düğüm bakım modunda olduğunda, kullanmak [gücünün kapatılmasını](azure-stack-node-actions.md#scale-unit-node-actions) eylem. Bu eylem fiziksel donanım koşula göre gerekli olmayabilir.

   > [!NOTE]
   > Eylem kapatma çalışmıyor olası durumda da, temel kart yönetim denetleyicisine (BMC) web arabirimini kullanın.

3. Bozuk donanım bileşeni değiştirin. OEM donanım satıcınıza bileşenini değiştirme gerçekleştirip gerçekleştirmediğini, destek sözleşmenize göre farklılık.  
4. Üretici yazılımı güncelleştirme. Donanım yaşam döngüsü konak düzeyinde uygulanan onaylı bellenim değiştirilen donanım bileşeni olduğundan emin olun kullanmayı, satıcıya özgü üretici yazılımı güncelleştirme işlemini izleyin. OEM donanım satıcınız bu adımı gerçekleştirip gerçekleştirmediğini, destek sözleşmenize göre farklılık.  
5. Kullanım [onarım](azure-stack-node-actions.md#scale-unit-node-actions) ölçek birimi düğümü Ölçek birimine geri alma eylemi.
6. Ayrıcalıklı uç noktasına kullanın [sanal disk onarma durumunu](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Yeni veri sürücüleri ile tam depolama Onar işi sistem yüküne bağlı olarak birkaç saat sürebilir ve kullanılan alanı.
7. Onarım işlemi tamamlandıktan sonra tüm etkin uyarıları otomatik olarak kapatılan doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Sık erişimli çıkarılabilen bir fiziksel disk değiştirme hakkında daha fazla bilgi için bkz. [disk değiştirme](azure-stack-replace-disk.md).
- Bir fiziksel düğüme değiştirme hakkında daha fazla bilgi için bkz: [bir ölçek birimi düğüm değiştirin](azure-stack-replace-node.md).
