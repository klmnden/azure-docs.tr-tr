---
title: 'Azure yedekleme: REST API kullanarak yedekleme ilkeleri oluşturma'
description: (Zamanlama ve bekletme) yedekleme ilkelerini yönetme REST API'sini kullanma
services: backup
author: pvrk
manager: shivamg
keywords: REST API; Azure VM yedeklemesi; Azure VM geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: pullabhk
ms.assetid: 5ffc4115-0ae5-4b85-a18c-8a942f6d4870
ms.openlocfilehash: 657a777da0e984a145c1c617a6194bf4ef56306e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60648814"
---
# <a name="create-azure-recovery-services-backup-policies-using-rest-api"></a>REST API kullanarak Azure kurtarma Hizmetleri yedekleme ilkeleri oluşturma

Azure kurtarma Hizmetleri kasası için bir yedekleme ilkesi oluşturmak için adımları özetlenen [ilke REST API belge](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate). Bize bu belge, Azure VM yedeklemesi için bir ilke oluşturmak için başvuru olarak kullanın.

## <a name="backup-policy-essentials"></a>Yedekleme İlkesi temelleri

- Bir yedekleme İlkesi, kasa oluşturulur.
- Aşağıdaki iş yüklerinin yedeklenmesi için bir yedekleme İlkesi oluşturulabilir
  - Azure VM
  - Azure VM'de SQL
  - Azure Dosya Paylaşımı
- Bir ilke birçok kaynağa atanabilir. Bir Azure VM yedekleme İlkesi, birçok Azure Vm'leri korumak için kullanılabilir.
- Bir ilke iki bileşenden oluşur.
  - Zamanlaması: Ne zaman yedekleyin
  - Saklama: Her yedekleme için ne kadar süre tutulacağını.
- Zamanlama, "Günlük" veya "haftalık" ile belirli bir zaman noktası olarak tanımlanabilir.
- Bekletme tanımlanabilir "Günlük", "haftalık", "aylık", "yıllık" Yedekleme noktaları için.
- "haftalık" bir yedekleme için haftanın belirli bir günde başvuruyor, "aylık" bir yedekleme bir ayın günü gösterir ve "yıllık" bir yedekleme için belirli bir yılın günü başvuruyor.
- "Aylık", "yıllık" Yedekleme noktaları için bekletme "LongTermRetention" adlandırılır.
- Bir kasa oluşturulur, "DefaultPolicy" adlı bir Azure VM yedeklemeleri için bir ilke de oluşturulur ve Azure Vm'leri için yedekleme kullanılabilir.

Oluşturun veya bir Azure yedekleme ilkesinin güncelleştirmek için aşağıdakileri kullanın *PUT* işlemi

```http
PUT https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupPolicies/{policyName}?api-version=2016-12-01
```

`{policyName}` Ve `{vaultName}` URI'de sağlanır. İstek gövdesinde ek bilgiler sağlanmıştır.

## <a name="create-the-request-body"></a>İstek gövdesi oluşturma

Örneğin, Azure VM yedeklemesi için bir ilke oluşturmak için istek gövdesi bileşenlerinin aşağıda verilmiştir.

|Ad  |Gerekli  |Tür  |Açıklama  |
|---------|---------|---------|---------|
|properties     |   True      |  ProtectionPolicy:[AzureIaaSVMProtectionPolicy](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate#azureiaasvmprotectionpolicy)      | ProtectionPolicyResource özellikleri        |
|etiketler     |         | Object        |  Kaynak etiketleri       |

İstek gövdesi tanımlarında tam listesi için başvurmak [yedekleme İlkesi REST API belge](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate).

### <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki istek gövdesi, Azure VM yedeklemeleri için yedekleme İlkesi tanımlar.

İlke sonucu:

- Her Pazartesi, Çarşamba, Perşembe saat 10: 00'da haftalık yedekleyin Pasifik Standart Saati.
- Bir hafta boyunca her Pazartesi, Çarşamba, Perşembe alınan yedeklemeler korur.
- Her ilk Çarşamba ve iki ay (geçersiz kılmaları önceki bekletme koşullar, eğer varsa) için ayın üçüncü Salı alınan yedeklemeler korur.
- Dördüncü Pazartesi ve dördüncü Perşembe Şubat ve Kasım üzerindeki dört yıl (geçersiz kılmaları önceki bekletme koşullar, eğer varsa) için gerçekleştirilen yedeklemeleri korur.

```json
{
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "timeZone": "Pacific Standard Time",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ]
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    }
  }
}
```

> [!IMPORTANT]
> Zamanlama ve bekletme için saat biçimleri yalnızca DateTime destekler. Saat biçimi tek başına desteklemez.

## <a name="responses"></a>Yanıtlar

Yedekleme ilkesi oluşturma/güncelleştirme bir [zaman uyumsuz işlem](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Bu işlem, ayrı ayrı izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt döndürür: 202 (kabul edildi başka bir işlem oluşturulurken) ve ardından 200 (Tamam) Bu işlem tamamlandığında.

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |    [Koruma PolicyResource](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate#protectionpolicyresource)     |  Tamam       |
|202 kabul edildi     |         |     Kabul Edildi    |

### <a name="example-responses"></a>Örnek yanıt

Gönderdiğiniz sonra *PUT* istemek için ilke oluşturma veya güncelleştirme, ilk yanıt, 202 (kabul edildi). bir konum üst bilgisi ya da Azure async başlığı.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operations/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
X-Powered-By: ASP.NET
```

Ardından ile basit bir konum üst bilgisi veya Azure-AsyncOperation başlığı kullanılarak elde edilen işlemi izlemek *alma* komutu.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
```

İşlem tamamlandığında, yanıt gövdesindeki ilke içerikle 200 (Tamam) döndürür.

```json
{
  "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1",
  "name": "testPolicy1",
  "type": "Microsoft.RecoveryServices/vaults/backupPolicies",
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ],
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleWeeklyFrequency": 0
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    },
    "timeZone": "Pacific Standard Time",
    "protectedItemsCount": 0
  }
}
```

Bir ilke zaten bir öğe korumak için kullanılıyorsa, herhangi bir güncelleştirme ilkesi sonuçlanır [koruma değiştirme](backup-azure-arm-userestapi-backupazurevms.md#changing-the-policy-of-protection) ilişkili tüm bu öğeleri için.

## <a name="next-steps"></a>Sonraki adımlar

[Korumasız bir Azure VM için korumayı etkinleştirin](backup-azure-arm-userestapi-backupazurevms.md).

Azure Backup REST API'leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure kurtarma Hizmetleri Sağlayıcısı REST API'si](/rest/api/recoveryservices/)
- [Azure REST API'si ile çalışmaya başlama](/rest/api/azure/)