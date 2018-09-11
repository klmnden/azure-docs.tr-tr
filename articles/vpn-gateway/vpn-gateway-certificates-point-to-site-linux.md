---
title: 'Oluşturma ve noktadan siteye için sertifika dışarı aktarma: Linux: CLI: Azure | Microsoft Docs'
description: Otomatik olarak imzalanan kök sertifika oluşturma, ortak anahtarını dışarı aktarmak ve Linux (strongSwan) CLI kullanarak istemci sertifikalarını oluşturmak.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/06/2018
ms.author: cherylmc
ms.openlocfilehash: 8647b3b3eda980dbd5d5ec368b6b4b13949ecaf1
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44305747"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-linux-strongswan-cli"></a>Oluşturma ve noktadan siteye için sertifika dışarı aktarma Linux strongSwan CLI kullanma

Noktadan siteye bağlantılar, kimlik doğrulaması için sertifikaları kullanır. Bu makalede Linux CLI ve strongSwan kullanarak istemci sertifikalarını oluşturmak ve otomatik olarak imzalanan kök sertifika oluşturma gösterilmektedir. Farklı bir sertifika yönergeler arıyorsanız bkz [Powershell](vpn-gateway-certificates-point-to-site.md) veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) makaleler.

> [!NOTE]
> Bu makaledeki adımlarda strongSwan gerektirir.
>

Bu makaledeki adımlar için kullanılan bilgisayar yapılandırması aşağıdaki şöyleydi:

| | |
|---|---|
|**Bilgisayar**| Ubuntu Server 16.04<br>ID_LIKE debian =<br>PRETTY_NAME = "16.04.4 Ubuntu LTS"<br>VERSION_ID "16.04" = |
|**Bağımlılıklar**| yüklemeyi apt-get strongswan IKEv2 strongswan-plugin-eap-tls<br>apt-yükleme libstrongswan-standart-eklentileri get |

## <a name="install-strongswan"></a>StrongSwan yükleyin

1. `apt-get install strongswan-ikev2 strongswan-plugin-eap-tls`
2. `apt-get install libstrongswan-standard-plugins`

Adımları strongSwan GUI kullanarak yükleme hakkında daha fazla bilgi için bkz. [istemci yapılandırması](point-to-site-vpn-client-configuration-azure-cert.md#install) makalesi.

## <a name="generate-keys-and-certificate"></a>Anahtarlar ve sertifika oluştur

1. CA sertifika oluşturun.

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```
2. CA sertifikasını base64 biçiminde yazdırılır. Bu, Azure tarafından desteklenen biçimidir. Daha sonra bu Azure'a P2S yapılandırmanızın bir parçası yükler.

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```
3. Kullanıcı sertifika oluşturun.

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```
4. Kullanıcı sertifikayı içeren bir p12 paketi oluşturun. Bu paket sonraki adımlarda ile çalışırken kullanılacak [istemcisi yapılandırma dosyalarını](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye yapılandırmanızı devam [VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).