---
title: Azure veri kutusu Edge bant genişliği zamanlamaları yönetme | Microsoft Docs
description: Azure veri kutusu Ucunuzdaki zamanlamalarda bant genişliğini yönetmek için Azure portalını kullanmayı açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: f88003e38a34eb3396b83158ff71e4739080cd9d
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401400"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-data-box-edge"></a>Azure veri kutusu Ucunuzdaki zamanlamalarda bant genişliğini yönetmek için Azure portalını kullanma  

Bu makalede, Azure veri kutusu edge'de kullanıcıları nasıl yöneteceğinizi açıklar. Bant genişliği zamanlamaları, ağ bant genişliği kullanımını birden çok zamanlamaya göre yapılandırmanızı sağlar. Bu zamanlamalar, cihazınızla bulut arasında gerçekleştirilen yükleme ve indirme işlemlerine uygulanabilir.

Ekleme, değiştirme veya Azure portalından, bir veri kutusu Edge için bant genişliği zamanlamaları silme.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Zamanlama ekleme
> * Zamanlamayı değiştirme
> * Zamanlamayı silme


## <a name="add-a-schedule"></a>Zamanlama ekleme

Bir zamanlama eklemek için Azure portalında aşağıdaki adımları uygulayın.

1. Azure portalında veri kutusu Edge kaynağınızın Git **bant genişliği**.
2. Sağ bölmede seçin **+ Ekle zamanlama**.

    ![Bant genişliğini seçin](media/data-box-edge-manage-bandwidth-schedules/add-schedule-1.png)

3. **Zamanlama ekle** sayfasında: 

   1. Zamanlamanın **Başlangıç günü**, **Bitiş günü**, **Başlangıç saati** ve **Bitiş saati** değerlerini belirleyin.
   2. Denetleme **tüm gün** seçeneğini Bu zamanlama, tüm gün çalıştırmanız gerekir.
   3. **Bant genişliği hızı**, cihazınızda gerçekleştirilen bulutla ilgili işlemler (yükleme ve indirme) için kullanılan bant genişliğidir ve saniye başına megabit (Mb/sn) cinsinden ölçülür. Bu alan için 1,000,000,007 ile 20 arasındaki bir sayı girin.
   4. Veri yükleme ve indirme işlemlerini kısıtlamak istemiyorsanız **Sınırsız** bant genişliğini seçin.
   5. **Add (Ekle)** seçeneğini belirleyin.

      ![Zamanlama ekle](media/data-box-edge-manage-bandwidth-schedules/add-schedule-2.png)

3. Belirtilen parametrelerle bir zamanlama oluşturulur. Bu zamanlama daha sonra portaldaki bant genişliği zamanlamaları listesinde görüntülenir.

    ![Bant genişliği zamanlamaları güncelleştirilmiş listesi](media/data-box-edge-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Zamanlamayı düzenleme

Bir bant genişliği zamanlamasını düzenlemek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **bant genişliği**. 
2. Bant genişliği zamanlamaları listesinden seçin ve değiştirmek istediğiniz bir zamanlamayı seçin.
    ![Bant genişliği zamanlama seçin](media/data-box-edge-manage-bandwidth-schedules/modify-schedule-1.png)

3. İstediğiniz değişiklikleri yapın ve değişiklikleri kaydedin.

    ![Kullanıcıyı değiştirme](media/data-box-edge-manage-bandwidth-schedules/modify-schedule-2.png)

4. Zamanlama değiştirildikten sonra zamanlama listesi değiştirilen zamanlamayı gösterecek şekilde güncelleştirilir.

    ![Kullanıcıyı değiştirme](media/data-box-edge-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Zamanlamayı silme

Veri kutusu Edge cihazınıza ile ilişkili bir bant genişliği zamanlamasını silmek için aşağıdaki adımları uygulayın.

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **bant genişliği**.  

2. Bant genişliği zamanlaması listesinden silmek istediğiniz zamanlamayı seçin. İçinde **zamanlamayı Düzenle**seçin **Sil**. Onayınız istendiğinde seçin **Evet**.

   ![Kullanıcı silme](media/data-box-edge-manage-bandwidth-schedules/delete-schedule-2.png)

3. Zamanlama silindikten sonra zamanlama listesi güncelleştirilir.


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [paylaşımları yönetme](data-box-edge-manage-shares.md).
