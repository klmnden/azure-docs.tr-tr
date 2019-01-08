---
title: Azure Marketi için bir şirket içi sanal makine görüntüsü oluşturma | Microsoft Docs
description: Anlama ve bir şirket içi VM görüntüsü oluşturma ve dağıtma satın almak için Azure Market'ten için diğer adımları yürütün.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ROBOTS: NOINDEX
ms.openlocfilehash: b9fbb2f50905b1b80a092ba13f860f30cb9423a9
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54077802"
---
# <a name="develop-an-on-premises-virtual-machine-image-for-the-azure-marketplace"></a>Azure Marketi için bir şirket içi sanal makine görüntüsü geliştirin
Uzak Masaüstü Protokolü'nü kullanarak Azure sanal sabit diskleri (VHD'ler) doğrudan bulutta geliştirmeniz kesinlikle önerilir. Ancak, gerekirse, VHD indirme ve şirket içi altyapıyı kullanarak geliştirme mümkün.  

Şirket içi geliştirme için işletim sistemi VHD'si oluşturulan VM'nin indirmeniz gerekir. Yukarıdaki adım 3.3, bir parçası olarak bir yerde şu adımları uygulayın.  

## <a name="download-a-vhd-image"></a>Bir VHD görüntüsü indirin
### <a name="locate-a-blob-url"></a>Bir blob URL'si ile bulun
VHD indirmek için önce işletim sistemi diski blob URL'sini bulun.

