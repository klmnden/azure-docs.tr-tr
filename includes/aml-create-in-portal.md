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
ms.openlocfilehash: 8bbdd2d49171ee8f4e7eb3cc0def1c7a6e59806b
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193497"
---
Oturum [Azure portalında](https://portal.azure.com/) kullandığınız Azure aboneliği için kimlik bilgilerini kullanarak. 

Portal'ın çalışma Pano yalnızca Microsoft Edge, Chrome ve Firefox tarayıcılarda desteklenir.

   ![Azure portal](./media/aml-create-in-portal/portal-dashboard.png)

Portalın sol üst köşedeki seçin **kaynak Oluştur**.

   ![Azure portalında kaynak oluşturma](./media/aml-create-in-portal/portal-create-a-resource.png)

Arama çubuğunda, **Machine Learning**. Seçin **Machine Learning hizmeti çalışma alanında** arama sonucu.

   ![Bir çalışma alanı arayın](./media/aml-create-in-portal/allservices-search.PNG)

İçinde **ML hizmeti çalışma alanında** bölmesinde seçin ve altındaki kaydırma **Oluştur** başlamak için.

   ![Oluştur](./media/aml-create-in-portal/portal-create-button.png)

İçinde **ML hizmeti çalışma alanında** bölmesinde, çalışma alanınızı yapılandırın.

   Alan|Açıklama
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin. Bu örnekte **docs ws**. Arasında kaynak grubu adları benzersiz olmalıdır. Geri çağırma ve başkaları tarafından oluşturulan çalışma alanlarından ayırt etmek kolay bir ad kullanın.  
   Abonelik |Kullanmak istediğiniz Azure aboneliğini seçin.
   Kaynak grubu | Aboneliğinizde mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturmak için bir ad girin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Bu örnekte **docs aml**. 
   Konum | Kullanıcılarınızı ve veri kaynakları için en yakın konumu seçin. Çalışma alanı oluşturulduğu bu konumdur.

   ![Çalışma alanı oluşturma](./media/aml-create-in-portal/workspace-create.png)

Oluşturma işlemini başlatmak için **Oluştur**. Uygulamanın, çalışma alanı oluşturmak için birkaç dakika sürebilir.

Dağıtım durumunu denetlemek için bildirimler simgesini seçin. **zil**, araç çubuğundaki.

   ![Çalışma alanı oluşturma durumu](./media/aml-create-in-portal/notifications.png)

İşlem tamamlandığında, bir dağıtım başarı iletisi görünür. Ayrıca, bildirimleri bölümünde de mevcuttur. Yeni çalışma alanı görüntülemek için seçin **kaynağa Git**.
