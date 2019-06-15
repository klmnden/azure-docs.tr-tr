---
title: Şirket içi veri ağ geçidi - Azure Logic Apps yükleme | Microsoft Docs
description: Azure Logic Apps'ten şirket içi veri erişebilmeniz için önce indirin ve şirket içi veri ağ geçidi yükleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 10/01/2018
ms.openlocfilehash: 10a6e5c33f6a3c23d98e6eb3380de0d6dc6ac216
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544475"
---
# <a name="install-on-premises-data-gateway-for-azure-logic-apps"></a>Azure Logic Apps için şirket içi veri ağ geçidi yükleme

Azure Logic Apps'ten şirket içi veri kaynaklarına bağlanmadan önce indirin ve yerel bir bilgisayarda şirket içi veri ağ geçidi yükleyin. Ağ geçidi hızlı veri aktarımı ve logic apps ile şirket içi (bulutta olmayan) veri kaynakları arasındaki şifreleme sunan bir köprü olarak çalışır. Bu makalede nasıl indirin, yükleyin ve şirket içi veri ağ geçidini ayarlamak gösterilmektedir. 

Power BI, Microsoft Flow, PowerApps ve Azure Analysis Services gibi diğer hizmetler ile aynı ağ geçidi yüklemesi'ni kullanabilirsiniz. Daha fazla bilgi edinin [veri ağ geçidi nasıl çalıştığını](#gateway-cloud-service).

<a name="supported-connections"></a>

