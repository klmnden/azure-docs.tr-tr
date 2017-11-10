---
title: "Pano, İzleyici, Ölçek, yapılandırmak ve karma bağlantılar BizTalk Services | Microsoft Docs"
description: "Denetimleri hakkında bilgi edinin ve BizTalk Services için performans izleme"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 351809cd5f165a863dc02bfadf78fa59cbaabfd7
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Pano, İzleme, Ölçeklendirme, Yapılandırma ve Karma Bağlantı sekmelerini inceleyin

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

BizTalk hizmeti oluşturma ve uygulamanızı dağıttıktan sonra bazı BizTalk hizmeti ayarlarını değiştirin ve uygulama performansı izleme. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Aşağıdaki sekmeleri içeren yeni bir pencere açılır. Bu konuda aşağıdaki sekmelerden açıklanmaktadır.

## <a name="quickstart-quickstartquickstart"></a>Hızlı Başlangıç)![Hızlı Başlangıç][Quickstart])
BizTalk Services sürümüne göre listelenen tüm seçenekleri kullanılamayabilir. 

<table border="1">
    <tr>
        <td><strong>Araçları edinin</strong></td>
        <td>Şirket içi geliştirme bilgisayarınızda Visual Studio Proje şablonları yüklemek için BizTalk Services SDK'sını indirin. Bu şablonlar oluşturma <strong>BizTalk Services</strong> (köprü) ve <strong>BizTalk hizmeti yapıları</strong> BizTalk hizmetinize dağıtılan (dönüştürme) Visual Studio projeleri.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Ne başlamalıyım Azure BizTalk Services SDK'sını kullanarak </a> ve <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Azure BizTalk Services SDK'sını yükleme</a> başlamak için adımları listeler.
        </td>
    </tr>
    <tr>
        <td><strong>İş ortağı anlaşmaları oluşturma</strong></td>
        <td>Azure BizTalk Services burada ortakları ekleme ve X12, AS2, oluşturma Azure üzerinde barındırılan portalı açar ve EDIFACT EDI sözleşmeleri.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">EDI ileti BizTalk Services Portal bileşenlerini yapılandırma</a> başlamak için adımları listeler.
        </td>
    </tr>

<tr>
        <td><strong>BizTalk hizmetleri hakkında daha fazla bilgi edinin</strong></td>
        <td>Git <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">center öğrenme</a> Azure BizTalk Services hakkında daha fazla bilgi edinmek için.</td>
</tr>
</table>


Altındaki görev çubuğunda şunları yapabilirsiniz:

<table border="1">

<tr>
<td><strong>Yönetme</strong> , uygulama dağıtımı</td>
<td>Azure BizTalk Services Portalı'nı açar. BizTalk Services ortakları ekleme ve X12, AS2, oluşturma dahil olmak üzere EDI yapılandırma girişinin portalıdır ve EDIFACT sözleşmelerini.
<br/><br/>
Bu aynı sonucu verir <strong>ortak sözleşmeleri oluşturmak</strong> üzerinde <strong>Hızlı Başlangıç</strong> sekmesi.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">EDI ileti BizTalk Services Portal bileşenlerini yapılandırma</a> BizTalk Services portalı üzerinde daha fazla bilgi sağlar.</td>
</tr>

<tr>
<td><strong>Bağlantı bilgilerini</strong> , erişim denetimi Namespace</td>
<td>Bağlantı bilgilerini seçtiğinizde, erişim denetimi Namespace, varsayılan veren ve varsayılan anahtar görüntülenir. Bu değerleri kopyalayabilirsiniz.
<br/><br/>
Erişim denetimi portalı da açabilirsiniz. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Erişim denetimi Namespace oluşturmak</a> erişim denetimi portalı üzerinde daha fazla bilgi sağlar.</td>
</tr>

