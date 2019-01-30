---
title: "Yük: Python SDK'sı veri hazırlama"
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning veri hazırlığı SDK'sı ile veri yükleme hakkında bilgi edinin. Farklı türde giriş verileri yükleme, dosya türleri ve parametreleri belirtin veya dosya türü otomatik olarak algılamak için SDK'sı akıllı okuma işlevini kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 08dcb75fabc109a8869151402d3a448333beb556
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247536"
---
# <a name="load-and-read-data-with-azure-machine-learning"></a>Yükleme ve Azure Machine Learning ile veri okuma

Bu makalede, farklı yöntemler kullanarak verileri yükleme bilgi [Azure Machine Learning veri hazırlığı SDK'sı](https://aka.ms/data-prep-sdk). SDK'sı dahil olmak üzere birden çok veri alımı özellikleri destekler:

* Birçok dosya türünü parametre çıkarımı (kodlama, ayırıcı, üst bilgiler) ayrıştırma ile yükleme
* Tür-dönüştürme çıkarımı kullanılarak sırasında dosya yükleniyor
* MS SQL Server ve Azure Data Lake Storage bağlantı desteği

## <a name="load-data-automatically"></a>Otomatik olarak veri yükleme

Dosya türü belirtmeden verileri otomatik olarak yüklemek için kullanmak `auto_read_file()` işlevi. Dosya ve okumak için gerekli bağımsız değişken türü olayla otomatik olarak.

```python
import azureml.dataprep as dprep

dataflow = dprep.auto_read_file(path='./data/any-file.txt')
```

Bu işlev, dosya türü, kodlama ve diğer ayrıştırma bağımsız değişkenler tek bir kullanışlı giriş noktasından tüm otomatik olarak algılamak için yararlıdır. İşlev da otomatik olarak ayrılmış verileri yüklenirken yaygın olarak gerçekleştirilen aşağıdaki adımları gerçekleştirir:

* Çıkarımını yapma ve sınırlayıcı ayarlama
* Dosyanın üst boş kayıtları
* Çıkarımını yapma ve üst bilgi satırı ayarlama

Alternatif olarak, önceden yazın ve açıkça bu ayrıştırılır şeklini denetlemek istediğiniz dosyanın biliyorsanız, aşağıdaki özel SDK işlevleri görmek için bu makaleyi sunar devam edin.

## <a name="load-text-line-data"></a>Metin satırı veri yükleme

Bir veri akışı basit metin verileri okumak için kullanın `read_lines()` isteğe bağlı parametreleri belirtmeden.

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

Bir Pandas dataframe veri akışı nesneyi dönüştürmek için aşağıdaki kodu çalıştırın sonra verileri alınır.

```python
pandas_df = dataflow.to_pandas_dataframe()
```

## <a name="load-csv-data"></a>CSV veri yükleme

Temel çalışma zamanı, sınırlandırılmış dosyalar okurken, ayrıştırma parametreleri (kodlama, üst bilgiler vb. kullanılacağını ayırıcı.) çıkarabilir. Yalnızca konumunu belirterek bir CSV dosyası okuma girişimi için aşağıdaki kodu çalıştırın.

```python
# SAS expires June 16th, 2019
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
dataflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0||stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|1|ALABAMA|1|101710|Hale ilçe|10171002158| |
|2|ALABAMA|1|101710|Hale ilçe|10171002162| |
|3|ALABAMA|1|101710|Hale ilçe|10171002156| |
|4|ALABAMA|1|101710|Hale ilçe|10171000588|2|

Yükleme sırasında satırları tutmak için tanımlama `skip_rows` parametresi. Bu parametre (bir tabanlı bir dizin kullanarak) CSV dosyasında azalan yükleme satırları atlar.

```python
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1)
dataflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|Hale ilçe|10171002158|29|
|1|ALABAMA|1|101710|Hale ilçe|10171002162|40 |
|2|ALABAMA|1|101710|Hale ilçe|10171002156| 43|
|3|ALABAMA|1|101710|Hale ilçe|10171000588|2|
|4|ALABAMA|1|101710|Hale ilçe|10171000589|23 |

