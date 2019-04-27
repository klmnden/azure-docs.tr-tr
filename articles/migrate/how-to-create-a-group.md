---
title: Grup makineleri Azure geçişi ile değerlendirme için | Microsoft Docs
description: Azure geçişi hizmeti ile bir değerlendirmeyi çalıştırmadan önce makineleri gruplandırın açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/28/2018
ms.author: raynew
ms.openlocfilehash: c11d2f22fa08417107b0eecdd902b4521410b358
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60544926"
---
# <a name="group-machines-for-assessment"></a>Makineleri değerlendirme için

Bu makale makine tarafından değerlendirmesi için bir grubu oluşturmayı açıklar [Azure geçişi](migrate-overview.md). Azure geçişi, Azure'a geçiş için uygun ve makine Azure'da çalıştırmak için boyutlandırma ve maliyet tahminleri sağlar olup olmadığını denetlemek için gruptaki makinelerin değerlendirir. Birlikte geçirilmesi makineler biliyorsanız, aşağıdaki yöntemi kullanarak, Azure geçişi, gruba el ile oluşturabilirsiniz. Birlikte gruplanması gereken makineler hakkında çok emin değilseniz, bağımlılık görselleştirme işlevini Azure Geçişi'nde grupları oluşturmak için kullanabilirsiniz. [Daha fazla bilgi edinin.](how-to-create-group-machine-dependencies.md)

> [!NOTE]
> Bağımlılık görselleştirme işlevini Azure Kamu'da kullanılabilir değil.

## <a name="create-a-group"></a>Grup oluşturma

1. İçinde **genel bakış** Azure geçişi projesinin altında yönetin, tıklayın **grupları** > **+ grup**, bir grup adı belirtin.
2. Bir veya daha fazla makine gruba ekleyin ve **Oluştur**.
3. İsteğe bağlı olarak, grup için yeni bir değerlendirme çalıştırmayı seçebilirsiniz.

    ![Grup oluşturma](./media/how-to-create-a-group/create-group.png)

Grup oluşturulduktan sonra grubun seçerek değiştirebilirsiniz **grupları** sayfa ve ekleme veya makineler kaldırma.

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl kullanacağınızı öğrenin [makine bağımlılık eşlemesi](how-to-create-group-machine-dependencies.md) yüksek güvenilirliğe sahip gruplar oluşturmak için.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
