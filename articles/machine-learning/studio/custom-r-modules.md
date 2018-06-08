---
title: Azure Machine learning'de özel R modülleri yazma | Microsoft Docs
description: Azure Machine learning'de özel R modülleri yazma için hızlı başlangıç.
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 6cbc628a-7e60-42ce-9f90-20aaea7ba630
ms.service: machine-learning
ms.component: studio
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/29/2017
ms.openlocfilehash: 555672df5b0b86858d460ff7606bc6ca23f4f103
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834364"
---
# <a name="author-custom-r-modules-in-azure-machine-learning"></a>Azure Machine Learning'de özel R modülleri yazma
Bu konu, yazar ve Azure Machine learning'de özel R modülü dağıtabilirsiniz açıklar. Özel R modülleri nelerdir ve hangi dosyaların bunları tanımlamak için kullanılan açıklanmaktadır. Bir modülün tanımlanması dosyaları oluşturma ve Machine Learning çalışma alanında dağıtım modülü nasıl gösterilmektedir. Özel modülü tanımında kullanılan öznitelikler ve öğeler daha ayrıntılı olarak açıklanmıştır. Yardımcı işlevleri, dosya ve birden çok çıktıları kullanmayı da ele alınmıştır. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="what-is-a-custom-r-module"></a>Özel bir R Modülü nedir?
A **özel Modülü** alanınıza yüklenebilir ve bir Azure Machine Learning deneme bir parçası olarak çalıştırılan bir kullanıcı tarafından tanımlanan modüldür. A **özel R Modülü** kullanıcı tanımlı bir R işlev yürüten özel bir modüldür. **R** istatistiksel bilgi işlem ve istatistikçiler ve veri bilimcilerine tarafından algoritmaları uygulamak için yaygın olarak kullanılan grafik için bir programlama dilidir. Şu anda R özel modüller, ancak destek ek dilleri zamanlandığı yönelik için gelecek sürümlerde desteklenen tek dilidir.

Özel modüller sahip **birinci sınıf durum** Azure Machine learning'de diğer modülü gibi kullanılabilir olduğunu herkese açık. Yayımlanan denemeler veya görselleştirmeleri dahil, diğer modüllerle çalıştırılabilir. Modül, giriş ve kullanılacak çıkış bağlantı noktaları, modelleme parametreleri ve diğer çeşitli çalışma zamanı davranışları tarafından uygulanan algoritması üzerinde denetiminiz yoktur. Özel modüller içeren bir denemeyi kolay paylaşım Azure AI Galerisi içine de yayımlanabilir.

## <a name="files-in-a-custom-r-module"></a>Özel bir R Modülü dosyaları
Özel bir R modülü, en az iki dosyalarını içeren bir .zip dosyası tanımlanır:

* A **kaynak dosyası** modülü tarafından kullanıma sunulan R işlevi uygular
* Bir **XML tanım dosyasını** özel modülü arabirimi açıklar

Ek yardımcı dosyalar da özel modülünden erişilebilir işlevselliği sağlayan .zip dosya eklenebilir. Bu seçenek içinde ele alınmıştır **bağımsız değişkenleri** başvuru bölümünde parçası **XML tanım dosyasını öğelerinde** hızlı başlangıç örnek aşağıdaki.

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a>Hızlı Başlangıç örnek: tanımlamak, paket ve özel bir R modülü kaydetme
Bu örnek özel bir R modülü tarafından gerekli dosyaları oluşturmak, bir zip dosyasına paketini ve ardından modülü Machine Learning çalışma alanınızda kaydetmek nasıl gösterilmektedir. Örnek zip paketini ve örnek dosyaları yüklenebilir [CustomAddRows.zip karşıdan dosya](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).

## <a name="the-source-file"></a>Kaynak dosya
Örneği göz önünde bulundurun bir **özel Add Rows** standart uygulaması değiştirir Modülü **Add Rows** satırları (gözlemleri) iki veri kümesi (veri çerçevelerini) birleştirmek için kullanılan modül. Standart **Add Rows** modül kullanarak ilk girdi veri kümesi sonuna ikinci girdi veri kümesi satırlarını ekler `rbind` algoritması. Özelleştirilmiş `CustomAddRows` işlevi benzer şekilde iki veri kümesi kabul eder, ancak ayrıca bir Boolean takas parametresi ek bir girdi olarak kabul eder. Takas parametresi ayarlanmışsa **yanlış**, aynı veri kümesi standart uygulaması olarak döndürür. Ancak takas parametresi ise **doğru**, bunun yerine ikinci veri kümesi sonuna işlevi ilk girdi veri kümesi satırlarını ekler. R uyarlamasını içeren CustomAddRows.R dosyası `CustomAddRows` işlevi kullanıma sunulan **özel Add Rows** Modülü aşağıdaki R kodu içeriyor.

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

