---
title: 'Bölüm ve örnek: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bölüm ve örnek modülü Azure Machine Learning hizmetinde bir veri kümesi üzerinde örnekleme gerçekleştirmek için veya bölümler, veri kümesinden oluşturmak için kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 72f9e11e3582d804eecc7479ea079276564bd12f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029288"
---
# <a name="partition-and-sample-module"></a>Bölüm ve örnek Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Örnekleme bir veri kümesi üzerinde gerçekleştirmek için veya bölümler, veri kümesinden oluşturmak için bu modülü kullanın.

Örnekleme, değerlerin aynı oranını koruyarak bir veri kümesi boyutu azaltmanıza izin verdiğinden, machine learning'e önemli bir araçtır. Bu modül, machine learning'de önemli olan birkaç ilgili görevleri destekler: 

- Verilerinizi aynı boyutta birden çok alt bölümlere ayırma. 

    Bölümler, çapraz doğrulama veya durumları rastgele gruplara atamak için kullanabilirsiniz.

- Gruplar halinde veri ayrılması ve ardından belirli bir grup verileri ile çalışma. 

    Çalışmaları farklı gruplara rastgele atadıktan sonra yalnızca bir grubuyla ilişkili olan özelliklerle değiştirmeniz gerekebilir.

- Örnekleme. 

    Verilerin yüzde ayıklayın, rastgele örnekleme Uygula veya veri kümesini dengeleyici'ye kullanın ve değerleri üzerinde stratified örnekleme gerçekleştirmek için bir sütun seçin.

- Test etmek için daha küçük veri kümesi oluşturma. 

    Çok fazla veri varsa yalnızca ilk kullanmak isteyebilirsiniz *n* satırları çalışırken deneme ve model oluşturma sırasında tam veri kümesi kullanarak sonra geçme ayarlama. Örnekleme, geliştirme s kullanmak için daha küçük veri kümesi oluşturmak için de kullanabilirsiniz.

## <a name="configure-partition-and-sample"></a>Bölüm ve örnek yapılandırma

Bu modül, örnekleme veya verilerinizi bölümlere ayırma için birden çok yöntemini destekler. Yöntemin ilk seçin ve ardından ek seçeneklerini ayarlama yöntemi tarafından gerekli.

- Baş
- Örnekleme
- Hatları için atama
- Katlama seçin

### <a name="get-top-n-rows-from-a-dataset"></a>Bir veri kümesinden üst N satırları Al

Bu mod yalnızca ilk gidebilir *n* satır. Bir denemeyi az sayıda satır üzerinde test etmek istediğiniz ve dengeli ya da herhangi bir yolla örneklenmiş verilerin gerekmez bu seçenek yararlı olur.

1. Ekleme **bölüm ve örnek** denemenizde arabirimi modülüne ve veri kümesine bağlanın.  

2. **Bölüm veya örnek modu**: Bu seçenek kümesine **baş**.

3. **Seçmek için satır sayısı**: Döndürülecek satırların sayısını yazın.

    Belirttiğiniz satır sayısı negatif olmayan tamsayı olmalıdır. Seçilen satır sayısının veri kümesinde satır sayısı değerinden daha büyük veri kümesinin tamamının döndürülür.

4. Denemeyi çalıştırın.

Modül, yalnızca belirtilen satır sayısını içeren tek bir veri kümesi çıkarır. Satırları her zaman veri kümesinin başından okunur.

### <a name="create-a-sample-of-data"></a>Bir örnek veri oluşturma

Bu seçenek, basit rastgele örneklemeyi destekleyip veya rastgele örnekleme stratified. Bu, test etmek için daha küçük bir temsili bir örnek veri oluşturmak istiyorsanız kullanışlıdır.

1. Ekleme **bölüm ve örnek** modülü, denemenize ve veri kümesine bağlanın.

2. **Bölüm veya örnek modu**: Bu ayar **örnekleme**.

