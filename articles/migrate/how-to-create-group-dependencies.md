---
title: "Grup bağımlılık eşlemesindeki Azure geçirmek değerlendirme grubuyla İyileştir | Microsoft Docs"
description: "Azure geçirmek hizmetinde Grup bağımlılık eşlemesi kullanarak bir değerlendirme iyileştirmek açıklar."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0527e34e-a078-405e-aeb9-c91a5808112a
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: b4d6861f147fbb6e65a9d529f17f78b54075eb90
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Grup bağımlılık eşlemesi kullanarak bir grup daraltın

Bu grup için bağımlılık eşlemesi ayarlamak makalede [Azure geçirmek](migrate-overview.md) değerlendirmesi. Bir değerlendirme çalıştırmadan önce varolan bir grubu ayarlarını Çapraz denetimi Grup bağımlılıkları tarafından iyileştirmek istediğinizde genellikle bu yöntemi kullanın. Grup bağımlılık eşlemesi çalıştırmak istediğiniz grupları 10'dan fazla makineler içermemelidir.  

## <a name="modify-a-group"></a>Bir grubu değiştirin

1. Azure projesi altında geçirmek **Yönet**, tıklatın **grupları**ve grubu seçin.
2. Grup sayfasında, tıklatın **bağımlılıklarını görüntüleme**Grup bağımlılık Haritası açmak için. 

     ![Görünüm Grup](./media/how-to-create-group-dependencies/create-group.png)

3. Daha ayrıntılı bağımlılıkları görüntülemek için zaman aralığını değiştirmek için tıklayın. Varsayılan olarak, aralığı bir saattir. Zaman aralığını değiştirmek veya başlangıç ve bitiş tarihleri ve süresini belirtin.
4. Harita makinelerde seçmek için CTRL + Click kullanın. Ekleyebilir veya eşlemesinden makineleri kaldırın.
    - Yalnızca bulunmuş makineler ekleyebilirsiniz.
    - Ekleme ve makineler bir gruptan kaldırma değerlendirmeleri onun için geçmiş geçersiz kılar.
    - Grup değiştirdiğinizde, isteğe bağlı olarak yeni bir değerlendirme oluşturabilirsiniz.
5. Tıklatın **Tamam** grubunu kaydetmek için.

    ![Ekleme ve kaldırma](./media/how-to-create-group-dependencies/add-remove.png)

Grup bağımlılık Haritası görünür belirli bir makine bağımlılıkları denetlemek istiyorsanız [makine bağımlılık eşlemesi ayarlamanız](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](concepts-assessment-calculation.md) değerlendirmelerinin nasıl hesaplandığını hakkında.
