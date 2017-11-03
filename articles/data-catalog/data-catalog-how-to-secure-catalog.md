---
title: "Azure veri Kataloğu erişimin güvenliğini sağlama | Microsoft Docs"
description: "Bu makalede, veri kataloğu ve veri varlıklarını nasıl güvenli kılacağınızı açıklanmaktadır."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: 9664a7bc8493b08c8e0797ac6f1b212079829833
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a>Veri kataloğu ve veri varlıklarını erişimin güvenliğini sağlama
> [!IMPORTANT]
> Bu özellik yalnızca Azure veri Kataloğu standard Edition'da kullanılabilir.

Azure veri Kataloğu veri Kataloğu kimlerin erişebileceğini ve ne gibi işlemler belirtmenize olanak verir (kaydetmek, açıklama, sahipliğini) meta veri Kataloğu üzerinde gerçekleştirebilirsiniz. 

## <a name="catalog-users-and-permissions"></a>Katalog kullanıcıları ve izinleri
Bir kullanıcı veya grup için bir veri Kataloğu erişmenizi ve izinlerini ayarlamak için:

1. Üzerinde [, veri Kataloğu giriş sayfasına](http://www.azuredatacatalog.com), tıklatın **ayarları** araç çubuğunda.

    ![Veri Kataloğu - ayarlar](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. Ayarlar sayfasında genişletin **katalog kullanıcıları** bölümü.
    ![Kullanıcılar - katalog ekleme](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)
3. **Ekle**'ye tıklayın.
4. Tam girin **kullanıcı adı** veya adını **güvenlik grubu** içinde Azure Active Directory (Kataloğu ile ilişkili AAD). Virgül kullanın (', ') ekliyorsanız ayırıcı birden fazla bir kullanıcı veya grup olarak.
    ![Katalog kullanıcıları - kullanıcıları veya grupları](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)
5. Tuşuna **ENTER** veya **sekmesini** metin kutusunun dışında. 
6.  Onaylayın tüm izinleri (**Açıklama Ekle**, **kaydetmek**, ve **Sahipliği Al**) bu kullanıcıları veya grupları varsayılan olarak atanır. Diğer bir deyişle, kullanıcı veya grup için [veri varlıklarını kaydetme]( data-catalog-how-to-register.md), [veri varlıklarına açıklama]( data-catalog-how-to-annotate.md), ve [veri varlıklarının sahipliğini]( data-catalog-how-to-manage.md). 
    ![Katalog kullanıcıları - varsayılan izinleri](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)
7.  Bir kullanıcı veya grup kataloğa yalnızca okuma erişimi vermek için temizleyin **açıklama** kullanıcı veya grup için seçeneği. Bunu yaptığınızda, kullanıcı veya grup katalogdaki veri varlıklarına açıklama olamaz ancak bunları görüntüleyebilirsiniz. 
8.  Bir kullanıcının veya grubun veri varlıklarını kaydetme reddedecek şekilde temizleyin **kaydetmek** kullanıcı veya grup için seçeneği.
9.  Bir kullanıcının bir veri varlığına sahipliğini reddedecek şekilde temizleyin **sahipliğini** kullanıcı veya grup için seçeneği. 
10. Katalog kullanıcılardan kullanıcı/grup silmek için tıklatın **x** listesinin altındaki kullanıcı/grup için. 
    ![Kullanıcıların katalog - kullanıcı silme](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

    > [!IMPORTANT]
    > Güvenlik grupları ekleme kullanıcılar yerine kullanıcıların doğrudan katalog ve izinleri atamak için eklemenizi öneririz. Ardından, rollerine ve gerekli erişimlerinin katalog aynı güvenlik gruplarına kullanıcıları ekleyin.

## <a name="special-considerations"></a>Özel durumlar

- Güvenlik gruplarına atanan izinler eklenebilir. Örneğin, bir iki grupta kullanıcısıdır. Bir grup izinleri ek açıklama ve diğer grup değil sahip açıklama izinleri. Ardından, kullanıcı sahip açıklama izinleri. 
- Açıkça bir kullanıcıya atanmış izinleri ait olduğu kullanıcı grupları için atanan izinler geçersiz kılar. Önceki örnekte söyleyin, açıkça katalog kullanıcıları ve kullanıcı eklediğiniz değil Ata açıklama izinleri. Kullanıcı sahip bir grup üyesi açıklama izinler olsa bile kullanıcı veri varlıklarına açıklama olamaz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md)

