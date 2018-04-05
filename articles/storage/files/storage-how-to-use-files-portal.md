---
title: Azure portalıyla Azure dosya paylaşımlarını yönetme
description: Azure portalından Azure Dosyaları'nı yönetmeyi öğrenin.
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
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: 588d260bb939c8f6439ca66828296ea455f1524a
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-with-the-azure-portal"></a>Azure portalıyla Azure dosya paylaşımlarını yönetme 
[Azure Dosyaları](storage-files-introduction.md), Microsoft’un kullanımı kolay bulut dosya sistemidir. Azure dosya paylaşımları, Windows, Linux ve macOS platformlarına bağlanabilir. Bu kılavuzda, [Azure portalını](https://portal.azure.com/) kullanarak Azure dosya paylaşımlarıyla çalışmanın temel bilgileri gösterilmektedir. Şunları nasıl yapacağınızı öğrenin:

> [!div class="checklist"]
> * Kaynak grubu ve depolama hesabı oluşturma
> * Azure dosya paylaşımı oluşturma 
> * Dizin oluşturma
> * Dosyayı karşıya yükleme 
> * Dosya indirme
> * Paylaşım anlık görüntüsü oluşturma ve kullanma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
Dosya paylaşımı oluşturmak için:

1. Panonuzdan depolama hesabını seçin.
2. Depolama hesabı sayfasında, **Hizmetler** bölümünden **Dosyalar**’ı seçin.
    ![Depolama hesabının hizmetler bölümünün anlık görüntüsü; Dosyalar hizmetini seçin](media/storage-how-to-use-files-portal/create-file-share-1.png)

3. **Dosya hizmeti** sayfasının üst kısmındaki menüden **+ Dosya paylaşımı**’na tıklayın. **Yeni dosya paylaşımı** sayfası aşağı doğru açılır.
4. **Ad** alanına *myshare* yazın.
5. **Tamam**’a tıklayarak Azure dosya paylaşımını oluşturun.

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Azure dosya paylaşımının içindekileri işleme
Artık Azure dosya paylaşımını oluşturduğunuza göre, [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) veya [macOS](storage-how-to-use-files-mac.md)'ta SMB ile dosya paylaşımını bağlayabilirsiniz. Alternatif olarak, Azure dosya paylaşımınızı Azure portalıyla da işleyebilirsiniz. Azure portalı üzerinde gelen tüm istekler Dosya REST API ile yapılır; böylelikle SMB erişimi olmadan istemcilerdeki dosyaları ve dizinleri oluşturabilir, değiştirebilir ve silebilirsiniz.

### <a name="create-directory"></a>Dizin oluşturma
Azure dosya paylaşımınızın kökünde *myDirectory* adlı yeni bir dizin oluşturmak için:

1. **Dosya Hizmeti** sayfasında **myshare** dosya paylaşımını seçin. Dosya paylaşımınızın sayfası açılır.
2. Sayfanın en üstündeki menüden **+ Dizin ekle**’yi seçin. **Yeni dizin** sayfası aşağı doğru açılır.
3. *myDirectory* yazın ve **Tamam**’a tıklayın.

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme 
Bir dosyayı karşıya yüklemeyi göstermek için önce karşıya yüklenecek bir dosya oluşturmanız veya seçmeniz gerekir. Uygun gördüğünüz herhangi bir yolla bunu yapabilirsiniz. Karşıya yüklemek istediğiniz dosyayı seçtikten sonra:

1. **myDirectory** dizinine tıklayın. **myDirectory** paneli açılır.
2. En üstteki menüde **Karşıya Yükle**’ye tıklayın. **Dosyaları karşıya yükleme** paneli açılır.  
    ![Dosyaları karşıya yükleme panelinin ekran görüntüsü](media/storage-how-to-use-files-portal/upload-file-1.png)

3. Yerel dosyalarınıza göz atmak amacıyla bir pencereyi açmak için klasör simgesine tıklayın. 
4. Bir dosya seçin ve **Aç**’a tıklayın. 
5. **Dosyaları karşıya yükleme** sayfasında, dosya adını doğrulayın ve **Karşıya Yükle**’ye tıklayın.
6. Tamamlandığında, dosyanın **myDirectory** sayfasındaki listede gösterilmesi gerekir.

### <a name="download-a-file"></a>Dosya indirme
Dosyaya sağ tıklayarak, karşıya yüklediğiniz dosyanın bir kopyasını indirebilirsiniz. İndir düğmesine tıklandıktan sonra olacak işlemler, kullandığınız işletim sistemine ve tarayıcıya bağlıdır.

## <a name="create-and-modify-share-snapshots"></a>Paylaşım anlık görüntülerini oluşturma ve değiştirme
Azure dosya paylaşımıyla yerine getirebileceğiniz kullanışlı bir diğer görev de paylaşım anlık görüntüleri oluşturmaktır. Anlık görüntü, Azure dosya paylaşımının zamanın bir noktasındaki durumunu saklar. Paylaşım anlık görüntüleri, aşağıdakiler gibi zaten tanıyor olabileceğiniz işletim sistemi teknolojilerine benzer:
- NTFS ve ReFS gibi Windows dosya sistemleri için [Birim Gölge Kopyası Hizmeti (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636)
- Linux sistemleri için [Mantıksal Birim Yöneticisi (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) anlık görüntüleri
- macOS için [Apple Dosya Sistemi (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) anlık görüntüleri. 

Paylaşım anlık görüntüsü oluşturmak için:

1. Panonuz > **Dosyalar** > **myshare** bölümüne gidip depolama hesabını açarak dosya paylaşımı sayfasını açın. 
2. Dosya paylaşımı sayfasında, sayfanın üst kısmındaki menüden **Anlık Görüntü** düğmesine tıklayın ve **Anlık görüntü oluştur**’u seçin.  
    ![Anlık görüntü oluşturma düğmesinin nasıl bulunacağını gösteren bir ekran görüntüsü](media/storage-how-to-use-files-portal/create-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Paylaşım anlık görüntülerini listeleme ve bunlara göz atma
Anlık görüntü oluşturulduktan sonra, tekrar **Anlık Görüntü**’ye tıklayıp **Anlık görüntüleri görüntüle**’yi seçerek paylaşım için anlık görüntüleri listeleyebilirsiniz. Sonuç bölmesinde, bu paylaşım için anlık görüntüler gösterilir. Paylaşım anlık görüntüsüne göz atmak için anlık görüntüye tıklayın.

### <a name="restore-from-a-share-snapshot"></a>Paylaşım anlık görüntüsünden geri yükleme
Paylaşım anlık görüntüsünden dosyanın nasıl geri yüklendiğini göstermek için, önce canlı Azure dosya paylaşımımızdan bir dosyayı silmeliyiz. *myDirectory* klasörüne gidin, karşıya yüklediğiniz dosyaya sağ tıklayın ve ardından **Sil**'e tıklayın. Sonra, o dosyayı paylaşım anlık görüntüsünden geri yüklemek için:

1. Üst menüden **Anlık Görüntüler**’e tıklayın ve **Anlık görüntüleri görüntüle**’yi seçin. 
2. Daha önce oluşturduğunuz anlık görüntüye tıklayın. İçerikler yeni bir sayfada açılır. 
3. Anlık görüntüde **myDirectory** seçeneğine tıklayın. Böylece, sildiğiniz dosyayı görmeniz gerekir. 
4. Silinen dosyaya sağ tıklayıp **Geri Yükle**’yi seçin.
5. Bir açılır pencere görüntülenerek dosyayı kopya olarak geri yükleme ve özgün dosyanın üzerine yazma arasında seçim yapmanızı sağlar. Özgün dosyayı sildiğimizden, dosyayı silinmeden önceki haline geri yüklemek için **Özgün dosyanın üzerine yaz** seçeneğini belirleyebiliriz. **Tamam**’a tıklayarak Azure dosya paylaşımına dosyayı geri yükleyin.  
    ![Dosyayı geri yükle iletişim kutusunun ekran görüntüsü](media/storage-how-to-use-files-portal/restore-snapshot-1.png)

6. Dosyanın geri yüklenmesi tamamlandıktan sonra, anlık görüntü sayfasını kapatıp **myshare** > **myDirectory** konumuna gidin. Dosya, özgün konumuna geri gelmiş olmalıdır.

### <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüsünü silme
Paylaşım anlık görüntüsünü silmek için [paylaşım anlık görüntüleri listesine gidin](#list-and-browse-a-share-snapshot). Paylaşım anlık görüntüsünün adının yanındaki onay kutusuna tıklayıp **Sil** düğmesini seçin.

![Paylaşım anlık görüntüsünü silme anlık görüntüsü](media/storage-how-to-use-files-portal/delete-snapshot-1.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar
- [Dosya paylaşımlarını Azure PowerShell ile yönetme](storage-how-to-use-files-powershell.md)
- [Dosya paylaşımlarını Azure CLI ile yönetme](storage-how-to-use-files-cli.md)
- [Dosya paylaşımlarını Azure Depolama Gezgini ile yönetme](storage-how-to-use-files-storage-explorer.md)
- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)