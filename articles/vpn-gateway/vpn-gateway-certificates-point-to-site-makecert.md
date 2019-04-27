---
title: 'Oluşturma ve noktadan siteye için sertifikalar verme: MakeCert : Azure | Microsoft Docs'
description: Otomatik olarak imzalanan kök sertifika oluşturma, ortak anahtarını dışarı aktarmak ve MakeCert kullanarak istemci sertifikalarını oluşturmak.
services: vpn-gateway
documentationcenter: na
author: WenJason
ms.service: vpn-gateway
ms.topic: article
origin.date: 09/05/2018
ms.date: 10/01/2018
ms.author: v-jay
ms.openlocfilehash: 973c0aa3bd187e963f15adbe34955d6bc9fa612d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60768115"
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Oluşturma ve MakeCert kullanarak noktadan siteye bağlantılar için sertifikaları dışarı aktarma

Noktadan siteye bağlantılar, kimlik doğrulaması için sertifikaları kullanır. Bu makalede bir otomatik olarak imzalanan kök sertifika oluşturma ve MakeCert kullanarak istemci sertifikalarını oluşturmak nasıl gösterir. Farklı bir sertifika yönergeler arıyorsanız bkz [sertifikalar - PowerShell](vpn-gateway-certificates-point-to-site.md) veya [sertifikalar - Linux](vpn-gateway-certificates-point-to-site-linux.md).

Kullanmanızı öneririz ancak [Windows 10 PowerShell adımları](vpn-gateway-certificates-point-to-site.md) isteğe bağlı bir yöntem olarak bu MakeCert yönergeleri sağlarız sertifikalarınızı oluşturmak için. Her iki yöntemi kullanarak oluşturduğunuz sertifika yüklenebilir [herhangi bir desteklenen istemci işletim sistemini](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq). Ancak, MakeCert aşağıdaki sınırlamalara sahiptir:

* MakeCert kullanım dışıdır. Başka bir deyişle, herhangi bir noktada bu aracı kaldırılamadı. MakeCert artık kullanılabilir olduğunda, MakeCert kullanarak zaten oluşturulmuş herhangi bir sertifika etkilenmez. MakeCert, yalnızca doğrulama mekanizması olarak değil, sertifikaları oluşturmak için kullanılır.

## <a name="rootcert"></a>Otomatik olarak imzalanan kök sertifika oluşturma

Aşağıdaki adımlar MakeCert kullanarak otomatik olarak imzalanan bir sertifika oluşturma işlemini gösterir. Bu adımlar, dağıtım modeli özel değildir. Bunlar, Resource Manager ve klasik için geçerli olur.

1. İndirme ve yükleme [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx).
2. Yüklemeden sonra bu yol altındaki makecert.exe yardımcı programını genellikle bulabilirsiniz: ' C:\Program dosyaları (x86) \Windows Kits\10\bin\<arch >'. Ancak başka bir konuma yüklendiğini mümkündür. Yönetici olarak bir komut istemi açın ve MakeCert yardımcı programının konumuna gidin. Aşağıdaki örnekte, doğru konumu ayarlama kullanabilirsiniz:

   ```cmd
   cd C:\Program Files (x86)\Windows Kits\10\bin\x64
   ```
3. Oluşturun ve bilgisayarınızın kişisel sertifika deposunda bir sertifika yükleyin. Aşağıdaki örnek, karşılık gelen oluşturur *.cer* P2S yapılandırırken Azure'a yükleme dosyası. 'P2SRootCert' ve 'p2srootcert.cer'olarak adlandırıp sertifika için kullanmak istediğiniz adla değiştirin. Sertifika, 'Sertifikalar - Geçerli kullanıcı\kişisel\sertifikalar' bulunur.

   ```cmd
   makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha256 -len 2048 -ss My
   ```

## <a name="cer"></a>Ortak anahtarı (.cer) dışarı aktarma

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Azure'a exported.cer dosya karşıya yüklenmelidir. Yönergeler için [noktadan siteye bağlantı yapılandırma](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). Bir güvenilir kök sertifika eklemek için bkz [Bu bölümde](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) makalenin.