<tr>
<td><strong>Eşitleme anahtarları</strong> depolama hesabındaki</td>
<td>Storage hesabı oluşturduğunuzda, Birincil ve İkincil Anahtar da otomatik olarak oluşur. Bu şifreleme anahtarları, depolama hesabınıza erişimi denetler. BizTalk hizmeti otomatik olarak birincil anahtarı kullanır. <strong>Eşitleme anahtarları</strong> BizTalk hizmeti kesintiye uğratmadan birincil anahtar ve ikincil anahtar arasında geçiş yapma olanağı verir.
<br/><br/>
Örneğin, depolama hesabı için yeni bir birincil anahtar kullanmak için BizTalk hizmeti istiyorsunuz. Bunu yapmak için:
<br/><br/>
<ol>
<li>BizTalk hizmetinizi seçip <strong>anahtarları Eşitle</strong>. İkincil anahtar seçin. Bunu yaptığınızda, ikincil anahtarı kullanarak BizTalk hizmetini başlatır.</li>
<li>Depolama hesabınızı seçin ve birincil anahtarını yeniden oluşturma. BizTalk hizmetinizi ikincil anahtarı kullanarak unutmayın.</li>
<li>BizTalk hizmetinizi seçip <strong>anahtarları Eşitle</strong>. Şimdi, birincil anahtar seçin. Bu yeni birincil, yeniden anahtarıdır.</li>
<li>Depolama hesabınızı seçin ve ikincil anahtar yeniden.</li>
</ol>
<br/>
Bu işlem, "rollover anahtarları" adı verilir. Amacı, BizTalk hizmeti kesintiye uğratmadan birincil anahtar ve ikincil anahtar arasında geçiş yapmak kullanıcıların sağlamaktır.</td>
</tr>

<tr>
<td><strong>Silme</strong> uygulamanız</td>
<td>Seçtiğinizde silin, BizTalk hizmeti ve dağıtılmış tüm öğeler kaldırılır.</td>
</tr>
</table>


## <a name="dashboard"></a>Pano
BizTalk Services sürümüne göre listelenen tüm seçenekleri kullanılamayabilir. 

BizTalk hizmet adınızı seçin, Pano sekmesi görüntülenir. Panosunda, şunları yapabilirsiniz:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Kullanıma genel bakış: kullanılan karma bağlantı sayısını gösterir
Ayrıca veri kullanımı GB cinsinden görüntüler. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Ölçüm grafiği: performans ölçümleri sabit bir listesini gösterir
Bu ölçümler BizTalk hizmeti ile ilgili gerçek zamanlı değerleri sağlayın. Ayrıca seçebilirsiniz **göreli** veya **mutlak** değerleri ve zaman aralığını **aralığı** grafikte görüntülenen ölçümleri. 

