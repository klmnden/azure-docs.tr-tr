---
title: Karşıya yükleme ve SSL sertifikası - Azure App Service'ı bağlama | Microsoft Docs
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
ms.date: 06/06/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 32e6311a8796e708119f3e1df813b6ebb2ed0673
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743000"
---
# <a name="tutorial-upload-and-bind-ssl-certificates-to-azure-app-service"></a>Öğretici: Karşıya yükleme ve Azure App Service için SSL sertifikası bağlama

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğreticide, App Service'te özel etki alanı bir güvenilir sertifika yetkilisinden satın aldığınız bir sertifika ile güvenli hale getirmeyi gösterir. Ayrıca, herhangi bir özel yükleme işlemini gösterir ve uygulama ihtiyaçlarınızı ortak sertifikalar. İşlemi tamamladığınızda, uygulamanıza özel DNS etki alanınızın HTTPS uç noktasında erişmek mümkün olacaktır.

![Özel SSL sertifikası ile web uygulaması](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanını yükseltme
> * Bir sertifika ile özel bir etki alanını güvenli hale getirme
> * Özel bir sertifika yükleyin
> * Bir ortak sertifika karşıya yükleme
> * Sertifikaları yenileme
> * HTTPS zorlama
> * TLS 1.1/1.2 zorlama
> * TLS yönetimini betiklerle otomatikleştirme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [App Service uygulamanıza özel bir DNS adı eşleme](app-service-web-tutorial-custom-domain.md) (özel bir etki alanını güvenli hale getirme ise)
- Bir güvenilir sertifika yetkilisinden bir sertifika edinme
- (Özel sertifikaları için) sertifika isteğini imzalamak için kullanılan özel bir anahtara sahip

<a name="requirements"></a>

## <a name="prepare-a-private-certificate"></a>Özel bir sertifika hazırlama

Bir etki alanını güvenli hale getirmek için sertifikanın aşağıdaki gereksinimleri karşılaması gerekir:

* Sunucu kimlik doğrulaması için yapılandırılmış
* Güvenilir bir sertifika yetkilisi tarafından imzalanması
* Parola korumalı bir PFX dosyası olarak dışarı aktarılması
* En az 2048 bit uzunluğunda özel anahtar içermesi
* Sertifika zincirindeki tüm ara sertifikaları içermesi

> [!TIP]
> Özel bir SSL sertifikası almanız gerekiyorsa, Azure Portalı'nda doğrudan almak ve uygulamanıza içeri aktarabilirsiniz. [App Service Sertifikaları öğreticisini](web-sites-purchase-ssl-web-site.md) takip edin.

> [!NOTE]
> **Eliptik Eğri Şifrelemesi (ECC) sertifikaları**, App Service ile birlikte çalışabilir ancak bu makalenin konusu değildir. ECC sertifikaları oluşturmaya ilişkin tam adımlar için sertifika yetkilinizle birlikte çalışın.

Sertifika, sertifika sağlayıcınızdan edindikten sonra App Service için hazır hale getirmek için bu bölümdeki adımları izleyin.

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

App Service sertifika artık hazır karşıya yükleme, demektir.

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<a name="upload"></a>

## <a name="secure-a-custom-domain"></a>Özel bir etki alanını güvenli hale getirme

> [!TIP]
> Özel bir SSL sertifikası almanız gerekiyorsa, Azure Portalı'nda doğrudan almak ve uygulamanıza bağlayın. [App Service Sertifikaları öğreticisini](web-sites-purchase-ssl-web-site.md) takip edin.

Güvenli hale getirmek için bir [özel etki alanı](app-service-web-tutorial-custom-domain.md) bir üçüncü taraf sertifika ile karşıya yüklediğiniz [özel sertifika hazır](#prepare-a-private-certificate) ve ardından özel etki bağlama, ancak App Service, işlemi basitleştirir. Aşağıdaki adımları uygulayın:

Tıklayın **özel etki alanları** uygulamanızın sol gezinti bölmesinde, ardından **bağlama Ekle** istediğiniz güvenliğini sağlamak için etki alanı için. Görmüyorsanız **bağlama Ekle** bir etki alanı için sonra zaten güvenlidir ve olmalıdır bir **güvenli** SSL durumu.

![Etki bağlama Ekle](./media/app-service-web-tutorial-custom-ssl/secure-domain-launch.png)

**Sertifikayı Karşıya Yükle**’ye tıklayın.

**PFX Sertifika Dosyası**’nda PFX dosyanızı seçin. **Sertifika parolası** alanına PFX dosyasını dışa aktardığınızda oluşturduğunuz parolayı yazın.

**Karşıya Yükle**'ye tıklayın.

![Etki alanı için sertifikayı karşıya yükleyin](./media/app-service-web-tutorial-custom-ssl/secure-domain-upload.png)

Sertifikanızı karşıya yüklemek ve SSL bağlamaları iletişim başlatmak, Azure için bekleyin.

SSL bağlamaları iletişim kutusunda, karşıya yüklediğiniz sertifikaya ve SSL türü seçin ve ardından **bağlaması Ekle**.

> [!NOTE]
> Aşağıdaki SSL türleri desteklenir:
>
> - **[SNI tabanlı SSL](https://en.wikipedia.org/wiki/Server_Name_Indication)**  -birden fazla SNI tabanlı SSL bağlaması eklenebilir. Bu seçenek, aynı IP adresi üzerinde birden fazla SSL sertifikası ile birden fazla etki alanının güvenliğini sağlamaya olanak tanır. Çoğu modern tarayıcı (Internet Explorer, Chrome, Firefox ve Opera dahil) SNI’yi destekler (daha kapsamlı tarayıcı desteği bilgilerini [Sunucu Adı Belirtimi](https://wikipedia.org/wiki/Server_Name_Indication) bölümünde bulabilirsiniz).
> - **IP tabanlı SSL** - Yalnızca bir adet IP tabanlı SSL bağlaması eklenebilir. Bu seçenek yalnızca bir SSL sertifikası ile ayrılmış bir genel IP adresinin güvenliğini sağlamaya olanak tanır. Birden fazla etki alanının güvenliğini sağlamak için tümünün güvenliğini aynı SSL sertifikası ile sağlamanız gerekir. SSL bağlaması için geleneksel seçenek budur.

![Etki alanı için SSL bağlama](./media/app-service-web-tutorial-custom-ssl/secure-domain-bind.png)

Etki alanının SSL durumu için artık değiştirilmelidir **güvenli**.

![Etki alanı güvenliği](./media/app-service-web-tutorial-custom-ssl/secure-domain-finished.png)

> [!NOTE]
> A **güvenli** içinde durum **özel etki alanları** sertifikayı otomatik olarak imzalanan veya süresi dolduğunda, örneğin, ayrıca bir sertifika ile güvenli olduğundan, ancak değil App Service denetleyin anlamına gelir neden tarayıcılar bir hata veya uyarı gösterir.

## <a name="remap-a-record-for-ip-ssl"></a>IP SSL için A kaydını yeniden eşleme

Uygulamanızda IP tabanlı SSL kullanmıyorsanız, atlamak [özel etki alanınız için Test HTTPS](#test).

Varsayılan olarak, uygulamanızı paylaşılan bir genel IP adresini kullanır. Bir sertifikayı IP tabanlı SSL ile bağladığınızda App Service uygulamanız için yeni ve ayrılmış bir IP adresi oluşturur.

Uygulamanız için bir A kaydını eşlediyseniz, etki alanı kaydınızı bu yeni ve ayrılmış IP adresi ile güncelleştirin.

Uygulamanızın **özel etki alanı** sayfası, yeni ve ayrılmış IP adresi ile güncelleştirilir. [Bu IP adresini kopyalayın](app-service-web-tutorial-custom-domain.md#info), ardından bu yeni IP adresine [A kaydını yeniden eşleyin](app-service-web-tutorial-custom-domain.md#map-an-a-record).

<a name="test"></a>

## <a name="test-https"></a>HTTPS’yi test etme

Şimdi yapılması gereken tek şey, HTTPS’nin özel etki alanınız için çalıştığından emin olmaktır. Çeşitli tarayıcılarda göz atın `https://<your.custom.domain>` uygulamanızı vermediğini görün.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Uygulamanızı sağlıyorsa, sertifika doğrulama hataları, büyük olasılıkla otomatik olarak imzalanan bir sertifika kullanıyorsunuz.
>
> Böyle bir durum söz konusu değilse, sertifikanızı PFX dosyasına aktardığınızda ara sertifikaları dışarıda bırakmış olabilirsiniz.

## <a name="renew-certificates"></a>Sertifikaları yenileme

Bir bağlamayı sildiğinizde, bu bağlama IP tabanlı olsa bile gelen IP adresiniz değişebilir. Bu, zaten IP tabanlı bağlamada yer alan bir sertifikayı yenilerken özellikle önemlidir. Uygulamanızın IP adresinin değişmesini önlemek için şu adımları sırasıyla izleyin:

1. Yeni sertifikayı karşıya yükleyin.
2. Eskisini silmeden yeni sertifikayı istediğiniz özel etki alanına bağlayın. Bu eylem, eskisini kaldırmak yerine bağlamayı değiştirir.
3. Eski sertifikayı silin. 

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>HTTPS zorlama

Varsayılan olarak, herkes HTTP kullanarak uygulamanıza erişmeye devam edebilirsiniz. Tüm HTTPS isteklerini HTTP bağlantı noktasına yeniden yönlendirebilirsiniz.

Uygulaması sayfanızın sol gezinti bölmesinde seçin **SSL ayarları**. Ardından **Yalnızca HTTPS** menüsünde **Açık**’ı seçin.

![HTTPS zorlama](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

İşlem tamamlandığında, uygulamanıza işaret eden HTTP URL'lerinden herhangi birine gidin. Örneğin:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="enforce-tls-versions"></a>TLS sürümlerini zorlama

Uygulamanız varsayılan olarak [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.2 sürümüne izin verir. Bu, [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard) gibi endüstri standartlarınca önerilen TLS düzeyidir. Farklı TLS sürümlerini zorlamak için aşağıdaki adımları uygulayın:

Uygulaması sayfanızın sol gezinti bölmesinde seçin **SSL ayarları**. Ardından **TLS sürümü**’nde istediğiniz en düşük TLS sürümünü seçin. Bu ayar yalnızca gelen çağrıları denetler. 

![TLS 1.1 veya 1.2’yi zorlama](./media/app-service-web-tutorial-custom-ssl/enforce-tls1.2.png)

İşlem tamamlandığında, uygulamanız daha düşük TLS sürümleriyle tüm bağlantıları reddeder.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

Kullanarak uygulamanızın, betiklerle SSL bağlamaları otomatikleştirebilirsiniz [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut, dışarı aktarılan bir PFX dosyasını karşıya yükler ve parmak izini alır.

```azurecli-interactive
thumbprint=$(az webapp config ssl upload \
    --name <app-name> \
    --resource-group <resource-group-name> \
    --certificate-file <path-to-PFX-file> \
    --certificate-password <PFX-password> \
    --query thumbprint \
    --output tsv)
```

Aşağıdaki komut, önceki komutta alınan parmak izini kullanarak SNI tabanlı bir SSL bağlaması ekler.

```azurecli-interactive
az webapp config ssl bind \
    --name <app-name> \
    --resource-group <resource-group-name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

Aşağıdaki komut TLS için minimum 1.2 sürümünü zorunlu tutar.

```azurecli-interactive
az webapp config set \
    --name <app-name> \
    --resource-group <resource-group-name>
    --min-tls-version 1.2
```

### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki komut, dışarı aktarılan bir PFX dosyasını karşıya yükler ve SNI tabanlı bir SSL bağlaması ekler.

```powershell
New-AzWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="use-certificates-in-your-code"></a>Kodunuzda sertifikaları kullan

Uzak kaynaklara bağlanmak uygulamanız gereken ve uzak kaynak sertifika doğrulaması gerektiren, uygulamanıza genel veya özel sertifikaları karşıya yükleyebilirsiniz. Bu sertifikalar uygulamanızda herhangi bir özel etki bağlama gerek yoktur. Daha fazla bilgi için bkz. [Azure App Service'teki uygulama kodunuzda SSL sertifikası kullanma](app-service-web-ssl-cert-load.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanını yükseltme
> * Özel sertifikanızı App Service'e bağlama
> * Sertifikaları yenileme
> * HTTPS zorlama
> * TLS 1.1/1.2 zorlama
> * TLS yönetimini betiklerle otomatikleştirme

Azure Content Delivery Network kullanımı hakkında bilgi almak için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure App Service’e İçerik Teslim Ağı (CDN) Ekleme](../cdn/cdn-add-to-web-app.md)

Daha fazla bilgi için bkz. [Azure App Service'teki uygulama kodunuzda SSL sertifikası kullanma](app-service-web-ssl-cert-load.md).