Ağ geçidinin desteklediği [şirket içi Bağlayıcılar](../connectors/apis-list.md#on-premises-connectors) bu veri kaynakları için Azure Logic Apps içinde:

*   BizTalk Server 2016
*   Dosya Sistemi
*   IBM DB2  
*   IBM Informix
*   IBM MQ
*   MySQL
*   Oracle Veritabanı
*   PostgreSQL
*   SAP Uygulama Sunucusu 
*   SAP İleti Sunucusu
*   SharePoint Server
*   SQL Server
*   Teradata

Diğer hizmetlerle ağ geçidi kullanma hakkında daha fazla bilgi için şu makalelere bakın:

* [Microsoft Power BI şirket içi veri ağ geçidi](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Microsoft PowerApps şirket içi veri ağ geçidi](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Microsoft Flow şirket içi veri ağ geçidi](https://flow.microsoft.com/documentation/gateway-manage/)
* [Azure Analysis Services şirket içi veri ağ geçidi](../analysis-services/analysis-services-gateway.md)

<a name="requirements"></a>

## <a name="prerequisites"></a>Önkoşullar

* A [iş veya Okul hesabı](../active-directory/fundamentals/sign-up-organization.md) olan bir [Azure aboneliği](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access) 

  Ağ geçidi yüklemesi Azure aboneliğinizle ilişkilendirebilmeniz için ağ geçidi yüklemesi sırasında bu hesap için oturum açın. 
  Daha sonra Azure portalında, ağ geçidi yüklemesi için bir Azure kaynağı oluştururken de aynı hesabı kullanın. 
  Henüz Azure aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Yerel bilgisayarınıza gereksinimleri şunlardır:

  **En düşük gereksinimleri**

  * .NET Framework 4.5.2
  * Windows 7 veya Windows Server 2008 R2 64 bit sürümü (veya üzeri)

  **Önerilen gereksinimleri**

  * 8 çekirdekli CPU
  * 8 GB bellek
  * 64-bit sürüm Windows Server 2012 R2'in (veya üzeri)

* **Önemli noktalar**

  * Yalnızca yerel bir bilgisayara, etki alanı denetleyicisi, şirket içi veri ağ geçidi yükleyebilirsiniz. Ancak veri kaynağınız ile aynı bilgisayarda ağ geçidini yüklemenize gerek yoktur. Ayrıca, tüm veri kaynaklarınız, her veri kaynağı için ağ geçidini yüklemeye gerek yoktur için yalnızca bir ağ geçidi gerekir.

    > [!TIP]
    > Gecikmeyi en aza indirmek için izinlere sahip olduğunuz varsayılarak veri kaynağınız için ya da aynı bilgisayarda mümkün olabildiğince yakın ağ geçidi yükleyebilirsiniz.

  * Ağ geçidi her zaman açık İnternet'e bağlı bir bilgisayara yükleyin ve *değil* uyu. Aksi takdirde, ağ geçidi çalıştırılamaz. 
  Ayrıca, performansı, kablosuz ağ üzerinde düşebilir.

  * Yükleme sırasında yalnızca oturum açmak bir [iş veya Okul hesabı](../active-directory/sign-up-organization.md) yönetilen Azure Active Directory (Azure AD) tarafından örneğin @contoso.onmicrosoft.comve Azure B2B (konuk) hesabı veya kişisel Microsoft hesabı, örneğin @hotmail.com veya @outlook.com. 
  Bir ağ geçidi kaynağı oluşturarak Azure Portalı'nda, ağ geçidi yüklemesi kaydettiğinizde aynı oturum açma hesabı kullandığınızdan emin olun. 
  Şirket içi veri kaynağınıza mantıksal uygulamanızdan bağlantı oluşturduğunuzda, bu ağ geçidi kaynağı seçebilirsiniz. 
  [Neden gerekir ben bir Azure AD iş veya Okul hesabı?](#why-azure-work-school-account)

  > [!TIP]
  > Bir Office 365 teklifine kaydolduysanız ve gerçek iş e-postanıza sağlamadı, şu örnekteki gibi görünür bir oturum açma adresi olabilir: `username@domain.onmicrosoft.com` 
  >
  > Olan bir Microsoft hesabı kullanmak için bir [Visual Studio standart aboneliği](https://visualstudio.microsoft.com/vs/pricing/), ilk [Azure Active Directory'de bir dizin (Kiracı) oluşturma](../active-directory/develop/quickstart-create-new-tenant.md), ya da varsayılan dizin ile Microsoft hesabınızı kullanın. 
  > Dizine bir parolası olan bir kullanıcı eklemek ve ardından aboneliğiniz için bu kullanıcı erişimi verme. 
  > Ardından bu kullanıcı adı ve parola ile ağ geçidi yüklemesi sırasında oturum açabilirsiniz.

  * Ağ geçidi yüklemeniz için seçtiğiniz bölge burada daha sonra ağ geçidiniz Azure'da bir Azure kaynağı oluşturarak kayıt konumunu belirler. 
  Azure'da bu ağ geçidi kaynağı oluştururken seçmelisiniz *aynı* , ağ geçidi yükleme konumu. Azure hesabınızı yönetir, Azure AD kiracınız ile aynı konumda varsayılan bölgesidir ancak ağ geçidi yüklemesi sırasında konumu değiştirebilirsiniz.

  * Bir yükleyici ile 14.16.6317.4 öncesi ayarlamak için bir ağ geçidi zaten varsa, en son Yükleyicisi'ni çalıştırarak ağ geçidinizin konumu değiştiremezsiniz. Ancak, bunun yerine istediğiniz konumu ile yeni bir ağ geçidi ayarlamak için en son yükleyicisi kullanabilirsiniz.
  
    14\.16.6317.4 sürümden daha eski bir ağ geçidi yükleyicisini varsa, ancak ağ geçidi yüklemediyseniz henüz, indirebilir ve bunları en son yükleyicisi kullanın.

## <a name="high-availability-support"></a>Yüksek kullanılabilirlik desteği

Birden fazla ağ geçidi yüklemeniz ve kümeleri olarak ayarlamak, şirket içi veri ağ geçidi yüksek kullanılabilirliği destekler. Başka bir ağ geçidi oluşturmak için çalışmaya başladığınızda, var olan bir ağ geçidi varsa, isteğe bağlı olarak yüksek kullanılabilirlik kümeleri oluşturabilirsiniz. Bu kümeleri, ağ geçitleri tek hata noktalarından kaçınmak yardımcı olabilecek grupları halinde düzenleyin. Ayrıca, tüm şirket içi veri ağ geçidi bağlayıcıları artık yüksek kullanılabilirliği destekler.

Şirket içi veri ağ geçidini kullanmak için bu gereksinimleri ve unsurları gözden geçirin:

* En az bir ağ geçidi yüklemesi birincil ağ geçidini ve bu yükleme için kurtarma anahtarı olarak aynı Azure aboneliğinde zaten olmalıdır. 

* Birincil ağ geçidi ağ geçidi güncelleştirme Kasım 2017'den veya üzeri çalıştırmalıdır.

Bu gereksinimleri sağladıktan sonra sonraki ağ geçidi oluşturduğunuzda, seçin **eklemek için var olan bir ağ geçidi kümesi**, kümeniz için birincil ağ geçidini seçin ve bu birincil ağ geçidine ilişkin kurtarma anahtarını belirtin.
Daha fazla bilgi için [şirket içi veri ağ geçidi için yüksek kullanılabilirlik kümeleri](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters).

<a name="install-gateway"></a>

## <a name="install-data-gateway"></a>Veri ağ geçidi yükleme

1. [Karşıdan yükleme, kaydetme ve yerel bir bilgisayara ağ geçidi yükleyicisini çalıştırın](https://aka.ms/on-premises-data-gateway-installer).

2. Varsayılan yükleme yolu kabul edin veya bilgisayarınızda ağ geçidini yüklemek istediğiniz konumu belirtin.

3. Gözden geçirin ve kullanım ve Gizlilik Bildirimi'nin koşullarını kabul edin ve ardından **yükleme**.

   ![Kullanım ve Gizlilik Bildirimi'nin koşullarını kabul edin](./media/logic-apps-gateway-install/accept-terms.png)

4. Ağ geçidi başarıyla yüklendikten sonra iş veya Okul hesabınız için e-posta adresi girin ve seçin **oturum**.

   ![İş veya Okul hesabıyla oturum açın](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. Seçin **bu bilgisayara yeni bir ağ geçidi kaydedin** > **sonraki**, ağ geçidi yüklemenizle birlikte kaydeder [ağ geçidi bulut hizmeti](#gateway-cloud-service). 

   ![Ağ geçidini kaydetme](./media/logic-apps-gateway-install/register-new-gateway.png)

6. Ağ geçidi yüklemeniz için bu bilgileri sağlayın:

   * Yüklemeniz için istediğiniz adı 

   * En az 8 karakter bulunmalı oluşturmak istediğiniz kurtarma anahtarı

     > [!IMPORTANT]
     > Kaydet ve kurtarma anahtarınızı güvenli bir yerde saklayın. Ağ geçidinin konumu değiştiğinde bu anahtara ihtiyacınız veya ne zaman geçirme, kurtarmak veya mevcut bir ağ geçidini.

   * Kurtarma anahtarınızı onayı 

     ![Ağ geçidi ayarlama](./media/logic-apps-gateway-install/set-up-gateway.png)

7. Azure Service Bus'de, ağ geçidi yüklemesi tarafından kullanılır ve ağ geçidi bulut hizmeti için seçtiğiniz bölgeye denetleyin. 

   ![Bölgeyi denetle](./media/logic-apps-gateway-install/check-region.png)

   > [!IMPORTANT]
   > Bu bölge, ağ geçidi yükleme işlemini tamamladıktan sonra değiştirmek için bu ağ geçidi yükleme için kurtarma anahtarı gerekir. Ayrıca, kaldırma ve ağ geçidini yeniden yükleyin. Daha fazla bilgi için [Konum Değiştir, geçirme, kurtarmak veya mevcut ağ geçidini](#update-gateway-installation).

   *Ağ geçidi yüklemenizi için bölge neden değişiyor?* 

   Örneğin, gecikme süresini azaltmak için aynı bölgede mantıksal uygulamanız için ağ geçidinizin bölge değişebilir. 
   Ya da şirket içi veri kaynağınıza en yakın bölgeyi seçebilirsiniz. 
   *Azure'da ağ geçidi kaynağı* ve mantıksal uygulamanızın farklı konumlarda olabilir.

8. Varsayılan bölge kabul etmeyi tercih **yapılandırma**. Veya, varsayılan bölgeyi değiştirmek için aşağıdaki adımları izleyin:

   1. Geçerli bölgeyi yanındaki **bölgeyi Değiştir**. 

      ![Bölgeyi değiştir](./media/logic-apps-gateway-install/change-region.png)

   2. Sonraki sayfada açın **bölge seçin** listesinde, istediğiniz ve seçin bölgeyi seçin **Bitti**.

      ![Başka bir bölge seçin](./media/logic-apps-gateway-install/select-region-gateway-install.png)

9. Onay sayfası görüntülendikten sonra seçin **Kapat**. 

   Yükleyici, ağ geçidi artık çevrimiçi ve kullanılmaya hazır olduğunu doğrular.

   ![Tamamlanmış bir ağ geçidi](./media/logic-apps-gateway-install/finished-gateway-default-location.png)

10. Artık Azure tarafından ağ geçidi kaydetmek [ağ geçidi yüklemeniz için bir Azure kaynağı oluşturma](../logic-apps/logic-apps-gateway-connection.md). 

<a name="update-gateway-installation"></a>

## <a name="change-location-migrate-restore-or-take-over-existing-gateway"></a>Konumu değiştirmek, geçirme, geri yükleme veya mevcut ağ geçidini

Ağ geçidinizin konumunu değiştirmek, ağ geçidi yüklemenizi yeni bir bilgisayara taşımak hasarlı bir ağ geçidini kurtarma veya gerekir var olan bir ağ geçidinin sahipliğini, ağ geçidi yükleme sırasında sağlanan kurtarma anahtarı gerekir. Bu eylem, eski ağ geçidi bağlantısını keser.

1. Kişinin bilgisayarınızda **Denetim Masası**Git **programlar ve Özellikler**. Programlar listesinde **şirket içi veri ağ geçidi**ve ardından **kaldırma**.

2. [Şirket içi veri ağ geçidi yeniden](https://aka.ms/on-premises-data-gateway-installer).

3. Yükleyici açıldıktan sonra aynı iş veya Okul hesabı, daha önce ağ geçidini yüklemek için kullanılan oturum açın.

4. Seçin **geçirme, geri yükleme veya mevcut bir ağ geçidini devralma**ve ardından **sonraki**.

   !["Geçirme, geri yükleme veya mevcut bir ağ geçidini devralma" seçin](./media/logic-apps-gateway-install/migrate-recover-take-over-gateway.png)

5. Altında **kullanılabilir ağ geçitleri** veya **kullanılabilir ağ geçidi kümeleri**, değiştirmek istediğiniz ağ geçidi yüklemesi'ni seçin. Ağ geçidi yüklemesi için kurtarma anahtarını girin. 

   ![Birincil ağ geçidi seçin](./media/logic-apps-gateway-install/select-existing-gateway.png)

6. Bölge seçin **değiştirme bölge** ve yeni bölge.

7. İşiniz bittiğinde seçin **yapılandırma**.

## <a name="configure-proxy-or-firewall"></a>Ara sunucu veya güvenlik duvarı yapılandırma

Şirket içi veri ağ geçidi için bir giden bağlantı oluşturur [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Çalışma ortamınızda trafiği internet'e bir proxy üzerinden gider gerektiriyorsa, bu kısıtlama veri ağ geçidi, ağ geçidi bulut hizmetine bağlanmasını engelleyebilir. Ağınızdaki bir proxy kullanıp kullanmadığını belirlemek için bu makaleye superuser.com gözden geçirin: 

[Kullanıyorum hangi proxy sunucusu olduğunu nasıl öğrenebilirim? (SuperUser.com)](https://superuser.com/questions/346372/how-do-i-know-what-proxy-server-im-using) 

Ağ geçidi için proxy bilgilerini sağlamak için bkz: [proxy ayarlarını yapılandırma](https://docs.microsoft.com/power-bi/service-gateway-proxy). Makinenizin gerçekten internet'e bağlanıp bağlanamayacağını Ara sunucunuzda veya güvenlik duvarı bağlantıları engelleyebilir olup olmadığını denetlemek için onaylayın ve [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Bir PowerShell isteminden şu komutu çalıştırın:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> Bu komut yalnızca ağ bağlantısını ve Azure Service Bus bağlantısını test eder. Ağ geçidi veya şifreler ve kimlik bilgilerini ve ağ geçidi ayrıntıları depolar ağ geçidi bulut hizmeti ile komut yaramıyor. 
>
> Ayrıca, bu komut yalnızca Windows Server 2012 R2 veya daha sonra kullanılabilir ve Windows 8.1 veya üzeri. Önceki işletim sistemi sürümlerinde, Telnet bağlantısını test etmek için kullanabilirsiniz. Daha fazla bilgi edinin [Azure Service Bus ve karma çözümler](../service-bus-messaging/service-bus-messaging-overview.md).

Sonuçlarınızı ile bu örneğe benzemelidir **TcpTestSucceeded** kümesine **True**:

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Varsa **TcpTestSucceeded** ayarlı değil **True**, ağ geçidiniz bir güvenlik duvarı tarafından engelleniyor olabilir. Kapsamlı olmasını istiyorsanız, değiştirin **ComputerName** ve **bağlantı noktası** altında listelenen değerleri değerlerle [bağlantı noktalarını yapılandırma](#configure-ports) bu makaledeki.

Güvenlik Duvarı'nı, ayrıca Azure veri merkezlerine sağlayan Azure Service Bus bağlantıları engelleyebilir. Böyle bir durumda, onaylama (engellemesini kaldırmak) için söz konusu veri merkezleri, bölgenizdeki tüm IP adresleri. Bu IP adresleri için [Azure IP adresleri listesi alma](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="configure-ports"></a>Bağlantı noktalarını yapılandırma

Ağ geçidi için bir giden bağlantı oluşturur [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) ve giden bağlantı noktaları üzerinden iletişim kurar: TCP 443 (varsayılan), 5671, 5672, 9350-9354. Ağ geçidi, gelen bağlantı noktası gerekmez. Daha fazla bilgi edinin [Azure Service Bus ve karma çözümler](../service-bus-messaging/service-bus-messaging-overview.md).

Ağ geçidi bu tam etki alanı adlarını kullanır:

| Etki alanı adları | Giden bağlantı noktaları | Açıklama | 
| ------------ | -------------- | ----------- | 
| *. analysis.windows.net | 443 | HTTPS | 
| *. core.windows.net | 443 | HTTPS | 
| *.frontend.clouddatahub.net | 443 | HTTPS | 
| *.login.windows.net | 443 | HTTPS | 
| *.microsoftonline-p.com | 443 | Yapılandırmasına bağlı olarak kimlik doğrulaması için kullanılır. | 
| *.msftncsi.com | 443 | Ağ geçidi Power BI hizmeti tarafından erişilemez olduğunda internet bağlantısını test etmek için kullanılır. | 
| *.servicebus.windows.net | 443, 9350-9354 | Dinleyiciler (erişim denetimi belirtecinin alınması için 443 gerekir) TCP üzerinden Service Bus geçişi hakkında | 
| *.servicebus.windows.net | 5671-5672 | Gelişmiş ileti sıraya alma Protokolü (AMQP) | 
| login.microsoftonline.com | 443 | HTTPS | 
||||

Bazı durumlarda, Azure Service Bus bağlantıları tam etki alanı adları yerine IP adresleri ile yapılır. Bu nedenle, beyaz liste IP adreslerine duvarınızda veri bölgenize yönelik isteyebilirsiniz. Beyaz liste IP adresleri yerine etki alanı, sizin indirip kullanabilirsiniz [Microsoft Azure veri merkezi IP aralıkları listesini](https://www.microsoft.com/download/details.aspx?id=41653). Bu listedeki IP adreslerini bulunan [sınıfsız etki alanları arası yönlendirme (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösterimi.

### <a name="force-https-communication-with-azure-service-bus"></a>Azure Service Bus ile HTTPS iletişimini zorlama

Bazı Ara sunucular trafiği yalnızca için 80 ve 443 bağlantı noktalarını izin. Varsayılan olarak, Azure Service Bus ile iletişim 443 dışındaki bağlantı noktaları üzerinde gerçekleşir.
Ağ geçidinin Azure Service Bus ile doğrudan TCP yerine HTTPS iletişim kurmak için zorlayabilirsiniz fakat bunu yaparsanız bu nedenle büyük ölçüde performansı düşürebilir. Bu görevi gerçekleştirmek için şu adımları izleyin:

1. Genellikle, burada bulabilirsiniz şirket içi veri ağ geçidi istemci konumuna göz atın: ```C:\Program Files\On-premises data gateway\Microsoft.PowerBI.EnterpriseGateway.exe```

   Aksi takdirde, istemci konumu bulmak için aynı bilgisayardaki Hizmetler konsolunu açın, bulma **şirket içi veri ağ geçidi hizmeti**ve görüntüleme **yürütülebilir dosyanın yolu** özelliği.

2. Açık *yapılandırma* dosyası: **Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config**

3. Değişiklik **ServiceBusSystemConnectivityModeString** değerini **otomatik algıla** için **Https**:

   ```html
   <setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
      <value>Https</value>
   </setting>
   ```

<a name="windows-service-account"></a>

## <a name="windows-service-account"></a>Windows hizmet hesabı

Şirket içi veri ağ geçidini yüklediğiniz bilgisayarda, ağ geçidi "Veri ağ geçidi hizmetini şirket içinde" adlı bir Windows hizmet hesabı olarak çalıştırılır. Ancak, ağ geçidi "NT servıce\pbıegwservice" adı "Farklı Oturum Aç" hesap kimlik bilgilerini kullanır. Varsayılan olarak, ağ geçidi, ağ geçidini yüklediğiniz bilgisayarda "hizmet olarak oturum aç" izinlere sahip. Windows hizmet hesabını ağ geçidi için iş ve şirket içi veri kaynaklarına bağlanmak için kullandığınız hesap ya da Okul hesabı oturum açmak için bulut Hizmetleri için kullandığınız genellikle farklıdır.

Oluşturma ve Azure portalında bir ağ geçidi korumak için bu Windows hizmet hesabı en az olmalıdır **katkıda bulunan** izinleri. Bu izinleri denetlemek için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md). 

<a name="restart-gateway"></a>

## <a name="restart-gateway"></a>Ağ geçidini yeniden başlatma

Veri ağ geçidi bir Windows hizmeti çalışır. Bu nedenle diğer Windows hizmeti gibi başlatabilir ve ağ geçidini çeşitli şekillerde durdurun. Örneğin, ağ geçidinin çalıştığı bilgisayarda yükseltilmiş izinlerle bir komut istemi açın ve her iki komutu çalıştırın:

* Hizmeti durdurmak için şu komutu çalıştırın:
  
  `net stop PBIEgwService`

* Hizmeti başlatmak için şu komutu çalıştırın:
  
  `net start PBIEgwService`

## <a name="tenant-level-administration"></a>Kiracı düzeyinde Yönetim 

Şu anda Kiracı yöneticilerinin diğer kullanıcılar tarafından yüklenmiş ve yapılandırılmış olan tüm ağ geçitlerini yönetebileceği tek bir yer yoktur. Kiracı yöneticisiyseniz kuruluşunuzdaki yükledikleri her ağ geçidi için bir yönetici olarak kullanıcıların sahip olmak isteyebilirsiniz. Bu şekilde, tüm ağ geçitleri, kuruluşunuzun ağ geçidi ayarları sayfasından veya aracılığıyla yönetebilirsiniz [PowerShell komutlarını](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters#powershell-support-for-gateway-clusters). 

<a name="gateway-cloud-service"></a>

## <a name="how-does-the-gateway-work"></a>Ağ geçidi nasıl çalışır?

Veri ağ geçidi mantıksal uygulamanızı, ağ geçidi bulut hizmeti ve şirket içi veri kaynağı arasında hızlı ve güvenli iletişimi kolaylaştırır. Ağ geçidi bulut hizmeti şifreler ve veri kaynağı kimlik bilgileri ve ağ geçidi ayrıntıları depolar. Hizmet ayrıca şirket içi sorgular ve sonuçları mantıksal uygulamanızı, şirket içi veri ağ geçidi ile veri kaynağınız arasında yönlendirir. 

Ağ geçidi, güvenlik duvarları ile çalışır ve yalnızca giden bağlantılar kullanır. Tüm trafiği, ağ geçidi aracının giden trafiği güvenli olarak kaynaklanır. Ağ geçidi, şifrelenmiş kanallarda Azure Service Bus aracılığıyla şirket içi kaynaklardan gelen verileri geçirir. Bu service bus ağ geçidi ve arama hizmeti arasında bir kanal oluşturur, ancak tüm verileri depolar. Ağ geçidi üzerinden geçen tüm veriler şifrelenir.

![Diagram-for-on-Premises-Data-Gateway-Flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Bu adımlar, bulutta bir kullanıcı, şirket içi veri kaynağınıza bağlı bir öğeyle etkileşim kurduğunda gerçekleşen açıklar:

1. Ağ geçidi bulut hizmeti, veri kaynağı için şifrelenmiş kimlik bilgileriyle birlikte bir sorgu oluşturur ve sıra işlemek ağ geçidi için sorgu gönderir.

2. Ağ geçidi bulut hizmeti sorguyu analiz eder ve isteği Azure Service Bus'a gönderir.

3. Şirket içi veri ağ geçidi, Azure Service Bus'ta bekleyen istekler yoklar.

4. Ağ geçidi sorguyu alır, kimlik bilgilerinin şifresini çözer ve bu kimlik bilgileriyle veri kaynağına bağlanır.

5. Ağ geçidi, yürütme için veri kaynağına sorgu gönderir.

6. Sonuçlar veri kaynağından ağ geçidine dön ve ağ geçidi bulut hizmetinde gönderilir. Ağ geçidi bulut hizmeti, ardından sonuçları kullanır.

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="general"></a>Genel

**Q**: Bir ağ geçidi, Azure SQL veritabanı gibi buluttaki veri kaynakları için ihtiyacım var? <br/>
**A**: Hayır, ağ geçidi, yalnızca şirket içi veri kaynaklarına bağlanır.

**Q**: Ağ geçidi veri kaynağı ile aynı makinede yüklü olması gerekiyor mu? <br/>
**A**: Hayır, ağ geçidi, sağlanan bağlantı bilgilerini kullanarak veri kaynağına bağlanır. Ağ geçidini bu anlamda bir istemci uygulaması olarak düşünün. Ağ geçidi, yalnızca sağlanan sunucu adı için bağlama özelliği gerekiyor.

<a name="why-azure-work-school-account"></a>

**Q**: Bir iş veya Okul hesabıyla oturum açmak için neden kullanmalıyım? <br/>
**A**: Şirket içi veri ağ geçidi yüklediğinizde, yalnızca bir iş veya Okul hesabı kullanabilirsiniz. Oturum açma hesabınız Azure Active Directory (Azure AD) tarafından yönetilen bir kiracı depolanır. Genellikle, Azure AD hesabınızın kullanıcı asıl adı (UPN) e-posta adresi ile eşleşir.

**Q**: Kimlik bilgilerim nerede depolanır? <br/>
**A**: Bir veri kaynağı için girdiğiniz kimlik bilgileri şifrelenir ve ağ geçidi bulut hizmetinde depolanır. Kimlik bilgileri, şirket içi veri ağ geçidi şifresi çözülür.

**Q**: Ağ bant genişliği için herhangi bir gereksinimi var mıdır? <br/>
**A**: İyi bir aktarım hızı ağ bağlantınız olduğundan emin olun. Her ortam farklıdır ve gönderilen veri miktarını sonuçlar etkileyebilir. Şirket içi veri kaynağınıza ve Azure veri merkezleri arasında bir üretilen iş düzeyi sağlamak için deneyin [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Aktarım hızınızı ölçmenize yardımcı olması için Azure Speed Test gibi bir dış araç deneyin.

**Q**: Ağ geçidinden bir veri kaynağına sorguları çalıştırmak için gecikme süresi nedir? En iyi mimari nedir? <br/>
**A**: Ağ gecikme süresini azaltmak için ağ geçidi olarak veri kaynağına yakın olabildiğince yükleyin. Ağ geçidini gerçek veri kaynağına yükleyebilirseniz bu yakınlık yükleyebilirseniz gecikme süresi en aza indirir. Ayrıca, Azure veri merkezlerine yakınlık göz önünde bulundurun. Örneğin, Batı ABD veri merkezinde hizmetinizi kullanır ve bir Azure VM'de SQL Server varsa, daha sonra Azure VM'niz Batı ABD bölgesinde çok isteyebilirsiniz. Bu yakınlık gecikme süresini en aza indirir ve Azure sanal makinesinde çıkış ücretlerini ortadan kaldırır.

**Q**: Nasıl sonuçlar buluta nasıl gönderilir? <br/>
**A**: Sonuçlar Azure Service Bus gönderilir.

**Q**: Herhangi bir gelen bağlantı ağ geçidine buluttan var mı? <br/>
**A**: Hayır, ağ geçidinin Azure Service Bus'a yönelik giden bağlantılar kullanır.

**Q**: Peki giden bağlantıları engellersem? Açmak ne gerekiyor? <br/>
**A**: Ağ geçidini kullanan konak ve bağlantı noktalarını bakın.

**Q**: Ne, gerçek Windows hizmetinin adı nedir? <br/>
**A**: Hizmetler sekmesinde Görev Yöneticisi'nde hizmet "PBIEgwService" ya da Power BI Enterprise Gateway Service adıdır. Hizmetler konsolunda, "şirket içi veri Ağ Geçidi Hizmeti" hizmet adıdır. Windows hizmeti "NT servıce\pbıegwservice" hizmet SID'sini (SSID) olarak kullanır.

**Q**: Ağ geçidi Windows hizmetini bir Azure Active Directory hesabıyla çalıştırılabilir mi? <br/>
**A**: Hayır, Windows hizmet geçerli bir Windows hesabı olmalıdır.

### <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

**Q**: Olağanüstü durum kurtarma için hangi seçenekler kullanılabilir? <br/>
**A**: Geri yüklemek veya bir ağ geçidi taşımak için kurtarma anahtarını kullanabilirsiniz. Ağ geçidini yüklerken kurtarma anahtarını belirtin.

**Q**: Kurtarma anahtarının avantajı nedir? <br/>
**A**: Kurtarma anahtarı, olağanüstü bir durumla karşılaştığınızda ağ geçidinizi kurtarmak veya geçirmek için bir yol sağlar.

## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde ayarlama ve şirket içi veri ağ geçidini kullanarak sahip olabileceğiniz bazı yaygın sorunlar ele alır.

**Q**: Neden ağ geçidi yükleme başarısız oldu? <br/>
**A**: Güncel virüsten koruma yazılımı hedef bilgisayara değil Bu sorun oluşabilir. Ya da virüsten koruma yazılımını güncelleştirmek veya virüsten koruma yazılımı yalnızca ağ geçidi yüklemesi sırasında devre dışı bırakın ve yazılım'ı yeniden etkinleştirin.

**Q**: Ağ geçidi kaynağı Azure'da oluşturduğunuzda ağ geçidi yükleme neden göremiyorum? <br/>
**A**: Bu sorun şu nedenlerden kaynaklanabilir:

* Ağ geçidi yüklemenizi zaten kaydedildi ve azure'da başka bir ağ geçidi kaynağı tarafından talep. Ağ geçidi kaynakları için bunları oluşturulduktan sonra ağ geçidi yüklemelerinizi örnekleri listede görünmez.
Ağ geçidi kayıtlarınızı Azure portalında kontrol etmek için tüm Azure kaynaklarınızla gözden **şirket içi veri ağ geçitleri** yazın *tüm* Azure aboneliği. 

* Azure portalında oturum açmıştır kişiye ağ geçidini yükleyen kişinin için Azure AD kimlik farklıdır. Ağ geçidinin yüklü aynı kimliği ile oturum açmış olduğunuzu denetleyin.

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**Q**: Ağ geçidi günlükleri nerededir? <br/>
**A**: Bkz: [ **günlükleri** bölümü](#logs) bu makalenin ilerleyen bölümlerinde.

**Q**: Hangi sorguların gönderildiğini nasıl görebilirim şirket içi veri kaynağına gönderilen? <br/>
**A**: Gönderilen sorgular içeren sorgu izlemeyi etkinleştirebilirsiniz. Sorgu sorun giderme işlemi tamamlandıktan özgün değerine geri izleme değiştirmeyi unutmayın. Sorgu izleme açık bırakarak daha büyük günlükleri oluşturur.

Veri kaynağınızda bulunan, sorguları izlemeye yönelik araçlara da göz atabilirsiniz. Örneğin, SQL Server ve Analysis Services için SQL Profiler veya genişletilmiş olaylar kullanabilirsiniz.

### <a name="outdated-gateway-version"></a>Ağ geçidinin eski sürüm

Ağ geçidi sürümü güncel olmayan hale geldiğinde birçok sorun ortaya çıkabilir. Genel iyi uygulama olarak en son sürümü kullandığınızdan emin olun. Ağ geçidini bir ay veya daha uzun güncelleştirmediyseniz ağ geçidinin en son sürümünü yüklemeyi deneyin ve sorun oluşturamadığınızı.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Hata: Kullanıcı grubuna eklenemedi. (-2147463168 PBIEgwService performans günlük kullanıcılar)

Ağ geçidi desteklenmez ve etki alanı denetleyicisi üzerinde yüklemeye çalıştığınızda bu hatayı alabilirsiniz. Ağ geçidini bir etki alanı denetleyicisi olmayan bir makineye dağıttığınızdan emin olun.

<a name="logs"></a>

### <a name="logs"></a>Günlükler

Gidermenize yardımcı olması için toplama ve ağ geçidi günlükleri gözden her zaman başlatın. Günlükleri toplamaya ilişkin çeşitli yolları vardır, ancak ağ geçidini yükledikten sonra en basit ağ geçidi Yükleyicisi'nin kullanıcı arabirimi aracılığıyla seçenektir. 

1. Bilgisayarınızda, şirket içi veri ağ geçidi yükleyicisini açın.
2. Sol menüden **tanılama**.
3. Altında **ağ geçidi günlükleri**seçin **günlükleri dışarı aktar**.

   ![Ağ geçidi Yükleyicisi'nden günlükleri Dışarı Aktar](./media/logic-apps-gateway-install/export-logs.png)

Çeşitli günlüklerden bulabileceğiniz diğer konumlar şunlardır:

| Günlük türü | Location | 
|----------|----------| 
| **Yükleyici günlükleri** | %LocalAppData%\Temp\On-premises_data_gateway_ <*yyyymmdd*>. <*numarası*> .log | 
| **Yapılandırma günlükleri** | C:\Users\<*kullanıcıadı*> \AppData\Local\Microsoft\On-premises veri gateway\GatewayConfigurator <*yyyymmdd*>. <*numarası*>. Günlük | 
| **Kurumsal ağ geçidi hizmeti günlükleri** | C:\Users\PBIEgwService\AppData\Local\Microsoft\On-Premises veri gateway\Gateway <*yyyymmdd*>. <*numarası*> .log | 
||| 

**Olay günlükleri**

Ağ geçidi için olay günlüklerini bulmak için aşağıdaki adımları izleyin:

1. Ağ geçidi yüklemesi ile bilgisayarda açın **Olay Görüntüleyicisi'ni**. 
2. Genişletin **Olay Görüntüleyicisi'ni (yerel)**  > **uygulama ve hizmet günlükleri**. 
3. Seçin **şirket içi veri ağ geçidi hizmeti**.

   ![Ağ geçidi için olay günlüklerini görüntüleme](./media/logic-apps-gateway-install/event-viewer.png)

### <a name="review-slow-query-performance"></a>Gözden geçirme yavaş sorgu performansı

Sorgular ağ geçidi üzerinden yavaş çalışmasına bulursanız, sorgular ve süreler veren ek günlük kaydını etkinleştirebilirsiniz. Bu günlükler, hangi sorguların yavaş veya uzun süre çalışan bulmanıza yardımcı. Sorgu performansını ayarlamak için olabilir, veri kaynağı örneğin değiştirmek, SQL Server sorguları için dizinleri ayarlama.

Bir sorgu süresini belirlemek için aşağıdaki adımları izleyin:

1. Genellikle, burada bulabilirsiniz ağ geçidi istemci aynı konuma göz atın: ```C:\Program Files\On-premises data gateway```

   Aksi takdirde, istemci konumu bulmak için aynı bilgisayardaki Hizmetler konsolunu açın, bulma **şirket içi veri ağ geçidi hizmeti**ve görüntüleme **yürütülebilir dosyanın yolu** özelliği.

2. Açın ve bu yapılandırma dosyaları açıklandığı gibi düzenleyin:

   * **Microsoft.powerbı.datamovement.Pipeline.gatewaycore.dll.config**

     Bu dosyada değişiklik **Emitquerytraces'in** değerini **false** için **true** ağ geçidinizin ağ geçidinden bir veri kaynağına gönderilen sorguların oturum için:

     ```html
     <setting name="EmitQueryTraces" serializeAs="String">
        <value>true</value>
     </setting>
     ```

     > [!IMPORTANT]
     > Emitquerytraces'in ayarın etkinleştirilmesi, ağ geçidi kullanımına bağlı olarak günlük boyutunu önemli ölçüde artabilir. Günlükleri gözden tamamladıktan sonra Emitquerytraces'in için sıfırlama emin **false** yeniden yerine bu ayarı uzun süreli bırakın.

   * **Microsoft.PowerBI.DataMovement.Pipeline.Diagnostics.dll.config**

     Günlük, süreyi gösteren girişler de dahil olmak üzere ayrıntılı girişleri, ağ geçidiniz için değiştirme **TracingVerbosity** değerini **4** için **5** ya da adım gerçekleştirerek: 

     * Bu yapılandırma dosyasında değişiklik **TracingVerbosity** değerini **4** için **5** 

       ```html
       <setting name="TracingVerbosity" serializeAs="String">
          <value>5</value>
       </setting>
       ```

     * Ağ geçidi yükleyicisini açın, **tanılama**, açma **ek günlükler**ve ardından **Uygula**:

       ![Ek günlük özelliğini açar](./media/logic-apps-gateway-install/turn-on-additional-logging.png)

     > [!IMPORTANT]
     > TracingVerbosity ayarın etkinleştirilmesi, ağ geçidi kullanımına bağlı olarak günlük boyutunu önemli ölçüde artabilir. Günlükleri gözden tamamladıktan sonra emin devre dışı **ek günlükler** ağ geçidi Yükleyicisi'nde veya sıfırlamak için TracingVerbosity **4** yeniden yapılandırma dosyasında yerine bu ayarı açık bırak uzun süreli.

3. Sorgu süresince bulmak için aşağıdaki adımları izleyin:

   1. [Dışarı aktarma](#logs) ve ağ geçidi günlüğünü açın.

   2. Bir sorgu bulmak için örneğin bir etkinlik türü için ara: 

      | Etkinlik türü | Açıklama | 
      |---------------|-------------| 
      | MGEQ | ADO.NET üzerinden çalışan sorguları. | 
      | MGEO | OLEDB üzerinden çalışan sorguları. | 
      | MGEM | Mashup altyapısından çalışan sorguları. | 
      ||| 

   3. Not istek kimliğidir ikinci GUID

   4. Etkinlik türü için "milisaniye cinsinden süre içeren FireActivityCompletedSuccessfullyEvent" adlı bir giriş bulana kadar aramaya devam edin. 
   Giriş örneğin aynı istek Kimliğine sahip olmadığını onaylayın:

      ```text 
      DM.EnterpriseGateway Verbose: 0 : 2016-09-26T23:08:56.7940067Z DM.EnterpriseGateway    baf40f21-2eb4-4af1-9c59-0950ef11ec4a    5f99f566-106d-c8ac-c864-c0808c41a606    MGEQ    21f96cc4-7496-bfdd-748c-b4915cb4b70c    B8DFCF12 [DM.Pipeline.Common.TracingTelemetryService] Event: FireActivityCompletedSuccessfullyEvent (duration=5004)
      ```

      > [!NOTE] 
      > "FireActivityCompletedSuccessfullyEvent" girişini ayrıntılı bir giriştir ve "TracingVerbosity" ayarı düzeyi 5 değilse günlüğe kaydedilmez.

### <a name="trace-traffic-with-fiddler"></a>Fiddler ile izleme trafiği

[Fiddler](https://www.telerik.com/fiddler) telerik HTTP trafiğini izleyen ücretsiz bir araçtır. İstemci makinesinden Power BI hizmeti ile bu trafiği gözden geçirebilirsiniz. Bu hizmet, hataları ve diğer ilgili bilgileri gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar
    
* [Mantıksal uygulamalardan şirket içi verilere bağlanma](../logic-apps/logic-apps-gateway-connection.md)
* [Kurumsal tümleştirme özellikleri](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Azure Logic Apps için bağlayıcılar](../connectors/apis-list.md)
