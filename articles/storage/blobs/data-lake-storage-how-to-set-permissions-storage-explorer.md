---
title: Azure Depolama Gezgini ile Data Lake depolama Gen2 izinlerini ayarlama
description: İçinde bu nasıl yapılır, dosyalar ve dizinler, Azure Data Lake depolama Gen2 içinde Azure Depolama Gezgini ile izinleri ayarlamak öğrenin (Önizleme) özellikli depolama hesabı.
services: storage
author: roygara
ms.custom: mvc
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: rogarana
ms.openlocfilehash: fd4ca3946ed4c32a8fd2f08c1c242c33dbca2aaf
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238322"
---
# <a name="set-file-and-directory-level-permissions-using-azure-storage-explorer-with-azure-data-lake-storage-gen2-preview"></a>Azure Data Lake depolama 2. nesil ile Azure Depolama Gezgini'ni kullanarak dosya ve dizin düzeyi izinleri ayarla (Önizleme)

Azure Data Lake depolama Gen2'içinde depolanan dosyalar (Önizleme) desteği ayrıntılı izinler ve erişim denetimi listesi (ACL) yönetimi. Birlikte ayrıntılı izinler ve ACL yönetimi, çok ayrıntılı bir düzeyde verilerinize erişimini yönetmenizi sağlar.

Bu makalede, Azure Depolama Gezgini'ni kullanarak öğreneceksiniz:

> [!div class="checklist"]
> * Dosya düzeyinde izinler ayarlayın
> * Dizin düzeyi izinlerini ayarlayın
> * Kullanıcıları veya grupları bir erişim denetim listesine ekleyin.

## <a name="prerequisites"></a>Önkoşullar

İşlemi en iyi tarif için tamamlamanız gerekiyor bizim [Azure Depolama Gezgini hızlı](data-lake-storage-Explorer.md). Bu, depolama hesabınız (dosya sistemi oluşturulan ve için karşıya veri) en uygun durumda olacak sağlar.

## <a name="managing-access"></a>Erişimi yönetme

Dosya sistemi kökünde izinleri ayarlayabilirsiniz. Bunu yapmak için dosya sistemi sağ tıklatın ve seçin **yönetme izinleri**, yedekleme yapmak **Manage Permission** iletişim kutusu.

![Microsoft Azure Depolama Gezgini - dizin erişimi yönetme](media/storage-quickstart-blobs-storage-Explorer/manageperms.png)

**Manage Permission** iletişim kutusu sahibi ve sahipleri grup için izinleri yönetmenize olanak tanır. Ayrıca yeni kullanıcılar ve gruplar için daha sonra izinleri yönetebilirsiniz erişim denetim listesine eklemenize olanak sağlar.

Erişim denetim listesine yeni bir kullanıcı veya grup eklemek için seçin **kullanıcı veya Grup Ekle** alan.

Listeye ekleyin ve ardından istediğiniz ilgili Azure Active Directory (AAD) girişi girin **Ekle**.

Kullanıcı veya grup artık görünür **kullanıcılar ve gruplar:** alanı izinlerini yönetmeye başlamak etmenize imkan sağlar.

> [!NOTE]
> En iyi bir uygulamadır ve AAD içinde bir güvenlik grubu oluşturun ve bireysel kullanıcılar yerine grup izinleri korumak için önerilir. Bu öneri yanı sıra, diğer en iyi yöntemler hakkında daha fazla bilgi için bkz: [en iyi uygulamalar için Data Lake depolama Gen2](data-lake-storage-best-practices.md).

İzinleri atayabilirsiniz iki kategorisi vardır: erişim ACL'leri ve varsayılan ACL.

* **Erişim**: Erişim ACL'leri bir nesneye erişimi denetler. Dosyalar ve dizinler erişim ACL'leri vardır.

* **Varsayılan**: Bu dizin altında oluşturulan tüm alt öğelere ilişkin erişim ACL'lerini belirleyen bir dizin ile ilişkili ACL'leri şablonu. Dosyaları varsayılan ACL'ye sahip değildir.

İçinde her ikisi de bu kategorilerin dosyaları veya dizinleri ardından atayabilirsiniz üç izinleri vardır: **Okuma**, **yazma**, ve **yürütme**.

>[!NOTE]
> Burada seçim yapmayı izinleri dizini içinde şu anda var olan herhangi bir öğeyi ayarlamaz. Tek tek her öğesine gidin ve bir dosya zaten mevcutsa izinleri el ile ayarlamanız gerekir.

Ayrıntılı erişim denetimi sağlar tek tek dosyaların yanı sıra bireysel dizin izinlerini yönetebilir. Hem dizinler ve dosyalar için izinleri yönetme işlemi yukarıda açıklanan ile aynıdır. Aynı süreci izleyin ve üzerinde izinleri yönetmek istediğiniz dosya veya dizin sağ tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde izinler dosyaları ve dizinleri kullanma işleminin nasıl yapılacağını öğrendiniz **Azure Depolama Gezgini**. Varsayılan ACL'ler dahil olmak üzere, ACL'ler hakkında daha fazla bilgi edinmek, ACL'ler, davranışları ve karşılık gelen izinlerini erişim, konu hakkında kavramsal makalemize devam için.

> [!div class="nextstepaction"]
> [Azure Data Lake depolama Gen2'ye erişim denetimi](data-lake-storage-access-control.md)
