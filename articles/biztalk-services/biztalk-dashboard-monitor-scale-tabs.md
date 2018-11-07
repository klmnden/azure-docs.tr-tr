---
title: Pano, İzleyici, Ölçek, yapılandırma ve BizTalk Services karma bağlantılar | Microsoft Docs
description: Denetimleri hakkında bilgi edinin ve BizTalk Hizmetleri performansını izleme
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 3f4763b5e15d4b9b84e868262a9e8538b8a407a2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228836"
---
# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Pano, İzleme, Ölçeklendirme, Yapılandırma ve Karma Bağlantı sekmelerini inceleyin

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

BizTalk hizmeti oluşturma ve uygulamanızı dağıtmak sonra bazı BizTalk hizmeti ayarlarını değiştirin ve uygulama performansını izleme. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Bu, aşağıdaki sekmeleri içeren yeni bir pencere açılır. Bu konuda aşağıdaki sekmelerden açıklanmaktadır.

## <a name="quickstart-quickstartquickstart"></a>Hızlı Başlangıç)![Hızlı Başlangıç][Quickstart])
BizTalk Hizmetleri sürüme bağlı olarak listelenen tüm seçenekleri kullanılamayabilir. 

<table border="1">
    <tr>
        <td><strong>Araçları edinin</strong></td>
        <td>Visual Studio Proje şablonları, şirket içi geliştirme bilgisayarınıza yüklemek için BizTalk Hizmetleri SDK'sını indirin. Bu şablonlar oluşturma <strong>BizTalk Hizmetleri</strong> (köprü) ve <strong>BizTalk hizmeti yapıları</strong> BizTalk hizmetinize dağıtılır (dönüştürme) Visual Studio projeleri.
        <br/><br/>
        <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302335"> Ne başlarım Azure BizTalk Hizmetleri SDK'sını kullanarak </a> ve <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=241589">Azure BizTalk Hizmetleri SDK'sını yükleme</a> başlama adımları listelenir.
        </td>
    </tr>
    <tr>
        <td><strong>İş ortağı sözleşmeleri oluşturma</strong></td>
        <td>Burada iş ortakları ekleme ve oluşturma X12, AS2, Azure'da barındırılan Azure BizTalk Hizmetleri portalını açar ve EDIFACT EDI sözleşmesini.
        <br/><br/>
        <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=303653">EDI ileti BizTalk Hizmetleri portalında bileşenlerini yapılandırma</a> başlama adımları listelenir.
        </td>
    </tr>

<tr>
        <td><strong>BizTalk hizmetleri hakkında daha fazla bilgi edinin</strong></td>
        <td>Git <a HREF="https://azure.microsoft.com/documentation/services/biztalk-services/">merkezi öğrenme</a> Azure BizTalk hizmetleri hakkında daha fazla bilgi için.</td>
</tr>
</table>


Görev çubuğunda, alttaki şunları yapabilirsiniz:

<table border="1">

<tr>
<td><strong>Yönetme</strong> uygulama dağıtımınızı</td>
<td>Azure BizTalk Hizmetleri portalını açar. BizTalk Hizmetleri portalını girişinin EDI yapılandırma, iş ortakları ekleme ve X12, AS2, oluşturma dahil olan ve EDIFACT sözleşmelerini.
<br/><br/>
Bu, aynı <strong>ortak sözleşmeleri oluşturma</strong> üzerinde <strong>Hızlı Başlangıç</strong> sekmesi.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=303653">EDI ileti BizTalk Hizmetleri portalında bileşenlerini yapılandırma</a> BizTalk Hizmetleri portalında daha fazla bilgi sağlar.</td>
</tr>

<tr>
<td><strong>Bağlantı bilgilerini</strong> erişim denetimi Namespace,</td>
<td>Bağlantı bilgilerini seçme, erişim denetimi Namespace, varsayılan veren ve varsayılan anahtar görüntülenir. Bu değerleri kopyalayabilirsiniz.
<br/><br/>
Erişim denetimi portalı da açabilirsiniz. <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=285670">Bir erişim denetimi Namespace</a> erişim denetimi Portalı hakkında daha fazla bilgi sağlar.</td>
</tr>

