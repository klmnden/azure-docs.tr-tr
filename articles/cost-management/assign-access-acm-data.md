---
title: Azure maliyet Yönetimi verilerine erişim atama | Microsoft Docs
description: Bu makalede, Azure maliyet Yönetimi verilerine erişim kapsamları çeşitli iznini atama rağmen gösterilmektedir.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/09/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: e32e281509da32d4816c9e137a462553891c82f1
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686152"
---
# <a name="assign-access-to-cost-management-data"></a>Maliyet Yönetimi verilerine erişim atama

Çoğu kullanıcı için Azure portalında hem de kurumsal (EA) portalında verilen izinler, Azure maliyet Yönetimi verilerine bir kullanıcının erişim düzeyini tanımlayın. Bu makalede, maliyet Yönetimi verilerine erişim atama aracılığıyla gösterilmektedir. Maliyet Yönetimi'nde kullanıcı görünümleri verileri için ve Azure Portalı'nda seçtiğiniz kapsamda erişime sahip oldukları kapsamına göre bu izinleri birleşimi atandıktan sonra.

Bir kullanıcının seçtiği kapsam maliyet yönetimi, veri birleştirme sağlamak ve maliyet bilgilerini erişimi denetlemek için kullanılır. Kapsamları kullanırken, kullanıcılar çoklu seçim yoksa bunları. Bunun yerine, alt kapsamlar kadar geri alma ve sonra bunlar filtre görüntülemek istediklerini için aşağı daha büyük bir kapsam seçin. Veri birleştirme, bazı kişiler, alt kapsamlar aktarma hedefi bir üst kapsama erişimi olmaması nedeniyle anlamak önemlidir.

## <a name="cost-management-scopes"></a>Maliyet Yönetimi kapsamları

Maliyet verilerini görüntülemek için bir kullanıcı en az bir veya daha fazla aşağıdaki kapsamlar için erişim okuma olmalıdır.

