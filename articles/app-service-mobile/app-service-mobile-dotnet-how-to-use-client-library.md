---
title: App Service Mobile Apps yönetilen istemci kitaplığı ile çalışma | Microsoft Docs
description: Azure App Service Mobile Apps ile Windows ve Xamarin uygulamaları için .NET İstemci Kitaplığı'nı kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 0280785c-e027-4e0d-aaf2-6f155e5a6197
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 09/24/2018
ms.author: crdun
ms.openlocfilehash: 8f014f1cb40e1a629d1989f00805fc91015a3ae9
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119316"
---
# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Azure Mobile Apps için yönetilen istemci kullanma
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz, yaygın senaryoları için Azure App Service Mobile Apps Windows ve Xamarin uygulamaları için yönetilen istemci kitaplığı kullanarak nasıl gerçekleştireceğinizi gösterir. Mobile Apps yeniyseniz tamamlamadan önce dikkate almanız gereken [Azure Mobile Apps Hızlı Başlangıç] [ 1] öğretici. Bu kılavuzda, biz üzerindeki yönetilen istemci tarafı SDK odaklanın. Mobile Apps için sunucu tarafı SDK'lar hakkında daha fazla bilgi edinmek için belgelere bakın [.NET sunucu SDK'sı] [ 2] veya [Node.js sunucu SDK'sı] [ 3].

## <a name="reference-documentation"></a>Başvuru belgeleri
İstemci için başvuru belgeleri SDK şuradan ulaşabilirsiniz: [Azure Mobile Apps .NET istemci başvurusu][4].
Birden çok istemci örnekleri de bulabilirsiniz [Azure örnekleri GitHub deposunda][5].

## <a name="supported-platforms"></a>Desteklenen platformlar
.NET platformu aşağıdaki platformları destekler:

* Xamarin Android API 19 için 24 (KitKat Nougat üzerinden) aracılığıyla yayımlar.
* İOS 8.0 ve üzeri sürümler için Xamarin iOS sürümleri
* Evrensel Windows Platformu
* Windows Phone 8.1
* Windows Phone 8.0 Silverlight uygulamalarının dışında

"Sunucu-flow" kimlik doğrulaması, kullanıcı Arabiriminde sunulan bir WebView kullanır.  Ardından, cihaz bir WebView kullanıcı Arabirimi sunmanızı mümkün değilse, diğer kimlik doğrulama yöntemlerini gereklidir.  Bu SDK'sı böylece izleme türü veya benzer şekilde kısıtlı cihazlar için uygun değil.

## <a name="setup"></a>Kurulum ve Önkoşullar
Zaten oluşturduğunuz ve en az bir tablo içerir, mobil uygulama arka uç projesini yayımlanan olduğunu varsayıyoruz.  Bu konuda kullanılan kodda, tabloda adlı `TodoItem` ve aşağıdaki sütunlar vardır: `Id`, `Text`, ve `Complete`. Bu tablo, tamamladığınızda oluşturulan aynı tablodur [Azure Mobile Apps Hızlı Başlangıç][1].

C# karşılık gelen türü belirlenmiş istemci-tarafı türü aşağıdaki sınıfı şöyledir:

```csharp
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }
}
```

[JsonPropertyAttribute] [ 6] tanımlamak için kullanılan *PropertyName* istemci alanı ve tablo alanı eşleme.

Mobile Apps arka ucunuzu tabloları oluşturmaya ilişkin bilgi edinmek için [.NET sunucu SDK'sı konu] [ 7] veya [Node.js sunucu SDK'sı konu][8]. Hızlı Başlangıç adımlarını kullanarak Azure portalında mobil uygulama arka ucunuzu oluşturduysanız, ayrıca kullanabileceğiniz **kolay tablolar** ayarı [Azure portal].

### <a name="how-to-install-the-managed-client-sdk-package"></a>Nasıl yapılır: Yönetilen istemci SDK paketini yükleme
Mobil uygulamaları için yönetilen istemci SDK paketini yüklemek için aşağıdaki yöntemlerden birini kullanın [NuGet][9]:

* **Visual Studio** projenize sağ tıklayın, **NuGet paketlerini Yönet**, arama `Microsoft.Azure.Mobile.Client` paketini ve ardından tıklayın **yüklemek**.
* **Xamarin Studio** projenize sağ tıklayın, **Ekle** > **NuGet paketleri Ekle**, arama `Microsoft.Azure.Mobile.Client` paketini ve ardından **' paket Ekle** .

Ana etkinlik dosyanıza aşağıdaki unutmayın **kullanarak** deyimi:

```csharp
using Microsoft.WindowsAzure.MobileServices;
```

> [!NOTE]
> Android projenizde başvurulan tüm destek paketlerinin aynı sürüme sahip olması gerektiğini unutmayın. SDK'sını sahip `Xamarin.Android.Support.CustomTabs` projenize yeni kullanıyorsa, destek, paketler için Android platformu için bağımlılık doğrudan çakışmalarını önlemek için gerekli sürümü bu paketi yüklemeniz gerekir.

### <a name="symbolsource"></a>Nasıl Yapılır: Visual Studio'da hata ayıklama sembolleri ile çalışma
Microsoft.Azure.Mobile ad alanı için simgeler kullanılabilir [SymbolSource][10].  Başvurmak [SymbolSource yönergeleri] [ 11] SymbolSource Visual Studio ile tümleştirmek için.

## <a name="create-client"></a>Mobile Apps istemci oluşturma
Aşağıdaki kod oluşturur [MobileServiceClient] [ 12] Mobile App arka ucunuzu erişmek için kullanılan nesne.

```csharp
var client = new MobileServiceClient("MOBILE_APP_URL");
```

Önceki kod içinde `MOBILE_APP_URL` mobil uygulama arka uç URL'si ile bulunduğu dikey penceresinde mobil uygulama arka ucunuzu [Azure portal]. Bir tekliyi MobileServiceClient nesnesi olmalıdır.

## <a name="work-with-tables"></a>Tablolarla çalışma
Aşağıdaki bölümde, arama ve kayıtlarını almak ve tablo içindeki verileri değiştirme işlemi açıklanmaktadır.  Aşağıdaki konular ele alınmaktadır:

* [Bir tablo başvurusu](#instantiating)
* [Verileri sorgulama](#querying)
* [Döndürülen verileri filtreleme](#filtering)
* [Döndürülen verileri sıralama](#sorting)
* [Dönüş verileri sayfalarında](#paging)
* [Belirli sütunları seçin](#selecting)
* [Bir kayıt kimliğine göre arayın](#lookingup)
* [Yazılmamış sorgularla ilgilenme](#untypedqueries)
* [Veri ekleme](#inserting)
* Verileri güncelleştirme
* [Verileri silme](#deleting)
* [Çakışma çözümü ve iyimser eşzamanlılık](#optimisticconcurrency)
* [Bir Windows kullanıcı arabirimine bağlama](#binding)
* [Sayfa boyutunu değiştirme](#pagesize)

### <a name="instantiating"></a>Nasıl Yapılır: Bir tablo başvurusu
Erişen veya bir arka uç tablodaki verileri değiştiren tüm kodlar üzerinde işlevleri çağırır `MobileServiceTable` nesne. Çağırarak tablosuna bir başvurudur elde [GetTable] yöntemini aşağıdaki gibi:

```csharp
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

Döndürülen nesne türü belirtilmiş bir seri hale getirme modeli kullanır. Yazılmamış seri hale getirme modeli de desteklenir. Aşağıdaki örnek [bir başvuru türü belirsiz bir tablo oluşturur]:

```csharp
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

Yazılmamış sorgularda, temel alınan OData sorgu dizesi belirtmeniz gerekir.

### <a name="querying"></a>Nasıl Yapılır: Mobil uygulamanızdan gelen verileri Sorgulama
Bu bölümde, aşağıdaki işlevleri içeren mobil uygulama arka sorguları göndermek amacıyla açıklanmaktadır:

* [Döndürülen verileri filtreleme](#filtering)
* [Döndürülen verileri sıralama](#sorting)
* [Dönüş verileri sayfalarında](#paging)
* [Belirli sütunları seçin](#selecting)
* [Veri Kimliğe göre arayın](#lookingup)

> [!NOTE]
> Bir sunucu tabanlı sayfa boyutu tüm satırları döndürülmesini önlemek için uygulanmaz.  Disk belleği, büyük veri kümeleri için varsayılan istekleri hizmeti olumsuz yönde etkilemesini tutar.  50'den fazla satır döndürmek için `Skip` ve `Take` açıklandığı yöntemi [dönüş verileri sayfalarında](#paging).

### <a name="filtering"></a>Nasıl Yapılır: Döndürülen verileri filtreleme
Aşağıdaki kodu ekleyerek verilerini filtrelemek verilmektedir bir `Where` sorgu yan tümcesi. Tüm öğeleri döndürür `todoTable` olan `Complete` özelliğini eşittir `false`. [Burada] işlevi bir satır tabloya yönelik sorgu koşulu filtreleme uygular.

```csharp
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

Tarayıcının geliştirici araçları gibi ileti İnceleme yazılımları kullanarak arka uca gönderilen isteğin URI'si görüntüleyebilir veya [Fiddler]. İstek URI'si bakarsanız, sorgu dizesi değiştirildiğinde dikkat edin:

```csharp
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

Bu OData isteği SQL sorgusu sunucu SDK'sı tarafından çevrilir:

```csharp
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

Geçirilen işlev `Where` yöntemi, rastgele bir sayıdan koşulları olabilir.

```csharp
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

Bu örnek SQL sorgusu sunucu SDK'sı tarafından çevrilmesi:

```csharp
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

Bu sorgu, birden çok yan tümcesine da bölünebilir:

```csharp
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

İki yöntem eşdeğerdir ve birbirlerinin yerine kullanılabilir.  Önceki seçenek&mdash;bir sorguda birden çok koşullarda bitiştirme,&mdash;daha kompakt ve önerilen.

`Where` Yan tümcesi olan işlemleri destekleyen OData alt kümesini çevrilir. İşlemler şunlardır:

* İlişkisel işleçleri (==,! =, <, < =, >, > =),
* Aritmetik işleçler (+, -, /, *, %),
* Duyarlık (Math.Floor, Math.Ceiling) numarası
* Dize işlevleri (uzunluğu, Substring, Değiştir, IndexOf, StartsWith, EndsWith)
* (Yıl, ay, gün, saat, dakika, saniye), tarih özellikleri
* Bir nesnenin özelliklerine erişim ve
* Bu işlemlerden birini birleştirme ifadeleri.

Sunucu SDK'sı neyi desteklediğine dikkate alındığında, önünde [OData v3 belgeleri].

### <a name="sorting"></a>Nasıl Yapılır: Döndürülen verileri sıralama
Aşağıdaki kodu ekleyerek verileri sıralamak verilmektedir bir [OrderBy] veya [OrderByDescending] sorgu işlevi. Öğeleri döndürür `todoTable` göre artan düzende sıralandı `Text` alan.

```csharp
// Sort items in ascending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderBy(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();

// Sort items in descending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderByDescending(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();
```

### <a name="paging"></a>Nasıl Yapılır: Dönüş verileri sayfalarında
Varsayılan olarak, arka uç yalnızca ilk 50 satır döndürür. Arama döndürülen satır sayısını artırabilir [ele] yöntemi. Kullanım `Take` ile birlikte [atla] belirli bir toplam veri kümesinin "sayfası sorgu tarafından döndürülen" istek için yöntem. Aşağıdaki sorgu yürütüldüğünde, tablodaki ilk üç öğeleri döndürür.

```csharp
// Define a filtered query that returns the top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

Aşağıdaki düzeltilmiş sorgu ilk üç sonuçları atlar ve sonraki üç sonuçlarını döndürür. Bu sorgu, veri, sayfa boyutu üç öğe olduğu ikinci "sayfası" üretir.

```csharp
// Define a filtered query that skips the top 3 items and returns the next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

[IncludeTotalCount] yöntemi istekleri için toplam sayıyı *tüm* , belirtilen herhangi bir disk belleği/sınırlama yan tümcesi yok sayılıyor döndürülen kayıtları:

```csharp
query = query.IncludeTotalCount();
```

Gerçek bir uygulamada, sorgular önceki örneğe benzer bir çağrı cihazı denetimi veya benzer kullanıcı Arabirimi ile sayfalar arasında gezinmek için kullanabilirsiniz.

> [!NOTE]
> Bir mobil uygulama arka ucu 50 satır sınırı geçersiz kılmak için de uygulamanız gerekir [EnableQueryAttribute] genel yöntem elde ve disk belleği davranışı belirtin. Yönteme uygulandığında, aşağıdaki satırları 1000 döndürülen en fazla ayarlar:
>
> `[EnableQuery(MaxTop=1000)]`


### <a name="selecting"></a>Nasıl Yapılır: Belirli sütunları seçin
Özellikleri ekleyerek sonuçların dahil edileceği ayarladığı belirtebileceğiniz bir [Seç] sorgunuzu yan tümcesini. Örneğin, aşağıdaki kod, yalnızca bir alan seçin ve ayrıca seçin ve birden çok alan biçimlendirmek gösterir:

```csharp
// Select one field -- just the Text
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => todoItem.Text);
List<string> items = await query.ToListAsync();

// Select multiple fields -- both Complete and Text info
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => string.Format("{0} -- {1}",
                    todoItem.Text.PadRight(30), todoItem.Complete ?
                    "Now complete!" : "Incomplete!"));
List<string> items = await query.ToListAsync();
```

Şu ana kadar açıklanan tüm işlevleri biz zincirleme bunları tutmak için eklenebilir. Zincirleme her arama daha fazla sorgu etkiler. Bir örnek daha:

```csharp
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <a name="lookingup"></a>Nasıl Yapılır: Veri Kimliğe göre arayın
[LookupAsync] işlevi, veritabanında belirli bir kimliğe sahip nesneleri ara için kullanılabilir

```csharp
// This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <a name="untypedqueries"></a>Nasıl Yapılır: Yazılmamış sorguları yürütme
Yazılmamış tablo nesnesi kullanarak bir sorgu yürütülürken açıkça OData sorgu dizesi çağırarak belirtmelisiniz [ReadAsync], aşağıdaki örnekte olduğu gibi:

```csharp
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

Bir özellik paketi gibi kullanabileceğiniz JSON değerlerinin ulaşırsınız. JToken ve Newtonsoft Json.NET ile ilgili daha fazla bilgi için bkz. [Json.NET] site.

### <a name="inserting"></a>Nasıl Yapılır: Bir mobil uygulama arka ucuna veri ekleme
Tüm istemci türleri adında bir üye içermelidir **kimliği**, olan varsayılan olarak bir dize. Bu **kimliği** CRUD işlemleri gerçekleştirmek için gereken ve çevrimdışı eşitleme. Aşağıdaki kod nasıl kullanılacağını göstermektedir [InsertAsync] tabloya yeni satır eklemek için yöntemi. Parametresi, bir .NET nesnesi olarak eklenmesini verileri içerir.

```csharp
await todoTable.InsertAsync(todoItem);
```

Özel benzersiz bir kimliği değeri yer almayan varsa `todoItem` bir ekleme sırasında sunucu tarafından bir GUID oluşturulur.
Nesne araması döndürdükten sonra inceleyerek oluşturulan kimliği alabilir.

Yazılmayan veri eklemek için Json.NET yararlanmak:

```csharp
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

Benzersiz dize kimliği bir e-posta adresi kullanarak bir örnek aşağıda verilmiştir:

```csharp
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a>Kimliği değerleri ile çalışma
Mobile Apps tablo için benzersiz bir özel dize değerleri destekler **kimliği** sütun. Bir dize değeri uygulamaları e-posta adresleri veya kimliği için kullanıcı adı gibi özel değerler kullanmaya izin verir.  Dize kimlikleri ile aşağıdaki avantajları sağlar:

* Kimlikleri, veritabanına bir gidiş dönüş yapmadan oluşturulur.
* Kayıtları farklı tablolar veya veritabanlarına birleştirme daha kolaydır.
* Kimlikleri değerler daha iyi bir uygulama mantığı ile tümleştirebilirsiniz.

Eklenen bir kayıtla ilgili bir dize kimliği değeri olarak ayarlanmadığında, mobil uygulama arka ucu kimliği için benzersiz bir değer oluşturur. Kullanabileceğiniz [Guid.NewGuid] kendi kimlik değerleri, istemci veya arka uç oluşturmak için yöntemi.

```csharp
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <a name="modifying"></a>Nasıl Yapılır: Bir mobil uygulama arka ucu verileri değiştirme
Aşağıdaki kod nasıl kullanılacağını göstermektedir [UpdateAsync] yeni bilgilerle aynı Kimliğe sahip varolan bir kaydı güncelleştirmek için yöntemi. Parametresi, bir .NET nesnesi olarak güncelleştirilmesi için verileri içerir.

```csharp
await todoTable.UpdateAsync(todoItem);
```

Yazılmayan veri güncelleştirmek için avantajlarından sürebilir [Json.NET] gibi:

```csharp
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

Bir `id` güncelleştirme yapılırken alan belirtilmelidir. Arka uç kullanan `id` güncelleştirmek için hangi satırı tanımlamak için alan. `id` Alan sonuç elde edilebilir `InsertAsync` çağırın. Bir `ArgumentException` sağlamadan bir öğe güncelleştirmeye çalıştığınızda, tetiklenir `id` değeri.

### <a name="deleting"></a>Nasıl Yapılır: Bir mobil uygulama arka ucu verilerini silme
Aşağıdaki kod nasıl kullanılacağını göstermektedir [DeleteAsync] var olan bir örneğini silmek için yöntemi. Örneği tarafından tanımlanır `id` alan kümesinde `todoItem`.

```csharp
await todoTable.DeleteAsync(todoItem);
```

Yazılmayan veri silmek için Json.NET avantajı şu şekilde alabilir:

```csharp
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

Bir silme isteği yaptığınız zaman, bir kimliği belirtilmelidir. Diğer özellikler hizmetine iletilir değil veya hizmetin göz ardı edilir. Sonucu bir `DeleteAsync` çağrıdır, genellikle `null`. Kimliği geçirin sonuç elde edilebilir `InsertAsync` çağırın. A `MobileServiceInvalidOperationException` belirtmeden bir öğe silmeye çalıştığınızda durum `id` alan.

### <a name="optimisticconcurrency"></a>Nasıl Yapılır: İyimser eşzamanlılık çakışma çözümü için kullanın.
İki veya daha fazla istemci değişiklikler, aynı anda aynı öğeye yazabilir. Çakışma algılaması son yazma önceki tüm güncelleştirmelerin üzerine yazacak. **İyimser eşzamanlılık denetimi** her işlem onaylayabilirsiniz ve bu nedenle kaynak kilitleme kullanmaz varsayar.  Bir işlem yapmadan önce iyimser eşzamanlılık denetimi başka bir işlem veri değiştirdi doğrular. Veri değiştirilirse uygulanıyor işlem geri alınır.

Mobile Apps her öğesi kullanarak değişiklikleri izleme iyimser eşzamanlılık denetimini destekleyen `version` Mobile App arka ucunuzu her tablo için tanımlanan sistem özelliğinin sütunu. Bir kayıt güncelleştirildiğinde, her zaman Mobile Apps ayarlar `version` özelliği bu kaydın yeni bir değer. Her güncelleştirme isteği sırasında `version` istekte kaydı özelliği, sunucu kaydı için aynı özelliğe karşılaştırılır. Sürümü ile aktarılırsa isteği arka uç eşleşmiyor ve ardından istemci kitaplığı oluşturur bir `MobileServicePreconditionFailedException<T>` özel durum. Özel durum ile dahil kayıt sunucuları sürümünü içeren arka kayıttan türüdür. Uygulama daha sonra güncelleştirme isteğini yeniden ile doğru yürütmek karar vermek için bu bilgiyi kullanabilirsiniz `version` değişiklikleri işlemek için arka uç değeri.

Bir sütun için tablo sınıfı üzerinde tanımlamak `version` iyimser eşzamanlılık etkinleştirmek için sistem özelliği. Örneğin:

```csharp
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }

    // *** Enable Optimistic Concurrency *** //
    [JsonProperty(PropertyName = "version")]
    public string Version { set; get; }
}
```

Yazılmamış tabloları kullanarak uygulamaları etkinleştirmek iyimser eşzamanlılık ayarlayarak `Version` üzerinde bayrak `SystemProperties` aşağıdaki gibi tablo.

```csharp
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

İyimser eşzamanlılık etkinleştirmenin yanı sıra, ayrıca yakalamalıdır `MobileServicePreconditionFailedException<T>` kodunuzda çağrılırken özel durum [UpdateAsync].  Doğru uygulayarak çakışmayı `version` güncelleştirilen kaydı ve çağrı [UpdateAsync] çözümlenen kayıt. Aşağıdaki kod, bir kez yazma çakışması algılandı çözümlenecek gösterilmektedir:

```csharp
private async void UpdateToDoItem(TodoItem item)
{
    MobileServicePreconditionFailedException<TodoItem> exception = null;

    try
    {
        //update at the remote table
        await todoTable.UpdateAsync(item);
    }
    catch (MobileServicePreconditionFailedException<TodoItem> writeException)
    {
        exception = writeException;
    }

    if (exception != null)
    {
        // Conflict detected, the item has changed since the last query
        // Resolve the conflict between the local and server item
        await ResolveConflict(item, exception.Item);
    }
}


private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
{
    //Ask user to choose the resolution between versions
    MessageDialog msgDialog = new MessageDialog(
        String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
        serverItem.Text, localItem.Text),
        "CONFLICT DETECTED - Select a resolution:");

    UICommand localBtn = new UICommand("Commit Local Text");
    UICommand ServerBtn = new UICommand("Leave Server Text");
    msgDialog.Commands.Add(localBtn);
    msgDialog.Commands.Add(ServerBtn);

    localBtn.Invoked = async (IUICommand command) =>
    {
        // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
        // catching a MobileServicePreConditionFailedException.
        localItem.Version = serverItem.Version;

        // Updating recursively here just in case another change happened while the user was making a decision
        UpdateToDoItem(localItem);
    };

    ServerBtn.Invoked = async (IUICommand command) =>
    {
        RefreshTodoItems();
    };

    await msgDialog.ShowAsync();
}
```

Daha fazla bilgi için [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme] konu.

### <a name="binding"></a>Nasıl Yapılır: Bir Windows kullanıcı arabirimine Mobile Apps veri bağlama
Bu bölümde, bir Windows uygulaması kullanıcı Arabirimi öğeleri kullanarak döndürülen veriler nesne görüntülenecek gösterilmektedir.  Aşağıdaki kod örneği, tamamlanmamış öğeleri için sorgu listesiyle kaynağına bağlar. [MobileServiceCollection] mobil uygulamaları algılayan bir bağlama koleksiyonu oluşturur.

```csharp
// This query filters out completed TodoItems.
MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToCollectionAsync();

// itemsControl is an IEnumerable that could be bound to a UI list control
IEnumerable itemsControl  = items;

// Bind this to a ListBox
ListBox lb = new ListBox();
lb.ItemsSource = items;
```

Adlı bir arabirim yönetilen çalışma zamanındaki bazı denetimler Destek [ISupportIncrementalLoading]. Bu arabirim, ek veriler kullanıcı kaydırdığında istemek denetimleri sağlar. Bu arabirim Evrensel Windows uygulamaları için yerleşik desteği [MobileServiceIncrementalLoadingCollection], hangi otomatik olarak işler denetimleri gelen çağrıları. Kullanım `MobileServiceIncrementalLoadingCollection` aşağıdaki gibi Windows uygulamaları:

```csharp
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

Yeni koleksiyon, Windows Phone 8 ve "Silverlight" uygulamaları kullanmak için `ToCollection` üzerinde genişletme yöntemleri `IMobileServiceTableQuery<T>` ve `IMobileServiceTable<T>`. Verileri yüklemek için çağrı `LoadMoreItemsAsync()`.

```csharp
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

Çağrılarak oluşturulan koleksiyonu kullandığınızda `ToCollectionAsync` veya `ToCollection`, UI denetimine bağlı bir koleksiyonunu Al.  Bu disk belleği kullanan koleksiyonudur.  Koleksiyon verileri ağ üzerinden yüklüyor olduğundan, bazen yükleme başarısız olur. Bu tür hataları işlemek için geçersiz kılma `OnException` metodunda `MobileServiceIncrementalLoadingCollection` çağrıları kaynaklanan özel durumları işlemek için `LoadMoreItemsAsync`.

Tablonuzun birçok alan vardır ancak yalnızca bazıları denetiminizde görüntülemek istediğiniz göz önünde bulundurun. Önceki bölümde yönergeleri kullanabilir "[belirli sütunları seçmek](#selecting)" kullanıcı Arabiriminde görüntülemek için belirli sütunları seçmek için.

### <a name="pagesize"></a>Sayfa boyutunu değiştirme
Azure Mobile Apps, varsayılan olarak en fazla istek başına 50 öğe döndürür.  İstemci ve sunucu üzerinde en fazla sayfa boyutunu artırarak, disk belleği boyutunu değiştirebilirsiniz.  İstenen sayfa boyutunu artırmak için belirtin `PullOptions` kullanırken `PullAsync()`:

```csharp
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

Yapmış olduğunuz varsayılarak `PageSize` eşit veya sunucu içinde 100'den büyük bir isteği en fazla 100 öğeleri döndürür.

## <a name="#offlinesync"></a>Çevrimdışı tablolarla çalışma
Çevrimdışı tabloları çevrimdışıyken kullanım için yerel bir SQLite depolamak için verileri kullanın.  Tüm tablo işlemleri yerel karşı yapılır uzak sunucu deposu yerine SQLite depolayın.  Çevrimdışı bir tablo oluşturmak için önce projenizi hazırlayın:

1. Visual Studio'da çözüme sağ tıklayın > **çözüm için NuGet paketlerini Yönet...** , ardından aramak ve yüklemek **Microsoft.Azure.Mobile.Client.SQLiteStore** Çözümdeki tüm projeleri için NuGet paketi.
2. (İsteğe bağlı) Windows cihazları desteklemek için aşağıdaki SQLite çalışma zamanı paketlerini yükleyin:

   * **Windows 8.1 çalışma zamanı:** Yükleme [Windows 8.1 için SQLite][3].
   * **Windows Phone 8.1:** Yükleme [Windows Phone 8.1 için SQLite][4].
   * **Evrensel Windows platformu** yükleme [Evrensel Windows için SQLite][5].
3. (İsteğe bağlı). Windows cihazlar için tıklatın **başvuruları** > **Başvuru Ekle...** , genişletin **Windows** klasör > **uzantıları**, ardından uygun etkinleştirin **için SQLite Windows** SDK'sını **Windows için visual C++ 2013 çalışma zamanı** SDK.
    SQLite SDK adları, her Windows platformuyla biraz farklılık gösterir.

Bir tablo başvurusu oluşturulabilmesi için önce yerel depo hazırlanması gerekir:

```csharp
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes the SyncContext using the default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

İstemci anında oluşturulduktan sonra Store başlatma normal olarak gerçekleştirilir.  **OfflineDbPath** bir dosya adı, destek, tüm platformlarda kullanım için uygun olmalıdır.  Yol tam nitelenmiş bir yol ise (diğer bir deyişle, eğik çizgi ile başlar), bu yolu kullanılır.  Yol tam olarak nitelenmiş değil, dosyanın bir platforma özgü konuma yerleştirilir.

* İOS ve Android cihazlar için varsayılan "Kişisel Files" klasörü yoludur.
* Windows cihazlar için varsayılan yol uygulamaya özgü "AppData" klasörüdür.

Bir tablo başvurusu kullanarak elde edilebilir `GetSyncTable<>` yöntemi:

```csharp
var table = client.GetSyncTable<TodoItem>();
```

Çevrimdışı bir tablosunu kullanmak için kimlik doğrulaması gerekmez.  Arka uç hizmetiyle iletişim kurduğunda kimliğini doğrulamak yeterlidir.

### <a name="syncoffline"></a>Çevrimdışı bir tablo eşitleniyor
Çevrimdışı tabloları, varsayılan olarak arka uç ile eşitlenmez.  Eşitleme, iki parçalara bölünür.  Yeni öğeler indirmesini değişiklikleri ayrı olarak gönderebilirsiniz.  Tipik bir eşitleme yöntemi aşağıda verilmiştir:

```csharp
public async Task SyncAsync()
{
    ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

    try
    {
        await this.client.SyncContext.PushAsync();

        await this.todoTable.PullAsync(
            //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
            //Use a different query name for each unique query in your program
            "allTodoItems",
            this.todoTable.CreateQuery());
    }
    catch (MobileServicePushFailedException exc)
    {
        if (exc.PushResult != null)
        {
            syncErrors = exc.PushResult.Errors;
        }
    }

    // Simple error/conflict handling. A real application would handle the various errors like network conditions,
    // server conflicts and others via the IMobileServiceSyncHandler.
    if (syncErrors != null)
    {
        foreach (var error in syncErrors)
        {
            if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
            {
                //Update failed, reverting to server's copy.
                await error.CancelAndUpdateItemAsync(error.Result);
            }
            else
            {
                // Discard local change.
                await error.CancelAndDiscardItemAsync();
            }

            Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
        }
    }
}
```

İlk bağımsız değişkeni `PullAsync` null, artımlı eşitleme değil kullanılır.  Her eşitleme işlemi, tüm kayıtları alır.

SDK'sı örtük gerçekleştirir `PushAsync()` kayıtları çekmeden önce.

Çakışma işleme olur üzerinde bir `PullAsync()` yöntemi.  Çakışıyor Online'a tablolar aynı şekilde giderebilirsiniz.  Çakışma üretilen olduğunda `PullAsync()` INSERT, update veya delete sırasında yerine olarak adlandırılır. Birden çok çakışma varsa, bunlar tek bir MobileServicePushFailedException paketlenir.  Her hata ayrı olarak işleyin.

## <a name="#customapi"></a>Özel bir API ile çalışma
Özel API eşlemek için bir ekleme, güncelleştirme, silme, veya okuma işlemi sunucusu işlevselliği kullanıma sunan özel uç noktalar tanımlamanızı sağlar. Özel API kullanarak okuma ve HTTP ileti üstbilgilerini ayarlama ve ileti gövdesi biçimi JSON dışında tanımlama gibi Mesajlaşma hakkında daha fazla denetime sahip olabilir.

Aşağıdakilerden birini çağırarak özel bir API çağrısı [InvokeApiAsync] istemci üzerinde yöntemleri. Örneğin, aşağıdaki kod satırını bir POST isteği gönderir **completeAll** arka uç API:

```javascript
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

Bu form türü belirtilmiş yöntem çağrısı ve gerektiren **MarkAllResult** dönüş türü tanımlanmıştır. Yazılan ve yazılmayan yöntemleri desteklenir.

InvokeApiAsync() yöntemi '/ api /' ile API başlatana kadar çağırmak istediğiniz API başına bir '/'.
Örneğin:

* `InvokeApiAsync("completeAll",...)` arka uçta /api/completeAll çağırır
* `InvokeApiAsync("/.auth/me",...)` arka uçta /.auth/Me çağırır

InvokeApiAsync tanımlanmamışsa bu WebAPIs Azure Mobile Apps ile de dahil olmak üzere herhangi bir Webapı çağırmak için kullanabilirsiniz.  InvokeApiAsync() kullandığınızda, kimlik doğrulama üst bilgileri, dahil olmak üzere uygun üst bilgiler, istekle birlikte gönderilir.

## <a name="authentication"></a>Kullanıcıların kimlik doğrulaması
Mobile Apps kimlik doğrulaması ve yetkilendirme çeşitli dış kimlik sağlayıcısı kullanarak uygulama kullanıcılarının destekler: Facebook, Google, Microsoft hesabı, Twitter ve Azure Active Directory. Tablolarda yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi sınırlandırmak için izinleri ayarlayabilirsiniz. Sunucu betiklerini yetkilendirme kurallarını uygulamak için kimliği doğrulanmış kullanıcıların kimliğini de kullanabilirsiniz. Daha fazla bilgi için [Uygulamanıza kimlik doğrulaması ekleme] öğreticisine bakın.

İki kimlik doğrulama akışı desteklenir: *yönetilen* ve *sunucusu yönetilen* akış. Sağlayıcının web kimlik doğrulaması arabirimde alacağından sunucusu yönetilen akış Basit kimlik doğrulaması deneyimi sağlar. Sağlayıcıya özgü cihaza özgü dillerde alacağından yönetilen akış cihaza özgü özellikleri ile daha derin tümleştirme sağlar.

> [!NOTE]
> Yönetilen bir akışı üretim uygulamalarınızda kullanmanızı öneririz.

Kimlik doğrulamasını ayarlamak için bir veya daha fazla kimlik sağlayıcıları ile uygulamanızı kaydetmeniz gerekir.  Kimlik sağlayıcısı, bir istemci kimliği ve uygulamanız için bir istemci gizli anahtarı oluşturur.  Bu değerler, Azure App Service kimlik doğrulama/yetkilendirme etkinleştirmek için arka ucunuzu sonra ayarlanır.  Daha fazla bilgi için aşağıdaki ayrıntılı yönergeleri öğreticide izleyin [Uygulamanıza kimlik doğrulaması ekleme].

Bu bölümde aşağıdaki konular ele alınmaktadır:

* [Yönetilen kimlik doğrulaması](#clientflow)
* [Sunucu yönetilen kimlik doğrulaması](#serverflow)
* [Kimlik doğrulama belirteci önbelleğe alma](#caching)

### <a name="clientflow"></a>Yönetilen kimlik doğrulaması
Uygulamanız, bağımsız olarak kimlik sağlayıcısına başvurun ve sonra döndürülen belirteci ile arka ucunuzu oturum açma sırasında sağlayın. Bu istemci akışı, kullanıcılara çoklu oturum açma deneyimini sağlamak için veya kimlik sağlayıcısından ek kullanıcı verilerini almak için sağlar. İstemci akışı kimlik doğrulaması kimlik sağlayıcısına SDK daha doğal bir UX görünümünü sağlar ve ek özelleştirmesi sağlayan bir sunucu akışı kullanmaya tercih edilir.

Örnekler için aşağıdaki istemci akışı kimlik doğrulaması desenleri verilmiştir:

* [Active Directory kimlik doğrulama kitaplığı](#adal)
* [Facebook veya Google](#client-facebook)

#### <a name="adal"></a>Kullanıcıların Active Directory kimlik doğrulama kitaplığı ile kimlik doğrulaması
Azure Active Directory kimlik doğrulamasını kullanarak istemciden başlatma kullanıcı kimlik doğrulaması için Active Directory Authentication Library (ADAL) kullanabilirsiniz.

1. AAD oturum açma için mobil uygulama arka ucunuzu izleyerek yapılandırın [App Service, Active Directory oturum açma için yapılandırma] öğretici. Yerel istemci uygulaması kaydetme isteğe bağlı bir adım tamamladığınızdan emin olun.
2. Visual Studio veya Xamarin Studio, projenizi açın ve bir başvuru ekleyin `Microsoft.IdentityModel.Clients.ActiveDirectory` NuGet paketi. Arama yaparken, yayın öncesi sürümlerini içerir.
3. Kullandığınız platform göre uygulamanız için aşağıdaki kodu ekleyin. Her, aşağıdaki değişiklikleri yapın:

   * Değiştirin **INSERT yetkilisi burada** uygulamanızı sağlanan Kiracı adı. Biçim olmalıdır https://login.microsoftonline.com/contoso.onmicrosoft.com. Bu değer, Azure Active Directory etki alanı sekmesinden kopyalanabilir [Azure portal].
   * Değiştirin **Ekle-RESOURCE-kimliği-Buraya** mobil uygulamanızın arka ucu için istemci kimliği. İstemci kimliği edinebilirsiniz **Gelişmiş** sekmesinde altında **Azure Active Directory ayarları** portalında.
   * Değiştirin **istemci kimliği burayı INSERT** yerel istemci uygulamasından kopyaladığınız istemci kimliği.
   * Değiştirin **ekleme-yeniden yönlendirme-URI-Buraya** sitenizin ile */.auth/login/done* uç noktasını, HTTPS düzenini kullanarak. Bu değer, aşağıdakine benzer olmalıdır *https://contoso.azurewebsites.net/.auth/login/done*.

     Her platform için gereken kod aşağıdaki gibidir:

     **Windows:**

     ```csharp
     private MobileServiceUser user;
     private async Task AuthenticateAsync()
     {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        while (user == null)
        {
            string message;
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await App.MobileService.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
     }
     ```

     **Xamarin.iOS**

     ```csharp
     private MobileServiceUser user;
     private async Task AuthenticateAsync(UIViewController view)
     {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(view));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
        }
     }
     ```

     **Xamarin.Android**

     ```csharp
     private MobileServiceUser user;
     private async Task AuthenticateAsync()
     {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(this));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(ex.Message);
            builder.SetTitle("You must log in. Login Required");
            builder.Create().Show();
        }
     }
     protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
     {

        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
     }
     ```

#### <a name="client-facebook"></a>Tek bir belirteç Facebook veya Google kullanarak oturum açmayı
İstemci akış, Facebook veya Google için bu kod parçacığında gösterildiği gibi kullanabilirsiniz.

```csharp
var token = new JObject();
// Replace access_token_value with actual value of your access token obtained
// using the Facebook or Google SDK.
token.Add("access_token", "access_token_value");

private MobileServiceUser user;
private async Task AuthenticateAsync()
{
    while (user == null)
    {
        string message;
        try
        {
            // Change MobileServiceAuthenticationProvider.Facebook
            // to MobileServiceAuthenticationProvider.Google if using Google auth.
            user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
            message = string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

### <a name="serverflow"></a>Sunucu yönetilen kimlik doğrulaması
Kimlik sağlayıcınızı kaydettikten sonra çağırma [LoginAsync] [MobileServiceClient] ile metodunda [MobileServiceAuthenticationProvider] sağlayıcınızın değeri. Örneğin, aşağıdaki kod bir sunucu akışı oturum açma Facebook kullanarak başlatır.

```csharp
private MobileServiceUser user;
private async System.Threading.Tasks.Task Authenticate()
{
    while (user == null)
    {
        string message;
        try
        {
            user = await client
                .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
            message =
                string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, değiştirin [MobileServiceAuthenticationProvider] sağlayıcınız için değer.

Azure App Service, sunucu akışı, OAuth kimlik doğrulaması akışı seçili sağlayıcının oturum açma sayfası görüntüleyerek yönetir.  Kimlik sağlayıcısı döndürür, Azure App Service oluşturur sonra bir App Service kimlik doğrulama belirteci. [LoginAsync] yöntemi döndürür bir [MobileServiceUser], her ikisi de sağlayan [Kullanıcı Kimliği] kimliği doğrulanmış kullanıcının ve [MobileServiceAuthenticationToken], JSON web Token (JWT). Bu belirteç önbelleğe alınabilir süresi sona erene kadar yeniden kullanılabilir. Daha fazla bilgi için [kimlik doğrulama belirteci önbelleğe alma](#caching).

### <a name="caching"></a>Kimlik doğrulama belirteci önbelleğe alma
Bazı durumlarda, oturum açma yöntemi çağrısı sağlayıcıdan kimlik doğrulaması belirteci depolayarak ilk başarılı kimlik doğrulamasından sonra önlenebilir.  Microsoft Store ve UWP uygulamaları kullanabilir [PasswordVault] gibi geçerli kimlik doğrulama belirtecini bir başarılı oturum açma işleminden sonra önbelleğe almak için:

```csharp
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

UserId değer kimlik bilgisini kullanıcı adı olarak depolanır ve parola olarak depolanan bir belirteçtir. Sonraki yeni üzerinde denetleyebilirsiniz **PasswordVault** önbelleğe alınan kimlik bilgileri. Aşağıdaki örnek bulunur ve aksi takdirde arka uç ile yeniden kimlik doğrulaması girişimlerini önbelleğe alınmış kimlik bilgilerini kullanır:

```csharp
// Try to retrieve stored credentials.
var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
if (creds != null)
{
    // Create the current user from the stored credentials.
    client.currentUser = new MobileServiceUser(creds.UserName);
    client.currentUser.MobileServiceAuthenticationToken =
        vault.Retrieve("Facebook", creds.UserName).Password;
}
else
{
    // Regular login flow and cache the token as shown above.
}
```

Bir kullanıcının oturumunu kapatmaz, ayrıca depolanan kimlik bilgileri gibi kaldırmanız gerekir:

```csharp
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

Xamarin uygulamaları kullanım [Xamarin.Auth] API'leri güvenli bir şekilde kimlik bilgilerini depolamak için bir **hesabı** nesne. Bu API'leri kullanarak bir örnek için bkz: [AuthStore.cs] kod dosyasında [örnek paylaşımı ContosoMoments fotoğraf](https://github.com/azure-appservice-samples/ContosoMoments).

Yönetilen kimlik doğrulaması kullandığınızda, Facebook veya Twitter gibi sağlayıcınızdan alınan erişim belirteci de önbelleğe alabilir. Yeni bir kimlik doğrulama belirteci arka ucundan gibi istemek için bu belirteci sağlanabilir:

```csharp
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using the access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <a name="pushnotifications"></a>Anında iletme bildirimleri gönderme
Aşağıdaki konular, anında iletme bildirimleri kapsar:

* [Anında iletme bildirimleri için kaydolun](#register-for-push)
* [Microsoft Store paket SID'si alın](#package-sid)
* [Platformlar arası şablonları ile kaydetme](#register-xplat)

### <a name="register-for-push"></a>Nasıl Yapılır: Anında iletme bildirimleri için kaydolun
Mobile Apps istemci, Azure Notification Hubs ile anında iletme bildirimlerine kaydetmenizi sağlar. Kaydederken, platforma özgü anında iletme bildirimi hizmeti (PNS) öğesinden elde bir tanıtıcı edinin. Kayıt oluşturduğunuzda ardından herhangi bir etiket yanı sıra bu değeri sağlayın. Aşağıdaki kod Windows bildirim Hizmeti'ni (WNS) ile anında iletme bildirimleri için uygulamanızı Windows kaydeder:

```csharp
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using the new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

WNS'ye zorlayan sonra yapmanız gerekenler [Microsoft Store paket SID'si elde](#package-sid).  Şablon kayıtlar için nasıl dahil olmak üzere Windows uygulamaları hakkında daha fazla bilgi için bkz: [Uygulamanıza anında iletme bildirimleri ekleme].

İstemciden etiketleri isteyen desteklenmiyor.  Etiket istekleri sessizce kaydından bırakılır.
Etiketlerle cihazını kaydetmek istiyorsanız, kayıt sizin adınıza gerçekleştirmesi için bildirim hub'ları API kullanan özel API oluşturun.  Özel API yerine çağırma `RegisterNativeAsync()` yöntemi.

### <a name="package-sid"></a>Nasıl Yapılır: Microsoft Store paket SID'si alın
Paket SID'si Microsoft Store uygulamalarında anında iletme bildirimleri etkinleştirmek için gereklidir.  Paket SID'si almak için Microsoft Store ile kaydedin.

Bu değeri elde etmek için:

1. Microsoft Store uygulaması projesi Visual Studio Çözüm Gezgini'nde sağ tıklayın, **Store** > **uygulamayı Store ile ilişkilendir...** .
2. Sihirbazı'nda tıklatın **sonraki**, Microsoft hesabınızla oturum açın, uygulamanız için bir ad yazın **yeni bir uygulama adı ayrılmaya**, ardından **ayırma**.
3. Uygulama kaydı başarıyla oluşturulduktan sonra uygulama adı seçin, **sonraki**ve ardından **ilişkilendirmek**.
4. Oturum [Windows Geliştirme Merkezi] Microsoft Account kullanarak. Altında **uygulamalarım**, oluşturduğunuz uygulama kaydı tıklayın.
5. Tıklayın **Uygulama Yönetimi** > **uygulama kimliği**ve ardından bulma aşağı kaydırın, **paket SID'si**.

Paket SID'si birçok kullanımı, bir URI olarak kullanmanız gerekir ve bu durumda kabul *ms-app: / /* düzeni olarak. Bir ön ek olarak bu değer ile birleştirerek biçimlendirilmiş SID, paketin sürümü not edin.

Xamarin uygulamaları iOS veya Android platformları üzerinde çalışan bir uygulamayı kaydetme yapabilmek için bazı ek kod gerektirir. Daha fazla bilgi için platformunuza yönelik konuya bakın:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <a name="register-xplat"></a>Nasıl Yapılır: Çapraz platform bildirimleri göndermek için kayıt anında iletme şablonları
Şablonları kaydetmek için kullanın `RegisterAsync()` aşağıdaki gibi şablonlarla yöntemi:

```csharp
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

Şablonlarınızı olmalıdır `JObject` türleri ve birden fazla şablon içinde şu JSON biçimini içerebilir:

```csharp
public JObject myTemplates()
{
    // single template for Windows Notification Service toast
    var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

    var templates = new JObject
    {
        ["generic-message"] = new JObject
        {
            ["body"] = template,
            ["headers"] = new JObject
            {
                ["X-WNS-Type"] = "wns/toast"
            },
            ["tags"] = new JArray()
        },
        ["more-templates"] = new JObject {...}
    };
    return templates;
}
```

Yöntem **RegisterAsync()** ikincil kutucuk de kabul eder:

```csharp
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

Tüm etiketleri yerine güvenlik için kayıt sırasında kaldırılır. Yüklemeleri veya yüklemeleri şablonlarında etiket eklemek için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma].

Kayıtlı bu şablonları kullanarak bildirim göndermek için başvurmak [Bildirim hub'ları API'leri].

## <a name="misc"></a>Çeşitli konular
### <a name="errors"></a>Nasıl Yapılır: Hataları işleme
Arka uçtaki bir hata oluştuğunda, SDK'sını istemcinin oluşturduğu bir `MobileServiceInvalidOperationException`.  Aşağıdaki örnekte, arka uç tarafından döndürülen bir özel durumu işlemek üzere gösterilmektedir:

```csharp
private async void InsertTodoItem(TodoItem todoItem)
{
    // This code inserts a new TodoItem into the database. When the operation completes
    // and App Service has assigned an Id, the item is added to the CollectionView
    try
    {
        await todoTable.InsertAsync(todoItem);
        items.Add(todoItem);
    }
    catch (MobileServiceInvalidOperationException e)
    {
        // Handle error
    }
}
```

Başka bir örnek hata koşulları uğraşmanızı bulunabilir [Mobil uygulamalar dosyaları örnek]. [LoggingHandler] örnek arka uç için yapılan isteklerini günlüğe kaydetmek için günlüğe kaydetme temsilci işleyicisi sağlar.

### <a name="headers"></a>Nasıl Yapılır: İstek üstbilgilerini özelleştirme
Belirli uygulama senaryonuzu desteklemek için mobil uygulama arka ucu ile iletişim özelleştirmek gerekebilir. Örneğin, her bir giden istek için özel bir başlık ekleyin veya hatta yanıt durum kodları değiştirmek isteyebilirsiniz. Özel bir kullanabileceğiniz [DelegatingHandler], aşağıdaki örnekte olduğu gibi:

```csharp
public async Task CallClientWithHandler()
{
    MobileServiceClient client = new MobileServiceClient("AppUrl", new MyHandler());
    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
    var newItem = new TodoItem { Text = "Hello world", Complete = false };
    await todoTable.InsertAsync(newItem);
}

public class MyHandler : DelegatingHandler
{
    protected override async Task<HttpResponseMessage>
        SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        // Change the request-side here based on the HttpRequestMessage
        request.Headers.Add("x-my-header", "my value");

        // Do the request
        var response = await base.SendAsync(request, cancellationToken);

        // Change the response-side here based on the HttpResponseMessage

        // Return the modified response
        return response;
    }
}
```


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: https://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: https://github.com/SymbolSource/SymbolSource
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Uygulamanıza kimlik doğrulaması ekleme]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[Uygulamanıza anında iletme bildirimleri ekleme]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Register your app to use a Microsoft account login]: ../app-service/configure-authentication-provider-microsoft.md
[App Service, Active Directory oturum açma için yapılandırma]: ../app-service/configure-authentication-provider-aad.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/library/azure/jj554275(v=azure.10).aspx
[bir başvuru türü belirsiz bir tablo oluşturur]: https://msdn.microsoft.com/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/library/azure/mt691741(v=azure.10).aspx
[ele]: https://msdn.microsoft.com/library/azure/dn250574(v=azure.10).aspx
[Seç]: https://msdn.microsoft.com/library/azure/dn250569(v=azure.10).aspx
[Atla]: https://msdn.microsoft.com/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/library/azure/dn250536.(v=azure.10)aspx
[Kullanıcı Kimliği]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Burada]: https://msdn.microsoft.com/library/azure/dn250579(v=azure.10).aspx
[Azure portal]: https://portal.azure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: https://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows Geliştirme Merkezi]: https://dev.windows.com/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[PasswordVault]: https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: https://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Bildirim hub'ları API'leri]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobil uygulamalar dosyaları örnek]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 belgeleri]: https://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: https://www.telerik.com/fiddler
[Json.NET]: https://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
