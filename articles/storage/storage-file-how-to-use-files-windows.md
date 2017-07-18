---
title: "Azure Dosya paylaşımını bağlama ve Windows’da paylaşıma erişme | Microsoft Docs"
description: "Azure Dosya paylaşımını bağlayın ve Windows’da paylaşıma erişin."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: a6d3a6f6e3457c84c5a7dc7d3601ef9495c060fe
ms.contentlocale: tr-tr
ms.lasthandoff: 07/12/2017

---

# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a>Azure Dosya paylaşımını bağlama ve Windows’da paylaşıma erişme
[Azure Dosya depolama](storage-dotnet-how-to-use-files.md), Windows’un kolay kullanılan bulut dosya sistemidir. Azure Dosya paylaşımları, Windows ve Windows Server’a bağlanabilir. Bu makale Windows’da Azure Dosya paylaşımının üç farklı yolla bağlanmasını gösterir: Dosya Gezgini kullanıcı arabirimi ile, Powershell ve Komut İstemi aracılığıyla. 

Bir Azure Dosya paylaşımını, barındırıldığı Azure bölgesinin dışında bağlamak için (örneğin, şirket içinde veya farklı bir Azure bölgesinde) işletim sisteminin SMB 3.x’i desteklemesi gerekir. Aşağıdaki tablo en son Windows sürümlerinin SMB sürümlerini gösterir:

| Windows sürümü | SMB sürümü | Azure VM’den bağlamayı destekler | Şirket içinden bağlamayı destekler | Önerilen En Az KB |
|----|----|----|----|----|
| Windows 10 sürüm 1703 | SMB 3.1.1 | Evet | Evet | |
| Windows Server 2016 | SMB 3.1.1 | Evet | Evet | [KB4015438](https://support.microsoft.com/help/4015438) |
| Windows 10 sürüm 1607 | SMB 3.1.1 | Evet | Evet | [KB4015438](https://support.microsoft.com/help/4015438) | 
| Windows 10 sürüm 1511 | SMB 3.1.1 | Evet | Evet | [KB4013198](https://support.microsoft.com/help/4013198) |
| Windows 10 sürüm 1507 | SMB 3.1.1 | Evet | Evet | [KB4012606](https://support.microsoft.com/help/4012606) | 
| Windows 8.1 | SMB 3.0.2 | Evet | Evet | [KB4012216](https://support.microsoft.com/help/4012216) |
| Windows Server 2012 R2 | SMB 3.0.2 | Evet | Evet | [KB4012216](https://support.microsoft.com/help/4012216) |
| Windows Server 2012 | SMB 3.0 | Evet | Evet | [KB4012214](https://support.microsoft.com/help/4012214) |
| Windows 7 | SMB 2.1 | Evet | Hayır | [KB4012215](https://support.microsoft.com/help/4012215) |
| Windows Server 2008 R2 | SMB 2.1 | Evet | Hayır | [KB4012215](https://support.microsoft.com/help/4012215) |

> [!Note]  
> Her zaman Windows sürümünüz için en yeni KB’yi almanızı öneririz. Önerilen en az KB, güncelleştirme karşıtı BT yöneticileri için SMB düzeltmelerini içeren en son paketi sağlamaya yöneliktir.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Windows ile Azure Dosya Paylaşımını bağlama önkoşulları 
* **Depolama Hesabı Adı**: Azure Dosya paylaşımını bağlayabilmeniz için depolama hesabınızın adı gerekir.

* **Depolama Hesabı Anahtarı**: Azure Dosya paylaşımını bağlayabilmeniz için birincil (veya ikincil) depolama anahtarı gerekir. SAS anahtarları şu an bağlama için desteklenmemektedir.

* **Bağlantı noktası 445’in açık olduğundan emin olun**: Azure Dosya depolama SMB protokolünü kullanır. SMB, TCP bağlantı noktası 445 üstünden iletişim kurar. İstemci makinenizde güvenlik duvarının TCP bağlantı noktaları 445’i engellemediğinden emin olun.

## <a name="mount-the-azure-file-share-with-file-explorer"></a>Azure Dosya paylaşımını Dosya Gezgini ile bağlama
> [!Note]  
> Aşağıdaki yönergelerin Windows 10’da gösterildiğini ve eski sürümlerde biraz değişiklik gösterebileceğini aklınızda bulundurun. 

1. **Dosya Gezgini’ni açın**: Başlat Menüsünden veya Win+E kısayoluna basarak açılabilir.

2. **Pencerenin sol tarafındaki “Bu Bilgisayar” öğesine gidin. Bu, şeritteki kullanılabilir menüleri değiştirir. Bilgisayar menüsünün altındaki “Ağ Sürücüsüne Bağlan”ı** seçin.
    
    ![“Ağ Sürücüsüne Bağlan” açılan menüsünün ekran görüntüsü](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Azure portalının “Bağlan” bölmesindeki UNC adını kopyalayın**: Bu bilgiyi nasıl bulacağınıza ilişkin ayrıntılı açıklamayı [burada](storage-file-how-to-use-files-portal.md#connect-to-file-share) bulabilirsiniz.

    ![Azure Dosya depolamanın Bağlan bölmesinden UNC adı](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. **Sürücü harfini seçin ve UNC adını girin.** 
    
    ![“Ağ Sürücüsüne Bağlan” iletişim kutusunun ekran görüntüsü](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. **Kullanıcı adı olarak, başına `Azure\` ekleyip Depolama Hesabı Adını ve parola olarak Depolama Hesabı Anahtarını kullanın.**
    
    ![Ağ kimlik bilgileri iletişim kutusunun ekran görüntüsü](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Azure Dosya paylaşımını istediğiniz gibi kullanın**.
    
    ![Azure Dosya paylaşımı artık bağlanmıştır](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. **Azure Dosya paylaşımını çıkarmaya (veya bağlantısını kesmeye) hazır olduğunuzda, Dosya Gezgini’ndeki “Ağ konumları”nın altında bulunan girdiye sağ tıklayıp “Bağlantıyı Kes”i seçerek bunu yapabilirsiniz**.

## <a name="mount-the-azure-file-share-with-powershell"></a>Azure Dosya paylaşımı PowerShell ile bağlama
1. **Azure Dosya paylaşımını bağlamak için aşağıdaki komutları kullanın**: `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` öğelerini uygun bilgilerle değiştirmeyi unutmayın.

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. **Azure Dosya paylaşımını istediğiniz gibi kullanın**.

3. **İşiniz bittiğinde, aşağıdaki komutu kullanarak Azure Dosya paylaşımını çıkarabilirsiniz**.

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> `New-PSDrive` üzerinde `-Persist` parametresini kullanarak, Azure Dosya paylaşımını bağlıyken işletim sisteminin geri kalanında görünür duruma getirebilirsiniz.

## <a name="mount-the-azure-file-share-with-command-prompt"></a>Azure Dosya paylaşımı Komut İstemi ile bağlama
1. **Azure Dosya paylaşımını bağlamak için aşağıdaki komutları kullanın**: `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` öğelerini uygun bilgilerle değiştirmeyi unutmayın.

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **Azure Dosya paylaşımını istediğiniz gibi kullanın**.

3. **İşiniz bittiğinde, aşağıdaki komutu kullanarak Azure Dosya paylaşımını çıkarabilirsiniz**.

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> Windows’daki kimlik bilgilerini kalıcı hale getirerek Azure Dosya paylaşımını yeniden başlatıldığında otomatik olarak yeniden bağlanacak şekilde yapılandırabilirsiniz. Aşağıdaki komut, kimlik bilgilerini kalıcı hale getirir:
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>Sonraki adımlar
Azure File Storage hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın.

* [SSS](storage-files-faq.md)
* [Sorun giderme](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Kavramsal makaleler ve videolar
* [Azure Dosya depolama: Windows ve Linux için uyumlu bulut SMB dosya sistemi](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Azure Dosya depolamayı Linux ile kullanma](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Azure Dosya depolama için araç desteği
* [Azure Depolama ile Azure PowerShell’i kullanma](storage-powershell-guide-full.md)
* [Microsoft Azure Depolama ile AzCopy kullanma](storage-use-azcopy.md)
* [Azure Depolama ile Azure CLI kullanma](storage-azure-cli.md#create-and-manage-file-shares)
* [Azure Dosya depolama sorunlarını giderme](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a>Blog yazıları
* [Azure Dosya Depolama genel kullanıma sunulmuştur](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Azure Dosya depolama incelemesi](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Microsoft Azure Dosya Hizmeti’ne Giriş](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Azure Dosya Hizmeti’ne verileri geçirme ](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Başvuru
* [.NET başvurusu için Depolama İstemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Dosya Hizmeti REST API başvurusu](http://msdn.microsoft.com/library/azure/dn167006.aspx)
