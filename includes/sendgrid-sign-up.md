---
author: erikre
ms.service: multiple
ms.topic: include
ms.date: 11/25/2018
ms.author: erikre
ms.openlocfilehash: 96c4da8465a87fee4c00bfc6177515c94910704a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121370"
---
Azure müşterileri her ay 25.000 ücretsiz e-postanın kilidini açabilir. Bu aylık 25.000 ücretsiz e-posta sayesinde gelişmiş raporlama ve analitiklerin yanı sıra [tüm API'lere][all APIs] (Web, SMTP, Olay, Ayrıştırma ve diğerleri) erişebilirsiniz. SendGrid tarafından sağlanan ek hizmetler hakkında bilgi için [SendGrid Solutions][SendGrid Solutions] sayfasını ziyaret edin.

### <a name="to-sign-up-for-a-sendgrid-account"></a>SendGrid hesabı açmak için
1. [Azure portalında][Azure portal] oturum açın.
2. Soldaki menüde **kaynak Oluştur**.

    ![komut-çubuğu-yeni][command-bar-new]
3. **Eklentiler**'e ve ardından **SendGrid E-posta Teslimi**'ne tıklayın.

    ![sendgrid-mağazası][sendgrid-store]
4. Kayıt formunu doldurun ve **Oluştur**'u seçin.

    ![sendgrid-oluştur][sendgrid-create]
5. SendGrid hizmetinizi Azure ayarlarınızda tanımlamak için bir **Ad** girin. Adın 1-100 karakter arasında olması ve yalnızca alfasayısal karakterler, çizgi, nokta ve alt çizgi içermesi gerekir. Adın abone olunan Azure Mağazası Öğeleri arasında benzersiz olması gerekir.
6. **Parolanızı** girin ve onaylayın.
7. **Aboneliğinizi** seçin.
8. Yeni bir **Kaynak grubu** oluşturun veya var olanlardan birini kullanın.
9. **Fiyatlandırma katmanı** bölümünde kaydolmak istediğiniz SendGrid planını seçin.

    ![sendgrid-fiyatlandırması][sendgrid-pricing]
10. Varsa **Promosyon Kodu** girin.
11. **İletişim Bilgilerinizi** girin.
12. **Yasal koşulları** gözden geçirin ve kabul edin.
13. Satın alma işlemini onayladıktan sonra **Dağıtım Başarılı** açılır penceresini ve **Tüm kaynaklar** bölümündeki listede hesabınızı göreceksiniz.

    ![tüm-kaynaklar][all-resources]

    Satın alma işlemini tamamladıktan ve e-posta doğrulama işlemini başlatmak için **Yönet** düğmesine tıkladıktan sonra SendGrid'den hesabınızı doğrulamanızı isteyen bir e-posta alacaksınız. Bu e-postayı almadıysanız veya hesabınızı doğrulamayla ilgili sorunlar yaşıyorsanız lütfen bu SSS sayfasını inceleyin.

    ![yönet][manage]

    **Hesabınızı doğrulayana kadar günde en fazla 100 e-posta gönderebilirsiniz.**

    Abonelik planınızı değiştirmek veya SendGrid iletişim ayarlarını görüntülemek için SendGrid hizmetinizin adına tıklayarak SendGrid Marketplace panosunu açın.

    ![ayarlar][settings]

    SendGrid kullanarak bir e-posta göndermek için API Anahtarınızı girmeniz gerekir.

### <a name="to-find-your-sendgrid-api-key"></a>SendGrid API Anahtarınızı bulmak için
1. **Yönet**'e tıklayın.

    ![yönet][manage]
2. SendGrid panonuzda **Ayarlar**'ı ve ardından sol taraftaki menüden **API Anahtarları**'nı seçin.

    ![api-anahtarları][api-keys]

3. Tıklayın **API anahtarı oluşturma**.

    ![genel-api-anahtarı][general-api-key]
4. En azından **Bu anahtarın adı** alanını doldurun, **Posta Gönderimi** için tam erişim verin ve **Kaydet**'i seçin.

    ![erişim][access]
5. API anahtarınız bu adımda bir kez görüntülenir. Güvenli bir yerde saklamayı unutmayın.

### <a name="to-find-your-sendgrid-credentials"></a>SendGrid kimlik bilgilerinizi bulmak için
1. **Kullanıcı adınızı** bulmak için anahtar simgesine tıklayın.

    ![anahtar][key]
2. Parola, kurulum sırasında seçtiğiniz paroladır. **Parolayı değiştir** veya **Parolayı sıfırla**'yı seçerek değişiklik yapabilirsiniz.

E-posta teslim ayarlarınızı yönetmek için **Yönet** düğmesine tıklayın. Bu, SendGrid Panonuzda yönlendirir.

![yönet][manage]

SendGrid aracılığıyla e-posta gönderme hakkında daha fazla bilgi için ziyaret [e-posta API'sine genel bakış][Email API Overview].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/new-addon.png
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid-store.png
[sendgrid-create]: ./media/sendgrid-sign-up/sendgrid-create.png
[sendgrid-pricing]: ./media/sendgrid-sign-up/sendgrid-pricing.png
[all-resources]: ./media/sendgrid-sign-up/all-resources.png
[manage]: ./media/sendgrid-sign-up/manage.png
[settings]: ./media/sendgrid-sign-up/settings.png
[api-keys]: ./media/sendgrid-sign-up/api-keys.png
[general-api-key]: ./media/sendgrid-sign-up/general-api-key.png
[access]: ./media/sendgrid-sign-up/access.png
[key]: ./media/sendgrid-sign-up/key.png

<!--Links-->

[SendGrid Solutions]: https://sendgrid.com/solutions
[Azure portal]: https://portal.azure.com
[SendGrid Getting Started]: http://sendgrid.com/docs
[SendGrid Provisioning Process]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[all APIs]: https://sendgrid.com/docs/API_Reference/index.html
[Email API Overview]: https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html
