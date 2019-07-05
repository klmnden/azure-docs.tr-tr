---
title: Android için Azure Mobile Apps SDK'sını kullanma | Microsoft Docs
description: Android için Azure Mobile Apps SDK'sını kullanma
services: app-service\mobile
documentationcenter: android
author: elamalani
manager: crdun
ms.assetid: 5352d1e4-7685-4a11-aaf4-10bd2fa9f9fc
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 6a6db136926a7f9d631c717f5cab6c025d97fb48
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443537"
---
# <a name="how-to-use-the-azure-mobile-apps-sdk-for-android"></a>Android için Azure Mobile Apps SDK'sını kullanma

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirme merkezi hizmetlerinde yeni ve tümleşik yatırım yapıyor. Geliştiriciler **derleme**, **Test** ve **Dağıt** hizmetlerinin sürekli tümleştirme ve teslim işlem hattı ayarlayın. Uygulama dağıtıldığında, geliştiriciler kendi uygulamasını kullanarak kullanımı ve durumu izleyebilirsiniz **Analytics** ve **tanılama** kullanarak kullanıcılarla etkileşim kurun ve hizmetlerini **anında iletme** hizmeti. Geliştiriciler de yararlanabilir **Auth** , kullanıcıların kimliğini doğrulamak ve **veri** kalıcı hale getirmek ve uygulama verilerini bulutta eşitleme hizmeti. Kullanıma [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-android-how-to-use-client-library) bugün.
>

Bu kılavuzu gibi yaygın senaryoları uygulamak için Android istemci SDK'sı mobil uygulamalar için kullanmayı gösterir:

* (Ekleme, güncelleştirme, silme) veri sorgulama.
* kimlik doğrulaması.
* Hataları işleme.
* İstemci özelleştirme.

Bu kılavuz, istemci tarafı Android SDK üzerinde odaklanır.  Mobile Apps için sunucu tarafı SDK'lar hakkında daha fazla bilgi edinmek için [.NET arka uç SDK'sı ile çalışma][10] or [How to use the Node.js backend SDK][11].

## <a name="reference-documentation"></a>Başvuru belgeleri

Bulabilirsiniz [Javadocs API Başvurusu][12] github'da Android istemci kitaplığı.

## <a name="supported-platforms"></a>Desteklenen platformlar

Android için Azure Mobile Apps SDK'sı, telefon ve tablet form faktörleri için API düzey 19 ile 24 (KitKat Nougat aracılığıyla) destekler.  Kimlik doğrulaması, özellikle, kimlik bilgilerini toplamak için genel bir web çerçevesi yaklaşım kullanır.  Sunucu akışı kimlik doğrulaması, izlemeleri gibi küçük form faktörü cihazlarla çalışmaz.

## <a name="setup-and-prerequisites"></a>Kurulum ve Önkoşullar

Tamamlamak [Mobile Apps Hızlı Başlangıç](app-service-mobile-android-get-started.md) öğretici.  Bu görevi, Azure mobil uygulamaları geliştirmek için tüm ön koşullar karşılandığında sağlar.  Bu hızlı başlangıçta ayrıca hesabınızı yapılandırın ve ilk mobil uygulama arka ucu oluşturmanıza yardımcı olur.

Hızlı Başlangıç öğreticisini tamamlamak değil karar verirseniz, aşağıdaki görevleri tamamlayın:

