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
ms.date: 09/24/2018
ms.openlocfilehash: 05331c710817e575deb7729189c9b2d8ccbafd7d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60753929"
---
1. Oturum [Azure portalında](https://portal.azure.com/) kullandığınız Azure aboneliği için kimlik bilgilerini kullanarak. 

   ![Azure portal](./media/aml-create-in-portal/portal-dashboard.png)

1. Portalın sol üst köşedeki seçin **kaynak Oluştur**.

   ![Azure portalında kaynak oluşturma](./media/aml-create-in-portal/portal-create-a-resource.png)

1. Arama çubuğunda, **Machine Learning**. Seçin **Machine Learning hizmeti çalışma alanında** arama sonucu.

   ![Bir çalışma alanı arayın](./media/aml-create-in-portal/allservices-search.PNG)

1. İçinde **ML hizmeti çalışma alanında** bölmesinde seçin ve altındaki kaydırma **Oluştur** başlamak için.

   ![Oluştur](./media/aml-create-in-portal/portal-create-button.png)

1. İçinde **ML hizmeti çalışma alanında** bölmesinde, çalışma alanınızı yapılandırın.

   Alan|Açıklama
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin. Bu örnekte **docs ws**. Arasında kaynak grubu adları benzersiz olmalıdır. Geri çağırma ve başkaları tarafından oluşturulan çalışma alanlarından ayırt etmek kolay bir ad kullanın.  
   Abonelik |Kullanmak istediğiniz Azure aboneliğini seçin.
   Kaynak grubu | Aboneliğinizde mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturmak için bir ad girin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Bu örnekte **docs aml**. 
   Location | Kullanıcılarınızı ve veri kaynakları için en yakın konumu seçin. Çalışma alanı oluşturulduğu bu konumdur.

   ![Çalışma alanı oluşturma](./media/aml-create-in-portal/workspace-create.png)

1. Oluşturma işlemini başlatmak için **Oluştur**. Uygulamanın, çalışma alanı oluşturmak için birkaç dakika sürebilir.

1. Dağıtım durumunu denetlemek için bildirimler simgesini seçin. **zil**, araç çubuğundaki.

1. İşlem tamamlandığında, bir dağıtım başarı iletisi görünür. Ayrıca, bildirimleri bölümünde de mevcuttur. Yeni çalışma alanı görüntülemek için seçin **kaynağa Git**.

   ![Çalışma alanı oluşturma durumu](./media/aml-create-in-portal/notifications.png)
