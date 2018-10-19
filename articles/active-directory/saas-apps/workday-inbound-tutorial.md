---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Workday yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Workday kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/18/2018
ms.author: asmalser
ms.openlocfilehash: 2ab2ac34132eff65e1d6c77794486bc8d9858b40
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408192"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-preview"></a>Öğretici: Workday otomatik kullanıcı hazırlama (Önizleme) için yapılandırma

Bu öğreticinin amacı, kişiler için Workday bazı öznitelikler isteğe bağlı geri yazma ile Active Directory ve Azure Active Directory içinde Workday'den almak için gerçekleştirmeniz gereken adımlarda sağlamaktır.

## <a name="overview"></a>Genel Bakış

[Azure Active Directory kullanıcı sağlama hizmeti](../manage-apps/user-provisioning.md) ile tümleşir [Workday, İnsan Kaynakları API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) kullanıcı hesapları sağlamak için. Azure AD sağlama iş akışları şu kullanıcının etkinleştirmek için bu bağlantı kullanır:

* **Active Directory kullanıcılara sağlama** -workday'deki kullanıcıları ayıklayıp sorgulayabilecek seçili kümesi bir veya daha fazla Active Directory ormanları eşitleyin.

* **Yalnızca bulutta yer alan kullanıcılar Azure Active Directory'ye sağlama** - senaryolarında olduğu Active Directory kullanılmaz, şirket içinde kullanıcıların sağlanabilir Workday'den doğrudan Azure sağlama hizmetini Azure AD kullanıcısını kullanarak Active Directory için. 

* **Geri yazma e-posta adresleri için Workday** -Azure AD kullanıcı sağlama hizmeti Azure AD kullanıcılarını e-posta adresleri için Workday yazabilirsiniz. 

### <a name="what-human-resources-scenarios-does-it-cover"></a>Hangi İnsan Kaynakları senaryoları kapsıyor mu?

Azure AD kullanıcı sağlama hizmeti tarafından desteklenen Workday kullanıcı sağlama iş akışları şu İnsan Kaynakları ve kimlik yaşam döngüsü yönetim senaryolarının otomasyonunu sağlar:

* **Yeni çalışanların işe** - yeni bir çalışan için Workday, eklendiğinde, bir kullanıcı hesabı Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak otomatik olarak oluşturulur ve [AzureADtarafındandesteklenendiğerSaaSuygulamaları](../manage-apps/user-provisioning.md), e-posta adresinin Workday geri yazma özelliğiyle.

* **Çalışan özniteliği ve profili güncelleştirmeleri** - bir çalışan kaydı (kendi ad, başlık veya Yöneticisi gibi) iş günü içinde güncelleştirilir, kendi kullanıcı hesabına otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak güncelleştirilmesi ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarına](../manage-apps/user-provisioning.md).

* **Çalışan sonlandırmalar** -, Workday'de bir çalışan sonlandırıldığında, kullanıcı hesabı otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak devre dışıdır ve [Azure tarafından desteklenen diğer SaaS uygulamaları AD](../manage-apps/user-provisioning.md).

