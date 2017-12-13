---
title: "Azure portalını kullanarak cihazları yönetme | Microsoft Docs"
description: "Cihazları yönetmek için Azure portalını kullanmayı öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1e0d40b996e181a606d16d26633f890b9169ecbb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="managing-devices-using-the-azure-portal"></a>Azure portalını kullanarak cihazları yönetme


Azure Active Directory'de (Azure AD) ile cihaz yönetimi, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak aygıtlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. 

Bu konuda:

- Aşina olduğunuzu varsayar [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)

- Azure Portalı'nı kullanarak, aygıtları yönetme hakkında bilgi sağlar

## <a name="manage-devices"></a>Cihazları yönetme 

Azure portal, cihazlarınızı yönetmek için merkezi bir konum sağlar. Her iki kullanarak bu konuma alabilirsiniz bir [doğrudan bağlantı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) veya el ile şu adımları izleyin:

1. Oturum açma [Azure portal](https://portal.azure.com) yönetici olarak.

2. Sol gezinti çubuğu üzerinde tıklatın **Active Directory**.

    ![Aygıt ayarlarını yapılandır](./media/device-management-azure-portal/01.png)

3. İçinde **Yönet** 'yi tıklatın **aygıtları**.

    ![Aygıt ayarlarını yapılandır](./media/device-management-azure-portal/11.png)
 
**Aygıtları** sayfası olanak sağlar:

- Aygıt yönetimi ayarlarını yapılandır

- Aygıtlar bulunamadı

- Aygıt yönetim görevlerini gerçekleştirme

- İlgili cihaz yönetimi gözden denetim günlükleri  
  

## <a name="configure-device-settings"></a>Aygıt ayarlarını yapılandır

Azure Portalı'nı kullanarak, cihazlarınızı yönetmek için aygıtlarınızı aşağıdakilerden biri olması gerekir [kayıtlı veya birleştirilmiş](device-management-introduction.md#getting-devices-under-the-control-of-azure-ad) Azure ad. Yönetici olarak, kaydetme ve aygıt ayarlarını yapılandırarak aygıtları birleştirme işleminin ince ayar yapabilirsiniz. 

![Aygıt ayarlarını yapılandır](./media/device-management-azure-portal/22.png)

Aygıt Ayarları sayfasından yapılandırmanıza olanak sağlar:

![Bir Intune cihaz yönetme](./media/device-management-azure-portal/21.png)


- **Kullanıcılar cihazları Azure AD'ye katılma** -Bu ayar için kullanıcıları seçmenize olanak tanır [cihazları](device-management-introduction.md#azure-ad-joined-devices) Azure ad. Varsayılan değer **tüm**.

- **Ek yerel Yöneticiler Azure AD alanına katılmış aygıtlar** -bir cihazda yerel yönetici hakları verilen kullanıcılar seçebilir. Buraya eklenen kullanıcılar için eklendiğinde *cihaz yöneticileri* Azure AD'de rol. Azure AD'de genel Yöneticiler ve cihaz sahiplerine yerel yönetici hakları varsayılan olarak verilmiştir. Bu seçenek bir premium edition Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilen bir özelliktir. 

- **Kullanıcıları Azure AD ile cihazlarını kaydetme** -aygıtlarının izin vermek için bu ayarı yapılandırmak gereken [kayıtlı](device-management-introduction.md#azure-ad-registered-devices) Azure AD ile. Seçerseniz **hiçbiri**, Azure AD alanına bağlı olmadıkları zaman Kaydet veya karma Azure AD alanına katılmış aygıtlar izin verilmez. Office 365 için Microsoft Intune veya mobil cihaz Yönetimi (MDM) ile kayıt kayıt gerektirir. Bu hizmetlerden birini yapılandırdıysanız **tüm** seçilir ve **NONE** kullanılamıyor...

- **Aygıtları katılmak çok öğeli kimlik doğrulama gerektiren** -kullanıcıların ikinci bir kimlik doğrulama faktörü sağlamalarının gerekip gerekmediğini seçebilirsiniz [birleştirme](device-management-introduction.md#azure-ad-joined-devices) cihazlarını Azure ad. Varsayılan değer **Hayır**. Bir cihaz kaydedilirken çok faktörlü kimlik doğrulaması gerektiren öneririz. Bu hizmet için çok faktörlü kimlik doğrulamasını etkinleştirmeden önce çok faktörlü kimlik doğrulamasının cihazlarını kaydeden kullanıcılar için yapılandırılmış emin olmalısınız. Farklı Azure çok faktörlü kimlik doğrulama hizmetleri hakkında daha fazla bilgi için bkz: [Azure multi-Factor authentication ile çalışmaya başlama](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **En fazla cihaz sayısını** -Bu ayar, Azure AD'de kullanıcı olan aygıtların sayısı seçmenize olanak sağlar. Bir kullanıcı bu kota ulaşırsa, bunlar olan değil kadar ek cihaz ekleyemez veya daha fazla var olan cihazları kaldırılır. Azure AD alanına katılmış veya Azure AD bugün kayıtlı olan tüm aygıtları için aygıt teklif sayılır. Varsayılan değer **20**.

- **Kullanıcıların eşitleme ayarları ve uygulama verileri cihaz üzerinden** -varsayılan olarak, bu ayar **NONE**. Belirli kullanıcıları veya grupları veya tüm seçilmesi, kullanıcının ayarları ve uygulama verilerini Windows 10 cihazlarını arasında eşitlemeye izin verir. Eşitleme Windows 10'da nasıl çalıştığı hakkında daha fazla bilgi edinin.
Bu seçenek bir premium Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilen bir özelliktir.
 




## <a name="locate-devices"></a>Aygıtlar bulunamadı

Kayıtlı ve birleştirilmiş cihazları bulmak için iki seçeneğiniz vardır:

- **Tüm cihazlar** içinde **Yönet** bölümünü **aygıtları** sayfası  

    ![Tüm cihazlar](./media/device-management-azure-portal/41.png)


- **Aygıtları** içinde **Yönet** bölümünü bir **kullanıcı** sayfası
 
    ![Tüm cihazlar](./media/device-management-azure-portal/43.png)



Her iki seçenek ile bir görünüm elde edebilirsiniz:


- Filtre olarak görünen adı kullanarak cihazları için aramanıza olanak tanır.

- Kayıtlı ve birleştirilmiş aygıtları ayrıntılı bakış sağlar

- Genel cihaz yönetim görevlerini gerçekleştirmenize olanak sağlar
   

![Tüm cihazlar](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Aygıt yönetim görevleri

Bir yönetici olarak kayıtlı veya birleştirilmiş cihazları yönetebilirsiniz. Bu bölümde, genel cihaz yönetim görevleri hakkında bilgi sağlar.


### <a name="manage-an-intune-device"></a>Bir Intune cihaz yönetme

Intune yöneticisiyseniz, olarak işaretlenmiş cihazlarını yönetebilmeniz için **Microsoft Intune**. Bir yönetici ek aygıt görebilirsiniz 

![Bir Intune cihaz yönetme](./media/device-management-azure-portal/31.png)


### <a name="enable--disable-an-azure-ad-device"></a>Etkinleştir / devre dışı bir Azure AD cihaz

Etkinleştir / bir aygıtı devre dışı bırakmak için iki seçeneğiniz vardır:

- Görevler menüsünde ("...") **tüm cihazlar** sayfası

    ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/71.png)

- Araç çubuğunda **aygıtları** sayfası

    ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/32.png)


**Notlar:**

- Etkinleştir / bir aygıtı devre dışı bırakmak için Azure AD genel yönetici olmanız gerekir. 
- Bir aygıtı devre dışı bırakma, bir aygıt, Azure AD kaynaklarını erişmesini engeller. 



### <a name="delete-an-azure-ad-device"></a>Bir Azure AD cihaz silme

Bir cihazı silmek için iki seçeneğiniz vardır:

- Görevler menüsünde ("...") **tüm cihazlar** sayfası

    ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/72.png)

- Araç çubuğunda **aygıtları** sayfası

    ![Bir aygıtı silme](./media/device-management-azure-portal/34.png)


**Notlar:**

- Bir cihazı silmek için Azure AD genel yönetici olmanız gerekir.  

- Bir cihazı silme:
 
    - Bir aygıt, Azure AD kaynaklara erişmesini engeller. 

    - Tüm aygıt örneğin bağlı ayrıntıları, Windows cihazları için BitLocker anahtarları kaldırır.  

    - Kurtarılamaz bir etkinliği temsil eder ve gerekli olmadığı sürece önerilmez.

Bir cihaz başka bir yönetim yetkilisi (örneğin, Microsoft Intune) tarafından yönetiliyorsa, lütfen aygıt yok temizlenmeden / Azure AD'de cihazın silmeden önce devre dışı olduğunu emin olun.

 


### <a name="view-or-copy-device-id"></a>Görüntülemek veya aygıtın Kimliğini kopyalayın

Cihaz kimliği ayrıntıları aygıttaki veya sorun giderme sırasında PowerShell kullanarak doğrulamak için bir cihaz Kimliği'ni kullanabilirsiniz. Kopyalama seçeneği erişmek için aygıt'ı tıklatın.

![Bir cihaz kimliği görüntüleyin](./media/device-management-azure-portal/35.png)
  

### <a name="view-or-copy-bitlocker-keys"></a>Görüntüleme veya BitLocker anahtarları kopyalama

Bir yöneticiyseniz, görüntüleyebilir ve kullanıcıların kendi şifreli sürücüyü kurtarmasına yardımcı olmak için BitLocker anahtarları kopyalayın. Bu anahtarlar yalnızca şifrelenmiş Windows cihazları için kullanılabilir ve kendi anahtarları Azure AD'de depolanan sahip. Cihaz ayrıntılarını erişirken bu anahtarları kopyalayabilirsiniz.
 
![BitLocker anahtarları görüntüleyin](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Denetim günlükleri


Cihaz etkinliklerini, etkinlik günlükleri ile kullanılabilir. Bu cihaz Kayıt Hizmeti'ni ve kullanıcılar tarafından tetiklenen etkinliklerin içerir:

- Cihaz oluşturma ve sahipleri ekleme / cihazdaki kullanıcılar

- Cihaz ayarlarındaki değişiklikler

- Bir aygıt güncelleştirme veya silme gibi aygıt işlemleri
 
Giriş noktanızdır denetim verilere **denetim günlüklerini** içinde **etkinlik** bölümünü **aygıtları** sayfası.

![Denetim günlükleri](./media/device-management-azure-portal/61.png)


Denetim günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Tarih ve saat oluşum

- hedefleri

- Başlatıcı / aktör (kimin) etkinliğin

- Etkinlik (ne)

![Denetim günlükleri](./media/device-management-azure-portal/63.png)

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.
 
![Denetim günlükleri](./media/device-management-azure-portal/64.png)


Raporlanan verileri istediğiniz düzeye gelecek şekilde daraltmak için, aşağıdaki alanları kullanarak denetim verilerini filtreleyebilirsiniz:

- Kategori
- Etkinlik kaynak türü
- Etkinlik
- Tarih aralığı
- Hedef
- (Aktör) tarafından başlatılan

Filtreler yanı sıra belirli girişleri için arama yapabilirsiniz.

![Denetim günlükleri](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)



