---
title: IOT Central'ı Azure portalından yönetin | Microsoft Docs
description: IOT Central, Azure portalından yönetin.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/30/2018
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 73c8d64c53e23601b19a775a07b0b630c675b91f
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200486"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Azure portalından IOT Central'ı yönetme 
Oluşturma ve IOT Central Web sitesinden IOT Central uygulamaları yönetmeye ek olarak, IOT Central Azure portalından yönetebilirsiniz. Bu makalede, nelerin mümkün olduğunu ve bunun nasıl yapıldığını göstereceğiz.

## <a name="create-iot-central-applications"></a>IOT Central uygulamaları oluşturma
Yeni bir uygulama oluşturmak için gidin [Azure portalında](https://ms.portal.azure.com) ve sol taraftaki ana gezinti menüsündeki "Kaynak Oluştur"'a tıklayın. 

![Yönetim Portalı: Gezinti Menüsü](media/howto-manage-iot-central-from-portal/image0.png)

"IoT Central" terimi, arama çubuğu türü içinde.

![Yönetim Portalı: arama](media/howto-manage-iot-central-from-portal/image0a.png)

IOT Central uygulamasına satır öğesi arama sonuçlarında tıklayın.

![Yönetim Portalı: arama sonuçları](media/howto-manage-iot-central-from-portal/image0b.png)

Şimdi, doldurmanız gereken form görmek için "Oluştur" düğmesine tıklayın.

![Yönetim Portalı: IOT Central kaynak](media/howto-manage-iot-central-from-portal/image0c.png)

Formdaki tüm alanları doldurun. Bu form, formun IOT Central Web sitesinden uygulamaları oluşturmak için doldurmak için ihtiyacınız benzerdir. Her alanı doldurun hakkında daha fazla bilgi için kullanıma [IOT Central uygulamasına oluşturma](quick-deploy-iot-central.md) hızlı başlangıç. 

![Yönetim Portalı: IOT Central kaynak oluştur](media/howto-manage-iot-central-from-portal/image1.png)  

Tüm alanları doldurduktan sonra "Oluştur" düğmesine tıklayın.

## <a name="manage-existing-iot-central-applications"></a>Mevcut IOT Central uygulamaları yönetme
Bir Azure IOT Central uygulamasına zaten varsa, silebilirsiniz taşıyabilirsiniz Azure portalında farklı bir abonelik veya kaynak grubu. Bu deneme aboneliği yedekler beri Azure portalında deneme uygulamalarını göremez.

Başlamak için sol taraftaki ana gezinti menüsündeki "tüm kaynaklar" a tıklayın. Uygulamanızın adını yazın ve kaynak listesinde, bulmak için arama kutusunu kullanın. Yönetmek istediğiniz IOT Central uygulaması üzerinde'ye tıklayın.

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image2.png)

Uygulamaya gidin, IOT Central uygulaması URL'Sİ'a tıklayın.

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image3.png)

Uygulamayı farklı bir kaynak grubuna taşımak için tıklatın **değiştirme** bağlantı kaynak grubunun yanında. Bu uygulamada görüntülenen iletişim geçirmek istediğiniz kaynak grubunu seçin.

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image4.png)

Uygulamayı farklı bir aboneliğe taşımak için tıklatın **değiştirme** abonelik yanında bağlantı. Bu uygulamada görüntülenen iletişim geçirmek istediğiniz aboneliği seçin.

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image5.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure portalından Azure IOT Central uygulamaları yönetmek öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)