Yeni blob URL'den bulun [Microsoft Azure Portal'da](https://portal.azure.com):

1. Git **Gözat** > **Vm'leri**ve ardından dağıtılmış VM'yi seçin.
2. Altında **yapılandırma**seçin **diskleri** kutucuğunda, diskleri dikey penceresi açılır.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. Seçin **işletim sistemi diski**, VHD konumu dahil olmak üzere, disk özelliklerini görüntüler başka bir dikey pencere açılır.
4. Bu blob URL'sini kopyalayın.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. Şimdi, dağıtılan VM yedekleme diskleri silmeden silin. Ayrıca, VM'yi silmek yerine durdurabilirsiniz. VM çalışırken işletim sistemi VHD'si yüklemeyin.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a>VHD indirme
Blob URL'si bulduktan sonra VHD kullanarak indirebilirsiniz [Azure portalında](http://manage.windowsazure.com/) veya PowerShell.  

> [!NOTE]
> Bu kılavuz oluşturma sırasında VHD indirme için gereken işlevleri henüz yeni Microsoft Azure Portalı'nda mevcut değil.  
> 
> 

**İndirme yoluyla geçerli işletim sistemi VHD'si [Azure portalı](http://manage.windowsazure.com/)**

1. Bunu zaten yapmadıysanız, Azure portalında oturum açın.
2. Tıklayın **depolama** sekmesi.
3. VHD'nin depolandığı depolama hesabını seçin.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. Bu depolama hesabı özelliklerini görüntüler. Seçin **kapsayıcıları** sekmesi.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. VHD'nin depolandığı kapsayıcıyı seçin. Varsayılan olarak, portaldan oluştururken VHD'yi bir VHD kapsayıcısı içinde depolanır.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. Doğru işletim sistemi VHD'si, kaydettiğiniz bir URL'ye karşılaştırarak seçin.
7. **İndir**’e tıklayın.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a>PowerShell kullanarak bir VHD'yi indirin
Azure portalını kullanmaya ek olarak kullanabilirsiniz [Save-AzureVhd](https://msdn.microsoft.com/library/dn495297.aspx) cmdlet'ini işletim sistemi VHD'si indirin.

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
Örneğin, Save-AzureVhd-kaynak "https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd" - LocalFilePath "C:\Users\Administrator\Desktop\baseimagevm.vhd" - depolama anahtarı <String>

> [!NOTE]
> **Kaydet-AzureVhd** de sahip bir **NumberOfThreads** indirme için kullanılabilir bant genişliğini en iyi kullanılmasını sağlamak için paralellik artırmak için kullanılan seçeneği.
> 
> 

## <a name="upload-vhds-to-an-azure-storage-account"></a>Azure depolama hesabınız VHD yükleme
VHD'ler şirket içi hazırladığınız, bunları azure'da bir depolama hesabına dosya yükleme gerekir. Bu adım, VHD oluştururken şirket içi sonra ancak VM görüntünüz için sertifika alma önce gerçekleşir.

### <a name="create-a-storage-account-and-container"></a>Bir depolama hesabı ve kapsayıcı oluşturma
Amerika Birleşik Devletleri'nde bulunan bir bölgede depolama hesabına VHD yüklenmesini öneririz. Tek bir SKU için tüm VHD'ler, tek bir depolama hesabında içindeki tek bir kapsayıcıda yerleştirilmelidir.

Bir depolama hesabı oluşturmak için kullanabileceğiniz [Microsoft Azure Portal'da](https://portal.azure.com/), PowerShell veya Linux komut satırı aracı.  

**Microsoft Azure Portal'da depolama hesabı oluşturma**

1. **Kaynak oluştur**’a tıklayın.
2. **Depolama**’yı seçin.
3. Depolama hesabı adını girin ve ardından bir konum seçin.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. **Oluştur**’a tıklayın.
5. Oluşturulan depolama hesabı için dikey pencereyi açık olmalıdır. Aksi takdirde, seçin **Gözat** > **depolama hesapları**. Depolama hesabı dikey penceresinde, oluşturduğunuz depolama hesabını seçin.
6. Seçin **kapsayıcıları**.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. Kapsayıcıları dikey penceresinde seçin **Ekle**ve ardından bir kapsayıcı adı ve kapsayıcı izinlerini girin. Seçin **özel** kapsayıcı izinleri.

> [!TIP]
> Bir kapsayıcı başına yayımlamayı planlama SKU oluşturmanızı öneririz.
> 
> 

  ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a>PowerShell kullanarak bir depolama hesabı oluşturma
PowerShell'i kullanarak bir depolama hesabı oluştur kullanarak [New-AzureStorageAccount](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azurestorageaccount) cmdlet'i.

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

Bu depolama hesabında bir kapsayıcı kullanarak oluşturabileceğiniz daha sonra [New-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) cmdlet'i.

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> Bu komutları, geçerli depolama hesabı bağlamını PowerShell'de zaten ayarlanmış varsayılır.   Başvurmak [Azure PowerShell ayarlama ayarlama](marketplace-publishing-powershell-setup.md) PowerShell kurulumu hakkında daha fazla bilgi.  
> 
> ### <a name="create-a-storage-account-by-using-the-command-line-tool-for-mac-and-linux"></a>Mac ve Linux için komut satırı aracını kullanarak bir depolama hesabı oluşturma
> Gelen [Linux komut satırı aracı](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), şu şekilde bir depolama hesabı oluşturun.
> 
> 

        azure storage account create mystorageaccount --location "West US"

Şu şekilde bir kapsayıcı oluşturun.

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a>VHD’yi karşıya yükleme
Depolama hesabı ve kapsayıcı oluşturulduktan sonra hazırlanmış Vhd'lerinizi karşıya yükleyebilirsiniz. PowerShell, Linux komut satırı aracı veya diğer Azure Depolama Yönetimi araçlarını kullanabilirsiniz.

### <a name="upload-a-vhd-via-powershell"></a>PowerShell aracılığıyla bir VHD'yi karşıya yükleme
Kullanım [Add-AzureVhd](https://msdn.microsoft.com/library/dn495173.aspx) cmdlet'i.

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-the-command-line-tool-for-mac-and-linux"></a>Mac ve Linux için komut satırı aracını kullanarak bir VHD'yi karşıya yükleme
İle [Linux komut satırı aracı](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), aşağıdaki komutu kullanın: `azure vm image create <image name> --location <Location of the data center> --OS Linux <LocationOfLocalVHD>`

## <a name="see-also"></a>Ayrıca bkz.
* [Market'te bir sanal makine görüntüsü oluşturma](marketplace-publishing-vm-image-creation.md)
* [Azure PowerShell ayarlama](marketplace-publishing-powershell-setup.md)

