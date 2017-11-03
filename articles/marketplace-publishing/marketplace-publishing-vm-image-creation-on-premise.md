---
title: "Bir şirket içi sanal makine görüntüsü için Azure Marketi oluşturma | Microsoft Docs"
description: "Anlama ve şirket içi VM görüntü oluşturma ve Azure satın almak için Marketinde başkalarının dağıtma adımları yürütün."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: 8f6b9a9293dc149586e6e5fd55028170ea825b07
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-the-azure-marketplace"></a>Bir şirket içi sanal makine görüntüsü için Azure Marketi geliştirin
Uzak Masaüstü Protokolü kullanarak Azure sanal sabit diskleri (VHD) doğrudan bulutta geliştirmek öneririz. Ancak, gerekirse, bir VHD karşıdan yüklemek ve şirket içi altyapıyı kullanarak geliştirmek mümkündür.  

Şirket içi geliştirme için işletim sistemi oluşturulan VM VHD indirmeniz gerekir. Bu adımları adım 3.3, bir parçası olarak yukarıda gerçekleşmesi.  

## <a name="download-a-vhd-image"></a>Bir VHD görüntüsü indirin
### <a name="locate-a-blob-url"></a>Bir blob URL'si bulun
VHD indirmek için önce işletim sistemi diski blob URL'si bulun.

Yeni blob URL'den bulun [Microsoft Azure portal](https://portal.azure.com):

1. Git **Gözat** > **VM'ler**ve ardından dağıtılan VM seçin.
2. Altında **yapılandırma**seçin **diskleri** döşeme, hangi diskleri bir dikey pencere açılır.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. Seçin **işletim sistemi diski**, disk özellikleri, VHD konumu dahil olmak üzere görüntüler başka bir dikey pencere açılır.
4. Bu blob URL'sini kopyalayın.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. Şimdi, dağıtılan VM yedekleme diskleri silmeden silin. Ayrıca silmeden yerine VM durdurabilirsiniz. VM çalışırken işletim sistemi VHD yüklemeyin.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a>VHD indirme
Blob URL'si öğrendikten sonra kullanarak VHD indirebilirsiniz [Azure portal](http://manage.windowsazure.com/) veya PowerShell.  

> [!NOTE]
> Bu kılavuz oluşturma sırasında bir VHD yüklemek için işlevsellik henüz yeni Microsoft Azure Portalı'nda mevcut değil.  
> 
> 