* [bir mobil uygulama arka ucu oluşturma][13] Android uygulamanızı kullanmak için.
* Android Studio'da [güncelleştirme Gradle derleme dosyaları](#gradle-build).
* [İnternet iznini etkinleştirme](#enable-internet).

### <a name="gradle-build"></a>Gradle derleme dosyasını güncelleştirme

Her ikisini de değiştirme **build.gradle** dosyaları:

1. Bu kodu ekleyin *proje* düzeyi **build.gradle** dosyası:

    ```gradle
    buildscript {
        repositories {
            jcenter()
            google()
        }
    }

    allprojects {
        repositories {
            jcenter()
            google()
        }
    }
    ```

2. Bu kodu ekleyin *modülü uygulama* düzeyi **build.gradle** içinde dosya *bağımlılıkları* etiketi:

    ```gradle
    implementation 'com.microsoft.azure:azure-mobile-android:3.4.0@aar'
    ```

    Şu anda en son sürümü 3.4.0 gösterilir. Desteklenen sürümler listelenmiştir [bintray'deki][14].

### <a name="enable-internet"></a>İnternet iznini etkinleştirme

Azure'a erişmek için uygulamanızı etkin INTERNET izni olmalıdır. Zaten etkinse, aşağıdaki kod satırını ekleyin, **AndroidManifest.xml** dosyası:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a>İstemci bağlantısı oluşturma

Azure Mobile Apps mobil uygulamanıza dört işlev sağlar:

* Veri erişimi ve bir Azure mobil uygulamaları hizmeti ile çevrimdışı eşitleme.
* Azure mobil uygulamalar sunucusu SDK ile yazılmış özel API'ler çağırın.
* Azure App Service kimlik doğrulaması ve yetkilendirme ile kimlik doğrulaması.
* Anında iletme bildirimi kaydı Notification Hubs ile'nı tıklatın.

Bu işlevlerin her biri önce oluşturmanızı gerektiren bir `MobileServiceClient` nesne.  Yalnızca bir `MobileServiceClient` nesne içinde mobil istemci oluşturulması (diğer bir deyişle, bir Singleton deseni olmalıdır).  Oluşturmak için bir `MobileServiceClient` nesnesi:

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with the Site URL
    this);                  // Your application Context
```

`<MobileAppUrl>` Bir dize ya da mobil arka ucunuza işaret eden bir URL nesnesi.  Mobil arka ucunuzdaki barındırmak için Azure App Service kullanıyorsanız, kullandığınız güvenli olduğundan emin olun `https://` URL sürümü.

İstemci ayrıca etkinlik veya bağlam - erişim gerektiren `this` örnekte parametre.  MobileServiceClient yapı içinde gerçekleşmelidir `onCreate()` etkinliğin yöntemi başvuruda `AndroidManifest.xml` dosya.

En iyi uygulama, sunucu yazışmaya kendi (singleton deseni) sınıfı soyut.  Bu durumda, hizmet uygun şekilde yapılandırmak için etkinliğe oluşturucu içinde geçmelidir.  Örneğin:

```java
package com.example.appname.services;

import android.content.Context;
import com.microsoft.windowsazure.mobileservices.*;

public class AzureServiceAdapter {
    private String mMobileBackendUrl = "https://myappname.azurewebsites.net";
    private Context mContext;
    private MobileServiceClient mClient;
    private static AzureServiceAdapter mInstance = null;

    private AzureServiceAdapter(Context context) {
        mContext = context;
        mClient = new MobileServiceClient(mMobileBackendUrl, mContext);
    }

    public static void Initialize(Context context) {
        if (mInstance == null) {
            mInstance = new AzureServiceAdapter(context);
        } else {
            throw new IllegalStateException("AzureServiceAdapter is already initialized");
        }
    }

    public static AzureServiceAdapter getInstance() {
        if (mInstance == null) {
            throw new IllegalStateException("AzureServiceAdapter is not initialized");
        }
        return mInstance;
    }

    public MobileServiceClient getClient() {
        return mClient;
    }

    // Place any public methods that operate on mClient here.
}
```

Artık çağırabilirsiniz `AzureServiceAdapter.Initialize(this);` içinde `onCreate()` ana etkinliği yöntemi.  İstemci erişimi gerektiren diğer yöntemleri `AzureServiceAdapter.getInstance();` hizmeti bağdaştırıcısı için bir başvuru almak için.

## <a name="data-operations"></a>Veri işlemleri

Azure Mobile Apps SDK'sı setinin mobil uygulama arka uçta SQL Azure içinde depolanan verilere erişim sağlamaktır.  Türü kesin belirlenmiş sınıf (tercih edilir) kullanarak bu verilere erişmesinden veya türsüz sorgular (önerilmez).  Bu bölümün toplu kullanarak türü kesin belirlenmiş sınıf ile ilgilidir.

### <a name="define-client-data-classes"></a>İstemci veri sınıflarını tanımlamak

SQL Azure tablolardaki verilere erişmek için mobil uygulama arka ucu tablolarında karşılık gelen istemci veri sınıflarını tanımlayın. Bu konudaki örnekler varsayar adlı bir tablo **MyDataTable**, aşağıdaki sütunlar vardır:

* id
* metin
* Tamamlayın

Karşılık gelen türü belirlenmiş istemci-tarafı nesnesi adlı bir dosyada bulunan **MyDataTable.java**:

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

Eklediğiniz her bir alan için alıcı ve ayarlayıcı yöntemleri ekleyin.  SQL Azure tablonuza daha fazla sütun içeriyorsa, bu sınıf için karşılık gelen alanlarını eklersiniz.  Örneğin, bir DTO (veri aktarımı nesnesi) sahip bir tamsayı öncelik sütunu, ardından alıcı ve ayarlayıcı yöntemlerinden yanı sıra, bu alanın ekleyebilirsiniz:

```java
private Integer priority;

/**
* Returns the item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets the item priority
*
* @param priority
*            priority to set
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

Mobile Apps arka ucunuzu ek tablolar oluşturmayı öğrenmek için bkz: [nasıl yapılır: Bir tablo denetleyicisi tanımlamak][15] (.NET backend) or [Define Tables using a Dynamic Schema][16] (Node.js arka ucu).

Bir Azure Mobile Apps arka uç tablosu, biri dört istemciler için kullanılabilir beş özel alanlar tanımlar:

* `String id`: Kayıt için genel olarak benzersiz kimliği.  En iyi uygulama, dize gösterimi kimliği olun bir [UUID][17] nesne.
* `DateTimeOffset updatedAt`: Son güncelleştirme tarih/saat.  UpdatedAt alan, sunucu tarafından ayarlanır ve hiçbir zaman istemci kodunuz tarafından ayarlanması gerekir.
* `DateTimeOffset createdAt`: Nesne oluşturulduğu tarih.  CreatedAt alan, sunucu tarafından ayarlanır ve hiçbir zaman istemci kodunuz tarafından ayarlanması gerekir.
* `byte[] version`: Normalde bir dize olarak temsil edilen, sürüm sunucu tarafından ayarlanır.
* `boolean deleted`: Kayıt silindi ancak henüz temizleneceği değil olduğunu gösterir.  Kullanmayın `deleted` sınıfınıza özelliği olarak.

`id` alanı gereklidir.  `updatedAt` Alan ve `version` çevrimdışı eşitleme için kullanılan alan (Artımlı eşitleme ve Çakışma çözümlemesi için sırasıyla).  `createdAt` Alanı bir başvuru alan olup, istemci tarafından kullanılmaz.  Adları "arasında hat" özellik adlarının ve ayarlanabilir değildir.  Ancak, nesnenizin kullanarak "arasında hat" adları arasında bir eşleme oluşturabilirsiniz [gson][3] kitaplığı.  Örneğin:

```java
package com.example.zumoappname;

import com.microsoft.windowsazure.mobileservices.table.DateTimeOffset;

public class ToDoItem
{
    @com.google.gson.annotations.SerializedName("id")
    private String mId;
    public String getId() { return mId; }
    public final void setId(String id) { mId = id; }

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;
    public boolean isComplete() { return mComplete; }
    public void setComplete(boolean complete) { mComplete = complete; }

    @com.google.gson.annotations.SerializedName("text")
    private String mText;
    public String getText() { return mText; }
    public final void setText(String text) { mText = text; }

    @com.google.gson.annotations.SerializedName("createdAt")
    private DateTimeOffset mCreatedAt;
    public DateTimeOffset getCreatedAt() { return mCreatedAt; }
    protected void setCreatedAt(DateTimeOffset createdAt) { mCreatedAt = createdAt; }

    @com.google.gson.annotations.SerializedName("updatedAt")
    private DateTimeOffset mUpdatedAt;
    public DateTimeOffset getUpdatedAt() { return mUpdatedAt; }
    protected void setUpdatedAt(DateTimeOffset updatedAt) { mUpdatedAt = updatedAt; }

    @com.google.gson.annotations.SerializedName("version")
    private String mVersion;
    public String getVersion() { return mVersion; }
    public final void setVersion(String version) { mVersion = version; }

    public ToDoItem() { }

    public ToDoItem(String id, String text) {
        this.setId(id);
        this.setText(text);
    }

    @Override
    public boolean equals(Object o) {
        return o instanceof ToDoItem && ((ToDoItem) o).mId == mId;
    }

    @Override
    public String toString() {
        return getText();
    }
}
```

### <a name="create-a-table-reference"></a>Bir tablo başvurusu

Bir tablo erişmek için öncelikle oluşturma bir [MobileServiceTable][8] çağırarak **getTable** metodunda [MobileServiceClient][9].  Bu yöntemin iki aşırı yüklemesi vardır:

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

Aşağıdaki kodda, **mClient** MobileServiceClient nesnenizin bir başvurudur.  İlk aşırı yükleme, burada sınıf adı ve tablo adıyla aynıdır ve bir hızlı başlangıç bölümünde kullanılan kullanılır:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

Tablo adı sınıf adından farklı olduğunda ikinci aşırı yüklemesi kullanılır: ilk parametre tablo adıdır.

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <a name="query"></a>Sorgu bir arka uç tablosu

İlk olarak bir tablo başvurusu edinin.  Ardından tablo başvurusu üzerinde bir sorguyu yürütün.  Bir sorgu, herhangi bir birleşimini şöyledir:

* A `.where()` [filtre yan tümcesi](#filtering).
* Bir `.orderBy()` [ordering yan tümcesi](#sorting).
* A `.select()` [alan seçimi yan tümcesi](#selection).
* A `.skip()` ve `.top()` için [sonuçları disk belleğine alınan](#paging).

Yan tümceleri önceki sırayla sunulmalıdır.

### <a name="filter"></a> Sonuçları filtreleme

Bir sorgunun genel formu şöyledir:

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts the async into a sync result
```

Yukarıdaki örnekte (en fazla sayfa boyutu sunucu tarafından ayarını kadar) tüm sonuçları döndürür.  `.execute()` Yöntemi, arka uçta sorguyu yürütür.  Sorgu dönüştürülür bir [OData v3][19] Mobile Apps arka uca iletimden önce sorgu.  İstek alındığında, Mobile Apps arka uç SQL Azure örneğinde yürütmeden önce bir SQL deyimi sorgu dönüştürür.  Ağ etkinliği biraz zaman alır. bu yana `.execute()` yöntemi döndürür bir [ `ListenableFuture<E>` ][18].

### <a name="filtering"></a>Döndürülen verileri filtreleme

Şu sorgu Yürütmeyle tüm öğeleri döndürür **Todoıtem** tablo where **tam** eşittir **false**.

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

**mToDoTable** daha önce oluşturduğumuz mobil hizmeti tablo başvurudur.

Bir filtre kullanarak tanımlarsınız **burada** Tablo başvurusunda yöntem çağrısı. **Burada** yöntemi tarafından izlenen bir **alan** yöntemi, mantıksal koşul belirten bir yöntem tarafından izlenen. Koşul yöntemden dahil **eq** (eşittir) **ne** (eşit değildir), **gt** (büyüktür), **ge** (büyüktür veya eşittir) **lt** (küçüktür), **le** (küçüktür veya eşittir). Bu yöntemlerin sayısı ve dize alanları belirli değerlerle karşılaştırmak olanak tanır.

Tarihleri filtreleyebilirsiniz. Aşağıdaki yöntemlerden tarih kısımlarını ve tamamını tarih alanı karşılaştırmanıza olanak tanır: **yıl**, **ay**, **gün**, **saat**,  **dakika**, ve **ikinci**. Aşağıdaki örnek, öğeler için bir filtre ekler, *son tarih* 2013'e eşittir.

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

Aşağıdaki yöntemlerden karmaşık filtreleri dize alanları desteği: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **değiştirin**, **toLower**, **toUpper**, **trim**, ve **uzunluğu** . Aşağıdaki örnek filtrelerini tablo satırları *metin* sütun "PRI0" ile başlar

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

Aşağıdaki işleci yöntemleri üzerinde sayı alanları desteklenir: **ekleme**, **alt**, **mul**, **div**, **mod**, **kat**, **tavan**, ve **yuvarlak**. Aşağıdaki örnek filtrelerini tablo satırları **süresi** bir çift sayı.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

Koşullar ile mantıksal bu yöntemleri birleştirebilirsiniz: **ve**, **veya** ve **değil**. Aşağıdaki örnek iki Yukarıdaki örneklerde birleştirir.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

Grup ve iç içe mantıksal işleçleri:

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013)
    .and(
        startsWith("text", "PRI0")
        .or()
        .field("duration").gt(10)
    )
    .execute().get();
