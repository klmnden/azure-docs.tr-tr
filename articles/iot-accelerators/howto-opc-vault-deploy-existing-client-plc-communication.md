---
title: Güvenli iletişim OPC istemci ve OPC Azure IOT OPC UA sertifika Yönetimi'ni kullanarak PLC | Microsoft Docs
description: OPC istemci ve OPC PLC iletişimi kullanarak OPC kasa CA sertifikalarını imzalama tarafından güvenli hale getirin.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: c437f6db21956d1be5e4f6d3512f325f37ca7308
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759669"
---
# <a name="secure-the-communication-of-opc-client-and-opc-plc"></a>Güvenli iletişim OPC istemci ve OPC PLC

Azure IOT OPC UA sertifika yönetimi, ayrıca OPC kasası olarak yapılandırabilirsiniz, bir mikro hizmet kaydı, bildiğiniz ve sertifika yaşam döngüsü için OPC UA sunucu ve istemci uygulamalarını bulutta yönetin. Bu makalede OPC kasa CA kullanarak sertifikalarını açarak OPC istemci ve OPC PLC iletişimin güvenliğini gösterilmektedir.

Aşağıdaki Kurulum, OPC istemci OPC PLC bağlantısını test eder. Her iki bileşenin doğru sertifika ile sağlanan değil çünkü varsayılan olarak, bağlantı mümkün değildir. Bir sertifika ile bir OPC UA bileşeni henüz sağlanmadı durumunda, başlangıçta otomatik olarak imzalanan bir sertifika oluşturur. Ancak, sertifika bir sertifika yetkilisi (CA) tarafından imzalanmış ve OPC UA bileşeni yüklü. OPC istemci ve OPC PLC için bunu yaptıktan sonra bağlantının etkin. Aşağıdaki iş akışı işlemi açıklanmaktadır. OPC UA güvenliği hakkında bazı bilgiler bulunabilir [bu belgeyi](https://opcfoundation.org/wp-content/uploads/2014/05/OPC-UA_Security_Model_for_Administrators_V1.00.pdf) teknik incelemesi. OPC UA belirtiminde eksiksiz bilgiler bulunabilir.

Testbed: Şu ortam, test etmek için yapılandırılır.

OPC kasa komut dosyaları:
- OPC istemci ve OPC PLC iletişimi kullanarak OPC kasa CA sertifikalarını imzalama tarafından güvenli hale getirin.

> [!NOTE]
> Daha fazla bilgi için bkz. GitHub [depo](https://github.com/Azure-Samples/iot-edge-industrial-configs#testbeds).

## <a name="generate-a-self-signed-certificate-on-startup"></a>Başlangıçta otomatik olarak imzalanan sertifika oluştur

**Hazırlık**

- Ortam değişkenlerini emin `$env:_PLC_OPT` ve `$env:_CLIENT_OPT` gibi tanımsızdır `$env:_PLC_OPT=""` , PowerShell'de.

- Ortam değişkenini ayarlamak `$env:_OPCVAULTID` bir dizeye verilerinizi OPC kasaya yeniden bulmanıza olanak tanır. Yalnızca alfasayısal karakterlere izin verilir. Bizim örneğimizde "123456", bu değişken için değeri olarak kullanıldı.

- Docker birim yok olun `opcclient` veya `opcplc`. Danışın `docker volume ls` ve bunları kaldırma `docker volume rm <volumename>`. Kapsayıcılarla da silmeniz gerekebilir `docker rm <containerid>` birimleri bir kapsayıcı tarafından kullanılıyorsa.

**Hızlı Başlangıç**

Depo kök dizininde aşağıdaki PowerShell komutunu çalıştırın:

```
docker-compose -f connecttest.yml up
```

**Doğrulama**

Oturum açma, ilk başlatmada yüklü hiçbir sertifika olduğunu doğrulayın. Burada günlük çıktısını OPC PLC (OPC istemci için benzer gösterir):...
```
opcplc-123456 | [20:51:32 INF] Trusted issuer store contains 0 certs
opcplc-123456 | [20:51:32 INF] Trusted issuer store has 0 CRLs.
opcplc-123456 | [20:51:32 INF] Trusted peer store contains 0 certs
opcplc-123456 | [20:51:32 INF] Trusted peer store has 0 CRLs.
opcplc-123456 | [20:51:32 INF] Rejected certificate store contains 0 certs
```
Bildirilen sertifikaları görürseniz, yukarıdaki hazırlama adımları ve docker birimleri silin.

OPC PLC bağlantısı başarısız olduğunu doğrulayın. Çıkış oturum OPC istemci aşağıdaki çıktı görmeniz gerekir:

```
opcclient-123456 | [20:51:35 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [20:51:36 ERR] Session creation to endpoint 'opc.tcp://opcplc-123456:50000/' failed 1 time(s). Please verify if server is up and OpcClient configuration is correct.
opcclient-123456 | Opc.Ua.ServiceResultException: Certificate is not trusted.
```
Hatanın nedenini sertifikası güvenilir değil ' dir. Diğer bir deyişle `opc-client` bağlanmaya çalıştığınız `opc-plc` geri gösteren yanıt alınıp `opc-plc` güvenmediği `opc-client`, sonucunu `opc-plc` sertifika doğrulanamıyor, `opc-client` sağlamıştır. Bu ek sertifika yapılandırma ile otomatik olarak imzalanan bir sertifika olduğundan `opc-plc`, ve bu nedenle bağlantı başarısız oldu.

## <a name="sign-and-install-certificates-in-opc-ua-components"></a>Oturum ve OPC UA bileşenlerde sertifikaları yükleyin

**Hazırlık**
1. Adım 1'in günlük çıktısına bakın ve "CreateSigningRequest bilgi" OPC PLC ve OPC istemci için getirilemedi. Burada çıktısını OPC PLC için yalnızca gösterilmiştir:

    ```
    opcplc-123456 | [20:51:32 INF] ----------------------- CreateSigningRequest information ------------------
    opcplc-123456 | [20:51:32 INF] ApplicationUri: urn:OpcPlc:opcplc-123456
    opcplc-123456 | [20:51:32 INF] ApplicationName: OpcPlc
    opcplc-123456 | [20:51:32 INF] ApplicationType: Server
    opcplc-123456 | [20:51:32 INF] ProductUri: https://github.com/azure-samples/iot-edge-opc-plc
    opcplc-123456 | [20:51:32 INF] DiscoveryUrl[0]: opc.tcp://opcplc-123456:50000
    opcplc-123456 | [20:51:32 INF] ServerCapabilities: DA
    opcplc-123456 | [20:51:32 INF] CSR (base64 encoded):
    opcplc-123456 | MIICmzCCAYMCAQAwETEPMA0GA1UEAwwGT3BjUGxjMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAwTvlbinAPWPxR9Lw1ndGsrLGy8GiqVOxyGaDpPUcMchX0k0/ncg28h7xrB2H1PThdiZxUJuUwsNM74HrVgt
    ofmXhA4dLM1cTxZHyJVFjl2L3vK5M58NNf6UNdKcB0x3LyuoT6mAIMXmCioqymFCk1TMzIMzbAe7JVAdUaSRBP1vuqQ1rV/cfNAe35dKQW4aPYgl7pR5f1hqprcDu/oca67X8L4kTl4oN0/bCYTk+Ibcd9cG462oAN+bSwbHn8a2jNky8rGsofA
    o9DOT+0ALPhk6CApCYIP2yxoI/kT188eqUUxzAFF9nyU79AyCkpGPuY8DGMyf56pDofgtGpfY3wQIDAQABoEUwQwYJKoZIhvcNAQkOMTYwNDAyBgNVHREEKzAphhh1cm46T3BjUGxjOm9wY3BsYy0xMjM0NTaCDW9wY3BsYy0xMjM0NTYwDQYJK
    oZIhvcNAQELBQADggEBAAsZLoOLzS2VhDcQRu0QhRbG7CGAxX19l7fDCG2WjU7lTFnCvYVTWTYyaY61ljmrWc7IbCaQdMJM8GRnAnvAzUh/PBDxkOX7NqI2+8F1yQOHgs/AfKuppOd6DIP8EzFAHnc0H85jay6zFdmIDWoWwpy0ACqOVooOTKST
    7uty0mT87bj8Cdy1yf4mvBNQx+nsuTbKgxWCBxGYAyg9dIL2uKL0aeB/ROW5Gkelz5sCEzQ1fFDokUA4oC5QiATQBN3cY7EmvRbPgdToY7CpRN3iiO7J+7bC7BP9YKfuE34E8xOFpskHPHAPf3r002/L0S67HyuVSXLUj1+Jc0LeAAF9Bw0=
    opcplc-123456 | [20:51:32 INF] ---------------------------------------------------------------------------
    ```
    
1. Git [OPC kasa Web sitesi](https://opcvault.azurewebsites.net/).

1. `Register New` seçeneğini belirleyin

1. Günlük çıkışları OPC PLC bilgi girin `CreateSigningRequest information` giriş alanlarına alan `Register New OPC UA Application` sayfasında `Server` ApplicationType olarak.

1. `Register` öğesini seçin.

1. Sonraki sayfada `Request New Certificate for OPC UA Application` seçin `Request new Certificate with Signing Request`.

1. Sonraki sayfada `Generate a new Certificate with a Signing Request` yapıştırın `CSR (base64 encoded)` günlük çıkış dizeden `CreateRequest` giriş alanı. Tam dize kopyalama emin olun.

1. `Generate New Certificate` öğesini seçin.

1. Artık ileriye doğru geçiş yapıyorsanız `View Certificate Request Details`. Bu sayfada sertifika depoları sağlamak için gerekli tüm bilgileri indirebilirsiniz `opc-plc`.

1. Bu sayfada:  
    - Seçin `Certificate` içinde `Download as Base64` kopyalayıp metin dizesi sunulan içinde `EncodedBase64` alan ve daha sonra kullanmak üzere saklayın. Olarak diyoruz `<applicationcertbase64-string>` daha sonra. `Back` öğesini seçin.

    - Seçin `Issuer` içinde `Download as Base64` kopyalayıp metin dizesi sunulan içinde `EncodedBase64` alan ve daha sonra kullanmak üzere saklayın. Olarak diyoruz `<addissuercertbase64-string>` daha sonra. `Back` öğesini seçin.

    - Seçin `Crl` içinde `Download as Base64` kopyalayıp metin dizesi sunulan içinde `EncodedBase64` alan ve daha sonra kullanmak üzere saklayın. Olarak diyoruz `<updatecrlbase64-string>` daha sonra. `Back` öğesini seçin.

1. Şimdi, PowerShell'de adlı bir değişken ayarlamak `$env:_PLC_OPT`:

    ```
    `$env:_PLC_OPT="--applicationcertbase64 <applicationcertbase64-string> --addtrustedcertbase64 <addissuercertbase64-string> --addissuercertbase64 <addissuercertbase64-string> --updatecrlbase64 <updatecrlbase64-string>"`  
    ```
    > [!NOTE] 
    > Web sitesinden alınan seçeneği değerleri Base64 dizesi olarak geçilen dizelerin değiştirin.

Sürümünden itibaren tam bu işlemi yinelemeniz `Register New` (Yukarıdaki 3. adım) OPC istemcisi. Yalnızca, farkında olmanız gereken aşağıdaki farklılıklar vardır:

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

**Doğrulama** iki bileşenden artık uygulama otomatik olarak imzalanan sertifikalar olduğunu doğrulayın. Aşağıdaki çıktı için günlük çıktısını denetleyin:

```
opcplc-123456 | [20:54:38 INF] Starting to add certificate(s) to the trusted issuer store.
opcplc-123456 | [20:54:38 INF] Certificate 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' and thumbprint 'BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83' was added to the trusted issuer store.
opcplc-123456 | [20:54:38 INF] Starting to add certificate(s) to the trusted peer store.
opcplc-123456 | [20:54:38 INF] Certificate 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' and thumbprint 'BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83' was added to the trusted peer store.
opcplc-123456 | [20:54:38 INF] Starting to update the current CRL.
opcplc-123456 | [20:54:38 INF] Remove the current CRL from the trusted peer store.
opcplc-123456 | [20:54:38 INF] The new CRL issued by 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' was added to the trusted peer store.
opcplc-123456 | [20:54:38 INF] Remove the current CRL from the trusted issuer store.
opcplc-123456 | [20:54:38 INF] The new CRL issued by 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' was added to the trusted issuer store.
opcplc-123456 | [20:54:38 INF] Start updating the current application certificate.
opcplc-123456 | [20:54:38 INF] The current application certificate has SubjectName 'CN=OpcPlc' and thumbprint '8FD43F66479398BDA3AAF5B193199A6657632B49'.
opcplc-123456 | [20:54:39 INF] Remove the existing application certificate with thumbprint '8FD43F66479398BDA3AAF5B193199A6657632B49'.
opcplc-123456 | [20:54:39 INF] The new application certificate 'CN=OpcPlc' and thumbprint 'DA6B8B2FB533FBC188F7017BAA8A36FDB77E2586' was added to the application certificate store.
opcplc-123456 | [20:54:39 INF] Activating the new application certificate with thumbprint 'DA6B8B2FB533FBC188F7017BAA8A36FDB77E2586'.
opcplc-123456 | [20:54:39 INF] Application certificate with thumbprint 'DA6B8B2FB533FBC188F7017BAA8A36FDB77E2586' found in the application certificate store.
opcplc-123456 | [20:54:39 INF] Application certificate is for ApplicationUri 'urn:OpcPlc:opcplc-123456', ApplicationName 'OpcPlc' and Subject is 'OpcPlc'
 ```
Uygulama sertifikasını yoktur ve CA tarafından imzalanmış.

Oturum açma artık yüklü sertifikalar doğrulayın. Günlük çıktısını OPC PLC aşağıda verilmiştir ve OPC istemci benzer bir çıktıya sahiptir.
```
opcplc-123456 | [20:54:39 INF] Trusted issuer store contains 1 certs
opcplc-123456 | [20:54:39 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [20:54:39 INF] Trusted issuer store has 1 CRLs.
opcplc-123456 | [20:54:39 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [20:54:39 INF] Trusted peer store contains 1 certs
opcplc-123456 | [20:54:39 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [20:54:39 INF] Trusted peer store has 1 CRLs.
opcplc-123456 | [20:54:39 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [20:54:39 INF] Rejected certificate store contains 0 certs
```
Uygulama sertifikasını veren CA'dır `CN=Azure IoT OPC Vault CA, O=Microsoft Corp.` ve OPC PLC güven de bu CA tarafından imzalanmış tüm sertifikalar.


OPC PLC bağlantısı başarıyla oluşturuldu ve OPC istemci OPC PLC verileri okuyabilir doğrulayın. Çıkış oturum OPC istemci aşağıdaki çıktı görmeniz gerekir:
```
opcclient-123456 | [20:54:42 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [20:54:42 INF] Session successfully created with Id ns=3;i=1085867946.
opcclient-123456 | [20:54:42 INF] The session to endpoint 'opc.tcp://opcplc-123456:50000/' has 4 entries in its namespace array:
opcclient-123456 | [20:54:42 INF] Namespace index 0: http://opcfoundation.org/UA/
opcclient-123456 | [20:54:42 INF] Namespace index 1: urn:OpcPlc:opcplc-123456
opcclient-123456 | [20:54:42 INF] Namespace index 2: http://microsoft.com/Opc/OpcPlc/
opcclient-123456 | [20:54:42 INF] Namespace index 3: http://opcfoundation.org/UA/Diagnostics
opcclient-123456 | [20:54:42 INF] The server on endpoint 'opc.tcp://opcplc-123456:50000/' supports a minimal sampling interval of 0 ms.
opcclient-123456 | [20:54:42 INF] Execute 'OpcClient.OpcTestAction' action on node 'i=2258' on endpoint 'opc.tcp://opcplc-123456:50000/' with security.
opcclient-123456 | [20:54:42 INF] Action (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258') completed successfully
opcclient-123456 | [20:54:42 INF] Value (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258'): 10/20/2018 20:54:42
```
Şu çıktıyı görürsünüz, OPC PLC OPC istemci güvenen artık olur ve bunun tersi de geçerlidir her ikisi de sahip olduğu artık CA tarafından otomatik olarak imzalanan sertifikalar ve hem güven hangi nerede sertifikaları bu CA tarafından imzalanmış.

> [!NOTE] 
> Biz yalnızca OPC PLC için ilk iki doğrulama adımlarını gösteriyordu ancak olanlar için OPC istemci de doğrulanması gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Mevcut bir projeyi OPC kasa dağıtmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Mevcut bir projeyi OPC İkizi dağıtma](howto-opc-twin-deploy-existing.md)