Sütun veri türlerini görüntülemek için aşağıdaki kodu çalıştırın.

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

Varsayılan olarak, Azure Machine Learning veri hazırlığı SDK'sı, veri türünü değiştirmez. Makaleyi okuduğunuz veri kaynağı bir metin dosyası olduğundan SDK tüm değerleri dize olarak okur. Bu örnekte, sayı olarak sayısal sütunları ayrıştırılması gerekip. Ayarlama `inference_arguments` parametresi `InferenceArguments.current_culture()` otomatik olarak çıkarır ve dosyanın okuma sırasında sütun türlerini dönüştürmek için.

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

Bazı sütunların düzgün sayısal algılandı ve kendi tür kümesine `float64`.

## <a name="use-excel-data"></a>Excel verilerini kullanma

SDK'sını içeren bir `read_excel()` Excel dosyalarını yüklemek için işlevi. Varsayılan olarak, çalışma kitabındaki ilk sayfa işlevi yükler. Yüklemek için belirli bir sayfaya tanımlamak için tanımladığınız `sheet_name` parametresiyle sayfa adı dize değeri.

```python
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2')
dataflow.head(5)
```

||Column1|Column2|Sütun3|Sütun4|Sütun5|Sütun6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|None|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|None|
|1|None|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|None|
|2|None|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|None|
|3|boyut sayısı|Unvan|Studio|Dünya çapında|Yerel / %|Column1|Deniz aşırı / %|Column2|Yıl ^|
|4|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|5|

Çıktı, ikinci sayfa verileri üç boş satır üstbilgileri önce olduğunu gösterir. `read_excel()` İşlevi satırları ve üst bilgileri kullanmak için isteğe bağlı parametreler içeriyor. İlk üç satırı atlamak için aşağıdaki kodu çalıştırın ve dördüncü satırı başlık olarak kullanma.

```python
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2', use_column_headers=True, skip_rows=3)
```

||boyut sayısı|Unvan|Studio|Dünya çapında|Yerel / %|Column1|Deniz aşırı / %|Column2|Yıl ^|
|------|------|------|-----|------|-----|-------|----|-----|-----|
|0|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|
|1|2|Titanic|Par.|2186.8|658.7|0.301|1528.1|0.699|1997'den ^|
|2|3|Marvel'ın Avengers|BV|1518.6|623.4|0.41|895.2|0.59|2012|
|3|4|Harry Potter ve Deathly Hallows 2. Bölüm|WB|1341.5|381|0.284|960.5|0.716|2011|
|4|5|Dondurulmuş|BV|1274.2|400.7|0.314|873.5|0.686|2013|

## <a name="load-fixed-width-data-files"></a>Sabit genişlikte veri dosyalarını yükleme

Loadfixed genişlikli dosyalar için karakter ofsetleri listesini belirtin. İlk sütunda, her zaman sıfır uzaklığında başlatmak için varsayılır.

```python
dataflow = dprep.read_fwf('./data/fixed_width_file.txt', offsets=[7, 13, 43, 46, 52, 58, 65, 73])
dataflow.head(5)
```

||010000|99999|SAHTE NORVEÇ|NO|NO_1|ENRS|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010003|99999|SAHTE NORVEÇ|NO|NO|ENSO||||
|1|010010|99999|JAN MAYEN|NO|JN|ENJA|+70933|-008667|+00090|
|2|010013|99999|ROST|NO|NO|||||
|3|010014|99999|SOERSTOKKEN|NO|NO|ENSO|+59783|+005350|+00500|
|4|010015|99999|BRINGELAND|NO|NO|ENBL|+61383|+005867|+03270|

Üst bilgi algılama önlemek ve doğru verileri ayrıştırmak için geçirmek `PromoteHeadersMode.NONE` için `header` parametresi.

```python
dataflow = dprep.read_fwf('./data/fixed_width_file.txt',
                          offsets=[7, 13, 43, 46, 52, 58, 65, 73],
                          header=dprep.PromoteHeadersMode.NONE)
```

