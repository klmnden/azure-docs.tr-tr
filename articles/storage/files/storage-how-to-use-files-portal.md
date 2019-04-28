---
title: Azure portal ile Azure dosya paylaşımlarını yönetme hızlı başlangıcı
description: Azure portaldan Azure Dosyaları'nı yönetmeyi öğrenmek için bu hızlı başlangıcı kullanın.
services: storage
author: roygara
ms.service: storage
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 69e7dba06fce14c2411a82dfade344ba1b14a6d3
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63763861"
---
# <a name="quickstart-create-and-manage-azure-file-shares-with-the-azure-portal"></a>Hızlı Başlangıç: Oluşturma ve Azure portalıyla Azure dosya paylaşımlarını yönetme 
[Azure Dosyaları](storage-files-introduction.md), Microsoft’un kullanımı kolay bulut dosya sistemidir. Azure dosya paylaşımları, Windows, Linux ve macOS platformlarına bağlanabilir. Bu kılavuzda, [Azure portalını](https://portal.azure.com/) kullanarak Azure dosya paylaşımlarıyla çalışmanın temel bilgileri gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Azure dosya paylaşımı oluşturmak için:

1. Panonuzdan depolama hesabını seçin.
2. Depolama hesabı sayfasında, **Hizmetler** bölümünden **Dosyalar**’ı seçin.
    ![Depolama hesabının hizmetler bölümünün anlık görüntüsü; Dosyalar hizmetini seçin](media/storage-how-to-use-files-portal/create-file-share-1.png)

3. **Dosya hizmeti** sayfasının üst kısmındaki menüden **+ Dosya paylaşımı**’na tıklayın. **Yeni dosya paylaşımı** sayfası aşağı doğru açılır.
4. **Ad** alanına *myshare* yazın.
5. **Tamam**’a tıklayarak Azure dosya paylaşımını oluşturun.

Paylaşım adları tümüyle küçük harf, sayı ve tek kısa çizgilerden oluşmalı ve kısa çizgiyle başlamamalıdır. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata)

## <a name="use-your-azure-file-share"></a>Azure dosya paylaşımınızı kullanma
Azure Dosyaları, Azure dosya paylaşımınızdaki dosya ve klasörler ile çalışmak için iki yöntem sunar: sektör standardı [Sunucu İleti Bloğu (SMB) protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) ve [Dosya REST protokolü](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api). 

Bir dosya paylaşımını SMB ile bağlayabilmeniz için işletim sisteminize göre aşağıdaki belgeye bakın:
- [Windows](storage-how-to-use-files-windows.md)
- [Linux](storage-how-to-use-files-linux.md)
- [macOS](storage-how-to-use-files-mac.md)

### <a name="using-an-azure-file-share-from-the-azure-portal"></a>Azure portalda Azure dosya paylaşımını kullanma
Azure portalı üzerinde gelen tüm istekler Dosya REST API ile yapılır; böylelikle SMB erişimi olmadan istemcilerdeki dosyaları ve dizinleri oluşturabilir, değiştirebilir ve silebilirsiniz. Doğrudan dosya REST protokolü ile olası çalışma doğrudan olduğu (yani REST HTTP handcrafting kendiniz çağrıları), ancak en yaygın yolu (ötesinde, Azure portalını kullanarak) dosya REST protokolü kullanmak için kullanmaktır [Azure PowerShell Modülü](storage-how-to-use-files-powershell.md), [Azure CLI](storage-how-to-use-files-cli.md), veya dosya REST protokolü, tercih ettiğiniz betik programlama dilinde geçici güzel bir sarmalayıcı tümü sağlamak bir Azure depolama SDK. 

Kullanabilmeyi umdukları mevcut uygulama ve araçlarını kullanmalarına izin vereceği için Azure Dosyaları kullanıcılarının çoğunluğunun Azure dosya paylaşımları ile SMP protokolü üzerinden çalışmasını bekliyoruz, ancak SMB yerine Dosya REST API'si kullanmanın aşağıdaki gibi bazı avantajları bulunmaktadır:

- SMB erişiminizin olmadığı bir dizüstü bilgisayar, tablet veya mobil cihaz vb. ile hareket halindeyken Azure dosya paylaşımınızda hızlı bir değişiklik yapmanız gerekir.
- SMB paylaşımını bağlayamayan istemcilerden; örneğin 445 numaralı bağlantı noktası engeli kaldırılmamış şirket içi bir istemciden bir betik veya uygulama yürütmeniz gerekiyorsa.
- [Azure İşlevleri](../../azure-functions/functions-overview.md) gibi sunucusuz kaynaklardan yararlanıyorsanız. 

Aşağıdaki örnekler, Azure portalın Azure dosya paylaşımınızı Dosya REST protokolü ile yönetmek için nasıl kullanılacağını göstermektedir. 

Artık Azure dosya paylaşımını oluşturduğunuza göre, [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) veya [macOS](storage-how-to-use-files-mac.md)'ta SMB ile dosya paylaşımını bağlayabilirsiniz. Alternatif olarak, Azure dosya paylaşımınızla Azure portalını kullanarak çalışabilirsiniz. 

#### <a name="create-a-directory"></a>Dizin oluşturma
Azure dosya paylaşımınızın kökünde *myDirectory* adlı yeni bir dizin oluşturmak için:

1. **Dosya Hizmeti** sayfasında **myshare** dosya paylaşımını seçin. Dosya paylaşımınızın sayfası açılır.
2. Sayfanın en üstündeki menüden **+ Dizin ekle**’yi seçin. **Yeni dizin** sayfası aşağı doğru açılır.
3. *myDirectory* yazın ve **Tamam**’a tıklayın.

#### <a name="upload-a-file"></a>Dosyayı karşıya yükleme 
Bir dosyayı karşıya yüklemeyi göstermek için önce karşıya yüklenecek bir dosya oluşturmanız veya seçmeniz gerekir. Uygun gördüğünüz herhangi bir yolla bunu yapabilirsiniz. Karşıya yüklemek istediğiniz dosyayı seçtikten sonra:

1. **myDirectory** dizinine tıklayın. **myDirectory** paneli açılır.
2. En üstteki menüde **Karşıya Yükle**’ye tıklayın. **Dosyaları karşıya yükleme** paneli açılır.  
    ![Dosyaları karşıya yükleme panelinin ekran görüntüsü](media/storage-how-to-use-files-portal/upload-file-1.png)

3. Yerel dosyalarınıza göz atmak amacıyla bir pencereyi açmak için klasör simgesine tıklayın. 
4. Bir dosya seçin ve **Aç**’a tıklayın. 
5. **Dosyaları karşıya yükleme** sayfasında, dosya adını doğrulayın ve **Karşıya Yükle**’ye tıklayın.
6. Tamamlandığında, dosyanın **myDirectory** sayfasındaki listede gösterilmesi gerekir.

#### <a name="download-a-file"></a>Dosya indirme
Dosyaya sağ tıklayarak, karşıya yüklediğiniz dosyanın bir kopyasını indirebilirsiniz. İndir düğmesine tıklandıktan sonra olacak işlemler, kullandığınız işletim sistemine ve tarayıcıya bağlıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme
[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Dosyaları nedir?](storage-files-introduction.md)
