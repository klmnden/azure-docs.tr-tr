---
title: Azure Web Apps’e var olan bir özel SSL sertifikası bağlama | Microsoft Docs
description: Azure App Service’te web uygulamanıza, mobil uygulama arka ucuna veya API uygulamasına özel bir SSL sertifikası bağlamayı öğrenin.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: ''
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 11/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 7c14b241155e10f0bb325b50819e2277622e4dff
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="tutorial-bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a>Öğretici: Azure Web Apps’e var olan bir özel SSL sertifikası bağlama

Azure Web Apps, yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğreticide, güvenilir bir sertifika yetkilisinden satın aldığınız özel bir SSL sertifikasını [Azure Web Apps](app-service-web-overview.md)’e nasıl bağlayacağınız gösterilmektedir. İşiniz bittiğinde, web uygulamanıza DNS etki alanınızın HTTPS uç noktasından erişebilirsiniz.

![Özel SSL sertifikası ile web uygulaması](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanını yükseltme
> * Özel SSL sertifikanızı App Service’e bağlama
> * Uygulamanız için HTTPS zorlama
> * Betiklerle SSL sertifikası bağlamayı otomatikleştirme

> [!NOTE]
> Özel bir SSL sertifikası almanız gerekirse, doğrudan Azure portalından bir tane edinerek web uygulamanıza bağlayabilirsiniz. [App Service Sertifikaları öğreticisini](web-sites-purchase-ssl-web-site.md) takip edin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Özel bir DNS adını web uygulamanıza eşleme](app-service-web-tutorial-custom-domain.md)
- Güvenilir sertifika yetkilisinden SSL sertifikası alma
- SSL sertifika isteğini imzalamak için kullandığınız özel anahtara sahip olma

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>SSL sertifikanıza yönelik gereksinimler

Bir sertifikayı App Service’te kullanabilmek için sertifikanın aşağıdaki tüm gereksinimleri karşılaması gerekir:

* Güvenilir bir sertifika yetkilisi tarafından imzalanması
* Parola korumalı bir PFX dosyası olarak dışarı aktarılması
* En az 2048 bit uzunluğunda özel anahtar içermesi
* Sertifika zincirindeki tüm ara sertifikaları içermesi

> [!NOTE]
> **Eliptik Eğri Şifrelemesi (ECC) sertifikaları**, App Service ile birlikte çalışabilir ancak bu makalenin konusu değildir. ECC sertifikaları oluşturmaya ilişkin tam adımlar için sertifika yetkilinizle birlikte çalışın.

## <a name="prepare-your-web-app"></a>Web uygulamanızı hazırlama

