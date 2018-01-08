---
title: "Bir Azure sanal makinesindeki değişikliklerle ilgili sorunları giderme | Microsoft Docs"
description: "Bir Azure sanal makinesi üzerindeki değişikliklerle ilgili sorunları gidermek için Değişiklik İzleme özelliğini kullanabilirsiniz."
services: automation
keywords: "değişiklik, izleme, otomasyon"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 12/14/2017
ms.topic: hero-article
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: ae9ac6baaaeca418fcd3478145c50d1fa7917d7e
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="troubleshoot-changes-in-your-environment"></a>Ortamınızdaki değişikliklerle ilgili sorunları giderme

Bu öğreticide bir Azure sanal makinesi üzerindeki değişikliklerle ilgili sorunları gidermeyi öğreneceksiniz. Değişiklik İzleme özelliğini etkinleştirerek bilgisayarlarınızda gerçekleştirilen yazılımlar, dosyalar, Linux daemon'ları, Windows hizmetleri ve Windows kayıt defteri anahtarlarıyla ilgili değişikliklikleri izleyebilirsiniz.
Bu yapılandırma değişikliklerinin tanımlanması, ortamınızdaki işletimsel sorunları belirlemenize yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * VM'de Değişiklik İzleme ve Stok özelliklerini etkinleştirme
> * Durdurulan hizmetlerin değişiklik günlüklerinde arama yapma
> * Değişiklik izlemeyi yapılandırma
> * Etkinlik günlüğü bağlantısını etkinleştirme
> * Bir olay tetikleme
> * Değişiklikleri görüntüleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* İzleyiciyi, eylem runbook'larını ve İzleyici Görevi'ni barındıracak bir [Otomasyon hesabı](automation-offering-get-started.md).
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok özelliklerini etkinleştirme

Bu öğreticide ilk yapmanız gereken VM'inizde Değişiklik İzleme ve Stok özelliklerini etkinleştirmektir. Daha önce bu VM için başka bir otomasyon çözümünü etkinleştirdiyseniz bu adımı atlayabilirsiniz.

1. Soldaki menüden **Sanal makineler**'i ve listedeki VM'lerden birini seçin
1. Soldaki menünün **İşlemler** bölümünde **Stok**'a tıklayın. **Değişiklik İzlemeyi ve Stoku Etkinleştir** sayfası açılır.

Bu VM için Değişiklik İzlemeyi ve Stok özelliklerinin etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir.
Bu doğrulama kapsamında Log Analytics çalışma alanı ve bağlantılı Otomasyon hesabının yanı sıra çözümün çalışma alanında olup olmadığı kontrol edilir.

[Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı, Stok gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır.
Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.

Doğrulama işlemi ayrıca VM'nin Microsoft Monitoring Agent (MMA) ve karma çalışan ile sağlanıp sağlanmadığını da kontrol eder.
Bu aracı, VM ile iletişim kurmak ve yüklü yazılım hakkında bilgi almak için kullanılır.
Doğrulama işlemi ayrıca VM'nin Microsoft Monitoring Agent (MMA) ve Otomasyon karma runbook çalışanı ile sağlanıp sağlanmadığını da kontrol eder.

Bu önkoşullar sağlanmadıysa, çözümü etkinleştirme seçeneği sunan bir başlık görüntülenir.

![Değişiklik izleme ve Stok devreye alma yapılandırma başlığı](./media/automation-tutorial-troubleshoot-changes/enableinventory.png)

Çözümü etkinleştirmek için başlığa tıklayın.
Doğrulama sonrasında aşağıdaki önkoşullardan birinin karşılanmadığı tespit edilirse ilgili önkoşul otomatik olarak eklenir:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı
* [Otomasyon](./automation-offering-get-started.md)
* VM üzerinde etkin bir [Karma runbook çalışanı](./automation-hybrid-runbook-worker.md)

**Değişiklik İzleme ve Stok** ekranı açılır. Kullanılacak konumu, Log Analytics çalışma alanını ve Otomasyon hesabını yapılandırdıktan sonra **Etkinleştir**'e tıklayın. Bu alanların gri renkte olması, VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

![Değişiklik izleme çözümünü etkinleştirme penceresi](./media/automation-tutorial-troubleshoot-changes/installed-software-enable.png)

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir.
Çözüm etkinleştirildikten sonra VM üzerine yüklenen yazılımlar ve yapılan değişiklikler hakkında bilgiler Log Analytics'e aktarılır.
Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.


## <a name="using-change-tracking-in-log-analytics"></a>Log Analytics'teki Değişiklik izleme özelliğini kullanma

Değişiklik izleme özelliği ile oluşturulan günlük verileri Log Analytics'e gönderilir. Sorgu çalıştırarak günlüklerde arama yapmak için **Değişiklik izleme** penceresinin en üstünde bulunan **Log Analytics**'i seçin.
Değişiklik izleme verileri **ConfigurationChange** türü altında depolanır. Aşağıdaki örnek Log Analytics sorgusu, durdurulmuş olan tüm Windows Hizmetleri'ni döndürür.

```
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Log Analytics'te sorgu çalıştırma ve günlük dosyalarında arama yapma hakkında daha fazla bilgi edinmek için bkz. [Azure Log Analytics](https://docs.loganalytics.io/index).

## <a name="configure-change-tracking"></a>Değişiklik izlemeyi yapılandırma

Değişiklik izleme, VM üzerindeki yapılandırma değişikliklerini izlemenizi sağlar. Aşağıdaki adımlar kayıt defteri anahtarlarının ve dosyaların izlenmesini yapılandırma konusunda yol göstermektedir.
 
Toplanıp izlenecek dosyaları ve Kayıt defteri anahtarlarını belirlemek için **Değişiklik izleme** sayfasının üst kısmından **Ayarları düzenle**'yi seçin.

> [!NOTE]
> Stok ve Değişiklik izleme özellikleri aynı koleksiyon ayarlarını kullanır ve bu ayarlar çalışma alanı düzeyinde yapılandırılır.

**Çalışma Alanı Yapılandırması** penceresinde izlenmesini istediğiniz Windows Kayıt Defteri anahtarlarını, Windows dosyalarını veya Linux dosyalarını aşağıdaki üç bölümde belirtilen şekilde seçin.

### <a name="add-a-windows-registry-key"></a>Windows Kayıt Defteri anahtarı ekleme

1. **Windows Kayıt Defteri** sekmesinde **Ekle**'yi seçin.
    **Değişiklik İzleme için Windows Kayıt Defteri Ekle** penceresi açılır.

   ![Değişiklik İzleme için kayıt ekleme](./media/automation-vm-change-tracking/change-add-registry.png)

2. **Etkin**'in altında **True**'yu seçin.
3. **Öğe Adı** kutusuna kolay ad girin.
4. (İsteğe bağlı) **Grup** kutusuna bir grup adı girin.
5. **Windows Kayıt Defteri Anahtarı** kutusuna izlemek istediğiniz kayıt defteri anahtarının adını girin.
6. **Kaydet**’i seçin.

### <a name="add-a-windows-file"></a>Windows dosyası ekleme

1. **Windows Dosyaları** sekmesinde **Ekle**'yi seçin. **Değişiklik İzleme için Windows Dosyası Ekle** penceresi açılır.

   ![Değişiklik İzleme için Windows dosyası ekleme](./media/automation-vm-change-tracking/change-add-win-file.png)

2. **Etkin**'in altında **True**'yu seçin.
3. **Öğe Adı** kutusuna kolay ad girin.
4. (İsteğe bağlı) **Grup** kutusuna bir grup adı girin.
5. **Yolu Girin** kutusuna izlemek istediğiniz dosyanın tam yolunu ve adını girin.
6. **Kaydet**’i seçin.

### <a name="add-a-linux-file"></a>Linux dosyası ekleme

1. **Linux Dosyaları** sekmesinde **Ekle**'yi seçin. **Değişiklik İzleme için Linux Dosyası Ekle** penceresi açılır.

   ![Değişiklik İzleme için Linux dosyası ekleme](./media/automation-vm-change-tracking/change-add-linux-file.png)

2. **Etkin**'in altında **True**'yu seçin.
3. **Öğe Adı** kutusuna kolay ad girin.
4. (İsteğe bağlı) **Grup** kutusuna bir grup adı girin.
5. **Yolu Girin** kutusuna izlemek istediğiniz dosyanın tam yolunu ve adını girin.
6. **Yol Türü** kutusunda **Dosya** veya **Dizin**'i seçin.
7. **Özyineleme** bölümünde belirtilen yol ve altındaki tüm yollarda yapılan değişiklikleri izlemek için **Açık** seçeneğini belirleyin. Yalnızca seçilen yolu veya dosyayı izlemek için **Kapalı**'yı seçin.
8. **Sudo Kullan** bölümünde erişim için `sudo` komutunun kullanılmasını gerektiren dosyaları izlemek istiyorsanız **Açık** seçeneğini belirleyin. İstemiyorsanız **Kapalı**'yı seçin.
9. **Kaydet**’i seçin.

## <a name="enable-activity-log-connection"></a>Etkinlik günlüğü bağlantısını etkinleştirme

VM'nizdeki **Değişik izleme** sayfasında **Etkinlik Günlüğü Bağlantısını Yönetme**'yi seçin. Bu görev **Azure Etkinlik günlüğü** sayfasını açar. Değişiklik izleme özelliğini VM'nizin Azure etkinlik günlüğü verilerine bağlamak için **Bağlan**'ı seçin.

Bu ayar etkin durumdayken VM'nizin **Özet** sayfasına gidip **Durdur**'u seçerek VM'nizi durdurun. Sorulduğunda **Evet**'i seçerek VM'yi durdurun. Serbest bırakıldıktan sonra **Başlat**'ı seçerek VM'nizi yeniden başlatın.

VM'yi durdurup başlattığınızda etkinlik günlüğüne bir olay kaydedilir. **Değişiklik izleme** sayfasına dönün. Sayfanın en altındaki **Olaylar** sekmesini seçin. Bir süre sonra olaylar grafik ve tablo şeklinde gösterilir. Önceki adımda olduğu gibi her bir olayı seçerek ayrıntılı bilgileri görüntüleyebilirsiniz.

![Değişiklik ayrıntılarını portalda görüntüleme](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

## <a name="view-changes"></a>Değişiklikleri görüntüleme

Değişiklik izleme ve Stok çözümünü etkinleştirdikten sonra sonuçları **Değişiklik izleme** sayfasında görüntüleyebilirsiniz.

VM'nizin içinde **İŞLEMLER** bölümünden **Değişiklik izleme**'yi seçin.

![OMS klasik portalında uyarı oluşturma](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

Grafik, zaman içinde gerçekleştirilen değişiklikleri gösterir.
Etkinlik Günlüğü bağlantısı eklediğinizde en üstteki çizgi grafik Azure Etkinlik Günlüğü olaylarını görüntüler.
Çubuk grafiklerin her satırı farklı bir izlenebilir Değişiklik türünü temsil eder.
Bu türler Linux daemon'ları, dosyalar, Windows Kayıt Defteri anahtarları, yazılımlar ve Windows hizmetleridir.
Değişiklik sekmesinde görselleştirmede gösterilen değişikliklerle ilgili ayrıntılar, değişikliğin oluşma zamanına göre azalan düzende (en son gerçekleşen en başta) gösterilir.
**Olaylar** sekmesindeki tabloda bağlı Etkinlik Günlüğü olayları ile ayrıntıları en son gerçekleşen en başta olacak şekilde gösterilir.

Sonuçlara bakarak hizmet ve yazılım değişiklikleri dahil olmak üzere sistemde birçok değişiklik yapıldığını görebilirsiniz. Sayfanın en üstünde bulunan filtreleri kullanarak sonuçları **Değişiklik türü** veya zaman aralığına göre filtreleyebilirsiniz.

Bir **WindowsServices** değişikliğini seçtiğinizde **Değişiklik Ayrıntıları** penceresi açılır. Değişiklik ayrıntıları penceresinde değişiklik hakkında ayrıntılı bilgilerin yanı sıra değişiklik öncesi ve sonrası değerler gösterilir. Bu örnekte Yazılım Koruması hizmeti durdurulmuştur.

![Değişiklik ayrıntılarını portalda görüntüleme](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="next-steps"></a>Sonraki Adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * VM'de Değişiklik İzleme ve Stok özelliklerini etkinleştirme
> * Durdurulan hizmetlerin değişiklik günlüklerinde arama yapma
> * Değişiklik izlemeyi yapılandırma
> * Etkinlik günlüğü bağlantısını etkinleştirme
> * Bir olay tetikleme
> * Değişiklikleri görüntüleme

Daha fazla bilgi edinmek için Değişiklik izleme ve Stok çözümü özetine göz atın.

> [!div class="nextstepaction"]
> [Değişiklik yönetimi ve Stok çözümü](../log-analytics/log-analytics-change-tracking.md?toc=%2fazure%2fautomation%2ftoc.json)