| **Kapsam** | **Tanımlanma yeri** | **Verileri görüntülemek için gerekli erişim** | **Önkoşul EA ayarı** | **Verileri bir araya getirir** |
| --- | --- | --- | --- | --- |
| Faturalama hesabı<sup>1</sup> | [https://ea.azure.com](https://ea.azure.com/) | Kuruluş Yöneticisi | None | Kurumsal sözleşmedeki tüm abonelikler |
| Bölüm | [https://ea.azure.com](https://ea.azure.com/) | Bölüm Yöneticisi | **Ücretleri görüntüle DA** etkin | Bölüme bağlı olan kayıt hesabına ait olan tüm abonelikler |
| Kayıt hesabı<sup>2</sup> | [https://ea.azure.com](https://ea.azure.com/) | Hesap Sahibi | **Saniye başına AO ücretleri görüntüle** etkin | Kayıt hesabındaki tüm abonelikler |
| Yönetim grubu | [https://portal.azure.com](https://portal.azure.com/) | Maliyet Yönetimi Okuyucusu (veya Okuyucu) | **Saniye başına AO ücretleri görüntüle** etkin | Yönetim grubu altındaki tüm abonelikler |
| Abonelik | [https://portal.azure.com](https://portal.azure.com/) | Maliyet Yönetimi Okuyucusu (veya Okuyucu) | **Saniye başına AO ücretleri görüntüle** etkin | Abonelikteki tüm kaynaklar/kaynak grupları |
| Kaynak grubu | [https://portal.azure.com](https://portal.azure.com/) | Maliyet Yönetimi Okuyucusu (veya Okuyucu) | **Saniye başına AO ücretleri görüntüle** etkin | Kaynak grubundaki tüm kaynaklar |

<sup>1</sup> fatura hesabın da Kurumsal Anlaşma ya da kayıt adlandırılır.

<sup>2</sup> kayıt hesabı da hesap sahibi olarak adlandırılır.

## <a name="enable-access-to-costs-in-the-ea-portal"></a>EA portal maliyetlerini erişimi etkinleştirme

Fatura hesabı kapsamı gerektirir **DA ücretleri görüntüle** seçeneği **etkin** EA portalında. Diğer tüm kapsamları **AO ücretleri görüntüle** seçeneği **etkin** EA portalında.

Seçeneği etkinleştirmek için:

1. EA portalında oturum açın [ https://ea.azure.com ](https://ea.azure.com) Kurumsal Yönetici hesabı.
2. Seçin **Yönet** sol bölmesinde.
3. Maliyet yönetimi, kapsamları için ücret seçeneğini etkinleştirmek için erişim sağlamak istiyorsanız **DA ücretleri görüntüle** ve/veya **AO ücretleri görüntüle**.  
    ![Seçenekleri DA ve saniye başına AO görünümü gösteren kayıt sekmesi ücretleri](./media/assign-access-acm-data/ea-portal-enrollment-tab.png)

Görünüm ücret seçenekleri etkinleştirildikten sonra çoğu kapsamları, ayrıca Azure portalında rol tabanlı erişim denetimi (RBAC) iznini yapılandırma gerektirir.

## <a name="enterprise-administrator-role"></a>Kuruluş yöneticisi rolü

Varsayılan olarak, bir kuruluş yöneticisi Faturalama hesabı (Kurumsal Anlaşma/kaydı) ve alt kapsamlar olan tüm diğer kapsamlar için erişim vardır. Kuruluş Yöneticisi, diğer kullanıcılar için kapsamlar için erişim atar. İş sürekliliği için en iyi uygulama olarak her zaman iki kullanıcı Kurumsal yönetici erişimine sahip olmalıdır. Aşağıdaki bölümlerde kapsamlar diğer kullanıcılar için Kurumsal Yönetici atama erişim gözden geçirme örnekleri var.

## <a name="assign-billing-account-scope-access"></a>Fatura hesabı kapsam erişim atama

Fatura hesabı kapsamı erişim EA portalında Kurumsal yönetici izni gerektirir. Kuruluş Yöneticisi tüm EA kaydı veya birden çok kayıtlar arasında maliyetlerini görüntüleme erişimine sahiptir. Azure portalında Faturalama hesabı kapsam için herhangi bir eylemi gereklidir.

1. EA portalında oturum açın [ https://ea.azure.com ](https://ea.azure.com) Kurumsal Yönetici hesabı.
2. Seçin **Yönet** sol bölmesinde.
3. Üzerinde **kayıt** sekmesinde, yönetmek istediğiniz kaydı seçin.  
    ![EA portal](./media/assign-access-acm-data/ea-portal.png)
4. Tıklayın **+ yönetici Ekle**.
5. Yönetici Ekle iletişim kutusunda, kimlik doğrulama türünü seçin ve kullanıcının e-posta adresini yazın.
6. Kullanıcı altında maliyet ve kullanım verileri, salt okunur erişimi olmaması gerekiyorsa **salt okunur**seçin **Evet**.  Aksi takdirde seçin **Hayır**.
7. Tıklayın **Ekle** hesabı oluşturmak için.  
    ![Yönetici kutusu ekleme](./media/assign-access-acm-data/add-admin.png)

Bu yeni kullanıcı maliyet Yönetimi'nde verilere erişmeden önce tamamlanması 30 dakika sürebilir.

### <a name="assign-department-scope-access"></a>Departman kapsam erişim atama

Departman kapsamı için EA portalında departman Yöneticisi (DA ücretleri görüntüle) erişim gerekir. Bölüm Yöneticisi maliyetleri ve bir departman veya birden çok bölümlerine ilişkili kullanım verilerini görüntüleme erişimine sahiptir.  Veri bölümü için bölüm için bağlı bir kayıt hesabına ait tüm abonelikler içerir. Azure portalında herhangi bir eylemi gereklidir.

1. EA portalında oturum açın [ https://ea.azure.com ](https://ea.azure.com) Kurumsal Yönetici hesabı.
2. Seçin **Yönet** sol bölmesinde.
3. Üzerinde **kayıt** sekmesinde, yönetmek istediğiniz kaydı seçin.
4. Tıklayın **departmanı** sekmesine ve ardından **Yöneticisi Ekle**.
5. Departman Yöneticisi Ekle iletişim kutusunda, kimlik doğrulaması türünü seçin ve ardından kullanıcının e-posta adresini yazın.
6. Kullanıcı altında maliyet ve kullanım verileri, salt okunur erişimi olmaması gerekiyorsa **salt okunur**seçin **Evet**.  Aksi takdirde seçin **Hayır**.
7. Departman yönetici izni vermek istediğiniz bölümleri seçin.
8. Tıklayın **Ekle** hesabı oluşturmak için.  
    ![Bölüm Yöneticisi kutusu ekleme](./media/assign-access-acm-data/add-depart-admin.png)

## <a name="assign-enrollment-account-scope-access"></a>Kayıt hesabı kapsam erişim atama

Kayıt hesabı kapsam erişim EA portalında hesap sahibi (saniye başına AO görünümü ücretleri) erişimi gerektirir. Hesap sahibinin kim maliyetlerini ve bir kayıt hesabı ile ilişkili kullanım verileri görüntüleyebilirsiniz. Veriler kayıt hesabı, kayıt için ilişkili tüm Azure abonelikleri içerir. Azure portalında herhangi bir eylemi gereklidir.

1. EA portalında oturum açın [ https://ea.azure.com ](https://ea.azure.com) Kurumsal Yönetici hesabı.
2. Seçin **Yönet** sol bölmesinde.
3. Üzerinde **kayıt** sekmesinde, yönetmek istediğiniz kaydı seçin.
4. Tıklayın **hesabı** sekmesine ve ardından **hesabı Ekle**.
5. Hesabı Ekle kutusunda **departmanı** atanmamış ilişkilendirmek için hesap ya da devre dışı olarak bırakın.
6. Kimlik doğrulaması türünü seçin ve hesap adını yazın.
7. Kullanıcının e-posta adresini yazın ve isteğe bağlı olarak maliyet merkezini girin.
8. Tıklayarak **Ekle** hesabı oluşturmak için.  
    ![Hesap kutusu ekleme](./media/assign-access-acm-data/add-account.png)

## <a name="assign-management-group-scope-access"></a>Yönetim grubu kapsamı erişim atama

Bir yönetim grubu kapsamı erişmesi en az maliyet Yönetimi Okuyucu (ya da okuyucu) izni. Yönetim grubu izni Azure Portalı'nda yapılandırırsınız. En az olmalıdır başkaları için erişimi etkinleştirmek için yönetim grubu için katkıda bulunan izni. Ve ayrıca etkinleştirdiyseniz gerekir **AO ücretleri görüntüle** EA portalında ayarlama.

1. [http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.
2. Seçin **tüm hizmetleri** ve Kenar çubuğunda arama _Yönetim grupları_, ardından **Yönetim grupları**.
3. Yönetim grubu ve hiyerarşide seçebilirsiniz.
4. Yönetim grubunuzun adının yanında, tıklayın **ayrıntıları**.
5. Seçin **erişim denetimi (IAM)** sol bölmeden.
6. **Ekle**'ye tıklayın.
7. Altında **rol**seçin **maliyet Yönetimi okuyucu**.
8. Altında **erişim Ata**seçin **Azure AD kullanıcı, Grup veya uygulama**.
9. Erişim atamak için arama yapın ve ardından kullanıcıyı seçin.
10. **Kaydet**’e tıklayın.  
    ![İzinleri Kutusu Ekle](./media/assign-access-acm-data/add-permissions.png)

## <a name="assign-subscription-scope-access"></a>Abonelik kapsamı erişim atama

Bir aboneliğe erişim gerektiren en az maliyet Yönetimi Okuyucu (ya da okuyucu) izni. Azure portalında bir aboneliğe izin yapılandırdığınız. En az olmalıdır başkaları için erişimi etkinleştirmek için aboneliğe katkıda bulunan izni. Ve ayrıca etkinleştirdiyseniz gerekir **AO ücretleri görüntüle** EA portalında ayarlama.

1. [http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.
2. Seçin **tüm hizmetleri** ve Kenar çubuğunda arama _abonelikleri_, ardından **abonelikleri**.
3. Aboneliğinizi seçin.
4. Seçin **erişim denetimi (IAM)** sol bölmeden.
5. **Ekle**'ye tıklayın.
6. Altında **rol**seçin **maliyet Yönetimi okuyucu**.
7. Altında **erişim Ata**seçin **Azure AD kullanıcı, Grup veya uygulama**.
8. Erişim atamak için arama yapın ve ardından kullanıcıyı seçin.
9. **Kaydet**’e tıklayın.

## <a name="assign-resource-group-scope-access"></a>Kaynak grubu kapsamı erişim atama

Bir kaynak grubuna erişim gerektiren en az maliyet Yönetimi Okuyucu (ya da okuyucu) izni. Azure portalında bir kaynak grubu için izin yapılandırdığınız. En az olmalıdır başkaları için erişimi etkinleştirmek için kaynak grubuna katkıda bulunan izni. Ve ayrıca etkinleştirdiyseniz gerekir **AO ücretleri görüntüle** EA portalında ayarlama.

1. [http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.
2. Seçin **tüm hizmetleri** ve Kenar çubuğunda arama _kaynak grupları_, ardından **kaynak grupları**.
3. Kaynak grubunuzu seçin.
4. Seçin **erişim denetimi (IAM)** sol bölmeden.
5. **Ekle**'ye tıklayın.
6. Altında **rol**seçin **maliyet Yönetimi okuyucu**.
7. Altında **erişim Ata**seçin **Azure AD kullanıcı, Grup veya uygulama**.
8. Erişim atamak için arama yapın ve ardından kullanıcıyı seçin.
9. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- Maliyet yönetimi için zaten ilk hızlı tamamlamadıysanız, hem okuma [maliyetleri başlamanızı](quick-acm-cost-analysis.md).