**Bir işletim sistemi geçerli aracılığıyla karşıdan [Azure portalı](http://manage.windowsazure.com/)**

1. Siz bunu zaten yapmadıysanız, Azure portalında oturum açın.
2. Tıklatın **depolama** sekmesi.
3. VHD depolandığı depolama hesabını seçin.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. Bu depolama hesabı özellikleri görüntüler. Seçin **kapsayıcıları** sekmesi.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. VHD depolandığı kapsayıcısı seçin. Varsayılan olarak, portalından oluşturulması sırasında VHD'yi bir VHD kapsayıcısında depolanır.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. Kaydedilen bir URL karşılaştırarak doğru işletim sistemi VHD seçin.
7. **İndir**’e tıklayın.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a>PowerShell kullanarak bir VHD'yi indirin
Azure Portalı'nı kullanarak ek olarak, kullanabileceğiniz [Kaydet AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) VHD işletim sistemi yüklemek için cmdlet'i.

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
Örneğin, kaydetme AzureVhd-kaynak "https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd" - LocalFilePath "C:\Users\Administrator\Desktop\baseimagevm.vhd" - depolama anahtarı<String>

> [!NOTE]
> **Kaydet-AzureVhd** de sahip bir **NumberOfThreads** indirme için kullanılabilir bant genişliğini en iyi kullanılmasını sağlamak üzere paralelliği artırmak için kullanılan seçeneği.
> 
> 

## <a name="upload-vhds-to-an-azure-storage-account"></a>VHD bir Azure storage hesabına yükleme
VHD'ler şirket içi hazırlanmış, Azure depolama hesabında içine karşıya yüklemeniz gerekir. Bu adım, VHD oluşturma şirket içi sonra ancak VM görüntüsü için sertifika alma önce gerçekleşir.

### <a name="create-a-storage-account-and-container"></a>Bir depolama hesabı ve kapsayıcı oluşturma
VHD'ler Amerika Birleşik Devletleri'nde bir bölgede depolama hesaba yüklenebilmesi öneririz. Tüm VHD'leri tek bir SKU için tek bir depolama hesabına içindeki tek bir kapsayıcıda yerleştirilmelidir.

Bir depolama hesabı oluşturmak için kullanabileceğiniz [Microsoft Azure portal](https://portal.azure.com/), PowerShell veya Linux komut satırı aracı.  

**Microsoft Azure Portalı'ndan bir depolama hesabı oluşturma**

1. **Yeni**’ye tıklayın.
2. Seçin **depolama**.
3. Depolama hesabı adı girin ve ardından bir konum seçin.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. **Oluştur**'a tıklayın.
5. Oluşturulan depolama hesabı için dikey penceresi açık olması gerekir. Aksi takdirde, seçin **Gözat** > **depolama hesapları**. Depolama hesabı dikey penceresinde, oluşturduğunuz depolama hesabını seçin.
6. Seçin **kapsayıcıları**.
   
   ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. Kapsayıcıları dikey penceresinde, seçin **Ekle**ve ardından bir kapsayıcı adı hem kapsayıcı izinleri girin. Seçin **özel** kapsayıcı izinleri.

> [!TIP]
> Bir kapsayıcıya yayımlamak için planlama SKU başına oluşturmanızı öneririz.
> 
> 

  ![Çizim](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a>PowerShell kullanarak bir depolama hesabı oluşturma
PowerShell kullanarak oluşturduğunuz bir depolama hesabı kullanarak [yeni AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet'i.

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

Ardından kullanarak bu depolama hesabı içindeki bir kapsayıcı oluşturabilirsiniz [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet'i.

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> Bu komutlar, geçerli depolama hesabı bağlamını PowerShell'de zaten ayarlanmış varsayalım.   Başvurmak [Azure PowerShell ayarı](marketplace-publishing-powershell-setup.md) PowerShell kurulumu hakkında daha fazla ayrıntı için.  
> 
> ### <a name="create-a-storage-account-by-using-the-command-line-tool-for-mac-and-linux"></a>Mac ve Linux için komut satırı aracını kullanarak bir depolama hesabı oluşturma
> Gelen [Linux komut satırı aracı](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), aşağıdaki gibi bir depolama hesabı oluşturun.
> 
> 

        azure storage account create mystorageaccount --location "West US"

Şu şekilde bir kapsayıcı oluşturun.

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a>VHD’yi karşıya yükleme
Depolama hesabı ve kapsayıcı oluşturduktan sonra hazırlanan Vhd'lerinizi karşıya yükleyebilirsiniz. PowerShell, Linux komut satırı aracını veya diğer Azure Depolama Yönetimi araçlarını kullanabilirsiniz.

### <a name="upload-a-vhd-via-powershell"></a>Bir VHD PowerShell aracılığıyla karşıya yükleme
Kullanım [Ekle AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet'i.

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-the-command-line-tool-for-mac-and-linux"></a>Mac ve Linux için komut satırı aracını kullanarak bir VHD'yi karşıya yükle
İle [Linux komut satırı aracı](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), aşağıdakileri kullanın: azure vm görüntüsü oluşturma <image name> --konum <Location of the data center> --OS Linux<LocationOfLocalVHD>

## <a name="see-also"></a>Ayrıca bkz.
* [Market bir sanal makine görüntüsü oluşturma](marketplace-publishing-vm-image-creation.md)
* [Azure PowerShell ayarlama](marketplace-publishing-powershell-setup.md)

