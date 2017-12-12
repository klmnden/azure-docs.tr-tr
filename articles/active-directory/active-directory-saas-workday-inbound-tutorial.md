---
title: "Öğretici: Workday ile şirket içi Active Directory ve Azure Active Directory sağlama otomatik olarak bir kullanıcı için yapılandırma | Microsoft Docs"
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
ms.date: 05/26/2017
ms.author: asmalser
ms.openlocfilehash: f267a59fadb7f402ac81f43b5465b6ac1f28943e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-with-on-premises-active-directory-and-azure-active-directory"></a>Öğretici: Workday ile şirket içi Active Directory ve Azure Active Directory sağlama otomatik olarak bir kullanıcı için yapılandırın.
Bu öğreticinin amacı kişiler Workday için bazı özniteliklerin isteğe bağlı geri yazma ile Active Directory ve Azure Active Directory'de Workday'deki alma işlemini gerçekleştirmek için gereken adımları Göster sağlamaktır. 



## <a name="overview"></a>Genel Bakış

[Hizmet sağlama Azure Active Directory kullanıcı](active-directory-saas-app-provisioning.md) ile tümleştirilir [Workday İnsan Kaynakları API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) kullanıcı hesaplarını sağlamak için. Azure AD iş akışları sağlama aşağıdaki kullanıcı etkinleştirmek için bu bağlantı kullanır:

* **Active Directory kullanıcılara sağlama** -Workday kullanıcı seçili kümeleri bir veya daha fazla Active Directory ormanları eşitleyin. 

* **Azure Active Directory salt bulut kullanıcılara sağlama** -ikinci kullanarak Active Directory ve Azure Active Directory içinde mevcut karma kullanıcılar sağlanabilir [AAD Connect](connect/active-directory-aadconnect.md). Ancak, salt bulut olan kullanıcılar, Azure hizmet sağlama Azure AD kullanıcısının kullanarak Active Directory için doğrudan Workday'den sağlanabilir.

* **Geri yazma e-posta adresleri için Workday** -hizmet sağlama Azure AD kullanıcı yazma seçilen Azure AD kullanıcı özniteliklerini e-posta adresi gibi Workday dön.

### <a name="scenarios-covered"></a>Kapsanan senaryolar

Azure AD kullanıcı sağlama hizmeti tarafından desteklenen Workday kullanıcı sağlama iş akışları şu İnsan Kaynakları ve kimlik yaşam döngüsü yönetimi senaryoları etkinleştirin:

* **Yeni çalışan işe alma** - yeni bir çalışan iş günü için eklendiğinde, bir kullanıcı hesabı otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak oluşturulur ve [AzureADtarafındandesteklenendiğerSaaSuygulamaları](active-directory-saas-app-provisioning.md), Workday için e-posta adresinin sonradan yazma ile.

* **Çalışan özniteliği ve profil güncelleştirmeleri** - zaman (kendi adı, başlık veya Yöneticisi gibi) bir çalışan kaydı iş günü içinde güncelleştirilir, kendi kullanıcı hesabına otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak güncelleştirilir ve [Azure AD tarafından desteklenen diğer SaaS uygulamaları](active-directory-saas-app-provisioning.md).

* **Çalışan sonlandırmalar** - çalışan iş günü içinde sonlandırıldığında kendi kullanıcı hesabına otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak devre dışıdır ve [Azure AD tarafından desteklenen diğer SaaS uygulamaları](active-directory-saas-app-provisioning.md).

* **Çalışan yeniden anlaşır** - çalışan iş günü içinde rehired eski hesaplarında bulunabilir otomatik olarak yeniden veya Active Directory, Azure Active Directory ve Office 365 için isteğe bağlı olarak (tercihinize bağlı olarak) yeniden sağlandı ve [Azure AD tarafından desteklenen diğer SaaS uygulamaları](active-directory-saas-app-provisioning.md).


## <a name="planning-your-solution"></a>Çözümünüzü planlarken

