---
title: "Grup makineler değerlendirmesi ile Azure geçirmek için | Microsoft Docs"
description: "Azure geçiş hizmeti ile bir değerlendirme çalıştırmadan önce makineleri Grup açıklar."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5c279804-aa30-4946-a222-6b77c7aac508
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 966e0aebf56ab049e898d1787bfdfbb2ef23635e
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="group-machines-for-assessment"></a>Değerlendirme için Grup makineler

Bu makalede tarafından değerlendirmesi için makine grubu oluşturmak nasıl [Azure geçirmek](migrate-overview.md). Azure geçir makinelerinizde bunlar Azure geçiş için uygun ve makine Azure'da çalışan için boyutlandırma ve maliyet tahminler sunar olup olmadığını denetlemek için Grup değerlendirir.


## <a name="create-a-group"></a>Bir grup oluşturun

1. İçinde **Pano** Azure geçirmek projenin tıklatın **grupları** > **+ grup**ve bir grup adı belirtin.
2. Bir veya daha fazla makine grubuna ekleyin ve **oluşturma**. 
3. İsteğe bağlı olarak, grup için yeni bir değerlendirme çalıştırmayı seçebilirsiniz. 

    ![Bir grup oluşturun](./media/how-to-create-a-group/create-group.png)

Grup oluşturulduktan sonra üzerinde bir grup seçerek değiştirebilirsiniz **grupları** sayfasında ve ekleme veya makineler kaldırma.

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl kullanacağınızı öğrenin [makine bağımlılık eşleme](how-to-create-group-machine-dependencies.md) daha ayrıntılı grupları oluşturma.
- [Daha fazla bilgi edinin](concepts-assessment-calculation.md) değerlendirmelerinin nasıl hesaplandığını hakkında.
