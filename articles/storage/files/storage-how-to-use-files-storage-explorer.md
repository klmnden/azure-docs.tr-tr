---
title: Azure Depolama Gezgini'ni kullanarak Azure dosya paylaşımlarını yönetme
description: Azure Depolama Gezgini'ni kullanarak Azure Dosyaları'nı yönetmeyi öğrenin.
services: storage
documentationcenter: ''
author: wmgries
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2018
ms.author: wgries
ms.openlocfilehash: 1953ee18fe878c33a1a0965937f64056278875cf
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="manage-azure-file-shares-with-azure-storage-explorer"></a>Azure Depolama Gezgini ile Azure dosya paylaşımlarını yönetme 
[Azure Dosyaları](storage-files-introduction.md), Microsoft’un kullanımı kolay bulut dosya sistemidir. Bu makalede, [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)'ni kullanarak Azure dosya paylaşımlarında çalışmanın temelleri gösterilir. Depolama Gezgini Windows, macOS ve Linux için kullanılabilen popüler bir istemci aracıdır. Depolama Gezgini’ni kullanarak Azure dosya paylaşımlarını ve diğer depolama kaynaklarını yönetebilirsiniz.

Bu hızlı başlangıç için Depolama Gezgini'nin yüklü olması gerekir. İndirip yüklemek için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)’ne gidin.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kaynak grubu ve depolama hesabı oluşturma
> * Azure dosya paylaşımı oluşturma 
> * Dizin oluşturma
> * Dosyayı karşıya yükleme
> * Dosya indirme
> * Paylaşım anlık görüntüsü oluşturma ve kullanma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Depolama Gezgini’ni yeni kaynaklar oluşturmak için kullanamazsınız. Bu tanıtım için [Azure portalında](https://portal.azure.com/) depolama hesabı oluşturun. 

[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="connect-storage-explorer-to-azure-resources"></a>Depolama Gezgini'ni Azure kaynaklarına bağlama
Depolama Gezgini’ni ilk kez başlattığınızda **Microsoft Azure Depolama Gezgini - Bağlan** penceresi görüntülenir. Depolama Gezgini depolama hesaplarına bağlamak için birçok yol sağlar: 

- **Azure hesabınızı kullanarak oturum açma**: Kuruluşunuzun kimlik bilgilerini veya Microsoft hesabınızı kullanarak oturum açabilirsiniz. 
- **Bir bağlantı dizesi veya SAS belirteci kullanarak belirli bir depolama hesabına bağlanma**: Bağlantı dizesi, depolama hesabı adını ve depolama hesabı anahtarını/SAS belirtecini içeren özel bir dizedir. Depolama Gezgini, belirteci kullanarak doğrudan depolama hesabına erişir (bir Azure hesabındaki tüm depolama hesaplarını görmek yerine). Bağlantı dizeleri hakkında daha fazla bilgi edinmek için bkz. [Azure depolama bağlantı dizelerini yapılandırma](../common/storage-configure-connection-string.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
- **Depolama hesabı adı ve anahtarı kullanarak belirli bir depolama hesabına bağlanma**: Azure depolamaya bağlanmak için depolama hesabınızın depolama hesabı adını ve anahtarını kullanın.

Bu hızlı başlangıç için, Azure hesabınızı kullanarak oturum açın. **Azure Hesabı Ekle**’yi ve sonra **Oturum Aç**’ı seçin. Azure hesabınızda oturum açmak için yönergeleri izleyin.

![Microsoft Azure Depolama Gezgini - Bağlan penceresinin ekran görüntüsü](./media/storage-how-to-use-files-storage-explorer/connect-to-azure-storage-1.png)

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
*storageacct<random number>* depolama hesabı içinde ilk Azure dosya paylaşımınızı oluşturmak için:

1. Oluşturduğunuz depolama hesabını genişletin.
2. **Dosya Paylaşımları**'na sağ tıklayın ve sonra **Dosya Paylaşımı Oluştur**'u seçin.  
    ![Dosya paylaşımları klasörünün ve bağlama uygun bağlam menüsünün ekran görüntüsü](media/storage-how-to-use-files-storage-explorer/create-file-share-1.png)

3. Dosya paylaşımı için *myshare* yazın ve sonra Enter tuşuna basın.

> [!IMPORTANT]  
> Paylaşım adları yalnızca küçük harf, sayı ve tek kısa çizgi içerebilir (ancak kısa çizgiyle başlayamaz). Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşım, dizin, dosya ve meta verileri adlandırma ve bunlara başvuruda bulunma](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

Dosya paylaşımı oluşturulduktan sonra sağ bölmede dosya paylaşımınız için bir sekme açılır. 

## <a name="work-with-the-contents-of-an-azure-file-share"></a>Bir Azure dosya paylaşımının içerikleriyle çalışma
Artık Azure dosya paylaşımını oluşturduğunuza göre, [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) veya [macOS](storage-how-to-use-files-mac.md)'ta SMB ile dosya paylaşımını bağlayabilirsiniz. Alternatif olarak, Azure CLI’yi kullanarak Azure dosya paylaşımınızla çalışabilirsiniz. SMB kullanarak dosya paylaşımınızı bağlamak yerine Azure CLI kullanmanın avantajı, Azure CLI ile yapılan tüm isteklerin Dosya REST API’si kullanılarak yapılmasıdır. Dosya REST API’sini kullanarak, SMB erişimi olmayan istemciler üzerinde dosyalar ve dizinler oluşturabilir, değiştirebilir ve silebilirsiniz.

### <a name="create-a-directory"></a>Dizin oluşturma
Dizin eklemek, dosya paylaşımınızın yönetimi için hiyerarşik bir yapı sağlar. Dizininizde birden çok düze oluşturabilirsiniz. Ancak, alt dizinleri oluşturmadan önce üst dizinlerin var olduğundan emin olmanız gerekir. Örneğin, myDirectory/mySubDirectory yolu için öncelikle *myDirectory* dizinini oluşturmanız gerekir. Daha sonra *mySubDirectory* dizinini oluşturabilirsiniz. 

1. Dosya paylaşımının sekmesinde, en üstteki menüden **Yeni Klasör** düğmesini seçin. **Yeni Dizin Oluştur** bölmesi açılır.
    ![Bağlamda Yeni Klasör düğmesinin ekran görüntüsü](media/storage-how-to-use-files-storage-explorer/create-directory-1.png)

2. Dizin adı olarak *myDirectory* yazın ve ardından **Tamam**’ı seçin. 

*myDirectory* dizini, *myshare* dosya paylaşımının sekmesinde listelenir.

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme 
Yerel makinenizden bir dosyayı dosya paylaşımınızdaki yeni dizine yükleyebilirsiniz. Klasörün tamamını veya tek bir dosyayı karşıya yükleyebilirsiniz.

1. Üst menüden **Karşıya Yükle**’yi seçin. Bu işlemi kullanarak klasör veya dosyayı karşıya yükleyebilirsiniz.
2. **Dosyayı Karşıya Yükle**'yi seçin ve sonra yerel makinenizden karşıya yüklenecek dosyayı seçin.
3. **Dizine yükle** alanına *myDirectory* yazın ve sonra **Karşıya Yükle**'yi seçin. 

İşiniz bittiğinde, dosya *myDirectory* bölmesindeki listede görünür.

### <a name="download-a-file"></a>Dosya indirme
Dosya paylaşımınızdan bir dosyanın kopyasını indirmek için dosyaya sağ tıklayın ve ardından **İndir**’i seçin. Dosyanın yerel makinenizde nereye konmasını istediğinizi seçin ve ardından **Kaydet**'i seçin.

Pencerenin en altındaki **Etkinlikler** bölmesinde indirme işleminin ilerleme durumu görünür.

## <a name="create-and-modify-share-snapshots"></a>Paylaşım anlık görüntülerini oluşturma ve değiştirme
Anlık görüntü, Azure dosya paylaşımının zamanın bir noktasındaki kopyasını saklar. Dosya paylaşımı anlık görüntüleri, aşağıdakiler gibi zaten tanıyor olabileceğiniz diğer teknolojilere benzer:
- NTFS ve ReFS gibi Windows dosya sistemleri için [Birim Gölge Kopyası Hizmeti (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636)
- Linux sistemleri için [Mantıksal Birim Yöneticisi (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) anlık görüntüleri
- macOS için [Apple Dosya Sistemi (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) anlık görüntüleri

Paylaşım anlık görüntüsü oluşturmak için:

1. *myshare* dosya paylaşımının sekmesini seçin.
2. Üst menüden **Anlık Görüntü Oluştur**’u seçin. (Depolama Gezgini’nin pencere boyutlarına bağlı olarak, bu seçeneği görmek için öncelikle **Diğer** öğesini seçmeniz gerekebilir.)  
    ![Bağlamda Anlık Görüntü Oluştur düğmesinin ekran görüntüsü](media/storage-how-to-use-files-storage-explorer/create-share-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Paylaşım anlık görüntülerini listeleme ve bunlara göz atma
Anlık görüntü oluşturulduktan sonra, paylaşımın anlık görüntülerini listelemek için **Dosya Paylaşımının Anlık Görüntülerini Gör**’ü seçin. (Depolama Gezgini’nin pencere boyutlarına bağlı olarak, bu seçeneği görmek için öncelikle **Diğer** öğesini seçmeniz gerekebilir.) Paylaşım anlık görüntüsüne göz atmak için anlık görüntüye çift tıklayın.

![Anlık görüntülere göz atma penceresinin anlık görüntüsü](media/storage-how-to-use-files-storage-explorer/list-browse-snapshots-1.png)

### <a name="restore-from-a-share-snapshot"></a>Paylaşım anlık görüntüsünden geri yükleme
Paylaşım anlık görüntüsünden dosyanın nasıl geri yüklendiğini göstermek için, ilk olarak canlı Azure dosya paylaşımından bir dosyayı silmeniz gerekir. *myDirectory* klasörüne gidin, karşıya yüklediğiniz dosyaya sağ tıklayın ve ardından **Sil**'i seçin. O dosyayı paylaşım anlık görüntüsünden geri yüklemek için:

1. **Dosya Paylaşımının Anlık Görüntülerini Gör**’ü seçin. (Depolama Gezgini’nin pencere boyutlarına bağlı olarak, bu seçeneği görmek için öncelikle **Diğer** öğesini seçmeniz gerekebilir.)
2. Paylaşım anlık görüntüleri listesinde paylaşım anlık görüntüsüne çift tıklayın.
3. Sildiğiniz dosyayı bulana kadar anlık görüntüye göz atın. Dosya paylaşımını seçin ve ardından **Anlık Görüntüyü Geri Yükle**’yi seçin. (Depolama Gezgini’nin pencere boyutlarına bağlı olarak, bu seçeneği görmek için öncelikle **Diğer** öğesini seçmeniz gerekebilir.) Bir pencere açılır ve dosyayı geri yüklemeniz durumunda dosya paylaşımındaki içeriğin üzerine yazılacağı ve bu işlemin geri alınamayacağı hakkında bir uyarı gösterilir. **Tamam**’ı seçin.
4. Dosya artık canlı Azure dosya paylaşımının altındaki özgün konumunda olmalıdır.

### <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüsünü silme
Paylaşım anlık görüntüsünü silmek için [paylaşım anlık görüntüleri listesine gidin](#list-and-browse-share-snapshots). Silmek istediğiniz paylaşım anlık görüntüsüne sağ tıklayın ve **Sil**'i seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Depolama Gezgini’ni kaynakları kaldırmak için kullanamazsınız. Bu hızlı başlangıçtan temizlemek için [Azure portalını](https://portal.azure.com/) kullanabilirsiniz. 

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar
- [Dosya paylaşımlarını Azure portalıyla yönetme](storage-how-to-use-files-portal.md)
- [Dosya paylaşımlarını Azure PowerShell ile yönetme](storage-how-to-use-files-powershell.md)
- [Dosya paylaşımlarını Azure CLI ile yönetme](storage-how-to-use-files-cli.md)
- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)