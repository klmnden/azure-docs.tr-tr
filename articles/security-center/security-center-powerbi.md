<properties
   pageTitle="Power BI ile Azure Güvenlik Merkezi verilerinden öngörü edinme | Microsoft Azure"
   description="Azure Güvenlik Merkezi Power BI içerik paketi, raporlama işleminiz için oluşturulan veri kümesi tabanlı güvenlik uyarılarının, önerilerin, saldırıya uğrayan kaynakların ve eğilimlerin bulunmasını kolaylaştırır."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/03/2016"
   ms.author="yurid"/>

# Power BI ile Azure Güvenlik Merkezi verilerinden öngörü edinme
Azure Güvenlik Merkezi için [Power BI Panosu](http://aka.ms/azure-security-center-power-bi), önerileri ve güvenlik uyarılarını her yerden (mobil cihazınız dahil) görselleştirmenizi, çözümlemenizi ve filtrelemenizi etkinleştirir. Power BI panosunu, eğilimleri ve saldırı desenlerini göstermek için kullanma - Kaynağa veya IP adresine göre güvenlik uyarılarını ve kaynak ya da yaşa göre adresi olmayan güvenlik risklerini görüntüler. Ayrıca, Güvenlik Merkezi önerileri ve güvenlik uyarılarını diğer verilerle ilginç şekillerde birleştirebilirsiniz; örneğin, Power BI panolarını sunan [Azure Denetim Günlükleri](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/)'ni ve [Azure SQL Database Denetleme](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/)'yi dahil edebilirsiniz veya bulut kaynaklarınızın güvenlik durumuna ilişkin raporlamanın kolay olması için bu verileri Excel'e aktarabilirsiniz.

> [AZURE.NOTE] Bu belgedeki bilgiler Azure Güvenlik Merkezi önizleme sürümü için geçerlidir.


##Power BI hizmetine erişmek için Azure Güvenlik Merkezi panosunu kullanma
Ayrıca, Power BI raporlarına erişmek için Azure Güvenlik Merkezi panosunu da kullanabilirsiniz. Bu görevi gerçekleştirmek için şu adımları izleyin: 

1. **Azure Güvenlik Merkezi** panosunda **Power BI'da Araştırma** düğmesine basın.

    ![Power BI'ı kullanarak Azure Güvenlik Merkezi'ne bağlanma](./media/security-center-powerbi/security-center-powerbi-fig9-new.png) 

2. Aşağıda gösterildiği gibi sağ tarafta **Power BI'da Araştırma** dikey penceresi açılır:

    ![Power BI'ı kullanarak Azure Güvenlik Merkezi'ne bağlanma](./media/security-center-powerbi/security-center-powerbi-fig2-new.png)

3. Power BI panosunu ilk kez oluşturuyorsanız Power BI'da Araştırma dikey penceresindeki aşağıdaki seçeneklerden birini belirleyebilirsiniz: 

    - **Güvenlik öngörüleri panosu**: Güvenlik durumunu, tehditleri ve algılamaları içeren bir pano oluşturmak istiyorsanız bu seçeneği belirleyin. Bu, aboneliklerde algılanan uyarılar ve koruma durumlarını çözümlemeye yönelik olarak sorumlu DevOps rolü için daha yaygın kullanılan bir seçenektir.
    - **İlke yönetimi panosu**: Yönetim ve zorlama ilkesini araştırmak istiyorsanız bu seçeneği belirleyin.  Bu, daha çok idare üzerine odaklanan Merkezi BT için yaygın kullanılan bir seçenektir. Bu panoyu, kuruluşlarındaki güvenlik ilkesi uygunluğuna ilişkin görünürlük ve öngörü kazanmak için kullanabilirler.
    - Zaten Power BI panonuz varsa **Geçerli Power BI panosuna git**'e tıklayın.

4. Bu örneğin amacı doğrultusunda **Güvenlik öngörüleri panosu**'na tıkladığınızda şu pencere görünür:

    ![Azure Güvenlik Merkezi Güvenlik Öngörüleri panosu](./media/security-center-powerbi/security-center-powerbi-fig3-new.png)

5. **Kimlik Doğrulama Yöntemi** seçeneğinin **oAuth2** olduğuna emin olun ve **Oturum Aç**'a tıklayın.
6. **Power BI** penceresi açılır ve aşağıda gösterilenle benzer yapıya sahip bir rapor görüntülenir:
    
    ![Güvenlik Öngörüleri panosu](./media/security-center-powerbi/security-center-powerbi-fig5.png)

> [AZURE.NOTE] Raporun yenilenmesi günlük olarak gerçekleşecek şekilde zamanlanır. Bu yenilemede bir arızayla karşılaşırsanız sorun giderme hakkında daha fazla bilgi almak için bkz. [Azure Güvenlik Merkezi Power BI ile Olası Yenileme Sorunları](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/).

Burada, güvenlik uyarısı ve öneri sayısının yanı sıra Azure Güvenlik Merkezi tarafından izlenen VM, Azure SQL veritabanı ve ağ kaynağı sayısını da görebilirsiniz.

Azure Güvenlik Merkezi'ne bağlantı, sizi Azure portalına yönlendirir. Grafikler; güvenlik önerileri ve uyarılar hakkındaki bilgileri görselleştirmeyi şunlar dahil olmak üzere kolaylaştırır:

- Kaynak Güvenlik Durumu
- Bekleyen Genel Öneriler
- VM Önerileri
- Süreçteki Uyarılar
- Saldırıya Uğrayan Kaynaklar
- Saldırıya Uğrayan IP'ler

Her grafiğin arkasında ek öngörüler vardır. Daha fazla bilgi için bir kutucuk seçin. Örneğin, Kaynak Güvenlik Durumu kutucuğu, aşağıda gösterildiği gibi kaynaklara göre bekleyen öneriler hakkında ek ayrıntılar gösterir:

![Öneriler](./media/security-center-powerbi/security-center-powerbi-fig6.png)

Bu grafikteki bir satıra tıklarsanız diğer satırlar gri renge dönüşür ve yalnızca seçtiğiniz satıra odaklanırsınız. Panoya dönmek için bu sayfanın sol bölmesindeki **Panolar** seçeneğinin altındaki **Azure Güvenlik Merkezi**'ne tıklayın.

> [AZURE.NOTE] İlave alanlar ekleyerek veya var olan görselleri değiştirerek raporlarınızı özelleştirmek istiyorsanız raporu düzenleyebilirsiniz. Daha fazla bilgi için [Power BI'da Görünüm Düzenleme seçeneğindeki bir rapor ile etkileşimde bulunma](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) bölümünü okuyun.

**Süreçteki Uyarı, Saldırıya Uğrayan Kaynaklar** ve **Saldırgan IP'ler** kutucuklarının her biri, üzerine tıkladığınızda benzer bir çıktıya sahiptir. Rapor, bu üç değişkenin hepsiyle ilgili bilgi topladığı için bu durum oluşur ve aşağıda gösterildiği gibi **Saldırıya Uğrayan Kaynaklar** olarak adlandırır:

![Saldırıya uğrayan kaynaklar](./media/security-center-powerbi/security-center-powerbi-fig7.png)

Bu noktada, bu raporun bir kopyasını kaydedebilirsiniz, yazdırabilirsiniz veya **Dosya** menüsünde kullanılabilir seçenekleri belirterek web'de yayımlayabilirsiniz.

![Dosya menüsü](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## Power BI hizmetleri ile Azure Güvenlik Merkezi verilerinizi araştırma

Power BI'daki [Power BI İçerik Paketi Hizmetleri](https://msit.powerbi.com/groups/me/getdata/services)'ne bağlanın ve şu adımları izleyin:

1. **Power BI için İçerik Paketi** penceresinde, aşağıda gösterildiği gibi iki seçenek görürsünüz.

    ![Power BI için içerik paketi](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

2. Bu örneğin amacı doğrultusunda **Azure Güvenlik Merkezi İlke Yönetimi** kutucuğundaki **Al** seçeneğine tıklayın.

3. **Azure Güvenlik Merkezi İlke Yönetimi'ne Bağlanma** penceresinde, aşağıda gösterildiği gibi **Kimlik Doğrulama Yöntemi** açılan menüsünün altında **oAuth2** seçeneğini belirttiğinizden emin olun ve **Oturum Aç** düğmesine tıklayın.

    ![İlke Yönetimi penceresi](./media/security-center-powerbi/security-center-powerbi-fig4-new.png)

4. Azure Güvenlik Merkezi'ne bağlanmak için kullandığınız kimlik bilgilerini yazmanız gereken kimlik doğrulama sayfasına yönlendirileceksiniz. Kimlik doğrulama işlemi tamamlandıktan sonra Power BI, raporlarınızı oluşturmak için verileri içeri aktarmaya başlar. Bu süre boyunca tarayıcınızın sağ üst köşesinde şu iletiyi görebilirsiniz:

    ![Power BI'ı kullanarak Azure Güvenlik Merkezi'ne bağlanma](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Özellikle birden fazla aboneliğine sahip olduğunuz senaryolar için panoyu ilk kez oluşturmak normalden daha uzun sürebilir. 

5. İşlem tamamlandıktan sonra Azure Güvenlik Merkezi Power BI panonuz, **İlke Yönetimi** raporuyla birlikte yüklenir.


## Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi'nde Power BI hizmetini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md) - Azure Güvenlik Merkezi'ni benimsemeyi planlama hakkında bilgi edinin.
- [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) - Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırma hakkında bilgi edinin
- [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanma hakkında sık sorulan soruları bulun
- [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun



<!----HONumber=Jun16_HO2-->


