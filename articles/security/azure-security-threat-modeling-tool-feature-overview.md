---
title: "Microsoft tehdit modelleme aracı - Azure | Microsoft Docs"
description: "Tehdit modelleme aracında kullanılabilir tüm özellikler hakkında bilgi edinin"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 5c60e13028c3ccdf3269d74ab4724bb34ca10c19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="threat-modeling-tool-feature-overview"></a>Tehdit modelleme aracı özelliğine genel bakış

Tehdit modelleme aracı gereksinimlerini modelleme, tehdidin yardımcı olabilir. Aracı temel bir giriş için bkz: [tehdit modelleme aracı ile çalışmaya başlama](./azure-security-threat-modeling-tool-getting-started.md).

> [!NOTE]
>Tehdit modelleme Aracı'nı sık sık güncelleştirilen, genellikle, en son özellikleri ve geliştirmeleri görmek için bu kılavuzu kontrol edin.

Boş bir sayfa açmak için seçin **bir modeli oluşturma**.

![Boş bir sayfa](./media/azure-security-threat-modeling-tool/tmtstart.png)

Aracı şu anda kullanılabilen özellikleri görmek için bizim ekibi tarafından oluşturulan tehdit modeli kullanın [başlama](./azure-security-threat-modeling-tool-getting-started.md) örnek.

