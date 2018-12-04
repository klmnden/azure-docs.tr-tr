---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Workday yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Workday kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
author: cmmdesai
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
ms.author: chmutali
ms.openlocfilehash: 754c3278cb01e010718fa4d3cb257acf6ffe99c9
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849862"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-preview"></a>Öğretici: Workday otomatik kullanıcı hazırlama (Önizleme) için yapılandırma

Bu öğreticinin amacı, çalışan profillerini isteğe bağlı geri yazma özelliğiyle Workday e-posta adresinin hem Active Directory ve Azure Active Directory'yle Workday'den alma işlemini gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="overview"></a>Genel Bakış

[Azure Active Directory kullanıcı sağlama hizmeti](../manage-apps/user-provisioning.md) ile tümleşir [Workday, İnsan Kaynakları API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) kullanıcı hesapları sağlamak için. Azure AD sağlama iş akışları şu kullanıcının etkinleştirmek için bu bağlantı kullanır:

* **Active Directory kullanıcılara sağlama** -workday'deki kullanıcıları ayıklayıp sorgulayabilecek seçili kümesi bir veya daha fazla Active Directory etki alanına eşitleyin.

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

## <a name="solution-architecture"></a>Çözüm Mimarisi

Bu bölümde, ortak karma ortamlar için çözüm mimarisi sağlayan uçtan uca kullanıcı açıklanmaktadır. İki ilişkili akışlar vardır:

* **Yetkili ik veri akışından – şirket içi Active Directory'ye Workday:** Bu akışta alt olaylar (örneğin, New Hires aktarımları Sonlandırmalar) ilk Workday ik Kiracı bulutta oluşur ve ardından olay veri akışları ile şirket içi Active Azure AD üzerinden dizin ve sağlama Aracısı. Olay bağlı olarak, AD'de oluşturma/güncelleştirme/etkinleştirme/devre dışı işlemlerine neden olabilir.
* **Workday için şirket içi Active Directory'den geri yazma akış – e-posta:** Active Directory'deki hesap oluşturma tamamlandıktan sonra Azure AD Connect ile Azure AD ile eşitlenen ve Active Directory'den kaynaklanan e-posta özniteliği sonradan yazılabilir Workday için.

![Genel Bakış](./media/workday-inbound-tutorial/wd_overview.png)

### <a name="end-to-end-user-data-flow"></a>Uçtan uca kullanıcı veri akışı

1. İK takım Workday HCM içinde çalışan işlemler (birleştiriciler/satış/Leavers veya yeni işe alımlar/aktarımları/Sonlandırmalar) gerçekleştirir.
2. Azure AD sağlama hizmeti, Workday'deki ik kimliklerinin zamanlanmış eşitleme çalışır ve şirket içi Active Directory ile eşitleme için işlenmesi gereken değişiklikleri tanımlar.
3. Azure AD sağlama hizmeti AD hesabı oluşturma/güncelleştirme/etkinleştirme/devre dışı bırakma işlemleri içeren bir istek yükü ile şirket içi AAD Connect sağlama Aracısı çağırır.
4. Azure AD Connect aracı sağlama AD hesabı veri ekleme/güncelleştirme için bir hizmet hesabı kullanır.
5. Azure AD Connect / AD eşitleme Altyapısı güncelleştirmeleri AD'de çekmek için delta eşitleme çalıştırır.
6. Active Directory güncelleştirmeleri, Azure Active Directory ile eşitlenir.
7. Workday geri yazma bağlayıcısını yapılandırdıysanız, sonradan yazma e-posta özniteliği Workday için kullanılan eşleşen özniteliğine dayanarak işlemlerini.


## <a name="planning-your-deployment"></a>Dağıtımınızı planlama

Workday tümleştirmenizi başlamadan önce aşağıdaki önkoşulları denetleyin ve geçerli Active Directory mimarisi ve kullanıcı gereksinimleri ile Azure Active Directory tarafından sağlanan çözümler sağlama konusunda aşağıdaki yönergeleri okuyun.

