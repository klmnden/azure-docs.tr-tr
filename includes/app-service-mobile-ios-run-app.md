---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: f4ba467b6d80c9ccafba0a91c1f04152b92cf869
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140658"
---
1. Mac’inizde [Azure portal] ziyaret edin. **Tüm Hizmetler** > **Uygulama Hizmetleri** > oluşturduğunuz arka uca tıklayın. Mobil uygulama ayarlarında, tercih ettiğiniz dili seçin:

   - Objective-C &ndash; **Hızlı Başlangıç** > **iOS (Objective-C)**
   - Swift &ndash; **Hızlı Başlangıç** > **iOS (Swift)**

     **3. İstemci uygulamanızı yapılandırın**’ın altında **İndir**’e tıklayın. Arka ucunuza bağlanacak önceden yapılandırılmış tam bir Xcode projesini indirir. Projeyi Xcode kullanarak açın.

1. Projeyi oluşturmak için **Çalıştır** düğmesine basın ve uygulamayı iOS benzeticisinde başlatın.

1. Uygulamada, artı işaretine tıklayın (**+**) simgesi gibi anlamlı bir metin yazın *öğreticiye*ve sonra Kaydet'e tıklayın düğmesi. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler.

   ![iOS’ta çalışan hızlı başlangıç uygulaması](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure portal]: https://portal.azure.com/
