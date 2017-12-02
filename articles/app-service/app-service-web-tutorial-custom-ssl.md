---
title: "Azure Web uygulamaları için var olan özel bir SSL sertifikası bağlama | Microsoft Docs"
description: "Web uygulaması, mobil uygulama arka ucu veya Azure App Service'teki API uygulamasına özel bir SSL sertifikası bağlanılacağını öğrenin."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 11/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f69bc731b2858c338d7f7b4d347e7107a0f4eeed
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a>Azure Web uygulamaları için var olan özel bir SSL sertifikası bağlama

Azure Web Apps düzeyde ölçeklenebilir, otomatik olarak düzeltme eki uygulama web barındırma hizmeti sağlar. Bu öğretici için güvenilir bir sertifika yetkilisinden satın aldığınız özel bir SSL sertifikası bağlanacağı gösterilmektedir [Azure Web Apps](app-service-web-overview.md). İşiniz bittiğinde, web uygulamanızı DNS etki alanınız HTTPS uç noktası erişmek mümkün olur.

![Özel SSL sertifikası ile Web uygulaması](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanı yükseltme
> * Özel SSL sertifikanızı uygulama hizmetine bağlama
> * Uygulamanız için HTTPS zorla
> * SSL sertifikası bağlaması kodlarıyla otomatikleştirme

> [!NOTE]
> Özel bir SSL sertifikası almanız gerekiyorsa, Azure portalında doğrudan edinebileceğinizi ve web uygulamanıza bağlayın. İzleyin [uygulama hizmeti sertifikaları Öğreticisi](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Bir App Service uygulaması oluşturma](/azure/app-service/)
- [Harita web uygulamanız için özel bir DNS adı](app-service-web-tutorial-custom-domain.md)
- Güvenilen sertifika yetkilisinden bir SSL sertifikası alın
- SSL sertifika isteğini imzalamak için kullanılan özel anahtara sahip

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>SSL sertifika için gereksinimler

App Service'te bir sertifika kullanmak üzere sertifika aşağıdaki gereksinimleri karşılamalıdır:

* Bir güvenilir sertifika yetkilisi tarafından imzalanmış
* Parola korumalı bir PFX dosyası olarak dışa
* Özel anahtarı en az 2048 bit uzun içerir
* Sertifika zincirindeki tüm ara sertifikaların içerir

> [!NOTE]
> **Eliptik Eğri Şifrelemesi (ECC) sertifikaları** App Service ile çalışabilir, ancak bu makalenin ele alınmamaktadır. Sertifika yetkilisi ile ECC sertifikaları oluşturmak için uygulanacak adımlar üzerinde çalışır.

## <a name="prepare-your-web-app"></a>Web uygulamanızı hazırlama

Özel bir SSL sertifikası, web uygulamanızın bağlamak için [uygulama hizmeti planı](https://azure.microsoft.com/pricing/details/app-service/) olmalıdır **temel**, **standart**, veya **Premium** katmanı. Bu adımda, web uygulamanızı desteklenen fiyatlandırma katmanı olduğundan emin emin olun.

### <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure portalı](https://portal.azure.com) açın.

### <a name="navigate-to-your-web-app"></a>Web uygulamanıza gidin

Sol menüden **uygulama hizmetleri**ve ardından, web uygulamanızın adına tıklayın.

![Web uygulaması seçin](./media/app-service-web-tutorial-custom-ssl/select-app.png)

Web uygulaması Yönetimi sayfasındaki indiniz.  

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanı denetleyin

Web uygulaması sayfanızı sol gezinti bölmesinde kaydırın **ayarları** bölümünde ve seçin **(uygulama hizmeti planı) ölçeklendirme**.

![Büyütme menüsü](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Web uygulamanızı içinde olmadığından emin olmak için kontrol edin **serbest** veya **paylaşılan** katmanı. Web uygulamanızın geçerli katmanı Koyu mavi bir kutu vurgulanır.

![Fiyatlandırma katmanı denetleyin](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

Özel SSL desteklenmez **serbest** veya **paylaşılan** katmanı. Ölçeği gerekiyorsa, sonraki bölümde yer alan adımları izleyin. Aksi takdirde, kapatmak **fiyatlandırma katmanınızı seçin** sayfasında ve geçin [karşıya yükleme ve SSL sertifikanız bağlama](#upload).

### <a name="scale-up-your-app-service-plan"></a>Uygulama hizmeti planınızı ölçeklendirin

Aşağıdakilerden birini seçin **temel**, **standart**, veya **Premium** katmanları.

**Seç**'e tıklayın.

![Fiyatlandırma katmanı seçin](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

Aşağıdaki bildirim görürseniz, bir ölçeklendirme işlemi tamamlanır.

![Bildirimi ölçeklendirme](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>SSL sertifikası bağlama

Web uygulamanız için SSL sertifikanızı karşıya yüklemeye hazırsınız demektir.

### <a name="merge-intermediate-certificates"></a>Ara Sertifika birleştirme

Sertifika yetkilisi sertifika zincirinde birden çok sertifika veriyorsa, sipariş sertifikaları birleştirmeniz gerekir. 

Bunu yapmak için bir metin düzenleyicisinde alınan her sertifika açın. 

Adlı birleştirilmiş sertifikası için bir dosya oluşturmak _mergedcertificate.crt_. Bir metin düzenleyicisinde içeriği her sertifika, bu dosyaya kopyalayın. Sertifikalarınızı sırasını sertifikanızı ile başlayan ve kök sertifika ile biten sertifika zinciri sırada izlemelidir. Aşağıdaki gibi görünür:

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

### <a name="export-certificate-to-pfx"></a>PFX için sertifika verme

Birleştirilmiş SSL sertifikanızı sertifika isteğinizi ile oluşturulan özel anahtarla dışarı aktarın.

Ardından OpenSSL kullanarak sertifika isteğinizi oluşturursa, özel bir anahtar dosyası oluşturdunuz. PFX için sertifika vermek için aşağıdaki komutu çalıştırın. Yer tutucuları değiştirmek  _&lt;özel anahtar dosyası >_ ve  _&lt;birleştirilmiş-sertifika-dosyası >_ özel anahtarınızı ve birleştirilmiş sertifika dosyanızı yollara ile.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

İstendiğinde, bir dışarı aktarma parolası tanımlayın. Daha sonra App Service'e SSL sertifikanızı karşıya yüklenirken bu parolayı kullanacaksınız.

IIS kullandıysanız veya _Certreq.exe_ , sertifika isteği oluşturmak için sertifika yerel makinenize yükleyin ve ardından [PFX için sertifika verme](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>SSL sertifikasını karşıya yükle

SSL sertifikanızı karşıya yüklemek için tıklayın **SSL sertifikalarını** , web uygulamanızın sol gezinti bölmesinde.

Tıklatın **karşıya sertifika**. 

İçinde **PFX sertifika dosyanızı**, PFX dosyasını seçin. İçinde **sertifika parolası**, PFX dosyasını dışarı aktardığınızda, oluşturduğunuz parolayı yazın.

**Karşıya Yükle**'ye tıklayın.

![Sertifikayı karşıya yükleme](./media/app-service-web-tutorial-custom-ssl/upload-certificate-private1.png)

Uygulama hizmeti sertifikanızı karşıya yükleme tamamlandığında, görünür **SSL sertifikalarını** sayfası.

![Karşıya sertifika](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>SSL sertifikası bağlama

İçinde **SSL bağlamaları** 'yi tıklatın **eklemek bağlama**.

İçinde **SSL bağlaması Ekle** sayfasında, bırakmalar güvenliğini sağlamak için etki alanı adı seçin ve kullanılacak sertifikayı kullanın.

> [!NOTE]
> Sertifikanız karşıya yüklediğiniz halde etki alanı adları göremiyorsanız **ana bilgisayar adı** açılan listesinde, tarayıcı sayfayı yenilemeyi deneyin.
>
>

İçinde **SSL türü**, kullanıp kullanmayacağınızı seçin  **[sunucu adı göstergesi (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  ya da IP tabanlı SSL.

- **SNI tabanlı SSL** -birden çok SNI tabanlı SSL bağlamaları eklenebilir. Bu seçenek, aynı IP adresinde birden çok etki alanı güvenliğini sağlamak birden çok SSL sertifikası sağlar. (Internet Explorer, Chrome, Firefox ve Opera dahil) çoğu modern tarayıcılar SNI destekler (daha kapsamlı tarayıcı destek bilgilerini bulmak [sunucu adı göstergesi](http://wikipedia.org/wiki/Server_Name_Indication)).
- **IP tabanlı SSL** -yalnızca bir IP temelli SSL bağlama eklenebilir. Bu seçenek, ayrılmış bir ortak IP adresi güvenli hale getirmek yalnızca bir SSL sertifikası sağlar. Birden çok etki alanı güvenliğini sağlamak için bunların tümü aynı SSL sertifikası kullanarak güvenli hale getirmenize gerekir. SSL bağlaması için geleneksel seçenek budur.

Tıklatın **bağlama eklemek**.

![SSL sertifikası bağlama](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Uygulama hizmeti sertifikanızı karşıya yükleme tamamlandığında, görünür **SSL bağlamaları** bölümler.

![Web uygulaması'na bağlı sertifikası](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>IP SSL için bir kayıt yeniden eşleme

Web uygulamanızda IP tabanlı SSL kullanmıyorsanız, geçin [Test HTTPS özel etki alanınız için](#test).

Varsayılan olarak, web uygulamanızı paylaşılan bir ortak IP adresini kullanır. IP tabanlı SSL sertifikasıyla bağladığınızda, App Service web uygulamanız için yeni, ayrılmış bir IP adresi oluşturur.

Web uygulamanız için bir A kaydı eşledikten ise, etki alanı kayıt defteri bu yeni, özel IP adresi ile güncelleştirin.

Web uygulamanızın **özel etki alanı** sayfası, yeni, özel IP adresi ile güncelleştirilir. [Bu IP adresi Kopyala](app-service-web-tutorial-custom-domain.md#info), ardından [A kaydını yeniden eşleme](app-service-web-tutorial-custom-domain.md#map-an-a-record) bu yeni bir IP adresi için.

<a name="test"></a>

## <a name="test-https"></a>Test HTTPS

Tüm yapmak için sol sunulmuştur HTTPS için özel etki alanınızı çalıştığından emin olmak için. Çeşitli tarayıcılarda Gözat `https://<your.custom.domain>` web uygulamanıza hizmet etmediğini görmek için.

![Azure App portalında gezinme](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Web uygulamanız varsa doğrulama hatalarını sertifika, büyük olasılıkla otomatik olarak imzalanan bir sertifika kullanıyorsanız.
>
> Bu durumda değilse, PFX dosyası, sertifika verirken Ara sertifikalara sol.

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>HTTPS zorlama

Varsayılan olarak, herkesin web uygulamanızın HTTP kullanarak erişmeye devam edebilirsiniz. HTTPS bağlantı noktasına tüm HTTP isteklerini yeniden yönlendirebilirsiniz.

Sol gezinti bölmesinde, web uygulaması sayfasında seçin **özel etki alanları**. Ardından **yalnızca HTTPS**seçin **üzerinde**.

![HTTPS zorlama](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

İşlem tamamlandığında, uygulamanıza noktası HTTP URL'lerini birini gidin. Örneğin:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="automate-with-scripts"></a>Komut dosyalarıyla otomatikleştirme

Kullanarak, kodlarıyla web uygulamanız için SSL bağlamaları otomatikleştirebilirsiniz [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki komutu bir dışarı aktarılan PFX dosyasını yükler ve parmak izi alır.

```bash
thumbprint=$(az webapp config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

Aşağıdaki komut, parmak izini önceki komutu kullanarak bir SNI tabanlı SSL bağlama ekler.

```bash
az webapp config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki komutu, dışa aktarılan bir PFX dosyası yükler ve SNI tabanlı SSL bağlaması ekler.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```
## <a name="public-certificates-optional"></a>Ortak sertifikaları (isteğe bağlı)
Karşıya yüklediğiniz [ortak sertifikaları](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer/) web uygulamanız için. Ortak Sertifikalar App Service ortamları uygulamalar için de kullanabilirsiniz. Sertifikayı LocalMachine sertifika deposunda depolamak ihtiyacınız varsa, bir web uygulaması uygulama hizmeti ortamı kullanmanız gerekebilir. Daha fazla bilgi için bkz: [Web uygulamanız için ortak sertifikaları yapılandırma](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer).

![Ortak sertifikasını karşıya yükle](./media/app-service-web-tutorial-custom-ssl/upload-certificate-public1.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanı yükseltme
> * Özel SSL sertifikanızı uygulama hizmetine bağlama
> * Uygulamanız için HTTPS zorla
> * SSL sertifikası bağlaması kodlarıyla otomatikleştirme

Azure içerik teslim ağı kullanmayı öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [İçerik teslim ağı (CDN) için bir Azure uygulama hizmeti Ekle](app-service-web-tutorial-content-delivery-network.md)

Daha fazla bilgi için bkz: [Azure App Service'deki uygulama kodunuzda bir SSL sertifikası kullanmak](app-service-web-ssl-cert-load.md).