<tr>
<td><strong>Anahtarları Eşitle</strong> depolama hesabındaki</td>
<td>Storage hesabı oluşturduğunuzda, Birincil ve İkincil Anahtar da otomatik olarak oluşur. Bu şifreleme anahtarları, depolama hesabınıza erişimi denetler. BizTalk hizmetiniz otomatik olarak birincil anahtarı kullanır. <strong>Anahtarları Eşitle</strong> BizTalk hizmeti kesintiye uğratmadan birincil anahtar ve ikincil anahtar arasında geçiş yapmak kullanıcıları etkinleştirin.
<br/><br/>
Örneğin, BizTalk hizmeti, depolama hesabı için yeni bir birincil anahtar kullanmak istersiniz. Bunu yapmak için:
<br/><br/>
<ol>
<li>BizTalk hizmetinizi seçip <strong>anahtarları Eşitle</strong>. İkincil anahtar seçin. Bunu yaptığınızda ikincil anahtarını kullanarak BizTalk hizmeti başlatır.</li>
<li>Depolama hesabınızı seçin ve birincil anahtarı yeniden oluştur. BizTalk hizmetiniz, ikincil bir anahtar kullanarak unutmayın.</li>
<li>BizTalk hizmetinizi seçip <strong>anahtarları Eşitle</strong>. Şimdi, birincil anahtarı seçin. Yeni birincil, yeniden anahtar budur.</li>
<li>Depolama hesabınızı seçin ve ikincil anahtarı yeniden oluştur.</li>
</ol>
<br/>
Bu işlem, "anahtarları geçir" adı verilir. BizTalk hizmeti kesintiye uğratmadan birincil anahtar ve ikincil anahtar arasında geçiş yapmak kullanıcıların amacı etkinleştirmektir.</td>
</tr>

<tr>
<td><strong>Silme</strong> uygulamanız</td>
<td>Seçeneğini belirlediğinizde silin, BizTalk hizmeti ve ona dağıtılan tüm öğeler kaldırılır.</td>
</tr>
</table>


## <a name="dashboard"></a>Pano
BizTalk Hizmetleri sürüme bağlı olarak listelenen tüm seçenekleri kullanılamayabilir. 

BizTalk hizmeti adınızı seçin, Pano sekmesi görüntülenir. Panoda şunları yapabilirsiniz:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Kullanıma genel bakış: kullanılan karma bağlantı sayısını gösterir.
Ayrıca veri kullanımı, GB cinsinden görüntüler. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Ölçüm grafiği: performans ölçümlerini sabit bir listesini gösterir.
Bu ölçümler, BizTalk hizmeti ile ilgili gerçek zamanlı değerleri sağlayın. Ayrıca seçebilirsiniz **göreli** veya **mutlak** değerleri ve zaman aralığını **aralığı** grafikte görüntülenen ölçümlerin. 