### <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Genel yönetici erişimi olan geçerli bir Azure AD Premium P1 aboneliği
* Sınama ve tümleştirme amacıyla Workday uygulama Kiracı
* Sınama amacıyla çalışan verilerini test etmek için yönetici izinleri workday'deki sistem tümleştirme kullanıcısı oluşturun ve değişiklikler yapmak için
* Active Directory, Windows Server 2012 veya üzeri ile .NET 4.7 + çalıştıran bir sunucu için kullanıcı sağlama için çalışma zamanı ana bilgisayar için gerekli [şirket sağlama aracı](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) Active Directory ve Azure AD arasında eşitlemek için

### <a name="planning-considerations"></a>Planlama konusunda dikkat edilmesi gerekenler

Azure AD sağlama bağlayıcılar sağlama ve kimlik Yaşam Döngüsü Yönetimi'nden Workday Active Directory, Azure AD, SaaS uygulamaları ve sonrasındaki gidermenize yardımcı olacak zengin bir özellik kümesi sağlar. Hangi özellikleri kullanır ve çözümünü nasıl ayarlanacağını kuruluşunuzun ortam ve gereksinimlerinize bağlı olarak farklılık gösterir. İlk adım, stok varsa ve kuruluşunuzdaki dağıtılan kaç aşağıdakilerden birini gerçekleştirin:

* Kaç tane Active Directory ormanları kullanılıyor?
* Active Directory etki alanları kaç tane kullanılıyor?
* Kaç tane Active Directory kurumsal birimleri (OU) kullanılıyor?
* Kaç tane Azure Active Directory Kiracı kullanılıyor?
* Active Directory ve Azure Active Directory (örneğin, "hibrit" kullanıcılar) için sağlanması gereken kullanıcılar var mı?
* Azure Active Directory, ancak Active Directory değil (örneğin, "yalnızca bulut" kullanıcılar) için sağlanması gereken kullanıcılar var mı?
* Kullanıcı e-posta adresleri için Workday'e geri yazılması gerekiyor mu?

Bu soruların yanıtlarını aldıktan sonra aşağıdaki yönergeleri izleyerek dağıtım sağlama İş gününüzün planlayabilirsiniz.

#### <a name="planning-deployment-of-aad-connect-provisioning-agent"></a>AAD Connect sağlama Aracısı dağıtımını planlama

Workday AD kullanıcı sağlama çözüme bir veya daha fazla sağlama aracı Windows 2012 R2 çalıştıran sunuculara dağıtma gerekir veya en az 4 GB RAM ve .NET 4.7 + ile büyük çalışma zamanı. Sağlama Aracısı'nı yüklemeden önce aşağıdaki konuları göz önünde bulundurulması gerekir:

* Sağlama Aracısı'nı çalıştıran konak sunucuya hedef AD etki alanına ağ erişimi olduğundan emin olun
* Sağlama Aracısı Yapılandırma Sihirbazı'nı aracının Azure AD kiracınız ile kaydeder ve kayıt işlemini erişmesi *. bağlantı noktası 8082 msappproxy.net. Giden güvenlik duvarı kuralları bu iletişimin yerinde olduğundan emin olun.
* Şirket içi ile iletişim kurmak için bir hizmet hesabı sağlama Aracısı'nı kullanan AD etki alanı. Aracıyı yüklemeden önce kullanıcı özelliklerini okuma/yazma izinleri ve dolmayan parola ile bir hizmet hesabı oluşturmanız önerilir.  
* Sağlama Aracısı yapılandırması sırasında istekleri sağlama işlemesi gereken etki alanı denetleyicileri seçebilirsiniz. Birden çok coğrafi olarak dağıtılmış bir etki alanı denetleyiciniz varsa, aracı sağlama güvenilirlik ve performansını uçtan uca çözüm geliştirmek için tercih edilen etki alanı denetleyicileri aynı sitede yükleyin.
* Yüksek kullanılabilirlik için birden fazla sağlama Aracısı dağıtma ve aynı şirket içi kümesini işlemeye kaydetmek AD etki alanları.