### <a name="the-xml-definition-file"></a>XML tanım dosyasını
Bu kullanıma sunmak için `CustomAddRows` işlevi bir Azure Machine Learning modülü, bir XML tanım dosyası olarak oluşturulan, belirtmek için nasıl **özel Add Rows** modülü görünüş ve davranışı aynıdır. 

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


Unutmayın önemlidir değerini **kimliği** özniteliklerini **giriş** ve **Arg** XML dosyasındaki öğeleri R kodu işlevi parametre adları eşleşmelidir CustomAddRows.R dosya tam: (*dataset1*, *dataset2*, ve *takas* örnekte). Benzer şekilde, değeri **entryPoint** özniteliği **dil** öğesi eşleşmelidir R betiği işlevinde adı: (*CustomAddRows* örnekte) . 

Buna karşılık, **kimliği** için öznitelik **çıkış** öğesi R betiği herhangi değişkenlerine karşılık gelmiyor. Birden fazla çıkış gerekli olduğunda, bir liste yerleştirilen sonuçlarıyla R işlevinden yalnızca dönmek *aynı sırada* olarak **çıkışları** öğeleri XML dosyasında bildirilen.

### <a name="package-and-register-the-module"></a>Paket ve kaydetme modülü
Bu iki dosyaları olarak kaydetmeniz *CustomAddRows.R* ve *CustomAddRows.xml* ve ardından iki birlikte içine ZIP dosyaları bir *CustomAddRows.zip* dosya.

Machine Learning çalışma alanınızda kaydetmek için Machine Learning Studio'da çalışma alanınıza gidin, **+ yeni** alta düğmesini tıklatın ve seçin **MODÜL ZIP PAKETİNİ gelen ->** yeni karşıya yüklemek için **Özel Add Rows** modülü.

![Zip karşıya yükle](./media/custom-r-modules/upload-from-zip-package.png)

**Özel Add Rows** modülüdür şimdi, Machine Learning denemelerini tarafından erişilebilmesi hazır.

## <a name="elements-in-the-xml-definition-file"></a>XML tanım dosyasını öğeleri
### <a name="module-elements"></a>Modül öğeleri
**Modülü** öğe XML dosyasında özel bir modülü tanımlamak için kullanılır. Birden fazla modülü birden çok kullanarak bir XML dosyasında tanımlanabilir **Modülü** öğeleri. Her modül çalışma alanınızda benzersiz bir ad olmalıdır. Var olan bir özel modül aynı ada sahip özel bir modülü kaydetmek ve var olan bir modül yeni bir bağlantıyla değiştirir. Özel modüller ancak olabilir var olan bir Azure Machine Learning modül adıyla aynı kayıtlı. Bu nedenle, bunlar görünüyorsa **özel** modül paleti kategorisi.

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another...</Description>/> 


İçinde **Modülü** öğesi, iki ek isteğe bağlı öğeleri belirtebilirsiniz:

* bir **sahibi** modüle katıştırılmış öğesi  
* bir **açıklama** modülü için hızlı Yardımı'nda görüntülenir ve makine öğrenme UI modülünde üzerine getirdiğinizde metni içeren öğe.

Kuralları modülü öğelerindeki karakter sınırları için:

* Değeri **adı** özniteliğini **Modülü** öğesi uzunluğu 64 karakteri aşmamalıdır. 
* İçeriği **açıklama** öğesi 128 karakterden uzun olamaz.
* İçeriği **sahibi** öğesi 32 karakterden uzun olamaz.

Bir modülün sonuçları belirleyici olabilir veya nondeterministic.* * varsayılan olarak tüm modüllerin belirleyici olduğu kabul edilir. Diğer bir deyişle, değişmeyen dizi giriş parametreleri ve verileri verildiğinde, modül aynı sonuçları eacRAND veya bir functionh çalıştırıldığında döndürmelidir. Bu davranış verildiğinde, Azure Machine Learning Studio, bir parametre değilse belirleyici olarak işaretlenmiş Modüller yalnızca yeniden çalıştırır veya giriş verileri değiştirildi. Önbelleğe alınan sonuçları döndüren çok daha hızlı denemeler yürütülmesini sağlar.

