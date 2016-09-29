<properties
    pageTitle="Log Analytics ile Çalışmaya Başlama | Microsoft Azure"
    description="Microsoft Operations Management Suite'te (OMS) Log Analytics'i birkaç dakikada kullanmaya başlayabilirsiniz."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/09/2016"
    ms.author="banders"/>



# Log Analytics'i Kullanmaya Başlama

Microsoft Operations Management Suite'te (OMS) Log Analytics'i birkaç dakikada kullanmaya başlayabilirsiniz. Hesap oluştururken olduğu gibi, OMS çalışma alanı oluştururken de iki seçeneğiniz vardır:

- Microsoft Operations Management Suite web sitesi
- Microsoft Azure aboneliği

OMS web sitesini kullanarak ücretsiz bir OMS çalışma alanı oluşturabilirsiniz. Veya bir OMS çalışma alanı oluşturmak için Microsoft Azure aboneliğini kullanabilirsiniz. Ücretsiz bir OMS çalışma alanının OMS hizmetine günlük olarak en fazla 500 MB veri gönderebilmesi dışında, her iki çalışma alanı da işlevsel olarak eşdeğerdir. Bir Azure aboneliği kullanıyorsanız bu aboneliği aynı zamanda diğer Azure hizmetlerine erişmek için de kullanabilirsiniz. Çalışma alanını oluşturmak için kullandığınız yöntemden bağımsız olarak, çalışma alanını bir Microsoft hesabıyla veya bir kuruluş hesabıyla oluşturursunuz.

Bu işlemle ilgili adımlar:

![Diyagram ekleme](./media/log-analytics-get-started/oms-onboard-diagram.png)

## Log Analytics önkoşulları ve dağıtım ile ilgili dikkat edilmesi gerekenler

- Log Analytics'i tam olarak kullanmak için ücretli bir Microsoft Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa istediğiniz Azure hizmetine erişmenizi sağlayan [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. Alternatif olarak, [Operations Management Suite](http://microsoft.com/oms) web sitesinde ücretsiz bir OMS hesabı oluşturabilir ve **Ücretsiz deneyin**'e tıklayabilirsiniz.
- OMS çalışma alanı
- Veri toplamak istediğiniz her Windows bilgisayarının Windows Server 2008 SP1 veya sonraki bir sürümünü çalıştırması gerekir
- OMS web hizmeti adreslerine yönelik [Güvenlik Duvarı](log-analytics-proxy-firewall.md) erişimi
- Bilgisayarlarda İnternet erişiminin olmaması durumunda, trafiği sunuculardan OMS'ye iletmek için bir [OMS Log Analytics İleticisi](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (Ağ Geçidi) sunucusu
- Operations Manager kullanıyorsanız Log Analytics, Operations Manager 2012 SP1 UR6 ile sonraki bir sürümünü ve Operations Manager 2012 R2 UR2 ile sonraki bir sürümünü destekler. Operations Manager 2012 SP1 UR7 ve Operations Manager 2012 R2 UR3'e ara sunucu desteği eklenmiştir. OMS ile nasıl tümleştirileceğini belirleyin.
- Bilgisayarlarınızın doğrudan İnternet erişiminin olup olmadığını belirleyin. Doğrudan İnternet erişimi yoksa OMS web hizmeti sitelerine erişim için bir ağ geçidi sunucusu gerekir. Tüm erişim HTTPS üzerinden gerçekleşir.
- Hangi teknolojilerin ve sunucuların OMS'ye veri göndereceğini belirleyin. Örneğin, etki alanı denetleyicileri, SQL Server vb.
- OMS ve Azure'daki kullanıcılara izin verin.
- Veri kullanımı ile ilgili endişeleriniz varsa her bir çözümü ayrı olarak dağıtın ve ek çözümler eklemeden önce performans etkisini test edin.
- Log Analytics'e çözüm ve özellik eklerken veri kullanımınızı ve performansınızı gözden geçirin. Buna olay koleksiyonu, günlük koleksiyonu, performans verileri koleksiyonu vb. dahildir. Veri kullanımı ve performans etkisi tanımlanana kadar minimal bir koleksiyonla başlamak daha uygundur.
- Windows aracılarının da Operations Manager kullanılarak yönetilmediğini doğrulayın, aksi halde yinelenen veriler ortaya çıkar. Bu, Azure Tanılama'nın etkin olduğu Azure tabanlı aracılar için de geçerlidir.
- Aracıları yükledikten sonra, aracının düzgün çalıştığını doğrulayın. Düzgün çalışmıyorsa, Şifreleme API'si: Yeni Nesil (CNG) Anahtar Yalıtımı'nın Grup İlkesi yoluyla devre dışı bırakılmadığından emin olun.
- Bazı Log Analytics çözümlerinin ek gereksinimleri vardır



