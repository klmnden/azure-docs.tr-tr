---
title: "Çalışma alanlarını yönetme | Microsoft Belgeleri"
description: "Kullanıcılar, hesaplar, çalışma alanları ve Azure hesapları ile ilgili çeşitli yönetim görevlerini kullanarak Log Analytics&quot;teki çalışma alanlarını yönetin."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/02/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: b78199a672528c475f4f299faaf6406089e95d01


---
# <a name="manage-workspaces"></a>Çalışma alanlarını yönetme

Log Analytics'e erişimi yönetmek için çalışma alanları ile ilgili çeşitli yönetim görevleri gerçekleştirirsiniz. Bu makalede çeşitli hesap türlerini kullanarak çalışma alanlarını yönetmek için kullanacağını en iyi uygulama önerileri ve yordamları verilmektedir. Çalışma alanı, temelde hesap bilgilerini ve hesaba ilişkin basit yapılandırma bilgilerini içeren bir kapsayıcıdır. Siz veya kuruluşunuzun diğer üyeleri, IT altyapınızın tümünden veya bir bölümünden toplanan farklı veri kümelerini yönetmek için birden çok çalışma alanı kullanabilirsiniz.

Bir çalışma alanı oluşturmak için şunlar gereklidir:

1. Bir Azure aboneliğine sahip olmanız.
2. Bir çalışma alanı adı seçmeniz.
3. Çalışma alanını aboneliğinizle ilişkilendirmeniz.
4. Coğrafi bir konum seçmeniz.

## <a name="determine-the-number-of-workspaces-you-need"></a>İhtiyacınız olan çalışma alanı sayısını belirleme
Çalışma alanı, bir Azure kaynağıdır. Bu alan, verilerin toplandığı, derlendiği, çözümlendiği ve Azure portalında sunulduğu bir kapsayıcıdır.

Kullanıcıların bir veya daha çok çalışma alanı için erişime sahip olması amacıyla birden çok çalışma alanı oluşturulabilir. Çalışma alanı sayısının azaltılması, verilerin çoğunu sorgulamanıza ve ilişkilendirmenize olanak tanır. Bu bölümde birden çok çalışma alanı oluşturmanın yararlı olabileceği durumlar açıklanır.

Günümüzde bir çalışma alanı aşağıdakileri sağlar:

* Veri depolama için coğrafi bir konum
* Fatura için ayrıntı düzeyi
* Veri yalıtımı

Yukarıdaki özelliklere bağlı olarak, şu durumlarda birden çok çalışma alanı oluşturmak isteyebilirsiniz:

* Global bir şirketseniz ve veri bağımsızlığı veya uyumluluk nedenleriyle verilerin belirli bölgelerde depolanmasına gerek duyuyorsanız.
* Azure kullanıyorsanız ve çalışma alanını, yönettiği Azure kaynaklarıyla aynı bölgede bulundurarak giden veri aktarımı ücretlerini ortadan kaldırmak istiyorsanız.
* Kullanımlarına dayalı olarak, ücretleri farklı departmanlara veya iş gruplarına göre ayırmak istiyorsanız. Her departman veya iş grubu için bir çalışma alanı oluşturduğunuzda, Azure faturanız ve kullanım ekstreniz her çalışma alanına ilişkin ücretleri ayrı olarak gösterir.
* Yönetilen bir hizmet sağlayıcısıysanız ve yönettiğiniz her bir müşteriye ilişkin Log Analytics verilerini diğer müşterilerin verilerinden yalıtmak istiyorsanız.
* Birden çok müşteriyi yönetiyorsanız ve her bir müşterinin/departmanın/iş grubunun yalnızca kendi verilerini görmesini istiyorsanız.

Verileri toplamak için aracıları kullanıyorsanız her bir aracıyı, bir veya daha fazla çalışma alanına raporlama yapacak şekilde yapılandırabilirsiniz.

System Center Operations Manager'ı kullanıyorsanız her bir Operations Manager yönetim grubu yalnızca bir çalışma alanıyla bağlantılı olabilir. Operations Manager tarafından yönetilen bilgisayarlara Microsoft İzleme Aracısını yükleyebilir ve hem Operations Manager hem de farklı bir Log Analytics çalışma alanı için aracı raporu alabilirsiniz.

### <a name="workspace-information"></a>Çalışma alanı bilgileri

