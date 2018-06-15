---
title: Azure Windows sanal makineleriniz için güncelleştirme ve düzeltme eki yönetimi
description: Bu makalede Azure Windows VM'leriniz için güncelleştirme ve düzeltme eki yönetimi uygulaması olan Azure Otomasyonu'nu nasıl kullanacağınız anlatılmaktadır.
services: automation
author: zjalexander
ms.service: automation
ms.component: update-management
ms.topic: tutorial
ms.date: 02/28/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: 84ec2a5852e6aaeb4b9fe6ef11924209d03fb54b
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34054768"
---
# <a name="manage-windows-updates-with-azure-automation"></a>Azure Otomasyonu ile Windows güncelleştirmelerini yönetme

Güncelleştirme yönetimi, sanal makineleriniz için güncelleştirme ve düzeltme eki yönetimi gerçekleştirmenize olanak tanır.
Bu öğreticide kullanılabilir durumdaki güncelleştirmelerin durumunu değerlendirmeyi, gerekli güncelleştirmelerin yüklenmesini zamanlamayı, dağıtım sonuçlarını gözden geçirmeyi ve güncelleştirmelerin başarılı bir şekilde uygulandığını doğrulamak için bir uyarı oluşturmayı öğreneceksiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Güncelleştirme yönetimi için VM ekleme
> * Güncelleştirme değerlendirmesini görüntüleme
> * Uyarıları yapılandırma
> * Güncelleştirme dağıtımı zamanlama
> * Dağıtım sonuçlarını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [Visual Studio aboneleri için aylık Azure kredinizi etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* İzleyiciyi, eylem runbook'larını ve İzleyici Görevi'ni barındıracak bir [Otomasyon hesabı](automation-offering-get-started.md).
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-update-management"></a>Güncelleştirme yönetimini etkinleştirme

Bu öğretici için öncelikle VM'nizde Güncelleştirme yönetimini etkinleştirmeniz gerekir.

1. Azure portalında, soldaki menüden **Sanal makineler**'i ve listedeki VM'lerden birini seçin
2. VM sayfasındaki **İşlemler** bölümünde **Güncelleştirme yönetimi**’ne tıklayın. **Güncelleştirme Yönetimini Etkinleştirme** sayfası açılır.

Bu VM için Güncelleştirme yönetimi özelliğinin etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir. Bu doğrulama kapsamında Log Analytics çalışma alanı ve bağlantılı Otomasyon hesabının yanı sıra Güncelleştirme yönetimi çözümünün çalışma alanında olup olmadığı kontrol edilir.

[Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı, Güncelleştirme yönetimi gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır. Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.

Doğrulama işlemi ayrıca VM'nin Microsoft Monitoring Agent (MMA) ve Otomasyon karma runbook çalışanı ile sağlanıp sağlanmadığını da kontrol eder.
Bu aracı, Azure Otomasyonu ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi almak için kullanılır. Aracı, Azure Otomasyonu hizmetiyle iletişim kurmak ve güncelleştirmeleri indirmek için 443 numaralı bağlantı noktasının açık olmasını gerektirir.

Ekleme sırasında aşağıdaki önkoşullardan birinin karşılanmadığı tespit edilirse ilgili önkoşul otomatik olarak eklenir:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı
* [Otomasyon Hesabı](./automation-offering-get-started.md)
* VM üzerinde etkin bir [Karma runbook çalışanı](./automation-hybrid-runbook-worker.md)

**Güncelleştirme Yönetimi** ekranı açılır. Kullanılacak konumu, Log Analytics çalışma alanını ve Otomasyon hesabını yapılandırdıktan sonra **Etkinleştir**'e tıklayın. Bu alanların gri renkte olması VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

![Güncelleştirme yönetimi çözümünü etkinleştirme ekranı](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

Çözümün etkinleştirilmesi birkaç dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir.
Çözüm etkinleştirildikten sonra VM'deki eksik güncelleştirmeler hakkında bilgiler Log Analytics'e aktarılır.
Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görünür.
Varsa **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

Güncelleştirmeyle ilgili destek makalesini ayrı bir pencerede açmak için güncelleştirme üzerindeki **BİLGİ BAĞLANTISINI** seçin. Açılan sayfadan güncelleştirmeyle ilgili önemli bilgilere ulaşabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Güncelleştirmenin başka bir alanına tıkladığınızda seçilen güncelleştirmeye ait **Günlük Araması** penceresi açılır. Günlük araması sorgusu ilgili güncelleştirmeye göre önceden tanımlanmıştır. Bu sorguyu değiştirebilir veya kendi sorgunuzu oluşturarak ortamınıza dağıtılmış olan veya ortamınızda eksik olan güncelleştirmeler hakkında ayrıntılı bilgilere ulaşabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerting"></a>Uyarıları yapılandırma

Bu adımda, güncelleştirmelerin başarıyla dağıtıldığının size bildirilmesi için bir uyarı yapılandırırsınız. Oluşturduğunuz uyarı, bir Log Analytics sorgusunu temel alır. Birçok farklı senaryoda kullanılabilecek ek uyarılar oluşturmak için dilediğiniz gibi özel sorgu yazabilirsiniz. Azure portalında **İzleyici**’ye gidip **Uyarı Oluştur**’a tıklayın. Bunu yaptığınızda **Kural oluşturma** sayfası açılır.

**1. Uyarı koşulunu tanımlama** bölümünden **+  Hedef seçin**’i seçin. **Kaynak türüne göre filtrele** bölümünden **Log Analytics**’i seçin. Log Analytics çalışma alanınızı seçip **Bitti**’ye tıklayın.

![Uyarı oluşturma](./media/automation-tutorial-update-management/create-alert.png)

**+ Ölçüt ekle** düğmesine tıklayarak **Sinyal mantığını yapılandırma** sayfasını açın. Tabloda **Özel günlük arama**’yı seçin. **Arama sorgusu** metin kutusuna aşağıdaki sorguyu girin. Bu sorgu, bilgisayarları ve belirtilen zaman çerçevesinde tamamlanan güncelleştirme çalıştırma adını döndürür.

```loganalytics
UpdateRunProgress
| where InstallationStatus == 'Succeeded'
| where TimeGenerated > now(-10m)
| summarize by UpdateRunName, Computer
```

Uyarı mantığı için **Eşik** olarak **1** değerini girin. Tamamladığınızda **Bitti**’ye tıklayın.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/signal-logic.png)

**2. Uyarı ayrıntılarını tanımlayın**, uyarıya kolay bir ad verip bir açıklama ekleyin. Uyarı başarılı bir çalıştırma için olduğundan **Önem Derecesi**’ni **Bilgilendirici (Önem Derecesi 2)** olarak ayarlayın.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/define-alert-details.png)

**3. Eylem grubunu tanımlama** bölümünden **+ Yeni eylem grubu**’na tıklayın. Eylem grubu, birden çok uyarıda kullanabileceğiniz eylemlerden oluşan bir gruptur. Eylem gruplarına e-posta bildirimleri, runbook'lar, web kancaları ve diğer birçok şey dahildir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../monitoring-and-diagnostics/monitoring-action-groups.md)

**Eylem grubu adı** kutusuna bir kolay ad bir de kısa ad yazın. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

**Eylemler** bölümünden eyleme **E-posta Bildirimleri** gibi kolay bir ad verin ve **EYLEM TÜRÜ** bölümünden **E-posta/SMS/Anında İletme/Ses**’i seçin. **AYRINTILAR** bölümünden **Ayrıntıları düzenle**’yi seçin.

**E-posta/SMS/Anında İletme/Ses** sayfasında buna bir ad verin. **E-posta** onay kutusunu işaretleyin ve kullanılması için geçerli bir e-posta adresi girin.

![E-posta eylem grubunu yapılandırma](./media/automation-tutorial-update-management/configure-email-action-group.png)

**E-posta/SMS/Anında İletme/Ses** sayfasında **Tamam**’a tıklayarak sayfayı kapatın ve **Eylem grubu ekle** sayfasında **Tamam**’a tıklayarak bu sayfayı da kapatın.

**Kural oluştur** sayfasındaki **Eylemleri Özelleştirin** bölümünden **E-posta konusu**’na tıklayarak gönderilen e-postanın konusunu değiştirebilirsiniz. İşiniz bitince **Uyarı kuralı oluştur**’a tıklayın. Bu işlem sonucunda, bir güncelleştirme dağıtımı başarılı olduğunda sizi uyaran ve güncelleştirme dağıtımı çalıştırmasının hangi makineleri kapsadığını bildiren kural oluşturulur.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Artık uyarıları yapılandırdığınıza göre, güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın.
Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz.
Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

> [!WARNING]
> Güncelleştirmeler gerektirdiğinde VM otomatik olarak yeniden başlatılır.

**Güncelleştirme yönetimi** sayfasına dönüp ekranın en üstündeki **Güncelleştirme dağıtımı zamanla**'yı seçerek VM için yeni güncelleştirme dağıtımı zamanlayabilirsiniz.

**Yeni güncelleştirme dağıtım** ekranında aşağıdaki bilgileri belirtin:

* **Ad**: Güncelleştirme dağıtımı için benzersiz bir ad belirtin.

* **İşletim sistemi** - Güncelleştirme dağıtımı için hedeflenecek işletim sistemini seçin.

* **Güncelleştirme sınıflandırması**: Güncelleştirme dağıtımının dağıtıma dahil olan yazılım türlerini seçin. Bu öğreticide tüm türleri seçili halde bırakın.

  Sınıflandırma türleri şunlardır:

   |İşletim Sistemi  |Tür  |
   |---------|---------|
   |Windows     | Kritik güncelleştirmeler</br>Güvenlik güncelleştirmeleri</br>Güncelleştirme paketleri</br>Özellik paketleri</br>Hizmet paketleri</br>Tanım güncelleştirmeleri</br>Araçlar</br>Güncelleştirmeler        |
   |Linux     | Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri</br>Diğer güncelleştirmeler       |

   Sınıflandırma türlerinin açıklaması için bkz. [sınıflandırmaları güncelleştirme](automation-update-management.md#update-classifications).

* **Zamanlama ayarları** - Bu, Zamanlama Ayarları sayfasını açar. Varsayılan başlangıç zamanı, geçerli zamandan 30 dakika sonradır. Gelecekte 10 dakikadan uzun herhangi bir süre olacak şekilde ayarlanabilir.

   Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz.
   **Yineleme** bölümünden **Bir Kez**’i seçin. Varsayılan ayar olan 1 gün ayarını değiştirmeden **Tamam**'a tıklayın. Yinelenen bir zamanlama oluşturulur.

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

Güncelleştirme dağıtımınız başarılı olursa, dağıtımın başarılı olduğunu göstermek için aşağıdaki görüntüye benzer bir e-posta gönderilir.

![E-posta eylem grubunu yapılandırma](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Sonraki Adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Güncelleştirme yönetimi için VM ekleme
> * Güncelleştirme değerlendirmesini görüntüleme
> * Uyarıları yapılandırma
> * Güncelleştirme dağıtımı zamanlama
> * Dağıtım sonuçlarını görüntüleme

Güncelleştirme Yönetimi çözümüne genel bakış bölümüne geçin.

> [!div class="nextstepaction"]
> [Güncelleştirme Yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)
