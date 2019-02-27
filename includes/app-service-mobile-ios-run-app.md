---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 8146489a913ce863cee7534331231a248a3ea7ac
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56890972"
---
1. Mac’inizde [Azure portal] ziyaret edin. **Tüm Hizmetler** > **Uygulama Hizmetleri** > oluşturduğunuz arka uca tıklayın. Mobil uygulama ayarlarında, tercih ettiğiniz dili seçin:

    - Objective-C &ndash; **Hızlı Başlangıç** > **iOS (Objective-C)**
    - Swift &ndash; **Hızlı Başlangıç** > **iOS (Swift)**

    **3. İstemci uygulamanızı yapılandırın**’ın altında **İndir**’e tıklayın. Arka ucunuza bağlanacak önceden yapılandırılmış tam bir Xcode projesini indirir. Projeyi Xcode kullanarak açın.

1. Projeyi oluşturmak için **Çalıştır** düğmesine basın ve uygulamayı iOS benzeticisinde başlatın.

1. Uygulamada, artı işaretine tıklayın (**+**) simgesi gibi anlamlı bir metin yazın *öğreticiye*ve sonra Kaydet'e tıklayın düğmesi. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler.

   ![iOS’ta çalışan hızlı başlangıç uygulaması](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure portal]: https://portal.azure.com/
