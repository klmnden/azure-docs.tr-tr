---
title: Azure portalını kullanarak cihazları yönetme | Microsoft Docs
description: Cihazları yönetmek için Azure portalını kullanmayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: a4a0037d46db67460d507c6e92ab550f7d9c2fbe
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341424"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Azure portalını kullanarak cihaz kimliklerini yönetme

Azure Active Directory'de (Azure AD) cihaz Kimlik Yönetimi ile kullanıcılarınızın kaynaklarınıza güvenlik ve uyumluluğa yönelik standartlarınızı karşılayan cihazlardan eriştiğiniz emin olabilirsiniz.

Bu makalede:

- Aşina olduğunuzu varsayar [Azure Active Directory'de cihaz kimlik yönetimine giriş](overview.md)
- Azure AD portalı kullanarak, cihaz kimliklerini yönetme hakkında bilgi sağlar

## <a name="manage-device-identities"></a>Cihaz kimliklerini yönetme

Azure AD portalında cihaz kimliklerinizi yönetmek için merkezi bir konum sağlar. Bu konuma ya da kullanarak alabilirsiniz bir [doğrudan bağlantı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) veya el ile şu adımları izleyin:

1. Oturum [Azure portalında](https://portal.azure.com) yönetici olarak.
2. Sol gezinti tıklatın **Active Directory**.

   ![Cihaz ayarlarını yapılandırma](./media/device-management-azure-portal/01.png)

3. İçinde **Yönet** bölümünde **cihazları**.

   ![Cihaz ayarlarını yapılandırma](./media/device-management-azure-portal/74.png)

**Cihazları** sayfası, olanak tanır:

- Cihaz ayarlarını yapılandırma
- Cihazlar bulun
- Cihaz kimlik yönetimi görevleri
- Cihaz ile ilgili denetim günlüklerini gözden geçirin  
  
## <a name="configure-device-settings"></a>Cihaz ayarlarını yapılandırma

Azure AD portalı kullanarak, cihaz kimliklerini yönetme için cihazlarınızı aşağıdakilerden biri olması gerekir [kaydedilen veya bu hizmete katılan](overview.md) Azure AD'ye. Yönetici olarak, kaydetme ve aygıt ayarlarını yapılandırarak cihazları birleştirme işlemini hassas ayarlamalar yapabilirsiniz.

![Cihaz ayarlarını yapılandırma](./media/device-management-azure-portal/22.png)

Cihaz ayarları sayfasına yapılandırmanızı sağlar:

![Bir Intune cihaz yönetme](./media/device-management-azure-portal/21.png)

- **Kullanıcılar cihazları Azure AD'ye Katıl** -bu ayarı olarak cihazlarını kaydedebilirsiniz kullanıcılar seçmenizi sağlar [Azure AD join cihazları](overview.md#azure-ad-joined-devices). Varsayılan değer **tüm**.

> [!NOTE]
> **Kullanıcılar cihazları Azure AD'ye Katıl** ayardır yalnızca Windows 10 Azure AD join uygulanabilir.

- **Ek yerel Yöneticiler Azure AD alanına katılmış cihazlar** -bir cihazda yerel yönetici hakları verilen kullanıcılar seçebilirsiniz. Buraya eklenen kullanıcılar için eklendiğinde *cihaz yöneticileri* Azure AD'de rol. Azure AD'de genel Yöneticiler ve varsayılan olarak cihaz sahiplerine yerel yönetici hakları verilir. Bu seçenek, bir premium edition Özelliği Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilir.
- **Kullanıcıların cihazlarını Azure AD'ye kaydetme** -olması Windows 10 kişisel, iOS, Android ve macOs cihazlara izin vermek için bu ayarı yapılandırmanız gereken [kayıtlı](overview.md#azure-ad-registered-devices) Azure AD ile. Seçerseniz **hiçbiri**, cihazları Azure AD'ye kaydetme izin verilmez. Office 365 için Microsoft Intune veya mobil cihaz Yönetimi (MDM) ile kayıt kaydı gerektirir. Bu hizmetlerin herhangi birini yapılandırdıysanız **tüm** seçilir ve **NONE** kullanılabilir değil. Intune Bu ayar etkinleştirilirse ardından Seçenekler burada gri görünür.
- **Cihazları eklemek multi-Factor Auth gerektir** -kullanıcı için ikinci bir kimlik doğrulama faktörü sağlamalarının gerekip gerekmediğini seçin [birleştirme](overview.md#azure-ad-joined-devices) cihazlarını Azure AD'ye. Varsayılan değer **Hayır**. Bir cihaz kaydederken çok faktörlü kimlik doğrulaması gerektiren öneririz. Bu hizmet için multi-Factor authentication'ı etkinleştirmeden önce Azure multi-Factor Authentication, kullanıcıların aygıtlarını kaydetmesini kullanıcılar için yapılandırıldığından emin olmanız gerekir. Daha fazla bilgi için bkz [Azure multi-Factor Authentication dağıtım](../authentication/howto-mfa-getstarted.md). 

> [!NOTE]
> **Cihazları eklemek multi-Factor Auth gerektir** ayarı hibrit Azure AD'ye katılmış cihazlar için geçerli değildir.

- **En fazla cihaz sayısını** -Bu ayar, kullanıcının Azure AD'de sahip olabileceği cihaz sayısının seçmenize olanak sağlar. Bir kullanıcı bu kotaya ulaştığında, bunlar olan yapamaz kadar ek cihazları eklemek veya mevcut cihazların daha fazla kaldırılır. Azure AD'ye katılmış veya Azure AD şu anda kayıtlı olan tüm cihazlar için cihaz kotasına hesaba katılır. Varsayılan değer **20**.

> [!NOTE]
> **En fazla cihaz sayısını** ayarı hibrit Azure AD'ye katılmış cihazlar için geçerli değildir.

- **Kullanıcılar eşitleme ayarları ve uygulama verilerini cihazlarda** -varsayılan olarak, bu ayar **NONE**. Belirli kullanıcılar veya gruplar veya tüm seçilmesi, kullanıcının ayarları ve uygulama verilerini, Windows 10 cihazlarınız arasında eşitlemeye izin verir. Windows 10'da eşitleme birlikte nasıl çalıştığı hakkında daha fazla bilgi edinin.
Bu seçenek, bir premium özelliği, Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilir.

## <a name="locate-devices"></a>Cihazlar bulun

Kayıtlı ve alanına katılmış cihazları bulmak için iki seçeneğiniz vardır:

- **Tüm cihazlar** içinde **Yönet** bölümünü **cihazları** sayfası  

   ![Tüm cihazlar](./media/device-management-azure-portal/41.png)

- **Cihazları** içinde **Yönet** bölümünü bir **kullanıcı** sayfası

   ![Tüm cihazlar](./media/device-management-azure-portal/43.png)

Her iki seçenek ile bir görünüm elde edebilirsiniz:

- Filtre olarak görünen adı kullanan cihazlar için aramanızı sağlar.
- İle kaydedilen ve alanına katılmış cihazların ayrıntılı genel bakış sağlar
- Genel cihaz yönetim görevlerini gerçekleştirmenize olanak sağlar

![Tüm cihazlar](./media/device-management-azure-portal/51.png)

Bazı iOS cihazlarınız için kesme içeren cihaz adları, potansiyel olarak kesme gibi görünen farklı karakter kullanabilirsiniz. Bu tür cihazlar aranıyor değil görüyorsanız biraz karmaşık - olacak şekilde arama sonuçları doğru olun arama dizesi eşleşen kesme işareti karakter içeriyor.

## <a name="device-identity-management-tasks"></a>Cihaz kimlik yönetimi görevleri

Bir genel yönetici veya Bulut cihaz Yöneticisi olarak kayıtlı veya alanına katılmış cihazları yönetebilirsiniz. Intune hizmet yöneticileri yapabilirsiniz:

- Update - örnekler etkinleştirme/cihazları devre dışı bırakma gibi günlük işlemlerini cihazlardır
- Bir cihaz kullanımdan kaldırıldığında ve Azure AD'de silinmesi gereken cihazları Sil

Bu bölümde, genel cihaz kimlik yönetimi görevleri hakkında bilgi sağlar.

### <a name="manage-an-intune-device"></a>Bir Intune cihaz yönetme

Bir Intune Yöneticisi olarak, istiyorsanız olarak işaretlenmiş cihazlarını yönetebilmeniz için **Intune**.

![Bir Intune cihaz yönetme](./media/device-management-azure-portal/31.png)

### <a name="enable--disable-an-azure-ad-device"></a>Bir Azure AD cihaz devre dışı bırak / etkinleştir

Bir cihazı devre dışı bırakmak / etkinleştirmek için iki seçeneğiniz vardır:

- Görevler menüsünde ("...") **tüm cihazlar** sayfası

   ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/71.png)

- Araç çubuğunda **cihazları** sayfası

   ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/32.png)

**Notlar:**

- Genel yöneticisi olmanız ya da cihaz Yöneticisi bir cihazın devre dışı bırakmak / etkinleştirmek için Azure AD'de bulut gerekir. 
- Bir cihazı devre dışı bırakma başarıyla böylece cihaz CA tarafından korunan, Azure AD kaynaklarına erişmesini veya WH4B kimlik bilgilerinizi kullanarak cihaz önleme, Azure AD ile kimlik doğrulaması, bir cihaz engeller.

### <a name="delete-an-azure-ad-device"></a>Bir Azure AD cihaz silme

Bir cihazı silmek için iki seçeneğiniz vardır:

- Görevler menüsünde ("...") **tüm cihazlar** sayfası

   ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/72.png)