> [!IMPORTANT]
> Üretim ortamlarında, Microsoft, en az 3 sağlama aracıları yüksek kullanılabilirlik için Azure AD kiracınız ile yapılandırılmış olmasını önerir.

#### <a name="selecting-provisioning-connector-apps-to-deploy"></a>Sağlama bağlayıcı uygulamaları dağıtmak için seçme

Workday ve Active Directory Tümleştirme, değerlendirilmesi için birden çok kaynak ve hedef sistemler mevcuttur:

| Kaynak sistem | Hedef sistem | Notlar |
| ---------- | ---------- | ---------- |
| Workday | Active Directory etki alanı | Her etki alanı farklı bir hedef sistem olarak kabul edilir |
| Workday | Azure AD kiracısı | Yalnızca bulutta yer alan kullanıcılar için gerekli |
| Active Directory Ormanı | Azure AD kiracısı | Bu akış AAD Connect tarafından hemen işlenir |
| Azure AD kiracısı | Workday | E-posta adresi geri yazma için |

Workday ile Active Directory arasında sağlama iş akışları kolaylaştırmak için Azure AD uygulama galerisinden ekleyebilirsiniz birden fazla sağlama bağlayıcı uygulama Azure AD sağlar:

![AAD uygulama Galerisi](./media/workday-inbound-tutorial/wd_gallery.png)

* **Active Directory sağlama için workday** -bu uygulamanın kullanıcı hesabı için tek bir Active Directory etki alanı Workday'den sağlamayı kolaylaştırır. Birden çok etki alanı varsa, bu uygulamanın bir örneği için sağlamak için gereken her Active Directory etki alanı için Azure AD uygulama galerisinde ekleyebilirsiniz.

* **Azure AD sağlama için workday** - Active Directory Kullanıcıları için Azure Active Directory, bu uygulama, tek bir Azure yalnızca bulut kullanıcıları ayıklayıp sorgulayabilecek sağlanmasını kolaylaştırmak için kullanılabilir eşitlemek için kullanılması gereken aracı olsa da AAD Connect Active Directory kiracısı.

* **Workday geri yazma** -bu uygulamanın geri yazma kullanıcının e-posta adresleri, Azure Active Directory'den Workday kolaylaştırır.

> [!TIP]
> Normal "İş günü" uygulama, Workday Azure Active Directory ile çoklu oturum açmayı ayarlamak için kullanılır. 

#### <a name="determine-workday-to-ad-user-attribute-mapping-and-transformations"></a>AD kullanıcı özniteliği eşleme ve dönüşümleri Workday belirleme

Bir Active Directory etki alanı için kullanıcı sağlamayı yapılandırmadan önce aşağıdaki soruları göz önünde bulundurun. Bu soruların yanıtlarını ayarlamak kapsam belirleme filtrelerini ve öznitelik eşlemelerini nasıl gerektiğini belirler.

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



Ayarlanmış ve bu özel bağlayıcı uygulamaları sağlama yapılandırma, bu öğreticinin geri kalan bölümler konudur. Ortamınızda kiracılar hangi sistemlerin, sağlamak ve kaç Active Directory etki alanları ve Azure AD için ihtiyacınız olan hangi uygulamaların yapılandırmayı seçtiğinize bağlıdır.



## <a name="configure-integration-system-user-in-workday"></a>Workday'de tümleştirme sistemi kullanıcısı yapılandırma

Tüm Workday sağlama bağlayıcılardan oluşan genel bir gereksinim, Workday İnsan Kaynakları API'sine bağlanmak Workday sistem tümleştirme hesabı için kimlik bilgilerini gerektir ' dir. Bu bölümde, Workday'de bir tümleştirme sistemi kullanıcısı Oluştur açıklar.

