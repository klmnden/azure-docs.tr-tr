---
title: "Grup bağımlılık eşlemesindeki Azure geçirmek değerlendirme grubuyla İyileştir | Microsoft Docs"
description: "Azure geçirmek hizmetinde Grup bağımlılık eşlemesi kullanarak bir değerlendirme iyileştirmek açıklar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: c30d6546e7c2d471d4b262a8af1ce593b2c1c3fb
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
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
