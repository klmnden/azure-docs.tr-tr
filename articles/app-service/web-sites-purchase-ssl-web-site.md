---
title: Satın alma ve Azure App Service için SSL sertifikası yapılandırma | Microsoft Docs
description: Bir App Service sertifikası satın alma ve App Service uygulamanızı bağlama hakkında bilgi edinin
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
ms.openlocfilehash: 85d0c91a0b1cdf5703b394d6d232ab9cee72ee0c
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627153"
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Azure App Service için SSL Sertifikası Satın Alma ve Yapılandırma

Bu öğretici, web uygulamanız için SSL sertifikası satın alarak güvenliğinin nasıl sağlanacağını gösterir,  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, güvenli bir şekilde de depolamak [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis), ve Bu, özel bir etki alanıyla ilişkilendirmeyi.

## <a name="step-1---sign-in-to-azure"></a>Adım 1 - oturum azure'a

Adresinden Azure portalında oturum açın http://portal.azure.com

## <a name="step-2---place-an-ssl-certificate-order"></a>2. adım - bir SSL sertifikası sipariş verin

Yeni bir SSL sertifikası sipariş yerleştirebilirsiniz [App Service sertifikası](https://portal.azure.com/#create/Microsoft.SSL) içinde **Azure portalında**.

![Sertifika oluşturma](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Kullanımı kolay bir girin **adı** için SSL sertifikası ve girin **etki alanı adı**

> [!NOTE]
> Bu adım satın alma işleminin en kritik parçalarından biridir. Bu sertifika ile korumak istediğiniz doğru konak adı (özel etki alanı) girdiğinizden emin olun. **DO NOT** WWW ana bilgisayar adıyla önüne ekleyin. 
>

Seçin, **abonelik**, **kaynak grubu**, ve **sertifika SKU'su**

> [!TIP]
> App Service sertifikaları, herhangi bir Azure veya Azure Hizmetleri için kullanılabilir ve uygulama hizmetleri için sınırlı değildir. Bunu yapmak için onu istediğiniz yere kullanabileceğiniz bir App Service sertifikasının yerel PFX kopyasını oluşturmak gerekir. Daha fazla bilgi için okuma [bir App Service sertifikasının yerel PFX kopyasını oluşturma](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>3. adım: Azure anahtar Kasası'nda sertifika Store

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) şifreleme anahtarlarını ve gizli bulut uygulamaları ve Hizmetleri tarafından kullanılan korunmasına yardımcı olan bir Azure hizmetidir.
>

SSL sertifikası satın alma işlemini tamamladıktan sonra açmak gereken [App Service sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfası.

![içinde KV depolamak hazır görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

Sertifika durumu **"Verme bekleniyor"** olarak bu sertifikayı kullanarak başlamadan önce tamamlamanız gereken birkaç adım vardır.

Tıklayın **sertifika Yapılandırması** sertifika özellikleri sayfasında ve tıklayarak **1. adım: Store** bu sertifikanın Azure Key Vault'ta depolamak için.

Gelen **Key Vault durumu** sayfasında **Key Vault deposu** bu sertifikanın depolanacağı mevcut bir Key Vault seçmek için **veya yeni Key Vault Oluştur** yeni anahtar kasası oluşturmak için aynı abonelik ve kaynak grubu içinde.

> [!NOTE]
> Azure Key Vault bu sertifikayı depolamak için en düşük ücreti vardır.
> Daha fazla bilgi için  **[Azure anahtar kasası fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Bu sertifikada depolamak için Key Vault deposu seçtikten sonra **Store** seçeneği başarı göstermelidir.

![Görüntü deposu başarı KV Ekle](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>4. adım - etki alanı sahipliğini doğrulayın

Aynı **sertifika Yapılandırması** adım 3'te kullanılan sayfasında **2. adım: doğrulama**.

Tercih edilen etki alanı doğrulama yöntemi seçin. 

App Service sertifikaları tarafından desteklenen etki alanı doğrulaması dört tür vardır: App Service, etki alanı ve el ile doğrulama. Bu doğrulama türleri daha ayrıntılı olarak açıklanmıştır [bölümü Gelişmiş](#advanced).

> [!NOTE]
> **App Service doğrulaması** doğrulamak istediğiniz etki alanını aynı Abonelikteki bir App Service uygulaması zaten eşlenmiş en uygun seçenektir. App Service uygulaması zaten etki alanı sahipliğini doğruladı olgu avantajlarından yararlanır.
>

Tıklayarak **doğrulama** bu adımı tamamlamak için düğme.

![etki alanı doğrulaması görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

Tıkladıktan sonra **doğrulama**, kullanın **Yenile** kadar düğmesini **doğrulama** seçeneği başarı göstermelidir.

![ekleme görüntüsü KV başarılı olun](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a>Adım 5 - App Service uygulamasına atamak sertifika

> [!NOTE]
> Bu bölümdeki adımları gerçekleştirmeden önce bir özel etki alanı adı uygulamanızla ilişkili gerekir. Daha fazla bilgi için  **[bir web uygulaması için özel etki alanı adı yapılandırma.](app-service-web-tutorial-custom-domain.md)**
>

İçinde  **[Azure portalında](https://portal.azure.com/)**, tıklayın **App Service** sayfasının sol taraftaki seçeneği.

Bu sertifikayı atamak istediğiniz uygulamanın adına tıklayın.

İçinde **ayarları**, tıklayın **SSL ayarları**.

Tıklayın **alma App Service sertifikası** ve yeni satın aldığınız sertifikayı seçin.

![Sertifika İçeri Aktar görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

İçinde **ssl bağlamaları** tıklatın bölümünde **bağlamalar**, açılır menüleri kullanarak SSL ve sertifika ile kullanmak üzere güvenli hale getirmek için etki alanı adını kullanarak. Ayrıca kullanılıp kullanılmayacağını seçebilirsiniz **[sunucu adı belirtme (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** veya IP tabanlı SSL.

![SSL bağlamaları görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Tıklayın **bağlaması Ekle** değişiklikleri kaydedin ve SSL'i etkinleştirin.

> [!NOTE]
> Seçtiyseniz **IP tabanlı SSL** ve bir A kaydı kullanarak özel etki alanınızı yapılandırılmış, aşağıdaki ek adımları gerçekleştirmeniz gerekir. Bunlar daha ayrıntılı olarak açıklanmıştır [bölümü Gelişmiş](#Advanced).

Bu noktada, kullanıp uygulamanızın ziyaret etmek erişebileceğinizi `HTTPS://` yerine `HTTP://` sertifika doğru şekilde yapılandırıldığını doğrulayın.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Adım 6 - yönetim görevleri

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a>Gelişmiş

### <a name="verifying-domain-ownership"></a>Etki alanı sahipliğini doğrulama

App service sertifikaları tarafından desteklenen etki alanı doğrulaması diğer iki tür vardır: etki alanı doğrulaması ve el ile doğrulama.

#### <a name="domain-verification"></a>Etki Alanı Doğrulama

Bu seçeneği yalnızca [satın aldığınız Azure App Service etki alanı.](custom-dns-web-site-buydomains-web-app.md). Azure, otomatik olarak sizin için doğrulama TXT kaydı ekler ve işlemini tamamlar.

#### <a name="manual-verification"></a>El ile Doğrulama

> [!IMPORTANT]
> HTML Web sayfası Doğrulama (yalnızca standart sertifika SKU ile çalışır)
>

1. Adlı bir HTML dosyası oluşturun **"starfield.html"**

1. Bu dosya adı tam etki alanı doğrulama belirteci olarak içerik olmalıdır. (Etki alanı doğrulama durumu sayfasından belirteç kopyalayabilirsiniz)

1. Etki alanınızı barındıran web sunucusunun köküne bu dosyayı karşıya yükleyin `/.well-known/pki-validation/starfield.html`

1. Tıklayın **Yenile** doğrulama tamamlandıktan sonra sertifika durumunu güncelleştirmek için. Bu doğrulamanın tamamlanması birkaç dakika sürebilir.

> [!TIP]
> Bir terminal kullanarak doğrulama `curl -G http://<domain>/.well-known/pki-validation/starfield.html` yanıt içermelidir `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>DNS TXT kaydını doğrulama

1. DNS Yöneticisi'ni kullanarak bir TXT kaydı üzerinde oluşturmanız `@` alt etki alanı doğrulama belirteci için eşit.
1. Tıklayın **"Yenile"** doğrulama tamamlandıktan sonra sertifika durumunu güncelleştirmek için.

> [!TIP]
> Bir TXT kaydını oluşturmak ihtiyacınız `@.<domain>` değerle `<verification-token>`.

### <a name="assign-certificate-to-app-service-app"></a>Sertifika App Service uygulamasına atama

Seçtiyseniz **IP tabanlı SSL** ve bir A kaydı kullanarak özel etki alanınızı yapılandırılmış, aşağıdaki ek adımları gerçekleştirmeniz gerekir:

Yapılandırmasını tamamladıktan sonra IP temelli SSL bağlaması, uygulamanıza bir ayrılmış IP adresi atanır. Bu IP adresini bulabilirsiniz **özel etki alanı** üzerinde doğru altında Uygulama Ayarları sayfasında **ana bilgisayar adları** bölümü. Olarak listelenen **dış IP adresi**

![IP SSL görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Bu IP adresi, etki alanınız için A kaydı yapılandırmak için daha önce kullanılan sanal IP adresi farklıdır. Kullanılacak şekilde yapılandırılmışsa SNI tabanlı SSL ve SSL kullanacak şekilde yapılandırılmamış, bu giriş için adres listelenir.

Etki alanı adı kayıt tarafından sağlanan araçları kullanarak, önceki adımdaki IP adresine işaret edecek şekilde özel etki alanı adınız için A kaydı değiştirin.

## <a name="rekey-and-sync-the-certificate"></a>Yeniden anahtarla ve Eşitle sertifika

Sertifikanızın yeniden oluşturmak gerekiyorsa seçin **yeniden anahtarlama ve eşitleme** seçeneğini **sertifika özellikleri** sayfası.

Tıklayın **yeniden anahtarlama** işlemini başlatmak için düğme. Bu işlemin tamamlanması 1-10 dakika sürebilir.

![yeniden anahtarlama SSL görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Sertifikanız yeniden anahtarlama için izine sahip sertifika yetkilisinden verilen yeni bir sertifika yapar.

## <a name="renew-the-certificate"></a>Sertifikayı Yenile

Dilediğiniz zaman, sertifikanın otomatik yenilenmesini üzerinde etkinleştirmek için tıklayın **otomatik yenileme ayarları** Sertifika Yönetim sayfasında. Seçin **üzerinde** tıklatıp **Kaydet**. Sertifika otomatik yenileme açık varsa 90 gün süresi dolmadan önce otomatik olarak yenileme başlayabilirsiniz.

![](./media/app-service-web-purchase-ssl-web-site/auto-renew.png)

Sertifikayı bunun yerine el ile yenilemek için tıklayın **el ile yenileme** yerine. Sertifikanızın süresi dolmadan önce 60 gün el ile yenilemek için istekte bulunabilir.

> [!NOTE]
> Yenilenen sertifikanın otomatik olarak uygulamanıza el ile yenilenmesi ya da otomatik olarak yenilendiğinde bağlı değil. Uygulamanıza bağlamak için bkz: [sertifikaları yenileme](./app-service-web-tutorial-custom-ssl.md#renew-certificates). 

<a name="notrenewed"></a>
## <a name="why-is-my-certificate-not-auto-renewed"></a>Neden Sertifikamı otomatik-yenilenmez?

SSL sertifikanızı otomatik yenileme için yapılandırılmış, ancak otomatik olarak yenilenmediği, bekleyen bir etki alanı doğrulama olabilir. Şunlara dikkat edin: 

- App Service sertifikaları oluşturur, bir GoDaddy etki alanı doğrulaması iki yılda bir kez gerektirir. Etki alanı yöneticisi etki alanını doğrulamak için her üç yılda bir e-posta alır. E-postayı denetlemek veya etki alanınızı doğrulayın, App Service sertifikasını otomatik olarak yenilenir engeller. 
- (Otomatik yenilemeyi için sertifika etkin olsa bile) GoDaddy ilkesinde değişiklik nedeniyle, bir sonraki yenileme sırasında etki alanının reverification 1 Mart 2018'e kadar verilen tüm App Service sertifikaları gerektirir. E-postanızı kontrol edin ve App Service sertifikası otomatik yenilemeyi devam etmek için bu tek seferlik bir etki alanı doğrulaması tamamlayın. 

## <a name="more-resources"></a>Diğer kaynaklar

* [HTTPS zorlama](app-service-web-tutorial-custom-ssl.md#enforce-https)
* [TLS 1.1/1.2 zorlama](app-service-web-tutorial-custom-ssl.md#enforce-tls-1112)
* [Azure App Service'teki uygulama kodunuzda SSL sertifikası kullanma](app-service-web-ssl-cert-load.md)
* [SSS: App Service sertifikaları](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/24/faq-app-service-certificates/)