> [!NOTE]
> Bu yordamı atlayabilir ve bunun yerine sistem tümleştirme hesabı olarak Workday genel yönetici hesabını kullanmak mümkündür. Tanıtımlar düzgün çalışıyor, ancak üretim dağıtımları için önerilmez.

### <a name="create-an-integration-system-user"></a>Bir tümleştirme sistemi kullanıcısı oluştur

**Bir tümleştirme sistemi kullanıcısı oluşturmak için:**

1. Bir yönetici hesabını kullanarak Workday kiracınızda oturum oturum açın. İçinde **Workday uygulama**, girin arama kutusuna bir kullanıcı oluşturup ardından **tümleştirme sistemi kullanıcısı Oluştur**.

    ![Kullanıcı oluşturma](./media/workday-inbound-tutorial/wd_isu_01.png "kullanıcı oluşturma")
2. Tamamlamak **tümleştirme sistemi kullanıcısı Oluştur** görev tarafından yeni bir tümleştirme sistemi kullanıcısı için bir kullanıcı adı ve parola belirtin.  
 * Bırakın **gerektiren yeni parolayı sonraki oturum açma** seçeneğinin işaretli olmadığından bu kullanıcının programlı olarak oturum açan.
 * Bırakın **oturum zaman aşımı dakikaları** 0 varsayılan değeri ile hangi engeller kullanıcının oturumları tamamlanmadan zaman aşımına uğramasını.
 * Seçeneğini **UI oturumları izin** olarak eklenen bir tümleştirme sisteminin parolası olan bir kullanıcı iş günü içinde oturum açmasını engelleyen güvenlik katmanı sağlar. 

    ![Tümleştirme sistemi kullanıcısı Oluştur](./media/workday-inbound-tutorial/wd_isu_02.png "tümleştirme sistemi kullanıcısı oluştur")

### <a name="create-a-security-group"></a>Bir güvenlik grubu oluşturun
Bu adımda, Workday'de bir sınırlandırılmamış tümleştirme sistemi güvenlik grubu oluşturun ve bu grup için bir önceki adımda oluşturduğunuz tümleştirme sistemi kullanıcısı atayın.

**Bir güvenlik grubu oluşturmak için:**

1. Girin arama kutusuna güvenlik grubu oluşturun ve ardından **güvenlik grubu oluşturma**.

    ![Güvenlik grubu](./media/workday-inbound-tutorial/wd_isu_03.png "güvenlik grubu oluştur")
2. Tamamlamak **güvenlik grubu oluşturma** görev.  
   * Seçin **tümleştirme sistemi güvenlik grubunu (sınırlandırılmamış)** gelen **kiralanan güvenlik grubu türü** açılır.

    ![Güvenlik grubu](./media/workday-inbound-tutorial/wd_isu_04.png "güvenlik grubu oluştur")

3. Güvenlik grubu oluşturma işlemi başarılı olduktan sonra burada üyeleri güvenlik grubuna atayabilirsiniz bir sayfa görürsünüz. Yeni tümleştirme sistemi kullanıcısı bu güvenlik grubuna ekleyin ve uygun kuruluş kapsamı seçin.
![Güvenlik grubunu Düzenle](./media/workday-inbound-tutorial/wd_isu_05.png "güvenlik grubunu Düzenle")
 
### <a name="configure-domain-security-policy-permissions"></a>Etki alanı güvenlik ilkesi izinleri yapılandırma
Bu adımda, "etki alanı güvenliği" çalışan verileri güvenlik grubuna ilke izin vermesi.

**Etki alanı güvenlik ilkesi izinleri yapılandırmak için:**

1. Girin **etki alanı güvenlik yapılandırması** arama kutusuna ve ardından bağlantıyı **etki alanı güvenlik yapılandırma raporu**.  

    ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/wd_isu_06.png "etki alanı güvenlik ilkeleri")  