||Column1|Column2|Sütun3|Sütun4|Sütun5|Sütun6|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010000|99999|SAHTE NORVEÇ|NO|NO_1|ENRS|Column7|Column8|Column9|
|1|010003|99999|SAHTE NORVEÇ|NO|NO|ENSO||||
|2|010010|99999|JAN MAYEN|NO|JN|ENJA|+70933|-008667|+00090|
|3|010013|99999|ROST|NO|NO|||||
|4|010014|99999|SOERSTOKKEN|NO|NO|ENSO|+59783|+005350|+00500|
|5|010015|99999|BRINGELAND|NO|NO|ENBL|+61383|+005867|+03270|

## <a name="load-sql-data"></a>SQL veri yükleme

SDK Ayrıca verileri bir SQL kaynağı'ndan yükleyin. Şu anda yalnızca Microsoft SQL Server desteklenir. Oluşturma bir SQL server verilerini okumak için bir `MSSQLDataSource` bağlantı parametrelerini içeren nesne. Parola parametresi `MSSQLDataSource` kabul eden bir `Secret` nesne. Gizli bir nesne iki şekilde oluşturabilirsiniz:

* Gizli anahtarı ve değeri yürütme altyapısı ile kaydedin.
* Gizli dizi ile yalnızca oluşturma bir `id` (gizli değer zaten yürütme ortamında kayıtlı olup olmadığını) kullanarak `dprep.create_secret("[SECRET-ID]")`.

```python
secret = dprep.register_secret(value="[SECRET-PASSWORD]", id="[SECRET-ID]")

ds = dprep.MSSQLDataSource(server_name="[SERVER-NAME]",
                           database_name="[DATABASE-NAME]",
                           user_name="[DATABASE-USERNAME]",
                           password=secret)
```

Bir veri kaynağı nesnesi oluşturduktan sonra Sorgu çıktısı verileri okumak için geçebilirsiniz.

```python
dataflow = dprep.read_sql(ds, "SELECT top 100 * FROM [SalesLT].[Product]")
dataflow.head(5)
```

||ProductID|Ad|ProductNumber|Renk|StandardCost|ListPrice|Boyut|Ağırlık|ProductCategoryID|ProductModelID|SellStartDate|SellEndDate|DiscontinuedDate|ThumbNailPhoto|ThumbnailPhotoFileName|ROWGUID|ModifiedDate|
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
|0|680|HL yol kadrosu - siyah, 58|FR R92B 58|Siyah|1059.3100|1431.50|58|1016.04|18|6|2002-06-01: 00:00:00 + 00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|43dd68d6-14a4-461f-9069-55309d90ea7e|2008-03-11 |0:01:36.827000 + 00:00|
|1|706|HL yol kadrosu - kırmızı, 58|FR R92R 58|Kırmızı|1059.3100|1431.50|58|1016.04|18|6|2002-06-01: 00:00:00 + 00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|9540ff17-2712-4c90-a3d1-8ce5568b2462|2008-03-11 |10:01:36.827000 + 00:00|
|2|707|Spor-100 Kask, kırmızı|HL U509 R|Kırmızı|13.0863|34.99 Dolar|None|None|35|33|2005-07-01: 00:00:00 + 00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|2e1ef41a-c08a-4ff6-8ada-bde58b64a712|2008-03-11 |10:01:36.827000 + 00:00|
|3|708|Spor-100 Kask, siyah|HL U509|Siyah|13.0863|34.99 Dolar|None|None|35|33|2005-07-01: 00:00:00 + 00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|a25a44fb-c2de-4268-958f-110b8d7621e2|2008-03-11 |10:01:36.827000 + 00:00|
|4|709|DAĞ bisikleti Çorapları, M|BÖYLE B909 M|Beyaz|3.3963|9.50|M|None|27|18|2005-07-01: 00:00:00 + 00:00|2006-06-30 00:00:00 + 00:00|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|18f95f47-1540-4e02-8f1f-cc1bcb6828d0|2008-03-11 |10:01:36.827000 + 00:00|

## <a name="use-azure-data-lake-storage"></a>Azure Data Lake depolama kullanma

Var olan iki yolu SDK'sı Azure Data Lake depolamaya erişmek için gerekli OAuth belirteci elde:

* Bir son kullanıcının Azure CLI ile oturum açma oturumunda erişim belirtecini alma
* Bir hizmet sorumlusu (SP) ve sertifika parolası olarak kullanın.

### <a name="use-an-access-token-from-a-recent-azure-cli-session"></a>Yeni bir Azure CLI oturumu bir erişim belirteci kullanın

Yerel makinenizde aşağıdaki komutu çalıştırın.

```azurecli
az login
az account show --query tenantId
dataflow = read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', tenant='microsoft.onmicrosoft.com')) head = dataflow.head(5) head
```

> [!NOTE]
> Kullanıcı hesabınızın birden fazla Azure kiracısının bir üyesiyse, Kiracı, AAD URL ana bilgisayar adı biçiminde belirtmeniz gerekir.

### <a name="create-a-service-principal-with-the-azure-cli"></a>Azure CLI ile hizmet sorumlusu oluşturma

Bir hizmet sorumlusu ve ilgili sertifikayı oluşturmak için Azure CLI'yı kullanın. Bu belirli bir hizmet sorumlusu ile yapılandırılmış `reader` Azure Data Lake Storage hesabını yalnızca 'dpreptestfiles' sınırlı kapsamı ile rolü.

```azurecli
az account set --subscription "Data Wrangling development"
az ad sp create-for-rbac -n "SP-ADLS-dpreptestfiles" --create-cert --role reader --scopes /subscriptions/35f16a99-532a-4a47-9e93-00305f6c40f2/resourceGroups/dpreptestfiles/providers/Microsoft.DataLakeStore/accounts/dpreptestfiles
```

Bu komut yayan `appId` ve sertifika dosyasını (genellikle, giriş klasörü) yolu. .Crt dosyası, ortak sertifika hem de özel anahtarı PEM biçiminde içerir.

```
openssl x509 -in adls-dpreptestfiles.crt -noout -fingerprint
```

Azure Data Lake Store dosya sistemine ACL yapılandırmak için kullanıcının objectID kullanın. Bu örnekte, hizmet sorumlusu kullanılır.

```azurecli
az ad sp show --id "8dd38f34-1fcb-4ff9-accd-7cd60b757174" --query objectId
```

Yapılandırmak için `Read` ve `Execute` erişim Azure Data Lake Store dosya sistemi için ACL klasörleri ve dosyaları için ayrı ayrı yapılandırın. Ana HDFS ACL modelini devralmayı desteklemez. Bunun nedeni, budur.

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

Kullanım `adal` paket (`pip install adal`) kimlik doğrulaması bağlamını MSFT Kiracı'da oluşturmak ve bir OAuth erişim belirteci almak için. ADLS için kaynak belirteci isteği için olmalıdır 'https://datalake.azure.net', çoğu diğer Azure kaynaklarından farklı.

```python
import adal
from azureml.dataprep.api.datasources import DataLakeDataSource

ctx = adal.AuthenticationContext('https://login.microsoftonline.com/microsoft.onmicrosoft.com')
token = ctx.acquire_token_with_client_certificate('https://datalake.azure.net/', servicePrincipalAppId, certificate, certThumbprint)
dataflow = dprep.read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', accessToken=token['accessToken']))
dataflow.to_pandas_dataframe().head()
```

||FMID|MarketName|Web sitesi|Sokak|city|Vilayet|
|----|------|-----|----|----|----|----|
|0|1012063|İlişkilendirme - Danville Kaledonya Çiftçilerden pazarlayın|https://sites.google.com/site/caledoniafarmers... ||Danville|Kaledonya|
|1|1011871|Stearns yerimizle Çiftçilerden'Pazar|http://Stearnshomestead.com |6975 ridge yol|Parma|Cuyahoga|
|2|1011878|100 mil Pazar|http://www.pfcmarkets.com |507 Harrison St|Kalamazoo|Kalamazoo|
|3|1009364|106 S. ana Sokak Çiftçilerden Pazar|http://thetownofsixmile.wordpress.com/ |106 S. ana Sokak|Altı mil|||
|4|1010691|10 Sokak topluluk Çiftçilerden Pazar|https://agrimissouri.com/... |10 Sokak ve Poplar|Lamar|Barton|
