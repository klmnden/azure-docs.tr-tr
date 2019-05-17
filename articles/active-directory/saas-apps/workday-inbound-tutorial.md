---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Workday yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Workday kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: daveba
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: 31cf1f6da515aa9b453987383e78f466c5ba4fb9
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827285"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning"></a>Öğretici: Workday için otomatik kullanıcı sağlamayı yapılandırma

Bu öğreticinin amacı, çalışan profilleri isteğe bağlı geri yazma özelliğiyle Workday kullanıcı adına e-posta adresi ve Active Directory ve Azure Active Directory içinde Workday'den almak için gerçekleştirmeniz gereken adımlar göstermektir.

## <a name="overview"></a>Genel Bakış

[Azure Active Directory kullanıcı sağlama hizmeti](../manage-apps/user-provisioning.md) ile tümleşir [Workday, İnsan Kaynakları API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) kullanıcı hesapları sağlamak için. Azure AD sağlama iş akışları şu kullanıcının etkinleştirmek için bu bağlantı kullanır:

* **Active Directory kullanıcılara sağlama** -sağlama seçilen bir veya daha fazla Active Directory etki alanına Workday'den kullanıcıları ayarlar.

* **Yalnızca bulutta yer alan kullanıcılar Azure Active Directory'ye sağlama** - senaryolarında olduğu Active Directory kullanılmaz, şirket içinde kullanıcıların sağlanabilir Workday'den doğrudan Azure sağlama hizmetini Azure AD kullanıcısını kullanarak Active Directory için.

* **E-posta adresi ve kullanıcı adı için Workday geri yazma** -sağlama hizmetini Azure AD kullanıcısının kullanıcı adı ve e-posta adreslerini Azure AD'den geri için Workday yazabilirsiniz.

### <a name="what-human-resources-scenarios-does-it-cover"></a>Hangi İnsan Kaynakları senaryoları kapsıyor mu?

Azure AD kullanıcı sağlama hizmeti tarafından desteklenen Workday kullanıcı sağlama iş akışları şu İnsan Kaynakları ve kimlik yaşam döngüsü yönetim senaryolarının otomasyonunu sağlar:

* **Yeni çalışanların işe** - yeni bir çalışan için Workday, eklendiğinde, bir kullanıcı hesabı Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak otomatik olarak oluşturulur ve [AzureADtarafındandesteklenendiğerSaaSuygulamaları](../manage-apps/user-provisioning.md), e-posta adresinin Workday geri yazma özelliğiyle.

* **Çalışan özniteliği ve profili güncelleştirmeleri** - bir çalışan kaydı (kendi ad, başlık veya Yöneticisi gibi) iş günü içinde güncelleştirilir, kendi kullanıcı hesabına otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak güncelleştirilmesi ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarına](../manage-apps/user-provisioning.md).

* **Çalışan sonlandırmalar** -, Workday'de bir çalışan sonlandırıldığında, kullanıcı hesabı otomatik olarak Active Directory, Azure Active Directory ve Office 365 isteğe bağlı olarak devre dışıdır ve [Azure tarafından desteklenen diğer SaaS uygulamaları AD](../manage-apps/user-provisioning.md).

