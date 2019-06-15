---
title: Azure'da faturalandırma ve maliyet Yönetimi bütçe senaryosu | Microsoft Docs
description: Belirli bir bütçe eşiklere dayanarak Vm'leri kapatmak için Azure otomasyonunu kullanmayı öğrenin.
services: billing
documentationcenter: ''
author: Erikre
manager: dougeby
editor: ''
tags: billing
ms.assetid: db93f546-6b56-4b51-960d-1a5bf0274fc8
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 03/13/2019
ms.author: erikre
ms.openlocfilehash: 4bf76ac0bdd59764815f18a40a3e243d7cf9d920
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60617427"
---
# <a name="manage-costs-with-azure-budgets"></a>Azure Budgets ile maliyetleri yönetme

Maliyet denetimi, bulutta yatırımınızın değerini en üst düzeye çıkarma için kritik bir bileşenidir. Maliyet görünürlük, raporlama ve maliyet tabanlı düzenleme sürekli işletme işlemleri için çok önemli olduğu bazı senaryolar vardır. [Azure maliyet Yönetimi API'leri](https://docs.microsoft.com/rest/api/consumption/) bu senaryoların her biri desteklemek için API kümesi sağlar. API'leri, ayrıntılı örnek düzeyi maliyetlerini görüntülemenizi sağlayan kullanım ayrıntılarını sağlar.

Bütçe maliyet denetimi bir parçası olarak yaygın olarak kullanılır. Azure'da bütçelerini sınırlayabilirsiniz. Örneğin, bütçe görünümünüzü abonelik, kaynak grupları veya kaynak koleksiyonunu göre daraltabilirsiniz. Kullanabileceğiniz bir bütçe Eşiğe ulaşıldığında e-posta ile bilgilendirmek üzere API bütçelerini kullanmanın yanı sıra [Azure İzleyici Eylem grupları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) eylemlerin bir bütçe olay sonucu olarak düzenlenmiş bir dizi tetiklemek için.

Bütçe karşı yönetin ve tahmin edilebilir bir bedeli baktığımda bir aylık fatura ayrıca Al istediklerinde kritik olmayan bir iş yükü çalıştıran bir müşteri için yaygın bir bütçe senaryo gerçekleşebilir. Bu senaryo, bazı Azure ortamının bir parçası olan kaynakların maliyet tabanlı düzenleme gerektirir. Bu senaryoda, 1000 abonelik için aylık bir bütçe ayarlanır. Ayrıca, bazı düzenlemeler tetiklemek için uyarı eşikleri ayarlanır. Bu senaryoda, kaynak grubundaki tüm Vm'leri durdurur bir maliyet % 80 eşiğini başlar **isteğe bağlı**. Ardından, % 100 maliyet eşiğine tüm sanal makine örnekleri durdurulur.
Bu senaryo yapılandırmak için bu öğreticideki her bölümde verilen adımları izleyerek aşağıdaki eylemleri tamamlanır.

Bu öğreticide dahil bu eylemleri için izin ver:

- Web kancalarını kullanarak sanal makineleri durdurmak için bir Azure Otomasyonu Runbook'u oluşturma.
- Bütçe eşiği değerine göre tetiklenmesi için bir Azure Logic App oluşturma ve doğru parametrelere sahip runbook çağırın.
- Bütçe eşiğine ulaşıldığında, Azure Logic App tetiklemek için yapılandırılmış bir Azure İzleyici eylem grubu oluşturun.
- Azure bütçe ile istediğiniz eşikleri oluşturun ve eylem grubuna bağlayabilirsiniz.

## <a name="create-an-azure-automation-runbook"></a>Azure Otomasyonu Runbook'u oluşturma

[Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) betik, kaynak yönetim görevlerinin çoğunu ve zamanlanmış veya isteğe bağlı olarak bu görevleri çalıştırmak sağlayan bir hizmettir. Bu senaryonun bir parçası olarak, oluşturacağınız bir [Azure Otomasyonu runbook'u](https://docs.microsoft.com/azure/automation/automation-runbook-types) durdurmak üzere kullanılır. Kullanacağınız [Azure V2 sanal makinesi durdurma](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafik runbook'tan [galeri](https://docs.microsoft.com/azure/automation/automation-runbook-gallery) bu senaryoyu oluşturmak için. Bu runbook, Azure hesabınızda oturum içeri aktarma ve yayımlama bütçe Eşiğe ulaşıldığında durdurmak mümkün olacaktır.

### <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com/) oturum açın.
2. Tıklayın **kaynak Oluştur** düğmesi Azure sol üst köşesinde bulunan.
3. Seçin **Yönetim Araçları** > **Otomasyon**.
   > [!NOTE]
   > Bir Azure hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
4. Hesap bilgilerini girin. İçin **oluşturma Azure farklı çalıştır hesabı**, seçin **Evet** Azure kimlik doğrulamasını kolaylaştırmak için gereken ayarları otomatik olarak sağlamak.
5. İşlemi tamamladığınızda Otomasyon hesabı dağıtımını başlatmak için **Oluştur**'a tıklayın.

### <a name="import-the-stop-azure-v2-vms-runbook"></a>Azure V2 sanal makinesi durdurma runbook'u İçeri Aktar

Kullanarak bir [Azure Otomasyonu runbook'u](https://docs.microsoft.com/azure/automation/automation-runbook-types), içeri aktarma [Azure V2 sanal makinesi durdurma](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafik runbook Galerisi.

1.  Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com/) oturum açın.
2.  Otomasyon hesabınızı seçerek açın **tüm hizmetleri** > **Otomasyon hesapları**. Ardından, Otomasyon hesabınızı seçin.
3.  Tıklayın **Runbook'lar Galerisi** gelen **süreç otomasyonu** bölümü.
4.  Ayarlama **galeri kaynağı** için **betik Merkezi** seçip **Tamam**.
5.  Bulun ve seçin [Azure V2 sanal makinesi durdurma](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) Azure portalındaki galeri öğesi.
6.  Tıklayın **alma** görüntülemek için düğmeyi **alma** dikey penceresinde ve select **Tamam**. Runbook genel bakış dikey penceresinde görüntülenir.
7.  Runbook içeri aktarma işlemi tamamlandıktan sonra seçin **Düzenle** grafik runbook düzenleyici ve yayımlama seçeneğini görüntülemek için.

    ![Azure - grafik runbook'u Düzenle](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-01.png)
8.  Tıklayın **Yayımla** runbook'u yayımlayamadı ve ardından düğmeyi **Evet** istendiğinde. Bir runbook yayımladığınızda, herhangi bir mevcut yayımlanan sürümü taslak sürümle geçersiz kılar. Bu durumda, runbook oluşturduğunuzdan hiçbir yayımlanan bir sürüme sahip.

    Runbook yayımlama hakkında daha fazla bilgi için bkz. [grafik runbook'u oluşturma](https://docs.microsoft.com/azure/automation/automation-first-runbook-graphical).

## <a name="create-webhooks-for-the-runbook"></a>Runbook için Web kancaları oluşturma

Kullanarak [Azure V2 sanal makinesi durdurma](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafiksel runbook tek bir HTTP isteği ile Azure Otomasyonu'nda runbook başlatmaya iki Web kancaları oluşturacaktır. İlk Web kancası durdurulması isteğe bağlı Vm'leri izin vererek bir parametre olarak bir kaynak grubu adı ile % 80 bütçe eşiği, runbook'u çağırır. Ardından, ikinci bir Web kancası, kalan tüm sanal makine örnekleri durdurulur parametresiz (% 100), bir runbook çağırır.

1. Gelen **runbook'ları** sayfasını [Azure portalında](https://portal.azure.com/), tıklayın **StopAzureV2Vm** runbook'un genel bakış dikey penceresinde görüntüler runbook.
2. Tıklayın **Web kancası** açmak için sayfanın üst kısmındaki **Web kancası Ekle** dikey penceresi.
3. Tıklayın **yeni Web kancası oluşturma** açmak için **yeni Web kancası oluşturma** dikey penceresi.
4. Ayarlama **adı** için Web kancasının **isteğe bağlı**. **Etkin** özelliği olmalıdır **Evet**. **Expires** değeri değiştirilmesi gerekmez. Web kancası özellikleri hakkında daha fazla bilgi için bkz. [bir Web kancası ayrıntılarını](https://docs.microsoft.com/azure/automation/automation-webhooks#details-of-a-webhook).
5. URL değerin yanında, Web kancası URL'sini kopyalamak için Kopyala simgesine tıklayın.
   > [!IMPORTANT]
   > Adlı Web kancası URL'sini Kaydet **isteğe bağlı** güvenli bir yerde. Bu öğreticide daha sonra URL'yi kullanır. Güvenlik nedeniyle, Web kancası oluşturulduktan sonra görüntüleyemez veya URL'sini yeniden alın.
6. Tıklayın **Tamam** yeni Web kancası oluşturmak için.
7. Tıklayın **parametrelerini yapılandırma ve çalıştırma ayarları** parametresi görüntülemek için runbook için değerler.
   > [!NOTE]
   > Runbook zorunlu parametrelere sahipse, sonra değerleri belirtilmediği sürece, Web kancası oluşturmanız mümkün değildir.
8. Tıklayın **Tamam** Web kancası parametre değerlerini kabul etmek için.
9. Tıklayın **Oluştur** Web kancası oluşturmak için.
10. Ardından, adlı ikinci bir Web kancası oluşturmak için yukarıdaki adımları **tam**.
    > [!IMPORTANT]
    > Her iki Web kancası URL'leri daha sonra Bu öğreticide kullanmak üzere kaydettiğinizden emin olun. Güvenlik nedeniyle, Web kancası oluşturulduktan sonra görüntüleyemez veya URL'sini yeniden alın.

Şimdi iki ile kullanılabilir her kaydettiğiniz URL'leri yapılandırılmış Web kancalarını da olmalıdır.

![Web kancaları - isteğe bağlı ve tam](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-02.png)

Azure Otomasyonu Kurulum artık hazırsınız. Web kancası çalışır durumda olduğunu doğrulamak için basit bir Postman test ile Web kancalarını test edebilirsiniz. Ardından, düzenleme için mantıksal uygulama oluşturmanız gerekir.

## <a name="create-an-azure-logic-app-for-orchestration"></a>Düzenleme için bir Azure Logic App oluşturma

Mantıksal uygulamalar oluşturun, zamanlayın ve böylece kuruluşlar veya kuruluşlar arasında uygulamaları, verileri, sistemleri ve Hizmetleri tümleştirebilirsiniz iş akışları olarak işlemleri otomatikleştirme yardımcı olur. Bu senaryoda, [mantıksal uygulama](https://docs.microsoft.com/azure/logic-apps/) oluşturduğunuz oluşturduğunuz Otomasyonu Web kancası çağrısı biraz daha fazlasını yapın.

Bütçe belirtilen eşiği karşılandığında bir uyarı tetiklemek için ayarlanabilir. Bildirim almak için birden çok eşikleri sağlayabilir ve mantıksal uygulama karşılanıyor eşiği temel alarak farklı eylemler gerçekleştirmesini becerisini gösterilecektir. Bu örnekte, ayarlar bütçesinin % 100 olduğunda ikinci bildirim bütçesinin % 80'e ulaştı ve ulaşıldığı için birkaç bildirimleri aldığınız bir senaryoyu oluşturan ilk bildirimidir. Mantıksal uygulama, kaynak grubundaki tüm Vm'leri kapatmak için kullanılır. İlk olarak, **isteğe bağlı** eşik % 80 sınırına ve Abonelikteki tüm sanal makineler burada kapatılacak ikinci Eşiğe ulaşıldığında.

Logic apps, HTTP tetikleyicisi için örnek bir şema sağlar, ancak, ayarlamanızı zorunlu izin **Content-Type** başlığı. Eylem grubu için Web kancasını özel üst bilgiler olmadığı için ayrı bir adım yükteki ayrıştırabilir gerekir. Kullanacağınız **ayrıştırma** eylem ve bir örnek yük ile sağlayın.

### <a name="create-the-logic-app"></a>Mantıksal uygulama oluşturma

Mantıksal uygulama çeşitli eylemler gerçekleştirir. Aşağıdaki listede, üst düzey bir mantıksal uygulama gerçekleştireceği eylemlerin kümesini sağlar:
- Bir HTTP isteği alındığında tanır.
- Geçirilen ulaşıldı eşik değeri belirlemek için JSON verilerini ayrıştırma
- % 100'e eşit veya eşik miktarı % 80 veya daha fazla bütçe aralığını ancak büyüktür ulaşıp ulaşmadığını kontrol etmek için bir koşullu ifade'ı kullanın.
    - Bu eşik miktarı sınırına adlı Web kancası kullanarak bir HTTP POST gönderin **isteğe bağlı**. Bu eylem, "İsteğe bağlı" grubundaki VM'lerin kapatır.
- Eşik miktarı ulaştı veya % Bütçe değeri 100'ü aştı olup olmadığını denetlemek için koşullu bir deyim kullanın.
    - Eşik miktarı sınırına adlı Web kancası kullanarak bir HTTP POST gönderin **tam**. Bu eylem, tüm kalan Vm'leri kapatacak.

Yukarıdaki adımları gerçekleştiren mantıksal uygulamanızı oluşturmak için aşağıdaki adımları gerekir:

1.  İçinde [Azure portalında](https://portal.azure.com/)seçin **kaynak Oluştur** > **tümleştirme** > **mantıksal uygulama**.

    ![Azure - mantıksal uygulama kaynağı seçin](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-03.png)
2.  İçinde **mantıksal uygulama oluştur** dikey penceresinde, select, mantıksal uygulamanızı oluşturmak gereken ayrıntılarını sağlamaya **panoya Sabitle**, tıklatıp **Oluştur**.

    ![Azure - mantıksal uygulama oluşturma](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-03a.png)

Azure mantıksal uygulamanızı dağıttıktan sonra **Logic Apps Tasarımcısı'nda** açılır ve bir giriş içeren bir dikey pencere, video ve sık kullanılan Tetikleyicileri gösterir.

### <a name="add-a-trigger"></a>Bir tetikleyici ekleme

Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her etkinleştirildiğinde Logic Apps altyapısı iş akışınızı başlatan ve çalıştıran bir mantıksal uygulama örneği oluşturur. Eylemler tetikleyiciden sonra gerçekleşen tüm adımlardır.

1.  Altında **şablonları** , **Logic Apps Tasarımcısı'nda** dikey penceresinde, seçin **boş mantıksal uygulama**.
2.  Ekleme bir [tetikleyici](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts) içinde "http isteği" girerek **Logic Apps Tasarımcısı'nda** bulmak ve adlı tetikleyici seçmek için arama kutusuna **isteği-zaman bir HTTP isteği alındığında**.

    ![Azure - mantıksal uygulama - Http tetikleyicisi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-04.png)
3.  Seçin **yeni adım** > **Eylem Ekle**.

    ![Azure - yeni adım - Eylem Ekle](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-05.png)
4.  İçin "JSON Ayrıştır" arama **Logic Apps Tasarımcısı'nda** bulmak ve seçmek için arama kutusuna **veri işlemleri - JSON Ayrıştır** [eylem](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts).

    ![Azure - mantıksal uygulama - ekleme eylemi JSON Ayrıştır](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-06.png)
5.  "Yükü" olarak girin **içeriği** adı için JSON Ayrıştır yükü veya dinamik içerik "Body" etiketini kullanın.
6.  Seçin **şema oluşturmak için örnek yük kullanma** seçeneğini **JSON Ayrıştır** kutusu.

    ![Şema oluşturmak için azure - mantıksal uygulama - örnek JSON verileri kullanın](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-07.png)
7.  Aşağıdaki JSON örnek yük aşağıdaki metin kutusuna yapıştırın: `{"schemaId":"AIP Budget Notification","data":{"SubscriptionName":"CCM - Microsoft Azure Enterprise - 1","SubscriptionId":"<GUID>","SpendingAmount":"100","BudgetStartDate":"6/1/2018","Budget":"50","Unit":"USD","BudgetCreator":"email@contoso.com","BudgetName":"BudgetName","BudgetType":"Cost","ResourceGroup":"","NotificationThresholdAmount":"0.8"}}`

    Metin kutusu aşağıdaki gibi görünür:

    ![Azure - mantıksal uygulama - örnek JSON yükü](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-08.png)
8.  **Bitti**’ye tıklayın.

### <a name="add-the-first-conditional-action"></a>İlk koşullu eylemi ekleme

% 100'e eşit veya eşik miktarı % 80 veya daha fazla bütçe aralığını ancak büyüktür ulaşıp ulaşmadığını kontrol etmek için bir koşullu ifade'ı kullanın. Bu eşik miktarı sınırına adlı Web kancası kullanarak bir HTTP POST gönderin **isteğe bağlı**. Bu işlem sanal makineleri kapatır **isteğe bağlı** grubu.

1.  Seçin **yeni adım** > **koşul Ekle**.

    ![Azure - mantıksal uygulama - koşul Ekle](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-09.png)
2.  İçinde **koşul** kutusunda, metin kutusu içeren **bir değer seçin** kullanılabilir değerler listesini görüntüleyin.

    ![Azure - mantıksal uygulama - koşul kutusu](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-10.png)

3.  Tıklayın **ifade** listenin üst kısmındaki ve ifade düzenleyicisinde şu ifadeyi girin: `float()`

    ![Azure - mantıksal uygulama - Float ifadesi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-11.png)

4.  Seçin **dinamik içerik**parantez () imleci yerleştirin ve seçin **NotificationThresholdAmount** tam ifade doldurmak için listeden.

    İfade, aşağıdakiler olur:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`

5.  Seçin **Tamam** ifade ayarlanamadı.
6.  Seçin **büyüktür veya eşittir** açılan kutusunda **koşul**.
7.  İçinde **bir değer seçin** koşulu kutusuna `.8`.

    ![Azure - mantıksal uygulama - Float ifade değeri](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-12.png)

8.  Tıklayın **Ekle** > **Satır Ekle** ek bir koşul bölümünü eklemek için koşulu kutusuna.
9.  İçinde **koşul** kutusunda, metin kutusu içeren **bir değer seçin**.
10. Tıklayın **ifade** listenin üst kısmındaki ve ifade düzenleyicisinde şu ifadeyi girin: `float()`
11. Seçin **dinamik içerik**parantez () imleci yerleştirin ve seçin **NotificationThresholdAmount** tam ifade doldurmak için listeden.
12. Seçin **Tamam** ifade ayarlanamadı.
13. Seçin **olduğu küçüktür** açılan kutusunda **koşul**.
14. İçinde **bir değer seçin** koşulu kutusuna `1`.

    ![Azure - mantıksal uygulama - Float ifade değeri](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-13.png)

15. İçinde **doğruysa** kutusunda **Eylem Ekle**. İsteğe bağlı Vm'leri kapatacak bir HTTP POST eylem ekleyeceksiniz.

    ![Azure - mantıksal uygulama - Eylem Ekle](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-14.png)

16. Girin **HTTP** HTTP eylemini arayın ve seçin için **HTTP-HTTP** eylem.

    ![Azure - mantıksal uygulama - ekleme HTTP eylemi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-15.png)

17. Seçin **Post** olarak için **yöntemi** değeri.
18. Adlı Web kancası URL'sini **isteğe bağlı** Bu öğreticinin önceki bölümünde oluşturduğunuz **URI** değeri.

    ![Azure - mantıksal uygulama - HTTP eylemi URI'si](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-16.png)

19. Seçin **Eylem Ekle** içinde **doğruysa** kutusu. İsteğe bağlı Vm'leri kapatıldığından alıcı bildiren bir e-posta gönderecek bir e-posta eylem ekleyeceksiniz.
20. "E-postası gönderme için" bulup seçin bir *e-posta Gönder* eylem tabanlı e-posta hizmetini kullanın.

    ![Azure - mantıksal uygulama - gönderme e-posta eylemi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-17.png)

    Kişisel Microsoft hesapları için **Outlook.com** girişini seçin. Azure iş veya okul hesapları için **Office 365 Outlook** girişini seçin. Önceden bir bağlantınız yoksa e-posta hesabınızda oturum açmanız istenir. Logic Apps, e-posta hesabınıza bir bağlantı oluşturur.

    Mantıksal uygulama, e-posta bilgilerinize erişmek izin gerekir.

    ![Azure - mantıksal uygulama - erişim bildirimi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-18.png)

21. Ekleme **için**, **konu**, ve **gövdesi** isteğe bağlı Vm'leri kapatıldığından alıcı bildiren e-posta metni. Kullanım **BudgetName** ve **NotificationThresholdAmount** konu ve gövde alanları doldurmak için dinamik içerik.

    ![Azure - mantıksal uygulama - e-posta ayrıntıları](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-19.png)

### <a name="add-the-second-conditional-action"></a>İkinci koşullu eylemi ekleme

Eşik miktarı ulaştı veya % Bütçe değeri 100'ü aştı olup olmadığını denetlemek için koşullu bir deyim kullanın. Eşik miktarı sınırına adlı Web kancası kullanarak bir HTTP POST gönderin **tam**. Bu eylem, tüm kalan Vm'leri kapatacak.

1.  Seçin **yeni adım** > **koşul Ekle**.

    ![Azure - mantıksal uygulama - Eylem Ekle](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-20.png)

2.  İçinde **koşul** kutusunda, metin kutusu içeren **bir değer seçin** kullanılabilir değerler listesini görüntüleyin.
3.  Tıklayın **ifade** listenin üst kısmındaki ve ifade düzenleyicisinde şu ifadeyi girin: `float()`
4.  Seçin **dinamik içerik**parantez () imleci yerleştirin ve seçin **NotificationThresholdAmount** tam ifade doldurmak için listeden.

    İfade, aşağıdakiler olur:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`

5.  Seçin **Tamam** ifade ayarlanamadı.
6.  Seçin **büyüktür veya eşittir** açılan kutusunda **koşul**.
7.  İçinde **Seç değer kutusu** koşulunu girin `1`.

    ![Azure - mantıksal uygulama - koşul değerini ayarla](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-21.png)

8.  İçinde **doğruysa** kutusunda **Eylem Ekle**. Tüm kalan Vm'leri kapatacak bir HTTP POST eylem ekleyeceksiniz.

    ![Azure - mantıksal uygulama - Eylem Ekle](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-22.png)

9.  Girin **HTTP** HTTP eylemini arayın ve seçin için **HTTP-HTTP** eylem.
10. Seçin **Post** olarak için **yöntemi** değeri.
11. Adlı Web kancası URL'sini **tam** Bu öğreticinin önceki bölümünde oluşturduğunuz **URI** değeri.

    ![Azure - mantıksal uygulama - Eylem Ekle](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-23.png)

12. Seçin **Eylem Ekle** içinde **doğruysa** kutusu. Kalan VM'ler kapatıldığından alıcı bildiren bir e-posta gönderecek bir e-posta eylem ekleyeceksiniz.
13. "E-postası gönderme için" bulup seçin bir *e-posta Gönder* eylem tabanlı e-posta hizmetini kullanın.
14. Ekleme **için**, **konu**, ve **gövdesi** isteğe bağlı Vm'leri kapatıldığından alıcı bildiren e-posta metni. Kullanım **BudgetName** ve **NotificationThresholdAmount** konu ve gövde alanları doldurmak için dinamik içerik.

    ![Azure - mantıksal uygulama - gönderme e-posta ayrıntıları](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-24.png)

15. Tıklayın **Kaydet** en üstündeki **mantıksal Uygulama Tasarımcısı** dikey penceresi.

### <a name="logic-app-summary"></a>Mantıksal uygulama özeti

İşiniz bittiğinde mantıksal uygulamanız nasıl göründüğünü aşağıda verilmiştir. İçinde en temel senaryo yok gerek duyduğunuz herhangi bir eşik tabanlı düzenleme Otomasyon betiği doğrudan çağırabilir **İzleyici** ve atlama **mantıksal uygulama** adım.

   ![Azure - mantıksal uygulama - tam görünümü](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-25.png)

Mantıksal uygulamanızı kaydettiğinizde, bir URL arayabilmesi için olacaktır oluşturuldu. Bu URL'ye Bu öğreticinin sonraki bölümde kullanacaksınız.

## <a name="create-an-azure-monitor-action-group"></a>Azure İzleyici eylem grubu oluştur

Bir eylem grubu, tanımladığınız bildirim tercihleri koleksiyonudur. Bir uyarı tetiklendiğinde, belirli bir eylem grubu bildirim tarafından uyarı alabilirsiniz. Azure uyarı proaktif olarak belirli koşullara göre bir bildirim oluşturur ve eylem olanağı sunar. Bir uyarı, ölçüm ve günlükleri de dahil olmak üzere birden fazla kaynaktan veri kullanabilirsiniz.

Eylem, bütçenizi tümleştirilecek tek bir uç nokta gruplarıdır. Çok sayıda kanalı bildirimlerini ayarlayabilirsiniz, ancak bu öğreticide daha önce oluşturduğunuz mantıksal uygulama üzerinde odaklanır. Bu senaryo için.

### <a name="create-an-action-group-in-azure-monitor"></a>Azure İzleyici'de bir eylem grubu oluştur

Eylem grubu oluşturduğunuzda, bu öğreticide daha önce oluşturduğunuz mantıksal uygulamaya yönlendirir.

1.  Zaten oturum açmış için kök kullanıcı değilseniz [Azure portalında](https://portal.azure.com/), oturum açma ve seçin **tüm hizmetleri** > **İzleyici**.
2.  Seçin **Eylem grupları** gelen **ayarı** bölümü.
3.  Seçin **eylem grubu ekleme** gelen **Eylem grupları** dikey penceresi.
4.  Ekleyin ve aşağıdaki öğeleri doğrulayın:
    - Eylem grubu adı
    - Kısa ad
    - Abonelik
    - Kaynak grubu

    ![Azure - mantıksal uygulama - bir eylem grubu Ekle](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-26.png)

5.  İçinde **eylem grubu Ekle** bölmesinde, bir mantıksal uygulama eylemi ekleyin. Eylem adı **bütçe BudgetLA**. İçinde **mantıksal uygulama** bölmesinde **abonelik** ve **kaynak grubu**. Ardından, **mantıksal uygulama** Bu öğreticide daha önce oluşturduğunuz.
6.  Tıklayın **Tamam** mantıksal uygulama ayarlamak için. Ardından, **Tamam** içinde **eylem grubu Ekle** bölmesinde eylem grubunu oluşturmak için.

Bütçenizi verimli düzenlemek için gerekli tüm destekleyici bileşenleri ile hazırsınız. Şimdi tek yapmanız gereken bütçenin oluşturabilir ve oluşturduğunuz eylem grubu kullanacak şekilde yapılandırın.

## <a name="create-the-azure-budget"></a>Azure bütçe oluşturun

Portal kullanarak azure'da bir bütçe oluşturabilirsiniz [bütçe özellik](../cost-management/tutorial-acm-create-budgets.md) maliyet Yönetimi'nde. Veya REST API'ler, Powershell cmdlet'lerini kullanarak bir bütçe oluşturun veya CLI kullanın. Aşağıdaki yordam, REST API kullanır. REST API'yi çağırmadan önce bir yetkilendirme belirteci gerekir. Bir yetkilendirme belirteci oluşturmak için kullanabileceğiniz [ARMClient](https://github.com/projectkudu/ARMClient) proje. **ARMClient** kendiniz için Azure Resource Manager kimliğini doğrulamak ve API'leri çağırmak için bir belirteç almak sağlar.

### <a name="create-an-authentication-token"></a>Bir kimlik doğrulama belirteci oluştur

1.  Gidin [ARMClient](https://github.com/projectkudu/ARMClient) github'daki proje.
2.  Yerel bir kopyasını almak için deposunu kopyalayın.
3.  Projeyi Visual Studio'da açın ve derleyin.
4.  Derleme başarılı olduktan sonra yürütülebilir dosya içinde olmalıdır *\bin\debug* klasör.
5.  ARMClient çalıştırın. Bir komut istemi açın ve gidin *\bin\debug* proje kök klasöründen.
6.  Oturum açma ve kimlik doğrulaması yapmak için komut isteminde aşağıdaki komutu girin:<br>
    `ARMClient login prod`
7.  Kopyalama **abonelik guid'ini** çıktısından.
8.  Bir yetkilendirme belirteci panonuza kopyalamak için komut isteminde, ancak emin yukarıdaki adımdan kopyalanan abonelik kimliği kullanmak aşağıdaki komutu girin: <br>
    `ARMClient token <subscription GUID from previous step>`

    Yukarıdaki adımı tamamladıktan sonra aşağıdaki görürsünüz:<br>
    **Belirteç başarıyla panoya kopyalandı.**
9.  Bu öğreticinin sonraki bölümünde açıklanan adımları izleyerek için kullanılacak belirteci kaydedin.

### <a name="create-the-budget"></a>Bütçe oluşturun

Ardından, yapılandıracağınız **Postman** Azure tüketim REST API'lerini çağırarak bir bütçe oluşturmak için. Postman, bir API geliştirme ortamıdır. Postman içinde ortam ve koleksiyon dosyaları içeri aktaracak. Koleksiyon Azure tüketim REST API'lerini çağırma HTTP isteklerinin gruplandırılmış tanımları içerir. Koleksiyon tarafından kullanılan değişkenleri ortam dosyası içerir.

1.  İndir ve Aç [Postman REST istemcisi](https://www.getpostman.com/) REST API'lerini yürütülecek.
2.  Postman içinde yeni bir istek oluşturun.

    ![Postman - yeni bir istek oluşturun](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-27.png)

3.  Yeni istek hiçbir şey üzerinde sahip olacak şekilde yeni istek koleksiyon olarak kaydedin.

    ![Postman - yeni isteği Kaydet](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-28.png)

4.  Değişiklik isteğini bir `Get` için bir `Put` eylem.
5.  Değiştirerek aşağıdaki URL'yi Değiştir `{subscriptionId}` ile **abonelik kimliği** Bu öğreticinin önceki bölümünde kullanılan. Ayrıca, "SampleBudget" değeri olarak dahil edilecek URL değiştirme `{budgetName}`: `https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2018-03-31`
6.  Seçin **üstbilgileri** Postman içine sekmesi.
7.  Yeni bir **anahtar** "Yetkilendirme" adlı.
8.  Ayarlama **değer** ArmClient son bölümde sonunda kullanılarak oluşturulmuş belirtecine.
9.  Seçin **gövdesi** Postman içine sekmesi.
10. Seçin **ham** seçenek düğmesini.
11. Metin kutusuna yapıştırın örnek bütçe tanımı ancak gerekir değiştirdiğiniz **subscriptionıd**, **budgetname**, ve **actiongroupname** parametrelerle, Abonelik kimliği, bütçenizi için benzersiz bir ad ve hem URL hem de istek gövdesi içinde oluşturulan eylem grubu adı:

    ```
        {
            "properties": {
                "category": "Cost",
                "amount": 100.00,
                "timeGrain": "Monthly",
                "timePeriod": {
                "startDate": "2018-06-01T00:00:00Z",
                "endDate": "2018-10-31T00:00:00Z"
                },
                "filters": {
                },
            "notifications": {
                "Actual_GreaterThan_80_Percent": {
                    "enabled": true,
                    "operator": "GreaterThan",
                    "threshold": 80,
                    "contactEmails": [
                    ],
                    "contactRoles": [
                    ],
                    "contactGroups": [
                    "/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/microsoft.insights/actionGroups/{actiongroupname}
                    ]
                },
            "Actual_EqualTo_100_Percent": {
                    "operator": "EqualTo",
                    "threshold": 100,
                    "contactGroups": [
                "/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/microsoft.insights/actionGroups/{actiongroupname}"
                    ]
                }
            }
        }
    ```
12. Tuşuna **Gönder** isteği göndermek için.

Çağırmak için gereken tüm parçaları artık sahip [API bütçesinin](https://docs.microsoft.com/rest/api/consumption/budgets). Bütçelerini API Başvurusu, aşağıdakiler dahil olmak üzere belirli istekler hakkında ayrıntılar bulunur:
    - **budgetName** -birden çok bütçe desteklenir.  Bütçe adları benzersiz olmalıdır.
    - **Kategori** -olmalıdır **maliyet** veya **kullanım**. API hem maliyet ve kullanım bütçeleriyle destekler.
    - **timeGrain** -aylık, üç aylık veya yıllık bir bütçe. Tutarı, dönem sonunda sıfırlar.
    - **filtreler** -filtreler seçili kapsamındaki kaynakların belirli bir dizi bütçeye daraltmak sağlar. Örneğin, bir filtre koleksiyonu için bir abonelik düzeyi bütçe kaynak grupları olabilir.
    - **bildirimleri** – eşikleri ve bildirim ayrıntılarını belirler. Birden çok eşikleri ayarlayın ve bir e-posta adresi veya bir bildirim almak için bir eylem grubu sağlayın.

## <a name="summary"></a>Özet

Bu öğreticideki adımları uygulayarak şunları öğrendiniz:
- Sanal makineleri durdurmak için bir Azure Otomasyonu Runbook'u oluşturma
- Tetiklenen Azure mantıksal uygulaması oluşturma, bütçe eşiği değerlerine göre ve ilişkili runbook doğru parametrelere sahip çağırın.
- Barındıracak Azure İzleyici eylem grubu oluşturma, bütçe eşiğine ulaşıldığında, Azure Logic App tetiklemek için yapılandırıldı.
- Azure oluşturma ile istediğiniz eşikleri bütçe ve eylem grubuna bağlayabilirsiniz.

Yapılandırılan bütçe eşikleri ulaştığında artık tam olarak işlevsel bir bütçe kapatacak, aboneliğiniz için Vm'lerinizi sahipsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Fatura Azure senaryolarla ilgili daha fazla bilgi için bkz. [faturalandırma ve maliyet Yönetimi Otomasyon senaryoları](billing-cost-management-automation-scenarios.md).
