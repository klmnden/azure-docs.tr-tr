---
title: Özel R modülleri tanımlayın
titleSuffix: Azure Machine Learning Studio
description: Bu konuda, yazar ve özel bir R Studio dağıtma işlemleri açıklanmaktadır. Bu özel R modülleri nedir ve hangi dosyaların bunları tanımlamak için kullanılan açıklar.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 11/29/2017
ms.openlocfilehash: 6d330340ff09ddb6c2bec04259f964f2298dbffc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65025056"
---
# <a name="define-custom-r-modules-for-azure-machine-learning-studio"></a>Özel R modülleri Azure Machine Learning Studio'da tanımlayın

Bu konuda, yazar ve özel bir R Studio dağıtma işlemleri açıklanmaktadır. Bu özel R modülleri nedir ve hangi dosyaların bunları tanımlamak için kullanılan açıklar. Bu, bir modül tanımlama dosyaları oluşturmak nasıl ve dağıtım için modül bir Machine Learning çalışma alanında kaydetmeyi nasıl göstermektedir. Ardından, özel modül tanımında kullanılan öznitelikler ve öğeler daha ayrıntılı olarak açıklanmıştır. Yardımcı işlevleri ve dosyaları ve birden çok çıktı nasıl kullanılacağı da ele alınmıştır. 



## <a name="what-is-a-custom-r-module"></a>Özel bir R Modülü nedir?
A **Özel Modül** çalışma alanınıza yüklediğiniz ve bir Azure Machine Learning Studio denemesine bir parçası olarak yürütülen ve kullanıcı tanımlı bir modül. A **özel R Modülü** kullanıcı tanımlı bir R işlevi yürüten özel bir modüldür. **R** istatistiksel bilgi işlem ve istatistikçilerin ve veri uzmanları tarafından algoritmaları uygulamak için yaygın olarak kullanılan grafik için bir programlama dilidir. Şu anda R özel modüller, ancak destek ek diller zamanlandığı için gelecek sürümleri için desteklenen tek bir dildir.

Özel modüller sahip **birinci sınıf durumu** Azure Machine Learning Studio'da anlamında bunlar diğer modülü gibi kullanılabilir. Yayımlanmış denemeleri veya görselleştirmeler bulunan diğer modüllerle çalıştırılabilir. Modülü, giriş ve çıkış bağlantı noktaları, kullanılacak, modelleme parametreleri ve çeşitli diğer çalışma zamanı davranışları tarafından uygulanan algoritması üzerinde denetiminiz vardır. Özel modüller içeren bir denemeyi kolay paylaşım Azure AI Gallery içine de yayımlanabilir.

## <a name="files-in-a-custom-r-module"></a>Özel bir R Modülü dosyaları
Özel bir R modülü, en az iki dosya içeren bir .zip dosyası tanımlanır:

* A **kaynak dosyası** modülü tarafından kullanıma sunulan R işlevi uygular
* Bir **XML tanım dosyasını** açıklayan özel modül arabirimi

