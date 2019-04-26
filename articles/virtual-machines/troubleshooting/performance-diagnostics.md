---
title: Azure sanal makineler için performans tanılama | Microsoft Docs
description: Windows için Azure performans tanılama tanıtır.
services: virtual-machines-windows'
documentationcenter: ''
author: anandhms
manager: cshepard
editor: przlplx
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 9/20/2018
ms.author: anandh
ms.openlocfilehash: c2089f9f6267f318dafe641a6a5b22e7e87427ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60308202"
---
# <a name="performance-diagnostics-for-azure-virtual-machines"></a>Azure sanal makineleri için performans tanılamaları

Performans Tanılama Aracı'nı bir Windows sanal makinesi (VM) etkileyebilecek performans sorunlarını gidermenize yardımcı olur. Desteklenen sorun giderme senaryoları bilinen sorunları ve en iyi yöntemler ve VM performansı yavaş veya yüksek CPU, bellek veya disk alanı kullanımını içeren karmaşık sorunlar hızlı denetimleri içerir. 

Burada ayrıca Öngörüler ve raporda çeşitli günlükler, zengin yapılandırma ve tanılama verilerini gözden geçirebilirsiniz, doğrudan Azure portalından, performans tanılama çalıştırabilirsiniz. Performans Tanılama çalıştırın ve Microsoft Support başvurmadan önce Öngörüler ve tanılama verilerini gözden öneririz.

> [!NOTE]
> Performans Tanılama, .NET SDK'sı sürüm 4.5 veya üzeri bir sürümü yüklü Windows Vm'lerinde desteklenmektedir. Performans Tanılama Klasik Vm'leri çalıştırma adımları için bkz: [Azure performans tanılama VM uzantısı](performance-diagnostics-vm-extension.md).

### <a name="supported-operating-systems"></a>Desteklenen İşletim Sistemleri
Windows 10, Windows 8, Windows 8 Enterprise, Windows 8 Pro, Windows 8.1, Windows Server 2016, Windows Server 2012, Windows Server 2012 Datacenter, Windows Server 2012 R2, Windows Server 2012 R2 Datacenter, Windows Server 2012 R2 Standard, Windows Server 2012 Standard, Windows Server 2008 R2, Windows Server 2008 R2 Datacenter, Windows Server 2008 R2 Enterprise, Windows Server 2008 R2 Foundation, Windows Server 2008 R2 SP1, Windows Server 2008 R2 Standard.