Workday entegrasyonu başlamadan önce aşağıdaki önkoşulları denetleyin ve geçerli bir Active Directory mimarisi ve Azure Active Directory tarafından sağlanan solution(s) ile gereksinimleri sağlama kullanıcı eşleşmesi konusunda aşağıdaki yönergeleri okuyun.

### <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Genel yönetici erişimi olan geçerli bir Azure AD Premium P1 abonelik
* Sınama ve tümleştirme amaçları için bir iş günü uygulama Kiracı
* Test amacıyla çalışan verilerini test etmek için yönetici izinleri sistem tümleştirme kullanıcı oluşturmak ve değişiklikler yapmak için iş günü içinde
* Active Directory'ye kullanıcı sağlamak için 2012 veya daha fazla Windows hizmetini çalıştıran bir etki alanına katılmış sunucuya konağa gereklidir [şirket içi eşitleme Aracısı](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](connect/active-directory-aadconnect.md) Active Directory ve Azure AD arasında eşitlemek için


### <a name="solution-architecture"></a>Çözüm mimarisi

Azure AD sağlama gidermenize yardımcı olacak bağlayıcılar ve kimlik Yaşam Döngüsü Yönetimi'nden Workday Active Directory, Azure AD, SaaS uygulamaları için ve ötesinde sağlama zengin bir özellik kümesi sağlar. Hangi özellikleri kullanır ve çözümü ayarlama nasıl kuruluşunuzun ortamlarına ve gereksinimlerine bağlı olarak farklılık gösterir. İlk adım olarak, stok mevcut ve kuruluşunuzdaki dağıtılan kaç aşağıdakilerden birini gerçekleştirin:

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

* **Azure AD sağlama için iş günü** - AAD Connect Azure Active Directory, bu uygulama bir kullanıcıya, Workday yalnızca bulut kullanıcıları için tek bir Azure Active Directory Kiracı sağlanmasını kolaylaştırmak için kullanılabilir Active Directory eşitleme için kullanılması gereken aracı olsa da.

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
   
    ![Güvenlik grubu oluştur](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "güvenlik grubu oluştur")
2. Tamamlamak **güvenlik grubu oluşturma** görev.  
3. Tümleştirme sistemi güvenlik grubu seç — Kısıtlanmamış öğesinden **kiralanan güvenlik grubu türü** açılır.
4. İçin açıkça üyeleri eklenecek bir güvenlik grubu oluşturun. 
   
    ![Güvenlik grubu oluştur](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "güvenlik grubu oluştur")

### <a name="assign-the-integration-system-user-to-the-security-group"></a>Tümleştirme sistemi kullanıcısı güvenlik grubuna atayın

**Tümleştirme sistemi kullanıcısı atamak için:**

1. Güvenlik grubunu Düzenle arama kutusuna girin ve ardından **güvenlik grubunu Düzenle**. 
   
    ![Güvenlik grubunu Düzenle](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "güvenlik grubunu Düzenle")
2. Arayın ve ada göre yeni bir tümleştirme güvenlik grubu seçin. 
   
    ![Güvenlik grubunu Düzenle](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "güvenlik grubunu Düzenle")
3. Yeni tümleştirme sistemi kullanıcısı yeni güvenlik grubuna ekleyin. 
   
    ![Sistem güvenlik grubu](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "sistem güvenlik grubu")  

### <a name="configure-security-group-options"></a>Güvenlik grubu seçeneklerini yapılandırın
Bu adımda, yeni güvenlik grubu izinlerini vermek **almak** ve **Put** aşağıdaki etki alanı güvenlik ilkeleri tarafından güvenliği sağlanan nesneleri işlemleri:

* Harici hesap sağlama
* Çalışan verileri: Ortak çalışan raporları
* Çalışan verileri: Tüm Pozisyonlar
* Çalışan verileri: Geçerli personel bilgileri
* Çalışan verileri: Çalışan profilindeki iş başlığı

**Güvenlik grubu seçeneklerini yapılandırmak için:**

