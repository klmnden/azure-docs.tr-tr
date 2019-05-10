---
title: Azure IOT Central kullanıcı Arabirimi özelleştirme | Microsoft Docs
description: Azure IOT central uygulamanız için tema ve Yardım bağlantıları özelleştirme
author: dominicbetts
ms.author: dobett
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 0256396cd228898f3852772b113e6064a0656746
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65237665"
---
# <a name="customize-the-azure-iot-central-ui"></a>Azure IOT Central kullanıcı Arabirimi özelleştirme 

*Bu makale, Yöneticiler için geçerlidir.*

IOT Central özel temalar uygulayarak ve Yardım bağlantıları için kendi özel bir Yardım kaynakları işaret edecek şekilde değiştirilmesi, uygulamanızın kullanıcı arabirimini özelleştirmenizi sağlar. Aşağıdaki ekran görüntüsünde, standart tema kullanarak bir sayfa görüntülenir:

![Standart IOT Central tema](./media/howto-customize-ui/standard-ui.png)

Aşağıdaki ekran görüntüsünde vurgulanmış özelleştirilmiş kullanıcı Arabirimi öğeleri ile özel bir ekran görüntüsü kullanan bir sayfa gösterilmektedir:

![Özel IOT Central tema](./media/howto-customize-ui/themed-ui.png)

## <a name="create-theme"></a>Tema oluşturma

Özel tema oluşturma için gidin **uygulamanızı özelleştirme** sayfasını **Yönetim** bölümü:

![IOT Central temalar](./media/howto-customize-ui/themes.png)

Bu sayfada, uygulamanızın şu yönlerini özelleştirebilirsiniz:

### <a name="application-logo"></a>Uygulama logosu

Bir PNG görüntüsünü, saydam arka plana sahip 1 MB'den büyük. Bu logo, IOT Central uygulaması başlık çubuğunda sol görüntüler.

Logo resmi uygulamanızın adını içeriyorsa, uygulama adı metin gizleyebilirsiniz. Daha fazla bilgi için [uygulama ayarlarını yönetme](./howto-administer.md#manage-application-settings).

### <a name="browser-icon-favicon"></a>Tarayıcı simgesine (favicon)

Bir PNG görüntüsünü, saydam arka plana sahip 32 x 32 piksel değerinden daha büyük. Bir web tarayıcısında adres çubuğuna, geçmiş, yer işaretleri ve tarayıcı sekmesinde bu görüntüde kullanabilirsiniz.

### <a name="browser-colors"></a>Tarayıcı renkleri

Sayfa üstbilgisi rengini ve düğme ve diğer önemli accenting için kullanılan rengi değiştirebilirsiniz. Altı karakterlik bir onaltılık renk değer biçiminde kullanın `##ff6347`. Hakkında daha fazla bilgi için **ONALTILIK değer** renk gösterimi için bkz. [HTML renkleri](https://www.w3schools.com/html/html_colors.asp).

> [!NOTE]
> Üzerinde her zaman varsayılan seçenekleri döndürebilirsiniz **uygulamanızı özelleştirme** sayfası.

### <a name="changes-for-operators"></a>İşleçler için değişiklikler

Bir yönetici özel bir tema oluşturur sonra operatörler ve uygulamanızın diğer kullanıcılar artık seçim yapabileceğiniz bir tema **ayarları**.

## <a name="replace-help-links"></a>Yardım bağlantıları Değiştir

Operatörler ve diğer kullanıcılar için özel bir Yardım bilgileri sağlamak için uygulama bağlantıları değiştirebilirsiniz **yardımcı** menüsü.

Yardım bağlantıları değiştirmek için gidin **Yardım özelleştirme** sayfasını **Yönetim** bölümü:

![IOT Central Yardım bağlantıları özelleştirme](./media/howto-customize-ui/help-links.png)

Ayrıca, Yardım menüsüne yeni girişler ekleyin ve varsayılan girdilerini kaldırın:

![Özelleştirilmiş IOT Central Yardım](./media/howto-customize-ui/custom-help.png)

> [!NOTE]
> Üzerinde her zaman varsayılan Yardım bağlantıları geri döndürebilirsiniz **Yardım özelleştirme** sayfası.

## <a name="next-steps"></a>Sonraki adımlar

IOT Central uygulamanızın kullanıcı arabirimini özelleştirmek öğrendiniz, bazı önerilen sonraki adımlar şunlardır:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetmek](./howto-administer.md)
> [uygulama Panosu yapılandırın](./howto-configure-homepage.md)