RAND veya geçerli tarih ve saati döndüren bir işlev gibi belirleyici işlevleri vardır. Modülünüzün belirleyici olmayan bir işlev kullanıyorsa, isteğe bağlı ayarlayarak modülü belirleyici olduğunu belirtebilirsiniz **IsDeterministic** özniteliğini **FALSE**. Bu denemeyi çalıştırın her giriş modülü ve parametreleri değişip değişmediğini olsa bile modülü yeniden çalıştırılır, yöntem başlanmasını sağlar. 

### <a name="language-definition"></a>Dil tanımı
**Dil** XML tanım dosyasını öğesinde özel modülü dili belirtmek için kullanılır. Şu anda R yalnızca desteklenen dilidir. Değeri **sourceFile** özniteliği modülü çalıştırdığınızda çağrılacak işlevi içeren R dosya adı olmalıdır. Bu dosya zip paketinin bir parçası olması gerekir. Değeri **entryPoint** özniteliği çağrılan işlev adıdır ve kaynak dosyasında tanımlanmış geçerli bir işlev eşleşmesi gerekir.

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a>Bağlantı Noktaları
Özel bir modülü için giriş ve çıkış bağlantı noktalarını alt öğelerinin içinde belirtilen **bağlantı noktalarını** XML tanım dosyasını bölümü. Bu öğelerin sırasını düzeni deneyimli (UX) kullanıcılar tarafından belirler. İlk alt **giriş** veya **çıkış** listelenen **bağlantı noktalarını** XML dosyasının öğe haline gelir, Machine Learning UX en sol giriş bağlantı noktası
Her giriş ve çıkış bağlantı noktasına isteğe sahip **açıklama** Machine Learning arabiriminde bağlantı noktası üzerinden fare imleci getirdiğinizde gösterilen metni belirtir alt öğesi.

**Bağlantı noktası kuralları**:

* En fazla **giriş ve çıkış bağlantı noktaları** her biri için 8'dir.

### <a name="input-elements"></a>Giriş öğeleri
Giriş bağlantı noktaları, R işlevi ve çalışma alanı için veri iletmek izin verir. **Veri türleri** giriş bağlantı noktaları gibi için desteklenir: 

**DataTable:** bu tür R işlevinizde bir data.frame olarak geçirilir. Aslında, Machine Learning ve, tarafından desteklenen tüm türleri (örneğin, CSV dosyalarını veya ARFF dosyaları) ile uyumlu **DataTable** data.frame için otomatik olarak dönüştürülür. 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

**Kimliği** her ilişkilendirilmiş özniteliği **DataTable** giriş bağlantı noktası benzersiz bir değer olmalıdır ve bu değer, R işlevi parametresinde belirtilen ilgili eşleşmesi gerekir.
İsteğe bağlı **DataTable** Giriş bir deney olarak geçmedi bağlantı noktalarının olması ve değerin **NULL** giriş bağlı değilse bağlantı noktaları göz ardı R işlev ve isteğe bağlı zip geçirildi. **İsteğe bağlıdır** özniteliktir her ikisi için isteğe bağlı **DataTable** ve **Zip** türleri ve olduğu *false* varsayılan olarak.

**Zip:** özel modüller bir zip dosyası giriş olarak kabul edebilir. Bu giriş işlevinizi R çalışma dizinine açılmış

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files to be extracted to the R working directory.</Description>
           </Input>

Özel R modülleri için herhangi bir parametre R işlevinin eşleşecek şekilde Zip bağlantı noktası kimliği yok. Zip dosyası otomatik olarak R çalışma dizinini ayıklanan olmasıdır.

**Giriş kuralları:**

* Değeri **kimliği** özniteliği **giriş** öğesi geçerli bir R değişken adı olması gerekir.
* Değeri **kimliği** özniteliği **giriş** öğesi 64 karakterden uzun olmamalıdır.
* Değeri **adı** özniteliği **giriş** öğesi 64 karakterden uzun olmamalıdır.
* İçeriği **açıklama** öğesi 128 karakterden uzun olmamalıdır
* Değeri **türü** özniteliği **giriş** öğesi olmalıdır *Zip* veya *DataTable*.
* Değeri **isteğe bağlıdır** özniteliği **giriş** öğesi gerekli değildir (ve *false* belirtilmemişse varsayılan olarak); ancak belirtilirse, olmalıdır*true* veya *false*.

