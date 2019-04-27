---
title: Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Tehdit modelleme aracı sunulan tüm özellikleri hakkında bilgi edinin
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 601f3bf05388406c8f96a7351f7fb3aa4de2650a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60588800"
---
# <a name="threat-modeling-tool-feature-overview"></a>Tehdit modelleme aracı özelliğine genel bakış

Tehdit modelleme aracı gereksinimlerini modelleme, tehdidin yardımcı olabilir. Araç için temel bir giriş için bkz: [tehdit modelleme aracı ile çalışmaya başlama](./azure-security-threat-modeling-tool-getting-started.md).

> [!NOTE]
>Tehdit modelleme aracı sık sık güncelleştirilir, böylece müşterilerimize en son özellikler ve geliştirmeler genellikle görmek için bu kılavuzu denetleyin.

Boş bir sayfa açmak için seçmeniz **bir modeli oluşturma**.

![Boş sayfa](./media/azure-security-threat-modeling-tool-feature-overview/tmtstart.png)

Aracı şu anda kullanılabilir özellikleri görmek için ekibimiz tarafından oluşturulan tehdit modeli kullanan [başlama](./azure-security-threat-modeling-tool-getting-started.md) örnek.

![Temel bir tehdit modeli](./media/azure-security-threat-modeling-tool-feature-overview/basictmt.png)

## <a name="navigation"></a>Gezinme

Yerleşik özellikler ele önce aracında bulunan ana bileşenleri gözden geçirelim.

### <a name="menu-items"></a>Menü öğeleri

Diğer Microsoft ürünleri için benzer deneyimidir. Üst düzey menü öğeleri gözden geçirelim.

![Menü öğeleri](./media/azure-security-threat-modeling-tool-feature-overview/menuitems.png)

| Etiket                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Dosya** | <ul><li>Açın, kaydedin ve dosyaları kapatın</li><li>Oturumu açmak ve OneDrive hesabı dışında.</li><li>Bağlantılar (görüntüleme ve düzenleme) paylaşın.</li><li>Dosya bilgileri görüntüleyin.</li><li>Yeni bir şablonu mevcut modelleri için geçerlidir.</li></ul> |
| **Düzenle** | Geri Al ve Yinele eylemleri yanı sıra, kopyalama, yapıştırma ve silin. |
| **Görünümü** | <ul><li>Arasında geçiş **analiz** ve **tasarım** görünümleri.</li><li>Kapatılan pencereler (örneğin, şablonlar, öğe özelliklerinin ve iletileri).</li><li>Düzen, varsayılan ayarlarına sıfırlanır.</li></ul> |
| **Diyagramı** | Ekleme ve diyagramları silin ve diyagramların sekmeler arasında taşıyın. |
| **Raporlar** | HTML raporları başkalarıyla paylaşmak için oluşturun. |
| **Yardım** | Aracı'nı kullanmanıza yardımcı olacak Kılavuzlar bulun. |

Simgeleri kısayolları en üst düzey menüler şunlardır:

| Sembol                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **açın** | Yeni bir dosya açar. |
| **Kaydet** | Geçerli dosyaya kaydeder. |
| **Tasarım** | Açılır **tasarım** modelleri oluşturabileceğiniz görünümü. |
| **Çözümleme** | Tehditler ve bunların özelliklerini gösterir oluşturulur. |
| **Diyagram ekleme** | Yeni bir diyagramda (Excel'de yeni sekmeler benzer) ekler. |
| **Diyagram Sil** | Geçerli diyagrama siler. |
| **Kes/kopyala/yapıştır** | Kopyalar, keser ve öğeleri yapıştırır. |
| **Geri Al/Yinele** | Alır ve eylemleri yineler. |
| **Yakınlaştırma / Uzaklaştır** | Daha iyi bir görünüm için diyagramın içine ve dışına yakınlaştırır. |
| **Geri Bildirim** | MSDN Forumu açılır. |

### <a name="canvas"></a>Canvas

Tuval burada sürükle ve bırak öğeleri bir alandır. Sürükle ve bırak, modelleri oluşturmak için hızlı ve en verimli yoludur. Ayrıca, sağ tıklayın ve menüden gösterildiği gibi genel sürümlerini öğeleri eklemek için öğeleri seçin:

#### <a name="drop-the-stencil-on-the-canvas"></a>Şablon tuvaline sürükleyip bırakabilir.

![Canvas drop](./media/azure-security-threat-modeling-tool-feature-overview/canvasdrop1.png)

#### <a name="select-the-stencil"></a>Bir şablon seçin

![Öğe özellikleri](./media/azure-security-threat-modeling-tool-feature-overview/canvasdrop2.png)

### <a name="stencils"></a>Şablonlar

Seçtiğiniz şablona bağlı olarak, kullanabileceğiniz tüm şablonlar bulabilirsiniz. Doğru öğeleri bulamazsanız, başka bir şablon kullanın. Veya bir şablon kendi gereksinimlerinize uyacak şekilde değiştirebilirsiniz. Genel olarak, bunlar gibi kategoriler birleşimi bulabilirsiniz:

| Şablon adı                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **İşlem** | Uygulamaları, tarayıcı eklentileri, iş parçacıkları, sanal makineler |
| **Dış etkileşen** | Kimlik doğrulama sağlayıcıları, tarayıcılar, kullanıcılar, web uygulamaları |
| **Veri deposu** | Önbellek, depolama, yapılandırma dosyaları, veritabanları, kayıt defteri |
| **Veri akışı** | İkili, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, adlandırılmış kanal, RPC/DCOM, SMB, UDP |
| **Satır/Kenarlık sınır güven** | Kurumsal ağlarda, internet, makine, korumalı alan, kullanıcı/çekirdek modu |

### <a name="notesmessages"></a>Notları/iletileri

| Bileşen                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **İletileri** | Öğeler arasındaki bir veri akışı gibi bir hata olduğunda kullanıcıları uyarır iç aracı mantığı. |
| **Notlar** | El ile notları tasarım boyunca mühendislik ekipleri tarafından dosyasına eklenir ve işlemini gözden geçirin. |

### <a name="element-properties"></a>Öğe özellikleri

Öğe özelliklerinin seçtiğiniz öğeler farklılık gösterir. Güven sınırları dışında diğer tüm öğeleri üç genel seçimleri içerir:

| Öğe özelliği                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Ad** | Böylece bir kolayca tanınan adlandırma işlemleri, depolar, interactors ve akışlar için kullanışlıdır. |
| **Kapsam dışına** | Seçili öğe (önerilmez) tehdit nesil matris dışı alınır. |
| **Kapsam dışında nedeni** | Neden kapsam dışında kullanıcılarınıza için gerekçe alanı seçilmiştir. |

Özellikleri, her öğe kategorisi altında değiştirilir. Kullanılabilir seçenekler incelemek için her bir öğe seçin. Veya daha fazla bilgi için şablon açabilirsiniz. Özellikleri gözden geçirelim.

## <a name="welcome-screen"></a>Hoş Geldiniz ekranı

Uygulamayı açtığınızda **Hoş Geldiniz** ekran.

### <a name="open-a-model"></a>Model açma

Üzerine **açık bir modeli** iki seçeneklerini görüntülemek için: **Bu bilgisayardan Aç** ve **Onedrive'dan açık**. İlk seçeneği açılır **Dosya Aç** ekran. İkinci seçenek için OneDrive oturum açma işlemine götürür. Başarılı kimlik doğrulamadan sonra klasörleri ve dosyaları seçebilirsiniz.

![Açık modeli](./media/azure-security-threat-modeling-tool-feature-overview/openmodel.png)

![Bilgisayar veya OneDrive buradan Aç](./media/azure-security-threat-modeling-tool-feature-overview/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Geribildirim, öneri ve sorunları

Seçtiğinizde, **geribildirim, öneri ve sorunları**, SDL araçları için MSDN forumuna gidin. Geçici çözümler ve yeni fikirler dahil olmak üzere aracı hakkında diğer kullanıcıların görüşlerini okuyabilirsiniz.

![Geri Bildirim](./media/azure-security-threat-modeling-tool-feature-overview/feedback.png)

## <a name="design-view"></a>Tasarım görünümü

Açarken veya yeni bir model oluşturmak **tasarım** görüntülemek açar.

### <a name="add-elements"></a>Öğeleri Ekle

İki yolla kılavuzundaki öğeler ekleyebilirsiniz:

- **Sürükle ve bırak**: İstenen öğe kılavuza sürükleyin. Ardından öğe özelliklerini ek bilgi sağlamak için kullanın.
- **Sağ**: Herhangi bir yere Kılavuzu'nun sağ tıklayın ve açılan menüden öğeleri seçin. Seçtiğiniz öğenin genel bir temsilini ekranda görünür.

### <a name="connect-elements"></a>Öğeleri bağlanma

Öğeleri iki yolla bağlanabilir:

- **Sürükle ve bırak**: İstenen veri akışı kılavuza sürükleyin ve ikisinde uygun öğelerine bağlanın.
- **Shift + tıklayın**: (Veri gönderen) ilk öğe, tuşuna basın ve Shift tuşunu basılı tutun ve ardından (veri alma) ikinci öğeyi seçin. Sağ tıklatın ve seçin **Connect**. Bir çift yönlü veri akışı kullanırsanız, siparişin önemli değil.

### <a name="properties"></a>Özellikler

 Şablonlarda select şablon ve bilgiler değiştirilebilir özellikleri görmek için uygun şekilde doldurur. Aşağıdaki örnek, önceki ve sonraki gösterir bir **veritabanı** kalıbı diyagram üzerine sürüklediğiniz:

#### <a name="before"></a>Önce

![Önce](./media/azure-security-threat-modeling-tool-feature-overview/properties1.png)

#### <a name="after"></a>Sonra

![Sonra](./media/azure-security-threat-modeling-tool-feature-overview/properties2.png)

### <a name="messages"></a>İletiler

Bir tehdit modeli oluşturmak ve veri akışlarını öğeleri bağlanmak unutursanız, bir bildirim alırsınız. Bu iletiyi yoksayabilirsiniz veya sorunu düzeltmek için yönergeleri izleyebilirsiniz. 

![İletiler](./media/azure-security-threat-modeling-tool-feature-overview/messages.png)

### <a name="notes"></a>Notlar

Diyagram için Not eklemek için geçmek **iletileri** için sekmesinde **notları** sekmesi.

## <a name="analysis-view"></a>Analiz görünümü

Diyagram derledikten sonra seçin **analiz** simgesi (Büyüteç) geçmek için kısayollar araç çubuğunda **analiz** görünümü.

![Analiz görünümü](./media/azure-security-threat-modeling-tool-feature-overview/analysisview.png)

### <a name="generated-threat-selection"></a>Oluşturulan tehdit seçimi

Tehdit seçtiğinizde, üç ayrı işlev kullanabilirsiniz:

| Özellik                               | Bilgi      |
| --------------------------------------- | ------------ |
| **Okuma göstergesi** | <p>Tehdit gözden geçirmeniz öğelerini izlemenize yardımcı olan okundu olarak işaretlenir.</p><p>![Okuma/okunmamış göstergesi](./media/azure-security-threat-modeling-tool-feature-overview/readmode.png)</p> |
| **Etkileşim odağı** | <p>Tehdit ait olduğu diyagram etkileşiminde vurgulanır.</p><p>![Etkileşim odağı](./media/azure-security-threat-modeling-tool-feature-overview/interactionfocus.png)</p> |
| **İş parçacığı özellikleri** | <p>Tehdit hakkında ek bilgi görünür **tehdit özellikleri** penceresi.</p><p>![İş parçacığı özellikleri](./media/azure-security-threat-modeling-tool-feature-overview/threatproperties.png)</p> |

### <a name="priority-change"></a>Öncelik değişikliği

Oluşturulan her tehdit öncelik düzeyini değiştirebilirsiniz. Farklı renkler, yüksek, Orta ve düşük öncelikli tehditler tanımlamak kolaylaştırır.

![Öncelik değişikliği](./media/azure-security-threat-modeling-tool-feature-overview/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Tehdit özellikler düzenlenebilir alanları

Önceki görüntüde görüldüğü gibi araç tarafından oluşturulan bilgileri değiştirebilirsiniz. Gerekçe gibi belirli alanları bilgi ekleyebilirsiniz. Bu alanlar, şablon tarafından oluşturulur. Her tehdit için daha fazla bilgiye ihtiyacınız varsa, değişiklikler yapabilirsiniz.

![İş parçacığı özellikleri](./media/azure-security-threat-modeling-tool-feature-overview/threatproperties.png)

## <a name="reports"></a>Reports

Önceliklerini değiştirmek ve oluşturulan her tehdit durumunu güncelleştirerek tamamladıktan sonra size dosyayı kaydedin ve/veya bir raporu yazdırın. Git **rapor** > **tam raporu oluşturma**. Rapor adı ve aşağıdaki görüntüye benzer bir şey görmeniz gerekir:

![Rapor](./media/azure-security-threat-modeling-tool-feature-overview/report.png)

## <a name="next-steps"></a>Sonraki adımlar

- Soru, yorum ve konuları tmtextsupport@microsoft.com. **[İndirme](https://aka.ms/threatmodelingtool)**  kullanmaya başlamak için tehdit modelleme aracı.
- Bir şablon için topluluğa katkıda bulunmak için Git bizim [GitHub](https://github.com/Microsoft/threat-modeling-templates) sayfası.