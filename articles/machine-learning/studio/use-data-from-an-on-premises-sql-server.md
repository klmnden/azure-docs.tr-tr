---
title: Azure Machine Learning'de bir şirket içi SQL Server kullanmak | Microsoft Docs
description: Azure Machine Learning ile gelişmiş analizler yapmak için şirket içi SQL Server veritabanından veri kullanın.
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 08e4610d-02b6-4071-aad7-a2340ad8e2ea
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.openlocfilehash: 73b68ec612f10fabe0891bfddfa7783b981642bc
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Şirket içi SQL Server veritabanındaki verileri kullanarak Azure Machine Learning ile gelişmiş analiz gerçekleştirme

[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

Genellikle şirket içi verilerle çalışmak kuruluşların ölçeği ve bunların machine learning iş yükleri için bulut çevikliğini yararlanacak ister misiniz? Ancak, geçerli iş süreçlerini ve iş akışları buluta kendi şirket içi verileri taşıyarak kesintiye istemiyorum. Azure Machine Learning artık bir şirket içi SQL Server veritabanından veri okumak ve ardından eğitim ve bu verileri ile bir model Puanlama destekler. Artık bu el ile kopyalayın ve Bulut ve şirket içi sunucunuz arasında veri eşitlemek gerekmez. Bunun yerine, **veri içeri aktarma** modül Azure Machine Learning Studio'da şimdi okuyabilir, doğrudan, şirket içi SQL Server veritabanından kendi eğitim ve puanlama işleri için.

Bu makalede nasıl giriş için bir genel bakış SQL server verilerini Azure Machine Learning içi sağlar. Çalışma alanları, modüller, veri kümeleri, denemeler, gibi Azure Machine Learning kavramlarını aşina varsayar *vb.*.

> [!NOTE]
> Bu özellik için ücretsiz çalışma alanı kullanılamıyor. Machine Learning fiyatlandırması ve katmanları hakkında daha fazla bilgi için bkz: [Azure Machine Learning fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).
>
>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="install-the-microsoft-data-management-gateway"></a>Microsoft Veri Yönetimi ağ geçidi yükleyin
Azure Machine Learning bir şirket içi SQL Server veritabanına erişmek için Microsoft Veri Yönetimi ağ geçidi yükleyip gerekir. Ağ geçidi bağlantısını Machine Learning Studio'da yapılandırdığınızda, indirmek ve ağ geçidini kullanarak yüklemek için olanağına sahip **karşıdan yükleme ve kaydetme veri ağ geçidi** aşağıda açıklanan iletişim.

İndirme ve MSI Kurulumu paketinden çalıştırarak veri yönetimi ağ geçidi önceden yükleyebilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).
32 bit veya 64-bit, bilgisayarınız için uygun şekilde seçerek en son sürümünü seçin. MSI, en son sürümü için mevcut bir veri yönetimi ağ geçidi korunan tüm ayarlarla yükseltmek için de kullanılabilir.

Ağ geçidi aşağıdaki önkoşullar vardır:

* Desteklenen Windows işletim sistemi sürümleri, Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2 ' dir.
* Önerilen ağ geçidi makinesi için en az 2 GHz, 4 çekirdek, 8 GB RAM ve 80 GB disk yapılandırmadır.
* Konak makine hazırda bekleme, ağ geçidi veri isteklerine yanıt vermiyor. Bu nedenle, ağ geçidi yüklemeden önce bilgisayarda uygun güç planı yapılandırın. Makine hazırda bekleme için yapılandırılmışsa, ağ geçidi yüklemesi bir ileti görüntüler.
* Kopyalama etkinliği belirli bir sıklıkta oluştuğundan makinedeki kaynak kullanımı (CPU, bellek) de en yüksek ve boşta kalma süreleri ile aynı düzeni izler. Kaynak Kullanımı Yoğun bir şekilde de taşınan veri miktarına bağlıdır. Birden çok kopyası işleri devam ederken, kaynak kullanımı yoğun zamanlarda gidebilir girdiğini. Yukarıda listelenen en düşük yapılandırma teknik olarak yeterli olsa da, veri taşıma için belirli yük bağlı olarak en az yapılandırma daha fazla kaynak ile bir yapılandırmaya sahip isteyebilirsiniz.

