---
title: Azure veri Kataloğu'na erişim güvenliğini sağlama
description: Bu makalede veri kataloğu ve veri varlıklarını güvenliğinin nasıl sağlanacağını açıklar.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 6c09b509399647f4cacbc96427200da5a1b00ac9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61000788"
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a>Veri kataloğu ve veri varlıklarına erişim güvenliğini sağlama
> [!IMPORTANT]
> Bu özellik yalnızca Azure veri Kataloğu'nun standard Edition'da kullanılabilir.

Azure veri Kataloğu, veri Kataloğu kimlerin erişebileceğini ve ne gibi işlemler belirtmenize olanak sağlar (kaydedin, açıklama, sahipliğini) meta verileri katalogdaki gerçekleştirebilirsiniz. 

## <a name="catalog-users-and-permissions"></a>Katalog kullanıcıları ve izinleri
Bir kullanıcıyı veya grubu, bir veri Kataloğu'na erişim verin ve izinlerini ayarlamak için:

1. Üzerinde [giriş sayfası, veri Kataloğu'nun](https://www.azuredatacatalog.com), tıklayın **ayarları** araç.

    ![Veri Kataloğu - ayarlar](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. Ayarları sayfasında genişletin **katalog kullanıcıları** bölümü.
    ![Kullanıcılar - katalog Ekle](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)
3. **Ekle**'ye tıklayın.
4. Tam girin **kullanıcı adı** veya adını **güvenlik grubu** , Azure Active Directory (Kataloğu ile ilişkili AAD). Virgül kullanın (', ') ayırıcı ekliyorsanız birden fazla bir kullanıcı veya grup olarak.
    ![Katalog kullanıcıları - kullanıcılar veya gruplar](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)
5. Tuşuna **ENTER** veya **sekmesini** metin kutusunun dışında. 
6.  Onaylayın tüm izinleri (**ek açıklama Ekle**, **kaydetme**, ve **Sahipliği Al**) varsayılan olarak, bu kullanıcılara veya gruplara atanır. Diğer bir deyişle, kullanıcı veya grup için [veri varlıklarını kaydetme]( data-catalog-how-to-register.md), [veri varlıklarına açıklama]( data-catalog-how-to-annotate.md), ve [veri varlıklarının sahipliğini]( data-catalog-how-to-manage.md). 
    ![Katalog kullanıcıları - varsayılan izinler](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)
7.  Bir kullanıcı veya grup kataloğa yalnızca okuma erişimi vermek için Temizle **açıklama** kullanıcı veya grup için seçeneği. Bunu yaptığınızda, kullanıcı veya grup katalogdaki veri varlıklarına Not ekleme yapamazsınız, ancak bunları görüntüleyebilirsiniz. 
8.  Bir kullanıcı veya grup veri varlıklarını kaydetmesini engellemek için Temizle **kaydetme** kullanıcı veya grup için seçeneği.
9.  Bir kullanıcının bir veri varlığına sahipliğini reddetmek için Temizle **sahipliğini** kullanıcı veya grup için seçeneği. 
10. Bir kullanıcı/Grup katalog kullanıcıları silmek için tıklayın **x** listenin altındaki kullanıcı/grup için. 
    ![Kullanıcıların katalog - kullanıcı silme](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

    > [!IMPORTANT]
    > Güvenlik grupları ekleme kullanıcılar yerine kullanıcıların doğrudan katalog ve izinleri atamak için eklemenizi öneririz. Daha sonra kullanıcılar, rollerine ve gerekli kataloğa erişimlerinin eşleşen güvenlik gruplarını ekleyin.

## <a name="special-considerations"></a>Özel durumlar

- Güvenlik gruplarına atanan izinler eklenebilir. Örneğin, bir kullanıcı iki grupta ' dir. Bir grup izinleri ek açıklama ve başka bir grup değil sahip açıklama izinleri. Ardından, kullanıcı sahip açıklama izinleri. 
- Açıkça bir kullanıcıya atanmış izinler, kullanıcının ait olduğu gruba atanan izinleri geçersiz kılar. Önceki örnekte, örneğin, kullanıcıların katalog ve kullanıcı eklenmezse değil atama izinleri ek açıklama. Kullanıcı, kullanıcı izinlerine sahip bir grubun bir üyesi açıklama olsa bile veri varlıklarına açıklama olamaz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md)

