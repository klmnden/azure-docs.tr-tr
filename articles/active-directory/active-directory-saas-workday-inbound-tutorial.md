---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı için Workday yapılandırma | Microsoft Docs"
description: "Active Directory ve Azure Active Directory için Workday kimlik veri kaynağı olarak kullanmayı öğrenin."
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/26/2018
ms.author: asmalser
ms.openlocfilehash: 976d7e7cb304a24f235e51952ce04826776e2789
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning"></a>Öğretici: otomatik kullanıcı sağlamayı için Workday yapılandırın

Bu öğreticinin amacı kişiler Workday için bazı özniteliklerin isteğe bağlı geri yazma ile Active Directory ve Azure Active Directory'de Workday'deki alma işlemini gerçekleştirmek için gereken adımları Göster sağlamaktır. 



## <a name="overview"></a>Genel Bakış

[Hizmet sağlama Azure Active Directory kullanıcı](active-directory-saas-app-provisioning.md) ile tümleştirilir [Workday İnsan Kaynakları API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) kullanıcı hesaplarını sağlamak için. Azure AD iş akışları sağlama aşağıdaki kullanıcı etkinleştirmek için bu bağlantı kullanır:

* **Active Directory kullanıcılara sağlama** -Workday kullanıcı seçili kümeleri bir veya daha fazla Active Directory ormanları eşitleyin. 

* **Azure Active Directory salt bulut kullanıcılara sağlama** -ikinci kullanarak Active Directory ve Azure Active Directory içinde mevcut karma kullanıcılar sağlanabilir [AAD Connect](connect/active-directory-aadconnect.md). Ancak, salt bulut olan kullanıcılar, Azure hizmet sağlama Azure AD kullanıcısının kullanarak Active Directory için doğrudan Workday'den sağlanabilir.

* **Geri yazma e-posta adresleri için Workday** -hizmet sağlama Azure AD kullanıcı yazma seçilen Azure AD kullanıcı özniteliklerini e-posta adresi gibi Workday dön.

### <a name="what-human-resources-scenarios-does-it-cover"></a>Hangi İnsan Kaynakları senaryolarını ele alınmamaktadır?

Azure AD kullanıcı sağlama hizmeti tarafından desteklenen Workday kullanıcı sağlama iş akışları şu İnsan Kaynakları ve kimlik yaşam döngüsü yönetimi senaryoları etkinleştirin:

* **Yeni çalışan işe alma** - yeni bir çalışan iş günü için eklendiğinde, bir kullanıcı hesabı otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak oluşturulur ve [AzureADtarafındandesteklenendiğerSaaSuygulamaları](active-directory-saas-app-provisioning.md), Workday için e-posta adresinin sonradan yazma ile.

* **Çalışan özniteliği ve profil güncelleştirmeleri** - zaman (kendi adı, başlık veya Yöneticisi gibi) bir çalışan kaydı iş günü içinde güncelleştirilir, kendi kullanıcı hesabına otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak güncelleştirilir ve [Azure AD tarafından desteklenen diğer SaaS uygulamaları](active-directory-saas-app-provisioning.md).

* **Çalışan sonlandırmalar** - çalışan iş günü içinde sonlandırıldığında kendi kullanıcı hesabına otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak devre dışıdır ve [Azure tarafından desteklenen diğer SaaS uygulamaları AD](active-directory-saas-app-provisioning.md).

* **Çalışan yeniden anlaşır** - çalışan iş günü içinde rehired eski hesaplarında bulunabilir otomatik olarak yeniden veya Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve (tercihinizebağlıolarak)yenidensağlanan[Azure AD tarafından desteklenen diğer SaaS uygulamaları](active-directory-saas-app-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>İçin en iyi bu kullanıcı sağlama çözümünü kim uygundur?

Çözüm sağlama bu Workday kullanıcı şu anda genel önizlemede olan ve ideal için uygundur:

* Workday'den kullanıcı hazırlama için önceden derlenmiş, bulut tabanlı bir çözüm işlemleriniz kuruluşlar

* Active Directory veya Azure Active Directory Workday'den doğrudan kullanıcı hazırlama gerektiren kuruluşların

* Kullanıcılar verileri kullanarak sağlanacak gerektiren kuruluşların elde Workday HCM modülünden (bkz [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html)) 

* Kuruluşlar gerektiren birleştirme, taşıma ve bir veya daha fazla Active Directory ormanları eşitlenen kullanıcılar bırakarak, etki alanları ve OU'lar yalnızca temel bilgileri Workday HCM modülünde algılandı değiştirin (bkz [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Office 365 için e-posta kullanarak kuruluşlar


## <a name="planning-your-solution"></a>Çözümünüzü planlarken

Workday entegrasyonu başlamadan önce aşağıdaki önkoşulları denetleyin ve geçerli bir Active Directory mimarisi ve Azure Active Directory tarafından sağlanan solution(s) ile gereksinimleri sağlama kullanıcı eşleşmesi konusunda aşağıdaki yönergeleri okuyun.

### <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Genel yönetici erişimi olan geçerli bir Azure AD Premium P1 abonelik
* Sınama ve tümleştirme amaçları için bir iş günü uygulama Kiracı
* Test amacıyla çalışan verilerini test etmek için yönetici izinleri sistem tümleştirme kullanıcı oluşturmak ve değişiklikler yapmak için iş günü içinde
* Active Directory'ye kullanıcı sağlamak için 2012 veya daha fazla Windows hizmetini çalıştıran bir etki alanına katılmış sunucuya konağa gereklidir [şirket içi eşitleme Aracısı](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](connect/active-directory-aadconnect.md) Active Directory ve Azure AD arasında eşitlemek için

### <a name="solution-architecture"></a>Çözüm mimarisi

Azure AD sağlama ve kimlik Yaşam Döngüsü Yönetimi'nden Workday Active Directory, Azure AD, SaaS uygulamaları için ve ötesinde gidermenize yardımcı olacak bağlayıcılar sağlama zengin bir özellik kümesi sağlar. Hangi özellikleri kullanır ve çözümü ayarlama nasıl kuruluşunuzun ortamlarına ve gereksinimlerine bağlı olarak farklılık gösterir. İlk adım olarak, stok mevcut ve kuruluşunuzdaki dağıtılan kaç aşağıdakilerden birini gerçekleştirin:

* Kaç tane Active Directory ormanları kullanılıyor?
* Kaç tane Active Directory etki alanları kullanılıyor?
* Kaç tane Active Directory kuruluş birimlerini (OU) kullanılıyor?
* Kaç tane Azure Active Directory kiracıları kullanılıyor?
* Active Directory ve Azure Active Directory (örneğin "karma" kullanıcılar) için sağlanması gereken kullanıcılar var mı?
* Azure Active Directory, ancak Active Directory değil (örneğin "yalnızca bulut" kullanıcılar) için sağlanması gereken kullanıcılar var mı?
* Kullanıcı e-posta adresleri için Workday geri yazılması gerekiyor mu?


Bu soruların yanıtları olduktan sonra aşağıdaki yönergeleri izleyerek dağıtım sağlama İş gününüzün planlayabilirsiniz.

#### <a name="using-provisioning-connector-apps"></a>Sağlama bağlayıcı uygulamaları kullanma

Azure Active Directory Workday ve çok sayıda diğer SaaS uygulamaları için önceden tümleştirilmiş sağlama bağlayıcıları destekler. 

Tek bir sağlama bağlayıcı tek kaynak sistemi API'si ile arabirimleri ve sağlama verilerini tek hedef sisteme yardımcı olur. Azure AD destekleyen çoğu sağlama bağlayıcılar tek bir kaynak ve hedef sisteme (örneğin Azure AD'ye ServiceNow) içindir ve uygulama ekleyerek Azure AD uygulama galerisinde (örneğin ServiceNow) söz konusu ayarlanabilir. 

