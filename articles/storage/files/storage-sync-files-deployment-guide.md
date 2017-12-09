---
title: "Azure dosya eşitleme (Önizleme) dağıtma | Microsoft Docs"
description: "Başlangıçtan bitişe kadar Azure dosya eşitleme dağıtmayı öğrenin."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 7b4de3e7b7e98ab76c02ea7c1cf069cee94706fc
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-file-sync-preview"></a>Azure dosya eşitleme (Önizleme) dağıtma
Esneklik, performans ve uyumluluk bir şirket içi dosya sunucusunun tanırken kuruluşunuzun dosya paylaşımları Azure dosyalarında merkezileştirmek için Azure dosya eşitleme (Önizleme) kullanın. Azure dosya eşitleme, Windows Server Hızlı Azure dosya paylaşımınıza önbelleğine dönüştürür. SMB ve NFS FTPS çeşitli verilerinize yerel olarak erişmek için Windows Server üzerinde kullanılabilir herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gerektiği kadar önbellekleri olabilir.

Okumanızı öneririz [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md) ve [bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md) önce bu makalede açıklanan adımları tamamlayın.

## <a name="prerequisites"></a>Ön koşullar
* Azure dosya eşitleme dağıtmak istediğiniz aynı bölgede bir Azure Storage hesabını ve Azure dosya paylaşın. Daha fazla bilgi için bkz.
    - [Bölge kullanılabilirliği](storage-sync-files-planning.md#region-availability) Azure dosya eşitleme için.
    - [Depolama hesabı oluşturma](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) bir depolama hesabının nasıl oluşturulacağı hakkında adım adım açıklaması.
    - [Bir dosya paylaşımı oluşturmak](storage-how-to-create-file-share.md) nasıl bir dosya paylaşımı oluşturmak adım adım açıklaması.
* Windows Server veya Windows Server kümesi Azure dosya eşitleme ile eşitlemek için en az bir desteklenen örneği. Windows Server'ın desteklenen sürümleri hakkında daha fazla bilgi için bkz: [Windows Server ile birlikte çalışabilirlik](storage-sync-files-planning.md#azure-file-sync-interoperability).

## <a name="deploy-the-storage-sync-service"></a>Depolama eşitleme hizmeti dağıtma 
Depolama eşitleme Azure dosya eşitleme için üst düzey Azure kaynağı hizmetidir. Bir depolama eşitleme hizmeti dağıtmak için Git [Azure portal](https://portal.azure.com/)ve Azure dosya eşitleme için arama yapın. Arama sonuçlarında seçin **Azure dosya eşitleme (Önizleme)**ve ardından **oluşturma** açmak için **dağıtmak depolama eşitleme** sekmesi.

Açılan bölmesinde, aşağıdaki bilgileri girin:

- **Ad**: depolama eşitleme hizmeti için benzersiz bir ad (her abonelik).
- **Abonelik**: depolama eşitleme hizmeti oluşturmak istediğiniz aboneliği. Kuruluşunuzun yapılandırma stratejisi bağlı olarak, bir veya daha fazla abonelik erişiminiz olması. Bir Azure aboneliği (örneğin, Azure dosyaları) her bir bulut hizmeti için fatura için en temel kapsayıcıdır.
- **Kaynak grubu**: bir kaynak grubu bir depolama hesabı veya bir depolama eşitleme hizmeti gibi Azure kaynakları mantıksal grubudur. Yeni bir kaynak grubu oluşturabilir veya varolan bir kaynak grubu Azure dosya eşitleme için kullanabilirsiniz. (Kaynak grupları kapsayıcılar olarak HR kaynakları veya belirli bir proje için kaynakları gruplandırma gibi mantıksal olarak, kuruluşunuz için kaynaklar yalıtmak için kullanmanızı öneririz.)
- **Konum**: Azure dosya eşitleme dağıtmak istediğiniz bölgesini. Desteklenen bölgeleri bu listede yalnızca kullanılabilir.

İşiniz bittiğinde, seçin **oluşturma** depolama eşitleme hizmeti dağıtmak için.

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Windows Server'ın Azure dosya eşitleme ile kullanmak için hazırlama
Bir yük devretme kümesinde sunucu düğümleri dahil olmak üzere Azure dosya eşitleme ile kullanmak istediğiniz her sunucu için aşağıdaki adımları tamamlayın:

1. Devre dışı **Internet Explorer Artırılmış Güvenlik Yapılandırması**. Bu, yalnızca ilk sunucu kayıt için gereklidir. Sunucu kaydedildikten sonra yeniden etkinleştirebilirsiniz.
    1. Sunucu Yöneticisi'ni açın.
    2. Tıklatın **yerel sunucu**:  
        ![Sunucu Yöneticisi kullanıcı Arabirimi sol tarafındaki "yerel sunucu"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Üzerinde **özellikleri** subpane, bağlantısını seçin **IE Artırılmış Güvenlik Yapılandırması**.  
        ![Sunucu Yöneticisi kullanıcı Arabirimi "IE Artırılmış Güvenlik Yapılandırması" bölmesinde](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. İçinde **Internet Explorer Artırılmış Güvenlik Yapılandırması** iletişim kutusunda **kapalı** için **Yöneticiler** ve **kullanıcılar**:  
        !["Kapalı" ile Internet Explorer Artırılmış Güvenlik Yapılandırması pop-penceresinde seçili](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. En az çalıştığından emin olun PowerShell 5.1.\* (PowerShell 5.1 Windows Server 2016 varsayılandır). PowerShell 5.1 çalıştığını doğrulayabilirsiniz. \* değeri, bakarak **PSVersion** özelliği **$PSVersionTable** nesnesi:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    PSVersion değeriniz değerinden 5.1 ise. \*olarak olacaktır çoğu Windows Server 2012 R2 yüklemelerinin durumuyla, indirme ve yükleme kolayca yükseltebilirsiniz [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). İndirmek ve Windows Server 2012 R2 için yüklemek için uygun paket **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

3. [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Azure PowerShell modüllerinin en son sürümünü kullanmanızı öneririz.

## <a name="install-the-azure-file-sync-agent"></a>Azure dosya eşitleme Aracısı'nı yükleme
Azure dosya eşitleme Aracısı'nı Windows Server'ın bir Azure dosya paylaşımı ile senkronize sağlayan indirilebilir bir pakettir. Aracısı'ndan yükleyebilirsiniz [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Yükleme tamamlandığında, Azure dosya eşitleme aracı yüklemesini başlatmak için MSI paketini çift tıklatın.

> [!Important]  
> Azure dosya eşitleme ile bir yük devretme kümesi kullanmayı planlıyorsanız, kümedeki her düğümde Azure dosya eşitleme Aracısı yüklenmesi gerekir.

Azure dosya eşitleme aracı yükleme paketi oldukça hızlı bir şekilde ve çok fazla ek sorulmadan yüklemeniz gerekir. Şunları yapmanız önerilir:
- Sorun giderme ve sunucu bakımının basitleştirmek için varsayılan yükleme yolunu (C:\Program Files\Azure\StorageSyncAgent) bırakın.
- Azure dosya eşitleme güncel tutmak için Microsoft Update'e etkinleştirin. Tüm güncelleştirmeleri özelliği güncelleştirmeler ve düzeltmeler de dahil olmak üzere Azure dosya eşitleme aracısı için Microsoft Update sitesinden oluşur. Azure dosya eşitleme için en son güncelleştirmeyi yüklemenizi öneririz. Daha fazla bilgi için bkz: [Azure dosya eşitleme güncelleştirme ilkesi](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Azure dosya eşitleme aracı yüklemesi tamamlandığında, sunucu kayıt UI otomatik olarak açılır. Bu sunucuyu Azure dosya eşitleme ile kaydetmek öğrenmek için sonraki bölüme bakın.

## <a name="register-windows-server-with-storage-sync-service"></a>Windows Server depolama eşitleme hizmetine kaydetme
Windows Server bir depolama eşitleme hizmeti ile kaydetme sunucunuz (veya küme) arasında bir güven ilişkisi oluşturur ve depolama eşitleme hizmeti. Sunucu kayıt UI Azure dosya eşitleme Aracısı yüklendikten sonra otomatik olarak açmanız gerekir. Seçili değilse, onu el ile dosya konumundan açabilirsiniz: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Sunucu kayıt UI açıldığında seçin **oturum açma** başlamak için.

Oturum açtıktan sonra aşağıdaki bilgileri girmeniz istenir:

![Sunucu kayıt kullanıcı arabiriminin ekran görüntüsü](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure aboneliği**: depolama eşitleme hizmeti içeren abonelik (bkz [depolama eşitleme hizmeti dağıtmak](#deploy-the-storage-sync-service)). 
- **Kaynak grubu**: depolama eşitleme hizmeti içeren kaynak grubu.
- **Depolama eşitleme hizmeti**: depolama eşitleme istediğiniz kaydetmek hizmetin adı.

Uygun bilgileri seçtikten sonra Seç **kaydetmek** sunucu kayıt işlemini tamamlamak için. Kayıt işleminin bir parçası olarak, bir ek oturum açma için istenir.

## <a name="create-a-sync-group"></a>Eşitleme grubu oluşturma
Eşitleme grubu bir Grup dosyası için eşitleme topolojisi tanımlar. Bir eşitleme grubundaki uç noktaları birbirleri ile eşitlenmiş tutulur. Eşitleme grubu, Azure dosya paylaşımının temsil eder, en az bir bulut uç noktası ve Windows Server'da bir yol temsil eden bir sunucu uç nokta içermesi gerekir. Bir eşitleme grubu oluşturmak için [Azure portal](https://portal.azure.com/)depolama eşitleme hizmetinize gidin ve ardından **+ eşitleme grubu**:

![Azure portalında yeni bir eşitleme grubu oluşturma](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Açılır bölmesinde, bir bulut uç noktası ile bir eşitleme grubu oluşturmak için aşağıdaki bilgileri girin:

- **Eşitleme grubu adı**: Oluşturulacak eşitleme grubunun adı. Bu ad depolama eşitleme hizmeti içinde benzersiz olmalıdır, ancak sizin için mantıklı olan herhangi bir ad olabilir.
- **Abonelik**: depolama eşitleme hizmetinin dağıtıldığı abonelik [depolama eşitleme hizmeti dağıtmak](#deploy-the-storage-sync-service).
- **Depolama hesabı**: seçerseniz **depolama hesabını seçin**, eşitlemek istediğiniz Azure dosya paylaşımı olan depolama hesabını seçebileceğiniz başka bir bölmesinde görünür.
- **Azure dosya paylaşımı**: istediğiniz eşitleme Azure dosya paylaşımı adı.

Sunucusu uç noktası eklemek için yeni oluşturulan eşitleme grubuna gidin ve ardından **sunucusu uç noktası ekleme**.

![Eşitleme grubu Bölmesi'nde yeni bir sunucu uç noktası ekleme](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

İçinde **sunucusu uç noktası ekleme** bölmesinde, bir sunucu uç noktası oluşturmak için aşağıdaki bilgileri girin:

- **Kayıtlı sunucu**: sunucu veya sunucu uç noktası oluşturmak istediğiniz kümenin adını.
- **Yol**: Windows Server yolu eşitleme grubunun bir parçası eşitlenir.
- **Bulut Katmanlandırma**: etkinleştirmek veya Bulut devre dışı bırakmak için bir anahtar katmanlama. Katmanlama, sık kullanılan veya erişilen dosyaları bulut ile Azure dosyaları katmanlı.
- **Birim boş alanı**: sunucusu uç noktası olduğu bulunduğu birimde ayrılan boş alan miktarı. Birim boş alanı, bir tek sunucu uç noktası bir birimi üzerinde % 50'si ayarlanırsa, örneğin, kabaca veri miktarının yarısına Azure dosyaları katmanlı. Bakılmaksızın olup bulut katmanlama etkinleştirildiğinde, Azure dosya paylaşımınıza verilerin tam bir kopyasını eşitleme grubunda her zaman vardır.

Sunucusu uç noktası eklemek için seçin **oluşturma**. Dosyalarınızı artık Azure dosya paylaşımı ve Windows Server arasında eşitleme tutulur. 

> [!Important]  
> Herhangi bir bulut uç noktası veya eşitleme grubundaki sunucusu uç noktası için değişiklik yapabilirsiniz ve dosyalarınızı eşitleme grubundaki diğer uç noktalarına eşitlendiğinden. (Azure dosya paylaşımı) bulut uç noktasına doğrudan bir değişiklik yaparsanız değişiklikleri ilk Azure dosya eşitleme değişiklik algılama işi tarafından bulunmaları gerekir. Bir değişiklik algılama işi yalnızca bir kez her 24 saatte bir bulut uç noktası için başlatılır. Daha fazla bilgi için bkz: [Azure sık sorulan sorular dosyaları](storage-files-faq.md#afs-change-detection).

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>DFS Çoğaltma (DFS-R) dağıtımı için Azure dosya eşitleme geçirme
Azure dosya eşitleme DFS-R dağıtım geçirmek için:

1. Değiştirdiğiniz DFS-R topoloji temsil etmek için bir eşitleme grubu oluşturun.
2. Tam veri geçirmek için DFS-R topolojinizi ayarlanmış sunucuda başlatın. Azure dosya eşitleme bu sunucuya yükleyin.
3. Bu sunucuyu kaydetmek ve yükseltilecek ilk sunucu için bir sunucu uç noktası oluşturur. Bulut etkinleştirmeyin katmanlama.
4. Tüm veri eşitleme, Azure dosya paylaşımına (bulut uç noktası) sağlar.
5. Yükleyin ve Azure dosya eşitleme Aracısı kalan DFS-R sunucuların her birinde kaydedin.
6. DFS-r devre dışı bırak 
7. Sunucusu uç noktası DFS-R sunucuların her birinde oluşturun. Bulut etkinleştirmeyin katmanlama.
8. Eşitleme işlemi tamamlandıktan ve topolojinizi istediğiniz gibi test emin olun.
9. DFS-r devre dışı bırakma
10. Katmanlama may bulut şimdi etkin olması istediğiniz gibi herhangi bir sunucu uç nokta üzerinde.

Daha fazla bilgi için bkz: [Azure dosya eşitleme birlikte çalışabilirliği dağıtılmış dosya sistemi (DFS) ile](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>Sonraki adımlar
- [Azure dosya eşitleme sunucusu uç noktası Ekle Kaldır](storage-sync-files-server-endpoint.md)
- [Kaydedilemedi veya Azure dosya eşitleme sunucusuyla kaydı silinemedi](storage-sync-files-server-registration.md)