2. İçinde **etki alanı** metin kutusuna aşağıdaki etki alanları için arama yapın ve bunları tek tek filtre ekleyin.  
   * *Dış hesap sağlama*
   * *Çalışan verileri: Ortak çalışanı raporları*
   * *Kişi verilerini: İş iletişim bilgileri*
   * *Çalışan verileri: Tüm konumlar*
   * *Çalışan verileri: Geçerli personel bilgileri*
   * *Çalışan verileri: Çalışan profilindeki iş başlığı*
 
    ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/wd_isu_07.png "etki alanı güvenlik ilkeleri")  

    ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/wd_isu_08.png "etki alanı güvenlik ilkeleri") 

    **Tamam** düğmesine tıklayın.

3. Gösterilir rapora yanında üç nokta (...) seçin **Harici hesap sağlama** ve menü seçeneğine tıklayın **etki alanı güvenlik ilkesi izinleri Düzenle ->**

    ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/wd_isu_09.png "etki alanı güvenlik ilkeleri")  

4. Üzerinde **etki alanı güvenlik ilkesi izinleri Düzenle** sayfasında bölümüne kaydırın **tümleştirme izinleri**. Güvenlik grupları listesine tümleştirme sistem grubu eklemek için "+" işaretine tıklayın **alma** ve **Put** tümleştirme izinleri.

    ![İzni Düzenle](./media/workday-inbound-tutorial/wd_isu_10.png "izni Düzenle")  

5. Güvenlik grupları listesine tümleştirme sistem grubu eklemek için "+" işaretine tıklayın **alma** ve **Put** tümleştirme izinleri.

    ![İzni Düzenle](./media/workday-inbound-tutorial/wd_isu_11.png "izni Düzenle")  

6. Her bu kalan güvenlik ilkelerinin için yukarıdaki 3-5 arasındaki adımları yineleyin:

   | İşlem | Etki alanı güvenlik ilkesi |
   | ---------- | ---------- | 
   | GET ve Put | Çalışan verileri: Ortak çalışanı raporları |
   | GET ve Put | Kişi verilerini: İş iletişim bilgileri |
   | Al | Çalışan verileri: Tüm konumlar |
   | Al | Çalışan verileri: Geçerli personel bilgileri |
   | Al | Çalışan verileri: Çalışan profilindeki iş başlığı |

### <a name="configure-business-process-security-policy-permissions"></a>İş işlemi Güvenlik İlkesi izinleri yapılandırma
Bu adımda, "iş işlem güvenliği" çalışan verileri güvenlik grubuna ilke izin vermesi. Bu, Workday geri yazma uygulama bağlayıcısını ayarlarken ayarlanması için gereklidir. 

**İş işlemi Güvenlik İlkesi izinleri yapılandırmak için:**

1. Girin **işlem İlkesi** arama kutusuna ve ardından bağlantıyı **iş işlem güvenlik ilkesini Düzenle** görev.  

    ![İş işlem güvenlik ilkelerini](./media/workday-inbound-tutorial/wd_isu_12.png "iş işlem güvenlik ilkeleri")  

2. İçinde **iş işlem türü** metin arama *kişi* seçip **kişi değişiklik** iş süreçleri ve tıklatın **Tamam**.

    ![İş işlem güvenlik ilkelerini](./media/workday-inbound-tutorial/wd_isu_13.png "iş işlem güvenlik ilkeleri")  

3. Üzerinde **iş işlem güvenlik ilkesini Düzenle** sayfasında, kaydırma **korumak iletişim bilgileri (Web hizmeti)** bölümü.

    ![İş işlem güvenlik ilkelerini](./media/workday-inbound-tutorial/wd_isu_14.png "iş işlem güvenlik ilkeleri")  

4. Seçin ve web hizmetleri isteği başlatabilirsiniz güvenlik grupları listesine yeni bir tümleştirme sistemi güvenlik grubunu ekleyin. Tıklayarak **Bitti**. 

    ![İş işlem güvenlik ilkelerini](./media/workday-inbound-tutorial/wd_isu_15.png "iş işlem güvenlik ilkeleri")  

 
### <a name="activate-security-policy-changes"></a>Güvenlik İlkesi değişikliklerini etkinleştirin

