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
ms.openlocfilehash: 2ec45b67367198c14fc9d03cdb659a51aed8a504
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841506"
---
1. Oturum [Azure portalında](https://portal.azure.com/) kullandığınız Azure aboneliği için kimlik bilgilerini kullanarak. 

   ![Azure portal](./media/aml-create-in-portal/portal-dashboard-05-2019.png)

1. Portalın sol üst köşedeki seçin **kaynak Oluştur**.

   ![Azure portalında kaynak oluşturma](./media/aml-create-in-portal/portal-create-a-resource-07-2019.png)

1. Arama çubuğunu kullanarak **Machine Learning hizmeti çalışma alanında**.

   ![Bir çalışma alanı arayın](./media/aml-create-in-portal/allservices-search.png)

1. İçinde **ML hizmeti çalışma alanında** bölmesinde **Oluştur** başlamak için.

    ![Oluştur düğmesi](./media/aml-create-in-portal/portal-create-button.png)

1. İçinde **ML hizmeti çalışma alanında** bölmesinde, çalışma alanınızı yapılandırın.

    ![Çalışma alanı oluşturma](./media/aml-create-in-portal/workspace-create-main-tab.png)

   Alan|Açıklama
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin. Bu örnekte **docs ws**. Arasında kaynak grubu adları benzersiz olmalıdır. Geri çağırma ve başkaları tarafından oluşturulan çalışma alanlarından ayırt etmek kolay bir ad kullanın.  
   Subscription |Kullanmak istediğiniz Azure aboneliğini seçin.
   Resource group | Aboneliğinizde mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturmak için bir ad girin. Bir kaynak grubu, bir Azure çözümü için ilgili kaynakları içerir. Bu örnekte **docs aml**. 
   Location | Kullanıcılarınızı ve veri kaynakları için en yakın konumu seçin. Çalışma alanı oluşturulduğu bu konumdur.

1. Çalışma alanı yapılandırmanızı gözden geçirin ve seçin **Oluştur**. Uygulamanın, çalışma alanı oluşturmak için birkaç dakika sürebilir.

1. İşlem tamamlandığında, bir dağıtım başarı iletisi görünür. Ayrıca, bildirimleri bölümünde de mevcuttur. Yeni çalışma alanı görüntülemek için seçin **kaynağa Git**.

   ![Çalışma alanı oluşturma durumu](./media/aml-create-in-portal/notifications.png)
