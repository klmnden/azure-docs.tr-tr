---
title: Hızlı Başlangıç - Azure Depolama Gezgini kullanarak nesne depolamada blob oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, Azure Depolama Gezgini bir dizin ve dosya oluşturmak için nasıl kullanılacağını öğrenin. Ardından, dosyayı yerel bilgisayarınıza indirmek nasıl yanı sıra, bir dizindeki tüm dosyayı görüntüleme öğrenin. Ayrıca bir dosyanın anlık görüntüsünü oluşturma, dizin erişim ilkelerini yönetmenize ve bir paylaşılan erişim imzası oluşturma konusunda bilgi edinin.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 12/05/2018
ms.author: tamram
ms.openlocfilehash: c5b674a33b400a1e920b10c839d4708897120a31
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975973"
---
# <a name="quickstart-use-azure-storage-explorer-to-create-a-blob-in-object-storage"></a>Hızlı Başlangıç - Azure Depolama Gezgini kullanarak nesne depolamada blob oluşturma | Microsoft Docs

Bu hızlı başlangıçta nasıl kullanılacağını öğrenin [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) bir dizin ve blob oluşturmak için. Ardından, nasıl blob yerel bilgisayarınıza indirin ve tüm blobların bir dizinde görüntüleme öğrenin. Ayrıca bir blobun anlık görüntüsünü oluşturma, dizin erişim ilkelerini yönetmenize ve paylaşılan erişim imzası oluşturma hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Bu hızlı başlangıç Azure Depolama Gezgini'ni yüklemenizi gerektirir. Windows, Macintosh veya Linux işletim sisteminde Azure Depolama Gezgini’ni yüklemek için bkz. [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

## <a name="log-in-to-storage-explorer"></a>Depolama Gezgini oturumu açma

Uygulamayı ilk kez başlattığınızda **Microsoft Azure Depolama Gezgini - Bağlan** penceresi görüntülenir. Depolama Gezgini depolama hesaplarına bağlamak için birçok yol sağlar ancak yalnızca bir yolu şu anda ACL'leri yönetmek için desteklenir.

|Görev|Amaç|
|---|---|
|Azure Hesabı ekleme | Azure'da kimlik doğrulaması gerçekleştirmek için kuruluşunuzun oturum açma sayfasını açar. Yönetme ve ACL'ler ayarlamak istiyorsanız şu anda bu tek desteklenen kimlik doğrulama yöntemidir. |

**Azure Hesabı Ekle**'yi seçip **Oturum açın**'a tıklayın. Azure hesabınızda oturum açmak için ekrandaki talimatları izleyin.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](media/storage-quickstart-blobs-storage-explorer/connect.png)

Bağlantı kurulduğunda Azure Depolama Gezgini yüklenir ve **Gezgin** sekmesi gösterilir. Bu görünümde tüm Azure depolama hesaplarınıza ek olarak [Azure Depolama Öykünücüsü](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), [Cosmos DB](../../cosmos-db/storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) hesapları veya [Azure Stack](../../azure-stack/user/azure-stack-storage-connect-se.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ortamları üzerinden yapılandırılan yerel depolama alanlarını görebilirsiniz.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](media/storage-quickstart-blobs-storage-explorer/mainpage.png)

## <a name="create-a-filesystem"></a>Bir dosya sistemi oluşturun

Bloblar her zaman bir dizine yüklenir. Bu, blob gruplarını bilgisayarınızdaki dosyaları klasörler halinde düzenlediğiniz gibi düzenleyebilmenizi sağlar.

Bir dizin oluşturmak için önceki adımda oluşturduğunuz depolama hesabını genişletin. Seçin **Blob kapsayıcısı**seçin ve sağ tıklatıp **oluşturma Blob kapsayıcısı**. Dosya sistemi için bir ad girin. İşlem tamamlandığında basın **Enter** dosya sistemi oluşturmak için. Blob dizini başarıyla oluşturulduktan sonra altında gösterilir **Blob kapsayıcısı** seçili depolama hesabı için bir klasör.

![Microsoft Azure Depolama Gezgini - bir dosya sistemi oluşturma](media/storage-quickstart-blobs-storage-explorer/creating-a-filesystem.png)

## <a name="upload-blobs-to-the-directory"></a>Dizine blobları karşıya yüklemek

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. IaaS VM’lerini yedeklemek için kullanılan VHD dosyaları sayfa bloblarıdır. Ekleme blobları, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetme için kullanılır. Blob depolamada depolanan çoğu dosya blok blobudur.

Dizin Şerit üzerinde seçin **karşıya**. Bu işlemi kullanarak klasör veya dosya yükleyebilirsiniz.

Yüklenecek dosyaları veya klasörü seçin. **Blob türü**'nü seçin. **Ekleme**, **Sayfa** veya **Blok** bloblarını seçebilirsiniz.

.vhd veya .vhdx dosyası yüklüyorsanız **.vhd/.vhdx dosyalarını sayfa blobları olarak yükle (önerilen)** seçeneğini belirleyin.

İçinde **karşıya yükleme (isteğe bağlı) klasörü** dosyaları depolamak için bir klasör adı veya dizin altında klasörlerin alan. Hiçbir klasör seçilmezse dosyalar doğrudan dizininin altına yüklenir.

![Microsoft Azure Depolama Gezgini - blob yükleme](media/storage-quickstart-blobs-storage-explorer/uploadblob.png)

**Tamam**'ı seçtiğinizde dosyalar yüklenmek üzere kuyruğa alınır ve tüm dosyalar yüklenir. Yükleme işlemi tamamlandığında sonuçlar **Etkinlikler** penceresinde gösterilir.

## <a name="view-blobs-in-a-directory"></a>Bir dizin blob'larda görüntüle

İçinde **Azure Depolama Gezgini** uygulama dizininin altında bir depolama hesabı seçin. Ana bölmede, Seçili dizinde blobların listesi gösterilir.

![Microsoft Azure Depolama Gezgini - bir dizin içindeki blobları listeleme](media/storage-quickstart-blobs-storage-explorer/listblobs.png)

## <a name="download-blobs"></a>Blob’ları indirme

**Azure Depolama Gezgini**'ni kullanarak blobları indirmek için istediğiniz blobu seçtikten sonra şeritteki **İndir**'e tıklayın. Açılan dosya iletişim kutusuna dosya adı girebilirsiniz. Blobu yerel konuma indirmeye başlamak için **Kaydet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları **Azure Depolama Gezgini** kullanarak yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. ACL, dosyaları ve dizinleri ayarlama hakkında bilgi edinmek için konu ile ilgili bizim nasıl yapılır devam edin.

> [!div class="nextstepaction"]
> [ACL'ler dosyaları ve dizinleri ayarlama](data-lake-storage-how-to-set-permissions-storage-explorer.md)