Bu performans ölçümlerini bir açıklaması için Git [kullanılabilir ölçümler](#Metrics) bu konuda.

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Hızlı Bakış: BizTalk hizmeti özelliklerinizi listeler
<table border="1">

<tr>
<td><strong>İzleme veritabanı kimlik bilgileri güncelleştir</strong></td>
<td>Kullanıcı adı ve izleme veritabanına oturum açmak için kullandığınız parolayı değiştirir.</td>
</tr>
<tr>
<td><strong>SSL sertifikasını güncelleştir</strong></td>
<td>BizTalk hizmetini farklı bir SSL sertifikası kullanacak şekilde güncelleştirebilirsiniz. Otomatik olarak imzalanan bir SSL sertifikası otomatik olarak oluşturulur, <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">BizTalk hizmeti oluşturma</a>.</td>
</tr>
<tr>
<td><strong>Sertifikayı indirin</strong></td>
<td>Yerel makineye BizTalk hizmeti tarafından kullanılan SSL sertifikası yükleyebilirsiniz.</td>
</tr>
<tr>
<td><strong>Durumu</strong></td>
<td>BizTalk hizmeti geçerli durumunu görüntüler. Bkz: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Hizmet durumu grafiği</a>. </td>
</tr>
<tr>
<td><strong>Hizmet URL'si</strong></td>
<td>BizTalk hizmeti için URL. Bu aynı sonucu verir <strong>etki alanı URL'si</strong> BizTalk hizmeti oluşturulduğunda girildi.</td>
</tr>
<tr>
<td><strong>Genel sanal IP (VIP) adresi</strong></td>
<td>BizTalk Hizmetinize atanmış IP adresi. Tüm giriş uç noktaları için kullanılır ve kaynak adres giden trafik için. Oluşturulan sürece bu IP adresi, BizTalk hizmetinize aittir. BizTalk hizmeti silerseniz, IP adresi için başka bir BizTalk hizmeti atanır.</td>
</tr>
<tr>
<td><strong>ACS Namespace</strong></td>
<td>BizTalk hizmeti ile kimlik doğrulamasını yapar.</td>
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
<td>BizTalk hizmeti tarafından kullanılan izleme tablosu depolar Azure SQL veritabanı adı. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Gereksinimleri Explained</a> izleme veritabanında ayrıntıları sağlar.</td>
</tr>
<tr>
<td><strong>Depolama izleme/arşivleme</strong></td>
<td>BizTalk hizmeti izleme çıktısını depolar Azure depolama hesabı adı.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Gereksinimleri Explained</a> depolama hesabında ayrıntıları sağlar.</td>
</tr>
<tr>
<td><strong>Abonelik adı</strong></td>
<td>BizTalk hizmetinizi barındıran abonelik listeler. Abonelik erişimi yönetir.</td>
</tr>
<tr>
<td><strong>Abonelik kimliği</strong></td>
<td>Bir abonelik kimliği, bir abonelik oluşturduğunuzda, otomatik olarak oluşturulur. REST API kullanırken, abonelik kimliği girmeniz gerekebilir</td>
</tr>
</table>

[BizTalk Services: Sağlama](http://go.microsoft.com/fwlink/p/?LinkID=302280) BizTalk hizmeti oluşturma adımlarını listeler.

##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>, Bağlantı bilgilerini, eşitleme anahtarları, yönetin ve görev çubuğunda silin:
<table border="1">

<tr>
<td><strong>Yönetme</strong> , uygulama dağıtımı</td>
<td>Azure BizTalk Services Portalı'nı açar. BizTalk Services ortakları ekleme ve X12, AS2, oluşturma dahil olmak üzere EDI yapılandırma girişinin portalıdır ve EDIFACT sözleşmelerini.
<br/><br/>
Bu aynı sonucu verir <strong>ortak sözleşmeleri oluşturmak</strong> üzerinde <strong>Hızlı Başlangıç</strong> sekmesi.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">EDI ileti BizTalk Services Portal bileşenlerini yapılandırma</a> BizTalk Services portalı üzerinde daha fazla bilgi sağlar.</td>
</tr>
<tr>
<td><strong>Bağlantı bilgilerini</strong> , erişim denetimi Namespace</td>
<td>Erişim denetimi Namespace, varsayılan veren ve varsayılan anahtar değerleri görüntüler; hangi kopyalanabilir.
<br/><br/>
Erişim denetimi portalı da açabilirsiniz. Bu erişim denetimi portalı sol gezinti bölmesinde Active Directory seçeneğini kullanarak aynıdır.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Bilgisayarınızı ACS Namespace yönetme</a> erişim denetimi portalı üzerinde daha fazla bilgi sağlar.</td>
</tr>
<tr>
<td><strong>Eşitleme anahtarları</strong> depolama hesabındaki</td>
<td>Storage hesabı oluşturduğunuzda, Birincil ve İkincil Anahtar da otomatik olarak oluşur. Bu şifreleme anahtarları, depolama hesabınıza erişimi denetler. BizTalk hizmeti otomatik olarak birincil anahtarı kullanır. <strong>Eşitleme anahtarları</strong> BizTalk hizmeti kesintiye uğratmadan birincil anahtar ve ikincil anahtar arasında geçiş yapma olanağı verir.
<br/><br/>
Örneğin, depolama hesabı için yeni bir birincil anahtar kullanmak için BizTalk hizmeti istiyorsunuz. Bunu yapmak için:
<br/><br/>
<ol>
<li>BizTalk hizmetinizi seçip <strong>anahtarları Eşitle</strong>. İkincil anahtar seçin. Bunu yaptığınızda, ikincil anahtarı kullanarak BizTalk hizmetini başlatır.</li>
<li>Depolama hesabınızı seçin ve birincil anahtarını yeniden oluşturma. BizTalk hizmetinizi ikincil anahtarı kullanarak unutmayın.</li>
<li>BizTalk hizmetinizi seçip <strong>anahtarları Eşitle</strong>. Şimdi, birincil anahtar seçin. Bu yeni birincil, yeniden anahtarıdır.</li>
<li>Depolama hesabınızı seçin ve ikincil anahtar yeniden.</li>
</ol>
<br/>
Bu işlem, "rollover anahtarları" adı verilir. Amacı, BizTalk hizmeti kesintiye uğratmadan birincil anahtar ve ikincil anahtar arasında geçiş yapmak kullanıcıların sağlamaktır.</td>
</tr>

<tr>
<td><strong>Silme</strong> uygulamanız</td>
<td>BizTalk hizmeti ve dağıtılmış tüm öğeler kaldırıldı.</td>
</tr>
</table>


## <a name="monitor"></a>İzleme
Ücretsiz sürüm için geçerli değildir.

BizTalk hizmet adınızı seçtiğinizde, İzleyici sekmesi kullanılabilir ve şunları görüntüler:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Ölçüm grafiği: Seçili performans ölçümleri görüntüler
Bu ölçümler BizTalk hizmeti ile ilgili gerçek zamanlı değerleri sağlayın. Hangi performans ölçümleri görüntülenen seçin. En fazla altı performans ölçümleri eşzamanlı olarak görüntülenebilir. 

Ayrıca seçebilirsiniz **göreli** veya **mutlak** değerleri ve zaman aralığını **aralığı** görüntülenen ölçümleri. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Grafikte ölçümleri görüntülemek veya kaldırmak için:
1. Seçin **İzleyici** sekmesi.
2. Seçin **ölçüm Ekle** görev çubuğunda:  
   ![Select ölçüm Ekle][AddMetrics]
3. Görüntülemek istediğiniz performans ölçümleri denetleyin.
4. Geri dönmek için onay işaretini seçin **İzleyici** sekmesi.
5. Bu ölçüm ait değer grafikte görüntülenecek ölçüm yanındaki daire seçin.  
   
    Örneğin, **CPU kullanımı** çıkışı ölçüm gri; çıktısını grafikte gösterilmez:  
   ![CPU kullanım ölçüm gri][GrayedMetric]  
   
    Gri etkinleştirmek için büyüyen daire seçin **CPU kullanımı** ölçüm grafikte çıktısını görüntülemek için:  
   ![CPU kullanım ölçüm etkin][EnabledMetric]
6. Görüntü grafik hem de listesinden bir ölçüm kaldırmak için seçin **silmek ölçüm** görev çubuğunda. Ölçüm geri listesine eklemek için seçin **ölçüm Ekle** görev çubuğunda ölçüm denetleyin ve geri dönmek için onay işaretini seçin **İzleyici** sekmesi. Gri ölçüm etkinleştirmek için büyüyen daire seçin.

## <a name="Metrics"></a>Kullanılabilir ölçümler
Aşağıdaki performans sayaçları/ölçümleri kullanılabilir:

<table border="1">

<tr>
<td><strong>RountdTrip gecikme süresi</strong></td>
<td>Milisaniye (ms) iletiyi tamamen BizTalk hizmeti tarafından işlenen kadar ileti alındığında bir ileti zamandan işlenmesi için geçen ortalama süre tüm köprüleri görüntüler. Başarıyla işlenen iletiler kabul edilir.<br/><br/>
Aşağıdaki olaylar gerçekleştiğinde, zaman damgası oluşturulur:
<ul>
<li>Ağ geçidi ileti girer</li>
<li>İleti hedef yönlendirilir</li>
<li>Hedef yanıtı aldı</li>
<li>Ağ geçidine gönderilen hedef onay yanıtı</li>
</ul>
<br/>
Bu ölçüm, aşağıdaki hesaplamanın sonucu gösterir:
<br/><br/>
[Hedef onay yanıtı ağ geçidine gönderilen] - [ileti girer ağ geçidi]</td>
</tr>
<tr>
<td><strong>Kaynaktaki hataları</strong></td>
<td>Başarısız iletilerin toplam sayısını görüntüler BizTalk kaynak uç noktaları iletilerden çekme zaman hizmeti tarafından.</td>
</tr>
<tr>
<td><strong>CPU kullanımı</strong></td>
<td>Tüm rol örneklerinin ortalama % işlemci zamanı listeler.</td>
</tr>
<tr>
<td><strong>İşleme gecikmesi</strong></td>
<td>Varış yeri için harcanan zaman hariç tüm köprüleri arasında milisaniye (ms) BizTalk hizmeti tarafından bir iletiyi işlemek için geçen ortalama süre görüntüler. Başarıyla işlenen iletiler kabul edilir.<br/><br/>
Her aşağıdaki olaylardan biri gerçekleştiğinde bir zaman damgası oluşturulur:

<ul>
<li>Ağ geçidi ileti girer</li>
<li>İleti hedef yönlendirilir</li>
<li>Hedef yanıtı aldı</li>
<li>Ağ geçidine gönderilen hedef onay yanıtı</li>
</ul>
<br/>Bu ölçüm, aşağıdaki hesaplamanın sonucu gösterir:<br/><br/>
[Hedef onay yanıtı ağ geçidine gönderilen] - [ileti girer ağ geçidi] - [hedef yanıt alındığında] + [hedefe ileti yönlendirilmiş]</td>
</tr>
<tr>
<td><strong>İşlemdeki hataları</strong></td>
<td>Tüm köprüleri BizTalk hizmeti tarafından işlenmesi sırasında bir zaman aralığı içinde başarısız oldu iletilerin toplam sayısını görüntüler.</td>
</tr>
<tr>
<td><strong>Gönderilen iletileri</strong></td>
<td>BizTalk hizmeti tarafından bir zaman aralığı içinde tüm köprüleri gönderilen iletilerin toplam sayısını görüntüler. Bu ölçüm, ardışık düzen tarafından gönderilen bir ileti rota hedef ulaştığında artırılır. Bu ölçüm, bir ileti başarıyla işlendi göstermez.<br/><br/>
Rota hedef ardışık düzene alınmasını alındısı gönderdiğinde bir istek-yanıt senaryosunda, ölçüm artırılır.</td>
</tr>
<tr>
<td><strong>Alınan iletileri</strong></td>
<td>Bir zaman aralığı içinde tüm köprüleri BizTalk hizmeti tarafından alınan iletilerin toplam sayısını görüntüler. Bu ölçüm ardışık düzen tarafından yeni bir ileti alındığında artırılır.</td>
</tr>
<tr>
<td><strong>İşlemindeki iletiler</strong></td>
<td>BizTalk hizmeti tarafından bir zaman aralığı içinde işlenmekte olan iletilerin toplam sayısını görüntüler.</td>
</tr>
<tr>
<td><strong>İşlenen iletileri</strong></td>
<td>BizTalk hizmeti tarafından tüm köprüleri arasında bir zaman aralığı içinde başarıyla işlenen iletilerin toplam sayısını görüntüler. Bu ölçüm, bir ileti ardışık düzen tarafından başarıyla alındı ve başarıyla hedefe yönlendirilen olduğunda artırılır.</td>
</tr>
</table>


## <a name="scale"></a>Ölçek
Ölçek sekmesini ekleyin ya da BizTalk hizmeti tarafından kullanılan birim sayısını çıkarın. Varsayılan olarak, yoktur birim yapılandırılmış bir. Ek birimler, BizTalk hizmeti ölçeklendirmek için eklenebilir. Ölçek artırdığınızda, üretilen iş artmaktadır. Kaynaklar ayrıca, işlemci gücü ve dağıtılan köprüleri, anlaşmalar, LOB bağlantıları dahil olmak üzere artar. Örneğin, 1 birim ölçekte 2 birimlerine artırın. Bu durumda, çift köprü sayısı dağıtmak, anlaşmaları çift, LOB bağlantıları çift ve işlemci gücü çift.

Bazı BizTalk sürümleri Ölçek seçeneği sağlamaz. Bu durumda, tek bir birim izin verilir. Sürümünüz genişletilmiş birim sayısını belirlemek için başvurmak [BizTalk Services: sürümler grafiği](biztalk-editions-feature-chart.md).

Birim sayısını artırmayı fiyatlandırmayı etkileyebilir. Birimleri artırırsanız, seçme **kaydetmek** faturalama etkilenen olmadığını bildiren bir ileti görüntüler. Ardından devam etmek seçin. Zaman birimleri, güncelleştirme Etkin'den BizTalk hizmeti durumu değişiklikleri sayısını artırın. Güncelleştirme durumu, BizTalk hizmeti çalışmaya devam eder.

[BizTalk Services: Sürümler grafiği](biztalk-editions-feature-chart.md) "Birim" tanımlar.

## <a name="configure"></a>Yapılandırma
Karma bağlantılar için geçerli değildir.

Yedekleme durumu Yok'a ayarlar veya otomatik. None olarak ayarlandığında, yedekleme otomatik olarak oluşturulur. Otomatik olarak ayarlandığında, yedek dosyaları saklamak için sıklığı, yedekleme ve ne kadar süre yedeklemeyi yapılandırın. 

[BizTalk Services: Yedekleme ve geri yükleme](biztalk-backup-restore.md) ait ayrıntıları sağlar. 

## <a name="HybridConnections"></a>Karma bağlantılar
Karma bağlantılar Azure uygulaması, Web uygulamaları veya Azure App Service'de Mobile Apps gibi statik TCP bağlantı noktası, SQL Server, MySQL, HTTP Web API'leri ve birçok özel Web hizmeti gibi kullanan bir şirket içi kaynağa bağlayın. Karma bağlantılar, BizTalk Services'da yönetilir.

Oluşturmak veya karma bağlantılar Azure BizTalk Services yönetmek için bkz: [karma bağlantılar](integration-hybrid-connection-overview.md).

## <a name="next"></a>Sonraki
Farklı sekmelerle tanıdık, Azure BizTalk Services özellikleri hakkında daha fazla bilgi edinebilirsiniz:

* [BizTalk Services: Azaltma](biztalk-throttling-thresholds.md)  
* [BizTalk Services: Verenin Adı ve Verenin Anahtarı](biztalk-issuer-name-issuer-key.md)  
* [BizTalk Services: Yedekleme ve Geri Yükleme](biztalk-backup-restore.md)

## <a name="see-also"></a>Ayrıca Bkz.
* [Karma Bağlantılar](integration-hybrid-connection-overview.md)  
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](biztalk-editions-feature-chart.md)  
* [BizTalk Services: sağlama](biztalk-provision-services.md)  
* [BizTalk Services: BizTalk hizmeti durumu grafiği](biztalk-service-state-chart.md)  
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