Bu performans ölçümlerini açıklaması için Git [kullanılabilir ölçümler](#Metrics) bu konuda.

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Hızlı Bakış: BizTalk hizmeti özelliklerinizi listeler
<table border="1">

<tr>
<td><strong>İzleme veritabanı kimlik bilgilerini güncelleştirme</strong></td>
<td>Kullanıcı adı ve izleme veritabanına oturum açmak için kullandığınız parolayı değiştirir.</td>
</tr>
<tr>
<td><strong>SSL sertifikasını güncelleştirme</strong></td>
<td>BizTalk hizmeti farklı bir SSL sertifikası kullanmak üzere güncelleştirebilirsiniz. Otomatik olarak imzalanan bir SSL sertifikası otomatik olarak oluşturulur, <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302280">BizTalk hizmeti oluşturma</a>.</td>
</tr>
<tr>
<td><strong>Sertifikayı indirin</strong></td>
<td>Bir yerel makineye BizTalk hizmeti tarafından kullanılan SSL sertifikasını indirebilirsiniz.</td>
</tr>
<tr>
<td><strong>Durum</strong></td>
<td>BizTalk hizmeti geçerli durumunu görüntüler. Bkz: <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Hizmet durumu grafiği</a>. </td>
</tr>
<tr>
<td><strong>Hizmet URL'si</strong></td>
<td>BizTalk hizmeti için URL. Bu, aynı <strong>etki alanı URL'si</strong> BizTalk hizmeti oluşturulduğunda girildi.</td>
</tr>
<tr>
<td><strong>Genel sanal IP (VIP) adresi</strong></td>
<td>BizTalk hizmetiniz için atanan IP adresi. Tüm giriş uç noktaları için kullanılır ve giden trafik için kaynak adresi. Oluşturulduğu sürece bu IP adresi, BizTalk hizmetinize ait. BizTalk hizmeti silerseniz, başka bir BizTalk hizmeti için IP adresi atanır.</td>
</tr>
<tr>
<td><strong>ACS Namespace</strong></td>
<td>BizTalk hizmeti ile kimliğini doğrular.</td>
</tr>
<tr>
<td><strong>Sürüm</strong></td>
<td>BizTalk hizmeti oluşturulduğunda listeleri Edition girdi.</td>
</tr>
<tr>
<td><strong>Konum</strong></td>
<td>BizTalk hizmetinizi barındıran coğrafi bölgeyi görüntüler.</td>
</tr>
<tr>
<td><strong>Oluşturulan</strong></td>
<td>BizTalk hizmeti oluşturulduğu saat ve tarihi görüntüler.</td>
</tr>
<tr>
<td><strong>Veritabanı izleme</strong></td>
<td>BizTalk hizmeti tarafından kullanılan izleme tablosu depolayan Azure SQL veritabanı adı. 
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302280">Gereksinimleri Explained</a> izleme veritabanında ayrıntıları sağlar.</td>
</tr>
<tr>
<td><strong>Depolama izleme/arşivleme</strong></td>
<td>BizTalk hizmeti izleme çıkışını depolayan Azure depolama hesabı adı.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302280">Gereksinimleri Explained</a> depolama hesabı ayrıntıları sağlar.</td>
</tr>
<tr>
<td><strong>Abonelik adı</strong></td>
<td>BizTalk hizmetinizi barındıran abonelik listeler. Abonelik erişimini yönetir.</td>
</tr>
<tr>
<td><strong>Abonelik kimliği</strong></td>
<td>Abonelik kimliği, bir abonelik oluşturulurken otomatik olarak oluşturulur. REST API'lerini kullanarak, abonelik kimliği girin gerekebilir</td>
</tr>
</table>

[BizTalk Services: Sağlama](https://go.microsoft.com/fwlink/p/?LinkID=302280) BizTalk hizmeti oluşturma adımları listelenir.

##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>, Bağlantı bilgilerini, anahtarları Eşitle, yönetmek ve görev çubuğunda silin:
<table border="1">

<tr>
<td><strong>Yönetme</strong> uygulama dağıtımınızı</td>
<td>Azure BizTalk Hizmetleri portalını açar. BizTalk Hizmetleri portalını girişinin EDI yapılandırma, iş ortakları ekleme ve X12, AS2, oluşturma dahil olan ve EDIFACT sözleşmelerini.
<br/><br/>
Bu, aynı <strong>ortak sözleşmeleri oluşturma</strong> üzerinde <strong>Hızlı Başlangıç</strong> sekmesi.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=303653">EDI ileti BizTalk Hizmetleri portalında bileşenlerini yapılandırma</a> BizTalk Hizmetleri portalında daha fazla bilgi sağlar.</td>
</tr>
<tr>
<td><strong>Bağlantı bilgilerini</strong> erişim denetimi Namespace,</td>
<td>Erişim denetimi Namespace, varsayılan veren ve varsayılan anahtar değerleri görüntüler. hangi kopyalanabilir.
<br/><br/>
Erişim denetimi portalı da açabilirsiniz. Bu erişim denetimi portalı sol gezinti bölmesinde Active Directory seçeneği kullanılarak aynıdır.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=285670">Bilgisayarınızı ACS Namespace yönetme</a> erişim denetimi Portalı hakkında daha fazla bilgi sağlar.</td>
</tr>
<tr>
<td><strong>Anahtarları Eşitle</strong> depolama hesabındaki</td>
<td>Storage hesabı oluşturduğunuzda, Birincil ve İkincil Anahtar da otomatik olarak oluşur. Bu şifreleme anahtarları, depolama hesabınıza erişimi denetler. BizTalk hizmetiniz otomatik olarak birincil anahtarı kullanır. <strong>Anahtarları Eşitle</strong> BizTalk hizmeti kesintiye uğratmadan birincil anahtar ve ikincil anahtar arasında geçiş yapmak kullanıcıları etkinleştirin.
<br/><br/>
Örneğin, BizTalk hizmeti, depolama hesabı için yeni bir birincil anahtar kullanmak istersiniz. Bunu yapmak için:
<br/><br/>
<ol>
<li>BizTalk hizmetinizi seçip <strong>anahtarları Eşitle</strong>. İkincil anahtar seçin. Bunu yaptığınızda ikincil anahtarını kullanarak BizTalk hizmeti başlatır.</li>
<li>Depolama hesabınızı seçin ve birincil anahtarı yeniden oluştur. BizTalk hizmetiniz, ikincil bir anahtar kullanarak unutmayın.</li>
<li>BizTalk hizmetinizi seçip <strong>anahtarları Eşitle</strong>. Şimdi, birincil anahtarı seçin. Yeni birincil, yeniden anahtar budur.</li>
<li>Depolama hesabınızı seçin ve ikincil anahtarı yeniden oluştur.</li>
</ol>
<br/>
Bu işlem, "anahtarları geçir" adı verilir. BizTalk hizmeti kesintiye uğratmadan birincil anahtar ve ikincil anahtar arasında geçiş yapmak kullanıcıların amacı etkinleştirmektir.</td>
</tr>

<tr>
<td><strong>Silme</strong> uygulamanız</td>
<td>BizTalk hizmetiniz ve ona dağıtılan tüm öğeler kaldırılır.</td>
</tr>
</table>


## <a name="monitor"></a>İzleme
Ücretsiz sürüm için geçerli değildir.

BizTalk hizmeti adınızı seçin, İzleyici sekmesi kullanılabilir ve şunları görüntüler:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Ölçüm grafiği: Seçili performans ölçümlerini görüntüler.
Bu ölçümler, BizTalk hizmeti ile ilgili gerçek zamanlı değerleri sağlayın. Seçtiğiniz hangi performans ölçümlerini görüntülenir. En fazla altı performans ölçümlerinin aynı anda görüntülenebilir. 

Ayrıca seçebilirsiniz **göreli** veya **mutlak** değerleri ve zaman aralığını **aralığı** görüntülenen ölçümlerin. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Grafikte ölçümleri görüntülemek veya kaldırmak için:
1. Seçin **İzleyici** sekmesi.
2. Seçin **ölçüm Ekle** görev çubuğunda:  
   ![Ölçümleri Ekle'yi seçin][AddMetrics]
3. Görüntülemek istediğiniz performans ölçümlerini denetleyin.
4. Geri dönmek için onay işaretini seçin **İzleyici** sekmesi.
5. Grafikte, ölçüm'ın değerini görüntülemek için ölçüm yanındaki daireye seçin.  
   
    Örneğin, **CPU kullanımı** ölçüm gri; çıktısını grafikte gösterilmez:  
   ![CPU kullanım ölçümü gri][GrayedMetric]  
   
    Gri etkinleştirmek için büyüyen daire seçin **CPU kullanımı** ölçüm grafikte çıktısını görüntülemek için:  
   ![CPU kullanım ölçüm etkin][EnabledMetric]
6. Görüntü grafiği ve listenin bir ölçüm kaldırmak için işaretleyin **Sil ölçüm** görev çubuğunda. Ölçüm arka listeye eklemek için seçin **ölçüm Ekle** görev çubuğunda, ölçüm denetleyin ve geri dönmek için onay işaretini seçin **İzleyici** sekmesi. Gri, büyüyen daire ölçüm etkinleştirmek için seçin.

## <a name="Metrics"></a>Kullanılabilir ölçümler
Aşağıdaki performans sayaçları/ölçümler kullanılabilir:

<table border="1">

<tr>
<td><strong>RountdTrip gecikme süresi</strong></td>
<td>Milisaniye (ms) kadar ileti tam olarak BizTalk hizmeti tarafından işlenen bir ileti alındığında bir ileti zamandan işlemek için geçen ortalama süre tüm köprüleri görüntüler. Yalnızca başarıyla işlenen iletilerin sayılır.<br/><br/>
Aşağıdaki olaylar gerçekleştiğinde bir zaman damgası oluşturulur:
<ul>
<li>İleti, ağ geçidi girer.</li>
<li>İleti hedefine yönlendirilir</li>
<li>Hedef yanıtı aldı</li>
<li>Ağ geçidi gönderilen hedef onay yanıtı</li>
</ul>
<br/>
Bu ölçüm hesaplama aşağıdaki sonucu gösterilmiştir:
<br/><br/>
[Hedef onay yanıtı ağ geçidine gönderilen] - [ileti girer ağ geçidi]</td>
</tr>
<tr>
<td><strong>Kaynak hataları</strong></td>
<td>Başarısız olan iletilerin toplam sayısını görüntüler kaynak uç noktalarından iletileri çekme sırasında BizTalk hizmeti tarafından.</td>
</tr>
<tr>
<td><strong>CPU kullanımı</strong></td>
<td>Tüm rol örneklerine ortalama % işlemci zamanı listeler.</td>
</tr>
<tr>
<td><strong>İşlem gecikme süresi</strong></td>
<td>BizTalk hizmeti tarafından milisaniye (ms) bir iletiyi işlemek için geçen ortalama süre varış yeri için harcanan süre hariç tüm köprüleri arasında görüntüler. Yalnızca başarıyla işlenen iletilerin sayılır.<br/><br/>
Her aşağıdaki olaylardan biri oluştuğunda, bir zaman damgası oluşturulur:

<ul>
<li>İleti, ağ geçidi girer.</li>
<li>İleti hedefine yönlendirilir</li>
<li>Hedef yanıtı aldı</li>
<li>Ağ geçidi gönderilen hedef onay yanıtı</li>
</ul>
<br/>Bu ölçüm hesaplama aşağıdaki sonucu gösterilmiştir:<br/><br/>
[Hedef onay yanıtı ağ geçidine gönderilen] - [ileti girer ağ geçidi] - [hedef yanıt alındığında] + [hedefe ileti yönlendirilen]</td>
</tr>
<tr>
<td><strong>İşlemdeki hataları</strong></td>
<td>BizTalk hizmeti tarafından işlenmesi sırasında tüm köprüleri bir zaman aralığı içinde başarısız iletileri toplam sayısını görüntüler.</td>
</tr>
<tr>
<td><strong>Gönderilen iletileri</strong></td>
<td>BizTalk hizmeti tarafından bir zaman aralığında tüm köprüleri arasında gönderilen iletilerin toplam sayısını görüntüler. Bu ölçüm, bir ardışık düzen tarafından gönderilen bir iletinin yol hedef ulaştığında artırılır. Bu ölçüm, bir ileti başarıyla işlenen göstermez.<br/><br/>
Yol hedef ardışık düzenine giriş bildirim gönderdiğinde istek-yanıt senaryosunda, ölçüm artırılır.</td>
</tr>
<tr>
<td><strong>Alınan iletiler</strong></td>
<td>BizTalk hizmeti tarafından bir zaman aralığı tüm köprüleri alınan iletilerin toplam sayısını görüntüler. Bu ölçüm, işlem hattı tarafından yeni bir ileti alındığında artırılır.</td>
</tr>
<tr>
<td><strong>İşlemindeki iletiler</strong></td>
<td>BizTalk hizmeti tarafından bir zaman aralığında işlenmekte olan iletilerin toplam sayısını görüntüler.</td>
</tr>
<tr>
<td><strong>İşlenen iletiler</strong></td>
<td>BizTalk hizmeti tarafından tüm köprüleri arasında bir zaman aralığı içinde başarıyla işlenen iletilerin toplam sayısını görüntüler. Bu ölçüm, bir ileti işlem hattı tarafından başarıyla alındı ve hedefine başarıyla yönlendirilen artırılır.</td>
</tr>
</table>


## <a name="scale"></a>Ölçek
' Ndaki Ölçek sekmesini, ekleme veya çıkarma, BizTalk hizmeti tarafından kullanılan birim sayısı. Varsayılan olarak yoktur birim yapılandırılmış bir. Ek birimler BizTalk hizmetinizi ölçeklendirin eklenebilir. Ölçeğini artırdığınızda, aktarım hızı artmaktadır. Kaynakların miktarını da, dağıtılan köprüleri, anlaşmalar, LOB bağlantılar dahil olmak üzere ve işleme gücü artırır. Örneğin, 2 birim 1 birimi kadar ölçeklendirme artırın. Bu durumda, çift köprü sayısı dağıtmak, anlaşmalarını çift, LOB bağlantıları çift ve işlem gücü çift.

Bazı BizTalk sürümleri Ölçek seçeneği sunmaz. Bu durumda, bir birim izin verilir. Sürümünüzü ölçeklenebilen birim sayısını belirlemek için başvurmak [BizTalk Services: sürümler grafiği](biztalk-editions-feature-chart.md).

Birimi sayısını artırabilir, fiyatlandırmayı etkileyebilir. Birimleri artırırsanız seçerek **Kaydet** faturalandırma etkilenip etkilenmediğini bildiren bir ileti görüntüler. Ardından devam etmek seçin. Zaman birimi, BizTalk hizmeti durumu değişiklikleri etkin güncelleştirme için sayısını artırın. Güncelleştirme durumda BizTalk Hizmetiniz çalışmaya devam eder.

[BizTalk Services: Sürümler grafiği](biztalk-editions-feature-chart.md) "Birim" tanımlar.

## <a name="configure"></a>Yapılandırma
Karma bağlantılar için geçerli değildir.

Yedekleme durumu yok ayarlar veya otomatik. None olarak ayarlandığında, yedekleme otomatik olarak oluşturulur. Otomatik olarak ayarlandığında, yedekleme dosyaları korumak için sıklığı ve ne kadar yedekleme, yedekleme konumunu yapılandırın. 

[BizTalk Services: Yedekleme ve geri yükleme](biztalk-backup-restore.md) ayrıntıları sağlar. 

## <a name="HybridConnections"></a>Karma bağlantılar
Karma bağlantılar, Web Apps veya Azure App service'taki Mobile Apps gibi Azure uygulaması, SQL Server, MySQL, HTTP Web API'leri ve çoğu özel Web hizmeti gibi statik TCP bağlantı kullanan bir şirket içi kaynağa bağlanın. Karma bağlantılar, BizTalk Services hizmetinde yönetilir.

Oluşturmak ve Azure BizTalk Services karma bağlantılar'ı yönetmek için bkz: [karma bağlantılar](integration-hybrid-connection-overview.md).

## <a name="next"></a>Sonraki
Artık farklı sekmelerde ile ilgili bilgi sahibi olduğunuza göre Azure BizTalk Services özellikleri hakkında daha fazla bilgi edinebilirsiniz:

* [BizTalk Services: Azaltma](biztalk-throttling-thresholds.md)  
* [BizTalk Services: Verenin Adı ve Verenin Anahtarı](biztalk-issuer-name-issuer-key.md)  
* [BizTalk Services: Yedekleme ve Geri Yükleme](biztalk-backup-restore.md)

## <a name="see-also"></a>Ayrıca Bkz.
* [Karma Bağlantılar](integration-hybrid-connection-overview.md)  
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](biztalk-editions-feature-chart.md)  
* [BizTalk Services: sağlama](biztalk-provision-services.md)  
* [BizTalk Services: BizTalk hizmet durumu grafiği](biztalk-service-state-chart.md)  
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](https://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

