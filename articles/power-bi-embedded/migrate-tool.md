---
title: Power BI Embedded Geçiş Aracı'nı | Microsoft Docs
description: Power BI Embedded geçiş aracı, raporlarınızı Power BI çalışma koleksiyonlarından Power BI Embedded kopyalamak için kullanılabilir.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: maghan
ms.openlocfilehash: 4f76b1efb509745653bfde0926f56032030f7d47
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-the-power-bi-embedded-migration-tool"></a>Power BI Embedded geçiş aracını kullanma

Power BI Embedded geçiş aracı, raporlarınızı Power BI çalışma koleksiyonlarından Power BI Embedded kopyalamak için kullanılabilir.

İçeriğinizi Power BI çalışma koleksiyonlarından geçiş hizmeti, geçerli çözümünüz için paralel yapılabilir ve kapalı kalma süresi gerektirmez.

## <a name="limitations"></a>Sınırlamalar

* Zorlanan veri kümeleri karşıdan yüklenemiyor ve Power BI hizmeti için Power BI REST API'lerini kullanarak yeniden oluşturulması gerekir.
* 26 Kasım 2016 indirilebilir olmaz önce içeri PBIX dosyaları.

## <a name="download"></a>İndirme

Geçiş Aracı örnekten indirebilirsiniz [GitHub](https://github.com/Microsoft/powerbi-migration-sample). Depo bir zip ya da yükleyebilir veya yerel olarak kopyalayabilirsiniz. Yüklendikten sonra açabilirsiniz *powerbı geçiş sample.sln* derlemek ve geçiş aracı çalıştırmak için Visual Studio içinde.

## <a name="migration-plans"></a>Geçiş planları

Geçiş planınız, Power BI çalışma koleksiyonları ve bunları Power BI Embedded yayımlamak istediğiniz nasıl içindeki içeriğin kataloglar meta verilerdir.

### <a name="start-with-a-new-migration-plan"></a>Yeni bir geçiş planı ile Başlat

Bir geçiş planı kullanılabilir koleksiyonlarda Power BI çalışma sonra Power BI Embedded taşımak istediğiniz öğelerin meta verilerdir. Geçiş planı XML dosyası olarak depolanır.

Yeni bir geçiş planı oluşturarak başlayın isteyeceksiniz. Yeni bir geçiş planı oluşturmak için aşağıdakileri yapın.

1. Seçin **dosya** > **yeni geçiş planı**.

    ![Yeni geçiş planı](media/migrate-tool/migrate-tool-plan.png)

2. İçinde **Power BI çalışma koleksiyonları kaynak grubu seçin** iletişim kutusunda, seçmek istediğiniz **ortam** üretim açılır ve seçin.

3. Oturum açmak için istenir. Azure aboneliği oturumunuzla kullanır.

    > [!IMPORTANT]
    > Bu **değil** Power BI ile oturum Office 365 kuruluş hesabınızı.

4. Power BI çalışma koleksiyonları kaynağınız depolayan Azure aboneliğini seçin.

    ![Azure aboneliğinizi seçin](media/migrate-tool/migrate-tool-select-resource-group.png)

5. Abonelik listesi seçin **kaynak grubu** seçin ve çalışma koleksiyonları içeren **seçin**.

    ![Kaynak grubu seçin](media/migrate-tool/migrate-tool-select-resource-group2.png)

6. Seçin **analiz**. Bu, planınız başlamak, Azure aboneliğinizin öğeleri envanterini alırsınız.

    ![Çözümle düğmesini seçin](media/migrate-tool/migrate-tool-analyze-group.png)

    > [!NOTE]
    > Çözümle işlem çalışma koleksiyonlar ve ne kadar içerik çalışma koleksiyonda mevcut sayısına bağlı olarak birkaç dakika sürebilir.

7. Zaman **Çözümle** olan tam, bu geçiş planınız kaydetmenizi ister.

Bu noktada, Azure aboneliğinize geçiş planınız bağlandınız. Geçiş planınız ile çalışmaya nasıl akışını anlamak için okuyun aşağıdaki. Bu çözümle & geçiş planlama, yükleme, grupları oluşturma ve karşıya yükleme içerir.

### <a name="save-your-migration-plan"></a>Geçiş planınız Kaydet

Geçiş planınız daha sonra kullanmak için kaydedebilirsiniz. Bu geçiş planınızdaki tüm bilgileri içeren bir XML dosyası oluşturur.

Geçiş planınız kaydetmek için aşağıdakileri yapın.

1. Seçin **dosya** > **Geçiş Planı Kaydet**.

    ![Geçiş planı menü seçeneği Kaydet](media/migrate-tool/migrate-tool-save-plan.png)

2. Bir ad verin veya oluşturulan dosya adı kullanın ve seçin **kaydetmek**.

### <a name="open-an-existing-migration-plan"></a>Var olan bir geçiş planı açın

Geçişinizi üzerinde çalışmaya devam etmek için kaydedilmiş geçiş planı açabilirsiniz.

Varolan geçiş planınız açmak için aşağıdakileri yapın.

1. Seçin **dosya** > **açmak varolan geçiş planı**.

    ![Açık varolan geçiş planı menü seçeneği](media/migrate-tool/migrate-tool-open-plan.png)

2. Geçiş dosyanızın seçip **açık**.

## <a name="step-1-analyze-and-plan-migration"></a>1. adım: Çözümlemek ve geçiş planı

**Çözümle & planlama geçiş** sekmesi, şu anda Azure aboneliğinizin kaynak grubunda nedir bir görünümünü verir.

![Analiz ve planlama geçiş sekmesi](media/migrate-tool/migrate-tool-step1.png)

Ele alacağız *SampleResourceGroup* bir örnek olarak.

### <a name="paas-topology"></a>PaaS topolojisi

Bu bir listesi bulunmaktadır, *kaynak grubu > çalışma alanı koleksiyonları > çalışma alanları*. Kaynak grubu ve çalışma alanı koleksiyonları kolay adı gösterir. Çalışma alanları bir GUID gösterir.

Listedeki öğeler de bir renk ve bir sayı (#/ #) biçiminde görüntüler. Bu indirilebilir raporları sayısını gösterir.

Siyah bir renk tüm raporları indirilebilir anlamına gelir. Kırmızı renk, bazı raporlar indirilemiyor anlamına gelir. Soldaki sayı indirilebilir raporları toplam sayısını gösterir. Sağdaki sayı raporları gruplandırma içindeki toplam sayısını gösterir.

Raporlar bölümünde raporları görüntülemek için PaaS topoloji içinde öğe seçebilirsiniz.

### <a name="reports"></a>Reports

Raporları bölümüne raporların kullanılabilir listeler ve onu veya indirilip indirilmeyeceğini belirtir.

![Power BI çalışma alanı koleksiyonu içinde rapor listesi](media/migrate-tool/migrate-tool-analyze-reports.png)

### <a name="target-structure"></a>Hedef yapısı

**Hedef yapısı** aracı söyleyin nerede olduğunu şeyler için burada indirilir ve bunları karşıya yüklemek nasıl.

#### <a name="download-plan"></a>Plan indirin

Bir yol sizin için otomatik olarak oluşturulur. İsterseniz, bu yol değiştirebilirsiniz. Yolunu değiştirirseniz seçmeniz gerekir **güncelleştirme yolları**.

**Bu gerçekten indirme işlemini gerçekleştirir.** Bu raporlar için burada indirilecek yapısı yalnızca belirtilmesidir.

#### <a name="upload-plan"></a>Plan karşıya yükle

Burada Power BI hizmet içinde oluşturulur ve uygulama çalışma alanları için kullanılacak bir ön eki belirtebilirsiniz. Sonra önek Azure'da varolan çalışma için GUID olacaktır.

![Grup adı öneki belirtin](media/migrate-tool/migrate-tool-upload-plan.png)

**Bu Power BI hizmetinde içindeki grupları gerçekte oluşturmaz.** Bu, yalnızca gruplar için adlandırma yapısını tanımlar.

Önek değiştirirseniz, seçmeniz gerekir **oluşturmak, karşıya yükleme planlama**.

Bir gruba sağ tıklayın ve isterseniz, karşıya yükleme planı içinde grubunu doğrudan, yeniden adlandırmak seçin.

![Grup bağlam menüsü seçeneğini yeniden adlandırma](media/migrate-tool/migrate-tool-upload-report-rename-item.png)

> [!NOTE]
> Adını *grup* boşluk veya geçersiz karakterler içermemelidir.

## <a name="step-2-download"></a>Adım 2: karşıdan yükle

Üzerinde **karşıdan** sekmesinde, raporları ve ilişkili metadata listesini görürsünüz. Önceki verme durumunun yanı sıra verme durumu nedir görebilirsiniz.

![Sekme indirin](media/migrate-tool/migrate-tool-download-tab.png)

İki seçeneğiniz vardır.

* Belirli raporları seçip **karşıdan seçili**
* Seçin **tüm indirin**.

![Seçilen düğmesini indirin](media/migrate-tool/migrate-tool-download-options.png)

Başarılı bir yükleme olarak durumunu görürsünüz *Bitti* ve PBIX dosyasının varolduğunu yansıtır.

Yükleme tamamlandıktan sonra Seç **oluşturduğunuz grupların** sekmesi.

## <a name="step-3-create-groups"></a>3. adım: Grup oluşturma

Kullanılabilir raporlar indirdikten sonra gidebilirsiniz **oluşturduğunuz grupların** sekmesi. Bu sekme, oluşturduğunuz geçiş plana göre Power BI hizmetinde uygulama çalışma alanları oluşturur. Uygulama çalışma alanı, sağladığınız ada sahip oluşturacağı **karşıya** içinde sekmesinde **Çözümle & planlama geçiş**.

![Grupları sekmesi oluştur](media/migrate-tool/migrate-tool-create-groups.png)

Uygulama çalışma alanları oluşturmak için her ikisini seçebilirsiniz **oluşturduğunuz seçili grupların** veya **tüm eksik grupları oluşturma**.

Bu seçeneklerden birini seçtiğinizde, oturum açmak için istenir. *Uygulama çalışma alanları oluşturmak için istediğiniz Power BI hizmeti için kimlik bilgilerinizi kullanmak istersiniz.*

![Power BI'da oturum açma ekranı](media/migrate-tool/migrate-tool-create-group-sign-in.png)

Bu Power BI hizmetinde uygulama çalışma alanı oluşturur. Bu rapor uygulama çalışma alanına karşıya.

Uygulama çalışma Power BI'a imzalama ve çalışma alanı var olduğunu doğrulama oluşturulduğunu doğrulayabilirsiniz. Hiçbir şey çalışma alanında olduğunu fark edeceksiniz.

![Power BI hizmetinde uygulama çalışma](media/migrate-tool/migrate-tool-app-workspace.png)

Çalışma alanı oluşturulduktan sonra üzerine taşıyabilirsiniz **karşıya** sekmesi.

## <a name="step-4-upload"></a>4. adım: karşıya yükle

Üzerinde **karşıya** sekmesinde, bu raporları Power BI hizmetine yükleyeceksiniz. Geçiş planınız temel hedef grup adı ile birlikte indirme sekmesinde biz indirilen raporların listesini görürsünüz.

![Sekme karşıya yükle](media/migrate-tool/migrate-tool-upload-tab.png)

Seçili raporları karşıya yükleyebilir veya tüm raporları karşıya yüklenemedi. Öğeleri yeniden karşıya yüklemek için yükleme durumunu da sıfırlayabilirsiniz.

Ayrıca, aynı ada sahip bir rapor varsa yapmanız gerekenler seçme seçeneğiniz de vardır. Arasından seçim yapabilirsiniz **Abort**, **Yoksay** ve **üzerine yaz**.

![Seçenek açılır için rapor varsa yapmanız gerekenler](media/migrate-tool/migrate-tool-upload-report-same-name.png)

![Seçili sonuçlarını karşıya yükle](media/migrate-tool/migrate-tool-upload-selected.png)

### <a name="duplicate-report-names"></a>Yinelenen Rapor adları

Aynı ada sahip bir rapor var ancak farklı bir rapor olduğunu bildiğiniz değiştirmeniz gerekecektir **TargetName** raporun. El ile geçiş planı XML düzenleyerek adını değiştirebilirsiniz.

Değişiklik ve aracı ve geçiş planı yeniden açmak için geçiş aracı kapatmanız gerekir.

Yukarıdaki örnekte, kopyalanan raporlardan birini aynı ada sahip bir rapor vardı belirten başarısız oldu. Geçiş planı XML Ara gidin, biz görürsünüz.

```
<ReportMigrationData>
    <PaaSWorkspaceCollectionName>SampleWorkspaceCollection</PaaSWorkspaceCollectionName>
    <PaaSWorkspaceId>4c04147b-d8fc-478b-8dcb-bcf687149823</PaaSWorkspaceId>
    <PaaSReportId>525a8328-b8cc-4f0d-b2cb-c3a9b4ba2efe</PaaSReportId>
    <PaaSReportLastImportTime>1/3/2017 2:10:19 PM</PaaSReportLastImportTime>
    <PaaSReportName>cloned</PaaSReportName>
    <IsPushDataset>false</IsPushDataset>
    <IsBoundToOldDataset>false</IsBoundToOldDataset>
    <PbixPath>C:\MigrationData\SampleResourceGroup\SampleWorkspaceCollection\4c04147b-d8fc-478b-8dcb-bcf687149823\cloned-525a8328-b8cc-4f0d-b2cb-c3a9b4ba2efe.pbix</PbixPath>
    <ExportState>Done</ExportState>
    <LastExportStatus>OK</LastExportStatus>
    <SaaSTargetGroupName>SampleMigrate</SaaSTargetGroupName>
    <SaaSTargetGroupId>6da6f072-0135-4e6c-bc92-0886d8aeb79d</SaaSTargetGroupId>
    <SaaSTargetReportName>cloned</SaaSTargetReportName>
    <SaaSImportState>Failed</SaaSImportState>
    <SaaSImportError>Report with the same name already exists</SaaSImportError>
</ReportMigrationData>
```

Başarısız öğe için biz SaaSTargetReportName adını değiştirebilirsiniz.

```
<SaaSTargetReportName>cloned2</SaaSTargetReportName>
```

Biz daha sonra planında Geçiş Aracı yeniden açın ve başarısız rapor karşıya yükleyin.

Power BI'a geri dönerseniz, raporları ve veri kümeleri uygulama çalışma alanında yüklenmiş görebiliriz.

![Power BI hizmetinde içinde rapor listesi](media/migrate-tool/migrate-tool-upload-app-workspace.png)

<a name="upload-local-file"></a>
### <a name="upload-a-local-pbix-file"></a>Yerel PBIX dosyasını karşıya yükle

Power BI Desktop dosyasının yerel bir sürümünü yükleyebilirsiniz. Aracı'nı kapatmak, XML düzenleme ve tam yolu, yerel PBIX put gerekecek **PbixPath** özelliği.

```
<PbixPath>[Full Path to PBIX file]</PbixPath>
```

Xml düzenledikten sonra geçiş aracı içinde planı yeniden açın ve raporu karşıya yükleyin.

<a name="directquery-reports"></a>
### <a name="directquery-reports"></a>DirectQuery raporları

DirectQuery raporlar için bağlantı dizesini güncelleştirmek için güncelleştirmeniz gerekir. Bu, içinde yapılabilir *powerbi.com*, veya Power BI Embedded (Paas) bağlantı dizesinden program aracılığıyla sorgulayabilirsiniz. Bir örnek için bkz: [PaaS rapor ayıklamak DirectQuery bağlantı dizesinden](migrate-code-snippets.md#extract-directquery-connection-string-from-power-bi-workspace-collections).

Power BI hizmetinde veri kümesi için bağlantı dizesini güncellemeniz ve veri kaynağı için kimlik bilgilerini ayarlayın. Bunun nasıl yapılacağı görmek için aşağıdaki örneklerde bakabilirsiniz.

* [DirectQuery bağlantı dizesinde Power BI Embedded güncelleştir](migrate-code-snippets.md#update-directquery-connection-string-in-power-bi-embedded)
* [Power BI Embedded'de DirectQuery kimlik bilgilerinin ayarlanması](migrate-code-snippets.md#set-directquery-credentials-in-power-bi-embedded)

## <a name="next-steps"></a>Sonraki adımlar

Raporlarınızı Power BI çalışma koleksiyonlarından Power BI Embedded geçirilen, şimdi uygulamanızı güncelleştirmeniz ve raporlar bu uygulama çalışma alanında katıştırma başlar.

Daha fazla bilgi için bkz: [Power BI çalışma alanı koleksiyonu içerik Power BI Embedded geçirmek nasıl](migrate-from-power-bi-workspace-collections.md).

Başka sorunuz mu var? [Power BI topluluk isteyen deneyin](http://community.powerbi.com/)