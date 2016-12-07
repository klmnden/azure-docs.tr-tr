---
title: "Azure portalında Azure BizTalk Services oluşturma | Microsoft Belgeleri"
description: "Azure portalında Azure BizTalk Services hazırlamayı veya oluşturmayı öğrenin; MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 9cf1faabe3ea12af0ee5fd8a825975e30947b03a
ms.openlocfilehash: 12606d312ba95d9ef73e988fa4677a8314f9a579


---
# <a name="create-biztalk-services-using-the-azure-portal"></a>Azure portalını kullanarak BizTalk Services oluşturma

> [!TIP]
> Azure portalında oturum açmak için bir Azure hesabınız ve Azure aboneliğiniz olması gerekir. Hesabınız yoksa birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Bkz. [Azure Ücretsiz Deneme](http://go.microsoft.com/fwlink/p/?LinkID=239738).
> 
> 

## <a name="create-a-biztalk-service"></a>BizTalk Hizmeti oluşturma
Seçtiğiniz Sürüm’e bağlı olarak, BizTalk Hizmeti ayarlarının tümü kullanılamayabilir.

1. [Azure portalında](http://go.microsoft.com/fwlink/p/?LinkID=213885) oturum açın.
2. Alt gezinti bölmesinde **YENİ**’yi seçin:  
   ![Yeni düğmesini seçin][NEWButton]
3. **UYGULAMA HİZMETLERİ** > **BIZTALK HİZMETİ** > **ÖZEL OLUŞTUR**’u seçin:  
   ![BizTalk Hizmeti’ni seçin ve Özel Oluştur’u seçin][NewBizTalkService]
4. BizTalk Hizmeti ayarlarını girin:
   
    <table border="1">
    <tr>
    <td><strong>BizTalk hizmeti adı</strong></td>
    <td>Herhangi bir ad girin, ancak özel olsun. Bazı örnekler:<br/><br/>
    <em>mycompany</em>.biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>.biztalk.windows.net<br/>
    <em>myapplication</em>.biztalk.windows.net<br/><br/>". biztalk.windows.net" otomatik olarak girdiğiniz ada eklenir. Bu, BizTalk hizmetinize erişmek için kullanılan bir URL oluşturur; örneğin, <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Sürüm</strong></td>
    <td>Test etme/geliştirme aşamasındaysanız, <strong>Geliştirici</strong>’yi seçin. Üretim aşamasındaysanız, kullanırsanız <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: Sürümler Grafiği</a>’ni, iş senaryonuz için <strong>Premium</strong> mu, <strong>Standart</strong> mı, yoksa <strong>Temel</strong> sürümünün mü daha doğru olduğun saptamak amacıyla kullanın.
    </td>
    </tr>
    <tr>
    <td><strong>Bölge</strong></td>
    <td>BizTalk Hizmetinizi barındıracak coğrafi bölgeyi seçin.</td>
    </tr>
    <tr>
    <td><strong>Etki alanı URL'si</strong></td>
    <td><strong>İsteğe bağlı</strong>. Varsayılan olarak, etki alanı URL'si <em>YourBizTalkServiceName</em>. biztalk.windows.net biçimindedir. Özel bir etki alanı da girilebilir. Örneğin, etki alanınız <em>contoso</em> olarak belirtilmişse şunları girebilirsiniz: <br/><br/>
    <em>MyCompany</em>.contoso.com<br/>
    <em>MyCompanyMyApplication</em>.contoso.com<br/>
    <em>MyApplication</em>.contoso.com<br/>
    <em>YourBizTalkServiceName</em>.contoso.com<br/>
    </td>
    </tr>
    </table>
   İLERİ okunu seçin.
5. Storage ve Veritabanı Ayarlarını girin:
   
    <table border="1">
    <tr>
    <td><strong>Depolama hesabını izleme/arşivleme</strong></td>
    <td>Mevcut bir depolama hesabını seçin veya yeni bir depolama hesabı oluşturun. <br/><br/>Yeni bir Storage hesabı oluşturuyorsanız, <strong>Storage Hesabı Adı</strong> girin.</td>
    </tr>
    <tr>
    <td><strong>Veritabanı izleme</strong></td>
    <td>Mevcut bir Azure SQL veritabanı kullanıyorsanız, başka bir BizTalk hizmeti tarafından kullanılamaz. Azure SQL Veritabanı Sunucusu oluşturulduğunda oturum açma adı ve parola girilmesi gerekir.<br/><br/><strong>İPUCU</strong> BizTalk hizmetiyle aynı bölgede Veritabanı izleme ve depolama hesabı izleme/arşivleme oluşturun.</td>
    </tr>
    </table>
   İLERİ okunu seçin.
6. Veritabanı ayarlarını girin:
   
    <table border="1">
    <tr>
    <td><strong>Ad</strong></td>
    <td>Önceki ekranda <strong>Yeni bir SQL veritabanı oluştur</strong> seçili olduğunda kullanılabilir.
    <br/><br/>
    BizTalk Hizmeti tarafından kullanılacak bir SQL Veritabanı adı girin.</td>
    </tr>
    <tr>
    <td><strong>Sunucu</strong></td>
    <td>Önceki ekranda <strong>Yeni bir SQL veritabanı oluştur</strong> seçili olduğunda kullanılabilir.
    <br/><br/>
    Mevcut bir SQL Veritabanı Sunucusu seçin veya yeni bir SQL Veritabanı sunucusu oluşturun.</td>
    </tr>
    <tr>
    <td><strong>Sunucu oturum açma adı</strong></td>
    <td>Oturum açma kullanıcı adını girin.</td>
    </tr>
    <tr>
    <td><strong>Sunucu oturum açma parolası</strong></td>
    <td>Oturum açma parolasını girin.</td>
    </tr>
    <tr>
    <td><strong>Bölge</strong></td>
    <td><strong>Yeni bir SQL veritabanı oluştur</strong> seçili olduğunda kullanılabilir. SQL Database’inizi barındıracak coğrafi bölgeyi seçin.</td>
    </tr>
    </table>

Sihirbazı tamamlamak için onay işaretini seçin. İlerleme simgesi görünür:  
![Tamamlandığında ilerleme simgesi görüntülenir][ProgressComplete]

Tamamlandığında, Azure BizTalk Hizmeti oluşturulmuştur ve uygulamalarınız için hazırdır. Varsayılan ayarlar yeterlidir. Varsayılan ayarları değiştirmek istiyorsanız, sol gezinti bölmesinde **BIZTALK HİZMETLERİ**’ni, sonra da BizTalk Hizmetinizi seçin. En üstteki [Pano, İzleyici ve Ölçek sekmelerinde](biztalk-dashboard-monitor-scale-tabs.md) ek ayarlar görüntülenir.

BizTalk Hizmeti durumunu bağlı olarak, bazı tamamlanamayan işlemler vardır. Bu işlemlerin bir listesi için [BizTalk Services Durumu Grafiği](biztalk-service-state-chart.md)’ne gidin.

## <a name="post-provisioning-steps"></a>Hazırlama sonrası adımlar
* [Sertifikayı yerel bir bilgisayara yükleme](#InstallCert)
* [Üretime hazır sertifikayı ekleme](#AddCert)
* [Access Control ad alanını alma](#ACS)

#### <a name="a-nameinstallcertainstall-the-certificate-on-a-local-computer"></a><a name="InstallCert"></a>Sertifikayı yerel bir bilgisayara yükleme
BizTalk Hizmeti hazırlamanın bir parçası olarak, otomatik olarak imzalanan ve BizTalk hizmeti aboneliğinizle ilişkili bir sertifika oluşturulur. Bu sertifikayı indirip BizTalk Hizmeti uç noktasına iletileri göndereceğiniz veya BizTalk Hizmeti uygulamalarını dağıtacağınız bilgisayarlara bunu yüklemeniz gerekir.

1. [Azure portalında](http://go.microsoft.com/fwlink/p/?LinkID=213885) oturum açın.
2. Sol gezinti bölmesinde **BIZTALK HİZMETLERİ**’ni, sonra da BizTalk Hizmeti aboneliğinizi seçin.
3. **Pano** sekmesini seçin.
4. **SSL Sertifikası İndir**’i seçin:  
   ![SSL Sertifikasını Değiştirme][QuickGlance]
5. Sertifikaya çift tıklayın ve sertifikayı yüklemek için sihirbazda ilerleyin. **Güvenilen Kök Sertifika Yetkilileri** deposu altına sertifikayı yüklediğinizden emin olun.

#### <a name="a-nameaddcertaadd-a-production-ready-certificate"></a><a name="AddCert"></a>Üretime hazır sertifikayı ekleme
BizTalk Services oluşturulduğunda otomatik olarak oluşturulan otomatik imzalı sertifika yalnızca geliştirme ortamlarında kullanılmak üzere tasarlanmıştır. Üretim senaryoları için üretime hazır sertifikayla değiştirin.

1. **Pano** sekmesinde **SSL Sertifikasını Güncelleştir**’i seçin.
2. BizTalk Hizmet adınızı ekleyen özel SSL sertifikanıza (*CertificateName*.pfx) göz atın, parolayı girin ve ardından onay işaretine tıklayın.

#### <a name="a-nameacsaget-the-access-control-namespace"></a><a name="ACS"></a>Access Control ad alanını alma
1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885) oturum açın.
2. Sol gezinti bölmesinde **BIZTALK HİZMETLERİ**’ni, sonra da BizTalk Hizmetini seçin.
3. Görev çubuğunda **Bağlantı Bilgileri**’ni seçin.  
   ![Bağlantı Bilgilerini seçme][ACSConnectInfo]
4. Erişim Denetimi değerlerini kopyalayın.

Visual Studio’dan BizTalk Hizmeti projesini dağıttığınızda, Erişim Denetimi ad alanına bunu girin. Erişim Denetimi ad alanı BizTalk Hizmetiniz için otomatik olarak oluşturulur.

Erişim Denetimi değerleri herhangi bir uygulamayla birlikte kullanılabilir. Azure BizTalk Services oluşturulduğunda, bu Erişim Denetimi ad alanı kimlik doğrulamasını BizTalk hizmeti dağıtımınızla denetler. Aboneliğinizi değiştirmek veya ad alanını yönetmek isterseniz, sol bölmedeki **ACTIVE DIRECTORY**’yi, sonra da ad alanınızı seçin. Görev çubuğu seçeneklerinizi listeler.

**Yönet**’e tıklanması Erişim Denetimi Yönetim Portalı’nı açar. Erişim Denetimi Yönetim Portalı'nda, BizTalk hizmeti **hizmet kimliklerini** kullanır:  
![Access Control Yönetim Portalı’nda ACS Hizmet Kimlikleri][ACSServiceIdentities]

Erişim Denetimi hizmeti kimliği, uygulamaların veya istemcilerin Erişim Denetimi’yle doğrudan kimlik doğrulaması yapmasını ve belirteç almasını sağlayan bir dizi kimlik bilgisidir.

> [!IMPORTANT]
> BizTalk Hizmeti varsayılan hizmet kimliği ve **Parola** değeri için **Sahip**’i kullanır. Parola değeri yerine Simetrik Anahtar değeri kullanırsanız aşağıdaki hata oluşabilir.<br/><br/>*Belirtilen kimlik bilgileriyle Access Control Yönetim Hizmeti hesabına bağlanılamadı*
> 
> 

[ACS Ad Alanınızı Yönetme](https://msdn.microsoft.com/library/azure/hh674478.aspx) bazı kılavuzları ve önerileri listeler.

## <a name="requirements-explained"></a>Açıklanan gereksinimler
Bu gereksinimler Ücretsiz Sürüm için geçerli değildir.

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Ne gerekiyor</strong></td>
        <td><strong>Neden gerekiyor</strong></td>
</tr>
<tr>
<td>Azure aboneliği</td>
<td>Abonelik, Azure portalında kimin oturum açabileceğini saptar. Hesap sahibi aboneliği <a HREF="https://account.windowsazure.com/Subscriptions"> Azure Abonelikleri</a>’nde oluşturur.
<br/><br/>
Azure hesabında birden fazla abonelik olabilir ve izni olan herkes tarafından yönetilebilir. Örneğin, Azure hesap sahibi <em>BizTalkServiceSubscription</em> adlı bir abonelik oluşturur ve bu aboneliğe şirket içindeki BizTalk Yöneticilerine (örneğin, ContosoBTSAdmins@live.com) erişim verir. Bu senaryoda, BizTalk Yöneticileri Azure portalında oturum açar ve Azure BizTalk Services de dahil olmak üzere abonelikte barındırılan tüm hizmetler için tam yönetici haklarına sahip olurlar. BizTalk Yöneticileri Azure hesap sahipleri değildir; bu nedenle de herhangi fatura bilgilerine erişimleri yoktur.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577"> Azure portalında Abonelikleri ve Depolama Hesaplarını Yönetme</a> konusu daha fazla bilgi sağlar.
</td>
</tr>
<tr>
<td>Azure SQL Database</td>
<td>Veri izleme de dahil, BizTalk Hizmeti tarafından kullanılan tablolar, görünümler ve depolanan yordamlar.
<br/><br/>
BizTalk Hizmeti oluşturduğunuzda, mevcut Azure SQL Sunucusu, Azure SQL Database kullanırsınız ya da otomatik olarak yeni bir Sunucu veya SQL Database oluşturursunuz.
<br/><br/>
SQL Database ölçeği otomatik olarak yapılandırılır. Genellikle, varsayılan ölçek BizTalk hizmeti için yeterlidir. Ölçeğin değiştirilmesi fiyatı etkiler. Bkz. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930"> Azure SQL Veritabanı'nda Hesaplar ve Faturalar</a>
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
<li>BizTalk Hizmetini izleyen günlük dosyaları. İzleme çıktısı Azure portalındaki **İzleme** sekmesinde de görüntülenir.</li>
<li>İş ortakları arasında X12 veya AS2 sözleşmesi oluşturulduğunda, ileti özelliklerini depolamak için Arşivleme özelliğini etkinleştirebilirsiniz. Bu veriler Storage hesabına kaydedilir.</li>
</ul>
<br/>
BizTalk hizmeti oluşturduğunuzda, mevcut bir Storage hesabını kullanabilir ya da otomatik olarak yeni bir Storage hesabı oluşturabilirsiniz.
<br/><br/>
Varsayılan Storage ayarları BizTalk hizmeti için yeterlidir.
<br/><br/>
Storage hesabı oluşturduğunuzda, Birincil ve İkincil Anahtar da otomatik olarak oluşur. Bu Anahtarlar Storage hesabınıza erişimi denetler. BizTalk Hizmeti otomatik olarak Birincil Anahtarı kullanır.
<br/><br/>
Daha fazla bilgi için bkz. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">Storage</a>.
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
<li><strong>Gelişmiş Anahtar Kullanımı</strong>: En azından, Azure BizTalk Services için Sunucu Kimlik Doğrulaması gerekir.</li>
<li><strong>Ortak Ad</strong>: Azure BizTalk Hizmeti URL'sinin tam etki alanı adını (FQDN) girin. Bu makaledeki <a HREF="#BizTalk">BizTalk Hizmeti oluşturma</a> bölümüne bakın.</li>
</ul>
<br/>
BizTalk Hizmeti oluşturulduktan sonra yeni veya farklı bir sertifika eklenebilir.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Karma Bağlantılar
Azure BizTalk hizmeti oluşturduğunuzda **Karma Bağlantılar** sekmesi kullanılabilir:

![Karma Bağlantılar Sekmesi][HybridConnectionTab]

Karma Bağlantılar Azure web sitesine veya Azure mobil hizmetinden SQL Sunucusu, MySQL, HTTP Web API'leri, Mobile Services ve birçok özel Web Hizmeti gibi statik TCP bağlantı noktası kullanan şirket içi kaynaklara bağlanmak için kullanılır.  Karma Bağlantılar ve BizTalk Bağdaştırıcı Hizmeti farklıdır. BizTalk Bağdaştırıcı Hizmeti, Azure BizTalk Services’ı şirket içi iş kolu (LOB) sistemine bağlamak için kullanılır.

 Karma Bağlantıları oluşturma ve yönetme de dahil, daha fazla bilgi için bkz. [Karma Bağlantılar](integration-hybrid-connection-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
Artık bir BizTalk Hizmeti oluşturuldu, şimdi de kendinizi farklı [BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](biztalk-dashboard-monitor-scale-tabs.md) ile tanışmaya hazırlayın. BizTalk Hizmeti uygulamalarınız için hazır. Uygulamalar oluşturmaya başlamak için [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197)’a gidin.

## <a name="see-also"></a>Ayrıca bkz.
* [BizTalk Services: Sürümler Grafiği](biztalk-editions-feature-chart.md)<br/>
* [BizTalk Services: Durum Grafiği](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Yedekleme ve Geri Yükleme](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Azaltma](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Verenin Adı ve Verenin Anahtarı](biztalk-issuer-name-issuer-key.md)<br/>
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Karma Bağlantılar](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png



<!--HONumber=Nov16_HO3-->


