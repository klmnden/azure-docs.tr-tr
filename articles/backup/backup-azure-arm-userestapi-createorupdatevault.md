---
title: 'Azure yedekleme: REST API kullanarak bir kurtarma Hizmetleri kasası oluşturma'
description: Yedeklemeyi yönetme ve geri yükleme işlemleri, Azure sanal makine REST API kullanarak yedekleme
services: backup
author: pvrk
manager: shivamg
keywords: REST API; Azure VM yedeklemesi; Azure VM geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: pullabhk
ms.assetid: e54750b4-4518-4262-8f23-ca2f0c7c0439
ms.openlocfilehash: 7d1a4e6b1093344d1217e8577a56f34cd3c1f52c
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289704"
---
# <a name="create-azure-recovery-services-vault-using-rest-api"></a>REST API kullanarak Azure kurtarma Hizmetleri kasası oluşturma

REST API kullanarak bir Azure kurtarma Hizmetleri kasası oluşturmak için adımları özetlenen [kasası REST API oluşturma](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate) belgeleri. Bize bu belgenin "testVault" adlı "Batı ABD" bir kasa oluşturmak için bir başvuru olarak kullanın.

Azure kurtarma Hizmetleri kasası oluşturma veya güncelleştirme için aşağıdakileri kullanın *PUT* işlemi.

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}?api-version=2016-06-01
```

## <a name="create-a-request"></a>İstek oluştur

Oluşturulacak *PUT* isteği `{subscription-id}` parametresi gereklidir. Birden fazla aboneliğiniz varsa, bkz. [birden çok abonelik ile çalışma](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions). Tanımladığınız bir `{resourceGroupName}` ve `{vaultName}` kaynaklarınız için birlikte `api-version` parametresi. Bu makalede `api-version=2016-06-01`.

Aşağıdaki üst bilgiler gereklidir:

| İstek üstbilgisi   | Açıklama |
|------------------|-----------------|
| *İçerik türü:*  | Gereklidir. Kümesine `application/json`. |
| *Yetkilendirme:* | Gereklidir. Geçerli bir kümesi `Bearer` [erişim belirteci](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

İstek oluşturma hakkında daha fazla bilgi için bkz. [bir REST API istek/yanıt bileşenleri](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>İstek gövdesi oluşturma

Aşağıdaki ortak tanımları, istek gövdesi oluşturmak için kullanılır:

|Ad  |Gerekli  |Tür  |Açıklama  |
|---------|---------|---------|---------|
|eTag     |         |   Dize      |  İsteğe bağlı bir eTag       |
|location     |  true       |Dize         |   Kaynak konumu      |
|properties     |         | [VaultProperties](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vaultproperties)        |  Kasa Özellikleri       |
|sku     |         |  [Sku](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#sku)       |    Her Azure kaynağı için benzersiz sistem tanımlayıcıyı belirtir     |
|etiketler     |         | Nesne        |     Kaynak etiketleri    |

Kasa adı ve kaynak grubu adı PUT URI'SİNDE verildiğini unutmayın. İstek gövdesi konumunu tanımlar.

## <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki örnek gövdesi bir kasada "Batı ABD" oluşturmak için kullanılır. Konumu belirtin. SKU, her zaman "Standart" olur.

```json
{
  "properties": {},
  "sku": {
    "name": "Standard"
  },
  "location": "West US"
}
```

## <a name="responses"></a>Yanıtlar

Bir kurtarma Hizmetleri kasası oluşturma veya güncelleştirme işlemi iki başarılı yanıtlar vardır:

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |   [Kasa](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)      | Tamam        |
|201 oluşturuldu     | [Kasa](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)        |   Oluşturulan      |

REST API yanıtları hakkında daha fazla bilgi için bkz: [yanıt iletisini işlemek](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Örnek yanıt

Sıkıştırılmış bir *201 oluşturuldu* gösterir önceki örnek istekten gelen yanıt gövdesi bir *kimliği* atanmış olan ve *provisioningState* olduğu *başarılı oldu* :

```json
{
  "location": "westus",
  "name": "testVault",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "id": "/subscriptions/77777777-b0c6-47a2-b37c-d8e65a629c18/resourceGroups/Default-RecoveryServices-ResourceGroup/providers/Microsoft.RecoveryServices/vaults/testVault",
  "type": "Microsoft.RecoveryServices/vaults",
  "sku": {
    "name": "Standard"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Bu kasada Azure VM yedekleme için bir yedekleme ilkesi oluşturma](backup-azure-arm-userestapi-createorupdatepolicy.md).

Azure REST API'leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure kurtarma Hizmetleri Sağlayıcısı REST API'si](/rest/api/recoveryservices/)
- [Azure REST API'si ile çalışmaya başlama](/rest/api/azure/)
