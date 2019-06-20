---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: b8585b62b0728d1ba6e010e42b44840903c46833
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188839"
---
### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
**Azure depolama bağlı hizmeti** kullanarak bir Azure depolama hesabı bir Azure data factory'ye bağlamak tanır **hesap anahtarı**, sağlayan data factory ile küresel erişim için Azure depolama. Aşağıdaki tabloda, Azure depolama bağlı hizmetine özel JSON öğeleri için bir açıklama sağlar.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **AzureStorage** |Evet |
| bağlantı dizesi |ConnectionString özelliği için Azure depolamaya bağlanmak için gereken bilgileri belirtin. |Evet |

Görünüm/hesap anahtarı için bir Azure depolama kopyalama adımları için aşağıdaki bölüme bakın: [Erişim anahtarları](../articles/storage/common/storage-account-manage.md#access-keys).

**Örnek:**  

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="azure-storage-sas-linked-service"></a>Azure depolama Sas bağlı hizmeti
Paylaşılan erişim imzası (SAS) depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Bir istemci, depolama hesabınızdaki nesnelere ve belirtilen bir izin kümesi ile belirli bir süre için hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan sınırlı izinleri vermenizi sağlar. SAS depolama kaynak kimliği doğrulanmış erişim için gerekli tüm bilgileri sorgu parametrelerini kapsayan bir URI'dir. SAS ile depolama kaynaklarına erişmek için istemci SAS uygun oluşturucu veya yönteme geçirmek yeterlidir. SAS hakkında ayrıntılı bilgi için bkz: [paylaşılan erişim imzaları: SAS modelini anlama](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure Data Factory artık yalnızca destekler **hizmet SAS** ancak hesap SAS. Bkz: [türleri, paylaşılan erişim imzaları](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) bu iki tür ve nasıl oluşturulacağıyla ilgili ayrıntılar için. Azure Portalı'ndan generable SAS URL'sini not alın veya desteklenmeyen bir hesap SAS, Depolama Gezgini.

> [!TIP]
> Aşağıdaki depolama hesabınız (yer tutucuları değiştirin ve gerekli izin verme) için hizmet SAS oluşturmak için PowerShell komutları çalıştırabilirsiniz: `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Azure depolama SAS bağlı hizmet, bir paylaşılan erişim imzası (SAS) kullanarak bir Azure data factory'de bir Azure depolama hesabı bağlantı sağlar. Data factory ile kısıtlı/zamana bağlı depolama (blob/kapsayıcı) tüm/özel kaynaklarına erişimi sağlar. Aşağıdaki tabloda, Azure depolama SAS bağlantılı hizmete özgü JSON öğeleri için bir açıklama sağlar. 

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **AzureStorageSas** |Evet |
| sasUri |Blob, kapsayıcı veya tablo gibi Azure Storage kaynakları için paylaşılan erişim imzası URI'si belirtin.  |Evet |

**Örnek:**

```json
{
    "name": "StorageSasLinkedService",
    "properties": {
        "type": "AzureStorageSas",
        "typeProperties": {
            "sasUri": "<Specify SAS URI of the Azure Storage resource>"
        }
    }
}
```

Oluştururken bir **SAS URI'sini**, aşağıdakileri de göz önünde bulundurur:  

* Uygun okuma/yazma ayarlamak **izinleri** (okuma, yazma, okuma/yazma) bağlı hizmet, data factory'de nasıl kullanıldığına göre nesneler üzerinde.
* Ayarlama **süre sonu** uygun şekilde. Azure depolama nesnelerine erişimi işlem hattının etkin dönemini içinde dolmaz emin olun.
* URI, doğru kapsayıcı/blob veya tablo düzeyinde ihtiyaca göre oluşturulmalıdır. Data Factory hizmetinin, belirli bir bloba erişmek bir Azure blob için SAS URI'si sağlar. Kapsayıcıdaki bloblara yinelemek Data Factory hizmetinin Azure blob kapsayıcısı için SAS URI'si sağlar. Daha fazla/daha az nesne, daha sonra erişim sağlamak veya SAS URI'sini güncelleştirmek gerekiyorsa, yeni bir URI'ya bağlı hizmet güncelleştirmeyi unutmayın.   

