---
title: "Microsoft Azure Storage Gezgini kullanarak Azure DevTest Labs için VHD dosyasını karşıya | Microsoft Docs"
description: "Microsoft Azure Storage Gezgini kullanarak Laboratuvar ait depolama hesabı için VHD dosyasını karşıya yükle"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: v-craic
ms.openlocfilehash: 25675aae77fbe2610fe416210de9a306c1c09f3d
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="upload-vhd-file-to-labs-storage-account-using-microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Gezgini kullanarak Laboratuvar ait depolama hesabı için VHD dosyasını karşıya yükle

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs'de VHD dosyaları, sanal makineler sağlamak için kullanılan özel görüntülerinizi oluşturmak için kullanılabilir. Bu makalede nasıl kullanılacağını anlatan [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) Laboratuvar ait depolama hesabı için bir VHD dosyasını karşıya yükleyin. VHD dosyası yüklediğiniz sonra [bölüm'sonraki adımlar](#next-steps) karşıya yüklenen VHD dosyasından özel bir görüntü oluşturmak nasıl çalışılacağını bazı makaleleri listeler. Diskleri ve Azure içinde VHD'leri hakkında daha fazla bilgi için bkz: [diskler ve sanal makineler için VHD'ler hakkında](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar bir VHD dosyası aracılığıyla karşıya yükleme DevTest Labs kullanmaya yol [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).

1. [Microsoft Azure Storage Gezgini en son sürümünü karşıdan yükleyip](http://www.storageexplorer.com).

1. Azure portalını kullanarak laboratuar ait depolama hesabı adını alın:

    1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
    
    1. **More services**’i (Daha fazla hizmet’i) seçip ardından listeden **DevTest Labs**’i seçin.
    
    1. İstenen Laboratuvar labs listesinden seçin.  
    
    1. Laboratuvar 's dikey penceresinde, seçin **yapılandırma**. 
    
    1. Laboratuvar üzerinde **yapılandırma** dikey penceresinde, select **özel görüntülerini (VHD)**.
    
    1. Üzerinde **özel görüntüleri** dikey penceresinde, seçin **+ Ekle**. 
    
    1. Üzerinde **özel görüntü** dikey penceresinde, select **VHD**.
    
    1. Üzerinde **VHD** dikey penceresinde, select **PowerShell kullanarak bir VHD'yi karşıya**.
    
        ![PowerShell kullanarak VHD karşıya yükle][0]
    
    1. **PowerShell kullanarak bir görüntüyü karşıya yüklemeden** dikey penceresinde görüntüler yapılan bir çağrı **Ekle AzureVhd** cmdlet'i. İlk parametre (*hedef*) depolama hesabı adı şu biçimde Laboratuvar için içerir:
    
        https://<Storage-ACCOUNT-Name>.BLOB.Core.Windows.NET/uploads/... 

    1. Sonraki adımlarda kullanılmak üzere depolama hesabı adını not edin.
    
1. Depolama Gezgini'ni kullanarak bir Azure aboneliği hesabınıza bağlanın.

    > [!TIP] 
    > 
    > Depolama Gezgini çeşitli bağlantı seçenekleri destekler. Bu bölümde, Azure aboneliğinizle ilişkili bir depolama hesabı bağlanma gösterilmektedir. Depolama Gezgini tarafından desteklenen diğer bağlantı seçeneklerini görmek için makaleyi için başvuruda [Depolama Gezgini ile çalışmaya başlama](../vs-azure-tools-storage-manage-with-storage-explorer.md).
 
    1. Depolama Gezgini'ni açın.
    
    1. Depolama Gezgini'nde seçin **Azure hesap ayarlarını**. 
    
        ![Azure hesap ayarları][1]
    
    1. Sol bölmede, oturum açtığınız Microsoft hesapları görüntülenir. Başka bir hesaba bağlanmak için **Hesap ekle**’yi seçin ve en az bir etkin Azure aboneliği ile ilişkilendirilen bir Microsoft hesabı ile oturum açmak için iletişim kutularını izleyin.
    
        ![Hesap ekleme][2]
    
    1. Bir Microsoft hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin. (Seçme **tüm abonelikleri** veya listelenen Azure aboneliklerinin hiçbirinin seçimini geçiş yapar.)
    
        ![Azure aboneliklerini seçme][3]
    
    1. Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.
    
        ![Seçili Azure abonelikleri][4]

1. Laboratuvar ait depolama hesabını bulun:

    1. Depolama Gezgini sol bölmede, bulun ve Laboratuvar sahip Azure aboneliği için düğümü genişletebilirsiniz.
    
    1. Aboneliğin düğümünde genişletin **depolama hesapları**.

    1. Düğümleri için ortaya çıkarmak için laboratuvar ait depolama hesabı düğümünü **Blob kapsayıcıları**, **dosya paylaşımları**, **sıraları**, ve **tabloları**.
    
    1. Genişletme **Blob kapsayıcıları** düğümü.
    
    1. Sağ bölmede içeriğini görüntülemek için karşıya blob kapsayıcısı seçin.
        
        ![Dizin karşıya yükle][5]

1. Depolama Gezgini'ni kullanarak VHD dosyasını karşıya yükle:

    1. Depolama Gezgini sağ bölmede, BLOB'ları listesini görmelisiniz **yükler** Laboratuvar ait depolama hesabının blob kapsayıcısı. Blob Düzenleyicisi araç çubuğunda seçin **karşıya yükle** 
        
        ![Karşıya yükle düğmesi][6]
    
    1. Gelen **karşıya** açılır menüsünde, select **dosyaları karşıya yükleme...** .
    
    1. Üzerinde **dosyaları karşıya yükleme** iletişim kutusunda, üç nokta seçin.
        
        ![Dosya seç][8]  

    1. Üzerinde **karşıya yüklemek için dosyaları seçin** iletişim kutusunda, istenen VHD dosyasına göz atın, onu seçin ve ardından **açık**.
    
    1. Döndürülen zaman **dosyaları karşıya yükleme** iletişim kutusunda, değişiklik **Blob türü** için **sayfa blobu**.
    
    1. **Karşıya Yükle**’yi seçin.

        ![Dosya seç][9]  
    
    1. Depolama Gezgini **etkinlik günlüğü** bölmesi (birlikte karşıya yüklemeyi iptal etmek için bağlantılar) yükleme durumunu gösterir. VHD dosyasını karşıya yükleme işlemi VHD dosyasını ve bağlantı hızı boyutuna bağlı olarak uzun olabilir. 

        ![Dosya karşıya yükleme durumu][10]  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DevTest Labs Azure portalını kullanarak bir VHD dosyasında özel bir görüntü oluşturun](devtest-lab-create-template.md)
- [Azure DevTest Labs PowerShell kullanarak bir VHD dosyasında özel bir görüntü oluşturun](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

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
