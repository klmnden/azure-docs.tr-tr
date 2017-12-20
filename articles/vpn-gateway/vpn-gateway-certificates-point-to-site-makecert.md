---
title: "Oluştur ve noktası Site için sertifikalar verme: MakeCert: Azure | Microsoft Docs"
description: "Bu makale, otomatik olarak imzalanan sertifika oluşturmak, ortak anahtarını dışarı aktarmak ve MakeCert kullanarak istemci sertifikalarını oluşturmak için adımlar içerir."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: 2beacc461370f268e54e1eedcb32939f7c606b14
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Oluştur ve noktadan siteye bağlantıları MakeCert kullanarak için sertifikaları verme

Noktadan siteye bağlantılar, kimlik doğrulaması için sertifikaları kullanır. Bu makalede bir otomatik olarak imzalanan sertifika oluşturmak ve MakeCert kullanarak istemci sertifikalarını oluşturmak nasıl gösterir. Kök sertifikalarını yüklemek nasıl gibi noktadan siteye yapılandırma adımlarını arıyorsanız ' yapılandırma noktası siteye ' makaleleri birini aşağıdaki listeden seçin:

> [!div class="op_single_selector"]
> * [Otomatik olarak imzalanan sertifikalar - PowerShell oluşturma](vpn-gateway-certificates-point-to-site.md)
> * [Otomatik olarak imzalanan sertifikalar - MakeCert oluşturma](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Noktası-Site - Resource Manager - Azure portalında yapılandırın](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Noktadan siteye - Resource Manager - PowerShell yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Noktası-Site - Classic - Azure portalında yapılandırın](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Kullanmanızı öneririz sırada [Windows 10 PowerShell adımları](vpn-gateway-certificates-point-to-site.md) isteğe bağlı bir yöntem olarak bu MakeCert yönergeleri sunuyoruz sertifikalarınızı oluşturmak için. Her iki yöntemi kullanarak oluşturduğunuz sertifika yüklenebilir [herhangi bir desteklenen istemci işletim sistemi](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq). Ancak, MakeCert aşağıdaki sınırlamalara sahiptir:

* MakeCert kullanım dışıdır. Başka bir deyişle, bu aracı herhangi bir noktada kaldırılamadı. MakeCert artık kullanılabilir olduğunda zaten MakeCert kullanılarak oluşturulan herhangi bir sertifika etkilenmeyecek. MakeCert yalnızca değil doğrulama mekanizması olarak sertifikalarını oluşturmak için kullanılır.

## <a name="rootcert"></a>Otomatik olarak imzalanan sertifika oluştur

Aşağıdaki adımlar MakeCert kullanarak otomatik olarak imzalanan bir sertifika oluşturmak nasıl gösterir. Bu adımları dağıtım modeli özel değildir. Bunlar, Resource Manager ve klasik için geçerli olur.

1. İndirme ve yükleme [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx).
2. Yükleme tamamlandıktan sonra bu yol altındaki makecert.exe yardımcı programını genellikle bulabilirsiniz: ' C:\Program Files (x86) \Windows Kits\10\bin\<arch >'. Ancak başka bir konuma yüklendiğini mümkündür. Yönetici olarak bir komut istemi açın ve MakeCert yardımcı programı konumuna gidin. Aşağıdaki örnekte, doğru konumunu ayarlama kullanabilirsiniz:

  ```cmd
  cd C:\Program Files (x86)\Windows Kits\10\bin\x64
  ```
3. Oluşturun ve bilgisayarınızdaki kişisel sertifika deposunda bir sertifika yükleyin. Aşağıdaki örnek, karşılık gelen oluşturur *.cer* Azure'a P2S yapılandırırken karşıya dosya. 'P2SRootCert' ve 'P2SRootCert.cer' sertifika için kullanmak istediğiniz adla değiştirin. Sertifika, 'sertifikalarda - geçerli User\Personal\Certificates' bulunur.

  ```cmd
  makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha256 -len 2048 -ss My
  ```

## <a name="cer"></a>Ortak anahtarı (.cer) aktarın

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Exported.cer dosyasını Azure'a yüklenmelidir. Yönergeler için bkz: [noktadan siteye bağlantı yapılandırma](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). Bir ek güvenilen kök sertifika eklemek için bkz: [Bu bölümde](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) makalenin.

### <a name="export-the-self-signed-certificate-and-private-key-to-store-it-optional"></a>Kendinden imzalı bir sertifika ve (isteğe bağlı) depolamak için özel anahtarı dışarı aktar

Otomatik olarak imzalanan kök sertifikasını dışarı aktarma ve güvenli bir şekilde depolamak isteyebilirsiniz. Varsa olmadan, yapabilir daha sonra başka bir bilgisayara yükleyin ve daha fazla istemci sertifikalarını oluşturmak veya başka bir .cer dosyasına dışarı aktarma. Bir .pfx otomatik olarak imzalanan kök sertifikasını dışarı aktarmak için kök sertifikasını seçin ve açıklandığı gibi aynı adımları kullanın [bir istemci sertifikası verme](#clientexport).

## <a name="create-and-install-client-certificates"></a>Oluşturun ve istemci sertifikalarını yükleyin

Doğrudan istemci bilgisayarda otomatik olarak imzalanan sertifika yüklemeyin. Bir istemci sertifikasını otomatik olarak imzalanan sertifika oluşturmak gerekir. Sonra dışarı aktarın ve istemci bilgisayara istemci sertifikası yükleyin. Aşağıdaki adımlar dağıtım modeli özel değildir. Bunlar, Resource Manager ve klasik için geçerli olur.

### <a name="clientcert"></a>İstemci sertifikası oluşturma

Noktadan Siteye bağlantı kullanarak bir sanal ağa bağlanan her istemci bilgisayarda bir istemci sertifikası yüklü olmalıdır. Ardından dışa otomatik olarak imzalanan kök sertifikasından bir istemci sertifikasını oluşturmak ve istemci sertifikasını yükleyin. İstemci sertifikası yüklü değilse, kimlik doğrulaması başarısız olur. 

Aşağıdaki adımlarda, otomatik olarak imzalanan kök sertifikasından bir istemci sertifikası oluşturma aracılığıyla yol. Birden çok istemci sertifikaları aynı kök sertifikası oluşturabilir. Aşağıdaki adımları kullanarak istemci sertifikalarını oluşturmak, istemci sertifikasının, sertifikayı oluşturmak için kullanılan bilgisayarda otomatik olarak yüklenir. Bir istemci sertifikası başka bir istemci bilgisayara yüklemek istiyorsanız, sertifikayı dışa aktarabilirsiniz.
 
1. Otomatik olarak imzalanan sertifika oluşturmak için kullanılan aynı bilgisayarda yönetici olarak bir komut istemi açın.
2. Değiştirme ve bir istemci sertifikasını oluşturmak için örnek çalıştırın.
  * Değişiklik *"P2SRootCert"* istemci sertifikası oluşturma otomatik olarak imzalanan kök adı. Ne olursa olsun kök sertifikanın adı kullandığınızdan emin olun ' CN =' değer olan otomatik olarak imzalanan kök oluşturduğunuzda belirttiğiniz.
  * Değişiklik *P2SChildCert* olması için bir istemci sertifikası oluşturmak için istediğiniz adı.

  Değişiklik yapmadan aşağıdaki örneği çalıştırırsanız, kişisel sertifika deposunda kök sertifikasının P2SRootCert oluşturulan P2SChildcert adlı bir istemci sertifikası sonucudur.

  ```cmd
  makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha256
  ```

### <a name="clientexport"></a>Bir istemci sertifikasını dışarı aktarma

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Dışarı aktarılan istemci sertifikası yükleme

Bir istemci sertifikası yüklemek için bkz: [bir istemci sertifikası yüklemek](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Sonraki adımlar

Noktası Site yapılandırmanızı ile devam edin. 

* İçin **Resource Manager** dağıtım modeli adımları bkz [yapılandırma P2S yerel Azure sertifika kimlik doğrulaması kullanarak](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* İçin **Klasik** dağıtım modeli adımları bkz [bir sanal ağ (Klasik) bir noktadan siteye VPN bağlantısı yapılandırma](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
