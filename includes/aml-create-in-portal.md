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
ms.openlocfilehash: edcb2ecb74255ddbb8d601cb69565fb401b756d2
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52886427"
---
Oturum [Azure portalında](https://portal.azure.com/) kullandığınız Azure aboneliği için kimlik bilgilerini kullanarak. 

Portal'ın çalışma Pano yalnızca Edge, Chrome ve Firefox tarayıcılarda desteklenir.

   ![Azure portal](./media/aml-create-in-portal/portal-dashboard.png)

Portalın sol üst köşedeki seçin **kaynak Oluştur**.

   ![Azure portalında kaynak oluşturma](./media/aml-create-in-portal/portal-create-a-resource.png)

Arama çubuğunda, **Machine Learning**. Seçin **Machine Learning hizmeti çalışma alanında** arama sonucu.

   ![Çalışma alanı için arama](./media/aml-create-in-portal/allservices-search.PNG)

İçinde **Machine Learning hizmeti çalışma alanında** bölmesinde seçin ve altındaki kaydırma **Oluştur** başlamak için.

   ![Oluştur](./media/aml-create-in-portal/portal-create-button.png)

İçinde **ML hizmeti çalışma alanında** bölmesinde, çalışma alanınızı yapılandırın.

   Alan|Açıklama
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin. Burada docs ws kullanın. Arasında kaynak grubu adları benzersiz olmalıdır. Geri çağırma ve başkaları tarafından oluşturulan çalışma alanlarından ayırt etmek kolay bir ad kullanın.  
   Abonelik |Kullanmak istediğiniz Azure aboneliğini seçin.
   Kaynak grubu | Aboneliğinizde mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturmak için bir ad girin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Burada docs aml kullanın. 
   Konum | Kullanıcılarınızı ve veri kaynakları için en yakın konumu seçin. Çalışma alanı oluşturulduğu bu konumdur.

   ![Çalışma alanı oluşturma](./media/aml-create-in-portal/workspace-create.png)

Oluşturma işlemini başlatmak için **Oluştur**. Uygulamanın, çalışma alanı oluşturmak için birkaç dakika sürebilir.

Dağıtım durumunu denetlemek için araç çubuğundaki bildirim simgesine (zil) seçin.

   ![Çalışma alanı oluşturma durumu](./media/aml-create-in-portal/notifications.png)

İşlem tamamlandığında, bir dağıtım başarı iletisi görünür. Ayrıca, bildirimleri bölümünde de mevcuttur. Yeni çalışma alanı görüntülemek için seçin **kaynağa Git**.
