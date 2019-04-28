---
title: Azure Veri Kataloğu oluşturma
description: Bir Azure veri Kataloğu oluşturma Hızlı başlangıcı.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: quickstart
ms.date: 04/05/2019
ms.openlocfilehash: f00e9eaf56f3973b357792a8d1923a4b5998e0a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61003414"
---
# <a name="quickstart-create-an-azure-data-catalog"></a>Hızlı Başlangıç: Azure Veri Kataloğu oluşturma

Azure Veri Kataloğu kurumsal veri varlıkları için bir kayıt sistemi ve bulma sistemi olarak görev yapan tam yönetilen bir bulut hizmetidir. Ayrıntılı bir genel bakış için bkz. [Azure Veri Kataloğu nedir](overview.md).

Bu hızlı başlangıçta bir Azure veri Kataloğu oluşturmaya başlamanıza yardımcı olur.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için sahip olmanız gerekir:

* A [Microsoft Azure](https://azure.microsoft.com/) abonelik.
* Kendi ihtiyacınız [Azure Active Directory kiracısı](../active-directory/fundamentals/active-directory-access-create-new-tenant.md).

Veri Kataloğu'nu ayarlamak için sahibi veya bir Azure aboneliğinin ortak sahibi olmalıdır.

## <a name="create-a-data-catalog"></a>Veri Kataloğu oluşturma

Bir kuruluş (Azure Active Directory etki alanı) için yalnızca bir tane veri kataloğu hazırlayabilirsiniz. Sahip veya bu Azure Active Directory etki alanına ait bir Azure aboneliğinin ortak sahibi zaten bir katalog oluşturduysa, birden çok Azure aboneliğiniz varsa, bu nedenle, ardından, bir katalog yeniden oluşturamazsınız. Azure Active Directory etki alanınızda bir kullanıcı tarafından veri kataloğu oluşturulup oluşturulmadığını test etmek için [Azure Veri Kataloğu giriş sayfasına](http://azuredatacatalog.com) gidin ve kataloğu görüp görmediğinizi doğrulayın. Sizin için katalog zaten oluşturulmuşsa aşağıdaki yordamı atlayın ve sonraki bölümüne gidin.

1. Git [Azure portalında](https://portal.azure.com) > **kaynak Oluştur** seçip **veri Kataloğu**.

    ![Veri Kataloğu oluşturma](media/data-catalog-get-started/data-catalog-create.png)

2. Belirtin bir **adı** veri kataloğu için **abonelik** kullanmak istediğiniz **konumu** kataloğu için ve **fiyatlandırma katmanı**. Ardından **Oluştur**’u seçin.

3. [Azure Veri Kataloğu giriş sayfasına](http://azuredatacatalog.com) gidin ve **Verileri Yayımla**’ya tıklayın.

   ![Azure Veri Kataloğu--Verileri Yayımla düğmesi](media/data-catalog-get-started/data-catalog-publish-data.png)

   Ayrıca veri Kataloğu giriş sayfasından ulaşmak için [veri Kataloğu hizmet sayfasına](https://azure.microsoft.com/services/data-catalog) seçerek **başlama**.

   ![Azure Veri Kataloğu--pazarlama giriş sayfası](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)

4. Git **ayarları** sayfası.

    ![Azure Veri Kataloğu--veri kataloğu hazırlama](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)

5. Genişletin **fiyatlandırma** ve Azure veri kataloğunuz doğrulayın **edition** (ücretsiz veya standart).

    ![Azure Veri Kataloğu--sürüm seçme](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)

6. Seçerseniz *standart* edition fiyatlandırma katmanınızı olarak genişletebilirsiniz **güvenlik grupları** ve yetkilendirme veri kataloğuna erişecek ve otomatik ayarlamasını etkinleştirmek için Active Directory güvenlik gruplarını etkinleştir Faturalama.

    ![Azure veri Kataloğu güvenlik grupları](media/data-catalog-get-started/data-catalog-standard-security-groups.png)

7. **Katalog Kullanıcıları** seçeneğini genişletin ve **Ekle**’ye tıklayarak veri kataloğu için kullanıcı ekleyin. Bu gruba otomatik olarak eklenir.

    ![Azure Veri Kataloğu--kullanıcılar](media/data-catalog-get-started/data-catalog-add-catalog-user.png)

8. Seçerseniz *standart* edition fiyatlandırma katmanınızı olarak genişletebilirsiniz **sözlük yöneticileri** tıklatıp **Ekle** sözlüğü yönetici kullanıcıları eklemek için. Bu gruba otomatik olarak eklenir.

    ![Azure veri Kataloğu sözlük yöneticileri](media/data-catalog-get-started/data-catalog-standard-glossary-admin.png)

9. **Katalog Yöneticileri** seçeneğini genişletin ve **Ekle**’ye tıklayarak veri kataloğu için başka yöneticiler ekleyin. Bu gruba otomatik olarak eklenir.

    ![Azure Veri Kataloğu--yöneticiler](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)

10. Genişletin **Portal Başlığı** portalı başlığında görüntülenecek ek metin ekleyin.

    ![Azure veri Kataloğu portalının başlığı](media/data-catalog-get-started/data-catalog-portal-title.png)

11. Tamamladıktan sonra **ayarları** sayfasında, ileri gitmek **Yayımla** sayfası.

    ![Azure Veri Kataloğu--oluşturuldu](media/data-catalog-get-started/data-catalog-created.png)

## <a name="find-a-data-catalog-in-the-azure-portal"></a>Azure portalında veri kataloğu bulma

1. Web tarayıcısının ayrı bir sekmesinde veya ayrı bir web tarayıcısı penceresinde [Azure portalına](https://portal.azure.com) gidin ve önceki adımda veri kataloğunu oluşturmak için kullandığınız hesabın aynısıyla oturum açın.

2. Seçin **tüm hizmetleri** ve ardından **veri Kataloğu**.

    ![Azure veri Kataloğu--Azure'a göz atın](media/data-catalog-get-started/data-catalog-browse-azure-portal.png)

    Oluşturduğunuz veri kataloğunu görürsünüz.

    ![Azure Veri Kataloğu--kataloğu listede görüntüleme](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)

3. Oluşturduğunuz kataloğa tıklayın. Portalda **Veri Kataloğu** dikey penceresini görürsünüz.

   ![Azure Veri Kataloğu--portaldaki dikey pencere](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)

4. Veri kataloğunun özelliklerini görüntüleyebilir ve güncelleştirebilirsiniz. Örneğin, **Fiyatlandırma katmanı**’na tıklayın ve sürümü değiştirin.

    ![Azure Veri Kataloğu--fiyatlandırma katmanı](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, kuruluşunuz için bir Azure veri Kataloğu oluşturulacağını öğrendiniz. Veri kaynakları veri Kataloğu'nda artık kaydedebilirsiniz.

> [!div class="nextstepaction"]
> [Azure veri Kataloğu'nda veri kaynaklarını kaydetme](data-catalog-how-to-register.md)
