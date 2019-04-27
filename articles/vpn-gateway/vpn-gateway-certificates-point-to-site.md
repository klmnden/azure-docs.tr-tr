---
title: 'Oluşturma ve noktadan siteye için sertifikalar verme: PowerShell: Azure | Microsoft Docs'
description: Otomatik olarak imzalanan kök sertifika oluşturma, ortak anahtarını dışarı aktarmak ve Windows 10 veya Windows Server 2016 üzerinde PowerShell kullanarak istemci sertifikalarını oluşturmak.
services: vpn-gateway
documentationcenter: na
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: cherylmc
ms.openlocfilehash: 74639dee6fb548e1c9067cae6fc22f6e3cc872c3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60764025"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-powershell"></a>Oluşturma ve noktadan siteye için sertifika dışarı aktarma PowerShell'i kullanma

Noktadan siteye bağlantılar, kimlik doğrulaması için sertifikaları kullanır. Bu makalede bir otomatik olarak imzalanan kök sertifika oluşturma ve Windows 10 veya Windows Server 2016 üzerinde PowerShell kullanarak istemci sertifikalarını oluşturmak nasıl gösterir. Farklı bir sertifika yönergeler arıyorsanız bkz [sertifikalar - Linux](vpn-gateway-certificates-point-to-site-linux.md) veya [sertifikalar - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).

Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayarda bu makaledeki adımları gerçekleştirmeniz gerekir. Sertifikaları oluşturmak için kullanabileceğiniz PowerShell cmdlet'leri, işletim sisteminin bir parçasıdır ve diğer Windows sürümlerinde çalışmaz. Windows 10 veya Windows Server 2016 bilgisayarı, yalnızca sertifikalarını oluşturmak için gereklidir. Sertifikaları oluşturduktan sonra bunları karşıya yükleyebilir veya bunları tüm desteklenen istemci işletim sistemine yükleyin. 

Windows 10 veya Windows Server 2016'ya bir bilgisayara erişiminiz yoksa, kullanabileceğiniz [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) sertifikalarını oluşturmak için. Her iki yöntemi kullanarak oluşturduğunuz sertifika birinde yüklenebilir [desteklenen](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) istemci işletim sistemi.

## <a name="rootcert"></a>1. Otomatik olarak imzalanan kök sertifika oluşturma

Bir otomatik olarak imzalanan kök sertifika oluşturmak için New-SelfSignedCertificate cmdlet'ini kullanın. Ek bir parametre için bilgi [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

1. Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayarda, yükseltilmiş ayrıcalıklarla bir Windows PowerShell konsolu açın. Bu örnekler, "Deneyin" Azure Cloud Shell'de çalışmaz. Bu örnek yerel olarak çalıştırmanız gerekir.
2. Otomatik olarak imzalanan kök sertifika oluşturmak için aşağıdaki örneği kullanın. Aşağıdaki örnek, '' Sertifikalar-Geçerli kullanıcı\kişisel\sertifikalar' otomatik olarak yüklenen P2SRootCert' adlı bir otomatik olarak imzalanan kök sertifika oluşturur. Sertifika açarak görüntüleyebilirsiniz *certmgr.msc*, veya *kullanıcı sertifikalarını Yönet*.

   ```powershell
   $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
   -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
   -HashAlgorithm sha256 -KeyLength 2048 `
   -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
   ```

## <a name="clientcert"></a>2. İstemci sertifikası oluşturma

Noktadan Siteye bağlantı kullanarak bir sanal ağa bağlanan her istemci bilgisayarda bir istemci sertifikası yüklü olmalıdır. Ardından vermek otomatik olarak imzalanan kök sertifikadan istemci sertifikası oluşturma ve istemci sertifikasını yükleme. İstemci sertifikası yüklü değilse, kimlik doğrulaması başarısız olur. 

Aşağıdaki adımlarda bir otomatik olarak imzalanan kök sertifikadan bir istemci sertifikası oluşturma aracılığıyla yol. Birden çok istemci sertifikaları aynı kök sertifikadan oluşturabilir. Aşağıdaki adımları kullanarak istemci sertifikası oluşturduğunuzda, istemci sertifikasının, sertifikayı oluşturmak için kullandığınız bilgisayarda otomatik olarak yüklenir. Bir istemci sertifikasını başka bir istemci bilgisayara yüklemek istiyorsanız, sertifikayı dışarı aktarabilirsiniz.

Örnekler, bir yıl içinde süresi dolan bir istemci sertifikası oluşturmak için New-SelfSignedCertificate cmdlet'ini kullanın. İstemci sertifikası farklı bir zaman aşımı değerini ayarlama gibi ek parametre bilgi edinmek için bkz [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

### <a name="example-1"></a>Örnek 1

Bu örnek, PowerShell Konsolunuzu otomatik olarak imzalanan kök sertifikayı oluşturduktan sonra kapatılmamış kullanın. Bu örnek önceki bölümden devam eder ve bildirilen '$cert' değişkenini kullanır. Otomatik olarak imzalanan kök sertifikayı oluşturduktan sonra PowerShell konsolunu kapalı ya da ek istemci sertifikaları yeni bir PowerShell Konsolu oturumu oluşturuyorsunuz içindeki adımları kullanın [örnek 2](#ex2).

Değiştirebilir ve bir istemci sertifikası oluşturmak için örneği çalıştırın. Değişiklik yapmadan aşağıdaki örneği çalıştırırsanız, sonuç 'P2SChildCert' adlı bir istemci sertifikası olur.  Başka bir alt sertifikayı Adlandır istiyorsanız, CN değeri değiştirin. Bu örnekte çalıştırırken TextExtension değiştirmeyin. Oluşturduğunuz istemci sertifikası, bilgisayarınızda 'Sertifikalar - Geçerli kullanıcı\kişisel\sertifikalar' otomatik olarak yüklenir.

```powershell
New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>Örnek 2

