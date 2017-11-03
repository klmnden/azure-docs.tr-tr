---
title: "Azure kullanarak cihazları yönetme portalı - Önizleme | Microsoft Docs"
description: "Cihazları yönetmek için Azure portalını kullanmayı öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 4b46e1627a229b0649d9ccd2550cd28fda9849f8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="managing-devices-using-the-azure-portal---preview"></a>Azure kullanarak cihazları yönetme portalı - Önizleme

>[!NOTE]
>Bu özellik şu anda genel önizlemede değil. Geri veya herhangi bir değişiklik kaldırmak hazırlıklı olun. Genel Önizleme sırasında herhangi bir Azure Active Directory (Azure AD) Abonelik özelliği kullanılabilir. Ancak, özelliği genel kullanıma sunulduğunda özelliği bazı yönlerini bir Azure Active Directory premium aboneliği gerektirebilir.


Azure Active Directory'de (Azure AD) ile cihaz yönetimi, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak aygıtlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. 

Bu konuda:

- Aşina olduğunuzu varsayar [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)

- Azure Portalı'nı kullanarak, aygıtları yönetme hakkında bilgi sağlar


Azure portalında cihazları yönetmek için tıklatın ihtiyacınız **aygıtları** içinde **Yönet** bölümünü **Azure Active Directory** dikey.

