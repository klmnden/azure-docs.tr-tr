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
ms.date: 05/29/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: ead1892062912840c9931ae60d11c90975ad26ac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475113"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Azure App Service'teki uygulama kodunuzda SSL sertifikası kullanma

Bu nasıl yapılır kılavuzunda, genel veya özel sertifikalarının uygulama kodunuzda nasıl kullanılacağını gösterir. Kullanım örneği uygulamanızın sertifika doğrulaması gerektiren bir dış hizmete erişimi olduğunu örneğidir.

Sertifikaları kullanarak kodunuzda bu yaklaşım SSL yararlanır App Service, uygulamanız olması için gerekli işlevselliği **temel** katmanı veya üzeri. Alternatif olarak, [uygulama deponuza sertifika dosyasını dahil](#load-certificate-from-file), ancak özel sertifikaları için önerilen bir yöntem değildir.

App Service, SSL sertifikalarını yönetmenize olanak tanır, sertifikaları ve uygulama kodunuzu ayrı olarak korumak ve hassas verilerinizi koruyun.

## <a name="upload-a-private-certificate"></a>Özel bir sertifika yükleyin

Özel bir sertifikayı karşıya yüklemeden önce emin [tüm gereksinimlerini karşılayan](app-service-web-tutorial-custom-ssl.md#prepare-a-private-certificate)dışında sunucu kimlik doğrulaması için yapılandırılmış olması gerekmez.

Yüklemeye hazır olduğunuzda, aşağıdaki komutu çalıştırarak <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config ssl upload --name <app-name> --resource-group <resource-group-name> --certificate-file <path-to-PFX-file> --certificate-password <PFX-password> --query thumbprint
```

Sertifika parmak izini kopyalayın ve bkz [sertifika erişilebilir hale](#make-the-certificate-accessible).

## <a name="upload-a-public-certificate"></a>Bir ortak sertifika karşıya yükleme

Ortak Sertifikalar desteklenir *.cer* biçimi. Bir ortak sertifika karşıya yüklemek için <a href="https://portal.azure.com" target="_blank">Azure portalında</a>ve uygulamanıza gidin.

Tıklayın **SSL ayarları** > **ortak sertifika (.cer)**  > **ortak sertifika karşıya** uygulamanızın sol gezinti bölmesinden.

İçinde **adı**, sertifika için bir ad yazın. İçinde **CER sertifika dosyası**, CER dosyanızı seçin.

**Karşıya Yükle**'ye tıklayın.

![Ortak sertifikayı karşıya yükleme](./media/app-service-web-ssl-cert-load/private-cert-upload.png)

Sertifika karşıya yüklendikten sonra sertifika parmak izini kopyalayın ve bkz [sertifika erişilebilir hale](#make-the-certificate-accessible).

## <a name="import-an-app-service-certificate"></a>Bir App Service sertifikasını içeri aktarın

Bkz: [satın alma ve Azure App Service için SSL sertifikası yapılandırma](web-sites-purchase-ssl-web-site.md).

Sertifikayı içeri aktarıldıktan sonra sertifika parmak izini kopyalayın ve bkz [sertifika erişilebilir hale](#make-the-certificate-accessible).

## <a name="make-the-certificate-accessible"></a>Sertifika erişilebilir olun

Karşıya yüklenen veya içeri aktarılan bir sertifika uygulama kodunuzda kullanmak için parmak izi ile erişilebilir olun `WEBSITE_LOAD_CERTIFICATES` uygulama ayarı, aşağıdaki komutu çalıştırarak <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Tüm sertifikaları erişilebilir hale getirmek için değerine `*`.

> [!NOTE]
> Bu ayar belirtilen sertifikaları yerleştirir [geçerli User\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) ancak çoğu fiyatlandırma katmanları için mağazası **yalıtılmış** Katmanı (yani uygulama çalışan bir [App Service ortamı](environment/intro.md)), sertifikaları yerleştirir [yerel Machine\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) depolayın.
>

![Uygulama ayarı yapılandırın](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

İşlemi tamamladıktan sonra **Kaydet**’e tıklayın.

Yapılandırılan sertifikaları kodunuz tarafından kullanılmaya hazır.

## <a name="load-the-certificate-in-code"></a>Kodda sertifikasını yükleme

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
## <a name="load-certificate-from-file"></a>Dosyadan Sertifika Yükleme

Bir sertifika dosyası uygulama dizininize yüklemek ihtiyacınız varsa uygulamayı kullanarak yüklemek daha iyi [FTPS](deploy-ftp.md) yerine [Git](deploy-local-git.md), örneğin. Kaynak denetimi dışında özel bir sertifika gibi hassas verileri tutmanız gerekir.

.NET kodunuzu doğrudan Dosya yükleniyor olsa da, kitaplığı yine de geçerli kullanıcı profili yüklenip yüklenmediğini doğrular. Geçerli kullanıcı profilini yüklemek için `WEBSITE_LOAD_USER_PROFILE` uygulama ayarı aşağıdaki komutla <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
```

Bu ayar sonra aşağıdaki C# örnek adı verilen bir sertifika yüklediğinde `mycert.pfx` gelen `certs` uygulamanızın deponun dizin.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```
