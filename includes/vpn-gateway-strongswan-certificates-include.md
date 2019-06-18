---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/16/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c6f9065786879749eee6187e93283f4c026b7fff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66150060"
---
Aşağıdaki bilgisayar yapılandırması için aşağıdaki adımları kullanılmıştır:

  | | |
  |---|---|
  |Computer| Ubuntu Server 16.04<br>ID_LIKE debian =<br>PRETTY_NAME="Ubuntu 16.04.4 LTS"<br>VERSION_ID="16.04" |
  |Bağımlılıkları| strongSwan |

#### <a name="1-install-strongswan"></a>1. StrongSwan yükleyin

Gerekli strongSwan yapılandırmasını yüklemek için aşağıdaki komutları kullanın:

```
apt-get install strongswan-ikev2 strongswan-plugin-eap-tls
```

```
apt-get install libstrongswan-standard-plugins
```

```
apt-get install strongswan-pki
```

#### <a name="2-generate-keys-and-certificate"></a>2. Anahtarlar ve sertifika oluştur

CA sertifika oluşturun.

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```

CA sertifikasını base64 biçiminde yazdırılır. Bu, Azure tarafından desteklenen biçimidir. Daha sonra bu Azure'a P2S yapılandırmanızın bir parçası yükler.

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```

Kullanıcı sertifika oluşturun.

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```

Kullanıcı sertifikayı içeren bir p12 paketi oluşturun. Bu paket, istemci yapılandırma dosyalarıyla çalışırken sonraki adımlarda kullanılacaktır.

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```