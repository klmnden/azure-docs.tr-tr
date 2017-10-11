## <a name="repeatability-during-copy"></a>Kopyalama sırasında Yinelenebilirlik
Azure SQL/SQL Server veri kopyalamayı diğer verilerden depoladığında bir Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurmanız gerekir. 

Azure SQL/SQL Server veritabanına veri kopyalama, kopyalama etkinliği varsayılan APPEND havuz tablosu veri kümesi varsayılan olarak kullanacak. Örneğin, verileri Azure SQL/SQL Server veritabanına iki kayıtlarını içeren bir CSV (virgülle ayrılmış değerler veri kaynağından) dosya kopyalarken, bu tablonun benzer.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Kaynak dosyasında hata buldu ve miktarını aşağı boru 2-4 kaynak dosyasında güncelleştirilmiş varsayalım. Veri dilimi belirli bir döneme ait yeniden çalıştırırsanız, Azure SQL/SQL Server veritabanına eklenen iki yeni kayıtlar bulabilirsiniz. Aşağıdaki tablodaki sütunların hiçbiri birincil anahtar kısıtlaması varsayar.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Bunu önlemek için aşağıdakilerden birini yararlanarak UPSERT semantiği belirtin gerekecektir aşağıda belirtildiği 2 mekanizmaları aşağıda.

> [!NOTE]
> Bir dilim otomatik olarak Azure Data Factory içinde belirtilen yeniden deneme ilkesi uyarınca yeniden çalıştırılabilir.
> 
> 

### <a name="mechanism-1"></a>Mekanizması 1
Yararlanabileceğiniz **sqlWriterCleanupScript** bir dilim çalıştırıldığında ilk temizleme eylemi gerçekleştirmek için özellik. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Temizleme betiğini yürütülen hangi verileri bu dilim karşılık gelen SQL tablosundan siler belirli bir dilim için kopyalama sırasında ilk. Etkinlik sonradan verileri SQL tablosuna ekler. 

Dilimi yeniden çalıştırın. ardından, miktarı olarak güncelleştirilir bulacaksınız şimdi ise, istenen.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Düz rondela kaydı özgün csv kaldırılana varsayalım. Dilimi yeniden çalıştırmak aşağıdaki sonucu oluşturur: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Yeni bir şey yapılması gerekiyordu. Kopyalama etkinliği, dilim karşılık gelen verileri silmek için temizleme betiğini verdi. Giriş (hangi sonra yalnızca 1 kaydı bulunan) csv okuma sonra ve tabloya eklenen. 

### <a name="mechanism-2"></a>Mekanizması 2
> [!IMPORTANT]
> Sliceıdentifiercolumnname Azure SQL Data Warehouse için şu anda desteklenmiyor. 

Ayrılmış bir sütun sağlayarak Yinelenebilirlik elde etmek için başka bir mekanizma olan (**Sliceıdentifiercolumnname**) hedef tablo. Azure Data Factory tarafından bu sütun kaynak ve hedef eşitlenmesine emin olmak için kullanılır. Bu yaklaşım, değiştirme veya hedef SQL tablo şemasını tanımlama esneklik olduğunda çalışır. 

Bu sütun Azure Data Factory'nin Yinelenebilirlik amaçlar için kullanılır ve işlem sırasında tablonun herhangi bir şema değişikliği Azure Data Factory yapmaz. Bu yaklaşımı kullanmak için yol:

1. Türünde bir sütun ikili (32) hedef SQL tablosu tanımlayın. Bu sütunda hiç bir kısıtlama olması gerekir. Şimdi bu sütun bu örnekte 'ColumnForADFuseOnly' adlandırın.
2. Kopyalama etkinliği şu şekilde kullanın:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Azure Data Factory bu sütun kaynak ve hedef eşitlenmesine emin olmak için gerek göredir doldurur. Bu sütundaki değerleri dışında bu bağlamda kullanıcı tarafından kullanılmamalıdır. 

Benzer şekilde mekanizması 1, kopyalama etkinliği otomatik olarak ayarlanır hedef SQL tablosu verilen dilim için verileri ilk Temizle ve normal veri kaynağından bu dilim için hedef eklemek için kopyalama etkinliği çalıştırın. 