- Araç çubuğunda **cihazları** sayfası

   ![Bir cihazı silme](./media/device-management-azure-portal/34.png)

**Notlar:**

- Bir cihazı silmek için Azure AD'de bir genel yönetici veya Intune yönetici olmanız gerekir.
- Bir cihazı silme:
   - Bir cihazı Azure AD'ye kaynaklarınıza erişmesini engeller.
   - Tüm cihaza, örneğin eklenmiş ayrıntıları, Windows cihazları için BitLocker anahtarlarını kaldırır.  
   - Kurtarılamaz bir etkinliği temsil eder ve gerekli olmadığı sürece önerilmez.

Bir cihaz başka bir yönetim yetkilisi (örneğin, Microsoft Intune) tarafından yönetiliyorsa, cihaz silinebilen / Azure AD'de cihaz silmeden önce devre dışı olduğunu emin olun.

### <a name="view-or-copy-device-id"></a>Görüntüleme veya cihaz Kimliğini kopyalama

Bir cihaz kimliği, cihaz kimliği ayrıntılarını cihazında veya sorun giderme sırasında PowerShell kullanarak doğrulamak için kullanabilirsiniz. Kopyalama seçeneği erişmek için cihaz seçeneğine tıklayın.

![Bir cihaz kimliği görüntüleyin](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>Görüntüleme veya BitLocker anahtarlarını kopyalama

Görüntüleyebilir ve bunların şifreli sürücüyü kurtarmasına imkan kullanıcılara yardımcı olmak için BitLocker anahtarları kopyalayın. Bu anahtarlar yalnızca şifrelenmiş Windows cihazlar için kullanılabilir ve anahtarlarını, Azure AD'de depolanan sahip. Cihaz ayrıntılarını erişirken, bu anahtarlar kopyalayabilirsiniz.

![BitLocker anahtarlarını görüntüle](./media/device-management-azure-portal/36.png)

BitLocker anahtarları kopyalayın ya da görüntülemek için cihaz sahibi veya aşağıdaki rolleri atanmış en az birine sahip bir kullanıcı olması gerekir:

- Genel Yönetici
- Yardım Masası Yöneticisi
- Güvenlik Yöneticisi
- Güvenlik okuyucusu
- Intune Hizmet Yöneticisi

> [!NOTE]
> Hibrit Azure AD'ye katılmış Windows 10 cihazlarını bir sahibi yoktur. Bir cihaz sahibi tarafından arayan ve bulunamadı, bu nedenle, cihaz kimliğine göre arama

## <a name="audit-logs"></a>Denetim günlükleri

Cihaz etkinliklerini, etkinlik günlükleri kullanılabilir. Bu günlükler, cihaz Kayıt Hizmeti'ni ve kullanıcılar tarafından tetiklenen etkinlikleri içerir:

- Cihaz oluşturma ve sahipleri ekleme / cihazdaki kullanıcılar
- Cihaz ayarlarında yapılan değişiklikler
- Bir cihaz güncelleştirme veya silme gibi cihaz işlemleri

Denetim verilerine giriş noktanız, **denetim günlükleri** içinde **etkinlik** bölümünü **cihazları** sayfası.

![Denetim günlükleri](./media/device-management-azure-portal/61.png)

Denetim günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Tarih ve saat oluşum
- Hedefleri
- Başlatıcısı / aktörü (kim) bir etkinlik
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
- Başlatan (aktör)

Filtreler yanı sıra belirli girdiler için arama yapabilirsiniz.

![Denetim günlükleri](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD'de eski cihazları yönetme](manage-stale-devices.md)