### <a name="output-elements"></a>Çıktı öğeleri
**Standart çıktı bağlantı noktaları:** çıkış bağlantı noktaları ardından sonraki modülleri tarafından kullanılan, R işlevinin dönüş değerleri eşlendi. *DataTable* yalnızca standart çıktı bağlantı noktası türü şu anda desteklenmiyor. (Desteği *Öğrencileriyle* ve *dönüştüren* gelecek olan.) A *DataTable* çıktı olarak tanımlanır:

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

Özel R modülleri, değerini çıktılarında için **kimliği** özniteliği her şeyi R betiği ile karşılık gelen gerekmez, ancak benzersiz olmalıdır. Tek modülü çıktı için R işlevden dönüş değeri olmalıdır bir *data.frame*. Desteklenen veri türünde birden fazla nesne çıktı için uygun çıkış bağlantı noktaları XML tanım dosyasında belirtilmesi gerekir ve bir liste olarak döndürülecek nesneleri gerekir. Çıkış nesnelerini çıkış bağlantı noktasına soldan sağa nesneleri döndürülen listede yerleştirilir sipariş yansıtma atanmış.

Örneğin, değişiklik yapmak istiyorsanız **özel satırları ekleyin** özgün iki veri kümesi çıkış modülü *dataset1* ve *dataset2*, yeni birleştirilmiş veri kümesi yanı sıra *dataset*, (soldan sağa, bir sırada olarak: *dataset*, *dataset1*, *dataset2*), çıkış bağlantı noktaları tanımlayın CustomAddRows.xml dosya şu şekilde:

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


Ve doğru sırada 'CustomAddRows.R' listesinde nesnelerinin listesini döndürür:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

**Görselleştirme çıktı:** türü bir çıkış bağlantı noktasına da belirtebilirsiniz *görselleştirme*, R grafik cihaz ve konsol çıkışını bir çıkış görüntüler. Bu bağlantı noktası R işlevi çıktı parçası olmayan ve bir çıkış bağlantı noktası türleri sırasını etkilemez. Özel modüllerle görselleştirme bağlantı noktası eklemek için Ekle bir **çıkış** değerini bir öğesiyle *görselleştirme* için kendi **türü** özniteliği:

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>

**Çıkış kuralları:**

* Değeri **kimliği** özniteliği **çıkış** öğesi geçerli bir R değişken adı olması gerekir.
* Değeri **kimliği** özniteliği **çıkış** öğesi 32 karakterden uzun olmamalıdır.
* Değeri **adı** özniteliği **çıkış** öğesi 64 karakterden uzun olmamalıdır.
* Değeri **türü** özniteliği **çıkış** öğesi olmalıdır *görselleştirme*.

### <a name="arguments"></a>Bağımsız Değişkenler
Ek veri içinde tanımlanan modülü parametreleri aracılığıyla R işleve geçirilebilir **bağımsız değişkenleri** öğesi. Modül seçildiğinde bu parametreler Machine Learning UI en sağdaki Özellikler bölmesinde görüntülenir. Bağımsız değişkenler desteklenen türlerinden herhangi birinde olabilir veya gerekli olduğunda özel bir numaralandırma oluşturabilirsiniz. Benzer şekilde **bağlantı noktaları** öğeleri **bağımsız değişkenler** öğeleri isteğe bağlı bir olabilir **açıklama** fare üzerine geldiğinizde görüntülenen metni belirtir öğesi parametre adı.
DefaultValue, minValue ve maxValue gibi bir modül için isteğe bağlı özellikler öznitelikler için olarak için herhangi bir bağımsız değişken eklenebilir bir **özellikleri** öğesi. Geçerli özelliklerini **özellikleri** öğesi bağımsız değişken türüne bağlıdır ve sonraki bölümde desteklenen bağımsız değişken türleriyle açıklanmıştır. Bağımsız değişkenlerle **isteğe bağlıdır** özelliğini **"true"** bir değer girmesini gerektirmez. Bir değer bağımsız değişkeni sağlanmazsa, bağımsız değişkeni için giriş noktası işlevi aktarılmaz. İsteğe bağlı bağımsız değişkenler giriş noktası işlevinin açıkça örneğin varsayılan değeri NULL giriş noktası işlevi tanımında atanan işlevi tarafından ele alınması gerekir. Kullanıcı tarafından sağlanan bir değeri, isteğe bağlı bir bağımsız değişken yalnızca diğer bağımsız değişkeni kısıtlamaları, yani min veya Mak uygulayın.
Girişleri ve çıkışları'te olduğu gibi her bir parametreyi kendileriyle ilişkilendirilmiş benzersiz kimliği değerlere sahip önemlidir. Hızlı Başlangıç örneğimizde ilişkili kimliği/parametre olan *takas*.