1. Etki alanı güvenlik ilkeleri arama kutusuna girin ve ardından bağlantıyı tıklatın **işlevsel alanı için etki alanı güvenlik ilkeleri**.  
   
    ![Etki alanı güvenlik ilkeleri](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "etki alanı güvenlik ilkeleri")  
2. Sistem ve select arama **sistem** işlevsel alan.  **Tamam** düğmesine tıklayın.  
   
    ![Etki alanı güvenlik ilkeleri](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "etki alanı güvenlik ilkeleri")  
3. Sistem işlevsel alan için güvenlik ilkelerini listesinde genişletin **güvenlik Yönetim** ve etki alanı güvenlik ilkesi seçin **Harici hesap sağlama**.  
   
    ![Etki alanı güvenlik ilkeleri](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "etki alanı güvenlik ilkeleri")  
4. Tıklatın **izinleri Düzenle**ve ardından **izinleri Düzenle** iletişim sayfası, güvenlik grupları listesine yeni bir güvenlik grubu eklemek **almak** ve **yerleştirin**  tümleştirme izinleri. 
   
    ![İzni Düzenle](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "düzenleme izni")  
5. İşlevsel alanlara seçme ekran geri dönmek için yukarıdaki 1. adım ve bu süre, personel, arama seçin yineleyin **işlevsel alan personel** tıklatıp **Tamam**.
   
    ![Etki alanı güvenlik ilkeleri](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "etki alanı güvenlik ilkeleri")  
6. Güvenlik ilkeleri Staffing işlevsel alan için listesinde genişletin **çalışan verileri: Staffing** ve bunların güvenlik ilkeleri kalan her biri için yineleme adım yukarıdaki 4:

   * Çalışan verileri: Ortak çalışan raporları
   * Çalışan verileri: Tüm Pozisyonlar
   * Çalışan verileri: Geçerli personel bilgileri
   * Çalışan verileri: Çalışan profilindeki iş başlığı
   
7. Seçme ekran işlevsel alanları ve bu süre, arama için geri dönmek için yukarıdaki 1. adımı yineleyin **irtibat bilgileri**Staffing işlevsel alanı seçin ve tıklatın **Tamam**.

8.  Güvenlik ilkeleri Staffing işlevsel alan için listesinde genişletin **çalışan verileri: çalışma kişi bilgilerini**ve yineleyin yukarıdaki 4 aşağıdaki güvenlik ilkeleri için:

    * Çalışan verileri: İş e-posta

    ![Etki alanı güvenlik ilkeleri](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "etki alanı güvenlik ilkeleri")  
    
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

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>1. Kısım: sağlama bağlayıcı uygulama ekleme ve iş günü bağlantısı oluşturma

**Active Directory sağlama için Workday yapılandırmak için:**

1.  Git <https://portal.azure.com>

2.  Sol gezinti çubuğunda seçin **Azure Active Directory**

3.  Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4.  Seçin **bir uygulama eklemek**seçip **tüm** kategorisi.

5.  Arama **Workday sağlama Active Directory'ye**ve bu uygulama Galeriden ekleyin.

6.  Uygulama eklenir ve uygulama ayrıntıları ekranına gösterilen, select sonra **sağlama**

7.  Değişiklik **sağlama** **modu** için **otomatik**

8.  Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – eklenmiş Kiracı etki alanı adıyla Workday entegrasyonu sistem hesabı kullanıcı adı girin. **Gibi görünmelidir:username@contoso4**

   * **Yönetici parolası –** Workday entegrasyonu sistem hesabı için parola girin

   * **Kiracı URL –** kiracınız için Workday web hizmetleri uç noktası için URL'yi girin. Aşağıdaki gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4, burada contoso4 doğru Kiracı adınız ile değiştirilir ve wd3 impl doğru ortamı dize ile değiştirilir.

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

      * **Kaynak özniteliği** -Workday kullanıcı özniteliği.

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

-   AD özniteliği parentDistinguishedName eşlemeleri ifadesi, bir veya daha fazla iş günü kaynak özniteliklerini temel alarak belirli bir kuruluş için bir kullanıcı sağlamak için kullanılabilir. Bu örnek kullanıcılar Şehir verilerine bağlı olarak farklı OU'lar iş günü içinde yerleştirir.

