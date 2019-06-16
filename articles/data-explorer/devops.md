---
title: Azure DevOps görev için Azure Veri Gezgini
description: Bu konu başlığında, bir yayın ardışık düzeni oluşturmak ve dağıtmak öğrenin
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 05/05/2019
ms.openlocfilehash: 0628d5c07d7258cc4d68727c364e65bd81c78e8e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66388986"
---
# <a name="azure-devops-task-for-azure-data-explorer"></a>Azure DevOps görev için Azure Veri Gezgini

[Azure DevOps hizmetleriyle](https://azure.microsoft.com/services/devops/) geliştirme gibi yüksek performanslı işlem hatları, ücretsiz özel Git depoları, sağlanan yapılandırılabilir Kanban panoları ve kapsamlı otomatik ve sürekli test etme özellikleri işbirliği araçları sağlar. [Azure işlem hatları](https://azure.microsoft.com/services/devops/pipelines/) herhangi bir dil, platformu ve bulut ile çalışan yüksek performanslı işlem hatları ile kodunuzu dağıtmak için CI/CD yönetmenize olanak sağlayan bir Azure DevOps özelliktir.
[Azure Veri Gezgini - yönetici komutları](https://marketplace.visualstudio.com/items?itemName=Azure-Kusto.PublishToADX) sayesinde sürüm ardışık düzenleri oluşturun ve veritabanınızı dağıtmak Azure işlem hatları görev, Azure Veri Gezgini veritabanlarınızı değiştirir. Ücretsiz olarak kullanılabilir [Visual Studio Market](https://marketplace.visualstudio.com/).

Bu belge, basit bir örnek kullanımını açıklar. **Azure Veri Gezgini – yönetici komutları** şemanızı dağıtmak için görev veritabanınıza değiştirir. Tam CI/CD işlem hatları için başvurmak [Azure DevOps belgeleri](/azure/devops/user-guide/what-is-azure-devops?view=azure-devops#vsts).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* Azure Veri Gezgini Küme kurulumu:
    * Bir [Azure Veri Gezgini kümesi ile veritabanı](/azure/data-explorer/create-cluster-database-portal)
    * Azure Active Directory (Azure AD) uygulama tarafından oluşturma [bir Azure AD uygulaması sağlama](/azure/kusto/management/access-control/how-to-provision-aad-app).
    * Azure AD uygulamanızı Azure Veri Gezgini veritabanınız için erişim verin [Azure Veri Gezgini veritabanı izinlerinin yönetilmesi](/azure/data-explorer/manage-database-permissions).
* Azure DevOps Kurulumu:
    * [Ücretsiz bir kuruluş için kaydolun](/azure/devops/user-guide/sign-up-invite-teammates?view=azure-devops)
    * [Kuruluş oluştur](/azure/devops/organizations/accounts/create-organization?view=azure-devops)
    * [Azure DevOps proje oluşturma](/azure/devops/organizations/projects/create-project?view=azure-devops)
    * [Git ile kod](/azure/devops/user-guide/code-with-git?view=azure-devops)

## <a name="create-folders"></a>Klasör oluşturma

Aşağıdaki örnek klasörler oluşturmanız (*işlevleri*, *ilkeleri*, *tabloları*) Git deponuzda. Dosyaların kopyalanacağı [burada](https://github.com/Azure/azure-kusto-docs-samples/tree/master/DevOps_release_pipeline) aşağıdaki karşılık gelen klasörlere olarak görülen ve değişiklikleri uygulayın. Örnek dosyaları, aşağıdaki iş akışını yürütmek için sağlanır.

![Klasör oluşturma](media/devops/create-folders.png)

> [!TIP]
> Kendi akışınızı oluştururken, kodu bir kere etkili hale getirme öneririz. Örneğin, [.create birleştirme tablo](/azure/kusto/management/tables#create-merge-tables) yerine [.create tablo](/azure/kusto/management/tables#create-table)ve [.create-veya-alter](/azure/kusto/management/functions#create-or-alter-function) yerine işlev [.create](/azure/kusto/management/functions#create-function) işlev.

## <a name="create-a-release-pipeline"></a>Yayın işlem hattı oluşturma

1. Oturum açın, [Azure DevOps kuruluş](https://dev.azure.com/).
1. Seçin **işlem hatları** > **yayınlar** sol taraftaki menüsünden ve select **yeni işlem hattı**.

    ![Yeni işlem hattı](media/devops/new-pipeline.png)

1. **Yeni yayın işlem hattı** penceresi açılır. İçinde **işlem hatları** sekmesinde **bir şablon seçin** bölmesinde seçin **boş iş**.

     ![Bir şablon seçin](media/devops/select-template.png)

1. Seçin **aşama** düğmesi. İçinde **aşama** bölmesinde Ekle **aşama adı**. Seçin **Kaydet** işlem hattınızı kaydetmek için.

    ![Aşama adı](media/devops/stage-name.png)

1. Seçin **bir yapıt ekleme** düğmesi. İçinde **bir yapıt ekleme** bölmesinde kodunuzu bulunduğu depoyu seçin, ilgili bilgileri doldurun ve tıklayın **Ekle**. Seçin **Kaydet** işlem hattınızı kaydetmek için.

    ![Bir yapıt ekleme](media/devops/add-artifact.png)

1. İçinde **değişkenleri** sekmesinde **+ Ekle** için bir değişken oluşturmak için **uç nokta URL'si** görevde kullanılır. Yazma **adı** ve **değer** uç nokta. Seçin **Kaydet** işlem hattınızı kaydetmek için. 

    ![Değişken oluşturma](media/devops/create-variable.png)

    Genel bakış sayfasında bulunan, Endpoint_URL bulmak için **Azure Veri Gezgini küme** Azure portalı, Azure Veri Gezgini küme URI'si içerir. URI'yi şu biçimde oluşturmak `https://<Azure Data Explorer cluster URI>?DatabaseName=<DBName>`.  Örneğin, https:\//kustodocs.westus.kusto.windows.net?DatabaseName=SampleDB

    ![Azure Veri Gezgini küme URI'si](media/devops/adx-cluster-uri.png)

## <a name="create-tasks-to-deploy"></a>Dağıtmak için Görevler oluşturun

1. İçinde **işlem hattı** sekmesinde, üzerinde **1 iş, 0 görev** görevler eklemek için. 

    ![Görev ekleme](media/devops/add-task.png)

1. Dağıtmak için üç görev oluşturma **tabloları**, **işlevleri**, ve **ilkeleri**, sırasıyla. 

1. İçinde **görevleri** sekmesinde **+** tarafından **aracı işi**. **Azure Veri Gezgini** için arama yapın. İçinde **Market**, yükleme **Azure Veri Gezgini – yönetici komutları** uzantısı. Ardından, **Ekle** içinde **Azure Veri Gezgini komutu Çalıştır**.

     ![Yönetim komutları eklendi](media/devops/add-admin-commands.png)

1. Tıklayarak **Kusto komut** seçeneğini ve güncelleştirme görevi aşağıdaki bilgilerle:
    * **Görünen ad**: Görevin adı
    * **Dosya yolu**: İçinde **tabloları** belirtin, görev */Tables/* tablosu oluşturma dosyaları içinde olduğundan .csl *tablo* klasör.
    * **Uç nokta URL'si**: girin `EndPoint URL`önceki adımda oluşturduğunuz değişkenin.
    * Seçin **hizmet uç noktası** seçip **+ yeni**.

    ![Kusto komut görevi güncelleştir](media/devops/kusto-command-task.png)

1. Aşağıdaki bilgileri tamamlayın **hizmet Bağlantısı Ekle Azure Veri Gezgini** penceresi:

    |Ayar  |Önerilen değer  |
    |---------|---------|
    |**Bağlantı adı**     |    Bu hizmet uç noktası tanımlamak için bir ad girin     |
    |**Küme URL'si**    |    Değeri, Azure portalında Azure Veri Gezgini kümenizin genel bakış bölümünde bulunabilir | 
    |**Hizmet sorumlusu kimliği**    |    (Önkoşul olarak oluşturulan) AAD uygulama kimliği girin     |
    |**Hizmet sorumlusu uygulama anahtarı**     |    AAD uygulaması (önkoşul olarak oluşturulan) anahtarını girin    |
    |**AAD Kiracı kimliği**    |      AAD kiracınızın (örn. microsoft.com... contoso.com) girin    |

    Seçin **bu bağlantıyı kullanmak tüm işlem hatları izin** onay kutusu. **Tamam**’ı seçin.

    ![Hizmet Bağlantısı Ekle](media/devops/add-service-connection.png)

1. Adımları yineleyin 1-5 başka iki kez dosyaları dağıtmak için *işlevleri* ve *ilkeleri* klasörleri. **Kaydet**’i seçin. İçinde **görevleri** sekmesinde, oluşturduğunuz üç görev bakın: **Tabloları dağıtma**, **işlevleri dağıtma**, ve **ilkelerini dağıtmak**.

    ![Tüm klasörleri dağıtma](media/devops/deploy-all-folders.png)

1. Seçin **+ yayın** > **yayın oluştur** sürüm yapılandırması oluşturmak için.

    ![Bir yayın oluşturun](media/devops/create-release.png)

1. İçinde **günlükleri** sekmesinde, dağıtım durumu başarılı olup olmadığını denetleyin.

    ![Dağıtım başarılı](media/devops/deployment-successful.png)

Üç üretim öncesi görevlere dağıtımı için bir yayın ardışık düzeni oluşturulmasını tamamladınız.