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
ms.openlocfilehash: 6d6e6a2aea867c5b603d950a4bbaa33f14695f45
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47011033"
---
Azure aboneliği için kullandığınız hesap bilgilerini kullanarak [Azure portalında](https://portal.azure.com/) oturum açın. Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Portal'ın çalışma Pano yalnızca Edge, Chrome ve Firefox tarayıcılarda desteklenir.

   ![Azure portal](./media/aml-create-in-portal/portal-dashboard.png)

Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin.

   ![Azure portalında kaynak oluşturma](./media/aml-create-in-portal/portal-create-a-resource.png)

Arama çubuğuna **Machine Learning** yazın. Adlı arama sonucunu seçin **Machine Learning çalışma alanı**.

   ![Çalışma alanı için arama](./media/aml-create-in-portal/allservices-search.PNG)

İçinde **Machine Learning çalışma alanı** bölmesinde seçin ve altındaki kaydırma **Oluştur** başlamak için.

   ![oluşturmaya](./media/aml-create-in-portal/portal-create-button.png)

İçinde **ML çalışma alanı** bölmesinde, çalışma alanınızı yapılandırın.

   Alan|Açıklama
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin.  Docs ws burayı kullanacağız. Arasında kaynak grubu adları benzersiz olmalıdır. Geri çağırma ve başkaları tarafından oluşturulan çalışma alanlarından ayırt etmek kolay bir ad kullanın.  
   Abonelik |Kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin.
   Kaynak grubu | Aboneliğinizde mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturmak için bir ad girin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır.  Docs aml burayı kullanacağız. 
   Konum | Kullanıcılarınıza ve veri kaynaklarınıza en yakın konumu seçin. Çalışma alanı oluşturulduğu budur.

   ![Çalışma alanı oluşturma](./media/aml-create-in-portal/workspace-create.png)

Oluşturma işlemini başlatmak için **Oluştur**'u seçin.  Uygulamanın, çalışma alanı oluşturmak için birkaç dakika sürebilir.

   Dağıtım durumunu denetlemek için araç çubuğundaki bildirim simgesine (zil) seçin.

   ![Çalışma alanı oluşturma](./media/aml-create-in-portal/notifications.png)

   İşiniz bittiğinde dağıtım başarılı iletisi görüntülenir.  Ayrıca, bildirimleri bölümünde de mevcuttur.   Tıklayarak **kaynağa Git** yeni çalışma alanını görüntülemek için düğme.