Azure AD'de sağlama bağlayıcı örnekleri ve app örnekleri arasında bire bir ilişki vardır:

| Kaynak sistemi | Hedef sistem |
| ---------- | ---------- | 
| Azure AD kiracısı | SaaS uygulaması |


Ancak, Workday ve Active Directory ile çalışırken, olarak kabul edilmesi için birden çok kaynak ve hedef sistemleri vardır:

| Kaynak sistemi | Hedef sistem | Notlar |
| ---------- | ---------- | ---------- |
| İş günü | Active Directory Ormanı | Her bir orman ayrı hedef sistem olarak kabul edilir |
| İş günü | Azure AD kiracısı | Yalnızca bulut kullanıcıları için gerektiği şekilde |
| Active Directory Ormanı | Azure AD kiracısı | Bu akış AAD Connect tarafından bugün işlenir |
| Azure AD kiracısı | İş günü | E-posta adreslerini geri yazma için |

Bu birden çok iş akışı birden çok kaynak ve hedef sistemlere kolaylaştırmak için Azure AD Azure AD uygulama galerisinde ekleyebilirsiniz birden fazla sağlama bağlayıcı uygulama sağlar:

![AAD uygulama Galerisi](./media/active-directory-saas-workday-inbound-tutorial/WD_Gallery.PNG)

* **Active Directory sağlama iş günü** -bu uygulama için tek bir Active Directory ormanı Workday'den hesabı kullanıcı hazırlama kolaylaştırır. Birden çok ormanınız varsa, bu uygulamanın bir örneği için sağlamak için gereken her Active Directory ormanı için Azure AD uygulama galerisinde ekleyebilirsiniz.

* **Azure AD sağlama için iş günü** - Azure Active Directory, bu uygulama bir kullanıcıya, Workday yalnızca bulut kullanıcıları için tek bir Azure sağlanmasını kolaylaştırmak için kullanılabilir Active Directory eşitleme için kullanılması gereken aracı olsa da AAD Connect Active Directory kiracısı.

* **İş günü geri yazma** -bu uygulamayı Azure Active Directory'den iş günü, kullanıcının e-posta adresi geri yazma kolaylaştırır.

> [!TIP]
> Normal "Workday" uygulaması, çoklu oturum açma Workday ve Azure Active Directory arasında ayarlamak için kullanılır. 

Ayarlanmış ve bu özel sağlama bağlayıcı uygulamaları yapılandırmak bu öğreticinin kalan bölümleri konu şeklidir. Yapılandırmak için seçtiğiniz hangi uygulamaları hangi sistemleri, sağlamak ve kaç tane Active Directory ormanları ve Azure AD için gereksinim duyduğunuz kiracılar ortamınızda olduğuna bağlıdır.

