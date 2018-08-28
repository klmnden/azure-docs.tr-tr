---
title: Azure Windows sanal makineleriniz için güncelleştirme ve düzeltme eki yönetimi
description: Bu makalede Azure Windows VM'leriniz için güncelleştirme ve düzeltme eki yönetimi uygulaması olan Azure Otomasyonu Güncelleştirme Yönetimi’ni nasıl kullanacağınız anlatılmaktadır.
services: automation
author: zjalexander
ms.service: automation
ms.component: update-management
ms.topic: tutorial
ms.date: 02/28/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: 4d5222889d5e840bd03bf77a56584dac48bb740c
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41920802"
---
# <a name="manage-windows-updates-by-using-azure-automation"></a>Azure Otomasyonu'nu kullanarak Windows güncelleştirmelerini yönetme

Güncelleştirme Yönetimi çözümünü kullanarak sanal makineleriniz için güncelleştirmeleri ve yamaları yönetebilirsiniz. Bu öğreticide kullanılabilir durumdaki güncelleştirmelerin durumunu değerlendirmeyi, gerekli güncelleştirmelerin yüklenmesini zamanlamayı, dağıtım sonuçlarını gözden geçirmeyi ve güncelleştirmelerin başarılı bir şekilde uygulandığını doğrulamak için bir uyarı oluşturmayı öğreneceksiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme Yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Güncelleştirme Yönetimi için VM ekleme
> * Güncelleştirme değerlendirmesini görüntüleme
> * Uyarıları yapılandırma
> * Güncelleştirme dağıtımı zamanlama
> * Dağıtım sonuçlarını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [Visual Studio aboneleri için aylık Azure kredinizi etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* İzleyiciyi, eylem runbook'larını ve İzleyici Görevi'ni barındıracak bir [Azure Otomasyonu hesabı](automation-offering-get-started.md).
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-update-management"></a>Güncelleştirme Yönetimi’ni etkinleştirme

Bu öğretici için öncelikle VM'nizde Güncelleştirme Yönetimi'ni etkinleştirin:

1. Azure portalda sol taraftaki menüden **Sanal makineler**'i seçin. Listeden bir VM seçin.
2. VM sayfasının **İŞLEMLER** bölümünde **Güncelleştirme yönetimi**'ni seçin. **Güncelleştirme Yönetimini Etkinleştirme** sayfası açılır.

Bu VM için Güncelleştirme Yönetimi özelliğinin etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir. Bu doğrulama kapsamında Azure Log Analytics çalışma alanı ve bağlantılı Otomasyon hesabının yanı sıra Güncelleştirme Yönetimi çözümünün çalışma alanında olup olmadığı kontrol edilir.

[Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı, Güncelleştirme Yönetimi gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır. Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.

Doğrulama işlemi ayrıca VM'nin Microsoft Monitoring Agent (MMA) ve Otomasyon Karma Runbook Çalışanı ile sağlanıp sağlanmadığını da kontrol eder. Bu aracı, Azure Otomasyonu ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi almak için kullanılır. Aracı, Azure Otomasyonu hizmetiyle iletişim kurmak ve güncelleştirmeleri indirmek için 443 numaralı bağlantı noktasının açık olmasını gerektirir.

Ekleme sırasında aşağıdaki önkoşullardan birinin karşılanmadığı tespit edilirse ilgili önkoşul otomatik olarak eklenir:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı
* Bir [Otomasyon hesabı](./automation-offering-get-started.md)
* Bir [Karma Runbook Çalışanı](./automation-hybrid-runbook-worker.md) (VM üzerinde etkin)

**Güncelleştirme Yönetimi** bölümünde konumu, Log Analytics çalışma alanını ve kullanılacak Otomasyon hesabını belirleyin. Ardından **Etkinleştir**'i seçin. Bu seçeneklerin olmaması VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir. Bu durumda aynı çalışma alanını ve Otomasyon hesabı kullanılmalıdır.

![Güncelleştirme Yönetimi çözümünü etkinleştirme penceresi](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

Çözümün etkinleştirilmesi birkaç dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmayın. Çözüm etkinleştirildikten sonra VM'deki eksik güncelleştirmeler hakkında bilgiler Log Analytics'e aktarılır. Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme Yönetimi etkinleştirildikten sonra **Güncelleştirme yönetimi** bölmesi açılır. Varsa **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesi gösterilir.

Güncelleştirmeyle ilgili destek makalesini ayrı bir pencerede açmak için **BİLGİ BAĞLANTISI** bölümünden güncelleştirme bağlantısını seçin. Bu pencereden güncelleştirmeyle ilgili önemli bilgilere ulaşabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Güncelleştirmenin başka bir alanına tıkladığınızda seçilen güncelleştirmeye ait **Günlük Araması** bölmesi açılır. Günlük araması sorgusu ilgili güncelleştirmeye göre önceden tanımlanmıştır. Bu sorguyu değiştirebilir veya kendi sorgunuzu oluşturarak ortamınıza dağıtılmış olan veya ortamınızda eksik olan güncelleştirmeler hakkında ayrıntılı bilgilere ulaşabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Uyarı yapılandırma

Bu adımda, güncelleştirmelerin başarıyla dağıtıldığının size bildirilmesi için bir uyarı ayarlarsınız. Oluşturduğunuz uyarı, bir Log Analytics sorgusunu temel alır. Birçok farklı senaryoda kullanılabilecek ek uyarılar için özel bir sorgu yazabilirsiniz. Azure portalında **İzleyici**'ye gidip **Uyarı Oluştur**'u seçin. 

**Kural oluştur** bölümünün **1. Uyarı koşulunu tanımlama** kısmından **Hedef seçin**'i belirleyin. **Kaynak türüne göre filtrele** bölümünden **Log Analytics**’i seçin. Log Analytics çalışma alanınızı ve ardından **Bitti**'yi seçin.

![Uyarı oluşturma](./media/automation-tutorial-update-management/create-alert.png)

**Ölçüt ekle**'yi seçin.

**Sinyal mantığını yapılandırma** bölümündeki tablodan **Özel günlük araması**'nı seçin. **Arama sorgusu** metin kutusuna aşağıdaki sorguyu girin:

```loganalytics
UpdateRunProgress
| where InstallationStatus == 'Succeeded'
| where TimeGenerated > now(-10m)
| summarize by UpdateRunName, Computer
```
Bu sorgu, bilgisayarları ve belirtilen zaman çerçevesinde tamamlanan güncelleştirme çalıştırma adını döndürür.

**Uyarı mantığı** bölümünde **Eşik** alanına **1** değerini girin. İşiniz bittiğinde **Bitti**'yi seçin.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/signal-logic.png)

**2. Uyarı ayrıntılarını tanımlama** bölümünde uyarı adını ve açıklamasını girin. Uyarı başarılı bir çalıştırma için olduğundan **Önem Derecesi**'ni **Bilgilendirici (Önem Derecesi 2)** olarak ayarlayın.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/define-alert-details.png)

**3. Eylem grubunu tanımlama** bölümünde **Yeni eylem grubu**'nu seçin. Eylem grubu, birden çok uyarıda kullanabileceğiniz eylemlerden oluşan bir gruptur. Eylemlere e-posta bildirimleri, runbook'lar, web kancaları ve diğer birçok şey dahildir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../monitoring-and-diagnostics/monitoring-action-groups.md).

**Eylem grubu adı** kutusuna uyarı için ad ve kısa ad. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

**Eylemler** bölümünde eyleme için **E-posta Bildirimleri** gibi bir ad girin. **EYLEM TÜRÜ** bölümünde **E-posta/SMS/Anında İletme/Ses**'i seçin. **AYRINTILAR** bölümünden **Ayrıntıları düzenle**’yi seçin.

**E-posta/SMS/Anında İletme/Ses** bölmesine bir ad girin. **E-posta** onay kutusunu seçip geçerli bir e-posta adresi girin.

![E-posta eylem grubu yapılandırma](./media/automation-tutorial-update-management/configure-email-action-group.png)

**E-posta/SMS/Anında İletme/Ses** bölmesinde **Tamam**'ı seçin. **Eylem grubu ekle** bölmesinde **Tamam**'ı seçin.

Uyarı e-postasının konusunu özelleştirmek için **Eylemleri Özelleştirin** bölümündeki **Kural oluştur**, kısmında **E-posta konusu**'nu seçin. İşleminiz bittiğinde **Uyarı kuralı oluştur**'u seçin. Bu uyarı, bir güncelleştirme dağıtımı başarılı olduğunda sizi uyarır ve güncelleştirme dağıtımı çalıştırmasının hangi makineleri kapsadığını bildirir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Şimdi güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

Yeni bir VM güncelleştirme dağıtımı zamanlamak için **Güncelleştirme yönetimi**'ne gidip **Güncelleştirme dağıtımı zamanla**'yı seçin.

**Yeni güncelleştirme dağıtımı** bölümüne aşağıdaki bilgileri girin:

* **Ad**: Güncelleştirme dağıtımı için benzersiz bir ad girin.

* **İşletim sistemi**: Güncelleştirme dağıtımı için hedeflenecek işletim sistemini seçin.

* **Güncelleştirilecek makineler**: Kayıtlı bir aramayı veya İçeri aktarılan grubu seçin veya açılan menüden Makine'yi seçerek belirli makineleri seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir. Log Analytics'te bilgisayar grubu oluşturmak için kullanılan farklı yöntemler hakkında bilgi edinmek için bkz. [Log Analytics'teki bilgisayar grupları](../log-analytics/log-analytics-computer-groups.md)

* **Güncelleştirme sınıflandırması**: Güncelleştirme dağıtımının dağıtıma dahil olan yazılım türlerini seçin. Bu öğreticide tüm türleri seçili halde bırakın.

  Sınıflandırma türleri şunlardır:

   |İşletim Sistemi  |Tür  |
   |---------|---------|
   |Windows     | Kritik güncelleştirmeler</br>Güvenlik güncelleştirmeleri</br>Güncelleştirme paketleri</br>Özellik paketleri</br>Hizmet paketleri</br>Tanım güncelleştirmeleri</br>Araçlar</br>Güncelleştirmeler        |
   |Linux     | Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri</br>Diğer güncelleştirmeler       |

   Sınıflandırma türlerinin açıklaması için bkz. [sınıflandırmaları güncelleştirme](automation-update-management.md#update-classifications).

* **Zamanlama ayarları**: **Zamanlama Ayarları** bölmesi açılır. Varsayılan başlangıç zamanı, geçerli zamandan 30 dakika sonradır. Başlangıç zamanını en düşük 10 dakika olmak üzere istediğiniz değere ayarlayabilirsiniz.

   Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz. **Yinelenme** bölümünde **Bir Kez**'i seçin. Varsayılan 1 gün değerini bırakın ve **Tamam**'ı seçin. Yinelenen bir zamanlama oluşturulur.

* **Bakım penceresi (dakika)**: Varsayılan değeri bırakın. Güncelleştirme dağıtımının gerçekleşmesini istediğiniz zaman aralığını belirtebilirsiniz. Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

* **Yeniden başlatma seçenekleri**: Bu ayar, yeniden başlatma işlemlerinin nasıl gerçekleştirileceğini belirler. Kullanılabilen seçenekler:
  * Gerekirse yeniden başlat (Varsayılan)
  * Her zaman yeniden başlat
  * Hiçbir zaman yeniden başlatma
  * Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez

Zamanlamayı yapılandırdıktan sonra **Oluştur**'u seçin.

![Güncelleştirme Zamanlama Ayarları bölmesi](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Durum panosu açılır. Oluşturduğunuz dağıtım zamanlamasını göstermek için **Zamanlanan güncelleştirme dağıtımları**'nı seçin.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** bölümündeki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz. Dağıtım o anda çalışıyorsa, durum **Sürüyor** şeklinde olur. Dağıtım tamamlandıktan sonra başarılıysa durumu **Başarılı** olarak değişir. Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **Kısmen başarısız** şeklindedir.

Tamamlanan güncelleştirme dağıtımını seçerek bu güncelleştirme dağıtımının panosunu görebilirsiniz.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/automation-tutorial-update-management/manageupdates-view-results.png)

**Güncelleştirme sonuçları** bölümünde toplam güncelleştirme sayısının bir özeti ve VM'deki dağıtım sonuçları gösterilir. Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir.

Aşağıdaki listede görüntülenebilecek değerler gösterilmiştir:

* **Denenmedi**: Tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
* **Başarılı**: Güncelleştirme başarılı oldu.
* **Başarısız**: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’i seçin.

Hedef VM'de güncelleştirme dağıtımını yönetmekten sorumlu runbook'un iş akışını görmek için **Çıktı**'yı seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgiler için **Hatalar**’ı seçin.

Güncelleştirme dağıtımınız başarılı olursa, dağıtımın başarılı olduğunu göstermek için aşağıdaki örneğe benzer bir e-posta gönderilir:

![E-posta eylem grubunu yapılandırma](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Güncelleştirme Yönetimi için VM ekleme
> * Güncelleştirme değerlendirmesini görüntüleme
> * Uyarıları yapılandırma
> * Güncelleştirme dağıtımı zamanlama
> * Dağıtım sonuçlarını görüntüleme

Güncelleştirme Yönetimi çözümüne genel bakış bölümüne geçin.

> [!div class="nextstepaction"]
> [Güncelleştirme Yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)
