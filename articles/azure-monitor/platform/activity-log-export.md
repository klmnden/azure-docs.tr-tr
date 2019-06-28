---
title: Azure etkinlik günlüğü dışarı aktarma
description: Azure etkinlik günlüğü depolama, arşivleme veya Azure Event Hubs için Azure dışında dışa aktarmak için verin.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: acf2526e79519e610614dc5217efbfe5e327b90f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66248152"
---
# <a name="export-azure-activity-log-to-storage-or-azure-event-hubs"></a>Depolama veya Azure Event Hubs, Azure etkinlik günlüğü dışarı aktarma
[Azure etkinlik günlüğü](activity-logs-overview.md) Azure aboneliğinizde gerçekleşen abonelik düzeyindeki olayların hakkında Öngörüler sağlar. Azure portalında Etkinlik günlüğünü görüntüleme veya burada analiz Azure İzleyici tarafından toplanan diğer verilerle bir Log Analytics çalışma alanına kopyalama yanı sıra, Azure depolama hesabınız için Etkinlik günlüğünü arşivleme veya kendisine akış için bir günlük profili oluşturabilirsiniz. bir  Olay hub'ı.

## <a name="archive-activity-log"></a>Etkinlik günlüğünü arşivleme
Etkinlik günlüğü bir depolama hesabına arşivleme, günlük verilerinizi 90 günden uzun (ile bekletme ilkesini üzerinde tam denetim) denetim, statik analiz veya yedekleme korumak istiyorsanız kullanışlıdır. Yalnızca olaylarınızı 90 gün boyunca Beklet gerekir ya da daha az, etkinlik günlüğü olayları Azure platformunda 90 gün boyunca saklanır olduğundan bir depolama hesabına arşivleme ayarlamak ihtiyacınız yoktur.

## <a name="stream-activity-log-to-event-hub"></a>Olay hub'ına Stream etkinlik günlüğü
[Azure Event Hubs](/azure/event-hubs/) bir veri akışı platformu ve olay alma hizmetidir alabilir ve saniyede milyonlarca işlem. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Etkinlik günlüğü için akış özelliği kullanabileceğinize iki yöntemdir:
* **Üçüncü taraf günlüğe kaydetme ve telemetri sistemleriyle Stream**: Zamanla, Azure Event Hubs akış etkinlik günlüğünüzü üçüncü taraf Sıem'lerden kanal oluşturarak ve analiz çözümleri oturum için bir mekanizma olur.
* **Özel telemetri ve günlüğe kaydetme platform derleme**: Zaten bir ürettikleri telemetri platformu varsa veya biri oluşturma hakkında düşünmeye olan yüksek düzeyde ölçeklenebilir Yayımla-doğasını abone ol olay hub'ları esnek etkinlik günlüğü alma olanak sağlar. 

## <a name="prerequisites"></a>Önkoşullar

### <a name="storage-account"></a>Depolama hesabı
Etkinlik günlüğünüzü arşivleme için gerekirse [depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) zaten yoksa. İçinde depolanır, bu sayede daha iyi İzleme verilerine erişimi denetleyebilir, diğer izleme olmayan veriler içeren mevcut bir depolama hesabını kullanmamanız gerekir. Ayrıca Tanılama günlükleri ve ölçümler bir depolama hesabına ancak arşivleme, tüm izleme verilerini merkezi bir konumda tutmak için aynı depolama hesabını kullanmayı tercih edebilirsiniz.

Ayarı yapılandıran kullanıcının her iki abonelik için uygun RBAC erişimine sahip olduğu sürece, günlükleri yayan ile aynı abonelikte olması depolama hesabı yok.
> [!NOTE]
>  Verileri güvenli bir sanal ağda olduğu bir depolama hesabına şu anda arşivlenemiyor.

### <a name="event-hubs"></a>Event Hubs
Etkinlik günlüğünüzü bir olay hub'ına gönderiyorsanız sonra yapmanız [bir olay hub'ı oluşturma](../../event-hubs/event-hubs-create.md) zaten yoksa. Daha önce bu Event Hubs ad alanı için etkinlik günlüğü olayları akış, olay hub'ı yeniden kullanılır.

Paylaşılan Erişim İlkesi akış mekanizması olan izni tanımlar. Event Hubs'a akış Yönet, gönderme ve dinleme izinleri gerektirir. Oluşturun veya Event Hubs ad alanınız için paylaşılan erişim ilkeleri Yapılandır sekmesi altında Azure portalında Event Hubs ad alanı için değiştirin.

Etkinlik günlüğü günlük profilini akış içerecek şekilde güncelleştirmek için olay hub'ları yetkilendirme kuralı ListKey izniniz olmalıdır. Event Hubs ad alanı uygun RBAC ayarı yapılandıran kullanıcının sahip olduğu sürece, günlükleri yayan aboneliğe erişmek için her iki aboneliğin ve her iki aboneliğin aynı AAD kiracısındaki aynı abonelikte olması gerekmez.

Etkinlik günlüğüne bir olay hub'ı Stream [günlük profilini oluşturma](#create-a-log-profile).

