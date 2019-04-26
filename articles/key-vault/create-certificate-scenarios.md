---
title: Sertifika oluşturmayı izleme ve yönetme
description: Çeşitli oluşturmaya yönelik seçenekleri gösteren senaryoları, Key Vault ile izleme ve sertifika oluşturma ile etkileşim kurma işlemi.
services: key-vault
author: msmbaldwin
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 854d0e8f6927c9ce4855435a02b4819055111ceb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306028"
---
# <a name="monitor-and-manage-certificate-creation"></a>Sertifika oluşturmayı izleme ve yönetme
Uygulama hedefi: Azure

Aşağıdaki 

Senaryo / şu makalede açıklanan işlemleri:

- Desteklenen bir veren ile KV sertifika isteme
- İstek Al - istek durumu "Sürüyor"
- İstek Al - istek durumu "tamamlandı"
- Bekleyen isteği - durum "İptal" veya "başarısız" Bekleyen isteği alma
- Bekleyen isteği - durumu "silinmiş" veya "üzerine" Bekleyen isteği alma
- Oluşturun (veya içeri aktarın) zaman bekleyen istek yok - durumu "Sürüyor"
- Birleştirme isteği olduğunda oluşturulan bir veren (örneğin, DigiCert) ile
- Bekleyen istek durumu "Sürüyor" olsa da, bir iptal isteğinde
- Bekleyen istek nesnesini silme
- KV sertifikayı el ile oluşturma
- Bekleyen istek oluşturulduktan sonra - el ile sertifika oluşturma Birleştir

## <a name="request-a-kv-certificate-with-a-supported-issuer"></a>Desteklenen bir veren ile KV sertifika isteme 

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|

Aşağıdaki örnekler, zaten DigiCert olarak veren sağlayıcısı ile anahtar kasanızın kullanılabilir olması için "mydigicert" adlı bir nesne gerektirir. Sertifikayı veren süresi kaynak olarak Azure anahtar kasası (KV) olarak temsil edilen bir varlıktır. KV sertifikanın kaynağı hakkında bilgi sağlamak için kullanılır; Verenin adı, sağlayıcı, kimlik bilgilerini ve diğer yönetimsel ayrıntıları.

### <a name="request"></a>İstek

```json
{
  "policy": {
    "x509_props": {
      "subject": "CN=MyCertSubject1"
    },
    "issuer": {
      "name": "mydigicert",
      "cty": "OV-SSL",
    }
  }
}
```

### <a name="response"></a>Yanıt

```
StatusCode: 202, ReasonPhrase: 'Accepted'
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "mydigicert"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "InProgress",
  "status_details": "Pending certificate created. Certificate request is in progress. This may take some time based on the issuer provider. Please check again later",
  "request_id": "a76827a18b63421c917da80f28e9913d"
}

```

## <a name="get-pending-request---request-status-is-inprogress"></a>İstek Al - istek durumu "Sürüyor"

|Yöntem|İstek URI'si|
|------------|-----------------|
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

### <a name="request"></a>İstek
AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

> [!NOTE]
> Varsa *request_id* belirtilirse sorgu, filtre gibi davranır. Varsa *request_id* sorgu ve bekleyen nesnesindeki farklıdır, 404 bir http durum kodu döndürülür.

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "inProgress",
  "status_details": "…",
  "request_id": "a76827a18b63421c917da80f28e9913d"
}

```

## <a name="get-pending-request---request-status-is-complete"></a>İstek Al - istek durumu "tamamlandı"

### <a name="request"></a>İstek

|Yöntem|İstek URI'si|
|------------|-----------------|
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "completed",
  "request_id": "a76827a18b63421c917da80f28e9913d",
  "target": “https://mykeyvault.vault.azure.net/certificates/mycert1?api-version={api-version}"
}

```

## <a name="get-pending-request---pending-request-status-is-canceled-or-failed"></a>Bekleyen isteği - durum "İptal" veya "başarısız" Bekleyen isteği alma

### <a name="request"></a>İstek

