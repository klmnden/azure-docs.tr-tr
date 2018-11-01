---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 1d1d593b7305e0cd9899f4ec388cb441ced90b10
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133676"
---
1. Mac’inizde [Azure Portal] ziyaret edin. **Tüm Hizmetler** > **Uygulama Hizmetleri** > oluşturduğunuz arka uca tıklayın. Mobil uygulama ayarlarında, tercih ettiğiniz dili seçin:

    - Objective-C &ndash; **Hızlı Başlangıç** > **iOS (Objective-C)**
    - Swift &ndash; **Hızlı Başlangıç** > **iOS (Swift)**

    **3. İstemci uygulamanızı yapılandırın**’ın altında **İndir**’e tıklayın. Arka ucunuza bağlanacak önceden yapılandırılmış tam bir Xcode projesini indirir. Projeyi Xcode kullanarak açın.

1. Projeyi oluşturmak için **Çalıştır** düğmesine basın ve uygulamayı iOS benzeticisinde başlatın.

1. Uygulamada, *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından artı (**+**) simgesine basın. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler.

   ![iOS’ta çalışan hızlı başlangıç uygulaması](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure Portal]: https://portal.azure.com/
