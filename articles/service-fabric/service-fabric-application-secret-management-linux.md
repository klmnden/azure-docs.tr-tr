---
title: Bir şifreleme sertifikası ayarlayın ve Azure Service Fabric Linux kümelerinde parolaları şifrelemek | Microsoft Docs
description: Bir şifreleme sertifikası ayarlayın ve Linux kümelerinde parolaları şifrelemek hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: shsha
manager: ''
editor: ''
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2019
ms.author: shsha
ms.openlocfilehash: 9589d6ea69a2293d592a9e63f2b726f1a620bb9e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62126997"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-linux-clusters"></a>Linux kümeleri parolaları şifrelemek ve bir şifreleme sertifikasını ayarlama
Bu makalede, bir şifreleme sertifikası ayarlayın ve Linux kümelerinde parolaları şifrelemek için kullanma gösterilmektedir. Windows kümeleri için bkz: [ayarlanmış yukarı bir şifreleme sertifikası ve Windows Küme parolaları şifrelemek][secret-management-windows-specific-link].

## <a name="obtain-a-data-encipherment-certificate"></a>Veri şifreleme sertifikası alın
Veri şifreleme sertifikası kesinlikle şifrelenmesi ve şifresinin çözülmesi için kullanılan [parametreleri] [ parameters-link] bir hizmetin Settings.XML dosyasının içinde ve [ortam değişkenlerini] [ environment-variables-link] bir hizmetin ServiceManifest.xml içinde. Bu şifre metninin imzalama veya kimlik doğrulaması için kullanılabilir değildir. Sertifikanın aşağıdaki gereksinimleri karşılaması gerekir:

* Sertifika özel anahtar içermelidir.
* Sertifika anahtar kullanımı, veri şifreleme (10) içermelidir ve sunucu kimlik doğrulaması veya istemci kimlik doğrulaması içermemelidir.

  Örneğin, aşağıdaki komutları kullanarak OpenSSL gerekli sertifikayı oluşturmak için kullanılabilir:
  
  ```console
  user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
  user@linux:~$ cat TestCert.prv >> TestCert.pem
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Kümenizin sertifikasını yükleme
Sertifikanın altında kümedeki her düğümde yüklenmelidir `/var/lib/sfcerts`. Kullanıcı hesabı altında hizmetin çalıştığı (varsayılan olarak sfuser) **okuma erişimi olması gereken** yüklü sertifika (diğer bir deyişle, `/var/lib/sfcerts/TestCert.pem` geçerli örnek için).

## <a name="encrypt-secrets"></a>Parolaları şifrelemek
Aşağıdaki kod parçacığında, bir şifrelemek için kullanılabilir. Bu kod parçacığı, değer yalnızca şifreler; Mevcut **değil** şifre metni oturum açın. **Kullanmalısınız** ciphertext gizli değerleri oluşturmak için kümenizdeki yüklü aynı şifreleme sertifikası.

```console
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```
Elde edilen encrypted.txt base-64 kodlama dizesi çıktı hem gizli ciphertext hem de bunu şifrelemek için kullanılan sertifikayla ilgili bilgileri içerir. OpenSSL ile şifresini çözerek geçerliliğini doğrulayabilirsiniz.
```console
user@linux:$ cat encrypted.txt | base64 -d | openssl smime -decrypt -inform der -inkey TestCert.prv
```

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [uygulamada şifrelenmiş parolalar belirtin.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
