---
title: 'Oluştur ve noktası Site için sertifikalar verme: PowerShell: Azure | Microsoft Docs'
description: Bir otomatik olarak imzalanan sertifika oluşturmak, ortak anahtarını dışarı aktarmak ve Windows 10 veya Windows Server 2016 PowerShell kullanarak istemci sertifikalarını oluşturmak.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2018
ms.author: cherylmc
ms.openlocfilehash: 385b6ed2e8104fd2e15e6e55d46dcd12b963ec6b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="generate-and-export-certificates-for-point-to-site-using-powershell"></a>Oluştur ve noktası Site için sertifikalar verme PowerShell'i kullanma

Noktadan siteye bağlantılar, kimlik doğrulaması için sertifikaları kullanır. Bu makalede bir otomatik olarak imzalanan sertifika oluşturmak ve Windows 10 veya Windows Server 2016 PowerShell kullanarak istemci sertifikalarını oluşturmak nasıl gösterir. Kök sertifikalarını yüklemek nasıl gibi noktadan siteye yapılandırma adımlarını arıyorsanız ' yapılandırma noktası siteye ' makaleleri birini aşağıdaki listeden seçin:

> [!div class="op_single_selector"]
> * [Otomatik olarak imzalanan sertifikalar - PowerShell oluşturma](vpn-gateway-certificates-point-to-site.md)
> * [Otomatik olarak imzalanan sertifikalar - MakeCert oluşturma](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Noktası-Site - Resource Manager - Azure portalında yapılandırın](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Noktadan siteye - Resource Manager - PowerShell yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Noktası-Site - Classic - Azure portalında yapılandırın](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayarda bu makaledeki adımları uygulamanız gerekir. Sertifikalarını oluşturmak için kullandığınız PowerShell cmdlet'leri işletim sisteminin bir parçasıdır ve diğer Windows sürümlerinde çalışmaz. Windows 10 veya Windows Server 2016 bilgisayar yalnızca sertifikalarını oluşturmak için gereklidir. Sertifikaları oluşturduktan sonra bunları karşıya yükleyebilir veya tüm desteklenen istemci işletim sistemine yükleyin. 

Windows 10 veya Windows Server 2016 bir bilgisayara erişiminiz yoksa kullanabileceğiniz [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) sertifikalarını oluşturmak için. Her iki yöntemi kullanarak oluşturduğunuz sertifikalar herhangi yüklenebilir [desteklenen](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) istemci işletim sistemi.

## <a name="rootcert"></a>1. Otomatik olarak imzalanan sertifika oluştur

Bir otomatik olarak imzalanan sertifika oluşturmak için New-SelfSignedCertificate cmdlet'ini kullanın. Ek parametre bilgi için bkz: [yeni SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

1. Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayarda, yükseltilmiş ayrıcalıklarla bir Windows PowerShell konsolu açın.
2. Otomatik olarak imzalanan sertifika oluşturmak için aşağıdaki örneği kullanın. Aşağıdaki örnek, '' Sertifikalar-Geçerli User\Personal\Certificates' otomatik olarak yüklenen P2SRootCert' adlı otomatik olarak imzalanan bir sertifika oluşturur. Sertifika açarak görüntüleyebileceğiniz *certmgr.msc*, veya *kullanıcı sertifikaları yönetme*.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

## <a name="clientcert"></a>2. İstemci sertifikası oluşturma

Noktadan Siteye bağlantı kullanarak bir sanal ağa bağlanan her istemci bilgisayarda bir istemci sertifikası yüklü olmalıdır. Ardından dışa otomatik olarak imzalanan kök sertifikasından bir istemci sertifikasını oluşturmak ve istemci sertifikasını yükleyin. İstemci sertifikası yüklü değilse, kimlik doğrulaması başarısız olur. 

Aşağıdaki adımlarda, otomatik olarak imzalanan kök sertifikasından bir istemci sertifikası oluşturma aracılığıyla yol. Birden çok istemci sertifikaları aynı kök sertifikası oluşturabilir. Aşağıdaki adımları kullanarak istemci sertifikalarını oluşturmak, istemci sertifikasının, sertifikayı oluşturmak için kullanılan bilgisayarda otomatik olarak yüklenir. Bir istemci sertifikası başka bir istemci bilgisayara yüklemek istiyorsanız, sertifikayı dışa aktarabilirsiniz.

Örnekler, bir yıl içinde süresi dolar bir istemci sertifikasını oluşturmak için yeni SelfSignedCertificate cmdlet'ini kullanın. İstemci sertifikası için farklı süre sonu değeri ayarlama gibi ek parametre bilgi için bkz [yeni SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

### <a name="example-1"></a>Örnek 1

Bu örnek önceki bölümden bildirilen '$cert' değişkeni kullanır. PowerShell Konsolu otomatik olarak imzalanan sertifika oluşturduktan sonra kapalı veya yeni bir PowerShell konsol oturumda ek istemci sertifikaları oluşturma, içindeki adımları kullanın [örnek 2](#ex2).

Değiştirme ve bir istemci sertifikasını oluşturmak için örnek çalıştırın. Değişiklik yapmadan aşağıdaki örneği çalıştırırsanız, 'P2SChildCert' adlı bir istemci sertifikası sonucudur.  Alt sertifika başka bir ad vermek istiyorsanız, CN değeri değiştirin. Bu örnek çalıştırırken TextExtension değiştirmeyin. Oluşturduğunuz istemci sertifikası, bilgisayarınızda 'Sertifikalar - Geçerli User\Personal\Certificates' otomatik olarak yüklenir.

```powershell
New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>Örnek 2

Ek istemci sertifikalarını oluşturmakta ya da otomatik olarak imzalanan sertifika oluşturmak için kullanılan aynı PowerShell oturumunda kullanmıyorsanız, aşağıdaki adımları kullanın:

1. Bilgisayarda yüklü otomatik olarak imzalanan kök sertifikayı belirleyin. Bu cmdlet, bilgisayarınızda yüklü sertifikaların listesini döndürür.

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. Konu adı döndürülen listesinden bulun ve sonra onu yanında bir metin dosyasına bulunur parmak izini kopyalayın. Aşağıdaki örnekte, iki sertifika vardır. CN adı bir alt sertifika oluşturmak istediğiniz otomatik olarak imzalanan sertifika adıdır. Bu durumda, 'P2SRootCert'.

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. Önceki adımdan parmak kullanarak kök sertifikası için bir değişken bildirin. Parmak İZİ alt sertifika oluşturmak istediğiniz kök sertifika parmak iziyle değiştirin.

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  Örneğin, önceki adımda P2SRootCert için parmak izini kullanarak değişkeni şöyle görünür:

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  Değiştirme ve bir istemci sertifikasını oluşturmak için örnek çalıştırın. Değişiklik yapmadan aşağıdaki örneği çalıştırırsanız, 'P2SChildCert' adlı bir istemci sertifikası sonucudur. Alt sertifika başka bir ad vermek istiyorsanız, CN değeri değiştirin. Bu örnek çalıştırırken TextExtension değiştirmeyin. Oluşturduğunuz istemci sertifikası, bilgisayarınızda 'Sertifikalar - Geçerli User\Personal\Certificates' otomatik olarak yüklenir.

  ```powershell
  New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="cer"></a>3. Kök sertifika ortak anahtarı (.cer) dışarı aktarma

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]


### <a name="export-the-self-signed-root-certificate-and-private-key-to-store-it-optional"></a>Otomatik olarak imzalanan kök sertifikayı ve (isteğe bağlı) depolamak için özel anahtarı dışarı aktar

Otomatik olarak imzalanan sertifika vermek ve depolamak güvenli olarak yedekleme isteyebilirsiniz. Varsa olabilir, daha sonra başka bir bilgisayara yükleyin ve daha fazla istemci certifiates oluşturur. Bir .pfx otomatik olarak imzalanan kök sertifikasını dışarı aktarmak için kök sertifikasını seçin ve açıklandığı gibi aynı adımları kullanın [bir istemci sertifikası verme](#clientexport).

## <a name="clientexport"></a>4. İstemci sertifikasını dışarı aktarma

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]


## <a name="install"></a>5. Dışarı aktarılan bir istemci sertifikasını yükleme

Vnet'e P2S bağlantısı üzerinden bağlanan her bir istemci bir istemci sertifikası, yerel olarak yüklü olmasını gerektirir.

Bir istemci sertifikası yüklemek için bkz: [noktadan siteye bağlantıları için bir istemci sertifikası yüklemek](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="install"></a>6. P2S yapılandırma adımlarını uygulayın

Noktası Site yapılandırmanızı ile devam edin.

* İçin **Resource Manager** dağıtım modeli adımları bkz [yapılandırma P2S yerel Azure sertifika kimlik doğrulaması kullanarak](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* İçin **Klasik** dağıtım modeli adımları bkz [bir sanal ağ (Klasik) bir noktadan siteye VPN bağlantısı yapılandırma](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
* P2S için sorun giderme bilgileri için bkz: [sorun giderme Azure noktadan siteye bağlantıları](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
