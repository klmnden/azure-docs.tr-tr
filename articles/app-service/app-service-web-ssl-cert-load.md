---
title: Uygulama kodunda - Azure App Service istemci SSL sertifikasını kullan | Microsoft Docs
description: Onları gerektiren uzak kaynaklara bağlanmak için istemci sertifikaları kullanmayı öğrenin.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 763aadc50a8760b4265dbfc21e9278f909b68433
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60851128"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Azure App Service'teki uygulama kodunuzda SSL sertifikası kullanma

Bu nasıl yapılır kılavuzunda, karşıya yüklediğiniz veya App Service uygulamanıza içeri aktarılan uygulama kodunuzda SSL sertifikaları birini kullanmayı gösterir. Kullanım örneği uygulamanızın sertifika doğrulaması gerektiren bir dış hizmete erişimi olduğunu örneğidir. 

Kodunuzda SSL sertifikalarının kullanılması için bu yaklaşım SSL yararlanır App Service, uygulamanız olması için gerekli işlevselliği **temel** katmanı veya üzeri. Sertifika dosyası uygulama dizininizde dahil etmek ve doğrudan yüklemek için bir alternatifidir (bkz [alternatif: yük sertifika dosyası olarak](#file)). Ancak, bu seçenek, uygulama kodu veya Geliştirici sertifikası özel anahtarı Gizle izin vermez. Uygulama kodunuz bir açık kaynak depoda ise, ayrıca, bir sertifika deposunda özel anahtarı olan tutarak bir seçenek değil.

App Service, SSL sertifikalarını yönetmenize olanak tanır, sertifikaları ve uygulama kodunuzu ayrı olarak korumak ve hassas verilerinizi koruyun.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda tamamlamak için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Özel bir DNS adını web uygulamanıza eşleme](app-service-web-tutorial-custom-domain.md)
- [Bir SSL sertifikasını karşıya yükle](app-service-web-tutorial-custom-ssl.md) veya [bir App Service sertifikasını içeri](web-sites-purchase-ssl-web-site.md) web uygulamanıza


## <a name="load-your-certificates"></a>Sertifikalarınızı yüklemek

Yüklenen veya App Service içeri aktarılan bir sertifikayı kullanmak için önce uygulama kodunuz için erişilebilir hale. Bunu `WEBSITE_LOAD_CERTIFICATES` uygulama ayarı.

İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, web uygulaması sayfanızın açın.

Sol gezinti bölmesinde tıklayın **SSL sertifikaları**.

![Sertifika karşıya yüklendi](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

Tüm karşıya yüklenen ve içeri aktarılan SSL sertifikalarınızı bu web uygulaması için kendi parmak izleri ile burada gösterilir. Kullanmak istediğiniz sertifikanın parmak izini kopyalayın.

Sol gezinti bölmesinde tıklayın **uygulama ayarları**.

Çağrılan ayarlama uygulama ekleme `WEBSITE_LOAD_CERTIFICATES` ve sertifikanın parmak izi değerini ayarlayın. Birden çok sertifika erişilebilir hale getirmek için virgülle ayrılmış bir parmak izi değerleri kullanın. Tüm sertifikaları erişilebilir hale getirmek için değerine `*`. Bu sertifikayı yerleştireceğiniz Al Not `CurrentUser\My` depolayın.

![Uygulama ayarı yapılandırın](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

İşlemi tamamladıktan sonra **Kaydet**’e tıklayın.

Yapılandırılmış sertifika artık kodunuz tarafından kullanılmak üzere hazırdır.

## <a name="use-certificate-in-c-code"></a>C# kodunda sertifika kullan

Sertifikanızı erişilebilir olduğunda, C# kodu tarafından sertifika parmak izini erişim. Aşağıdaki kod bir sertifika parmak izi yükler `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

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
## <a name="alternative-load-certificate-as-a-file"></a>Alternatif: sertifika dosyası olarak yükleme

Bu bölümde gösterilmiştir nasıl yapılır ve uygulama dizininizde bir sertifika dosyası yükleyin. 

Aşağıdaki C# örneği adı verilen bir sertifika yüklediğinde `mycert.pfx` gelen `certs` uygulamanızın deponun dizin.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```