## Operations Management Suite'i kullanarak 3 adımda kaydolma

1. [Operations Management Suite](http://microsoft.com/oms) web sitesine gidin ve **Ücretsiz deneyin**'e tıklayın. Şirketiniz veya eğitim kurumunuz tarafından Office 365 veya diğer Microsoft hizmetleriyle kullanmanız için sağlanmış olan Outlook.com veya kuruluş hesabı gibi bir Microsoft hesabıyla oturum açın.
2. Benzersiz bir çalışma alanı adı sağlayın. Çalışma alanı, yönetim verilerinizin depolandığı mantıksal bir kapsayıcıdır. Veriler kendi çalışma alanına özel olduğundan, çalışma alanı; verileri, kuruluşunuzdaki farklı ekipler arasında bölümlendirmenizi sağlar. Verilerinizin depolanmasını istediğiniz bir e-posta adresi ve bölge belirtin.  
    ![çalışma alanı oluşturma ve aboneliği bağlama](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Ardından, yeni bir Azure aboneliği oluşturabilir veya mevcut bir Azure aboneliğine bağlanabilirsiniz. Ücretsiz Deneme Sürümünü kullanmaya devam etmek istiyorsanız **Şimdi Değil**'e tıklayın.  
  ![çalışma alanı oluşturma ve aboneliği bağlama](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Operations Management Suite portalı ile çalışmaya hazırsınız.

[Log Analytics'e erişimi yönetme](log-analytics-manage-access.md) bölümünde çalışma alanınızı ayarlama ve mevcut Azure hesaplarını Operations Management Suite ile oluşturulan çalışma alanlarına bağlama konusunda daha fazla bilgi edinebilirsiniz.

## Microsoft Azure'ı kullanarak hızlı kaydolma

1. [Azure portalına](https://portal.azure.com) gidin ve oturum açın, hizmetler listesine göz atın ve ardından **Log Analytics (OMS)** seçeneğini belirleyin.  
    ![Azure portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. **Ekle**'ye tıklayın, ardından şu öğeler için seçim yapın:
    - **OMS Çalışma Alanı** adı
    - **Abonelik** - Birden çok aboneliğiniz varsa yeni çalışma alanıyla ilişkilendirmek istediğiniz aboneliği seçin.
    - **Kaynak grubu**
    - **Konum**
    - **Fiyatlandırma katmanı**  
        ![hızlı oluşturma](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. **Oluştur**'a tıklayın, ardından Azure portalındaki çalışma alanı ayrıntılarını göreceksiniz.       
    ![çalışma alanı ayrıntıları](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Yeni çalışma alanınızla Operations Management Suite web sitesini açmak için **OMS Portalı** bağlantısına tıklayın.

Operations Management Suite portalını kullanmaya hazırsınız.

[Log Analytics'e erişimi yönetme](log-analytics-manage-access.md) bölümünde çalışma alanınızı ayarlama ve Operations Management Suite ile oluşturduğunuz mevcut çalışma alanlarını Azure aboneliklerine bağlama konusunda daha fazla bilgi edinebilirsiniz.

## Operations Management Suite portalı ile çalışmaya başlama
Çözümleri seçmek ve yönetmek istediğiniz sunucuları bağlamak için **Ayarlar**'a tıklayın ve bu bölümdeki adımları izleyin.  

![başlarken](./media/log-analytics-get-started/oms-onboard-get-started.png)  

- **Çözüm ekleme** - Kullanmak istediğiniz çözümleri seçin ve ardından **Add selected Solutions (Seçilen Çözümleri Ekle)** düğmesine tıklayın.  
    ![çözümler](./media/log-analytics-get-started/oms-onboard-solutions.png)
- **Veri kaynağı bağlama** - Veri toplamak için sunucu ortamınıza nasıl bağlanmak istediğinizi seçin:
    - Aracı yükleyerek herhangi bir Windows Sunucusunu veya istemciyi doğrudan bağlayın.
    - Yönetim gruplarınızı veya tüm Operations Manager dağıtımınızı eklemek için System Center Operations Manager'ı kullanın.
    - Windows veya Linux Azure Tanılama VM uzantısıyla yapılandırılmış bir Azure depolama hesabı kullanın.
        ![veri kaynakları](./media/log-analytics-get-started/oms-onboard-data-sources.png)    
- **Günlük ekleme** Verilerinizi doldurmak için en az bir veri kaynağını yapılandırın ve ardından **Kaydet**'i seçin. Olay günlükleri için hata, uyarı ve izlenecek bilgiler de dahil olmak üzere ileti türünü belirtebilirsiniz.    

    ![günlükler](./media/log-analytics-get-started/oms-onboard-logs.png)    


## İsteğe bağlı olarak, aracı yükleyerek sunucuları doğrudan Operations Management Suite'e bağlama
1. Yüklemek istediğiniz bilgisayarın mimarisi için **Ayarlar** kutucuğuna tıklayın, **Bağlı Kaynaklar** sekmesine ve ardından **Windows Agent'ı İndir**'e tıklayın. Aracıyı yalnızca Windows Server 2008 SP 1 ve sonraki veya Windows 7 SP1 veya sonraki işletim sistemlerine yükleyebilirsiniz.
2. Aracıyı bir veya daha fazla sunucuya yükleyin. Aracıları teker teker veya [özel komut dosyası](log-analytics-windows-agents.md) ile daha otomatikleştirilmiş bir yöntem kullanarak yükleyebilir veya sahip olduğunuz mevcut bir yazılım dağıtımı çözümünü kullanabilirsiniz.
3. Lisans sözleşmesini kabul ettikten ve yükleme klasörünüzü seçtikten sonra, **Connect the agent to Microsoft Azure Operational Insights (Aracıyı Microsoft Azure Operasyonel Öngörüler'e bağla)** seçeneğini belirleyin. (OMS önceden Operasyonel Öngörüler olarak adlandırılıyordu).  
    ![aracı kurulumu](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Sonraki sayfada, sizden Çalışma Alanı Kimliğiniz ve Çalışma Alanı Anahtarınız istenir. Çalışma Alanı kimliğiniz ve anahtarınız, aracı dosyasını indirdiğiniz ekranda görüntülenir.  
    ![aracı anahtarları](./media/log-analytics-get-started/oms-onboard-mma-keys.png)
    ![sunucuları ekleme](./media/log-analytics-get-started/oms-onboard-key.png)
5. Yükleme sırasında, isteğe bağlı olarak ara sunucunuzu ayarlamak ve kimlik doğrulama bilgilerini sağlamak için **Gelişmiş**'e tıklayabilirsiniz. Çalışma alanı bilgi ekranına geri dönmek için **İleri** düğmesine tıklayın.
6. Çalışma Alanı Kimliğinizi ve Anahtarınızı doğrulamak için **İleri**'ye tıklayın. Herhangi bir hata bulunması durumunda, düzeltme yapmak için **Geri**'ye tıklayabilirsiniz. Çalışma Alanı Kimliğiniz ve Anahtarınız doğrulandığında, aracı yüklemesini tamamlamak için **Yükle**'ye tıklayın.
7. Operations Management Suite portalında yeniden oturum açın ve Genel Bakış sayfasındaki **Ayarlar** kutucuğuna tıklayın. Aracılar Operations Management Suite hizmetiyle iletişim kurduğunda yeşil bir onay işareti simgesi görünür. Başlangıçta, bu yaklaşık 5-10 dakika sürer.

>[AZURE.NOTE] Kapasite yönetimi ve yapılandırma değerlendirmesi çözümleri şu anda doğrudan Operations Management Suite'e bağlı sunucular tarafından desteklenmemektedir.


Aracıyı aynı zamanda System Center Operations Manager 2012 SP1 ve sonraki bir sürümüne de bağlayabilirsiniz. Bunu yapmak için, **Connect the agent to System Center Operations Manager (Aracıyı System Center Operations Manager'a bağla)** seçeneğini belirleyin. Bu seçeneği belirlediğinizde, yönetim gruplarınızda herhangi bir ek donanım veya yük gerekmeksizin verileri hizmete göndermiş olursunuz.

Aracıları Operations Management Suite'e bağlama konusunda daha fazla bilgi için bkz. [Windows bilgisayarlarını Log Analytics'e bağlama](log-analytics-windows-agents.md).

## İsteğe bağlı olarak, System Center Operations Manager kullanarak sunucuları bağlama

1. Operations Manager konsolunda **Yönetim**'i seçin.
2. **Operasyonel Öngörüler** düğümünü genişletin ve **Operasyonel Öngörüler Bağlantısı**'nı seçin.

  >[AZURE.NOTE] Hangi SCOM Güncelleştirme Paketini kullandığınıza bağlı olarak, *System Center Advisor*, *Operasyonel Öngörüler* veya *Operations Management Suite* için bir düğüm görebilirsiniz.

3. Sağ üst kısımda yer alan **Operasyonel Öngörülere Kaydol** bağlantısına tıklayın ve yönergeleri uygulayın.
4. Kayıt sihirbazını tamamladıktan sonra, **Bilgisayar/Grup Ekle** bağlantısına tıklayın.
5. **Bilgisayar Araması** iletişim kutusunda Operations Manager tarafından izlenen bilgisayarları veya grupları arayabilirsiniz. Log Analytics'e eklemek için bilgisayarları veya grupları seçin, **Ekle**'ye tıklayın ve ardından **Tamam**'a tıklayın. OMS hizmetinin veri aldığını, Operations Management Suite portalındaki **Kullanım** kutucuğuna giderek doğrulayabilirsiniz. Veriler yaklaşık 5-10 dakika içerisinde görünmelidir.

Operations Manager'ı Operations Management Suite'e bağlama hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics'e Bağlama](log-analytics-om-agents.md).

## İsteğe bağlı olarak, Microsoft Azure'da bulut hizmetlerinden veri çözümleme

Operations Management Suite kullanarak, Azure Bulut Hizmetleri için tanılamayı etkinleştirip bulut hizmetleri ve sanal makinelere ilişkin olay ve IIS günlüklerini hızlıca arayabilirsiniz. Ayrıca Microsoft İzleme Aracısı'nı yükleyerek Azure sanal makineleriniz için ek öngörüler elde edebilirsiniz. Operations Management Suite'i kullanmak için Azure ortamınızı nasıl yapılandırabileceğiniz hakkında daha fazla bilgi için bkz. [Azure depolama alanını Log Analytics'e bağlama](log-analytics-azure-storage.md).


## Sonraki adımlar

- İşlev eklemek ve veri toplamak için bkz. [Çözüm Galerisinden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md).
- Çözümler tarafından toplanan ayrıntılı bilgileri görüntülemek için [günlük aramaları](log-analytics-log-searches.md) hakkında bilgi edinin.
- Özel aramalarınızı kaydetmek ve görüntülemek için [panolar](log-analytics-dashboards.md)ı kullanın.



<!--HONumber=Sep16_HO3-->