![Genel Bakış](./media/active-directory-saas-workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Bir sistem tümleştirme kullanıcı iş günü içinde yapılandırın
Bir ortak, tüm iş günü sağlama bağlayıcıların Workday İnsan Kaynakları API'sine bağlanmak bir iş günü sistem tümleştirme hesabı kimlik bilgileri iste gereksinimdir. Bu bölümde, iş günü içinde bir sistem Tümleştirici hesabı oluşturmayı açıklar.

> [!NOTE]
> Bu yordamı atlayabilir ve bunun yerine bir iş günü genel yönetici sistem tümleştirme aynı hesabı kullan mümkündür. Bu tanıtımları için düzgün çalışıyor, ancak üretim dağıtımları için önerilmez.

### <a name="create-an-integration-system-user"></a>Tümleştirme sistemi kullanıcısı oluştur

**Tümleştirme sistemi kullanıcısı oluşturmak için:**

1. Workday Kiracı yönetici hesabı kullanarak oturum açın. İçinde **Workday çalışma ekranı**, girin arama kutusuna kullanıcı oluşturmak ve ardından **tümleştirme sistemi kullanıcısı Oluştur**. 
   
    ![Kullanıcı oluşturma](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "kullanıcı oluşturma")
2. Tamamlamak **tümleştirme sistemi kullanıcısı Oluştur** yeni bir tümleştirme sistemi kullanıcısı için bir kullanıcı adı ve parola sağlayarak görev.  
 * Bırakın **gerektiren yeni parolayı sonraki oturum açma** seçeneği işaretsiz, bu kullanıcının program aracılığıyla oturum açan olduğundan. 
 * Bırakın **oturum zaman aşımı dakika** ile varsayılan değeri 0 olan engeller kullanıcının oturumları erken zaman aşımı. 
   
    ![Tümleştirme sistemi kullanıcısı Oluştur](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "tümleştirme sistemi kullanıcısı oluştur")

### <a name="create-a-security-group"></a>Bir güvenlik grubu oluşturun
Kısıtlanmamış tümleştirme sistemi güvenlik grubu oluşturun ve kullanıcıya atamak gerekir.

**Bir güvenlik grubu oluşturmak için:**

1. Girin arama kutusunda güvenlik grubu oluşturun ve ardından **güvenlik grubu oluşturma**. 
   
    ![CreateSecurity Group](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity Group")
2. Tamamlamak **güvenlik grubu oluşturma** görev.  
3. Seçin **tümleştirme sistemi güvenlik grubunu (sınırlandırılmamış)** gelen **kiralanan güvenlik grubu türü** açılır.
4. İçin açıkça üyeleri eklenecek bir güvenlik grubu oluşturun. 
   
    ![CreateSecurity Group](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity Group")

### <a name="assign-the-integration-system-user-to-the-security-group"></a>Tümleştirme sistemi kullanıcısı güvenlik grubuna atayın

**Tümleştirme sistemi kullanıcısı atamak için:**

1. Güvenlik grubunu Düzenle arama kutusuna girin ve ardından **güvenlik grubunu Düzenle**. 
   
    ![Güvenlik grubunu Düzenle](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "güvenlik grubunu Düzenle")
2. Arayın ve ada göre yeni bir tümleştirme güvenlik grubu seçin. 
   
    ![Güvenlik grubunu Düzenle](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "güvenlik grubunu Düzenle")
3. Yeni tümleştirme sistemi kullanıcısı yeni güvenlik grubuna ekleyin. 
   
    ![Sistem güvenlik grubu](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "sistem güvenlik grubu")  

### <a name="configure-security-group-options"></a>Güvenlik grubu seçeneklerini yapılandırın
Bu adımda, etki alanı güvenlik ilkesi güvenlik grubuna çalışan veriler için izinler.

**Güvenlik grubu seçeneklerini yapılandırmak için:**

1. Girin **etki alanı güvenlik ilkeleri** arama kutusuna ve ardından bağlantıyı **işlevsel alanı için etki alanı güvenlik ilkeleri**.  
   
    ![Etki alanı güvenlik ilkeleri](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "etki alanı güvenlik ilkeleri")  
2. Sistem ve select arama **sistem** işlevsel alan.  **Tamam**’a tıklayın.  
   
    ![Etki alanı güvenlik ilkeleri](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "etki alanı güvenlik ilkeleri")  
3. Sistem işlevsel alan için güvenlik ilkelerini listesinde genişletin **güvenlik Yönetim** ve etki alanı güvenlik ilkesi seçin **Harici hesap sağlama**.  
   
    ![Etki alanı güvenlik ilkeleri](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "etki alanı güvenlik ilkeleri")  
4. Tıklatın **izinleri Düzenle**ve ardından **izinleri Düzenle** iletişim sayfası, güvenlik grupları listesine yeni bir güvenlik grubu eklemek **almak** ve **yerleştirin**  tümleştirme izinleri. 
   
    ![İzni Düzenle](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "düzenleme izni")  
    
5. Yukarıdaki 1-4 arası adımları bu ilkelerin her birinde kalan güvenlik için yineleyin:

| İşlem | Etki alanı güvenlik ilkesi |
| ---------- | ---------- | 
| Alma ve yerleştirme | Çalışan verileri: Ortak çalışan raporları |
| Alma ve yerleştirme | Çalışan verileri: İş kişi bilgileri |
| Al | Çalışan verileri: Tüm Pozisyonlar |
| Al | Çalışan verileri: Geçerli personel bilgileri |
| Al | Çalışan verileri: Çalışan profilindeki iş başlığı |
   
    
### <a name="activate-security-policy-changes"></a>Güvenlik İlkesi değişikliklerini etkinleştir

**Güvenlik İlkesi değişikliklerini etkinleştirmek için:**

1. Girin arama kutusuna etkinleştirin ve ardından bağlantıyı tıklatın **etkinleştirme bekleyen güvenlik ilkesi değişikliklerini**. 
   
    ![Etkinleştirme](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "etkinleştir") 
2. Bekleyen Güvenlik İlkesi değişikliklerini etkinleştir görev denetim amacıyla için bir açıklama girerek başlayın ve ardından **Tamam**. 
   
    ![Bekleyen güvenlik ayarlarını etkinleştir](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "bekleyen güvenlik ayarlarını etkinleştir")   
3. Onay kutusunu işaretleyerek sonraki ekranda görevi tamamlamak **Onayla**ve ardından **Tamam**. 
   
    ![Bekleyen güvenlik ayarlarını etkinleştir](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "bekleyen güvenlik ayarlarını etkinleştir")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Active Directory'ye Workday'den kullanıcı hazırlama yapılandırma
Kullanıcı hesabı için sağlama gerektiren her bir Active Directory ormanına Workday'deki sağlama yapılandırmak için bu yönergeleri izleyin.

### <a name="planning"></a>Planlama

Kullanıcı bir Active Directory ormanı hazırlama yapılandırmadan önce aşağıdaki soruları göz önünde bulundurun. Bu soruların yanıtlarını nasıl kapsam filtreleri ve öznitelik eşlemelerini ayarlanması gereken belirler. 

* **İş günü içinde hangi kullanıcıların bu Active Directory ormanına sağlanması gerekiyor?**

   * *Örnek: kullanıcıların nerede Workday "Şirket" özniteliği "Contoso" değerini içeren ve "Worker_Type" özniteliği "Normal" içerir*

* **Kullanıcıların farklı kuruluş birimlerine (OU) nasıl yönlendirilen?**

   * *Örnek: Kullanıcıların bir office konumuna karşılık gelen OU'ları Workday "Belediye" ve "Country_Region_Reference" öznitelikleri tanımlanan yönlendirilir*

* **Nasıl aşağıdaki öznitelikler Active Directory'de doldurulması gerekir?**

   * Ortak ad (cn)
      * *Örnek:, İnsan kaynakları tarafından belirlenen Workday USER_ID değerini kullanın.*
      
   * Çalışan kimlik numarası (EmployeeID)
      * *Örnek: Workday Worker_ID değeri kullanın*
      
   * SAM hesap adı (sAMAccountName)
      * *Örnek: geçersiz karakterler kaldırmak için ifade sağlama Azure AD ile filtrelenmiş Workday USER_ID değeri kullanın*
      
   * Kullanıcı asıl adı (userPrincipalName)
      * *Örnek: Workday USER_ID değeri, bir etki alanı adı eklemek için ifade sağlama bir Azure AD ile kullanma*

* **Nasıl kullanıcılar Workday ve Active Directory arasında eşleşen?**

  * *Örnek: Kullanıcılarla belirli bir iş günü "Worker_ID eşleşen değeri" ile "EmployeeID" aynı değere sahip olduğu Active Directory Kullanıcıları. Active Directory'de Worker_ID değeri bulunamadı, yeni bir kullanıcı oluşturun.*
  
* **Active Directory ormanı çalışmaya kimlikleri eşleşen mantığı için gerekli kullanıcı zaten içeriyor mu?**

  * *Örnek: Bu yeni bir iş günü dağıtım ise, Active Directory eşleşen mantığı olabildiğince basit tutmak için doğru Workday Worker_ID değerlerin (veya tercih ettiğiniz benzersiz kimliği değeri) ile önceden doldurulmuş haldedir önerilir.*
    
    
### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>1. Kısım: sağlama bağlayıcı uygulama ekleme ve iş günü bağlantısı oluşturma

**Active Directory sağlama için Workday yapılandırmak için:**

1.  Şuraya gidin: <https://portal.azure.com>

2.  Sol gezinti çubuğunda seçin **Azure Active Directory**

3.  Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4.  Seçin **bir uygulama eklemek**seçip **tüm** kategorisi.

5.  Arama **Workday sağlama Active Directory'ye**ve bu uygulama Galeriden ekleyin.

6.  Uygulama eklenir ve uygulama ayrıntıları ekranına gösterilen, select sonra **sağlama**

7.  Değişiklik **sağlama** **modu** için **otomatik**

8.  Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – eklenmiş Kiracı etki alanı adıyla Workday entegrasyonu sistem hesabı kullanıcı adı girin. **Gibi görünmelidir: username@contoso4**

   * **Yönetici parolası –** Workday entegrasyonu sistem hesabı için parola girin

   * **Kiracı URL –** kiracınız için Workday web hizmetleri uç noktası için URL'yi girin. Aşağıdaki gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4burada contoso4 doğru Kiracı adınız ile değiştirilir ve wd3 impl doğru ortamı dize ile değiştirilir.

   * **Active Directory ormanı -** "Name", Active Directory orman, Get-ADForest powershell komutunu tarafından döndürülen. Bu genellikle bir dize gibi olur: *contoso.com*

   * **Active Directory kapsayıcısı -** AD ormanınızdaki tüm kullanıcıları içeren kapsayıcı dizeyi girin. Örnek: *OU standart kullanıcılar, OU = Kullanıcılar, DC = contoso, DC = test =*

   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.

   * Tıklatın **Bağlantıyı Sına** düğmesi. Bağlantı testi başarılı olursa, tıklatın **kaydetmek** üstündeki düğmesi. Başarısız olursa, Workday kimlik iş günü içinde geçerli olduğunu denetleyin. 

![Azure portalına](./media/active-directory-saas-workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>2. Kısım: öznitelik eşlemelerini yapılandırın 

Bu bölümde, kullanıcı verilerini Workday'deki Active Directory ile nasıl akacağını yapılandırır.

1.  Sağlama sekmesinde altında **eşlemeleri**, tıklatın **OnPremises Workday çalışanlarına eşitleme**.

2.  İçinde **kaynak nesne kapsamı** alan, hangi kullanıcı kümeleri için iş günü içinde öznitelik tabanlı bir filtre kümesi tanımlayarak AD için sağlama kapsamında olmalıdır seçebilirsiniz. Varsayılan "tüm kullanıcılar iş günü içinde" kapsamıdır. Örnek filtreler:

   * Örnek: Kapsamıyla kullanıcılara 1000000 2000000 arasında çalışan kimlikler

      * Öznitelik: WorkerID

      * İşleci: REGEX eşleşmiyor

      * Değer: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Örnek: Yalnızca çalışanlar ve değil contingent çalışanları 

      * Öznitelik: EmployeeID

      * İşleci: NULL değil

3.  İçinde **hedef nesne eylemleri** alan, genel filtre uygulayabilirsiniz hangi eylemleri üzerinde Active Directory gerçekleştirilmesine izin verilir. **Oluşturma** ve **güncelleştirme** en sık kullanılan.

4.  İçinde **öznitelik eşlemelerini** bölümünde, tek tek nasıl Workday tanımlayabilirsiniz öznitelikleri eşlemek için Active Directory öznitelikleri.

5. Güncelleştirmek için varolan bir özniteliği eşlemesini üzerinde veya tıklatın **yeni eklemesi** yeni eşlemeler eklemek için ekranın altındaki. Bir tek özniteliği eşlemesi bu özellikleri destekler:

      * **Eşleme türü**

         * **Doğrudan** – hiçbir değişikliği AD öznitelik için Workday özniteliğinin değeri Yazar

         * **Sabit** -AD özniteliği için bir statik, sabit dize değeri yazma

         * **İfade** – bir veya daha fazla iş günü özniteliklerini temel alarak AD özniteliği için özel bir değer yazmanızı sağlar. [Daha fazla bilgi için bu makalede ifadeleri bkz](active-directory-saas-writing-expressions-for-attribute-mappings.md).

      * **Kaynak özniteliği** -Workday kullanıcı özniteliği. Aradığınız özniteliği mevcut değilse, bkz: [Workday kullanıcı özniteliklerinin listesi özelleştirme](#customizing-the-list-of-workday-user-attributes).

      * **Varsayılan değer** – isteğe bağlıdır. Eşleme kaynak özniteliği boş bir değer varsa, bu değer yerine yazacaksınız.
            Bu alanı boş bırakın en yaygın yapılandırmadır.

      * **Hedef öznitelik** – Active Directory'deki kullanıcı özniteliği.

      * **Bu öznitelik kullanarak nesneleri eşleşen** – bu eşlemeyi kullanıcılar Workday ve Active Directory arasında benzersiz şekilde tanımlamak için kullanılması gereken olup olmadığına bakılmaksızın. Bu genellikle, çalışan kimliği alanı bir Active Directory içinde çalışan kimliği özniteliklerin genellikle eşlenen iş günü için ayarlanır.

      * **Öncelik eşleşen** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alana göre tanımlanan sırayla değerlendirilir. Bir eşleşme olarak başka eşleştirme öznitelikleri değerlendirilir.

      * **Bu eşleme Uygula**
       
         * **Her zaman** – hem kullanıcı oluşturulması bu eşleme uygulamak ve güncelleştirme eylemleri

         * **Yalnızca oluşturma sırasında** -bu eşlemenin yalnızca kullanıcı oluşturma eylemlerini uygulamak

6. Eşlemelerinizin kaydetmek için tıklatın **kaydetmek** eşleme özniteliği bölümünün üstünde.

![Azure portalına](./media/active-directory-saas-workday-inbound-tutorial/WD_2.PNG)

**Aşağıda bazı örnek, bazı ortak ifadelerle Workday ve Active Directory arasında öznitelik eşlemelerini verilmiştir**

-   ParentDistinguishedName özniteliğine eşlenir ifade, farklı OU'ları bir veya daha fazla iş günü kaynak özniteliklerini temel alarak kullanıcılara sağlamak için kullanılır. Bu örnek kullanıcılar göre farklı OU'larda yerleştirir hangi Şehir üzerinde bulundukları.

-   UserPrincipalName özniteliği Active Directory'de bir etki alanı soneki Workday kullanıcı Kimliğiyle birleştirilmesiyle oluşturulur

-   [Burada ifadeleri yazma belge yok](active-directory-saas-writing-expressions-for-attribute-mappings.md). Bu özel karakterlerin tümünü kaldırmak nasıl örnekleri içerir.

  
| İŞ GÜNÜ ÖZNİTELİĞİ | ACTIVE DIRECTORY ÖZNİTELİĞİ |  KİMLİĞİ EŞLEŞİYOR MU? | OLUŞTUR / GÜNCELLEŞTİR |
| ---------- | ---------- | ---------- | ---------- |
| **WorkerID**  |  EmployeeID | **Evet** | Yazılan üzerinde yalnızca oluştur | 
| **UserID**    |  cn    |   |   Yazılan üzerinde yalnızca oluştur |
| **Birleştirme ("@", [UserID] "contoso.com")**   | userPrincipalName     |     | Yazılan üzerinde yalnızca oluştur 
| **Değiştirin (Mid (Değiştir (\[UserID\],, "(\[ \\ \\ / \\ \\ \\ \\ \\ \\ \[ \\\\\]\\\\:\\\\;\\ \\|\\\\=\\\\,\\\\+\\\\\*\\ \\? \\ \\ &lt; \\ \\ &gt; \]) "," ",), 1, 20)," ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**      |    sAMAccountName            |     |         Yazılan üzerinde yalnızca oluştur |
| **Anahtar (\[etkin\],, "0", "True", "1")** |  AccountDisabled      |     | Oluştur + güncelleştir |
| **FirstName**   | givenName       |     |    Oluştur + güncelleştir |
| **Soyadı**   |   sn   |     |  Oluştur + güncelleştir |
| **PreferredNameData**  |  displayName |     |   Oluştur + güncelleştir |
| **Şirket**         | Şirket   |     |  Oluştur + güncelleştir |
| **SupervisoryOrganization**  | Bölüm  |     |  Oluştur + güncelleştir |
| **ManagerReference**   | Yöneticisi  |     |  Oluştur + güncelleştir |
| **BusinessTitle**   |  başlık     |     |  Oluştur + güncelleştir | 
| **AddressLineData**    |  streetAddress  |     |   Oluştur + güncelleştir |
| **Belediye**   |   l   |     | Oluştur + güncelleştir |
| **CountryReferenceTwoLetter**      |   Ortak |     |   Oluştur + güncelleştir |
| **CountryReferenceTwoLetter**    |  c  |     |         Oluştur + güncelleştir |
| **CountryRegionReference** |  st     |     | Oluştur + güncelleştir |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Oluştur + güncelleştir |
| **PostalCode**  |   posta kodu  |     | Oluştur + güncelleştir |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Oluştur + güncelleştir |
| **Faks**      | facsimileTelephoneNumber     |     |    Oluştur + güncelleştir |
| **Mobil**  |    Mobil       |     |       Oluştur + güncelleştir |
| **LocalReference** |  preferredLanguage  |     |  Oluştur + güncelleştir |                                               
| **Anahtar (\[belediye\], "OU standart kullanıcılar, OU = Kullanıcılar, OU = varsayılan, OU = konumları, DC = contoso, DC = com =", "Dallas", "OU standart kullanıcılar, OU = Kullanıcılar, OU = Dallas, OU = konumları, DC = contoso, DC = com =", "Ankara'da", "OU standart kullanıcılar, OU = Kullanıcılar, OU = Ankara'da, OU = konumları, DC = contoso, DC = com = ","Seattle"," OU standart kullanıcılar, OU = Kullanıcılar, OU = Seattle, OU = konumları, DC = contoso, DC = com = ","Londra"," OU standart kullanıcılar, OU = Kullanıcılar, OU = Londra, OU = konumları, DC = contoso, DC = com = ")**  | parentDistinguishedName     |     |  Oluştur + güncelleştir |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a>3. Kısım: şirket içi eşitleme Aracısı'nı yapılandırma

Active Directory şirket içi sağlamak için bir aracı Active Directory ormanı isteğini bir etki alanına katılmış sunucuda yüklenmesi gerekir. Yordamı tamamlamak için gerekli kimlik bilgileri etki alanı yöneticisi (veya Kurumsal Yönetici).

**[Şirket içi eşitleme Aracısı indirebilirsiniz](https://go.microsoft.com/fwlink/?linkid=847801)**

Aracıyı yükledikten sonra ortamınız için aracısını yapılandırmak için aşağıdaki Powershell komutları çalıştırın.

**#1 komutu**

> CD C:\\Program dosyaları\\Microsoft Azure Active Directory Eşitleme Aracı\\modülleri\\AADSyncAgent

> import-module AADSyncAgent.psd1

**Komut #2**

> Add-ADSyncAgentActiveDirectoryConfiguration

* Giriş: "Dizin", AD orman adı kısmen girildiği gibi adı \#2
* Giriş: Yönetici kullanıcı adı ve parolası Active Directory ormanı için

**Komut #3**

> Add-ADSyncAgentAzureActiveDirectoryConfiguration

* Giriş: Genel yönetici kullanıcı adı ve parola Azure AD kiracınız için

>[!IMPORTANT]
>Şu anda özel bir etki alanı kullanıyorsanız çalışmıyor genel yönetici kimlik bilgileri bilinen bir sorun olduğunu (örnek: admin@contoso.com). Geçici bir çözüm olarak oluşturun ve bir onmicrosoft.com etki alanı ile bir genel yönetici hesabı kullanın (örnek: admin@contoso.onmicrosoft.com)


**Komut #4**

> Get-AdSyncAgentProvisioningTasks

* Eylem: veriler döndürülür onaylayın. Bu komut, Azure AD kiracınıza uygulamalarında sağlama Workday otomatik olarak bulur. Örnek çıktı:

> Ad: AD Ormanım
>
> Etkin: True
>
> DirectoryName: mydomain.contoso.com
>
> Belgeli: yanlış
>
> Identifier    : WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**Komut #5**

> Start-AdSyncAgentSynchronization-otomatik

**Komut #6**

> net stop aadsyncagent

**Komut #7**

> net start aadsyncagent

>[!TIP]
>Powershell "net" komutlar, ek olarak, eşitleme Aracısı hizmeti da başlatılabilir ve kullanarak durduruldu **Services.msc**. Powershell komutlarını çalıştırırken hataları alırsanız, emin **Microsoft Azure AD Connect sağlama Aracısı** çalıştığı **Services.msc**.

![Hizmetler](./media/active-directory-saas-workday-inbound-tutorial/Services.png)  

**Avrupa Birliği müşteriler için ek yapılandırma**

Azure Active Directory kiracınızın AB veri merkezleri birinde yer alıyorsa, aşağıdaki ek adımları izleyin.

1. Açık **Services.msc** , durdurup **Microsoft Azure AD Connect sağlama Aracısı** hizmet.
2. Aracı yükleme klasörüne gidin (örnek: C:\Program Files\Microsoft Azure AD Connect Aracısı sağlama).
3. Açık **SyncAgnt.exe.config** bir metin düzenleyicisinde.
4. Değiştir https://manage.hub.syncfabric.windowsazure.com/Management ile **https://eu.manage.hub.syncfabric.windowsazure.com/Management**
5. Değiştir https://provision.hub.syncfabric.windowsazure.com/Provisioning ile **https://eu.provision.hub.syncfabric.windowsazure.com/Provisioning**
6. Kaydet **SyncAgnt.exe.config** dosya.
7. Açık **Services.msc**ve başlangıç **Microsoft Azure AD Connect sağlama Aracısı** hizmet.

**Sorun giderme aracı**

[Windows olay günlüğü](https://technet.microsoft.com/en-us/library/cc722404(v=ws.11).aspx) Windows Server aracıyı barındıran makine aracısı tarafından gerçekleştirilen tüm işlemler için olayları içerir. Bu olayları görüntülemek için:
    
1. Açık **Eventvwr.msc**.
2. Seçin **Windows Günlükleri > Uygulama**.
3. Altında kaynak günlüğe kaydedilen tüm olaylar görüntülemek **AADSyncAgent**. 
4. Hataları ve Uyarıları denetleyin.

Powershell komutları sağlanan Active Directory veya Azure Active Directory kimlik bilgilerine sahip izinlerle ilgili bir sorun varsa, bunun gibi bir hata görürsünüz: 
    
![Olay günlükleri](./media/active-directory-saas-workday-inbound-tutorial/Windows_Event_Logs.png) 


### <a name="part-4-start-the-service"></a>4. Kısım: Hizmetini başlatmak için
Bölümleri 1-3 tamamladıktan sonra Azure portalında sağlama hizmeti başlatabilirsiniz.

1.  İçinde **sağlama** sekmesinde, ayarlamak **sağlama durumu** için **üzerinde**.

2. **Kaydet**’e tıklayın.

3. Bu değişken sayıda iş günü içinde kaç kullanıcılardır bağlı olarak saatler sürebilir ilk eşitlemeyi başlatır.

4. Herhangi bir zamanda denetleyin **denetim günlüklerini** sağlama hizmeti gerçekleştirilen eylemleri görmek için Azure portalında sekmesi. Denetim günlüklerini olduğu gibi kullanıcılar dışında Workday okuma yükleniyor ve daha sonra eklenen veya Active Directory'ye güncelleştirilmiş sağlama hizmeti tarafından gerçekleştirilen tüm bireysel eşitleme olayları listeler. **[Denetim günlüklerini okumak ayrıntılı yönergeler için sağlama raporlama Kılavuzu bakın.](active-directory-saas-provisioning-reporting.md)**

5.  Denetleme [Windows olay günlüğü](https://technet.microsoft.com/en-us/library/cc722404(v=ws.11).aspx) yeni hatalar veya uyarılar için aracıyı barındıran Windows Server makinesinde. Bu olaylar başlatarak görüntülenebilir **Eventvwr.msc** sunucuda seçerek **Windows Günlükleri > Uygulama**. Sağlama ile ilgili tüm iletileri kaynak kaydedilir **AADSyncAgent**. 
    

6. Bir tamamlandı, onu bir denetim özet raporu yazacak **sağlama** sekmesinde, aşağıda gösterildiği gibi.

![Azure portalına](./media/active-directory-saas-workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Azure Active Directory'ye kullanıcı sağlamayı yapılandırma
Aşağıdaki tabloda ayrıntılı olarak sağlama gereksinimlerinizi üzerinde Azure Active Directory'ye sağlama nasıl yapılandırdığınıza bağlıdır.

| Senaryo | Çözüm |
| -------- | -------- |
| **Kullanıcıların Active Directory ve Azure AD için hazırlanması gerekir** | Kullanım  **[AAD bağlanma](connect/active-directory-aadconnect.md)** |
| **Kullanıcıların Active Directory'ye yalnızca sağlanması gerekir** | Kullanım  **[AAD bağlanma](connect/active-directory-aadconnect.md)** |
| **Kullanıcıların Azure AD için yalnızca (yalnızca bulut) sağlanması gerekir** | Kullanım **Azure Active Directory sağlama iş günü** uygulama galerisinde uygulama |

Azure AD Connect ayarlama hakkında yönergeler için bkz: [Azure AD Connect belgelerini](connect/active-directory-aadconnect.md).

Aşağıdaki bölümlerde yalnızca bulut kullanıcıları sağlamak için Workday ve Azure AD arasında bir bağlantı ayarı açıklanmaktadır.

> [!IMPORTANT]
> Azure AD ile sağlanması yalnızca bulut kullanıcılar varsa ve Active Directory içi değil yalnızca aşağıdaki yordamı izleyin.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>1. Kısım: Azure AD sağlama bağlayıcı uygulama ekleme ve iş günü bağlantısı oluşturma

**Yalnızca bulut kullanıcıları için Azure Active Directory sağlama için Workday yapılandırmak için:**

1.  <https://portal.azure.com> kısmına gidin.

2.  Sol gezinti çubuğunda seçin **Azure Active Directory**

3.  Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4.  Seçin **bir uygulama eklemek**ve ardından **tüm** kategorisi.

5.  Arama **Azure AD sağlama Workday**ve bu uygulama Galeriden ekleyin.

6.  Uygulama eklenir ve uygulama ayrıntıları ekranına gösterilen, select sonra **sağlama**

7.  Değişiklik **sağlama** **modu** için **otomatik**

8.  Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – eklenmiş Kiracı etki alanı adıyla Workday entegrasyonu sistem hesabı kullanıcı adı girin. Gibi görünmelidir: username@contoso4

   * **Yönetici parolası –** Workday entegrasyonu sistem hesabı için parola girin

   * **Kiracı URL –** kiracınız için Workday web hizmetleri uç noktası için URL'yi girin. Aşağıdaki gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4burada contoso4 doğru Kiracı adınız ile değiştirilir ve wd3 impl doğru ortamı dize ile değiştirilir. Bu URL bilinmiyor, Lütfen doğru URL'sini belirlemek için Workday entegrasyonu iş ortağı veya destek temsilcinizle çalışır.

   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.

   * Tıklatın **Bağlantıyı Sına** düğmesi.

   * Bağlantı testi başarılı olursa, tıklatın **kaydetmek** üstündeki düğmesi. Başarısız olursa Workday URL ve kimlik bilgilerini iş günü içinde geçerli olduğunu denetleyin.


### <a name="part-2-configure-attribute-mappings"></a>2. Kısım: öznitelik eşlemelerini yapılandırın 

Bu bölümde, kullanıcı verilerini Workday'deki Azure Active Directory'ye yalnızca bulut kullanıcıları için nasıl akacağını yapılandırır.

1.  Sağlama sekmesinde altında **eşlemeleri**, tıklatın **Azure ad eşitleme çalışanları**.

2.   İçinde **kaynak nesne kapsamı** alan, hangi kullanıcı kümeleri için iş günü içinde Azure AD ile öznitelik tabanlı bir filtre kümesi tanımlayarak sağlama kapsamında olmalıdır seçebilirsiniz. Varsayılan "tüm kullanıcılar iş günü içinde" kapsamıdır. Örnek filtreler:

   * Örnek: Kapsamıyla kullanıcılara 1000000 2000000 arasında çalışan kimlikler

      * Öznitelik: WorkerID

      * İşleci: REGEX eşleşmiyor

      * Değer: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Örnek: Yalnızca contingent çalışanları ve değil Normal çalışanlar

      * Öznitelik: ContingentID

      * İşleci: NULL değil

3.  İçinde **hedef nesne eylemleri** alan, genel filtre uygulayabilirsiniz hangi eylemleri üzerinde Azure AD gerçekleştirilmesine izin verilir. **Oluşturma** ve **güncelleştirme** en sık kullanılan.

4.  İçinde **öznitelik eşlemelerini** bölümünde, tek tek nasıl Workday tanımlayabilirsiniz öznitelikleri eşlemek için Active Directory öznitelikleri.

5. Güncelleştirmek için varolan bir özniteliği eşlemesini üzerinde veya tıklatın **yeni eklemesi** yeni eşlemeler eklemek için ekranın altındaki. Bir tek özniteliği eşlemesi bu özellikleri destekler:

   * **Eşleme türü**

      * **Doğrudan** – hiçbir değişikliği AD öznitelik için Workday özniteliğinin değeri Yazar

      * **Sabit** -AD özniteliği için bir statik, sabit dize değeri yazma

      * **İfade** – bir veya daha fazla iş günü özniteliklerini temel alarak AD özniteliği için özel bir değer yazmanızı sağlar. [Daha fazla bilgi için bu makalede ifadeleri bkz](active-directory-saas-writing-expressions-for-attribute-mappings.md).

   * **Kaynak özniteliği** -Workday kullanıcı özniteliği. Aradığınız özniteliği mevcut değilse, bkz: [Workday kullanıcı özniteliklerinin listesi özelleştirme](#customizing-the-list-of-workday-user-attributes).

   * **Varsayılan değer** – isteğe bağlıdır. Eşleme kaynak özniteliği boş bir değer varsa, bu değer yerine yazacaksınız.
            Bu alanı boş bırakın en yaygın yapılandırmadır.

   * **Hedef öznitelik** – Azure AD'de kullanıcı özniteliği.

   * **Bu öznitelik kullanarak nesneleri eşleşen** – bu eşlemeyi kullanıcılar Workday ve Azure AD arasında benzersiz şekilde tanımlamak için kullanılması gereken olup olmadığına bakılmaksızın. Bu genellikle, çalışan kimliği alanı genellikle Azure AD içinde çalışan ID özniteliği (yeni) ya da uzantı özniteliği eşlenen iş günü için ayarlanır.

   * **Öncelik eşleşen** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alana göre tanımlanan sırayla değerlendirilir. Bir eşleşme olarak başka eşleştirme öznitelikleri değerlendirilir.

   * **Bu eşleme Uygula**

     * **Her zaman** – hem kullanıcı oluşturulması bu eşleme uygulamak ve güncelleştirme eylemleri

     * **Yalnızca oluşturma sırasında** -bu eşlemenin yalnızca kullanıcı oluşturma eylemlerini uygulamak

6. Eşlemelerinizin kaydetmek için tıklatın **kaydetmek** eşleme özniteliği bölümünün üstünde.

### <a name="part-3-start-the-service"></a>3. Kısım: Hizmetini başlatmak için
Bölümleri 1-2 tamamladıktan sonra sağlama hizmeti başlatabilirsiniz.

1.  İçinde **sağlama** sekmesinde, ayarlamak **sağlama durumu** için **üzerinde**.

2. **Kaydet**’e tıklayın.

3. Bu değişken sayıda iş günü içinde kaç kullanıcılardır bağlı olarak saatler sürebilir ilk eşitlemeyi başlatır.

4. Bireysel eşitleme olayları görüntülenebilir **denetim günlüklerini** sekmesi. **[Denetim günlüklerini okumak ayrıntılı yönergeler için sağlama raporlama Kılavuzu bakın.](active-directory-saas-provisioning-reporting.md)**

5. Bir tamamlandı, onu bir denetim özet raporu yazacak **sağlama** sekmesinde, aşağıda gösterildiği gibi.


## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Geri yazma Workday e-posta adreslerini yapılandırma
Azure Active Directory'den kullanıcı e-posta adreslerini Workday geri yazma yapılandırmak için bu yönergeleri izleyin.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>1. Kısım: sağlama bağlayıcı uygulama ekleme ve iş günü bağlantısı oluşturma

**Active Directory sağlama için Workday yapılandırmak için:**

1.  Şuraya gidin: <https://portal.azure.com>

2.  Sol gezinti çubuğunda seçin **Azure Active Directory**

3.  Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4.  Seçin **bir uygulama eklemek**seçeneğini belirleyip **tüm** kategorisi.

5.  Arama **Workday geri yazma**, bu uygulama Galeriden ekleyin.

6.  Uygulama eklenir ve uygulama ayrıntıları ekranına gösterilen, select sonra **sağlama**

7.  Değişiklik **sağlama** **modu** için **otomatik**

8.  Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – eklenmiş Kiracı etki alanı adıyla Workday entegrasyonu sistem hesabı kullanıcı adı girin. Gibi görünmelidir: username@contoso4

   * **Yönetici parolası –** Workday entegrasyonu sistem hesabı için parola girin

   * **Kiracı URL –** kiracınız için Workday web hizmetleri uç noktası için URL'yi girin. Aşağıdaki gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4burada contoso4 doğru Kiracı adınız ile değiştirilir ve (gerekirse) wd3 impl doğru ortamı dizesi ile değiştirilir.

   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.

   * Tıklatın **Bağlantıyı Sına** düğmesi. Bağlantı testi başarılı olursa, tıklatın **kaydetmek** üstündeki düğmesi. Başarısız olursa Workday URL ve kimlik bilgilerini iş günü içinde geçerli olduğunu denetleyin.


### <a name="part-2-configure-attribute-mappings"></a>2. Kısım: öznitelik eşlemelerini yapılandırın 


Bu bölümde, kullanıcı verilerini Workday'deki Active Directory ile nasıl akacağını yapılandırır.

1.  Sağlama sekmesinde altında **eşlemeleri**, tıklatın **eşitleme Azure AD iş günü kullanıcılara**.

2.  İçinde **kaynak nesne kapsamı** alan, isteğe bağlı olarak filtreleyebilir hangi kullanıcı kümeleri için Azure Active Directory'de e-posta adreslerini yazdığınız geri Workday için. Varsayılan "tüm kullanıcılar Azure AD'de" kapsamıdır. 

3.  İçinde **öznitelik eşlemelerini** bölümünde, tek tek nasıl Workday tanımlayabilirsiniz öznitelikleri eşlemek için Active Directory öznitelikleri. Varsayılan olarak e-posta adresi için bir eşleme yoktur. Ancak, Azure AD'de Workday bunların karşılık gelen girdilere sahip kullanıcıları eşleştirmek için eşleşen kimliği güncelleştirilmesi gerekir. Popüler eşleşen yöntemi Workday çalışan kimliği veya çalışan kimliği extensionAttribute1 15 Azure AD'de eşitleme ve ardından bu öznitelik Azure AD'de geri iş günü içinde kullanıcıları eşleştirmek için kullanmaktır.

4.  Eşlemelerinizin kaydetmek için tıklatın **kaydetmek** eşleme özniteliği bölümünün üstünde.

### <a name="part-3-start-the-service"></a>3. Kısım: Hizmetini başlatmak için
Bölümleri 1-2 tamamladıktan sonra sağlama hizmeti başlatabilirsiniz.

1.  İçinde **sağlama** sekmesinde, ayarlamak **sağlama durumu** için **üzerinde**.

2. **Kaydet**’e tıklayın.

3. Bu değişken sayıda iş günü içinde kaç kullanıcılardır bağlı olarak saatler sürebilir ilk eşitlemeyi başlatır.

4. Bireysel eşitleme olayları görüntülenebilir **denetim günlüklerini** sekmesi. **[Denetim günlüklerini okumak ayrıntılı yönergeler için sağlama raporlama Kılavuzu bakın.](active-directory-saas-provisioning-reporting.md)**

5. Bir tamamlandı, onu bir denetim özet raporu yazacak **sağlama** sekmesinde, aşağıda gösterildiği gibi.


## <a name="customizing-the-list-of-workday-user-attributes"></a>Workday kullanıcı özniteliklerinin listesi özelleştirme
Active Directory ve her ikisi Workday kullanıcı özniteliklerinin varsayılan listesini içeren Azure AD için uygulamalar sağlama Workday arasından seçim yapabilirsiniz. Ancak, bu listeleri kapsamlı değildir. Workday yüzlerce ya da standart ya da iş günü kiracınız için benzersiz olabilir olası kullanıcı öznitelikleri destekler. 

Hizmet sağlama Azure AD listesi veya de sağlanmaktadır öznitelikler eklemek üzere Workday özniteliği özelleştirme yeteneği destekleyen [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) İnsan Kaynakları API işlemi.

Bunu yapmak için kullanmanız gerekir [Workday Studio](https://community.workday.com/studio-download) kullanmak istediğiniz özniteliklerini temsil eder ve bunları Azure portalında Gelişmiş Öznitelik Düzenleyicisi'ni kullanarak sağlama yapılandırmanızda Ekle XPath ifadeler ayıklayın.

**Bir XPath ifadesi Workday kullanıcı özniteliği için almak için:**

1. İndirme ve yükleme [Workday Studio](https://community.workday.com/studio-download). Yükleyici erişmek için bir iş günü topluluk hesabınızın olması gerekir.

2. Bu URL'den Workday Human_Resources WDSL indirilemedi: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. İş günü Studio'yu başlatın.

4. Komut çubuğundan seçin **Workday > Tester Test Web hizmetinde** seçeneği.

5. Seçin **dış**ve 2. adımda indirdiğiniz Human_Resources WSDL dosyası seçin.

    ![İş günü Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio1.PNG)

6. Ayarlama **konumu** alanı `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, ancak "IMPL-CC", gerçek ile değiştirerek örnek türü ve "KİRACI", gerçek Kiracı adı.

7. Ayarlama **işlemi** için **Get_Workers**

8.  Küçük tıklatın **yapılandırma** Workday kimlik bilgilerinizi ayarlamak için istek/yanıt bölmeleri bağlantıya. Denetleme **kimlik doğrulaması**ve ardından Workday entegrasyonu sistem hesabı için kullanıcı adı ve parolayı girin. Kullanıcı adı olarak biçimlendirmek mutlaka name@tenant, bırakıp **WS-güvenlik UsernameToken** seçeneği belirlenmiş.

    ![İş günü Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio2.PNG)

9. **Tamam**’ı seçin.

10. **İsteği** bölmesinde, aşağıdaki ve ayarlanmış XML Yapıştır **Employee_ID** Workday kiracınızda gerçek bir kullanıcının çalışan kimliği. Özniteliğine sahip bir kullanıcı seçin, ayıklamak istediğiniz doldurulur.

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
      <env:Body>
        <wd:Get_Workers_Request xmlns:wd="urn:com.workday/bsvc" wd:version="v21.1">
          <wd:Request_References wd:Skip_Non_Existing_Instances="true">
            <wd:Worker_Reference>
              <wd:ID wd:type="Employee_ID">21008</wd:ID>
            </wd:Worker_Reference>
          </wd:Request_References>
          <wd:Response_Group>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Personal_Information>true</wd:Include_Personal_Information>
            <wd:Include_Employment_Information>true</wd:Include_Employment_Information>
            <wd:Include_Management_Chain_Data>true</wd:Include_Management_Chain_Data>
            <wd:Include_Organizations>true</wd:Include_Organizations>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Transaction_Log_Data>true</wd:Include_Transaction_Log_Data>
            <wd:Include_Photo>true</wd:Include_Photo>
            <wd:Include_User_Account>true</wd:Include_User_Account>
            <wd:Include_Roles>true</wd:Include_Roles>
          </wd:Response_Group>
        </wd:Get_Workers_Request>
      </env:Body>
    </env:Envelope>
    ```
 
11. Tıklatın **İsteği Gönder** (komutu yürütmek için yeşil ok). Başarılı yanıt görüntülenmelidir varsa **yanıt** bölmesi. Girdiğiniz kullanıcı kimliği veriler içerdiğinden emin olmak için yanıt ve bir hata denetleyin.

12. Başarılı olursa, XML'den kopyalama **yanıt** bölmesi ve bir XML dosyası olarak kaydedin.

13. Komut çubuğu, Workday Studio'da seçin **Dosya > Dosya Aç...**  ve kaydettiğiniz XML dosyasını açın. Bu, iş günü Studio XML Düzenleyicisi'nde açar.

    ![İş günü Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio3.PNG)

14. Dosya ağacında gezinmek **/env:Envelope > env:Body > wd:Get_Workers_Response > wd:Response_Data > wd:Worker** kullanıcı verileri bulmak üzere. 

15. Altında **wd:Worker**eklemek istediğiniz öznitelik bulun ve seçin.

16. Seçili özniteliğinizi dışı XPath ifadesi kopyalama **belgesinin yolu** alan.

17. Remove the **/env:Envelope/env:Body/wd:Get_Workers_Response/wd:Response_Data/** prefix from the copied expression. 

18. Kopyalanan ifade son öğenin bir düğüm olup olmadığını (örnek: "/ wd:Birth_Date"), ardından append **/text()** ifadesinin sonunda. Bu son öğenin bir özniteliği ise gerekli değildir (örnek: "/@wd:type").

19. Sonuç aşağıdakine benzer olmalıdır `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Azure portalında kopyalayacak budur.


**Sağlama yapılandırmanızı, özel Workday kullanıcı özniteliği eklemek için:**

1. Başlatma [Azure portal](https://portal.azure.com)ve Bu öğreticide daha önce açıklandığı gibi uygulama, sağlama İş gününüzün hazırlama bölümüne gidin.

2. Ayarlama **sağlama durumu** için **kapalı**seçip **kaydetmek**. Bu, hazır olduğunuzda, yaptığınız değişiklikler etkili olmanıza yardımcı olur.

3. Altında **eşlemeleri**seçin **OnPremises çalışanlarına eşitleme** (veya **Azure ad eşitleme çalışanları**).

4. Sonraki ekranda sonuna kaydırın ve seçin **Gelişmiş Seçenekleri Göster**.

5. Seçin **Workday düzenleme öznitelik listesi**.

    ![İş günü Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio_AAD1.PNG)

6. Giriş alanlarının nerede için öznitelik listesi sonuna kaydırın.

7. İçin **adı**, öznitelik için bir görünen ad girin.

8. İçin **türü**, uygun şekilde, özniteliğe karşılık gelen türünü seçin (**dize** yaygın olarak kullanılır).

9. İçin **API ifade**, Workday Studio'dan kopyaladığınız XPath ifadesi girin. Örnek: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Seçin **öznitelik Ekle**.

    ![İş günü Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio_AAD2.PNG)

11. Seçin **kaydetmek** yukarıdaki ve ardından **Evet** iletişim. Eşleme özniteliği ekranı hala açıksa kapatın.

12. Ana üzerinde geri **sağlama** sekmesine **OnPremises çalışanlarına eşitleme** (veya **Azure ad eşitleme çalışanları**) yeniden.

13. Seçin **yeni eklemesi**.

14. Yeni öznitelik şimdi görüntülenmelidir **kaynak özniteliği** listesi.

15. Yeni öznitelik için bir eşleme istendiği gibi ekleyin.

16. Tamamlandığında, ayarlamayı unutmayın **sağlama durumu** başa **üzerinde** ve kaydedin.


## <a name="known-issues"></a>Bilinen sorunlar

* Çalıştırırken **Ekle ADSyncAgentAzureActiveDirectoryConfiguration** Powershell komutu, şu anda özel bir etki alanı kullanıyorsanız çalışmıyor genel yönetici kimlik bilgileri bilinen bir sorun olduğunu (örnek: admin@contoso.com) . Geçici bir çözüm olarak oluşturun ve Azure AD'de bir onmicrosoft.com etki alanı ile bir genel yönetici hesabı kullanın (örnek: admin@contoso.onmicrosoft.com).

* Avrupa Birliği bulunan Azure AD kiracılarıyla görünmeyen denetim günlüklerini önceki bir sorun çözüldü. Ancak, ek Aracısı yapılandırması AB Azure AD kiracıları için gereklidir. Ayrıntılar için bkz [bölümü 3: şirket içi eşitleme Aracısı'nı yapılandırma](#Part 3: Configure the on-premises synchronization agent)

## <a name="gdpr-compliance"></a>GDPR uyumluluk

[Genel veri koruma düzenleme (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm) Avrupa Birliği (AB) veri koruma ve gizlilik yasaları değil. GDPR şirketler kuralları uygular, devlet dairesi, kar kaybı olmayan ve AB veya kişilere mal ve hizmet sunmak diğer kuruluşların toplamak ve AB Satışlar bağlı verileri analiz etmek. 

Azure AD sağlama GDPR Microsoft'un hizmetlerin ve özelliklerin yanı sıra rest uyumlu hizmetidir. Microsoft'un GDPR Öykü hakkında daha fazla bilgi için bkz: [hizmet koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31).

Ancak, Active Directory için iş günü sağlama çözümünü etki alanına katılmış bir sunucuya yüklenmesi bir eşitleme Aracısı gerektirdiğinden, GDPR uyumlu de Kal izlemek için gereken bazı olaylar vardır.
 
Aracı günlüklerinde oluşturur **Windows olay günlüğü**, kişisel olarak tanımlanabilir bilgiler içerebilir.

GDPR uyumlu olmak iki yol vardır:

1. İstek, bir kişi için verileri ayıklamak ve Windows olay günlükleri bu kişiden veri kaldırın. 
2. Windows olay günlüklerini altında 48 saat AADSyncAgent işleminden kaynaklanan bekletme tutun

Veri saklama için Windows olay günlüklerini yapılandırma hakkında daha fazla bilgi için bkz: [olay günlüğü ayarları](https://technet.microsoft.com/en-us/library/cc952132.aspx). Windows olay günlüğü hakkında genel bilgi için bkz: [bu makalede](https://msdn.microsoft.com/en-us/library/windows/desktop/aa385772.aspx).


## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)
* [Çoklu oturum açma Workday ve Azure Active Directory arasında yapılandırmayı öğrenin](active-directory-saas-workday-tutorial.md)
* [Diğer SaaS uygulamaları Azure Active Directory ile tümleştirme öğrenin](active-directory-saas-tutorial-list.md)

