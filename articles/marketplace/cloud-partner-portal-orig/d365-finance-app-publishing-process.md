---
title: Uygulama adımları yayımlama | Azure Market
description: Bir uygulamayı Azure Market'te yayımlamak için adımlar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pabutler
ms.openlocfilehash: 04278f50366ee91738fd36e64331572e14baf17c
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935150"
---
# <a name="app-publishing-steps"></a>Uygulama adımları yayımlama

Yayımlama işlemini başlatmak için "Yayımla" altında Düzenleyici sekmesini tıklatın.

![CPP uygulama Yayımla düğmesi](./media/d365-financials/image014.jpg)


Durum sekmesinde yayımlama teklifinizi yayımlama işleminin neresinde olduğunu belirten adımlar görürsünüz. Yayımlama işleminde herhangi bir noktada oturum açın ve herhangi bir Teklifleriniz için en son durumu görüntülemek için tüm sunar sekmesine tıklayın. Teklifiniz için doğrudan duruma tıklayın ve teklifinizi yayımlama işleminin neresinde olduğunu görebilir.

Her yayımlama adımları atalım, her adımda neler tartışın ve her bir adımın ne kadar süreyle tahmin götürür.

![Yayımlama işlemi diyagramı](./media/d365-financials/image017.png)


**Önkoşulları doğrulama**

"Yayımla"'ye tıkladığınızda bir otomatik onay teklifinizi tüm gerekli alanları doldurduktan sonra emin olmak için gerçekleşir. Herhangi bir alan, alanın yanında bir uyarı görünür doldurulmaz ve doğru bir şekilde doldurmak ihtiyacınız olacak 'Yayımla' tekrar tıklayın.

Doğru bu adımı tamamladıktan sonra yayımlama bildirimleri göndermek için kullanılacak bir e-posta adresi için bir açılır pencere istenecektir. Bu adım, e-posta adresinizi gönderildikten sonra tamamlanmıştır.


**Otomatik uygulama doğrulama**

Bu adımda, uygulama uzantıları, içeriklerini ile eşleşen bir teklifi ile sağlanan meta verileri sunmak otomatik sertifika hizmetimiz denetler. Her zaman uygulama adı, sürüm, yayımcı ve kimliği ile adlı uzantı bildiriminde belirtilen olduğundan emin olun `app.json`.


**Sürücü doğrulama testi**

Test Sürüşü ' ayarlamak için seçtiyseniz, bu Test Sürüşü ayarlarınızı burada doğrulanır aşamadır.


**Tedarik Yönetimi doğrulama ve kayıt**

Müşteri adayı oluşturma özelliği yapılandırdıysanız bu aşamasında, biz CRM tümleştirmenizi CRM'İNİZE test müşteri adayı göndererek çalışır durumda olduğunu doğrular. Bu adım tamamlandıktan sonra CRM veya Azure tablo doldurmak sahte veri içeren bir kayıt görürsünüz. Müşteri adayı oluşturma için tüm belgeleri burada bulunur.


**AppSource paketleme**

Mağaza ayrıntıları yapıtlarınızı denetlenir ve AppSource önizlemesi paket oluşturulur.


**Yayımcı Oturumu Kapat**

Bu aşamasında **Go Live** düğmesi artık etkin olacak. Ayrıca artık (ile hidekey) teklifinizin önizlemesi için bir bağlantı gerekir. Önizleme nasıl göründüğünü ile memnun olduğunuzda, Go Live düğmesine tıklayın.
Unutmayın, bu istekte bulunma değil uygulamanızı Canlı uygulama kaynağında, ancak bunun yerine iç doğrulama sürecimiz tetikler.


**Pazarlama ve teknik uygulama doğrulama**

Bu adım, burada size pazarlama ve teknik doğrulamaları paralel yürütmek bağlıdır. Başvurmak [uygulamanızı göndermek için Denetim listesi](https://aka.ms/CheckBeforeYouSubmit) ve [teknik incelemesi Finans ve operasyon için Dynamics 365 için geliştirme uygulamaları](https://go.microsoft.com/fwlink/?linkid=841518) zorunlu gereksinimleri ve önerileri için rehberlik belgeleri. Doğrulama işlemi sırasında yapacağız:
-  sizinle herhangi bekleyen soruları ve sorunları çalışır.  
- bir uygulama yayımlama tarihi ile sağlayın ve uygulamanız yayımlandığında size bildirir. 
- Teknik ve pazarlama doğrulama 5-7 iş günü içinde ilgili bir ilk geri bildirim sağlar.

Bu adımları genellikle bir hafta içerisinde alabilir ve, bulut iş ortağı portalına sürekli olarak kapatmayın gerek yoktur.


**Hizmet uygulaması yayımlama**

Teklifiniz, bazı son işlemeden geçiyor. Uygulamanızı pazarlama ve teknik doğrulamadan geçti, ancak artık uygulama kaynağı için hazır hale getirmek için son işlemleri aracılığıyla gitmeniz gerekir.


**Canlı**

Appsource'ta Canlı teklifinizi olan ve müşterilerin görüntüleyebilir ve Microsoft Dynamics 365 Business Central aboneliklerini uygulamanızda dağıtmak olacaktır. Bizim uygulamanızı uygulama kaynağında genel yapılmadığını bildiren bir e-posta alırsınız. Herhangi bir noktada tüm teklifler sekmesine tıklayın ve sağ sütunda listelenen, tüm tekliflerin durumlarını görebilirsiniz. Teklifiniz için ayrıntılı yayımlama akış durumunu görmek için durumunu tıklayabilirsiniz.


<a name="error-handling"></a>Hata işleme
--------------

Yayımlama işlemi sırasında bir hatayla karşılaştı. Hatayla karşılaşılırsa, bir hata üzerinden sonraki adımlarla ilgili yönergeleri ile ortaya çıktığını bildiren bir bildirim e-posta alırsınız. Hataları bu işlem sırasında herhangi bir zamanda durumu sekmesine tıklayarak da görebilirsiniz. Hatanın çözülmesi için gerekenler anahat bir hata iletisi ile birlikte işleminde hangi işaret göreceksiniz.

Yayımlama işlemi sırasında hatalarla karşılaşırsanız, bu hataları düzeltin ve ardından gerekli **Yayımla** işlemini yeniden başlatmak için. Yayımlama adımları başlangıcında başlamalıdır **doğrulama ön koşullar** sonra herhangi bir hata düzeltme yeniden yayımlanması olduğunda.

Bir hatayı giderme sorunları yaşıyorsanız destek mühendislerimizle Yardım almak için bir destek isteği açın.


<a name="canceling-the-publishing-request"></a>Yayımlama istek iptal ediliyor
--------------------------------

Yayımlama sürecini başlatmak ve isteğiniz iptal gerekmesi. Yayımlama isteği yayımcı sonuçlandırma adım ulaştığında, yalnızca bir yayımlama isteği iptal edebilirsiniz. İptal etmek için tıklayın **yayımlamayı iptal et**. Adım 1'e Yayımlama durumunu sıfırlar ve tekrar yayımlamanız için Yayımla'ya tıklayın ve durum adımları izleyin.

![İptal düğmesi yayımlama](./media/d365-financials/image013.png)
