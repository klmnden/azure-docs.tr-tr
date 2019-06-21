---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 75bcb9d27ee6f66a1d9c15093d9f933a3ad25881
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188868"
---
1. Visual Studio Çözüm Gezgini'nde, Windows Store uygulaması projesine sağ tıklayın. Ardından **Store** > **Store uygulamayla ilişkilendirmek**.

    ![Uygulamayı Windows Store ile ilişkilendirme](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Sihirbazda **sonraki**. Ardından, Microsoft hesabınızla oturum açın. İçinde **yeni bir uygulama adı ayrılmaya**, uygulamanız için bir ad yazın ve ardından **ayırma**.
3. Uygulama kaydı başarıyla oluşturulduktan sonra yeni bir uygulama adı seçin. Seçin **sonraki**ve ardından **ilişkilendirmek**. Bu işlem, uygulama bildirimine gerekli Windows Store kayıt bilgilerini ekler.
4. Adım 1 ve 3, daha önce oluşturduğunuz aynı kayıt için Windows Store uygulaması kullanarak Windows Phone Store uygulaması projesi için tekrarlayın.  
5. Git [Windows Dev Center](https://dev.windows.com/en-us/overview)ve ardından Microsoft hesabınızla oturum açın. İçinde **uygulamalarım**, yeni uygulama kaydı seçin. Ardından **Hizmetleri** > **anında iletme bildirimleri**.
6. Üzerinde **anında iletme bildirimleri** sayfasındaki **Windows anında bildirim Hizmetleri (WNS) ve Microsoft Azure Mobile Apps**seçin **Live Services sitesi**.  Değerlerini not edin **paket SID'si** ve *geçerli* değerini **uygulama gizli anahtarı**. 

    ![Geliştirici Merkezi'nde uygulama ayarı](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Uygulama gizli anahtarı ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın bunları kullanmayın.
   >
   >
