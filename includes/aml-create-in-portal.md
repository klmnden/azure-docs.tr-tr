---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/21/2019
ms.openlocfilehash: 72f23b10047928f32886d9054f4dd1abdc569bd8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66396904"
---
1. Oturum [Azure portalında](https://portal.azure.com/) kullandığınız Azure aboneliği için kimlik bilgilerini kullanarak. 

   ![Azure portal](./media/aml-create-in-portal/portal-dashboard-05-2019.png)

1. Portalın sol üst köşedeki seçin **kaynak Oluştur**.

   ![Azure portalında kaynak oluşturma](./media/aml-create-in-portal/portal-create-a-resource-05-2019.png)

1. Arama çubuğunda, **Machine Learning**. Seçin **Machine Learning hizmeti çalışma alanında** arama sonucu.

   ![Bir çalışma alanı arayın](./media/aml-create-in-portal/allservices-search-05-2019.png)

1. İçinde **ML hizmeti çalışma alanında** bölmesinde **Oluştur** başlamak için.

    ![Oluştur düğmesi](./media/aml-create-in-portal/portal-create-button-05-2019.png)

1. İçinde **ML hizmeti çalışma alanında** bölmesinde, çalışma alanınızı yapılandırın.

   Alan|Açıklama
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin. Bu örnekte **docs ws**. Arasında kaynak grubu adları benzersiz olmalıdır. Geri çağırma ve başkaları tarafından oluşturulan çalışma alanlarından ayırt etmek kolay bir ad kullanın.  
   Abonelik |Kullanmak istediğiniz Azure aboneliğini seçin.
   Kaynak grubu | Aboneliğinizde mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturmak için bir ad girin. Bir kaynak grubu, bir Azure çözümü için ilgili kaynakları içerir. Bu örnekte **docs aml**. 
   Location | Kullanıcılarınızı ve veri kaynakları için en yakın konumu seçin. Çalışma alanı oluşturulduğu bu konumdur.

   ![Çalışma alanı oluşturma](./media/aml-create-in-portal/workspace-create-main-tab-05-2019.png)

1. Oluşturma işlemini başlatmak için **gözden geçir + Oluştur**.

    ![Create](./media/aml-create-in-portal/workspace-create-main-review-button-05-2019.png)

1. Çalışma alanı yapılandırmanızı gözden geçirin. Doğru ise, seçin **Oluştur**. Uygulamanın, çalışma alanı oluşturmak için birkaç dakika sürebilir.

    ![Create](./media/aml-create-in-portal/workspace-create-review-tab-05-2019.png)

1. Dağıtım durumunu denetlemek için bildirimler simgesini seçin. **zil**, araç çubuğundaki.

1. İşlem tamamlandığında, bir dağıtım başarı iletisi görünür. Ayrıca, bildirimleri bölümünde de mevcuttur. Yeni çalışma alanı görüntülemek için seçin **kaynağa Git**.

   ![Çalışma alanı oluşturma durumu](./media/aml-create-in-portal/notifications-05-2019.png)
