## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Yapı tanımı dikdörtgen veri kümeleri için belirtme
JSON veri kümesi yapısı bölümünde bir **isteğe bağlı** bölümünde dikdörtgen tablolar için (satırlar ve sütunlar) ve tablonun sütunlarını koleksiyonunu içerir. Yapısı bölüm iki tür bilgi sağlayan tür dönüştürmeleri için veya sütun eşlemelerini yapmak için kullanır. Aşağıdaki bölümlerde bu özellikler ayrıntılı açıklanmaktadır. 

Her sütun aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Sütunun adı. |Evet |
| type |Sütunun veri türü. Zaman tür bilgileri belirtmelisiniz için aşağıdaki türü dönüşümleri bölümüne daha ayrıntılı bakın |Hayır |
| Kültür |.NET türü belirtilir ve .NET türü Datetime veya Datetimeoffset olduğunda kullanılacak kültürü temel. Varsayılan değer "en-us". |Hayır |
| Biçimi |Türü belirtilmiş ve .NET olduğunda kullanılacak biçim dizesi Datetime veya Datetimeoffset yazın. |Hayır |

Aşağıdaki örnek, üç sütun UserID, adı ve lastlogindate sahip bir tablo yapısı bölüm JSON gösterir.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Lütfen "yapı" bilgileri içerecek şekilde ne zaman ve ne dahil etmek için aşağıdaki kılavuzları kullanın **yapısı** bölümü.

* **Yapılandırılmış veri kaynakları için** deposu veri şeması ve türü bilgileri verilerin kendisini (yalnızca istiyorsanız "yapısı" bölümü belirtmelidir kaynakları SQL Server, Oracle, Azure tablo vb. gibi), yanı sıra belirli bir kaynak sütun eşlemesi yapın Havuz ve adları belirli sütunlardaki sütunları (sütun eşleme bölümünde aşağıdaki ayrıntılarına bakın) aynı değildir. 
  
    Yukarıda belirtildiği gibi tür bilgisi "yapısı" bölümünde isteğe bağlıdır. Yapılandırılmış kaynakları için tür bilgileri zaten kullanılabilir veri kümesi tanımı veri deposundaki bir parçası olarak, bu nedenle dahil türü bilgileri "yapısı" bölümünde eklediğinizde.
* **Şema okuma veri kaynaklarında (özellikle Azure blob) için** herhangi bir şema veya türü bilgi verilerle depolamadan veri depolamayı seçebilirsiniz. Bu veri kaynağı türleri için aşağıdaki 2 durumlarda "yapısı" içermelidir:
  * Sütun eşlemesi yapmak istiyor.
  * Veri kümesi kopyalama etkinliğinde bir kaynak olduğunda, "yapısındaki" türü bilgileri sağlayabilir ve veri fabrikası dönüştürme havuz için yerel türleri için bu tür bilgileri kullanır. Bkz: [için ve Azure Blob veri taşıma](../articles/data-factory/v1/data-factory-azure-blob-connector.md) daha fazla bilgi için makalenin.

### <a name="supported-net-based-types"></a>Desteklenir. NET tabanlı türleri
Veri Fabrikası okuma veri kaynaklarında Azure blob gibi şema "yapısındaki" türü bilgileri sağlamak için aşağıdaki CLS uyumlu dayalı .NET türü değerleri destekler.

* Int16
* Int32 
* Int64
* Tek
* Çift
* Ondalık
* Byte]
* bool
* Dize 
* GUID
* Tarih saat
* Datetimeoffset
* Timespan 

DateTime ve Datetimeoffset için de isteğe bağlı olarak, özel Datetime dizesini ayrıştırma kolaylaştırmak için "kültür" & "format" dizesini belirtebilirsiniz. Tür dönüşümü aşağıdaki örneğe bakın.

