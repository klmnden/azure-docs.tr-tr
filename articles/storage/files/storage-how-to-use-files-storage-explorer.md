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
ms.openlocfilehash: 0c16d3b0ef0b178f99eaafecd74eabb00cca5af9
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-with-azure-storage-explorer"></a>Azure Depolama Gezgini'yle Azure dosya paylaşımlarını yönetme 
[Azure Dosyaları](storage-files-introduction.md), Windows'un kolay kullanılan bulut dosya sistemidir. Bu kılavuzda, [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)'ni kullanarak Azure dosya paylaşımlarında çalışmanın temelleri gösterilir. Azure Depolama Gezgini, Azure dosya paylaşımlarını ve diğer depolama kaynaklarını yönetmek Windows, macOS ve Linux platformlarında sağlanan popüler bir istemci aracıdır.

Bu hızlı başlangıç için Azure Depolama Gezgini'nin yüklenmiş olması gerekir. Yüklemeniz gerekiyorsa [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) sayfasını ziyaret edip bunu indirin.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kaynak grubu ve depolama hesabı oluşturma
> * Azure dosya paylaşımı oluşturma 
> * Dizin oluşturma
> * Dosyayı karşıya yükleme
> * Dosya indirme
> * Paylaşım anlık görüntüsü oluşturma ve paylaşma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Azure Depolama Gezgini'nde yeni kaynaklar oluşturma özelliği yoktur, dolayısıyla bu tanıtım için depolama hesabını [Azure portalında](https://portal.azure.com/) oluşturun. 

[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="connecting-azure-storage-explorer-to-azure-resources"></a>Azure Depolama Gezgini'ni Azure kaynaklarına bağlama
Uygulamayı ilk kez başlattığınızda **Microsoft Azure Depolama Gezgini - Bağlan** penceresi görüntülenir. Azure Depolama Gezgini depolama hesaplarına bağlanmak için birçok yol sağlar: 

- **Azure hesabınızla oturum açma**: Kuruluşunuz için olan kullanıcı kimlik bilgilerinizle veya Microsoft hesabınızla oturum açmanıza olanak tanır. 
- **Bağlantı dizesi veya SAS belirteciyle belirli bir depolama hesabına bağlanma**: Bağlantı dizesi, depolama hesabı adıyla depolama hesabı anahtarını/SAS belirtecini içeren özel bir dizedir ve yalnızca Azure hesabı içindeki tüm depolama hesaplarını görüntülemek yerine Azure Depolama Gezgini'nin depolama hesabına doğrudan erişmesine olanak tanır. Bağlantı dizeleri hakkında daha fazla bilgi edinmek için bkz. [Azure depolama bağlantı dizelerini yapılandırma](../common/storage-configure-connection-string.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
- **Depolama hesabı adı ve anahtarıyla belirli bir depolama hesabına bağlanma**: Azure depolamaya bağlanmak için depolama hesabınızın depolama hesabı adını ve anahtarını kullanın.

Bu hızlı başlangıç için, Azure hesabınızla oturum açın. **Azure Hesabı Ekle**'yi seçip **Oturum açın**'a tıklayın. Azure hesabınızda oturum açmak için ekrandaki talimatları izleyin.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](./media/storage-how-to-use-files-storage-explorer/connect-to-azure-storage-1.png)

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
*storageacct<random number>* depolama hesabı içinde ilk Azure dosya paylaşımınızı oluşturmak için:

1. Oluşturduğunuz depolama hesabını genişletin.
2. **Dosya Paylaşımları**'na sağ tıklayın ve **Dosya Paylaşımı Oluştur**'u seçin.  
    ![Dosya paylaşımları klasörünün ve bağlama uygun bağlam menüsünün ekran görüntüsü](media/storage-how-to-use-files-storage-explorer/create-file-share-1.png)

3. Dosya paylaşımı için *myshare* yazın ve **Enter**'a basın.

> [!Important]  
> Paylaşım adları tümüyle küçük harf, sayı ve tek kısa çizgilerden oluşmalı ve kısa çizgiyle başlamamalıdır. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata)

Dosya paylaşımı oluşturulduktan sonra, sağ bölmede dosya paylaşımınız için bir sekme açılır. 

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Azure dosya paylaşımının içindekileri işleme
Artık Azure dosya paylaşımını oluşturduğunuza göre, [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) veya [macOS](storage-how-to-use-files-mac.md)'ta SMB ile dosya paylaşımını bağlayabilirsiniz. Alternatif olarak, Azure dosya paylaşımınızı Azure portalıyla da işleyebilirsiniz. Azure portalı üzerinde gelen tüm istekler Dosya REST API ile yapılır; böylelikle SMB erişimi olmadan istemcilerdeki dosyaları ve dizinleri oluşturabilir, değiştirebilir ve silebilirsiniz.

### <a name="create-a-directory"></a>Dizin oluşturma
Dizin eklemek, dosya paylaşımınızın yönetimi için hiyerarşik bir yapı sağlar. Birden çok düzey oluşturabilirsiniz, ama alt dizini oluşturmadan önce tüm üst dizinlerin var olduğundan emin olmalısınız. Örneğin myDirectory/mySubDirectory yolu için, önce *myDirectory* dizinini ve ardından *mySubDirectory* alt dizinini oluşturmanız gerekir. 

1. Dosya paylaşımının sekmesinde, en üstteki menüde **+ Yeni Klasör** düğmesine tıklayın. **Yeni Dizin Oluştur** sayfası açılır.
    ![Bağlamda yeni klasör düğmesinin ekran görüntüsü](media/storage-how-to-use-files-storage-explorer/create-directory-1.png)

2. Ad olarak *myDirectory* yazın ve **Tamam**'a tıklayın. 

