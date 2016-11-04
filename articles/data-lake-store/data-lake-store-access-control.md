---
title: Data Lake Store’da Access Control özelliğine genel bakış | Microsoft Docs
description: Azure Data Lake Store’da erişim denetimi hakkında bilgi edinin
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun

ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/06/2016
ms.author: nitinme

---
# Azure Data Lake Store’da erişim denetimi
Data Lake Store; HDFS ve sonuç olarak POSIX erişim denetimi modelinden türetilen bir erişim denetimi modeli kullanır. Bu makalede Data Lake Store için erişim denetimi modelinin temel bilgileri özetlenmektedir. HDFS erişim denetimi modeli hakkında daha fazla bilgi almak için bkz. [HDFS İzinleri Kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## Dosyalar ve klasörler üzerindeki erişim denetimi listeleri
İki tür Erişim denetimi listesi (ACL) vardır: **Erişim ACL’leri** ve **Varsayılan ACL’ler**.

* **Erişim ACL'leri** – Bunlar bir nesneye erişimi denetler. Hem Dosyalar hem de Klasörler ACL erişimine sahiptir.
* **Varsayılan ACL'ler** – Bir klasör ile ilişkili olan ACL’lerin o klasör altında oluşturulan tüm alt öğelere ilişkin Erişim ACL’lerini belirleyen bir "şablonu". Dosyalar Varsayılan ACL’ye sahip değildir.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Hem Erişim ACL'leri hem de Varsayılan ACL'ler aynı yapıdadır.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

> [!NOTE]
> Bir üst öğe üzerindeki Varsayılan ACL’nin değiştirilmesi zaten var olan alt öğelerin Erişim ACL’sini veya Varsayılan ACL’sini etkilemez.
> 
> 

## Kullanıcılar ve kimlikler
Her dosya ve klasör bu kimlikler için farklı izinlere sahiptir:

* Dosyanın sahibi olan kullanıcı
* Sahip olan grup
* Adlandırılmış kullanıcılar
* Adlandırılmış gruplar
* Diğer tüm kullanıcılar

Kullanıcıların ve grupların kimlikleri Azure Active Directory (AAD) kimlikleridir; bu nedenle aksi belirtilmedikçe Data Lake Store bağlamında "kullanıcı" bir AAD kullanıcısını ya da bir AAD güvenlik grubunu ifade edebilir.

## İzinler
Dosya sistemi nesnesi üzerinde **Okuma**, **Yazma** ve **Yürütme** izinleri bulunur ve bunlar aşağıdaki tabloda gösterildiği gibi dosyalar ve klasörler üzerinde kullanılabilir.

|  | Dosya | Klasör |
| --- | --- | --- |
| **Okuma (R)** |Bir dosyanın içeriğini okuyabilir |Klasörün içeriğini listelemek için **Okuma** ve **Yürütme** izinlerini gerektirir. |
| **Yazma (W)** |Bir dosyaya yazabilir veya ekleyebilir |Bir klasörde alt öğeler oluşturmak için **Yazma ve Yürütme** gerektirir. |
| **Yürütme (X)** |Data Lake Store bağlamında herhangi bir anlamı yoktur |Bir klasörün alt öğelerini geçirmek için gereklidir. |

### İzinlerin kısaltmaları
**RWX**, **Okuma + Yazma + Yürütme** için kullanılır. **Okuma=4**, **Yazma=2** ve **Yürütme=1** olup toplamları izinleri temsil eden daha da kısaltılmış bir sayısal biçim mevcuttur. Bazı örnekler aşağıda verilmiştir.

| Sayısal biçim | Kısa biçim | Anlamı |
| --- | --- | --- |
| 7 |RWX |Okuma + Yazma + Yürütme |
| 5 |R-X |Okuma + Yürütme |
| 4 |R-- |Okuma |
| 0 |--- |İzin yok |

### İzinler devralınmaz
Data Lake Store tarafından kullanılan POSIX stili modellinde bir öğenin izinleri öğenin kendisine depolanır. Diğer bir deyişle, bir öğenin izinleri üst öğelerinden devralınamaz.

## İzinlerle ilgili yaygın senaryolar
Bir Data Lake Store hesabı üzerinde belirli işlemlerin gerçekleştirilmesi için gereken izinleri anlamaya yönelik bazı yaygın senaryolar aşağıda verilmiştir.

### Bir dosyayı okumak için gereken izinler
![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Okunacak dosya için çağıranın **Okuma** izinlerine sahip olması gerekir
* Dosyayı içeren klasör yapısındaki tüm klasörler için çağıranın **Yürütme** izinlerine sahip olması gerekir

### Bir dosyaya eklemek için gereken izinler
![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Eklemenin yapılacağı dosya için çağıranın **Yazma** izinlerine sahip olması gerekir
* Dosyayı içeren tüm klasörler için çağıranın **Yürütme** izinlerine sahip olması gerekir

### Bir dosyayı silmek için gereken izinler
![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Üst klasör için çağıranın **Yazma + Yürütme** izinlerine sahip olması gerekir
* Dosyanın yolundaki diğer tüm klasörler için çağıranın **Yürütme** izinlerine sahip olması gerekir

> [!NOTE]
> Yukarıdaki koşullar geçerli oldukça dosyayı silmek için dosya üzerinde yazma izinleri geçerli değildir.
> 
> 

### Bir klasörü listelemek için gereken izinler
![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Numaralandırılacak klasör için çağıranın **Okuma + Yürütme** izinlerine sahip olması gerekir
* Tüm üst klasörler için - çağıranın **Yürütme** izinlerine sahip olması gerekir

## Azure portalında görüntüleme izinleri
Data Lake Store hesabının **Veri Gezgini** dikey penceresinde **Erişim**’e tıklayarak bir dosya veya klasöre ilişkin ACL’leri görebilirsiniz. Aşağıdaki ekran görüntüsünde **mydatastore** hesabı altındaki **catalog** klasörüne ilişkin ACL’leri görmek için Erişim’e tıklayın.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Bundan sonra **Erişim** dikey penceresinde **Basit Görünüm**’e tıklayarak daha basit bir görünüm görün.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Daha gelişmiş görünümü görmek için **Gelişmiş Görünüm**’e tıklayın.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## Süper kullanıcı
Süper kullanıcı, Data Lake Store’daki tüm kullanıcılar arasında en fazla hakka sahiptir. Bir süper kullanıcı:

* **Tüm** dosya ve klasörlerde RWX İzinlerine sahiptir
* herhangi bir dosya veya klasörün izinlerini değiştirebilir.
* herhangi bir dosya veya klasörün sahibi olan kullanıcıyı ya da grubu değiştirebilir.

Azure’da bir Data Lake Store hesabının birkaç Azure rolü vardır:

* Sahipler
* Katkıda Bulunanlar
* Okuyucular
* Etc.

Bir Data Lake Store hesabında **Sahipler** rolündeki herkes otomatik olarak o hesabın süper kullanıcısıdır. Azure Rol Tabanlı Erişim Denetimi (RBAC) hakkında daha fazla bilgi almak için bkz. [Rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).

## Sahip olan kullanıcı
Öğeyi oluşturan kullanıcı otomatik olarak öğenin sahibi olan kullanıcıdır. Sahip olan kullanıcı şunları yapabilir:

* Sahip olunan bir dosyanın izinlerini değiştirme
* Sahip olan kullanıcı aynı zamanda hedef grubun bir üyesi oldukça, sahip olunan bir dosyanın sahibi olan grubunu değiştirme.

> [!NOTE]
> Sahip olan kullanıcı başka bir sahibi olunan dosyanın sahibini **değiştiremez**. Bir dosya veya klasörün sahibi olan kullanıcıyı yalnızca süper kullanıcılar değiştirebilir.
> 
> 

## Sahip olan grup
POSIX ACL’lerinde her kullanıcı bir "birincil grup" ile ilişkilendirilir. Örneğin, "gamze" adlı kullanıcı "finans" grubuna ait olabilir. Gamze birden fazla gruba ait olabilir, ancak bir grup her zaman birincil grubu olarak atanır. POSIX’te Gamze bir dosya oluşturduğunda o dosyanın sahibi olan grup birincil grubu olarak ayarlanır (bu örnekte “finans” grubudur).

Yeni bir dosya sistemi öğesi oluşturulduğunda, Data Lake Store sahip olan gruba bir değer atar. 

* **Olay 1** - Kök klasör "/". Bir Data Lake Store hesabı oluşturulduğunda bu klasör oluşturulur. Bu durumda sahip olan grup, hesabı oluşturan kullanıcıya ayarlanır.
* **Olay 2** (diğer her olay) - Yeni bir olay oluşturulduğunda sahip olan grup üst klasörden kopyalanır.

Sahip olan grup aşağıdakiler tarafından değiştirilebilir:

* Herhangi bir süper kullanıcı
* Sahip olan kullanıcı aynı zamanda hedef grubun üyesi ise sahip olan kullanıcı.

## Erişim denetimi algoritması
Aşağıdaki çizimde Data Lake Store hesaplarına yönelik erişim denetimi algoritması gösterilmektedir.

![Data Lake Store ACL’leri algoritması](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)

## Maske ve "etkili izinler"
**Maske**, Erişim Denetimi algoritmasını gerçekleştirirken **adlandırılmış kullanıcılar**, **sahip olan grup** ve **adlandırılmış gruplar** için erişimi sınırlandırmak üzere kullanılan bir RWX değeridir. Maskeye ilişkin anahtar kavramlar aşağıda verilmiştir. 

* Maske "etkili izinler" oluşturur; diğer bir deyişle, Erişim Denetimi zamanında izinleri değiştirir.
* Maske doğrudan dosya sahibi ve herhangi bir süper kullanıcı tarafından düzenlenebilir.
* Maske etkili izin oluşturmaya yönelik izinleri kaldırabilir. Maske etkili izne izinler **ekleyemez**. 

Bazı örneklere bakalım. Aşağıda maske **RWX** olarak ayarlanmıştır; diğer bir deyişle maske herhangi bir izni kaldırmaz. Adlandırılmış kullanıcı, sahip olan kullanıcı ve adlandırılmış grubun erişim denetimi sırasında değiştirilmediğine dikkat edin.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

Aşağıdaki örnekte maske **R-X** olarak ayarlanmıştır. Bu nedenle, erişim denetimi sırasında **adlandırılmış kullanıcı**, **sahip olan grup** ve **adlandırılmış grup** için **Yazma izinlerini kapatır**.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Başvuru için bir dosyanın veya klasörün maskesinin Azure Portal’da nerede göründüğü aşağıda gösterilmiştir.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> Yeni bir Data Lake Store hesabı için kök klasörün ("/") Erişim ACL’leri ve Varsayılan ACL için maske varsayılan olarak RWX’tir.
> 
> 

## Yeni dosyalar ve klasörler üzerindeki izinler
Var olan bir klasör altında yeni bir dosya ya da klasör oluşturulduğunda üst klasördeki Varsayılan ACL aşağıdakileri belirler:

* Bir alt klasörün Varsayılan ACL’si ve Erişim ACL’si
* Bir alt dosyanın Erişim ACL’si (dosyaları Varsayılan ACL’ye sahip değildir)

### Bir alt dosya veya klasörün Erişim ACL’si
Bir alt dosya veya klasör oluşturulduğunda, üst öğenin Varsayılan ACL’si alt dosya veya klasörün Erişim ACL’si olarak kopyalanır. Ayrıca, **diğer** kullanıcının üst klasör varsayılan ACL’sinde RWX izinleri varsa alt öğenin Erişim ACL’sinden tamamen kaldırılır.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

Çoğu senaryoda yukarıdaki bilgiler bir alt öğesinin Erişim ACL’sinin belirlenmesi için yeterlidir. Ancak, POSIX sistemlerini biliyor ve bu bilgilerin nasıl elde edildiğini derinlemesine öğrenmek istiyorsanız bu makalenin sonraki bölümlerinde bulunan [Yeni dosyalar ve klasörler için Erişim ACL’lerini oluşturmada Umask rolü](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) kısmına bakın.

### Bir alt klasörün Varsayılan ACL’si
Üst klasör altında bir alt klasör oluşturulduğunda üst klasörün Varsayılan ACL’si olduğu gibi alt klasörün Varsayılan ACL’sine kopyalanır.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## Data Lake Store’da ACL’leri anlamaya yönelik gelişmiş konular
Data Lake Store dosyaları veya klasörleri için ACL’lerin nasıl belirlendiğini anlamanıza yardımcı olan birkaç gelişmiş konu aşağıda verilmiştir.

### Yeni dosyalar ve klasörler için Erişim ACL’lerini oluşturmada Umask rolü
POSIX ile uyumlu bir sistemde genel kavram umask’in yeni bir alt klasör veya klasörün Erişim ACL’si üzerinde **sahip olan kullanıcı**, **sahip olan grup** ve **diğer** iznini dönüştürmek için kullanılan üst klasördeki 9 bitlik bir değer olmasıdır. Bir umask’in bit değerleri alt öğenin Erişim ACL’sinde hangi bitlerin kapatılacağını belirler. Bu nedenle, sahip olan kullanıcı, sahip olan grup ve diğer için izinlerin yayılmasını seçici olarak önlemek üzere kullanılır.

Bir HDFS sisteminde umask genellikle yöneticiler tarafından denetlenen site genelindeki bir yapılandırma seçeneğidir. Data Lake Store değiştirilemeyen bir **hesap genelinde umask** kullanır. Aşağıdaki tabloda Data Lake Store umask gösterilmektedir.

| Kullanıcı grubu | Ayar | Yeni alt öğenin Erişim ACL’si üzerindeki etkisi |
| --- | --- | --- |
| Sahip olan kullanıcı |--- |Etki yok |
| Sahip olan grup |--- |Etki yok |
| Diğer |RWX |Okuma + Yazma + Yürütme iznini kaldırma |

Aşağıdaki çizimde bu umask eylemi gösterilmektedir. Net etki **diğer** kullanıcı için **Okuma + Yazma + Yürütme** izninin kaldırılmasıdır. Umask **sahip olan kullanıcı** ve **sahip olan grup** için bitleri belirtmediğinden bu izinler dönüştürülmez.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### Yapışkan bit
Yapışkan bit POSIX dosya sisteminin daha gelişmiş bir özelliğidir. Data Lake Store bağlamında yapışkan bitin gerekli olması düşük bir olasılıktır.

Aşağıdaki tabloda yapışkan bitin Data Lake Store’da nasıl çalıştığı gösterilmektedir.

| Kullanıcı grubu | Dosya | Klasör |
| --- | --- | --- |
| Yapışkan bit **Kapalı** |Etki yok |Etki yok |
| Yapışkan bit **Açık** |Etki yok |Bir alt öğenin **süper kullanıcıları** ve **sahip olan kullanıcısı** dışında herkesin alt öğeyi silmesini veya yeniden adlandırmasını önler. |

Yapışkan bit Azure Portal'da gösterilmez.

## Data Lake Store’daki ACL’lere ilişkin yaygın sorular
Data Lake Store’daki ACL’lerle ilgili olarak sık sorulan bazı sorular aşağıda verilmiştir.

### ACL desteğini etkinleştirmem gerekiyor mu?
Hayır. ACL’ler üzerinden erişim denetimi Data Lake Store hesabı için her zaman açıktır.

### Bir klasörü ve içindekileri yinelemeli olarak silmek için hangi izinler gereklidir?
* Üst klasör **Yazma + Yürütme** izinlerine sahip olmalıdır.
* Silinecek klasör ve içindeki her klasör **Okuma + Yazma + Yürütme** izinlerini gerektirir.
  >[AZURE.NOTE] Klasörlerdeki dosyaların silinmesi bu dosyalar üzerinde Yazma iznini gerektirmez. Ayrıca, "/" Kök klasörü **hiçbir zaman** silinemez.

### Bir dosyanın veya klasörün sahibi olarak kim ayarlanır?
Bir dosyayı veya klasörü oluşturan kişi bunların sahibi olur.

### Oluşturma sırasında bir dosyanın veya klasörün sahibi olan grup olarak kim ayarlanır?
Yeni dosya veya klasörün oluşturulduğu üst klasörün sahibi olan gruptan kopyalanır.

### Bir dosyanın sahibiyim, ancak gereken RWX izinlerine sahip değilim. Ne yapmalıyım?
Sahip olan kullanıcı kendisine gerekli olan her türlü RWX iznii vermek için dosyanın izinlerini değiştirebilir.

### Data Lake Store ACL’lerin devralınmasını destekler mi?
Hayır.

### Maske ile umask arasındaki fark nedir?
| maske | umask |
| --- | --- |
| **Maske** özelliği her dosya ve klasörde bulunur. |**Umask** ise Data Lake Store hesabının bir özelliğidir. Bu nedenle, Data Lake Store’de yalnızca tek bir umask vardır. |
| Bir dosya veya klasördeki maske özelliği, dosyanın sahibi olan kullanıcı veya grup ya da süper kullanıcı tarafından değiştirilebilir. |Umask özelliği ise süper kullanıcı dahil hiçbir kullanıcı tarafından değiştirilemez. Bu özellik, değiştirilemeyen sabit bir değerdir. |
| Maske özelliği bir kullanıcının dosya ya da klasör üzerinde işlem gerçekleştirme hakkına sahip olup olmadığını belirlemek üzere çalışma zamanındaki Erişim Denetimi algoritması sırasında kullanılır. Maskenin rolü, erişim denetimi sırasında "etkili izinleri" oluşturmaktır. |Umask, Erişim Denetimi sırasında hiç kullanılmaz. Umask bir klasörün yeni alt öğelerinin Erişim ACL’sini belirlemek için kullanılır. |
| Maske; erişim denetimi sırasında adlandırılmış kullanıcı, adlandırılmış grup ve sahip olan kullanıcı için geçerli olan 3 bitlik bir RWX değeridir. |Umask ise yeni bir alt öğenin sahip olan kullanıcı, sahip olan grup ve diğer kullanıcısı için geçerli olan 9 bitlik bir değerdir. |

### POSIX erişim denetimi modeli hakkında daha fazla bilgiyi nereden bulabilirim?
* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)
* [HDFS İzin Kılavuzu](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 
* [POSIX SSS](http://www.opengroup.org/austin/papers/posix_faq.html)
* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)
* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)
* [Linux üzerinde POSIX ACL’si](http://users.suse.com/~agruen/acl/linux-acls/online/)
* [Linux üzerinde Access Control kullanan ACL](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## Ayrıca bkz.
* [Azure Data Lake Store'a genel bakış](data-lake-store-overview.md)
* [Azure Data Lake Analytics ile Çalışmaya Başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

<!--HONumber=Sep16_HO3-->


