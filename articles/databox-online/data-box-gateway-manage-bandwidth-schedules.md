---
title: Azure veri kutusu ağ geçidi bant genişliği zamanlamalarda yönetme | Microsoft Docs
description: Azure portalı kullanarak Azure Data Box Gateway bant genişliği zamanlamalarını yönetme adımları.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: overview
ms.date: 03/20/2019
ms.author: alkohli
ms.openlocfilehash: a50091ec8878cbc8c1167c03acddaf269d697f31
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400414"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-data-box-gateway"></a>Azure Data Box Gateway bant genişliği zamanlamalarını yönetmek için Azure portalı kullanma  

Bu makalede Azure Data Box Gateway kullanıcılarını yönetme adımları açıklanmaktadır. Bant genişliği zamanlamaları, ağ bant genişliği kullanımını birden çok zamanlamaya göre yapılandırmanızı sağlar. Bu zamanlamalar, cihazınızla bulut arasında gerçekleştirilen yükleme ve indirme işlemlerine uygulanabilir. 

Data Box Gateway cihazınız için bant genişliği zamanlaması ekleme, değiştirme veya silme işlemlerini Azure portaldan gerçekleştirebilirsiniz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Zamanlama ekleme
> * Zamanlamayı değiştirme
> * Zamanlamayı silme 


## <a name="add-a-schedule"></a>Zamanlama ekleme

Kullanıcı eklemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Data Box Gateway kaynağınızın Azure portal sayfasında **Bant genişliği** bölümüne gidin.
2. Sağdaki bölmede **+ Zamanlama ekle**'ye tıklayın.

    ![Kullanıcı ekle'ye tıklayın](media/data-box-gateway-manage-bandwidth-schedules/add-schedule-1.png)

3. **Zamanlama ekle** sayfasında: 

   1. Zamanlamanın **Başlangıç günü**, **Bitiş günü**, **Başlangıç saati** ve **Bitiş saati** değerlerini belirleyin. 
   2. Zamanlama gün boyu çalışıyorsa **Tüm gün** seçeneğini işaretleyebilirsiniz. 
   3. **Bant genişliği hızı**, cihazınızda gerçekleştirilen bulutla ilgili işlemler (yükleme ve indirme) için kullanılan bant genişliğidir ve saniye başına megabit (Mb/sn) cinsinden ölçülür. Bu alana 1 ile 1000 arasında bir sayı girin. 
   4. Veri yükleme ve indirme işlemlerini kısıtlamak istemiyorsanız **Sınırsız** bant genişliğini seçin. 
   5. **Ekle**'ye tıklayın.

      ![Kullanıcı ekle'ye tıklayın](media/data-box-gateway-manage-bandwidth-schedules/add-schedule-2.png)

3. Belirtilen parametrelerle bir zamanlama oluşturulur. Bu zamanlama daha sonra portaldaki bant genişliği zamanlamaları listesinde görüntülenir.


## <a name="edit-schedule"></a>Zamanlamayı düzenleme

Bir bant genişliği zamanlamasını düzenlemek için aşağıdaki adımları gerçekleştirin. 

1. Data Box Gateway kaynağınızın Azure portal sayfasında Bant genişliği bölümüne gidin. 
2. Bant genişliği zamanlaması listesinden değiştirmek istediğiniz zamanlamayı seçin ve tıklayın.
    ![Kullanıcıyı değiştirme](media/data-box-gateway-manage-bandwidth-schedules/modify-schedule-1.png)

3. İstediğiniz değişiklikleri yapın ve değişiklikleri kaydedin.

    ![Kullanıcıyı değiştirme](media/data-box-gateway-manage-bandwidth-schedules/modify-schedule-2.png)

4. Zamanlama değiştirildikten sonra zamanlama listesi değiştirilen zamanlamayı gösterecek şekilde güncelleştirilir.

    ![Kullanıcıyı değiştirme](media/data-box-gateway-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Zamanlamayı silme

Data Box Gateway cihazınızla ilişkilendirilmiş bant genişliği zamanlamasını silmek için aşağıdaki adımları gerçekleştirin.

1. Data Box Gateway kaynağınızın Azure portal sayfasında **Bant genişliği** bölümüne gidin.  

2. Bant genişliği zamanlaması listesinden silmek istediğiniz zamanlamayı seçin. Sağ tıklayın ve açılan bağlam menüsünde **Sil**'e tıklayın. 

   ![Kullanıcı silme](media/data-box-gateway-manage-bandwidth-schedules/delete-schedule-1.png)

3.  Zamanlama silindikten sonra zamanlama listesi güncelleştirilir.



## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](data-box-gateway-manage-bandwidth-schedules.md) öğrenin.
