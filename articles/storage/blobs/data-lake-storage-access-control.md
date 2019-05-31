---
title: Azure Data Lake depolama Gen2'ye erişim denetimine genel bakış | Microsoft Docs
description: Azure Data Lake depolama Gen2 erişim denetiminin nasıl çalıştığını anlama
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 5adba958ed3bcb9efbf66c079b541e11ceed570c
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243602"
---
# <a name="access-control-in-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2'ye erişim denetimi

Azure Data Lake depolama 2. nesil hem Azure rol tabanlı erişim denetimi (RBAC) hem de benzer POSIX erişim denetim listeleri (ACL'ler) destekleyen bir erişim denetimi modeli kullanır. Bu makalede Data Lake depolama 2. nesil için erişim denetimi modelinin temel bilgileri özetlenmektedir.

<a id="azure-role-based-access-control-rbac" />

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

RBAC, izin kümelerini etkili bir şekilde uygulamak için rol atamalarını kullanan *güvenlik sorumluları*. A *güvenlik sorumlusu* bir kullanıcı, Grup, hizmet sorumlusu veya Azure Active Directory (Azure kaynaklarına erişimi isteyen AD) tanımlanan yönetilen kimlik temsil eden bir nesnedir.

Bu Azure kaynakları için en üst düzey kaynaklar genellikle, sınırlıdır (örneğin: Azure depolama hesapları için). Azure depolama ve Azure Data Lake depolama Gen2 söz konusu olduğunda sonuç olarak, bu mekanizma kapsayıcı (dosya sistemi) kaynağa genişletilmiştir.

Depolama hesabınız kapsamında güvenlik sorumlularının rollerini atama bilgi edinmek için [verilere Azure blob ve kuyruk RBAC ile Azure portalında erişim ver](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="the-impact-of-role-assignments-on-file-and-directory-level-access-control-lists"></a>Dosya ve dizin düzeyinde erişim denetim listeleri rol atamalarında etkisini

RBAC rolü atamalarını kullanarak erişim izinlerini denetlemek için güçlü bir mekanizma olsa da, bunu bir çok kaba ayrıntılı ACL'ler göre mekanizmadır. Dosya sistemi düzeyinde RBAC için en küçük ayrıntı düzeyi ise ve bu ACL daha yüksek bir önceliğe adresindeki değerlendirilir. Bu nedenle, bir dosya sistemi kapsam içinde bir güvenlik sorumlusu için bir rol atamanız durumunda güvenlik sorumlusunu ACL atamaları bağımsız olarak, dosya sistemindeki tüm dizinler ve dosyalar için bu rol ile ilişkili yetki düzeyi vardır.

Bir güvenlik sorumlusu RBAC aracılığıyla veri izinleri verildi ne zaman bir [yerleşik rol](https://docs.microsoft.com/azure/storage/common/storage-auth-aad?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#built-in-rbac-roles-for-blobs-and-queues), ya da özel bir rol bu izinleri önce yetkilendirme isteği değerlendirilir. Yetkilendirme hemen çözümlenmiş ve hiç ise güvenlik sorumlusunun RBAC atamaları tarafından istenen işlem yetkiliyse ACL denetimleri yapılır. Alternatif olarak, güvenlik sorumlusu RBAC atama yok ya da istenen işlem atanan izni eşleşmiyor, ardından ACL denetimlerini güvenlik sorumlusu istenen işlemi gerçekleştirmek için yetkili olup olmadığını belirlemek için gerçekleştirilir.

> [!NOTE]
> Depolama Blob verileri sahip yerleşik rol ataması güvenlik sorumlusu atanmış sonra güvenlik sorumlusu olarak kabul edilir bir *süper kullanıcı* ve ayarı dahil olmak üzere tüm mutating işlemleri için tam erişim izni dizinleri ve sahibi oldukları olmayan dosyaları için ACL'leri yanı sıra bir dizin veya dosya sahibini. Süper kullanıcı, bir kaynağın sahibini değiştirmek için yalnızca yetkili şekilde erişimdir.

## <a name="shared-key-and-shared-access-signature-sas-authentication"></a>Paylaşılan anahtar ve paylaşılan erişim imzası (SAS) kimlik doğrulaması

Azure Data Lake depolama Gen2'ye kimlik doğrulaması için paylaşılan anahtar ve SAS yöntemleri destekler. Bu kimlik doğrulama yöntemlerinin bir karakteristik kimliksiz arayanla ilişkili ise ve bu nedenle güvenlik sorumlusu izin tabanlı yetkilendirme gerçekleştirilemiyor ' dir.

Paylaşılan anahtar söz konusu olduğunda, çağırana etkili bir şekilde 'süper kullanıcı' erişim sahibi ayarlama ve ACL'ler değiştirme dahil olmak üzere tüm kaynaklar üzerindeki tüm işlemler için tam erişim anlamı kazanır.

SAS belirteçleri belirtecinin bir parçası izin verilen izinleri içerir. SAS belirteci dahil izinleri tüm yetkilendirme kararları için etkili bir şekilde uygulanır, ancak hiçbir ek ACL denetimler gerçekleştirilir.

## <a name="access-control-lists-on-files-and-directories"></a>Erişim denetim listelerini dosyalar ve dizinler

Dosyalar ve dizinler için erişim düzeyine sahip bir güvenlik sorumlusu ilişkilendirebilirsiniz. Bu ilişkilendirmeleri, yakalanan bir *erişim denetim listesi (ACL)* . Her dosya ve dizin depolama hesabınızda bir erişim denetim listesi vardır.

Depolama hesabı düzeyinde bir güvenlik sorumlusu bir rolü atandı, güvenlik sorumlusunu belirli dosyalara ve dizinlere erişim yükseltilmiş erişim denetim listelerini kullanın.

Erişim denetim listeleri, bir rol ataması tarafından verilen bir düzeyi daha düşük erişim düzeyini sağlamak için kullanamazsınız. Örneğin, atadığınız [depolama Blob verileri katkıda bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview) güvenlik sorumlusunu bir dizine yazmasını önlemek için erişim denetimi kullanamazsınız sonra asıl güvenlik rolüne listeler.

### <a name="set-file-and-directory-level-permissions-by-using-access-control-lists"></a>Erişim denetim listeleri kullanarak dosya ve dizin düzeyi izinleri ayarlama

Dosya ve dizin düzeyinde izinleri ayarlamak için aşağıdaki makalelerden herhangi birine bakın:

|Bu aracı kullanmak istiyorsanız:    |Bu makaleye bakın:    |
|--------|-----------|
|Azure Depolama Gezgini    |[Azure Data Lake depolama 2. nesil ile Azure Depolama Gezgini'ni kullanarak dosya ve dizin düzeyi izinleri ayarlayın](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)|
|REST API    |[Yol - güncelleştirme](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/path/update)|

> [!IMPORTANT]
> Güvenlik sorumlusu ise bir *hizmet* asıl, hizmet sorumlusu nesne kimliği ve nesne kimliği değil ilgili uygulama kaydını kullanmak önemlidir. Azure CLI açık hizmet sorumlusu nesne Kimliğini alın ve ardından bu komutu kullanın: `az ad sp show --id <Your App ID> --query objectId`. değiştirdiğinizden emin olun `<Your App ID>` yer tutucu uygulaması kaydınızı uygulama kimliği.

### <a name="types-of-access-control-lists"></a>Erişim denetimi listesi türleri

İki tür erişim denetim listeleri vardır: *ACL'leri erişim* ve *ACL'leri varsayılan*.

Erişim ACL'leri bir nesneye erişimi denetler. Dosyalar ve dizinler erişim ACL'leri vardır.

Varsayılan ACL'ler, bu dizin altında oluşturulan tüm alt öğelere ilişkin erişim ACL'lerini belirleyen bir dizinle ilişkili ACL'leri şablonlardır. Dosyaları varsayılan ACL'ye sahip değildir.

Hem erişim ACL'leri hem de varsayılan ACL'ler aynı yapıdadır.

> [!NOTE]
> Varsayılan değiştirme üst ACL erişim ACL'sini etkilemez veya varsayılan ACL'si alt öğelerinin zaten mevcut.

### <a name="levels-of-permission"></a>İzin düzeyleri

Bir dosya sistemi nesnesi üzerinde izinler **okuma**, **yazma**, ve **yürütme**, ve bunlar üzerinde dosya ve dizinleri aşağıdaki tabloda gösterildiği gibi kullanılabilir:

|            |    Dosya     |   Dizin |
|------------|-------------|----------|
| **Okuma (R)** | Bir dosyanın içeriğini okuyabilir | Gerektirir **okuma** ve **yürütme** dizinin içeriğini listelemek için |
| **Yazma (W)** | Bir dosyaya yazabilir veya ekleyebilir | Gerektirir **yazma** ve **yürütme** alt öğeler bir dizin oluşturmak için |
| **Yürütme (X)** | Data Lake depolama Gen2 bağlamında herhangi bir şey gelmez | Bir dizinin alt öğelerini geçirmek için gereklidir |

> [!NOTE]
> Yalnızca ACL'leri (hiçbir RBAC) kullanarak izinleri verdiğiniz sonra bir hizmet sorumlusu okuma veya yazma erişimi bir dosyaya vermek için hizmet sorumlusu vermek gerekecektir **yürütme** dosya sistemi ve her bir klasörü izinleri dosyayı neden klasör hiyerarşisi.

#### <a name="short-forms-for-permissions"></a>İzinlerin kısaltmaları

**RWX**, **Okuma + Yazma + Yürütme** için kullanılır. **Okuma=4**, **Yazma=2** ve **Yürütme=1** olup toplamları izinleri temsil eden daha da kısaltılmış bir sayısal biçim mevcuttur. Bazı örnekler aşağıda verilmiştir.

| Sayısal biçim | Kısa biçim |      Anlamı     |
|--------------|------------|------------------------|
| 7            | `RWX`        | Okuma + Yazma + Yürütme |
| 5            | `R-X`        | Okuma + Yürütme         |
| 4            | `R--`        | Okuma                   |
| 0            | `---`        | İzin yok         |

#### <a name="permissions-inheritance"></a>İzinleri devralmayı

Data Lake depolama Gen2 tarafından kullanılan POSIX stili modelinde bir öğenin izinleri öğenin kendisine depolanır. Diğer bir deyişle, alt öğesi zaten oluşturulduktan sonra izinleri ayarlandıysa bir öğenin izinleri üst öğelerinden devralınamaz. Üst öğeler alt öğelerini oluşturmadan önce varsayılan izinleri atanmışsa, izinler yalnızca devralınır.

### <a name="common-scenarios-related-to-permissions"></a>İzinlerle ilgili yaygın senaryolar

Aşağıdaki tabloda, bir depolama hesabı üzerinde belirli işlemlerin gerçekleştirilmesi için gereken izinleri anlamanıza yardımcı olacak bazı yaygın senaryolar listelenmektedir.

|    İşlem             |    /    | Oregon / | Portland / | Data.txt     |
|--------------------------|---------|----------|-----------|--------------|
| Data.txt okuyun            |   `--X`   |   `--X`    |  `--X`      | `R--`          |
| Data.txt için ekleme       |   `--X`   |   `--X`    |  `--X`      | `RW-`          |
| Data.txt Sil          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Data.txt oluşturma          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Liste /                   |   `R-X`   |   `---`    |  `---`      | `---`          |
| Liste /Oregon/           |   `--X`   |   `R-X`    |  `---`      | `---`          |
| Liste /Oregon/Portland /  |   `--X`   |   `--X`    |  `R-X`      | `---`          |

> [!NOTE]
> Yazma önceki iki koşul true olduğu sürece dosyasındaki izinleri, silmek için gerekli değildir.

### <a name="users-and-identities"></a>Kullanıcılar ve kimlikler

Her dosya ve dizin bu kimlikler için farklı izinlere sahiptir:

- Sahip olan kullanıcı
- Sahip olan grup
- Adlandırılmış kullanıcılar
- Adlandırılmış gruplar
- Adlandırılmış hizmet sorumluları
- Adlandırılmış bir yönetilen kimlik
- Diğer tüm kullanıcılar

Kullanıcıların ve grupların kimlikleri, Azure Active Directory (Azure AD) kimlikleridir. Aksi belirtilmediği sürece, böyle bir *kullanıcı*, Data Lake depolama Gen2 bağlamında başvurmak için bir Azure AD kullanıcısı, hizmet sorumlusu, yönetilen bir kimlik veya güvenlik grubu.

#### <a name="the-owning-user"></a>Sahip olan kullanıcı

Öğeyi oluşturan kullanıcı otomatik olarak öğenin sahibi olan kullanıcıdır. Sahip olan kullanıcı şunları yapabilir:

* Sahip olunan bir dosyanın izinlerini değiştirme.
* Sahip olan kullanıcı aynı zamanda hedef grubun bir üyesi oldukça, sahip olunan bir dosyanın sahibi olan grubunu değiştirme.

> [!NOTE]
> Sahip olan kullanıcı *olamaz* sahibi olan kullanıcıyı bir dosya veya dizin değiştirin. Bir dosya veya dizin sahibi olan kullanıcıyı yalnızca süper kullanıcılar değiştirebilirsiniz.

#### <a name="the-owning-group"></a>Sahip olan grup

POSIX ACL'lerinde her kullanıcı ile ilişkili bir *birincil grup*. Örneğin, "Gamze adlı" kullanıcı "Finans" grubuna ait olabilir. Gamze ayrıca birden fazla gruba ait, ancak bir grup her zaman kendi birincil grubu olarak atanır. POSIX’te Gamze bir dosya oluşturduğunda o dosyanın sahibi olan grup birincil grubu olarak ayarlanır (bu örnekte "finans" grubudur). Aksi takdirde sahip olan grup, diğer kullanıcılar/gruplar için atanan izinlere benzer şekilde davranır.

##### <a name="assigning-the-owning-group-for-a-new-file-or-directory"></a>Yeni dosya veya dizin sahip olan grup atama

* **Case 1**: Kök dizin "/". Bu dizin, bir Data Lake depolama 2. nesil dosya sistemini oluşturduğunuzda oluşturulur. Bu durumda sahip olan Grup bitti dedik, dosya sistemi oluşturan kullanıcıya ayarlanır OAuth kullanarak. Paylaşılan anahtar, bir hesap SAS veya bir hizmet SAS'ı kullanarak dosya sistemine oluşturulduktan sonra sahibi ve sahip olan Grup ayarlanır **$superuser**.
* **2. durum** (diğer her olay): Yeni bir öğe oluşturulduğunda sahip olan Grup üst dizininden kopyalanır.

##### <a name="changing-the-owning-group"></a>Sahip olan Grup değiştirme

Sahip olan grup aşağıdakiler tarafından değiştirilebilir:
* Herhangi bir süper kullanıcı.
* Sahip olan kullanıcı aynı zamanda hedef grubun üyesi ise sahip olan kullanıcı.

> [!NOTE]
> Sahip olan Grup, bir dosya veya dizin ACL'leri değiştiremezsiniz.  Kök dizin, söz konusu olduğunda hesabı oluşturan kullanıcıya sahip olan Grup ayarlanırken **vaka 1** yukarıda tek bir kullanıcı hesabı sahip grup üzerinden izin sağlamak için geçerli değildir. Uygunsa bu izni geçerli bir kullanıcı hesabına atayabilirsiniz.

### <a name="access-check-algorithm"></a>Erişim denetimi algoritması

Aşağıdaki sözde kod depolama hesapları için erişim denetimi algoritması temsil eder.

```
def access_check( user, desired_perms, path ) : 
  # access_check returns true if user has the desired permissions on the path, false otherwise
  # user is the identity that wants to perform an operation on path
  # desired_perms is a simple integer with values from 0 to 7 ( R=4, W=2, X=1). User desires these permissions
  # path is the file or directory
  # Note: the "sticky bit" isn't illustrated in this algorithm
  
# Handle super users.
  if (is_superuser(user)) :
    return True

# Handle the owning user. Note that mask isn't used.
entry = get_acl_entry( path, OWNER )
if (user == entry.identity)
    return ( (desired_perms & entry.permissions) == desired_perms )

# Handle the named users. Note that mask IS used.
entries = get_acl_entries( path, NAMED_USER )
for entry in entries:
    if (user == entry.identity ) :
        mask = get_mask( path )
        return ( (desired_perms & entry.permissions & mask) == desired_perms)

# Handle named groups and owning group
member_count = 0
perms = 0
entries = get_acl_entries( path, NAMED_GROUP | OWNING_GROUP )
for entry in entries:
if (user_is_member_of_group(user, entry.identity)) :
    member_count += 1
    perms | =  entry.permissions
if (member_count>0) :
return ((desired_perms & perms & mask ) == desired_perms)

# Handle other
perms = get_perms_for_other(path)
mask = get_mask( path )
return ( (desired_perms & perms & mask ) == desired_perms)
```

#### <a name="the-mask"></a>Maskesi

Maske, erişim denetimi algoritması'içinde gösterildiği gibi adlandırılmış kullanıcı, sahip olan Grup ve adlandırılmış gruplara erişim sınırlar.  

> [!NOTE]
> İçin yeni bir Data Lake depolama Gen2'ye dosya sistemi, 750 dizinler ve dosyalar için 640 erişim ("/") kök dizin ACL için maske varsayılan olarak. Yalnızca depolama sisteminde dosyaları alakasız olduğu gibi dosyaları X bit almazsınız.
>
> Maske, çağrı başına temelinde belirtilebilir. Bu kümeler, kendi dosya işlemleri için farklı etkili maskeleri olması gibi farklı alıcı sistemleri sağlar. Tamamen maske belirli bir istek üzerinde belirtilmezse, varsayılan maskesi geçersiz kılar.

#### <a name="the-sticky-bit"></a>Yapışkan bit

Yapışkan bit POSIX dosya sisteminin daha gelişmiş bir özelliktir. Data Lake depolama Gen2 bağlamında Yapışkan bitin gerekli olması düşüktür. Özet olarak, bir dizin, Yapışkan bitin etkinse, bir alt öğesi yalnızca silinebilir veya alt öğenin sahip olan kullanıcı tarafından yeniden adlandırıldı.

Yapışkan bit Azure portalında gösterilmiyor.

### <a name="default-permissions-on-new-files-and-directories"></a>Varsayılan izinler yeni dosyalar ve dizinler

Mevcut bir dizini altında yeni bir dosya veya dizin oluşturulduğunda varsayılan üst dizininde ACL belirler:

- Bir alt dizinin varsayılan ACL'si ve erişim ACL'si.
- Bir alt dosyanın erişim ACL'si (dosyaları varsayılan ACL'nin gerekmez).

#### <a name="umask"></a>umask

Bir dosya veya dizin oluştururken umask varsayılan ACL'ler alt öğede nasıl ayarlanacağını değiştirmek için kullanılır. umask olan 9 bitlik bir değer içeren bir RWX değeri için üst dizinlerde **sahip olan kullanıcı**, **sahip olan grup**, ve **diğer**.

Azure Data Lake depolama Gen2'ye bir sabit değeri için umask 007 için ayarlayın. İçin bu değeri çevirir:

| umask bileşeni     | Sayısal biçim | Kısa biçim | Anlamı |
|---------------------|--------------|------------|---------|
| umask.owning_user   |    0         |   `---`      | Sahip olan kullanıcı için üst öğenin varsayılan ACL'si alt öğenin erişim ACL'si kopyalayın | 
| umask.owning_group  |    0         |   `---`      | Sahip olan Grup üst öğenin varsayılan ACL'si alt öğenin erişim ACL'si kopyalayın | 
| umask.Other         |    7         |   `RWX`      | Diğer için alt öğenin erişim ACL'si üzerindeki tüm izinleri Kaldır |

Umask değerin etkili bir şekilde Azure Data Lake depolama Gen2 tarafından kullanılan değeri anlamına **diğer** varsayılan ACL belirten varsayılan bağımsız olarak yeni alt öğe olarak hiçbir zaman iletilmez. 

Aşağıdaki sözde kod umask bir alt öğesi ACL'leri oluştururken nasıl uygulanacağını gösterir.

```
def set_default_acls_for_new_child(parent, child):
    child.acls = []
    for entry in parent.acls :
        new_entry = None
        if (entry.type == OWNING_USER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_user))
        elif (entry.type == OWNING_GROUP) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_group))
        elif (entry.type == OTHER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.other))
        else :
            new_entry = entry.clone(perms = entry.perms )
        child_acls.add( new_entry )
```

## <a name="common-questions-about-acls-in-data-lake-storage-gen2"></a>Data Lake depolama Gen2 ACL'ler hakkında sık sorulan sorular

### <a name="do-i-have-to-enable-support-for-acls"></a>ACL desteğini etkinleştirmem gerekiyor mu?

Hayır. ACL'ler üzerinden erişim denetimi, hiyerarşik Namespace (özellik HNS) üzerinde etkin olduğu sürece bir depolama hesabı için etkinleştirilir.

HNS kapalı, Azure RBAC yetkilendirme kuralları hala açıksa uygulayın.

### <a name="what-is-the-best-way-to-apply-acls"></a>ACL uygulamak için en iyi yolu nedir?

ACL'ler atanan sorumlu olarak her zaman Azure AD güvenlik grupları kullanın. Doğrudan bireysel kullanıcıları veya hizmet sorumlusu atamak için bir fırsat kaçının. Bu yapıyı kullanarak, kullanıcı veya hizmet sorumluları için tüm dizin yapısının ACL'leri yeniden gerek kalmadan ekleyip olanak tanıyacaktır. ) Bunun yerine, yalnızca eklemek veya bunları uygun kaldırmak ihtiyacınız Azure AD güvenlik grubu. ACL'ler devralınmaz ve her dosya ve alt noktasındaki ACL güncelleniyor gerekir böylece ACL'leri yeniden uygulama aklınızda bulundurun. 

### <a name="which-permissions-are-required-to-recursively-delete-a-directory-and-its-contents"></a>Ve içindekileri yinelemeli olarak silmek bir dizin için hangi izinler gereklidir?

- Çağıranın 'süper kullanıcı' izinlerine sahip,

Or

- Üst dizine yazma + yürütme izinleri gerekir.
- Silinecek dizin ve her bir dizininde okuma + yazma + yürütme izinleri gerektirir.

> [!NOTE]
> Dizinlerde dosyaları silmek için yazma izni yok. Ayrıca, kök dizin "/" hiçbir zaman silinebilir.

### <a name="who-is-the-owner-of-a-file-or-directory"></a>Bir dosya veya dizin sahibi kimdir?

Bir dosya veya dizin oluşturucusu sahibi olur. Kök dizin söz konusu olduğunda, bu dosya sistemi oluşturan kullanıcı kimliğidir.

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-directory-at-creation"></a>Hangi Grup bir dosya veya dizine sahip olan grup oluşturma sırasında ayarlanır?

Sahip olan Grup, yeni bir dosya veya dizin oluşturulduğu üst dizine sahip olan gruptan kopyalanır.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Bir dosyanın sahibiyim, ancak gereken RWX izinlerine sahip değilim. Ne yapmalıyım?

Sahip olan kullanıcı kendisine gerekli olan her türlü RWX iznini vermek için dosyanın izinlerini değiştirebilir.

### <a name="why-do-i-sometimes-see-guids-in-acls"></a>Neden bazen ACL'lerinde GUID'leri görüyorum?

Giriş bir kullanıcıyı temsil eder ve bu kullanıcı artık Azure AD'de mevcut değilse bir GUID gösterilir. Bu genellikle, kullanıcı şirketten ayrıldığında veya Azure AD’de kullanıcının hesabı silindiğinde gerçekleşir. Ayrıca, hizmet sorumluları ve güvenlik grupları bir kullanıcı asıl adı (bunları tanımlamak için UPN) sahip değildir ve bu nedenle bunlar OID özniteliği (GUID) tarafından temsil edilir.

### <a name="how-do-i-set-acls-correctly-for-a-service-principal"></a>Nasıl ACL'leri doğru için bir hizmet sorumlusu ayarlayabilirim?

Hizmet sorumluları için ACL'leri tanımladığınızda, nesne kimliği (OID) kullanın önemli olduğu *hizmet sorumlusu* oluşturduğunuz uygulama kaydı için. Kayıtlı uygulama içinde belirli bir ayrı bir hizmet sorumlusu olduğunu unutmamak önemlidir Azure AD kiracısı. Kayıtlı uygulamalar Azure portalda bir OID sahiptir ancak *hizmet sorumlusu* başka bir (farklı) OID sahiptir.

Bir uygulama kaydı için karşılık gelen hizmet sorumlusu OID almak için kullanabileceğiniz `az ad sp show` komutu. Uygulama kimliği, parametre olarak belirtin. İşte bir örnek uygulama kimliği ile bir uygulama kaydı karşılık gelen hizmet sorumlusu için OID edinme 18218b12-1895-43e9-ad80-6e8fc1ea88ce =. Azure CLI içinde aşağıdaki komutu çalıştırın:

`az ad sp show --id 18218b12-1895-43e9-ad80-6e8fc1ea88ce --query objectId
<<OID will be displayed>>`

Hizmet sorumlusu için doğru OID varsa, depolama Gezgini'ne gidin **erişimini yönetme** sayfasına OID ekleyin ve OID için uygun izinleri atayın. Seçtiğinizden emin olun **Kaydet**.

### <a name="does-data-lake-storage-gen2-support-inheritance-of-acls"></a>Data Lake depolama Gen2 ACL'lerin devralınmasını destekler mi?

Azure RBAC atamaları devralır. Abonelik, kaynak grubu ve depolama hesabı kaynaklarına dosya sistemi kaynak aşağı akış atamaları.

ACL'ler devralmaz. Ancak, alt alt ve üst dizini altında oluşturulan dosyaları için ACL'leri ayarlamak için varsayılan ACL'ler kullanılabilir. 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>POSIX erişim denetimi modeli hakkında daha fazla bilgiyi nereden bulabilirim?

* [Linux üzerinde POSIX Erişim Denetim Listeleri](https://www.linux.com/news/posix-acls-linux)
* [HDFS izin kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)
* [POSIX SSS](https://www.opengroup.org/austin/papers/posix_faq.html)
* [POSIX 1003.1 2008](https://standards.ieee.org/findstds/standard/1003.1-2008.html)
* [POSIX 1003.1 2013](https://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)
* [POSIX 1003.1 2016](https://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)
* [Ubuntu üzerinde POSIX ACL](https://help.ubuntu.com/community/FilePermissionsACLs)
* [Linux üzerinde erişim denetim listelerini kullanan ACL](https://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Data Lake depolama Gen2'ye genel bakış](../blobs/data-lake-storage-introduction.md)