Ayarlama işlemleri ve veri yönetimi ağ geçidi kullanırken aşağıdakileri göz önünde bulundurun:

* Veri Yönetimi ağ geçidi yalnızca bir örneği tek bir bilgisayara yükleyebilirsiniz.
* Tek bir ağ geçidi birden çok şirket içi veri kaynakları için kullanabilirsiniz.
* Birden çok ağ geçidi farklı bilgisayarlarda aynı şirket içi veri kaynağına bağlanabilir.
* Aynı anda yalnızca bir çalışma alanı için bir ağ geçidi yapılandırın. Şu anda, ağ geçitleri çalışma alanları arasında paylaşılamaz.
* Tek bir çalışma alanı için birden çok ağ geçidi yapılandırabilirsiniz. Örneğin, faaliyete geçirme için hazır olduğunuzda, geliştirme sırasında test veri kaynaklarına bağlı bir ağ geçidi ve bir üretim ağ geçidi kullanmak isteyebilirsiniz.
* Ağ geçidi veri kaynağı ile aynı makinede olması gerekmez. Ancak veri kaynağına yakın kaldığını veri kaynağına bağlanmak ağ geçidi için süreyi azaltır. Şirket içi veri kaynağı kaynaklar için ağ geçidi ve veri kaynağı yok rekabet böylece barındıran olandan farklı bir makinede ağ geçidi yüklemenizi öneririz.
* Ayrı bir ağ geçidi, Power BI veya Azure Data Factory senaryoları hizmet veren, bilgisayarınızda yüklü bir ağ geçidi zaten varsa, Azure Machine Learning için başka bir bilgisayara yükleyin.

  > [!NOTE]
  > Veri Yönetimi ağ geçidi ve Power BI Gateway aynı bilgisayar üzerinde çalıştırılamaz.
  >
  >
* Diğer veri için Azure ExpressRoute kullanıyor olsa bile Azure Machine Learning için veri yönetimi ağ geçidi kullanmanız gerekir. (Bir güvenlik duvarının arkasında olan) bir şirket içi veri kaynağı olarak veri kaynağınız düşünmelisiniz bile ExpressRoute kullandığınızda. Machine Learning ile veri kaynağı arasında bağlantı kurmak için veri yönetimi ağ geçidi kullanın.

Yükleme önkoşulları, yükleme adımlarını ve sorun giderme ipuçları hakkında ayrıntılı bilgi makalesinde bulabilirsiniz [veri yönetimi ağ geçidi](../../data-factory/v1/data-factory-data-management-gateway.md).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Şirket içi SQL Server veritabanınıza Azure Machine Learning giriş verileri
Bu kılavuzda, bir Azure Machine Learning çalışma alanında bir veri yönetimi ağ geçidi kurun ayarlama, yapılandırmak ve bir şirket içi SQL Server veritabanından veri okumak.