Özel bir SSL sertifikasını web uygulamanıza bağlamak için [App Service planınız](https://azure.microsoft.com/pricing/details/app-service/) **Temel**, **Standart** veya **Premium** katmanında olmalıdır. Bu adımda, web uygulamanızın desteklenen bir fiyatlandırma katmanında olduğundan emin olacaksınız.

### <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure portalı](https://portal.azure.com) açın.

### <a name="navigate-to-your-web-app"></a>Web uygulamanıza gidin

Sol menüden **Uygulama Hizmetleri**'ne ve ardından web uygulamanızın adına tıklayın.

![Web uygulaması seçme](./media/app-service-web-tutorial-custom-ssl/select-app.png)

Web uygulamanızın yönetim sayfasına geldiniz.  

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

Web uygulaması sayfasının sol gezinti bölmesinde **Ayarlar** bölümüne kaydırın ve **Ölçeği artır (App Service planı)** öğesini seçin.

![Ölçeği artır menüsü](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Web uygulamanızın **Ücretsiz** veya **Paylaşılan** katmanında olmadığından emin olun. Web uygulamanızın geçerli katmanı koyu mavi bir kutuyla vurgulanır.

![Fiyatlandırma katmanını denetleyin](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

**Ücretsiz** veya **Paylaşılan** katmanında özel SSL desteklenmez. Ölçeği artırmanız gerekirse sonraki bölümde verilen adımları izleyin. Aksi takdirde, **Fiyatlandırma katmanınızı seçin** sayfasını kapatıp [SSL sertifikanızı karşıya yükleme ve bağlama](#upload) bölümüne atlayın.

### <a name="scale-up-your-app-service-plan"></a>App Service planınızın ölçeğini artırma

**Temel**, **Standart** veya **Premium** katmanlarından birini seçin.

**Seç**'e tıklayın.

![Fiyatlandırma katmanı seçme](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

Aşağıdaki bildirimi gördüğünüzde, ölçeklendirme işlemi tamamlanmıştır.

![Ölçek artırma bildirimi](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>SSL sertifikanızı bağlama

SSL sertifikanızı web uygulamanıza yüklemeye hazırsınız.

### <a name="merge-intermediate-certificates"></a>Ara sertifikaları birleştirme

Sertifika yetkiliniz size sertifika zincirinde birden çok sertifika verirse, sertifikaları sırayla birleştirmeniz gerekir. 

Bunu yapmak için, aldığınız her sertifikayı bir metin düzenleyicisinde açın. 

Birleştirilmiş sertifika için _mergedcertificate.crt_ adlı bir dosya oluşturun. Bir metin düzenleyicisinde her bir sertifikanın içeriğini bu dosyaya kopyalayın. Sertifikalarınızın sırası, sertifikanızla başlayıp kök sertifika ile sona ererek sertifika zincirindeki sırayla aynı olmalıdır. Aşağıdaki örneğe benzer şekilde görünür:

```
-----BEGIN CERTIFICATE-----
<your entire Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a>Sertifikayı PFX dosyasına aktarma

Birleştirilmiş SSL sertifikanızı sertifika isteğinizi oluşturmak için kullanılan özel anahtarla dışarı aktarın.

Sertifika isteğinizi OpenSSL kullanarak oluşturduysanız bir özel anahtar dosyası oluşturduğunuz anlamına gelir. Sertifikanızı PFX dosyasına aktarmak için aşağıdaki komutu çalıştırın. _&lt;private-key-file>_ ve _&lt;merged-certificate-file>_ yer tutucularını özel anahtarınızın ve birleştirdiğiniz sertifika dosyasının yollarıyla değiştirin.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Sorulduğunda bir dışarı aktarma parolası tanımlayın. Bu parolayı daha sonra SSL sertifikanızı App Service’e yüklerken kullanacaksınız.

Sertifika isteğinizi oluşturmak için IIS veya _Certreq.exe_ kullandıysanız, sertifikayı yerel makinenize yükleyin ve sonra [sertifikayı PFX’e aktarın](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>SSL sertifikanızı karşıya yükleme

SSL sertifikanızı karşıya yüklemek için web uygulamanızın sol gezinti bölmesindeki **SSL sertifikaları** öğesine tıklayın.

**Sertifikayı Karşıya Yükle**’ye tıklayın. 

**PFX Sertifika Dosyası**’nda PFX dosyanızı seçin. **Sertifika parolası** alanına PFX dosyasını dışa aktardığınızda oluşturduğunuz parolayı yazın.

**Karşıya Yükle**'ye tıklayın.

![Sertifikayı karşıya yükleme](./media/app-service-web-tutorial-custom-ssl/upload-certificate-private1.png)

App Service sertifikanızı karşıya yüklemeyi tamamladığında sertifikanız **SSL sertifikaları** sayfasında görünür.

![Sertifika karşıya yüklendi](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>SSL sertifikanızı bağlama

**SSL bağlamaları** bölümünde **Bağlama ekle**’ye tıklayın.

**SSL Bağlaması Ekleyin** sayfasında açılır menüleri kullanarak güvenliği sağlanacak etki alanı adını ve kullanılacak sertifikayı seçin.

> [!NOTE]
> Sertifikanızı karşıya yüklediğiniz halde **Ana bilgisayar adı** açılır listesinde etki alanı adlarını göremiyorsanız, tarayıcı sayfasını yenilemeyi deneyin.
>
>

**SSL Türü** menüsünde **[Sunucu Adı Belirtme (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** veya IP tabanlı SSL seçeneklerinden hangisini kullanacağınızı belirleyin.

- **SNI tabanlı SSL** - Birden fazla SNI tabanlı SSL bağlaması eklenebilir. Bu seçenek, aynı IP adresi üzerinde birden fazla SSL sertifikası ile birden fazla etki alanının güvenliğini sağlamaya olanak tanır. Çoğu modern tarayıcı (Internet Explorer, Chrome, Firefox ve Opera dahil) SNI’yi destekler (daha kapsamlı tarayıcı desteği bilgilerini [Sunucu Adı Belirtimi](http://wikipedia.org/wiki/Server_Name_Indication) bölümünde bulabilirsiniz).
- **IP tabanlı SSL** - Yalnızca bir adet IP tabanlı SSL bağlaması eklenebilir. Bu seçenek yalnızca bir SSL sertifikası ile ayrılmış bir genel IP adresinin güvenliğini sağlamaya olanak tanır. Birden fazla etki alanının güvenliğini sağlamak için tümünün güvenliğini aynı SSL sertifikası ile sağlamanız gerekir. SSL bağlaması için geleneksel seçenek budur.

**Bağlama Ekle**’ye tıklayın.

![SSL sertifikası bağlama](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

App Service sertifikanızı karşıya yüklemeyi tamamladığında sertifikanız **SSL bağlamaları** bölümlerinde görünür.

![Web uygulamasına bağlı sertifika](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>IP SSL için A kaydını yeniden eşleme

Web uygulamanızda IP tabanlı SSL kullanmıyorsanız [HTTPS’yi özel etki alanınız için test etme](#test) bölümüne atlayın.

Varsayılan olarak, web uygulamanız paylaşılan bir genel IP adresini kullanır. Bir sertifikayı IP tabanlı SSL ile bağladığınızda App Service, web uygulamanız için yeni ve ayrılmış bir IP adresi oluşturur.

Web uygulamanız için bir A kaydını eşlediyseniz, etki alanı kaydınızı bu yeni ve ayrılmış IP adresi ile güncelleştirin.

Web uygulamanızın **Özel etki alanı** sayfası yeni ve ayrılmış IP adresi ile güncelleştirilir. [Bu IP adresini kopyalayın](app-service-web-tutorial-custom-domain.md#info), ardından bu yeni IP adresine [A kaydını yeniden eşleyin](app-service-web-tutorial-custom-domain.md#map-an-a-record).

<a name="test"></a>

## <a name="test-https"></a>HTTPS’yi test etme

Şimdi yapılması gereken tek şey, HTTPS’nin özel etki alanınız için çalıştığından emin olmaktır. Çeşitli tarayıcılarda `https://<your.custom.domain>` sayfasına göz atarak web uygulamanıza hizmet verip vermediğini görün.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Web uygulamanız sertifika doğrulama hataları veriyorsa, büyük olasılıkla otomatik olarak imzalanan bir sertifika kullanıyorsunuz.
>
> Böyle bir durum söz konusu değilse, sertifikanızı PFX dosyasına aktardığınızda ara sertifikaları dışarıda bırakmış olabilirsiniz.

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>HTTPS zorlama

Varsayılan olarak, herkes HTTP kullanarak web uygulamanıza erişmeye devam edebilir. Tüm HTTPS isteklerini HTTP bağlantı noktasına yeniden yönlendirebilirsiniz.

Web uygulaması sayfanızın sol gezinti bölmesinde **Özel etki alanları**'nı seçin. Ardından **Yalnızca HTTPS** menüsünde **Açık**’ı seçin.

![HTTPS zorlama](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

İşlem tamamlandığında, uygulamanıza işaret eden HTTP URL'lerinden herhangi birine gidin. Örnek:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

Web uygulamalarınıza yönelik SSL bağlamalarını, [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview) kullanarak betiklerle otomatik hale getirebilirsiniz.

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut, dışarı aktarılan bir PFX dosyasını karşıya yükler ve parmak izini alır.

```bash
thumbprint=$(az webapp config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

Aşağıdaki komut, önceki komutta alınan parmak izini kullanarak SNI tabanlı bir SSL bağlaması ekler.

```bash
az webapp config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki komut, dışarı aktarılan bir PFX dosyasını karşıya yükler ve SNI tabanlı bir SSL bağlaması ekler.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```
## <a name="public-certificates-optional"></a>Ortak sertifikalar (isteğe bağlı)
Web uygulamanıza [ortak sertifikalar](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer/) yükleyebilirsiniz. Uygulamalara yönelik ortak sertifikaları App Service Ortamlarında da kullanabilirsiniz. Sertifikayı LocalMachine sertifika deposuna kaydetmeniz gerekirse, App Service Ortamında bir web uygulaması kullanmanız gerekir. Daha fazla bilgi için bkz. [Web Uygulamanızda ortak sertifikaları yapılandırma](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer).

![Ortak Sertifikayı Karşıya Yükleme](./media/app-service-web-tutorial-custom-ssl/upload-certificate-public1.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanını yükseltme
> * Özel SSL sertifikanızı App Service’e bağlama
> * Uygulamanız için HTTPS zorlama
> * Betiklerle SSL sertifikası bağlamayı otomatikleştirme

Azure Content Delivery Network kullanımı hakkında bilgi almak için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure App Service’e İçerik Teslim Ağı (CDN) Ekleme](app-service-web-tutorial-content-delivery-network.md)

Daha fazla bilgi için bkz. [Azure App Service'teki uygulama kodunuzda SSL sertifikası kullanma](app-service-web-ssl-cert-load.md).