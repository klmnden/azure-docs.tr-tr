---
title: "Android için Azure Mobile Apps SDK'sını kullanma | Microsoft Docs"
description: "Android için Azure Mobile Apps SDK'sını kullanma"
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
ms.assetid: 5352d1e4-7685-4a11-aaf4-10bd2fa9f9fc
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: glenga
ms.openlocfilehash: c1b868c07522a8df8b574b3bf3d31de512a547fe
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="how-to-use-the-azure-mobile-apps-sdk-for-android"></a>Android için Azure Mobile Apps SDK'sını kullanma

Bu kılavuzu gibi yaygın senaryolar uygulamak için mobil uygulamalar için SDK'sı Android istemci kullanmayı gösterir:

* Verileri (ekleme, güncelleştirme ve silme) sorgulama.
* Kimlik doğrulaması.
* Hataları işleme.
* İstemci özelleştirme.

Bu kılavuz, istemci-tarafı Android SDK odaklanmıştır.  Mobil uygulamaları için sunucu tarafı SDK'ları hakkında daha fazla bilgi için bkz: [iş SDK .NET arka ucu ile] [ 10] veya [Node.js arka ucu SDK kullanmayı][11].

## <a name="reference-documentation"></a>Başvuru belgeleri

Bulabileceğiniz [Javadocs API Başvurusu] [ 12] github'da Android istemci kitaplığı.

## <a name="supported-platforms"></a>Desteklenen platformlar

Android için Azure Mobile Apps SDK'sı telefon ve tablet form faktörleri için API düzey 19 ile 24 (KitKat Nougat aracılığıyla) destekler.  Kimlik doğrulaması, özellikle, kimlik bilgilerini toplamak için ortak bir web framework yaklaşımı kullanır.  Sunucu akış kimlik doğrulaması saatlerde gibi küçük form faktörü cihazlarla çalışmaz.

## <a name="setup-and-prerequisites"></a>Kurulum ve Önkoşullar

Tamamlamak [Mobile Apps quickstart](app-service-mobile-android-get-started.md) Öğreticisi.  Bu görev, Azure Mobile Apps geliştirmek için tüm ön koşulların yerine sağlar.  Hızlı Başlangıç ayrıca hesabınızı yapılandırma ve ilk mobil uygulama arka oluşturmanıza yardımcı olur.

Hızlı Başlangıç Öğreticisi tamamlamak istemediğinize karar verirseniz, aşağıdaki görevleri tamamlayın:

* [bir mobil uygulama arka ucu oluşturma] [ 13] Android uygulamanızı kullanılacak.
* Android Studio'da [güncelleştirme Gradle derleme dosyalarını](#gradle-build).
* [Internet izni etkinleştirmek](#enable-internet).

### <a name="gradle-build"></a>Gradle derleme dosyasını güncelleştirme

Her ikisi de değiştirme **build.gradle** dosyaları:

1. Bu kodu ekleyin *proje* düzeyi **build.gradle** içinde dosya *buildscript* etiketi:

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. Bu kodu ekleyin *modülü uygulama* düzeyi **build.gradle** içinde dosya *bağımlılıkları* etiketi:

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.3.0'
    ```

    Şu anda son 3.3.0 sürümüdür. Desteklenen sürümleri listelenir [bintray'deki üzerinde][14].

### <a name="enable-internet"></a>Internet izin etkinleştir

Azure erişmek için uygulamanızı etkin Internet izni olmalıdır. Zaten etkinleştirilmişse, aşağıdaki kod satırını ekleyin, **AndroidManifest.xml** dosyası:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a>İstemci bağlantısı oluşturma

Azure mobil uygulamalar, mobil uygulamanıza dört işlevleri sağlar:

* Veri erişimi ve bir Azure mobil uygulamalar hizmeti ile çevrimdışı eşitleme.
* Özel Azure mobil uygulamalar sunucusu SDK ile yazılmış API'leri çağırın.
* Azure uygulama hizmeti kimlik doğrulama ve yetkilendirme ile kimlik doğrulaması.
* Anında iletme bildirimi kaydı Notification Hubs ile.

Bu işlevlerin her biri ilk oluşturduğunuz gerektiren bir `MobileServiceClient` nesnesi.  Yalnızca bir `MobileServiceClient` nesne içinde mobil istemci oluşturulmalıdır (diğer bir deyişle, bir Singleton deseni olmalıdır).  Oluşturmak için bir `MobileServiceClient` nesnesi:

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with the Site URL
    this);                  // Your application Context
```

`<MobileAppUrl>` Bir dize ya da mobil arka ucunuza işaret eden bir URL nesnesi.  Mobil arka barındırmak için Azure App Service kullanıyorsanız, kullandığınız güvenli olduğundan emin olun `https://` URL sürümü.

İstemci ayrıca etkinlik veya Context - erişmesi `this` örnekte parametresi.  MobileServiceClient yapı içinde gerçekleşmelidir `onCreate()` yöntemi etkinliğin başvurulan `AndroidManifest.xml` dosya.

En iyi uygulama, sunucu iletişimi kendi (singleton deseni) sınıfına soyut.  Bu durumda, hizmeti uygun şekilde yapılandırmak için oluşturucusu içinde etkinlik geçirmelisiniz.  Örneğin:

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

Şimdi Ara `AzureServiceAdapter.Initialize(this);` içinde `onCreate()` ana etkinlik yöntemi.  İstemci erişim gerektiren herhangi bir yöntem `AzureServiceAdapter.getInstance();` hizmeti bağdaştırıcısı için bir başvuru elde edilir.

## <a name="data-operations"></a>Veri işlemleri

Azure Mobile Apps SDK'sı çekirdek mobil uygulama arka uç SQL Azure içinde depolanan verilere erişim sağlamaktır.  Kesin türü belirtilmiş sınıfları (tercih edilen) kullanarak bu verilere erişebilir veya türsüz sorgular (önerilmez).  Bu bölümde toplu kesin türü belirtilmiş sınıflarını kullanma ile ilgilidir.

### <a name="define-client-data-classes"></a>İstemci veri sınıflarını tanımlamak

SQL Azure tablolardan verilere erişmek için mobil uygulama arka ucu tablolarda için karşılık gelen istemci veri sınıflarını tanımlayın. Bu konudaki örnekler varsayar adlı bir tablo **MyDataTable**, aşağıdaki sütunları vardır:

* id
* Metin
* Tamamlayın

Adlı bir dosyaya karşılık gelen yazılan istemci-tarafı nesnenin bulunduğundan **MyDataTable.java**:

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

Eklediğiniz her bir alan için'Set ' yordamı yöntemleri ekleyin.  SQL Azure tablonuz daha fazla sütun içeriyorsa, bu sınıfa karşılık gelen alanlara eklersiniz.  Örneğin, varsa DTO (veri aktarımı nesne) sahip bir tamsayı öncelik sütunu ve'Set ' yordamı yöntemlerinin yanı sıra bu alanı ekleyebilirsiniz:

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

Ek tablolar, Mobile Apps arka oluşturmayı öğrenmek için bkz: [nasıl yapılır: bir tablo denetleyicisi tanımlamak] [ 15] (.NET arka ucu) veya [dinamik bir şema kullanarak tabloları tanımlayın] [ 16] (Node.js arka ucu).

Bir Azure Mobile Apps arka uç tablosu dört biri istemciler için kullanılabilir beş özel alanlar tanımlar:

* `String id`: Kaydı genel benzersiz kimliği.  En iyi uygulama, dize gösterimini kimliği olun bir [UUID] [ 17] nesnesi.
* `DateTimeOffset updatedAt`: Son tarih/saat güncelleştirin.  UpdatedAt alan sunucu tarafından ayarlanır ve istemci kodunuz tarafından hiçbir zaman ayarlamanız gerekir.
* `DateTimeOffset createdAt`: Bu nesnenin oluşturulduğu tarih.  CreatedAt alan sunucu tarafından ayarlanır ve istemci kodunuz tarafından hiçbir zaman ayarlamanız gerekir.
* `byte[] version`: Normal olarak bir dize olarak gösterilen, sürüm sunucu tarafından ayarlanır.
* `boolean deleted`: Kayıt silindi ancak henüz temizlendi değil olduğunu gösterir.  Kullanmayın `deleted` sınıfınız özelliği olarak.

`id` Alan gereklidir.  `updatedAt` Alan ve `version` alan çevrimdışı eşitleme için kullanılan (Artımlı eşitleme ve Çakışma çözümlemesi için sırasıyla).  `createdAt` Alan başvurusu alan ve istemci tarafından kullanılmaz.  Adları "arasında hat" adlarının özelliklerinin ve ayarlanabilir değildir.  Ancak, nesneniz ve "arasında hat" adları kullanarak arasında bir eşleme oluşturabilirsiniz [gson] [ 3] kitaplığı.  Örneğin:

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
    public DateTimeOffset getUpdatedAt() { return mCreatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset createdAt) { mCreatedAt = createdAt; }

    @com.google.gson.annotations.SerializedName("updatedAt")
    private DateTimeOffset mUpdatedAt;
    public DateTimeOffset getUpdatedAt() { return mUpdatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset updatedAt) { mUpdatedAt = updatedAt; }

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

### <a name="create-a-table-reference"></a>Bir tablo başvurusu oluşturma

Bir tablo erişmek için ilk oluşturma bir [MobileServiceTable] [ 8] çağırarak nesne **getTable** yöntemi [MobileServiceClient] [9].  Bu yöntem iki aşırı yüklemeye sahip:

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

Aşağıdaki kodda, **mClient** MobileServiceClient nesnenizin başvurudur.  İlk aşırı burada sınıf adı ve tablo adı aynı olan ve bir hızlı başlangıcı kullanılan kullanılır:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

Tablo adı sınıfı adından farklı olduğunda ikinci aşırı kullanılır: ilk parametre tablo adıdır.

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <a name="query"></a>Arka uç tablosu sorgulama

İlk olarak, bir tablo başvurusu edinin.  Ardından bir sorgu üzerinde tablo başvurusu yürütün.  Bir sorgu herhangi bir bileşimini şöyledir:

* A `.where()` [filtre yan tümcesi](#filtering).
* Bir `.orderBy()` [yan tümcesinin sıralama](#sorting).
* A `.select()` [alan seçimi yan tümcesi](#selection).
* A `.skip()` ve `.top()` için [sonuçları disk belleği](#paging).

Yan tümceler önceki sırayla sunulmalıdır.

### <a name="filter"></a>Sonuçları filtreleme

Genel sorgu şu şekildedir:

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts the async into a sync result
```

Önceki örnekte tüm sonuçları (kadar en büyük sayfa boyutu sunucu tarafından ayarını) döndürür.  `.execute()` Yöntemi arka uçta sorgu yürütür.  Sorgu dönüştürülür bir [OData v3] [ 19] Mobile Apps arka uç iletimden önce sorgu.  Alındığında, Mobile Apps arka uç SQL Azure örneğinde yürütmeden önce bir SQL deyimini sorgu dönüştürür.  Ağ etkinliği biraz zaman alır beri `.execute()` yöntemi döndürür bir [ `ListenableFuture<E>` ] [ 18].

### <a name="filtering"></a>Döndürülen veri filtreleme

Aşağıdaki sorgu yürütme tüm öğeleri döndürür **Todoıtem** tablo nerede **tam** eşittir **false**.

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

**mToDoTable** daha önce oluşturduğumuz mobil hizmeti tablo başvurusudur.

Bir filtre kullanarak tanımlarsınız **burada** tablo başvurusu üzerinde yöntem çağrısı. **Nerede** yöntemi tarafından izlenen bir **alan** yöntemi ve ardından bir yöntemle mantıksal koşulu belirtir. Olası koşul yöntemleri dahil **eq** (eşittir) **ne** (eşit değildir), **gt** (büyük), **ge** (büyük veya eşittir) **lt** (küçüktür), **le** (küçük veya eşittir). Bu yöntemleri belirli değerleri numarası ve dize alanları karşılaştırmanıza olanak sağlar.

Tarihleri filtreleyebilirsiniz. Aşağıdaki yöntemlerden tüm tarih alanı ya da tarih kısımlarını karşılaştırmanıza olanak tanır: **yıl**, **ay**, **gün**, **saat**,  **dakika**, ve **ikinci**. Aşağıdaki örnek, öğe için bir filtre ekler, *son tarih* 2013'e eşittir.

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

Aşağıdaki yöntemlerden karmaşık filtreler dize alanları destekler: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **Değiştir**, **toLower**, **toUpper**, **kırpma**, ve **uzunluğu** . Aşağıdaki örnek filtreleri tablosu için satırları *metin* sütun "PRI0" ile başlar

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

Aşağıdaki işleci yöntemleri numara alanları desteklenir: **ekleme**, **alt**, **mul**, **div**, **mod**, **kat**, **tavan**, ve **yuvarlak**. Aşağıdaki örnek filtreleri tablosu için satırları **süresi** bir çift sayı.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

Bu mantıksal yöntemlerle koşulları birleştirebilirsiniz: **ve**, **veya** ve **değil**. Aşağıdaki örnekte iki önceki örneklerin birleştirir.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

Mantıksal işleçler: Grup ve iç içe geçirme

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

Daha ayrıntılı tartışma ve filtreleme örnekleri için bkz: [Android istemci sorgu modelini zenginliğinin keşfetme][20].

### <a name="sorting"></a>Döndürülen veriler sıralama

Aşağıdaki kod bir tablodan tüm öğeleri döndürür **Todoıtems** göre artan *metin* alan. *mToDoTable* daha önce oluşturduğunuz arka uç tablo başvurusudur:

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

Öğesinin ilk parametresi **orderBy** bir dize sıralama yapılacak alanı adına eşit bir yöntemdir. İkinci parametre **QueryOrder** artan veya azalan sıralama belirtmek için numaralandırması.  Kullanarak filtre ***nerede*** yöntemi, ***nerede*** yöntemi çağrılır, önce ***orderBy*** yöntemi.

### <a name="selection"></a>Belirli sütunları seçin

Aşağıdaki kod bir tablodan tüm öğeleri döndürmek nasıl gösterir **Todoıtems**, ancak yalnızca görüntüler **tam** ve **metin** alanları. **mToDoTable** daha önce oluşturduğumuz arka uç tablo başvurusudur.

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

Select işlevi parametreleri döndürmek istediğiniz tablonun sütunlarının dize adlardır.  **Seçin** yöntemi gibi yöntemleri uygulayın gerekiyor **nerede** ve **orderBy**. Disk belleği yöntemler gibi tarafından izlenebilir **atla** ve **üst**.

### <a name="paging"></a>Dönüş verileri sayfalarında

Veri **her zaman** sayfalarında döndürdü.  Döndürülen kayıt sayısını sunucu tarafından ayarlanır.  Daha fazla kayıt istemci isteğinde bulunursa sunucu en fazla kayıt sayısı döndürür.  Varsayılan olarak, sunucu üzerindeki en büyük sayfa boyutu 50 kayıttır.

İlk örnek bir tablodan en üstteki beş öğelerini seçmek nasıl gösterir. Sorgu bir tablodan döndürürse **Todoıtems**. **mToDoTable** daha önce oluşturduğunuz arka uç tablo başvurusudur:

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

İlk beş öğeleri atlar ve ardından sonraki beş döndüren bir sorgu şöyledir:

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

Bir tablodaki tüm kayıtları almak istiyorsanız, tüm sayfaları yinelemek için kod uygulayın:

```java
List<MyDataModel> results = new List<MyDataModel>();
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

Bir istek bu yöntemi kullanarak tüm kayıtlar için en az iki isteklerine Mobile Apps arka uç oluşturur.

> [!TIP]
> Sağ sayfa boyutunu seçme isteği sürerken bellek kullanımı, bant genişliği kullanımı ve veri tamamen alma gecikme arasında bir denge ' dir.  Varsayılan değer (50 kayıtları) tüm aygıtlar için uygundur.  Özellikle büyük bellek cihazlarda çalışmazsa, en fazla 500 artırın.  Biz, kabul edilebilir gecikme ve büyük bellek sorunları 500 kayıtları sonuçlarında ötesinde sayfa boyutunu artırmayı buldunuz.

### <a name="chaining"></a>Nasıl yapılır: Sorgu yöntemleri birleştirme

Arka uç tabloları sorgulama kullanılan yöntemleri art arda eklenmiş. Sorgu yöntemleri zincirleme sıralanır ve disk belleği filtrelenmiş satır belirli sütunları seçmek sağlar. Karmaşık mantıksal filtreler oluşturabilirsiniz.  Her sorgu yöntemi, bir sorgu nesnesi döndürür. Bir dizi yöntem bitirmek ve gerçekte sorguyu çalıştırmak için arama **yürütme** yöntemi. Örneğin:

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
3. Seçim (**seçin**) yöntemleri.
4. disk belleği (**atla** ve **üst**) yöntemleri.

## <a name="binding"></a>Veri bağlama için kullanıcı arabirimi

Veri bağlama üç bileşenleri içerir:

* Veri kaynağı
* Ekranı düzeni
* İki birbirine bağlar bağdaştırıcısı.

Bizim örnek kodda veri Mobile Apps SQL Azure tablosundan döndürürüz **Todoıtem** bir dizi içine. Veri uygulamaları için genel bir desen etkinliktir.  Veritabanı sorguları genellikle bir liste veya dizi istemci alır satır koleksiyonu döndürür. Bu örnekte, veri kaynağı bir dizidir.  Kod cihazda görüntülenen verileri görünümünü tanımlayan bir ekran düzenini belirtir.  İki bağlı olan bu kodda bir uzantı bir bağdaştırıcı ile birlikte, **ArrayAdapter&lt;Todoıtem&gt;**  sınıfı.

#### <a name="layout"></a>Düzen tanımlayın

Düzen XML kodunu birkaç parçacıkları tarafından tanımlanır. Varolan bir düzeni verilen, aşağıdaki kod temsil **ListView** bizim sunucu veri ile doldurulacak istiyoruz.

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

Önceki kod *LISTITEM* özniteliği listesinde tek bir satır için Düzen kimliğini belirtir. Bu kod bir onay kutusu ve ilgili metin belirtir ve listedeki her bir öğe için bir kez örneği. Bu düzen görüntülenmez **kimliği** alan ve daha karmaşık bir düzen belirtirsiniz ek alanlar görüntüleme. Bu kod **row_list_to_do.xml** dosya.

```java
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
Veri kaynağı bizim görünümünün bir dizi olduğundan **Todoıtem**, biz alt bizim bağdaştırıcısından bir **ArrayAdapter&lt;Todoıtem&gt;**  sınıfı. Bu alt sınıf için bir görünüm üreten her **Todoıtem** kullanarak **row_list_to_do** düzeni.  Bizim kodda bir uzantısıdır ve aşağıdaki sınıf tanımlarız **ArrayAdapter&lt;E&gt;**  sınıfı:

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

Bağdaştırıcıları geçersiz kılma **getView** yöntemi. Örneğin:

```
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

Biz bu sınıfının bir örneği bizim etkinliğinde gibi oluşturun:

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

ToDoItemAdapter Oluşturucusu ikinci parametresi, Düzen başvurudur. Biz şimdi örneğini oluşturabilirsiniz **ListView** ve bağdaştırıcıya atayın **ListView**.

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <a name="use-adapter"></a>Kullanıcı arabirimini bağlaması bağdaştırıcıya kullanın

Veri bağlama kullanmak artık hazırsınız. Aşağıdaki kod tabloda öğeleri alma gösterir ve yerel bağdaştırıcı döndürülen öğe ile doldurur.

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

Bağdaştırıcı, değiştirmek istediğiniz zaman arama **Todoıtem** tablo. Bir kayıt kayıt temelinde yapılır ve değişiklikleri olduğundan, bir koleksiyon yerine tek bir satır işleyici. Bir öğe eklediğinizde, çağrı **ekleme** yöntemi bağdaştırıcısında; silerken, çağrı **kaldırmak** yöntemi.

Tam bir örnek bulabilirsiniz [Android hızlı başlangıç projesi][21].

## <a name="inserting"></a>Arka uç veri Ekle

Bir örneği *Todoıtem* sınıfını ve özelliklerini ayarlayın.

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

Ardından **INSERT()** nesne eklemek için:

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

Döndürülen varlığı eşleşmeleri arka uç tabloya eklenen verilere dahil kimliği ve diğer tüm değerler (gibi `createdAt`, `updatedAt`, ve `version` alanları) arka uç ayarlayın.

Mobile Apps tabloları gerektiren adlı birincil anahtar sütunu **kimliği**. Bu sütun bir dize olmalıdır. Kimlik sütununun varsayılan değeri bir GUID değeridir.  E-posta adresleri veya kullanıcı adları gibi diğer benzersiz değerler sağlayabilirsiniz. Bir dize kimliği değeri için eklenen bir kaydı sağlanmadığında arka uç yeni bir GUID oluşturur.

Dize kimliği değerleri aşağıdaki avantajları sağlar:

* Kimlikleri veritabanına gidiş dönüş yapmadan oluşturulabilir.
* Kayıtları farklı tablolar veya veritabanlarına birleştirme kolaydır.
* KOD değerleri daha iyi bir uygulama mantığı ile tümleştirin.

Dize kimliği değerler **gerekli** çevrimdışı eşitleme desteği.  Arka uç veritabanında depolandıktan sonra bir kimliğini değiştiremezsiniz.

## <a name="updating"></a>Bir mobil uygulama verileri güncelleştirme

Bir tablodaki verileri güncelleştirmek için yeni nesne için geçirmek **update()** yöntemi.

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

Bu örnekte, *öğesi* bir satıra bir başvuru *Todoıtem* üzerinde yapılan bazı değişiklikler dolmadığı tablo.  Aynı satır **kimliği** güncelleştirilir.

## <a name="deleting"></a>Bir mobil uygulama verilerini sil

Aşağıdaki kod, veri nesnesi belirterek tablodan veri silme gösterilmektedir.

```java
mToDoTable
    .delete(item);
```

Belirterek öğeyi silebilirsiniz **kimliği** silmek için satırı alanı.

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <a name="lookup"></a>Belirli bir öğeyi kimliğe göre arayın

Belirli bir sahip bir öğe aramak **kimliği** ile alan **lookUp()** yöntemi:

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <a name="untyped"></a>Nasıl yapılır: türsüz verilerle çalışma

Türsüz programlama modeli, JSON seri hale getirme üzerinde tam denetim sağlar.  Burada bir türsüz programlama modelini kullanmak isteyebilirsiniz bazı yaygın senaryolar vardır. Örneğin, arka uç tablonuz fazla sayıda sütun içeriyor ve yalnızca bir sütun alt kümesini başvuru gerekiyorsa.  Yazılı modeli Mobile Apps arka uç veri sınıfınızda tanımlanan tüm sütunları tanımlamanızı gerektirir.  Verilere erişmek için API çağrıları çoğunu yazılan programlama çağrıları benzerdir. Ana farktır türsüz modelinde, yöntemleri üzerinde çağırmasını **MobileServiceJsonTable** nesnesi yerine **MobileServiceTable** nesnesi.

### <a name="json_instance"></a>Türsüz tablo örneği oluşturma

Yazılı modeline benzer bir tablo başvurusu alarak başlattığınız, ancak bu durumda olduğu bir **MobileServicesJsonTable** nesnesi. Başvuru çağırarak elde **getTable** istemci örneği üzerinde yöntemi:

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

Örneği oluşturduktan sonra **MobileServiceJsonTable**, neredeyse aynı API ile yazılan programlama modeli kullanılabilir olarak sahiptir. Bazı durumlarda, yöntemleri türü belirsiz bir parametre türü belirtilmiş bir parametre yerine getirin.

### <a name="json_insert"></a>Türü belirsiz bir tabloya ekleme
Aşağıdaki kod, bir ekleme yapmak gösterilmiştir. İlk adım oluşturmaktır bir [JsonObject][1], parçası olduğu [gson] [ 3] kitaplığı.

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

Ardından, **INSERT()** tabloya türsüz nesne eklemek için.

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

Eklenen nesnesinin kimliği almanız gereken kullanırsanız **getAsJsonPrimitive()** yöntemi.

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <a name="json_delete"></a>Türü belirsiz bir tablodan silin
Aşağıdaki kod örneği, bu durumda, aynı örneğini silmek nasıl gösterir bir **JsonObject** önceden oluşturulmuş *Ekle* örnek. Kod yazılan durumunda olduğu gibi ile aynıdır, ancak başvurduğu beri yöntemi farklı bir imzaya sahip bir **JsonObject**.

```java
mToDoTable
    .delete(insertedItem);
```

Doğrudan Kimliğini kullanarak bir örneği daha da silebilirsiniz:

```java
mToDoTable.delete(ID);
```

### <a name="json_get"></a>Türü belirsiz bir tablodan tüm satırları döndürür
Aşağıdaki kod, tüm bir tabloyu almak gösterilmiştir. Bir JSON tablo kullandığından, seçmeli olarak yalnızca bazı tablonun sütunlarının alabilirsiniz.

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

Filtreleme aynı kümesi, filtreleme ve yazılı model için kullanılabilir yöntemleri disk belleği türsüz modeli için kullanılabilir.

## <a name="offline-sync"></a>Çevrimdışı eşitleme uygulama

Azure Mobile Apps istemci SDK'sı sunucu verilerini yerel olarak bir kopyasını saklamak için bir SQLite veritabanı kullanarak çevrimdışı veri eşitlemeyi de uygular.  Çevrimdışı bir tablo üzerinde gerçekleştirilen işlemler, iş için mobil bağlantısı gerektirmez.  Çevrimdışı eşitleme yardımları esnekliği ve çakışma çözümü için daha karmaşık mantığı ödün verme pahasına performans.  Azure Mobile Apps istemci SDK'sı aşağıdaki özellikleri gerçekleştirir:

* Artımlı eşitleme: Bant genişliği ve bellek tüketimi kaydetme yalnızca güncelleştirilmiş ve yeni kayıtlar indirilir.
* İyimser eşzamanlılık: İşlemleri başarılı olarak kabul edilir.  Çakışma çözümü, güncelleştirmelerinin sunucuda gerçekleştirilir kadar ertelenir.
* Çakışma çözümü: çakışan değişikliği sunucuda yapılan ve kullanıcıyı uyarmak için kancaları SDK algılar.
* Geçici silme: Silinen kayıtlar, çevrimdışı önbelleklerini güncelleştirmek diğer cihazları izin vererek, silinen işaretlenir.

### <a name="initialize-offline-sync"></a>Çevrimdışı eşitleme başlatılamadı

Çevrimdışı her tablo kullanmadan önce çevrimdışı önbellek tanımlanması gerekir.  Normalde, tablo tanımı istemci oluşturulduktan hemen sonra gerçekleştirilir:

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

### <a name="obtain-a-reference-to-the-offline-cache-table"></a>Çevrimdışı Önbellek tablo başvurusu alın

Çevrimiçi bir tablo için kullandığınız `.getTable()`.  Çevrimdışı bir tablo için kullanın `.getSyncTable()`:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

(Filtreleme, sıralama, disk belleği, veri ekleme, verilerini güncelleştirmek ve verileri silme de dahil olmak üzere) çevrimiçi tablolar için kullanılabilir olan tüm yöntemleri eşit oranda işe çevrimiçi ve çevrimdışı tablolarda.

### <a name="synchronize-the-local-offline-cache"></a>Yerel Çevrimdışı Önbellek Eşitle

Eşitleme, uygulamanızın içinde denetimdir.  Bir örnek eşitleme yöntemi şöyledir:

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

İçin bir sorgu adı sağlanmışsa `.pull(query, queryname)` yöntemi sonra Artımlı eşitleme oluşturulan veya en son başarıyla değiştirildi yalnızca kayıtlar tamamlandı çekme döndürmek için kullanılır.

### <a name="handle-conflicts-during-offline-synchronization"></a>Çevrimdışı eşitleme sırasında çakışmalarını işleme

Sırasında bir çakışma olursa bir `.push()` işlemi, bir `MobileServiceConflictException` oluşturulur.   Sunucu tarafından verilen öğesi özel durum katıştırılır ve tarafından alınabilir `.getItem()` özel durumunda.  Anında iletme MobileServiceSyncContext nesnesinde aşağıdaki öğeleri çağırarak ayarlayın:

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

İstediğiniz gibi tüm çakışmaları işaretlenmiş sonra çağrı `.push()` yeniden tüm çakışmaları çözümlemek için.

## <a name="custom-api"></a>Özel bir API çağrısı

Özel bir API değil eşlemek için bir ekleme, güncelleştirme, silme veya okuma işlemi sunucusu işlevselliği kullanıma özel uç noktaları tanımlamanızı sağlar. Özel bir API kullanarak, okuma ve HTTP ileti üstbilgilerini ayarlama ve JSON dışında bir ileti gövdesinin biçimi tanımlama gibi Mesajlaşma üzerinde daha fazla denetim olabilir.

Bir Android istemciden çağırmanız **invokeApi** özel API uç noktası çağrılacak yöntem. Aşağıdaki örnek adlı bir API uç noktası çağrı gösterilmektedir **completeAll**, adlı bir koleksiyon sınıfı döndürür **MarkAllResult**.

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

**İnvokeApi** yöntemi yeni özel API için bir POST isteği gönderir istemcide çağrılır. Herhangi bir hata olduğundan özel API tarafından döndürülen sonuç bir ileti iletişim kutusunda görüntülenir. Diğer sürümleri **invokeApi** isteğe bağlı olarak istek gövdesinde nesneyi gönderme, HTTP yöntemini belirtin ve sorgu parametreleri istekle Gönder olanak tanır. Türsüz sürümlerini **invokeApi** de sağlanır.

## <a name="authentication"></a>Uygulamanıza kimlik doğrulaması ekleme

Öğreticiler zaten bu özellikler ekleme ayrıntılı olarak açıklanmaktadır.

Uygulama hizmetini destekleyen [uygulama kullanıcıların kimlik doğrulaması](app-service-mobile-android-get-started-users.md) çeşitli dış kimlik sağlayıcılarını kullanarak: Facebook, Google, Microsoft Account, Twitter ve Azure Active Directory. Yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi kısıtlamak için tablolarda izinlerini ayarlayabilirsiniz. Yetkilendirme kuralları arka ucunuzu uygulamak için kimliği doğrulanmış kullanıcıların kimliğini de kullanabilirsiniz.

İki kimlik doğrulama akışı desteklenir: bir **server** akış ve **istemci** akış. Kimlik sağlayıcılar web arabirimi alacağından sunucu akış Basit kimlik doğrulama deneyimi sağlar.  Hiçbir ek SDK'ları akış kimlik doğrulaması uygulamak için gereklidir. Kimlik doğrulaması akışı mobil cihaz derin bir tümleştirmeye sağlamaz ve yalnızca kavram senaryoları kanıtı için önerilir.

Kimlik sağlayıcısı tarafından sağlanan SDK'ları kullanır gibi istemci akışı çoklu oturum açma gibi aygıta özgü özellikleri ile daha derin tümleştirme sağlar.  Örneğin, mobil uygulamanıza Facebook SDK'sı tümleştirebilirsiniz.  Mobil istemci Facebook uygulamaya değiştirir ve, mobil uygulamanıza değiştirmeden önce oturum onaylar.

Dört adım, uygulamanıza kimlik doğrulamasını etkinleştirmek için gereklidir:

* Uygulamanızın kimlik doğrulaması için bir kimlik sağlayıcısı ile kaydedin.
* Uygulama hizmeti arka yapılandırın.
* Uygulama hizmeti arka uç kimliği doğrulanmış kullanıcılara yalnızca tablo kısıtlayın.
* Kimlik doğrulama kodu uygulamanıza ekleyin.

Yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi kısıtlamak için tablolarda izinlerini ayarlayabilirsiniz. Kimliği doğrulanmış bir kullanıcı SID'si, istekleri değiştirmek için de kullanabilirsiniz.  Daha fazla bilgi için gözden [kimlik doğrulamayı kullanmaya başlama] ve sunucu SDK nasıl yapılır belgeleri.

### <a name="caching"></a>Kimlik doğrulaması: Sunucu akışı

Aşağıdaki kod, Google sağlayıcısını kullanarak bir sunucu akış oturum açma işlemi başlatır.  Google sağlayıcısı için güvenlik gereksinimleri nedeniyle ek yapılandırma gereklidir:

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

`GOOGLE_LOGIN_REQUEST_CODE` , Ana tanımlanan etkinlik için kullanıldığından `login()` yöntemi ve içinde `onActivityResult()` yöntemi.  İçinde kullanılan aynı numarası olduğu sürece herhangi bir benzersiz sayı seçebilirsiniz `login()` yöntemi ve `onActivityResult()` yöntemi.  İstemci kodu (daha önce gösterildiği gibi) bir hizmet bağdaştırıcısı soyut, hizmet bağdaştırıcıda uygun yöntemlerini çağırmalıdır.

Ayrıca proje customtabs için yapılandırmanız gerekir.  Önce bir yeniden yönlendirme URL'si belirtin.  Aşağıdaki kod parçacığını ekleyin `AndroidManifest.xml`:

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

Ekleme **redirectUriScheme** için `build.gradle` dosya uygulamanız için:

```text
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

Son olarak, ekleme `com.android.support:customtabs:23.0.1` bağımlılıkları listesinde için `build.gradle` dosyası:

```text
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.google.code.gson:gson:2.3'
    compile 'com.google.guava:guava:18.0'
    compile 'com.android.support:customtabs:23.0.1'
    compile 'com.squareup.okhttp:okhttp:2.5.0'
    compile 'com.microsoft.azure:azure-mobile-android:3.2.0@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@jar'
}
```

Oturum açmış kullanıcıdan kimliği elde bir **MobileServiceUser** kullanarak **getuserıd öğesini** yöntemi. Vadeli zaman uyumsuz oturum açma API'leri çağırmak için nasıl kullanılacağını örneği için bkz: [kimlik doğrulamayı kullanmaya başlama].

> [!WARNING]
> Belirtilen URL şeması büyük/küçük harf duyarlıdır.  Emin tüm oluşumlarını `{url_scheme_of_you_app}` eşleşen durumda.

### <a name="caching"></a>Önbellek kimlik doğrulama belirteçleri

Kimlik doğrulama belirteçleri önbelleğe alma kullanıcı kimliği ve kimlik doğrulama belirteci cihazda yerel olarak depolamak gerekir. Uygulamayı bir sonraki başlatılışında önbellek denetleyin ve bu değerleri varsa, yordam günlüğüne atlayın ve istemci bu verilerle rehydrate. Ancak bu verileri duyarlıdır ve telefon çalınırsa durumda güvenliği şifrelenmiş depolanması gerekir.  Tam bir örnek nasıl önbellek kimlik doğrulama belirteçleri için gördüğünüz [önbelleğe kimlik doğrulama belirteçleri bölümü][7].

Süresi dolmuş bir belirteç kullanmaya çalıştığınızda aldığınız bir *yetkisiz 401* yanıt. Filtreleri kullanarak kimlik doğrulama hataları işleyebilir.  Uygulama hizmeti arka uç isteklerine filtreleri kesebilir. Filtre kodu bir 401 yanıtı testleri, oturum açma işlemini tetikler ve 401 oluşturulan istek sürdürür.

### <a name="refresh"></a>Yenileme belirteçleri kullanın

Azure App Service kimlik doğrulaması ve yetkilendirme tarafından döndürülen belirteci tanımlı yaşam süresi bir saat sahip.  Bu süre kullanıcı sağlamalarını gerekir.  Ardından istemci akışı kimlik doğrulaması yoluyla aldığınız uzun süreli bir belirteç kullanıyorsanız, Azure App Service kimlik doğrulaması ve yetkilendirme aynı belirteci kullanan sağlamalarını.  Başka bir Azure uygulama hizmeti belirteci ile yeni bir yaşam süresi oluşturulur.

Ayrıca, yenileme belirteçleri için kullanılan sağlayıcının kaydedebilirsiniz.  Bir yenileme belirteci her zaman kullanılabilir değil.  Ek yapılandırma gerekli değildir:

* İçin **Azure Active Directory**, Azure Active Directory uygulaması için bir istemci parolası yapılandırın.  İstemci parolası, Azure Active Directory kimlik doğrulamasını yapılandırırken Azure App Service'te belirtin.  Çağrılırken `.login()`, geçirmek `response_type=code id_token` bir parametre olarak:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* İçin **Google**, geçirmek `access_type=offline` bir parametre olarak:

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

Bir belirteç yenilemek için arama `.refreshUser()`:

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

En iyi uygulama, sunucudan bir 401 yanıtına algılar ve kullanıcı belirteci yenilemeye çalışır bir filtre oluşturun.

## <a name="log-in-with-client-flow-authentication"></a>Oturum akış olmayan istemci kimlik doğrulaması ile oturum

Akış olmayan istemci kimlik doğrulaması oturum açma için genel işlem aşağıdaki gibidir:

* Azure App Service kimlik doğrulama ve yetkilendirme akışı sunucu kimlik doğrulaması gibi yapılandırın.
* Kimlik doğrulama sağlayıcısı kimlik doğrulaması için bir erişim belirteci üretmek için SDK tümleştirin.
* Çağrı `.login()` yöntemini aşağıdaki şekilde:

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

Değiştir `onSuccess()` başarılı oturum açma kullanmak istediğiniz, kod ile yöntemi.  `{provider}` Dizedir geçerli bir sağlayıcısı: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, veya **twitter**.  Özel kimlik doğrulama uyguladıysanız, özel kimlik doğrulama sağlayıcısı etiketi de kullanabilirsiniz.

### <a name="adal"></a>Kullanıcıların Active Directory Authentication Library (ADAL) ile kimlik doğrulaması

Kullanıcılar Azure Active Directory'yi kullanarak uygulamanıza oturum için Active Directory Authentication Library (ADAL) kullanabilirsiniz. Bir istemci akış oturum açma kullanılarak genellikle kullanılması tercih `loginAsync()` yöntemleri olarak daha yerel bir UX fikir sağlar ve ek özelleştirme için sağlar.

1. İzleyerek, mobil uygulamanızın arka ucuna AAD oturum açma için yapılandırma [uygulama hizmeti Active Directory oturum açma için yapılandırma] [ 22] Öğreticisi. İsteğe bağlı bir adım yerel istemci uygulaması kaydı tamamlamak emin olun.
2. ADAL aşağıdaki tanımları eklemek için build.gradle dosyanızla değiştirerek yükleyin:

```
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
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

1. Aşağıdaki değişiklik yapmadan uygulamanız için aşağıdaki kodu ekleyin:

* Değiştir **INSERT yetkilisi burada** uygulamanızı sağlanan Kiracı adı. Https://login.microsoftonline.com/contoso.onmicrosoft.com biçiminde olmalıdır.
* Değiştir **Ekle-RESOURCE-kimliği-Buraya** , mobil uygulamanızın arka ucuna için istemci kimliği. İstemci kimliği elde edebilirsiniz **Gelişmiş** altında sekmesinde **Azure Active Directory ayarları** Portalı'nda.
* Değiştir **Ekle-istemci-kimliği-Buraya** yerel istemci uygulamasından kopyaladığınız istemci kimliği.
* Değiştir **Ekle-REDIRECT-URI-Buraya** sitenizin ile */.auth/login/done* uç noktasını, HTTPS şeması kullanarak. Bu değer benzer olmalıdır *https://contoso.azurewebsites.net/.auth/login/done*.

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

## <a name="filters"></a>İstemci-sunucu iletişimi Ayarla

İstemci bağlantı normalde Android SDK'sı ile sağlanan temel HTTP kitaplığını kullanarak temel bir HTTP bağlantısı olur.  Neden, değiştirmek istediğiniz birkaç nedeni vardır:

* Zaman aşımları ayarlamak için alternatif bir HTTP kitaplığı kullanmak istediğiniz.
* Bir ilerleme çubuğu sağlamak istiyor.
* API yönetim işlevleri desteklemek için özel bir üstbilgisi eklemek istiyor.
* Böylece, yeniden kimlik doğrulamanın uygulayabileceğiniz başarısız bir yanıt müdahale istiyor.
* Günlük analizi hizmeti için arka uç isteklerini istiyor.

### <a name="using-an-alternate-http-library"></a>Alternatif bir HTTP kitaplığı kullanma

Çağrı `.setAndroidHttpClientFactory()` istemci başvurusu oluşturduktan sonra hemen yöntemi.  Örneğin, bağlantı zaman aşımını 60 saniye (yerine varsayılan 10 saniye) ayarlamak için şunu yazın:

```java
mClient = new MobileServiceClient("https://myappname.azurewebsites.net");
mClient.setAndroidHttpClientFactory(new OkHttpClientFactory() {
    @Override
    public OkHttpClient createOkHttpClient() {
        OkHttpClient client = new OkHttpClinet();
        client.setReadTimeout(60, TimeUnit.SECONDS);
        client.setWriteTimeout(60, TimeUnit.SECONDS);
        return client;
    }
});
```

### <a name="implement-a-progress-filter"></a>Bir ilerleme filtre uygulama

Her istek bir kesme noktası uygulayarak uygulayabilirsiniz bir `ServiceFilter`.  Örneğin, aşağıdaki önceden oluşturulmuş ilerleme çubuğu güncelleştirmeleri:

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
                    pubic void run() {
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

Bu filtre istemciye aşağıdaki gibi ekleyebilirsiniz:

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

Kullanarak her sütun için geçerli bir dönüştürme stratejisi belirtebilirsiniz [gson] [ 3] API. Android istemci kitaplığını kullanan [gson] [ 3] arka planda verileri Azure App Service'e gönderilmeden önce JSON verilerini Java nesnelerini seri hale getirilemedi.  Aşağıdaki kod **setFieldNamingStrategy()** stratejisini ayarlamak için yöntem. Bu örnek ilk karakter (bir "m"), silinmesine neden olur ve ardından küçük her alan adı için sonraki karakteri. Örneğin, "id" "Orta" etkinleştirmeniz  Gereksinimini azaltmak için bir dönüştürme stratejisini uygula `SerializedName()` çoğu alanlara ek açıklamaları.

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
        .setFieldNamingStrategy(namingStategy)
);
```

Bu kod kullanarak bir mobil istemci başvuru oluşturmadan önce yürütülmelidir **MobileServiceClient**.

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[kimlik doğrulamayı kullanmaya başlama]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[17]: https://developer.android.com/reference/java/util/UUID.html
[18]: https://github.com/google/guava/wiki/ListenableFutureExplained
[19]: http://www.odata.org/documentation/odata-version-3-0/
[20]: http://hashtagfail.com/post/46493261719/mobile-services-android-querying
[21]: https://github.com/Azure-Samples/azure-mobile-apps-android-quickstart
[22]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Future]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html