*myDirectory* dizini, *myshare* dosya paylaşımının sekmesinde listelenir.

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme 
Yerel makinenizden bir dosyayı dosya paylaşımınızdaki yeni dizine yükleyin. Klasörün tamamını veya tek bir dosyayı karşıya yükleyebilirsiniz.

1. En üstteki menüde **Karşıya Yükle**'yi seçin. Bu işlemi kullanarak klasör veya dosyayı karşıya yükleyebilirsiniz.

2. **Karşıya Dosya Yükle**'yi seçin ve sonra da yerel makinenizden karşıya yüklenecek dosyayı seçin.

3. **Dizine yükle** alanına *myDirectory* yazın ve **Karşıya Yükle**'ye tıklayın. 

Tamamlandığında, dosyanın **myDirectory** sayfasındaki listede gösterilmesi gerekir.

### <a name="download-a-file"></a>Dosya indirme
Dosyaya sağ tıklayıp **İndir**'i seçerek dosya paylaşımınızdaki bir dosyanın kopyasını indirebilirsiniz. Dosyanın yerel makinenizde nereye konmasını istediğinizi seçin ve ardından **Kaydet**'e tıklayın.

Pencerenin en altındaki **Etkinlikler** bölmesinde indirme işleminin ilerleme durumunu görürsünüz.

## <a name="create-and-modify-share-snapshots"></a>Paylaşım anlık görüntülerini oluşturma ve değiştirme
Anlık görüntü, Azure dosya paylaşımının zamanın bir noktasındaki kopyasını saklar. Dosya paylaşımı anlık görüntüleri, aşağıdakiler gibi zaten tanıyor olabileceğiniz diğer teknolojilere benzer:
- NTFS ve ReFS gibi Windows dosya sistemleri için [Birim Gölge Kopyası Hizmeti (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636).
- Linux sistemleri için [Mantıksal Birim Yöneticisi (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) anlık görüntüleri
- macOS için [Apple Dosya Sistemi (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) anlık görüntüleri. 

Paylaşım anlık görüntüsü oluşturmak için:

1. *myshare* dosya paylaşımının sekmesini açın.
2. Sekmenin en üstündeki menüde **Anlık Görüntü Oluştur**'a tıklayın (Azure Depolama Gezgini'nin pencere boyutlarına bağlı olarak bu seçenek **... Diğer** öğesinin arkasına gizlenmiş olabilir).  
    ![Bağlamda anlık görüntü oluştur düğmesinin ekran görüntüsü](media/storage-how-to-use-files-storage-explorer/create-share-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Paylaşım anlık görüntülerini listeleme ve bunlara göz atma
Anlık görüntü oluşturulduktan sonra, paylaşımın anlık görüntülerini listelemek için **Dosya Paylaşımları için Anlık Görüntüleri Göster**'e tıklayabilirsiniz (Azure Depolama Gezgini'nin pencere boyutlarına bağlı olarak bu seçenek **... Diğer** öğesinin arkasına gizlenmiş olabilir). Paylaşım anlık görüntüsüne göz atmak için anlık görüntüye çift tıklayın.

![Anlık görüntülere göz atma ekranının anlık görüntüsü](media/storage-how-to-use-files-storage-explorer/list-browse-snapshots-1.png)

### <a name="restore-from-a-share-snapshot"></a>Paylaşım anlık görüntüsünden geri yükleme
Paylaşım anlık görüntüsünden dosyanın nasıl geri yüklendiğini göstermek için, önce canlı Azure dosya paylaşımımızdan bir dosyayı silmeliyiz. *myDirectory* klasörüne gidin, karşıya yüklediğiniz dosyaya sağ tıklayın ve ardından **Sil**'e tıklayın. Sonra, o dosyayı paylaşım anlık görüntüsünden geri yüklemek için:

1. **Dosya Paylaşımı için Anlık Görüntüleri Göster**'e tıklayın (Azure Depolama Gezgini'nin pencere boyutlarına bağlı olarak bu seçenek **... Diğer** öğesinin arkasına gizlenmiş olabilir).
2. Listeden bir paylaşım anlık görüntüsü seçin ve gitmek için çift tıklayın.
3. Sildiğiniz dosyayı bulana kadar anlık görüntüde gezinin, dosyayı seçin ve ardından **Anlık Görüntüyü Geri Yükle**'ye tıklayın (Azure Depolama Gezgini'nin pencere boyutlarına bağlı olarak bu seçenek **... Diğer** öğesinin arkasına gizlenmiş olabilir). Dosya geri yüklendiğinde dosya paylaşımındaki içeriğin üzerine yazılacağı ve bu işlemin geri alınamayacağı hakkında bir uyarı alırsınız. **Tamam**’ı seçin.
4. Artık dosya canlı Azure dosya paylaşımının altında özgün konumunda olmalıdır.

### <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüsünü silme
Paylaşım anlık görüntüsünü silmek için [paylaşım anlık görüntüleri listesine gidin](#list-and-browse-share-snapshots). Silmek istediğiniz paylaşım anlık görüntüsüne sağ tıklayın ve sil'i seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Azure Depolama Gezgini'nde kaynakları kaldırma özelliği yoktur; bu hızlı başlangıcı [Azure portalıyla](https://portal.azure.com/) temizleyebilirsiniz. 

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar
- [Dosya paylaşımlarını Azure portalıyla yönetme](storage-how-to-use-files-portal.md)
- [Dosya paylaşımlarını Azure PowerShell ile yönetme](storage-how-to-use-files-powershell.md)
- [Dosya paylaşımlarını Azure CLI ile yönetme](storage-how-to-use-files-cli.md)
- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)