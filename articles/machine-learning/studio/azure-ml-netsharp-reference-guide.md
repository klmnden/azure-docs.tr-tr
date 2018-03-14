---
title: "NET # sinir ağları belirtimi dili Kılavuzu - Azure Machine Learning | Microsoft Docs"
description: "Net # kullanarak bir özel sinir ağı model oluşturmak nasıl örnekleri ile birlikte belirtimi dili sözdizimi için Net # sinir ağları"
services: machine-learning
documentationcenter: 
author: jeannt
manager: cgronlun
editor: 
ms.assetid: cfd1454b-47df-4745-b064-ce5f9b3be303
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 03/01/2018
ms.author: jeannt
ms.openlocfilehash: a166b45e7e482092006ddad276986b6f8b0f378c
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2018
---
# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Azure Machine Learning için NET # sinir ağı belirtimi dili için kılavuz

NET # sinir ağı mimarileri tanımlamak için kullanılan Microsoft tarafından geliştirilen bir dildir. NET # sinir ağı yapısını tanımlamak için kullanarak, derin sinir ağları veya görüntü, ses veya video gibi verileri öğrenmeyi artırmak için bilinen rasgele boyutlarının convolutions gibi karmaşık yapıları tanımlamanızı mümkün kılar.

Bu bağlamlarda Net # mimarisi belirtimi kullanabilirsiniz:

+ Microsoft Azure Machine Learning Studio'da tüm sinir ağı modülleri: [veya çoklu sınıflar sinir ağı](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/multiclass-neural-network), [iki sınıflı sinir ağı](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/two-class-neural-network), ve [sinir ağı regresyon](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/neural-network-regression)
+ Sinir ağı işlevlerde MicrosoftML: [NeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/neuralnet) ve [rxNeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxneuralnet)R dil ve [rx_neural_network](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-neural-network) Python için.


Bu makalede, Net # kullanarak özel bir sinir ağı geliştirmek için gerekli sözdizimi ve temel kavramlar açıklanmaktadır: 

+ Sinir ağı gereksinimleri ve birincil bileşenleri tanımlama
+ Net # belirtimi dili anahtar sözcüklerini ve sözdizimi
+ Net # kullanılarak oluşturulan özel sinir ağları örnekleri 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="neural-network-basics"></a>Sinir ağı temelleri

Sinir ağı yapısı katmanları ve ağırlıklı bağlantıları (veya kenarlar) düzenlenir düğümleri düğümler arasında oluşur. Bağlantılar tek yönlü ve her bağlantı kaynak düğüm ve hedef düğüm vardır.  

Bir veya daha fazla trainable her katman (gizli veya bir çıkış katmanı) sahip **bağlantı paketleri**. Bir bağlantı paket kaynak katmanı ve kaynak katman bağlantılarından belirtimini oluşur. Belirli bir paketteki tüm bağlantıları aynı kaynak katman ve aynı hedef katman paylaşır. NET #'ta bağlantı paket paketin hedef katmana ait olarak kabul edilir.

NET # çeşitli şekilde girişleri özelleştirmenizi sağlar paketleri gizli katmanlara eşlenen ve çıktıları eşlenen bağlantısı destekler.

Varsayılan veya standart paket bir **tam paket**, hedef katmanı kümedeki her düğüm için kaynak katmandaki her düğüme bağlı olarak.

Ayrıca, Gelişmiş bağlantı paketleri aşağıdaki dört tür Net # destekler:

+ **Filtre paketleri**. Kullanıcı, kaynak katman ve hedef katman düğümlerini konumlarını kullanarak bir koşul tanımlayabilirsiniz. Koşul True olduğunda düğümleri bağlanır.

+ **Convolutional paketleri**. Kullanıcı düğümlerinin küçük Semt kaynak katmanda tanımlayabilirsiniz. Hedef katmanı içindeki her bir düğümün bir Komşuları kaynak katmandaki düğümlerinin bağlı.

+ **Paket havuzu** ve **yanıt normalleştirme paketleri**. Kullanıcının kaynak katmanda düğümlerinin küçük Semt tanımlar, bunlar convolutional paketleri benzerdir. Bu paketleri kenarları ağırlıkları trainable olmayan farktır. Bunun yerine, önceden tanımlanmış bir işlev hedef düğüm değeri belirlemek için kaynak düğüm değerleri uygulanır.


## <a name="supported-customizations"></a>Desteklenen özelleştirme

Azure Machine Learning ile oluşturduğunuz sinir ağı modelleri mimarisini Net # kullanarak kapsamlı bir şekilde özelleştirilebilir. Şunları yapabilirsiniz:

+ Gizli katmanları oluşturun ve her katmandaki düğüm sayısını denetler.
+ Nasıl birbirine bağlı olması katmanlardır belirtin.
+ Convolutions ve paket paylaşımı ağırlık gibi özel bağlantı yapılarını tanımlar.
+ Başka bir etkinleştirme işlevleri belirtin.

Belirtimi dili sözdizimi Ayrıntılar için bkz [yapısı belirtimi](#Structure-specifications).  

Karmaşık için tek yönlü görevlerden öğrenme bazı ortak makine sinir ağları tanımlayan örnekler için bkz: [örnekleri](#Examples-of-Net#-usage).

## <a name="general-requirements"></a>Genel gereksinimler

+ Tam olarak bir çıkış katman, en az bir giriş katman ve sıfır veya daha çok gizli katmanları olmalıdır. 
+ Her katman rasgele boyutları dikdörtgen bir dizi kavramsal olarak düzenlenmiş düğümler, sabit bir sayısına sahip. 
+ Giriş Katmanlar ilişkili eğitilen parametresi olmayan ve burada örnek verilerini ağ girer noktasını temsil eder. 
+ Trainable Katmanlar (gizli ve çıkış katmanları) ağırlıkları ve stratejimizdeki bilinen eğitilen parametreler ilişkilendirdiniz. 
+ Kaynak ve hedef düğümleri ayrı katmanda olmalıdır. 
+ Bağlantıları Çevrimsiz olmalıdır; diğer bir deyişle, ilk kaynak düğüme geri önde gelen bağlantılar zinciri olamaz.
+ Çıktı katman bir bağlantı paketin kaynak katmanı olamaz.  

## <a name="structure-specifications"></a>Yapı belirtimleri

Sinir ağı yapısı belirtimi üç bölümlerini oluşur: **Sabit bildiriminde**, **katman bildirimi**, **bağlantı bildirimi**. Ayrıca vardır isteğe bağlı bir **paylaşmak bildirimi** bölümü. Bölümler herhangi bir sırada belirtilebilir.

## <a name="constant-declaration"></a>Sabit bildirimi

Sabit bildiriminde isteğe bağlıdır. Sinir ağı tanımı'nda başka bir yerde kullanılan değerleri tanımlamak için bir yol sağlar. Ardından bir eşittir işareti ve değer ifadesi bir tanımlayıcı, bildirim deyiminin oluşur.

Örneğin, bir sabit aşağıdaki ifadeyi tanımlar `x`:  

`Const X = 28;`

İki veya daha fazla sabitleri eşzamanlı olarak tanımlamak için tanımlayıcı adları ve değerleri ayraç içine alın ve bunları noktalı virgülle ayırın. Örneğin:

`Const { X = 28; Y = 4; }`

Her atama ifadesi sağ tarafında, tamsayı, gerçek bir sayı, bir Boole değeri (True veya False) veya bir matematik ifadesindeki olabilir. Örneğin:

`Const { X = 17 * 2; Y = true; }`

## <a name="layer-declaration"></a>Katman bildirimi

Katman bildirimi gereklidir. Boyut ve kaynak öznitelikleri ve bağlantı paketleri de dahil olmak üzere bu katmanın tanımlar. Bildirim deyiminin (giriş, gizli veya çıktı) katman adı ile başlar (pozitif tamsayılar tuple) katman boyutları tarafından izlenen. Örneğin:

```Net#
input Data auto;
hidden Hidden[5,20] from Data all;
output Result[2] from Hidden all;  
```

+ Katmandaki düğüm sayısını boyutları ürünüdür. Bu örnekte, katmanda 100 düğümleri olduğu anlamına gelir iki boyut [5,20] vardır.
+ Bir istisna dışında herhangi bir sırada katmanları bildirilebilir: birden fazla giriş katman tanımlanmış olması durumunda, bunlar bildirilen giriş verilerini özelliklerinde sırasını eşleşmesi gerekir.

Katmandaki düğüm sayısını otomatik olarak belirlenmesi belirtmek için kullanın `auto` anahtar sözcüğü. `auto` Anahtar sözcüğü katman bağlı olarak farklı efektler vardır:

+ Bir giriş katman bildiriminde düğüm sayısını giriş verilerini özelliklerinde sayısıdır.
+ Gizli Katman bildiriminde düğümleri sayısı için parametre değeri tarafından belirtilen sayıdır **gizli düğüm sayısını**.
+ Bir çıkış katman bildiriminde düğüm sayısını iki sınıflı Sınıflandırma, regresyon ve çok sınıflı sınıflandırma için çıktı düğüm sayısına eşittir 1 için 2'dir.

Örneğin, aşağıdaki ağ tanımını otomatik olarak belirlenmesi için tüm katmanlar boyutunu sağlar:

```Net#
input Data auto;
hidden Hidden auto from Data all;
output Result auto from Hidden all;  
```

Bir katman bildirimi trainable katman (gizli veya çıkış katmanları) için isteğe bağlı olarak varsayılan olarak (bir etkinleştirme işlevi olarak da bilinir) çıktı işlevini içerebilir **sigmoid** sınıflandırma modelleri için ve  **Doğrusal** regresyon modelleri için. Varsayılan kullansanız bile, açıkça etkinleştirme işlevi, daha anlaşılır olması için isterseniz durumu.

Aşağıdaki çıkış işlevleri desteklenir:

+ sigmoid
+ Doğrusal
+ softmax
+ rlinear
+ Kare
+ sqrt
+ srlinear
+ Abs
+ TANH
+ brlinear

Örneğin, aşağıdaki bildirimini kullanır **softmax** işlevi:

`output Result [100] softmax from Hidden all;`

## <a name="connection-declaration"></a>Bağlantı bildirimi

Hemen trainable katman tanımladıktan sonra tanımladığınız katmanlar arasında bağlantı bildirmeniz gerekir. Bağlantı paket bildirimi anahtar sözcüğüyle başlar `from`, paketin kaynak katman ve bağlantı paket oluşturmak için tür adının ardından.

Şu anda beş bağlantı paket türleri desteklenir:

+ **Tam** anahtar sözcüğüyle belirtilen paketleri `all`
+ **Filtre** anahtar sözcüğüyle belirtilen paket, `where`takip eden bir koşul ifadesi
+ **Convolutional** anahtar sözcüğüyle belirtilen paket, `convolve`, ardından evrişim öznitelikleri
+ **Havuzu** anahtar sözcükleri belirtilen paket, **en büyük havuz** veya **havuzu anlama**
+ **Yanıt normalleştirme** anahtar sözcüğüyle belirtilen paket, **yanıt norm**

## <a name="full-bundles"></a>Tam paketleri

Bir tam bağlantı paket Hedef katmanı her düğüme kaynak katmanda her düğümden bir bağlantı içerir. Varsayılan ağ bağlantısı türü budur.

## <a name="filtered-bundles"></a>Filtrelenmiş paketler

Filtrelenmiş bağlantıda paket belirtimi, C# lambda ifadesi gibi koşul, ifade edilen sözdizimsel olarak, çok içerir. Aşağıdaki örnek, iki filtrelenmiş paketler tanımlar:

```Net#
input Pixels [10, 20];
hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  
```

+ İçin koşulunda `ByRow`, `s` dizin giriş katman düğümlerinin dikdörtgen diziye temsil eden bir parametre `Pixels`, ve `d` dizin gizli katmanın düğümlerinin diziye temsil eden bir parametre `ByRow`. Her ikisi de türünü `s` ve `d` iki uzunluğu tamsayıların bir tanımlama grubu değil. Kavramsal olarak, `s` aralıkları ile tamsayıların tüm çiftleri üzerinden `0 <= s[0] < 10` ve `0 <= s[1] < 20`, ve `d` ile tamsayılar, tüm çiftlerini aralıkları `0 <= d[0] < 10` ve `0 <= d[1] < 12`. 

+ Koşul ifadesi sağ tarafta bir koşulu yoktur. Bu örnekte, her değeri için `s` ve `d` koşul True olacak şekilde, kaynak katman düğümünden kenar hedef katman düğümüne yoktur. Bu nedenle, bu filtre ifadesi paket bağlantı tarafından tanımlanan düğümünden içerdiğini gösterir `s` tarafından tanımlanan düğüme `d` burada s [0], [0] d eşit tüm durumlarda.

İsteğe bağlı olarak, ağırlıkları filtre uygulanmış bir paket için bir dizi belirtebilirsiniz. Değeri **ağırlıkları** özniteliği, kayan nokta değerlerine paket tarafından tanımlanan bağlantı sayısı ile eşleşen bir uzunluğa sahip bir tanımlama grubu olmalıdır. Varsayılan olarak, ağırlıkları rastgele üretilir.

Ağırlık değerleri hedef düğüm dizine göre gruplandırılır. Diğer bir deyişle, ilk hedef düğüm K kaynak düğümlerine bağlıysa ilk `K` öğeleri **ağırlıkları** tanımlama grubu olan ilk hedef düğüm, kaynak dizin sırasını ağırlıkları. Aynı kalan hedef düğümleri için geçerlidir.

Ağırlıkları doğrudan sabit değer olarak belirtmek mümkündür. Örneğin, ağırlıkları önceden öğrenilen varsa, bunları bu söz dizimini kullanarak sabitleri belirtebilirsiniz:

`const Weights_1 = [0.0188045055, 0.130500451, ...]`

## <a name="convolutional-bundles"></a>Convolutional paketleri

Eğitim verileri homojen bir yapıya sahip olduğunda, convolutional bağlantıları verileri üst düzey özelliklerini öğrenmek için yaygın olarak kullanılır. Örneğin, ses veya video veriler, uzamsal veya zamana bağlı boyut görüntüde oldukça Tekdüzen olabilir.  

Convolutional paketleri uygulamadığınız dikdörtgen **tekrar** boyutları aracılığıyla slid. Esas olarak, her çekirdek yerel Semt içinde uygulanan ağırlıkları olarak adlandırılan bir dizi tanımlar **çekirdek uygulamaları**. Bir düğüm olarak adlandırılır kaynak katmanda her çekirdek uygulama karşılık **merkezi düğümü**. Bir çekirdek ağırlıkları birçok bağlantıları arasında paylaşılır. Bir convolutional paketteki her dikdörtgen çekirdeğidir ve tüm çekirdek uygulamalar aynı boyuttadır.  

Convolutional paketleri aşağıdaki öznitelikleri destekler:

**InputShape** convolutional bu paket amaçlarla kaynak katman boyut tanımlar. Değer pozitif tamsayılar tanımlama grubu olmalıdır. Tamsayıların ürün kaynak katmandaki düğüm sayısını eşit olmalıdır, ancak Aksi halde, kaynak katmanı için bildirilen boyut özelliğiyle eşleşen gerekmez. Bu kayıt uzunluğu hale **parametre** convolutional paket için değer. Genellikle parametre sayıda bağımsız değişken veya işlev sürebilir işlenenler anlamına gelir.

Şekil ve tekrar konumları tanımlamak için öznitelikleri kullanma **KernelShape**, **STRIDE**, **doldurma**, **LowerPad**ve  **UpperPad**:   

+ **KernelShape**: her çekirdek convolutional paket için boyut (gerekli) tanımlar. Değer pozitif tamsayılar paket kapsamalıdır eşittir bir uzunluğa sahip bir tanımlama grubu olmalıdır. Bu tanımlama grubu alanının her bileşeni ilgili bileşen büyük olmaması **InputShape**. 

+ **STRIDE**: (isteğe bağlı) merkezi düğümü arasında uzaklık Evrişim (tek bir adımda boyutu) her boyut için kayan adım boyutları tanımlar. Değer pozitif tamsayılar paket kapsamalıdır olan uzunluğa sahip bir tanımlama grubu olmalıdır. Bu tanımlama grubu alanının her bileşeni ilgili bileşen büyük olmaması **KernelShape**. Tüm bileşenlerle birine eşit bir tanımlama grubu varsayılan değerdir. 

+ **Paylaşımı**: (isteğe bağlı) evrişim her boyut için paylaşımı ağırlık tanımlar. Değer, tek bir Boole değeri veya Boolean değeri paket kapsamalıdır olan uzunluğa sahip bir tanımlama grubu olabilir. Tek bir Boole değeri belirtilen değere eşit tüm bileşenlerle doğru uzunlukta bir tanımlama grubu olarak genişletilir. Tüm gerçek değerlerin oluşan bir dizi varsayılan değerdir. 

+ **MapCount**: özellik sayısı eşler için convolutional paket (isteğe bağlı) tanımlar. Değer, tek bir pozitif tamsayı veya bir tanımlama grubu pozitif tamsayılar paket kapsamalıdır olan uzunluğa sahip olabilir. Tek bir tamsayı değeri belirtilen değere eşit ilk bileşenlerle doğru uzunlukta bir tanımlama grubu olması için genişletilmiş ve kalan tüm bileşenleri birine eşit. Varsayılan değer biridir. Özellik eşlemeleri toplam sayısı tanımlama grubu bileşenlerinin ürünüdür. Bu toplam sayısı bileşenlerinde Finansman özellik eşlemesi değerleri hedef düğümleri nasıl gruplandırılacağını belirler. 

+ **Ağırlıkları**: (isteğe bağlı) paket için ilk ağırlıkları tanımlar. Değer, bu makalenin sonraki bölümlerinde tanımlanan ağırlıkları sayısı nokta değerleri tekrar sayısı olan uzunluğa sahip Çekirdek kayan bir tanımlama grubu olmalıdır. Varsayılan ağırlıkları rastgele üretilir.  

Doldurma, birbirini dışlayan olan özellikleri denetleyen özellikler iki kümesi vardır:

+ **Doldurma**: (isteğe bağlı) belirler olup giriş kullanarak dolgu uygulanması bir **varsayılan doldurma düzenini**. Değer, tek bir Boole değeri veya Boolean değeri paket kapsamalıdır olan uzunluğa sahip bir tanımlama grubu olabilir. 

    Tek bir Boole değeri belirtilen değere eşit tüm bileşenlerle doğru uzunlukta bir tanımlama grubu olarak genişletilir. 
    
    Bir boyut değeri True ise, bu boyuttaki ilk ve son tekrar merkezi düğümleri, ilk ve son düğümlerin olan şekilde kaynak mantıksal olarak sıfır değerli hücreler ek çekirdek uygulamaları desteklemek için o boyutundaki sıfır eklenir Kaynak katman boyutu. Bu nedenle, her boyut "kukla" düğümlerin sayısı otomatik olarak tam olarak sığması için belirlenen `(InputShape[d] - 1) / Stride[d] + 1` tekrar doldurulan kaynak katmana. 
    
    Bir boyut değeri False ise, sol çıkışı her tarafında düğüm sayısını (en fazla bir fark 1) aynı böylece tekrar tanımlanır. Bir tanımlama grubu yanlış eşit tüm bileşenlerle bu özniteliğin varsayılan değerdir.

+ **UpperPad** ve **LowerPad**: (isteğe bağlı) sağlama büyük denetime kullanmak için doldurma miktarı. **Önemli:** tanımlanmış ise ve yalnızca bu öznitelikler olabilir **doldurma** özelliği yukarıdaki ***değil*** tanımlanmış. Değerleri tamsayı değerli diziler paket kapsamalıdır olan uzunluklarına sahip olmalıdır. Bu öznitelikler belirtildiğinde, "kukla" düğümler giriş katmanın her boyut alt ve üst ucunun eklenir. Her boyut alt ve üst sona eriyor eklenen düğüm sayısı tarafından belirlenir **LowerPad**[i] ve **UpperPad**[i] sırasıyla. 

    Tekrar yalnızca "gerçek" düğümler ve "kukla" düğümleri karşılık geldiğinden emin olmak için aşağıdaki koşullar karşılanmalıdır:
      - Her bir bileşeninin **LowerPad** olmalıdır değerinden kesinlikle küçük `KernelShape[d]/2`. 
      - Her bir bileşeninin **UpperPad** daha büyük olmalıdır `KernelShape[d]/2`. 
      - Tüm bileşenlerle 0'a eşit bir tanımlama grubu bu özniteliklerin varsayılan değerdir. 

    Ayar **doldurma** = true "giriş merkezini" çekirdek "gerçek" içinde tutmak için gerektiği kadar doldurma sağlar. Bu çıktı boyutunu bilgi işlem için bir bit matematik değiştirir. Genellikle, çıkış boyutu *D* olarak hesaplanan `D = (I - K) / S + 1`, burada `I` giriş boyutu `K` çekirdek boyutu `S` STRIDE olduğu ve `/` tamsayı bölme (sıfıra doğru yuvarlar olduğu ). UpperPad ayarlarsanız = [1, 1], giriş boyutu `I` 29 etkin olduğunu ve bu nedenle `D = (29 - 5) / 2 + 1 = 13`. Ancak, ne zaman **doldurma** = true, aslında `I` tarafından indirgenmesine `K - 1`; bu nedenle `D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14`. İçin değerler belirterek **UpperPad** ve **LowerPad** yalnızca ayarlarsanız daha çok daha fazla denetime doldurma alma **doldurma** = true.

Convolutional ağları ve bunların uygulamaları hakkında daha fazla bilgi için bu makalelere bakın: 

+ [http://deeplearning.net/tutorial/lenet.html ](http://deeplearning.net/tutorial/lenet.html)
+ [http://research.microsoft.com/pubs/68920/icdar03.pdf](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
+ [http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Paket havuzu

A **paket havuzu** geometri convolutional bağlantısı benzer geçerlidir, ancak hedef düğüm değeri türetilmesi için kaynak düğüm değerleri için önceden tanımlanmış işlevleri kullanır. Bu nedenle, havuzu paketleri trainable durum yok (ağırlıkları veya stratejimizdeki) sahip. Havuzu paketleri destek dışındaki tüm convolutional öznitelikleri **paylaşım**, **MapCount**, ve **ağırlıkları**.

Genellikle, bitişik havuzu birimler tarafından özetlenen tekrar çakışmaz. STRIDE [d] her boyut KernelShape [d] eşit ise, elde edilen katman hangi convolutional sinir ağlarda yaygın olarak kullanılan geleneksel yerel havuzu oluşturma, katmanıdır. Her hedef düğüm maksimum veya kendi çekirdek kaynak katmandaki etkinliklerini ortalaması hesaplar.  

Aşağıdaki örnek bir havuzu paket gösterilmektedir: 

```Net#
hidden P1 [5, 12, 12]
  from C1 max pool {
  InputShape  = [ 5, 24, 24];
   KernelShape = [ 1,  2,  2];
   Stride      = [ 1,  2,  2];
  }  
```

+ Paket sayısı 3'tür: başka bir deyişle, başlıklar uzunluğu `InputShape`, `KernelShape`, ve `Stride`. 
+ Kaynak katmandaki düğüm sayısı: `5 * 24 * 24 = 2880`. 
+ Bunun nedeni geleneksel yerel havuzu katman, **KernelShape** ve **STRIDE** eşit. 
+ Hedef katmandaki düğüm sayısı: `5 * 12 * 12 = 1440`.

Havuzu katmanları hakkında daha fazla bilgi için bu makalelere bakın:  

+ [http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Bölüm 3.4)
+ [http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
+ [http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)

## <a name="response-normalization-bundles"></a>Yanıt normalleştirme paketleri

**Yanıt normalleştirme** ilk Geoffrey Hinton tarafından sunulan bir yerel normalleştirme düzenidir yazıda et al [ImageNet Classiﬁcation Convolutional derin sinir ağları ile](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). 

Yanıt normalleştirme sinir ağ Genelleştirme yardımcı olmak için kullanılır. Çok yüksek etkinleştirme düzeyinde bir neuron tetikleme, yerel yanıt normalleştirme katman çevresindeki neurons etkinleştirme düzeyini gizler. Bu üç parametre kullanılarak yapılır (`α`, `β`, ve `k`) ve bir convolutional yapısı (veya Komşuları Şekil). Hedef katmanda her neuron **y** neuron için karşılık gelen **x** kaynak katmandaki. Etkinleştirme düzeyi **y** aşağıdaki formülde verilen nerede `f` bir neuron etkinleştirme düzeyi ve `Nx` çekirdeğidir (veya Komşuları içinde neurons içeren küme **x**), aşağıdaki convolutional yapısı tarafından tanımlanan:  

![convolutional yapısı formülü](./media/azure-ml-netsharp-reference-guide/formula_large.png)

Yanıt normalleştirme paketleri destek dışındaki tüm convolutional öznitelikleri **paylaşım**, **MapCount**, ve **ağırlıkları**.  

+ Çekirdek neurons aynı eşlemesindeki içeriyorsa ***x***, normalleştirme düzeni olarak adlandırılır **aynı eşleme normalleştirme**. Aynı eşleme normalleştirme, ilk koordinat tanımlamak için **InputShape** 1 değeri olmalıdır.

+ Çekirdek neurons aynı konumda yer alan uzamsal içeriyorsa ***x***, ancak neurons diğer eşlemeleri, normalleştirme düzeni adı verilen **normalleştirme'çapraz eşlemeleri**. Yanıt normalleştirme bu tür yanal inhibition farklı eşlemeleri hesaplanan neuron çıktılar arasında büyük etkinleştirme düzeyleri için rekabet oluşturma gerçek neurons içinde bulunan türe göre esin biçimi uygular. Eşlemeleri normalleştirme arasında tanımlamak için ilk koordinat birden büyük ve haritalar sayısından daha büyük bir tamsayı olmalıdır ve koordinatları kalan 1 değeri olmalıdır.

Yanıt normalleştirme paketleri önceden tanımlanmış bir işlev hedef düğüm değeri belirlemek için kaynak düğüm değerleri için geçerli olduğundan, bunlar trainable durum yok (ağırlıkları veya stratejimizdeki) sahip.

> [!NOTE]
> Hedef katmanı düğümlerin tekrar merkezi düğümler neurons karşılık gelir. Örneğin, varsa `KernelShape[d]` tek, olup `KernelShape[d]/2` merkezi çekirdek düğüme karşılık gelir. Varsa `KernelShape[d]` olsa bile, merkezi düğümdür adresindeki `KernelShape[d]/2 - 1`. Bu nedenle, varsa `Padding[d]` False, ilk ve son `KernelShape[d]/2` düğümleri hedef katmanda ilgili düğümleri gerekmez. Bu durumu önlemek için tanımlamak **doldurma** olarak [true, true,..., true].

Daha önce açıklanan dört öznitelikleri yanı sıra yanıt normalleştirme paketleri ayrıca aşağıdaki özniteliklere destekler:

+ **Alpha**: (gerekli) belirtir karşılık gelen bir kayan nokta değer `α` önceki formülünde. 
+ **Beta**: (gerekli) belirtir karşılık gelen bir kayan nokta değer `β` önceki formülünde. 
+ **Uzaklık**: (isteğe bağlı) belirtir karşılık gelen bir kayan nokta değer `k` önceki formülünde. Bu varsayılan olarak 1.

Aşağıdaki örnek, bu öznitelikler kullanarak bir yanıt normalleştirme paketini tanımlar:  

```Net#
hidden RN1 [5, 10, 10]
from P1 response norm {
  InputShape  = [ 5, 12, 12];
  KernelShape = [ 1,  3,  3];
  Alpha = 0.001;
  Beta = 0.75;
  }  
```

+ Kaynak katmanın her 12 x 12, 1440 düğümler toplamda aof boyutla beş eşlemeleri içerir. 
+ Değeri **KernelShape** bu Komşuları 3 x 3 dikdörtgen olduğu aynı bir harita normalleştirme katman olduğunu gösterir. 
+ Varsayılan değer olan **doldurma** false, böylece Hedef katmanı her boyut yalnızca 10 düğüm yok. Kaynak katman kümedeki her düğüm için karşılık gelen hedef katmandaki tek bir düğüm eklemek için dolgu ekleme [true, true, true] =; ve [5, 12, 12] RN1 boyutunu değiştirin.  

## <a name="share-declaration"></a>Paylaşım bildirimi

NET # isteğe bağlı olarak paylaşılan ağırlıklara sahip birden çok paket tanımlama destekler. Kendi yapıları aynıysa herhangi iki paket ağırlıkları paylaşılabilir. Aşağıdaki söz dizimini paylaşılan ağırlıklara sahip paketleri tanımlar:  

```Net#
share-declaration:
  share    {    layer-list    }
  share    {    bundle-list    }
  share    {    bias-list    }

  layer-list:
    layer-name    ,    layer-name
    layer-list    ,    layer-name

  bundle-list:
    bundle-spec    ,    bundle-spec
    bundle-list    ,    bundle-spec

  bundle-spec:
    layer-name    =>     layer-name

  bias-list:
    bias-spec    ,    bias-spec
    bias-list    ,    bias-spec

  bias-spec:
    1    =>    layer-name

  layer-name:
    identifier
```

Örneğin, aşağıdaki paylaşım bildirimi ağırlıkları ve stratejimizdeki paylaşılan olduğunu gösteren katman adlarını belirtir:  

```Net#
Const {
  InputSize = 37;
  HiddenSize = 50;
  }
input {
  Data1 [InputSize];
  Data2 [InputSize];
  }
hidden {
  H1 [HiddenSize] from Data1 all;
  H2 [HiddenSize] from Data2 all;
  }
output Result [2] {
  from H1 all;
  from H2 all;
  }
share { H1, H2 } // share both weights and biases  
```

+ Giriş özellikleri iki eşit boyutta giriş katmanlara bölümlenir. 
+ Gizli katmanları sonra iki giriş katmanlardaki daha yüksek düzey özelliklerinin işlem. 
+ Paylaşım bildirimi belirleyen *H1* ve *H2* kendi ilgili girişler aynı biçimde hesaplanan gerekir.  

Alternatif olarak, bu iki ayrı paylaşımı-bildirimlerle gibi belirtilebilir:  

```Net#
share { Data1 => H1, Data2 => H2 } // share weights  
<!-- -->
    share { 1 => H1, 1 => H2 } // share biases  
```

Yalnızca tek bir paket katmanları içeriyorsa, kısa biçimi kullanabilirsiniz. Yalnızca ilgili yapısı aynı boyut, aynı convolutional geometri ve benzeri olduğunu yani aynı genel olarak, paylaşımı mümkün olur.  

## <a name="examples-of-net-usage"></a>Net # kullanım örnekleri

Bu bölümde nasıl Net # gizli katmanları eklemek için gizli katmanları diğer katmanları ile etkileşim kurmanızı ve convolutional ağlar oluşturmak şekilde tanımlamak için kullanabileceğiniz bazı örnekler sağlar.

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Basit bir özel sinir ağı tanımlar: "Hello World" örnek

Bu basit örnekte, tek bir gizli katmanın sahip olduğu bir sinir ağı model oluşturmak gösterilmiştir.

```Net#
input Data auto;
hidden H [200] from Data all;
output Out [10] sigmoid from H all;  
```

Örnek gibi bazı temel komutları gösterir:  

+ İlk satırı giriş katman tanımlar (adlı `Data`). Kullandığınızda `auto` anahtar sözcüğü, sinir ağı giriş örneklerde otomatik olarak tüm özellik sütunları içerir. 
+ İkinci satır gizli katman oluşturur. Adı `H` 200 düğümü olan bir gizli katmana atanır. Bu katman tam giriş katmanına bağlıdır.
+ Üçüncü satır çıktı katman tanımlar (adlı `O`), 10 çıkış düğümlerini içerir. Sinir ağı için sınıflandırma kullanılırsa, bir çıktı düğüm başına sınıfı yok. Anahtar sözcüğü **sigmoid** çıkış işlevi çıktı katmana uygulandığını gösterir.

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Birden çok gizli katmanları tanımlayın: bilgisayar görme örneği

Aşağıdaki örnek, birden çok özel gizli katmanlarıyla biraz daha karmaşık bir sinir ağı tanımlamak gösterilmiştir.  

```Net#
// Define the input layers 
input Pixels [10, 20];
input MetaData [7];

// Define the first two hidden layers, using data only from the Pixels input
hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;

// Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
hidden Gather [100] 
{
from ByRow all;
from ByCol all;
}

// Define the output layer and its sources
output Result [10]  
{
from Gather all;
from MetaData all;
}  
```

Bu örnek sinir ağları belirtimi dil çeşitli özellikleri gösterir:

+ İki giriş Katmanlar yapısının `Pixels` ve `MetaData`.
+ `Pixels` Katmanıdır hedef katmanlarıyla iki bağlantı paketleri için bir kaynak katman `ByRow` ve `ByCol`.
+ Katmanları `Gather` ve `Result` birden çok bağlantı paket hedef katmanlarda olan.
+ Çıktı katman `Result`, bir hedef katmandaki iki bağlantı paketleri; ikinci düzey gizli katmana sahip bir `Gather` hedef katman ve diğer giriş katman olarak `MetaData` Hedef katmanı olarak.
+ Gizli katmanları `ByRow` ve `ByCol`, filtrelenmiş bağlantı koşullu ifadeler kullanarak belirtin. Daha hassas bir şekilde düğümünde `ByRow` adresindeki [x, y] düğümlerin bağlı `Pixels` kullanıcının ilk koordinat, x sahip ilk dizin koordine, düğüm eşittir. Benzer şekilde, düğümünde `ByCol` konumundaki [x, y] düğümlerin bağlı `Pixels` sahip ikinci bir dizin koordine etmek, bir düğüm kullanıcının ikinci koordinat, y.

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Çok sınıflı sınıflandırma convolutional ağ tanımlayın: rakam tanıma örneği

Aşağıdaki ağ tanımını numaraları tanımak için tasarlanmıştır ve sinir ağı özelleştirmek için Gelişmiş bazı teknikleri göstermektedir.  

```Net#
input Image [29, 29];
hidden Conv1 [5, 13, 13] from Image convolve 
  {
  InputShape  = [29, 29];
  KernelShape = [ 5,  5];
  Stride      = [ 2,  2];
  MapCount    = 5;
  }
hidden Conv2 [50, 5, 5]
from Conv1 convolve 
  {
  InputShape  = [ 5, 13, 13];
  KernelShape = [ 1,  5,  5];
  Stride      = [ 1,  2,  2];
  Sharing     = [false, true, true];
  MapCount    = 10;
  }
hidden Hid3 [100] from Conv2 all;
output Digit [10] from Hid3 all;  
```

+ Tek bir giriş katman yapısının `Image`.
+ Anahtar sözcüğü `convolve` katmanları adlı gösterir `Conv1` ve `Conv2` convolutional katmanlardır. Bu katman bildirimleri her evrişim özniteliklerin bir listesi tarafından izlenir.
+ Net üçüncü katman, gizlenmiş `Hid3`, hangi tam olarak bağlı olan ikinci gizli katmana `Conv2`.
+ Çıktı katman `Digit`, yalnızca üçüncü gizli katmana, bağlı `Hid3`. Anahtar sözcüğü `all` için çıktı katmanı tam olarak bağlı olduğunu belirtiyor `Hid3`.
+ Evrişim kapsamalıdır üç olduğu: başlıklar uzunluğu `InputShape`, `KernelShape`, `Stride, and `Paylaşım '. 
+ Çekirdek başına ağırlıkları sayısı `1 + KernelShape\[0] * KernelShape\[1] * KernelShape\[2] = 1 + 1 * 5 * 5 = 26`. veya `26 * 50 = 1300`.
+ Her Gizli katmandaki düğüm şu şekilde hesaplayabilirsiniz:

    `NodeCount\[0] = (5 - 1) / 1 + 1 = 5` `NodeCount\[1] = (13 - 5) / 2 + 1 = 5` `NodeCount\[2] = (13 - 5) / 2 + 1 = 5`

+ Toplam düğümlerin sayısının katmanının bildirilen boyut kullanılarak hesaplanabilir [50, 5, 5], şu şekilde: `MapCount * NodeCount\[0] * NodeCount\[1] * NodeCount\[2] = 10 * 5 * 5 * 5`
+ Çünkü `Sharing[d]` yanlış yalnızca `d == 0`, tekrar sayısı `MapCount * NodeCount\[0] = 10 * 5 = 50`. 

## <a name="acknowledgements"></a>Bildirimler

Sinir ağları mimarisini özelleştirmek için Net # dili Microsoft'taki Shon Katzenberger (Mimarı, Machine Learning) ve Alexey Kamenev (yazılım mühendisi, Microsoft Research) tarafından geliştirilmiştir. Machine learning projeleri ve görüntü algılama metin analizi arasında değişen uygulamalar için dahili olarak kullanılır. Daha fazla bilgi için bkz: [sinir ağ Azure ML - giriş Net # içinde](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)
