---
title: "Azure dosya eşitleme (Önizleme) dağıtma | Microsoft Docs"
description: "Azure dosya eşitleme başlangıçtan bitişe kadar dağıtmayı öğrenin."
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
ms.openlocfilehash: b31b6ae413f72c626e2601ba860aad44ddaa29cd
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-deploy-azure-file-sync-preview"></a>Azure dosya eşitleme (Önizleme) dağıtma
Azure Dosya Eşitleme (önizleme) aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunun için Windows sunucularınızı hızlı bir Azure Dosyaları paylaşım önbelleğine dönüştürür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Okuma tavsiye [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md) ve [bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md) bu kılavuzdaki adımları izleyerek önce.

## <a name="prerequisites"></a>Ön koşullar
* Azure dosya eşitleme dağıtmak istediğiniz aynı bölgede, bir depolama hesabı ve bir Azure dosya paylaşımı. Daha fazla bilgi için bkz.
    - [Bölge kullanılabilirliği](storage-sync-files-planning.md#region-availability) Azure dosya eşitleme
    - [Depolama hesabı oluşturma](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) bir depolama hesabı oluşturma hakkında adım adım yönergeler için ve
    - [Bir dosya paylaşımı oluşturmak](storage-how-to-create-file-share.md) bir dosya paylaşımı oluşturma hakkında adım adım yönergeler için.
* En az bir Windows Server veya Windows Server kümesi Azure dosya eşitleme ile eşitlemek için desteklenir. Bkz: [Windows Server ile birlikte çalışabilirlik](storage-sync-files-planning.md#azure-file-sync-interoperability) Windows Server'ın desteklenen sürümleri hakkında daha fazla bilgi için.

## <a name="deploy-the-storage-sync-service"></a>Depolama eşitleme hizmeti dağıtma 
Depolama eşitleme hizmeti, Azure dosya eşitleme temsil eden üst düzey Azure kaynaktır. Bir depolama eşitleme hizmeti dağıtmak için gidin [Azure portal](https://portal.azure.com/), Azure dosya eşitleme arayın. Arama sonuçlarından "Azure dosya eşitleme (Önizleme)" seçtikten sonra "açık"dağıtmak depolama eşitleme"sekmesini pop oluştur" seçin.

Sonuçta elde edilen dikey için aşağıdaki bilgileri ister:

- **Ad**: depolama eşitleme hizmeti için benzersiz bir ad (her abonelik).
- **Abonelik**: depolama eşitleme hizmeti oluşturulacağı abonelik. Kuruluşunuzun yapılandırma stratejisi bağlı olarak, bir veya daha fazla abonelik erişiminiz olması. Bir Azure aboneliği (örneğin, Azure dosyaları) her bir bulut hizmeti için fatura için en temel kapsayıcıdır.
- **Kaynak grubu**: bir kaynak grubu bir depolama hesabı veya bir depolama eşitleme hizmeti gibi Azure kaynakları mantıksal grubudur. Azure dosya (HR kaynakları veya belirli bir projenin kaynakları gruplandırma gibi mantıksal olarak, kuruluşunuz için kaynakları ayırmak için kullanılan kapsayıcı olarak kaynak gruplarını kullanma önerilir) eşitleme için varolan bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturmak olabilir .
- **Konum**: bölge içinde Azure dosya eşitleme dağıtmak istiyor. Desteklenen bölgeleri bu listede yalnızca kullanılabilir.

"Depolama eşitleme dağıtma" form tamamlandığında depolama eşitleme hizmeti dağıtmak için "Oluştur" seçeneğini tıklatın.

## <a name="prepare-windows-servers-for-use-with-azure-file-sync"></a>Azure dosya eşitleme ile kullanılmak üzere Windows sunucuları hazırlama
İçin hiç Azure dosya eşitleme, bir yük devretme kümesinde sunucu düğümleri dahil olmak üzere kullanmak istediğiniz sunucu, aşağıdaki adımları tamamlayın:

Bir yük devretme kümesinde sunucu düğümleri dahil olmak üzere her sunucu için düşündüğünüz Azure dosya eşitleme ile kullanmak için aşağıdaki adımları tamamlayın:

1. Internet Explorer Artırılmış Güvenlik Yapılandırması devre dışı bırakın. Bu yalnızca ilk sunucu kayıt için gereken ve sunucu sahip olduktan sonra yeniden iler hale kayıtlı.
    1. Sunucu Yöneticisi'ni açın.
    2. Tıklatın **yerel sunucu**:  
        ![Sunucu Yöneticisi'ni UI sol taraftaki "yerel sunucu"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Bağlantısını seçin **IE Artırılmış Güvenlik Yapılandırması** özellikleri alt bölmede:  
        !["IE Artırılmış Güvenlik Yapılandırması Sunucu Yöneticisi Arabiriminde"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. Seçin **kapalı** hem yöneticilerin hem de Internet Explorer Artırılmış Güvenlik Yapılandırması açılır penceresinde kullanıcı:  
        !["Kapalı" ile Internet Explorer Artırılmış Güvenlik Yapılandırması pop-penceresinde seçili](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. En az çalıştığından emin olun PowerShell 5.1.\* (PowerShell 5.1 Windows Server 2016 varsayılandır). PowerShell 5.1 çalışan doğrulayabilirsiniz. \* $PSVersionTable nesnesinin PSVersion özelliğinin değerinde bakarak:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    - PSVersion değerinden 5.1 ise. \*olarak olacaktır çoğu Windows Server 2012 R2 yüklemelerinin durumuyla, indirme ve yükleme kolayca yükseltebilirsiniz [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). İndirmek ve Windows Server 2012 R2 için yüklemek için uygun paket **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

3. [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Her zaman Azure PowerShell modüllerinin en son sürümünü kullanmanızı öneririz.

## <a name="install-the-azure-file-sync-agent"></a>Azure dosya eşitleme Aracısı'nı yükleme
Azure dosya eşitleme Aracısı, Windows Server'ın bir Azure dosyasıyla eşitlenecek bir hangi etkinleştirir paylaşmak indirilebilir bir pakettir. Aracı yüklenebilir [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Yüklendikten sonra VM'deki MSI paketini Azure dosya eşitleme aracı yüklemesini başlatmak için çift tıklayın.

> [!Important]  
> Azure dosya eşitleme ile bir yük devretme kümesi kullanmayı düşünüyorsanız, Azure dosya eşitleme Aracısı kümedeki her düğümde yüklü olması gerekir.

Göreceli olarak, Azure dosya eşitleme aracı yükleme paketi hızla çok fazla ek sormadan yüklemeniz gerekir. Şunları öneririz:
- Bırakın varsayılan yükleme yolunu `C:\Program Files\Azure\StorageSyncAgent`) sorun giderme ve sunucu bakımının basitleştirmek için.
- Azure dosya eşitleme güncel tutmak için Microsoft Update'i etkinleştirme. Özellik güncelleştirmeleri ve düzeltmeleri, Azure dosya eşitleme Aracısı dahil olmak üzere tüm güncelleştirmeleri Microsoft Update sitesinden meydana gelir. Her zaman en son güncelleştirmeyi Azure dosya eşitleme sürüyor öneririz. Lütfen bakın [Azure dosya eşitleme güncelleştirme ilkesi](storage-sync-files-planning.md#azure-file-sync-agent-update-policy) daha fazla bilgi için.

Azure dosya eşitleme aracı yüklemesi sonuç sunucu kayıt UI otomatik başlatma. Lütfen bu sunucuyu Azure dosya eşitleme ile kaydetmek nasıl için sonraki bölüme bakın.

## <a name="register-windows-server-with-storage-sync-service"></a>Windows Server depolama eşitleme hizmetine kaydetme
Windows sunucunuzu bir depolama eşitleme hizmeti ile kaydetme sunucunuz (veya küme) arasında bir güven ilişkisi oluşturur ve depolama eşitleme hizmeti. Sunucu kayıt UI otomatik Azure dosya eşitleme Aracısı yüklendikten sonra başlatması, ancak seçili değilse, onu el ile konumundan açabilirsiniz: `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`. Sunucu kayıt UI açıldıktan sonra tıklatın **oturum açma** başlamak için.

Oturum açma işleminden sonra aşağıdaki bilgileri girmeniz istenir:

![Sunucu kayıt kullanıcı arabiriminin ekran görüntüsü](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure aboneliği**: depolama eşitleme hizmeti içeren abonelik (bkz [depolama eşitleme hizmeti dağıtmak](#deploy-the-storage-sync-service) yukarıda). 
- **Kaynak grubu**: depolama eşitleme hizmeti içeren kaynak grubunu.
- **Depolama eşitleme hizmeti**: depolama eşitleme istediğiniz kaydetmek hizmetin adı.

Açılır listeleri uygun bilgileri seçtikten sonra tıklatın **kaydetmek** sunucu kayıt işlemini tamamlamak için. Kayıt işleminin bir parçası olarak, bir ek oturum açma için istenir.

## <a name="create-a-sync-group"></a>Eşitleme grubu oluşturma
Eşitleme grubu bir Grup dosyası için eşitleme topolojisi tanımlar. Bir eşitleme grubundaki uç noktaları birbirleri ile eşitlenmiş olarak saklanacaktır. Eşitleme grubu, Azure dosya paylaşımı temsil eder, en az bir bulut uç ve bir Windows Server'da bir yol temsil eden bir sunucu uç içermesi gerekir. Bir eşitleme grubu oluşturmak için depolama eşitleme hizmetinizi gidin [Azure portal](https://portal.azure.com/), tıklatıp **+ eşitleme grubu**:

![Azure portalında yeni eşitleme grubu oluştur](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Sonuç bölmesinde bir bulut uç noktası ile bir eşitleme grubu oluşturmak için aşağıdaki bilgileri ister:

- **Eşitleme grubu adı**: Oluşturulacak eşitleme grubunun adı. Bu ad depolama eşitleme hizmeti içinde benzersiz olmalıdır, ancak sizin için mantıklı olan herhangi bir ad olabilir.
- **Abonelik**: Abonelik depolama eşitleme hizmetinde dağıtılan [depolama eşitleme hizmeti dağıtmak](#deploy-the-storage-sync-service) üstünde.
- **Depolama hesabı**: "depolama hesabı seç" tıklandığında pop ile gibi eşitlemek Azure dosya paylaşımı içeren depolama hesabını seçin izin vererek bir ek bölmesi açılır.
- **Azure dosya paylaşımı**: ile gibi eşitlemek Azure dosya paylaşımının adı.

Sunucusu uç noktası eklemek için yeni oluşturulan eşitleme grubuna gidin ve "sunucusu uç noktası Ekle" yi tıklatın.

![Eşitleme grubu Bölmesi'nde yeni bir sunucu uç noktası ekleme](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

Sonuç "sunucusu uç noktası Ekle" bölmesinde sunucusu uç noktası oluşturmak için aşağıdaki bilgileri gerektirir:

- **Sunucusu kayıtlı**: sunucu veya küme sunucusu uç noktası oluşturulacağı adı.
- **Yol**: eşitleme grubunun bir parçası eşitlenecek Windows Server'da yolu.
- **Bulut Katmanlandırma**: hangi etkinleştirir sık kullanılan veya erişilen dosyaları Azure dosyaları katmanlı etkinleştirmek veya Bulut katmanlandırma, devre dışı bırakmak için bir anahtar.
- **Birim boş alanı**: sunucusu uç noktası bulunduğu birimde ayrılan boş alan miktarı. Birim boş alanı, tek bir sunucu uç noktası olan bir birimde % 50'si ayarlanırsa, örneğin, kabaca veri miktarının yarısına Azure dosyaları katmanlı. Bağımsız olarak mı yoksa katmanlama bulut unutmayın etkinleştirildiğinde, Azure dosya paylaşımınızı verilerin tam bir kopyasını eşitleme grubundaki her zaman vardır.

Sunucusu uç noktası eklemek için "Oluştur" seçeneğini tıklatın. Dosyalarınızı şimdi, Azure dosya paylaşımı ve Windows Server'ınızın arasında eşitleme tutulacak. 

> [!Important]  
> Herhangi bir bulut veya sunucusu uç noktası eşitleme grubundaki değişiklik yapabilirsiniz ve dosyalarınızı diğer uç noktalarına eşitleme grubundaki eşitlenmemiş. (Azure dosya paylaşımı) bulut uç noktasına doğrudan bir değişiklik yaparsanız değişiklikleri ilk 24 saatte bir kez yalnızca initatiated bulut uç noktası için olan bir Azure dosya eşitleme değişiklik algılama işi tarafından bulunacak gerektiğini unutmayın. Bkz: [Azure dosyaları ile ilgili SSS](storage-files-faq.md#afs-change-detection) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure dosya eşitleme sunucusu uç noktası Ekle/Kaldır](storage-sync-files-server-endpoint.md)
- [Azure dosya eşitleme sunucusuyla kaydı/kaydını kaldırmasına](storage-sync-files-server-registration.md)