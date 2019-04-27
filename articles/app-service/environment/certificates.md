---
title: Sertifikalar ve Azure App Service ortamı-
description: Bir ASE üzerinde sertifikalar ilgili çeşitli konularda açıklanır
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 9e21a7e4-2436-4e81-bb05-4a6ba70eeaf7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: bcb0c806d916b9dff4461cad829a1d75e8df7cf6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60766276"
---
# <a name="certificates-and-the-app-service-environment"></a>Sertifikalar ve App Service ortamı 

App Service engelleniyorsa, Azure App Service, Azure sanal ağ içinde çalıştırılan bir dağıtımıdır. Bir Internet erişilebilir bir uygulama uç noktası veya ağınızda bir uygulama uç noktası ile dağıtılabilir. İnternet erişilebilir uç noktası ile ASE dağıtırsanız, bu dağıtımın dış ASE olarak adlandırılır. Ağınızda bir uç nokta ile ASE dağıtırsanız, bu dağıtım, ILB ASE olarak adlandırılır. ILB ASE'de hakkında daha fazla bilgi [oluşturma ve kullanma ILB ASE](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase) belge.

ASE tek kiracılı bir sistemdir. Tek kiracılı olduğu için bazı özellikler mevcuttur çok kiracılı App Service içinde kullanılabilir olmayan bir ASE ile kullanılabilir. 

## <a name="ilb-ase-certificates"></a>ILB ASE sertifikaları 

Ardından dış ASE kullanıyorsanız, uygulamalarınızı [appname] ulaşıldı. [asename]. p.azurewebsites.net. Varsayılan olarak tüm ase, ILB ase bile, bu biçime uygun sertifikalar ile oluşturuldu. ILB ASE sahip olduğunuzda, uygulamaları, ILB ASE oluştururken belirttiğiniz etki alanı adına göre ulaşıldı. SSL desteği uygulamaların sertifikalarını yüklemek gerekir. İç sertifika yetkililerini kullanarak harici bir verenden sertifika satın alma veya otomatik olarak imzalanan bir sertifika kullanarak geçerli bir SSL sertifikası alın. 

ILB ASE ile sertifikaları yapılandırma için iki seçenek vardır.  ILB ASE için varsayılan bir joker karakter sertifika ayarlayın veya ASE içindeki tek tek web apps'te sertifikaları ayarlayın.  Yaptığınız seçime bağımsız olarak, aşağıdaki sertifika özniteliklerinin doğru şekilde yapılandırılması gerekir:

- **Konu:** Bu öznitelik ayarlanmalıdır *. [your-root-domain-here] joker nitelikli bir ILB ASE sertifikası için. Uygulamanız için sertifika oluşturuyorsanız, [appname] olmalıdır. [your-root-domain-here]
- **Konu diğer adı:** Bu öznitelik her ikisini de içermelidir. *. [your-root-domain-here] ve *.scm. [your-kök-domain-here] joker ILB ASE sertifikası. Uygulamanız için sertifika oluşturuyorsanız, [appname] olmalıdır. [your-root-domain-here] ve [appname] .scm. [your-root-domain-here].

Üçüncü bir değişken bir joker karakter başvurusu kullanarak yerine sertifikanın SAN'daki tüm bilgilerinizi tek tek uygulama adları içeren bir ILB ASE sertifika oluşturabilirsiniz. Bu yöntem sorun Önden ASE'de faydalandığını uygulamaları adlarını bilmeniz gereken veya ILB ASE sertifikayı güncelleştirmek için ihtiyacınız olan.

### <a name="upload-certificate-to-ilb-ase"></a>ILB ASE için sertifikayı karşıya yükleyin 

Portalda bir ILB ASE oluşturulduktan sonra sertifika ILB ASE için ayarlamanız gerekir. Sertifika ayarlanana kadar ASE sertifika ayarlanmadı bir başlık gösterilir.  

Yüklediğiniz sertifikanın bir .pfx dosyası olmalıdır. Sertifika karşıya yüklendikten sonra ASE sertifika ayarlamak için bir ölçeklendirme işlemi gerçekleştirir. 