## <a name="create-a-log-profile"></a>Günlük profilini oluşturma
Azure etkinlik günlüğü nasıl olduğunu tanımladığınız kullanarak verilen bir **günlük profili**. Her Azure aboneliği yalnızca tek bir günlük profili olabilir. Bu ayarlar aracılığıyla yapılandırılabilir **dışarı** portalda etkinlik günlüğü dikey penceresindeki seçeneği. Bunlar aynı zamanda program aracılığıyla yapılandırılabilir [Azure İzleyici REST API'sini kullanarak](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell cmdlet'leri veya CLI.

Günlük profilini aşağıdaki tanımlar.

**Etkinlik günlüğü burada gönderilmelidir.** Şu anda, depolama hesabına veya olay hub'ları kullanılabilir seçenekler bulunur.

**Hangi olay kategorileri gönderilmelidir.** Anlamını *kategori* günlüğü profillerini ve etkinlik günlüğü olayları farklıdır. Günlük profilinde *kategori* işlem türü (yazma, silme, eylem) temsil eder. Bir etkinlik günlüğü olayında *kategori*"* özelliği kaynak veya olay (örneğin, yönetim, ServiceHealth ve uyarı) türünü temsil eder.

**Hangi bölgeler (konumlara) aktarılması.** Etkinlik günlüğünde çok sayıda olayları genel olaylar olduğundan tüm konumlara içermelidir.

**Ne kadar süreyle etkinlik günlüğü, depolama hesabında tutulmalıdır.** Bekletme günü sayısının sıfır günlükler süresiz olarak tutulur anlamına gelir. Aksi takdirde, değeri herhangi bir sayıda gün 1 ile 2147483647 arasında olabilir.

Bekletme ilkeleri ayarlayın, ancak günlükleri bir depolama hesabında depolama devre dışı, bekletme ilkeleri herhangi bir etkisi vardır. Bekletme ilkeleri uygulanan günlük, olduğundan, bir günün (UTC), şu anda sonra saklama günü günlüklerinden sonunda İlkesi silindi. Örneğin, bir günlük bir bekletme ilkesi olsaydı, bugün günün başında dünden önceki gün kayıtları silinir. Gece yarısı UTC, ancak bu günlükleri depolama hesabınızdan silinecek 24 saate kadar sürebilir not silme işlemi başlar.



> [!WARNING]
> 1 Kasım 2018'de JSON satırlarına değiştirildi depolama hesabında günlük veri biçimi. [Etkinin açıklaması ve yeni biçimi işlemek üzere araçlarınızı güncelleştirme için bu makaleye bakın.](diagnostic-logs-append-blobs.md)
>
>

### <a name="create-log-profile-using-the-azure-portal"></a>Azure portalını kullanarak günlük profilini oluşturma

Bir günlük profili ile oluştururken veya düzenlerken **dışarı aktarma, olay Hub'ına** seçeneği Azure portalında.

1. Gelen **İzleyici** Azure portalında, seçim menüsünde **dışarı aktarma, olay Hub'ına**.

    ![Portal, Dışarı Aktar](media/activity-log-export/portal-export.png)

3. Açılan dikey pencerede, aşağıdakileri belirtin:
   * Dışarı aktarmak için olaylarla bölgeleri. Etkinlik günlüğüne genel bir (Bölgesel olmayanlar) günlük olduğundan ve bunlarla ilişkilendirilmiş bir bölgeye en çok olayın zorunda kalmamanız anahtar olayları kaçırmayın emin olmak için tüm bölgeler seçmeniz gerekir. 
   * Depolama hesabına yazma istiyorsanız:
       * Olayları kaydetmek istediğiniz depolama hesabı.
       * Bu olaylar depolamadaki saklamak istediğiniz gün sayısı. 0 gün ayarı günlükler süresiz korur.
   * Olay hub'ına yazmak istiyorsanız:
       * Bu olayları akış için oluşturulacak bir olay hub'ı istediğiniz hizmet veri yolu Namespace.

     ![Etkinlik günlüğü dikey dışarı aktarma](./media/activity-logs-overview/activity-logs-portal-export-blade.png)


4. Tıklayın **Kaydet** bu ayarları kaydedin. Ayarlar aboneliğinize hemen uygulanır.


### <a name="configure-log-profile-using-powershell"></a>PowerShell kullanarak günlük profili yapılandırma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Günlük profilini zaten varsa önce mevcut günlük profilini kaldırın ve ardından yeni bir tane oluşturmak gerekir.

1. Kullanım `Get-AzLogProfile` günlük profili var olmadığını belirleyin.  Günlük profilini mevcut değilse, Not *adı* özelliği.

1. Kullanım `Remove-AzLogProfile` değerini kullanarak günlük profilini kaldırmak için *adı* özelliği.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzLogProfile -Name "default"
    ```

3. Kullanım `Add-AzLogProfile` yeni bir günlük profili oluşturmak için:

    ```powershell
    Add-AzLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus -RetentionInDays 90 -Category Write,Delete,Action
    ```

    | Özellik | Gerekli | Açıklama |
    | --- | --- | --- |
    | Name |Evet |Günlük profilinin adı. |
    | StorageAccountId |Hayır |Etkinlik günlüğünün kaydedileceği depolama hesabı kaynak kimliği. |
    | serviceBusRuleId |Hayır |Service Bus kural kimliği oluşturulan olay hub'ları olmasını istediğiniz Service Bus ad alanı. Dize biçimiyle budur: `{service bus resource ID}/authorizationrules/{key name}`. |
    | Location |Evet |Etkinlik günlüğü olayları toplamak istiyorsanız bölgelerin virgülle ayrılmış listesi. |
    | Retentionındays |Evet |Olaylar için 1 ile 2147483647 arasında bir depolama hesabında tutulacağını gün sayısı. Sıfır değeri, günlükler süresiz olarak depolar. |
    | Category |Hayır |Virgülle ayrılmış liste toplanması gereken olay kategorileri. Olası değerler _Write_, _Delete_, and _Action_. |

### <a name="example-script"></a>Örnek betik
Etkinlik günlüğü hem de bir depolama hesabı ve olay hub'ına yazan bir günlük profili oluşturmak için örnek PowerShell Betiği verilmiştir.

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespace/authorizationrules/RootManageSharedAccessKey"

   # Build the storage account Id from the settings above
   $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

   Add-AzLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```


### <a name="configure-log-profile-using-azure-cli"></a>Azure CLI'yı kullanarak günlük profili yapılandırma

Günlük profilini zaten varsa önce mevcut günlük profilini kaldırın ve ardından yeni bir günlük profili oluşturmak gerekir.

1. Kullanım `az monitor log-profiles list` günlük profili var olmadığını belirleyin.
2. Kullanım `az monitor log-profiles delete --name "<log profile name>` değerini kullanarak günlük profilini kaldırmak için *adı* özelliği.
3. Kullanım `az monitor log-profiles create` yeni bir günlük profili oluşturmak için:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

    | Özellik | Gerekli | Açıklama |
    | --- | --- | --- |
    | name |Evet |Günlük profilinin adı. |
    | storage-account-id |Evet |Etkinlik günlükleri kaydedileceği depolama hesabı kaynak kimliği. |
    | locations |Evet |Boşlukla ayrılmış etkinlik günlüğü olayları toplamak istiyorsanız bölgelerin listesi. Tüm bölgelerin listesi için aboneliği kullanarak görüntüleyebileceğiniz `az account list-locations --query [].name`. |
    | days |Evet |Hangi olayların tutulacağını, 1 ile 365 arasında bir gün sayısı. Sıfır değeri günlükler süresiz olarak depolar (sonsuz).  Sıfır ise, ardından etkin parametresi ayarlanmalıdır true. |
    |enabled | Evet |TRUE veya False.  Bekletme İlkesi devre dışı bırakmak veya etkinleştirmek için kullanılır.  TRUE ise gün parametresi 0'dan büyük bir değer olması gerekir.
    | categories |Evet |Boşlukla ayrılmış toplanması gereken olay kategorilerinin listesi. Olası değerler şunlardır: yazma, silme ve eylem. |



## <a name="activity-log-schema"></a>Etkinlik günlüğü şeması
Etkinlik günlüğü verileri Azure depolama veya olay hub'ına gönderilen olsun, JSON için şu biçimde yazılır.

``` JSON
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```
Bu JSON öğeleri aşağıdaki tabloda açıklanmıştır.

| Öğe adı | Açıklama |
| --- | --- |
| time |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| resourceId |Etkilenen kaynak kaynak kimliği. |
| operationName |İşlemin adı. |
| category |Eylem kategorisi örn. Yazma, okuma, eylem. |
| resultType |Sonuç türü örn. Başarılı, başarısız, başlangıç |
| resultSignature |Kaynak türüne bağlıdır. |
| durationMs |Milisaniye cinsinden işlem süresi |
| callerIpAddress |İşlem, UPN Talebi veya SPN talep kullanılabilirliğine göre gerçekleştiren kullanıcının IP adresi. |
| correlationId |Genellikle bir GUID dize biçiminde. Bir Correlationıd paylaşan olayları aynı uber eyleme ait. |
| identity |Yetkilendirme ve talep açıklayan JSON blob. |
| authorization |BLOB RBAC özelliklerinin olay. Genellikle, "action", "rolü" ve "scope" özelliklerini içerir. |
| level |Olay düzeyi. Aşağıdaki değerlerden biri: _Kritik_, _hata_, _uyarı_, _bilgilendirici_, ve _ayrıntılı_ |
| location |Bölge oluştuğu konumu (veya genel). |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (yani, sözlük). |

> [!NOTE]
> Özellikleri ve bu özelliklerini kullanımını kaynağa bağlı olarak değişebilir.



## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik günlüğü hakkında daha fazla bilgi edinin](../../azure-resource-manager/resource-group-audit.md)
* [Azure İzleyici günlüklerine etkinlik günlüğü toplayın](activity-log-collect.md)
