---
title: "Microsoft Power BI Embedded kullanmaya başlama"
description: "Power BI Embedded, iş zekası uygulamanıza etkileşimli Power BI raporları ekler"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 4afa8d2c7f8ec1942521ba5fa131967dfd581c91
ms.lasthandoff: 03/14/2017

---
# <a name="get-started-with-microsoft-power-bi-embedded"></a>Microsoft Power BI Embedded kullanmaya başlama

**Power BI Embedded**, uygulama geliştiricilerin kendi uygulamalarına etkileşimli Power BI raporları eklemelerini sağlayan bir Azure hizmetidir. **Power BI Embedded** kullanıcıların oturum açma yöntemini yeniden tasarlamaya veya değiştirmeye gerek kalmadan var olan uygulamalarla birlikte çalışır.

**Microsoft Power BI Embedded** kaynakları [Azure ARM API’leri](https://msdn.microsoft.com/library/mt712306.aspx) aracılığıyla sağlanır. Bu örnekte, sağladığınız kaynak **Power BI Çalışma Alanı Koleksiyonu**’dur.

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a>Çalışma alanı koleksiyonu oluşturma

**Çalışma Alanı Koleksiyonu**, uygulamanıza eklenecek içerik için üst düzey Azure kaynağı ve kapsayıcıdır. **Çalışma Alanı Koleksiyonu** iki şekilde oluşturulabilir:

* Azure Portal’ı el ile kullanarak
* Azure Resource Manager(ARM) API’lerini program aracılığıyla kullanarak

Şimdi Azure Portal kullanarak **Çalışma Alanı Koleksiyonu** oluşturma adımlarını inceleyelim.

1. **Azure Portal**’ı açın ve oturum açın: [http://portal.azure.com](http://portal.azure.com)
2. Üst panelde **+ Yeni**’ye tıklayın.
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. **Veri + Analiz** altında **Power BI Embedded**’a tıklayın.
4. **Çalışma Alanı Koleksiyonu Dikey Penceresi**’ne gerekli bilgileri girin. **Fiyatlandırma** için bkz. [Power BI Embedded fiyatlandırması](http://go.microsoft.com/fwlink/?LinkID=760527).
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. **Oluştur**’a tıklayın.

**Çalışma Alanı Koleksiyonu**’nun sağlanması birkaç dakika sürer. Tamamlandığında **Çalışma Alanı Koleksiyonu Dikey Penceresi**’ne götürülürsünüz.

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

**Oluşturma Dikey Penceresi** çalışma alanlarını oluşturan ve bunlara içerik dağıtan API’leri çağırmak için size gereken bilgileri içerir.

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a>Power BI API’si erişim anahtarlarını görüntüleme

Power BI API’lerini çağırmanın en önemli parçalarından biri **Erişim Anahtarları**’dır. Bunlar API isteklerinizin kimlik doğrulaması için kullanılan **uygulama belirteçlerini** oluşturmak için kullanılır. **Erişim Anahtarları**’nızı görüntülemek için **Ayarlar Dikey Penceresi**’nde **Erişim Anahtarları**’na tıklayın. **Uygulama belirteçleri** hakkında daha fazla bilgi için bkz. [Power BI Embedded ile kimlik doğrulama ve yetkilendirme](power-bi-embedded-app-token-flow.md).

   ![](media/power-bi-embedded-get-started/access-keys.png)

İki anahtarınız olduğunu fark edeceksiniz.

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

Bu anahtarları kopyalayın ve uygulamanızda güvenli bir şekilde depolayın. **Çalışma Alanı Koleksiyonu**’nuzdaki tüm içeriği erişim imkanı sağlayacağından bu anahtarları bir parolaymış gibi ele almanız çok önemlidir.

İki anahtar listelenmekle birlikte, belirli bir seferde yalnızca bir anahtar gerekir. İkinci anahtar, hizmete erişimi kesintiye uğratmadan anahtarları dönemsel olarak yeniden oluşturabilmeniz için verilir.

Artık uygulamanız için bir Power BI örneğine ve **Erişim Anahtarları**’na sahip olduğunuza göre, kendi uygulamanıza bir rapor aktarabilirsiniz. Bir raporu içeri aktarmayı öğrenmeden önce, sonraki bölümde bir uygulamaya eklemek için Power BI veri kümeleri ve raporları oluşturma açıklanır.

## <a name="working-with-workspaces"></a>Çalışma alanları ile çalışma

Çalışma alanı koleksiyonunuzu oluşturduktan sonra, raporlarınızı ve veri kümelerinizi barındıracak bir çalışma alanı oluşturmanız gerekir. Bir çalışma alanı oluşturmak için [Post Worksapce REST API’sini](https://msdn.microsoft.com/library/azure/mt711503.aspx) kullanmanız gerekir.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app-using-power-bi-desktop"></a>Power BI Desktop kullanarak bir uygulamaya eklemek için Power BI veri kümeleri ve raporları oluşturma

Artık, uygulamanız için Power BI örneğini oluşturduğunuza ve**Erişim Anahtarları**’na sahip olduğunuza göre, eklemek istediğiniz Power BI veri kümelerini ve raporlarını oluşturmanız gerekir. Veri kümeleri ve raporlar **Power BI Desktop** kullanarak oluşturulabilir. [Power BI Desktop’u ücretsiz olarak](https://go.microsoft.com/fwlink/?LinkId=521662) indirebilirsiniz. Hızlı bir şekilde kullanmaya başlamak için [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547)’i de indirebilirsiniz.

> [!NOTE]
> **Power BI Desktop** kullanma hakkında daha fazla bilgi için bkz. [Power BI Desktop kullanmaya başlama](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

**Power BI Desktop** ile, verilerin bir kopyasını **Power BI Desktop**’a aktararak ya da **DirectQuery** kullanarak doğrudan veri kaynağına bağlayarak veri kaynağınıza bağlanırsınız.

**İçeri Aktarma** ve **DirectQuery**.arasındaki farklar burada yer almaktadır.

| İçeri Aktarma | DirectQuery |
| --- | --- |
| Tablolar, sütunlar *ve veriler* **Power BI Desktop**’a aktarılır veya kopyalanır. Görselleştirmelerle çalışırken, **Power BI Desktop** verilerin bir kopyasını sorgular. Temel alınan verilerde meydana gelen değişiklikleri görmek için, tam, güncel veri kümesini tekrar yenilemeli ya da içeri aktarmalısınız. |Yalnızca *tablolar ve sütunlar* ve **Power BI Desktop**’a aktarılır veya kopyalanır. Görselleştirmelerle çalışırken, **Power BI Desktop** temel alınan veri kaynağını sorgular, bu da her zaman güncel verileri görüntülediğiniz anlamına gelir. |

Veri kaynağına bağlanma hakkında daha fazla bilgi için bkz. [Veri kaynağına bağlanma](power-bi-embedded-connect-datasource.md)

Çalışmanızı **Power BI Desktop**’a kaydettikten sonra, PBIX dosyası oluşturulur. Bu dosya raporunuzu içerir. Ayrıca, verileri içeri aktarırsanız PBIX tüm veri kümesini içerir ya da **DirectQuery** kullanırsanız PBIX yalnızca veri kümesi şemasını içerir. [Power BI İçeri Aktarma API’si](https://msdn.microsoft.com/library/mt711504.aspx) kullanarak PBIX’i program kullanarak çalışma alanınıza dağıtırsınız.

> [!NOTE]
> **Power BI Embedded**, veri kümenizin işaret ettiği sunucuyu ve veri tabanını değiştirmek ve veri kümesinin veri tabanınıza bağlanmak için kullanacağı bir hizmet hesabı kimlik bilgisi ayarlamak üzere ek API’lere sahiptir. Bkz. e [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) ve [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="create-power-bi-datasets-and-reports-using-apis"></a>API kullanarak Power BI veri kümeleri ve raporları oluşturma

### <a name="datsets"></a>Veri kümeleri

REST API kullanarak Power BI Embedded içinde veri kümeleri oluşturabilirsiniz. Daha sonra verileri veri kümenize gönderebilirsiniz. Bu, Power BI Desktop olmadan verilerinizle çalışmanıza olanak sağlar. Daha fazla bilgi için bkz. [Veri Kümeleri Gönderme](https://msdn.microsoft.com/library/azure/mt778875.aspx).

### <a name="reports"></a>Reports

JavaScript API kullanarak doğrudan uygulamanızda bir veri kümesinden rapor oluşturabilirsiniz. Daha fazla bilgi için bkz. [Power BI Embedded’de bir veri kümesinden yeni bir rapor oluşturma](power-bi-embedded-create-report-from-dataset.md).

## <a name="see-also"></a>Ayrıca Bkz.

[Bir örnek ile kullanmaya başlama](power-bi-embedded-get-started-sample.md)  
[Power BI Embedded ile kimlik doğrulaması ve yetkilendirme](power-bi-embedded-app-token-flow.md)  
[Rapor ekleme](power-bi-embedded-embed-report.md)  
[Power BI Embedded’de bir veri kümesinden yeni bir rapor oluşturma](power-bi-embedded-create-report-from-dataset.md)
[Raporları kaydetme](power-bi-embedded-save-reports.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)