Ek yardımcı dosyalar da özel modülünden erişilebilir işlevselliği sağlar .zip dosyasındaki eklenebilir. Bu seçenek içinde ele alınmıştır **bağımsız değişkenleri** başvuru bölümüne parçası **XML tanımı dosyasındaki öğeleri** hızlı başlangıç örneğini izleyerek.

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a>Hızlı Başlangıç örnek: tanımlayın, paket ve özel bir R modülü Kaydettir
Bu örnekte, özel bir R modülü tarafından gereken dosyaları oluşturmak, bunları bir ZIP dosyasına ekleyin paketleme ve ardından modülü, Machine Learning çalışma alanında kaydetmeyi göstermektedir. Örnek zip paketini ve örnek dosyalarını adresinden indirilip [CustomAddRows.zip karşıdan dosya](https://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).

## <a name="the-source-file"></a>Kaynak dosyası
Örneğini düşünün bir **özel satır Ekle** öğesinin standart uygulaması değiştiren Modülü **Add Rows** iki veri kümesi (veri çerçevelerini) tablosundan satırları (gözlemler) birleştirmek için kullanılan modül. Standart **Add Rows** modülü ikinci girdi veri kümesi satırlarını ilk girdi veri kümesi kullanarak sonuna ekler `rbind` algoritması. Özelleştirilmiş `CustomAddRows` işlevi benzer şekilde iki veri kümesi kabul eder, ancak aynı zamanda bir Boolean takas parametresi ek girdi olarak kabul eder. Takas parametresi ayarlanırsa **FALSE**, standart uygulama olarak aynı veri kümesi döndürür. Ancak takas parametresi ise **TRUE**, bunun yerine ikinci bir veri kümesi sonuna işlevi ilk girdi veri kümesi satır ekler. R uyarlamasını içeren CustomAddRows.R dosyasını `CustomAddRows` işlevi tarafından açığa çıkarılan **özel Add Rows** Modülü aşağıdaki R kodu içeriyor.

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="the-xml-definition-file"></a>XML tanım dosyası
Bu kullanıma sunmak için `CustomAddRows` işlevi bir Azure Machine Learning Studio modülü, bir XML tanım dosyası olarak oluşturulan, belirtmek için nasıl **özel Add Rows** modülü arayın ve davranır. 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another. Dataset 2 is concatenated to Dataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify the base language, script file and R function to use for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: The values of the id attributes in the Input and Arg elements must match the parameter names in the R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>The combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


Dikkat etmeniz önemlidir değerini **kimliği** özniteliklerini **giriş** ve **Arg** XML dosyasında öğeler, R kodu işlevi parametre adları eşleşmelidir Dosya tam olarak CustomAddRows.R: (*dataset1*, *dataset2*, ve *takas* örnekte). Benzer şekilde, değerini **entryPoint** özniteliği **dil** öğesi eşleşmelidir R betiğini işlev adı: (*CustomAddRows* örnekte). 

Buna karşılık, **kimliği** özniteliğini **çıkış** öğesi herhangi bir R betiği değişkenlerine karşılık gelmiyor. Birden fazla çıktı gerekli olduğunda, yalnızca R işlevden yerleştirilen sonuçlarıyla dönmesini *aynı sırada* olarak **çıkışları** öğeleri, XML dosyasında bildirilir.

### <a name="package-and-register-the-module"></a>Paketleme ve modülü Kaydettir
Bu iki dosyaları olarak kaydedin *CustomAddRows.R* ve *CustomAddRows.xml* ve ardından iki birlikte içine ZIP dosyaları bir *CustomAddRows.zip* dosya.

Machine Learning çalışma alanınızda bunları kaydetmek için Machine Learning Studio çalışma alanınıza gidin, **+ yeni** seçin ve düğme alt **MODÜLÜ gelen paket ZIP ->** yeni karşıya yüklemek için **Özel Add Rows** modülü.

![Zip karşıya yükleme](./media/custom-r-modules/upload-from-zip-package.png)

**Özel Add Rows** modülü, makine öğrenimi denemeleri tarafından erişilecek hazır sunuldu.

## <a name="elements-in-the-xml-definition-file"></a>XML tanımı dosyasındaki öğeleri
### <a name="module-elements"></a>Modül öğeleri
**Modülü** öğesi XML dosyasında özel bir modülü tanımlamak için kullanılır. Birden çok kullanarak bir XML dosyasında birden çok modül tanımlanabilir **Modülü** öğeleri. Çalışma alanınızdaki her modülün benzersiz bir ad olmalıdır. Varolan bir özel modülle aynı ada sahip bir özel modülü kaydetmek ve mevcut modülü yenisiyle değiştirir. Özel modüller olabilir ancak mevcut bir Azure Machine Learning Studio modülü aynı ada sahip kayıtlı. Bu nedenle, bunlar içinde görünüp görünmediğini **özel** modül paletinin kategorisidir.

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another...</Description>/> 


İçinde **Modülü** öğesini iki ek isteğe bağlı öğeler belirtebilirsiniz:

* bir **sahibi** modüle katıştırılmış öğesi  
* bir **açıklama** modülü için hızlı Yardımı'nda görüntülenir ve Machine Learning UI modülünde üzerine geldiğinizde metin içeren öğe.

Modül öğeleri karakter sınırları için kuralları:

* Değerini **adı** özniteliğini **Modülü** öğe uzunluğu 64 karakteri aşmamalıdır. 
* İçeriği **açıklama** öğesi 128 karakterden uzun olamaz.
* İçeriği **sahibi** öğesi 32 karakter uzunluğunda olamaz.

Bir modülün sonuçları belirleyici olabilir veya nondeterministic.* * varsayılan olarak, tüm modüllerin belirlenimci olarak değerlendirilir. Diğer bir deyişle, değişmeyen bir giriş parametreleri ve veri kümesi göz önünde bulundurulduğunda, modül aynı sonuçları eacRAND veya bir işlevi çalıştırıldığında döndürmelidir. Bu davranışı göz önünde bulundurulduğunda, Azure Machine Learning Studio, bir parametre, belirlenimci olarak işaretlenmiş Modüller yalnızca yeniden çalıştırır veya giriş veri değişti. Önbelleğe alınan sonuçları döndüren denemeleri kadar daha hızlı bir şekilde yürütülmesini sağlar.

RAND veya geçerli bir tarih veya saat döndüren bir işlev gibi belirleyici işlevi vardır. Modülünüzün belirleyici olmayan bir işlev kullanıyorsa, isteğe bağlı olarak ayarlayarak, modülün belirleyici olduğunu belirtebilirsiniz **IsDeterministic** özniteliğini **FALSE**. Deneme çalıştırıldığında, parametreleri ve Giriş modülü değişip değişmediğini bile modülü yeniden çalıştırılır, oluşturmasını sağlar. 

### <a name="language-definition"></a>Dil tanımı
**Dil** öğesi XML tanımı dosyasındaki özel modül dilini belirtmek için kullanılır. Şu an için yalnızca R dili desteklenmektedir. Değerini **Kaynakdosya** özniteliği içeren modül çalıştırılırken çağrılacak işlevin R dosyanın adı olmalıdır. Bu dosya, zip paketini bir parçası olmalıdır. Değerini **entryPoint** öznitelik çağrılan işlevin adını ve kaynak dosyasında tanımlanmış geçerli bir işlev ile eşleşmesi gerekir.

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a>Bağlantı Noktaları
Özel bir modülü için giriş ve çıkış bağlantı noktaları alt öğeleri belirtilen **bağlantı noktaları** XML tanım dosyasının bir bölümünde. Düzen deneyimli (UX) kullanıcılar tarafından bu öğelerin sırasını belirler. İlk alt **giriş** veya **çıkış** listelenen **bağlantı noktaları** öğesi XML dosyasının Machine Learning UX'i en sol giriş bağlantı noktasına olur
Her giriş ve çıkış bağlantı noktasına isteğe sahip **açıklama** Machine Learning kullanıcı arabiriminde bir bağlantı noktası üzerinden fare imlecini geldiğinizde gösterilen metni alt öğesi.

**Bağlantı noktası kuralları**:

* En fazla sayısını **giriş ve çıkış bağlantı noktaları** her biri için 8'dir.

### <a name="input-elements"></a>Giriş öğelerini
Giriş bağlantı noktaları, R işlevi ve çalışma alanı veri iletmek sağlar. **Veri türleri** giriş bağlantı noktaları gibi için desteklenir: 

**DataTable:** Bu tür bir data.frame, R işleve geçirilir. Aslında, makine öğrenimi ve, desteklenen tüm türleri (örneğin, CSV dosyaları veya ARFF'ye dosyaları) ile uyumlu **DataTable** bir data.frame için otomatik olarak dönüştürülür. 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

**Kimliği** her ilişkilendirilmiş özniteliği **DataTable** giriş bağlantı noktası benzersiz bir değere sahip olması gerekir ve bu değer, kendi R işlevinizi parametresinde belirtilen ilişkili eşleşmesi gerekir.
İsteğe bağlı **DataTable** Giriş bir deney olarak geçmedi bağlantı noktalarını değere sahip **NULL** giriş bağlı değilse bağlantı noktaları göz ardı R işlevi ve isteğe bağlı zip geçirildi. **İsteğe bağlıdır** özniteliktir hem isteğe bağlı **DataTable** ve **Zip** türleri ve olduğu *false* varsayılan olarak.

**Zip:** Özel modüller bir zip dosyası, girdi olarak kabul edebilir. Bu giriş, işlevinizin R çalışma dizinine açılmış

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files to be extracted to the R working directory.</Description>
           </Input>

Özel R modülleri için R işlevinin herhangi bir parametre eşleştirilecek Zip bağlantı noktası kimliği yok. Zip dosyası otomatik olarak R çalışma dizinini ayıklanan olmasıdır.

**Giriş kuralları:**

* Değerini **kimliği** özniteliği **giriş** öğe, geçerli bir R değişken adı olmalıdır.
* Değerini **kimliği** özniteliği **giriş** öğesi 64 karakterden uzun olmamalıdır.
* Değerini **adı** özniteliği **giriş** öğesi 64 karakterden uzun olmamalıdır.
* İçeriği **açıklama** öğesi 128 karakterden uzun olmamalıdır
* Değerini **türü** özniteliği **giriş** öğe olmalıdır *Zip* veya *DataTable*.
* Değerini **isteğe bağlıdır** özniteliği **giriş** öğe gerekli değildir (ve *false* belirtilmediğinde varsayılan olarak); ancak belirtilirse, olmalıdır*true* veya *false*.

### <a name="output-elements"></a>Çıkış öğeleri
**Standart çıkış bağlantı noktaları:** Çıkış bağlantı noktaları ardından sonraki modülleri tarafından kullanılan R işlevinizden gelen dönüş değerine eşlenir. *DataTable* yalnızca standart çıkış bağlantı noktası türü şu anda desteklenmiyor. (Desteği *Öğrencileriyle* ve *dönüştüren* gelecek olan.) A *DataTable* çıkış olarak tanımlanır:

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

Özel R modülleri, değeri olarak çıkış **kimliği** R betiğindeki herhangi bir şeyle karşılık gelecek şekilde öznitelik yok, ancak benzersiz olmalıdır. Bir tek modülün çıkış için R işlev dönüş değeri olmalıdır bir *data.frame*. Desteklenen veri türünün birden fazla nesne çıkış için uygun çıkış bağlantı noktaları XML tanım dosyasında belirtilmesi gerekir ve nesnelerin bir listesi olarak döndürülmesi gerekiyor. Çıkış nesnelerini çıkış bağlantı noktasına soldan sağa doğru nesneleri döndürülen listede yerleştirilir sırasını yansıtan atanır.

Örneğin, değiştirmek istediğiniz **özel satır Ekle** özgün iki veri kümesi çıkış modülü *dataset1* ve *dataset2*, yeni birleştirilmiş veri kümesinin yanı sıra *veri kümesi*, (bir sırada, soldan sağa doğru olarak: *veri kümesi*, *dataset1*, *dataset2*), çıkış bağlantı noktaları ardından tanımlayın CustomAddRows.xml dosya gibi:

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


Ve doğru sırada 'CustomAddRows.R' listesinde nesneleri listesini döndürür:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

**Görselleştirme çıktısı:** Türü bir çıkış bağlantı noktasına belirtebilirsiniz *görselleştirme*, R grafik cihazı ve konsol çıkışını çıkışı görüntüler. Bu bağlantı noktası R işlev çıktısının parçası değil ve bir çıkış bağlantı noktası türleri sırasını engellemez. Özel modüller için bir görselleştirme bağlantı noktası eklemek için Ekle bir **çıkış** öğe değerini *görselleştirme* için kendi **türü** özniteliği:

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>

**Çıkış kuralları:**

* Değerini **kimliği** özniteliği **çıkış** öğe, geçerli bir R değişken adı olmalıdır.
* Değerini **kimliği** özniteliği **çıkış** öğesi 32 karakterden uzun olmamalıdır.
* Değerini **adı** özniteliği **çıkış** öğesi 64 karakterden uzun olmamalıdır.
* Değerini **türü** özniteliği **çıkış** öğe olmalıdır *görselleştirme*.

### <a name="arguments"></a>Bağımsız Değişkenler
Ek veri içinde tanımlanan modül parametrelerini aracılığıyla R işleve geçirilebilir **bağımsız değişkenleri** öğesi. Bir modül seçildiğinde, bu parametreleri Machine Learning UI en sağdaki özellikleri bölmesinde görünür. Bağımsız değişkenler desteklenen türlerden biri olabilir veya gerektiğinde özel bir sabit listesi oluşturabilirsiniz. Benzer şekilde **bağlantı noktaları** öğeleri **bağımsız değişkenleri** öğeleri isteğe bağlı olarak olabilir **açıklama** fareyi üzerine geldiğinizde görüntülenen metni belirtir öğesi parametre adı.
DefaultValue minValue ve maxValue gibi bir modül için isteğe bağlı özellikler olarak öznitelikleri için herhangi bir bağımsız değişken eklenebilir bir **özellikleri** öğesi. Geçerli özellikleri **özellikleri** öğesi bağımsız değişken türüne bağlıdır ve sonraki bölümde desteklenen bağımsız değişken türleriyle açıklanmıştır. Bağımsız değişkenlerle **isteğe bağlıdır** özelliğini **"true"** bir değer girmesini gerektirmez. Bağımsız değişkeni için bir değer sağlanmazsa, bağımsız değişken için giriş noktası işlevini geçirilir değil. İsteğe bağlı bağımsız değişkenleri, giriş noktası işlevini açıkça varsayılan değeri NULL giriş noktası işlev tanımında örn: atanan işlev tarafından ele alınması gerekir. İsteğe bağlı bağımsız değişken, yalnızca bir değer kullanıcı tarafından sağlanmışsa diğer bağımsız değişken kısıtlamaları, yani min veya max, zorlar.
Giriş ve çıkışları'te olduğu gibi ile her parametrelerinin ilişkili benzersiz kimliği değerler olması önemlidir. Hızlı Başlangıç örneğimizde ilişkili kimliği/parametre olduğu *takas*.

### <a name="arg-element"></a>Arg öğesi
Bir modül parametresi kullanılarak tanımlanır **Arg** alt öğesi **bağımsız değişkenleri** XML tanım dosyasının bir bölümünde. Alt öğeleri ile **bağlantı noktaları** parametrelerinde sıralama bölümü **bağımsız değişkenleri** bölümü UX'i içinde karşılaştı düzenini tanımlar Parametreleri en üstünden aşağı XML dosyasında tanımlanan aynı sırada kullanıcı Arabiriminde görünür. Parametreler için makine öğrenimi tarafından desteklenen türleri burada listelenir. 

**int** – bir tamsayı (32-bit) tür parametresi.

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* *İsteğe bağlı özellikler*: **min**, **max**, **varsayılan** ve **isteğe bağlıdır**

**çift** – çift tür parametresi.

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* *İsteğe bağlı özellikler*: **min**, **max**, **varsayılan** ve **isteğe bağlıdır**

**bool** – UX'i içinde bir onay kutusu tarafından temsil edilen bir Boole parametresi

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* *İsteğe bağlı özellikler*: **varsayılan** -false ise ayarlı değil

**dize**: standart bir dize

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* *İsteğe bağlı özellikler*: **varsayılan** ve **isteğe bağlıdır**

**ColumnPicker**: bir sütun seçimi parametre. Bu tür bir UX ile bir sütun Seçici işler. **Özelliği** öğesi burada Kimlik sütunları seçileceği, hedef bağlantı noktası türü burada olmalıdır bağlantı noktasının belirtmek için kullanılır *DataTable*. Sütun Seçimi sonucunu seçili sütun adları içeren bir dize listesi olarak R işleve geçirilir. 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *Gerekli özellikler*: **Portıd** -bir giriş öğesinin kimliği türü ile eşleşen *DataTable*.
* *İsteğe bağlı özellikler*:
  
  * **allowedTypes** -sütun türleri olan, devam gelen filtreler. Geçerli değerler şunlardır: 
    
    * Sayısal
    * Boole
    * Kategorik
    * Dize
    * Etiket
    * Özellik
    * Puan
    * Tümü
  * **Varsayılan** -Sütun Seçici için geçerli varsayılan seçimleri içerir: 
    
    * None
    * NumericFeature
    * NumericLabel
    * NumericScore
    * NumericAll
    * BooleanFeature
    * BooleanLabel
    * BooleanScore
    * BooleanAll
    * CategoricalFeature
    * CategoricalLabel
    * CategoricalScore
    * CategoricalAll
    * StringFeature
    * StringLabel
    * StringScore
    * StringAll
    * AllLabel
    * AllFeature
    * AllScore
    * Tümü

**Açılan**: bir kullanıcı tarafından belirtilen Enum (açılır) listesi. Açılan öğe içinde belirtilen **özellikleri** öğesini kullanarak bir **öğesi** öğesi. **Kimliği** her **öğesi** benzersiz olmalıdır ve geçerli bir R değişken. Değerini **adı** , bir **öğesi** gördüğünüz metin hem de R işleve geçirilen değer olarak görev yapar.

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* *İsteğe bağlı özellikler*:
  * **Varsayılan** -varsayılan özelliğinin değeri birinden bir ID değeriyle eşleşmelidir **öğesi** öğeleri.

### <a name="auxiliary-files"></a>Yardımcı dosyalar
Özel Modül ZIP dosyasına yerleştirilmiş herhangi bir dosyayı yürütme sırasında kullanılabilir olacak. Mevcut herhangi bir dizin yapılarını korunur. Bu works kaynağını bu dosyayı aynı yerel olarak ve Azure Machine Learning Studio yürütme anlamına gelir. 

> [!NOTE]
> Tüm yolları böylece tüm dosyalar 'src' dizinine çıkarılır fark ' src /' öneki.
> 
> 

Örneğin, NAs ile herhangi bir satır veri kümesinden ve ayrıca herhangi bir yinelenen satır CustomAddRows çıktısı önce kaldırın. yapmak istediğiniz ve dosyasında RemoveDupNARows.R yapan bir R işlevi zaten yazdığınız varsayalım:

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
Yardımcı dosyada RemoveDupNARows.R CustomAddRows işlevi edinebilir:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
             } else { 
                  dataset <- rbind(dataset1, dataset2)) 
             } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

Ardından, 'CustomAddRows.R', 'CustomAddRows.xml' ve 'RemoveDupNARows.R' özel bir R modülü olarak içeren bir ZIP dosyasını karşıya yükleyin.

## <a name="execution-environment"></a>Yürütme Ortamı
R betik için yürütme ortamı R aynı sürümünü kullandığından **R betiği yürütme** modülü ve aynı varsayılan paketleri kullanabilirsiniz. Ek R paketleri özel modülünüzde Özel Modül ZIP paketteki dahil ederek de ekleyebilirsiniz. Yalnızca kendi R ortamında olduğu gibi bunları R betiğinizde yükleyin. 

**Yürütme Ortamı sınırlamaları** içerir:

* Kalıcı olmayan bir dosya sistemi: Özel Modül çalıştırıldığında yazılmış dosyalar aynı modülde birden fazla çalıştırma sonucunda kalıcı değildir.
* Ağ erişimi yok

