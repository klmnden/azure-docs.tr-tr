---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: e8daaaf5b6b15eb3095f11e94c707a33b4b18e28
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60681396"
---
Seçin **yapı**. Açılan iletişim kutusunda, Xcode projesine dışarı aktarmak için bir klasör seçin.

Dışarı aktarma tamamlandığında, dışarı aktarılan Xcode projesi içeren bir klasör görünür.

> [!NOTE]
> Değiştirmeyi veya eklemek isteyip istemediğinizi soran bir pencere görünürse, seçtiğiniz öneririz **ekleme** daha hızlı olduğundan. Yalnızca seçilecek gerekir **değiştirin** varlıklar, sahnede değiştiriyorsanız. (Örneğin, size, kaldırma, üst/alt ilişkilerini değiştirme veya ekliyorsanız, kaldırma veya özelliklerini değiştirme ekliyorsanız.) Kaynak kodu değişiklikleri yalnızca yapıyorsanız **ekleme** yeterli olmalıdır.

### <a name="open-the-xcode-project"></a>Xcode projesi açın

Dışarı aktarılan Xcode proje klasörü içinde proje için gerekli Cocoapods'u yüklemek üzere terminalde bu komutu çalıştırın:

```bash
pod install --repo-update
```

Açabileceğiniz artık `Unity-iPhone.xcworkspace` Xcode'da projeyi açmak için:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> Görürseniz bir `library not found for -lPods-Unity-iPhone` hata büyük olasılıkla açtığınız `.xcodeproj` yerine dosya `.xcworkspace` dosya. 

Kök seçin **Unity iPhone** proje ayarlarını görüntüleyin ve ardından düğüme **genel** sekmesi.

Altında **imzalama**, emin **otomatik olarak imzalanmasını yönetmek** etkinleştirilir. Yüklü değilse, bunu etkinleştirin ve ardından **etkinleştirmek otomatik** iletişim kutusundaki derleme ayarlarını sıfırlamak için görünür.

Altında **dağıtım bilgisi**, emin **dağıtım hedefi** ayarlanır `11.0`.

### <a name="deploy-the-app-to-your-ios-device"></a>İOS cihazınıza uygulaması dağıtma

İOS cihazı ayarlama ve Mac bağlayın **etkin düzeni** iOS cihazınıza.

![Cihazı seçin](./media/spatial-anchors-unity/select-device.png)

Seçin **oluşturun ve ardından geçerli düzenini çalıştırın**.

![Dağıtma ve çalıştırma](./media/spatial-anchors-unity/deploy-run.png)