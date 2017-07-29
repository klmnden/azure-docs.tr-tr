---
title: "Operations Management Suite Güvenlik ve Denetim Çözümünü kullanmaya başlama | Microsoft Belgeleri"
description: "Bu belge, karma bulutunuzu izlemek için Operations Management Suite Güvenlik ve Denetim çözümü becerilerini kullanmaya başlamanıza yardımcı olur."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 754796ef-a43e-468a-86c9-04a2eda55b5b
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: get-started-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 5753511d26c06f385fd4ff717d8592c321338172
ms.contentlocale: tr-tr
ms.lasthandoff: 06/17/2017

---
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Operations Management Suite Güvenlik ve Denetim Çözümünü kullanmaya başlama
Bu belge, size her bir seçenekte yol göstererek Operations Management Suite (OMS) Güvenlik ve Denetim çözümü becerilerini hızlıca kullanmaya başlamanıza yardımcı olur.

## <a name="what-is-oms"></a>OMS nedir?
Microsoft Operations Management Suite (OMS), şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan, Microsoft'un bulut tabanlı BT yönetim çözümüdür. OMS hakkında daha fazla bilgi için bkz. [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>OMS Güvenlik ve Denetim panosu
OMS Güvenlik ve Denetim çözümü, ilgilenmenizi gerektiren önemli sorunlar için şirket içi arama sorgularıyla kuruluşunuzun IT güvenlik duruşuna ilişkin kapsamlı bir görünüm sağlar. **Güvenlik ve Denetim** panosu, OMS'de yer alan güvenlikle ilgili tüm öğelere ilişkin giriş ekranıdır. Bu pano, size bilgisayarlarınızın güvenlik durumuyla ilgili yüksek düzeyde öngörü sağlar. Ayrıca son 24 saat, 7 gün veya herhangi bir özel zaman dilimine ait tüm olayları görüntüleme becerisine sahiptir. **Güvenlik ve Denetim** panosuna erişmek için şu adımları izleyin:

1. **Microsoft Operations Management Suite** ana panosunun sol kısmında yer alan **Ayarlar** kutucuğuna tıklayın.
2. **Ayarlar** dikey penceresinde, **Çözümler** altında **Güvenlik ve Denetim** seçeneğine tıklayın.
3. **Güvenlik ve Denetim** panosu görüntülenir:
   
    ![OMS Güvenlik ve Denetim panosu](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Bu panoya ilk kez erişiyorsanız ve OMS tarafından izlenen cihazlarınız yoksa kutucuklar aracıdan elde edilen verilerle doldurulmaz. Aracıyı yükledikten sonra kutucukların doldurulması biraz zaman alabilir. Bu nedenle, veriler hâlâ buluta yüklenmekte olduğundan, ilk gördüğünüz ekranda bazı veriler eksik olabilir.  Bu durumda, somut bilgiler içermeyen bazı kutucuklar görmeniz normaldir. OMS aracısının Windows sistemine yüklenmesiyle ilgili daha fazla bilgi için bkz. [Windows bilgisayarlarını doğrudan OMS'ye bağlama](https://technet.microsoft.com/library/mt484108.aspx) ve bu görevin bir Linux sisteminde nasıl gerçekleştirileceği hakkında daha fazla bilgi için bkz. [Linux bilgisayarları OMS'ye bağlama](https://technet.microsoft.com/library/mt622052.aspx).

> [!NOTE]
> Aracı, etkinleştirilmiş geçerli olaylara bağlı olarak bilgileri toplar; örneğin bilgisayar adı, IP adresi ve kullanıcı adı. Ancak belgeler/dosyalar, veritabanı adı veya özel veriler toplanmaz.   
> 
> 

Çözümler, önemli müşteri taleplerine yönelik mantık, görselleştirme ve veri alımı kurallarından oluşan bir koleksiyondur. Güvenlik ve Denetim tek bir çözümdür; diğer çözümler ayrıca eklenebilir. Yeni bir çözüm ekleme hakkında daha fazla bilgi için bkz. [Çözümleri ekleme](https://technet.microsoft.com/library/mt674635.aspx).

OMS Güvenlik ve Denetim panosu dört ana kategoride düzenlenmiştir:

* **Güvenlik Etki Alanları**: Bu alanda zamanla güvenlik kayıtlarını daha çok keşfedecek, kötü amaçlı yazılım değerlendirmesine erişebilecek, değerlendirme, ağ güvenliği, kimlik ve erişim bilgileri ve bilgisayarları güvenlik olaylarıyla güncelleştirebilecek ve Azure Güvenlik Merkezi panosuna hızlıca erişebileceksiniz.
* **Önemli Sorunlar**: Bu seçenek etkin sorunların sayısını ve sorunların önem derecesini hızlıca tanımlamanıza olanak verir.
* **Algılama (Önizleme)**: Kaynaklarınıza karşı gerçekleşen güvenlik uyarılarını görselleştirerek saldırı düzenlerini tanımlayabilmenizi sağlar.
* **Tehdit Bilgileri**: Giden kötü amaçlı IP trafiğine sahip sunucuların toplam sayısını, kötü amaçlı tehdit türünü ve bu IP'lerin nereden geldiğini gösteren bir haritayı görselleştirerek saldırı düzenlerini tanımlayabilmenizi sağlar. 
* **Sık kullanılan güvenlik sorguları**: Bu seçenek size ortamınızı izlemek için kullanabileceğiniz en sık kullanılan güvenlik sorgularının listesini sağlar. Bu sorgulardan birine tıkladığınızda, bu sorguya ilişkin sonuçları içeren **Arama** dikey penceresi açılır.

> [!NOTE]
> OMS'nin verilerinizi nasıl koruduğuna ilişkin daha fazla bilgi için bkz. OMS verilerinizin güvenliğini nasıl sağlar?.
> 
> 

## <a name="security-domains"></a>Güvenlik etki alanları
Kaynaklarınızı izlerken, ortamınızın geçerli durumuna hızlı bir şekilde erişebilmeniz önemlidir. Ancak aynı zamanda ortamınızda herhangi bir tarihte neler olduğunu daha iyi anlamanızı sağlamak için geçmişte gerçekleşen olayları izleyebilmek de önemlidir. 

> [!NOTE]
> Veri saklama OMS fiyatlandırma planına göre yapılır. Daha fazla bilgi için bkz. [Microsoft Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) fiyatlandırma sayfası.
> 
> 

Olay yanıtı ve adli tıp araştırma senaryoları doğrudan **Zaman İçindeki Güvenlik Kayıtları** kutucuğunda mevcut olan sonuçlardan yararlanır.

![Zaman içindeki güvenlik kayıtları](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Bu kutucuğa tıkladığınızda, aşağıda gösterildiği gibi son yedi güne ilişkin verileri içeren **Güvenlik Olayları** (tür=SecurityEvents) için sorgu sonucunu gösteren **Arama** dikey penceresi açılır:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Zaman içindeki güvenlik kayıtları](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Arama sonucu iki bölmeye ayrılır: Sol bölme size bulunan güvenlik olayları sayısının çözümlemesini, bu olayların bulunduğu bilgisayarları, bu bilgisayarlarda bulunan hesapların sayısını ve etkinlik türlerini sağlar. Sağ bölme ise size toplam sonuçları ve güvenlik olaylarının bilgisayar adı ile olay etkinliğini içeren kronolojik bir görünümünü sağlar. Ayrıca, bu olay hakkında olay verileri, olay kimliği ve olay kaynağı gibi daha fazla ayrıntıyı görüntülemek için **Daha Fazla Göster**'e tıklayabilirsiniz.

> [!NOTE]
> OMS arama sorgusu hakkında daha fazla bilgi için bkz. [OMS arama başvurusu](https://technet.microsoft.com/library/mt450427.aspx).
> 
> 

### <a name="antimalware-assessment"></a>Kötü amaçlı yazılımdan koruma değerlendirmesi
Bu seçenek yetersiz korumaya sahip ve kötü amaçlı bir yazılım tarafından güvenliği aşılmış olan bilgisayarları hızlıca tanımlamanızı sağlar. Kötü amaçlı yazılım değerlendirme durumu ve izlenen sunucularda algılanan tehditler okunur ve ardından veriler işlenmesi için buluttaki OMS hizmetine gönderilir. **Kötü Amaçlı Yazılımdan Koruma Değerlendirmesi** kutucuğuna tıklanarak erişilen kötü amaçlı yazılım değerlendirme panosunda, algılanan tehditleri içeren sunucular ve yetersiz korumaya sahip sunucular görüntülenir. 

![kötü amaçlı yazılım değerlendirmesi](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

OMS Panosunda yer alan tüm diğer canlı kutucuklarda olduğu gibi, üzerine tıkladığınızda sorgu sonucunu içeren **Arama** dikey penceresi açılır. Bu seçenekte, **Koruma Durumu** altındaki **Raporlama Yok** seçeneğine tıkladığınızda, aşağıda gösterildiği gibi bilgisayarın adını ve sıralamasını içeren tek bir girdiyi gösteren sorgu sonucuna erişirsiniz:

![arama sonucu](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [!NOTE]
> *sıralama* koruma durumunu (açık, kapalı, güncelleştirilmiş vb.) ve bulunan tehditleri yansıtmak üzere belirlenen bir derecedir. Bu değere sayısal olarak sahip olmak, toplama yapmanıza yardımcı olur.
> 
> 

Bilgisayarın adına tıkladığınızda, bu bilgisayara ilişkin koruma durumunun kronolojik bir görünümünü elde edersiniz. Bu, kötü amaçlı yazılımdan koruma yazılımının daha önceden yüklenmiş olup bir noktada kaldırıldığını anlamanız gereken senaryolarda çok yararlıdır.   

### <a name="update-assessment"></a>Güncelleştirme değerlendirmesi
Bu seçenek olası güvenlik sorunlarına genel olarak maruz kalma durumunu ve bu güncelleştirmelerin ortamınız için ne kadar kritik olduğunu hızlıca belirleyebilmenizi sağlar. OMS Güvenlik ve Denetim çözümü yalnızca bu güncelleştirmelere ilişkin bir görselleştirme sağlar; gerçek veriler OMS içinde farklı bir modül olan [Güncelleştirme Yönetimi Çözümleri](oms-solution-update-management.md)'nden gelir. Güncelleştirmelerin bir örneği aşağıda verilmiştir:

![sistem güncelleştirmeleri](./media/oms-security-getting-started/oms-getting-started-fig6-new.png)

> [!NOTE]
> Güncelleştirme Yönetimi çözümü hakkında daha fazla bilgi için, [OMS’de Güncelleştirme Yönetimi çözümü](oms-solution-update-management.md) konusunu okuyun.
> 
> 

### <a name="identity-and-access"></a>Kimlik ve Erişim
Kimlik, kuruluşunuz için denetim düzlemi olmalıdır, kimliğinizi korumak ise en yüksek önceliğiniz olmalıdır. Geçmişte kuruluşlar etrafında çevresel alanlar vardı ve bu alanlar birincil savunma sınırları olarak görev yapardı, ancak günümüzde giderek daha fazla veri ve uygulamanın buluta taşınmasıyla birlikte, kimlik yeni çevresel alan haline geldi. 

> [!NOTE]
> Veriler şu anda gelecekteki Office365 oturum açmalarında yalnızca Güvenlik Olayları oturum açma verilerini (olay kimliği 4624) temel alır ve Azure AD verileri de dahil edilir.
> 
> 

Kimlik etkinliklerinizi izleyerek bir olay gerçekleşmeden önce öngörülü eylemlerde veya bir saldırı girişimini durdurmak için reaktif eylemlerde bulunabilirsiniz. **Kimlik ve Erişim** panosu, size hatalı oturum açma girişimlerinin sayısını, bu girişimler sırasında kullanılan kullanıcı hesabını, kilitlenmiş hesapları, parolaları değiştirilen veya sıfırlanan hesapları ve şu anda oturum açmış olan hesap sayısını içerecek şekilde, kimlik durumunuza ilişkin bir genel bakış sağlar. 

**Kimlik ve Erişim** kutucuğuna tıkladığınızda, şu panoyu göreceksiniz:

![kimlik ve erişim](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Bu panoda mevcut olan bilgiler, olası bir şüpheli etkinliğini tanımlamanız için size hemen yardımcı olabilir. Örneğin, **Yönetici** olarak 338 defa oturum açma girişimi gerçekleşmiş ve bunların %100'ü başarısız olmuş olabilir. Bu durum, bu hesaba karşı gerçekleştirilen bir deneme yanılma saldırısından kaynaklanıyor olabilir. Bu hesaba tıkladığınızda, bu olası saldırının hedef kaynağını belirlemenize yardım edecek daha fazla bilgi elde edersiniz:

![arama sonuçları](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Ayrıntılı rapor bu olay hakkında hedef bilgisayar, oturum açma türü (bu durumda Ağ oturumu açma), etkinlik (bu durumda olay 4625) ve her girişimin kapsamlı bir zaman çizelgesini içeren önemli bilgileri sağlar. 

### <a name="computers"></a>Bilgisayarlar
Bu kutucuk, etkin bir şekilde güvenlik olayları içeren tüm bilgisayarlara erişmek için kullanılabilir. Bu kutucuğa tıkladığınızda, güvenlik olayları içeren bilgisayarların listesini ve her bilgisayardaki olay sayısını göreceksiniz:

![Bilgisayarlar](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Her bir bilgisayara tıklayarak araştırmanızı devam ettirebilir ve bayrak eklenmiş güvenlik olaylarını inceleyebilirsiniz.

### <a name="threat-intelligence"></a>Tehdit Bilgisi

OMS Güvenliği ve Denetimi’nde sağlanan Tehdit Bilgisi seçeneğini kullanarak, BT yöneticileri ortama yönelik güvenlik tehditlerini belirleyebilir, örneğin belirli bir bilgisayarın botnetin parçası olup olmadığını saptayabilir. Bilgisayar korsanları yasa dışı yollarla bu bilgisayarı bir komut veya denetime bağlayan kökü amaçlı bir yazılım yüklediklerinde, bilgisayarlar botnette düğümlere dönüşür. Ayrıca, darknet gibi yeraltı iletişim kanallarından gelen olası tehditleri de belirleyebilir. Tehdit Bilgileri hakkında daha fazla bilgi edinmek için, [Operations Management Suite Güvenlik ve Denetim Çözümü’nde güvenlik uyarılarını izleme ve yanıtlama](oms-security-responding-alerts.md) makalesini okuyun.

Bazı senaryolarda, izlenen bir bilgisayardan erişilen olası bir kötü amaçlı IP ile karşılaşabilirsiniz:

![Tehdit bilgisi haritası](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

Bu uyarı ve aynı kategoride bulunan diğer uyarılar OMS Güvenliği aracılığıyla, [Microsoft Tehdit Bilgileri](https://youtu.be/O4WtxgUrDc8)'nden yararlanılarak oluşturulur. Microsoft tarafından toplananların yanı sıra önde gelen tehdit bilgisi sağlayıcılarından satın alınan Tehdit Bilgisi verileri de bulunur. Bu veriler sıklıkla güncelleştirilir ve hızlı hareket eden tehditlere uyarlanır. Doğası gereği bu verilerin, bir güvenlik uyarısını [araştırma](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) sürecinde diğer güvenlik bilgisi kaynaklarıyla birleştirilmesi gerekir. 

### <a name="baseline-assessment"></a>Temel Değerlendirme

Dünya çapındaki sektör ve devlet kuruluşlarıyla birlikte Microsoft, yüksek güvenlikli sunucu dağıtımlarını temsil eden bir Windows yapılandırması tanımlar. Bu yapılandırma, kayıt defteri anahtarlarının, denetim ilkesi ayarlarının ve güvenlik ilkesi ayarlarının yanı sıra Microsoft'un bu ayarlar için önerilen değerlerinden oluşan bir kümedir. Bu kural kümesi, Güvenlik temeli olarak bilinir. Bu seçenek hakkında daha fazla bilgi için [Operations Management Suite Güvenlik ve Denetim Çözümünde Temel Değerlendirmesi](oms-security-baseline.md) makalesini okuyun.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Bu kutucuk temel olarak Azure Güvenlik Merkezi panosuna erişmek için bir kısayol görevi yapar. Bu çözüm hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezini Kullanmaya Başlama](../security-center/security-center-get-started.md).

## <a name="notable-issues"></a>Önemli sorunlar
Bu seçenek grubunun ana amacı, ortamınızda bulunan sorunları Kritik, Uyarı ve Bilgilendirici olarak kategorilere ayırarak bu sorunlara ilişkin hızlı bir görünüm sağlamaktır. Etkin sorun türü kutucuğu - bu sorunlara ilişkin bir görselleştirmedir, ancak sorunlar hakkında daha fazla ayrıntıyı keşfetmenize izin vermez. Bunun için sorunun adını (NAME), sorunun kaç nesnede gerçekleştiğini (COUNT) ve sorunun ne kadar kritik olduğunu (SEVERITY) gösteren kutucuğun alt bölümünü kullanmanız gerekir.

![Önemli sorunlar](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Bu sorunların daha önceden **Güvenlik Etki Alanları** grubundaki farklı alanlarda ele alındığını görebilirsiniz; bu durum bu görünümün amacını pekiştirir: Ortamınızdaki en önemli sorunları tek bir yerden görselleştirmek.

## <a name="detections-preview"></a>Algılama (Önizleme)
Bu seçeneğin ana amacı, IT'nin ortamındaki olası tehditleri ve bu tehditlerin önem derecesini hızlıca tanımlamasını sağlamaktır.

![Tehdit Bilgisi](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Bu seçenek bir [olay yanıtı araştırması](https://blogs.msdn.microsoft.com/azuresecurity/2016/11/30/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) sırasında değerlendirme gerçekleştirmek ve saldırı hakkında daha fazla bilgi elde etmek için de kullanılabilir.

> [!NOTE]
> OMS'nin Olay Yanıtı için nasıl kullanılabileceği hakkında daha fazla bilgi için şu videoyu izleyin: [Olay Yanıtı için Azure Güvenlik Merkezi'nden ve Microsoft Operations Management Suite'ten Yararlanma](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).
> 
> 

## <a name="threat-intelligence"></a>Tehdit Bilgisi
Güvenlik ve Denetim çözümünün yeni tehdit bilgisi bölümü olası saldırı düzenlerini çeşitli şekillerde görselleştirir: Giden kötü amaçlı IP trafiğinin toplam sayısı, kötü amaçlı tehdit türü ve bu IP'lerin nereden geldiğini gösteren bir harita. Haritayla etkileşim kurabilir ve daha fazla bilgi için IP'lere tıklayabilirsiniz.

Harita üzerindeki sarı raptiyeler kötü amaçlı IP'lerden gelen trafiği belirtir. İnternet'e bağlı sunucuların gelen kötü amaçlı trafiği görmesi alışılmamış bir durum değildir, ancak biz yine de saldırılardan hiçbirinin başarılı olmadığından emin olmak için bu saldırıları incelemenizi öneririz. Bu göstergeler IIS günlüklerini, İletilen Verileri ve Windows Güvenlik Duvarı günlüklerini temel alır.  

![Tehdit Bilgisi](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Ortak güvenlik sorguları
Mevcut ortak güvenlik sorguları listesi, kaynağın bilgilerine hızlıca ulaşmanız ve bunları ortamınızın ihtiyaçlarına göre özelleştirmeniz için yararlı olabilir. Bu ortak sorgular şunlardır:

* Tüm Güvenlik Etkinlikleri
* "bilgisayar01.contoso.com" (kendi bilgisayar adınızla değiştirin) bilgisayarındaki Güvenlik Etkinlikleri
* "Yönetici" hesabı için "bilgisayar01.contoso.com" bilgisayarındaki Güvenlik Etkinlikleri (kendi bilgisayar ve hesap adlarınızla değiştirin)
* Bilgisayara göre Oturum Açma Etkinliği
* Herhangi bir bilgisayarda Microsoft kötü amaçlı yazılımdan koruma yazılımını sonlandıran hesaplar
* Microsoft kötü amaçlı yazılımdan koruma işleminin sonlandırıldığı bilgisayarlar
* "Hash.exe" işleminin (farklı bir işlem adıyla değiştirin) yürütüldüğü bilgisayarlar
* Yürütülen tüm İşlem adları
* Hesaba göre Oturum Açma Etkinliği
* "Bilgisayar01.contoso.com" (kendi bilgisayarınızın adıyla değiştirin) bilgisayarında uzaktan oturum açan hesaplar

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede size OMS Güvenlik ve Denetim çözümü tanıtılmaktadır. OMS Güvenlik hakkında daha fazla bilgi edinmek için şu makalelere göz atın:

* [Operations Management Suite'e (OMS) genel bakış](operations-management-suite-overview.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Güvenlik Uyarılarını İzleme ve Yanıtlama](oms-security-responding-alerts.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Kaynakları İzleme](oms-security-monitoring-resources.md)