## <a name="install-and-run-performance-diagnostics-on-your-vm"></a>Yükleme ve sanal makinenizde performans tanılama çalıştırma
Performans Tanılama VM uzantısı adlı bir Tanılama aracını çalıştıran yükler [Perfınsights](https://aka.ms/perfinsights). Yükleme ve performans tanılama çalıştırmak için aşağıdaki adımları izleyin:
1.  Komutları sol sütununda seçin **sanal makineler**.
1.  VM adları listesinden tanılama çalıştırmak istediğiniz VM'yi seçin.
1.  Komutları sağ sütunda seçin **Performans Tanılama**.

    ![Azure portalının ekran görüntüsü, yükleme performans tanılama düğmesi vurgulanan](media/performance-diagnostics/performance-diagnostics-install.png)

    > [!NOTE]
    > Bu ekran görüntüsünde, bir sanal makine adı'nın dikey penceresini gizlenir.
1. (İsteğe bağlı) depolama hesabı seçin

    Birden çok VM için performans tanılama sonuçlarını depolamak için bir tek bir depolama hesabı kullanmak istiyorsanız, tıklayarak bir depolama hesabı seçebilirsiniz **ayarları** araç çubuğu düğmesi. Tıklayın **Tamam** depolama hesabını seçin sonra düğme.

    Bir depolama hesabı belirtmezseniz, varsayılan olarak yeni bir depolama hesabı oluşturulur.

    ![Ayarları araç çubuğu düğmesi vurgulanmış ekran görüntüsü / performans tanılama dikey](media/performance-diagnostics/settings-button.png)

    ![Performans Tanılama ayarları dikey penceresinde depolama hesabını seçiminin ekran görüntüsü](media/performance-diagnostics/select-storage-account.png)

1. Seçin **yükleme Performans Tanılama** düğmesi.
1. Seçin **tanılama Çalıştır** yükleme tamamlandıktan sonra bir tanılama çalıştırmak istiyorsanız kutuyu. Bu seçimi yaparsanız, ilgili seçenekleri ve Performans Analizi senaryosu tercih mümkün olacaktır.

    ![Ekran görüntüsü / performans tanılama Yükle düğmesi](media/performance-diagnostics/install-diagnostics-button.png)

## <a name="select-an-analysis-scenario-to-run"></a>Çalıştırılacak bir analiz senaryo seçin

Aşağıdaki analiz senaryoları, Azure portalından kullanılabilir. Bir analiz, karşılaştığınız performans sorunu bağlı olarak seçin. Analiz için gerekli süre ve İzleme Seçenekleri'ni seçin.

* **Hızlı Performans Analizi**  
    Bilinen sorunları denetler, en iyi analiz ve tanılama verilerini toplar. Bu analiz çalıştırmak için birkaç dakika sürer. [Daha fazla bilgi](https://aka.ms/perfinsights/quick)

* **Performans Analizi**  
    Tüm denetimleri hızlı performans analizi içerir ve yüksek kaynak tüketimi izler. Yüksek CPU, bellek ve disk kullanımı gibi genel performans sorunlarını gidermek için bu sürümü kullanın. Bu analiz, seçili süre bağlı olarak 15 dakika 30 saniye sürer. [Daha fazla bilgi](https://aka.ms/perfinsights/vmslow) 
    
* **Gelişmiş Performans Analizi**  
    Aşağıdaki bölümlerde belirtildiği gibi bir veya daha fazla izlemelerini toplar ve performansı analiz tüm denetimleri içerir. Bu senaryo ek izlemeleri gereken karmaşık sorunları gidermek için kullanılır. Bu senaryo için daha uzun süre çalışan VM ve seçilen izleme seçenekleri boyutuna bağlı olarak, tanılama çıkışı toplam boyutunu artırır. Bu analiz, seçilen süre bağlı olarak çalıştırmak için 15 dakika 30 saniye sürer. [Daha fazla bilgi](https://aka.ms/perfinsights/advanced) 
    
* **Azure dosyaları analizi**  
    Ağ izleme ve SMB sayaçları yakalar ve performansı analiz tüm denetimleri içerir. Bu senaryo, Azure dosyaları, performans sorunlarını gidermek için kullanılır. Bu analiz, seçilen süre bağlı olarak çalıştırmak için 15 dakika 30 saniye sürer. [Daha fazla bilgi](https://aka.ms/perfinsights/azurefiles)


![Tanılama bölmesinde performans tanılama dikey ekran görüntüsü çalıştırma](media/performance-diagnostics/run-diagnostics-pane.png)

### <a name="provide-symptoms-optional"></a>Belirtiler (isteğe bağlı) girin
Listeden herhangi bir seçilmiş belirtileri seçin veya yeni belirtileri ekleyin. Bu analiz gelecekte geliştirmemize yardımcı olur. 

### <a name="provide-support-request-number-if-available-optional"></a>Kullanılabilir destek isteği numarasını sağlayın (isteğe bağlı)
Bir Microsoft destek mühendisiyle var olan bir destek bileti üzerinde çalışıyorsanız, destek bileti numarası sağlayın. 

### <a name="review-the-privacy-policy-and-legal-terms-and-select-the-check-box-to-acknowledge-required"></a>Yasal koşulları ve Gizlilik İlkesi'ni gözden geçirin ve kabul etmek için (gerekli) onay kutusunu işaretleyin
Tanılama çalıştırmak için yasal koşulları kabul ve gizlilik ilkesini kabul edin.

### <a name="select-ok-to-run-the-diagnostics"></a>Tanılama çalıştırmak için Tamam'ı seçin 
Performans Tanılama yüklemek başlatılırken bir bildirim görüntülenir. Yükleme tamamlandıktan sonra yüklemenin başarılı olduğunu belirten bir bildirim görür. Seçili analiz ardından belirtilen süre için çalıştırılır. Bu tanılama veri doğru zamanda yakalanabilir böylece performans sorunu yeniden oluşturmak için iyi bir zaman olabilir. 

Analiz tamamlandıktan sonra aşağıdaki öğeleri Azure tabloları ve belirtilen depolama hesabındaki bir ikili büyük nesne (BLOB) kapsayıcısına yüklenir:

*   Tüm çalıştırma ilgili bilgiler ve Öngörüler
*   Bir çıkış sıkıştırılmış (.zip) dosyası (adlı **PerformanceDiagnostics_yyyy-MM-dd_hh-mm-ss-fff.zip**) içeren günlük dosyaları
*   Bir HTML raporu

Karşıya yükledikten sonra Azure portalında yeni bir tanılama raporu listelenir.

![Performans Tanılama dikey penceresindeki tanılama raporu listesinin ekran görüntüsü](media/performance-diagnostics/diagnostics-report-list.png)

## <a name="how-to-change-performance-diagnostics-settings"></a>Performans Tanılama ayarlarını değiştirme
Kullanım **ayarları** burada çıkış ve tanılama öngörüleri depolanabilir depolama hesabını değiştirmek için araç çubuğu düğmesi. Performans Tanılama kullanan birden çok VM için aynı depolama hesabını kullanabilirsiniz. Depolama hesabını değiştirdiğinizde, eski rapor ve öngörü silinmez. Ancak, bunlar artık tanılama raporları listesinde görüntülenir. 

## <a name="review-insights-and-performance-diagnostics-report"></a>Öngörü ve performans tanılama raporu gözden geçirin
Çalıştıran her tanılama öngörüleri ve öneriler, etkilenen kaynaklar, günlük dosyaları ve toplanan diğer zengin tanılama bilgileri listesini yanı sıra, çevrimdışı izleme için bir rapor içerir. Tüm toplanan Tanılama verileri tam bir listesi için bkz. [Perfınsights tarafından hangi tür bilgiler toplanır?](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-use-perfinsights#what-kind-of-information-is-collected-by-perfinsights) 

### <a name="select-a-performance-diagnostics-report"></a>Performans Tanılama raporunu seçin
Tanılama rapor listesi çalıştırılmış olan tüm tanılama raporları bulmak için kullanabilirsiniz. Liste kullanılan çözümlemesi hakkında daha fazla ayrıntı, bulunan Öngörüler ve kendi etki düzeyleri içerir. Daha fazla ayrıntı görüntülemek için bir satır seçin.

![Performans Tanılama dikey penceresinden bir tanılama raporu seçme işleminin ekran görüntüsü](media/performance-diagnostics/select-report.png)

### <a name="review-a-performance-diagnostics-report"></a>Performans tanılama raporu gözden geçirin
Her performans tanılama raporu birkaç ınsights içerir ve yüksek, Orta veya düşük etkisi düzeyini belirtmek. Her Insight sorunu azaltmak yardımcı olacak öneriler de içerir. Insights kolay filtreleme için gruplandırılır. 

Etki düzeyleri olası performans sorunlarını, yanlış yapılandırma, bilinen sorunları gibi faktörlere göre veya diğer kullanıcılar tarafından bildirilen sorunları temsil eder. Henüz bir veya daha fazla listelenen sorunlar yaşayabilen değil. Örneğin, SQL günlük dosyaları ve veritabanı dosyalarını aynı verileri disk üzerinde olabilir. Kullanım düşük ise, bir sorunu fark etmeyebilirsiniz ise veritabanı kullanımı yüksekse, bu durum yüksek olası performans sorunlarını ve diğer performans sorunları vardır.

![Genel Bakış dikey penceresi ekran görüntüsü / performans tanılama raporu](media/performance-diagnostics/performance-diagnostics-report-overview.png)

### <a name="reviewing-performance-diagnostics-insights-and-recommendations"></a>Performans Tanılama öngörüleri ve önerileri gözden geçirme
Etkilenen kaynaklar, önerilen risk azaltma işlemleri ve referans bağlantıları hakkında daha fazla ayrıntı görüntülemek için bir öngörü seçebilirsiniz. 

![Performans Tanılama Insight ayrıntı ekran görüntüsü](media/performance-diagnostics/insight-detail.png)

### <a name="download-and-review-the-full-performance-diagnostics-report"></a>İndirin ve tam performans tanılama raporu gözden geçirin
Kullanabileceğiniz **raporu indir** depolama ve ağ yapılandırması, performans sayaçları gibi ek zengin tanılama bilgileri içeren bir HTML raporu indirmek için düğmeye, süreçlerin listesini izler ve günlüğe kaydeder. Seçili çözümleme içeriği bağlıdır. Gelişmiş sorun giderme için rapor yüksek CPU kullanımı, yüksek disk kullanımı ve aşırı şekilde bellek kullanan işlemleri için ilgili etkileşimli grafikler ve ek bilgiler içerebilir. Performans tanılama raporu hakkında daha fazla bilgi için bkz: [gözden tanılama raporu](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-use-perfinsights#review-the-diagnostics-report).

## <a name="manage-performance-diagnostics-reports"></a>Performans Tanılama raporlarını yönetme
Bir veya daha fazla performans tanılama raporları kullanarak silebilirsiniz **silme rapor** düğmesi.

## <a name="how-to-uninstall-performance-diagnostics"></a>Performans Tanılama kaldırma
Performans Tanılama VM'den kaldırabilirsiniz. Bu eylem, VM uzantısını kaldırır ancak depolama hesabında herhangi bir tanılama veri etkilemez. 

![Kaldır düğmesi vurgulanan performans tanılama dikey penceresi araç ekran görüntüsü](media/performance-diagnostics/uninstal-button.png)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="where-is-the-diagnostics-data-from-my-vm-stored"></a>Depolanan sanal Makinem Tanılama verileri nerede? 
Tüm performans tanılama öngörüleri ve raporları kendi depolama hesabında depolanır. Insights, Azure tabloları içinde depolanır. Raporları sıkıştırılmış dosyayı azdiagextnresults adlı bir ikili büyük nesne (BLOB) kapsayıcıda depolanır.

Araç çubuğunda Ayarlar düğmesini kullanarak depolama hesabı bilgileri görüntüleyebilirsiniz. 

### <a name="how-do-i-share-this-data-with-microsoft-customer-support"></a>Bu veriler Microsoft müşteri desteği ile nasıl paylaşırım? 
Tanılama raporu Microsoft ile paylaşmak için birden çok yolu vardır.

**1. seçenek:** Otomatik olarak en son rapor paylaşma  
Microsoft ile bir destek bileti açtığınızda, performans tanılama raporu paylaşmak önemlidir. Tanılama çalıştırırken bu bilgileri Microsoft ile paylaşmak için seçimi yaptıysanız (seçerek "**tanılama bilgileri Microsoft ile paylaşmak kabul ediyorum**" onay kutusu), Microsoft Depolama hesabınızdan rapora erişmek mümkün olacaktır SAS bağlantısı çıkış zip dosyasını çalıştırma tarihten itibaren 30 gün boyunca kullanarak hesabı. Yalnızca en son rapor, destek mühendisine kullanılabilir. 

**2. seçenek:** Tanılama raporu sıkıştırılmış dosya için paylaşılan erişim imzası oluşturma  
Paylaşılan erişim imzaları'nı kullanarak raporları sıkıştırılmış dosyanın bir bağlantısını paylaşabilir. Bunu yapmak için şu adımları uygulayın: 
1.  Azure portalında, tanılama verilerin depolandığı depolama hesabına gidin.
1.  Seçin **Blobları** altında **Blob hizmeti** bölümü. 
1.  Seçin **azdiagextnresults** kapsayıcı.
1.  Paylaşmak istediğiniz performans tanılama çıkışı sıkıştırılmış dosyayı seçin.
1.  Üzerinde **Generate SAS** sekmesinde, paylaşımı ölçütlerini seçin. 
1.  Tıklayın **blob SAS belirteci ve URL üretmek**.
1.  Kopyalama **Blob SAS URL'si**ve Destek mühendisiyle paylaşın. 

**Seçenek 3:** Depolama hesabından raporunu indirin

Performans tanılama raporu sıkıştırılmış dosya seçeneği 2'de 1-4 arasındaki adımları kullanarak da bulabilirsiniz. Dosyayı indirin ve ardından e-posta yoluyla paylaşma veya dosyayı karşıya yüklemek yönergeler için destek mühendisinden için seçin.  

### <a name="how-do-i-capture-the-diagnostics-data-at-the-correct-time"></a>Doğru zamanda nasıl Tanılama verileri yakalamayı?
Her performans tanılama Çalıştır iki aşamadan oluşur: 
1.  Yükleme veya Performans Tanılama VM uzantısı güncelleştirme.
1.  Tanılama belirtilen süre için çalıştırın.

Şu anda tam olarak VM uzantısı yükleme işlemi tamamlandıktan sonra öğrenmek için kolay bir yolu yoktur. Genellikle VM uzantısı'nı yüklemek için yaklaşık 45 saniye ila 1 dakika sürer. VM uzantısı yüklendikten sonra sorun giderme için veri kümesini doğru yakalama performans tanılama için yeniden oluşturma adımlarınızı çalıştırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Devam edemez ve sorunun nedenini belirlemek daha fazla yardıma ihtiyacınız varsa, rapor ve performans tanılama öngörüleri inceledikten sonra Microsoft müşteri desteği ile bir destek bileti açabilirsiniz. 

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/)seçip **Destek**. Azure desteği'ni kullanma hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