```

Daha ayrıntılı tartışma ve filtreleme örnekler için bkz [Android istemci sorgu modelini zenginliğine keşfetmeye][20].

### <a name="sorting"></a>Döndürülen verileri sıralama

Aşağıdaki kod tablodan tüm öğeleri döndürür **Todoıtems** göre artan düzende sıralandı *metin* alan. *mToDoTable* daha önce oluşturduğunuz arka uç tablosuna başvuru:

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

İlk parametresi **orderBy** yöntemdir üzerinde sıralama yapılacak alan adına eşit olan bir dize. İkinci parametre **QueryOrder** artan veya azalan düzende sıralama belirtmek için sabit listesi.  Kullanarak uyguladıysanız ***burada*** yöntemi ***burada*** yöntemi çağrılır, önce ***orderBy*** yöntemi.

### <a name="selection"></a>Belirli sütunları seçin

Aşağıdaki kod tablodan tüm öğeleri döndürmek nasıl gösterir **Todoıtems**, ancak yalnızca görüntüler **tam** ve **metin** alanları. **mToDoTable** daha önce oluşturduğumuz arka uç tablosuna başvuru.

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

Select işlevi parametreleri dize iade etmek istediğiniz tablonun sütunlarını adlarıdır.  **Seçin** yöntemi gerekiyor gibi yöntemleri izlemek **burada** ve **orderBy**. Disk belleği yöntemlerinin gibi tarafından izlenebilir **atla** ve **üst**.

### <a name="paging"></a>Dönüş verileri sayfalarında

Veriler **her zaman** sayfalarında döndürdü.  Döndürülen kayıt sayısı, sunucu tarafından ayarlanır.  Daha fazla kaydı istemci isteğinde bulunursa sunucu en fazla kayıt sayısını döndürür.  Varsayılan olarak, sunucu üzerindeki en fazla sayfa boyutu 50 kayıt ' dir.

İlk örnek, bir tablonun ilk beş öğeleri seçmek gösterilmektedir. Sorgu tablonun öğeleri döndürür **Todoıtems**. **mToDoTable** daha önce oluşturduğunuz arka uç tablosuna başvuru:

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

İlk beş öğeleri atlar ve ardından sonraki beş döndüren bir sorgu aşağıda verilmiştir:

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

Bir tablodaki tüm kayıtları almak istiyorsanız, tüm sayfaları yinelemek için kodu Uygula:

```java
List<MyDataModel> results = new ArrayList<>();
int nResults;
do {
    int currentCount = results.size();
    List<MyDataModel> pagedResults = mDataTable
        .skip(currentCount).top(500)
        .execute().get();
    nResults = pagedResults.size();
    if (nResults > 0) {
        results.addAll(pagedResults);
    }
} while (nResults > 0);
```

Bu yöntemi kullanarak tüm kayıtlar için bir istek, iki isteği Mobile Apps arka ucuna en az oluşturur.

> [!TIP]
> Sağ bir sayfa boyutunu belirlemek, isteğin gerçekleştiği sırada bellek kullanımı, bant genişliği kullanımını ve veri tamamen alma gecikme arasında bir denge değerdir.  Varsayılan (50 kayıt), tüm cihazlar için uygundur.  Özel olarak daha büyük bellek cihazlarda çalışır, en fazla 500 artırın.  Kabul edilebilir gecikme ve büyük bellek sorunlarını 500 kayıt sonuçlarında ötesinde sayfa boyutunu artırma bulduk.

### <a name="chaining"></a>Nasıl Yapılır: Sorgu yöntemleri birleştirme

Arka uç tabloları sorgularken kullanılan yöntemleri birleştirilebilir. Sorgu yöntemleri zincirleme sıralanmış ve disk belleğine alınan bir filtrelenen satırlar belirli sütunları seçmenizi sağlar. Karmaşık mantıksal filtreler oluşturabilirsiniz.  Her sorgu yöntemine, bir sorgu nesnesi döndürür. Bir dizi yöntem bitirmek ve gerçekten sorguyu çalıştırmak için çağrı **yürütme** yöntemi. Örneğin:

```java
List<ToDoItem> results = mToDoTable
        .where()
        .year("due").eq(2013)
        .and(
            startsWith("text", "PRI0").or().field("duration").gt(10)
        )
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .skip(200).top(100)
        .execute()
        .get();
