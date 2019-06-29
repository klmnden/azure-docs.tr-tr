---
title: Azure Otomasyonu ile makinelerinizde yüklü olan yazılımları keşfetme | Microsoft Docs
description: Ortamınızdaki makinelerde yüklü olan yazılımları keşfetmek için Stok özelliğinden faydalanın.
services: automation
keywords: stok, otomasyon, değişiklik, izleme
author: jennyhunter-msft
ms.author: jehunte
ms.date: 04/11/2018
ms.topic: tutorial
ms.service: automation
ms.subservice: change-inventory-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 47313781756e460a8c30638661489874481b88a0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476831"
---
# <a name="discover-what-software-is-installed-on-your-azure-and-non-azure-machines"></a>Azure ve Azure harici makinelerinizde yüklü olan yazılımları keşfetme

Bu öğreticide ortamınızda yüklü olan yazılımları keşfetmeyi öğreneceksiniz. Yazılımlar, dosyalar, Linux daemon'ları, Windows hizmetleri ve Windows kayıt defteri anahtarlarıyla ilgili stok durumunu sorgulayabilir ve görüntüleyebilirsiniz. Makinelerinizin yapılandırmasını izlemek ortamınızdaki işletimsel sorunları bulmanıza ve makinelerinizin durumunu daha iyi anlamanıza yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Çözümü etkinleştirme
> * Bir Azure VM ekleme
> * Azure olmayan bir VM ekleme
> * Yüklü olan yazılımları görüntüleme
> * Yüklü olan yazılımların stok günlüklerinde arama yapma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* İzleyiciyi, eylem runbook'larını ve İzleyici Görevi'ni barındıracak bir [Otomasyon Hesabı](automation-offering-get-started.md).
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok özelliklerini etkinleştirme

Bu öğreticide ilk yapmanız gereken Değişiklik İzleme ve Stok özelliklerini etkinleştirmektir. Daha önce **Değişiklik İzleme** çözümünü etkinleştirdiyseniz bu adımı atlayabilirsiniz.

Otomasyon Hesabınıza gidin ve **YAPILANDIRMA YÖNETİMİ** altında **Stok**’u seçin.

Çözümü etkinleştirmek için Log Analytics çalışma alanını ve Otomasyon Hesabını seçip **Etkinleştir**’e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Stok devreye alma yapılandırma başlığı](./media/automation-tutorial-installed-software/enableinventory.png)

Çözümü etkinleştirmek için, kullanılacak konumu, Log Analytics çalışma alanını ve Otomasyon Hesabını yapılandırdıktan sonra **Etkinleştir**’e tıklayın. Bu alanların gri renkte olması, VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon Hesabının kullanılması gerekir.

[Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı, Stok gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır.
Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir.
Çözüm etkinleştirildikten sonra VM'de yüklü yazılımlar ve yapılan değişiklikler hakkında bilgi için Azure İzleyici günlüklerine akar.
Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="onboard-a-vm"></a>VM ekleme

Otomasyon Hesabınızda, **YAPILANDIRMA YÖNETİMİ** altında **Stok**’a gidin.

**+ Azure VM ekle**’yi seçin, bu **Sanal makineler** sayfasını açarak listeden mevcut bir VM seçmenize olanak sağlar. Eklemek istediğiniz sanal makineyi seçin. Açılan sayfada, VM’de çözümü etkinleştirmek için **Etkinleştir**’e tıklayın. Microsoft Yönetim Aracısı VM’ye dağıtılır ve aracıyı çözümü etkinleştirirken yapılandırdığınız Log Analytics çalışma alanıyla iletişim kurması için yapılandırır. Ekleme işleminin tamamlanması birkaç dakika sürebilir. Bu noktada, listeden yeni bir VM seçebilir ve başka bir VM ekleyebilirsiniz.

## <a name="onboard-a-non-azure-machine"></a>Azure olmayan bir makine ekleme

Azure olmayan makineler eklemek için, işletin sisteminize bağlı olarak [Windows](../azure-monitor/platform/agent-windows.md) veya [Linux](automation-linux-hrw-install.md) için aracıyı yükleyin. Aracı yüklendiğinde, Otomasyon Hesabınıza gidin ve **YAPILANDIRMA YÖNETİMİ** altında **Stok**’a gidin. **Makineleri Yönet**’e tıkladığınızda, Log Analytics çalışma alanınıza raporlayan çözüm etkinleştirilmemiş makinelerin bir listesini görürsünüz. Ortamınız için uygun seçeneği belirleyin.

* **Kullanılabilir tüm makinelerde etkinleştir** - Bu seçenek, çözümü şu anda Log Analytics çalışma alanınıza raporlayan tüm makinelerde etkinleştirir.
* **tüm kullanılabilir makinelerde ve gelecekteki makinelerde etkinleştir** - Bu seçenek, çözümü Log Analytics çalışma alanınıza raporlayan tüm makinelerde ve daha sonra çalışma alanına eklenen tüm gelecek makinelerde etkinleştirir.
* **Seçili makinelerde etkinleştir** - Bu seçenek çözümü yalnızca seçtiğiniz makinelerde etkinleştirir.

![Makineleri Yönetme](./media/automation-tutorial-installed-software/manage-machines.png)

## <a name="view-installed-software"></a>Yüklü olan yazılımları görüntüleme

Değişiklik izleme ve Stok çözümünü etkinleştirdikten sonra sonuçları **Stok** sayfasında görüntüleyebilirsiniz.

Otomasyon Hesabınızdan, **YAPILANDIRMA YÖNETİMİ** altında **Stok**’u seçin.

**Stok** sayfasında **Yazılım** sekmesine tıklayın.

**Yazılım** sekmesinde bulunan yazılımların listelendiği bir tablo yer alır. Yazılımlar ada ve sürüme göre gruplara ayrılmıştır.

Tabloda her yazılım kaydı hakkındaki üst düzey ayrıntılar görüntülenebilir. Bu ayrıntılar arasında yazılım adı, sürümü, yayımcı, son yenileme zamanı (grup içindeki bir makine tarafından bildirilen en son yenileme zamanı) ve makine sayısı (bu yazılıma sahip makinelerin sayısı) yer alır.

![Yazılım stoku](./media/automation-tutorial-installed-software/inventory-software.png)

Yazılım kaydının özelliklerini ve bu yazılımın yüklü olduğu makinelerin adlarını görüntülemek için satırlardan birine tıklayın.

Belirli bir yazılımı veya yazılım grubunu aramak için yazılım listesinin üstünde yer alan metin kutusundan arama yapabilirsiniz.
Filtre yazılım adı, sürümü veya yayımcı ile arama yapmanızı sağlar.

Örneğin "Contoso" araması yaptığınızda adı, yayımcısı veya sürümü "Contoso" içeren tüm yazılımlar görüntülenir.

## <a name="search-inventory-logs-for-installed-software"></a>Yüklü olan yazılımların stok günlüklerinde arama yapma

Envanter, Azure İzleyici günlüklerine gönderilen günlük verileri oluşturur. Sorgu çalıştırarak günlüklerde arama yapmak için **Stok** penceresinin en üstünde bulunan **Log Analytics**'i seçin.

Stok verileri **ConfigurationData** türü altında depolanır.
Aşağıdaki örnek Log Analytics sorgusu Yayıncının "Microsoft Corporation" değerine eşit olduğu envanter sonuçlarını döndürür.

```loganalytics
ConfigurationData
| where ConfigDataType == "Software"
| where Publisher == "Microsoft Corporation"
| summarize arg_max(TimeGenerated, *) by SoftwareName, Computer
```

Çalıştıran ve Azure İzleyici günlüklerine günlük dosyalarında arama yapma hakkında daha fazla bilgi için bkz: [Azure İzleyicisi](../azure-monitor/log-query/log-query-overview.md).

### <a name="single-machine-inventory"></a>Tek makine stoku

Tek bir makinenin yazılım stokunu görmek için Azure VM kaynak sayfasından stok erişmek veya Azure İzleyici günlüklerine karşılık gelen bir makine aşağı filtrelemek için kullanın.
Aşağıdaki örnek Log Analytics sorgusu, ContosoVM adlı bir makinedeki yazılımların listesini döndürür.

```loganalytics
ConfigurationData
| where ConfigDataType == "Software"
| summarize arg_max(TimeGenerated, *) by SoftwareName, CurrentVersion
| where Computer =="ContosoVM"
| render table
| summarize by Publisher, SoftwareName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakiler de dahil olmak üzere yazılım stok bilgisini görüntülemeyi öğrendiniz:

> [!div class="checklist"]
> * Çözümü etkinleştirme
> * Bir Azure VM ekleme
> * Azure olmayan bir VM ekleme
> * Yüklü olan yazılımları görüntüleme
> * Yüklü olan yazılımların stok günlüklerinde arama yapma

Daha fazla bilgi edinmek için Değişiklik izleme ve Stok çözümü özetine göz atın.

> [!div class="nextstepaction"]
> [Değişiklik yönetimi ve Stok çözümü](automation-change-tracking.md)