-   UserPrincipalName AD özniteliğine eşlenir ifadesi oluşturma UPN firstName.LastName@contoso.com. Geçersiz özel karakterler yerini alır.

-   [Burada ifadeleri yazma belge yok](active-directory-saas-writing-expressions-for-attribute-mappings.md)

  
| İŞ GÜNÜ ÖZNİTELİĞİ | ACTIVE DIRECTORY ÖZNİTELİĞİ |  KİMLİĞİ EŞLEŞİYOR MU? | OLUŞTUR / GÜNCELLEŞTİR |
| ---------- | ---------- | ---------- | ---------- |
|  **WorkerID**  |  EmployeeID | **Evet** | Yazılan üzerinde yalnızca oluştur | 
|  **Belediye**   |   m   |     | Oluştur + güncelleştir |
|  **Şirket**         | Şirket   |     |  Oluştur + güncelleştir |
|  **CountryReferenceTwoLetter**      |   Ortak |     |   Oluştur + güncelleştir |
| **CountryReferenceTwoLetter**    |  C  |     |         Oluştur + güncelleştir |
| **SupervisoryOrganization**  | Bölüm  |     |  Oluştur + güncelleştir |
|  **PreferredNameData**  |  Görünen adı |     |   Oluştur + güncelleştir |
| **EmployeeID**    |  CN =    |   |   Yazılan üzerinde yalnızca oluştur |
| **Faks**      | facsimileTelephoneNumber     |     |    Oluştur + güncelleştir |
| **FirstName**   | givenName       |     |    Oluştur + güncelleştir |
| **Anahtar (\[etkin\],, "0", "True", "1")** |  AccountDisabled      |     | Oluştur + güncelleştir |
| **Mobil**  |    Mobil       |     |       Oluştur + güncelleştir |
| **EmailAddress**    | Posta    |     |     Oluştur + güncelleştir |
| **ManagerReference**   | Yöneticisi  |     |  Oluştur + güncelleştir |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Oluştur + güncelleştir |
| **Posta kodu**  |   posta kodu  |     | Oluştur + güncelleştir |
| **LocalReference** |  preferredLanguage  |     |  Oluştur + güncelleştir |
| ** Değiştirin (Mid (Değiştir (\[EmployeeID\],, "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\ \\ &lt; \\ \\ &gt; \]) "," ",), 1, 20)," ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**      |    SAMAccountName            |     |         Yazılan üzerinde yalnızca oluştur |
| **Soyadı**   |   sn   |     |  Oluştur + güncelleştir |
| **CountryRegionReference** |  St     |     | Oluştur + güncelleştir |
| **AddressLineData**    |  StreetAddress  |     |   Oluştur + güncelleştir |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Oluştur + güncelleştir |
| **BusinessTitle**   |  Başlık     |     |  Oluştur + güncelleştir |
| **Join("@",Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Join(".", [FirstName], [LastName]), , "([Øø])", , "oe", , ), , "[Ææ]", , "ae", , ), , "([äãàâãåáąÄÃÀÂÃÅÁĄA])", , "a", , ), , "([B])", , "b", , ), , "([CçčćÇČĆ])", , "c", , ), , "([ďĎD])", , "d", , ), , "([ëèéêęěËÈÉÊĘĚE])", , "e", , ), , "([F])", , "f", , ), , "([G])", , "g", , ), , "([H])", , "h", , ), , "([ïîìíÏÎÌÍI])", , "i", , ), , "([J])", , "j", , ), , "([K])", , "k", , ), , "([ľłŁĽL])", , "l", , ), , "([M])" ,, "m",), "([ñńňÑŃŇN])", "n",), "([öòőõôóÖÒŐÕÔÓO])", "o",), "([P])", "p",), "([Q])", "q",), "([řŘR])", "r",), "([ßšśŠŚS])", "s",), "([TŤť])", "t",), "([üùûúůűÜÙÛÚŮŰU])", "u",), "([V])", "v",), "([]) w" harfinin, "w",), "([ýÿýŸÝY])", "y",), "([źžżŹŽŻZ])", "z",), "",,, "",), "contoso.com")**   | userPrincipalName     |     | Oluştur + güncelleştir                                                   
| **Anahtar (\[belediye\], "OU standart kullanıcılar, OU = Kullanıcılar, OU = varsayılan, OU = konumları, DC = contoso, DC = com =", "Dallas" "OU standart kullanıcılar, OU = Kullanıcılar, OU = Dallas, OU = konumları, DC = contoso, DC = com =", "Ankara'da" "OU standart kullanıcılar, OU = Kullanıcılar, OU = Ankara'da, OU = konumları, DC = contoso, DC = com =", "Seattle", "OU standart kullanıcılar, OU = Kullanıcılar, OU = Seattle, OU = konumları, DC = contoso, DC = com =", "Londra", "OU standart kullanıcılar = OU Kullanıcılar, OU = Londra, OU = konumları, DC = contoso, DC = com = ")**  | parentDistinguishedName     |     |  Oluştur + güncelleştir |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a>3. Kısım: şirket içi eşitleme Aracısı'nı yapılandırma