```

Zincirleme sorgu yöntemleri şöyle sıralanmalıdır gerekir:

1. Filtreleme (**burada**) yöntemleri.
2. Sıralama (**orderBy**) yöntemleri.
3. Seçimi (**seçin**) yöntemleri.
4. disk belleği (**atla** ve **üst**) yöntemleri.

## <a name="binding"></a>Kullanıcı arabirimine veri bağlama

Veri bağlama üç bileşenden oluşur:

* Veri kaynağı
* Ekran düzeni
* İki bağdaştırabilir bağdaştırıcısı.

Örnek kodumuz biz verileri Mobile Apps SQL Azure tablodan **Todoıtem** dizisine. Bu etkinlik bir veri uygulamaları için ortak desendir.  Veritabanı sorguları genellikle bir listesini ya da dizi istemci alır satırları koleksiyonunu döndürür. Bu örnekte, veri kaynağı bir dizidir.  Kod, cihaz üzerinde görünen verilerin görünümünü tanımlayan bir ekran düzenini belirtir.  In bir uzantısı olan bu kodda bir bağdaştırıcı ile birlikte, iki bağlı olan **ArrayAdapter&lt;Todoıtem&gt;**  sınıfı.

#### <a name="layout"></a>Düzen tanımlayın

Düzen, birden çok XML kod parçacıkları tarafından tanımlanır. Varolan bir düzen göz önünde bulundurulduğunda, aşağıdaki kod temsil **ListView** bizim sunucu verileriyle doldurmak istiyoruz.

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

Önceki kodda, *ListItem* öznitelik listesinde tek bir satır için Düzen kimliğini belirtir. Bu kod, bir onay kutusu ve ilgili metin belirtir ve listedeki her öğe için bir kez örneği. Bu düzen aşağıdaki dotnetclıtools'u görüntülemiyor **kimliği** alan ve daha karmaşık bir düzen belirtirsiniz ek alanlar görüntüsünü. Bu kodu **row_list_to_do.xml** dosya.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <CheckBox
        android:id="@+id/checkToDoItem"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/checkbox_text" />
</LinearLayout>
```

#### <a name="adapter"></a>Bağdaştırıcı tanımlayın
Veri kaynağı bizim görünümünün bir dizi olduğundan **Todoıtem**, biz alt bizim bağdaştırıcısından bir **ArrayAdapter&lt;Todoıtem&gt;**  sınıfı. Bu alt sınıfı için bir görünüm oluşturur. her **Todoıtem** kullanarak **row_list_to_do** düzeni.  Bizim kodda bir uzantısıdır ve aşağıdaki sınıf tanımlarız **ArrayAdapter&lt;E&gt;**  sınıfı:

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

Bağdaştırıcıları geçersiz kılma **getView** yöntemi. Örneğin:

```java
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }
        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });
        return row;
    }
```

Biz bu sınıfın bir örneği bizim etkinliğinde şu şekilde oluşturun:

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

ToDoItemAdapter oluşturucusunun ikinci parametresi, düzen bir başvurudur. Biz artık oluşturabileceğiniz **ListView** bağdaştırıcıya atayın **ListView**.

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <a name="use-adapter"></a>Kullanıcı arabirimini bağlanacağı bağdaştırıcısı kullanın

Artık veri bağlamayı kullanmak hazırsınız. Aşağıdaki kodu yerel bağdaştırıcısı döndürülen öğeleriyle doldurur ve tabloda öğeleri almak gösterir.

```java
    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }
```

Bağdaştırıcı, değiştirmek istediğiniz zaman arama **Todoıtem** tablo. Değişiklikler tek kayıt kayıt temelinde gerçekleştirilir olduğundan, bir koleksiyon yerine tek bir satır işleyin. Bir öğe eklediğinizde, çağrı **ekleme** bağdaştırıcısında; yöntemi silerken çağrı **kaldırmak** yöntemi.

Tam bir örnek bulabilirsiniz [Android hızlı başlangıç projesi][21].

## <a name="inserting"></a>Arka uca veri ekleme

Bir örneği *Todoıtem* sınıfını ve özelliklerini ayarlayın.

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

Ardından **INSERT()** bir nesneyi eklemek için:

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

Döndürülen varlığı eşleşmeleri arka uç tablosuna veri kimliği ve diğer tüm değerler dahil (gibi `createdAt`, `updatedAt`, ve `version` alanları) arka uç ayarlayın.

Mobile Apps tablolarda gereklidir adlı birincil anahtar sütunu **kimliği**. Bu sütun bir dize olmalıdır. Varsayılan kimlik sütunu bir GUID değeridir.  E-posta adresi veya kullanıcı adları gibi diğer benzersiz değerler sağlayabilirsiniz. Bir dize kimliği için eklenen bir kaydı sağlanmadığında, arka uç yeni GUID oluşturur.

Dize kimliği değerleri aşağıdaki avantajları sağlar:

* Veritabanına bir gidiş dönüş yapmadan kimlikleri oluşturulabilir.
* Kayıtları farklı tablolar veya veritabanlarına birleştirme daha kolaydır.
* Kimliği değerleri daha iyi bir uygulama mantığı ile tümleştirin.

Dize kimliği değerler **gerekli** çevrimdışı eşitleme desteği.  Arka uç veritabanında depolandıktan sonra bir kimliği değiştirilemiyor.

## <a name="updating"></a>Bir mobil uygulama verileri güncelleştirme

Bir tablodaki verileri güncelleştirmek için yeni nesneye geçirmek **update()** yöntemi.

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

Bu örnekte, *öğesi* bir satıra bir başvuru *Todoıtem* tablosu için yapılan birkaç değişiklik oluşturdu.  Aynı satırı **kimliği** güncelleştirilir.

## <a name="deleting"></a>Bir mobil uygulama verilerini silme

Aşağıdaki kod, veri nesnesi belirterek bir tablodan veri silme gösterir.

```java
mToDoTable
    .delete(item);
```

Bir öğe belirterek de silebilirsiniz **kimliği** silmek için satırın alan.

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <a name="lookup"></a>Belirli bir öğeyi kimliğine göre arayın

Belirli bir sahip bir öğe araması **kimliği** alanına **lookUp()** yöntemi:

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <a name="untyped"></a>Nasıl Yapılır: Yazılmamış verileri ile çalışma