> [!TIP]
> Başlamadan önce tarayıcınızın açılır pencere engelleyicisi için devre dışı `studio.azureml.net`. Google Chrome tarayıcısı kullanıyorsanız, karşıdan yükleyip birkaç eklentiler Google Chrome WebStore kullanılabilir birini [kez tıklayın uygulama uzantısı](https://chrome.google.com/webstore/search/clickonce?_category=extensions).
>
>

### <a name="step-1-create-a-gateway"></a>1. adım: bir ağ geçidi oluşturma
İlk adım, oluşturmak ve şirket içi SQL veritabanına erişmek için ağ geçidi kurun ayarlamaktır.

1. Oturum [Azure Machine Learning Studio](https://studio.azureml.net/Home/) ve çalışmak istediğiniz çalışma alanını seçin.
2. Tıklatın **ayarları** dikey penceresinde soldaki ve ardından **DATA GATEWAYS** üst sekmesini.
3. Tıklatın **yeni veri ağ geçidi** ekranın altındaki.

    ![Yeni veri ağ geçidi](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. İçinde **yeni veri ağ geçidi** iletişim kutusunda, girin **ağ geçidi adı** ve isteğe bağlı olarak ekleyin bir **açıklama**. Yapılandırma sonraki adıma gidin alt sağ köşesinde okuna tıklayın.

    ![Ağ geçidi ad ve açıklama girin](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. Karşıdan yükleme ve kaydetme veri ağ geçidi iletişim kutusunda, ağ geçidi kayıt ANAHTARINI panoya kopyalayın.

    ![Karşıdan yükle ve veri ağ geçidini kaydedin](./media/use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>Henüz indirdiğiniz ve Microsoft Veri Yönetimi ağ geçidi yüklü değilse, ardından **indirme veri yönetimi ağ geçidi**. Bu Microsoft Download geçirebileceğiniz indirir, ihtiyacınız olan ağ geçidi sürümü seçin ve yüklemeniz Center götürür. Makale başına bölümlerini yükleme önkoşulları, yükleme adımlarını ve sorun giderme ipuçları hakkında ayrıntılı bilgi bulabilirsiniz [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md) .
7. Ağ geçidi yüklendikten sonra veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni açar ve **kayıt ağ geçidi** iletişim kutusu gösterilir. Yapıştır **ağ geçidi kayıt anahtarı** tıklatın ve Pano kopyalanan **kaydetmek**.
8. Yüklü bir ağ geçidi zaten varsa, veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni çalıştırın. ' I tıklatın **anahtarı Değiştir**, Yapıştır **ağ geçidi kayıt anahtarı** panoya önceki adımda kopyaladığınız ve tıklatın **Tamam**.
9. Yükleme tamamlandığında, **kayıt ağ geçidi** için Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi iletişim kutusu gösterilir. Panoya bir önceki adımda kopyaladığınız ağ geçidi kayıt ANAHTARINI yapıştırın ve tıklatın **kaydetmek**.

    ![Ağ geçidini kaydedin](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. Aşağıdaki değerleri üzerinde ayarlandığında ağ geçidi Yapılandırma tamamlandıktan **giriş** sekmesini Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'nde:

    * **Ağ geçidi adı** ve **örnek adı** ağ geçidi adına ayarlayın.
    * **Kayıt** ayarlanır **kayıtlı**.
    * **Durum** ayarlanır **başlatıldı**.
    * Alt görüntüler durum çubuğu **veri yönetimi ağ geçidi bulut Hizmeti'ne bağlanıldı** yeşil bir onay işareti yanı sıra.

      ![Veri Yönetimi Ağ Geçidi Yöneticisi](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      Kayıt başarılı olduğunda azure Machine Learning Studio de güncelleştirilir.

    ![Ağ geçidi kaydı başarılı](./media/use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. İçinde **indirin ve veri ağ geçidini kaydetmek** iletişim kutusunda, Kurulumu tamamlamak için onay işaretine tıklayın. **Ayarları** sayfasında "Çevrimiçi" olarak ağ geçidi durumunu gösterir. Sağ taraftaki bölmede, durumunu ve diğer yararlı bilgiler bulabilirsiniz.

    ![Ağ Geçidi ayarları](./media/use-data-from-an-on-premises-sql-server/gateway-status.png)
12. Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'nde geçin **sertifika** sekmesi. Bu sekmede belirtilen sertifika kimlik bilgilerini Portalı'nda belirttiğiniz şirket içi veri depolama için şifreleme/şifre çözme için kullanılır. Bu sertifika, varsayılan sertifika yok. Microsoft, bu sertifika yönetim sisteminiz yedeklemek kullanarak kendi sertifikanızı değiştirme önerir. Tıklatın **değişiklik** kullanarak kendi sertifikanızı kullanmayı.

    ![Değişiklik ağ geçidi sertifikası](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. (isteğe bağlı) Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'nde ağ geçidi ile ilgili sorunları gidermek için ayrıntılı günlük kaydını etkinleştirmek istiyorsanız, geçiş için **tanılama** sekmesinde ve denetleme **ayrıntılı etkinleştir sorun giderme amacıyla günlüğü** seçeneği. Günlük bilgilerini Windows Olay Görüntüleyicisi'ni altında bulunabilir **uygulama ve hizmet günlükleri**  - &gt; **veri yönetimi ağ geçidi** düğümü. Aynı zamanda **tanılama** sekmesinde ağ geçidini kullanarak şirket içi veri kaynağına bağlantıyı sınayın.

    ![Ayrıntılı günlük kaydını etkinleştir](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

Bu, Azure Machine Learning ağ geçidi Kurulum işleminde tamamlar.
Artık, şirket içi verilerinizi kullanmak hazırsınız.

Oluşturun ve her çalışma alanı için birden çok ağ geçidi Studio'da ayarlayın. Örneğin, geliştirme sırasında test veri kaynaklarına bağlanmak istediğiniz bir ağ geçidi faklı bir ağ geçidi olarak da üretim veri kaynaklarınız için sahip olabilir. Azure Machine Learning Şirket ortamınıza bağlı olarak birden çok ağ geçidi ayarlamak için esneklik sunar. Şu anda çalışma alanları arasında bir ağ geçidi paylaşamaz ve yalnızca bir ağ geçidi tek bir bilgisayara yüklenebilir. Daha fazla bilgi için bkz: [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>2. adım: bir şirket içi veri kaynağından verileri okumak için ağ geçidi'ni kullanın.
Ağ geçidi kurun ayarladıktan sonra ekleyebileceğiniz bir **veri içeri aktarma** şirket içi SQL Server veritabanındaki verileri girdi bir denemeyi modülü.

1. Machine Learning Studio'da seçin **DENEMELER** sekmesini tıklatın, **+ yeni** sol alt köşesinde ve seçin **boş deneme** (veya birkaç örnek birini seçin denemeler kullanılabilir).
2. Bulma ve sürükleyin **veri içeri aktarma** modülünü deneme tuvaline.
3. Tıklatın **Farklı Kaydet** tuvale aşağıda. "Azure Machine Learning şirket içi SQL Server Öğreticisi" deneme adını girin, çalışma alanını seçin ve tıklatın **Tamam** onay işareti.

   ![Deneme Yeni adla kaydet](./media/use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. ' I tıklatın **veri içeri aktarma** , daha sonra seçmek için Modülü **özellikleri** bölmesi tuvalin sağındaki Seç "Şirket içi SQL veritabanı" **veri kaynağı** açılır liste.
5. Seçin **veri ağ geçidi** yüklü ve kayıtlı. Başka bir ağ geçidi kurun "(yeni veri ağ geçidi Ekle...)"'i seçerek ayarlayabilirsiniz.

   ![Veri içeri aktarma modülü için veri ağ geçidi seçin](./media/use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. SQL girin **veritabanı sunucusu adı** ve **veritabanı adı**, SQL birlikte **veritabanı sorgusu** yürütmek istiyor.
7. Tıklatın **değerleri girin** altında **kullanıcı adı ve parola** ve veritabanı kimlik bilgilerinizi girin. Windows tümleşik kimlik doğrulaması veya SQL Server şirket içi SQL Server'ınızdaki nasıl yapılandırıldığına bağlı olarak kimlik doğrulaması kullanabilirsiniz.

   ![Veritabanı kimlik bilgilerini girin](./media/use-data-from-an-on-premises-sql-server/database-credentials.png)

   Yeşil onay işaretiyle "değerlerini Ayarla" iletisi "gerekli değerler" geçer. Veritabanı bilgileri veya parola değiştirilmediği sürece kimlik bilgilerini bir kez girmeniz yeterlidir. Azure Machine Learning bulutta kimlik bilgilerini şifrelemek için ağ geçidi yüklendiğinde, sağlanan sertifika kullanır. Azure hiçbir zaman şifreleme olmadan şirket içi kimlik bilgilerini depolar.

   ![Veri modülü özelliklerini alma](./media/use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. Tıklatın **çalıştırmak** denemeyi çalıştırmak için.

Denemeyi çalıştırma tamamlandığında, çıkış bağlantı noktasına tıklayarak veritabanından alınan veri görselleştirebilirsiniz **veri içeri aktarma** modülü ve seçerek **Görselleştir**.

Denemenizi geliştirme tamamladıktan sonra dağıtın ve modelinizi faaliyete. Toplu yürütme hizmeti, şirket içi SQL Server verilerini kullanarak veritabanı yapılandırılan **veri içeri aktarma** modülü okuma ve puanlama için kullanılır. Puanlama veri şirket için istek yanıtı hizmeti kullanabilirsiniz, ancak Microsoft önerir kullanarak [Excel eklentisi](excel-add-in-for-web-services.md) yerine. Şu anda bir şirket içi SQL Server veritabanına yoluyla yazma **verileri dışa aktar** denemelerinizi içinde ya da desteklenen veya yayımlanmış web hizmetleri değil.
