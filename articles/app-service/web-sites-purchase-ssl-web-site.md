---
title: Satın alma ve Azure uygulama hizmetiniz için bir SSL sertifikası yapılandırma | Microsoft Docs
description: Bir uygulama hizmeti sertifika satın alın ve uygulama hizmeti uygulamanızı bağlama hakkında bilgi edinin
services: app-service
documentationcenter: .net
author: cephalin
manager: cfowler
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: apurvajo;cephalin
ms.openlocfilehash: 8c1db4693c6816ca7c3cc5b3147c0e8f3f8179c5
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807467"
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Azure App Service için SSL Sertifikası Satın Alma ve Yapılandırma

Bu öğreticide, web uygulamanız için bir SSL sertifikası satın alarak güvenli hale getirmek nasıl gösterilir,  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, güvenli bir şekilde de depolamak [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis), ve özel bir etki alanı ile ilişkilendirme.

## <a name="step-1---log-in-to-azure"></a>1. adım - Azure oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın

## <a name="step-2---place-an-ssl-certificate-order"></a>2. adım - bir SSL sertifikası sipariş verin

Yeni bir oluşturarak bir SSL sertifikası sipariş yerleştirebilirsiniz [uygulama hizmet sertifikası](https://portal.azure.com/#create/Microsoft.SSL) içinde **Azure portal**.

![Sertifika oluşturma](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Kolay bir girin **adı** girin ve için SSL sertifika **etki alanı adı**

> [!NOTE]
> Bu adım satın alma işleminin en önemli parçalarından biridir. Bu sertifika ile korumak istediğiniz doğru ana bilgisayar adı (özel etki alanı) girdiğinizden emin olun. **SAĞLAMADIĞI** WWW ana bilgisayar adıyla ekleyin. 
>

Seçin, **abonelik**, **kaynak grubu**, ve **sertifika SKU**

> [!TIP]
> Uygulama Hizmeti sertifikaları uygulama hizmetleri için sınırlı değildir ve herhangi bir Azure veya Azure Hizmetleri için kullanılabilir. Bunu yapmak için onu istediğiniz bir yerde kullanabileceğiniz bir uygulama hizmeti sertifikası yerel bir PFX kopyasını oluşturmanız gerekir. Daha fazla bilgi için okuma [bir uygulama hizmet sertifikası yerel bir PFX kopyasını oluşturma](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>3. adım - Azure anahtar kasası sertifika deposu

> [!NOTE]
> [Anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) şifreleme anahtarları ve gizli anahtarları bulut uygulamalar ve hizmetler tarafından kullanılan korumaya yardımcı olan bir Azure hizmetidir.
>

SSL sertifikası satın alma işlemi tamamlandıktan sonra açmak gereken [uygulama hizmeti sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfası.

![içinde KV depolamak hazır görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

Sertifika durum **"Bekleyen verme"** olarak bu sertifikayı kullanmaya başlamadan önce tamamlamanız gereken birkaç adım vardır.

' I tıklatın **sertifika Yapılandırması** sertifika özellikleri sayfasında ve tıklayın **adım 1: depolama** Azure anahtar Kasası ' Bu sertifikayı depolamak için.

Gelen **anahtar kasası durumu** sayfasında, **anahtar kasası deposu** Bu sertifika deposu için var olan bir anahtar kasası seçmek için **veya oluşturma yeni anahtar kasası** yeni anahtar kasası oluşturmak için aynı abonelik ve kaynak grubu içinde.

> [!NOTE]
> Azure anahtar kasası bu sertifikayı depolamak için en düşük ücretler sahiptir.
> Daha fazla bilgi için bkz:  **[Azure anahtar kasası fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Bu sertifikada depolamak için anahtar kasası deposu seçtikten sonra **depolamak** seçeneğini başarı göster.

![KV deposu başarılı bir görüntüsünü Ekle](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>Adım 4 - etki alanı sahipliğini doğrulayın

Aynı **sertifika Yapılandırması** adım 3'te kullanılan sayfasını tıklatın **2. adım: doğrulama**.

Tercih edilen etki alanı doğrulama yöntemi seçin. 

Uygulama Hizmeti sertifikaları tarafından desteklenen etki alanı doğrulama dört tür vardır: uygulama hizmeti, etki alanı, posta ve el ile doğrulama. Bu doğrulama türlerini daha ayrıntılı olarak açıklanmıştır [bölüm Gelişmiş](#advanced).

> [!NOTE]
> **Uygulama hizmeti doğrulama** doğrulamak istediğiniz etki alanını bir App Service uygulaması aynı abonelikte zaten eşlenmiş en uygun seçenektir. App Service uygulaması zaten etki alanı sahipliğini doğruladı olgu yararlanır.
>

Tıklayın **doğrula** bu adımı tamamlamak için düğmesi.

![etki alanı doğrulama görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

' I tıklattıktan sonra **doğrulayın**, kullanın **yenileme** kadar düğme **doğrula** seçeneğini başarı göster.

![INSERT görüntüsü KV başarılı doğrulayın](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a>Adım 5 - App Service uygulaması için Ata sertifika

> [!NOTE]
> Bu bölümdeki adımları gerçekleştirmeden önce bir özel etki alanı adı uygulamanızla ilişkili sahip olmalıdır. Daha fazla bilgi için bkz:  **[bir web uygulaması için bir özel etki alanı adı yapılandırma.](app-service-web-tutorial-custom-domain.md)**
>

İçinde  **[Azure portal](https://portal.azure.com/)**, tıklatın **uygulama hizmeti** sayfanın sol seçeneği.

Bu sertifikayı atamak istediğiniz uygulamanın adına tıklayın.

İçinde **ayarları**, tıklatın **SSL ayarları**.

Tıklatın **alma uygulaması hizmet sertifikası** ve yalnızca satın sertifikayı seçin.

![Sertifika İçeri Aktar görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

İçinde **ssl bağlamaları** tıklatın bölümünde **bağlamaları Ekle**ve bırakmalar SSL ve sertifika ile kullanmak için güvenli hale getirmek için etki alanı adını seçmek için kullanın. Kullanılıp kullanılmayacağını da seçebilirsiniz **[sunucu adı göstergesi (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** veya IP tabanlı SSL.

![SSL bağlamaları görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Tıklatın **bağlaması Ekle** değişiklikleri kaydederek SSL'i etkinleştirmek için.

> [!NOTE]
> Seçtiyseniz **IP tabanlı SSL** ve özel etki alanınızı bir A kaydı kullanılarak yapılandırılır, aşağıdaki ek adımları gerçekleştirmeniz gerekir. Bunlar daha ayrıntılı olarak açıklanmıştır [bölüm Gelişmiş](#Advanced).

Bu noktada, uygulamasını kullanarak ziyaret görüyor olmalısınız `HTTPS://` yerine `HTTP://` sertifika doğru şekilde yapılandırıldığını doğrulayın.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Adım 6 - yönetim görevleri

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a>Gelişmiş

### <a name="verifying-domain-ownership"></a>Etki alanı sahipliğini doğrulama

Uygulama Hizmeti sertifikaları tarafından desteklenen etki alanı doğrulama daha fazla iki tür vardır: posta ve el ile doğrulama.

#### <a name="mail-verification"></a>Posta Doğrulama

Bu özel etki alanı ile ilişkili e-posta adresi doğrulama e-posta zaten gönderildi.
E-posta doğrulama adımı tamamlamak için e-posta açın ve doğrulama bağlantısını tıklatın.

![e-posta doğrulama görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

Doğrulama e-postayı yeniden göndermek gerekiyorsa, tıklatın **yeniden e-posta** düğmesi.

#### <a name="domain-verification"></a>Etki Alanı Doğrulama

Bu seçeneği yalnızca [Azure'dan satın alınan bir uygulama hizmeti etki alanı.](custom-dns-web-site-buydomains-web-app.md). Azure otomatik olarak, TXT kaydını doğrulama ekler ve işlemini tamamlar.

#### <a name="manual-verification"></a>El ile Doğrulama

> [!IMPORTANT]
> HTML Web sayfası Doğrulama (yalnızca standart sertifika SKU çalışır)
>

1. Adlı bir HTML dosyası oluşturmak **"starfield.html"**

1. Bu dosya adı tam etki alanı doğrulama belirteci olarak içerik olması gerekir. (Etki alanı doğrulama durumunu sayfasından belirteç kopyalayabilirsiniz)

1. Etki alanınızı barındıran web sunucusunun kökünde bu dosyayı karşıya yükleme `/.well-known/pki-validation/starfield.html`

1. Tıklatın **yenileme** doğrulama tamamlandıktan sonra sertifika durumunu güncelleştirmek için. Doğrulama tamamlanması için birkaç dakika sürebilir.

> [!TIP]
> Terminal kullanarak bir doğrulama `curl -G http://<domain>/.well-known/pki-validation/starfield.html` yanıt içermelidir `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>DNS TXT kaydını doğrulama

1. DNS Yöneticisi'ni kullanarak bir TXT kaydı oluşturun üzerinde `@` değeri etki alanı doğrulama belirteci için eşit olan bir alt etki alanı.
1. Tıklatın **"Yenile"** doğrulama tamamlandıktan sonra sertifika durumunu güncelleştirmek için.

> [!TIP]
> TXT kaydı oluşturmak gereken `@.<domain>` değerle `<verification-token>`.

### <a name="assign-certificate-to-app-service-app"></a>App Service'e bir sertifikayı ata

Seçtiyseniz **IP tabanlı SSL** ve özel etki alanınızı bir A kaydı kullanılarak yapılandırılır, aşağıdaki ek adımları gerçekleştirmeniz gerekir:

Yapılandırdıktan sonra bir IP temelli SSL bağlaması, uygulamanıza bir ayrılmış IP adresi atanır. Bu IP adresi bulabilirsiniz **özel etki alanı** sayfa altında ayarları, uygulamanızın üzerinde doğru **ana bilgisayar adları** bölümü. Olarak listelenen **dış IP adresi**

![IP SSL görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Bu IP adresi, etki alanınız için A kaydı yapılandırmak için daha önce kullanılan sanal IP adresi farklıdır. Kullanmak üzere yapılandırılmışsa SNI tabanlı SSL ya da SSL kullanmak üzere yapılandırılmamış, bu giriş için bir adresinin.

Etki alanı adı kayıt şirketiniz tarafından sağlanan araçları kullanarak, önceki adımdaki IP adresine işaret edecek şekilde özel etki alanı adınız için A kaydını değiştirin.

## <a name="rekey-and-sync-the-certificate"></a>Anahtar yenileme ve sertifika eşitleme

Anahtar yenileme, sertifika ihtiyacınız varsa, seçin **yeniden anahtarlama ve eşitleme** gelen seçeneği **sertifika özellikleri** sayfası.

Tıklatın **anahtar yenileme** işlemini başlatmak için düğmesi. Bu işlemi tamamlamak için 1-10 dakika sürebilir.

![anahtar yenileme SSL görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Sertifikanızı yeniden anahtarlama sertifikanın sertifika yetkilisi tarafından verilen yeni bir sertifika ile yapar.

## <a name="renew-the-certificate"></a>Sertifikayı Yenile

Sertifikanın otomatik yenilenmesini istediğiniz zaman açmak için tıklatın **otomatik yenileme ayarlarını** sertifika Yönetimi sayfasında. Seçin **üzerinde** tıklatıp **kaydetmek**.

![](./media/app-service-web-purchase-ssl-web-site/auto-renew.png)

Sertifikayı bunun yerine el ile yenilemek için tıklatın **el ile yenileme** yerine.

> [!NOTE]
> Yenilenen sertifikanın otomatik olarak, uygulamanızın, el ile yenileme ya da otomatik olarak yenilenmesi bağlı değil. Uygulamanıza bağlamak için bkz: [sertifikalarını yeniler](./app-service-web-tutorial-custom-ssl.md#renew-certificates). 

<a name="notrenewed"></a>
## <a name="why-is-my-certificate-not-auto-renewed"></a>Neden my sertifikası otomatik-yenilenir değil mi?

SSL sertifikanızı otomatik yenileme için yapılandırılmış, ancak otomatik olarak yenilenmediği, bekleyen etki alanı doğrulama olabilir. Şunlara dikkat edin: 

- Uygulama Hizmeti sertifikaları oluşturur, GoDaddy etki alanı doğrulama iki yılda bir kez gerektirir. Etki alanı yöneticisi etki alanını doğrulamak için her üç yıla bir e-posta alır. E-postayı denetlemek veya etki alanınızda doğrulamak için hata uygulama hizmet sertifikası otomatik olarak yenilenmesi engeller. 
- (Otomatik yenileme için sertifika etkin olsa bile) GoDaddy İlkesi'nde bir değişiklik nedeniyle etki alanı reverification sonraki yenileme zaman 1 Mart 2018 önce verilen tüm uygulama hizmeti sertifikaları gerektirir. E-postanızı kontrol edin ve uygulama hizmeti sertifikanın otomatik yenilenmesini devam etmek için bu tek seferlik etki alanı doğrulamayı tamamlayın. 

## <a name="more-resources"></a>Diğer kaynaklar

* [HTTPS zorlama](app-service-web-tutorial-custom-ssl.md#enforce-https)
* [TLS 1.1/1.2 zorla](app-service-web-tutorial-custom-ssl.md#enforce-tls-1112)
* [Azure App Service'deki uygulama kodunuzda bir SSL sertifikası kullanın](app-service-web-ssl-cert-load.md)
* [Sık sorulan sorular: Uygulama hizmeti sertifikaları](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/24/faq-app-service-certificates/)