Yazılmamış bir programlama modeli, JSON seri hale getirme üzerinde tam denetim sağlar.  Burada yazılmamış bir programlama modeli kullanmak isteyebilir, sık karşılaşılan bazı senaryolar vardır. Örneğin, arka uç tablonuzun birçok sütun içeren ve yalnızca bir sütun alt kümesi başvuru gerekir.  Belirlenmiş model Mobile Apps arka uç veri Sınıfınız içinde tanımlanan tüm sütunları tanımlamanızı gerektirir.  Çoğu verilerine erişmek için API çağrıları, yazılan programlama çağrıları benzerdir. Ana fark yazılmamış modelinde, yöntemleri üzerinde çağırma **MobileServiceJsonTable** nesnesi yerine **MobileServiceTable** nesne.

### <a name="json_instance"></a>Yazılmamış bir tablo örneği oluşturma

Benzer şekilde türü belirlenmiş model, bir tablo başvurusu alarak başlattığınızda, ancak bu durumda, bir **MobileServicesJsonTable** nesne. Başvuru çağırarak elde **getTable** istemci örneği üzerinde yöntemi:

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

Örneğini oluşturduktan sonra **MobileServiceJsonTable**, neredeyse aynı API ile yazılan programlama modeli kullanılabilir olarak sahiptir. Bazı durumlarda, yöntemleri yerine belirtilmiş bir parametre türü belirsiz bir parametre alır.

### <a name="json_insert"></a>Yazılmamış bir tabloya Ekle
Aşağıdaki kod nasıl ekleme yapılacağını gösterir. İlk adım oluşturmaktır bir [JsonObject][1] , which is part of the [gson][3] kitaplığı.

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

Ardından, **INSERT()** yazılmamış nesne tabloya eklenecek.

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

Eklenen nesne Kimliğini almak ihtiyacınız varsa **getAsJsonPrimitive()** yöntemi.

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <a name="json_delete"></a>Yazılmamış bir tablodan silme
Aşağıdaki kod örneği, bu durumda, aynı örneğini silme işlemini gösterir. bir **JsonObject** önceki oluşturulduğu *Ekle* örnek. Kod olarak yazılan durum ile aynıdır, ancak bunu başvurduğundan yöntemi, farklı bir imzaya sahip. bir **JsonObject**.

```java
mToDoTable
    .delete(insertedItem);
```

Ayrıca, kendi Kimliğini kullanarak doğrudan örneği silebilirsiniz:

```java
mToDoTable.delete(ID);
```

### <a name="json_get"></a>Yazılmamış bir tablodan tüm satırları döndürür
Aşağıdaki kod, bir tablonun tamamını almak nasıl gösterir. Bir JSON tablo kullandığımızdan, seçmeli olarak tablonun sütunlarını yalnızca bazılarını alabilir.

```java
public void showAllUntyped(View view) {
    new AsyncTask<Void, Void, Void>() {
        @Override
        protected Void doInBackground(Void... params) {
            try {
                final JsonElement result = mJsonToDoTable.execute().get();
                final JsonArray results = result.getAsJsonArray();
                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        mAdapter.clear();
                        for (JsonElement item : results) {
                            String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                            String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                            Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                            ToDoItem mToDoItem = new ToDoItem();
                            mToDoItem.setId(ID);
                            mToDoItem.setText(mText);
                            mToDoItem.setComplete(mComplete);
                            mAdapter.add(mToDoItem);
                        }
                    }
                });
            } catch (Exception exception) {
                createAndShowDialog(exception, "Error");
            }
            return null;
        }
    }.execute();
}
```

Filtreleme aynı kümesi, filtreleme ve sayfalama belirlenmiş model için kullanılabilen yöntemler yazılmamış modeli için kullanılabilir.

## <a name="offline-sync"></a>Uygulama çevrimdışı eşitleme

Azure Mobile Apps istemci SDK'sı bir kopyasını sunucu verilerini yerel olarak depolamak için bir SQLite veritabanı kullanarak çevrimdışı veri eşitlemeyi de uygular.  Çevrimdışı bir tablo üzerinde gerçekleştirilen işlemler, iş için mobil bağlantısı gerektirmez.  Çevrimdışı eşitleme, esneklik ve performansı çakışmaları çözümlemek için daha karmaşık mantık çoğaltamaz kolaylık sağlar.  Azure Mobile Apps istemci SDK'sı aşağıdaki özellikler uygular:

* Artımlı eşitleme: Yalnızca güncelleştirilmiş ve yeni kayıt bant genişliği ve bellek tüketimi kaydetme indirilir.
* İyimser eşzamanlılık: İşlem başarılı olması için kabul edilir.  Çakışma, sunucu üzerinde güncelleştirme yapıldığında kadar ertelenir.
* Çakışma çözümü: Çakışan bir değişiklik sunucuda yapılan ve kullanıcıyı uyarmak için hooks sağlayan SDK algılar.
* Geçici silme: Silinmiş kayıtlar, çevrimdışı önbelleklerini güncelleştirmek diğer cihazlara izin verme, silinen işaretlenir.

### <a name="initialize-offline-sync"></a>Çevrimdışı eşitleme başlatın

Çevrimdışı her tablo kullanmadan önce çevrimdışı önbellekte tanımlanmalıdır.  Normalde, tablo tanımı istemci oluşturduktan hemen sonra gerçekleştirilir:

```java
AsyncTask<Void, Void, Void> initializeStore(MobileServiceClient mClient)
    throws MobileServiceLocalStoreException, ExecutionException, InterruptedException
{
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>() {
        @Override
        protected void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                if (syncContext.isInitialized()) {
                    return null;
                }
                SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "offlineStore", null, 1);

                // Create a table definition.  As a best practice, store this with the model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define the table in the local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize the local store
                syncContext.initialize(localStore, handler).get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

### <a name="obtain-a-reference-to-the-offline-cache-table"></a>Çevrimdışı Önbellek tablosuna bir başvurudur alın

Çevrimiçi bir tablo için kullandığınız `.getTable()`.  Çevrimdışı bir tablo için kullanın `.getSyncTable()`:

```java
MobileServiceSyncTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

(Filtreleme, sıralama, sayfalama, veri ekleme, verileri güncelleştirme ve verileri silme de dahil olmak üzere) çevrimiçi tablolar için kullanılabilir tüm yöntemleri eşit derecede iyi çalışması tablolarda, çevrimiçi ve çevrimdışı.

### <a name="synchronize-the-local-offline-cache"></a>Yerel önbellek çevrimdışı eşitleme

Uygulamanızın içinde eşitleme denetimidir.  Bir örnek eşitleme yöntemi aşağıda verilmiştir:

```java
private AsyncTask<Void, Void, Void> sync(MobileServiceClient mClient) {
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
        @Override
        protected Void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                syncContext.push().get();
                mToDoTable.pull(null, "todoitem").get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

Bir sorgu adı sağlanmışsa `.pull(query, queryname)` yöntemi sonra Artımlı eşitleme oluşturulmuş veya en son başarıyla değiştirildi yalnızca kayıtlar tamamlanan çekme döndürmek için kullanılır.

### <a name="handle-conflicts-during-offline-synchronization"></a>Sırasında çevrimdışı eşitleme çakışmalarını işleme

Sırasında bir çakışma meydana gelirse bir `.push()` işlemi, bir `MobileServiceConflictException` oluşturulur.   Sunucu tarafından verilen öğenin özel durumda katıştırılır ve tarafından alınabilen `.getItem()` üzerinde özel durum.  Anında iletme MobileServiceSyncContext nesne üzerinde aşağıdaki öğeleri çağırarak ayarlayın:

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

İstediğiniz gibi tüm çakışmaları işaretlenmiş sonra çağırma `.push()` tüm çakışmaları çözümlemeyi tekrar.

## <a name="custom-api"></a>Özel bir API çağrısı

Özel API eşlemek için bir ekleme, güncelleştirme, silme, veya okuma işlemi sunucusu işlevselliği kullanıma sunan özel uç noktalar tanımlamanızı sağlar. Özel API kullanarak okuma ve HTTP ileti üstbilgilerini ayarlama ve ileti gövdesi biçimi JSON dışında tanımlama gibi Mesajlaşma hakkında daha fazla denetime sahip olabilir.

Android bir istemciden çağırın **invokeApi** özel API uç noktası çağrılacak yöntem. Aşağıdaki örnek adlı bir API uç noktasını çağırmak nasıl gösterir **completeAll**, adlı bir koleksiyon sınıfı döndüren **MarkAllResult**.

```java
public void completeItem(View view) {
    ListenableFuture<MarkAllResult> result = mClient.invokeApi("completeAll", MarkAllResult.class);
    Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
        @Override
        public void onFailure(Throwable exc) {
            createAndShowDialog((Exception) exc, "Error");
        }

        @Override
        public void onSuccess(MarkAllResult result) {
            createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
            refreshItemsFromTable();
        }
    });
}
```

**İnvokeApi** yöntemi, yeni özel API için bir POST isteği gönderir istemcide çağrılır. Herhangi bir hata olduğu gibi özel API tarafından döndürülen sonuç bir ileti iletişim kutusu görüntülenir. Diğer sürümleri **invokeApi** isteğe bağlı olarak istek gövdesinde bir nesne göndermek, HTTP yöntemini belirtin ve sorgu parametreleri istekle Gönder olanak tanır. Türsüz sürümleri **invokeApi** de sağlanır.

## <a name="authentication"></a>Uygulamanıza kimlik doğrulaması ekleme

Öğreticiler, bu özellikler ekleme zaten ayrıntılı olarak açıklanmaktadır.

App Service destekler [uygulama kullanıcıların kimliğini doğrulama](app-service-mobile-android-get-started-users.md) çeşitli dış kimlik sağlayıcısı kullanarak: Facebook, Google, Microsoft hesabı, Twitter ve Azure Active Directory. Tablolarda yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi sınırlandırmak için izinleri ayarlayabilirsiniz. Kimliği doğrulanmış kullanıcıların kimliğini, yetkilendirme kuralları arka ucunuzu uygulamak için de kullanabilirsiniz.

İki kimlik doğrulama akışı desteklenir: bir **sunucu** akış ve **istemci** akış. Kimlik sağlayıcıları web arabirimi olmasına olduğundan sunucu akışı Basit kimlik doğrulaması deneyimi sağlar.  Ek SDK, kimlik doğrulaması akışını uygulamak için gereklidir. Akış doğrulaması, mobil cihaz kapsamlı bir tümleştirme sağlamaz ve yalnızca senaryoları kavram kanıtı için önerilir.

Kimlik sağlayıcısı tarafından sağlanan SDK'ları kullanır gibi istemci akışı çoklu oturum açma gibi cihaza özgü özellikler ile daha derin tümleştirme sağlar.  Örneğin, mobil uygulamanıza Facebook SDK tümleştirebilirsiniz.  Mobil istemciyi Facebook uygulamaya değiştirir ve, mobil uygulamanıza geçirmeden önce oturum onaylar.

Dört adımı, uygulamanızda kimlik doğrulamasını etkinleştirmek için gereklidir:

* Bir kimlik sağlayıcısı ile kimlik doğrulaması için uygulamanızı kaydedin.
* App Service arka ucunuzu yapılandırın.
* Tablo Kimliği doğrulanmış kullanıcılara yalnızca App Service arka ucu kısıtlayın.
* Kimlik doğrulama kodu uygulamanıza ekleyin.

Tablolarda yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi sınırlandırmak için izinleri ayarlayabilirsiniz. Kimliği doğrulanmış bir kullanıcının SID, değiştirme isteklerini için de kullanabilirsiniz.  Daha fazla bilgi için gözden [kimlik doğrulamayı kullanmaya başlama] ve sunucu SDK'sını nasıl yapılır belgeleri.

### <a name="caching"></a>Kimlik doğrulaması: Sunucu akışı

Aşağıdaki kod, Google Sağlayıcısı'nı kullanarak bir sunucu akışı oturum açma işlemi başlatır.  Ek yapılandırma, güvenlik gereksinimlerini Google sağlayıcısı nedeniyle gereklidir:

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

Ayrıca, ana etkinlik sınıfına aşağıdaki yöntemi ekleyin:

```java
// You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // When request completes
    if (resultCode == RESULT_OK) {
        // Check the request code matches the one we send in the login request
        if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
            MobileServiceActivityResult result = mClient.onActivityResult(data);
            if (result.isLoggedIn()) {
                // login succeeded
                createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                createTable();
            } else {
                // login failed, check the error message
                String errorMessage = result.getErrorMessage();
                createAndShowDialog(errorMessage, "Error");
            }
        }
    }
}
```

`GOOGLE_LOGIN_REQUEST_CODE` Tanımlanan, ana etkinlik için kullanılan `login()` yöntemi ve içinde `onActivityResult()` yöntemi.  İçinde kullanılan aynı sayıda sürece herhangi bir benzersiz sayı seçebilirsiniz `login()` yöntemi ve `onActivityResult()` yöntemi.  İstemci kodu (daha önce gösterildiği gibi) bir hizmet bağdaştırıcısına soyut, uygun yöntemleri hizmeti bağdaştırıcıda çağırmalıdır.

Ayrıca proje customtabs için yapılandırmak gerekir.  Önce bir yeniden yönlendirme URL'si belirtin.  Aşağıdaki kod parçacığını `AndroidManifest.xml`:

```xml
<activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback"/>
    </intent-filter>