**Güvenlik İlkesi değişiklikleri etkinleştirmek için:**

1. Girin arama kutusuna etkinleştirin ve ardından bağlantıyı **etkinleştirme bekleyen güvenlik ilkesi değişikliklerini**.

    ![Etkinleştirme](./media/workday-inbound-tutorial/wd_isu_16.png "etkinleştir") 
2. Bekleyen Güvenlik İlkesi değişikliklerini etkinleştir görev denetim amacıyla bir açıklama girerek başlayın ve ardından **Tamam**. 

    ![Bekleyen güvenlik ayarlarını etkinleştir](./media/workday-inbound-tutorial/wd_isu_17.png "bekleyen güvenlik ayarlarını etkinleştir")  
1. Onay kutusunu işaretleyerek sonraki ekranda bir görevi tamamlamak **Onayla**ve ardından **Tamam**.

    ![Bekleyen güvenlik ayarlarını etkinleştir](./media/workday-inbound-tutorial/wd_isu_18.png "bekleyen güvenlik ayarlarını etkinleştir")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Active Directory'ye Workday'den kullanıcı sağlamayı yapılandırma

Her Active Directory etki alanına tümleştirmenizi kapsamında Workday'den sağlama kullanıcı hesabı yapılandırmak için bu yönergeleri izleyin.

### <a name="part-1-install-and-configure-on-premises-provisioning-agents"></a>1. Kısım: Yükleme ve şirket içi sağlama Aracısı yapılandırın

Şirket için Active Directory sağlamak için bir aracı .NET 4.7 + sahip bir sunucuda yüklü Framework ve ağ erişimi istediğiniz Active Directory etki alanları.

