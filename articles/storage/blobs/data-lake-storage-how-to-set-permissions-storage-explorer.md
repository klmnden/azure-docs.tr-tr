---
title: "Nasıl yapılır: Ayarlama ACL'lerin dosyaları ve dizinleri Azure Depolama Gezgini'ni kullanma"
description: Bu nasıl için ACL'ler dosyaların ve dizinlerin nasıl ayarlayacağınızı öğrenin
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 12/05/2018
ms.author: rogarana
ms.openlocfilehash: c1bcb373fcf21906cbce9b7276e2a7dd7ebb7ed8
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975911"
---
# <a name="how-to-set-file-and-directory-level-permissions-using-azure-storage-explorer"></a>Nasıl yapılır: Azure Depolama Gezgini'ni kullanarak dosya ve dizin düzeyi izinleri ayarla

Bu makalede, dosya ve dizin ACL üzerinden Azure Storage explorer'ın Masaüstü sürümünü düzeyi gösterilmektedir.

Bu makalede, Azure Depolama Gezgini yüklemenizi gerektirir. Windows, Macintosh veya Linux işletim sisteminde Azure Depolama Gezgini’ni yüklemek için bkz. [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

## <a name="sign-in-to-storage-explorer"></a>Depolama Gezgini için oturum açın

Uygulamayı ilk kez başlattığınızda **Microsoft Azure Depolama Gezgini - Bağlan** penceresi görüntülenir. Depolama Gezgini depolama hesaplarına bağlamak için birçok yol sağlar ancak yalnızca bir yolu şu anda ACL'leri yönetmek için desteklenir.

|Görev|Amaç|
|---|---|
|Azure Hesabı ekleme | Azure'da kimlik doğrulaması gerçekleştirmek için kuruluşunuzun oturum açma sayfasını açar. Şu anda bu Depolama Gezgini'ni kullanarak ACL'leri yönetmenizi sağlayacak yalnızca kimlik doğrulama yöntemidir.|

Seçin **Azure hesabı Ekle** tıklatıp **oturum**. Azure hesabınızda oturum açmak için ekrandaki talimatları izleyin.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](media/storage-quickstart-blobs-storage-explorer/connect.png)

Bağlantı kurulduğunda Azure Depolama Gezgini yüklenir ve **Gezgin** sekmesi gösterilir. Bu görünümde tüm Azure depolama hesaplarınıza ek olarak [Azure Depolama Öykünücüsü](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), [Cosmos DB](../../cosmos-db/storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) hesapları veya [Azure Stack](../../azure-stack/user/azure-stack-storage-connect-se.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ortamları üzerinden yapılandırılan yerel depolama alanlarını görebilirsiniz.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](media/storage-quickstart-blobs-storage-explorer/mainpage.png)

## <a name="managing-access"></a>Erişimi yönetme

Dosya sistemi kökünde izinleri ayarlayabilirsiniz. Bunu yapmak için dosya sistemi sağ tıklatın ve seçin **yönetme izinleri**.

Bu süre sonundan sonra ortaya çıkarmak **Manage Permission** istemi.

![Microsoft Azure Depolama Gezgini - dizin erişimi yönetme](media/storage-quickstart-blobs-storage-explorer/manageperms.png)

**Erişimini yönetme** istemi, izinler, sahibi ve sahipleri Grup yanı sıra yeni kullanıcılar kendisi için ardından yönetebileceğiniz erişim denetim listesine eklemek için izinleri yönetmenize olanak sağlayacaktır. Varsayılan izinler, erişim izinleri ve bunların davranışlarını dahil olmak üzere izinleri hakkında bilgi edinmek için sunduğumuz makaleye bakın [data lake depolama gen2'deki erişim denetimi](data-lake-storage-access-control.md#access-control-lists-on-files-and-directories). Burada seçim yapmayı ACL'leri dizini içinde şu anda var olan herhangi bir öğeyi ayarlamaz.

Güvenlik grupları oluşturma ve koruma bireysel kullanıcılar yerine grup izinlerini öneririz. Bu öneri ve diğer en iyi uygulamalar hakkında daha fazla bilgi için bkz. bizim [data lake depolama Gen2'ye yönelik en iyi uygulamalar](data-lake-storage-best-practices.md) makalesi.

Ayrıntılı erişim denetimi sağlayarak tek tek dosyalar yanı sıra bireysel dizin izinlerini yönetebilir. Dizinleri ve dosyaları izinlerini yönetme işlemi yukarıda açıklanan ile aynıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde izinler dosyaları ve dizinleri kullanma işleminin nasıl yapılacağını öğrendiniz **Azure Depolama Gezgini**. Erişim denetim listeleri ve izinleri hakkında daha fazla bilgi için konu hakkında kavramsal makalemize geçin.

> [!div class="nextstepaction"]
> [Azure Data Lake depolama Gen2 HAccess denetiminde](data-lake-storage-access-control.md)