</activity>
```

Ekleme **redirectUriScheme** için `build.gradle` uygulamanız için dosya:

```gradle
android {
    buildTypes {
        release {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
        debug {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
    }
}
```

Son olarak, ekleme `com.android.support:customtabs:28.0.0` bağımlılıklar listesine `build.gradle` dosyası:

```gradle
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.google.code.gson:gson:2.3'
    implementation 'com.google.guava:guava:18.0'
    implementation 'com.android.support:customtabs:28.0.0'
    implementation 'com.squareup.okhttp:okhttp:2.5.0'
    implementation 'com.microsoft.azure:azure-mobile-android:3.4.0@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@jar'
}
```

Oturum açmış olan kullanıcı Kimliğini almak bir **MobileServiceUser** kullanarak **getuserıd öğesini** yöntemi. Zaman uyumsuz oturum açma API'leri çağırmak için vadeli kullanma örneği için bkz: [kimlik doğrulamayı kullanmaya başlama].

> [!WARNING]
> Belirtilen URL düzeni, büyük/küçük harf duyarlıdır.  Emin tüm oluşumlarını `{url_scheme_of_you_app}` eşleşen servis talebi.

### <a name="caching"></a>Kimlik doğrulama belirteçlerini önbelleğe alma

Kimlik doğrulama belirteçlerini önbelleğe alma kullanıcı kimliği ve kimlik doğrulama belirteci cihazda yerel olarak depolamak gerekir. Uygulamayı bir sonraki başlatılışında cache'i kontrol etme ve bu değerler varsa günlüğünde yordamı atlayın ve istemci ile bu verileri yeniden doldurma. Ancak bu veriler hassas ve güvenliği için telefon çalınırsa durumunda şifrelenmiş depolanması gerekir.  Nasıl önbellek kimlik doğrulama belirteçleri için bir tam örnek gördüğünüz [önbellek kimlik doğrulama belirteçleri bölümü][7].

Süresi dolmuş bir belirteç kullanmaya çalıştığınızda, aldığınız bir *401 Yetkisiz* yanıt. Filtreler kullanılarak kimlik doğrulaması hataları başa çıkabilir.  App Service arka ucu isteklerine filtreler müdahale. Filtreleme kodunu bir 401 yanıtı testleri, oturum açma işlemini tetikler ve ardından 401'i oluşturan istek sürdürür.

### <a name="refresh"></a>Yenileme belirteçleri kullanma

Azure App Service kimlik doğrulaması ve yetkilendirme tarafından döndürülen belirteci tanımlanmış bir saat ömrü vardır.  Bu süre bittikten sonra kullanıcı yeniden kimliğini doğrulaması gerekir.  Ardından istemci akışı kimlik doğrulaması aldığınız uzun süreli bir belirteç kullanıyorsanız, Azure App Service kimlik doğrulaması ve yetkilendirme aynı belirteci kullanarak ile sağlamalarını.  Başka bir Azure App Service belirteci ile yeni bir ömrü oluşturulur.

Ayrıca yenileme belirteçleri kullanılacak sağlayıcıyı kaydedebilirsiniz.  Bir yenileme belirteci her zaman kullanılabilir değil.  Ek yapılandırma gerekli değildir:

* İçin **Azure Active Directory**, Azure Active Directory uygulaması için bir istemci gizli anahtarını yapılandırın.  İstemci gizli anahtarı Azure App Service, Azure Active Directory kimlik doğrulamasını yapılandırırken belirtin.  Çağrılırken `.login()`, geçmesi `response_type=code id_token` bir parametre olarak:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* İçin **Google**, geçmesi `access_type=offline` bir parametre olarak:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* İçin **Microsoft Account**seçin `wl.offline_access` kapsam.

Bir belirteci yenilemek için çağrı `.refreshUser()`:

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

En iyi uygulama, sunucudan bir 401 yanıtı algılar ve kullanıcı belirteci yenilemeye çalışır bir filtre oluşturun.

## <a name="log-in-with-client-flow-authentication"></a>İstemci akışı kimlik bilgilerinizle oturum açın

Oturum açma akışı istemci kimlik doğrulaması için genel süreç aşağıdaki gibidir:

* Akış sunucu kimlik doğrulaması gibi Azure App Service kimlik doğrulaması ve yetkilendirme yapılandırın.
* Bir erişim belirteci oluşturmak için SDK kimlik doğrulaması için kimlik doğrulama sağlayıcısı tümleştirin.
* Çağrı `.login()` yöntemini aşağıdaki şekilde (`result` olmalıdır bir `AuthenticationResult`):

    ```java
    JSONObject payload = new JSONObject();
    payload.put("access_token", result.getAccessToken());
    ListenableFuture<MobileServiceUser> mLogin = mClient.login("{provider}", payload.toString());
    Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
        @Override
        public void onFailure(Throwable exc) {
            exc.printStackTrace();
        }
        @Override
        public void onSuccess(MobileServiceUser user) {
            Log.d(TAG, "Login Complete");
        }
    });
    ```

Sonraki bölümde tam kod örneğe bakın.

Değiştirin `onSuccess()` başarılı bir oturum açma kullanmak istediğiniz, kod ile yöntemi.  `{provider}` Dizedir geçerli sağlayıcı: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, veya **twitter**.  Özel kimlik doğrulama uyguladıysanız, özel kimlik doğrulama sağlayıcısı etiketi de kullanabilirsiniz.

### <a name="adal"></a>Kullanıcıların Active Directory Authentication Library (ADAL) ile kimlik doğrulaması

Kullanıcıların uygulamanızla Azure Active Directory'yi kullanarak oturum açmak için Active Directory Authentication Library (ADAL) kullanabilirsiniz. Bir istemci akışı oturum açma kullanarak genellikle kullanılması tercih `loginAsync()` yöntemleri, daha doğal bir UX görünümünü sağlar ve için ek özelleştirme yapmanıza izin verir.

1. AAD oturum açma için mobil uygulama arka ucunuzu izleyerek yapılandırın [App Service, Active Directory oturum açma için yapılandırma][22] öğretici. Yerel istemci uygulaması kaydetme isteğe bağlı bir adım tamamladığınızdan emin olun.
2. ADAL build.gradle dosyanıza aşağıdaki tanımları içerecek şekilde değiştirerek yükleyin:

    ```gradle
    repositories {
        mavenCentral()
        flatDir {
            dirs 'libs'
        }
        maven {
            url "YourLocalMavenRepoPath\\.m2\\repository"
        }
    }
    packagingOptions {
        exclude 'META-INF/MSFTSIG.RSA'
        exclude 'META-INF/MSFTSIG.SF'
    }
    dependencies {
        implementation fileTree(dir: 'libs', include: ['*.jar'])
        implementation('com.microsoft.aad:adal:1.16.1') {
            exclude group: 'com.android.support'
        } // Recent version is 1.16.1
        implementation 'com.android.support:support-v4:28.0.0'
    }
    ```

3. Aşağıdaki değişiklik yapmadan uygulamanıza aşağıdaki kodu ekleyin:

    * Değiştirin **INSERT yetkilisi burada** uygulamanızı sağlanan Kiracı adı. Biçim olmalıdır https://login.microsoftonline.com/contoso.onmicrosoft.com.
    * Değiştirin **Ekle-RESOURCE-kimliği-Buraya** mobil uygulamanızın arka ucu için istemci kimliği. İstemci kimliği edinebilirsiniz **Gelişmiş** sekmesinde altında **Azure Active Directory ayarları** portalında.
    * Değiştirin **istemci kimliği burayı INSERT** yerel istemci uygulamasından kopyaladığınız istemci kimliği.
    * Değiştirin **ekleme-yeniden yönlendirme-URI-Buraya** sitenizin ile */.auth/login/done* uç noktasını, HTTPS düzenini kullanarak. Bu değer, aşağıdakine benzer olmalıdır *https://contoso.azurewebsites.net/.auth/login/done* .

```java
private AuthenticationContext mContext;