|Yöntem|İstek URI'si|
|------------|-----------------|
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "failed",
  "status_details": "",
  "request_id": "a76827a18b63421c917da80f28e9913d",
  "error": {
    "code": "<errorcode>",
    "message": "<message>"
  }
}

```

> [!NOTE]
> Değerini *errorcode* veren ya da kullanıcı hataya sırasıyla göre "İstek reddedildi" veya "Sertifika veren hatası" olabilir.

## <a name="get-pending-request---pending-request-status-is-deleted-or-overwritten"></a>Bekleyen isteği - durumu "silinmiş" veya "üzerine" Bekleyen isteği alma
Bekleyen bir nesne silinmiş ya da durumu "Sürüyor" olmadığında bir Oluştur/içeri aktarma işlemiyle üzerine

|Yöntem|İstek URI'si|
|------------|-----------------|
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

### <a name="request"></a>İstek
AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

### <a name="response"></a>Yanıt

```
StatusCode: 404, ReasonPhrase: 'Not Found'
{
  "error": {
    "code": "PendingCertificateNotFound",
    "message": "…"
  }
}

```

## <a name="create-or-import-when-pending-request-exists---status-is-inprogress"></a>Oluşturun (veya içeri aktarın) zaman bekleyen istek yok - durumu "Sürüyor"
Bekleyen bir nesne dört durumda olabilir; "Sürüyor"İptal",", "başarısız" veya "tamamlandı"

Bekleyen bir isteğin durumu "Sürüyor" olduğunda oluşturmak (ve içeri aktarma) işlemleri, 409 (Çakışma) bir http durum kodu ile başarısız olacaktır.

Bir çakışmayı düzeltmeniz istenir:

- Sertifikayı el ile oluşturuluyorsa, bir birleştirme yaparak KV sertifika tamamlamak veya bekleyen nesneye silin.

- Sertifika bir veren ile oluşturuluyorsa, sertifika tamamlandıktan, başarısız veya iptal edilene kadar bekleyebilirsiniz. Alternatif olarak, bekleyen nesne silebilirsiniz.

> [!NOTE]
> Bekleyen bir nesneyi silme olabilir veya x509 iptal edemez sağlayıcısı ile sertifika isteği.

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|

### <a name="request"></a>İstek

```json
{
  "policy": {
    "x509_props": {
      "subject": "CN=MyCertSubject1"
    },
    "issuer": {
      "name": "mydigicert"
    }
  }
}
```

### <a name="response"></a>Yanıt

```
StatusCode: 409, ReasonPhrase: 'Conflict'
{
  "error": {
    "code": "Forbidden",
    "message": "A new key vault certificate can not be created or imported while a pending key vault certificate's status is inProgress."
  }
}

```

## <a name="merge-when-pending-request-is-created-with-an-issuer"></a>Bekleyen isteği ile bir veren oluşturulduğunda Birleştir
Birleştirme bekleyen bir nesne ile bir veren oluşturulur, ancak "devam ediyor" durumuna olduğunda izin verilmez 

Varsa x509 oluşturma isteği sertifika başarısız olursa veya herhangi bir nedenle iptal eder ve KV sertifika tamamlamak için sertifika, bant dışı yollarla alınabilir x x509, bir birleştirme işlemi gerçekleştirilebilir.

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|

### <a name="request"></a>İstek

```json
{
  "x5c": [ "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8=" ]
}

```

### <a name="response"></a>Yanıt

```json
StatusCode: 403, ReasonPhrase: 'Forbidden'
{
  "error": {
    "code": "Forbidden",
    "message": "Merge is forbidden on pending object created with issuer : <issuer-name> while it is in progess."
  }
}

```

## <a name="request-a-cancellation-while-the-pending-request-status-is-inprogress"></a>Bekleyen istek durumu "Sürüyor" olsa da, bir iptal isteğinde
İptal durumunda yalnızca istenebilir. Bir isteğin olabilir ya da iptal. Bir istek "Sürüyor" değil, bir http durumu 400 (Hatalı istek) döndürülür.

|Yöntem|İstek URI'si|
|------------|-----------------|
|PATCH|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

### <a name="request"></a>İstek
DÜZELTME EKİ `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

