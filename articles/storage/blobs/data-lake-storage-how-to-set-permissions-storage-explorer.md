---
title: Azure Depolama Gezgini ile Data Lake depolama Gen2 izinlerini ayarlama
description: Bu nasıl için Azure Depolama Gezgini ile dosya ve dizinleri, Azure Data Lake depolama Gen2 özellikli depolama hesabı içinde izinler öğrenin.
services: storage
author: roygara
ms.custom: mvc
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: rogarana
ms.openlocfilehash: f2569b29ab6124f1cfa22fa745d45082c213a6be
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60003488"
---
# <a name="set-file-and-directory-level-permissions-using-azure-storage-explorer-with-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama 2. nesil ile Azure Depolama Gezgini'ni kullanarak dosya ve dizin düzeyi izinleri ayarlayın

Azure Data Lake depolama Gen2'içinde depolanan dosyaların hassas izinlere desteklemek ve erişim denetimi listesi (ACL) yönetimi. Birlikte ayrıntılı izinler ve ACL yönetimi, çok ayrıntılı bir düzeyde verilerinize erişimini yönetmenizi sağlar.

Bu makalede, Azure Depolama Gezgini'ni kullanarak öğreneceksiniz:

> [!div class="checklist"]
> * Dosya düzeyinde izinler ayarlayın
> * Dizin düzeyi izinlerini ayarlayın
> * Kullanıcıları veya grupları bir erişim denetim listesine ekleyin.

## <a name="prerequisites"></a>Önkoşullar

İşlemi en iyi tarif için tamamlamanız gerekiyor bizim [Azure Depolama Gezgini hızlı](data-lake-storage-Explorer.md). Bu, depolama hesabınız (dosya sistemi oluşturulan ve için karşıya veri) en uygun durumda olacak sağlar.

## <a name="managing-access"></a>Erişimi yönetme

Dosya sisteminizi kökünde izinleri ayarlayabilirsiniz. Bunu yapmak için Azure depolama Gezgini'ne (ile bir bağlantı dizesi olarak) bunu haklarıyla bireysel hesabınızla oturum açmış olmanız gerekir. Dosya sisteminizi sağ tıklayıp **yönetme izinleri**, yedekleme yapmak **Manage Permission** iletişim kutusu.

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
> [Azure Data Lake Storage 2. Nesil'de Erişim Denetimi](data-lake-storage-access-control.md)