3. **Örnekleme oranını**: 0 ile 1 arasında bir değer yazın. Bu değer, satırları kaynak veri kümesi çıktı veri kümesi dahil edilmesi gereken yüzdesini belirtir.

    Özgün veri kümesinden yalnızca yarısını istiyorsanız, örneğin `0.5` örnekleme oranı % 50 olması gerektiğini belirtmek için.

    Giriş veri kümesi satırlarını karışık ve seçmeli olarak belirtilen oranına göre çıktı veri kümesi yerleştirin.

4. **Örnekleme için rastgele bir tohum**: İsteğe bağlı olarak, bir çekirdek değeri kullanılacak bir tamsayı yazın.

    Bu seçenek, aynı şekilde her saat Bölünecek satırları isterseniz önemlidir. Varsayılan değer 0 ise, sistem saatini başlangıç çekirdek oluşturulur anlamı tabanlı. Bu, denemeyi çalıştırma her zaman biraz farklı sonuçlara yol açabilir.

5. **Örnekleme için stratified bölünmüş**: Veri kümesindeki satırları örnekleme önce bazı anahtar sütununa göre eşit olarak bölüneceğini önemliyse, bu seçeneği belirleyin.

    İçin **Stratification anahtar sütun için örnekleme**, tek bir seçin *strata sütun* dataset bölerken kullanılacak. Veri kümesindeki satırları şu şekilde bölünür:

    1. Tüm giriş satırları (stratified) belirtilen strata sütunundaki değerlere göre gruplandırılır.

    2. Her grup satırları karışık.

    3. Her grup, belirtilen oranı karşılamak için çıktı veri kümesi için seçmeli olarak eklenir.


6. Denemeyi çalıştırın.

    Bu seçenekle bir temsili veri örnekleme özelliğini içeren tek bir veri kümesi modülü çıkarır. Veri kümesi kalan, örneklenmemiş bölümünü çıkış yok. 

## <a name="split-data-into-partitions"></a>Verileri bölümlere bölme

Veri kümesi verilerin alt kümelerine ayırmak istediğinizde bu seçeneği kullanın. Bu seçenek, büyük Katlama çapraz doğrulama için özel bir dizi oluşturun veya satır çeşitli gruplar halinde ayırmak için istediğiniz durumlarda da kullanışlıdır.

1. Ekleme **bölüm ve örnek** modülü, denemenize ve veri kümesine bağlanın.

2. İçin **bölüm veya örnek mod**seçin **hatları için Ata**.

3. **Bölümleme içinde değiştirme kullanın**: Örneklenen satır satır için olası yeniden havuza geri konmuş istiyorsanız bu seçeneği belirleyin. Sonuç olarak, aynı satırda birden çok büyük Katlama için atanmış olabilir.

    Değiştirme (varsayılan seçenek) kullanmıyorsanız, örneklenen satır satır için olası yeniden havuza geri eklenmez. Sonuç olarak, her satır için yalnızca bir Katlama atanabilir.

4. **Rastgele bölünmüş**:  Rastgele hatları için atanacak satırları istiyorsanız bu seçeneği belirleyin.

    Bu seçeneği belirlemezseniz, satırlar için hepsini bir kez deneme yöntemiyle hatları atanır.

5. **Rastgele bir tohum**: İsteğe bağlı olarak, çekirdek değeri kullanılacak bir tamsayı yazın. Bu seçenek, aynı şekilde her saat Bölünecek satırları isterseniz önemlidir. Aksi takdirde, varsayılan değer olan 0 rastgele bir sıra anlamına Çekirdek başlangıç kullanılır.