Ek istemci sertifikalarını oluşturma ya da otomatik olarak imzalanan kök sertifikanızı oluşturmak için kullanılan aynı PowerShell oturumunda kullanmıyorsanız, aşağıdaki adımları kullanın:

1. Bilgisayarda yüklü otomatik olarak imzalanan kök sertifikayı belirleyin. Bu cmdlet, bilgisayarınızda yüklü sertifikaların bir listesini döndürür.

   ```powershell
   Get-ChildItem -Path “Cert:\CurrentUser\My”
   ```
2. Döndürülen listeden konu adını bulun ve ardından bir metin dosyasına yanında bulunan parmak izini kopyalayın. Aşağıdaki örnekte, iki sertifika vardır. CN adının bir alt sertifikası oluşturmak istediğiniz otomatik olarak imzalanan kök sertifika adıdır. Bu durumda, 'P2SRootCert'.

   ```
   Thumbprint                                Subject
  
   AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
   7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
   ```
3. Önceki adımdaki parmak izini kullanarak kök sertifikası için bir değişken bildirir. Parmak İZİ alt sertifika oluşturmak istediğiniz kök sertifika parmak iziyle değiştirin.

   ```powershell
   $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
   ```

   Örneğin, önceki adımda P2SRootCert için parmak izini kullanarak değişkeni şöyle görünür:

   ```powershell
   $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
   ```
4. Değiştirebilir ve bir istemci sertifikası oluşturmak için örneği çalıştırın. Değişiklik yapmadan aşağıdaki örneği çalıştırırsanız, sonuç 'P2SChildCert' adlı bir istemci sertifikası olur. Başka bir alt sertifikayı Adlandır istiyorsanız, CN değeri değiştirin. Bu örnekte çalıştırırken TextExtension değiştirmeyin. Oluşturduğunuz istemci sertifikası, bilgisayarınızda 'Sertifikalar - Geçerli kullanıcı\kişisel\sertifikalar' otomatik olarak yüklenir.

   ```powershell
   New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
   -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
   -HashAlgorithm sha256 -KeyLength 2048 `
   -CertStoreLocation "Cert:\CurrentUser\My" `
   -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
   ```

## <a name="cer"></a>3. Kök sertifikanın ortak anahtarı (.cer) dışarı aktarma

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]


### <a name="export-the-self-signed-root-certificate-and-private-key-to-store-it-optional"></a>Otomatik olarak imzalanan kök sertifika ve (isteğe bağlı) depolamak için özel anahtarı dışarı aktar

Otomatik olarak imzalanan kök sertifikasını dışarı aktarma ve depolamak güvenli olarak yedekleme isteyebilirsiniz. Varsa olabilir, daha sonra başka bir bilgisayara yükleyin ve daha fazla istemci sertifikalarını oluşturmak. Otomatik olarak imzalanan kök sertifika .pfx dışarı aktarmak için kök sertifika Seç ve açıklandığı gibi aynı adımları kullanın [bir istemci sertifikasını dışarı aktarma](#clientexport).

## <a name="clientexport"></a>4. İstemci sertifikasını dışarı aktarma

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]


## <a name="install"></a>5. Dışarı aktarılan bir istemci sertifikasını yükleme

P2S bağlantısı üzerinden Vnet'e bağlanan her istemcinin bir istemci sertifikası, yerel olarak yüklü olmasını gerektirir.

Bir istemci sertifikası yüklemek için bkz [noktadan siteye bağlantılar için bir istemci sertifikasını yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="install"></a>6. P2S yapılandırma adımlarını uygulayın

Noktadan siteye yapılandırmanızı ile devam edin.

* İçin **Resource Manager** dağıtım modeli adımlara bakın [yapılandırma P2S yerel Azure sertifika doğrulaması kullanarak](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* İçin **Klasik** dağıtım modeli adımlara bakın [(Klasik) bir sanal ağa noktadan siteye VPN bağlantısı yapılandırma](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
* P2S için sorun giderme bilgileri için bkz. [sorun giderme Azure noktadan siteye bağlantılar](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
