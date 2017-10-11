---
title: "Çalıştırın ve bir Azure bulut hizmeti yerel makinede hata ayıklamak için emulator Express kullanarak | Microsoft Docs"
description: "Öykünücü Express'i çalıştırın ve bir bulut hizmeti yerel makinede hata ayıklamak için kullanma"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: d7d87045569fdc473333deae05f95d1df343b68c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="using-emulator-express-to-run-and-debug-an-azure-cloud-service-on-a-local-machine"></a>Öykünücü Express'i çalıştırın ve bir Azure bulut hizmeti yerel makinede hata ayıklamak için kullanma
Öykünücü Express kullanarak, test ve yönetici olarak Visual Studio çalıştırmadan bir bulut hizmetinde hata ayıklama. Proje ayarlarınızı Emulator Express veya Bulut hizmetinizin gereksinimlerine bağlı olarak tam öykünücü kullanacak şekilde ayarlayabilirsiniz. Tam öykünücü hakkında daha fazla bilgi için bkz: [Azure uygulamanın işlem Öykünücüde çalıştırma](storage/common/storage-use-emulator.md).

## <a name="using-emulator-express-in-visual-studio"></a>Visual Studio öykünücüsü kullanarak
Azure SDK 2.3 veya daha sonra bir Azure projesi oluşturduğunuzda, Emulator Express otomatik olarak kullanılır. Azure SDK'ın önceki bir sürümü ile oluşturulmuş mevcut projeleri için Emulator Express seçmek için aşağıdaki adımları kullanın:

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve bağlam menüsünden seçin **özellikleri**.

1. Proje Özellikleri sayfaları seçin **Web** sekmesi.

    ![Bir Azure bulut hizmeti projesi özellikleri](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. Altında **yerel geliştirme sunucusu**seçin **IIS Express kullan seçeneği**.

1. Altında **öykünücüsü**seçin **öykünücü kullan Express**.
   
1. Öykünücü Express başlatmak için komut isteminde aşağıdaki komutu çalıştırın: 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a>Öykünücü Express sınırlamaları
Aşağıdaki sorunlar Emulator Express sınırlamaları bilinmektedir: 

- Öykünücü Express IIS Web sunucusu ile uyumlu değil.
- Bulut hizmetinizi birden çok rol içerebilir, ancak her rol için bir örnek sınırlıdır.
- Bağlantı noktası numaralarını 1000 aşağıda erişemiyor. Normalde 1000 aşağıda bağlantı noktası kullanan bir kimlik doğrulama sağlayıcısı kullanırsanız, bu değer 1000'dir bir bağlantı noktası numarasını değiştirmek gerekebilir.
- Azure işlem öykünücüsü için geçerli sınırlamalar Emulator Express için de geçerlidir. Örneğin, dağıtım başına 50'den fazla rol örneği sahip olamaz. Azure işlem öykünücüsü hakkında daha fazla bilgi için bkz: [Azure uygulamanın işlem Öykünücüde çalıştırma](http://go.microsoft.com/fwlink/p/?LinkId=623050).

## <a name="next-steps"></a>Sonraki adımlar
[Hata ayıklama Azure bulut Hizmetleri](https://msdn.microsoft.com/library/azure/ee405479.aspx)
