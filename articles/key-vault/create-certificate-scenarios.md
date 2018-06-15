---
title: İzleme ve yönetme sertifika oluşturma
description: Bir dizi oluşturmak için seçenekler gösteren senaryoları, anahtar kasası ile izleme ve sertifika oluşturma ile etkileşim işleyin.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 0d0995aa-b60d-4811-be12-ba0a45390197
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: e1ea77304fa59b67e0e28a4c7e0b13633eeeff6f
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012215"
---
# <a name="monitor-and-manage-certificate-creation"></a>İzleme ve yönetme sertifika oluşturma
Uygulandığı öğe: Azure  

Aşağıdaki 

Senaryo / bu makalede açıklanan işlemleri:

- Desteklenen bir veren KV sertifika isteme
- Bekleyen isteği Al - istek durumunu "devam ediyor"
- Bekleyen isteği Al - istek durumu "tamamlandı"
- Bekleyen isteği - durum "İptal" veya "başarısız" Bekleyen isteği alma
- Bekleyen isteği - durumu "silinmiş" veya "üzerine" Bekleyen isteği alma
- Oluşturun (veya içeri aktarın) zaman bekleyen istek yok - durum "devam ediyor"
- Birleştirme bekleyen istek olduğunda oluşturulan bir veren (örneğin, DigiCert) ile
- Bekleyen isteği durumu "devam ediyor" iken iptal isteği
- Bekleyen istek nesnesini silme
- KV sertifikayı el ile oluşturma
- Bekleyen isteği oluşturulduktan sonra - el ile sertifika oluşturma birleştirme

## <a name="request-a-kv-certificate-with-a-supported-issuer"></a>Desteklenen bir veren KV sertifika isteme 

|Yöntem|İstek URI'si|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|  

Aşağıdaki örnekler zaten DigiCert veren sağlayıcı anahtar kasasıyla kullanılabilir olması için "mydigicert" adlı bir nesne gerektirir. Verenler ile çalışma hakkında daha fazla bilgi için bkz: [sertifika verenler](/rest/api/keyvault/certificate-issuers.md).  

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

## <a name="get-pending-request---request-status-is-inprogress"></a>Bekleyen isteği Al - istek durumunu "devam ediyor"

|Yöntem|İstek URI'si|  
|------------|-----------------|  
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>İstek  
 AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OR  

 AL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

> [!NOTE]
>  Varsa *request_id* belirtilen sorguda bir filtre gibi davranır. Varsa *request_id* sorgu ve bekleyen nesne farklı bir http durum kodu 404 döndürülür.  

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

## <a name="get-pending-request---request-status-is-complete"></a>Bekleyen isteği Al - istek durumu "tamamlandı"

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

 AL  `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

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
   "error":  
    {  
        "code": "<errorcode>",    
        "message": "<message>"  
    }  
}  

```  

> [!NOTE]
> Değeri *errorcode* veren ya da kullanıcı hataya sırasıyla göre "İsteği reddetti" veya "Sertifika veren hatası" olabilir.  

## <a name="get-pending-request---pending-request-status-is-deleted-or-overwritten"></a>Bekleyen isteği - durumu "silinmiş" veya "üzerine" Bekleyen isteği alma  
 Bekleyen bir nesne silinmiş ya da durumunu "devam ediyor." olmadığında Oluştur/içe aktarma işlemi tarafından üzerine

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
  "error":  
    {  
         "code": "PendingCertificateNotFound",  
        "message": "…"  
    }  
}  

```  

## <a name="create-or-import-when-pending-request-exists---status-is-inprogress"></a>Oluşturun (veya içeri aktarın) zaman bekleyen istek yok - durum "devam ediyor"
 Bekleyen bir nesne dört durumda olabilir; "" İptal"devam ediyor", "başarısız" veya "tamamlandı."

 Bekleyen bir isteğin durumu "devam ediyor" olduğunda, oluşturma (ve içeri aktarma) işlemleri 409 (Çakışma) bir http durum kodu ile başarısız olur.  

 Bir çakışmayı gidermek için:  

 - Sertifikayı el ile oluşturulduysa, bir birleştirme yaparak KV sertifika tamamlamak veya bekleyen bir nesne üzerinde silin.  

 - Sertifikayı veren ile oluşturulduysa, sertifikayı tamamlar, başarısız veya iptal edilene kadar bekleyebilirsiniz. Alternatif olarak, bekleyen nesne silebilirsiniz.

> [!NOTE]
> Bekleyen bir nesne silme olabilir veya x509 iptal edemez sağlayıcısı ile sertifika isteği.  

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
  "error":  
    {  
        "code": "Forbidden",  
        "message": "A new key vault certificate can not be created or imported while a pending key vault certificate's status is inProgress."  
    }  
}  

```  

## <a name="merge-when-pending-request-is-created-with-an-issuer"></a>Bekleyen isteği ile bir veren oluşturulduğunda birleştirme
 Bekleyen bir nesne ile bir veren oluşturulur ancak durumu "devam ediyor." olduğunda izin birleştirme izin verilmiyor 

 Varsa x509 oluşturma isteği sertifika başarısız olduğunda veya herhangi bir nedenden dolayı iptal eder ve KV sertifika tamamlamak için sertifika bant dışı yollarla alınabilir x509, bir birleştirme işlemi gerçekleştirilebilir.  

|Yöntem|İstek URI'si|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|  

### <a name="request"></a>İstek  

```json
{  
  "x5c": [  "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8="  
  ]  
}  

```  

### <a name="response"></a>Yanıt  

```json
StatusCode: 403, ReasonPhrase: 'Forbidden'  
{  
  "error":  
    {  
       "code": "Forbidden",  
       "message": "Merge is forbidden on pending object created with issuer : <issuer-name> while it is in progess."  
    }  
}  

```  

## <a name="request-a-cancellation-while-the-pending-request-status-is-inprogress"></a>Bekleyen isteği durumu "devam ediyor" iken iptal isteği  
 İptal yalnızca istenebilir.  Bir istek olabilir veya iptal. Bir istek "devam ediyor" değilse, http durum 400 (Hatalı istek) döndürülür.  

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
> Bekleyen nesne silme olabilir veya x509 iptal edemez sağlayıcısı ile sertifika isteği.  

|Yöntem|İstek URI'si|  
|------------|-----------------|  
|DELETE|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>İstek  
 SİL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OR  

 SİL `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

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
 El ile oluşturma işlemiyle tercih ettiğiniz bir CA ile verilen bir sertifika oluşturabilirsiniz. Verenin adı "Bilinmeyen" olarak ayarlayın veya veren alanı belirtmeyin.  

|Yöntem|İstek URI'si|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|  

### <a name="request"></a>İstek  

```json
{  
  "policy": {  
    "x509_props": {  
      "subject": "CN=MyCertSubject1"  
    }  
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

## <a name="merge-when-a-pending-request-is-created---manual-certificate-creation"></a>Bekleyen isteği oluşturulduktan sonra - el ile sertifika oluşturma birleştirme  

|Yöntem|İstek URI'si|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|  

### <a name="request"></a>İstek  

```json
{  
  "x5c": [  "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8="  
  ]  
}  

```  

|Öğe adı|Gerekli|Tür|Sürüm|Açıklama|  
|------------------|--------------|----------|-------------|-----------------|  
|x5c|Evet|array|\<Sürüm Giriş >|X509 temel 64 dize dizisi olarak sertifika zinciri.|  

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
                       "validity_months":12  
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