* **Çalışan yeniden hires** - bir çalışan, iş günü içinde rehired eski, hesap otomatik olarak yeniden veya Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve (tercihinizebağlıolarak)yenidensağlanan[Azure AD tarafından desteklenen diğer SaaS uygulamalarına](../manage-apps/user-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>İçin en iyi bu kullanıcı sağlama çözümünü kim uygun?

Çözüm sağlama bu Workday kullanıcı şu anda genel Önizleme aşamasındadır ve ideal olarak uygundur:

* Workday'den kullanıcı hazırlama için önceden oluşturulmuş, bulut tabanlı bir çözüm bağlamasına kuruluşlar

* Active Directory veya Azure Active Directory Workday'den doğrudan kullanıcı sağlamayı gerektiren kuruluşlar

* Kullanıcıların verileri kullanarak sağlanacak gerektiren kuruluşlar Workday HCM modülünden alınan (bkz [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Kuruluşların gerektiren birleştirme, taşıma ve kullanıcıların bir veya daha fazla Active Directory ormanları eşitlenmesi bırakarak, etki alanları ve OU'lar yalnızca temel Workday HCM modülünde algılandı bilgilerini değiştirebilirsiniz (bkz [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* E-posta için Office 365 kullanan kuruluşlar

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="planning-your-solution"></a>Çözümünüzü planlama

Workday tümleştirmenizi başlamadan önce aşağıdaki önkoşulları denetleyin ve geçerli Active Directory mimarisi ve kullanıcı gereksinimleri ile Azure Active Directory tarafından sağlanan çözümler sağlama konusunda aşağıdaki yönergeleri okuyun.

### <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Genel yönetici erişimi olan geçerli bir Azure AD Premium P1 aboneliği
* Sınama ve tümleştirme amacıyla Workday uygulama Kiracı
* Sınama amacıyla çalışan verilerini test etmek için yönetici izinleri workday'deki sistem tümleştirme kullanıcısı oluşturun ve değişiklikler yapmak için
* Windows Server 2012 veya üzeri çalıştıran etki alanına katılmış bir sunucu için Active Directory kullanıcı sağlama için ana bilgisayar için gerekli [şirket içi eşitleme Aracısı](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) Active Directory ve Azure AD arasında eşitlemek için

### <a name="solution-architecture"></a>Çözüm mimarisi

Azure AD sağlama bağlayıcılar sağlama ve kimlik Yaşam Döngüsü Yönetimi'nden Workday Active Directory, Azure AD, SaaS uygulamaları ve sonrasındaki gidermenize yardımcı olacak zengin bir özellik kümesi sağlar. Hangi özellikleri kullanır ve çözümünü nasıl ayarlanacağını kuruluşunuzun ortam ve gereksinimlerinize bağlı olarak farklılık gösterir. İlk adım, stok varsa ve kuruluşunuzdaki dağıtılan kaç aşağıdakilerden birini gerçekleştirin:

* Kaç tane Active Directory ormanları kullanılıyor?
* Active Directory etki alanları kaç tane kullanılıyor?
* Kaç tane Active Directory kurumsal birimleri (OU) kullanılıyor?
* Kaç tane Azure Active Directory Kiracı kullanılıyor?
* Active Directory ve Azure Active Directory (örneğin, "hibrit" kullanıcılar) için sağlanması gereken kullanıcılar var mı?
* Azure Active Directory, ancak Active Directory değil (örneğin, "yalnızca bulut" kullanıcılar) için sağlanması gereken kullanıcılar var mı?
* Kullanıcı e-posta adresleri için Workday'e geri yazılması gerekiyor mu?

Bu soruların yanıtlarını aldıktan sonra aşağıdaki yönergeleri izleyerek dağıtım sağlama İş gününüzün planlayabilirsiniz.

#### <a name="using-provisioning-connector-apps"></a>Sağlama bağlayıcı uygulamaları kullanma

Azure Active Directory, Workday ve çok sayıda diğer SaaS uygulamalarını önceden tümleştirilmiş sağlama bağlayıcıları destekler.

Tek bir sağlama Bağlayıcısı, tek bir kaynak sistemi API'SİYLE arabirimleri ve sağlama verilerini tek bir hedef sisteme yardımcı olur. Azure AD destekleyen çoğu sağlama bağlayıcılar, tek bir kaynak ve hedef sisteme (örneğin, Azure AD ServiceNow için) yöneliktir ve uygulama ekleyerek Azure AD uygulama galerisinde (örneğin, ServiceNow) söz konusu ayarlanabilir.

Azure AD'de sağlama bağlayıcı örnekleri ve uygulama örnekleri arasında bire bir ilişki yoktur:

| Kaynak Sistem | Hedef sistem |
| ---------- | ---------- |
| Azure AD kiracısı | SaaS uygulaması |

Ancak, Workday ve Active Directory ile çalışırken, değerlendirilmesi için birden çok kaynak ve hedef sistemler mevcuttur:

| Kaynak Sistem | Hedef sistem | Notlar |
| ---------- | ---------- | ---------- |
| Workday | Active Directory Ormanı | Her bir orman, farklı bir hedef sistem olarak kabul edilir |
| Workday | Azure AD kiracısı | Yalnızca bulutta yer alan kullanıcılar için gerekli |
| Active Directory Ormanı | Azure AD kiracısı | Bu akış AAD Connect tarafından hemen işlenir |
| Azure AD kiracısı | Workday | E-posta adresi için geri yazma |

Bu birden çok iş akışı için birden çok kaynak ve hedef sistemleri kolaylaştırmak için Azure AD uygulama galerisinden ekleyebilirsiniz birden fazla sağlama bağlayıcı uygulama Azure AD sağlar:

![AAD uygulama Galerisi](./media/workday-inbound-tutorial/WD_Gallery.PNG)

* **Active Directory sağlama için workday** -bu uygulamanın kullanıcı hesabı için tek bir Active Directory ormanı Workday'den sağlamayı kolaylaştırır. Birden çok ormanınız varsa, bu uygulamanın bir örneği için sağlamak için ihtiyacınız olan her Active Directory ormanı için Azure AD uygulama galerisinde ekleyebilirsiniz.

* **Azure AD sağlama için workday** - Active Directory Kullanıcıları için Azure Active Directory, bu uygulama, tek bir Azure yalnızca bulut kullanıcıları ayıklayıp sorgulayabilecek sağlanmasını kolaylaştırmak için kullanılabilir eşitlemek için kullanılması gereken aracı olsa da AAD Connect Active Directory kiracısı.

* **Workday geri yazma** -bu uygulamayı Azure Active Directory'den Workday, kullanıcının e-posta adresi geri yazmayı kolaylaştırır.

> [!TIP]
> Normal "İş günü" uygulama, Workday Azure Active Directory ile çoklu oturum açmayı ayarlamak için kullanılır. 

Ayarlanmış ve bu özel bağlayıcı uygulamaları sağlama yapılandırma, bu öğreticinin geri kalan bölümler konudur. Ortamınızda kiracılar hangi sistemlerin, sağlamak ve kaç Active Directory ormanları ve Azure AD için ihtiyacınız olan hangi uygulamaların yapılandırmayı seçtiğinize bağlıdır.

![Genel Bakış](./media/workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Workday'de sistem tümleştirme kullanıcısı yapılandırma
Tüm Workday sağlama bağlayıcılardan oluşan genel bir gereksinim, Workday İnsan Kaynakları API'sine bağlanmak Workday sistem tümleştirme hesabı için kimlik bilgilerini gerektir ' dir. Bu bölümde Workday'de bir sistem entegratörü hesabı oluşturmayı açıklar.

> [!NOTE]
> Bu yordamı atlayabilir ve bunun yerine sistem tümleştirme hesabı olarak Workday genel yönetici hesabını kullanmak mümkündür. Tanıtımlar düzgün çalışıyor, ancak üretim dağıtımları için önerilmez.

### <a name="create-an-integration-system-user"></a>Bir tümleştirme sistemi kullanıcısı oluştur

**Bir tümleştirme sistemi kullanıcısı oluşturmak için:**

1. Bir yönetici hesabını kullanarak Workday kiracınızda oturum oturum açın. İçinde **Workday Workbench**, girin arama kutusuna bir kullanıcı oluşturup ardından **tümleştirme sistemi kullanıcısı Oluştur**.

    ![Kullanıcı oluşturma](./media/workday-inbound-tutorial/IC750979.png "kullanıcı oluşturma")
2. Tamamlamak **tümleştirme sistemi kullanıcısı Oluştur** görev tarafından yeni bir tümleştirme sistemi kullanıcısı için bir kullanıcı adı ve parola belirtin.  
 * Bırakın **gerektiren yeni parolayı sonraki oturum açma** seçeneğinin işaretli olmadığından bu kullanıcının programlı olarak oturum açan.
 * Bırakın **oturum zaman aşımı dakikaları** 0 varsayılan değeri ile hangi engeller kullanıcının oturumları tamamlanmadan zaman aşımına uğramasını.

    ![Tümleştirme sistemi kullanıcısı Oluştur](./media/workday-inbound-tutorial/IC750980.png "tümleştirme sistemi kullanıcısı oluştur")

### <a name="create-a-security-group"></a>Bir güvenlik grubu oluşturun
Bir sınırlandırılmamış tümleştirme sistemi güvenlik grubu oluşturun ve kullanıcıyı kendisine atamanız gerekir.

**Bir güvenlik grubu oluşturmak için:**

1. Girin arama kutusuna güvenlik grubu oluşturun ve ardından **güvenlik grubu oluşturma**.

    ![Güvenlik grubu](./media/workday-inbound-tutorial/IC750981.png "güvenlik grubu oluştur")
2. Tamamlamak **güvenlik grubu oluşturma** görev.  
3. Seçin **tümleştirme sistemi güvenlik grubunu (sınırlandırılmamış)** gelen **kiralanan güvenlik grubu türü** açılır.
4. Olduğu açıkça üyeleri eklenecek bir güvenlik grubu oluşturun.

    ![Güvenlik grubu](./media/workday-inbound-tutorial/IC750982.png "güvenlik grubu oluştur")

### <a name="assign-the-integration-system-user-to-the-security-group"></a>Tümleştirme sistemi kullanıcısı güvenlik grubuna atayın.

**Tümleştirme sistemi kullanıcısı atamak için:**

1. Güvenlik grubunu Düzenle arama kutusuna girin ve ardından **güvenlik grubunu Düzenle**.

    ![Güvenlik grubunu Düzenle](./media/workday-inbound-tutorial/IC750983.png "güvenlik grubunu Düzenle")
1. Arayın ve ada göre yeni tümleştirme güvenlik grubunu seçin.

    ![Güvenlik grubunu Düzenle](./media/workday-inbound-tutorial/IC750984.png "güvenlik grubunu Düzenle")
2. Yeni tümleştirme sistemi kullanıcısı, yeni güvenlik grubuna ekleyin. 

    ![Sistem güvenlik grubu](./media/workday-inbound-tutorial/IC750985.png "sistem güvenlik grubu")  

### <a name="configure-security-group-options"></a>Güvenlik grubu seçeneklerini yapılandırın
Bu adımda, etki alanı güvenlik ilkesi güvenlik grubuna çalışan veriler için izinler.

**Güvenlik grubu seçeneklerini yapılandırmak için:**

1. Girin **etki alanı güvenlik ilkeleri** arama kutusuna ve ardından bağlantıyı **işlevsel alan için etki alanı güvenlik ilkeleri**.  

    ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/IC750986.png "etki alanı güvenlik ilkeleri")  
2. Sistem ve seçin için arama **sistem** işlevsel alan.  **Tamam** düğmesine tıklayın.  

    ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/IC750987.png "etki alanı güvenlik ilkeleri")  
3. Sistem işlevsel alan için güvenlik ilkeleri listesinde genişletin **güvenlik yönetimi** ve etki alanı güvenlik ilkesi seçin **Harici hesap sağlama**.  

    ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/IC750988.png "etki alanı güvenlik ilkeleri")  
1. Tıklayın **izinleri Düzenle**ve ardından **izinleri Düzenle** iletişim sayfası, güvenlik grupları listesine yeni bir güvenlik grubu Ekle **alma** ve **yerleştirin**  tümleştirme izinleri.

    ![İzni Düzenle](./media/workday-inbound-tutorial/IC750989.png "izni Düzenle")  

1. Bu ilkelerin her birinde kalan güvenlik için yukarıdaki 1-4 arası adımları yineleyin:

| İşlem | Etki alanı güvenlik ilkesi |
| ---------- | ---------- | 
| GET ve Put | Çalışan verileri: Ortak çalışanı raporları |
| GET ve Put | Çalışan verileri: İş iletişim bilgileri |
| Al | Çalışan verileri: Tüm konumlar |
| Al | Çalışan verileri: Geçerli personel bilgileri |
| Al | Çalışan verileri: Çalışan profilindeki iş başlığı |


### <a name="activate-security-policy-changes"></a>Güvenlik İlkesi değişikliklerini etkinleştirin

**Güvenlik İlkesi değişiklikleri etkinleştirmek için:**

1. Girin arama kutusuna etkinleştirin ve ardından bağlantıyı **etkinleştirme bekleyen güvenlik ilkesi değişikliklerini**.

    ![Etkinleştirme](./media/workday-inbound-tutorial/IC750992.png "etkinleştir") 
2. Bekleyen Güvenlik İlkesi değişikliklerini etkinleştir görev denetim amacıyla bir açıklama girerek başlayın ve ardından **Tamam**. 

    ![Bekleyen güvenlik ayarlarını etkinleştir](./media/workday-inbound-tutorial/IC750993.png "bekleyen güvenlik ayarlarını etkinleştir")  
1. Onay kutusunu işaretleyerek sonraki ekranda bir görevi tamamlamak **Onayla**ve ardından **Tamam**.

    ![Bekleyen güvenlik ayarlarını etkinleştir](./media/workday-inbound-tutorial/IC750994.png "bekleyen güvenlik ayarlarını etkinleştir")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Active Directory'ye Workday'den kullanıcı sağlamayı yapılandırma
Kullanıcı hesabı için sağlama gerektiren her bir Active Directory ormanına Workday'den sağlama yapılandırmak için bu yönergeleri izleyin.

### <a name="planning"></a>Planlama

Bir Active Directory ormanı için kullanıcı sağlamayı yapılandırmadan önce aşağıdaki soruları göz önünde bulundurun. Bu soruların yanıtlarını ayarlamak kapsam belirleme filtrelerini ve öznitelik eşlemelerini nasıl gerektiğini belirler. 

* **Workday'deki hangi kullanıcıların bu Active Directory ormanına sağlanması gerekir?**

   * *Örnek: Burada iş günü "Şirket" özniteliği değeri "Contoso" içeren ve "Worker_Type" özniteliği "Normal" içeren kullanıcılar*

* **Kullanıcılar farklı kuruluş birimlerine (OU) nasıl yönlendirileceğini?**

   * *Örnek: Kullanıcıların bir office konuma karşılık gelen OU'ları Workday "Belediye" ve "Country_Region_Reference" öznitelikler tanımlanan yönlendirilir*

* **Aşağıdaki öznitelikler Active Directory'de nasıl doldurulur?**

   * Ortak ad (cn)
      * *Örnek: İnsan kaynakları tarafından belirlenen Workday USER_ID değeri kullanın.*
      
   * Çalışan kimliği (EmployeeID)
      * *Örnek: Workday Worker_ID değerini kullanın.*
      
   * SAM hesabı adı (sAMAccountName)
      * *Örnek: geçersiz karakterler kaldırmak için ifade bir Azure AD sağlama filtrelenmiş Workday USER_ID değeri kullanın.*
      
   * Kullanıcı asıl adı (userPrincipalName)
      * *Örnek: Workday USER_ID değeri, bir etki alanı adı ekleme ifade sağlama bir Azure AD ile birlikte kullanın.*

* **Nasıl kullanıcılar Workday ile Active Directory arasında eşleşmesi?**

  * *Örnek: Kullanıcılar belirli bir iş günü "değeri eşleştirilir Worker_ID" ile "EmployeeID" aynı değere sahip olduğu Active Directory Kullanıcıları. Active Directory'de Worker_ID değeri bulunamazsa, ardından yeni bir kullanıcı oluşturun.*
  
* **Active Directory ormanında zaten kullanıcı çalışmak için eşleşen mantığı için gerekli kimlikleri içeriyor mu?**

  * *Örnek: Bu yeni bir iş günü dağıtım ise, Active Directory ile eşleşen mantıksal mümkün olduğunca basit tutmak için doğru Workday Worker_ID değerleri (veya tercih ettiğiniz benzersiz kimlik değerini) önceden doldurulmuş olması önerilir.*
    
    
### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>1. Bölüm: sağlama bağlayıcı uygulama ekleme ve Workday bağlantısı oluşturma

**Active Directory sağlama için Workday yapılandırmak için:**

1.  Şuraya gidin: <https://portal.azure.com>

2.  Sol gezinti çubuğunda **Azure Active Directory**

3.  Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4.  Seçin **uygulama ekleme**seçip **tüm** kategorisi.

5.  Arama **Active Directory'ye Workday sağlama**ve bu uygulama galerideki ekleyin.

6.  Uygulama eklenir ve uygulama Ayrıntılar ekranında gösterilen, seçin sonra **sağlama**

7.  Değişiklik **sağlama** **modu** için **otomatik**

8.  Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – Kiracı etki alanı adı eklenir Workday tümleştirmesi sistem hesabının kullanıcı adını girin. **Aşağıdaki gibi görünmelidir: username@contoso4**

   * **Yönetici parolası –** Workday tümleştirmesi sistem hesabının parolasını girin

   * **Kiracı URL'si –** kiracınız için Workday web hizmetleri uç nokta URL'sini girin. Aşağıdaki gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resourcesburada contoso4 doğru Kiracı adınızla değiştirilir ve wd3 Impl doğru ortamı dize ile değiştirilir.

   * **Active Directory ormanı -** Get-ADForest powershell komutu tarafından döndürülen "Name", Active Directory orman. Genellikle gibi dize budur: *contoso.com*

   * **Active Directory kapsayıcısı -** AD ormanınızdaki tüm kullanıcıları içeren kapsayıcı dizeyi girin. Örnek: *OU standart kullanıcılar, OU = Kullanıcılar, DC = contoso, DC = test =*

   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.

   * Tıklayın **Test Bağlantısı** düğmesi. Bağlantı testi başarılı olursa tıklayın **Kaydet** üstünde düğme. Başarısız olursa, Workday kimlik Workday'de geçerli olduğunu denetleyin. 

![Azure portal](./media/workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>2. Bölüm: öznitelik eşlemelerini yapılandırma 

Bu bölümde, Active Directory'ye Workday'den kullanıcı verilerin nasıl aktığını yapılandıracaksınız.

1.  Sağlama sekmesinde **eşlemeleri**, tıklayın **eşitleme Workday'deki çalışanları için şirket içi**.

2.  İçinde **kaynak nesne kapsamı** alan, kullanıcıların hangi kümesi workday'deki AD için öznitelik tabanlı bir filtre kümesi tanımlayarak sağlama kapsamında olmalıdır seçebilirsiniz. Varsayılan "iş günü içinde tüm kullanıcılar" kapsamıdır. Örnek filtreler:

   * Örnek: Kapsam kullanıcılara 1000000 2000000 arasındaki çalışan kimlikleri

      * Öznitelik: WorkerID

      * İşleci: Düzenli İFADESİ eşleştirmesi

      * Değer: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Örnek: Yalnızca çalışanları ve bağlı çalışanları değil 

      * Öznitelik: EmployeeID

      * İşleç: NULL değil

3.  İçinde **hedef nesne eylemleri** alan, küresel olarak filtreleyebilirsiniz Active Directory üzerinde gerçekleştirilecek eylemleri izin verilir. **Oluşturma** ve **güncelleştirme** en yaygın olarak.

4.  İçinde **öznitelik eşlemelerini** bölümünde, tek tek nasıl Workday tanımlayabilirsiniz öznitelikleri eşlemek için Active Directory öznitelikleri.

5. Güncelleştirmek için varolan bir özniteliği eşlemesini üzerinde veya tıklatın **yeni eklemesi** yeni eşlemeler eklemek için ekranın alt kısmındaki. Tek tek özellik eşlemesi, bu özellikleri destekler:

      * **Eşleme türü**

         * **Doğrudan** – Workday özniteliğinin değeri değişikliğine gerek olmadan AD özniteliği Yazar

         * **Sabit** -AD özniteliği için bir statik, sabit dize değeri yazın

         * **İfade** – bir veya daha fazla Workday özniteliklerine dayalı, AD özniteliği için özel bir değer yazmanızı sağlar. [Daha fazla bilgi için bu makalede ifadelerini bkz](../manage-apps/functions-for-customizing-application-data.md).

      * **Kaynak özniteliği** -Workday kullanıcı özniteliği. Aradığınız özniteliği mevcut değilse bkz [Workday kullanıcı özniteliklerinin listesi özelleştirme](#customizing-the-list-of-workday-user-attributes).

      * **Varsayılan değer** : isteğe bağlı. Kaynak özniteliği boş bir değere sahipse, eşleme bu değeri yerine yazın.
            Bu alanı boş bırakırsanız en yaygın yapılandırmadır.

      * **Hedef öznitelik** – Active Directory'deki kullanıcı özniteliği.

      * **Bu özniteliği kullanarak nesneleri eşleşen** – Bu eşleme kullanıcılar Workday ile Active Directory arasında benzersiz olarak tanımlanabilmesi için kullanılması gereken olup olmadığını. Bu genellikle, çalışan kimliği alanı genellikle Active Directory çalışanın Kimliğini öznitelikleri birine eşlenmiş iş günü için ayarlanır.

      * **Eşleşen öncelik** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alan tarafından tanımlanan sırayla değerlendirilir. Bir eşleşme bulunduğu sürece başka eşleştirme öznitelikleri değerlendirilir.

      * **Bu eşlemeyi Uygula**
       
         * **Her zaman** – bu eşlemeyi Uygula her iki kullanıcı oluşturma ve güncelleştirme eylemleri

         * **Yalnızca oluşturma sırasında** -yalnızca kullanıcı oluşturma eylemleri bu eşlemeyi Uygula

6. Eşlemelerinizi kaydetmek için tıklatın **Kaydet** öznitelik eşlemesi bölümün üst kısmındaki.

![Azure portal](./media/workday-inbound-tutorial/WD_2.PNG)

**Aşağıda bazı örnek, bazı yaygın ifadelerle Workday ve Active Directory arasında öznitelik eşlemelerini verilmiştir**

-   ParentDistinguishedName özniteliğine eşleyen bir ifade, bir veya daha fazla Workday kaynak özniteliklerine dayalı farklı kuruluş birimlerini kullanıcılara sağlamak için kullanılır. Bu örnek kullanıcılara göre farklı OU'larda yerleştirir içinde bulundukları üzerinde hangi şehirde.

-   UserPrincipalName özniteliği Active Directory'de Workday'deki kullanıcı kimliği bir etki alanı soneki ile birleştirerek oluşturulur

-   [Burada ifadeler yazma belgeleri yoktur](../manage-apps/functions-for-customizing-application-data.md). Bu özel karakterleri kaldırma örnekleri içerir.

  
| WORKDAY ÖZNİTELİĞİ | ACTIVE DIRECTORY ÖZNİTELİĞİ |  KİMLİĞİ EŞLEŞİYOR MU? | OLUŞTURMA / GÜNCELLEŞTİRME |
| ---------- | ---------- | ---------- | ---------- |
| **WorkerID**  |  EmployeeID | **Evet** | Yazılan yalnızca oluşturma sırasında |
| **Kullanıcı Kimliği**    |  CN =    |   |   Yazılan yalnızca oluşturma sırasında |
| **Birleştirme ("@", [UserID], "contoso.com")**   | userPrincipalName     |     | Yazılan yalnızca oluşturma sırasında 
| **Değiştir (Orta (değiştirin (\[UserID\],, "(\[ \\ \\ / \\ \\ \\ \\ \\ \\ \[ \\\\\]\\\\:\\\\;\\ \\|\\\\=\\\\,\\\\+\\\\\*\\ \\? \\ \\ &lt; \\ \\ &gt; \]) "," ",), 1, 20)," ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**      |    SAMAccountName            |     |         Yazılan yalnızca oluşturma sırasında |
| **Anahtar (\[etkin\],, "0", "True", "1")** |  accountDisabled      |     | Oluşturun ve güncelleştirme |
| **FirstName**   | givenName       |     |    Oluşturun ve güncelleştirme |
| **Soyadı**   |   sn   |     |  Oluşturun ve güncelleştirme |
| **PreferredNameData**  |  displayName |     |   Oluşturun ve güncelleştirme |
| **Şirket**         | Şirket   |     |  Oluşturun ve güncelleştirme |
| **SupervisoryOrganization**  | Bölüm  |     |  Oluşturun ve güncelleştirme |
| **ManagerReference**   | yönetici  |     |  Oluşturun ve güncelleştirme |
| **BusinessTitle**   |  başlık     |     |  Oluşturun ve güncelleştirme | 
| **AddressLineData**    |  streetAddress  |     |   Oluşturun ve güncelleştirme |
| **Belediye**   |   m   |     | Oluşturun ve güncelleştirme |
| **CountryReferenceTwoLetter**      |   Ortak |     |   Oluşturun ve güncelleştirme |
| **CountryReferenceTwoLetter**    |  c  |     |         Oluşturun ve güncelleştirme |
| **CountryRegionReference** |  St     |     | Oluşturun ve güncelleştirme |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Oluşturun ve güncelleştirme |
| **posta kodu**  |   posta kodu  |     | Oluşturun ve güncelleştirme |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Oluşturun ve güncelleştirme |
| **Faks**      | facsimileTelephoneNumber     |     |    Oluşturun ve güncelleştirme |
| **Mobil**  |    Mobil       |     |       Oluşturun ve güncelleştirme |
| **LocalReference** |  preferredLanguage  |     |  Oluşturun ve güncelleştirme |                                               
| **Anahtar (\[belediye\], "OU standart kullanıcılar, OU = Kullanıcılar, OU = varsayılan, OU = konumları, DC = contoso, DC = com", "Dallas", "OU standart kullanıcılar, OU = Kullanıcılar, OU = Dallas, OU = konumları, DC = contoso, DC = com", "Austin", "OU standart kullanıcılar, OU = Kullanıcılar, OU = Austin, OU = konumları, DC = contoso, DC = com ","Seattle"" OU standart kullanıcılar, OU = Kullanıcılar, OU = Seattle, OU = konumları, DC = contoso, DC = com ","London"," OU standart kullanıcılar, OU = Kullanıcılar, OU = Londra, OU = konumları, DC = contoso, DC = com ")**  | parentDistinguishedName     |     |  Oluşturun ve güncelleştirme |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a>Bölüm 3: şirket içi eşitleme Aracısı'nı yapılandırma

Şirket için Active Directory sağlamak için bir aracı arzusu Active Directory ormanı etki alanına katılmış bir sunucuya yüklenmelidir. Yordamı tamamlamak için kimlik bilgileri gerekli etki alanı yöneticisi (veya kuruluş yöneticisi).

**[Şirket içi eşitleme Aracısı indirebilirsiniz](https://go.microsoft.com/fwlink/?linkid=847801)**

Aracı yükledikten sonra ortamınız için aracıyı yapılandırmak için aşağıdaki Powershell komutları çalıştırın.

**Komut #1**

> CD "C:\Program Files\Microsoft Azure AD Connect Agent\Modules\AADSyncAgent sağlama" Aracısı\\modülleri\\AADSyncAgent

> Import-Module "C:\Program Files\Microsoft Azure AD Connect Agent\Modules\AADSyncAgent\AADSyncAgent.psd1 sağlama"

**Komut #2**

> ADSyncAgentActiveDirectoryConfiguration ekleyin

* Giriş: "Dizin adı" için AD ormanı, kısmen girildiği gibi adını \#2
* Giriş: Yönetici kullanıcı adı ve Active Directory ormanı için parola

>[!TIP]
> "Birincil etki alanı ve güvenilen etki alanı arasındaki ilişki başarısız oldu" hata iletisini alırsanız, yerel makine burada birden çok Active Directory ormanı veya etki alanı yapılandırılır ve en az bir güven yapılandırılmış bir ortamda, olduğundan başarısız olan veya çalışmayan ilişkidir. Sorunu çözmek için düzeltin veya bozulmuş güven ilişkisini kaldırın.

**Komut #3**

> ADSyncAgentAzureActiveDirectoryConfiguration ekleyin

* Giriş: Genel yönetici kullanıcı adı ve Azure AD kiracınızda parola

>[!IMPORTANT]
>Şu anda özel bir etki alanı kullanıyorlarsa çalışmıyor genel yönetici kimlik bilgileri bilinen bir sorun yoktur (örnek: admin@contoso.com). Geçici bir çözüm olarak oluşturun ve bir onmicrosoft.com etki alanı ile bir genel yönetici hesabı kullanın (örnek: admin@contoso.onmicrosoft.com)

>[!IMPORTANT]
>Şu anda çok faktörlü kimlik doğrulaması etkin oluşturulduysa çalışmıyor genel yönetici kimlik bilgileri bilinen bir sorun yoktur. Geçici çözüm olarak, genel yönetici için multi-Factor authentication devre dışı bırakın.

**Komut #4**

> Get-AdSyncAgentProvisioningTasks

* Eylem: veriler döndürülür onaylayın. Bu komut, uygulamaları Azure AD kiracınızda sağlama Workday otomatik olarak bulur. Örnek çıktı:

> Ad: My AD ormanı
>
> Etkin: True
>
> DizinAdı: mydomain.contoso.com
>
> Belgeli: False
>
> Tanımlayıcı: WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**Komut #5**

> Başlangıç AdSyncAgentSynchronization-otomatik

**Komut #6**

> net stop aadsyncagent

**Komut #7**

> net start aadsyncagent

>[!TIP]
>PowerShell'de "net" komutları, ek olarak, eşitleme Aracısı da başlatılabilir ve kullanılarak durdurulan **Services.msc**. Powershell komutlarını çalıştırırken hatalarla karşılaşırsanız, emin **Microsoft Azure AD Connect aracı sağlama** çalıştığı **Services.msc**.

![Hizmetler](./media/workday-inbound-tutorial/Services.png)  

**Avrupa Birliği'nde müşteriler için ek yapılandırma**

Azure Active Directory kiracınızın AB veri merkezlerinden birinde bulunuyorsa, ardından aşağıdaki ek adımları izleyin.

1. Açık **Services.msc**ve durdurma **Microsoft Azure AD Connect aracı sağlama** hizmeti.
2. Aracı yükleme klasörü gidin (örnek: C:\Program Files\Microsoft Azure AD Connect aracı sağlama).
3. Açık **SyncAgnt.exe.config** bir metin düzenleyicisinde.
4. Değiştirin https://manage.hub.syncfabric.windowsazure.com/Management ile **https://eu.manage.hub.syncfabric.windowsazure.com/Management**
5. Değiştirin https://provision.hub.syncfabric.windowsazure.com/Provisioning ile **https://eu.provision.hub.syncfabric.windowsazure.com/Provisioning**
6. Kaydet **SyncAgnt.exe.config** dosya.
7. Açık **Services.msc**ve başlangıç **Microsoft Azure AD Connect aracı sağlama** hizmeti.

**Sorun giderme aracı**

[Windows olay günlüğü](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) Windows Server'da aracıyı barındıran makine aracı tarafından gerçekleştirilen tüm işlemler için olayları içerir. Bu olayları görüntülemek için:
    
1. Açık **Eventvwr.msc**.
2. Seçin **Windows Günlükleri > Uygulama**.
3. Kaynak oturumu tüm olayları görüntüle **AADSyncAgent**. 
4. Hataları ve Uyarıları denetleyin.

Powershell komutlarında sağlanan Active Directory veya Azure Active Directory kimlik bilgileriyle izinlerle ilgili bir sorun varsa, bunun gibi bir hata görürsünüz: 
    
![Olay günlükleri](./media/workday-inbound-tutorial/Windows_Event_Logs.png) 


### <a name="part-4-start-the-service"></a>4. Bölüm: Hizmetini başlatın
1-3 bölümleri tamamladıktan sonra Azure portalında sağlama hizmeti başlatabilirsiniz.

1.  İçinde **sağlama** sekmesinde, belirleyin **sağlama durumu** için **üzerinde**.

2. **Kaydet**’e tıklayın.

3. Bu, değişken sayıda Workday'de kaç kullanıcının olduğunu bağlı olarak saat sürebilir ilk eşitlemeyi başlatır.

4. Herhangi bir zamanda denetleyin **denetim günlükleri** Azure portalında sağlama hizmeti gerçekleştirdiği eylemleri görmek için sekmesinde. Denetim günlüklerini olduğu gibi kullanıcıların Workday dışında okuma gönderildiğini ve ardından daha sonra eklendiğinde veya Active Directory sağlama hizmeti tarafından gerçekleştirilen tüm bireysel eşitleme olayları listeler. **[Denetim günlüklerini okumak hakkında ayrıntılı yönergeler için sağlama raporlama kılavuzuna bakın](../manage-apps/check-status-user-account-provisioning.md)**

1.  Denetleme [Windows olay günlüğü](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) yeni hatalar veya uyarılar için aracıyı barındıran Windows Server makinesinde. Bu olaylar başlatarak görüntülenebilir **Eventvwr.msc** sunucuda seçerek **Windows Günlükleri > Uygulama**. Altında kaynak sağlama ile ilgili tüm iletileri günlüğe kaydedilen **AADSyncAgent**.

6. Bir tamamlandı, bu denetim özet raporu yazacak **sağlama** sekmesinde, aşağıda gösterildiği gibi.

![Azure portal](./media/workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Azure Active Directory'ye kullanıcı sağlamayı yapılandırma
Azure Active Directory'ye sağlama nasıl yapılandırabileceğinizi aşağıdaki tabloda ayrıntılı olarak hazırlama gereksinimlerinize bağlıdır.

| Senaryo | Çözüm |
| -------- | -------- |
| **Kullanıcıları Active Directory ve Azure AD için sağlanması gereken** | Kullanım  **[AAD bağlanma](../hybrid/whatis-hybrid-identity.md)** |
| **Kullanıcıların Active Directory'ye yalnızca sağlanması gerekir** | Kullanım  **[AAD bağlanma](../hybrid/whatis-hybrid-identity.md)** |
| **Kullanıcıların Azure AD'de yalnızca (yalnızca bulutta) sağlanması gerekir** | Kullanım **Azure Active Directory sağlama için Workday** app Galerisi'nde uygulama |

Azure AD Connect ayarlama hakkında yönergeler için bkz: [Azure AD Connect belgelerini](../hybrid/whatis-hybrid-identity.md).

Aşağıdaki bölümlerde, yalnızca bulut kullanıcılarına sağlamak için Workday ile Azure AD arasında bir bağlantı ayarlama açıklanmaktadır.

> [!IMPORTANT]
> Azure AD'ye sağlanması gereken yalnızca bulut kullanıcılarına varsa ve şirket içi Active Directory'nin değil, yalnızca aşağıdaki yordamı izleyin.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>1. Bölüm: Azure AD sağlama bağlayıcı uygulamasını ekleme ve Workday bağlantısı oluşturma

**Yalnızca bulutta yer alan kullanıcılar için Azure Active Directory sağlama için Workday yapılandırmak için:**

1.  <https://portal.azure.com> kısmına gidin.

2.  Sol gezinti çubuğunda **Azure Active Directory**

3.  Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4.  Seçin **uygulama ekleme**ve ardından **tüm** kategorisi.

5.  Arama **Azure AD sağlama için Workday**ve bu uygulama galerideki ekleyin.

6.  Uygulama eklenir ve uygulama Ayrıntılar ekranında gösterilen, seçin sonra **sağlama**

7.  Değişiklik **sağlama** **modu** için **otomatik**

8.  Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – Kiracı etki alanı adı eklenir Workday tümleştirmesi sistem hesabının kullanıcı adını girin. Aşağıdaki gibi görünmelidir: username@contoso4

   * **Yönetici parolası –** Workday tümleştirmesi sistem hesabının parolasını girin

   * **Kiracı URL'si –** kiracınız için Workday web hizmetleri uç nokta URL'sini girin. Aşağıdaki gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resourcesburada contoso4 doğru Kiracı adınızla değiştirilir ve wd3 Impl doğru ortamı dize ile değiştirilir. Lütfen bu URL'yi bilinmiyor kullanılacak URL'nin doğru belirlemek için Workday tümleştirmesi iş ortağı veya destek temsilcinize ile çalışın.

   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.

   * Tıklayın **Test Bağlantısı** düğmesi.

   * Bağlantı testi başarılı olursa tıklayın **Kaydet** üstünde düğme. Başarısız olursa, Workday URL'yi ve kimlik bilgilerini Workday'de geçerli olduğunu denetleyin.

### <a name="part-2-configure-attribute-mappings"></a>2. Bölüm: öznitelik eşlemelerini yapılandırma 

Bu bölümde, kullanıcı verilerini Workday'den Azure Active Directory'ye yalnızca bulut kullanıcıları için nasıl aktığını yapılandıracaksınız.

1. Sağlama sekmesinde **eşlemeleri**, tıklayın **eşitleme çalışanları Azure AD'ye**.

2. İçinde **kaynak nesne kapsamı** alan, kullanıcıların hangi kümesi workday'deki öznitelik tabanlı bir filtre kümesi tanımlayarak Azure AD'ye sağlama kapsamında olmalıdır seçebilirsiniz. Varsayılan "iş günü içinde tüm kullanıcılar" kapsamıdır. Örnek filtreler:

   * Örnek: Kapsam kullanıcılara 1000000 2000000 arasındaki çalışan kimlikleri

      * Öznitelik: WorkerID

      * İşleci: Düzenli İFADESİ eşleştirmesi

      * Değer: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Örnek: Yalnızca bağlı çalışanları ve değil düzenli çalışanları

      * Öznitelik: ContingentID

      * İşleç: NULL değil

3. İçinde **hedef nesne eylemleri** alan, küresel olarak filtreleyebilirsiniz Azure AD üzerinde gerçekleştirilecek eylemleri izin verilir. **Oluşturma** ve **güncelleştirme** en yaygın olarak.

4. İçinde **öznitelik eşlemelerini** bölümünde, tek tek nasıl Workday tanımlayabilirsiniz öznitelikleri eşlemek için Active Directory öznitelikleri.

5. Güncelleştirmek için varolan bir özniteliği eşlemesini üzerinde veya tıklatın **yeni eklemesi** yeni eşlemeler eklemek için ekranın alt kısmındaki. Tek tek özellik eşlemesi, bu özellikleri destekler:

   * **Eşleme türü**

      * **Doğrudan** – Workday özniteliğinin değeri değişikliğine gerek olmadan AD özniteliği Yazar

      * **Sabit** -AD özniteliği için bir statik, sabit dize değeri yazın

      * **İfade** – bir veya daha fazla Workday özniteliklerine dayalı, AD özniteliği için özel bir değer yazmanızı sağlar. [Daha fazla bilgi için bu makalede ifadelerini bkz](../manage-apps/functions-for-customizing-application-data.md).

   * **Kaynak özniteliği** -Workday kullanıcı özniteliği. Aradığınız özniteliği mevcut değilse bkz [Workday kullanıcı özniteliklerinin listesi özelleştirme](#customizing-the-list-of-workday-user-attributes).

   * **Varsayılan değer** : isteğe bağlı. Kaynak özniteliği boş bir değere sahipse, eşleme bu değeri yerine yazın.
            Bu alanı boş bırakırsanız en yaygın yapılandırmadır.

   * **Hedef öznitelik** – Azure AD'de kullanıcı özniteliği.

   * **Bu özniteliği kullanarak nesneleri eşleşen** – Bu eşleme Workday ile Azure AD arasında kullanıcıları benzersiz olarak tanımlanabilmesi için kullanılması gereken olup olmadığını. Bu genellikle, çalışan kimliği alanı genellikle Azure AD'de çalışan ID özniteliği (yeni) ya da uzantısı özniteliği eşlenen iş günü için ayarlanır.

   * **Eşleşen öncelik** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alan tarafından tanımlanan sırayla değerlendirilir. Bir eşleşme bulunduğu sürece başka eşleştirme öznitelikleri değerlendirilir.

   * **Bu eşlemeyi Uygula**

     * **Her zaman** – bu eşlemeyi Uygula her iki kullanıcı oluşturma ve güncelleştirme eylemleri

     * **Yalnızca oluşturma sırasında** -yalnızca kullanıcı oluşturma eylemleri bu eşlemeyi Uygula

6. Eşlemelerinizi kaydetmek için tıklatın **Kaydet** öznitelik eşlemesi bölümün üst kısmındaki.

### <a name="part-3-start-the-service"></a>3. Bölüm: Hizmetini başlatın
1-2 bölümleri tamamladıktan sonra sağlama hizmeti başlatabilirsiniz.

1. İçinde **sağlama** sekmesinde, belirleyin **sağlama durumu** için **üzerinde**.

2. **Kaydet**’e tıklayın.

3. Bu, değişken sayıda Workday'de kaç kullanıcının olduğunu bağlı olarak saat sürebilir ilk eşitlemeyi başlatır.

4. Bireysel eşitleme olayları görüntülenebilir **denetim günlüklerini** sekmesi. **[Denetim günlüklerini okumak hakkında ayrıntılı yönergeler için sağlama raporlama kılavuzuna bakın](../manage-apps/check-status-user-account-provisioning.md)**

5. Bir tamamlandı, bu denetim özet raporu yazacak **sağlama** sekmesinde, aşağıda gösterildiği gibi.

## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Workday e-posta adreslerinin geri yazmayı yapılandırma
Azure Active Directory'den kullanıcı e-posta adreslerini Workday geri yazma yapılandırmak için bu yönergeleri izleyin.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>1. Bölüm: sağlama bağlayıcı uygulama ekleme ve Workday bağlantısı oluşturma

**Active Directory sağlama için Workday yapılandırmak için:**

1. Şuraya gidin: <https://portal.azure.com>

2. Sol gezinti çubuğunda **Azure Active Directory**

3. Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4. Seçin **uygulama ekleme**, ardından **tüm** kategorisi.

5. Arama **Workday geri yazma**ve bu uygulama galerideki ekleyin.

6. Uygulama eklenir ve uygulama Ayrıntılar ekranında gösterilen, seçin sonra **sağlama**

7. Değişiklik **sağlama** **modu** için **otomatik**

8. Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – Kiracı etki alanı adı eklenir Workday tümleştirmesi sistem hesabının kullanıcı adını girin. Aşağıdaki gibi görünmelidir: username@contoso4

   * **Yönetici parolası –** Workday tümleştirmesi sistem hesabının parolasını girin

   * **Kiracı URL'si –** kiracınız için Workday web hizmetleri uç nokta URL'sini girin. Aşağıdaki gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resourcesburada contoso4 doğru Kiracı adınızla değiştirilir ve (gerekirse) wd3 Impl doğru ortamı dize ile değiştirilir.

   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.

   * Tıklayın **Test Bağlantısı** düğmesi. Bağlantı testi başarılı olursa tıklayın **Kaydet** üstünde düğme. Başarısız olursa, Workday URL'yi ve kimlik bilgilerini Workday'de geçerli olduğunu denetleyin.

### <a name="part-2-configure-attribute-mappings"></a>2. Bölüm: öznitelik eşlemelerini yapılandırma 

Bu bölümde, Active Directory'ye Workday'den kullanıcı verilerin nasıl aktığını yapılandıracaksınız.

1. Sağlama sekmesinde **eşlemeleri**, tıklayın **Workday eşitleme Azure AD kullanıcılarını**.

2. İçinde **kaynak nesne kapsamı** alan, isteğe bağlı olarak filtreleyebilirsiniz hangi kullanıcı kümeleri için Azure Active Directory'de e-posta adreslerini yazdığınız geri Workday için. Varsayılan "tüm kullanıcılar Azure AD'de" kapsamıdır. 

3. İçinde **öznitelik eşlemelerini** bölümünde, Azure Active Directory'de Workday'deki çalışan kimliği veya çalışan kimliği depolandığı özniteliğini belirtmek için eşleşen kodunu güncelleştirin. Popüler eşleşen yöntemi Workday'deki çalışan kimliği veya extensionAttribute1 15 çalışan kimliği, Azure AD'de eşitlemek ve ardından bu özniteliği Azure AD'de geri Workday'de kullanıcıları eşleştirmek için sağlamaktır. 

4. Eşlemelerinizi kaydetmek için tıklatın **Kaydet** öznitelik eşlemesi bölümün üst kısmındaki.

### <a name="part-3-start-the-service"></a>3. Bölüm: Hizmetini başlatın
1-2 bölümleri tamamladıktan sonra sağlama hizmeti başlatabilirsiniz.

1. İçinde **sağlama** sekmesinde, belirleyin **sağlama durumu** için **üzerinde**.

2. **Kaydet**’e tıklayın.

3. Bu, değişken sayıda Workday'de kaç kullanıcının olduğunu bağlı olarak saat sürebilir ilk eşitlemeyi başlatır.

4. Bireysel eşitleme olayları görüntülenebilir **denetim günlüklerini** sekmesi. **[Denetim günlüklerini okumak hakkında ayrıntılı yönergeler için sağlama raporlama kılavuzuna bakın](../manage-apps/check-status-user-account-provisioning.md)**

5. Bir tamamlandı, bu denetim özet raporu yazacak **sağlama** sekmesinde, aşağıda gösterildiği gibi.

## <a name="customizing-the-list-of-workday-user-attributes"></a>Workday kullanıcı özniteliklerinin listesi özelleştirme
Active Directory ve Azure AD iki Workday kullanıcı özniteliklerinin varsayılan listesini dahil için uygulamaları sağlama Workday arasından seçim yapabilirsiniz. Ancak, bu liste kapsamlı değildir. Workday yüzlerce olası kullanıcı öznitelikleri, standart veya benzersiz Workday kiracınız olabilir ya da destekler. 

Azure AD sağlama hizmeti, liste veya Workday öznitelik olarak kullanıma sunulan herhangi bir özniteliği eklemek için özelleştirme yeteneği destekler [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) İnsan Kaynakları API işlemi.

Bunu yapmak için kullanmanız gerekir [Workday Studio](https://community.workday.com/studio-download) kullanmak istediğiniz özniteliklerini temsil eder ve ardından Azure portalında Gelişmiş Öznitelik Düzenleyicisi'ni kullanarak sağlama yapılandırmanıza ekleyin XPath ifadeleri ayıklamak için.

**Workday kullanıcı özniteliği için bir XPath ifadesi almak için:**

1. İndirme ve yükleme [Workday Studio](https://community.workday.com/studio-download). Yükleyici erişmek için bir iş günü topluluk hesabı gerekir.

2. Workday Human_Resources WDSL dosyayı bu URL'den indirin: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. Workday Studio'yu başlatın.

4. Komut çubuğundan seçin **Workday > test eden Test Web hizmetinde** seçeneği.

5. Seçin **dış**, 2. adımda indirdiğiniz Human_Resources WSDL dosyasını seçin.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio1.PNG)

6. Ayarlama **konumu** alanı `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, ancak "IMPL-CC", gerçek değiştirerek örnek türü ve "KİRACI" gerçek Kiracı adınızla.

7. Ayarlama **işlemi** için **Get_Workers**

8.  Küçük tıklatın **yapılandırma** istek/yanıt bölmeleri Workday kimlik bilgileriniz ayarlamak için aşağıdaki bağlantıya. Denetleme **kimlik doğrulaması**ve ardından, Workday tümleştirmesi sistem hesabı için kullanıcı adını ve parolasını girin. Kullanıcı adı olarak biçimlendirdiğinizden emin olun name@tenant, bırakıp **WS-güvenlik UsernameToken** seçeneği belirlenmiş.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio2.PNG)

9. **Tamam**’ı seçin.

10. **İstek** bölmesinde, aşağıdaki ve ayarlanmış XML yapıştırma seçeneğiyle **Employee_ID** gerçek bir kullanıcının Workday'deki kiracınızdaki çalışan kimliği. Özniteliğine sahip bir kullanıcı seçin ayıklamak istediğiniz doldurulur.

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
 
11. Tıklayın **İsteği Gönder** (komutu yürütmek için yeşil ok). Başarılı yanıt olarak görüntülenip görüntülenmeyeceğini **yanıt** bölmesi. Yanıt verileri girdiğiniz kullanıcı kimliği olduğundan emin olun ve bir hata denetleyin.

12. Başarılı olursa, XML'den kopyalama **yanıt** bölmesi ve bir XML dosyası olarak kaydedin.

13. Komut çubuğu, Workday Studio'da seçin **Dosya > Dosya Aç...**  kaydettiğiniz XML dosyasını açın. Bu, Workday Studio XML düzenleyicisinde açar.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio3.PNG)

14. Dosya ağacı içinde gezinmek **/env: Zarf > env: gövdesi > wd:Get_Workers_Response > wd:Response_Data > wd: çalışan** kullanıcı verileri bulmak için. 

15. Altında **wd: çalışan**, eklemek istediğiniz özniteliğini bulun ve seçin.

16. Seçili özniteliğinizi işyeri dışında XPath ifadesi kopyalama **belge yolu** alan.

1. Kaldırma **/env:Envelope / env:Body / wd:Get_Workers_Response / wd:Response_Data /** kopyalanan ifadesinden öneki.

18. Kopyalanan ifadedeki son öğenin bir düğüm olup olmadığını (örnek: "/ wd: Birth_Date"), ardından ekleme **/text()** ifadenin sonunda. Bu son öğeden bir öznitelik ise gerekli değildir (örnek: "/@wd: türü").

19. Sonucun şunun gibi olmalıdır `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Azure portalında kopyalayacak budur.


**Sağlama yapılandırmanızı, özel Workday kullanıcı özniteliği eklemek için:**

1. Başlatma [Azure portalında](https://portal.azure.com)ve Bu öğreticide daha önce açıklandığı gibi uygulama sağlama İş gününüzün hazırlama bölümüne gidin.

2. Ayarlama **sağlama durumu** için **kapalı**seçip **Kaydet**. Bu işlem, hazır olduğunuzda, değişikliklerinizi etkili olmanıza yardımcı olur.

3. Altında **eşlemeleri**seçin **işlem çalışanlarına şirket içi eşitleme** (veya **eşitleme çalışanları Azure AD'ye**).

4. Sonraki ekranda kısmına gidin ve seçin **Gelişmiş Seçenekleri Göster**.

5. Seçin **Workday için öznitelik listesini düzenle**.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio_AAD1.PNG)

6. Öznitelik listesi, giriş alanlarının olduğu için alt kısmına kaydırın.

7. İçin **adı**, öznitelik için bir görünen ad girin.

8. İçin **türü**, uygun şekilde, özniteliğe karşılık gelen türünü seçin (**dize** yaygın olarak kullanılır).

9. İçin **API ifadesi**, Workday Studio'dan kopyaladığınız XPath ifadesi girin. Örnek: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Seçin **öznitelik Ekle**.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio_AAD2.PNG)

11. Seçin **Kaydet** yukarıdaki ardından **Evet** iletişim kutusu. Öznitelik eşlemesi ekranı hala açıksa, kapatın.

12. Ana duyulduğundan **sağlama** sekmesinde **işlem çalışanlarına şirket içi eşitleme** (veya **eşitleme çalışanları Azure AD'ye**) yeniden.

13. Seçin **yeni eklemesi**.

14. Yeni bir öznitelik şimdi gözükeceğini **kaynak özniteliği** listesi.

15. İstediğiniz gibi yeni bir öznitelik için bir eşleme ekleyin.

16. İşiniz bittiğinde ayarlamayı unutmayın **sağlama durumu** geri **üzerinde** ve kaydedin.

## <a name="known-issues"></a>Bilinen sorunlar

* Çalıştırırken **Ekle ADSyncAgentAzureActiveDirectoryConfiguration** Powershell komutu, şu anda özel bir etki alanı kullanıyorlarsa çalışmıyor genel yönetici kimlik bilgileri bilinen bir sorun yoktur (örnek: admin@contoso.com) . Geçici bir çözüm olarak oluşturun ve Azure AD'de bir onmicrosoft.com etki alanı ile bir genel yönetici hesabı kullanın (örnek: admin@contoso.onmicrosoft.com).

* Şirket içi Active Directory'de thumbnailPhoto kullanıcı özniteliği için verileri yazma şu anda desteklenmiyor.

* "Azure AD iş günü" bağlayıcı burada AAD Connect etkin Azure AD kiracılarıyla üzerinde şu anda desteklenmiyor.  

* Avrupa Birliği ' yer alan Azure AD kiracılarıyla görünmeyen denetim günlükleri ile bir önceki Sorun giderildi. Ancak, ek aracı yapılandırması AB Azure AD kiracıları için gereklidir. Ayrıntılar için bkz [bölüm 3: şirket içi eşitleme Aracısı'nı yapılandırma](#Part 3: Configure the on-premises synchronization agent)

## <a name="managing-personal-data"></a>Kişisel verileri yönetme

Çözüm için Active Directory sağlama iş günü bir eşitleme aracısı etki alanına katılmış bir sunucuda yüklü olmasını gerektirir ve bu aracı Windows olay günlüğünde, kişisel olarak tanımlanabilir bilgiler içerebilir günlükleri oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
* [Workday Azure Active Directory ile çoklu oturum açmayı yapılandırma hakkında bilgi edinin](workday-tutorial.md)
* [Diğer SaaS uygulamalarına Azure Active Directory ile tümleştirme hakkında bilgi edinin](tutorial-list.md)

