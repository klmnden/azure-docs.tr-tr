---
title: OPC UA istemcisi ve OPC UA sunucu uygulaması OPC Vault Azure kullanarak güvenli hale getirme | Microsoft Docs
description: OPC UA istemcisi ve OPC UA sunucu uygulaması yeni bir anahtar çifti ve OPC kasasını kullanarak sertifika ile güvenli hale getirin.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 5ba2dba02585598b3797dd1b490976ebe34b489e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61450673"
---
# <a name="secure-opc-ua-client-and-opc-ua-server-application"></a>OPC UA güvenli istemci ve OPC UA sunucu uygulaması 
Kasa OPC yapılandırma, kaydetme ve sertifika yaşam döngüsü OPC UA sunucusu ve istemci uygulamalarını bulutta yönetme bir mikro hizmetidir. Bu makalede bir OPC UA istemcisi ve OPC UA sunucu uygulaması yeni bir anahtar çifti ve OPC kasasını kullanarak sertifika ile güvenli gösterilmektedir.

Aşağıdaki Kurulum, OPC istemci OPC PLC bağlanabilirliği test ediyor. Her iki bileşenin sağ sertifikalarla henüz sağlanan değil çünkü varsayılan olarak, bağlantı mümkün değildir. Bu iş akışında biz değil OPC UA bileşenleri otomatik olarak imzalanan sertifikalar kullanmak ve bunları OPC Vault aracılığıyla oturum. Öncekini Göster [testbed](howto-opc-vault-deploy-existing-client-plc-communication.md). Bunun yerine, bu testbed OPC kasa tarafından oluşturulan bileşenler ile yeni bir sertifika yanı sıra yeni bir özel anahtara sahip sağlar. OPC UA güvenliği hakkında bazı bilgiler bu bulunabilir [teknik incelemesi](https://opcfoundation.org/wp-content/uploads/2014/05/OPC-UA_Security_Model_for_Administrators_V1.00.pdf). OPC UA belirtiminde eksiksiz bilgiler bulunabilir.

Testbed: Şu ortam, test etmek için yapılandırılır.

OPC kasa komut dosyaları:
- OPC UA istemcisi ve OPC UA sunucu uygulamaları yeni bir anahtar çifti ve OPC kasasını kullanarak sertifika ile güvenli hale getirin.

> [!NOTE]
> Daha fazla bilgi için bkz. GitHub [depo](https://github.com/Azure-Samples/iot-edge-industrial-configs#testbeds).

## <a name="generate-a-new-certificate-and-private-key"></a>Yeni sertifika ve özel anahtar oluştur 
**Hazırlık**
- Ortam değişkenlerini emin `$env:_PLC_OPT` ve `$env:_CLIENT_OPT` tanımsızdır. Örneğin, `$env:_PLC_OPT=""` , PowerShell
- Ortam değişkenini ayarlamak `$env:_OPCVAULTID` bir dizeye verilerinizi OPC kasaya yeniden bulmanıza olanak tanır. 6 basamaklı sayıya ayarlanması önerilir. Bizim örneğimizde "123456" değişkeni için değer olarak kullanıldı.
- Docker birim bulunduğundan emin olun `opcclient` veya `opcplc`. Danışın `docker volume ls` ve bunları kaldırma `docker volume rm <volumename>`. Kapsayıcılarla da silmeniz gerekebilir `docker rm <containerid>` birimleri bir kapsayıcı tarafından kullanılıyorsa.

**Hızlı Başlangıç**
1. Git [OPC kasa Web sitesi](https://opcvault.azurewebsites.net/)

1. `Register New` seçeneğini belirleyin

1. Önceki testbed ait günlük çıktıda gösterildiği OPC PLC bilgi girin `CreateSigningRequest information` giriş alanlarına alan `Register New OPC UA Application` sayfasında `Server` ApplicationType olarak.

1. `Register` seçeneğini belirleyin

1. Sonraki sayfada `Request New Certificate for OPC UA Application` seçin `Request new KeyPair and Certificate`

1. Sonraki sayfada `Generate a new Certificate with a Signing Request` yapıştırın `CSR (base64 encoded)` günlük çıkış dizeden `CreateRequest` giriş alanı. Tam dize kopyalama emin olun.

1. Sonraki sayfada `Request New Certificate for OPC UA Application` seçin `Request new Certificate with Signing Request`

1. Sonraki sayfada `Generate a new KeyPair and for an OPC UA Application` girin `CN=OpcPlc` SubjectName, olarak `opcplc-<_OPCVAULTID>` (Değiştir `<_OPCVAULTID>` sizinki ile) DomainName seçin `PEM` PrivateKeyFormat olarak ve bir parola girin (daha sonra olarak diyoruz `<certpassword-string>`)

1. `Generate New KeyPair` seçeneğini belirleyin

1. Artık ileriye doğru geçiş yapıyorsanız `View Certificate Request Details`. Bu sayfada sertifika depoları sağlamak için gerekli tüm bilgileri indirebilirsiniz `opc-plc`.

1. Bu sayfada:  
    - Seçin `Certificate` içinde `Download as Base64` kopyalayıp metin dizesi sunulan içinde `EncodedBase64` alan ve daha sonra kullanmak üzere saklayın. Olarak diyoruz `<applicationcertbase64-string>` daha sonra. `Back` öğesini seçin.
    - Seçin `PrivateKey` içinde `Download as Base64` kopyalayıp metin dizesi sunulan içinde `EncodedBase64` alan ve daha sonra kullanmak üzere saklayın. Olarak diyoruz `<privatekeybase64-string>` daha sonra. `Back` öğesini seçin.
    - Seçin `Issuer` içinde `Download as Base64` kopyalayıp metin dizesi sunulan içinde `EncodedBase64` alan ve daha sonra kullanmak üzere saklayın. Olarak diyoruz `<addissuercertbase64-string>` daha sonra. `Back` öğesini seçin.
    - Seçin `Crl` içinde `Download as Base64` kopyalayıp metin dizesi sunulan içinde `EncodedBase64` alan ve daha sonra kullanmak üzere saklayın. Olarak diyoruz `<updatecrlbase64-string>` daha sonra. `Back` öğesini seçin.

1. Şimdi, PowerShell'de adlı bir değişken ayarlamak `$env:_PLC_OPT`:

   `$env:_PLC_OPT="--applicationcertbase64 <applicationcertbase64-string> --privatekeybase64 <privatekeybase64-string> --certpassword <certpassword-string> --addtrustedcertbase64 <addissuercertbase64-string> --addissuercertbase64 <addissuercertbase64-string> --updatecrlbase64 <updatecrlbase64-string>"`  

    Web sitesinden alınan seçeneği değerleri Base64 dizesi olarak geçilen dizelerin değiştirin.  

1. Sürümünden itibaren tam bu işlemi yinelemeniz `Register New` OPC istemcisi. Yalnızca, farkında olmanız gereken aşağıdaki farklılıklar vardır:
    - Günlük çıktısını kullanın `opcclient`.
    - Seçin `Client` kayıt sırasında ApplicationType olarak.
    - Kullanım `$env:_CLIENT_OPT` PowerShell değişkeninin adı.

    > [!NOTE] 
    > Bu senaryo ile çalışırken, size, tanınan `<addissuercertbase64-string>` ve `<updatecrlbase64-string>` değerler için özdeş `opcplc` ve `opcclient`. Bu, bizim kullanım durumu için geçerlidir ve adımları yaparken bazı zamandan tasarruf edebilirsiniz.

**Hızlı Başlangıç**

Depo kök dizininde aşağıdaki PowerShell komutunu çalıştırın:

```
docker-compose -f connecttest.yml up
```

**Doğrulama**

İki bileşeni varolan uygulama sertifikası uygulanmamış doğrulayın. Günlük çıktısını denetleyin. OPC PLC çıktısı aşağıdadır ve benzer bir günlük çıktısını OPC istemci vardır.

```
opcplc-123456 | [13:40:08 INF] There is no existing application certificate.
```
Uygulama sertifikası varsa, docker birimleri hazırlık adımlarda anlatılan şekilde kaldırın.

Oturum açma OPC kasa CA sertifikasını veren sertifika deposunda da güvenilen eş sertifika deposu olduğu gibi yüklendiğini doğrulayın. Günlük çıktısını OPC PLC aşağıda verilmiştir ve benzer bir günlük çıktısını OPC istemci vardır. 

```
opcplc-123456 | [13:40:09 INF] Trusted issuer store contains 1 certs
opcplc-123456 | [13:40:09 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [13:40:09 INF] Trusted issuer store has 1 CRLs.
opcplc-123456 | [13:40:09 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [13:40:09 INF] Trusted peer store contains 1 certs
opcplc-123456 | [13:40:09 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [13:40:09 INF] Trusted peer store has 1 CRLs.
opcplc-123456 | [13:40:09 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [13:40:09 INF] Rejected certificate store contains 0 certs
```
OPC PLC OPC kasa tarafından imzalanmış sertifikalara sahip tüm OPC UA istemciler artık güvenmez.

Oturum açma özel anahtar biçimi PEM tanınır ve yeni bir uygulama sertifikası yüklü olduğundan emin olun. Günlük çıktısını OPC PLC aşağıda verilmiştir ve benzer bir günlük çıktısını OPC istemci vardır. 

```
opcplc-123456 | [13:40:09 INF] The private key for the new certificate was passed in using PEM format.
opcplc-123456 | [13:40:09 INF] Remove the existing application certificate.
opcplc-123456 | [13:40:09 INF] The new application certificate 'CN=OpcPlc' and thumbprint 'A3CB288FC1D2B7A5C08AACF531CF4A85E44A6C4C' was added to the application certificate store.
opcplc-123456 | [13:40:09 INF] Activating the new application certificate with thumbprint 'A3CB288FC1D2B7A5C08AACF531CF4A85E44A6C4C'.
```

Uygulama sertifikasını ve özel anahtarı artık uygulama sertifika deposunda yüklü ve OPC UA uygulama tarafından kullanılır.

OPC PLC ve OPC istemci arasındaki bağlantı başarıyla gerçekleşip gerçekleşmediğini ve OPC istemci başarıyla OPC PLC verileri okuyabilir doğrulayın. Aşağıdaki çıktı OPC istemci günlük çıktısını görmeniz gerekir:
```
opcclient-123456 | [13:40:12 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [13:40:12 INF] Session successfully created with Id ns=3;i=941910499.
opcclient-123456 | [13:40:12 INF] The session to endpoint 'opc.tcp://opcplc-123456:50000/' has 4 entries in its namespace array:
opcclient-123456 | [13:40:12 INF] Namespace index 0: http://opcfoundation.org/UA/
opcclient-123456 | [13:40:12 INF] Namespace index 1: urn:OpcPlc:opcplc-123456
opcclient-123456 | [13:40:12 INF] Namespace index 2: http://microsoft.com/Opc/OpcPlc/
opcclient-123456 | [13:40:12 INF] Namespace index 3: http://opcfoundation.org/UA/Diagnostics
opcclient-123456 | [13:40:12 INF] The server on endpoint 'opc.tcp://opcplc-123456:50000/' supports a minimal sampling interval of 0 ms.
opcclient-123456 | [13:40:12 INF] Execute 'OpcClient.OpcTestAction' action on node 'i=2258' on endpoint 'opc.tcp://opcplc-123456:50000/' with security.
opcclient-123456 | [13:40:12 INF] Action (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258') completed successfully
opcclient-123456 | [13:40:12 INF] Value (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258'): 10/21/2018 13:40:12
```
Bu çıkış görürseniz, her ikisi de artık bir CA ve bu CA tarafından imzalanan sertifikalara hem tarafından imzalanmış sertifikalara sahip olduğundan ardından OPC PLC artık OPC istemci veya tersine güvenen.

### <a name="a-testbed-for-opc-publisher"></a>OPC Publisher'ın bir testbed ###

**Hızlı Başlangıç**

Depo kök dizininde aşağıdaki PowerShell komutunu çalıştırın:
```
docker-compose -f testbed.yml up
```

**Doğrulama**
- Yapılandırılmış ayarlayarak Iothub veri gönderildiğini doğrulamak `_HUB_CS` kullanarak [Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) veya [iothub-explorer](https://github.com/Azure/iothub-explorer).
- OPC test istemcisi IoTHub doğrudan yöntem çağrıları ve OPC yöntem çağrılarını Yayımla/düğümleri OPC test sunucudan yayımdan kaldırmak için OPC yayımcısını yapılandırmak için kullanmayı geçiyor.
- Hata iletileri için çıktıyı izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Mevcut bir projeyi OPC kasa dağıtmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Mevcut bir projeyi OPC İkizi dağıtma](howto-opc-twin-deploy-existing.md)