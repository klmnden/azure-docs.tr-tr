---
title: "Azure App Service'deki uygulama kodunuzda karşıya yüklenen bir SSL sertifikası kullanın | Microsoft Docs"
description: 
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: cephalin
ms.openlocfilehash: 6800bf766deb2044d400f92dbe370fa15bdd5f00
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Azure App Service'deki uygulama kodunuzda bir SSL sertifikası kullanın

Nasıl yapılır bu kılavuz karşıya veya uygulama hizmeti uygulamanızı uygulama kodunuzda alınan SSL sertifikalarını birini kullanmayı gösterir. Kullanım örneği uygulamanızı sertifika kimlik doğrulaması gerektiren bir dış hizmete erişen örneğidir. 

Kodunuzda SSL sertifikalarının kullanımı için bu yaklaşım SSL kullanır, uygulama olması gerekiyor. uygulama hizmeti işlevindeki **temel** katmanı veya üstü. Sertifika dosyası uygulama dizininize ve doğrudan yüklemek için alternatiftir (bkz [alternatif: yük sertifika dosyası olarak](#file)). Ancak, bu seçenek, uygulama kodu veya Geliştirici sertifikadaki özel anahtar Gizle izin vermez. Uygulama kodunuz bir açık kaynak havuzunda ise, ayrıca, bir sertifika deposunda özel anahtarı olan tutma bir seçenek değil.

Uygulama hizmeti SSL sertifikalarını yönetmenize izin verdiğinizde, sertifikaları ve uygulama kodunuz ayrı olarak korumak ve hassas verilerinizi koruyun.

## <a name="prerequisites"></a>Ön koşullar

Nasıl yapılır bu kılavuzu tamamlamak için:

- [Bir App Service uygulaması oluşturma](/azure/app-service/)
- [Harita web uygulamanız için özel bir DNS adı](app-service-web-tutorial-custom-domain.md)
- [Bir SSL sertifikası karşıya](app-service-web-tutorial-custom-ssl.md) veya [bir uygulama hizmeti sertifika alma](web-sites-purchase-ssl-web-site.md) web uygulamanız için


## <a name="load-your-certificates"></a>Sertifikalarınızı yükleme

Yüklenen veya uygulama hizmeti alınan bir sertifika kullanmak için önce uygulama kodunuzun erişebilmesini. Bunu ile `WEBSITE_LOAD_CERTIFICATES` uygulama ayarı.

İçinde <a href="https://portal.azure.com" target="_blank">Azure portal</a>, web app sayfanızı açın.

Sol gezinti bölmesinde tıklatın **SSL sertifikalarını**.

![Karşıya sertifika](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

Tüm içeri aktarılan ve karşıya SSL sertifikalarınızı bu web uygulamasının kendi parmak izleriyle burada gösterilir. Kullanmak istediğiniz sertifikanın parmak izini kopyalayın.

Sol gezinti bölmesinde tıklatın **uygulama ayarları**.

Denilen bir uygulama ekleyin `WEBSITE_LOAD_CERTIFICATES` ve sertifikanın parmak izi için değerini ayarlayın. Birden çok sertifika erişilebilir olması için parmak izi virgülle ayrılmış değerler kullanın. Tüm sertifikaları erişilebilir olması için değerine `*`. 

![Uygulama ayarını yapılandırın](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

Tamamlandığında tıklatarak **kaydetmek**.

Yapılandırılmış sertifika artık kodunuz tarafından kullanılmak üzere hazırdır.

## <a name="use-certificate-in-c-code"></a>C# kodunda sertifika kullan

Sertifikanızı erişilebilir olduğunda, C# kodunda sertifika parmak izi tarafından erişim. Aşağıdaki kod parmak izine sahip bir sertifika yükleyen `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser);
certStore.Open(OpenFlags.ReadOnly);
X509Certificate2Collection certCollection = certStore.Certificates.Find(
                            X509FindType.FindByThumbprint,
                            // Replace below with your certificate's thumbprint
                            "E661583E8FABEF4C0BEF694CBC41C28FB81CD870",
                            false);
// Get the first cert with the thumbprint
if (certCollection.Count > 0)
{
    X509Certificate2 cert = certCollection[0];
    // Use certificate
    Console.WriteLine(cert.FriendlyName);
}
certStore.Close();
...
```

<a name="file"></a>
## <a name="alternative-load-certificate-as-a-file"></a>Alternatif: bir dosya olarak sertifika yükleme

Bu bölümde gösterilmiştir nasıl ve uygulama dizininizde bir sertifika dosyası yükleyin. 

Aşağıdaki C# örnek adı verilen bir sertifika yükleyen `mycert.pfx` gelen `certs` uygulamanızın havuzun dizin.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```

