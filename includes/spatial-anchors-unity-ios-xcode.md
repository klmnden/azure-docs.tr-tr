---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 6768b1b8e0f5d7d3644779268025551c4e1aef9b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57964309"
---
Seçin **derleme** bir iletişim kutusunu açın. Ardından, Xcode projesi dışarı aktarmak için bir klasör seçin.

Dışarı aktarma tamamlandığında, dışarı aktarılan bir Xcode projesini içeren klasör görüntülenir.

> [!NOTE]
> Ayarlamak isteyip istemediğinizi soran bir iletişim kutusu açılır, **değiştirin** veya **ekleme**, **ekleme** daha hızlı olduğundan, önerilir. Yalnızca gerçekleştirmek gerekir bir **değiştirin** (ekleme, kaldırma, değiştirme üst/alt ilişkilerini, özellik ekleme/kaldırma/değiştirme, vb.), sahnede değiştiriyorsunuz varlıklar. Kaynak kodu değişiklikleri yalnızca yapıyorsanız **ekleme** yeterli olmalıdır.

### <a name="open-the-xcode-project"></a>Xcode projesi açın

Dışarı aktarılan Xcode proje klasörü içinde proje için gerekli Cocoapods'u yüklemek için terminalde aşağıdaki komutu çalıştırın:

```bash
pod install --repo-update
```

Şimdi kullanabilirsiniz açın `Unity-iPhone.xcworkspace` Xcode'da projeyi açmak için:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> Görürseniz bir `library not found for -lPods-Unity-iPhone` hata büyük olasılıkla açtığınız `.xcodeproj` yerine dosya `.xcworkspace`. Açık `.xcworkspace` ve yeniden deneyin.

Kök seçin **Unity iPhone** proje ayarlarını görüntülemek ve seçmek için düğüm **genel** sekmesi.

Altında **imzalama**, kontrol **otomatik olarak imzalanmasını yönetmek** etkinleştirilir. Yüklü değilse, etkinleştirmek ve seçin **etkinleştirmek otomatik** sıfırlamak için görüntülenen iletişim kutusunda derleme ayarları.

Altında **dağıtım bilgisi**, emin **dağıtım hedefi** ayarlanır `11.0`.

### <a name="deploy-the-app-to-your-ios-device"></a>İOS cihazınıza uygulaması dağıtma

İOS cihazı ayarlama ve Mac bağlayın **etkin düzeni** iOS cihazınıza.

![Cihazı seçin](./media/spatial-anchors-unity/select-device.png)

Seçin **oluşturun ve ardından geçerli düzenini çalıştırın**.

![Dağıtma ve çalıştırma](./media/spatial-anchors-unity/deploy-run.png)