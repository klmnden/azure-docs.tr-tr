<properties
   pageTitle="Microsoft Power BI Embedded kullanmaya başlama"
   description="Power BI Embedded, iş zekası uygulamanıza etkileşimli Power BI raporları ekler"
   services="power-bi-embedded"
   documentationCenter=""
   authors="minewiskan"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="07/05/2016"
   ms.author="owend"/>

# Microsoft Power BI Embedded kullanmaya başlama

**Power BI Embedded**, uygulama geliştiricilerin kendi uygulamalarına etkileşimli Power BI raporları eklemelerini sağlayan bir Azure hizmetidir. **Power BI Embedded** kullanıcıların oturum açma yöntemini yeniden tasarlamaya veya değiştirmeye gerek kalmadan var olan uygulamalarla birlikte çalışır.

**Microsoft Power BI Embedded** kaynakları [Azure ARM API’leri](https://msdn.microsoft.com/library/mt712306.aspx) aracılığıyla sağlanır. Bu örnekte, sağladığınız kaynak **Power BI Çalışma Alanı Koleksiyonu**’dur.

![](media\power-bi-embedded-get-started\introduction.png)

## Çalışma alanı koleksiyonu oluşturma
**Çalışma Alanı Koleksiyonu**, uygulamanıza eklenecek içerik için üst düzey Azure kaynağı ve kapsayıcıdır. **Çalışma Alanı Koleksiyonu** iki şekilde oluşturulabilir:

   -    Azure Portal’ı el ile kullanarak
   -    Azure Resource Manager(ARM) API’lerini program aracılığıyla kullanarak

Şimdi Azure Portal kullanarak **Çalışma Alanı Koleksiyonu** oluşturma adımlarını inceleyelim.

   1.   **Azure Portal**’ı açın ve oturum açın: [http://portal.azure.com](http://portal.azure.com)

   2.   Üst panelde **+ Yeni**’ye tıklayın.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   **Veri + Analiz** altında **Power BI Embedded**’a tıklayın.
   4.   **Oluşturma Dikey Penceresi**’ne gerekli bilgileri girin. **Fiyatlandırma** için bkz. [Power BI Embedded fiyatlandırması](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. **Oluştur**’a tıklayın.

**Çalışma Alanı Koleksiyonu**’nun sağlanması birkaç dakika sürer. Tamamlandığında **Çalışma Alanı Koleksiyonu Dikey Penceresi**’ne götürülürsünüz.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

**Oluşturma Dikey Penceresi** çalışma alanlarını oluşturan ve bunlara içerik dağıtan API’leri çağırmak için size gereken bilgileri içerir.

<a name="view-access-keys"/>
## Power BI API’si Erişim Anahtarlarınız

Power BI API’lerini çağırmanın en önemli parçalarından biri **Erişim Anahtarları**’dır. Bunlar API isteklerinizin kimlik doğrulaması için kullanılan **uygulama belirteçlerini** oluşturmak için kullanılır. **Erişim Anahtarları**’nızı görüntülemek için **Ayarlar Dikey Penceresi**’nde **Erişim Anahtarları**’na tıklayın. **Uygulama belirteçleri** hakkında daha fazla bilgi için bkz. [Power BI Embedded ile kimlik doğrulama ve yetkilendirme](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

İki anahtarınız olduğunu fark edeceksiniz.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Bu anahtarları kopyalayın ve uygulamanızda güvenli bir şekilde depolayın. **Çalışma Alanı Koleksiyonu**’nuzdaki tüm içeriği erişim imkanı sağlayacağından bu anahtarları bir parolaymış gibi ele almanız çok önemlidir.

İki anahtar listelenmekle birlikte, belirli bir seferde yalnızca bir anahtar gerekir. İkinci anahtar, hizmete erişimi kesintiye uğratmadan anahtarları dönemsel olarak yeniden oluşturabilmeniz için verilir.

Artık uygulamanız için bir Power BI örneğine ve **Erişim Anahtarları**’na sahip olduğunuza göre, kendi uygulamanıza bir rapor aktarabilirsiniz. Bir raporu içeri aktarmayı öğrenmeden önce, sonraki bölümde bir uygulamaya eklemek için Power BI veri kümeleri ve raporları oluşturma açıklanır.

## Bir uygulamaya eklemek için Power BI veri kümeleri ve raporları oluşturma

Artık, uygulamanız için Power BI örneğini oluşturduğunuza ve**Erişim Anahtarları**’na sahip olduğunuza göre, eklemek istediğiniz Power BI veri kümelerini ve raporlarını oluşturmanız gerekir. Veri kümeleri ve raporlar **Power BI Desktop** kullanarak oluşturulabilir. [Power BI Desktop’u ücretsiz olarak](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/) indirebilirsiniz. Veya, hızlı bir şekilde kullanmaya başlamak için [Retail Analysis Sample PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547) indirebilirsiniz. **Power BI Desktop** kullanma hakkında daha fazla bilgi için bkz. [Power BI Desktop kullanmaya başlama](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

**Power BI Desktop** ile, verilerin bir kopyasını **Power BI Desktop**’a aktararak ya da **DirectQuery** kullanarak doğrudan veri kaynağına bağlayarak veri kaynağınıza bağlanırsınız.

**İçeri Aktarma** ve **DirectQuery**.arasındaki farklar burada yer almaktadır.

|İçeri Aktarma | DirectQuery
|---|---
|Tablolar, sütunlar *ve veriler* **Power BI Desktop**’a aktarılır veya kopyalanır. Görselleştirmelerle çalışırken, **Power BI Desktop** verilerin bir kopyasını sorgular. Temel alınan verilerde meydana gelen değişiklikleri görmek için, tam, güncel veri kümesini tekrar yenilemeli ya da içeri aktarmalısınız.|Yalnızca *tablolar ve sütunlar* ve **Power BI Desktop**’a aktarılır veya kopyalanır. Görselleştirmelerle çalışırken, **Power BI Desktop** temel alınan veri kaynağını sorgular, bu da her zaman güncel verileri görüntülediğiniz anlamına gelir.

Veri kaynağına bağlanma hakkında daha fazla bilgi için bkz. [Veri kaynağına bağlanma](power-bi-embedded-connect-datasource.md)

Çalışmanızı **Power BI Desktop**’a kaydettikten sonra, PBIX dosyası oluşturulur. Bu dosya raporunuzu içerir. Ayrıca, verileri içeri aktarırsanız PBIX tüm veri kümesini içerir ya da **DirectQuery** kullanırsanız PBIX yalnızca veri kümesi şemasını içerir. [Power BI İçeri Aktarma API’si](https://msdn.microsoft.com/library/mt711504.aspx) kullanarak PBIX’i program kullanarak çalışma alanınıza dağıtırsınız.

> [AZURE.NOTE] **Power BI Embedded**, veri kümenizin işaret ettiği sunucuyu ve veri tabanını değiştirmek ve veri kümesinin veri tabanınıza bağlanmak için kullanacağı bir hizmet hesabı kimlik bilgisi ayarlamak üzere ek API’lere sahiptir. Bkz. e [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) ve [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## Sonraki Adımlar
Önceki adımlarda, çalışma alanı koleksiyonu ve ilk raporunuzu ve veri kümenizi oluşturdunuz. Şimdi **Power BI Embedded** için kod yazmayı öğrenme zamanı. Başlamanıza yardımcı olmak için, örnek web uygulaması oluşturduk: [Örneği kullanmaya başlama](power-bi-embedded-get-started-sample.md). Örnek size şunların nasıl yapılacağını gösterir:

  - İçerik sağlama
      - Çalışma Alanı oluşturma
      - PBIX dosyasını içeri aktarma
      - Veri kümeleriniz için bağlantı dizelerini güncelleştirin ve kimlik bilgilerini ayarlayın.

  - Güvenli bir şekilde rapor ekleme

## Ayrıca Bkz.
- [Örnek kullanmaya başlama](power-bi-embedded-get-started-sample.md)
- [Power BI Embedded ile kimlik doğrulama ve yetkilendirme](power-bi-embedded-app-token-flow.md)
- [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)



<!--HONumber=Aug16_HO1-->


