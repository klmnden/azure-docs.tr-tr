## <a name="repeatability-during-copy"></a>Kopyalama sırasında Yinelenebilirlik
İlk ve son ilişkisel depoları veri kopyalama işlemi sırasında Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurmanız gerekir. 

Bir dilim otomatik olarak Azure Data Factory içinde belirtilen yeniden deneme ilkesi uyarınca yeniden çalıştırılabilir. Geçici arızaya karşı koruma sağlamak için bir yeniden deneme ilkesi ayarlamanızı öneririz. Bu nedenle Yinelenebilirlik, veri taşıma sırasında halletmeniz için önemli bir yönü ' dir. 

**Bir kaynak olarak:**

> [!NOTE]
> Aşağıdaki örnekler için Azure SQL ancak dikdörtgen veri kümeleri destekleyen herhangi bir veri deposuna uygulanabilir. Ayarlamanız gerekebilir **türü** kaynağının ve **sorgu** özelliği (örneğin: Sorgu sqlReaderQuery yerine) verilerini depolamak.   
> 
> 

Genellikle, ilişkisel depoları okunurken, yalnızca o dilim karşılık gelen verileri okumak istersiniz. Bunu yapmanın bir yolu, Azure Data Factory WindowStart ve WindowEnd değişkenlerinin kullanarak olacaktır. Azure Data Factory'de burada işlevleri ve değişkenler hakkında bilgi [zamanlama ve yürütme](../articles/data-factory/v1/data-factory-scheduling-and-execution.md) makalesi. Örnek: 

```json
"source": {
"type": "SqlSource",
"sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Bu sorgu 'dilim süresi aralığının MyTable' verileri okur. Bu dilimin yeniden çalıştırın, ayrıca her zaman bu davranışı sağlamak. 

Diğer durumlarda, tüm tablo bölümünü okumanız önerilir (bir kez taşımak için yalnızca varsayalım) ve sqlReaderQuery gibi tanımlayın:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```
