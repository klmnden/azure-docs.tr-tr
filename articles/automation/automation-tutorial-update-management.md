---
title: Azure Windows sanal makineleriniz için güncelleştirme ve düzeltme eki yönetimi
description: Bu makalede Azure Windows VM'leriniz için güncelleştirme ve düzeltme eki yönetimi uygulaması olan Azure Otomasyonu'nu nasıl kullanacağınız anlatılmaktadır.
services: automation
author: zjalexander
ms.service: automation
ms.topic: tutorial
ms.date: 02/28/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: bded1621dc56a6e621408e567ce39a3107bec7c9
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="manage-windows-updates-with-azure-automation"></a>Azure Otomasyonu ile Windows güncelleştirmelerini yönetme

Güncelleştirme yönetimi, sanal makineleriniz için güncelleştirme ve düzeltme eki yönetimi gerçekleştirmenize olanak tanır.
Bu öğreticide kullanılabilir durumdaki güncelleştirmelerin durumunu değerlendirmeyi, gerekli güncelleştirmelerin yüklenmesini zamanlamayı ve güncelleştirmelerin başarılı bir şekilde uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirmeyi öğreneceksiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Güncelleştirme yönetimi için VM ekleme
> * Güncelleştirme değerlendirmesini görüntüleme
> * Güncelleştirme dağıtımı zamanlama
> * Dağıtım sonuçlarını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* İzleyiciyi, eylem runbook'larını ve İzleyici Görevi'ni barındıracak bir [Otomasyon hesabı](automation-offering-get-started.md).
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-update-management"></a>Güncelleştirme yönetimini etkinleştirme

Bu öğretici için öncelikle VM'nizde Güncelleştirme yönetimini etkinleştirmeniz gerekir. Daha önce bu VM için başka bir otomasyon çözümünü etkinleştirdiyseniz bu adımı atlayabilirsiniz.

1. Soldaki menüden **Sanal makineler**'i ve listedeki VM'lerden birini seçin
2. Soldaki menünün **İşlemler** bölümünde **Güncelleştirme yönetimi**'ne tıklayın. **Güncelleştirme Yönetimini Etkinleştirme** sayfası açılır.

Bu VM için Güncelleştirme yönetimi özelliğinin etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir.
Bu doğrulama kapsamında Log Analytics çalışma alanı ve bağlantılı Otomasyon hesabının yanı sıra çözümün çalışma alanında olup olmadığı kontrol edilir.

[Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı, Güncelleştirme yönetimi gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır.
Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.
Azure Otomasyonu, güncelleştirme yapılması gereken VM'lerde güncelleştirme indirme ve uygulama gibi ek işlemleri gerçekleştirmek için VM'ler üzerinde runbook'lar çalıştırmanızı sağlar.

Doğrulama işlemi ayrıca VM'nin Microsoft Monitoring Agent (MMA) ve Otomasyon karma runbook çalışanı ile sağlanıp sağlanmadığını da kontrol eder.
Bu aracı, VM ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi almak için kullanılır.

Çözümü etkinleştirmek için Log Analytics çalışma alanını ve otomasyon hesabını seçip **Etkinleştir**’e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

Ekleme sırasında aşağıdaki önkoşullardan birinin karşılanmadığı tespit edilirse ilgili önkoşul otomatik olarak eklenir:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı
* [Otomasyon](./automation-offering-get-started.md)
* VM üzerinde etkin bir [Karma runbook çalışanı](./automation-hybrid-runbook-worker.md)

**Güncelleştirme Yönetimi** ekranı açılır. Kullanılacak konumu, Log Analytics çalışma alanını ve Otomasyon hesabını yapılandırdıktan sonra **Etkinleştir**'e tıklayın. Bu alanların gri renkte olması VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

![Güncelleştirme yönetimi çözümünü etkinleştirme ekranı](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir.
Çözüm etkinleştirildikten sonra VM'deki eksik güncelleştirmeler hakkında bilgiler Log Analytics'e aktarılır.
Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görünür.
Varsa **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

Güncelleştirmeyle ilgili destek makalesini ayrı bir pencerede açmak için güncelleştirme üzerindeki **BİLGİ BAĞLANTISINI** seçin. Açılan sayfadan güncelleştirmeyle ilgili önemli bilgilere ulaşabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Güncelleştirmenin başka bir alanına tıkladığınızda seçilen güncelleştirmeye ait **Günlük Araması** penceresi açılır. Günlük araması sorgusu ilgili güncelleştirmeye göre önceden tanımlanmıştır. Bu sorguyu değiştirebilir veya kendi sorgunuzu oluşturarak ortamınıza dağıtılmış olan veya ortamınızda eksik olan güncelleştirmeler hakkında ayrıntılı bilgilere ulaşabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/logsearch.png)

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Şimdi VM'nizde eksik güncelleştirmeler olduğunu biliyorsunuz. Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın.
Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz.
Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

> [!WARNING]
> Güncelleştirmeler gerektirdiğinde VM otomatik olarak yeniden başlatılır.

**Güncelleştirme yönetimi** sayfasına dönüp ekranın en üstündeki **Güncelleştirme dağıtımı zamanla**'yı seçerek VM için yeni güncelleştirme dağıtımı zamanlayabilirsiniz.

**Yeni güncelleştirme dağıtım** ekranında aşağıdaki bilgileri belirtin:

* **Ad**: Güncelleştirme dağıtımı için benzersiz bir ad belirtin.
* **Güncelleştirme sınıflandırması**: Güncelleştirme dağıtımının dağıtıma dahil olan yazılım türlerini seçin. Bu öğreticide tüm türleri seçili halde bırakın.

  Sınıflandırma türleri şunlardır:

  * Kritik güncelleştirmeler
  * Güvenlik güncelleştirmeleri
  * Güncelleştirme paketleri
  * Özellik paketleri
  * Hizmet paketleri
  * Tanım güncelleştirmeleri
  * Araçlar
  * Güncelleştirmeler

* **Zamanlama ayarları**: Saati 5 dakika sonraya ayarlayın. İsterseniz varsayılan değer olan 30 dakika sonrasını da kabul edebilirsiniz.
Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz.
**Yinelenme** bölümünde **Yinelenen**'i seçin. Varsayılan ayar olan 1 gün ayarını değiştirmeden **Tamam**'a tıklayın. Yinelenen bir zamanlama oluşturulur.

* **Bakım penceresi (dakika)**: Bu değeri varsayılan değerde bırakın. Güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtebilirsiniz. Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

![Güncelleştirme Zamanlama Ayarları ekranı](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Zamanlama yapılandırmasını tamamladıktan sonra **Oluştur** düğmesine tıklayın. Durum panosu açılır. Oluşturduğunuz dağıtım zamanlamasını göstermek için **Zamanlanan güncelleştirme dağıtımları**'nı seçin.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** ekranındaki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
O anda çalışıyorsa, durumu **Devam ediyor** olarak gösterilir.
Tamamlandıktan sonra başarılı olursa durum **Başarılı** olarak değişir.
Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **Kısmen başarısız** şeklindedir.
Tamamlanan güncelleştirme dağıtımına tıklayarak bu güncelleştirme dağıtımının panosunu görebilirsiniz.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/automation-tutorial-update-management/manageupdates-view-results.png)

**Güncelleştirme sonuçları** kutucuğunda toplam güncelleştirme sayısının bir özeti ve VM'deki dağıtım sonuçları gösterilir.
Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir.
Aşağıdaki listede görüntülenebilecek değerler gösterilmiştir:

* **Denenmedi**: Tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
* **Başarılı**: Güncelleştirme başarılı oldu
* **Başarısız**: Güncelleştirme başarısız oldu

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’e tıklayın.

Hedef VM'de güncelleştirme dağıtımını yönetmekten sorumlu runbook'un iş akışını görmek için **Çıktı** kutucuğuna tıklayın.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’a tıklayın.

## <a name="next-steps"></a>Sonraki Adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Güncelleştirme yönetimi için VM ekleme
> * Güncelleştirme değerlendirmesini görüntüleme
> * Güncelleştirme dağıtımı zamanlama
> * Dağıtım sonuçlarını görüntüleme

Güncelleştirme Yönetimi çözümüne genel bakış bölümüne geçin.

> [!div class="nextstepaction"]
> [Güncelleştirme Yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)