> [!TIP]
> Sunucunuzda sağlanan yönergeleri kullanarak .NET framework sürümünü denetleyebilir [burada](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed).
> Sunucu .NET 4.7 yok ya da sonraki bir sürümün yüklü, buradan indirebilirsiniz [burada](https://support.microsoft.com/help/3186497/the-net-framework-4-7-offline-installer-for-windows).  

.NET 4.7 + dağıttıktan sonra indirebileceğiniz **[şirket burada sağlama aracı](https://go.microsoft.com/fwlink/?linkid=847801)** ve aracı yapılandırmasını tamamlamak için aşağıda verilen adımları izleyin.

1. Yeni aracıyı yüklemek istediğiniz Windows Server oturum açın.
2. Sağlama Aracısı Yükleyicisi'ni başlatın ve koşulları kabul tıklayın **yükleme** düğmesi.
![Ekran yükleme](./media/workday-inbound-tutorial/pa_install_screen_1.png "yükleme ekranı")

3. Yükleme tamamlandığında, Sihirbazı başlatılır ve göreceğiniz sonra **Azure AD Connect** ekran. Tıklayarak **doğrulaması** Azure AD Örneğinize bağlanmak için düğme.
![Azure AD connect](./media/workday-inbound-tutorial/pa_install_screen_2.png "Azure AD'ye bağlanma")

4. Azure AD Örneğinize genel yönetici kimlik bilgilerini kullanarak kimlik doğrulaması. 
![Yönetici kimlik doğrulama](./media/workday-inbound-tutorial/pa_install_screen_3.png "yönetici kimlik doğrulama")

5. Azure AD ile başarılı kimlik doğrulamadan sonra göreceğiniz **Active Directory'ye bağlanın** ekran. Bu adımda, AD etki alanı adınızı girin ve tıklayın **Dizin Ekle** düğmesi.
![Dizin Ekle](./media/workday-inbound-tutorial/pa_install_screen_4.png "Dizin Ekle")

6. Şimdi AD etki alanına bağlanmak için gereken kimlik bilgilerini girmeniz istenir. Aynı ekranda kullanabilirsiniz **seçin etki alanı denetleyicisi öncelik** aracı sağlama isteği göndermek için kullanacağı bir etki alanı denetleyicileri belirtmek için.
![Etki alanı kimlik bilgileri](./media/workday-inbound-tutorial/pa_install_screen_5.png "etki alanı kimlik bilgileri")

7. Etki alanı yapılandırdıktan sonra yükleyici yapılandırılmış etki alanlarının bir listesini görüntüler. Bu ekranda yineleyin #5 ve daha fazlasını eklemek için #6. adım etki alanları veya tıklayarak **sonraki** aracı kaydı devam etmek için. 
![Etki alanı yapılandırılmış](./media/workday-inbound-tutorial/pa_install_screen_6.png "yapılandırılmış etki alanları")

   > [!NOTE]
   > Birden çok AD etki alanına (örneğin, na.contoso.com, emea.contoso.com) ve ardından her bir etki alanı ayrı ayrı listeye eklemek Lütfen durumunda. Yalnızca üst etki alanını (örneğin, contoso.com) eklemek yeterli değildir ve aracıyla birlikte her alt etki alanı kayıt önerilir. 

8. Yapılandırma ayrıntılarını gözden geçirin ve tıklayın **Onayla** aracıyı kaydetmek için. 
![Ekran onaylayın](./media/workday-inbound-tutorial/pa_install_screen_7.png "ekran onaylayın")

9. Yapılandırma Sihirbazı'nı aracı kaydını ilerlemesini görüntüler.
![Aracı kaydı](./media/workday-inbound-tutorial/pa_install_screen_8.png "aracı kaydı")

10. Aracı kaydı başarılı olduktan sonra tıklayabilirsiniz **çıkmak** sihirbazdan çıkmak için. 
![Çıkış ekranı](./media/workday-inbound-tutorial/pa_install_screen_9.png "çıkış ekranı")

11. Aracı yüklemesini doğrulayın, "Hizmetler" ek bileşenini açarak çalıştığından emin olun ve "Microsoft Azure AD Connect'i Hazırlama aracı" adlı hizmetin bakılması ![Hizmetleri](./media/workday-inbound-tutorial/services.png)  


**Sorun giderme aracı**

[Windows olay günlüğü](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) Windows Server'da aracıyı barındıran makine aracı tarafından gerçekleştirilen tüm işlemler için olayları içerir. Bu olayları görüntülemek için:
    
1. Açık **Eventvwr.msc**.
2. Seçin **Windows Günlükleri > Uygulama**.
3. Kaynak oturumu tüm olayları görüntüle **AAD. Connect.ProvisioningAgent**. 
4. Hataları ve Uyarıları denetleyin.

    
### <a name="part-2-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>2. Bölüm: sağlama bağlayıcı uygulama ekleme ve Workday bağlantısı oluşturma

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

   * **Active Directory ormanı -** aracıyla kayıtlı, Active Directory etki alanı, "Name". Genellikle gibi dize budur: *contoso.com*

   * **Active Directory kapsayıcısı -** DN kapsayıcı Aracısı varsayılan olarak kullanıcı hesaplarını burada oluşturmalısınız girin. 
        Örnek: *OU standart kullanıcılar, OU = Kullanıcılar, DC = contoso, DC = test =*
> [!NOTE]
> Bu ayar yalnızca yürütme için kullanıcı hesabı oluşturma, salesforce'taki *parentDistinguishedName* öznitelik öznitelik eşlemelerini yapılandırılmadı. Bu ayar kullanılmaz kullanıcı arama veya güncelleştirme işlemleri. Tüm etki alanı alt ağacı, arama işlemi kapsamında döner.
   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.
> [!NOTE]
> İçine sağlama işi aşması durumunda Azure AD sağlama hizmeti, e-posta bildirimi gönderir. bir [karantina](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#quarantine) durumu.

   * Tıklayın **Test Bağlantısı** düğmesi. Bağlantı testi başarılı olursa tıklayın **Kaydet** üstünde düğme. Başarısız olursa, Workday kimlik bilgilerini ve aracı kurulumu üzerinde yapılandırılmış AD kimlik bilgileri geçerli olduğunu denetleyin.

![Azure portal](./media/workday-inbound-tutorial/wd_1.png)

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
| **Değiştir(Orta(değiştirin(\[UserID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**      |    SAMAccountName            |     |         Yazılan yalnızca oluşturma sırasında |
| **Anahtar (\[etkin\],, "0" "True,"1","False"")** |  accountDisabled      |     | Oluşturun ve güncelleştirme |
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

**Workday geri yazma bağlayıcı yapılandırmak için:**

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

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio1.png)

6. Ayarlama **konumu** alanı `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, ancak "IMPL-CC", gerçek değiştirerek örnek türü ve "KİRACI" gerçek Kiracı adınızla.

7. Ayarlama **işlemi** için **Get_Workers**

8.  Küçük tıklatın **yapılandırma** istek/yanıt bölmeleri Workday kimlik bilgileriniz ayarlamak için aşağıdaki bağlantıya. Denetleme **kimlik doğrulaması**ve ardından, Workday tümleştirmesi sistem hesabı için kullanıcı adını ve parolasını girin. Kullanıcı adı olarak biçimlendirdiğinizden emin olun name@tenant, bırakıp **WS-güvenlik UsernameToken** seçeneği belirlenmiş.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio2.png)

9. **Tamam**’ı seçin.

10. **İstek** bölmesinde, aşağıdaki ve ayarlanmış XML yapıştırma seçeneğiyle **Employee_ID** gerçek bir kullanıcının Workday'deki kiracınızdaki çalışan kimliği. Özniteliğine sahip bir kullanıcı seçin ayıklamak istediğiniz doldurulur.

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="https://www.w3.org/2001/XMLSchema">
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

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio3.png)

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

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio_aad1.png)

6. Öznitelik listesi, giriş alanlarının olduğu için alt kısmına kaydırın.

7. İçin **adı**, öznitelik için bir görünen ad girin.

8. İçin **türü**, uygun şekilde, özniteliğe karşılık gelen türünü seçin (**dize** yaygın olarak kullanılır).

9. İçin **API ifadesi**, Workday Studio'dan kopyaladığınız XPath ifadesi girin. Örnek: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Seçin **öznitelik Ekle**.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio_aad2.png)

11. Seçin **Kaydet** yukarıdaki ardından **Evet** iletişim kutusu. Öznitelik eşlemesi ekranı hala açıksa, kapatın.

12. Ana duyulduğundan **sağlama** sekmesinde **işlem çalışanlarına şirket içi eşitleme** (veya **eşitleme çalışanları Azure AD'ye**) yeniden.

13. Seçin **yeni eklemesi**.

14. Yeni bir öznitelik şimdi gözükeceğini **kaynak özniteliği** listesi.

15. İstediğiniz gibi yeni bir öznitelik için bir eşleme ekleyin.

16. İşiniz bittiğinde ayarlamayı unutmayın **sağlama durumu** geri **üzerinde** ve kaydedin.

## <a name="known-issues"></a>Bilinen sorunlar

* Şirket içi Active Directory'de thumbnailPhoto kullanıcı özniteliği için verileri yazma şu anda desteklenmiyor.

* "Azure AD iş günü" bağlayıcı burada AAD Connect etkin Azure AD kiracılarıyla üzerinde şu anda desteklenmiyor.  

## <a name="managing-personal-data"></a>Kişisel verileri yönetme

Çözüm için Active Directory sağlama iş günü bir eşitleme aracısı etki alanına katılmış bir sunucuda yüklü olmasını gerektirir ve bu aracı Windows olay günlüğünde, kişisel olarak tanımlanabilir bilgiler içerebilir günlükleri oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
* [Workday Azure Active Directory ile çoklu oturum açmayı yapılandırma hakkında bilgi edinin](workday-tutorial.md)
* [Diğer SaaS uygulamalarına Azure Active Directory ile tümleştirme hakkında bilgi edinin](tutorial-list.md)

