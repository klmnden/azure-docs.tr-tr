## <a name="fileshare-dataset-type-properties"></a>FileShare veri kümesi türü özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](../articles/data-factory/v1/data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri kümesi türleri için benzerdir.

**TypeProperties** bölümü, her veri kümesi türü için farklıdır. Veri kümesi türüne özgü bilgiler sağlar. TypeProperties bölüm türü veri kümesi için **FileShare** veri kümesine aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasörün alt yolu. Kaçış karakteri kullanmak ' \ ' dize özel karakter. Bkz: [örnek bağlantılı hizmeti ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekleri için.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün dilimine dayalı yol başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adını belirtin **folderPath** klasöründeki belirli bir dosya belirtmek için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tablonun klasördeki tüm dosyaları işaret eder.<br/><br/>FileName bir çıkış veri kümesi için belirtilmediğinde oluşturulan dosya adı aşağıdaki olacaktır bu biçimi: <br/><br/>Veriler. <Guid>.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1:`"fileFilter": "*.log"`<br/>Örnek 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter bir giriş FileShare veri kümesi için geçerlidir. Bu özellik ile HDFS desteklenmiyor. |Hayır |
| partitionedBy |partitionedBy filename zaman serisi veri için dinamik bir folderPath belirtmek için kullanılabilir. Örneğin, her veri saat için parametreli folderPath. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](#specifying-textformat), [Json biçimine](#specifying-jsonformat), [Avro biçimi](#specifying-avroformat), [Orc biçimi](#specifying-orcformat), ve [Parquet biçimi](#specifying-parquetformat) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**; ve desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [sıkıştırma belirtme](#specifying-compression) bölümü. |Hayır |
| useBinaryTransfer |Belirtin olup ikili aktarım modunu kullanın. İkili mod ve false ASCII için true. Varsayılan değer: True. İlişkili bağlantılı hizmet türü türü olduğunda bu özellik yalnızca kullanılabilir: Ftp_sunucusu. |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanılamaz.
>
>

### <a name="using-partionedby-property"></a>PartionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi dinamik bir folderPath, zaman serisi veri partitionedBy ile dosya adını belirtebilirsiniz. Veri Fabrikası makroları ve sistem değişkeni SliceStart, verilen veri dilimi için mantıksal süre belirtmek SliceEnd bunu yapabilirsiniz.

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında bilgi edinmek için [veri kümeleri oluşturma](../articles/data-factory/v1/data-factory-create-datasets.md), [zamanlama ve yürütme](../articles/data-factory/v1/data-factory-scheduling-and-execution.md), ve [oluşturma ardışık düzen](../articles/data-factory/v1/data-factory-create-pipelines.md) makaleleri.

#### <a name="sample-1"></a>Örnek 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Bu örnekte {dilim} belirtilen veri fabrikası sistem değişkenin değerini SliceStart (YYYYMMDDHH) biçiminde ile değiştirilir. Dilimin başlangıç SliceStart başvuruyor. FolderPath her dilim için farklıdır. Örnek: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Örnek 2:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Bu örnekte, folderPath ve fileName özellikleri tarafından kullanılan ayrı değişkenleri içine yıl, ay, gün ve saat SliceStart ayıklanır.
