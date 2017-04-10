---
title: "Data Lake Store’da erişim denetimine genel bakış | Microsoft Belgeleri"
description: "Azure Data Lake Store’da erişim denetiminin çalışma şekli hakkında bilgi edinin"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: 7533fe3758860111ae6c26630effedd673734b63
ms.lasthandoff: 04/04/2017


---
# <a name="access-control-in-azure-data-lake-store"></a>Azure Data Lake Store’da erişim denetimi

Azure Data Lake Store; HDFS ve sonuç olarak POSIX erişim denetimi modelinden türetilen bir erişim denetimi modeli kullanır. Bu makalede Data Lake Store için erişim denetimi modelinin temel bilgileri özetlenmektedir. HDFS erişim denetimi modeli hakkında daha fazla bilgi için bkz. [HDFS İzinleri Kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Dosyalar ve klasörler üzerindeki erişim denetimi listeleri

İki tür erişim denetim listesi (ACL) vardır: **Erişim ACL’leri** ve **Varsayılan ACL’ler**.

* **Erişim ACL’leri**: Bunlar bir nesneye erişimi denetler. Hem dosyalar hem de klasörler Erişim ACL’lerine sahiptir.

* **Varsayılan ACL’ler**: Bir klasör ile ilişkili olan ACL’lerin o klasör altında oluşturulan tüm alt öğelere ilişkin Erişim ACL’lerini belirleyen bir "şablonudur". Dosyalar Varsayılan ACL’ye sahip değildir.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Hem Erişim ACL'leri hem de Varsayılan ACL'ler aynı yapıdadır.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> Bir üst öğe üzerindeki Varsayılan ACL’nin değiştirilmesi zaten var olan alt öğelerin Erişim ACL’sini veya Varsayılan ACL’sini etkilemez.
>
>

## <a name="users-and-identities"></a>Kullanıcılar ve kimlikler

Her dosya ve klasör bu kimlikler için farklı izinlere sahiptir:

* Dosyanın sahibi olan kullanıcı
* Sahip olan grup
* Adlandırılmış kullanıcılar
* Adlandırılmış gruplar
* Diğer tüm kullanıcılar

Kullanıcıların ve grupların kimlikleri, Azure Active Directory (Azure AD) kimlikleridir. Bu nedenle, aksi belirtilmediği sürece, Data Lake Store bağlamında "Kullanıcı", Azure AD kullanıcısı veya Azure AD güvenlik grubu olabilir.

## <a name="permissions"></a>İzinler

Dosya sistemi nesnesi üzerinde **Okuma**, **Yazma** ve **Yürütme** izinleri bulunur ve bunlar aşağıdaki tabloda gösterildiği gibi dosyalar ve klasörler üzerinde kullanılabilir:

|            |    Dosya     |   Klasör |
|------------|-------------|----------|
| **Okuma (R)** | Bir dosyanın içeriğini okuyabilir | Klasörün içeriğini listelemek için **Okuma** ve **Yürütme** izinlerini gerektirir|
| **Yazma (W)** | Bir dosyaya yazabilir veya ekleyebilir | Bir klasörde alt öğeler oluşturmak için **Yazma** ve **Yürütme** gerektirir |
| **Yürütme (X)** | Data Lake Store bağlamında herhangi bir anlamı yoktur | Bir klasörün alt öğelerini geçirmek için gereklidir |

### <a name="short-forms-for-permissions"></a>İzinlerin kısaltmaları

**RWX**, **Okuma + Yazma + Yürütme** için kullanılır. **Okuma=4**, **Yazma=2** ve **Yürütme=1** olup toplamları izinleri temsil eden daha da kısaltılmış bir sayısal biçim mevcuttur. Bazı örnekler aşağıda verilmiştir.

| Sayısal biçim | Kısa biçim |      Anlamı     |
|--------------|------------|------------------------|
| 7            | RWX        | Okuma + Yazma + Yürütme |
| 5            | R-X        | Okuma + Yürütme         |
| 4            | R--        | Okuma                   |
| 0            | ---        | İzin yok         |


### <a name="permissions-do-not-inherit"></a>İzinler devralınmaz

Data Lake Store tarafından kullanılan POSIX stili modelinde bir öğenin izinleri öğenin kendisine depolanır. Diğer bir deyişle, bir öğenin izinleri üst öğelerinden devralınamaz.

## <a name="common-scenarios-related-to-permissions"></a>İzinlerle ilgili yaygın senaryolar

Bir Data Lake Store hesabı üzerinde belirli işlemlerin gerçekleştirilmesi için gereken izinleri anlamanıza yardımcı olacak bazı yaygın senaryolar aşağıda verilmiştir.

### <a name="permissions-needed-to-read-a-file"></a>Bir dosyayı okumak için gereken izinler

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Dosyanın okunması için çağıranın **Okuma** izinlerine sahip olması gerekir.
* Dosyayı içeren klasör yapısındaki tüm klasörler için çağıranın **Yürütme** izinlerine sahip olması gerekir.

### <a name="permissions-needed-to-append-to-a-file"></a>Bir dosyaya eklemek için gereken izinler

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Eklemenin yapılacağı dosya için çağıranın **Yazma** izinlerine sahip olması gerekir.
* Dosyayı içeren tüm klasörler için çağıranın **Yürütme** izinlerine sahip olması gerekir.

### <a name="permissions-needed-to-delete-a-file"></a>Bir dosyayı silmek için gereken izinler

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Üst klasör için çağıranın **Yazma + Yürütme** izinlerine sahip olması gerekir.
* Dosyanın yolundaki diğer tüm klasörler için çağıranın **Yürütme** izinlerine sahip olması gerekir.



> [!NOTE]
> Önceki iki koşul geçerli oldukça dosyayı silmek için dosya üzerinde yazma izinleri gerekli değildir.
>
>

### <a name="permissions-needed-to-enumerate-a-folder"></a>Bir klasörü listelemek için gereken izinler

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Listelenecek klasör için çağıranın **Okuma + Yürütme** izinlerine sahip olması gerekir.
* Tüm üst klasörler için çağıranın **Yürütme** izinlerine sahip olması gerekir.

## <a name="viewing-permissions-in-the-azure-portal"></a>Azure portalında görüntüleme izinleri

Data Lake Store hesabının **Veri Gezgini** dikey penceresinde **Erişim**’e tıklayarak bir dosya veya klasörün ACL’lerini görebilirsiniz. **mydatastore** hesabı altındaki **catalog** klasörüne ilişkin ACL’leri görmek için **Erişim**’e tıklayın.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Bu dikey pencerenin üst tarafında sahip olduğunuz izinlerin özeti gösterilir. (Ekran görüntüsünde Bob kullanıcıdır.) Bunun altında erişim izinleri gösterilir. Bundan sonra **Erişim** dikey penceresinde **Basit Görünüm**’e tıklayarak daha basit bir görünüm görün.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Varsayılan ACL’ler, maske ve süper kullanıcı kavramlarının gösterildiği daha gelişmiş görünümü görmek için **Gelişmiş Görünüm**’e tıklayın.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>Süper kullanıcı

Süper kullanıcı, Data Lake Store’daki tüm kullanıcılar arasında en fazla hakka sahiptir. Süper kullanıcı:

* **Tüm** dosya ve klasörlerde RWX İzinlerine sahiptir.
* Herhangi bir dosya veya klasörün izinlerini değiştirebilir.
* Herhangi bir dosya veya klasörün sahibi olan kullanıcıyı ya da grubu değiştirebilir.

Azure’da bir Data Lake Store hesabının birkaç Azure rolü vardır, bunlar:

* Sahipler
* Katkıda Bulunanlar
* Okuyucular

Bir Data Lake Store hesabında **Sahipler** rolündeki herkes otomatik olarak o hesabın süper kullanıcısıdır. Daha fazla bilgi için bkz. [Rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).
Süper kullanıcı izinlerine sahip özel bir rol tabanlı erişim denetimi (RBAC) rolü oluşturmak isterseniz şu izinleri vermeniz gerekir:
- Microsoft.DataLakeStore/accounts/Superuser/action
- Microsoft.Authorization/roleAssignments/write


## <a name="the-owning-user"></a>Sahip olan kullanıcı

Öğeyi oluşturan kullanıcı otomatik olarak öğenin sahibi olan kullanıcıdır. Sahip olan kullanıcı şunları yapabilir:

* Sahip olunan bir dosyanın izinlerini değiştirme.
* Sahip olan kullanıcı aynı zamanda hedef grubun bir üyesi oldukça, sahip olunan bir dosyanın sahibi olan grubunu değiştirme.

> [!NOTE]
> Sahip olan kullanıcı başka bir sahibi olunan dosyanın sahibini *değiştiremez*. Bir dosya veya klasörün sahibi olan kullanıcıyı yalnızca süper kullanıcılar değiştirebilir.
>
>

## <a name="the-owning-group"></a>Sahip olan grup

POSIX ACL’lerinde her kullanıcı bir "birincil grup" ile ilişkilendirilir. Örneğin, "gamze" adlı kullanıcı "finans" grubuna ait olabilir. Gamze ayrıca birden fazla gruba ait olabilir, ancak bir grup her zaman birincil grubu olarak atanır. POSIX’te Gamze bir dosya oluşturduğunda o dosyanın sahibi olan grup birincil grubu olarak ayarlanır (bu örnekte "finans" grubudur).

Yeni bir dosya sistemi öğesi oluşturulduğunda, Data Lake Store sahip olan gruba bir değer atar.

* **Olay 1**: Kök klasör "/". Bir Data Lake Store hesabı oluşturulduğunda bu klasör oluşturulur. Bu durumda sahip olan grup, hesabı oluşturan kullanıcıya ayarlanır.
* **Olay 2** (Diğer her olay): Yeni bir olay oluşturulduğunda sahip olan grup üst klasörden kopyalanır.

Sahip olan grup aşağıdakiler tarafından değiştirilebilir:
* Herhangi bir süper kullanıcı.
* Sahip olan kullanıcı aynı zamanda hedef grubun üyesi ise sahip olan kullanıcı.

## <a name="access-check-algorithm"></a>Erişim denetimi algoritması

Aşağıdaki çizimde Data Lake Store hesaplarına yönelik erişim denetimi algoritması gösterilmektedir.

![Data Lake Store ACL’leri algoritması](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Maske ve "etkili izinler"

**Maske**, erişim denetimi algoritmasını gerçekleştirirken **adlandırılmış kullanıcılar**, **sahip olan grup** ve **adlandırılmış gruplar** için erişimi sınırlandırmak üzere kullanılan bir RWX değeridir. Maskeye ilişkin anahtar kavramlar aşağıda verilmiştir.

* Maske, "etkili izinleri" oluşturur. Diğer bir deyişle, erişim denetimi zamanında izinleri değiştirir.
* Maske doğrudan dosya sahibi ve herhangi bir süper kullanıcı tarafından düzenlenebilir.
* Maske etkili izin oluşturmaya yönelik izinleri kaldırabilir. Maske etkili izne izinler *ekleyemez*.

Bazı örneklere bakalım. Aşağıdaki örnekte maske **RWX** olarak ayarlanmıştır. Diğer bir deyişle maske herhangi bir izni kaldırmaz. Adlandırılmış kullanıcı, sahip olan kullanıcı ve adlandırılmış grup, erişim denetimi sırasında değiştirilmez.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

Aşağıdaki örnekte maske **R-X** olarak ayarlanmıştır. Bu nedenle, erişim denetimi sırasında **adlandırılmış kullanıcı**, **sahip olan grup** ve **adlandırılmış grup** için **Yazma izinlerini kapatır**.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Başvuru için bir dosyanın veya klasörün maskesinin Azure portalında nerede göründüğü aşağıda gösterilmiştir.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> Yeni bir Data Lake Store hesabı için kök klasörün ("/") Erişim ACL’si ve Varsayılan ACL’si için maske varsayılan olarak RWX’tir.
>
>

## <a name="permissions-on-new-files-and-folders"></a>Yeni dosyalar ve klasörler üzerindeki izinler

Var olan bir klasör altında yeni bir dosya ya da klasör oluşturulduğunda üst klasördeki Varsayılan ACL aşağıdakileri belirler:

- Bir alt klasörün Varsayılan ACL’si ve Erişim ACL’si.
- Bir alt dosyanın Erişim ACL’si (dosyaları Varsayılan ACL’ye sahip değildir).

### <a name="the-access-acl-of-a-child-file-or-folder"></a>Bir alt dosya veya klasörün Erişim ACL’si

Bir alt dosya veya klasör oluşturulduğunda, üst öğenin Varsayılan ACL’si alt dosya veya klasörün Erişim ACL’si olarak kopyalanır. Ayrıca, **diğer** kullanıcının üst klasör varsayılan ACL’sinde RWX izinleri varsa alt öğenin Erişim ACL’sinden kaldırılır.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

Çoğu senaryoda yukarıdaki bilgiler bir alt öğenin Erişim ACL’sinin belirlenmesi için yeterlidir. Ancak, POSIX sistemlerini biliyor ve bu bilgilerin nasıl elde edildiğini derinlemesine öğrenmek istiyorsanız bu makalenin sonraki bölümlerinde bulunan [Yeni dosyalar ve klasörler için Erişim ACL’lerini oluşturmada Umask rolü](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) kısmına bakın.


### <a name="a-child-folders-default-acl"></a>Bir alt klasörün Varsayılan ACL’si

Üst klasör altında bir alt klasör oluşturulduğunda üst klasörün Varsayılan ACL’si olduğu gibi alt klasörün Varsayılan ACL’sine kopyalanır.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Data Lake Store’da ACL’leri anlamaya yönelik gelişmiş konular

Data Lake Store dosyaları veya klasörleri için ACL’lerin nasıl belirlendiğini anlamanıza yardımcı olan birkaç gelişmiş konu aşağıda verilmiştir.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>Yeni dosyalar ve klasörler için Erişim ACL’lerini oluşturmada Umask rolü

POSIX ile uyumlu bir sistemde genel kavram umask’in yeni bir alt klasör veya klasörün Erişim ACL’si üzerinde **sahip olan kullanıcı**, **sahip olan grup** ve **diğer** iznini dönüştürmek için kullanılan üst klasördeki 9 bitlik bir değer olmasıdır. Bir umask’in bit değerleri alt öğenin Erişim ACL’sinde hangi bitlerin kapatılacağını belirler. Bu nedenle **sahip olan kullanıcı**, **sahip olan grup** ve **diğer** için izinlerin yayılmasını seçici olarak önlemek üzere kullanılır.

Bir HDFS sisteminde umask genellikle yöneticiler tarafından denetlenen site genelindeki bir yapılandırma seçeneğidir. Data Lake Store değiştirilemeyen bir **hesap genelinde umask** kullanır. Aşağıdaki tabloda Data Lake Store için umask gösterilmektedir.

| Kullanıcı grubu  | Ayar | Yeni alt öğenin Erişim ACL’si üzerindeki etkisi |
|------------ |---------|---------------------------------------|
| Sahip olan kullanıcı | ---     | Etki yok                             |
| Sahip olan grup| ---     | Etki yok                             |
| Diğer       | RWX     | Okuma + Yazma + Yürütme iznini kaldırma         |

Aşağıdaki çizimde bu umask eylemi gösterilmektedir. Net etki **diğer** kullanıcı için **Okuma + Yazma + Yürütme** izninin kaldırılmasıdır. Umask **sahip olan kullanıcı** ve **sahip olan grup** için bitleri belirtmediğinden bu izinler dönüştürülmez.

![Data Lake Store ACL’leri](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="the-sticky-bit"></a>Yapışkan bit

Yapışkan bit POSIX dosya sisteminin daha gelişmiş bir özelliğidir. Data Lake Store bağlamında yapışkan bitin gerekli olması düşük bir olasılıktır.

Aşağıdaki tabloda yapışkan bitin Data Lake Store’da nasıl çalıştığı gösterilmektedir.

| Kullanıcı grubu         | Dosya    | Klasör |
|--------------------|---------|-------------------------|
| Yapışkan bit **Kapalı** | Etki yok   | Etki yok.           |
| Yapışkan bit **Açık**  | Etki yok   | Bir alt öğenin **süper kullanıcıları** ve **sahip olan kullanıcısı** dışında herkesin alt öğeyi silmesini veya yeniden adlandırmasını önler.               |

Yapışkan bit Azure portalında gösterilmez.

## <a name="common-questions-about-acls-in-data-lake-store"></a>Data Lake Store’daki ACL’ler hakkında sık sorulan sorular

Data Lake Store’daki ACL’lerle ilgili olarak sık sorulan bazı sorular aşağıda verilmiştir.

### <a name="do-i-have-to-enable-support-for-acls"></a>ACL desteğini etkinleştirmem gerekiyor mu?

Hayır. ACL’ler üzerinden erişim denetimi Data Lake Store hesabı için her zaman açıktır.

### <a name="which-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Bir klasörü ve içindekileri yinelemeli olarak silmek için hangi izinler gereklidir?

* Üst klasör **Yazma + Yürütme** izinlerine sahip olmalıdır.
* Silinecek klasör ve içindeki her klasör **Okuma + Yazma + Yürütme** izinlerini gerektirir.

> [!NOTE]
> Klasörlerdeki dosyaları silmek için Yazma izni gerekmez. Ayrıca, "/" kök klasör **hiçbir zaman** silinemez.
>
>

### <a name="who-is-the-owner-of-a-file-or-folder"></a>Bir dosyanın veya klasörün sahibi kimdir?

Bir dosyayı veya klasörü oluşturan kişi bunların sahibi olur.

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Oluşturma sırasında bir dosyanın veya klasörün sahibi olan grubu olarak hangi grup ayarlanır?

Sahip olan grup, yeni dosya veya klasörün oluşturulduğu üst klasörün sahibi olan gruptan kopyalanır.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Bir dosyanın sahibiyim, ancak gereken RWX izinlerine sahip değilim. Ne yapmalıyım?

Sahip olan kullanıcı kendisine gerekli olan her türlü RWX iznini vermek için dosyanın izinlerini değiştirebilir.

### <a name="when-i-look-at-acls-in-the-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a>Azure portalında ACL’lere baktığımda kullanıcı adlarını görüyorum, ancak API’lere baktığımda GUID’leri görüyorum, bunun nedeni nedir?

ACL’lerdeki girişler, Azure AD’de kullanıcılara karşılık gelen GUID’ler olarak depolanır. API’ler GUID’leri olduğu gibi döndürür. Azure portalı mümkün olduğunda GUID’leri kolay adlara çevirerek ACL’lerin daha kolay kullanılmasını sağlamaya çalışır.

### <a name="why-do-i-sometimes-see-guids-in-the-acls-when-im-using-the-azure-portal"></a>Azure portalını kullanırken neden bazen ACL’lerde GUID’leri görüyorum?

Kullanıcı artık Azure AD’de mevcut değilse bir GUID gösterilir. Bu genellikle, kullanıcı şirketten ayrıldığında veya Azure AD’de kullanıcının hesabı silindiğinde gerçekleşir.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Data Lake Store ACL’lerin devralınmasını destekler mi?

Hayır.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Maske ile umask arasındaki fark nedir?

| maske | umask|
|------|------|
| **Maske** özelliği her dosya ve klasörde bulunur. | **Umask** ise Data Lake Store hesabının bir özelliğidir. Bu nedenle, Data Lake Store’de yalnızca tek bir umask vardır.    |
| Bir dosya veya klasördeki maske özelliği, dosyanın sahibi olan kullanıcı veya grup ya da süper kullanıcı tarafından değiştirilebilir. | Umask özelliği ise süper kullanıcı dahil hiçbir kullanıcı tarafından değiştirilemez. Bu özellik, değiştirilemeyen sabit bir değerdir.|
| Maske özelliği bir kullanıcının dosya ya da klasör üzerinde işlem gerçekleştirme hakkına sahip olup olmadığını belirlemek üzere çalışma zamanındaki erişim denetimi algoritması sırasında kullanılır. Maskenin rolü, erişim denetimi sırasında "etkili izinleri" oluşturmaktır. | Umask, erişim denetimi sırasında hiç kullanılmaz. Umask bir klasörün yeni alt öğelerinin Erişim ACL’sini belirlemek için kullanılır. |
| Maske; erişim denetimi sırasında adlandırılmış kullanıcı, adlandırılmış grup ve sahip olan kullanıcı için geçerli olan 3 bitlik bir RWX değeridir.| Umask ise yeni bir alt öğenin sahip olan kullanıcı, sahip olan grup ve **diğer** kullanıcısı için geçerli olan 9 bitlik bir değerdir.|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>POSIX erişim denetimi modeli hakkında daha fazla bilgiyi nereden bulabilirim?

* [Linux üzerinde POSIX Erişim Denetim Listeleri](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [HDFS izin kılavuzu](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [POSIX SSS](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1 2013](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [POSIX 1003.1 2016](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [Ubuntu üzerinde POSIX ACL](https://help.ubuntu.com/community/FilePermissionsACLs)

* [Linux üzerinde erişim denetim listelerini kullanan ACL](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)

