---
title: Azure Machine Learning veri hazırlığı SDK - Python ile veri yükleme
description: Azure Machine Learning veri hazırlığı SDK'sı ile veri yükleme hakkında bilgi edinin. Farklı türde giriş verileri yükleme, dosya türleri ve parametreleri belirtin veya dosya türü otomatik olarak algılamak için SDK'sı akıllı okuma işlevini kullanın.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 436ff9d318dc311efe27352a8b2ac91cfb5be618
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221341"
---
#<a name="load-and-read-data-with-azure-machine-learning"></a>Yükleme ve Azure Machine Learning ile veri okuma

Kullanım [Azure Machine Learning veri hazırlığı SDK'sı](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py) giriş verisi farklı türler yüklenemedi. 

Verilerinizi yüklemek için iki yaklaşım vardır:
+ Veri dosyası türünü ve parametrelerini belirtin
+ Bir dosya türünü otomatik olarak algılamak için SDK'sı akıllı okuma işlevini kullanın

## <a name="use-text-line-data"></a>Metin satırı verileri kullanma 
Verileri yüklemek için en kolay yollarından biri, metin satırı okuma sağlamaktır.

Örnek kod aşağıda verilmiştir:
```python
dataflow = dprep.read_lines(path='./data/text_lines.txt')
dataflow.head(5)
```
||Çizgi|
|----|-----|
|0|Tarih \| \| en düşük sıcaklık \| \| en yüksek sıcaklık|
|1|2015-07-1 \| \| -4.1 \| \| 10.0|
|2|2015-07-2 \| \| -0.8 \| \| 10.8|
|3|2015-07-3 \| \| -7.0 \| \| 10.5|
|4|2015-07-4 \| \| -5.5 \| \| 9.3|

Veriler alınır sonra tam veri kümesi için bir pandas DataFrame alabilirsiniz.

Örnek kod aşağıda verilmiştir:
```python
df = dataflow.to_pandas_dataframe()
df
```
Örnek çıktı:
||Çizgi|
|----|-----|
|0|Tarih\| \| en düşük sıcaklık\| \| en yüksek sıcaklık|
|1|2015-07-1\| \| 4.1\| \| 10.0|
|2|2015-07-2\| \| 0.8\| \| 10.8|
|3|2015-07-3\| \| 7.0\| \| 10.5|
|4|2015-07-4\| \| 5.5\| \| 9.3|

## <a name="use-csv-data"></a>CSV verileri kullanın
Ayrılmış dosyalar okurken, temel alınan çalışma zamanı sağlamak yerine ayrıştırma parametreler (örneğin, kodlama, kullanıp kullanmayacağınızı üst bilgiler, vb. bir ayırıcı) Infer izin verebilirsiniz. Bu örnekte, yalnızca konumunu belirterek bir dosyayı okuma girişimi. 

Örnek kod aşağıda verilmiştir:
```python
# SAS expires June 16th, 2019
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
dataflow.head(5)
```

Örnek çıktı:
| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0||stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|1|ALABAMA|1|101710|Hale ilçe|10171002158| |
|2|ALABAMA|1|101710|Hale ilçe|10171002162| |
|3|ALABAMA|1|101710|Hale ilçe|10171002156| |
|4|ALABAMA|1|101710|Hale ilçe|10171000588|2|

Parametrelerden biri belirtebilirsiniz okuduğunuz dosyalarında atlanacak satır sayısıdır. Yinelenen satırına filtrelemek için aşağıdaki kodu kullanın.
```python
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1)
dataflow.head(5)
```

Örnek çıktı:
| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|Hale ilçe|10171002158|29|
|1|ALABAMA|1|101710|Hale ilçe|10171002162|40 |
|2|ALABAMA|1|101710|Hale ilçe|10171002156| 43|
|3|ALABAMA|1|101710|Hale ilçe|10171000588|2|
|4|ALABAMA|1|101710|Hale ilçe|10171000589|23 |

Ardından, sütunların veri türlerini bakabilirsiniz.
Örnek kod aşağıda verilmiştir:
```python
dataflow.head(1).dtypes

stnam                     object
fipst                     object
leaid                     object
leanm10                   object
ncessch                   object
schnam10                  object
MAM_MTH00numvalid_1011    object
dtype: object
```

Ne yazık ki, tüm birbirine karışmamasını dizeler olarak geri geldi. Varsayılan olarak, Azure Machine Learning veri hazırlığı SDK'sı, veri türünü değiştirmez nedeni budur. Biz okuma veri kaynağı bir metin dosyası olduğundan SDK tüm değerleri dize olarak okur. Bu örnekte, ancak sayı olarak sayısal sütunları ayrıştırılacak istiyoruz. Bunu yapmak için current_culture inference_arguments parametresi ayarlayabilirsiniz.

```
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1,
                          inference_arguments=dprep.InferenceArguments.current_culture())
dataflow.head(1).dtypes

stnam                      object
fipst                     float64
leaid                     float64
leanm10                    object
ncessch                   float64
schnam10                   object
ALL_MTH00numvalid_1011    float64
dtype: object
```

Birkaç sütunların düzgün sayı olarak algılandı ve kendi türü için float64 ayarlanır. Bitti alımı ile tam veri kümesi için bir pandas DataFrame alabilirsiniz.
Örnek kod aşağıda verilmiştir:
```python
df = dataflow.to_pandas_dataframe()
df
```

Örnek çıktı:
| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale ilçe|1.017100e + 10|49.0|
|1|ALABAMA|Hale ilçe|1.017100e + 10|40.0|
|2|ALABAMA|Hale ilçe|1.017100e + 10|43.0|
|3|ALABAMA|Hale ilçe|1.017100e + 10|2.0|
|4|ALABAMA|Hale ilçe|1.017100e + 10|23.0|

## <a name="use-excel-data"></a>Excel verilerini kullanma
Azure Machine Learning veri hazırlığı SDK'sını içeren bir `read_excel` Excel dosyalarını yüklemek için işlevi. Örnek kod aşağıda verilmiştir:
```python
dataflow = dprep.read_excel(path='./data/excel.xlsx')
dataflow.head(5)
```

Örnek çıktı:
||Column1|Column2|Sütun3|Sütun4|Sütun5|Sütun6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|Hoba|Demir, IVB|60000000.0|Bulundu|1920.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |-19.58333|17.91667|
|1|Cabo York|Demir, IIIAB|58200000.0|Bulundu|1818.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |76.13333|-64.93333|
|2|Campo del Cielo|Demir, IAB MG|50000000.0|Bulundu|1576.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |-27.46667|-60.58333|
|3|Diablo Canyon|Demir, IAB MG|30000000.0|Bulundu|1891.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |35.05000|-111.03333|
|4|Armanty|Demir, IIIE|28000000.0|Bulundu|1898.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |47.00000|88.00000|

Excel dosyası birinci sayfasını yüklediniz. Açıkça yüklemek istediğiniz sayfanın adını belirterek aynı sonucu elde edebilirsiniz. Bunun yerine ikinci sayfa yüklemek istiyorsanız, bağımsız değişken olarak abonenin adını sağlayabilirsiniz. Örneğin:
```python
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2')
dataflow.head(5)
```

Örnek çıktı:
||Column1|Column2|Sütun3|Sütun4|Sütun5|Sütun6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|None|None|None|None|None|None|None|None|None|
|1|None|None|None|None|None|None|None|None|None|
|2|None|None|None|None|None|None|None|None|None|
|3|boyut sayısı|Unvan|Studio|Dünya çapında|Yerel / %|Column1|Deniz aşırı / %|Column2|Yıl ^|
|4|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|5|

Gördüğünüz gibi tablonun ikinci sayfasında üst bilgiler ve üç boş satır yoktu. İşlev bağımsız uygun şekilde değiştirmeniz gerekir. Örneğin:
```python
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2', use_header=True, skip_rows=3)
df = dataflow.to_pandas_dataframe()
df
```

Örnek çıktı:
||boyut sayısı|Unvan|Studio|Dünya çapında|Yerel / %|Column1|Deniz aşırı / %|Column2|Yıl ^|
|------|------|------|-----|------|-----|-------|----|-----|-----|
|0|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|
|1|2|Titanic|Par.|2186.8|658.7|0.301|1528.1|0.699|1997'den ^|
|2|3|Marvel'ın Avengers|BV|1518.6|623.4|0.41|895.2|0.59|2012|
|3|4|Harry Potter ve Deathly Hallows 2. Bölüm|WB|1341.5|381|0.284|960.5|0.716|2011|
|4|5|Dondurulmuş|BV|1274.2|400.7|0.314|873.5|0.686|2013|

## <a name="use-fixed-width-data-files"></a>Sabit genişlikli veri dosyalarını kullanma
Sabit genişlikli dosyalar için uzaklık listesi belirtebilirsiniz. İlk sütun Uzaklık 0. Örneğin başlatmak için her zaman varsayılır:
```python
dataflow = dprep.read_fwf('./data/fixed_width_file.txt', offsets=[7, 13, 43, 46, 52, 58, 65, 73])
dataflow.head(5)
```

Örnek çıktı:
||010000|99999|SAHTE NORVEÇ|HAYIR|NO_1|ENRS|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010003|99999|SAHTE NORVEÇ|HAYIR|HAYIR|ENSO||||
|1|010010|99999|JAN MAYEN|HAYIR|JN|ENJA|+70933|-008667|+00090|
|2|010013|99999|ROST|HAYIR|HAYIR|||||
|3|010014|99999|SOERSTOKKEN|HAYIR|HAYIR|ENSO|+59783|+005350|+00500|
|4|010015|99999|BRINGELAND|HAYIR|HAYIR|ENBL|+61383|+005867|+03270|


Dosyaları bir üstbilgi varsa, ilk satır verisi olarak kabul isteyebilirsiniz. Geçirerek `PromoteHeadersMode.NONE` üstbilgi anahtar sözcük bağımsız değişkeni için üst bilgi algılama önlemek ve doğru verileri alın. Örneğin:
```python
dataflow = dprep.read_fwf('./data/fixed_width_file.txt',
                          offsets=[7, 13, 43, 46, 52, 58, 65, 73],
                          header=dprep.PromoteHeadersMode.NONE)

df = dataflow.to_pandas_dataframe()
df
```

Örnek çıktı:

||Column1|Column2|Sütun3|Sütun4|Sütun5|Sütun6|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010000|99999|SAHTE NORVEÇ|HAYIR|NO_1|ENRS|Column7|Column8|Column9|
|1|010003|99999|SAHTE NORVEÇ|HAYIR|HAYIR|ENSO||||
|2|010010|99999|JAN MAYEN|HAYIR|JN|ENJA|+70933|-008667|+00090|
|3|010013|99999|ROST|HAYIR|HAYIR|||||
|4|010014|99999|SOERSTOKKEN|HAYIR|HAYIR|ENSO|+59783|+005350|+00500|
|5|010015|99999|BRINGELAND|HAYIR|HAYIR|ENBL|+61383|+005867|+03270|

## <a name="use-sql-data"></a>SQL verileri kullanın
Azure Machine Learning veri hazırlığı SDK'sı, SQL sunuculardan veri de yükleyebilirsiniz. Şu anda yalnızca Microsoft SQL Server desteklenir.
SQL Server'dan veri okumak için gerekli bağlantı bilgilerini içeren bir veri kaynağı nesnesi oluşturun. Örneğin:
```python
secret = dprep.register_secret("[SECRET-USERNAME]", "[SECRET-PASSWORD]")

ds = dprep.MSSQLDataSource(server_name="[SERVER-NAME]",
                           database_name="[DATABASE-NAME]",
                           user_name="[DATABASE-USERNAME]",
                           password=[DATABASE-PASSWORD])
```
Gördüğünüz gibi parola parametresi `MSSQLDataSource` gizli bir nesneyi kabul eder. Gizli bir nesne iki yolla alabilirsiniz:
-   Gizli anahtarı ve değeri yürütme altyapısı ile kaydedin. 
-   Gizli dizi ile yalnızca bir kimlik (yararlı yürütme ortamında gizli değer zaten kayıtlı değilse) oluşturun.

Bir veri kaynağı nesnesi oluşturduktan sonra veri okumak için geçebilirsiniz. Örneğin:
```python
dataflow = dprep.read_sql(ds, "SELECT top 100 * FROM [SalesLT].[Product]")
dataflow.head(5)
```

Örnek çıktı:
||ProductID|Ad|ProductNumber|Renk|StandardCost|ListPrice|Boyut|Ağırlık|ProductCategoryID|ProductModelID|SellStartDate|SellEndDate|DiscontinuedDate|ThumbNailPhoto|ThumbnailPhotoFileName|ROWGUID|ModifiedDate|
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
|0|680|HL yol kadrosu - siyah, 58|FR R92B 58|Siyah|1059.3100|1431.50|58|1016.04|18|6|2002-06-01: 00:00:00 + 00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|43dd68d6-14a4-461f-9069-55309d90ea7e|2008-03-11 |0:01:36.827000 + 00:00|
|1|706|HL yol kadrosu - kırmızı, 58|FR R92R 58|Kırmızı|1059.3100|1431.50|58|1016.04|18|6|2002-06-01: 00:00:00 + 00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|9540ff17-2712-4c90-a3d1-8ce5568b2462|2008-03-11 |10:01:36.827000 + 00:00|
|2|707|Spor-100 Kask, kırmızı|HL U509 R|Kırmızı|13.0863|34.99 Dolar|None|None|35|33|2005-07-01: 00:00:00 + 00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|2e1ef41a-c08a-4ff6-8ada-bde58b64a712|2008-03-11 |10:01:36.827000 + 00:00|
|3|708|Spor-100 Kask, siyah|HL U509|Siyah|13.0863|34.99 Dolar|None|None|35|33|2005-07-01: 00:00:00 + 00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|a25a44fb-c2de-4268-958f-110b8d7621e2|2008-03-11 |10:01:36.827000 + 00:00|
|4|709|DAĞ bisikleti Çorapları, M|BÖYLE B909 M|Beyaz|3.3963|9.50|M|None|27|18|2005-07-01: 00:00:00 + 00:00|2006-06-30 00:00:00 + 00:00|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|18f95f47-1540-4e02-8f1f-cc1bcb6828d0|2008-03-11 |10:01:36.827000 + 00:00|

```python
df = dataflow.to_pandas_dataframe()
df.dtypes
```

Örnek çıktı:
```
ProductID                                     int64
Name                                         object
ProductNumber                                object
Color                                        object
StandardCost                                float64
ListPrice                                   float64
Size                                         object
Weight                                      float64
ProductCategoryID                             int64
ProductModelID                                int64
SellStartDate             datetime64[ns, UTC+00:00]
SellEndDate                                  object
DiscontinuedDate                             object
ThumbNailPhoto                               object
ThumbnailPhotoFileName                       object
rowguid                                      object
ModifiedDate              datetime64[ns, UTC+00:00]
dtype: object
```

## <a name="use-azure-data-lake-storage"></a>Azure Data Lake depolama kullanma
Var olan iki yolu SDK'sı Azure Data Lake depolamaya erişmek için gerekli OAuth belirteci elde:
-   Kullanıcının Azure CLI ile oturum açma son oturum açma oturumu bir erişim belirteci alma
-   Bir hizmet sorumlusu (SP) ve sertifika parolası olarak kullanın.

### <a name="use-an-access-token-from-a-recent-azure-cli-session"></a>Yeni bir Azure CLI oturumu bir erişim belirteci kullanın
Yerel makinenizde aşağıdaki komutu çalıştırın:

> [!NOTE] 
> Kullanıcı hesabınızın birden fazla Azure kiracısının bir üyesiyse, Kiracı, AAD URL ana bilgisayar adı biçiminde belirtmeniz gerekir.


Örneğin:
```azurecli
az login
az account show --query tenantId
dataflow = read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', tenant='microsoft.onmicrosoft.com')) head = dataflow.head(5) head
```
### <a name="create-a-service-principal-with-the-azure-cli"></a>Azure CLI ile hizmet sorumlusu oluşturma
Bir hizmet sorumlusu oluşturmak için Azure CLI'yı ve karşılık gelen sertifikayı kullanabilirsiniz. Bu belirli bir hizmet sorumlusu okuyucu Azure Data Lake Storage hesabını yalnızca 'dpreptestfiles' sınırlı kapsamı ile yapılandırılır.  Örneğin:
```azurecli
az account set --subscription "Data Wrangling development"
az ad sp create-for-rbac -n "SP-ADLS-dpreptestfiles" --create-cert --role reader --scopes /subscriptions/35f16a99-532a-4a47-9e93-00305f6c40f2/resourceGroups/dpreptestfiles/providers/Microsoft.DataLakeStore/accounts/dpreptestfiles
```
Bu komut yayan `appId` ve sertifika dosyasını (genellikle, giriş klasörü) yolu. .Crt dosyası, ortak sertifika hem de özel anahtarı PEM biçiminde içerir.

Parmak izi ayıklayın:
```
openssl x509 -in adls-dpreptestfiles.crt -noout -fingerprint
```

Azure Data Lake Store dosya sistemine ACL yapılandırmak için kullanıcının veya bu örneğin, hizmet sorumlusu nesne kimliği'ni kullanın. Örneğin:
```azurecli
az ad sp show --id "8dd38f34-1fcb-4ff9-accd-7cd60b757174" --query objectId
```

Yapılandırmak için `Read` ve `Execute` erişim Azure Data Lake Store dosya sistemi için ACL klasörleri ve dosyaları için tek tek yapılandırmanız gerekir. Ana HDFS ACL modelini devralmayı desteklemez. Bunun nedeni, budur. Örneğin:
```azurecli
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r-x" --path /
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r--" --path /farmers-markets.csv
```
```
certThumbprint = 'C2:08:9D:9E:D1:74:FC:EB:E9:7E:63:96:37:1C:13:88:5E:B9:2C:84'
certificate = ''
with open('./data/adls-dpreptestfiles.crt', 'rt', encoding='utf-8') as crtFile:
    certificate = crtFile.read()

servicePrincipalAppId = "8dd38f34-1fcb-4ff9-accd-7cd60b757174"
```
### <a name="acquire-an-oauth-access-token"></a>OAuth erişim belirteci alma
Kullanım `adal` paket (aracılığıyla: `pip install adal`) kimlik doğrulaması bağlamını MSFT Kiracı'da oluşturmak ve bir OAuth erişim belirteci almak için. ADLS için kaynak belirteci isteği için olmalıdır 'https://datalake.azure.net', çoğu diğer Azure kaynaklarından farklı.

```python
import adal
from azureml.dataprep.api.datasources import DataLakeDataSource

ctx = adal.AuthenticationContext('https://login.microsoftonline.com/microsoft.onmicrosoft.com')
token = ctx.acquire_token_with_client_certificate('https://datalake.azure.net/', servicePrincipalAppId, certificate, certThumbprint)
dataflow = dprep.read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', accessToken=token['accessToken']))
dataflow.to_pandas_dataframe().head()
```

||FMID|MarketName|Web sitesi|Sokak|city|Ülke|
|----|------|-----|----|----|----|----|
|0|1012063|İlişkilendirme - Danville Kaledonya Çiftçilerden pazarlayın|https://sites.google.com/site/caledoniafarmers... ||Danville|Kaledonya|
|1|1011871|Stearns yerimizle Çiftçilerden'Pazar|http://Stearnshomestead.com |6975 ridge yol|Parma|Cuyahoga|
|2|1011878|100 mil Pazar|http://www.pfcmarkets.com |507 Harrison St|Kalamazoo|Kalamazoo|
|3|1009364|106 S. ana Sokak Çiftçilerden Pazar|http://thetownofsixmile.wordpress.com/ |106 S. ana Sokak|Altı mil|||
|4|1010691|10 Steet topluluk Çiftçilerden Pazar|http://agrimissouri.com/mo-grown/grodetail.php... |10 Sokak ve Poplar|Lamar|Barton|

## <a name="use-smart-reading"></a>"Akıllı okuma" kullanın

Bir dosya türünü otomatik olarak algılamak için SDK'sı akıllı okuma işlevini kullanabilirsiniz.
