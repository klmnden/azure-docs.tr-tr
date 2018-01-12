---
title: "Azure Otomasyonu ile makinelerinizde yüklü olan yazılımları keşfetme | Microsoft Docs"
description: "Ortamınızdaki makinelerde yüklü olan yazılımları keşfetmek için Stok özelliğinden faydalanın."
services: automation
keywords: "stok, otomasyon, değişiklik, izleme"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 12/14/2017
ms.topic: tutorial
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: bdd638d0612a8ddee1a0ddb4fd4579f8da14b887
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="discover-what-software-is-installed-on-your-azure-and-non-azure-machines"></a>Azure ve Azure harici makinelerinizde yüklü olan yazılımları keşfetme

Bu öğreticide ortamınızda yüklü olan yazılımları keşfetmeyi öğreneceksiniz. Yazılımlar, dosyalar, Linux daemon'ları, Windows hizmetleri ve Windows kayıt defteri anahtarlarıyla ilgili stok durumunu sorgulayabilir ve görüntüleyebilirsiniz. Makinelerinizin yapılandırmasını izlemek ortamınızdaki işletimsel sorunları bulmanıza ve makinelerinizin durumunu daha iyi anlamanıza yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * VM'de Değişiklik İzleme ve Stok özelliklerini etkinleştirme
> * Yüklü olan yazılımları görüntüleme
> * Yüklü olan yazılımların stok günlüklerinde arama yapma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* İzleyiciyi, eylem runbook'larını ve İzleyici Görevi'ni barındıracak bir [Otomasyon hesabı](automation-offering-get-started.md).
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok özelliklerini etkinleştirme

Bu öğreticide ilk yapmanız gereken VM'inizde Değişiklik İzleme ve Stok özelliklerini etkinleştirmektir. Daha önce bu VM için başka bir otomasyon çözümünü etkinleştirdiyseniz bu adımı atlayabilirsiniz.

1. Soldaki menüden **Sanal makineler**'i ve listedeki VM'lerden birini seçin
2. Soldaki menünün **İşlemler** bölümünde **Stok**'a tıklayın. **Değişiklik İzlemeyi ve Stoku Etkinleştir** sayfası açılır.

Bu VM için Stok özelliğinin etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir.
Bu doğrulama kapsamında Log Analytics çalışma alanı ve bağlantılı Otomasyon hesabının yanı sıra çözümün çalışma alanında olup olmadığı kontrol edilir.

[Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı, Stok gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır.
Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.

Doğrulama işlemi ayrıca VM'nin Microsoft Monitoring Agent (MMA) ve karma çalışan ile sağlanıp sağlanmadığını da kontrol eder.
Bu aracı, VM ile iletişim kurmak ve yüklü yazılım hakkında bilgi almak için kullanılır.
Doğrulama işlemi ayrıca VM'nin Microsoft Monitoring Agent (MMA) ve Otomasyon karma runbook çalışanı ile sağlanıp sağlanmadığını da kontrol eder.

Bu önkoşullar sağlanmadıysa çözümü etkinleştirme seçeneği sunan bir başlık açılır.

![Stok devreye alma yapılandırma başlığı](./media/automation-tutorial-installed-software/enableinventory.png)

Çözümü etkinleştirmek için başlığa tıklayın.
Doğrulama sonrasında aşağıdaki önkoşullardan birinin karşılanmadığı tespit edilirse ilgili önkoşul otomatik olarak eklenir:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı
* [Otomasyon](./automation-offering-get-started.md)
* VM üzerinde etkin bir [Karma runbook çalışanı](./automation-hybrid-runbook-worker.md)

**Değişiklik İzleme ve Stok** ekranı açılır. Kullanılacak konumu, Log Analytics çalışma alanını ve Otomasyon hesabını yapılandırdıktan sonra **Etkinleştir**'e tıklayın. Bu alanların gri renkte olması, VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

![Değişiklik izleme çözümünü etkinleştirme penceresi](./media/automation-tutorial-installed-software/installed-software-enable.png)

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir.
Çözüm etkinleştirildikten sonra VM üzerine yüklenen yazılımlar ve yapılan değişiklikler hakkında bilgiler Log Analytics'e aktarılır.
Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="view-installed-software"></a>Yüklü olan yazılımları görüntüleme

Değişiklik izleme ve Stok çözümünü etkinleştirdikten sonra sonuçları **Stok** sayfasında görüntüleyebilirsiniz.

VM'nizin içinde **İŞLEMLER** bölümünden **Stok**'u seçin.

**Stok** sayfasında **Yazılım** sekmesine tıklayın.

**Yazılım** sekmesinde bulunan yazılımların listelendiği bir tablo yer alır. Yazılımlar ada ve sürüme göre gruplara ayrılmıştır.

Tabloda her yazılım kaydı hakkındaki üst düzey ayrıntılar görüntülenebilir. Bu ayrıntılar arasında yazılım adı, sürümü, yayımcı, son yenileme zamanı (grup içindeki bir makine tarafından bildirilen en son yenileme zamanı) ve makine sayısı (bu yazılıma sahip makinelerin sayısı) yer alır.

![Yazılım stoku](./media/automation-tutorial-installed-software/inventory-software.png)

Yazılım kaydının özelliklerini ve bu yazılımın yüklü olduğu makinelerin adlarını görüntülemek için satırlardan birine tıklayın.

Belirli bir yazılımı veya yazılım grubunu aramak için yazılım listesinin üstünde yer alan metin kutusundan arama yapabilirsiniz.
Filtre yazılım adı, sürümü veya yayımcı ile arama yapmanızı sağlar.

Örneğin "Contoso" araması yaptığınızda adı, yayımcısı veya sürümü "Contoso" içeren tüm yazılımlar görüntülenir.

## <a name="search-inventory-logs-for-installed-software"></a>Yüklü olan yazılımların stok günlüklerinde arama yapma

Stok özelliği ile oluşturulan günlük verileri Log Analytics'e gönderilir. Sorgu çalıştırarak günlüklerde arama yapmak için **Stok** penceresinin en üstünde bulunan **Log Analytics**'i seçin.

Stok verileri **ConfigurationData** türü altında depolanır.
Aşağıdaki örnek Log Analytics sorgusu, içinde "Microsoft" geçen Yayımcıları ve her Yayımcı için Yazılım kaydı sayısını (Yazılım Adı ve Bilgisayar ölçütlerine göre gruplara ayrılmış) döndürür.

```
ConfigurationData
| summarize arg_max(TimeGenerated, *) by SoftwareName, Computer
| where ConfigDataType == "Software"
| search Publisher:"Microsoft"
| summarize count() by Publisher
```

Log Analytics'te sorgu çalıştırma ve günlük dosyalarında arama yapma hakkında daha fazla bilgi edinmek için bkz. [Azure Log Analytics](https://docs.loganalytics.io/index).

### <a name="single-machine-inventory"></a>Tek makine stoku

Tek bir makinenin yazılım stokunu görmek için Azure VM kaynak sayfasından Stok özelliğine erişebilir veya Log Analytics ile ilgili makineyi filtreleyebilirsiniz. Aşağıdaki örnek Log Analytics sorgusu, ContosoVM adlı bir makinedeki yazılımların listesini döndürür.

```
ConfigurationData
| where ConfigDataType == "Software" 
| summarize arg_max(TimeGenerated, *) by SoftwareName, CurrentVersion
| where Computer =="ContosoVM"
| render table
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakiler de dahil olmak üzere yazılım stok bilgisini görüntülemeyi öğrendiniz:

> [!div class="checklist"]
> * VM'de Değişiklik İzleme ve Stok özelliklerini etkinleştirme
> * Yüklü olan yazılımları görüntüleme
> * Yüklü olan yazılımların stok günlüklerinde arama yapma

Daha fazla bilgi edinmek için Değişiklik izleme ve Stok çözümü özetine göz atın.

> [!div class="nextstepaction"]
> [Değişiklik yönetimi ve Stok çözümü](../log-analytics/log-analytics-change-tracking.md?toc=%2fazure%2fautomation%2ftoc.json)