private void authenticate() {
    String authority = "INSERT-AUTHORITY-HERE";
    String resourceId = "INSERT-RESOURCE-ID-HERE";
    String clientId = "INSERT-CLIENT-ID-HERE";
    String redirectUri = "INSERT-REDIRECT-URI-HERE";
    try {
        mContext = new AuthenticationContext(this, authority, true);
        mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
    } catch (Exception exc) {
        exc.printStackTrace();
    }
}

private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
    @Override
    public void onError(Exception exc) {
        if (exc instanceof AuthenticationException) {
            Log.d(TAG, "Cancelled");
        } else {
            Log.d(TAG, "Authentication error:" + exc.getMessage());
        }
    }

    @Override
    public void onSuccess(AuthenticationResult result) {
        if (result == null || result.getAccessToken() == null
                || result.getAccessToken().isEmpty()) {
            Log.d(TAG, "Token is empty");
        } else {
            try {
                JSONObject payload = new JSONObject();
                payload.put("access_token", result.getAccessToken());
                ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        exc.printStackTrace();
                    }
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        Log.d(TAG, "Login Complete");
                    }
                });
            }
            catch (Exception exc){
                Log.d(TAG, "Authentication error:" + exc.getMessage());
            }
        }
    }
};

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (mContext != null) {
        mContext.onActivityResult(requestCode, resultCode, data);
    }
}
```

## <a name="filters"></a>İstemci-sunucu iletişimi ayarlama

İstemci normal Android SDK'sı ile sağlanan temel alınan HTTP kitaplığını kullanarak temel bir HTTP bağlantısı bağlantısıdır.  Neden bunu değiştirmek istiyorsunuz birkaç nedeni vardır:

* Zaman aşımları ayarlamak için alternatif bir HTTP kitaplığı kullanmak istiyorsanız.
* Bir ilerleme çubuğu sağlamak istiyor.
* API Yönetimi işlevselliği desteklemek için özel bir başlık eklemek istiyor.
* Başarısız bir yanıt yeniden kimlik doğrulaması uygulayabilmesi ıntercept istiyor.
* Arka uç istekleri bir analytics hizmetinde oturum açmak istiyor.

### <a name="using-an-alternate-http-library"></a>Alternatif bir HTTP kitaplığı kullanma

Çağrı `.setAndroidHttpClientFactory()` istemci referans oluşturduktan sonra hemen yöntemi.  Örneğin, bağlantı zaman aşımını 60 saniye (yerine, varsayılan 10 saniye) ayarlamak için şunu yazın:

```java
mClient = new MobileServiceClient("https://myappname.azurewebsites.net");
mClient.setAndroidHttpClientFactory(new OkHttpClientFactory() {
    @Override
    public OkHttpClient createOkHttpClient() {
        OkHttpClient client = new OkHttpClient();
        client.setReadTimeout(60, TimeUnit.SECONDS);
        client.setWriteTimeout(60, TimeUnit.SECONDS);
        return client;
    }
});
```

### <a name="implement-a-progress-filter"></a>Uygulama bir ilerleme durumu filtresi

Uygulayarak, bir kesme noktası her isteğin uygulayabileceğiniz bir `ServiceFilter`.  Örneğin, aşağıdaki önceden oluşturulmuş bir ilerleme çubuğu güncelleştirir:

```java
private class ProgressFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
            }
        });

        ListenableFuture<ServiceFilterResponse> future = next.onNext(request);
        Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
            @Override
            public void onFailure(Throwable e) {
                resultFuture.setException(e);
            }
            @Override
            public void onSuccess(ServiceFilterResponse response) {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        if (mProgressBar != null)
                            mProgressBar.setVisibility(ProgressBar.GONE);
                    }
                });
                resultFuture.set(response);
            }
        });
        return resultFuture;
    }
}
```

Bu filtre istemciye şu şekilde ekleyebilirsiniz:

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a>İstek üstbilgilerini özelleştirme

Aşağıdaki `ServiceFilter` ve aynı şekilde filtre ekleme `ProgressFilter`:

```java
private class CustomHeaderFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                request.addHeader("X-APIM-Router", "mobileBackend");
            }
        });
        SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
        try {
            ServiceFilterResponse response = next.onNext(request).get();
            result.set(response);
        } catch (Exception exc) {
            result.setException(exc);
        }
    }
}
```

### <a name="conversions"></a>Otomatik serileştirme yapılandırın

Kullanarak her sütun için geçerli bir dönüştürme stratejisi belirtebilirsiniz [gson][3] API. Android istemci Kitaplığı'nı kullanan [gson][3] arka planda, Azure App Service'te veri gönderilmeden önce Java nesnelerini JSON verilerini seri hale getirmek için.  Aşağıdaki kod **setFieldNamingStrategy()** stratejisi ayarlamak için yöntemi. Bu örnekte, ilk karakter ("m"), siler ve ardından küçük her alan adı için bir sonraki karakteri. Örneğin, "id" içinde "Orta" etkinleştirmeniz  Gereksinimini azaltmak için bir dönüştürme stratejisi uygulamak `SerializedName()` çoğu alanlarda ek açıklamalar.

```java
FieldNamingStrategy namingStrategy = new FieldNamingStrategy() {
    public String translateName(File field) {
        String name = field.getName();
        return Character.toLowerCase(name.charAt(1)) + name.substring(2);
    }
}

client.setGsonBuilder(
    MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(namingStrategy)
);
```

Bu kod, bir mobil istemci başvurusu kullanarak oluşturmadan önce yürütülmelidir **MobileServiceClient**.

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: https://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: https://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Kimlik doğrulamayı kullanmaya başlama]: app-service-mobile-android-get-started-users.md
[1]: https://static.javadoc.io/com.google.code.gson/gson/2.8.5/com/google/gson/JsonObject.html
[2]: https://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: https://www.javadoc.io/doc/com.google.code.gson/gson/2.8.5
[4]: https://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: https://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: https://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: https://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: https://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[17]: https://developer.android.com/reference/java/util/UUID.html
[18]: https://github.com/google/guava/wiki/ListenableFutureExplained
[19]: https://www.odata.org/documentation/odata-version-3-0/
[20]: https://hashtagfail.com/post/46493261719/mobile-services-android-querying
[21]: https://github.com/Azure-Samples/azure-mobile-apps-android-quickstart
[22]: ../app-service/configure-authentication-provider-aad.md
[Future]: https://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: https://developer.android.com/reference/android/os/AsyncTask.html