![Bir Intune cihaz yönetme](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a>Aygıt ayarlarını yapılandır

Azure Portalı'nı kullanarak, cihazlarınızı yönetmek için bunlar, Azure AD alanına katılmış ya da kayıtlı gerekir. Yönetici olarak, kaydetme ve aygıt ayarlarını yapılandırarak aygıtları birleştirme işleminin ince ayar yapabilirsiniz.

![Bir Intune cihaz yönetme](./media/device-management-azure-portal/22.png)


Cihaz ayarları dikey yapılandırmanıza olanak sağlar:

- **Kullanıcılar cihazları Azure AD'ye katılma** - bu ayarları cihazları Azure AD'ye katılabilirsiniz kullanıcıları seçmenize olanak sağlar. Varsayılan değer **tüm**.

- **Ek yerel Yöneticiler Azure AD alanına katılmış aygıtlar** -bir cihazda yerel yönetici hakları verilen kullanıcılar seçebilir. Buraya eklenen kullanıcılar için eklendiğinde *cihaz yöneticileri* Azure AD'de rol. Azure AD'de genel Yöneticiler ve cihaz sahiplerine yerel yönetici hakları varsayılan olarak verilmiştir. Bu seçenek bir premium edition Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilen bir özelliktir. 

- **Kullanıcıları Azure AD ile cihazlarını kaydetme** -Azure AD ile kaydedilecek cihazları izin vermek için bu ayarı yapılandırmanız gerekir. Seçerseniz **hiçbiri**, Azure AD alanına bağlı olmadıkları zaman Kaydet veya karma Azure AD alanına katılmış aygıtlar izin verilmez. Office 365 için Microsoft Intune veya mobil cihaz Yönetimi (MDM) ile kayıt kayıt gerektirir. Bu hizmetlerden birini yapılandırdıysanız **tüm** seçilir ve **NONE** kullanılamıyor...

- **Aygıtları katılmak çok öğeli kimlik doğrulama gerektiren** -kullanıcıların cihazlarını Azure AD'ye katılmak için ikinci bir kimlik doğrulama faktörü sağlamalarının gerekip gerekmediğini seçin. Varsayılan değer **Hayır**. Bir cihaz kaydedilirken çok faktörlü kimlik doğrulaması gerektiren öneririz. Bu hizmet için çok faktörlü kimlik doğrulamasını etkinleştirmeden önce çok faktörlü kimlik doğrulamasının cihazlarını kaydeden kullanıcılar için yapılandırılmış emin olmalısınız. Farklı Azure çok faktörlü kimlik doğrulama hizmetleri hakkında daha fazla bilgi için bkz: [Azure multi-Factor authentication ile çalışmaya başlama](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **En fazla cihaz sayısını** -Bu ayar, Azure AD'de kullanıcı olan aygıtların sayısı seçmenize olanak sağlar. Bir kullanıcı bu kota ulaşırsa, bunlar olan değil kadar ek cihaz ekleyemez veya daha fazla var olan cihazları kaldırılır. Azure AD alanına katılmış veya Azure AD bugün kayıtlı olan tüm aygıtları için aygıt teklif sayılır. Varsayılan değer **20**.

- **Kullanıcıların eşitleme ayarları ve uygulama verileri cihaz üzerinden** -varsayılan olarak, bu ayar **NONE**. Belirli kullanıcıları veya grupları veya tüm seçilmesi, kullanıcının ayarları ve uygulama verilerini Windows 10 cihazlarını arasında eşitlemeye izin verir. Eşitleme Windows 10'da nasıl çalıştığı hakkında daha fazla bilgi edinin.
Bu seçenek bir premium Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilen bir özelliktir.
 
    ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a>Aygıtlar bulunamadı

Azure portalında yönetici olarak, kayıtlı ve birleştirilmiş cihazları bulmak için iki seçeneğiniz vardır:

- **Tüm cihazlar** içinde **Yönet** bölümünü **aygıtları** dikey penceresi  

    ![Tüm cihazlar](./media/device-management-azure-portal/41.png)


- **Aygıtları** içinde **Yönet** bölümünü bir **kullanıcı** dikey penceresi
 
    ![Tüm cihazlar](./media/device-management-azure-portal/43.png)



Her iki seçenek ile bir görünüm elde edebilirsiniz:


- Filtre olarak görünen adı kullanarak cihazları için aramanıza olanak tanır.

- Kayıtlı ve birleştirilmiş aygıtları ayrıntılı bakış sağlar

- Genel cihaz yönetim görevlerini gerçekleştirmenize olanak sağlar
   

![Tüm cihazlar](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Aygıt yönetim görevleri

Bir yönetici olarak kayıtlı veya birleştirilmiş cihazları yönetebilirsiniz. Bu bölümde, genel cihaz yönetim görevleri hakkında bilgi sağlar.


**Bir Intune cihaz yönetme** -Intune yöneticisiyseniz, olarak işaretlenmiş cihazlarını yönetebilmeniz için **Microsoft Intune**. Bir yönetici ek aygıt görebilirsiniz 

![Bir Intune cihaz yönetme](./media/device-management-azure-portal/31.png)


**Etkinleştir / devre dışı bir Azure AD cihaz**

Etkinleştirmek veya bir aygıtı devre dışı bırakmak için Azure AD genel yönetici olmanız gerekir. Bir aygıtı devre dışı bırakma, bir aygıt, Azure AD kaynaklarını erişmesini engeller.  Aygıt devre dışı bırakmak için her iki tıklatabilirsiniz *...* Aygıt ek ayrıntılar için tıklatın.

 
![Bir Intune cihaz yönetme](./media/device-management-azure-portal/33.png)

Bir aygıtı devre dışı bırakma durumunda değişiklikler **etkin** sütuna **Hayır**.

![Bir aygıtı devre dışı](./media/device-management-azure-portal/32.png)


**Bir Azure AD cihaz silme** - bir cihazı silmek için Azure AD genel yönetici olmanız gerekir.  
Bir cihazı silme:
 
- Bir aygıt, Azure AD kaynaklarına erişmesini engeller. 

- Tüm aygıt örneğin bağlı ayrıntıları, Windows cihazları için BitLocker anahtarları kaldırır  

- Kurtarılamaz bir etkinliği temsil eder ve gerekli olmadığı sürece önerilmez.

Bir cihaz başka bir yönetim yetkilisi (örneğin Microsoft Intune) tarafından yönetiliyorsa, lütfen aygıt yok temizlenmeden / Azure AD'de cihazın silmeden önce devre dışı olduğunu emin olun.

Ya da seçebileceğiniz "..." cihazı silmek veya cihazı ek ayrıntılar için tıklatın
 
![Bir aygıtı silme](./media/device-management-azure-portal/34.png)


**Görüntüleme veya cihaz kimliği kopyalama** -cihaz kimliği ayrıntıları aygıttaki veya sorun giderme sırasında PowerShell kullanarak doğrulamak için bir cihaz kimliği kullanabilirsiniz. Kopyalama seçeneği erişmek için aygıt'ı tıklatın.

![Bir cihaz kimliği görüntüleyin](./media/device-management-azure-portal/35.png)
  

**Görüntüleme veya BitLocker anahtarları kopyalama** -bir yöneticiyseniz, görüntüleyebilir ve kullanıcıların kendi şifreli sürücüyü kurtarmasına yardımcı olmak için BitLocker anahtarları kopyalayın. Bu anahtarlar yalnızca şifrelenmiş Windows cihazları için kullanılabilir ve kendi anahtarları Azure AD'de depolanan sahip. Cihaz ayrıntılarını erişirken bu anahtarları kopyalayabilirsiniz.
 
![BitLocker anahtarları görüntüleyin](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Denetim günlükleri


Cihaz etkinliklerini, etkinlik günlükleri ile kullanılabilir. Bu, kullanıcı veya cihaz kayıt hizmeti tarafından tetiklenen etkinliklerin içerir:

- Cihaz oluşturma ve cihazda sahipleri/kullanıcıları ekleme

- Cihaz ayarlarındaki değişiklikler

- Bir aygıt güncelleştirme veya silme gibi aygıt işlemleri
 
Giriş noktanızdır denetim verilere **denetim günlüklerini** içinde **etkinlik** bölümünü **aygıtları* dikey.

![Denetim günlükleri](./media/device-management-azure-portal/61.png)


Denetim günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Olayın tarihi ve saati

- hedefleri

- Başlatıcı / aktör (kimin) etkinliğin

- Etkinlik (ne)

![Denetim günlükleri](./media/device-management-azure-portal/63.png)

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.
 
![Denetim günlükleri](./media/device-management-azure-portal/64.png)


Raporlanan verileri istediğiniz düzeye gelecek şekilde daraltmak için, aşağıdaki alanları kullanarak denetim verilerini filtreleyebilirsiniz:

- Catergory
- Etkinlik kaynak türü
- Etkinlik
- Tarih aralığı
- Hedef
- (Aktör) tarafından başlatılan

Filtreler yanı sıra belirli girişleri için arama yapabilirsiniz.

![Denetim günlükleri](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)



