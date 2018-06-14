### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
**Azure depolama bağlantılı hizmeti** kullanarak Azure data factory için bir Azure depolama hesabı bağlantı sayesinde **hesap anahtarı**, sağlayan veri fabrikası genel erişim ile Azure depolama birimine. Aşağıdaki tabloda, JSON öğeleri Azure Storage bağlı hizmeti için belirli bir açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **AzureStorage** |Evet |
| connectionString |ConnectionString özelliği için Azure depolama alanına bağlanmak için gereken bilgileri belirtin. |Evet |

Bir Azure Storage için Görüntüle/kopyalama hesap anahtarı için adımlar için aşağıdaki makaleye bakın: [görüntüleme, kopyalama ve erişim anahtarları yeniden oluşturma depolama](../articles/storage/common/storage-create-storage-account.md#manage-your-storage-account).

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

### <a name="azure-storage-sas-linked-service"></a>Azure depolama Sas bağlantılı hizmetinin
Paylaşılan erişim imzası (SAS) depolama hesabınızdaki kaynaklara yetkilendirilmiş erişim sağlar. Bir istemci depolama hesabındaki nesnelere süre ve belirtilen bir izin kümesi ile belirli bir süre için hesap erişim tuşlarınızı paylaşmak zorunda kalmadan sınırlı izinleri vermek sağlar. SAS depolama kaynağı için kimlik doğrulamalı erişim için gerekli tüm bilgileri kendi sorgu parametrelerini kapsayan bir URI değil. SAS ile depolama kaynaklarına erişmek için istemcinin yalnızca uygun Oluşturucusu veya yöntem SAS geçirmek gerekir. SAS hakkında ayrıntılı bilgi için bkz: [paylaşılan erişim imzaları: SAS modelini anlama](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure Data Factory artık yalnızca destekler **hizmet SAS** ancak hesap SAS. Bkz: [türleri, paylaşılan erişim imzaları](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) bu iki tür ve nasıl oluşturulacağıyla ilgili ayrıntılar için. Desteklenmeyen bir hesap SAS, Depolama Gezgini değil veya Azure portalından generable SAS URL unutmayın.

> [!TIP]
> Depolama hesabınız (Değiştir yer tutucu ve gerekli izin verin) için hizmet SAS oluşturmak için PowerShell komutlarını aşağıda çalıştırabilirsiniz:`$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Azure depolama bağlı SAS hizmeti bir paylaşılan erişim imzası (SAS) kullanarak Azure data factory için bir Azure depolama hesabı bağlantı sağlar. Veri Fabrikası depolama alanındaki tüm/özel kaynakları (blob/kapsayıcısı) kısıtlanmış/zaman sınırlı erişim sağlar. Aşağıdaki tabloda Azure depolama bağlı SAS hizmete özgü JSON öğelerini açıklamasını sağlar. 

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **AzureStorageSas** |Evet |
| sasUri |Blob, kapsayıcı ya da tablo gibi Azure Storage kaynakları için paylaşılan erişim imzası URI belirtin.  |Evet |

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

Oluştururken bir **SAS URI'sini**, aşağıdakileri göz önünde bulunduruyor:  

* Ayarlama uygun okuma/yazma **izinleri** (okuma, yazma, okuma/yazma) bağlantılı hizmet, veri fabrikası nasıl kullanıldığına göre nesneler üzerinde.
* Ayarlama **sona erme saati** uygun şekilde. Azure Storage nesnelere erişimi ardışık düzen etkin süresi içinde dolmaz emin olun.
* URI sağ kapsayıcı/blob veya tablo düzeyinde gereksinimleri temelinde oluşturulmalıdır. Bir Azure blob için bir SAS URI'sini bu belirli blob erişmek Data Factory hizmeti sağlar. Bir Azure blob kapsayıcısı için SAS URI'sini bu kapsayıcıdaki blobları yinelemek Data Factory hizmeti sağlar. Daha fazla/az nesneleri daha sonra erişim sağlamak veya SAS URI'sini güncelleştirmek gerekiyorsa, yeni bir URI ile bağlantılı hizmet güncelleştirmeyi unutmayın.   