### <a name="arg-element"></a>Arg öğesi
Bir modül parametresi kullanılarak tanımlanan **Arg** alt öğesi olan **bağımsız değişkenleri** XML tanım dosyasını bölümü. Bir alt öğe ile **bağlantı noktalarını** bölümünde parametrelerinde sıralama **bağımsız değişkenleri** bölümü UX karşılaştı düzeni tanımlar Parametreleri üstten aşağı XML dosyasında tanımlanan aynı sırada kullanıcı Arabiriminde görüntülenir. Parametreler için makine öğrenme tarafından desteklenen türleri burada listelenir. 

**int** – bir tamsayı (32 bit) tür parametresi.

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

**bool** – UX kutusunda onay tarafından temsil edilen bir Boolean parametresiyle

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* *İsteğe bağlı özellikler*: **varsayılan** -false ise ayarlanmadı

**dize**: standart bir dize

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* *İsteğe bağlı özellikler*: **varsayılan** ve **isteğe bağlıdır**

**ColumnPicker**: sütun seçimi parametresi. Bu tür UX Sütun Seçici işler. **Özelliği** öğesi burada içinden sütun seçildi, hedef bağlantı noktası türü burada olmalıdır bağlantı noktasının kimliğini belirtmek için kullanılır *DataTable*. Sütun Seçimi sonucunu R işlevi seçili sütun adlarını içeren bir dize listesi olarak geçirilir. 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *Özellikler gerekli*: **portId** -giriş öğenin kimliği türü ile eşleşen *DataTable*.
* *İsteğe bağlı özellikler*:
  
  * **allowedTypes** -sütun türleri hangi, seçebilirsiniz gelen filtreleri. Geçerli değerler şunlardır: 
    
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

**Aşağı açılan**: bir kullanıcı tarafından belirtilen Enum (açılan) listesi. Açılır öğe içinde belirtilen **özellikleri** öğesini kullanarak bir **öğesi** öğesi. **Kimliği** her **öğesi** benzersiz olmalıdır ve geçerli bir R değişken. Değeri **adı** , bir **öğesi** gördüğünüz metin ve R işlevine geçirilen değer olarak görev yapar.

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* *İsteğe bağlı özellikler*:
  * **Varsayılan** -varsayılan özelliği için değer birinden bir ID değeriyle eşleşmelidir **öğesi** öğeleri.

### <a name="auxiliary-files"></a>Yardımcı dosyalar
Özel Modül ZIP dosyasında yerleştirilen herhangi bir dosyayı yürütme sırasında kullanılabilir olacak. Mevcut herhangi bir dizin yapıları korunur. Bu works kaynak dosyayı aynı yerel olarak ve Azure Machine Learning yürütme anlamına gelir. 

> [!NOTE]
> Tüm yola sahip olmanız gerekir böylece tüm dosyalar 'src' dizinine ayıklanır fark ' src /' öneki.
> 
> 

Örneğin, NAs ile herhangi bir satır kümesinden kaldırmak ve herhangi bir yinelenen satır CustomAddRows çıktısı önce de kaldırmak istiyor ve bir dosyada RemoveDupNARows.R yapan bir R işlevi zaten yazdıktan söyleyin:

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
CustomAddRows işlevi yardımcı dosyasında RemoveDupNARows.R kaynak:

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

Ardından, 'CustomAddRows.R', 'CustomAddRows.xml' ve 'RemoveDupNARows.R' özel bir R modülü olarak içeren bir zip dosyası karşıya yükleyin.

## <a name="execution-environment"></a>Yürütme Ortamı
R betiği yürütme ortamı R aynı sürümünü kullandığından **R betiği yürütün** modülü ve aynı varsayılan paketleri kullanabilirsiniz. Özel Modül zip pakete dahil ederek özel modülünüzün ek R paketleri de ekleyebilirsiniz. Yalnızca kendi R ortamında gibi bunları R komut dosyanıza yükleyin. 

**Yürütme Ortamı sınırlamaları** içerir:

* Kalıcı olmayan dosya sistemi: özel modülü çalıştırdığınızda yazılmış dosyaları aynı modülü birden çok çalıştırmaları arasında sürdürülmez.
* Ağ erişim yok