Active Directory şirket içi sağlamak için bir aracı Active Directory ormanı isteğini bir etki alanına katılmış sunucuda yüklenmesi gerekir. Yordamı tamamlamak için gerekli kimlik bilgileri etki alanı yöneticisi (veya Kurumsal Yönetici).

**[Şirket içi eşitleme Aracısı indirebilirsiniz](https://go.microsoft.com/fwlink/?linkid=847801)**

Aracıyı yükledikten sonra ortamınız için aracısını yapılandırmak için aşağıdaki Powershell komutları çalıştırın.

**#1 komutu**

> CD C:\\Program dosyaları\\Microsoft Azure Active Directory Eşitleme Aracı\\modülleri\\AADSyncAgent

> Import-module AADSyncAgent.psd1

**Komut #2**

> Ekleme ADSyncAgentActiveDirectoryConfiguration

* Giriş: "Dizin", AD orman adı kısmen girildiği gibi adı \#2
* Giriş: Yönetici kullanıcı adı ve parolası Active Directory ormanı için

**Komut #3**

> Ekleme ADSyncAgentAzureActiveDirectoryConfiguration

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
> Tanımlayıcı: WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

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
4. Https://Manage.hub.syncfabric.windowsazure.com/Management ile Değiştir **https://eu.manage.hub.syncfabric.windowsazure.com/Management**
5. Https://provision.hub.syncfabric.windowsazure.com/Provisioning ile Değiştir **https://eu.provision.hub.syncfabric.windowsazure.com/Provisioning**
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

2. **Kaydet** düğmesine tıklayın.

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

1.  Git <https://portal.azure.com>.

2.  Sol gezinti çubuğunda seçin **Azure Active Directory**

3.  Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4.  Seçin **bir uygulama eklemek**ve ardından **tüm** kategorisi.

5.  Arama **Azure AD sağlama Workday**ve bu uygulama Galeriden ekleyin.

6.  Uygulama eklenir ve uygulama ayrıntıları ekranına gösterilen, select sonra **sağlama**

7.  Değişiklik **sağlama** **modu** için **otomatik**

8.  Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – eklenmiş Kiracı etki alanı adıyla Workday entegrasyonu sistem hesabı kullanıcı adı girin. Gibi görünmelidir:username@contoso4

   * **Yönetici parolası –** Workday entegrasyonu sistem hesabı için parola girin

   * **Kiracı URL –** kiracınız için Workday web hizmetleri uç noktası için URL'yi girin. Aşağıdaki gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4, burada contoso4 doğru Kiracı adınız ile değiştirilir ve wd3 impl doğru ortamı dize ile değiştirilir. Bu URL bilinmiyor, Lütfen doğru URL'sini belirlemek için Workday entegrasyonu iş ortağı veya destek temsilcinizle çalışır.

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

   * **Kaynak özniteliği** -Workday kullanıcı özniteliği.

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

2. **Kaydet** düğmesine tıklayın.

3. Bu değişken sayıda iş günü içinde kaç kullanıcılardır bağlı olarak saatler sürebilir ilk eşitlemeyi başlatır.

4. Bireysel eşitleme olayları görüntülenebilir **denetim günlüklerini** sekmesi. **[Denetim günlüklerini okumak ayrıntılı yönergeler için sağlama raporlama Kılavuzu bakın.](active-directory-saas-provisioning-reporting.md)**

5. Bir tamamlandı, onu bir denetim özet raporu yazacak **sağlama** sekmesinde, aşağıda gösterildiği gibi.


## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Geri yazma Workday e-posta adreslerini yapılandırma
Azure Active Directory'den kullanıcı e-posta adreslerini Workday geri yazma yapılandırmak için bu yönergeleri izleyin.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>1. Kısım: sağlama bağlayıcı uygulama ekleme ve iş günü bağlantısı oluşturma

**Active Directory sağlama için Workday yapılandırmak için:**

1.  Git <https://portal.azure.com>

2.  Sol gezinti çubuğunda seçin **Azure Active Directory**

3.  Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4.  Seçin **bir uygulama eklemek**seçeneğini belirleyip **tüm** kategorisi.

5.  Arama **Workday geri yazma**, bu uygulama Galeriden ekleyin.

6.  Uygulama eklenir ve uygulama ayrıntıları ekranına gösterilen, select sonra **sağlama**

7.  Değişiklik **sağlama** **modu** için **otomatik**

8.  Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – eklenmiş Kiracı etki alanı adıyla Workday entegrasyonu sistem hesabı kullanıcı adı girin. Gibi görünmelidir:username@contoso4

   * **Yönetici parolası –** Workday entegrasyonu sistem hesabı için parola girin

   * **Kiracı URL –** kiracınız için Workday web hizmetleri uç noktası için URL'yi girin. Aşağıdaki gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4, burada contoso4 doğru Kiracı adınız ile değiştirilir ve (gerekirse) wd3 impl doğru ortamı dizesi ile değiştirilir.

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

2. **Kaydet** düğmesine tıklayın.

3. Bu değişken sayıda iş günü içinde kaç kullanıcılardır bağlı olarak saatler sürebilir ilk eşitlemeyi başlatır.

4. Bireysel eşitleme olayları görüntülenebilir **denetim günlüklerini** sekmesi. **[Denetim günlüklerini okumak ayrıntılı yönergeler için sağlama raporlama Kılavuzu bakın.](active-directory-saas-provisioning-reporting.md)**

5. Bir tamamlandı, onu bir denetim özet raporu yazacak **sağlama** sekmesinde, aşağıda gösterildiği gibi.

## <a name="known-issues"></a>Bilinen sorunlar

* Çalıştırırken **Ekle ADSyncAgentAzureActiveDirectoryConfiguration** Powershell komutu, şu anda özel bir etki alanı kullanıyorsanız çalışmıyor genel yönetici kimlik bilgileri bilinen bir sorun olduğunu (örnek: admin@contoso.com) . Geçici bir çözüm olarak oluşturun ve Azure AD'de bir onmicrosoft.com etki alanı ile bir genel yönetici hesabı kullanın (örnek: admin@contoso.onmicrosoft.com).

* Avrupa Birliği bulunan Azure AD kiracılarıyla görünmeyen denetim günlüklerini önceki bir sorun çözüldü. Ancak, ek Aracısı yapılandırması AB Azure AD kiracıları için gereklidir. Ayrıntılar için bkz [bölümü 3: şirket içi eşitleme Aracısı'nı yapılandırma](#Part 3: Configure the on-premises synchronization agent)

## <a name="additional-resources"></a>Ek kaynaklar
* [Öğretici: çoklu oturum açma Workday ve Azure Active Directory arasında yapılandırma](active-directory-saas-workday-tutorial.md)
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)