6. **Bölümleyici yöntemi belirtmek**: Nasıl bu seçenekleri kullanarak her bölüm için apportioned için veri istediğinizi belirtin:

    - **Eşit bir şekilde bölüm**: Her bölüme eşit sayıda satır yerleştirmek için bu seçeneği kullanın. Çıkış bölüm sayısını belirtmek için bir tamsayı türü **eşit olarak Bölünecek kat sayısı belirtin** metin kutusu.

    - **Özelleştirilmiş oranlarını bölümüyle**: Virgülle ayrılmış bir liste her bölümün boyutunu belirtmek için bu seçeneği kullanın.

        Örneğin, verilerin %50 içeren ilk bölümün ve kalan iki bölüm verileri içeren her 25 oranında üç bölüm oluşturmak istiyorsanız, tıklayın **oranlarını listesini virgülle ayrılmış** metin kutusu ve Bu sayı yazın: `.5, .25, .25`

        Tüm bölüm boyutları toplamını eklemelisiniz kadar tam olarak 1.

        - Ekleme en fazla sayı girerseniz **1'den az**, ek bir bölüm, kalan satırları tutmak için oluşturulur. Örneğin, yazarsanız değerler 2'dir ve.3, üçüncü bölüm tüm satırların kalan yüzde 50 tutan oluşturulur.

        - Ekleme en fazla sayı girerseniz **1'den fazla**, denemeyi çalıştırırken bir hata ortaya çıkar.

7. **Stratified bölünmüş**: Bu seçeneği belirleyin stratified için satır zaman bölmek istiyorsanız seçeneğini ve ardından _strata sütun_.

8. Denemeyi çalıştırın.

    Bu seçenek ile belirtilen kuralları kullanılarak bölümlenmiş birden fazla veri kümesi, modül çıkarır.

### <a name="use-data-from-a-predefined-partition"></a>Önceden tanımlanmış bir bölüm verileri kullan  

Bu seçenek, bir veri kümesi birden çok bölümlere ve artık daha fazla analiz veya işleme için her bölüm sırayla yüklemek istediğinizde kullanılır.

1. Ekleme **bölüm ve örnek** modülünü deneme.

2. Önceki örneği çıkışına bağlayın **bölüm ve örnek**. Bu örnekte kullanılan **hatları için Ata** bazı bölümlerini sayısını oluşturmak için seçeneği.

3. **Bölüm veya örnek modu**: Seçin **çekme Katlama**.

4. **Hangi den örneklenmiş için Katlama belirtin**: Dizinini yazarak kullanmak için bir bölüm seçin. Bölüm dizinleri 1 tabanlıdır. Örneğin, üç bölüme dataset ayrılmış, bölümleri dizinleri 1, 2 ve 3 gerekir.

    Geçersiz dizin değeri yazarsanız, tasarım zamanı hata ortaya çıkar: "Hata 0018: Veri kümesi geçersiz veri içeriyor."

    Veri kümesi tarafından hatları gruplandırma ek olarak, veri kümesini ikiye ayırabilirsiniz: hedef Katlama ve diğer her şey. Bunu yapmak için tek bir Katlama dizini yazın ve ardından bir seçenek belirleyin **çekme seçili Katlama tamamlayıcısına**, belirtilen kat her şeyi veri almak için.

5. Birden çok bölüm ile çalışıyorsanız, ek örneklerini eklemelisiniz **bölüm ve örnek** her bölüm işlemek için modülü.

    Dört hatları yaş kullanarak örneğin, daha önce bölümlenmiş hastalara varsayalım. Tek tek her Katlama ile çalışmak için dört kopyalarını gerekir. **bölüm ve örnek** modülü, ve her, farklı bir Katlama aşağıda gösterildiği gibi. Kullanılacak doğru değil **hatları için Ata** doğrudan çıktı.  

    [![Bölüm ve örnek](./media/partition-and-sample/partition-and-sample.png)](./media/partition-and-sample/partition-and-sample-lg.png#lightbox)

5. Denemeyi çalıştırın.

    Bu seçenek belirtilmişse, yalnızca bu Katlama için atanan satırları içeren tek bir veri kümesi modülü çıkarır.

> [!NOTE]
>  Katlama gösterimleri doğrudan görüntüleyemez; Bunlar yalnızca meta verilerde yok.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 