---
title: include dosyası
description: include dosyası
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 05/06/2019
ms.openlocfilehash: 623e993dfbe6bbb3297fa6470865ab1a04f55b37
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188788"
---
>[!IMPORTANT]
>Oluşturduğunuz kaynakları, diğer Azure Machine Learning hizmeti öğreticileri ve nasıl yapılır makaleleri için önkoşul olarak kullanabilirsiniz.


### <a name="delete-everything"></a>Her şeyi silin

Oluşturduğunuz herhangi bir şey kullanmayı planlamıyorsanız, herhangi bir ücret ödememeniz kaynak grubunun tamamını silin:

1. Azure portalında **kaynak grupları** pencerenin sol tarafındaki.
 
   ![Azure portalında kaynak grubunu silme](./media/aml-ui-cleanup/delete-resources.png)

1. Listesinde, oluşturduğunuz kaynak grubunu seçin.

1. Pencerenin sağ tarafında, üç nokta düğmesini seçin ( **...** ).

1. **Kaynak grubunu sil**'i seçin.

Kaynak grubunun silinmesi, görsel arabirim içinde oluşturulan tüm kaynakları siler.  

### <a name="delete-only-the-compute-target"></a>Yalnızca işlem hedefini Sil

Burada oluşturduğunuz işlem hedef *otomatik olarak daralttığında* değil kullanıldığında sıfır düğümleri. Ücretleri en aza indirmek için budur. İşlem hedefini silmek istiyorsanız, aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com), kendi çalışma alanını açın.

    ![İşlem hedefini Sil](./media/aml-ui-cleanup/delete-compute-target.png)

1. İçinde **işlem** bölümü, çalışma alanının kaynağı seçin.

1. **Sil**’i seçin.

### <a name="delete-individual-assets"></a>Tek tek varlığını silme

Bunları seçip ardından seçerek tek tek varlıklar denemenizi oluşturulduğu görsel arabirim Sil **Sil** düğmesi.

![Denemeleri Sil](./media/aml-ui-cleanup/delete-experiment.png)