* **Çalışan rehires** - bir çalışan, iş günü içinde rehired eski, hesap otomatik olarak yeniden veya Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve (tercihinizebağlıolarak)yenidensağlanan[Azure AD tarafından desteklenen diğer SaaS uygulamalarına](../manage-apps/user-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>İçin en iyi bu kullanıcı sağlama çözümünü kim uygun?

İdeal olarak, çözüm sağlama bu Workday kullanıcı için uygun olan:

* Workday'den kullanıcı hazırlama için önceden oluşturulmuş, bulut tabanlı bir çözüm bağlamasına kuruluşlar

* Active Directory veya Azure Active Directory Workday'den doğrudan kullanıcı sağlamayı gerektiren kuruluşlar

* Kullanıcıların verileri kullanarak sağlanacak gerektiren kuruluşlar Workday HCM modülünden alınan (bkz [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Kuruluşların gerektiren birleştirme, taşıma ve kullanıcıların bir veya daha fazla Active Directory ormanları eşitlenmesi bırakarak, etki alanları ve OU'lar yalnızca temel Workday HCM modülünde algılandı bilgilerini değiştirebilirsiniz (bkz [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* E-posta için Office 365 kullanan kuruluşlar

## <a name="solution-architecture"></a>Çözüm Mimarisi

Bu bölümde, ortak karma ortamlar için çözüm mimarisi sağlayan uçtan uca kullanıcı açıklanmaktadır. İki ilişkili akışlar vardır:

* **Yetkili ik veri akışından – şirket içi Active Directory'ye Workday:** Bu akışta alt olaylar (örneğin, New Hires aktarımları Sonlandırmalar) ilk Workday ik Kiracı bulutta oluşur ve ardından olay veri akışları Azure AD üzerinden şirket içi Active Directory ve aracı sağlama. Olay bağlı olarak, AD'de oluşturma/güncelleştirme/etkinleştirme/devre dışı işlemlerine neden olabilir.
* **E-posta ve kullanıcı geri yazma – şirket içi Active Directory'den Workday akış:** Active Directory'deki hesap oluşturma tamamlandıktan sonra Azure AD Connect ile Azure AD ile eşitlenir ve e-posta ve kullanıcı adı özniteliği için Workday yeniden yazılabilir.

![Genel Bakış](./media/workday-inbound-tutorial/wd_overview.png)

### <a name="end-to-end-user-data-flow"></a>Uçtan uca kullanıcı veri akışı

1. İK takım Workday HCM içinde çalışan işlemler (birleştiriciler/satış/Leavers veya yeni işe alımlar/aktarımları/Sonlandırmalar) gerçekleştirir.
2. Azure AD sağlama hizmeti, Workday'deki ik kimliklerinin zamanlanmış eşitleme çalışır ve şirket içi Active Directory ile eşitleme için işlenmesi gereken değişiklikleri tanımlar.
3. Azure AD sağlama hizmeti AD hesabı oluşturma/güncelleştirme/etkinleştirme/devre dışı bırakma işlemleri içeren bir istek yükü ile şirket içi AAD Connect sağlama Aracısı çağırır.
4. Azure AD Connect aracı sağlama AD hesabı veri ekleme/güncelleştirme için bir hizmet hesabı kullanır.
5. Azure AD Connect / AD eşitleme Altyapısı güncelleştirmeleri AD'de çekmek için delta eşitleme çalıştırır.
6. Active Directory güncelleştirmeleri, Azure Active Directory ile eşitlenir.
7. Workday geri yazma Bağlayıcısı yapılandırdıysanız, bu geri e-posta özniteliği ve kullanıcı adı Workday için kullanılan eşleşen özniteliğine dayanarak yazar.

## <a name="planning-your-deployment"></a>Dağıtımınızı planlama

Workday tümleştirmenizi başlamadan önce aşağıdaki önkoşulları denetleyin ve geçerli Active Directory mimarisi ve kullanıcı gereksinimleri ile Azure Active Directory tarafından sağlanan çözümler sağlama konusunda aşağıdaki yönergeleri okuyun. Kapsamlı bir [dağıtım planı](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans) planlama çalışma sayfaları ile ik proje katılımcıları ve Workday tümleştirmesi iş ortağı ile işbirliği yapmaya yardımcı olmak de kullanılabilir.

Bu bölüm, planlama şu yönlerini kapsar:

* [Önkoşullar](#prerequisites)
* [Sağlama bağlayıcı uygulamaları dağıtmak için seçme](#selecting-provisioning-connector-apps-to-deploy)
* [Azure AD Connect sağlama Aracısı dağıtımını planlama](#planning-deployment-of-azure-ad-connect-provisioning-agent)
* [Birden çok Active Directory etki alanı ile tümleştirme](#integrating-with-multiple-active-directory-domains)
* [Active Directory kullanıcı özniteliği eşleme ve dönüşümleri Workday planlama](#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)

### <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Geçerli Azure AD Premium P1 veya genel yönetici erişimi ile yüksek abonelik
* Sınama ve tümleştirme amacıyla Workday uygulama Kiracı
* Sınama amacıyla çalışan verilerini test etmek için yönetici izinleri workday'deki sistem tümleştirme kullanıcısı oluşturun ve değişiklikler yapmak için
* Active Directory kullanıcı sağlama için .NET 4.7.1+ çalışma zamanı ile Windows Server 2012 veya üzeri çalıştıran bir sunucu ana bilgisayar için gerekli [şirket sağlama aracı](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) Active Directory ve Azure AD arasında kullanıcıları eşitleme

### <a name="selecting-provisioning-connector-apps-to-deploy"></a>Sağlama bağlayıcı uygulamaları dağıtmak için seçme

Workday ile Active Directory arasında sağlama iş akışları kolaylaştırmak için Azure AD uygulama galerisinden ekleyebilirsiniz birden fazla sağlama bağlayıcı uygulama Azure AD sağlar:

![AAD uygulama Galerisi](./media/workday-inbound-tutorial/wd_gallery.png)

* **Active Directory kullanıcı sağlama için workday** -bu uygulamanın kullanıcı hesabı için tek bir Active Directory etki alanı Workday'den sağlamayı kolaylaştırır. Birden çok etki alanı varsa, bu uygulamanın bir örneği için sağlamak için gereken her Active Directory etki alanı için Azure AD uygulama galerisinde ekleyebilirsiniz.

* **Azure AD kullanıcı sağlama için workday** - Active Directory Kullanıcıları için Azure Active Directory, bu uygulama, tek bir Azure yalnızca bulut kullanıcıları ayıklayıp sorgulayabilecek sağlanmasını kolaylaştırmak için kullanılabilir eşitlemek için kullanılması gereken aracı olsa da AAD Connect Active Directory kiracısı.

* **Workday geri yazma** -bu uygulamanın geri yazma kullanıcının e-posta adresleri, Azure Active Directory'den Workday kolaylaştırır.

> [!TIP]
> Normal "İş günü" uygulama, Workday Azure Active Directory ile çoklu oturum açmayı ayarlamak için kullanılır.

Senaryonuz için uygun hangi Workday sağlama uygulamaları tanımlamak için aşağıdaki karar akış grafiği kullanın.
    ![Karar akış çizelgesi](./media/workday-inbound-tutorial/wday_app_flowchart.png "karar akış çizelgesi")

Bu öğreticinin ilgili bölüme gitmek için içindekiler kullanın.

### <a name="planning-deployment-of-azure-ad-connect-provisioning-agent"></a>Azure AD Connect sağlama Aracısı dağıtımını planlama

> [!NOTE]
> Bu bölüm, yalnızca Active Directory kullanıcı sağlama uygulaması için Workday dağıtmayı planlıyorsanız, ilgili değildir. Workday geri yazma veya Workday: Azure AD kullanıcı sağlama uygulama dağıtıyorsanız, atlayabilirsiniz.

Workday AD kullanıcı sağlama çözüme bir veya daha fazla sağlama aracı Windows 2012 R2 çalıştıran sunuculara dağıtma gerekir veya en az 4 GB RAM ve .NET 4.7.1+ çalışma zamanı ile daha büyük. Sağlama Aracısı'nı yüklemeden önce aşağıdaki konuları göz önünde bulundurulması gerekir:

* Sağlama Aracısı'nı çalıştıran konak sunucuya hedef AD etki alanına ağ erişimi olduğundan emin olun
* Sağlama Aracısı Yapılandırma Sihirbazı'nı aracının Azure AD kiracınız ile kaydeder ve kayıt işlemini erişmesi *. SSL bağlantı noktası 443 üzerinden msappproxy.net. Giden güvenlik duvarı kuralları bu iletişimin yerinde olduğundan emin olun. Aracısının desteklediği [giden HTTPS Ara sunucu yapılandırmasını](#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication).
* Şirket içi ile iletişim kurmak için bir hizmet hesabı sağlama Aracısı'nı kullanan AD etki alanı. Aracıyı yüklemeden önce dolmayan parola ve etki alanı yönetici izinleri ile bir hizmet hesabı oluşturmanız önerilir.  
* Sağlama Aracısı yapılandırması sırasında istekleri sağlama işlemesi gereken etki alanı denetleyicileri seçebilirsiniz. Birden çok coğrafi olarak dağıtılmış bir etki alanı denetleyiciniz varsa, aracı sağlama güvenilirlik ve performansını uçtan uca çözüm geliştirmek için tercih edilen etki alanı denetleyicileri aynı sitede yükleyin.
* Yüksek kullanılabilirlik için birden fazla sağlama Aracısı dağıtma ve aynı şirket içi kümesini işlemeye kaydetmek AD etki alanları.

> [!IMPORTANT]
> Üretim ortamlarında, Microsoft, en az 3 sağlama aracıları yüksek kullanılabilirlik için Azure AD kiracınız ile yapılandırılmış olmasını önerir.

### <a name="integrating-with-multiple-active-directory-domains"></a>Birden çok Active Directory etki alanı ile tümleştirme

> [!NOTE]
> Bu bölüm, yalnızca Active Directory kullanıcı sağlama uygulaması için Workday dağıtmayı planlıyorsanız, ilgili değildir. Workday geri yazma veya Workday: Azure AD kullanıcı sağlama uygulama dağıtıyorsanız, atlayabilirsiniz.

Active Directory topolojiniz bağlı olarak, kullanıcı sağlama bağlayıcı uygulama sayısı ve sağlama yapılandırmak için aracı sayısını karar vermeniz gerekir. Aşağıda listelenen dağıtımınızı planlarken başvurabilirsiniz yaygın dağıtım modellerinin bazılarıdır.

#### <a name="deployment-scenario-1--single-workday-tenant---single-ad-domain"></a>Dağıtım senaryosu #1: Workday Kiracısı tek tek bir AD etki alanı ->

Bu senaryoda, bir iş günü kiracısına sahip ve tek bir hedef AD etki alanı kullanıcılarına sağlamak istiyorum. Bu dağıtım için önerilen üretim yapılandırması aşağıda verilmiştir.

|   |   |
| - | - |
| Hayır. aracıları dağıtmak için sağlama işlemini şirket içi | (yüksek kullanılabilirlik ve yük devretme için) 3 |
| Hayır. Azure portalında yapılandırmak için AD kullanıcı uygulamalara sağlama iş günü | 1 |

  ![Senaryo 1](./media/workday-inbound-tutorial/dep_scenario1.png)

#### <a name="deployment-scenario-2--single-workday-tenant---multiple-child-ad-domains"></a>Dağıtım senaryosu #2: Tek Workday Kiracısı için birden çok alt AD etki alanı ->

Bu senaryo, bir ormanda birden çok hedef AD alt etki alanlarına workday'deki kullanıcıları ayıklayıp sorgulayabilecek sağlama içerir. Bu dağıtım için önerilen üretim yapılandırması aşağıda verilmiştir.

|   |   |
| - | - |
| Hayır. aracıları dağıtmak için sağlama işlemini şirket içi | (yüksek kullanılabilirlik ve yük devretme için) 3 |
| Hayır. Azure portalında yapılandırmak için AD kullanıcı uygulamalara sağlama iş günü | alt etki alanı başına tek bir uygulama |

  ![Senaryo 2](./media/workday-inbound-tutorial/dep_scenario2.png)

#### <a name="deployment-scenario-3--single-workday-tenant---disjoint-ad-forests"></a>Dağıtım senaryosu #3: Tek Workday Kiracısı ayrık AD ormanına ->

Bu senaryoda ayrık AD Ormanlardaki etki alanlarına workday'deki kullanıcıları ayıklayıp sorgulayabilecek sağlamayı içerir. Bu dağıtım için önerilen üretim yapılandırması aşağıda verilmiştir.

|   |   |
| - | - |
| Hayır. aracıları dağıtmak için sağlama işlemini şirket içi | ayrık AD ormanı başına 3 |
| Hayır. Azure portalında yapılandırmak için AD kullanıcı uygulamalara sağlama iş günü | alt etki alanı başına tek bir uygulama |

  ![Senaryo 3](./media/workday-inbound-tutorial/dep_scenario3.png)

### <a name="planning-workday-to-active-directory-user-attribute-mapping-and-transformations"></a>Active Directory kullanıcı özniteliği eşleme ve dönüşümleri Workday planlama

> [!NOTE]
> Bu bölüm, yalnızca Active Directory kullanıcı sağlama uygulaması için Workday dağıtmayı planlıyorsanız, ilgili değildir. Workday geri yazma veya Workday: Azure AD kullanıcı sağlama uygulama dağıtıyorsanız, atlayabilirsiniz.

Bir Active Directory etki alanı için kullanıcı sağlamayı yapılandırmadan önce aşağıdaki soruları göz önünde bulundurun. Bu soruların yanıtlarını ayarlamak kapsam belirleme filtrelerini ve öznitelik eşlemelerini nasıl gerektiğini belirler.

* **Workday'deki hangi kullanıcıların bu Active Directory ormanına sağlanması gerekir?**

  * *Örnek: Burada "Contoso" değeri "Şirket" Workday öznitelik içeriyor ve "Worker_Type" özniteliği "Normal" içeren kullanıcılar*

* **Kullanıcılar farklı kuruluş birimlerine (OU) nasıl yönlendirileceğini?**

  * *Örnek: Kullanıcıların bir office konuma karşılık gelen OU'ları Workday "Belediye" ve "Country_Region_Reference" öznitelikler tanımlanan yönlendirilir*

* **Aşağıdaki öznitelikler Active Directory'de nasıl doldurulur?**

  * Ortak ad (cn)
    * *Örnek: İnsan kaynakları tarafından belirlenen Workday USER_ID değeri kullanın.*

  * Çalışan kimliği (EmployeeID)
    * *Örnek: Workday Worker_ID değerini kullanın*

  * SAM hesabı adı (sAMAccountName)
    * *Örnek: Geçersiz karakterler kaldırmak için ifade bir Azure AD sağlama filtrelenmiş Workday USER_ID değeri kullanın*

  * Kullanıcı asıl adı (userPrincipalName)
    * *Örnek: Workday USER_ID değeri, bir etki alanı adı ekleme için ifade sağlama bir Azure AD ile kullanma*

* **Nasıl kullanıcılar Workday ile Active Directory arasında eşleşmesi?**

  * *Örnek: "EmployeeID" aynı değere sahip olduğu Active Directory Kullanıcıları ile belirli bir iş günü "değeri eşleştirilir Worker_ID" sahip kullanıcılar. Active Directory'de Worker_ID değeri bulunamazsa, ardından yeni bir kullanıcı oluşturun.*
  
* **Active Directory ormanında zaten kullanıcı çalışmak için eşleşen mantığı için gerekli kimlikleri içeriyor mu?**

  * *Örnek: Bu yeni bir iş günü dağıtım kurulumuysa, Active Directory ile eşleşen mantıksal mümkün olduğunca basit tutmak için doğru Workday Worker_ID değerleri (veya tercih ettiğiniz benzersiz kimlik değerini) önceden doldurulmuş olması önerilir.*

Ayarlanmış ve bu özel bağlayıcı uygulamaları sağlama yapılandırma, bu öğreticinin geri kalan bölümler konudur. Ortamınızda kiracılar hangi sistemlerin, sağlamak ve kaç Active Directory etki alanları ve Azure AD için ihtiyacınız olan hangi uygulamaların yapılandırmayı seçtiğinize bağlıdır.

## <a name="configure-integration-system-user-in-workday"></a>Workday'de tümleştirme sistemi kullanıcısı yapılandırma

Bağlayıcılar sağlama Workday ortak gereksinimi, Workday İnsan Kaynakları API'sine bağlanmak için bir Workday entegrasyonu sistem kullanıcının kimlik bilgilerini gerekmesidir. Bu bölümde, Workday'de bir tümleştirme sistemi kullanıcısı oluşturmayı açıklar ve aşağıdaki bölümleri içerir:

* [Bir tümleştirme sistemi kullanıcısı oluşturma](#creating-an-integration-system-user)
* [Bir tümleştirme güvenlik grubu oluşturma](#creating-an-integration-security-group)
* [Etki alanı güvenlik ilkesi izinleri yapılandırma](#configuring-domain-security-policy-permissions)
* [İş işlemi Güvenlik İlkesi izinleri yapılandırma](#configuring-business-process-security-policy-permissions)
* [Güvenlik İlkesi değişikliklerini etkinleştirme](#activating-security-policy-changes)

> [!NOTE]
> Bu yordamı atlayabilir ve bunun yerine sistem tümleştirme hesabı olarak Workday genel yönetici hesabını kullanmak mümkündür. Tanıtımlar düzgün çalışıyor, ancak üretim dağıtımları için önerilmez.

### <a name="creating-an-integration-system-user"></a>Bir tümleştirme sistemi kullanıcısı oluşturma

**Bir tümleştirme sistemi kullanıcısı oluşturmak için:**

1. Bir yönetici hesabını kullanarak Workday kiracınızda oturum oturum açın. İçinde **Workday uygulama**, girin arama kutusuna bir kullanıcı oluşturup ardından **tümleştirme sistemi kullanıcısı Oluştur**.

    ![Kullanıcı oluşturma](./media/workday-inbound-tutorial/wd_isu_01.png "kullanıcı oluşturma")
2. Tamamlamak **tümleştirme sistemi kullanıcısı Oluştur** görev tarafından yeni bir tümleştirme sistemi kullanıcısı için bir kullanıcı adı ve parola belirtin.  
  
* Bırakın **gerektiren yeni parolayı sonraki oturum açma** seçeneğinin işaretli olmadığından bu kullanıcının programlı olarak oturum açan.
* Bırakın **oturum zaman aşımı dakikaları** 0 varsayılan değeri ile hangi engeller kullanıcının oturumları tamamlanmadan zaman aşımına uğramasını.
* Seçeneğini **UI oturumları izin** olarak eklenen bir tümleştirme sisteminin parolası olan bir kullanıcı iş günü içinde oturum açmasını engelleyen güvenlik katmanı sağlar.

    ![Tümleştirme sistemi kullanıcısı Oluştur](./media/workday-inbound-tutorial/wd_isu_02.png "tümleştirme sistemi kullanıcısı oluştur")

### <a name="creating-an-integration-security-group"></a>Bir tümleştirme güvenlik grubu oluşturma

Bu adımda, Workday'de bir kendilerine, sınırlandırılmamış veya kısıtlı tümleştirme sistemi güvenlik grubu oluşturun ve bu grup için bir önceki adımda oluşturduğunuz tümleştirme sistemi kullanıcısı atayın.

**Bir güvenlik grubu oluşturmak için:**

1. Girin arama kutusuna güvenlik grubu oluşturun ve ardından **güvenlik grubu oluşturma**.

    ![Güvenlik grubu](./media/workday-inbound-tutorial/wd_isu_03.png "güvenlik grubu oluştur")
2. Tamamlamak **güvenlik grubu oluşturma** görev. 

   * Workday'de güvenlik gruplarının iki tür vardır:
     * **Sınırlandırılmamış:** Güvenlik grubunun tüm üyeleri güvenlik grubu tarafından korunan tüm veri örnekleri erişebilirsiniz.
     * **Kısıtlı:** Tüm güvenlik grubu üyelerini bir alt güvenlik grubunun erişimi olan veri örnekleri (satırlar) bağlamsal erişebilir.
   * Tümleştirme için uygun güvenlik grubu türünü seçin, Workday tümleştirmesi ortağınıza başvurun.
   * Grup türü öğrendikten sonra Seç **tümleştirme sistemi güvenlik grubunu (sınırlandırılmamış)** veya **tümleştirme sistemi güvenlik grubunu (Constrained)** gelen **kiralanan güvenlik grubu türü**  açılır.

     ![Güvenlik grubu](./media/workday-inbound-tutorial/wd_isu_04.png "güvenlik grubu oluştur")

3. Güvenlik grubu oluşturma işlemi başarılı olduktan sonra burada üyeleri güvenlik grubuna atayabilirsiniz bir sayfa görürsünüz. Bu güvenlik grubu için önceki adımda oluşturulan yeni tümleştirme sistemi kullanıcısı ekleyin. Kullanıyorsanız *kısıtlı* güvenlik grubu, aynı zamanda uygun kuruluş kapsamı seçmeniz gerekir.

    ![Güvenlik grubunu Düzenle](./media/workday-inbound-tutorial/wd_isu_05.png "güvenlik grubunu Düzenle")

### <a name="configuring-domain-security-policy-permissions"></a>Etki alanı güvenlik ilkesi izinleri yapılandırma

Bu adımda, "etki alanı güvenliği" çalışan verileri güvenlik grubuna ilke izin vermesi.

**Etki alanı güvenlik ilkesi izinleri yapılandırmak için:**

1. Girin **etki alanı güvenlik yapılandırması** arama kutusuna ve ardından bağlantıyı **etki alanı güvenlik yapılandırma raporu**.  

    ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/wd_isu_06.png "etki alanı güvenlik ilkeleri")  
2. İçinde **etki alanı** metin kutusuna aşağıdaki etki alanları için arama yapın ve bunları tek tek filtre ekleyin.  
   * *Dış hesap sağlama*
   * *Çalışan verileri: Ortak çalışanı raporları*
   * *Kişi veri: Kişi bilgileri*
   * *Çalışan verileri: Tüm Konumlar*
   * *Çalışan verileri: Geçerli portalınıza bilgileri*
   * *Çalışan verileri: Çalışan profilindeki iş başlığı*
   * *Workday hesapları*
   
     ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/wd_isu_07.png "etki alanı güvenlik ilkeleri")  

     ![Etki alanı güvenlik ilkeleri](./media/workday-inbound-tutorial/wd_isu_08.png "etki alanı güvenlik ilkeleri") 

     **Tamam**'ı tıklatın.

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
   | GET ve Put | Kişi veri: Kişi bilgileri |
   | Al | Çalışan verileri: Tüm Konumlar |
   | Al | Çalışan verileri: Geçerli portalınıza bilgileri |
   | Al | Çalışan verileri: Çalışan profilindeki iş başlığı |
   | GET ve Put | Workday hesapları |

### <a name="configuring-business-process-security-policy-permissions"></a>İş işlemi Güvenlik İlkesi izinleri yapılandırma

Bu adımda, "iş işlem güvenliği" çalışan verileri güvenlik grubuna ilke izin vermesi. Bu adım, Workday geri yazma uygulama bağlayıcısını ayarlarken ayarlanması için gereklidir.

**İş işlemi Güvenlik İlkesi izinleri yapılandırmak için:**

1. Girin **işlem İlkesi** arama kutusuna ve ardından bağlantıyı **iş işlem güvenlik ilkesini Düzenle** görev.  

    ![İş işlem güvenlik ilkelerini](./media/workday-inbound-tutorial/wd_isu_12.png "iş işlem güvenlik ilkeleri")  

2. İçinde **iş işlem türü** metin arama *kişi* seçip **kişi değişiklik** iş süreçleri ve tıklatın **Tamam**.

    ![İş işlem güvenlik ilkelerini](./media/workday-inbound-tutorial/wd_isu_13.png "iş işlem güvenlik ilkeleri")  

3. Üzerinde **iş işlem güvenlik ilkesini Düzenle** sayfasında, kaydırma **korumak iletişim bilgileri (Web hizmeti)** bölümü.

    ![İş işlem güvenlik ilkelerini](./media/workday-inbound-tutorial/wd_isu_14.png "iş işlem güvenlik ilkeleri")  

4. Seçin ve web hizmetleri isteği başlatabilirsiniz güvenlik grupları listesine yeni bir tümleştirme sistemi güvenlik grubunu ekleyin. Tıklayarak **Bitti**. 

    ![İş işlem güvenlik ilkelerini](./media/workday-inbound-tutorial/wd_isu_15.png "iş işlem güvenlik ilkeleri")  

### <a name="activating-security-policy-changes"></a>Güvenlik İlkesi değişikliklerini etkinleştirme

**Güvenlik İlkesi değişiklikleri etkinleştirmek için:**

1. Girin arama kutusuna etkinleştirin ve ardından bağlantıyı **etkinleştirme bekleyen güvenlik ilkesi değişikliklerini**.

    ![Etkinleştirme](./media/workday-inbound-tutorial/wd_isu_16.png "etkinleştir")

1. Bekleyen Güvenlik İlkesi değişikliklerini etkinleştir görev denetim amacıyla bir açıklama girerek başlayın ve ardından **Tamam**.
1. Onay kutusunu işaretleyerek sonraki ekranda bir görevi tamamlamak **Onayla**ve ardından **Tamam**.

    ![Bekleyen güvenlik ayarlarını etkinleştir](./media/workday-inbound-tutorial/wd_isu_18.png "bekleyen güvenlik ayarlarını etkinleştir")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Active Directory'ye Workday'den kullanıcı sağlamayı yapılandırma

Bu bölümde kullanıcı hesabı Workday'den tümleştirmenizi kapsamındaki her Active Directory etki alanına sağlama adımlarını sunar.

* [Yükleme ve şirket içi sağlama Aracısı Yapılandırma](#part-1-install-and-configure-on-premises-provisioning-agents)
* [Sağlama bağlayıcı uygulama ekleme ve Workday bağlantısı oluşturma](#part-2-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday)
* [Öznitelik eşlemelerini yapılandırma](#part-3-configure-attribute-mappings)
* [Etkinleştirme ve kullanıcı sağlamayı başlatın](#enable-and-launch-user-provisioning)

### <a name="part-1-install-and-configure-on-premises-provisioning-agents"></a>1. Bölüm: Yükleme ve şirket içi sağlama Aracısı Yapılandırma

Şirket için Active Directory sağlamak için bir aracı .NET 4.7.1+ Framework ve ağ erişmek istediğiniz Active Directory etki alanları için olan bir sunucuya yüklenmelidir.

> [!TIP]
> Sunucunuzda sağlanan yönergeleri kullanarak .NET framework sürümünü denetleyebilir [burada](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed).
> Sunucu .NET 4.7.1 yok ya da sonraki bir sürümün yüklü, buradan indirebilirsiniz [burada](https://support.microsoft.com/help/4033342/the-net-framework-4-7-1-offline-installer-for-windows).  

.NET 4.7.1+ dağıttıktan sonra indirebileceğiniz **[şirket burada sağlama aracı](https://go.microsoft.com/fwlink/?linkid=847801)** ve aracı yapılandırmasını tamamlamak için aşağıda verilen adımları izleyin.

1. Yeni aracıyı yüklemek istediğiniz Windows sunucusuna oturum açın.

1. Sağlama Aracısı Yükleyicisi'ni başlatın, koşulları kabul ve tıklayarak **yükleme** düğmesi.

   ![Ekran yükleme](./media/workday-inbound-tutorial/pa_install_screen_1.png "yükleme ekranı")
   
1. Yükleme tamamlandığında, Sihirbazı başlatılır ve göreceğiniz sonra **Azure AD Connect** ekran. Tıklayarak **doğrulaması** Azure AD Örneğinize bağlanmak için düğme.

   ![Azure AD connect](./media/workday-inbound-tutorial/pa_install_screen_2.png "Azure AD'ye bağlanma")
   
1. Azure AD Örneğinize genel yönetici kimlik bilgilerini kullanarak kimlik doğrulaması.

   ![Yönetici kimlik doğrulama](./media/workday-inbound-tutorial/pa_install_screen_3.png "yönetici kimlik doğrulama")

   > [!NOTE]
   > Azure AD yönetici kimlik bilgilerini yalnızca Azure AD kiracınıza bağlamak için kullanılır. Aracı kimlik bilgileri yerel olarak sunucuda depolamaz.

1. Azure AD ile başarılı kimlik doğrulamadan sonra göreceğiniz **Active Directory'ye bağlanın** ekran. Bu adımda, AD etki alanı adınızı girin ve tıklayın **Dizin Ekle** düğmesi.

   ![Dizin Ekle](./media/workday-inbound-tutorial/pa_install_screen_4.png "Dizin Ekle")
  
1. Şimdi AD etki alanına bağlanmak için gereken kimlik bilgilerini girmeniz istenir. Aynı ekranda kullanabilirsiniz **seçin etki alanı denetleyicisi öncelik** aracı sağlama isteği göndermek için kullanacağı bir etki alanı denetleyicileri belirtmek için.

   ![Etki alanı kimlik bilgileri](./media/workday-inbound-tutorial/pa_install_screen_5.png)
   
1. Etki alanı yapılandırdıktan sonra yükleyici yapılandırılmış etki alanlarının bir listesini görüntüler. Bu ekranda yineleyin #5 ve daha fazlasını eklemek için #6. adım etki alanları veya tıklayarak **sonraki** aracı kaydı devam etmek için.

   ![Etki alanı yapılandırılmış](./media/workday-inbound-tutorial/pa_install_screen_6.png "yapılandırılmış etki alanları")

   > [!NOTE]
   > Birden çok AD etki alanına (örneğin, na.contoso.com, emea.contoso.com) ve ardından her bir etki alanı ayrı ayrı listeye eklemek Lütfen durumunda.
   > Yalnızca üst etki alanını (örneğin, contoso.com) eklemek, yeterli değildir. Her alt etki alanı aracıyla kaydetmeniz gerekir.
   
1. Yapılandırma ayrıntılarını gözden geçirin ve tıklayın **Onayla** aracıyı kaydetmek için.
  
   ![Ekran onaylayın](./media/workday-inbound-tutorial/pa_install_screen_7.png "ekran onaylayın")
   
1. Yapılandırma Sihirbazı'nı aracı kaydını ilerlemesini görüntüler.
  
   ![Aracı kaydı](./media/workday-inbound-tutorial/pa_install_screen_8.png "aracı kaydı")
   
1. Aracı kaydı başarılı olduktan sonra tıklayabilirsiniz **çıkmak** sihirbazdan çıkmak için.
  
   ![Çıkış ekranı](./media/workday-inbound-tutorial/pa_install_screen_9.png "çıkış ekranı")
   
1. Aracı yüklemesini doğrulayın, "Hizmetler" ek bileşenini açarak çalıştığından emin olun ve "Microsoft Azure AD Connect'i Hazırlama aracı" adlı hizmet için Ara
  
   ![Hizmetler](./media/workday-inbound-tutorial/services.png)

### <a name="part-2-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>2. Bölüm: Sağlama bağlayıcı uygulama ekleme ve Workday bağlantısı oluşturma

**Active Directory sağlama için Workday yapılandırmak için:**

1. Şuraya gidin: <https://portal.azure.com>

2. Sol gezinti çubuğunda **Azure Active Directory**

3. Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4. Seçin **uygulama ekleme**seçip **tüm** kategorisi.

5. Arama **Active Directory'ye Workday sağlama**ve bu uygulama galerideki ekleyin.

6. Uygulama eklenir ve uygulama Ayrıntılar ekranında gösterilen, seçin sonra **sağlama**

7. Değişiklik **sağlama** **modu** için **otomatik**

8. Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – Kiracı etki alanı adı eklenir Workday tümleştirmesi sistem hesabının kullanıcı adını girin. Aşağıdakine benzer olmalıdır: **kullanıcıadı\@tenant_name**

   * **Yönetici parolası –** Workday tümleştirmesi sistem hesabının parolasını girin

   * **Kiracı URL'si –** kiracınız için Workday web hizmetleri uç nokta URL'sini girin. Bu değer gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4burada *contoso4* doğru Kiracı adınızla değiştirilir ve *wd3 Impl* doğru ortamı dize ile değiştirilir.

   * **Active Directory ormanı -** aracıyla kayıtlı, Active Directory etki alanı, "Name". Sağlama için hedef etki alanı seçmek için açılan listeyi kullanın. Bu değer genellikle olduğu gibi bir dizedir: *contoso.com*

   * **Active Directory kapsayıcısı -** DN kapsayıcı Aracısı varsayılan olarak kullanıcı hesaplarını burada oluşturmalısınız girin.
        Örnek: *OU standart kullanıcılar, OU = Kullanıcılar, DC = contoso, DC = test =*
        
     > [!NOTE]
     > Bu ayar yalnızca yürütme için kullanıcı hesabı oluşturma, salesforce'taki *parentDistinguishedName* öznitelik öznitelik eşlemelerini yapılandırılmadı. Bu ayar kullanılmaz kullanıcı arama veya güncelleştirme işlemleri. Tüm etki alanı alt ağacı, arama işlemi kapsamında döner.

   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.

     > [!NOTE]
     > İçine sağlama işi aşması durumunda Azure AD sağlama hizmeti, e-posta bildirimi gönderir. bir [karantina](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#quarantine) durumu.

   * Tıklayın **Test Bağlantısı** düğmesi. Bağlantı testi başarılı olursa tıklayın **Kaydet** üstünde düğme. Başarısız olursa, Workday kimlik bilgilerini ve aracı kurulumu üzerinde yapılandırılmış AD kimlik bilgileri geçerli olduğunu denetleyin.

     ![Azure portal](./media/workday-inbound-tutorial/wd_1.png)

   * Kimlik bilgileri başarıyla kaydedildikten sonra **eşlemeleri** bölümünde Varsayılan eşleme görüntüler **Workday çalışanlarına şirket içi Active Directory eşitleme**

### <a name="part-3-configure-attribute-mappings"></a>3. Bölüm: Öznitelik eşlemelerini yapılandırma

Bu bölümde, Active Directory'ye Workday'den kullanıcı verilerin nasıl aktığını yapılandıracaksınız.

1. Sağlama sekmesinde **eşlemeleri**, tıklayın **şirket içi Active Directory eşitleme Workday'deki çalışanları**.

1. İçinde **kaynak nesne kapsamı** alan, kullanıcıların hangi kümesi workday'deki AD için öznitelik tabanlı bir filtre kümesi tanımlayarak sağlama kapsamında olmalıdır seçebilirsiniz. Varsayılan "iş günü içinde tüm kullanıcılar" kapsamıdır. Örnek filtreler:

   * Örnek: Çalışan kimlikleri kullanıcılarla 1000000 2000000 (2000000 hariç) arasındaki kapsama

      * Öznitelik: WorkerID

      * İşleç: REGEX Match

      * Değer: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Örnek: Yalnızca çalışanları ve bağlı çalışanları değil

      * Öznitelik: EmployeeID

      * İşleç: NULL DEĞİL

   > [!TIP]
   > Sağlama uygulamayı ilk kez yapılandırırken, test edin ve bu, istenen sonucu veriyor emin olmak için ifadeleri ve öznitelik eşlemelerini doğrulayın gerekecektir. Microsoft öneriyor kapsam belirleme filtrelerini altında **kaynak nesne kapsamı** eşlemelerinizi birkaç test workday'deki kullanıcıları ayıklayıp sorgulayabilecek ile test etmek için. Bir kez eşlemeleri çalışır ve ardından filtreyi kaldırın veya aşamalı olarak daha fazla kullanıcı içerecek şekilde genişletin doğrulanmıştır.

1. İçinde **hedef nesne eylemleri** alan, küresel olarak filtreleyebilirsiniz hangi eylemleri Active Directory üzerinde gerçekleştirilir. **Oluşturma** ve **güncelleştirme** en yaygın olarak.

1. İçinde **öznitelik eşlemelerini** bölümünde, tek tek nasıl Workday tanımlayabilirsiniz öznitelikleri eşlemek için Active Directory öznitelikleri.

1. Güncelleştirmek için varolan bir özniteliği eşlemesini üzerinde veya tıklatın **yeni eklemesi** yeni eşlemeler eklemek için ekranın alt kısmındaki. Tek tek özellik eşlemesi, bu özellikleri destekler:

      * **Eşleme türü**

         * **Doğrudan** – Workday özniteliğinin değeri değişikliğine gerek olmadan AD özniteliği Yazar

         * **Sabit** -AD özniteliği için bir statik, sabit dize değeri yazın

         * **İfade** – bir veya daha fazla Workday özniteliklerine dayalı, AD özniteliği için özel bir değer yazmanızı sağlar. [Daha fazla bilgi için bu makalede ifadelerini bkz](../manage-apps/functions-for-customizing-application-data.md).

      * **Kaynak özniteliği** -Workday kullanıcı özniteliği. Aradığınız özniteliği mevcut değilse bkz [Workday kullanıcı özniteliklerinin listesi özelleştirme](#customizing-the-list-of-workday-user-attributes).

      * **Varsayılan değer** : isteğe bağlı. Kaynak özniteliği boş bir değere sahipse, eşleme bu değeri yerine yazın.
            Bu alanı boş bırakırsanız en yaygın yapılandırmadır.

      * **Hedef öznitelik** – Active Directory'deki kullanıcı özniteliği.

      * **Bu özniteliği kullanarak nesneleri eşleşen** – Bu eşleme kullanıcılar Workday ile Active Directory arasında benzersiz olarak tanımlanabilmesi için kullanılması gereken olup olmadığını. Bu değer genellikle Active Directory çalışanın Kimliğini öznitelikleri birine eşlenmiş iş günü için çalışan kimliği alanı genellikle ayarlanır.

      * **Eşleşen öncelik** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alan tarafından tanımlanan sırayla değerlendirilir. Bir eşleşme bulunduğu sürece başka eşleştirme öznitelikleri değerlendirilir.

      * **Bu eşlemeyi Uygula**

         * **Her zaman** – bu eşlemeyi Uygula her iki kullanıcı oluşturma ve güncelleştirme eylemleri

         * **Yalnızca oluşturma sırasında** -yalnızca kullanıcı oluşturma eylemleri bu eşlemeyi Uygula

1. Eşlemelerinizi kaydetmek için tıklatın **Kaydet** öznitelik eşlemesi bölümün üst kısmındaki.

   ![Azure portal](./media/workday-inbound-tutorial/wd_2.png)

#### <a name="below-are-some-example-attribute-mappings-between-workday-and-active-directory-with-some-common-expressions"></a>Aşağıda bazı örnek, bazı yaygın ifadelerle Workday ve Active Directory arasında öznitelik eşlemelerini verilmiştir

* Eşlenen ifade *parentDistinguishedName* özniteliği, bir kullanıcı bir veya daha fazla Workday kaynak özniteliklerine dayalı farklı OU'lara sağlamak için kullanılır. Bu örnek kullanıcılara göre farklı OU'larda yerleştirir içinde bulundukları üzerinde hangi şehirde.

* *UserPrincipalName* Active Directory'deki öznitelik yinelenenleri kaldırma işlevi kullanılarak oluşturulan [SelectUniqueValue](../manage-apps/functions-for-customizing-application-data.md#selectuniquevalue) üretilen bir değeri hedef AD etki alanında bulunup bulunmadığını denetler ve yalnızca benzersiz olup olmadığını ayarlar.  

* [Burada ifadeler yazma belgeleri yoktur](../manage-apps/functions-for-customizing-application-data.md). Bu bölüm, özel karakterler kaldırma örnekleri içerir.

| WORKDAY ÖZNİTELİĞİ | ACTIVE DIRECTORY ÖZNİTELİĞİ |  KİMLİĞİ EŞLEŞİYOR MU? | OLUŞTURMA / GÜNCELLEŞTİRME |
| ---------- | ---------- | ---------- | ---------- |
| **WorkerID**  |  EmployeeID | **Evet** | Yazılan yalnızca oluşturma sırasında |
| **PreferredNameData**    |  CN =    |   |   Yazılan yalnızca oluşturma sırasında |
| **SelectUniqueValue (katılın ("\@", katılın (".", \[FirstName\], \[LastName\]), "contoso.com"), katılın ("\@", katılın (".", Orta (\[FirstName\], 1, 1 () \[LastName\]), "contoso.com"), katılın ("\@", katılın (".", Orta (\[FirstName\], 1, 2), \[LastName\]), "contoso.com"))**   | userPrincipalName     |     | Yazılan yalnızca oluşturma sırasında 
| **Değiştir(Orta(değiştirin(\[UserID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**      |    SAMAccountName            |     |         Yazılan yalnızca oluşturma sırasında |
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

Öznitelik eşlemesi Yapılandırma tamamlandıktan sonra artık [etkinleştirme ve kullanıcı sağlama hizmeti başlatma](#enable-and-launch-user-provisioning).

## <a name="configuring-user-provisioning-to-azure-ad"></a>Azure AD'de kullanıcı sağlamayı yapılandırma

Aşağıdaki bölümlerde, yalnızca bulut dağıtımları için Azure AD'ye Workday'den kullanıcı hazırlama işleminin yapılandırılması için gerekli adımları açıklanmaktadır.

* [Azure AD Bağlayıcısı uygulama sağlama ve Workday bağlantı ekleme](#part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday)
* [Workday ile Azure AD öznitelik eşlemelerini yapılandırma](#part-2-configure-workday-and-azure-ad-attribute-mappings)
* [Etkinleştirme ve kullanıcı sağlamayı başlatın](#enable-and-launch-user-provisioning)

> [!IMPORTANT]
> Azure AD'ye sağlanması gereken yalnızca bulut kullanıcılarına varsa ve şirket içi Active Directory'nin değil, yalnızca aşağıdaki yordamı izleyin.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>1. Bölüm: Azure AD Bağlayıcısı uygulama sağlama ve Workday bağlantı ekleme

**Yalnızca bulutta yer alan kullanıcılar için Azure Active Directory sağlama için Workday yapılandırmak için:**

1. <https://portal.azure.com> kısmına gidin.

2. Sol gezinti çubuğunda **Azure Active Directory**

3. Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4. Seçin **uygulama ekleme**ve ardından **tüm** kategorisi.

5. Arama **Azure AD sağlama için Workday**ve bu uygulama galerideki ekleyin.

6. Uygulama eklenir ve uygulama Ayrıntılar ekranında gösterilen, seçin sonra **sağlama**

7. Değişiklik **sağlama** **modu** için **otomatik**

8. Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – Kiracı etki alanı adı eklenir Workday tümleştirmesi sistem hesabının kullanıcı adını girin. Aşağıdaki gibi görünmelidir: username@contoso4

   * **Yönetici parolası –** Workday tümleştirmesi sistem hesabının parolasını girin

   * **Kiracı URL'si –** kiracınız için Workday web hizmetleri uç nokta URL'sini girin. Bu değer gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resourcesburada *contoso4* doğru Kiracı adınızla değiştirilir ve *wd3 Impl* doğru ortamı dize ile değiştirilir. Lütfen bu URL'yi bilinmiyor kullanılacak URL'nin doğru belirlemek için Workday tümleştirmesi iş ortağı veya destek temsilcinize ile çalışın.

   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.

   * Tıklayın **Test Bağlantısı** düğmesi.

   * Bağlantı testi başarılı olursa tıklayın **Kaydet** üstünde düğme. Başarısız olursa, Workday URL'yi ve kimlik bilgilerini Workday'de geçerli olduğunu denetleyin.

### <a name="part-2-configure-workday-and-azure-ad-attribute-mappings"></a>2. Bölüm: Workday ile Azure AD öznitelik eşlemelerini yapılandırma

Bu bölümde, kullanıcı verilerini Workday'den Azure Active Directory'ye yalnızca bulut kullanıcıları için nasıl aktığını yapılandıracaksınız.

1. Sağlama sekmesinde **eşlemeleri**, tıklayın **eşitleme çalışanları Azure AD'ye**.

2. İçinde **kaynak nesne kapsamı** alan, kullanıcıların hangi kümesi workday'deki öznitelik tabanlı bir filtre kümesi tanımlayarak Azure AD'ye sağlama kapsamında olmalıdır seçebilirsiniz. Varsayılan "iş günü içinde tüm kullanıcılar" kapsamıdır. Örnek filtreler:

   * Örnek: Çalışan kimlikleri kullanıcılarla 1000000 2000000 arasındaki kapsama

      * Öznitelik: WorkerID

      * İşleç: REGEX Match

      * Değer: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Örnek: Yalnızca bağlı çalışanları ve değil düzenli çalışanları

      * Öznitelik: ContingentID

      * İşleç: NULL DEĞİL

3. İçinde **hedef nesne eylemleri** alan, küresel olarak filtreleyebilirsiniz hangi eylemlerin Azure AD üzerinde gerçekleştirilir. **Oluşturma** ve **güncelleştirme** en yaygın olarak.

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

   * **Bu özniteliği kullanarak nesneleri eşleşen** – bu öznitelik Workday ile Azure AD arasında kullanıcıları benzersiz olarak tanımlanabilmesi için kullanılması gereken olup olmadığını. Bu değer genellikle Azure AD'de çalışan ID özniteliği (yeni) ya da uzantısı özniteliği eşlenen iş günü için çalışan kimliği alanı genellikle ayarlanır.

   * **Eşleşen öncelik** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alan tarafından tanımlanan sırayla değerlendirilir. Bir eşleşme bulunduğu sürece başka eşleştirme öznitelikleri değerlendirilir.

   * **Bu eşlemeyi Uygula**

     * **Her zaman** – bu eşlemeyi Uygula her iki kullanıcı oluşturma ve güncelleştirme eylemleri

     * **Yalnızca oluşturma sırasında** -yalnızca kullanıcı oluşturma eylemleri bu eşlemeyi Uygula

6. Eşlemelerinizi kaydetmek için tıklatın **Kaydet** öznitelik eşlemesi bölümün üst kısmındaki.

Öznitelik eşlemesi Yapılandırma tamamlandıktan sonra artık [etkinleştirme ve kullanıcı sağlama hizmeti başlatma](#enable-and-launch-user-provisioning).

## <a name="configuring-azure-ad-attribute-writeback-to-workday"></a>Azure AD özniteliği geri yazma için Workday yapılandırma

Kullanıcı e-posta adreslerini ve Azure Active Directory'den Workday kullanıcı adı için geri yazma yapılandırmak için bu yönergeleri izleyin.

* [Geri yazma bağlayıcı uygulama ekleme ve Workday bağlantısı oluşturma](#part-1-adding-the-writeback-connector-app-and-creating-the-connection-to-workday)
* [Geri yazma öznitelik eşlemelerini yapılandırma](#part-2-configure-writeback-attribute-mappings)
* [Etkinleştirme ve kullanıcı sağlamayı başlatın](#enable-and-launch-user-provisioning)

### <a name="part-1-adding-the-writeback-connector-app-and-creating-the-connection-to-workday"></a>1. Bölüm: Geri yazma bağlayıcı uygulama ekleme ve Workday bağlantısı oluşturma

**Workday geri yazma bağlayıcı yapılandırmak için:**

1. Şuraya gidin: <https://portal.azure.com>

2. Sol gezinti çubuğunda **Azure Active Directory**

3. Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

4. Seçin **uygulama ekleme**, ardından **tüm** kategorisi.

5. Arama **Workday geri yazma**ve bu uygulama galerideki ekleyin.

6. Uygulama eklenir ve uygulama Ayrıntılar ekranında gösterilen, seçin sonra **sağlama**

7. Değişiklik **sağlama** **modu** için **otomatik**

8. Tamamlamak **yönetici kimlik bilgileri** gibi bölümünde:

   * **Yönetici kullanıcı adı** – Kiracı etki alanı adı eklenir Workday tümleştirmesi sistem hesabının kullanıcı adını girin. Aşağıdakine benzer olmalıdır: *kullanıcıadı\@contoso4*

   * **Yönetici parolası –** Workday tümleştirmesi sistem hesabının parolasını girin

   * **Kiracı URL'si –** kiracınız için Workday web hizmetleri uç nokta URL'sini girin. Bu değer gibi görünmelidir: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resourcesburada *contoso4* doğru Kiracı adınızla değiştirilir ve *wd3 Impl* (gerekirse) doğru ortamı dize ile değiştirilir.

   * **Bildirim e-posta –** e-posta adresinizi girin ve "hatası oluşursa, e-posta Gönder" onay kutusunu işaretleyin.

   * Tıklayın **Test Bağlantısı** düğmesi. Bağlantı testi başarılı olursa tıklayın **Kaydet** üstünde düğme. Başarısız olursa, Workday URL'yi ve kimlik bilgilerini Workday'de geçerli olduğunu denetleyin.

### <a name="part-2-configure-writeback-attribute-mappings"></a>2. Bölüm: Geri yazma öznitelik eşlemelerini yapılandırma

Bu bölümde, nasıl Azure AD'den geri yazma öznitelikleri için Workday akış yapılandıracaksınız. Şu anda bağlayıcı yalnızca e-posta adresi ve Workday kullanıcı adını geri yazmayı destekler.

1. Sağlama sekmesinde **eşlemeleri**, tıklayın **eşitleme Azure Active Directory Kullanıcıları için Workday**.

2. İçinde **kaynak nesne kapsamı** alan, isteğe bağlı olarak, hangi filtreleyebilirsiniz Azure Active Directory'de kullanıcı kümeleri için e-posta adreslerini geri Workday için yazılmış olmalıdır. Varsayılan "tüm kullanıcılar Azure AD'de" kapsamıdır.

3. İçinde **öznitelik eşlemelerini** bölümünde, Azure Active Directory'de Workday'deki çalışan kimliği veya çalışan kimliği depolandığı özniteliğini belirtmek için eşleşen kodunu güncelleştirin. Popüler eşleşen yöntemi Workday'deki çalışan kimliği veya extensionAttribute1 15 çalışan kimliği, Azure AD'de eşitlemek ve ardından bu özniteliği Azure AD'de geri Workday'de kullanıcıları eşleştirmek için sağlamaktır.

4. Azure AD genellikle eşlemeniz *userPrincipalName* özniteliği için Workday *UserID* özniteliği ve Azure AD eşleme *posta* özniteliği için Workday  *EmailAddress* özniteliği. Eşlemelerinizi kaydetmek için tıklatın **Kaydet** öznitelik eşlemesi bölümün üst kısmındaki.

Öznitelik eşlemesi Yapılandırma tamamlandıktan sonra artık [etkinleştirme ve kullanıcı sağlama hizmeti başlatma](#enable-and-launch-user-provisioning).

## <a name="enable-and-launch-user-provisioning"></a>Etkinleştirme ve kullanıcı sağlamayı başlatın

Workday sağlama uygulama yapılandırmaları tamamladıktan sonra Azure portalında sağlama hizmeti etkinleştirebilirsiniz.

> [!TIP]
> Sağlama hizmeti üzerinde açtığınızda varsayılan olarak kapsamdaki tüm kullanıcılar için sağlama işlemleri başlatır. Hatalar varsa eşleştirme veya Workday veri sorunlarıyla sonuçlanabileceğinden sağlama işi başarısız ve karantina durumuna geçer. Bu, en iyi uygulama önlemek için yapılandırma öneririz **kaynak nesne kapsamı** filtre ve tüm kullanıcılar için tam eşitleme başlatmadan önce öznitelik eşlemelerini birkaç test kullanıcı ile test etme. Eşlemeleri çalışma ve olduğunu doğruladıktan sonra filtreyi kaldırın veya aşamalı olarak daha fazla kullanıcı içerecek şekilde genişletin ya da sonra istenen sonuçları elde ayırabilir.

1. İçinde **sağlama** sekmesinde, belirleyin **sağlama durumu** için **üzerinde**.

2. **Kaydet**’e tıklayın.

3. Bu işlem, değişken sayıda kaç kullanıcının Workday'deki kiracıda olduğunu bağlı olarak saat sürebilir ilk eşitleme başlar. 

4. Herhangi bir zamanda denetleyin **denetim günlükleri** Azure portalında sağlama hizmeti gerçekleştirdiği eylemleri görmek için sekmesinde. Denetim günlüklerini olduğu gibi kullanıcıların Workday dışında okuma gönderildiğini ve ardından daha sonra eklendiğinde veya Active Directory sağlama hizmeti tarafından gerçekleştirilen tüm bireysel eşitleme olayları listeler. Denetim günlüklerini gözden geçirin ve sağlama hataları düzeltmek yönergeler için sorun giderme bölümüne bakın.

5. İlk eşitleme tamamlandıktan sonra bir denetim özet raporu yazacak **sağlama** sekmesinde, aşağıda gösterildiği gibi.

   ![Azure portal](./media/workday-inbound-tutorial/wd_3.png)

## <a name="frequently-asked-questions-faq"></a>Sık Sorulan Sorular (SSS)

* **Çözüm özelliği sorular**
  * [Yeni bir işe ayıklayıp sorgulayabilecek işlerken nasıl çözüme yeni kullanıcı hesabının parolasını Active Directory'de ayarlar?](#when-processing-a-new-hire-from-workday-how-does-the-solution-set-the-password-for-the-new-user-account-in-active-directory)
  * [Çözümü, e-posta bildirimleri gönderme işlemleri tam sağladıktan sonra destekliyor mu?](#does-the-solution-support-sending-email-notifications-after-provisioning-operations-complete)
  * [Nasıl teslim parolaların yeni katılanlarla ilgili yönetmek ve güvenli bir şekilde kullanıcının parolasını sıfırlamak için bir mekanizma sağlar?](#how-do-i-manage-delivery-of-passwords-for-new-hires-and-securely-provide-a-mechanism-to-reset-their-password)
  * [Çözüm, Workday kullanıcı profillerini Azure AD bulutta veya sağlama aracı katmanında önbelleğe almaz?](#does-the-solution-cache-workday-user-profiles-in-the-azure-ad-cloud-or-at-the-provisioning-agent-layer)
  * [Mu çözüm atama desteği şirket içi AD kullanıcı gruplarına?](#does-the-solution-support-assigning-on-premises-ad-groups-to-the-user)
  * [Hangi Workday API'leri, sorgu ve güncelleştirme Workday'deki çalışan profillerine çözüm kullanıyor mu?](#which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles)
  * [İki Azure AD kiracılarıyla paylaştığında Workday HCM kiracımı yapılandırabilir miyim?](#can-i-configure-my-workday-hcm-tenant-with-two-azure-ad-tenants)
  * [Neden Azure AD Connect biz dağıttıysanız, "Azure AD iş günü" kullanıcı uygulama sağlama desteklenmez?](#why-workday-to-azure-ad-user-provisioning-app-is-not-supported-if-we-have-deployed-azure-ad-connect)
  * [Nasıl gelişmeler Öner veya Workday ve Azure AD tümleştirmesiyle ilgili yeni özellikler talep?](#how-do-i-suggest-improvements-or-request-new-features-related-to-workday-and-azure-ad-integration)

* **Sağlama Aracısı sorular**
  * [Sağlama Aracısı'nın GA sürümü nedir?](#what-is-the-ga-version-of-the-provisioning-agent)
  * [My sağlama Aracısı sürümü olduğunu nasıl öğrenebilirim?](#how-do-i-know-the-version-of-my-provisioning-agent)
  * [Microsoft, sağlama aracı güncelleştirmeleri otomatik olarak gönderilmesi mu?](#does-microsoft-automatically-push-provisioning-agent-updates)
  * [AAD Connect çalıştıran sunucuda sağlama Aracısı'nı yükleyebilir miyim?](#can-i-install-the-provisioning-agent-on-the-same-server-running-aad-connect)
  * [Giden HTTP iletişimi için bir proxy sunucusu kullanacak şekilde sağlama aracı nasıl yapılandırabilirim?](#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication)
  * [Sağlama Aracısı Azure AD kiracınız ile iletişim kuramıyor ve güvenlik duvarı olmadığından, aracı tarafından gerekli bağlantı noktaları engelliyor nasıl emin olabilirim?](#how-do-i-ensure-that-the-provisioning-agent-is-able-to-communicate-with-the-azure-ad-tenant-and-no-firewalls-are-blocking-ports-required-by-the-agent)
  * [Hazırlama aracı ile ilişkili etki alanı XML'deki nasıl kaydedebilirim?](#how-do-i-de-register-the-domain-associated-with-my-provisioning-agent)
  * [Sağlama Aracısı'nı nasıl kaldırabilirim?](#how-do-i-uninstall-the-provisioning-agent)
  
* **Workday soruların AD özniteliği eşleme ve yapılandırma**
  * [Yedekleme veya nasıl bir çalışma kopyası Workday eşleme özniteliği sağlama ve şema dışarı?](#how-do-i-back-up-or-export-a-working-copy-of-my-workday-provisioning-attribute-mapping-and-schema)
  * [Özel öznitelikler Workday ve Active Directory sahibim. Çözüm my özel öznitelikleri ile çalışmaya nasıl yapılandırabilirim?](#i-have-custom-attributes-in-workday-and-active-directory-how-do-i-configure-the-solution-to-work-with-my-custom-attributes)
  * [Active Directory ayıklayıp sorgulayabilecek kullanıcının fotoğrafı sağlayabilirim?](#can-i-provision-users-photo-from-workday-to-active-directory)
  * [Cep telefonu numaralarını ayıklayıp sorgulayabilecek genel kullanım için kullanıcı onayı göre nasıl eşitliyor musunuz?](#how-do-i-sync-mobile-numbers-from-workday-based-on-user-consent-for-public-usage)
  * [Nasıl ben AD kullanıcının ülke/bölüm/Şehir öznitelikleri ve tanıtıcı bölgesel farklarını tabanlı görünen adları biçimlendirme?](#how-do-i-format-display-names-in-ad-based-on-the-users-departmentcountrycity-attributes-and-handle-regional-variances)
  * [SelectUniqueValue samAccountName özniteliği için benzersiz değerler oluşturmak için nasıl kullanabilirim?](#how-can-i-use-selectuniquevalue-to-generate-unique-values-for-samaccountname-attribute)
  * [Nasıl aksanlı karakterleri kaldırın ve bunları normal İngilizce harfler dönüştürün?](#how-do-i-remove-characters-with-diacritics-and-convert-them-into-normal-english-alphabets)

### <a name="solution-capability-questions"></a>Çözüm özelliği sorular

#### <a name="when-processing-a-new-hire-from-workday-how-does-the-solution-set-the-password-for-the-new-user-account-in-active-directory"></a>Yeni bir işe ayıklayıp sorgulayabilecek işlerken nasıl çözüme yeni kullanıcı hesabının parolasını Active Directory'de ayarlar?

Sağlama aracısı için bir istek alır şirket içinde yeni bir AD hesabı oluşturduğunuzda otomatik olarak AD sunucusu tarafından tanımlanan parola karmaşıklık gereksinimlerini karşılamak için tasarlanan karmaşık rastgele bir parola oluşturur ve bu kullanıcı nesnesindeki ayarlar. Bu parola herhangi bir yere günlüğe kaydedilmez.

#### <a name="does-the-solution-support-sending-email-notifications-after-provisioning-operations-complete"></a>Çözümü, e-posta bildirimleri gönderme işlemleri tam sağladıktan sonra destekliyor mu?

Hayır, sağlama işlemleri tamamlamak geçerli sürümde desteklenmiyor sonra e-posta bildirimleri gönderme.

#### <a name="how-do-i-manage-delivery-of-passwords-for-new-hires-and-securely-provide-a-mechanism-to-reset-their-password"></a>Nasıl teslim parolaların yeni katılanlarla ilgili yönetmek ve güvenli bir şekilde kullanıcının parolasını sıfırlamak için bir mekanizma sağlar?

Yeni AD hesabı sağlama yer alan son adımların kullanıcının AD hesabına atanmış bir geçici parola teslimini biridir. Çoğu kurum parola yeni işe alım/contingent çalışana sonra hands kullanıcının Yöneticisi,'için geçici parolayı gerçekleştirirken geleneksel yaklaşımlarını kullanmaya devam edebilirsiniz. Bu işlem bir iç güvenlik kusurdur ve Azure AD özelliklerini kullanarak daha iyi bir yaklaşım uygulamak kullanılabilecek bir seçenek yoktur.

İşe alma işleminin bir parçası olarak ik takımlar genellikle arka plan denetimini çalıştırın ve yeni işe alım cep telefonu numarasını Kıdemli. Workday AD kullanıcı sağlama tümleştirme ile piyasaya çıkma Self Servis parola sıfırlama özelliği için günlük 1 kullanıcı ve bu olgu üzerinde oluşturabilirsiniz. Bu ad ayıklayıp sorgulayabilecek yeni işe alım "Mobile Number" özniteliğini yayma ve ardından AD'den Azure AD'ye AAD Connect kullanılarak gerçekleştirilir. "Mobile Number" Azure AD'de mevcut olduğunda etkinleştirebilirsiniz [Self Servis parola sıfırlama (SSPR)](../authentication/howto-sspr-authenticationdata.md) kullanıcı hesabı için bu nedenle üzerinde günde 1, yeni alım kullanabilir kayıtlı ve doğrulanmış cep telefonu numarası kimlik doğrulaması için.

#### <a name="does-the-solution-cache-workday-user-profiles-in-the-azure-ad-cloud-or-at-the-provisioning-agent-layer"></a>Çözüm, Workday kullanıcı profillerini Azure AD bulutta veya sağlama aracı katmanında önbelleğe almaz?

Hayır, kullanıcı profillerini bir önbellek çözümü korumaz. Azure AD sağlama hizmeti, yalnızca bir veri Workday'den verilerin okunmasını ve hedef Active Directory veya Azure AD'ye yazmaya işlemci olarak görev yapar. Bölümüne bakın [kişisel verileri yönetme](#managing-personal-data) kullanıcı gizlilik ve veri saklama için ilgili ayrıntılar için.

#### <a name="does-the-solution-support-assigning-on-premises-ad-groups-to-the-user"></a>Mu çözüm atama desteği şirket içi AD kullanıcı gruplarına?

Bu işlevsellik şu anda desteklenmiyor. Önerilen geçici çözüm, Azure AD Graph API uç nokta için Denetim günlüğü verilerini sorgulayan bir PowerShell Betiği dağıtmak ve bu, Grup ataması gibi senaryoları tetiklemek için kullanmaktır. Bu PowerShell Betiği, bir Görev Zamanlayıcı'ya bağlı ve sağlama Aracısı çalıştıran kutusundaki dağıtılır.  

#### <a name="which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles"></a>Hangi Workday API'leri, sorgu ve güncelleştirme Workday'deki çalışan profillerine çözüm kullanıyor mu?

Çözüm, şu anda aşağıdaki Workday API'leri kullanır:

* Çalışan bilgileri getiriliyor için Get_Workers (v21.1)
* İş e-posta geri yazma özelliğinin Maintain_Contact_Information (v26.1)
* Kullanıcı geri yazma özelliğinin Update_Workday_Account (v31.2)

#### <a name="can-i-configure-my-workday-hcm-tenant-with-two-azure-ad-tenants"></a>İki Azure AD kiracılarıyla paylaştığında Workday HCM kiracımı yapılandırabilir miyim?

Evet, bu yapılandırma desteklenir. Bu senaryo yapılandırmak için üst düzey adımlar şunlardır:

* Sağlama Aracısı #1 dağıtma ve Azure AD kiracısı #1 ile kaydedin.
* Sağlama Aracısı #2 dağıtma ve Azure AD kiracısı #2'ile kaydedin.
* "Alt aracı her sağlama yönetecek etki alanları üzerinde" bağlı olarak, her bir aracı bir etki alanı ile yapılandırın. Bir aracı birden çok etki alanı başa çıkabilir.
* Azure portalında Workday AD kullanıcı sağlama uygulamada her Kiracı için Kurulum ve ile ilgili etki alanlarını yapılandırın.

#### <a name="why-workday-to-azure-ad-user-provisioning-app-is-not-supported-if-we-have-deployed-azure-ad-connect"></a>Neden Azure AD Connect biz dağıttıysanız, "Azure AD iş günü" kullanıcı uygulama sağlama desteklenmez?

Azure AD (burada, bulut karışık + şirket kullanıcıları) karma modda kullanıldığında, "yetki kaynağı" açık bir tanımı olması önemlidir. Karma senaryolar genelde Azure AD Connect dağıtımının gerektirir. Azure AD Connect dağıtıldığında, yetki kaynağı AD, şirket içi. Workday karışımı için Azure AD Bağlayıcısı ile tanışın Workday öznitelik değerleri Azure AD Connect tarafından ayarlanan değerlerle potansiyel olarak nerede üzerine bir durum neden olabilir. Azure AD Connect etkinleştirildiğinde bu nedenle "Azure AD'ye Workday" sağlama uygulamasının kullanımına desteklenmiyor. Bu gibi durumlarda, biz "Workday AD kullanıcıları almak için uygulama sağlama kullanıcıya" kullanmanızı öneririz. şirket içi AD ve ardından bunları Azure AD Connect kullanarak Azure AD ile eşitleme.

#### <a name="how-do-i-suggest-improvements-or-request-new-features-related-to-workday-and-azure-ad-integration"></a>Nasıl gelişmeler Öner veya Workday ve Azure AD tümleştirmesiyle ilgili yeni özellikler talep?

Geri bildiriminiz çok değerli aynıdır bize geliştirmeleri ve sonraki sürümlerde yönünü ayarlama yardımcı olur. Tüm geri Hoş Geldiniz ve fikriniz veya geliştirme önerinizi gönderin geçirmenizi [Azure ad geri bildirim Forumu](https://feedback.azure.com/forums/169401-azure-active-directory). Workday tümleştirmesiyle ilgili belirli geribildirim için kategoriyi seçin *SaaS uygulamalarına* ve anahtar sözcükleri kullanarak arama *Workday* Workday için ilgili mevcut geri bildirim bulunamadı.

![UserVoice SaaS Apps](media/workday-inbound-tutorial/uservoice_saas_apps.png)

![UserVoice Workday](media/workday-inbound-tutorial/uservoice_workday_feedback.png)

Yeni bir fikir önerme, başka birisi zaten benzer bir özellik önerdiği olmadığını görmek için lütfen denetleyin. Bu durumda, özellik veya geliştirme isteği en fazla oy verebilirsiniz. Destek Idea için göstermek ve nasıl özelliği çok sizin için yararlı olacak göstermek için belirli kullanım Örneğinize bir açıklama da bırakabilirsiniz.

### <a name="provisioning-agent-questions"></a>Sağlama Aracısı sorular

#### <a name="what-is-the-ga-version-of-the-provisioning-agent"></a>Sağlama Aracısı'nın GA sürümü nedir?

* Sağlama Aracısı'nın GA sürümünü 1.1.30 olduğu ve üstü.
* Küçüktür 1.1.30 aracınızın sürümü, genel önizleme sürümünü çalıştırıyorsanız ve aracıyı barındıran sunucuda .NET 4.7.1 varsa, otomatik olarak genel kullanım sürümü için güncelleştirilecek çalışma zamanı.
  * Yapabilecekleriniz [.NET sürümü denetlemek](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) sunucunuzda yüklü. Sunucu .NET 4.7.1 çalışmıyorsa yapabilecekleriniz [.NET 4.7.1 yükleyip](https://support.microsoft.com/help/4033342/the-net-framework-4-7-1-offline-installer-for-windows). .NET 4.7.1 yükledikten sonra sağlama aracınızı GA sürümü için otomatik olarak güncelleştirilir.

#### <a name="how-do-i-know-the-version-of-my-provisioning-agent"></a>My sağlama Aracısı sürümü olduğunu nasıl öğrenebilirim?

* Sağlama Aracısı yüklendiği Windows server için oturum açın.
* Git **Denetim Masası** -> **bir programı kaldırın veya değiştirin** menüsü
* Girişe karşılık gelen sürüm arayın **Microsoft Azure AD Connect aracı sağlama**

  ![Azure portal](./media/workday-inbound-tutorial/pa_version.png)

#### <a name="does-microsoft-automatically-push-provisioning-agent-updates"></a>Microsoft, sağlama aracı güncelleştirmeleri otomatik olarak gönderilmesi mu?

Evet, Microsoft, sağlama aracı otomatik olarak güncelleştirir. Windows hizmetini durdurarak otomatik güncelleştirmeler devre dışı bırakabilirsiniz **Microsoft Azure AD Connect aracı güncelleştirici**.

#### <a name="can-i-install-the-provisioning-agent-on-the-same-server-running-aad-connect"></a>AAD Connect çalıştıran sunucuda sağlama Aracısı'nı yükleyebilir miyim?

Evet, aracı sağlama AAD Connect çalıştıran sunucuya yükleyebilirsiniz.

#### <a name="at-the-time-of-configuration-the-provisioning-agent-prompts-for-azure-ad-admin-credentials-does-the-agent-store-the-credentials-locally-on-the-server"></a>Yapılandırma sırasında aracı sağlama için Azure AD yönetici kimlik bilgilerini ister. Aracı kimlik bilgilerini sunucu üzerinde yerel olarak depoluyor mu?

Yapılandırma sırasında sağlama Aracısı yalnızca Azure AD kiracınıza bağlamak Azure AD yönetici kimlik bilgilerini ister. Bu kimlik bilgilerini yerel olarak sunucuda depolamaz. Ancak bağlanmak için kullanılan kimlik bilgileri saklamanız *şirket içi Active Directory etki alanı* yerel bir Windows parola Kasası'nda.

#### <a name="how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication"></a>Giden HTTP iletişimi için bir proxy sunucusu kullanacak şekilde sağlama aracı nasıl yapılandırabilirim?

Sağlama Aracısı, giden bağlantı proxy'si kullanımını destekler. Aracı yapılandırma dosyasını düzenleyerek yapılandırabilirsiniz **C:\Program Files\Microsoft Azure AD Connect Agent\AADConnectProvisioningAgent.exe.config sağlama**. Aşağıdaki satırları, kapatma hemen önce dosyanın sonuna doğru ekleyin `</configuration>` etiketi.
Değişkenler [proxy sunucusu] ve [proxy bağlantı noktası], proxy sunucusu adıyla değiştirin ve değerler bağlantı noktası.

```xml
    <system.net>
          <defaultProxy enabled="true" useDefaultCredentials="true">
             <proxy
                usesystemdefault="true"
                proxyaddress="http://[proxy-server]:[proxy-port]"
                bypassonlocal="true"
             />
         </defaultProxy>
    </system.net>
```

#### <a name="how-do-i-ensure-that-the-provisioning-agent-is-able-to-communicate-with-the-azure-ad-tenant-and-no-firewalls-are-blocking-ports-required-by-the-agent"></a>Sağlama Aracısı Azure AD kiracınız ile iletişim kuramıyor ve güvenlik duvarı olmadığından, aracı tarafından gerekli bağlantı noktaları engelliyor nasıl emin olabilirim?

Tüm gerekli bağlantı noktalarını açmak açarak sahip olup olmadığını da denetleyebilirsiniz [Bağlayıcısı bağlantı noktaları Test aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/) şirket içi ağınızdan. Daha fazla yeşil onay işaretleri koyacağız büyük dayanıklılık anlamına gelir.

Aracı size doğru sonuçları emin olmak için emin olun:

* Sağlama Aracısı'nı yüklediğiniz sunucunun bir tarayıcı aracını açın.
* Herhangi bir proxy veya sağlama aracınızı uygun güvenlik duvarı de bu sayfaya uygulandığından emin olun. Internet Explorer'da bu yapılabilir giderek **ayarlar -> Internet Seçenekleri -> bağlantı LAN Ayarları ->**. Bu sayfada, alanın "Kullanımı bir Proxy sunucusu için bilgisayarınızı LAN" görürsünüz. Bu kutuyu seçin ve "Address" alanına proxy adresi yerleştirin.

#### <a name="can-one-provisioning-agent-be-configured-to-provision-multiple-ad-domains"></a>Bir aracı sağlama, birden çok AD etki alanı sağlamak için yapılandırılabilir mi?

Evet, bir aracı sağlama aracı görebilmesi için ilgili etki alanı denetleyicilerine sahip olduğu sürece, birden çok AD etki alanı işlemek için yapılandırılabilir. Microsoft, bir Grup 3 yüksek kullanılabilirlik sağlamak ve başarısız destek sağlamak için aracıları aynı AD etki alanı kümesini hizmet sağlama ayarlama önerir.

#### <a name="how-do-i-de-register-the-domain-associated-with-my-provisioning-agent"></a>Hazırlama aracı ile ilişkili etki alanı XML'deki nasıl kaydedebilirim?

* Azure Portal'dan alma *Kiracı kimliği* Azure AD kiracınızın.
* Sağlama Aracısı'nı çalıştıran Windows server için oturum açın.
* Windows Yönetici olarak PowerShell'i açın.
* Kayıt komut dosyaları içeren dizine değiştirin ve değiştirerek aşağıdaki komutları çalıştırın \[Kiracı kimliği\] Kiracı kimliğinizi değeriyle parametre

  ```powershell
  cd “C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\RegistrationPowershell\Modules\PSModulesFolder”
  Import-Module "C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\RegistrationPowershell\Modules\PSModulesFolder\AppProxyPSModule.psd1"
  Get-PublishedResources -TenantId "[tenant ID]"
  ```

* Görünen – aracılar listesinden Kopyala "ID" değerini alan bu kaynağı ayarlanmış *resourceName* AD etki alanı adınızı eşittir.
* Kimlik değerini bu komutuna yapıştırın ve komutu PowerShell'de yürütün.

  ```powershell
  Remove-PublishedResource -ResourceId "[resource ID]" -TenantId "[tenant ID]"
  ```

* Aracı Yapılandırma Sihirbazı'nı yeniden çalıştırın.
* Bu etki alanına daha önce atanmış olan tüm diğer aracıları, yapılandırılması gerekir.

#### <a name="how-do-i-uninstall-the-provisioning-agent"></a>Sağlama Aracısı'nı nasıl kaldırabilirim?

* Sağlama Aracısı yüklendiği Windows server için oturum açın.
* Git **Denetim Masası** -> **bir programı kaldırın veya değiştirin** menüsü
* Aşağıdaki Program Kaldır:
  * Microsoft Azure AD Connect sağlama Aracısı
  * Microsoft Azure AD Connect aracı güncelleştirici
  * Microsoft Azure AD Connect aracı paketi sağlama

### <a name="workday-to-ad-attribute-mapping-and-configuration-questions"></a>Workday soruların AD özniteliği eşleme ve yapılandırma

#### <a name="how-do-i-back-up-or-export-a-working-copy-of-my-workday-provisioning-attribute-mapping-and-schema"></a>Yedekleme veya nasıl bir çalışma kopyası Workday eşleme özniteliği sağlama ve şema dışarı?

Microsoft Graph API Workday'den kullanıcı hazırlama yapılandırmanızı dışarı aktarmak için kullanabilirsiniz. Bu bölümdeki adımları başvurmak [içeri ve dışarı aktarma Workday'den kullanıcı hazırlama özniteliği eşleme yapılandırmanızı](#exporting-and-importing-your-configuration) Ayrıntılar için.

#### <a name="i-have-custom-attributes-in-workday-and-active-directory-how-do-i-configure-the-solution-to-work-with-my-custom-attributes"></a>Özel öznitelikler Workday ve Active Directory sahibim. Çözüm my özel öznitelikleri ile çalışmaya nasıl yapılandırabilirim?

Çözüme özel Workday ve Active Directory öznitelikleri destekler. Eşleme Şeması için özel öznitelikler eklemek için açık **öznitelik eşlemesi** dikey penceresinde ve bölümü genişletin aşağı kaydırarak **Gelişmiş Seçenekleri Göster**. 

![Öznitelik Listesini Düzenle](./media/workday-inbound-tutorial/wd_edit_attr_list.png)

Özel, Workday öznitelikler eklemek için Ek Yardım seçeneğini *Workday için öznitelik listesini düzenle* ve kendi özel AD öznitelikleri eklemek için Ek Yardım seçeneğini *şirket içi Active Directory için öznitelik listesini düzenle*.

Ayrıca bkz:

* [Workday kullanıcı özniteliklerinin listesi özelleştirme](#customizing-the-list-of-workday-user-attributes)

#### <a name="how-do-i-configure-the-solution-to-only-update-attributes-in-ad-based-on-workday-changes-and-not-create-any-new-ad-accounts"></a>Workday değişikliklere dayalı AD öznitelikleri yalnızca güncelleştirmek ve herhangi bir yeni AD hesabı oluşturmamayı çözümün nasıl yapılandırabilirim?

Bu yapılandırma, ayarlayarak gerçekleştirilebilir **hedef nesne eylemleri** içinde **öznitelik eşlemelerini** aşağıda gösterildiği gibi dikey penceresinde:

![Eylemi güncelleştir](./media/workday-inbound-tutorial/wd_target_update_only.png)

Workday'den AD'ye akışına yalnızca güncelleştirme işlemleri için "Güncelleştir" onay kutusunu seçin. 

#### <a name="can-i-provision-users-photo-from-workday-to-active-directory"></a>Active Directory ayıklayıp sorgulayabilecek kullanıcının fotoğrafı sağlayabilirim?

İkili öznitelikler gibi ayarlama çözümü şu anda desteklemiyor *thumbnailPhoto* ve *jpegPhoto* Active Directory'de.

#### <a name="how-do-i-sync-mobile-numbers-from-workday-based-on-user-consent-for-public-usage"></a>Cep telefonu numaralarını ayıklayıp sorgulayabilecek genel kullanım için kullanıcı onayı göre nasıl eşitliyor musunuz?

* Workday uygulamanızın sağlama "Hazırlama" dikey penceresinde gidin.
* Öznitelik eşlemeleri üzerinde tıklayın 
* Altında **eşlemeleri**seçin **şirket içi Active Directory eşitleme Workday'deki çalışanları** (veya **eşitleme Workday'deki çalışanları Azure AD'ye**).
* Öznitelik eşlemeleri sayfasında, aşağı kaydırın ve "Gelişmiş Seçenekleri Göster" onay kutusunu işaretleyin.  Tıklayarak **Workday için öznitelik listesini düzenle**
* Açılan dikey pencerede "Mobile" özniteliğini bulun ve düzenleyebileceğiniz şekilde satırına tıklayın **API ifadesi** ![mobil GDPR](./media/workday-inbound-tutorial/mobile_gdpr.png)

* Değiştirin **API ifadesi** aşağıdaki yeni ifade ile iş cep telefonu numarası yalnızca "Genel kullanım bayrağı" Workday'de "True" olarak ayarlanmışsa, alan.

    ```
     wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Contact_Data/wd:Phone_Data[translate(string(wd:Phone_Device_Type_Reference/@wd:Descriptor),'abcdefghijklmnopqrstuvwxyz','ABCDEFGHIJKLMNOPQRSTUVWXYZ')='MOBILE' and translate(string(wd:Usage_Data/wd:Type_Data/wd:Type_Reference/@wd:Descriptor),'abcdefghijklmnopqrstuvwxyz','ABCDEFGHIJKLMNOPQRSTUVWXYZ')='WORK' and string(wd:Usage_Data/@wd:Public)='1']/@wd:Formatted_Phone
    ```

* Öznitelik listesi kaydedin.
* Öznitelik eşlemesi kaydedin.
* Geçerli durumu temizleyin ve tam eşitleme yeniden başlatın.

#### <a name="how-do-i-format-display-names-in-ad-based-on-the-users-departmentcountrycity-attributes-and-handle-regional-variances"></a>Nasıl ben AD kullanıcının ülke/bölüm/Şehir öznitelikleri ve tanıtıcı bölgesel farklarını tabanlı görünen adları biçimlendirme?

Yapılandırmak için ortak bir gereksinimdir *displayName* özniteliği AD'de kullanıcının departmanı ve ülke/bölge hakkında bilgi de sağlar. İçin Örneğin John Smith, ABD'de pazarlama departmanındaki çalışırsa, himself isteyebilirsiniz *displayName* olarak görünmesi *Smith, John (pazarlama-US)*.

İşte oluşturmak için bu gereksinimleri nasıl işleyebileceğini *CN* veya *displayName* şirket, departman, şehir ve ülke/bölge gibi öznitelikleri eklenecek.

* Her iş günü özniteliği içinde yapılandırılabilir bir temel alınan API XPATH ifadesi kullanarak alınır **-> Gelişmiş bölümü eşleme özniteliği için Workday öznitelik listesini Düzenle ->**. Workday varsayılan API XPATH ifadesi işte *PreferredFirstName*, *PreferredLastName*, *şirket* ve *SupervisoryOrganization* öznitelikleri.

     | Workday özniteliği | API XPATH ifadesi |
     | ----------------- | -------------------- |
     | PreferredFirstName | wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Name_Data/wd:Preferred_Name_Data/wd:Name_Detail_Data/wd:First_Name/text() |
     | PreferredLastName | wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Name_Data/wd:Preferred_Name_Data/wd:Name_Detail_Data/wd:Last_Name/text() |
     | Şirket | wd:Worker/wd:Worker_Data/wd:Organization_Data/wd:Worker_Organization_Data[wd:Organization_Data/wd:Organization_Type_Reference/wd:ID[@wd:type='Organization_Type_ID']='Company']/wd:Organization_Reference/@wd:Descriptor |
     | SupervisoryOrganization | WD:Worker / wd:Worker_Data / wd:Organization_Data / wd:Worker_Organization_Data / wd:Organization_Data [wd:Organization_Type_Reference / wd:ID [@wd:type= 'Organization_Type_ID'] 'Denetim' =] /wd:Organization_Name/text() |
  
   Workday takımınızla yukarıdaki API ifadesi Workday kiracısı yapılandırmanız için geçerli olduğunu doğrulayın. Gerekirse, bunları bölümünde açıklandığı gibi düzenleyebilirsiniz, [Workday kullanıcı özniteliklerinin listesi özelleştirme](#customizing-the-list-of-workday-user-attributes).

* Benzer şekilde Workday'de mevcut ülke bilgileri aşağıdaki XPATH kullanarak alınır: *wd:Worker / wd:Worker_Data / wd:Employment_Data / wd:Position_Data / wd:Business_Site_Summary_Data / wd:Address_Data / wd:Country_Reference*

     Workday öznitelik listesi bölümünde kullanılabilir 5 ülke ilgili öznitelikleri vardır.

     | Workday özniteliği | API XPATH ifadesi |
     | ----------------- | -------------------- |
     | CountryReference | WD:Worker / wd:Worker_Data / wd:Employment_Data / wd:Position_Data / wd:Business_Site_Summary_Data / wd:Address_Data / wd:Country_Reference / wd:ID [@wd:type='ISO_3166-1_Alpha-3_Code']/text() |
     | CountryReferenceFriendly | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference/@wd:Descriptor |
     | CountryReferenceNumeric | WD:Worker / wd:Worker_Data / wd:Employment_Data / wd:Position_Data / wd:Business_Site_Summary_Data / wd:Address_Data / wd:Country_Reference / wd:ID [@wd:type='ISO_3166-1_Numeric-3_Code']/text() |
     | CountryReferenceTwoLetter | WD:Worker / wd:Worker_Data / wd:Employment_Data / wd:Position_Data / wd:Business_Site_Summary_Data / wd:Address_Data / wd:Country_Reference / wd:ID [@wd:type='ISO_3166-1_Alpha-2_Code']/text() |
     | CountryRegionReference | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Region_Reference/@wd:Descriptor |

  Workday takımınızla yukarıdaki API ifadeleri Workday kiracısı yapılandırmanız için geçerli olduğundan emin olun. Gerekirse, bunları bölümünde açıklandığı gibi düzenleyebilirsiniz, [Workday kullanıcı özniteliklerinin listesi özelleştirme](#customizing-the-list-of-workday-user-attributes).

* Doğru öznitelik eşlemesi ifadeyi oluşturun, hangi Workday özniteliği "yetkili olarak" tanımlamak için kullanıcı adı, son adı, ülke/bölge ve departman temsil eder. Öznitelikler varsayalım *PreferredFirstName*, *PreferredLastName*, *CountryReferenceTwoLetter* ve *SupervisoryOrganization* sırasıyla. Bu AD için bir ifade oluşturmak için kullanabileceğiniz *displayName* gibi bir görünen ad gibi almak için öznitelik *Smith, John (pazarlama-US)*.

    ```
     Append(Join(", ",[PreferredLastName],[PreferredFirstName]), Join(""," (",[SupervisoryOrganization],"-",[CountryReferenceTwoLetter],")"))
    ```
    Sağ ifade oluşturduktan sonra öznitelik eşlemelerini tabloyu düzenlemek ve değiştirme *displayName* aşağıda gösterildiği gibi öznitelik eşlemesi:   ![DisplayName eşleme](./media/workday-inbound-tutorial/wd_displayname_map.png)

* Yukarıdaki örnek genişletme, şimdi say Şehir adları Workday'den gelen toplu değerlere dönüştürün ve oluşturmak için kullanmak istediğiniz görünen adlar gibi *Smith, John (CHI)* veya *Doe, Jane (NYC)*, Bu sonuç, Workday ile bir Switch ifadesi kullanarak gerçekleştirilebilir sonra *belediye* determinant değişkeni olarak özniteliği.

     ```
    Switch
    (
      [Municipality],
      Join(", ", [PreferredLastName], [PreferredFirstName]),  
           "Chicago", Append(Join(", ",[PreferredLastName], [PreferredFirstName]), "(CHI)"),
           "New York", Append(Join(", ",[PreferredLastName], [PreferredFirstName]), "(NYC)"),
           "Phoenix", Append(Join(", ",[PreferredLastName], [PreferredFirstName]), "(PHX)")
    )
     ```
    Ayrıca bkz:
  * [Geçiş işlevi sözdizimi](../manage-apps/functions-for-customizing-application-data.md#switch)
  * [İşlev sözdizimi katılın](../manage-apps/functions-for-customizing-application-data.md#join)
  * [İşlev sözdizimi ekleme](../manage-apps/functions-for-customizing-application-data.md#append)

#### <a name="how-can-i-use-selectuniquevalue-to-generate-unique-values-for-samaccountname-attribute"></a>SelectUniqueValue samAccountName özniteliği için benzersiz değerler oluşturmak için nasıl kullanabilirim?

Benzersiz değerleri oluşturmak istediğinizi varsayalım *samAccountName* bir birleşimi kullanılarak özniteliği *FirstName* ve *LastName* Workday öznitelikleri. Aşağıda verilen ile başlayan bir ifadesidir:

```
SelectUniqueValue(
    Replace(Mid(Replace(NormalizeDiacritics(StripSpaces(Join("",  Mid([FirstName],1,1), [LastName]))), , "([\\/\\\\\\[\\]\\:\\;\\|\\=\\,\\+\\*\\?\\<\\>])", , "", , ), 1, 20), , "(\\.)*$", , "", , ),
    Replace(Mid(Replace(NormalizeDiacritics(StripSpaces(Join("",  Mid([FirstName],1,2), [LastName]))), , "([\\/\\\\\\[\\]\\:\\;\\|\\=\\,\\+\\*\\?\\<\\>])", , "", , ), 1, 20), , "(\\.)*$", , "", , ),
    Replace(Mid(Replace(NormalizeDiacritics(StripSpaces(Join("",  Mid([FirstName],1,3), [LastName]))), , "([\\/\\\\\\[\\]\\:\\;\\|\\=\\,\\+\\*\\?\\<\\>])", , "", , ), 1, 20), , "(\\.)*$", , "", , )
)
```

Yukarıdaki ifadeyi nasıl çalışır: Kullanıcı, John Smith'in ise, ilk JSmith zaten varsa, JoSmith, oluşturduktan sonra JSmith, oluşturmak çalışır, varsa, bu JohSmith oluşturur. Oluşturulan değeri uzunluğu kısıtlaması ve ilişkili özel karakterler kısıtlama karşıladığını ifade ayrıca sağlar *samAccountName*.

Ayrıca bkz:

* [Mid işlev sözdizimi](../manage-apps/functions-for-customizing-application-data.md#mid)
* [İşlev sözdizimi değiştirin](../manage-apps/functions-for-customizing-application-data.md#replace)
* [SelectUniqueValue işlev sözdizimi](../manage-apps/functions-for-customizing-application-data.md#selectuniquevalue)

#### <a name="how-do-i-remove-characters-with-diacritics-and-convert-them-into-normal-english-alphabets"></a>Nasıl aksanlı karakterleri kaldırın ve bunları normal İngilizce harfler dönüştürün?

İşlevini [NormalizeDiacritics](../manage-apps/functions-for-customizing-application-data.md#normalizediacritics) ad ve Soyadı kullanıcının özel karakterler kaldırmak için CN ve e-posta adresi oluşturulurken kullanıcı için değer.

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

Bu bölümde, Azure AD denetim günlüklerini kullanarak, Workday Tümleştirmesi ile sağlama gidermeye ilişkin özel yönergeler verir ve Windows Server Olay Görüntüleyicisi günlüklerinde sağlar. Genel sorun giderme adımlarını üstünde oluşturur ve kavramları yakalanan içinde [Öğreticisi: Raporlama hesabı otomatik kullanıcı hazırlama](../manage-apps/check-status-user-account-provisioning.md)

Bu bölümde aşağıdaki sorun giderme yönlerini kapsar:

* [Sorun giderme aracı için Windows Olay Görüntüleyicisi'ni ayarlama](#setting-up-windows-event-viewer-for-agent-troubleshooting)
* [Hizmet sorun giderme için Azure portalında denetim günlükleri ayarlama](#setting-up-azure-portal-audit-logs-for-service-troubleshooting)
* [AD kullanıcı hesabı için anlama günlükleri oluşturma işlemleri](#understanding-logs-for-ad-user-account-create-operations)
* [Operations Manager için günlükleri anlama güncelleştir](#understanding-logs-for-manager-update-operations)
* [Yaygın olarak çözme hatalarla karşılaştı](#resolving-commonly-encountered-errors)

### <a name="setting-up-windows-event-viewer-for-agent-troubleshooting"></a>Sorun giderme aracı için Windows Olay Görüntüleyicisi'ni ayarlama

* Sağlama Aracısı dağıtıldığı Windows Server makinesinde oturum açın
* Açık **Windows Server Olay Görüntüleyicisi'ni** masaüstü uygulaması.
* Seçin **Windows Günlükleri > Uygulama**.
* Kullanım **geçerli günlüğü Filtrele...** Kaynak oturumu tüm olayları görüntüleme seçeneği **AAD. Connect.ProvisioningAgent** ve aşağıda gösterildiği gibi "-5" filtreyi belirterek olay kimliği "5" olan olaylar hariç tutun.

  ![Windows Olay Görüntüleyicisi](media/workday-inbound-tutorial/wd_event_viewer_01.png))

* Tıklayın **Tamam** ve sonuç görünümünde göre sıralama **tarih ve saat** sütun.

### <a name="setting-up-azure-portal-audit-logs-for-service-troubleshooting"></a>Hizmet sorun giderme için Azure portalında denetim günlükleri ayarlama

* Başlatma [Azure portalında](https://portal.azure.com)gidin **denetim günlükleri** uygulama sağlama İş gününüzün bölümü.
* Kullanım **sütunları** düğme görünümünde (tarih, etkinlik, durumu, durum nedeni) yalnızca aşağıdaki sütunları görüntülemek için Denetim günlükleri sayfasında. Bu yapılandırma, sorun giderme için ilgili veri odaklanmasını sağlar.

  ![Denetim günlüğü sütunları](media/workday-inbound-tutorial/wd_audit_logs_00.png)

* Kullanım **hedef** ve **tarih aralığı** sorgu görünüme filtre uygulamak için parametreleri. 
  * Ayarlama **hedef** Workday'deki çalışan nesne sorgu parametresi "Çalışan kimliği" veya "Çalışan kimliği".
  * Ayarlama **tarih aralığı** üzerinde incelemek istediğiniz hatalar veya sorunlar için bir uygun zaman aralığı için sağlama ile.

  ![Denetim günlüğü filtreleri](media/workday-inbound-tutorial/wd_audit_logs_01.png)

### <a name="understanding-logs-for-ad-user-account-create-operations"></a>AD kullanıcı hesabı için anlama günlükleri oluşturma işlemleri

Ne zaman yeni bir işe workday'deki algılandığında (diyelim ki çalışan kimliği ile *21023*), aşağıda açıklandığı gibi Azure AD hizmeti çalışan ve işlem yeni bir AD kullanıcı hesabı oluşturma denemesi sağlama 4 günlük kaydı oluşturur:

  [![Denetim günlüğü ops oluşturma](media/workday-inbound-tutorial/wd_audit_logs_02.png)](media/workday-inbound-tutorial/wd_audit_logs_02.png#lightbox)

Herhangi bir denetim günlüğü kayıtlarını tıkladığınızda **Etkinlik ayrıntıları** sayfası açılır. İşte **Etkinlik ayrıntıları** sayfası, her günlük kayıt türü için görüntüler.

* **Workday alma** kaydı: Bu günlük kaydı Workday'den getirilen çalışan bilgilerini görüntüler. Bilgileri *ek ayrıntılar* Workday'den veri getirme sorunlarını giderme için günlük kaydı bölümü. Bir örnek kayıt, her bir alan yorumlama işaretçilerde birlikte aşağıda gösterilmiştir.

  ```JSON
  ErrorCode : None  // Use the error code captured here to troubleshoot Workday issues
  EventName : EntryImportAdd // For full sync, value is "EntryImportAdd" and for delta sync, value is "EntryImport"
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID (usually the Worker ID or Employee ID field)
  SourceAnchor : a071861412de4c2486eb10e5ae0834c3 // set to the WorkdayID (WID) associated with the record
  ```

* **AD alma** kaydı: Bu günlük kaydı AD'den alınan hesabı bilgileri görüntüler. İlk kullanıcı oluşturma sırasında olmadığından hiçbir AD hesabı *etkinlik durum nedeni* eşleşen kimlik öznitelik değeri olan hesap Active Directory'de bulunamadı gösterir. Bilgileri *ek ayrıntılar* Workday'den veri getirme sorunlarını giderme için günlük kaydı bölümü. Bir örnek kayıt, her bir alan yorumlama işaretçilerde birlikte aşağıda gösterilmiştir.

  ```JSON
  ErrorCode : None // Use the error code captured here to troubleshoot Workday issues
  EventName : EntryImportObjectNotFound // Implies that object was not found in AD
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID
  ```

  Bu AD içeri aktarma işlemi için karşılık gelen Hazırlama aracı günlük kayıtları bulmak için Windows Olay Görüntüleyicisi günlüklerini açın ve kullanmak **Bul...** menü seçeneği kimliği/katılmasını özelliği eşleşen öznitelik değerini içeren günlük girişlerini bulmak için (Bu durumda *21023*).

  ![Bul](media/workday-inbound-tutorial/wd_event_viewer_02.png)

  Girişle arayın *Olay Kimliği = 9*, hangi, LDAP arama aracı tarafından AD hesabı almak için kullanılan filtre sağlayacaktır. Bu benzersiz kullanıcı girişlerini almak için doğru arama filtresi olup olmadığını doğrulayabilirsiniz.

  ![LDAP arama](media/workday-inbound-tutorial/wd_event_viewer_03.png)

  Hemen ardından kayıt *olay kimliği = 2* arama işleminin sonucu yakalar ve herhangi bir sonuç döndürdü.

  ![LDAP sonuçları](media/workday-inbound-tutorial/wd_event_viewer_04.png)

* **Eşitleme kuralı eylemi** kaydı: Bu günlük kaydı sonuçlarını özniteliği eşleme kurallarını görüntüler ve gelen Workday olayı işlemek için gerçekleştirilecek eylemi sağlama yanı sıra kapsamı belirleme filtreleri yapılandırılmış. Bilgileri *ek ayrıntılar* eşitleme eylemi ile ilgili sorunları giderme günlük kaydı bölümü. Bir örnek kayıt, her bir alan yorumlama işaretçilerde birlikte aşağıda gösterilmiştir.

  ```JSON
  ErrorCode : None // Use the error code captured here to troubleshoot sync issues
  EventName : EntrySynchronizationAdd // Implies that the object will be added
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID
  SourceAnchor : a071861412de4c2486eb10e5ae0834c3 // set to the WorkdayID (WID) associated with the profile in Workday
  ```

  Öznitelik eşlemesi ifadelerle sorunları vardır veya gelen Workday veri sorunları varsa (örneğin: boş veya null değer gerekli öznitelikler için), sonra da bu aşamada hatasının ayrıntılarını sağlayan bir hata kodu ile hata gözlemleyeceksiniz.

* **AD dışarı aktarma** kaydı: Bu günlük kaydı AD hesabı oluşturma işlemi işlemde ayarlanan öznitelik değerleri ile birlikte görüntüler. Bilgileri *ek ayrıntılar* hesabı ile ilgili sorunları giderme günlük kaydı bölümünü oluşturma işlemi. Bir örnek kayıt, her bir alan yorumlama işaretçilerde birlikte aşağıda gösterilmiştir. "Ayrıntılar" bölümünde "EntryExportAdd" için "EventName" ayarlandığında, "JoiningProperty" değeri ile eşleşen bir ID özniteliği olarak ayarlanır, "SourceAnchor" için WorkdayID (kaydıyla ilişkili WID) olarak ayarlanır ve "TargetAnchor" ayarlanır ' % s'AD "ObjectGuid" değerini yeni oluşturulan kullanıcı özniteliği. 

  ```JSON
  ErrorCode : None // Use the error code captured here to troubleshoot AD account creation issues
  EventName : EntryExportAdd // Implies that object will be created
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID
  SourceAnchor : a071861412de4c2486eb10e5ae0834c3 // set to the WorkdayID (WID) associated with the profile in Workday
  TargetAnchor : 83f0156c-3222-407e-939c-56677831d525 // set to the value of the AD "objectGuid" attribute of the new user
  ```

  Bu AD dışarı aktarma işlemi için karşılık gelen Hazırlama aracı günlük kayıtları bulmak için Windows Olay Görüntüleyicisi günlüklerini açın ve kullanmak **Bul...** menü seçeneği kimliği/katılmasını özelliği eşleşen öznitelik değerini içeren günlük girişlerini bulmak için (Bu durumda *21023*).  

  Zaman damgası ile dışarı aktarma işlemine karşılık gelen bir HTTP POST kayıt arayın *olay kimliği = 2*. Bu kayıt, sağlama aracıya sağlama hizmeti tarafından gönderilen öznitelik değerlerini içerecektir.

  [![SCIM ekleyin](media/workday-inbound-tutorial/wd_event_viewer_05.png)](media/workday-inbound-tutorial/wd_event_viewer_05.png#lightbox)

  Hemen yukarıdaki etkinliğin AD hesabı işlemi oluşturma yanıtı yakalar başka bir olay olmalıdır. Bu olay AD'de oluşturulan yeni objectGUID döndürür ve sağlama hizmeti TargetAnchor özniteliği olarak ayarlanır.

  [![SCIM ekleyin](media/workday-inbound-tutorial/wd_event_viewer_06.png)](media/workday-inbound-tutorial/wd_event_viewer_06.png#lightbox)

### <a name="understanding-logs-for-manager-update-operations"></a>Operations manager için günlükleri anlama güncelleştir

Yöneticisi öznitelik AD içinde bir başvuru özniteliğidir. Sağlama Hizmeti Yöneticisi'ni öznitelik kullanıcı oluşturma işleminin bir parçası olarak ayarlı değil. Yerine manager özniteliği bir parçası olarak ayarlanmış bir *güncelleştirme* kullanıcı için AD hesabı oluşturulduktan sonra işlem. Yukarıdaki örnekte genişleterek, Workday ve yeni işe alım kişinin Yöneticisi "21451" kimliğiyle çalışan yeni bir işe etkinleştirilirse varsayalım (*21023*) bir AD hesabı zaten sahip. Bu senaryoda, kullanıcı 21451 denetim günlüklerini arama 5 girdilerin gösterir.

  [![Yönetici güncelleştirme](media/workday-inbound-tutorial/wd_audit_logs_03.png)](media/workday-inbound-tutorial/wd_audit_logs_03.png#lightbox)

Oluşturma işlemi biz kullanıcı bir parçası olarak araştırılan olanlar gibi ilk 4 kayıtlardır. Dışarı aktarma Yöneticisi özniteliği güncelleştirme ile ilişkili 5 kaydıdır. Yöneticinin kullanarak gerçekleştirilen AD hesabı yöneticisi güncelleştirme işlemin sonucunu, günlük kaydı görüntüler *objectGUID* özniteliği.

  ```JSON
  // Modified Properties
  Name : manager
  New Value : "83f0156c-3222-407e-939c-56677831d525" // objectGuid of the user 21023

  // Additional Details
  ErrorCode : None // Use the error code captured here to troubleshoot AD account creation issues
  EventName : EntryExportUpdate // Implies that object will be created
  JoiningProperty : 21451 // Value of the Workday attribute that serves as the Matching ID
  SourceAnchor : 9603bf594b9901693f307815bf21870a // WorkdayID of the user
  TargetAnchor : 43b668e7-1d73-401c-a00a-fed14d31a1a8 // objectGuid of the user 21451

  ```

### <a name="resolving-commonly-encountered-errors"></a>Yaygın olarak çözme hatalarla karşılaştı

Bu bölüm, yaygın olarak görülen hatayla Workday'den kullanıcı hazırlama ve nasıl çözümleyeceğiniz kapsar. Hataları şu şekilde gruplanır:

* [Sağlama Aracısı hataları](#provisioning-agent-errors)
* [Bağlantı hataları](#connectivity-errors)
* [AD kullanıcı hesabı oluşturma hataları](#ad-user-account-creation-errors)
* [AD kullanıcı hesabını güncelleştirme hataları](#ad-user-account-update-errors)

#### <a name="provisioning-agent-errors"></a>Sağlama Aracısı hataları

|#|Hata senaryosu |Olası nedenleri|Önerilen çözüm|
|--|---|---|---|
|1.| Hata iletisiyle sağlama Aracısı yüklenirken hata:  *'Microsoft Azure AD Connect aracı sağlama' servis (AADConnectProvisioningAgent) başlatılamadı. Sistemi başlatmak için yeterli ayrıcalıklara sahip olduğunuzu doğrulayın.* | Bu hata genellikle bir etki alanı denetleyicisinde sağlama aracıyı yüklemeye çalıştığınız ve Grup İlkesi hizmetin başlatılmasını engelleyen gösterilir.  Ayrıca, çalışan aracısının önceki bir sürümü varsa ve yeni bir yükleme başlamadan önce kaldırıldıktan değil görülür.| Sağlama Aracısı bir DC olmayan sunucusuna yükleyin. Yeni aracı yüklemeden önce Aracısı'nın önceki sürümlerini kaldırıldığından emin olun.|
|2.| Windows hizmeti 'Microsoft Azure AD Connect aracı sağlama' konusu *başlangıç* belirtin ve geçin değil *çalıştıran* durumu. | Yüklemesinin bir parçası olarak, aracı Sihirbazı'nı yerel bir hesap oluşturur (**NT hizmeti\\AADConnectProvisioningAgent**) sunucusu ve bu değer **oturum açma** başlatmak için kullanılan hesap hizmeti. Windows Server'ınızdaki bir güvenlik ilkesi yerel hesaplar hizmetlerin çalışmasını engelliyorsa, bu hatayla karşılaşır. | Açık *Hizmetler konsolunu*. Windows hizmeti 'Microsoft Azure AD Connect aracı sağlama' sağ tıklayın ve oturum açma sekmede hizmeti çalıştırmak için bir etki alanı yöneticisi hesabı belirtin. Hizmeti yeniden başlatın. |
|3.| Sağlama Aracısı adımda, AD etki alanı ile yapılandırırken *Active Directory'ye bağlanın*, sihirbaz AD şema yüklenmeye çalışılırken zaman alıyor ve sonunda zaman aşımına uğrar. | Bu hata genellikle, güvenlik duvarı sorunlarından dolayı sihirbaz AD etki alanı denetleyicisi sunucusuna bağlanamadığında gösterilir. | Üzerinde *Active Directory'ye bağlanın* Sihirbazı ekran için AD etki alanı kimlik bilgilerini sağlarken adlı bir seçenek yoktur *seçin etki alanı denetleyicisi öncelik*. Aynı sitede Aracısı sunucusu olan etki alanı denetleyicisi seçmek için bu seçeneği kullanın ve iletişimini engelleyen bir güvenlik duvarı kuralları olmadığından emin olun. |

#### <a name="connectivity-errors"></a>Bağlantı hataları

Sağlama hizmeti Workday veya Active Directory hizmetine bağlanamıyor, sağlama karantinaya alınmış bir duruma geçmesine neden olabilir. Bağlantı sorunlarını gidermek için aşağıdaki tabloyu kullanın.

|#|Hata senaryosu |Olası nedenleri|Önerilen çözüm|
|--|---|---|---|
|1.| Tıkladığınızda **Bağlantıyı Sına**, hata iletisiyle karşılaşırsınız: *Active Directory'ye bağlanılırken bir hata oluştu. Şirket içi aracı sağlama çalıştığından ve doğru Active Directory etki alanı ile yapılandırıldığından emin olun.* | Bu hata genellikle if sağlama aracı çalışmıyor veya Azure AD arasındaki iletişimi engelleyen bir güvenlik duvarı gösterir ve sağlama Aracısı. Etki alanı aracı Sihirbazı'nda yapılandırılmamışsa, bu hatayı görebilirsiniz. | Açık *Hizmetleri* aracının çalışır durumda olduğunu doğrulamak için Windows sunucusu üzerinde Konsolu. Sağlama aracı Sihirbazı'nı açmak ve doğru etki alanını aracıyla kayıtlı olduğunu onaylayın.  |
|2.| Sağlama işi hafta sonları (Cuma Doy) karantina durumuna geçtiğinde ve biz eşitleme ile ilgili bir hata olduğunu bir e-posta bildirimi alın. | Bu hatanın yaygın nedenlerinden biri Workday'in planlı kapalı kalma süresidir. Workday uygulama kiracısını kullanıyorsanız, Workday'in uygulama kiracıları için hafta sonları zamanladığı kapalı kalma sürelerini (genellikle Cuma akşamından Cumartesi sabahına kadar) ve bu süre boyunca Workday sağlama uygulamalarının Workday'e bağlanamadığı için karantina durumuna geçebileceğini unutmayın. Workday uygulama kiracısı yeniden çevrimiçi olduğunda normal durumuna geri döner. Nadir durumlarda, kiracı yenilendiği veya hesap kilitlendiği ya da süresi dolduğu için Tümleştirme Sistemi Kullanıcısının parolası değiştirildiğinde de bu hatayı görebilirsiniz. | Workday'in kapalı kalma zamanlamasını öğrenmek için Workday yöneticinize veya iş ortağınıza danışın. Kapalı kalma süresince uyarı iletilerini yoksayın ve Workday örneği yeniden çevrimiçi olduğunda kullanılabilirliği onaylayın.  |


#### <a name="ad-user-account-creation-errors"></a>AD kullanıcı hesabı oluşturma hataları

|#|Hata senaryosu |Olası nedenleri|Önerilen çözüm|
|--|---|---|---|
|1.| Dışarı aktarma işlemi hatalarını Denetim günlüğüne iletiyle *hata: OperationsError-SvcErr: İşlem hatası oluştu. Dizin hizmeti için üst başvuru yapılandırılmamış. Dizin hizmeti, bu nedenle veremiyor bu orman dışındaki nesneler için silemiyor.* | Bu hata genellikle if gösterir *Active Directory kapsayıcısı* OU doğru şekilde ayarlanmadı veya ile ilgili sorunlar varsa ifade eşleştirmesi için kullanılan *parentDistinguishedName*. | Denetleme *Active Directory kapsayıcısı* yazım hataları için OU parametresi. Öznitelik eşlemesinde *parentDistinguishedName* kullanıyorsanız, bunun her zaman AD etki alanı içinde bilinen bir kapsayıcı olarak değerlendirildiğinden emin olun. Denetleme *dışarı* denetim olayı günlüğe kaydeder oluşturulan değeri görmek için. |
|2.| Denetim günlüğünde hata koduyla işlemi hatalarını dışarı aktarın: *SystemForCrossDomainIdentityManagementBadResponse* ve ileti *hata: ConstraintViolation AtrErr: İstek değeri geçersiz. Bir değer özniteliği için kabul edilebilir değerler aralığında değildi. \nError ayrıntıları: CONSTRAINT_ATT_TYPE - şirket*. | Bu hata özgü olsa *şirket* öznitelik gibi diğer öznitelikler için bu hata ile karşılaşabilirsiniz *CN* de. Bu hata, zorunlu AD şema kısıtlaması nedeniyle görüntülenir. Varsayılan olarak, öznitelikleri ister *şirket* ve *CN* AD'de 64 karakter üst sınırını sahip. Ardından, 64 karakterden uzun Workday'den gelen değer ise bu hata iletisini görürsünüz. | Denetleme *dışarı* olay özniteliğinin değeri görmek için Denetim günlüklerinde hata iletisinde bildirilen. Workday kullanarak gelen değer kesiliyor göz önünde bulundurun [Mid](../manage-apps/functions-for-customizing-application-data.md#mid) işlev veya bir AD özniteliği, uzunluk kısıtlamalarına benzer olmaması eşlemelerini değiştirme.  |

#### <a name="ad-user-account-update-errors"></a>AD kullanıcı hesabını güncelleştirme hataları

AD kullanıcı hesabını güncelleştirme işlemi sırasında sağlama hizmeti Workday hem AD bilgilerini okur, öznitelik eşleme kurallarını çalıştırır ve tüm ihtiyaçlarını etkili olması için değiştirirseniz belirler. Buna göre bir güncelleştirme olay tetiklenir. Aşağıdaki adımlardan birini bir hatayla karşılaştığında denetim günlüklerinde günlüğe kaydedilir. Genel güncelleştirme hataları gidermek için aşağıdaki tabloyu kullanın.

|#|Hata senaryosu |Olası nedenleri|Önerilen çözüm|
|--|---|---|---|
|1.| Eşitleme kuralı eylemi hataları Denetim günlüğüne iletiyle *EventName EntrySynchronizationError ve ErrorCode = EndpointUnavailable =*. | Sağlama hizmeti kullanıcı profil verileri Active Directory'den şirket tarafından karşılaşılan bir işleme hata nedeniyle alamadı. Bu hata gösterilir sağlama aracı. | Okuma işlemi (2 numaralı olay Kimliğine göre filtrele) ile ilgili sorunlar belirten hata olayları için sağlama aracı Olay Görüntüleyicisi günlüklerini denetleyin. |
|2.| AD Yöneticisi özniteliğinde belirli kullanıcılar için AD'de güncelleştirilmiyor. | Bu hatanın en olası nedeni, kapsam kuralları kullanıyorsanız ve kullanıcının yöneticisinin kapsamın bir parçası değil. ' dir. Bu sorunu, manager'ın eşleşen ID özniteliği (örneğin, EmployeeID) hedef AD etki alanı bulunamadı ya da doğru değerine ayarlı değil de çalıştırabilirsiniz. | Kapsam belirleme filtresi gözden geçirin ve kapsam içinde manager kullanıcısını ekleyin. Eşleşen bir ID özniteliği için bir değer olduğundan emin olmak için ad manager'ın profilini denetleyin. |

## <a name="managing-your-configuration"></a>Yapılandırmanızı yönetme

Bu bölümde, nasıl daha da genişletebilir, özelleştirme ve Workday tabanlı kullanıcı sağlama yapılandırmasını yönetme açıklanmaktadır. Aşağıdaki konular kapsar:

* [Workday kullanıcı özniteliklerinin listesi özelleştirme](#customizing-the-list-of-workday-user-attributes)  
* [Yapılandırmanızı alma ve verme](#exporting-and-importing-your-configuration)

### <a name="customizing-the-list-of-workday-user-attributes"></a>Workday kullanıcı özniteliklerinin listesi özelleştirme

Active Directory ve Azure AD iki Workday kullanıcı özniteliklerinin varsayılan listesini dahil için uygulamaları sağlama Workday arasından seçim yapabilirsiniz. Ancak, bu liste kapsamlı değildir. Workday yüzlerce olası kullanıcı öznitelikleri, standart veya benzersiz Workday kiracınız olabilir ya da destekler.

Azure AD sağlama hizmeti, liste veya Workday öznitelik olarak kullanıma sunulan herhangi bir özniteliği eklemek için özelleştirme yeteneği destekler [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) İnsan Kaynakları API işlemi.

Bu değişikliği yapmak için kullanmanız gerekir [Workday Studio](https://community.workday.com/studio-download) kullanmak istediğiniz özniteliklerini temsil eder ve ardından Azure portalında Gelişmiş Öznitelik Düzenleyicisi'ni kullanarak sağlama yapılandırmanıza ekleyin XPath ifadeleri ayıklamak için .

**Workday kullanıcı özniteliği için bir XPath ifadesi almak için:**

1. İndirme ve yükleme [Workday Studio](https://community.workday.com/studio-download). Yükleyici erişmek için bir iş günü topluluk hesabı gerekir.

2. Workday Human_Resources WSDL dosyası bu URL'den indirin: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. Workday Studio'yu başlatın.

4. Komut çubuğundan seçin **Workday > test eden Test Web hizmetinde** seçeneği.

5. Seçin **dış**, 2. adımda indirdiğiniz Human_Resources WSDL dosyasını seçin.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio1.png)

6. Ayarlama **konumu** alanı `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, ancak "IMPL-CC", gerçek değiştirerek örnek türü ve "KİRACI" gerçek Kiracı adınızla.

7. Ayarlama **işlemi** için **Get_Workers**

8.  Küçük tıklatın **yapılandırma** istek/yanıt bölmeleri Workday kimlik bilgileriniz ayarlamak için aşağıdaki bağlantıya. Denetleme **kimlik doğrulaması**ve ardından, Workday tümleştirmesi sistem hesabı için kullanıcı adını ve parolasını girin. Kullanıcı adı adı olarak biçimlendirdiğinizden emin olun\@Kiracı ve bırakın **WS-güvenlik UsernameToken** seçeneği belirlenmiş.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio2.png)

9. **Tamam**’ı seçin.

10. İçinde **istek** bölmesinde, aşağıdaki ve ayarlanmış XML yapıştırma seçeneğiyle **Employee_ID** gerçek bir kullanıcının Workday'deki kiracınızdaki çalışan kimliği. Özniteliğine sahip bir kullanıcı seçin ayıklamak istediğiniz doldurulur.

    ```xml
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

13. Komut çubuğu, Workday Studio'da seçin **Dosya > Dosya Aç...**  kaydettiğiniz XML dosyasını açın. Bu eylem, dosya Workday Studio XML düzenleyicisinde açılır.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio3.png)

14. Dosya ağacı içinde gezinmek   **/env: Zarf > env: Body > wd:Get_Workers_Response > wd:Response_Data > wd: Çalışan** kullanıcı verileri bulmak için.

15. Altında **wd: Çalışan**, eklemek istediğiniz özniteliğini bulun ve seçin.

16. Seçili özniteliğinizi işyeri dışında XPath ifadesi kopyalama **belge yolu** alan.

17. Kaldırma **/env:Envelope / env:Body / wd:Get_Workers_Response / wd:Response_Data /** kopyalanan ifadesinden öneki.

18. Kopyalanan ifadedeki son öğenin bir düğüm olup olmadığını (örnek: "/ wd: Birth_Date"), ardından ekleme **/text()** ifadenin sonunda. Bu son öğeden bir öznitelik ise gerekli değildir (örnek: "/@wd: türü").

19. Sonucun şunun gibi olmalıdır `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Azure portalında kopyalayacak değerdir.

**Sağlama yapılandırmanızı, özel Workday kullanıcı özniteliği eklemek için:**

1. Başlatma [Azure portalında](https://portal.azure.com)ve Bu öğreticide daha önce açıklandığı gibi uygulama sağlama İş gününüzün hazırlama bölümüne gidin.

2. Ayarlama **sağlama durumu** için **kapalı**seçip **Kaydet**. Bu adım, hazır olduğunuzda, değişikliklerinizi etkili olmanıza yardımcı olur.

3. Altında **eşlemeleri**seçin **şirket içi Active Directory eşitleme Workday'deki çalışanları** (veya **eşitleme Workday'deki çalışanları Azure AD'ye**).

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

12. Ana duyulduğundan **sağlama** sekmesinde **şirket içi Active Directory eşitleme Workday'deki çalışanları** (veya **eşitleme çalışanları Azure AD'ye**) yeniden.

13. Seçin **yeni eklemesi**.

14. Yeni bir öznitelik şimdi gözükeceğini **kaynak özniteliği** listesi.

15. İstediğiniz gibi yeni bir öznitelik için bir eşleme ekleyin.

16. İşiniz bittiğinde ayarlamayı unutmayın **sağlama durumu** geri **üzerinde** ve kaydedin.

### <a name="exporting-and-importing-your-configuration"></a>Yapılandırmanızı alma ve verme

Bu bölümde Microsoft Graph API ve Graph Gezgini öznitelik eşlemelerini Workday sağlama ve şema bir JSON dosyasına aktarın ve Azure AD'ye geri almak için nasıl kullanılacağını açıklar.

#### <a name="step-1-retrieve-your-workday-provisioning-app-service-principal-id-object-id"></a>1. Adım: İş günü, sağlama uygulama hizmet sorumlusu kimliği (nesne kimliği) alma

1. Başlatma [Azure portalında](https://portal.azure.com)ve uygulama sağlama İş gününüzün özellikler bölümüne gidin.
1. İle ilişkili GUID değeri sağlama uygulamanızın özellikler bölümü kopyalayın *nesne kimliği* alan. Bu değer olarak da adlandırılır **Serviceprincipalıd** uygulamanız ve onu Graph Gezgini işlemlerinde kullanılır.

   ![Workday uygulama hizmet sorumlusu kimliği](./media/workday-inbound-tutorial/wd_export_01.png)

#### <a name="step-2-sign-into-microsoft-graph-explorer"></a>2. Adım: Microsoft Graph Gezgini'nda oturum açın

1. Başlatma [Microsoft Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer)
1. "Oturum açma ile Microsoft" düğmesi ve Azure AD genel Yöneticisi veya uygulama kimlik bilgilerini kullanarak oturum açın tıklatın.

    ![Graf oturum açma](./media/workday-inbound-tutorial/wd_export_02.png)

1. Başarılı oturum açma sırasında sol bölmedeki kullanıcı hesabı ayrıntıları görürsünüz.

#### <a name="step-3-retrieve-the-provisioning-job-id-of-the-workday-provisioning-app"></a>3. adım: Workday sağlama uygulaması sağlama işi kimliği alınamıyor

Microsoft Graph Explorer'da ile [Serviceprincipalıd] değiştirerek aşağıdaki GET sorguyu çalıştırın. **Serviceprincipalıd** ayıklanan [1. adım](#step-1-retrieve-your-workday-provisioning-app-service-principal-id-object-id).

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs
```

Aşağıda gösterildiği gibi bir yanıtı alırsınız. "ID özniteliği" kopyalama yanıt yok. Bu değer **ProvisioningJobId** ve arka plandaki şema meta verilerini almak için kullanılır.

   [![Sağlama iş kimliği](./media/workday-inbound-tutorial/wd_export_03.png)](./media/workday-inbound-tutorial/wd_export_03.png#lightbox)

#### <a name="step-4-download-the-provisioning-schema"></a>4. Adım: Sağlama şema indirin

Microsoft Graph Explorer'da aşağıdaki GET sorgu, değiştirme [Serviceprincipalıd] ve [ProvisioningJobId] ile Serviceprincipalıd çalıştırın ve önceki adımlarda ProvisioningJobId alınır.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

JSON nesnesi yanıttan kopyalayın ve şema yedeğini oluşturmak için bir dosyaya kaydedin.

#### <a name="step-5-import-the-provisioning-schema"></a>5. Adım: Sağlama Şemayı içeri aktar

> [!CAUTION]
> Azure portalını kullanarak değiştirilemez configuration için şema değiştirmeniz gerekiyorsa veya bir geçerli daha önce yedeklenen dosyasıyla ve çalışma şema yapılandırmasını geri yüklemeniz gerekiyorsa, bu adımı gerçekleştirin.

Microsoft Graph Explorer'da aşağıdaki PUT sorgu, değiştirme [Serviceprincipalıd] ve [ProvisioningJobId] Serviceprincipalıd ile yapılandırın ve önceki adımlarda ProvisioningJobId alınır.

```http
    PUT https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

"İstek gövdesi" sekmesinde, JSON şema dosyasının içeriğini kopyalayın.

   [![İstek gövdesi](./media/workday-inbound-tutorial/wd_export_04.png)](./media/workday-inbound-tutorial/wd_export_04.png#lightbox)

"İstek üstbilgileri" sekmesinde "application/json" değerine sahip Content-Type üst bilgisi özniteliği Ekle

   [![İstek üst bilgileri](./media/workday-inbound-tutorial/wd_export_05.png)](./media/workday-inbound-tutorial/wd_export_05.png#lightbox)

Yeni Şemayı içeri aktarmak için "Sorgu Çalıştır" düğmesine tıklayın.

## <a name="managing-personal-data"></a>Kişisel verileri yönetme

Çözüm için Active Directory sağlama iş günü bir sağlama Aracısı şirket içi Windows server üzerinde yüklü olmasını gerektirir ve bu aracı, Windows olay günlüğünde, Workday AD özniteliğine bağlı olarak kişisel veri içerebilen günlükleri oluşturur. eşlemeleri. Kullanıcı gizliliği yükümlülüklerini uymak için olay günlükleri bir Windows ayarlayarak 48 saat olay günlüğünü Temizle Görevi Zamanlanmış veri tutulur emin olabilirsiniz.

Azure AD sağlama hizmeti sınıfındadır **veri işlemcisi** GDPR sınıflandırması kategorisidir. Bir veri işlemcisi işlem hattı anahtar iş ortakları ve son tüketici hizmeti veri işleme hizmetleri sağlar. Azure AD sağlama hizmeti, kullanıcı verilerini oluşturmaz ve bağımsız hangi kişisel veriler üzerinde toplanır ve nasıl kullanılacağını denetleyemez. Veri alma, toplama, analiz ve raporlama sağlama hizmeti, Azure AD'de mevcut Kurumsal verileri temel alır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

Veri saklama göre sağlama hizmetini Azure AD raporlar oluşturmaz, analizler gerçekleştirin veya 30 gün ayrıntılı bilgiler sağlar. Bu nedenle, Azure AD sağlama hizmeti depolayan, işleyen veya 30 gün hiçbir veriyi saklamak. Bu tasarım, GDPR düzenlemeler, Microsoft gizlilik uyumluluk düzenlemelerini ve Azure AD veri saklama ilkeleri ile uyumludur.

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
* [Workday Azure Active Directory ile çoklu oturum açmayı yapılandırma hakkında bilgi edinin](workday-tutorial.md)
* [Diğer SaaS uygulamalarına Azure Active Directory ile tümleştirme hakkında bilgi edinin](tutorial-list.md)
* [Sağlama yapılandırmalarını yönetmek için Microsoft Graph API'lerini kullanmayı öğrenin](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
