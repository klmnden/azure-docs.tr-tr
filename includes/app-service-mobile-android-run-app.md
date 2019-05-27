---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 505eac0996129a17b6b68e8ab4ea2d4fc80fd473
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140999"
---
1. [Azure Portal]’ı ziyaret edin. **Tümüne Gözat** > **Mobile Apps** > henüz oluşturmuş olduğunuz arka uca tıklayın. Mobil uygulama ayarlarında, **Hızlı Başlangıç** > **Android**'e tıklayın. **İstemci uygulamanızı yapılandırın**’ın altında **İndir**’e tıklayın. Arka ucunuza bağlanacak önceden yapılandırılmış bir uygulama için tam bir Android projesini indirir. 
2. **Android Studio** kullanarak, **Projeyi içeri aktar (Eclipse ADT, Gradle, vb.)** kullanarak projeyi açın. JDK hatalarını önlemek için bu içeri aktarma seçimini yaptığınızdan emin olun.
3. Projeyi oluşturmak için **’Uygulamayı’ çalıştır** düğmesine basın ve uygulamayı Android benzeticisinde başlatın.
4. Uygulamada, *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından ‘Ekle’ düğmesine basın. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler. 
   
    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure Portal]: https://portal.azure.com/