### <a name="export-the-self-signed-certificate-and-private-key-to-store-it-optional"></a>Otomatik olarak imzalanan sertifika ve (isteğe bağlı) depolamak için özel anahtarı dışarı aktar

Otomatik olarak imzalanan kök sertifikasını dışarı aktarma ve güvenli bir şekilde depolamak isteyebilirsiniz. Varsa olması, daha sonra başka bir bilgisayara yükleyin ve daha fazla istemci sertifikaları üretebilir veya başka bir .cer dosyasını dışarı aktarma. Otomatik olarak imzalanan kök sertifika .pfx dışarı aktarmak için kök sertifika Seç ve açıklandığı gibi aynı adımları kullanın [bir istemci sertifikasını dışarı aktarma](#clientexport).

## <a name="create-and-install-client-certificates"></a>Oluşturma ve istemci sertifikalarını yükleme

Doğrudan istemci bilgisayarda otomatik olarak imzalanan sertifika yüklemeyin. Otomatik olarak imzalanan sertifikadan bir istemci sertifikası gerekir. Ardından dışarı aktarın ve istemci bilgisayara istemci sertifikası yükleyin. Aşağıdaki adımlar, dağıtım modeli özel değildir. Bunlar, Resource Manager ve klasik için geçerli olur.

### <a name="clientcert"></a>İstemci sertifikası oluşturma

Noktadan Siteye bağlantı kullanarak bir sanal ağa bağlanan her istemci bilgisayarda bir istemci sertifikası yüklü olmalıdır. Ardından vermek otomatik olarak imzalanan kök sertifikadan istemci sertifikası oluşturma ve istemci sertifikasını yükleme. İstemci sertifikası yüklü değilse, kimlik doğrulaması başarısız olur. 

Aşağıdaki adımlarda bir otomatik olarak imzalanan kök sertifikadan bir istemci sertifikası oluşturma aracılığıyla yol. Birden çok istemci sertifikaları aynı kök sertifikadan oluşturabilir. Aşağıdaki adımları kullanarak istemci sertifikası oluşturduğunuzda, istemci sertifikasının, sertifikayı oluşturmak için kullandığınız bilgisayarda otomatik olarak yüklenir. Bir istemci sertifikasını başka bir istemci bilgisayara yüklemek istiyorsanız, sertifikayı dışarı aktarabilirsiniz.
 
1. Otomatik olarak imzalanan sertifika oluşturmak için kullanılan aynı bilgisayarda yönetici olarak bir komut istemi açın.
2. Değiştirebilir ve bir istemci sertifikası oluşturmak için örneği çalıştırın.
   * Değişiklik *"P2SRootCert"* istemci sertifikası ürettiğini otomatik olarak imzalanan kök adı. Ne olursa olsun kök sertifikasının adı kullandığınızdan emin olun ' CN =' değerindeydi otomatik olarak imzalanan kök oluştururken belirttiğiniz.
   * Değişiklik *P2SChildCert* bir istemci sertifikası oluşturmak istediğiniz adı.

   Değişiklik yapmadan aşağıdaki örneği çalıştırırsanız, sonuç P2SChildcert P2SRootCert'kök sertifikadan oluşturulmuş, kişisel sertifika deposunda adlı bir istemci sertifikası olur.

   ```cmd
   makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha256
   ```

### <a name="clientexport"></a>Bir istemci sertifikasını dışarı aktarma

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Dışarı aktarılan istemci sertifikası yükleme

Bir istemci sertifikası yüklemek için bkz [bir istemci sertifikasını yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye yapılandırmanızı ile devam edin. 

* İçin **Resource Manager** dağıtım modeli adımlara bakın [yapılandırma P2S yerel Azure sertifika doğrulaması kullanarak](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* İçin **Klasik** dağıtım modeli adımlara bakın [(Klasik) bir sanal ağa noktadan siteye VPN bağlantısı yapılandırma](vpn-gateway-howto-point-to-site-classic-azure-portal.md).

P2S sorun giderme bilgileri için [Azure noktadan siteye bağlantıları sorununu giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
