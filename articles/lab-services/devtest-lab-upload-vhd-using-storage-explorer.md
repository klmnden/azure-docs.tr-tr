---
title: Microsoft Azure Depolama Gezgini'ni kullanarak Azure DevTest Labs kullanarak karşıya VHD dosyası yükleme | Microsoft Docs
description: Laboratuvar depolama hesabı için Microsoft Azure Depolama Gezgini'ni kullanarak karşıya VHD dosyası yükleme
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 3c187d104334fe75ec9e0ce41a3fdc14b508dfb2
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521734"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-microsoft-azure-storage-explorer"></a>Laboratuvar depolama hesabı için Microsoft Azure Depolama Gezgini'ni kullanarak karşıya VHD dosyası yükleme

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs'de sanal makineler sağlamak için kullanılan özel görüntüler oluşturmak için VHD dosyaları kullanılabilir. Bu makalede nasıl kullanılacağı gösterilmektedir [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) bir laboratuvar depolama hesabına VHD dosyası yüklemek için. VHD dosyasını yükledikten sonra [Bölümü'sonraki adımlar](#next-steps) karşıya yüklenen VHD dosyasından bir özel görüntü oluşturma işlemini göstermektedir bazı makaleler listeler. Diskler ve VHD'ler Azure hakkında daha fazla bilgi için bkz: [yönetilen disklere giriş](../virtual-machines/linux/managed-disks-overview.md)

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlarda DevTest Labs kullanarak bir VHD dosyasını karşıya aracılığıyla yol [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).

1. [Microsoft Azure Depolama Gezgini en son sürümünü indirip](https://www.storageexplorer.com).

1. Azure portalını kullanarak Laboratuvar depolama hesabının adını alın:

    1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
    
    1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
    
    1. İstenen Laboratuvar labs listesinden seçin.  
    
    1. Laboratuvar dikey penceresinde seçin **yapılandırma**. 
    
    1. Laboratuvar **yapılandırma** dikey penceresinde **özel görüntülerin (VHD)**.
    
    1. Üzerinde **özel görüntüleri** dikey penceresinde seçin **+ Ekle**. 
    
    1. Üzerinde **özel görüntü** dikey penceresinde **VHD**.
    
    1. Üzerinde **VHD** dikey penceresinde **PowerShell kullanarak bir VHD'yi karşıya yükleme**.
    
        ![PowerShell kullanarak bir VHD'yi karşıya yükleme][0]
    
    1. **PowerShell kullanarak bir görüntüyü karşıya yükleme** dikey bir çağrı görüntüler **Add-AzureVhd** cmdlet'i. İlk parametre (*hedef*) depolama hesabı adı şu biçimde Laboratuvar için içerir:
    
        `https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...`

    1. Sonraki adımlarda kullanılan depolama hesabı adını not edin.
    
1. Depolama Gezgini'ni kullanarak bir Azure aboneliği hesabınıza bağlanın.

    > [!TIP] 
    > 
    > Depolama Gezgini, çeşitli bağlantı seçenekleri destekler. Bu bölümde, Azure aboneliğinizle ilişkili bir depolama hesabına bağlanmayı gösterir. Depolama Gezgini tarafından desteklenen diğer bağlantı seçenekleri görmek için makaleye bakın [Depolama Gezgini ile çalışmaya başlama](../vs-azure-tools-storage-manage-with-storage-explorer.md).
 
    1. Depolama Gezgini'ni açın.
    
    1. Depolama Gezgini'nde seçin **Azure hesap ayarları**. 
    
        ![Azure hesap ayarları][1]
    
    1. Sol bölmede oturum açtığınız Microsoft hesapları gösterilir. Başka bir hesaba bağlanmak için **Hesap ekle**’yi seçin ve en az bir etkin Azure aboneliği ile ilişkilendirilen bir Microsoft hesabı ile oturum açmak için iletişim kutularını izleyin.
    
        ![Hesap ekleme][2]
    
    1. Bir Microsoft hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin. (Seçerek **tüm abonelikleri** veya listelenen Azure aboneliklerinin hiçbirinin seçimini yapar.)
    
        ![Azure aboneliklerini seçme][3]
    
    1. Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.
    
        ![Seçili Azure abonelikleri][4]

1. Laboratuvar depolama hesabını bulun:

    1. Depolama Gezgini sol bölmede, bulun ve Laboratuvar sahibi olan Azure aboneliğini düğümünü genişletin.
    
    1. Aboneliğin düğümünde genişletin **depolama hesapları**.

    1. Laboratuvar depolama hesabı düğümü için düğümleri göstermek için genişletin **Blob kapsayıcıları**, **dosya paylaşımları**, **kuyrukları**, ve **tabloları**.
    
    1. Genişletin **Blob kapsayıcıları** düğümü.
    
    1. Sağ bölmede içeriği görüntülemek için karşıya blob kapsayıcıyı seçin.
        
        ![Dizin karşıya yükleme][5]

1. Depolama Gezgini'ni kullanarak karşıya VHD dosyası yükleme:

    1. Depolama Gezgini sağ bölmede, BLOB'ları listesini görmelisiniz **yükler** Laboratuvar depolama hesabının blob kapsayıcısı. Blob Düzenleyici araç çubuğunda **karşıya yükleme** 
        
        ![Karşıya yükle düğmesi][6]
    
    1. Gelen **karşıya** açılan menüsünde, select **dosyaları karşıya yükle...** .
    
    1. Üzerinde **dosyaları karşıya yükleme** iletişim kutusunda üç nokta simgesini seçin.
        
        ![Dosya seç][8]  

    1. Üzerinde **karşıya yüklenecek dosyaları seçin** iletişim kutusunda, istediğiniz VHD dosyasına göz atın, onu seçin ve ardından **açık**.
    
    1. İçin döndürülen **dosyaları karşıya yükleme** iletişim kutusunda değişiklik **Blob türü** için **sayfa blobu**.
    
    1. **Karşıya Yükle**’yi seçin.

        ![Dosya seç][9]  
    
    1. Depolama Gezgini **etkinlik günlüğü** bölmesi (birlikte karşıya yüklemeyi iptal etmek için bağlantılar) yükleme durumunu gösterir. Bir VHD dosyasını karşıya yükleme işlemi, VHD dosyasının ve bağlantı hızınızı boyutuna bağlı olarak uzun olabilir. 

        ![Dosya karşıya yükleme durumu][10]  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DevTest Labs Azure portalını kullanarak bir VHD dosyasından bir özel görüntü oluşturma](devtest-lab-create-template.md)
- [Azure DevTest labs'teki PowerShell kullanarak bir VHD dosyasından özel bir görüntü oluşturma](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

[0]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-image-using-psh.png
[1]: ./media/devtest-lab-upload-vhd-using-storage-explorer/settings-icon.png
[2]: ./media/devtest-lab-upload-vhd-using-storage-explorer/add-account-link.png
[3]: ./media/devtest-lab-upload-vhd-using-storage-explorer/subscriptions-list.png
[4]: ./media/devtest-lab-upload-vhd-using-storage-explorer/storage-accounts-list.png
[5]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-dir.png
[6]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-button.png
[7]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-files.png
[8]: ./media/devtest-lab-upload-vhd-using-storage-explorer/select-file.png
[9]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-file.png
[10]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-status.png
