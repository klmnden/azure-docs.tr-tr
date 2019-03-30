---
title: 'Özel sinir ağlarıyla Net # ile oluşturma'
titleSuffix: Azure Machine Learning Studio
description: "Net # sinir ağları belirtim dilinin söz dizimi Kılavuzu. Azure Machine Learning Studio'da özel sinir ağı modelleri oluşturmayı öğrenin."
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: reference
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/01/2018
ms.openlocfilehash: 891b2988d04a3cf2f7c6676a837bc1ee199f4d16
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651500"
---
# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning-studio"></a>Azure Machine Learning Studio için NET # sinir ağı belirtim dili Kılavuzu

NET # derin sinir ağı veya rastgele boyutlarının convolutions gibi karmaşık sinir ağı mimarileri tanımlamak için kullanılan Microsoft tarafından geliştirilmiş bir dildir. Karmaşık yapıları, öğrenme, resim, video veya ses gibi veri çubuğunda geliştirmek için kullanabilirsiniz.

Bu bağlamda, Net # mimarisi belirtimi kullanabilirsiniz:

+ Microsoft Azure Machine Learning Studio'da tüm sinir ağı modülleri: [Çok sınıflı sinir ağı](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/multiclass-neural-network), [iki sınıflı sinir ağı](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/two-class-neural-network), ve [sinir ağı regresyon](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/neural-network-regression)
+ Microsoft ML Server işlevlerde sinir ağı: [NeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/neuralnet) ve [rxNeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxneuralnet)R dili için ve [rx_neural_network](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-neural-network) Python için.


Bu makalede, Net # kullanarak özel bir sinir ağı geliştirmek için ihtiyaç duyulan sözdizimi ve temel kavramlar açıklanmaktadır:

+ Sinir ağı gereksinimleri ve birincil bileşenlerini tanımlama
+ Söz dizimi ve anahtar sözcükler Net # belirtim dilinin
+ Net # kullanılarak oluşturulan özel sinir ağlarıyla



## <a name="neural-network-basics"></a>Sinir ağı temelleri

Sinir ağı yapısı katmanları ve ağırlıklı bağlantılar (veya kenarlar) düzenlenir düğümlerinin düğümleri arasında oluşur. Bağlantılar tek yönlü ve her bağlantının kaynak düğümü ve bir hedef düğümü vardır.

Bir veya daha fazla trainable her katman (gizli veya bir çıkış katmanı) sahip **bağlantı paketleri**. Bir bağlantı paket kaynak katman ve kaynak katmanın bağlantılarından belirtimini oluşur. Belirli bir paketteki tüm bağlantılar, kaynak ve hedef katmanları paylaşır. NET #'ta bir bağlantı paketi paketin hedef katmana ait olarak kabul edilir.

NET # çeşitli şekilde girişleri özelleştirmenize olanak sağlayan paketleri gizli katmanlara eşlenen ve çıktıları eşlenen bağlantı destekler.

Varsayılan veya standart bir paket bir **tam paket**, hedef katmanı kümedeki her düğüm için kaynak katmandaki her düğüm bağlı olduğu içinde.

Ayrıca, Gelişmiş bağlantı paketleri aşağıdaki dört tür Net # destekler:

+ **Filtrelenmiş paketler**. Bir koşul kaynağı katman ve hedef katmanın düğümlerini konumlarını kullanarak tanımlayabilirsiniz. Koşul True olduğunda düğümlerine bağlı.

+ **Evrişimsel paketleri**. Kaynak katmanda düğümler küçük Semt tanımlayabilirsiniz. Hedef katmanın her düğüme bir Komşuları kaynak katmandaki düğüm bağlı.

+ **Paketleri havuzu** ve **yanıt normalleştirme paketleri**. Kullanıcının kaynak katmanda düğümler küçük Semt tanımlar, bunlar evrişimsel paketleri için benzerdir. Bu paketleri kenarları ağırlıkları trainable olmayan farktır. Bunun yerine, önceden tanımlanmış bir işlevi hedef düğüm değeri belirlemek için kaynak düğüm değerlere uygulanır.


## <a name="supported-customizations"></a>Desteklenen özelleştirme

Azure Machine Learning Studio'da oluşturduğunuz sinir ağı modelleri mimarisi, Net # kullanarak kapsamlı olarak özelleştirilebilir. Şunları yapabilirsiniz:

+ Gizli katmanları oluşturmak ve her katmandaki düğüm sayısını denetleyin.
+ Nasıl birbiriyle bağlanması katmanlardır belirtin.
+ Convolutions ve paket paylaşımı ağırlık gibi özel bir bağlantı yapıları tanımlayın.
+ Farklı etkinleştirme işlevleri belirtin.

Belirtim dili sözdizimi ayrıntıları için bkz. [yapısı belirtimi](#structure-specifications).

Sinir ağları için bazı ortak makine öğrenme, karmaşık için tek yönlü görevlerden tanımlama örnekleri için bkz. [örnekler](#examples-of-net-usage).

## <a name="general-requirements"></a>Genel gereksinimler

+ Tam olarak bir çıkış katman, en az bir giriş katmanı ve sıfır veya daha fazla gizli katmanları olmalıdır.
+ Her katman, rastgele boyutları dikdörtgen bir dizi kavramsal olarak düzenlenmiş düğümler, sabit sayıda vardır.
+ Giriş katmanları ilişkili eğitilen parametresiz olmalıdır ve burada örnek verilerini ağ girer noktasını temsil eder.
+ Trainable katmanları (gizli ve çıkış katmanları) eğitilen parametreleri ağırlıkları sapmaları olarak bilinen, ilişkilendirdiniz.
+ Kaynak ve hedef düğümleri ayrı katmanında olmalıdır.
+ Bağlantılar, döngüsel olmayan yönlü olmalıdır; diğer bir deyişle, ilk kaynak düğüme geri önde gelen bağlantılar zinciri olamaz.
+ Çıkış katman bir bağlantı paketin kaynak katmanı olamaz.

## <a name="structure-specifications"></a>Yapı özellikleri

Bir sinir ağı yapısı belirtimi üç bölümlerini oluşur: **Sabit bildiriminde**, **katman bildirimi**, **bağlantı bildirimi**. Var. Ayrıca isteğe bağlı **paylaşım bildirimi** bölümü. Aşağıdaki bölümlerde, herhangi bir sırada belirtilebilir.

## <a name="constant-declaration"></a>Sabit bildirimi

Sabit bildirimi isteğe bağlıdır. Bu, sinir ağı tanımı içinde başka bir yerde kullanılan değerleri tanımlamak için bir yol sağlar. Ardından bir eşittir işareti ve bir değer ifadesi bir tanımlayıcının bildirimi deyimi oluşur.

Örneğin, aşağıdaki ifade bir sabit tanımlar `x`:

`Const X = 28;`

Aynı anda iki veya daha fazla sabitleri tanımlamak için tanımlayıcı adları ve değerleri ayraçlarının içine alın ve bunları noktalı virgül kullanarak ayırın. Örneğin:

`Const { X = 28; Y = 4; }`

Her bir atama ifadesinin sağ tarafı, tamsayı, bir gerçek sayı, bir Boole değeri (True veya False) veya matematik ifadesi olabilir. Örneğin:

`Const { X = 17 * 2; Y = true; }`

## <a name="layer-declaration"></a>Katman bildirimi

Katman bildirimi gereklidir. Bu, boyut ve öznitelikler ve bağlantı paketleri de dahil olmak üzere, katman kaynağı tanımlar. Bildirim deyiminin (giriş, gizli veya çıktı) katman adı ile başlar (pozitif tam sayılar demet) katman boyutları tarafından izlenen. Örneğin:

```Net#
input Data auto;
hidden Hidden[5,20] from Data all;
output Result[2] from Hidden all;
```

+ Katmandaki düğüm sayısını boyutları ürünüdür. Bu örnekte, 100 düğüm katmanda olduğu anlamına gelir iki boyutta [5,20] vardır.
+ Bir istisna dışında herhangi bir sırada katmanları bildirilebilir: Birden fazla giriş katman tanımlanmazsa, bildirilmiş olan girdi verilerini özelliklerin sırasını eşleşmesi gerekir.

Bir katmandaki düğüm sayısını otomatik olarak belirlenmesi gerektiğini belirtmek için kullanın `auto` anahtar sözcüğü. `auto` Anahtar sözcüğü katmana bağlı olarak farklı etkileri vardır:

+ Bir giriş katman bildiriminde düğüm sayısı, giriş verilerinin özelliklerini sayısıdır.
+ Bir gizli katman bildiriminde düğüm sayısı için parametre değeri tarafından belirtilen sayı olan **gizli düğüm sayısını**.
+ Bir çıkış katman bildiriminde iki sınıflı Sınıflandırma, regresyon ve çok sınıflı sınıflandırma için çıkış düğüm sayısına eşittir 1 2 düğümler sayısıdır.

Örneğin, aşağıdaki ağ tanımını otomatik olarak belirlenmesi tüm katmanların boyutunu sağlar:

```Net#
input Data auto;
hidden Hidden auto from Data all;
output Result auto from Hidden all;
```

Bir katman bildirimi trainable katman (gizli veya çıkış katmanları) için varsayılan olarak (bir etkinleştirme işlevi olarak da bilinir) çıkış işlevini isteğe bağlı olarak içerebilir **sigmoid** sınıflandırma modelleri için ve  **Doğrusal** regresyon modelleri için. Varsayılan kullansanız bile, açıkça etkinleştirme işlevi, açıklık için istenirse durumu.

Aşağıdaki çıkış işlevleri desteklenir:

+ sigmoid
+ Doğrusal
+ softmax
+ rlinear
+ Kare
+ Sqrt
+ srlinear
+ Abs
+ TANH
+ brlinear

Örneğin, aşağıdaki bildirimi kullanan **softmax** işlevi:

`output Result [100] softmax from Hidden all;`

## <a name="connection-declaration"></a>Bağlantı bildirimi

Hemen trainable katman tanımladıktan sonra tanımladığınız katmanlar arasında bağlantılar bildirmeniz gerekir. Bağlantı paket bildirimi anahtar sözcüğüyle başlar `from`paketin kaynak katman ve bağlantı paketi oluşturulacak tür adını çizgidir.

Şu anda, beş tür bağlantı paketleri desteklenir:

+ **Tam** paketleri, belirtilen anahtar sözcüğü `all`
+ **Filtrelenmiş** paketleri, anahtar sözcüğü tarafından belirtilen `where`takip eden bir koşul ifadesi
+ **Evrişimsel** paketleri, anahtar sözcüğü tarafından belirtilen `convolve`çizgidir evrişim öznitelikleri
+ **Havuzu** paketleri, belirtilen anahtar sözcüklere göre **en büyük havuz** veya **havuzu anlama**
+ **Yanıt normalleştirme** paketleri, anahtar sözcüğü tarafından belirtilen **yanıt norm**

## <a name="full-bundles"></a>Tam paket

Bir tam bağlantı paket, kaynak katmanında hedef katmanın her düğüm için her düğüme bir bağlantı içerir. Varsayılan ağ bağlantısı türü budur.

## <a name="filtered-bundles"></a>Filtrelenmiş paketler

Filtrelenmiş bağlantıda paket belirtimi gibi koşul, ifade edilen sözdizimi, çok içeren bir C# lambda ifadesi. Aşağıdaki örnek, iki filtrelenmiş paketler tanımlar:

```Net#
input Pixels [10, 20];
hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
```

+ İçin koşul olarak `ByRow`, `s` dizin giriş katmanın düğümlerinin dikdörtgen diziyi temsil eden bir parametre `Pixels`, ve `d` dizin gizli katmanın düğümlerinin diziyi temsil eden bir parametre `ByRow`. Her ikisi de türünü `s` ve `d` uzunlukta iki tamsayı dizisi olan. Kavramsal olarak, `s` aralıkları ile tamsayıların tüm çiftler üzerinden `0 <= s[0] < 10` ve `0 <= s[1] < 20`, ve `d` ile tüm tamsayıların çiftlerini aralıkları `0 <= d[0] < 10` ve `0 <= d[1] < 12`.

+ Koşul ifadesi sağ tarafında bir koşulu yoktur. Bu örnekte, her değeri için `s` ve `d` koşul True olduğu gibi Hedef katmanı düğümü kaynak katman düğümünden bir kenara yoktur. Bu nedenle, bu filtre ifadesi paket bağlantı tarafından tanımlanan düğümünden içerdiğini gösterir. `s` tarafından tanımlanan düğümüne `d` tüm durumlarda s [0] olduğu d [0] eşit.

İsteğe bağlı olarak, filtrelenmiş bir paket için ağırlıkları kümesi belirtebilirsiniz. Değeri **ağırlıkları** özniteliği, kayan nokta değerleri paket tarafından tanımlanan bağlantı sayısını eşleşen bir uzunlukta bir dizi olmalıdır. Varsayılan olarak, rastgele ağırlıkları oluşturulur.

Ağırlık değerleri, hedef düğüm dizine göre gruplandırılır. Diğer bir deyişle, ilk hedef düğümü K kaynak düğümlerine bağlı olup olmadığını ilk `K` öğelerini **ağırlıkları** tanımlama grubu olan ilk hedef düğümde kaynak dizin sırasıyla olacak. Aynı hedef arasını için geçerlidir.

Ağırlıklar doğrudan sabit değerler belirtmek mümkündür. Örneğin, daha önce öğrendiğiniz ağırlıkları, bunları bu söz dizimini kullanarak sabitleri belirtebilirsiniz:

`const Weights_1 = [0.0188045055, 0.130500451, ...]`

## <a name="convolutional-bundles"></a>Evrişimsel paketleri

Eğitim verileri homojen bir yapısı varsa, evrişimsel bağlantılar verileri, üst düzey özellikleri öğrenmek için yaygın olarak kullanılır. Örneğin, görüntü, ses veya video veriler, uzamsal veya zamana bağlı işlenemez oldukça Tekdüzen olabilir.

Evrişimsel paketleri kullanmak istemiyorsunuz dikdörtgen **çekirdekler** boyutları slid. Aslında, her çekirdek yerel Semt içinde uygulanan ağırlıkları olarak adlandırılan kümesini tanımlayan **çekirdek uygulamaları**. Bir düğüm olarak adlandırılır kaynak katmandaki her çekirdek uygulama karşılık **merkezi düğümü**. Bir çekirdek ağırlıkları birçok bağlantılar arasında paylaşılır. Evrişimsel bir paketteki her dikdörtgen çekirdeğidir ve aynı boyutta tüm çekirdek uygulamalardır.

Evrişimsel paketleri aşağıdaki öznitelikleri destekler:

**InputShape** evrişimsel bu paket amacıyla kaynak katmanın işlenemez tanımlar. Değer bir dizi pozitif tamsayı olmalıdır. Tamsayı ürün kaynak katmandaki düğüm sayısını eşit olmalıdır, ancak Aksi halde, kaynak katman için bildirilen işlenemez eşleşmesi gerekmez. Bu dizi uzunluğunu olur **kutup** evrişimsel paket için değer. Genellikle kutup sayıda bağımsız değişken veya işlev sürebilir işlenenleri için anlamına gelir.

Şekil ve çekirdekler konumlarını tanımlamak için öznitelikleri kullanma **KernelShape**, **STRIDE**, **doldurma**, **LowerPad**ve  **UpperPad**:

+ **KernelShape**: (gerekli) tanımlar evrişimsel paket için her çekirdek işlenemez. Değer bir tanımlama grubu paket kapsamalıdır eşit uzunluğu pozitif tamsayı olmalıdır. Bu tanımlama grubu'nın her bileşeninin karşılık gelen bileşeninden büyük **InputShape**.

+ **STRIDE**: (isteğe bağlı) merkezi düğümler arasındaki uzaklık Evrişim (her boyut için bir adım boyutu), kayan adım boyutunu tanımlar. Değer, pozitif tam sayılar, paketin kapsamalıdır uzunluğu tanımlama grubu olmalıdır. Bu tanımlama grubu'nın her bileşeninin karşılık gelen bileşeninden büyük **KernelShape**. Tüm bileşenleriyle bire eşit bir dizi varsayılan değerdir.

+ **Paylaşımı**: (isteğe bağlı) her evrişim boyut için paylaşımı ağırlık tanımlar. Değer, tek bir Boole değeri veya Boolean değeri paket kapsamalıdır olan bir uzunlukla bir tanımlama grubu olabilir. Tek bir Boole değeri belirtilen değere eşit olan tüm bileşenleri doğru uzunlukta bir tanımlama grubu e genişletilir. Tüm gerçek değerlerin oluşan bir dizi varsayılan değerdir.

+ **MapCount**: özellik sayısı eşler için evrişimsel paket (isteğe bağlı) tanımlar. Değer, pozitif bir tamsayı ya da pozitif tam sayılar, paketin kapsamalıdır uzunluğu tanımlama grubu olabilir. Tek bir tamsayı değeri belirtilen değere eşit olan ilk bileşenleri doğru uzunlukta bir tanımlama grubu için genişletilmiş ve kalan tüm bileşenleri birine eşit. Varsayılan değer biridir. Özellik eşlemeleri toplam sayısı, tanımlama grubu bileşenlerinin ürünüdür. Bu toplam sayısı bileşenlerinde hesaba katarak özellik eşlemesi değerleri hedef düğümleri nasıl gruplandırıldığını belirler.

+ **Ağırlıklar**: (isteğe bağlı) paket için ilk ağırlıkları tanımlar. Değeri kayan nokta değerleri çekirdeklerinin sayısı olan bir uzunlukla ağırlıkları sayısı, çekirdek bu makalenin sonraki bölümlerinde tanımlandığı şekilde, bir dizi olmalıdır. Varsayılan ağırlıkları rastgele oluşturulur.

İki doldurma, birbirini dışlayan olan özellikleri denetleyen özellikler kümesi vardır:

+ **Doldurma**: (isteğe bağlı) belirler olup giriş kullanarak sıfır bir **varsayılan doldurma düzeni**. Değer, tek bir Boole değeri veya Boolean değeri paket kapsamalıdır olan bir uzunlukla bir tanımlama grubu olabilir.

    Tek bir Boole değeri belirtilen değere eşit olan tüm bileşenleri doğru uzunlukta bir tanımlama grubu e genişletilir.

    Bir boyut değeri True ise, bu boyuttaki ilk ve son çekirdekler merkezi düğümleri, ilk ve son düğümleri gibi şekilde kaynak mantıksal olarak bu boyuttaki ek çekirdek uygulamaları desteklemek için sıfır değerli hücrelere ile doldurulur Kaynak katman boyut. Bu nedenle, her boyuttaki "kukla" düğüm sayısını otomatik olarak tam olarak sığması için belirlenen `(InputShape[d] - 1) / Stride[d] + 1` çekirdekler doldurulan kaynak katmana.

    Bir boyut değeri False ise, çekirdekler her tarafındaki solda gösterilmiştir düğüm sayısını (en fazla bir fark 1) aynı olması tanımlanır. Tüm bileşenler için False eşit olan bir tanımlama grubu bu özniteliğin varsayılan değerdir.

+ **UpperPad** ve **LowerPad**: (isteğe bağlı) sağlama daha fazla denetim sahibi kullanmak için doldurma miktarı. **Önemli:** Bu öznitelikler tanımlanan ve yalnızca olabilir **doldurma** özelliği yukarıdaki ***değil*** tanımlı. Değerleri tamsayı değerli demetleri ile paket kapsamalıdır olan olmalıdır. Bu öznitelikleri belirtildiğinde, "kukla" düğümlerinin her boyut giriş katmanın alt ve üst ucunun eklenir. Her boyuttaki alt ve üst sona eklenen düğüm sayısı tarafından belirlenir **LowerPad**[i] ve **UpperPad**[i] sırasıyla.

    Çekirdekleri yalnızca "gerçek" düğümleri ve "kukla" düğümleri karşılık geldiğinden emin olmak için aşağıdaki koşullar karşılanmalıdır:
  - Her bir bileşeninin **LowerPad** olmalıdır değerinden kesinlikle küçük `KernelShape[d]/2`.
  - Her bir bileşeninin **UpperPad** değerinden büyük olmalıdır `KernelShape[d]/2`.
  - Tüm bileşenleri 0'a eşit olan bir tanımlama grubu bu özniteliklerin varsayılan değerdir.

    Ayar **doldurma** = true "Giriş" çekirdek "real" içinde tutmak için gerektiği kadar doldurma sağlar. Bu matematik biraz bilgi işlem boyutu için değiştirir. Genellikle, çıkış boyutu *D* olarak hesaplanır `D = (I - K) / S + 1`burada `I` giriş boyutu `K` çekirdek boyutu `S` ilerleme olan ve `/` tamsayı bölme (sıfıra doğru yuvarlar olduğu ). UpperPad ayarlarsanız = [1, 1], girdi boyutuna `I` 29 etkin olduğundan ve bu nedenle `D = (29 - 5) / 2 + 1 = 13`. Ancak, **doldurma** = true, temelde `I` tarafından indirgenmesine `K - 1`; bu nedenle `D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14`. Değerlerini belirterek **UpperPad** ve **LowerPad** yalnızca ayarlarsanız daha çok daha fazla denetime doldurmayı alma **doldurma** = true.

Karmaşık ağlar ve uygulamalarını hakkında daha fazla bilgi için şu makalelere bakın:

+ [http://deeplearning.net/tutorial/lenet.html](http://deeplearning.net/tutorial/lenet.html)
+ [https://research.microsoft.com/pubs/68920/icdar03.pdf](https://research.microsoft.com/pubs/68920/icdar03.pdf)

## <a name="pooling-bundles"></a>Paketleri havuzu

A **paket havuzu** geometri evrişimsel bağlantısı benzer geçerlidir ancak hedef düğüm değeri türetmek için önceden tanımlanmış işlevler için kaynak düğüm değerleri kullanır. Bu nedenle, havuzu paketleri trainable durumu olmadan (ağırlıkları veya sapmaları) sahip. Havuzu oluşturma paketleri destek dışında tüm evrişimsel öznitelikleri **paylaşım**, **MapCount**, ve **ağırlıkları**.

Genellikle, bitişik havuzu birimleri tarafından özetlenen çekirdekler çakışmaz. STRIDE [d] her boyut için KernelShape [d] eşit ise, elde edilen katman evrişimsel sinir ağları yaygın olarak kullanılır, geleneksel yerel havuzu oluşturma, katmanıdır. Her hedef düğüm, en yüksek veya kaynak katman, çekirdek etkinliklerini ortalamasını hesaplar.

Aşağıdaki örnek, bir havuzu paket gösterilmektedir:

```Net#
hidden P1 [5, 12, 12]
  from C1 max pool {
  InputShape  = [ 5, 24, 24];
   KernelShape = [ 1,  2,  2];
   Stride      = [ 1,  2,  2];
  }
```

+ Paket sayısı 3'tür: diğer bir deyişle, diziler uzunluğunu `InputShape`, `KernelShape`, ve `Stride`.
+ Kaynak katmandaki düğüm sayısı `5 * 24 * 24 = 2880`.
+ Bunun nedeni yerel geleneksel havuzu katman, **KernelShape** ve **STRIDE** eşit.
+ Hedef katmanında düğüm sayısı `5 * 12 * 12 = 1440`.

Havuzu oluşturma katmanları hakkında daha fazla bilgi için şu makalelere bakın:

+ [https://www.cs.toronto.edu/~hinton/absps/imagenet.pdf](https://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Bölüm 3.4)
+ [https://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf](https://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf)
+ [https://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf](https://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)

## <a name="response-normalization-bundles"></a>Yanıt normalleştirme paketleri

**Yanıt normalleştirme** ilk Geoffrey Hinton tarafından sunulan yerel normalleştirme düzenidir yazıda tarayıcılarınızda [derin Evrişimsel sinir ağları ile Imagenet sınıflandırma](https://www.cs.toronto.edu/~hinton/absps/imagenet.pdf).

Yanıt normalleştirme sinir ağ içinde Genelleştirme yardımcı olmak için kullanılır. Çok yüksek etkinleştirme düzeyinde bir neuron tetiklenmekte olan, yerel yanıt normalleştirme katman çevreleyen neurons etkinleştirme düzeyini bastırır. Bu üç parametre kullanarak gerçekleştirilir (`α`, `β`, ve `k`) ve bir evrişimsel yapısını (veya Komşuları Şekil). Hedef katmanın her neuron **y** bir neuron için karşılık gelen **x** kaynak katmandaki. Etkinleştirme düzeyini **y** aşağıdaki formülle verilen burada `f` bir neuron etkinleştirme düzeyi ve `Nx` çekirdeğidir (veya Komşuları içinde neurons içeren kümesi **x**), aşağıdaki evrişimsel yapısı tarafından tanımlandığı şekilde:

![evrişimsel yapısı için formülü](./media/azure-ml-netsharp-reference-guide/formula_large.png)

Yanıt normalleştirme paketleri destek dışında tüm evrişimsel öznitelikleri **paylaşım**, **MapCount**, ve **ağırlıkları**.

+ Çekirdek aynı haritadaki neurons içerip içermediğini ***x***, normalleştirme düzeni olarak adlandırılır **aynı Eşle normalleştirme**. Aynı harita normalleştirme, ilk koordinatı olarak tanımlamak için **InputShape** ' % s'değeri 1 olmalıdır.

+ Çekirdek uzamsal aynı konumda neurons içerip içermediğini ***x***, ancak neurons diğer eşlemelerinde, normalleştirme düzeni olarak adlandırılır **boyunca, normalleştirme eşler**. Bu tür bir yanıt normalleştirme, farklı eşlemeleri hesaplanan neuron çıktılar arasında büyük etkinleştirme düzeyleri için rekabet oluşturma gerçek neurons içinde bulunan türe göre ilham yanal inhibition biçiminin uygular. Arasında eşlemeler normalleştirme tanımlamak için ilk koordinat birden büyük ve haritalar sayısından daha büyük bir tamsayı olmalıdır ve rest koordinat değeri 1 olmalıdır.

Yanıt normalleştirme paketleri önceden tanımlanmış bir işlevi hedef düğüm değeri belirlemek için kaynak düğüm değerleri için geçerli olduğundan, hiçbir trainable durumunu (ağırlıkları veya sapmaları) sahiptirler.

> [!NOTE]
> Hedef katman düğümlerin çekirdekler merkezi düğümleri olan neurons karşılık gelir. Örneğin, varsa `KernelShape[d]` tek ise `KernelShape[d]/2` merkezi çekirdek düğüme karşılık gelir. Varsa `KernelShape[d]` olsa bile, merkezi düğümüdür adresindeki `KernelShape[d]/2 - 1`. Bu nedenle, `Padding[d]` False, ilk ve son `KernelShape[d]/2` düğümleri ilgili düğümlerin hedef katmanın sahip değildir. Bu durumu önlemek için tanımladığınız **doldurma** olarak [true, true,..., true].

Yanıt normalleştirme paketleri, daha önce açıklanan dört özniteliklere ek olarak aşağıdaki öznitelikleri de destekler:

+ **Alfa**: (gerekli) belirtir karşılık gelen bir kayan nokta değeri `α` önceki formülde.
+ **Beta**: (gerekli) belirtir karşılık gelen bir kayan nokta değeri `β` önceki formülde.
+ **Uzaklık**: (isteğe bağlı) belirtir karşılık gelen bir kayan nokta değeri `k` önceki formülde. Bu varsayılan olarak 1.

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

+ Kaynak katman, her 12 x 12 1440 düğümlerin toplam boyutunun aof beş eşlemeleri içerir.
+ Değerini **KernelShape** aynı bir harita normalleştirme katmanını Komşuları 3 x 3 dikdörtgen nerede olduğunu gösterir.
+ Varsayılan değer olan **doldurma** yanlış, böylece yalnızca 10 düğümü hedef katmanın her boyut sahiptir. Kaynak katman kümedeki her düğüm için karşılık gelen hedef katmanında bir düğüm eklemek için doldurma Ekle = [true, true, true]; ve [5, 12, 12] RN1 boyutunu değiştirin.

## <a name="share-declaration"></a>Paylaşım bildirimi

NET # isteğe bağlı olarak paylaşılan ağırlıklara sahip birden çok paket tanımlama destekler. Kendi yapılarını aynıysa, herhangi iki paketleri ağırlıkları paylaşılabilir. Aşağıdaki söz dizimini paylaşılan ağırlıklara sahip paketler tanımlar:

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

Örneğin, aşağıdaki paylaşım bildirimi ağırlıkları hem sapmaları paylaşılan olduğunu gösteren, bir katman adları belirtir:

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
+ Gizli katmanları iki giriş katmanlardaki daha yüksek düzey özellikler daha sonra işlem.
+ Paylaşım bildirimi belirten *H1* ve *H2* tıpkı kendi ilgili girişler hesaplanması gerekir.

Alternatif olarak, bu iki ayrı paylaşımı bildirimleri ile şu şekilde belirtilebilir:

```Net#
share { Data1 => H1, Data2 => H2 } // share weights
<!-- -->
    share { 1 => H1, 1 => H2 } // share biases
```

Yalnızca tek bir paket katmanları içeriyorsa, kısa biçim kullanabilirsiniz. Yalnızca ilgili yapısı aynı boyutta, aynı evrişimsel geometri ve benzeri olduğunu, yani aynı olduğunda genel olarak, paylaşımı mümkündür.

## <a name="examples-of-net-usage"></a>Net # kullanım örnekleri

Bu bölümde, nasıl Net # gizli katmanları eklemek için tanımlayabilirsiniz gizli katmanları diğer katmanları ile etkileşim kurmak ve karmaşık ağlar oluşturmak için kullanabileceğiniz bazı örnekler sağlar.

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Basit bir özel sinir ağı tanımlayın: "Hello World" örnek

Bu basit örnek, tek bir gizli katman olan sinir ağı modelinin nasıl oluşturulacağını gösterir.

```Net#
input Data auto;
hidden H [200] from Data all;
output Out [10] sigmoid from H all;
```

Örnekte, bazı temel komutlar şu şekilde gösterilmektedir:

+ İlk satırı giriş katman tanımlar (adlı `Data`). Kullanırken `auto` anahtar sözcüğü, sinir ağı giriş örnekleri otomatik olarak tüm özellik sütunları içerir.
+ İkinci satır, gizli katmanın oluşturur. Adı `H` 200 düğümü olan bir gizli katmana atanmış. Bu katman, tam olarak giriş katmana bağlı.
+ Üçüncü satır çıkış katman tanımlar (adlı `Out`), 10 çıkış düğümü içerir. Sinir ağı sınıflandırma için kullanılırsa, bir çıkış düğüm başına sınıfı yoktur. Anahtar sözcüğü **sigmoid** çıkış işlevi çıkış katmana uygulandığını gösterir.

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Birden çok gizli katmanları tanımlayın: bilgisayar işleme örneği

Aşağıdaki örnek, birden çok özel gizli katmanları ile biraz daha karmaşık sinir ağı tanımlamak gösterilmektedir.

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

Bu örnekte sinir ağları belirtim dili'nin birtakım özelliklerini gösterilmektedir:

+ İki giriş katmanları yapısının `Pixels` ve `MetaData`.
+ `Pixels` Katmanıdır hedef katmanlar ile iki bağlantı paketleri için bir kaynak katman `ByRow` ve `ByCol`.
+ Katmanları `Gather` ve `Result` birden çok bağlantı paket içindeki hedef katmanlardır.
+ Çıkış katman `Result`, bir hedef katmanın iki bağlantı paketleri; ikinci düzey gizli katmanın biriyle `Gather` hedef katman ve diğer giriş katman olarak `MetaData` Hedef katmanı olarak.
+ Gizli katmanları `ByRow` ve `ByCol`, filtrelenmiş bağlantı koşul ifadeleri kullanarak belirtin. Düğümde daha kesin `ByRow` adresindeki [x, y] düğümlere bağlı `Pixels` kullanıcının ilk koordinat, x sahip ilk dizin koordine, düğüm eşittir. Benzer şekilde, düğüm `ByCol` konumundaki [x, y] düğümlere bağlı `Pixels` sahip ikinci bir dizin koordine etmek, bir düğümün içinde uygulamanın ikinci koordinat, y.

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Karmaşık bir ağ sınıflı sınıflandırma için tanımlayın: rakam tanıma örneği

Aşağıdaki ağ tanımını numaraları tanımak için tasarlanmıştır ve bir sinir ağı özelleştirmek için bazı gelişmiş teknikleri gösterir.

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
+ Anahtar sözcüğü `convolve` katmanları adlı olduğunu gösteren `Conv1` ve `Conv2` evrişimsel katmanlardır. Bu katman bildirimlerinin her evrişim özniteliklerin bir listesi tarafından izlenir.
+ Net bir üçüncü katman gizledi `Hid3`, tam olarak bağlantısı olup ikinci gizli katmana `Conv2`.
+ Çıkış katman `Digit`, yalnızca üçüncü gizli katmana bağlı `Hid3`. Anahtar sözcüğü `all` çıkış katman için tam olarak bağlandığını gösteren `Hid3`.
+ Evrişim kapsamalıdır üçüncü bölümüdür: diziler uzunluğunu `InputShape`, `KernelShape`, `Stride, and `Paylaşım '.
+ Çekirdek başına ağırlıkları sayısı `1 + KernelShape\[0] * KernelShape\[1] * KernelShape\[2] = 1 + 1 * 5 * 5 = 26`. veya `26 * 50 = 1300`.
+ Her Gizli katmandaki düğüm şekilde hesaplayabilirsiniz:

    `NodeCount\[0] = (5 - 1) / 1 + 1 = 5``NodeCount\[1] = (13 - 5) / 2 + 1 = 5`
    `NodeCount\[2] = (13 - 5) / 2 + 1 = 5`

+ Düğümlerin toplam sayısı, katmanın bildirilen işlenemez kullanarak hesaplanabilir [50, 5, 5], şu şekilde: `MapCount * NodeCount\[0] * NodeCount\[1] * NodeCount\[2] = 10 * 5 * 5 * 5`
+ Çünkü `Sharing[d]` yanlış yalnızca `d == 0`, çekirdek sayısı `MapCount * NodeCount\[0] = 10 * 5 = 50`.

## <a name="acknowledgements"></a>Bildirimler

Sinir ağları mimarisi özelleştirmek için Net # dili, Microsoft'ta Shon Katzenberger (Mimarı, makine öğrenimi) ve Alexey Kamenev (yazılım mühendisi, Microsoft Research) tarafından geliştirilmiştir. Ayrıca, makine öğrenimi projeleri ve metin analizi için görüntü algılamadan kadar uygulamalarını için dahili olarak kullanılır. Daha fazla bilgi için [Azure Machine Learning Studio - giriş Net # sinir ağ](https://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)