![Temel tehdit modeli](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a>Gezinme

Şimdi yerleşik özellikleri aşağıdakiler ele önce aracında bulunan ana bileşenlerini gözden geçirin.

### <a name="menu-items"></a>Menü öğeleri

Diğer Microsoft ürünleri için benzer bir deneyim yaşanır. Şimdi en üst düzey menü öğeleri gözden geçirin.

![Menü öğeleri](./media/azure-security-threat-modeling-tool/menuitems.png)

| Etiket                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Dosya** | <ul><li>Açma, kaydetme ve dosyaları kapatın</li><li>Oturum açın ve OneDrive hesapları dışında imzalayın.</li><li>Bağlantılar (görüntüleme ve düzenleme) paylaşır.</li><li>Dosya bilgileri görüntüleyin.</li><li>Yeni bir şablon varolan modelleri için geçerlidir.</li></ul> |
| **Düzenleme** | Geri alma ve Yinele Eylemler yanı sıra, kopyalama, yapıştırma ve silin. |
| **Görünümü** | <ul><li>Arasında geçiş **analiz** ve **tasarım** görünümleri.</li><li>Kapalı pencereler (örneğin, şablonlar, öğe özellikleri ve iletileri).</li><li>Düzen, varsayılan ayarlarına sıfırlanır.</li></ul> |
| **Diyagramı** | Ekleme ve diyagramları silme ve diyagramları sekmeler arasında taşıyın. |
| **Raporlar** | Diğer kullanıcılarla paylaşmak için HTML raporlar oluşturun. |
| **Yardım** | Aracı'nı kullanmanıza yardımcı olması için Kılavuzlar bulabilirsiniz. |

Üst düzey menü kısayolları simgeler şunlardır:

| Simgesi                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Açık** | Yeni bir dosya açar. |
| **Kaydet** | Geçerli dosya kaydeder. |
| **Tasarım** | Açılır **tasarım** modelleri oluşturabileceğiniz görünümü. |
| **Çözümleme** | Tehditler ve bunların özelliklerini gösterir oluşturulur. |
| **Diyagrama ekleyin** | Yeni bir diyagram (Excel yeni sekmelerde benzer) ekler. |
| **Diyagram Sil** | Geçerli diyagram siler. |
| **Kes/kopyala/yapıştır** | Kopyalar, keser ve öğeleri gönderebilir. |
| **Geri alma/yineleme** | Alır ve eylemleri yineler. |
| **Yakınlaştırma / Uzaklaştır** | Ve daha iyi bir görünüm için diyagramı yakınlaştırır. |
| **Geri Bildirim** | MSDN Forumu açar. |

### <a name="canvas"></a>Tuvale

Tuvale yere sürükleyin ve öğeleri bırakma bir alandır. Sürükle ve bırak yoludur modelleri oluşturmak için hızlı ve en iyi yoldur. Ayrıca, sağ tıklatın ve menüsünden gösterildiği gibi öğeleri, genel sürümleri eklemek için öğeleri seçin:

#### <a name="drop-the-stencil-on-the-canvas"></a>Tuvalde şablon bırak

![Tuvale bırakma](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="select-the-stencil"></a>Bir şablon seçin

![Öğe özellikleri](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a>Şablonlar

Seçtiğiniz şablona göre kullanılabilir tüm şablonlar bulabilirsiniz. Sağ öğeleri bulamazsanız, başka bir şablon kullanın. Veya bir şablon gereksinimlerinize uyacak şekilde değiştirebilirsiniz. Genellikle, bu gibi kategoriler bileşimini bulabilirsiniz:

| Şablon adı                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **İşlem** | Uygulamalar, tarayıcı eklentileri, iş parçacıkları, sanal makineler |
| **Dış etkileşen** | Kimlik doğrulama sağlayıcıları, tarayıcılar, kullanıcılar, web uygulamaları |
| **Veri deposu** | Önbelleği, depolama, yapılandırma dosyalarını, veritabanları, kayıt defteri |
| **Veri akışı** | İkili, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, adlandırılmış kanal, RPC/DCOM, SMB, UDP |
| **Çizgi/Kenarlık sınır güven** | Şirket ağları, internet, makine, korumalı alan, kullanıcı/çekirdek modu |

### <a name="notesmessages"></a>Notlar/iletileri

| Bileşen                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **İletileri** | Öğeler arasında hiçbir veri akışları gibi bir hata olduğunda kullanıcıları uyarır iç aracı mantığı. |
| **Notlar** | El ile notları dosyaya tasarım boyunca mühendislik ekipleri tarafından eklenir ve işlem gözden geçirin. |

### <a name="element-properties"></a>Öğe özellikleri

Öğe özellikleri seçtiğiniz öğeler farklılık gösterir. Güven sınırları dışında üç genel seçimleri diğer tüm öğeleri içerir:

| Öğe özelliği                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Ad** | Kolay tanınan bir adlandırma işlemleri, depolar, interactors ve akışlar için kullanışlıdır. |
| **Kapsamının dışında** | Seçili olduğunda, öğe (önerilmez) tehdit nesil matris dışı alınır. |
| **Kapsam dışında nedeni** | Kapsam dışında neden bilmesini sağlamak üzere gerekçe alanları seçilmedi. |

Özellikleri her öğe kategorisi altında değiştirilir. Kullanılabilir seçenekler incelemek için her bir öğe seçin. Veya daha fazla bilgi için şablon açabilirsiniz. Şimdi özellikleri gözden geçirin.

## <a name="welcome-screen"></a>Hoş Geldiniz ekranı

Uygulamasını açın, gördüğünüz **Hoş Geldiniz** ekran.

### <a name="open-a-model"></a>Bir model açın

Üzerine gelerek **açık bir modeli** iki seçenek ortaya çıkarmak için: **açık bu bilgisayarı** ve **gelen açık OneDrive**. İlk seçeneği açılır **Dosya Aç** ekran. İkinci seçenek OneDrive oturum açma işlemini alır. Başarılı kimlik doğrulamasından sonra dosya ve klasörleri seçebilirsiniz.

![Açık modeli](./media/azure-security-threat-modeling-tool/openmodel.png)

![Bilgisayardan veya OneDrive Aç](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Geri bildirim, öneriler ve sorunları

Seçtiğinizde, **geri bildirim, öneriler ve sorunları**, MSDN Forumu için SDL araçları gidin. Geçici çözümler ve yeni fikirleri dahil olmak üzere aracı hakkında başkalarının ne dediğini okuyabilir.

![Geri Bildirim](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a>Tasarım görünümü

Açtığınızda veya yeni bir model oluşturmak **tasarım** görüntülemek açar.

### <a name="add-elements"></a>Öğeler ekleme

İki yolla kılavuz öğeleri ekleyebilirsiniz:

- **Sürükleme ve bırakma**: İstenen öğe kılavuza sürükleyin. Ardından ek bilgi sağlamak için öğe özelliklerini kullanın.
- **Sağ**: herhangi bir yere kılavuzda sağ tıklayın ve açılan menüden öğeleri seçin. Seçtiğiniz öğe genel bir gösterimini ekranında görüntülenir.

### <a name="connect-elements"></a>Öğeleri Bağlan

Öğeleri iki yolla bağlanabilir:

- **Sürükleme ve bırakma**: İstenen veri akışı kılavuza sürükleyin ve uygun öğeleri için her iki ucuna bağlayın.
- **Shift + tıklayın**: (veri gönderme) ilk öğesini tıklatın, tuşuna basın ve Shift tuşunu basılı tutun ve (veri alma) ikinci öğeyi seçin. Sağ tıklatın ve seçin **Bağlan**. İki yönlü veri akışı kullanırsanız, sırası gibi önemli değildir.

### <a name="properties"></a>Özellikler

 Şablonlarda select şablon ve bilgiler değiştirilebilir özelliklerini görmek için uygun şekilde doldurur. Aşağıdaki örnek, önce ve sonra gösterir bir **veritabanı** şablon diyagram üzerine sürüklediğiniz:

#### <a name="before"></a>Önce

![Önce](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a>Sonra

![Sonra](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a>İletiler

Bir tehdit modeli oluşturmak ve veri akışları öğelere bağlanmak unutursanız, bir bildirim alırsınız. Bu iletiyi yoksayabilirsiniz veya sorunu düzeltmek için yönergeleri izleyin. 

![İletiler](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a>Notlar

Notlar, diyagrama eklemek için geçmenize **iletileri** için sekme **notları** sekmesi.

## <a name="analysis-view"></a>Analiz görünümü

Diyagram oluşturma sonra seçin **analiz** geçmek için kısayolları araç çubuğunda simge (Büyüteç) **analiz** görünümü.

![Analiz görünümü](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a>Oluşturulan tehdit seçimi

Bir tehdit seçtiğinizde, üç ayrı işlev kullanabilirsiniz:

| Özellik                               | Bilgi      |
| --------------------------------------- | ------------ |
| **Okuma göstergesi** | <p>Tehdit gözden öğeleri izlemenize yardımcı olan okundu olarak işaretlenir.</p><p>![Okuma/okunmamış göstergesi](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| **Etkileşim odak** | <p>Etkileşim için bir tehdit ait diyagramda vurgulanır.</p><p>![Etkileşim odak](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| **İş parçacığı özellikleri** | <p>Tehdit hakkında ek bilgi görünür **tehdit özellikleri** penceresi.</p><p>![İş parçacığı özellikleri](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a>Öncelik değiştirme

Oluşturulan her tehdit öncelik düzeyini değiştirebilirsiniz. Farklı renk yüksek, Orta ve düşük öncelikli tehditleri tanımlamak kolaylaştırır.

![Öncelik değiştirme](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Tehdit özellikleri düzenlenebilir alanları

Önceki görüntüde görüldüğü gibi aracı tarafından oluşturulan bilgileri değiştirebilirsiniz. Düzeltme gibi bazı alanları bilgileri ekleyebilirsiniz. Bu alanların şablon tarafından üretilir. Her tehdit için daha fazla bilgiye ihtiyacınız varsa, değişiklikler yapabilirsiniz.

![İş parçacığı özellikleri](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a>Reports

Önceliklerini değiştirmek ve oluşturulan her tehdit durumunu güncelleştirme tamamladıktan sonra siz dosyayı kaydedin veya bir raporu yazdırın. Git **rapor** > **tam rapor oluşturma**. Rapor adı ve aşağıdaki görüntüye benzer bir şey görmeniz gerekir:

![Rapor](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bir şablon için topluluğa katkıda bulunmak için Git bizim [GitHub](https://github.com/Microsoft/threat-modeling-templates) sayfası. 
* Aracı ile çalışmaya başlamak için Git [karşıdan](https://aka.ms/tmtpreview) sayfası.