ASE oluşturma ve sertifika Portalı'nda veya hatta tek bir şablonda bir eylem olarak karşıya olamaz. Ayrı bir eylem açıklandığı gibi bir şablon kullanarak sertifikayı karşıya yükleyebilirsiniz [şablondan ASE oluşturma](./create-from-template.md) belge.  

Hızlı bir şekilde test etmek için bir kendinden imzalı bir sertifika oluşturmak istiyorsanız, aşağıdaki bit PowerShell kullanabilirsiniz:

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     


## <a name="application-certificates"></a>Uygulama sertifikaları 

Bir ASE'de barındırılan uygulamalar, çok kiracılı App Service içinde kullanılabilir olan uygulama merkezli sertifika özelliklerini kullanabilirsiniz. Bu özellikler şunlardır:  

- SNI sertifikaları 
- Dış ASE ile yalnızca desteklenen IP tabanlı SSL.  ILB ASE, IP tabanlı SSL desteklemez.
- Barındırılan KeyVault sertifikaları 

App Service SSL öğreticide karşıya yükleme ve bu sertifikaları yönetmek için yönergeleri kullanılabilir https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl.  Web uygulamanıza atamış olduğunuz bir özel etki alanı adı ile eşleşmesi için sertifikaları yalnızca yapılandırıyorsanız, bu yönergeleri yeterli olacaktır. Bir ILB ASE web uygulamasının varsayılan etki alanı adıyla sertifika yüklüyorsanız, scm sitesine daha önce belirtildiği gibi sertifika SAN'da belirtin. 

## <a name="tls-settings"></a>TLS ayarları 

Bir uygulama düzeyinde TLS ayarı yapılandırabilirsiniz.  

## <a name="private-client-certificate"></a>Özel bir istemci sertifikası 

Genel bir kullanım örneği, istemci bir istemci-sunucu modeli olarak uygulamanızı yapılandırmaktır. Sunucunuz özel bir CA sertifikası ile güvenli, uygulamanız için istemci sertifikasını karşıya yüklemek gerekir.  Aşağıdaki yönergeler, uygulamanızın üzerinde çalıştığı çalışan truststore sertifikaları yükler. Sertifika için bir uygulama yüklerseniz, bunu diğer uygulamalarla aynı App Service planında sertifikayı yeniden karşıya yüklemeye gerek kalmadan kullanabilirsiniz.

Uygulamanıza ASE'nizi sertifikayı karşıya yüklemek için:

1. Oluşturmak bir *.cer* sertifikanız için dosya. 
2. Azure portalındaki sertifikayla gereken uygulamaya gidin
3. Uygulamasında SSL ayarlarına gidin. Sertifikayı Karşıya Yükle'ye tıklayın. Ortak seçin. Yerel makine seçin. Bir ad sağlayın. Gözat ve Seç, *.cer* dosya. Yükleme'yi seçin. 
4. Parmak izini kopyalayın.
5. Uygulama Ayarları'na gidin. Bir uygulama ayarı WEBSITE_LOAD_ROOT_CERTIFICATES parmak iziyle değeri olarak oluşturun. Birden çok sertifikası varsa, bunları virgül ve gibi hiçbir boşluk tarafından ayrılmış ayarında koyabilirsiniz 

    84EC242A4EC7957817B8E48913E50953552DAFA6,6A5C65DC9247F762FE17BF8D4906E04FE6B31819

Sertifika aynı app service planında söz konusu ayarın yapılandırılmış uygulama olarak tüm uygulamalar tarafından kullanılabilir. Farklı bir App Service planındaki uygulamaları için kullanılabilir olması gerekiyorsa, bu App Service planında bir uygulamada uygulama ayarlama işlemini tekrarlamanız gerekir. Sertifika ayarlandığını kontrol etmek için Kudu konsoluna gidin ve bu komut dir cert: \localmachine\root PowerShell hata ayıklama konsolunda vermek. 

Test gerçekleştirmek için kendinden imzalı bir sertifika oluşturabilir ve oluşturma bir *.cer* aşağıdaki PowerShell ile dosya: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.cer"
    export-certificate -Cert $certThumbprint -FilePath $fileName -Type CERT