Azure portalında çalışma alanınızla ilgili bilgileri görüntüleyebilirsiniz. Bu bilgileri ayrıca OMS portalından da görüntüleyebilirsiniz.

#### <a name="view-workspace-information-the-azure-portal"></a>Çalışma alanı bilgilerini Azure portalında görüntüleme

1. Önceden yapmadıysanız Azure aboneliğinizi kullanarak [Azure Portal](https://portal.azure.com)'da oturum açın.
2. **Hub** menüsünde **Diğer hizmetler**’e tıklayıp kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girdinize göre filtrelenir. **Log Analytics**’i tıklayın.  
    ![Azure hub'ı](./media/log-analytics-manage-access/hub.png)  
3. Log Analytics abonelikler dikey penceresinden bir çalışma alanı seçin.
4. Çalışma alanı dikey penceresinde çalışma alanıyla ilgili bilgiler ve ek bilgi bağlantıları gösterilir.  
    ![çalışma alanı ayrıntıları](./media/log-analytics-manage-access/workspace-details.png)  


## <a name="manage-accounts-and-users"></a>Hesapları ve kullanıcıları yönetme
Her çalışma alanı kendisiyle ilişkilendirilmiş birden çok kullanıcı hesabı içerebilir ve her kullanıcı hesabı (Microsoft hesabı veya Kuruluş hesabı) birden çok çalışma alanına erişim sahibi olabilir.

Çalışma alanı oluşturmak için kullanılan Microsoft hesabı veya Kuruluş hesabı, varsayılan olarak çalışma alanının yöneticisi olur. Yönetici daha sonra ek Microsoft hesaplarını davet edebilir veya Azure Active Directory'den kullanıcı seçebilir.

Kullanıcılara çalışma alanı erişimi verme işlemi şu iki yerde denetlenir:

* Azure'da, Azure aboneliğine ve ilişkili olduğu Azure kaynaklarına erişim sağlamak için rol tabanlı erişim denetimini kullanabilirsiniz. Bu izinler, aynı zamanda PowerShell ve REST API erişimi için de kullanılır.
* OMS portalında, ilişkili Azure aboneliğine değil, yalnızca OMS portalına erişim vardır.

Kullanıcılar, onlara yalnızca OMS portalına erişim izni verdiğinizde ve bağlantılı Azure aboneliğine erişim izni vermediğinizde Backup ve Site Recovery çözümü kutucuklarındaki verileri görmez.
Tüm kullanıcıların bu çözümlerdeki verileri görebilmeleri için, çalışma alanına bağlı olan Backup Kasası ve Site Recovery kasası için en az bir **okuyucu** erişimine sahip olduklarından emin olun.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Azure portalını kullanarak Log Analytics'e erişimi yönetme
Örneğin, Azure portalında Azure izinlerini kullanarak kullanıcılara Log Analytics erişimi verirseniz aynı kullanıcılar Log Analytics portalına da erişebilir. Kullanıcılar Azure portalında olduklarında, Log Analytics çalışma alanı kaynağını görüntülerken **OMS Portalı** görevine tıklayarak OMS portalına gidebilir.

Azure portalı hakkında dikkate alınması gereken bazı noktalar:

* Bu bir *Rol Tabanlı Erişim Denetimi* değildir. Log Analytics çalışma alanı için Azure portalında *Okuyucu* erişim izinlerine sahipseniz OMS portalını kullanarak değişiklikler yapabilirsiniz. OMS portalında Yönetici, Katkıda Bulunan ve Yalnızca Okuma Erişimi Olan Kullanıcı kavramları vardır. Oturum açtığınız hesap, çalışma alanına bağlı olan Azure Active Directory'de yer alıyorsa OMS portalında Yönetici olursunuz, aksi halde Katkıda Bulunan olursunuz.
* http://mms.microsoft.com adresini kullanarak OMS portalında oturum açtığınızda varsayılan olarak **Çalışma alanı seçin** listesini görürsünüz. Bu listede yalnızca OMS portalı kullanılarak eklenmiş olan çalışma alanları bulunur. Azure abonelikleri ile erişim sahibi olduğunuz çalışma alanlarını görmek için, URL'nin parçası olarak bir kiracı belirtmeniz gerekir. Örneğin:

  `mms.microsoft.com/?tenant=contoso.com` Kiracı tanımlayıcısı, genellikle oturum açmak için kullandığınız e-posta adresinin bu son bölümüdür.
* Oturum açtığınız hesap, Azure Active Directory kiracısında bulunan bir hesapsa OMS portalında *Yönetici* olursunuz. CSP olarak oturum açmadığınız sürece genellikle bu durum geçerli olur.  Hesabınız Azure Active Directory kiracısında değilse OMS portalında bir *Kullanıcı* olursunuz.
* Azure izinlerini kullanarak, doğrudan erişim sahibi olduğunuz bir portala gitmek istiyorsanız URL'nin parçası olarak kaynağı belirtmeniz gerekir. Bu URL'yi PowerShell kullanarak elde edebilirsiniz.

  Örneğin, `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  URL şuna benzer: `https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-the-oms-portal"></a>OMS portalında kullanıcıları yönetme
Ayarlar sayfasının **Hesaplar** sekmesinin altında yer alan **Kullanıcıları Yönet** sekmesinde kullanıcıları ve grupları yönetebilirsiniz.   

![Kullanıcıları yönetme](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Mevcut bir çalışma alanına kullanıcı ekleme
Bir çalışma alanına kullanıcı veya grup eklemek için aşağıdaki adımları kullanın.

1. OMS portalında **Ayarlar** kutucuğuna tıklayın.
2. **Hesaplar** sekmesine ve ardından **Kullanıcıları Yönet** sekmesine tıklayın.
3. **Kullanıcıları Yönet** bölümünde, eklenecek hesap türünü seçin: **Kuruluş Hesabı**, **Microsoft Hesabı** ve **Microsoft Destek**.

   * Microsoft Hesabı seçeneğini belirlerseniz Microsoft Hesabı ile ilişkili kullanıcının e-posta adresini yazın.
   * Kuruluş Hesabı seçeneğini belirlerseniz kullanıcı veya grup adının bir bölümünü ya da e-posta diğer adını girebilirsiniz. Ardından açılan bir kutuda eşleşen kullanıcıların ve grupların listesi görünür. Bir kullanıcı veya grup seçin.
   * Sorun gidermeye yardımcı olması için bir Microsoft Destek mühendisine veya başka bir Microsoft çalışanına, çalışma alanınıza geçici erişim izni vermek üzere Microsoft Destek seçeneğini kullanın.

     > [!NOTE]
     > En iyi performansı elde etmek için, tek bir OMS hesabı ile ilişkili Active Directory gruplarının sayısını; yöneticiler için, katkıda bulunanlar için ve yalnızca okuma erişimi olan kullanıcılar için birer grup olacak şekilde üç grup ile sınırlayın. Daha fazla grubun kullanılması, Log Analytics performansını etkileyebilir.
     >
     >
4. Eklenecek kullanıcının veya grubun türünü seçin: **Yönetici**, **Katkıda Bulunan** ya da **Yalnızca Okuma Erişimi Olan Kullanıcı**.  
5. **Ekle**'ye tıklayın.

   Bir Microsoft hesabı ekliyorsanız sağladığınız e-posta adresine, çalışma alanına katılmaya yönelik bir davet gönderilir. Kullanıcı OMS’ye katılmak için davetteki yönergeleri uyguladıktan sonra, bu çalışma alanına erişebilir.
   Bir kuruluş hesabı ekliyorsanız kullanıcı Log Analytics'e hemen erişebilir.  

#### <a name="edit-an-existing-user-type"></a>Mevcut bir kullanıcı türünü düzenleme
OMS hesabınızla ilişkili bir kullanıcı için hesap rolünü değiştirebilirsiniz. Şu rol seçeneklerine sahipsiniz:

* *Yönetici*: Kullanıcıları yönetebilir, tüm uyarıları görüntüleyip bu uyarılar üzerinde işlem yapabilir ve sunucu ekleyip kaldırabilir
* *Katkıda Bulunan*: Tüm uyarıları görüntüleyip bu uyarılar üzerinde işlem yapabilir ve sunucu ekleyip kaldırabilir
* *Yalnızca Okuma Erişimi Olan Kullanıcı*: Yalnızca okuma erişimine sahip olarak işaretlenen kullanıcılar şunları yapamaz:

  1. Çözüm ekleme/kaldırma. Çözüm galerisi gizlidir.
  2. **Panom** üzerinde kutucuk ekleme/değiştirme/kaldırma.
  3. **Ayarlar** sayfalarını görüntüleme. Sayfalar gizlidir.
  4. Arama görünümünde; Power BI yapılandırması, Kaydedilen Aramalar ve Uyarılar görevleri gizlidir.

#### <a name="to-edit-an-account"></a>Bir hesabı düzenleme
1. OMS portalında **Ayarlar** kutucuğuna tıklayın.
2. **Hesaplar** sekmesine ve ardından **Kullanıcıları Yönet** sekmesine tıklayın.
3. Değiştirmek istediğiniz kullanıcı için rolü seçin.
4. Onay iletişim kutusunda, **Evet**'e tıklayın.

### <a name="remove-a-user-from-a-workspace"></a>Çalışma alanından bir kullanıcıyı kaldırma
Çalışma alanından bir kullanıcıyı kaldırmak için aşağıdaki adımları izleyin. Kullanıcının kaldırılması çalışma alanını kapatmaz. Bunun yerine, kullanıcı ve çalışma alanı arasındaki ilişki kaldırılır. Bir kullanıcı birden çok çalışma alanıyla ilişkilendirilmişse OMS'de oturum açmaya ve diğer çalışma alanlarını görmeye devam eder.

1. OMS portalında **Ayarlar** kutucuğuna tıklayın.
2. **Hesaplar** sekmesine ve ardından **Kullanıcıları Yönet** sekmesine tıklayın.
3. Kaldırmak istediğiniz kullanıcı adının yanındaki **Kaldır** düğmesine tıklayın.
4. Onay iletişim kutusunda, **Evet**'e tıklayın.

### <a name="add-a-group-to-an-existing-workspace"></a>Mevcut bir çalışma alanına grup ekleme
1. Yukarıdaki "Mevcut bir çalışma alanına kullanıcı ekleme" bölümünde yer alan 1 ila 4. adımları uygulayın.
2. **Kullanıcı/Grup Seç** bölümünde **Grup**'u seçin.  
   ![mevcut bir çalışma alanına grup ekleme](./media/log-analytics-manage-access/add-group.png)
3. Eklemek istediğiniz grup için Görünen Ad veya E-posta adresi girin.
4. Liste sonuçlarından grubu seçin ve ardından **Ekle**'ye tıklayın.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Mevcut bir çalışma alanını Azure aboneliğine bağlama
26 Eylül 2016'dan sonra oluşturulan tüm çalışma alanları, oluşturma zamanında bir Azure aboneliğine bağlanmalıdır. Bu tarihten önce oluşturulan çalışma alanları, bir sonraki oturum açışınızda bir çalışma alanına bağlanmalıdır. Çalışma alanını Azure portalından oluşturduğunuzda veya çalışma alanınızı bir Azure aboneliğine bağladığınızda, Azure Active Directory'niz kuruluş hesabınız olarak bağlanır.

![Azure aboneliğini bağlama](./media/log-analytics-manage-access/required-link.png)

> [!IMPORTANT]
> Bir çalışma alanını bağlamak için, Azure hesabınızın bağlamak istediğiniz çalışma alanına erişiminin olması gerekir.  Diğer bir deyişle Azure portalına erişmek için kullandığınız hesap, çalışma alanına erişmek için kullandığınız hesapla **aynı** olmalıdır. Aynı değilse bkz. [Mevcut bir çalışma alanına kullanıcı ekleme](#add-a-user-to-an-existing-workspace).
>
>

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Çalışma alanını OMS portalında bir Azure aboneliğine bağlama
Çalışma alanını OMS portalında bir Azure aboneliğine bağlamak için, oturum açmış olan kullanıcının ücretli bir Azure hesabının olması gerekir.

1. OMS portalında **Ayarlar** kutucuğuna tıklayın.
2. **Hesaplar** sekmesine ve ardından **Azure Aboneliği ve Veri Planı** sekmesine tıklayın.
3. Kullanmak istediğiniz veri planına tıklayın.
4. **Kaydet** düğmesine tıklayın.  
   ![abonelik ve veri planları](./media/log-analytics-manage-access/subscription-tab.png)

Yeni veri planınız, web sayfanızın üst kısmındaki OMS portalı şeridinde görüntülenir.

![OMS şeridi](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Çalışma alanını, Azure portalında bir Azure aboneliğine bağlama
1. [Azure portal](http://portal.azure.com) oturum açın.
2. **Log Analytics**’e göz atın ve ardından bu seçeneği belirleyin.
3. Mevcut çalışma alanlarınızın listesini görürsünüz. **Ekle**'ye tıklayın.  
   ![çalışma alanlarının listesi](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. **OMS Çalışma Alanı** altında, **Or link existing (Veya var olanı bağla)** seçeneğine tıklayın.  
   ![mevcut olanı bağlama](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. **Gerekli ayarları yapılandır**'a tıklayın.  
   ![gerekli ayarları yapılandırma](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Henüz Azure hesabınıza bağlanmamış olan çalışma alanlarının listesini görürsünüz. Bir çalışma alanı seçin.  
   ![çalışma alanları seçme](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. Gerekirse şu öğelere ilişkin değerleri değiştirebilirsiniz:
   * Abonelik
   * Kaynak grubu
   * Konum
   * Fiyatlandırma katmanı  
     ![değerleri değiştirme](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. **Tamam**’a tıklayın. Çalışma alanı artık Azure hesabınıza bağlı.

> [!NOTE]
> Bağlamak istediğiniz çalışma alanını görmüyorsanız Azure aboneliğinizin, OMS web sitesini kullanarak oluşturduğunuz çalışma alanına erişimi yoktur.  OMS portalından bu hesaba erişim izni vermeniz gerekir. Bunu yapmak için bkz. [Mevcut bir çalışma alanına kullanıcı ekleme](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Çalışma alanını ücretli plana yükseltme
OMS için üç çalışma alanı plan türü mevcuttur: **Ücretsiz**, **Tek Başına** ve **OMS**.  *Ücretsiz* plandaysanız bir günde Log Analytics’e gönderilebilecek veriler için üst sınır 500 MB’dir.  Bu miktarı aşarsanız bu sınırın üzerinde veri toplanmasını önlemek için çalışma alanınızı ücretli bir planla değiştirmeniz gerekir. Plan türünüzü istediğiniz zaman değiştirebilirsiniz.  OMS fiyatlandırması hakkında daha fazla bilgi için bkz. [Fiyatlandırma Ayrıntıları](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Bir OMS aboneliğinden gelen destek haklarını kullanma
OMS E1, OMS E2 OMS veya System Center için OMS Eklentisi satın alındıktan sonra sunulan destek haklarını kullanmak için OMS Log Analytics’in *OMS* planını seçin.

Bir OMS aboneliği satın aldığınızda, destek hakları Kurumsal Anlaşmanıza eklenir. Bu anlaşma kapsamında oluşturulan herhangi bir Azure aboneliği bu destek haklarını kullanabilir. Bu, örneğin OMS aboneliklerinden gelen destek haklarını kullanan birden fazla çalışma alanına sahip olmanızı sağlar.

Çalışma alanı kullanımının, OMS aboneliğinden gelen destek haklarınıza uygulandığından emin olmak için şunları yapmanız gerekir:

1. OMS aboneliğini içeren Kurumsal Anlaşmanın parçası olan Azure aboneliğinde çalışma alanınızı oluşturma
2. Çalışma alanı için *OMS* planını seçme

> [!NOTE]
> Çalışma alanınız 26 Eylül 2016’dan önce oluşturulduysa ve Log Analytics fiyatlandırma planınız *Premium* ise, bu çalışma alanı System Center için OMS Eklentisinden gelen destek haklarını kullanır. Destek haklarınızı, *OMS* fiyatlandırma katmanına geçerek de kullanabilirsiniz.
>
>

OMS aboneliği destek hakları, Azure veya OMS portalında görünmez. Destek haklarını ve kullanımı Enterprise Portal'da görebilirsiniz.  

Çalışma alanınızın bağlı olduğu Azure aboneliğini değiştirmeniz gerekiyorsa Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) cmdlet'ini kullanabilirsiniz.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Kurumsal Anlaşmadaki bir Azure Taahhüdünü Kullanma
OMS aboneliğiniz yoksa OMS’nin her bir bileşeni için ayrı olarak ödeme yaparsınız ve kullanım Azure faturanızda görünür.

Azure aboneliklerinizin bağlı olduğu kurumsal kayıt anlaşmasında bir Azure parasal taahhüdünüz varsa herhangi bir Log Analytics kullanımı, kalan parasal taahhüde otomatik olarak eklenir.

Çalışma alanının bağlı olduğu Azure aboneliğini değiştirmeniz gerekiyorsa Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) cmdlet'ini kullanabilirsiniz.  

### <a name="change-a-workspace-to-a-paid-data-plan"></a>Çalışma alanını ücretli veri planı olarak değiştirme
1. [Azure portal](http://portal.azure.com) oturum açın.
2. **Log Analytics**’e göz atın ve ardından bu seçeneği belirleyin.
3. Mevcut çalışma alanlarınızın listesini görürsünüz. Bir çalışma alanı seçin.  
4. **Genel** altındaki çalışma alanı dikey penceresinde **Fiyatlandırma katmanı**’na tıklayın.  
5. **Fiyatlandırma katmanı** altında bir veri planı seçin ve ardından **Seç**'e tıklayın.  
    ![plan seçme](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. Azure portalında görünümü yenilediğinizde, **Fiyatlandırma katmanı**'nın seçtiğiniz plan için güncelleştirildiğini görürsünüz.  
    ![güncelleştirilmiş plan](./media/log-analytics-manage-access/manage-access-change-plan04.png)

## <a name="change-an-azure-active-directory-organization-for-a-workspace"></a>Çalışma alanı için Azure Active Directory Kuruluşunu değiştirme

Bir çalışma alanının Azure Active Directory kuruluşunu değiştirebilirsiniz. Azure Active Directory Kuruluşunun değiştirilmesi, bu dizinden çalışma alanına kullanıcı ve grup eklemenize olanak sağlar.

### <a name="to-change-the-azure-active-directory-organization-for-a-workspace"></a>Çalışma alanındaki Azure Active Directory Kuruluşunu değiştirmek için

1. OMS portalındaki Ayarlar sayfasında **Hesaplar**'a ve ardından **Kullanıcıları Yönet** sekmesine tıklayın.  
2. Kuruluş hesapları ile ilgili bilgileri gözden geçirin ve ardından **Kuruluşu Değiştir** düğmesine tıklayın.  
    ![kuruluşu değiştir](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Azure Active Directory etki alanınızın yöneticisine ilişkin kimlik bilgilerini girin. Daha sonra, çalışma alanınızın Azure Active Directory etki alanınıza bağlandığını belirten bir bildirim görürsünüz.  
    ![bağlanan çalışma alanı bildirimi](./media/log-analytics-manage-access/manage-access-add-adorg02.png)


## <a name="delete-a-log-analytics-workspace"></a>Log Analytics çalışma alanını silme
Bir Log Analytics çalışma alanını sildiğinizde, çalışma alanınızla ilişkili veriler 30 gün içerisinde OMS hizmetinden silinir.

Bir yöneticiyseniz ve çalışma alanıyla ilişkilendirilmiş birden çok kullanıcı varsa bu kullanıcılar ve çalışma alanı arasındaki ilişki kaybolur. Kullanıcılar başka çalışma alanlarıyla ilişkilendirilmişse bu diğer çalışma alanlarıyla OMS'yi kullanmaya devam edebilirler. Ancak, başka çalışma alanlarıyla ilişkili olmamaları durumunda OMS'yi kullanmak için bir çalışma alanı oluşturmaları gerekir.

### <a name="to-delete-a-workspace"></a>Çalışma alanı silmek için
1. [Azure portal](http://portal.azure.com) oturum açın.
2. **Log Analytics**’e göz atın ve ardından bu seçeneği belirleyin.
3. Mevcut çalışma alanlarınızın listesini görürsünüz. Silmek istediğiniz çalışma alanını seçin.
4. Çalışma alanı dikey penceresinde **Sil**’e tıklayın.  
    ![sil](./media/log-analytics-manage-access/delete-workspace01.png)
5. Çalışma alanını silme onayı iletişim kutusunda **Evet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* Aracı eklemek ve veri toplamak için bkz. [Windows bilgisayarlarını Log Analytics'e bağlama](log-analytics-windows-agents.md).
* İşlev eklemek ve veri toplamak için bkz. [Çözüm Galerisinden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md).
* Kuruluşunuz, aracıların Log Analytics hizmetiyle iletişim kurabilmeleri için bir ara sunucu veya güvenlik duvarı kullanıyorsa bkz. [Log Analytics'te ara sunucu ve güvenlik duvarı ayarlarını yapılandırma](log-analytics-proxy-firewall.md).



<!--HONumber=Jan17_HO1-->