DÜZELTME EKİ `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

```json
{
  "cancellation_requested": true
}

```

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": true,
  "status": "inProgress",
  "status_details": "…",
  "request_id": "a76827a18b63421c917da80f28e9913d"
}
```

## <a name="delete-a-pending-request-object"></a>Bekleyen istek nesnesini silme

> [!NOTE]
> Bekleyen nesne silinirken olabilir veya x509 iptal edemez sağlayıcısı ile sertifika isteği.

|Yöntem|İstek URI'si|
|------------|-----------------|
|DELETE|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

### <a name="request"></a>İstek
DELETE `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

DELETE `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "inProgress",
  "request_id": "a76827a18b63421c917da80f28e9913d",
}
```

## <a name="create-a-kv-certificate-manually"></a>KV sertifikayı el ile oluşturma
El ile oluşturma sürecinde, tercih ettiğiniz bir CA bir sertifika oluşturabilirsiniz. Verenin adı "Bilinmeyen" olarak ayarlayın veya veren alanıyla belirtmeyin.

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|

### <a name="request"></a>İstek

```json
{
  "policy": {
    "x509_props": {
      "subject": "CN=MyCertSubject1"
    },
    "issuer": {
      "name": "Unknown"
    }
  }
}

```

### <a name="response"></a>Yanıt

```
StatusCode: 202, ReasonPhrase: 'Accepted'
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "Unknown"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "status": "inProgress",
  "status_details": "Pending certificate created. Please Perform Merge to complete the request.",
  "request_id": "a76827a18b63421c917da80f28e9913d"
}

```

## <a name="merge-when-a-pending-request-is-created---manual-certificate-creation"></a>Bekleyen istek oluşturulduktan sonra - el ile sertifika oluşturma Birleştir

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|

### <a name="request"></a>İstek

```json
{
  "x5c": [ "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8=" ]
}

```

|Öğe adı|Gerekli|Tür|Version|Açıklama|
|------------------|--------------|----------|-------------|-----------------|
|x5c|Evet|array|\<Karşınızda sürüm >|X509 taban 64 dize dizisi olarak sertifika zinciri.|

### <a name="response"></a>Yanıt

```
StatusCode: 201, ReasonPhrase: 'Created'
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1?api-version={api-version}"
{
    "id": "https mykeyvault.vault.azure.net/certificates/mycert1/f366e1a9dd774288ad84a45a5f620352",
    "kid": "https:// mykeyvault.vault.azure.net/keys/mycert1/f366e1a9dd774288ad84a45a5f620352",
    "sid": " mykeyvault.vault.azure.net/secrets/mycert1/f366e1a9dd774288ad84a45a5f620352",
    "cer": "……de34534……",
    "x5t": "n14q2wbvyXr71Pcb58NivuiwJKk",
    "attributes": {
        "enabled": true,
        "exp": 1530394215,
        "nbf": 1435699215,
        "created": 1435699919,
        "updated": 1435699919
    },
    "pending": {
        "id": "https:// mykeyvault.vault.azure.net/certificates/mycert1/pending"
    },
    "policy": {
        "id": "https:// mykeyvault.vault.azure.net/certificates/mycert1/policy",
        "key_props": {
            "exportable": false,
            "kty": "RSA",
            "key_size": 2048,
            "reuse_key": false
        },
        "secret_props": {
            "contentType": "application/x-pkcs12"
        },
        "x509_props": {
            "subject": "CN=Mycert1",
            "ekus": ["1.3.6.1.5.5.7.3.1", "1.3.6.1.5.5.7.3.2"],
            "validity_months": 12
        },
        "lifetime_actions": [{
            "trigger": {
                "lifetime_percentage": 80
            },
            "action": {
                "action_type": "EmailContacts"
            }
        }],
        "issuer": {
            "name": "Unknown"
        },
        "attributes": {
            "enabled": true,
            "created": 1435699811,
            "updated": 1435699811
        }
    }
}

```

## <a name="see-also"></a>Ayrıca Bkz.
- [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
