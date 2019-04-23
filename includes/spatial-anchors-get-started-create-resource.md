---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 12/13/2018
ms.author: crtreasu
ms.openlocfilehash: 32f4545a45eda8acddd7c93cc4917dbadca9ad4d
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58633047"
---
## <a name="create-a-spatial-anchors-resource"></a>Uzamsal bağlayıcılarını kaynak oluştur

<a href="https://portal.azure.com" target="_blank">Azure Portal</a> gidin.

Azure portalında sol gezinti bölmesinde seçin **kaynak Oluştur**.

Aramak için arama kutusunu kullanın **uzamsal bağlayıcılarını**.

   ![Uzamsal yer işaretleri arayın](./media/spatial-anchors-get-started-create-resource/portal-search.png)

Seçin **uzamsal bağlayıcılarını**. İletişim kutusunda **Oluştur**.

İçinde **uzamsal bağlayıcılarını hesabı** iletişim kutusunda:

- Normal alfanümerik karakterleri kullanarak bir benzersiz kaynak adı girin.
- Kaynak eklemek için kullanmak istediğiniz aboneliği seçin.
- Bir kaynak grubu seçerek oluşturun **Yeni Oluştur**. Adlandırın **myResourceGroup** seçip **Tamam**.
      [!INCLUDE [resource group intro text](resource-group.md)]
- Bir konum (bölge), kaynak yerleştirmek seçin.
- Seçin **yeni** kaynak oluşturmaya başlayın.

   ![Kaynak oluşturma](./media/spatial-anchors-get-started-create-resource/create-resource-form.png)

Kaynak oluşturulduktan sonra Azure portalı dağıtımınızı tamamlandığını gösterir. **Kaynağa git**'e tıklayın.

![Dağıtım tamamlandı](./media/spatial-anchors-get-started-create-resource/deployment-complete.png)

Ardından, kaynak özelliklerini görüntüleyebilirsiniz. Kaynağın kopyalama **hesap kimliği** daha sonra gerekeceği için bir metin düzenleyicisine değeri.

   ![Kaynak özellikleri](./media/spatial-anchors-get-started-create-resource/view-resource-properties.png)

Altında **ayarları**seçin **anahtar**. Kopyalama **birincil anahtar** bir metin düzenleyicisine değeri. Bu değer `Account Key`. Buna daha sonra ihtiyacınız olacak.

   ![Hesap anahtarı](./media/spatial-anchors-get-started-create-resource/view-account-key.png)
