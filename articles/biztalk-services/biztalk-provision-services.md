---
title: Azure portalında Azure BizTalk Services oluşturma | Microsoft Belgeleri
description: Azure portalında Azure BizTalk Services hazırlamayı veya oluşturmayı öğrenin; MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: f5ffd1a9d0e7ff515b0819bb678bf0263f53e0d2
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918782"
---
# <a name="create-biztalk-services-using-the-azure-portal"></a>Azure portalını kullanarak BizTalk Services oluşturma

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]
> 
> [!TIP]
> Azure portalında oturum açmak için bir Azure hesabınız ve Azure aboneliğiniz olması gerekir. Hesabınız yoksa birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Bkz. [Azure Ücretsiz Deneme](https://go.microsoft.com/fwlink/p/?LinkID=239738).


## <a name="CreateService"></a>BizTalk Hizmeti oluşturma

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

BizTalk Hizmeti durumunu bağlı olarak, bazı tamamlanamayan işlemler vardır. Bu işlemlerin bir listesi için [BizTalk Services Durumu Grafiği](biztalk-service-state-chart.md)’ne gidin.

## <a name="post-provisioning-steps"></a>Hazırlama sonrası adımlar
* [Sertifikayı yerel bir bilgisayara yükleme](#InstallCert)
* [Üretime hazır sertifikayı ekleme](#AddCert)
* [Erişim Denetimi ad alanını alma](#ACS)

#### <a name="InstallCert"></a>Sertifikayı yerel bir bilgisayara yükleme

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

#### <a name="AddCert"></a>Üretime hazır sertifikayı ekleme

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

#### <a name="ACS"></a>Access Control ad alanını alma

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Visual Studio’dan BizTalk Hizmeti projesini dağıttığınızda, Erişim Denetimi ad alanına bunu girin. Erişim Denetimi ad alanı BizTalk Hizmetiniz için otomatik olarak oluşturulur.

Erişim Denetimi değerleri herhangi bir uygulamayla birlikte kullanılabilir. Azure BizTalk Services oluşturulduğunda, bu Erişim Denetimi ad alanı kimlik doğrulamasını BizTalk hizmeti dağıtımınızla denetler. Aboneliğinizi değiştirmek veya ad alanını yönetmek isterseniz, sol bölmedeki **ACTIVE DIRECTORY**’yi, sonra da ad alanınızı seçin. Görev çubuğu seçeneklerinizi listeler.

**Yönet**’e tıklanması Erişim Denetimi Yönetim Portalı’nı açar. Erişim Denetimi Yönetim Portalı'nda, BizTalk hizmeti **hizmet kimliklerini** kullanır:  
![Erişim Denetimi Yönetim Portalı’nda ACS Hizmet Kimlikleri][ACSServiceIdentities]

Erişim Denetimi hizmeti kimliği, uygulamaların veya istemcilerin Erişim Denetimi’yle doğrudan kimlik doğrulaması yapmasını ve belirteç almasını sağlayan bir dizi kimlik bilgisidir.

> [!IMPORTANT]
> BizTalk Hizmeti varsayılan hizmet kimliği ve **Parola** değeri için **Sahip**’i kullanır. Parola değeri yerine Simetrik Anahtar değeri kullanırsanız aşağıdaki hata oluşabilir.<br/><br/>*Belirtilen kimlik bilgileriyle Erişim Denetimi Yönetim Hizmeti hesabına bağlanılamadı*
> 
> 

[ACS Ad Alanınızı Yönetme](/previous-versions/azure/azure-services/hh674478(v=azure.100)) bazı kılavuzları ve önerileri listeler.

## <a name="requirements-explained"></a>Açıklanan gereksinimler
Bu gereksinimler Ücretsiz Sürüm için geçerli değildir.

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Ne gerekiyor</strong></td>
        <td><strong>Neden gerekiyor</strong></td>
</tr>
<tr>
<td>Azure aboneliği</td>
<td>Abonelik, Azure’da kimlerin oturum açabileceğini saptar. Hesap sahibi aboneliği <a HREF="https://account.windowsazure.com/Subscriptions"> Azure Abonelikleri</a>’nde oluşturur.
<br/><br/>
Azure hesabında birden fazla abonelik olabilir ve izni olan herkes tarafından yönetilebilir. Örneğin, Azure hesap sahibiniz <em>BizTalkServiceSubscription</em> adlı bir abonelik oluşturur ve şirketinizdeki BizTalk Yöneticilerine (örneğin, ContosoBTSAdmins@live.com) bu abonelik için erişim hakkı verir. Bu senaryoda, BizTalk Yöneticileri Azure’da oturum açar ve Azure BizTalk Services de dahil olmak üzere abonelikte barındırılan tüm hizmetler için tam yönetici haklarına sahip olurlar. BizTalk Yöneticileri Azure hesap sahipleri değildir; bu nedenle de herhangi fatura bilgilerine erişimleri yoktur.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=267577"> Azure’da Abonelikleri ve Depolama Hesaplarını Yönetme</a> konusu daha fazla bilgi sağlar.
</td>
</tr>
<tr>
<td>Azure SQL Database</td>
<td>Veri izleme de dahil, BizTalk Hizmeti tarafından kullanılan tablolar, görünümler ve depolanan yordamlar.
<br/><br/>
BizTalk Hizmeti oluşturduğunuzda, mevcut Azure SQL Sunucusu, Azure SQL Database kullanırsınız ya da otomatik olarak yeni bir Sunucu veya SQL Database oluşturursunuz.
<br/><br/>
SQL Database ölçeği otomatik olarak yapılandırılır. Genellikle, varsayılan ölçek BizTalk hizmeti için yeterlidir. Ölçeğin değiştirilmesi fiyatı etkiler. Bkz: <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=234930"> Azure SQL veritabanı'nda hesaplar ve faturalar</a>
<br/><br/>
<strong>Notlar</strong>
<br/>
<ul>
<li> Yeni bir Azure SQL sunucusu ve SQL Database oluşturduğunuzda, Azure Hizmetleri otomatik olarak etkinleşir. BizTalk Hizmeti için Azure Hizmetlerinin etkin olması gerekir.</li>
<li>Mevcut Azure SQL Sunucusunda yeni bir Azure SQL Database oluşturursanız, Sunucunun güvenlik duvarı kuralları değişmez. Sonuç olarak, başka Azure Hizmetlerinin Sunucunun veritabanlarına erişim izni yoktur.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure Erişim Denetimi ad alanı</td>
<td>Azure BizTalk Services ile kimlik doğrular. Visual Studio’dan BizTalk Hizmeti projesini dağıttığınızda, Erişim Denetimi ad alanına bunu girin. BizTalk Hizmeti oluşturduğunuzda, Erişim Denetimi ad alanı otomatik olarak oluşur.</td>
</tr>

<tr>
<td>Azure Storage hesabı</td>
<td>BizTalk hizmetinizin kullandığı tablolara, blob'lara ve kuyruklara erişim vererek aşağıdakileri kaydedebilirsiniz:

<ul>
<li>BizTalk Hizmetini izleyen günlük dosyaları. </li>
<li>İş ortakları arasında X12 veya AS2 sözleşmesi oluşturulduğunda, ileti özelliklerini depolamak için Arşivleme özelliğini etkinleştirebilirsiniz. Bu veriler Storage hesabına kaydedilir.</li>
</ul>
<br/>
BizTalk hizmeti oluşturduğunuzda, mevcut bir Storage hesabını kullanabilir ya da otomatik olarak yeni bir Storage hesabı oluşturabilirsiniz.
<br/><br/>
Varsayılan Storage ayarları BizTalk hizmeti için yeterlidir.
<br/><br/>
Storage hesabı oluşturduğunuzda, Birincil ve İkincil Anahtar da otomatik olarak oluşur. Bu Anahtarlar Storage hesabınıza erişimi denetler. BizTalk Hizmeti otomatik olarak Birincil Anahtarı kullanır.
<br/><br/>
Daha fazla bilgi için bkz. <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=285671">Storage</a>.
</td>
</tr>

<tr>
<td>SSL özel sertifikası</td>
<td>
Azure BizTalk Hizmeti oluşturulduğunda, BizTalk hizmeti adını içeren bir HTTPS URL'si de oluşturulur. Bu URL, otomatik olarak imzalanan salt geliştirme sertifikasını kullanmak için otomatik olarak yapılandırılır. Üretim için özel bir SSL sertifikası gerekir.
<br/><br/>
<strong>Önemli SSL Sertifikası Bilgileri</strong>

<ul>
<li>Sertifikanın son kullanım tarihi 5 yıldan az olmalıdır.</li>
<li>Tüm özel sertifikaları için parola gerekir. Bu parolayı öğrenin ve en iyisi bu parolayı yöneticilerinizle paylaşın.</li>
<li>Otomatik olarak imzalanan sertifikalar test/geliştirme ortamında kullanılır. Otomatik olarak imzalanan sertifikalar kullanıldığında, sertifikayı Kişisel sertifika deponuza ve Güvenilen Kök Sertifika Yetkilileri sertifika deposuna içeri aktarın.</li>
</ul>
<br/>Sertifika yetkilinize üretim sertifikası isteği gönderildiğinde, şu sertifika özelliklerini verin:
<br/>

<ul>
<li><strong>Gelişmiş anahtar kullanımı</strong>: En azından, Azure BizTalk Services sunucu kimlik doğrulaması gerekir.</li>
<li><strong>Ortak ad</strong>: Azure BizTalk hizmeti URL'nizin tam etki alanı adını (FQDN) girin. Bu makaledeki <a HREF="#CreateService">BizTalk Hizmeti oluşturma</a> bölümüne bakın.</li>
</ul>
<br/>
BizTalk Hizmeti oluşturulduktan sonra yeni veya farklı bir sertifika eklenebilir.
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting to any location.--->



## <a name="hybrid-connections"></a>Karma Bağlantılar
Azure BizTalk hizmeti oluşturduğunuzda **Karma Bağlantılar** sekmesi kullanılabilir:

![Karma Bağlantılar Sekmesi][HybridConnectionTab]

Karma Bağlantılar Azure web sitesine veya Azure mobil hizmetinden SQL Sunucusu, MySQL, HTTP Web API'leri, Mobile Services ve birçok özel Web Hizmeti gibi statik TCP bağlantı noktası kullanan şirket içi kaynaklara bağlanmak için kullanılır.  Karma Bağlantılar ve BizTalk Bağdaştırıcı Hizmeti farklıdır. BizTalk Bağdaştırıcı Hizmeti, Azure BizTalk Services’ı şirket içi iş kolu (LOB) sistemine bağlamak için kullanılır.

 Karma Bağlantıları oluşturma ve yönetme de dahil, daha fazla bilgi için bkz. [Karma Bağlantılar](integration-hybrid-connection-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
BizTalk hizmeti oluşturulduktan sonra farklı bilgilenmeli [BizTalk Services: Pano, İzleyici ve ölçek sekmeleri](biztalk-dashboard-monitor-scale-tabs.md). BizTalk Hizmeti uygulamalarınız için hazır. Uygulamalar oluşturmaya başlamak için [Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=235197)’a gidin.

## <a name="see-also"></a>Ayrıca bkz.
* [BizTalk Services: Sürümler Grafiği](biztalk-editions-feature-chart.md)<br/>
* [BizTalk Services: Durum grafiği](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Yedekleme ve Geri Yükleme](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Azaltma](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Verenin adı ve verenin anahtarı](biztalk-issuer-name-issuer-key.md)<br/>
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Karma Bağlantılar](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
