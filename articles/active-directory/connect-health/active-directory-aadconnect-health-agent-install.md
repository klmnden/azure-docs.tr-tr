---
title: "Azure AD Connect Health Aracısı yüklemesi | Microsoft Belgeleri"
description: "Bu Azure AD Connect Health sayfasında, AD FS ve Eşitleme için aracı yükleme işlemi açıklanmıştır."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 1cc8ae90-607d-4925-9c30-6770a4bd1b4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: bfdcc4aadab18091b2f57e8bc751b37d1bac4d26
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-connect-health-agent-installation"></a>Azure AD Connect Health Aracısı Yüklemesi
Bu belge, Azure AD Connect Health Aracılarını yüklemenize ve yapılandırmanıza yardımcı olur. Aracıları [buradan](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent) indirebilirsiniz.

## <a name="requirements"></a>Gereksinimler
Aşağıdaki tabloda Azure AD Connect Health kullanımına ilişkin gereksinimler listelenmiştir.

| Gereksinim | Açıklama |
| --- | --- |
| Azure AD Premium |Azure AD Connect Health, bir Azure AD Premium özelliği olup Azure AD Premium gerektirir. </br></br>Daha fazla bilgi için bkz. [Azure AD Premium ile Çalışmaya Başlama](../active-directory-get-started-premium.md) </br>30 günlük ücretsiz denemeyi başlatmak için bkz. [Denemeyi başlatma.](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Azure AD Connect Health ile çalışmaya başlamak için Azure AD'nizin genel yöneticisi olmanız gerekir. |Varsayılan olarak yalnızca genel yöneticiler; çalışmaya başlamak üzere durum aracılarını yükleyip yapılandırabilir, portala erişebilir ve Azure AD Connect Health'te işlem gerçekleştirebilir. Daha fazla bilgi için bkz. [Azure AD dizininizi yönetme](../active-directory-administer.md). <br><br> Rol Tabanlı Erişim Denetimini kullanarak kuruluşunuzdaki diğer kullanıcılara Azure AD Connect Health erişim izni verebilirsiniz. Daha fazla bilgi için bkz. [Azure AD Connect Health için Rol Tabanlı Erişim Denetimi.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Önemli:** Aracıları yüklerken kullanılan hesap bir iş veya okul hesabı olmalıdır. Bir Microsoft hesabı olamaz. Daha fazla bilgi için bkz. [Azure'a kuruluş olarak kaydolma](../sign-up-organization.md) |
| Azure AD Connect Health Aracısı, hedeflenen tüm sunucularda yüklüdür | Azure AD Connect Health, veri almak ve İzleme ve Analiz özelliklerini sağlamak için hedeflenen sunucularda Sistem Durumu Aracılarının yüklü ve yapılandırılmış olmasını gerektirir </br></br>Örneğin, AD FS altyapınızdan veri alabilmek için AD FS sunucularında ve Web Uygulaması Proxy sunucularında aracının yüklü olması gerekir. Benzer şekilde, şirket içi AD DS altyapınızdaki verileri almak için aracının etki alanı denetleyicilerine yüklenmesi gerekir. </br></br> |
| Azure hizmet uç noktalarına giden bağlantı | Yükleme ve çalışma zamanı sırasında, aracı ile Azure AD Connect Health hizmet uç noktaları arasında bağlantı kurulması gerekir. Giden bağlantı Güvenlik Duvarları kullanılarak engellenirse aşağıdaki uç noktaların izin verilenler listesine eklendiğinden emin olun: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - Bağlantı Noktası: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|IP Adreslerini temel alan giden bağlantı | Güvenlik duvarlarında IP adresine göre filtreleme için bkz. [Azure IP Aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=41653).|
| Giden trafik için SSL İncelemesi filtrelenmiş ya da devre dışı | Ağ katmanında giden trafik için SSL incelemesi veya sonlandırması mevcutsa aracı kaydı adımı veya veri yükleme işlemleri başarısız olabilir. |
| Aracıyı çalıştıran sunucudaki güvenlik duvarı bağlantı noktaları. |Aracının Azure AD Health hizmet uç noktaları ile iletişim kurabilmesi için aşağıdaki güvenlik duvarı bağlantı noktalarının açık olması gerekir.</br></br><li>TCP bağlantı noktası 443</li><li>TCP bağlantı noktası 5671</li> |
| IE Artırılmış Güvenlik etkinse aşağıdaki web sitelerine izin verin |IE Artırılmış Güvenlik etkinse aracının yükleneceği sunucuda aşağıdaki web sitelerine izin verilmesi gerekir.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Kuruluşunuz için Azure Active Directory tarafından güvenilen federasyon sunucusu. Örnek: https://sts.contoso.com</li> |
| PowerShell v4.0 veya üzerinin yüklü olduğundan emin olun | <li>Windows Server 2008 R2, aracı için yeterli olmayan PowerShell v2.0 sürümüne sahiptir.  PowerShell'i [Windows Server 2008 R2 Sunucularında aracı yüklemesi](#agent-installation-on-windows-server-2008-r2-servers) belgesine göre güncelleştirin.</li><li>Windows Server 2012, aracı için yeterli olmayan PowerShell v3.0 sürümüne sahiptir.  Windows Management Framework'ü [güncelleştirin](http://www.microsoft.com/en-us/download/details.aspx?id=40855).</li><li>Windows Server 2012 R2 ve üzeri ile gelen PowerShell sürümü yeterli olacaktır.</li>|
|FIPS’yi devre dışı bırakma|FIPS, Azure AD Connect Health aracıları tarafından desteklenmez.|

## <a name="download-and-install-the-azure-ad-connect-health-agent"></a>Azure AD Connect Health Aracısını indirme ve yükleme
* Azure AD Connect Health [gereksinimlerini yerine getirdiğinizden](active-directory-aadconnect-health-agent-install.md#requirements) emin olun.
* AD FS için Azure AD Connect Health kullanmaya başlama
    * [AD FS için Azure AD Connect Health Aracısını indirin.](http://go.microsoft.com/fwlink/?LinkID=518973)
    * [Yükleme talimatlarına bakın](#installing-the-azure-ad-connect-health-agent-for-ad-fs).
* Eşitleme için Azure AD Connect Health kullanmaya başlama
    * [Azure AD Connect'in en son sürümünü indirip yükleyin](http://go.microsoft.com/fwlink/?linkid=615771). Eşitleme için Durum Aracısı, Azure AD Connect yüklemesinin bir parçası olarak yüklenir (sürüm 1.0.9125.0 veya daha yeni bir sürüm).
* AD DS için Azure AD Connect Health kullanmaya başlama
    * [AD DS için Azure AD Connect Health Aracısını indirin](http://go.microsoft.com/fwlink/?LinkID=820540).
    * [Yükleme talimatlarına bakın](#installing-the-azure-ad-connect-health-agent-for-ad-ds).

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>AD FS için Azure AD Connect Health Aracısını yükleme
Aracı yüklemesini başlatmak için indirdiğiniz .exe dosyasına çift tıklayın. İlk ekranda Yükle'ye tıklayın.

![Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health-requirements/install1.png)

Yükleme tamamlandıktan sonra Şimdi Yapılandır'a tıklayın.

![Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health-requirements/install2.png)

Bu durum, aracı kaydı işlemini başlatmak üzere bir PowerShell penceresi açar. İstendiğinde, aracı kaydını gerçekleştirme erişimi olan bir Azure AD hesabıyla oturum açın. Varsayılan olarak, Genel Yönetici hesabı erişime sahiptir.

![Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health-requirements/install3.png)

Oturum açtıktan sonra PowerShell işleme devam eder. İşlem tamamlandıktan sonra PowerShell'i kapatabilirsiniz ve yapılandırma tamamlanmış olur.

Bu noktada, aracı hizmetleri, aracının gerekli verileri bulut hizmetine güvenli bir şekilde yüklemesine izin vermek üzere otomatik olarak başlatılmalıdır.

Önceki bölümlerde belirtilen önkoşulların tümünü yerine getirmediyseniz PowerShell penceresinde uyarılar görüntülenir. Aracıyı yüklemeden önce [buradaki](active-directory-aadconnect-health-agent-install.md#requirements) gereksinimleri karşıladığınızdan emin olun. Bu hataların bir örneğini aşağıdaki ekran görüntüsünde görebilirsiniz.

![Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health-requirements/install4.png)

Aracının yüklü olduğunu doğrulamak için sunucuda aşağıdaki hizmetleri arayın. Yapılandırmayı tamamladıysanız bu hizmetlerin çalışır durumda olması gerekir. Aksi halde bu hizmetler yapılandırma tamamlanıncaya kadar durdurulur.

* Azure AD Connect Health AD FS Tanılama Hizmeti
* Azure AD Connect Health AD FS Öngörü Hizmeti
* Azure AD Connect Health AD FS İzleme Hizmeti

![Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health-requirements/install5.png)

### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Windows Server 2008 R2 Sunucularında aracı yüklemesi
Windows Server 2008 R2 sunucuları için adımlar:

1. Sunucunun Hizmet Paketi 1 veya daha yüksek bir sürümde çalıştığından emin olun.
2. Aracı yüklemesi için IE ESC'yi devre dışı bırakın:
3. AD Health aracısını yüklemeden önce her bir sunucuya Windows PowerShell 4.0 sürümünü yükleyin. Windows PowerShell 4.0 sürümünü yüklemek için:
   * Çevrimdışı yükleyiciyi indirmek için aşağıdaki bağlantıyı kullanarak [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) sürümünü yükleyin.
   * PowerShell ISE'yi yükleme (Windows Özelliklerinden)
   * [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) sürümünü yükleyin.
   * Sunucuda Internet Explorer 10 veya sonraki bir sürümünü yükleyin. (Sistem Sağlığı Hizmeti tarafından Azure Yönetici kimlik bilgileriniz kullanılarak kimliğinizin doğrulanması için gereklidir.)
4. Windows Server 2008 R2'ye Windows PowerShell 4.0 sürümünü yüklemek üzere daha fazla bilgi almak için [buradaki](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx) wiki makalesine bakın.

### <a name="enable-auditing-for-ad-fs"></a>AD FS için Denetimi Etkinleştirme
> [!NOTE]
> Bu bölüm yalnızca AD FS sunucuları için geçerlidir. Web Uygulaması Ara Sunucularında bu adımları izlemeniz gerekmez.
>

Kullanım Analizi özelliğinin verileri toplaması ve analiz edebilmesi için, Azure AD Connect Health aracılarının AD FS Denetim Günlüklerindeki bilgilere ihtiyacı vardır. Bu günlükler varsayılan olarak etkin değildir. AD FS denetimini etkinleştirmek ve AD FS denetim günlüklerini bulmak için AD FS sunucularınızda aşağıdaki yordamları kullanın.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2008-r2"></a>Windows Server 2008 R2'de AD FS için denetimi etkinleştirme
1. **Başlat** menüsüne tıklayın, **Programlar** ve daha sonra **Yönetim Araçları**'nın üzerine gidin ve ardından **Yerel Güvenlik İlkesi**'ne tıklayın.
2. **Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Ataması** klasörüne gidin ve **Güvenlik denetimleri oluştur**'a çift tıklayın.
3. **Yerel Güvenlik Ayarları** sekmesinde AD FS 2.0 hizmet hesabının listelenmiş olduğunu doğrulayın. Listelenmediyse **Kullanıcı veya Grup Ekle**'ye tıklayın, hizmet hesabını listeye ekleyin ve ardından **Tamam**'a tıklayın.
4. Denetimi etkinleştirmek için yükseltilmiş ayrıcalıklara sahip bir Komut İstemi açın ve şu komutu çalıştırın: <code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. **Yerel Güvenlik İlkesi**'ni kapatın ve ardından **AD FS Yönetim** ek bileşenini açın. AD FS Yönetim ek bileşenini açmak için **Başlat**'a tıklayın, **Programlar** ve **Yönetim Araçları**'nın üzerine gidin ve ardından **AD FS 2.0 Yönetimi**'ne tıklayın.
6. **Eylemler** bölmesinde **Federasyon Hizmeti Özelliklerini Düzenle**'ye tıklayın.
7. **Federation Service Properties (Federasyon Hizmeti Özellikleri)** iletişim kutusunda **Events (Olaylar)** sekmesine tıklayın.
8. **Success audits (Başarı denetimleri)** ve **Failure audits (Hata denetimleri)** onay kutularını seçin.
9. **OK (Tamam)** düğmesine tıklayın.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Windows Server 2012 R2'de AD FS için denetimi etkinleştirme
1. Başlat ekranından **Sunucu Yöneticisi**'ni açarak **Yerel Güvenlik İlkesi**'ni veya masaüstünde bulunan görev çubuğundan Sunucu Yöneticisi'ni açıp **Araçlar/Yerel Güvenlik İlkesi**'ne tıklayın.
2. **Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Ataması** klasörüne gidin ve **Güvenlik denetimleri oluştur**'a çift tıklayın.
3. **Yerel Güvenlik Ayarları** sekmesinde AD FS hizmet hesabının listelenmiş olduğunu doğrulayın. Listelenmediyse **Kullanıcı veya Grup Ekle**'ye tıklayın, hizmet hesabını listeye ekleyin ve ardından **Tamam**'a tıklayın.
4. Denetimi etkinleştirmek için yükseltilmiş ayrıcalıklara sahip bir komut istemi açın ve şu komutu çalıştırın: ```auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable```.
5. **Yerel Güvenlik İlkesi**'ni kapatın ve ardından **AD FS Yönetimi** ek bileşenini açın. (Sunucu Yöneticisi'nde Araçlar'a tıklayın ve AD FS Yönetimi'ni seçin).
6. Eylemler bölmesinde **Edit Federation Service Properties (Federasyon Hizmeti Özelliklerini Düzenle)** seçeneğine tıklayın.
7. Federation Service Properties (Federasyon Hizmeti Özellikleri) iletişim kutusunda **Events (Olaylar)** sekmesine tıklayın.
8. **(Success audits and Failure audits) Başarı denetimleri ve Hata denetimleri** onay kutularını seçin ve **OK (Tamam)** düğmesine tıklayın.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2016"></a>Windows Server 2016'da AD FS için denetimi etkinleştirme
1. Başlat ekranından **Sunucu Yöneticisi**'ni açarak **Yerel Güvenlik İlkesi**'ni veya masaüstünde bulunan görev çubuğundan Sunucu Yöneticisi'ni açıp **Araçlar/Yerel Güvenlik İlkesi**'ne tıklayın.
2. **Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Ataması** klasörüne gidin ve **Güvenlik denetimleri oluştur**'a çift tıklayın.
3. **Yerel Güvenlik Ayarları** sekmesinde AD FS hizmet hesabının listelenmiş olduğunu doğrulayın. Mevcut değilse, **Kullanıcı veya Grup Ekle**'ye tıklayın, AD FS hizmet hesabını listeye ekleyin ve ardından **Tamam**'a tıklayın.
4. Denetimi etkinleştirmek için yükseltilmiş ayrıcalıklara sahip bir komut istemi açın ve şu komutu çalıştırın: <code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. **Yerel Güvenlik İlkesi**'ni kapatın ve ardından **AD FS Yönetimi** ek bileşenini açın. (Sunucu Yöneticisi'nde Araçlar'a tıklayın ve AD FS Yönetimi'ni seçin).
6. Eylemler bölmesinde **Edit Federation Service Properties (Federasyon Hizmeti Özelliklerini Düzenle)** seçeneğine tıklayın.
7. Federation Service Properties (Federasyon Hizmeti Özellikleri) iletişim kutusunda **Events (Olaylar)** sekmesine tıklayın.
8. **(Success audits and Failure audits) Başarı denetimleri ve Hata denetimleri** onay kutularını seçin ve **OK (Tamam)** düğmesine tıklayın. Bu seçenek varsayılan olarak etkindir.
9. Bir PowerShell penceresi açın ve şu komutu çalıştırın: ```Set-AdfsProperties -AuditLevel Verbose```.

"Temel" denetim düzeyi varsayılan olarak etkindir. [Windows Server 2016’da AD FS Denetimini geliştirme](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/auditing-enhancements-to-ad-fs-in-windows-server-2016) hakkında daha fazla bilgi edinin


#### <a name="to-locate-the-ad-fs-audit-logs"></a>AD FS denetim günlüklerini bulma
1. **Olay Görüntüleyicisi**'ni açın.
2. Windows Günlüklerine gidin ve **Security (Güvenlik)** seçeneğini belirleyin.
3. Sağ tarafta bulunan **Filter Current Logs (Geçerli Günlükleri Filtrele)** seçeneğine tıklayın.
4. Event Source (Olay Kaynağı) altındaki **AD FS Auditing (AD FS Denetimi)** seçeneğini belirleyin.

![AD FS denetim günlükleri](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [!WARNING]
> AD FS denetimi bir grup ilkesiyle devre dışı bırakılabilir. AD FS denetimi devre dışı bırakılırsa, oturum açma etkinlikleriyle ilgili kullanım analizi mevcut olmaz. AD FS denetimini devre dışı bırakan bir grup ilkesine sahip olmadığınızdan emin olun.>
>


## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Eşitleme için Azure AD Connect Health aracısını yükleme
Eşitleme için Azure AD Connect Health aracısı, Azure AD Connect'in en son derlemesinde otomatik olarak yüklenir. Azure AD Connect'i eşitleme için kullanmak üzere, Azure AD Connect'in en son sürümünü indirip yüklemeniz gerekir. En son sürümü [buradan](http://www.microsoft.com/download/details.aspx?id=47594) indirebilirsiniz.

Aracının yüklü olduğunu doğrulamak için sunucuda aşağıdaki hizmetleri arayın. Yapılandırmayı tamamladıysanız bu hizmetlerin çalışır durumda olması gerekir. Aksi halde bu hizmetler yapılandırma tamamlanıncaya kadar durdurulur.

* Azure AD Connect Health Eşitleme Öngörü Hizmeti
* Azure AD Connect Health Eşitleme İzleme Hizmeti

![Eşitleme için Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health-sync/services.png)

> [!NOTE]
> Azure AD Connect Health'in kullanılabilmesi için Azure AD Premium'un gerekli olduğunu unutmayın. Azure AD Premium'unuz yoksa Azure portalında yapılandırmayı tamamlayamazsınız. Daha fazla bilgi için [gereksinim sayfasına](active-directory-aadconnect-health-agent-install.md#requirements) bakın.
>
>

## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>El ile Eşitleme için Azure AD Connect Health kaydı
Azure AD Connect'i başarıyla yükledikten sonra Eşitleme aracısı kaydı için Azure AD Connect Health başarısız olursa aracıyı el ile kaydetmek için aşağıdaki PowerShell komutunu kullanabilirsiniz.

> [!IMPORTANT]
> Bu PowerShell komutunun, yalnızca Azure AD Connect yüklendikten sonra aracı kaydının başarısız olması halinde kullanılması gerekir.
>
>

Aşağıdaki PowerShell komutu, YALNIZCA Azure AD Connect'in başarılı bir şekilde yüklenip yapılandırılmasına rağmen durum aracısı kaydının başarısız olması halinde gereklidir. Aracı başarılı bir şekilde kaydolduktan sonra Azure AD Connect Health hizmetleri başlatılır.

Eşitleme için Azure AD Connect Health aracısını, aşağıdaki PowerShell komutunu kullanarak el ile kaydedebilirsiniz:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Komut için şu parametreler kullanılır:

* AttributeFiltering: Azure AD Connect varsayılan özniteliği eşitlemiyor ve filtrelenmiş bir öznitelik kümesini kullanmak üzere özelleştirildiyse $true (varsayılan). Aksi halde $false değerini alır.
* StagingMode: Azure AD Connect sunucusu, hazırlama modunda DEĞİLSE $false (varsayılan) değerini; sunucu hazırlama moduna yapılandırıldıysa $true değerini alır.

Kimliğinizi doğrulamanız istendiğinde, Azure AD Connect'in yapılandırılması için kullanılan genel yönetici hesabını (örneğin, admin@domain.onmicrosoft.com) kullanmanız gerekir.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>AD DS için Azure AD Connect Health Aracısını yükleme
Aracı yüklemesini başlatmak için indirdiğiniz .exe dosyasına çift tıklayın. İlk ekranda Yükle'ye tıklayın.

![Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Yükleme tamamlandıktan sonra Şimdi Yapılandır'a tıklayın.

![Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Bir komut istemi başlatılır ve ardından bazı PowerShell uygulamaları tarafından Register-AzureADConnectHealthADDSAgent komutu yürütülür. Azure’da oturum açmanız istendiğinde devam edin ve oturum açın.

![Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Oturum açtıktan sonra PowerShell işleme devam eder. İşlem tamamlandıktan sonra PowerShell'i kapatabilirsiniz ve yapılandırma tamamlanmış olur.

Bu noktada, hizmetler otomatik olarak başlatılarak verilerin aracı tarafından izlenip toplanmasına izin verilir. Önceki bölümlerde belirtilen önkoşulların tümünü yerine getirmediyseniz PowerShell penceresinde uyarılar görüntülenir. Aracıyı yüklemeden önce [buradaki](active-directory-aadconnect-health-agent-install.md#requirements) gereksinimleri karşıladığınızdan emin olun. Bu hataların bir örneğini aşağıdaki ekran görüntüsünde görebilirsiniz.

![AD DS için Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Aracının yüklü olduğunu doğrulamak için etki alanı denetleyicisinde aşağıdaki hizmetleri arayın.

* Azure AD Connect Health AD DS Insights Hizmeti
* Azure AD Connect Health AD DS İzleme Hizmeti

Yapılandırmayı tamamladıysanız bu hizmetlerin çalışır durumda olması gerekir. Aksi halde bu hizmetler yapılandırma tamamlanıncaya kadar durdurulur.

![Azure AD Connect Health'i doğrulama](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)


### <a name="agent-registration-using-powershell"></a>PowerShell kullanarak Aracı Kaydı
Uygun aracı setup.exe dosyasını yükledikten sonra role bağlı olarak aşağıdaki PowerShell komutlarını kullanarak aracı kaydı adımını gerçekleştirebilirsiniz. Bir PowerShell penceresi açın ve uygun komutu yürütün:

```
    Register-AzureADConnectHealthADFSAgent
    Register-AzureADConnectHealthADDSAgent
    Register-AzureADConnectHealthSyncAgent

```

Bu komutlar "Credential"’ı, kayıt işlemini etkileşimli olmayan bir şekilde veya bir Sunucu Çekirdeği makinesinde tamamlamaya yönelik bir parametre olarak kabul eder.
* Credential, parametre olarak geçirilen bir PowerShell değişkeninde yakalanabilir.
* Aracıları kaydetme erişimi olan ve MFA özelliği etkin olmayan herhangi bir Azure AD Kimliği sağlayabilirsiniz.
* Varsayılan olarak, Genel Yöneticiler aracı kaydını gerçekleştirme erişimine sahiptir. Ayrıca, daha az ayrıcalıklı diğer kimliklerin bu adımı uygulamasına izin verebilirsiniz. [Rol Tabanlı Access Control](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) hakkında daha fazla bilgi edinin.

```
    $cred = Get-Credential
    Register-AzureADConnectHealthADFSAgent -Credential $cred

```

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Azure AD Connect Health Aracılarını HTTP Ara Sunucusunu kullanacak şekilde yapılandırma
Azure AD Connect Health Aracılarını bir HTTP Ara Sunucusunu kullanacak şekilde yapılandırabilirsiniz.

> [!NOTE]
> * Aracı, web isteklerinde bulunmak için Microsoft Windows HTTP Hizmetlerinin yerine System.Net hizmetini kullandığından "Netsh WinHttp set ProxyServerAddress" seçeneğini kullanmanız desteklenmez.
> * Şifrelenmiş Https iletilerinin geçilmesi için, yapılandırılan Http Ara Sunucu adresi kullanılır.
> * HTTPBasic kullanılarak kimliği doğrulanmış ara sunucular desteklenmez.
>
>

### <a name="change-health-agent-proxy-configuration"></a>Durum Aracısı Ara Sunucu Yapılandırmasını Değiştirme
Azure AD Connect Health Aracısını bir HTTP Ara Sunucusunu kullanacak şekilde yapılandırmak üzere aşağıdaki seçeneklere sahipsiniz.

> [!NOTE]
> Proxy ayarlarının güncelleştirilmesi için tüm Azure AD Connect Health Aracısı hizmetlerinin yeniden başlatılması gerekir. Şu komutu çalıştırın:<br>
> Restart-Service AdHealth*
>
>

#### <a name="import-existing-proxy-settings"></a>Var olan ara sunucu Ayarlarını içeri aktarma
##### <a name="import-from-internet-explorer"></a>Internet Explorer'dan içeri aktarma
Internet Explorer HTTP proxy ayarları Azure AD Connect Health Aracıları tarafından kullanılmak üzere içeri aktarılabilir. Health aracısını çalıştıran sunucuların her birinde aşağıdaki PowerShell komutunu yürütün:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>WinHTTP'den içeri aktarma
WinHTTP proxy ayarları Azure AD Connect Health Aracıları tarafından kullanılmak üzere içeri aktarılabilir. Health aracısını çalıştıran sunucuların her birinde aşağıdaki PowerShell komutunu yürütün:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Ara sunucu adreslerini el ile belirtme
Health Aracısını çalıştıran sunucuların her birinde aşağıdaki PowerShell komutunu yürüterek bir proxy sunucusunu elle belirtebilirsiniz:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Örnek: *Set-AzureAdConnectHealthProxySettings - HttpsProxyAddress myproxyserver: 443*

* "adres", DNS'nin çözümlenebileceği bir sunucu adı veya bir IPv4 adresi olabilir.
* "bağlantı noktası" atlanabilir. Atlanması durumunda varsayılan bağlantı noktası olarak 443 seçilir.

#### <a name="clear-existing-proxy-configuration"></a>Var olan ara sunucu yapılandırmasını silme
Aşağıdaki komutu çalıştırarak var olan proxy sunucu yapılandırmasını silebilirsiniz:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Geçerli ara sunucu ayarlarını okuma
Aşağıdaki komutu çalıştırarak geçerli olarak yapılandırılmış olan proxy sunucu ayarlarını okuyabilirsiniz:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Azure AD Connect Health Hizmeti için Bağlantı Testi
Azure AD Connect Health aracısıyla Azure AD Connect Health hizmeti arasındaki bağlantının kesilmesine neden olabilecek sorunlar meydana gelebilir. Ağ sorunları, izin sorunları veya diğer çeşitli nedenler bunlara dahildir.

Aracı iki saatten uzun bir süre boyunca Azure AD Connect Health hizmetine veri gönderemezse portalda şu uyarı ile gösterilir: "Health Hizmeti verileri güncel değil." Aşağıdaki PowerShell komutunu çalıştırarak, etkilenen Azure AD Connect Health aracısının Azure AD Connect Health hizmetine veri yükleyebildiğini onaylayabilirsiniz:

    Test-AzureADConnectHealthConnectivity -Role ADFS

Rol parametresi şu anda şu değerleri alır:

* ADFS
* Sync
* EKLER

Ayrıntılı günlükleri görüntülemek için komut içinde -ShowResults bayrağını kullanabilirsiniz. Şu örneği kullanın:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

> [!NOTE]
> Bağlantı aracını kullanabilmek için öncelikle aracı kaydını tamamlamanız gerekir. Aracı kaydını tamamlayamıyorsanız Azure AD Connect Health için tüm [gereksinimleri](active-directory-aadconnect-health-agent-install.md#requirements) karşıladığınızdan emin olun. Bu bağlantı testi, aracı kaydı sırasında varsayılan olarak gerçekleştirilir.
>
>

## <a name="related-links"></a>İlgili bağlantılar
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health İşlemleri](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health Sürüm Geçmişi](active-directory-aadconnect-health-version-history.md)
