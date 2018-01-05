---
title: "App Service Mobile Apps yönetilen istemci kitaplığı ile çalışma (Windows | Microsoft Docs"
description: "Azure App Service Mobile Apps ile Windows ve Xamarin uygulamaları için .NET istemcisi kullanmayı öğrenin."
services: app-service\mobile
documentationcenter: 
author: conceptdev
manager: crdun
editor: 
ms.assetid: 0280785c-e027-4e0d-aaf2-6f155e5a6197
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2017
ms.author: crdun
ms.openlocfilehash: a92fc21881375989f4ebd192c2c42e419e7aee59
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Azure Mobile Apps için yönetilen istemci kullanma
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz gerçekleştirmek için Azure App Service Mobile uygulamaları Windows ve Xamarin uygulamaları için yönetilen istemci kitaplığı kullanılarak yaygın senaryolar gösterilmektedir. Mobile Apps yeniyseniz, ilk Tamamlanıyor düşünmelisiniz [Azure Mobile Apps quickstart] [ 1] Öğreticisi. Bu kılavuzda, istemci-tarafı yönetilen SDK odaklanın. Mobil uygulamaları için sunucu tarafı SDK'ları hakkında daha fazla bilgi edinmek için belgelerine bakın [.NET Server SDK] [ 2] veya [Node.js sunucusu SDK][3].

## <a name="reference-documentation"></a>Başvuru belgeleri
İstemci SDK'sı için başvuru belgeleri burada bulunur: [Azure Mobile Apps .NET istemci başvurusu][4].
Çeşitli istemci örnekleri de bulabilirsiniz [Azure-Samples GitHub deposunu][5].

## <a name="supported-platforms"></a>Desteklenen platformlar
.NET platformu aşağıdaki platformları destekler:

* Xamarin Android API 19 için 24 (KitKat Nougat aracılığıyla) aracılığıyla yayımlar.
* Xamarin iOS iOS 8.0 ve sonraki sürümler için serbest bırakır
* Evrensel Windows platformu
* Windows Phone 8.1
* Windows Phone 8.0 Silverlight uygulamalarının dışında

"Sunucu akış" kimlik doğrulaması bir Web görünümü için sunulan kullanıcı arabirimini kullanır.  Cihaz WebView Arabirim sunmak mümkün değilse, diğer kimlik doğrulama yöntemlerini gereklidir.  Bu SDK böylece Gözcü türü veya benzer şekilde kısıtlı aygıtlar için uygun değil.

## <a name="setup"></a>Kurulum ve Önkoşullar
Zaten oluşturulmuş ve en az bir tablo içeren mobil uygulama arka uç projeniz, yayımlanan olduğunu varsayıyoruz.  Bu konuda kullanılan kodda tablo adlı `TodoItem` ve aşağıdaki sütunları içerir: `Id`, `Text`, ve `Complete`. Bu tablo, tamamladığınızda oluşturulan aynı tablodur [Azure Mobile Apps quickstart][1].

Karşılık gelen yazılan istemci-tarafı C# aşağıdaki sınıf türüdür:

```
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }
}
```

[JsonPropertyAttribute] [ 6] tanımlamak için kullanılan *PropertyName* istemci alanı ve tablo alanı arasında eşleme.

Tablolar, Mobile Apps arka oluşturmayı öğrenmek için bkz: [.NET Server SDK konu] [ 7] veya [Node.js sunucusu SDK konusuna][8]. Hızlı Başlangıç'ı kullanarak Azure portalında mobil uygulama arka oluşturduysanız de kullanabilirsiniz **kolay tablolar** ayarı [Azure portal].

### <a name="how-to-install-the-managed-client-sdk-package"></a>Nasıl yapılır: yönetilen istemci SDK paketini yükle
Mobil uygulamaları için yönetilen istemci SDK paketini yüklemek için aşağıdaki yöntemlerden birini kullanın [NuGet][9]:

* **Visual Studio** projenize sağ tıklayın, **NuGet paketlerini Yönet**, arama `Microsoft.Azure.Mobile.Client` paketini ve ardından **yükleme**.
* **Xamarin Studio** projenize sağ tıklayın, **Ekle** > **NuGet paketleri Ekle**, arama `Microsoft.Azure.Mobile.Client `paketini ve ardından **Paketi Ekle**.

Ana etkinlik dosyanızda aşağıdaki eklemeyi unutmayın **kullanarak** deyimi:

```
using Microsoft.WindowsAzure.MobileServices;
```

### <a name="symbolsource"></a>Nasıl yapılır: Visual Studio'da hata ayıklama simgeleri ile çalışma
Microsoft.Azure.Mobile ad alanı simgelerini kullanılabilir [SymbolSource][10].  Başvurmak [SymbolSource yönergeleri] [ 11] SymbolSource Visual Studio ile tümleştirmek için.

## <a name="create-client"></a>Mobile Apps istemci oluşturma
Aşağıdaki kod oluşturur [MobileServiceClient] [ 12] , mobil uygulamanızın arka ucuna erişmek için kullanılan nesne.

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

Önceki kodla `MOBILE_APP_URL` mobil uygulama arka ucu URL ile hangi bulunursa, mobil uygulama arka ucuna dikey [Azure portal]. MobileServiceClient nesne singleton olmalıdır.

## <a name="work-with-tables"></a>Tablolarla çalışma
Aşağıdaki bölümde aramak ve kayıtları almak ve tablodaki verileri değiştirmek nasıl ayrıntılarını verir.  Aşağıdaki konular ele alınmaktadır:

* [Bir tablo başvurusu oluşturma](#instantiating)
* [Verileri sorgulama](#querying)
* [Döndürülen veri filtreleme](#filtering)
* [Döndürülen veriler sıralama](#sorting)
* [Dönüş verileri sayfalarında](#paging)
* [Belirli sütunları seçin](#selecting)
* [Bir kayıt kimliğine göre arama](#lookingup)
* [Türsüz sorgularıyla ele alma](#untypedqueries)
* [Veri ekleme](#inserting)
* [Verileri güncelleştirme](#updating)
* [Verileri silme](#deleting)
* [Çakışma çözümü ve iyimser eşzamanlılık](#optimisticconcurrency)
* [Bir Windows kullanıcı arabirimini bağlaması](#binding)
* [Sayfa boyutunu değiştirme](#pagesize)

### <a name="instantiating"></a>Nasıl yapılır: bir tablo başvurusu oluşturma
Erişen ve arka uç tablodaki verileri değiştiren tüm kod üzerinde işlevleri çağırır `MobileServiceTable` nesnesi. Bir başvuru tablosu çağırarak elde [GetTable] şekilde yöntemi:

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

Döndürülen nesne yazılan serileştirme modeli kullanır. Türsüz serileştirme modeli de desteklenir. Aşağıdaki örnek [bir başvuru türü belirsiz bir tablo oluşturur]:

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

Türsüz sorgularda, temel alınan OData sorgu dizesi belirtmeniz gerekir.

### <a name="querying"></a>Nasıl yapılır: Mobil uygulamanızın veri sorgulama
Bu bölüm, aşağıdaki işlevleri içerir mobil uygulama arka ucuna sorguları göndermek amacıyla açıklar:

* [Döndürülen veri filtreleme](#filtering)
* [Döndürülen veriler sıralama](#sorting)
* [Dönüş verileri sayfalarında](#paging)
* [Belirli sütunları seçin](#selecting)
* [Veri Kimliğe göre arayın](#lookingup)

> [!NOTE]
> Bir sunucu tabanlı sayfa boyutu tüm satırları döndürülmesini önlemek için uygulanır.  Disk belleği, hizmet olumsuz etkileyen büyük veri kümeleri için varsayılan istekleri tutar.  50'den fazla satır döndürülecek kullanmak `Skip` ve `Take` açıklandığı gibi yöntemi [veri sayfalarında dönmek](#paging).

### <a name="filtering"></a>Nasıl yapılır: Filtre veri döndürdü
Aşağıdaki kod dahil ederek verilerini filtrelemek nasıl gösterir bir `Where` sorgu yan tümcesi. Tüm öğeleri döndürür `todoTable` , `Complete` özelliği eşittir `false`. [Burada] işlevi, Tablo sorgusu koşulu filtreleme bir satır uygular.

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

Tarayıcının geliştirici araçları gibi ileti denetleme yazılımı kullanarak arka ucuna gönderilen istek URI'sini görüntüleyebilir veya [Fiddler]. İstek URI'si bakarsanız, sorgu dizesi değiştirildiğini dikkat edin:

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

Bu OData isteği Server SDK'sı tarafından bir SQL sorgusu çevrilir:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

Geçirilir işlevi `Where` yöntemi koşulları rastgele bir sayı olabilir.

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

Bu örnek bir SQL sorgusu Server SDK'sı tarafından çevrilmesi:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

Bu sorgu, birden çok yan tümcesine da bölünebilir:

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

İki yöntem eşdeğerdir ve birbirlerinin yerine kullanılabilir.  Önceki seçenek&mdash;bir sorgu birden fazla koşullarında birleştirme,&mdash;daha compact ve önerilen.

`Where` Yan tümcesi olması işlemlerini destekleyen OData alt kümesini çevrilir. İşlemler şunları içerir:

* İlişkisel işleçler (==,! =, <, < =, >, > =),
* Aritmetik işleçler (+, -, /, *, %),
* Precision (Math.Floor, Math.Ceiling), numara
* Dize işlevleri (Uzunluk, Substring, Değiştir, IndexOf, StartsWith, EndsWith)
* Tarih özellikleri (yıl, ay, gün, saat, dakika, saniye)
* Erişim, nesne özelliklerini ve
* Bu işlemleri birleştirme ifadeler.

Ne Server SDK'yı destekleyen değerlendirirken düşünebilirsiniz [OData v3 belgelerine].

### <a name="sorting"></a>Nasıl yapılır: sıralama döndürülen verileri
Aşağıdaki kod ekleyerek verileri sıralamak nasıl gösterir bir [OrderBy] veya [OrderByDescending] sorgusunda işlevi. Öğelerden döndürür `todoTable` göre artan `Text` alan.

```
// Sort items in ascending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderBy(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();

// Sort items in descending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderByDescending(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();
```

### <a name="paging"></a>Nasıl yapılır: veri sayfalarında Döndür
Varsayılan olarak, arka uç yalnızca ilk 50 satırları döndürür. Çağıran döndürülen satır sayısını artırabilirsiniz [ele] yöntemi. Kullanım `Take` ile birlikte [atla] belirli bir toplam veri kümesi "sayfası sorgu tarafından döndürülen" istek yöntemi. Aşağıdaki sorgu çalıştırıldığında, tablodaki ilk üç öğeleri döndürür.

```
// Define a filtered query that returns the top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

Aşağıdaki değiştirilen sorgu ilk üç sonuçları atlar ve sonraki üç sonuçları döndürür. Bu sorgu burada üç öğe sayfa boyutu, verilerin ikinci "sayfasında" üretir.

```
// Define a filtered query that skips the top 3 items and returns the next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

[IncludeTotalCount] yöntemi istekleri için toplam sayıyı *tüm* , belirtilen herhangi bir disk belleği/sınırı koşul yoksayılıyor verilinceye kaydeder:

```
query = query.IncludeTotalCount();
```

Gerçek dünya uygulamada, sorguları önceki örneğe benzer bir çağrı cihazı denetim veya karşılaştırılabilir UI ile sayfaları arasında gezinmek için kullanabilirsiniz.

> [!NOTE]
> Bir mobil uygulama arka ucu, 50-satır sınırı geçersiz kılmak için de uygulamanız gerekir [EnableQueryAttribute] genel alma yöntemini ve disk belleği davranışı belirtin. Yönteme uygulandığında, aşağıdaki satırları 1000 döndürülen en fazla ayarlar:
>
> `[EnableQuery(MaxTop=1000)]`


### <a name="selecting"></a>Nasıl yapılır: Belirli sütunları seçin
Kullanılacak özelliklerini ekleyerek sonuçlarında kümesini belirtebilirsiniz bir [seçin] sorgunuzu yan tümcesini. Örneğin, yalnızca bir alan seçin ve ayrıca seçmek ve birden çok alan biçimlendirmek aşağıdaki kodu gösterir:

```
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

Şimdiye kadar anlatılan tüm işlevleri toplamalı biz zincirleme bunları tutmak,. Her zincirleme arama sorgusunun daha fazla etkiler. Daha fazla örnek:

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <a name="lookingup"></a>Nasıl yapılır: veri Kimliğe göre arayın
[LookupAsync] işlevi, belirli bir kimliğe sahip veritabanından nesneleri aramak için kullanılabilir

```
// This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <a name="untypedqueries"></a>Nasıl yapılır: türsüz sorgularını Yürüt
Türsüz tablo nesneyi kullanarak bir sorgu yürütülürken açıkça OData sorgu dizesi çağırarak belirtmeniz gerekir [ReadAsync], aşağıdaki örnekte olduğu gibi:

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

Bir özellik paketi gibi kullandığınız JSON değerleri ulaşırsınız. JToken ve Newtonsoft Json.NET hakkında daha fazla bilgi için bkz: [Json.NET] site.

### <a name="inserting"></a>Nasıl yapılır: bir mobil uygulama arka ucu veri Ekle
Tüm istemci türleri adlı bir üye içermelidir **kimliği**, olduğu varsayılan olarak bir dize. Bu **kimliği** CRUD işlemleri gerçekleştirmek için gereken ve çevrimdışı eşitleme için. Aşağıdaki kodu nasıl kullanılacağını anlatan [InsertAsync] yöntemi bir tabloya yeni satırlar ekleyin. Parametre .NET nesnesi olarak eklenecek verileri içerir.

```
await todoTable.InsertAsync(todoItem);
```

Benzersiz ve özel bir kimlik değeri dahil değilse, `todoItem` bir ekleme sırasında bir GUID sunucu tarafından oluşturulur.
Nesne araması döndükten sonra inceleyerek oluşturulan kimliği alabilir.

Yazılmayan veri eklemek için Json.NET yararlanmak:

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

Benzersiz bir dize kimliği olarak bir e-posta adresi kullanarak örnek aşağıda verilmiştir:

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a>KOD değerleri ile çalışma
Mobil uygulamaları destekleyen özel benzersiz bir dize değerleri tablonun için **kimliği** sütun. Bir dize değeri uygulamaların kimliği kullanıcı adlarını veya e-posta adresi gibi özel değerler kullanmasını sağlar  Dize kimlikleri ile aşağıdaki avantajları sağlar:

* Kimlikleri veritabanına gidiş dönüş yapmaya gerek olmadan oluşturulur.
* Kayıtları farklı tablolar veya veritabanlarına birleştirme kolaydır.
* Kimlikleri değerleri daha iyi bir uygulama mantığı ile tümleştirebilirsiniz.

Bir dize kimliği değeri eklenen bir kayıtta ayarlanmadığında, mobil uygulama arka ucu benzersiz bir değer için kimliği oluşturur. Kullanabileceğiniz [Guid.NewGuid] istemcide veya arka uç kendi kimliği değerlerini oluşturmak için yöntem.

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <a name="modifying"></a>Nasıl yapılır: bir mobil uygulama arka ucu verileri değiştirme
Aşağıdaki kodu nasıl kullanılacağını anlatan [UpdateAsync] yeni bilgilerle aynı Kimliğe sahip varolan bir kaydı güncelleştirmeye yöntemi. Parametre .NET nesnesi olarak güncelleştirilmesi için verileri içerir.

```
await todoTable.UpdateAsync(todoItem);
```

Yazılmayan veri güncelleştirmek için avantajlarından sürebilir [Json.NET] gibi:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

Bir `id` alan bir güncelleştirme yaparken belirtilmelidir. Arka uç kullanan `id` güncelleştirmek için hangi satır tanımlamak için alan. `id` Alan sonuç elde edilebilir `InsertAsync` çağırın. Bir `ArgumentException` sağlamadan öğeyi güncelleştirmeye çalıştığınızda, tetiklenir `id` değeri.

### <a name="deleting"></a>Nasıl yapılır: bir mobil uygulama arka ucu verileri Sil
Aşağıdaki kodu nasıl kullanılacağını anlatan [DeleteAsync] yöntemi var olan bir örneğini silin. Örneği tarafından tanımlanan `id` alan kümesinde `todoItem`.

```
await todoTable.DeleteAsync(todoItem);
```

Yazılmayan veri silmek için aşağıdaki gibi Json.NET avantajlarından alabilir:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

Bir silme isteği yaptığınızda, bir kimliği belirtilmelidir. Diğer özellikler hizmete geçmedi veya hizmeti göz ardı edilir. Sonucunu bir `DeleteAsync` çağrıdır genellikle `null`. İçinde geçirmek için kimliği sonuç elde edilebilir `InsertAsync` çağırın. A `MobileServiceInvalidOperationException` belirtmeden bir öğe silmeye çalıştığınızda durum `id` alan.

### <a name="optimisticconcurrency"></a>Nasıl yapılır: kullanım iyimser eşzamanlılık çakışması çözümleme
İki veya daha fazla istemcileri değişiklikler, aynı anda aynı öğeye yazabilir. Çakışma algılaması son yazma herhangi bir önceki güncelleştirme üzerine yazacak. **İyimser eşzamanlılık denetimini** her işlem tamamlayabilir ve bu nedenle hiçbir kaynak kilitleme kullanmaz varsayar.  Bir işlem gerçekleştirmeden önce başka bir işlem verileri değiştirdi iyimser eşzamanlılık denetimini doğrular. Veri değiştirilirse uygulanıyor işlem geri alındı.

Mobil uygulamaları destekleyen iyimser eşzamanlılık denetimini her öğesi kullanarak için değişiklik izleme tarafından `version` , mobil uygulamanızın arka ucuna her tablo için tanımlanan sistem özelliği sütun. Bir kayıt güncelleştirilir, her zaman Mobile Apps ayarlar `version` özelliği için yeni bir değer kayda. Her güncelleştirme isteği sırasında `version` sunucuda kaydı için aynı özelliği için istekte kayıt özelliğinin karşılaştırılan. Sürüm ile aktarılırsa isteği arka uç eşleşmiyor sonra istemci kitaplığı başlatır bir `MobileServicePreconditionFailedException<T>` özel durum. Özel durum ile dahil kayıttan kaydın sunucuları sürümünü içeren arka uç türüdür. Uygulama daha sonra yeniden ile doğru güncelleştirme isteği yürütmek karar vermek için bu bilgiyi kullanabilirsiniz `version` değişiklikleri yürütmek için arka uç değeri.

Bir sütun için tablo sınıfında tanımlamak `version` iyimser eşzamanlılık etkinleştirmek için sistem özelliği. Örneğin:

```
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

Türsüz tabloları kullanan uygulamalar ayarlayarak iyimser eşzamanlılık etkinleştirmek `Version` üzerinde bayrak `SystemProperties` şekilde tablosunun.

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

Ayrıca catch gerekir iyimser eşzamanlılık etkinleştirmeye ek olarak, `MobileServicePreconditionFailedException<T>` kodunuzda çağrılırken özel durum [UpdateAsync].  Doğru uygulayarak çakışmayı `version` güncelleştirilmiş kayıt ve çağrısı [UpdateAsync] çözülmüş kaydıyla. Aşağıdaki kod bir yazma çakışması kez algılandı gidermek nasıl gösterir:

```
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
    //Ask user to choose the resoltion between versions
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

Daha fazla bilgi için bkz: [çevrimdışı veri eşitlemeye Azure Mobile Apps içinde] konu.

### <a name="binding"></a>Nasıl yapılır: bir Windows kullanıcı arabirimini bağlaması Mobile Apps verileri
Bu bölümde, bir Windows uygulamasında kullanıcı Arabirimi öğeleri kullanarak döndürülen veri nesneleri görüntülemek nasıl gösterir.  Aşağıdaki kod örneği, tamamlanmamış öğeleri için bir sorgu ile listesi kaynağı bağlar. [MobileServiceCollection] mobil uygulamaları algılayan bir bağlama koleksiyonu oluşturur.

```
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

Yönetilen çalıştırma zamanı bazı denetimler adlı bir arabirim desteği [ISupportIncrementalLoading]. Bu arabirim kullanıcı kaydırdığında ek veri istemek denetimleri sağlar. Bu arabirim için evrensel Windows uygulamaları için yerleşik destek [MobileServiceIncrementalLoadingCollection], otomatik olarak işleme denetimleri gelen çağrıları. Kullanım `MobileServiceIncrementalLoadingCollection` şekilde Windows uygulamalarında:

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

Yeni koleksiyon Windows Phone 8 ve "Silverlight" uygulamaları kullanmak için `ToCollection` genişletme yöntemleri `IMobileServiceTableQuery<T>` ve `IMobileServiceTable<T>`. Verileri yüklemek için çağrı `LoadMoreItemsAsync()`.

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

Çağrılarak oluşturulan koleksiyonu kullandığınızda `ToCollectionAsync` veya `ToCollection`, UI denetimine bağlı bir koleksiyon alın.  Bu disk belleği algılayan koleksiyonudur.  Koleksiyon verileri ağ üzerinden yüklüyor olduğundan, bazen yükleme başarısız olur. Bu tür hataları işlemek için geçersiz kılma `OnException` yöntemi `MobileServiceIncrementalLoadingCollection` çağrıları kaynaklanan özel durumları işleme `LoadMoreItemsAsync`.

Tablonuzda fazla alan var ancak yalnızca bunlardan bazıları, denetiminde görüntülemek istediğiniz göz önünde bulundurun. Önceki bölümde kılavuzunu kullanabilir "[belirli sütunları seçmek](#selecting)" kullanıcı Arabiriminde görüntülenecek belirli sütunları seçmek için.

### <a name="pagesize"></a>Sayfa boyutunu değiştirme
Azure mobil uygulamalar varsayılan olarak en fazla istek başına 50 öğe döndürür.  İstemci ve sunucudaki maksimum sayfa boyutunu artırarak disk belleği boyutunu değiştirebilirsiniz.  İstenen sayfa boyutunu artırmak için belirtin `PullOptions` kullanırken `PullAsync()`:

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

Yapmış olduğunuz varsayılarak `PageSize` eşit veya 100 sunucu içindeki daha büyük bir istek en fazla 100 öğeleri döndürür.

## <a name="#offlinesync"></a>Çevrimdışı tablolarla çalışma
Çevrimdışı tabloları yerel SQLite depolamak için veri çevrimdışı olduğunda kullanın.  Tüm tablo işlemleri yerel karşı yapılır SQLite depolamak yerine uzak sunucu deposu.  Çevrimdışı bir tablo oluşturmak için ilk projeniz hazırlayın:

1. Visual Studio'da çözüme sağ tıklayın > **çözüm için NuGet paketlerini Yönet...** , ardından arayın ve yükleyin **Microsoft.Azure.Mobile.Client.SQLiteStore** Çözümdeki tüm projeleri için NuGet paketi.
2. (İsteğe bağlı) Windows cihazları desteklemek için aşağıdaki SQLite çalışma zamanı paketlerden birini yükleyin:

   * **Windows 8.1 çalışma zamanı:** yükleme [Windows 8.1 için SQLite][3].
   * **Windows Phone 8.1:** yükleme [Windows Phone 8.1 için SQLite][4].
   * **Evrensel Windows platformu** yükleme [Evrensel Windows SQLite][5].
3. (İsteğe bağlı). Windows cihazlar için tıklatın **başvuruları** > **Başvuru Ekle...** , genişletin **Windows** klasörü > **uzantıları**, uygun etkinleştirmek **SQLite for Windows** SDK ile birlikte **için Visual C++ 2013 çalışma zamanı Windows** SDK.
    SQLite SDK adları, her Windows platformuyla biraz farklılık gösterir.

Yerel depodan bir tablo başvurusu oluşturulabilmesi için hazırlanması gerekir:

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes the SyncContext using the default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

İstemci hemen oluşturulduktan sonra depolama başlatma normal olarak gerçekleştirilir.  **OfflineDbPath** filename desteklediğiniz tüm platformlarda kullanmaya uygun olmalıdır.  Yol tam nitelenmiş bir yol ise (diğer bir deyişle, bir eğik çizgi ile başlar), bu yolun kullanılır.  Yol tam olarak nitelenmiş değil, dosyanın bir platforma özgü konuma yerleştirilir.

* İOS ve Android cihazlar için varsayılan yolu "Kişisel dosyalar" klasörüdür.
* Windows cihazlar için uygulamaya özgü "AppData" klasörü varsayılan yoldur.

Bir tablo başvurusu kullanılarak edinilebilir `GetSyncTable<>` yöntemi:

```
var table = client.GetSyncTable<TodoItem>();
```

Çevrimdışı bir tablosunu kullanmak için kimlik doğrulaması gerekmez.  Yalnızca arka uç hizmeti ile iletişim kurarken kimlik doğrulaması gerekir.

### <a name="syncoffline"></a>Çevrimdışı bir tablo eşitleniyor
Çevrimdışı tabloları arka uç ile varsayılan olarak eşitlenmez.  Eşitleme iki parçalara bölünür.  Yeni öğeler indirmelerinin değişiklikleri ayrı olarak gönderebilir.  Tipik eşitleme yöntemi şöyledir:

```
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

İlk bağımsız değişken `PullAsync` null, artımlı eşitleme değil kullanılır.  Her eşitleme işlemi, tüm kayıtları alır.

SDK örtülü gerçekleştirir `PushAsync()` kayıtları çekme önce.

Çakışma işleme olur bir `PullAsync()` yöntemi.  Çevrimiçi tablolar aynı şekilde çakışıyor ilgilenir.  Çakışma üretilen zaman `PullAsync()` INSERT, update veya delete sırasında yerine olarak adlandırılır. Birden çok çakışma görülüyorsa, bunların tek MobileServicePushFailedException paketlenmiştir.  Her hata ayrı olarak işler.

## <a name="#customapi"></a>Özel bir API ile çalışma
Özel bir API değil eşlemek için bir ekleme, güncelleştirme, silme veya okuma işlemi sunucusu işlevselliği kullanıma özel uç noktaları tanımlamanızı sağlar. Özel bir API kullanarak, okuma ve HTTP ileti üstbilgilerini ayarlama ve JSON dışında bir ileti gövdesinin biçimi tanımlama gibi Mesajlaşma üzerinde daha fazla denetim olabilir.

Aşağıdakilerden birini çağırarak özel bir API çağrısı [InvokeApiAsync] istemcide yöntemleri. Örneğin, aşağıdaki kod satırını bir POST isteği gönderir **completeAll** arka uç API:

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

Bu form yazılan yöntem çağrısı ve gerektiren **MarkAllResult** dönüş türü tanımlanmıştır. Yazılan ve türsüz yöntemleri desteklenir.

InvokeApiAsync() yöntemi API ile başlayan sürece çağırmak istediğiniz API/api /' başına bir '/'.
Örneğin:

* `InvokeApiAsync("completeAll",...)`arka uçta /api/completeAll çağırır
* `InvokeApiAsync("/.auth/me",...)`arka uçta /.auth/Me çağırır

Tanımlı değil Bu WebAPIs Azure Mobile Apps ile de dahil olmak üzere tüm Webapı çağırmak için InvokeApiAsync kullanabilirsiniz.  InvokeApiAsync() kullandığınızda, kimlik doğrulama üstbilgileri dahil olmak üzere uygun üst bilgiler, istekle birlikte gönderilir.

## <a name="authentication"></a>Kullanıcıların kimlik doğrulaması
Mobile Apps, kimlik doğrulaması ve çeşitli dış kimlik sağlayıcılarını kullanarak uygulama kullanıcıları yetkilendirmek destekler: Facebook, Google, Microsoft Account, Twitter ve Azure Active Directory. Yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi kısıtlamak için tablolarda izinlerini ayarlayabilirsiniz. Kimliği doğrulanmış kullanıcıların kimliğini, sunucu komut dosyalarında yetkilendirme kuralları uygulamak için de kullanabilirsiniz. Daha fazla bilgi için [uygulamanıza kimlik doğrulaması ekleme] öğreticisine bakın.

İki kimlik doğrulama akışı desteklenir: *istemcisi yönetilen* ve *sunucusu yönetilen* akış. Sağlayıcının web kimlik doğrulaması arabirimde alacağından sunucusu yönetilen akış Basit kimlik doğrulama deneyimi sağlar. Sağlayıcıya özgü aygıta özgü SDK'ları üzerinde alacağından istemcisi yönetilen akış aygıta özgü özellikleri ile daha derin tümleştirme sağlar.

> [!NOTE]
> İstemci tarafından yönetilen bir akış üretim uygulamalarınızı kullanmanızı öneririz.

Kimlik doğrulamasını kurmak için bir veya daha fazla kimlik sağlayıcıları ile uygulamanızı kaydetmeniz gerekir.  Bir istemci kimliği ve istemci parolası, uygulamanız için kimlik sağlayıcısı oluşturur.  Bu değerler, Azure App Service kimlik doğrulama/yetkilendirme etkinleştirmek için arka ucunuzu sonra ayarlanır.  Daha fazla bilgi için aşağıdaki ayrıntılı yönergeleri öğreticide izleyin [uygulamanıza kimlik doğrulaması ekleme].

Aşağıdaki konular bu bölümde ele alınmıştır:

* [Yönetilen istemci kimlik doğrulaması](#clientflow)
* [Yönetilen sunucu kimlik doğrulaması](#serverflow)
* [Kimlik doğrulama belirteci önbelleğe alma](#caching)

### <a name="clientflow"></a>Yönetilen istemci kimlik doğrulaması
Uygulamanızı bağımsız olarak kimlik sağlayıcısı başvurun ve ardından döndürülen belirteç ile arka oturum açma sırasında sağlayın. Bu istemci akışı, kullanıcılar için çoklu oturum açma deneyimini sağlamak ya da kimlik sağlayıcısı'ndan ek kullanıcı verilerini almak için sağlar. İstemci akış kimlik doğrulaması kimlik sağlayıcısı SDK daha yerel bir UX fikir sağlar ve ek özelleştirme için sağlar gibi bir sunucu akışı kullanarak için tercih edilir.

Örnekler için aşağıdaki akış olmayan istemci kimlik doğrulaması düzenleri verilmiştir:

* [Active Directory kimlik doğrulama kitaplığı](#adal)
* [Facebook veya Google](#client-facebook)
* [Live SDK](#client-livesdk)

#### <a name="adal"></a>Kullanıcıların Active Directory kimlik doğrulama kitaplığı ile kimlik doğrulaması
Azure Active Directory kimlik doğrulaması kullanarak istemciden başlatabilir kullanıcı kimlik doğrulaması için Active Directory Authentication Library (ADAL) kullanabilirsiniz.

1. İzleyerek, mobil uygulamanızın arka ucuna AAD oturum açma için yapılandırma [uygulama hizmeti Active Directory oturum açma için yapılandırma] Öğreticisi. İsteğe bağlı bir adım yerel istemci uygulaması kaydı tamamlamak emin olun.
2. Visual Studio veya Xamarin Studio, projenizi açın ve bir başvuru ekleyin `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet paketi. Arama yaparken, yayın öncesi sürümlerini içerir.
3. Kullanmakta olduğunuz platformlara göre uygulamanız için aşağıdaki kodu ekleyin. Her, aşağıdaki değişiklikleri yapın:

   * Değiştir **INSERT yetkilisi burada** uygulamanızı sağlanan Kiracı adı. Https://login.microsoftonline.com/contoso.onmicrosoft.com biçiminde olmalıdır. Bu değer, Azure Active Directory etki alanı sekmesinden kopyalanabilir [Azure portal].
   * Değiştir **Ekle-RESOURCE-kimliği-Buraya** , mobil uygulamanızın arka ucuna için istemci kimliği. İstemci kimliği elde edebilirsiniz **Gelişmiş** altında sekmesinde **Azure Active Directory ayarları** Portalı'nda.
   * Değiştir **Ekle-istemci-kimliği-Buraya** yerel istemci uygulamasından kopyaladığınız istemci kimliği.
   * Değiştir **Ekle-REDIRECT-URI-Buraya** sitenizin ile */.auth/login/done* uç noktasını, HTTPS şeması kullanarak. Bu değer benzer olmalıdır *https://contoso.azurewebsites.net/.auth/login/done*.

     Her platform için gereken kod aşağıdaki gibidir:

     **Windows:**

    ```
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

    ```
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

    ```
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

#### <a name="client-facebook"></a>Çoklu oturum Facebook veya Google uygulamasından bir belirteç kullanarak açma
Facebook veya Google için bu parçacığında gösterildiği gibi istemci akışı kullanabilirsiniz.

```
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

#### <a name="client-livesdk"></a>Tekli Oturum Live SDK ile birlikte Microsoft Account kullanarak açma
Kullanıcıların kimliğini doğrulamak için Microsoft hesabı Geliştirici Merkezi uygulamanızı kaydetmeniz gerekir. Kayıt ayrıntıları, mobil uygulama arka uçta yapılandırın. Bir Microsoft hesabı kaydı oluşturun ve mobil uygulama arka ucunuza bağlanmak için bölümündeki adımları tamamlamanız [bir Microsoft hesabı oturum açma kullanmak için uygulamanızı kaydetme]. Uygulamanızı Windows mağazası ve Windows Phone 8/Silverlight sürümüne sahipseniz, Windows mağazası sürümü ilk kaydedin.

Aşağıdaki kod Live SDK'sını kullanarak kimliğini doğrular ve mobil uygulama arka ucunuza oturum açmak için döndürülen belirteç kullanır.

```
private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
private async System.Threading.Tasks.Task AuthenticateAsync()
{

    // Get the URL the Mobile App backend.
    var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

    // Create the authentication client for Windows Store using the service URL.
    LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
    //// Create the authentication client for Windows Phone using the client ID of the registration.
    //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

    while (session == null)
    {
        // Request the authentication token from the Live authentication service.
        // The wl.basic scope should always be requested.  Other scopes can be added
        LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
        if (result.Status == LiveConnectSessionStatus.Connected)
        {
            session = result.Session;

            // Get information about the logged-in user.
            LiveConnectClient client = new LiveConnectClient(session);
            LiveOperationResult meResult = await client.GetAsync("me");

            // Use the Microsoft account auth token to sign in to App Service.
            MobileServiceUser loginResult = await App.MobileService
                .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

            // Display a personalized sign-in greeting.
            string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
            var message = string.Format("You are now logged in - {0}", loginResult.UserId);
            var dialog = new MessageDialog(message, title);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
        else
        {
            session = null;
            var dialog = new MessageDialog("You must log in.", "Login Required");
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
}
```

Daha fazla bilgi için bkz: [Windows Live SDK] belgeleri.

### <a name="serverflow"></a>Yönetilen sunucu kimlik doğrulaması
Kimlik sağlayıcınızı kaydettikten sonra arama [LoginAsync] yöntemi ile [MobileServiceClient] üzerinde [MobileServiceAuthenticationProvider] sağlayıcınız değeri. Örneğin, aşağıdaki kod bir sunucu akış oturum açma Facebook kullanarak başlatır.

```
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

Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, değerini değiştirme [MobileServiceAuthenticationProvider] sağlayıcınız için değer.

Bir sunucu akışında Azure uygulama hizmeti oturum açma sayfası için seçilen sağlayıcıya görüntüleyerek OAuth kimlik doğrulaması akışı yönetir.  Azure App Service kimlik sağlayıcısı döndürür oluşturur sonra bir uygulama hizmeti kimlik doğrulama belirteci. [LoginAsync] yöntemi döndürür bir [MobileServiceUser], her ikisi de sağlayan [UserID] kimliği doğrulanmış kullanıcının ve [MobileServiceAuthenticationToken], JSON web Token (JWT). Bu belirteç önbelleğe alınabilir süresi sona erene kadar yeniden kullanılabilir. Daha fazla bilgi için bkz: [kimlik doğrulama belirteci önbelleğe alma](#caching).

### <a name="caching"></a>Kimlik doğrulama belirteci önbelleğe alma
Bazı durumlarda, oturum açma yöntemi çağrısı sağlayıcıdan kimlik doğrulama belirteci depolayarak ilk başarılı kimlik doğrulamasından sonra önlenebilir.  Windows mağazası ve UWP uygulamalar kullanabilir [PasswordVault] gibi geçerli kimlik doğrulama belirteci bir başarılı oturum açma işleminden sonra önbelleğe almak için:

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

UserId değeri kimlik bilgisi kullanıcı adı olarak depolanır ve depolanan parola olarak bir belirteçtir. Sonraki start-ups üzerinde kontrol edebilirsiniz **PasswordVault** önbelleğe alınmış kimlik bilgileri. Aşağıdaki örnek, bulunan ve aksi durumda yeniden arka ucuyla kimlik doğrulama girişiminde önbelleğe alınmış kimlik bilgilerini kullanır:

```
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

Bir kullanıcı oturum açtığınızda, depolanan kimlik bilgileri, aşağıdaki gibi de kaldırmanız gerekir:

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

Xamarin uygulamaları kullanım [Xamarin.Auth] kimlik bilgileri güvenli bir şekilde API'leri bir **hesap** nesnesi. Bu API'leri kullanarak bir örnek için bkz: [AuthStore.cs] kod dosyasında [örnek paylaşımı ContosoMoments fotoğraf](https://github.com/azure-appservice-samples/ContosoMoments).

Yönetilen istemci kimlik doğrulaması kullandığınızda, Facebook veya Twitter'da gibi sağlayıcınızdan alınan erişim belirteci önbelleğe alabilir. Bu belirteç arka ucundan gibi yeni bir kimlik doğrulama belirteci istemek için sağlanabilir:

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using the access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <a name="pushnotifications"></a>Anında iletme bildirimleri
Aşağıdaki konular, anında iletme bildirimleri kapsar:

* [Anında iletme bildirimleri için kaydolun](#register-for-push)
* [Windows mağazası paket SID'si alın](#package-sid)
* [Platformlar arası şablonları ile kaydetme](#register-xplat)

### <a name="register-for-push"></a>Nasıl yapılır: anında iletme bildirimleri için kaydolun
Mobile Apps istemci Azure Notification Hubs ile anında iletme bildirimleri için kaydetmenizi sağlar. Kaydederken, platforma özgü anında bildirim hizmeti (PNS) gelen elde bir tanıtıcı edinin. Kayıt oluşturduğunuzda, ardından tüm etiketlerin yanı sıra bu değer sağlayın. Aşağıdaki kod Windows bildirim Hizmeti'ni (WNS) ile Windows uygulamanız anında iletme bildirimleri için kaydeder:

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using the new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

WNS için itme sonra yapmanız gerekenler [Windows mağazası paket SID'si elde](#package-sid).  Şablon kayıtlar için nasıl dahil olmak üzere Windows uygulamaları hakkında daha fazla bilgi için bkz: [uygulamanıza anında iletme bildirimleri ekleme].

İstemciden etiketleri isteyen desteklenmiyor.  Etiket istekleri sessizce kaydından bırakılır.
Etiketleriyle Cihazınızı kaydetmek istiyorsanız, kayıt sizin adınıza gerçekleştirmesi için bildirim hub'ları API'sini kullanan bir özel API oluşturun.  [Özel API çağrısı](#customapi) yerine `RegisterNativeAsync()` yöntemi.

### <a name="package-sid"></a>Nasıl yapılır: Windows mağazası paket SID'si alın
Bir paket SID'si Windows mağazası uygulamalarında anında iletme bildirimleri etkinleştirmek için gereklidir.  Bir paket SID'si almak için Windows mağazası uygulamanızı kaydedin.

Bu değeri elde etmek için:

1. Visual Studio Çözüm Gezgini'nde, Windows mağazası uygulama projesine sağ tıklayın, **deposu** > **uygulamayı mağaza ile ilişkilendir...** .
2. Sihirbazı'nda tıklatın **sonraki**, Microsoft hesabınızla oturum açın, uygulamanız için bir ad yazın **yeni bir uygulama adı yedek**, ardından **ayırma**.
3. Uygulama kaydı başarıyla oluşturulduktan sonra uygulama adı seçin, **sonraki**ve ardından **ilişkilendirmek**.
4. Oturum [Windows Geliştirme Merkezi] Microsoft Account kullanarak. Altında **uygulamalarım**, oluşturduğunuz uygulama kayıt'a tıklayın.
5. Tıklatın **Uygulama Yönetimi** > **uygulama kimliği**ve ardından Bul aşağı kaydırın, **paket SID'si**.

Paket SID'si birçok kullanımı, bir URI olarak kullanmanıza gerek; bu durumda kabul *ms-app: / /* düzeni olarak. Paketinizin bir önek olarak bu değer birleştirerek biçimlendirilmiş SID sürümünü not edin.

Xamarin uygulamaları iOS veya Android platformları üzerinde çalışan uygulama kaydetmeyi yapabilmek için bazı ek kod gerektirir. Daha fazla bilgi için platformunuza ilişkin konuya bakın:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <a name="register-xplat"></a>Nasıl yapılır: platformlar arası bildirimleri göndermek için kayıt anında şablonları
Şablonları kaydetmek için kullanın `RegisterAsync()` şekilde şablonları yöntemiyle:

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

Şablonlarınızı olmalıdır `JObject` türleri ve aşağıdaki JSON biçiminde birden fazla şablon içerebilir:

```
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

Yöntem **RegisterAsync()** de ikincil döşeme kabul eder:

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

Tüm etiketleri hemen güvenlik için kayıt sırasında kaldırılır. Yüklemeleri veya şablonları yüklemeleri içinde etiketleri eklemek için bkz. [Azure mobil uygulamalar için .NET arka uç sunucusu SDK ile çalışma].

Bu kayıtlı şablonları kullanılarak bildirimleri göndermek için başvurmak [bildirim hub'ları API'leri].

## <a name="misc"></a>Çeşitli konuları
### <a name="errors"></a>Nasıl yapılır: hata işleme
Arka bir hata ortaya çıkarsa, istemcinin SDK başlatır bir `MobileServiceInvalidOperationException`.  Aşağıdaki örnek arka ucu tarafından döndürülen bir özel durum nasıl ele alınacağını gösterir:

```
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

Hata koşulları postalarla başka bir örneği bulunabilir [Mobile Apps dosyaları örnek]. [LoggingHandler] örnek arka uç için yapılan istekleri günlüğe kaydetmek için bir günlük temsilci işleyicisi sağlar.

### <a name="headers"></a>Nasıl yapılır: özelleştirme istek üstbilgileri
Belirli uygulama senaryonuz desteklemek için mobil uygulama arka ucu ile iletişim özelleştirme gerekebilir. Örneğin, giden her istek için bir özel üst bilgi eklemek veya bile yanıtları durum kodlarını değiştirmek isteyebilirsiniz. Özel bir kullanabilirsiniz [DelegatingHandler], aşağıdaki örnekte olduğu gibi:

```
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
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[uygulamanıza kimlik doğrulaması ekleme]: app-service-mobile-windows-store-dotnet-get-started-users.md
[çevrimdışı veri eşitlemeye Azure Mobile Apps içinde]: app-service-mobile-offline-data-sync.md
[uygulamanıza anında iletme bildirimleri ekleme]: app-service-mobile-windows-store-dotnet-get-started-push.md
[bir Microsoft hesabı oturum açma kullanmak için uygulamanızı kaydetme]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[uygulama hizmeti Active Directory oturum açma için yapılandırma]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[bir başvuru türü belirsiz bir tablo oluşturur]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[ele]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[seçin]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[atla]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Kullanıcı Kimliği]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Burada]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure portal]: https://portal.azure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows Geliştirme Merkezi]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[bildirim hub'ları API'leri]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobile Apps dosyaları örnek]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 belgelerine]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
