---
title: "Jenkins ve Azure kapsayıcı örneği kullanarak azure'da bir projeyi derleme"
description: "Azure kapsayıcı örnekleri ile azure'da bir projeyi derlemek için Jenkins için Azure kapsayıcı Aracısı eklentisi kullanmayı öğrenin"
author: tomarcher
manager: rloutlaw
ms.service: multiple
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: tarcher
ms.custom: Jenkins
ms.openlocfilehash: fc3ad4b68e29e9bd5666bb115306b452d074f682
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="build-a-project-in-azure-using-jenkins-and-azure-container-instances"></a>Jenkins ve Azure kapsayıcı örneği kullanarak azure'da bir projeyi derleme

Azure kapsayıcı örnekleri daha kolay hale getirmek ve çalışan sanal makineler sağlamak veya bir üst düzey hizmet benimsemeyi zorunda kalmadan kolaylaştırır. Azure kapsayıcı örnekleri ihtiyacınız kapasitesini temel saniyede faturalama sağlar; bir yapı gerçekleştirme gibi geçici iş yükleri için çekici bir seçenek yapılıyor.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:
> [!div class="checklist"]
> * Yükleme ve Azure üzerinde bir Jenkins sunucusu yapılandırma
> * Yükleme ve Azure kapsayıcı aracıları eklentisi Jenkins için yapılandırma
> * Azure kapsayıcı örnekleri oluşturmak için kullanmak [yay PetClinic örnek uygulama](https://github.com/spring-projects/spring-petclinic)

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - Azure satın alma seçenekleri hakkında bilgi edinmek için bkz: [Azure satın alma](https://azure.microsoft.com/pricing/purchase-options/) veya [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI 2.0 veya Azure bulut Kabuk** -hangi Azure komutları girmenizi içine aşağıdaki ürünlerden biri yükleyin:

    - [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) -Azure komutları bir komut veya terminal penceresi çalıştırmanıza olanak sağlar.
    - [Azure bulut Kabuk](/azure/cloud-shell/quickstart.md) - tarayıcı tabanlı kabuk deneyimi. Bulut Kabuk aklınızda Azure yönetim görevleri ile yerleşik bir tarayıcı tabanlı komut satırı deneyimi erişmesini sağlar.

## <a name="install-a-jenkins-server-on-azure-using-the-jenkins-marketplace-image"></a>Jenkins Market görüntüsü kullanarak Azure'da bir Jenkins sunucusu yükleyin

Jenkins burada server temsilciler projeleri çok sayıda konak veya farklı ortamlar için gerekli sağlamak için tek bir Jenkins yüklenmesine izin vermek için bir veya daha fazla aracılar için iş Jenkins derlemeler modelini destekler veya sınar. Bu bölümdeki adımları yüklerken ve Azure üzerinde bir Jenkins sunucusunu yapılandırırken size kılavuzluk.

[!INCLUDE [jenkins-install-from-azure-marketplace-image](../../includes/jenkins-install-from-azure-marketplace-image.md)]

## <a name="connect-to-the-jenkins-server-running-on-azure"></a>Azure üzerinde çalışan Jenkins sunucuya bağlanın

Azure üzerinde Jenkins yükledikten sonra Jenkins için bağlanmanız gerekir. Aşağıdaki adımlar, Azure üzerinde çalışan Jenkins VM için bir SSH bağlantısı kurma yol. 

[!INCLUDE [jenkins-connect-to-jenkins-server-running-on-azure](../../includes/jenkins-connect-to-jenkins-server-running-on-azure.md)]

## <a name="update-jenkins-dns"></a>Update Jenkins DNS

Jenkins geri kendisine işaret bağlantılar oluştururken kendi URL bilmesi gerekir. Örneğin, URL Jenkins sonuçları oluşturmak için doğrudan bağlantılar içeren e-posta gönderirken kullanılmalıdır. 

Bu bölümde, Jenkins URL ayarlanırken aracılığıyla açıklanmaktadır.

1. Jenkins Panosu'nda tarayıcınızda gidin `http://localhost:8080`.

1. Seçin **Jenkins yönetmek**.

    ![Jenkins panosunda Jenkins seçeneklerini yönetin](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Seçin **sistem yapılandırma**.

    ![Jenkins eklentileri seçeneği Jenkins panosunda yönetme](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. Altında **Jenkins konumu**, Jenkins sunucunuzun URL'sini girin.

1. **Kaydet**’i seçin.

## <a name="update-jenkins-to-allow-java-network-launch-protocol-jnlp"></a>Java ağ Başlatma Protokolü (JNLP) izin verecek şekilde Jenkins güncelleştir

Jenkins aracı Jenkins sunucusu aracılığıyla Java ağ Başlatma Protokolü (JNLP) ile bağlanır. Bu bölümde, JNLP aracılar Jenkins sunucuyla iletişim kurarken kullanması için bir bağlantı noktası belirtin açıklanmaktadır.

1. Jenkins panosunda seçin **yönetmek Jenkins**.

    ![Jenkins panosunda Jenkins seçeneklerini yönetin](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Seçin **genel güvenlik yapılandırma**.

    ![Jenkins Panoda genel güvenliği yapılandırma](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-global-security.png)

1. Altında **aracıları**seçin **sabit**ve bir bağlantı noktası girin. Örnek olarak bir bağlantı noktası değeri 12345 gösteren ekran görüntüsü. Ortamınız için anlamlı bir bağlantı noktası belirtmeniz gerekir.

    ![JNLP izin vermek için Jenkins genel güvenlik ayarlarını güncelleştirme](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-set-jnlp.png)

1. **Kaydet**’i seçin.

1. Azure CLI 2.0 veya Bulut Kabuğu'nu kullanarak, Jenkins ağ güvenlik grubu için bir gelen kuralı oluşturmak için aşağıdaki komutu girin:

    ```azurecli
    az network nsg rule create  \
    --resource-group JenkinsResourceGroup \
    --nsg-name JenkinsNSG  \
    --name allow-https \
    --description "Allow access to port 12345 for HTTPS" \
    --access Allow \
    --protocol Tcp  \
    --direction Inbound \
    --priority 102 \
    --source-address-prefix "*"  \
    --source-port-range "*"  \
    --destination-address-prefix "*" \
    --destination-port-range "12345"
    ```

## <a name="create-and-add-an-azure-service-principal-to-the-jenkins-credentials"></a>Oluşturma ve Jenkins kimlik bilgileri için bir Azure hizmet sorumlusu ekleme

Azure'a dağıtmak için bir Azure hizmet sorumlusu gerekir. Aşağıdaki adımları (, zaten yoksa) bir hizmet sorumlusu oluşturma ve Jenkins hizmet sorumlusuyla güncelleştirme işleminde size kılavuzluk eder.

1. Zaten bir hizmet sorumlusu varsa (ve abonelik kimliği, Kiracı, AppID ve parolayı biliyor varsa), bu adımı atlayabilirsiniz. Bir güvenlik sorumlusu oluşturmanız gerekiyorsa, makalesine başvurun [Azure, Azure CLI 2.0 ile hizmet sorumlusu oluşturmak](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Asıl oluşturulurken, abonelik kimliği, Kiracı, AppID ve parola değerlerini not edin.

1. Seçin **kimlik bilgileri**.

    ![Kimlik bilgileri seçeneği Jenkins panosunda yönetme](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials.png)

1. Altında **kimlik bilgileri**seçin **sistem**.

    ![Sistem kimlik bilgileri seçeneği Jenkins panosunda yönetme](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-system.png)

1. Seçin **(sınırsız) genel kimlik bilgileri**.

    ![Genel sistem kimlik bilgileri seçeneği Jenkins panosunda yönetme](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-global.png)

1. Seçin **bazı kimlik bilgileri eklenirken**.

    ![Jenkins panosunda kimlik bilgileri Ekle](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-adding-credentials.png)

1. İçinde **türü** açılır listesinden, **Microsoft Azure hizmet sorumlusu** alanlarını belirli bir hizmet sorumlusu ekleme ile görüntülemek sayfanın neden olacak. Ardından, istenen değerleri aşağıdaki gibi belirtin:

    - **Kapsam** -seçeneğini belirleyin **Global (Jenkins, düğümleri, öğeleri, tüm alt öğeleri, vs.)** .
    - **Abonelik kimliği** -çalıştırılırken belirtilen Azure abonelik kimliği kullanmak `az account set`.
    - **İstemci kimliği** -kullanım `appId` döndürülen değer `az ad sp create-for-rbac`.
    - **İstemci parolası** -kullanım `password` döndürülen değer `az ad sp create-for-rbac`.
    - **Kiracı kimliği** -kullanım `tenant` döndürülen değer `az ad sp create-for-rbac`.
    - **Azure ortamı** - seçin `Azure`.
    - **Kimliği** -girin `myTestSp`. Bu değer, bu öğreticide daha sonra yeniden kullanılır.
    - **Açıklama** - (isteğe bağlı) bu sorumlusu için bir açıklama değeri girin.

    ![Jenkins Panoda yeni hizmet asıl özelliklerini belirtin](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-principal-properties.png)

1. Asıl tanımlama bilgileri girdikten sonra isteğe bağlı olarak seçebileceğiniz **doğrulayın hizmet sorumlusu** her şeyin doğru şekilde çalıştığından emin olmak için. Hizmet sorumlusu doğru şekilde tanımlandığından, ileti belirten "başarıyla doğrulandı Microsoft Azure hizmet sorumlusu." konusuna bakın Aşağıda **açıklama** alan.

1. İşiniz bittiğinde, seçin **Tamam** Jenkins için asıl eklemek için. Yeni eklenen asıl Jenkins Pano görüntüler **genel kimlik bilgileri** sayfası.

## <a name="create-an-azure-resource-group-for-your-azure-container-instances"></a>Azure kapsayıcı örnekleri için bir Azure kaynak grubu oluşturun

Azure kapsayıcı örnekleri bir Azure kaynak grubunda yer almalıdır. Bir Azure kaynak grubu Azure çözümünü ilgili kaynaklara tutan bir kapsayıcıdır.

Azure CLI 2.0 veya Bulut Kabuğu'nu kullanarak adlı bir kaynak grubu oluşturmak için aşağıdaki komutu girin `JenkinsAciResourceGroup` konumda `eastus`:

```azurecli
az group create --name JenkinsAciResourceGroup --location eastus
```

Tamamlandığında, `az group create` komutu aşağıdaki örneğe benzer sonuçlar görüntüler:

```JSON
{
  "id": "/subscriptions/<subscriptionId>/resourceGroups/JenkinsAciResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "JenkinsAciResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="install-the-azure-container-agents-plugin-for-jenkins"></a>Jenkins için Azure kapsayıcı aracıları eklentisini yükleme

Azure kapsayıcı aracıları eklenti zaten yüklediyseniz, bu bölümü atlayabilirsiniz.

Jenkins aracınız için oluşturulan Azure kaynak grubu oluşturduktan sonra aşağıdaki adımları Azure kapsayıcı aracıları eklentiyi yüklemek üzere nasıl gösterilmektedir:

1. Jenkins panosunda seçin **yönetmek Jenkins**.

    ![Jenkins panosunda Jenkins seçeneklerini yönetin](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Seçin **eklentileri yönetme**.

    ![Jenkins eklentileri seçeneği Jenkins panosunda yönetme](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-plugins.png)

1. Seçin **kullanılabilir**.

    ![Jenkins eklentileri seçenek Jenkins panosunda Göster](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-view-available-plugins.png)

1. Girin `Azure Container Agents` içine **filtre** metin kutusu. (Metni girerken listesini filtreler.)

    ![Kullanılabilen Jenkins eklentileri Jenkins panosunda filtreleme](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-filter-available-plugins.png)

1. Yanındaki onay kutusunu işaretleyin **Azure kapsayıcı aracıları** eklentisi ve yükleme seçenekleri. Bu tanıtım amacıyla, ı seçtiğiniz **yeniden yükleme** seçeneği.

    ![Azure kapsayıcı aracıları eklentileri Jenkins panodan yükleyin](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin.png)

1.  İstenen plugin(s) yükleme seçeneği seçtikten sonra Jenkins Pano yüklemekte durumunu ayrıntılı sayfasını görüntüler.

    ![Azure kapsayıcı aracıları eklentileri Jenkins panodan yükleme yükleme durumu](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin-confirmation.png)

    Jenkins panonun ana sayfaya dönün seçin **ilk sayfasına geri dönün**.

## <a name="configure-the-azure-container-agents-plugin"></a>Azure kapsayıcı aracıları eklentisi yapılandırma

Azure kapsayıcı aracıları eklentisi yüklendikten sonra bu bölümde Jenkins Pano içinde eklentisi yapılandırma konusunda size rehberlik eder.

1. Jenkins panosunda seçin **yönetmek Jenkins**.

    ![Jenkins panosunda Jenkins seçeneklerini yönetin](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Seçin **sistem yapılandırma**.

    ![Jenkins eklentileri seçeneği Jenkins panosunda yönetme](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. Bulun **bulut** sayfasının ve gelen bölümü altındaki **yeni bir bulut eklemek** açılır listesinden, **Azure kapsayıcı örneği**.

    ![Jenkins panodan yeni bir bulut Sağlayıcısı Ekle](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-cloud-provider.png)

1. İçinde **Azure kapsayıcı örneği** bölümünde, aşağıdaki değerleri belirtin:

    - **Bulut adı** - (isteğe bağlı olarak bu değeri otomatik olarak oluşturulan bir adı olan varsayılan olarak.) Bu örneği için bir ad belirtin. 
    - **Azure kimlik bilgisi** - aşağı açılan oku tıklatın ve ardından seçin `myTestSp` Azure hizmet sorumlusu tanımlayan giriş daha önce oluşturduğunuz.
    - **Kaynak grubu** - aşağı açılan oku tıklatın ve ardından seçin `JenkinsAciResourceGroup` Azure kapsayıcı örneği kaynak grubunu tanımlar giriş daha önce oluşturduğunuz.

    ![Azure kapsayıcı örneği özellikleri tanımlama](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-properties.png)

1. Seçin **kapsayıcı Şablon Ekle** açılır okuna ve ardından **Aci kapsayıcı şablon**.

1. İçinde **Aci kapsayıcı şablonu** bölümünde, aşağıdaki değerleri belirtin:

    - **Ad** -girin `ACI-container`.
    - **Etiketleri** -girin `ACI-container`.
    - **Docker görüntü** -girin `cloudbees/jnlp-slave-with-java-build-tools`

    ![Azure kapsayıcı örneği görüntü özelliklerini tanımlama](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-image-properties.png)

1. Seçin **Gelişmiş**.

1. Seçin **bekletme stratejisi** açılan oku seçip **kapsayıcı boşta bekletme stratejisi**. Bu seçeneği belirterek Jenkins aracı hiçbir yeni iş aracı üzerinde yürütülür ve belirtilen boşta kalma süresi geçti kadar tutar.

    ![Azure kapsayıcı örneği bekletme strateji tanımlama](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-retention-strategy.png)

1. **Kaydet**’i seçin.

## <a name="create-the-spring-petclinic-application-job-in-jenkins"></a>Jenkins içinde Spring PetClinic uygulama işi oluşturma

Aşağıdaki adımları Jenkins iş - yay PetClinic uygulama oluşturmak için serbest stilde proje - olarak oluşturmada size yol.

1. Jenkins panosunda seçin **yeni öğe**.

    ![Jenkins panosunda yeni öğe menü seçeneği](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-item.png)

1. Girin `myPetClinicProject` öğe adı ve select **Serbest stilde proje**.

    ![Jenkins panosunda Serbest stilde yeni proje](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project.png)

1. **Tamam**’ı seçin.

1. Seçin **genel** sekmesini tıklatın ve aşağıdaki değerleri belirtin:

    - **Proje çalıştırdığı kısıtlamak** -bu seçeneği belirleyin.
    - **Etiket ifade** -girin `ACI-container`. Alan çıktığınızda etiketi önceki adımda oluşturduğunuz Bulutu yapılandırması tarafından sunulan onaylayan bir ileti görüntüler.

    ![Jenkins panosunda yeni bir Serbest stilde proje için genel ayarları](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project-general.png)

1. Seçin **kaynak kodu Yönetimi** sekmesini tıklatın ve aşağıdaki değerleri belirtin:

    - **Kaynak kodu Yönetimi** - seçin **Git**.
    - **Depo URL'si** -yay PetClinic örnek uygulama GitHub depo için aşağıdaki URL'yi girin: `https://github.com/spring-projects/spring-petclinic.git`.

1. Seçin **yapı** sekmesini tıklatın ve aşağıdaki görevleri gerçekleştirin:

    - Seçin **Ekle derleme adımı** açılan oku seçip **en üst düzey Maven hedefleri çağırma**.

    - İçin **hedefleri**, girin `package`.

1. Seçin **kaydetmek** yeni proje tanımını kaydetmek için.

## <a name="build-the-spring-petclinic-application-job-in-jenkins"></a>Jenkins içinde Spring PetClinic uygulama işi oluşturma

Projenizi derleme için zamanı geldi! Bu bölümde, Jenkins panodan bir projeyi derleme açıklanmaktadır.

1. Jenkins panosunda seçin `myPetClinicProject`.

    ![Jenkins panodan oluşturmak için projesini seçin.](./media/azure-container-agents-plugin-run-container-as-an-agent/select-project-to-build.png)

1. Seçin **şimdi yapı**. 

    ![Jenkins panosundan projesi oluşturun.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-project.png)

1. Jenkins bir yapı başlattığınızda, yapıyı sıraya alındı. Azure kapsayıcı aracısı başlatıldı ve çevrimiçi duruma kadar bir Azure kapsayıcı aracı söz konusu olduğunda, derleme çalıştırılamaz. O zamana kadar aracı adı ve yapı Beklemede olgu belirten bir ileti görürsünüz. (Bu işlem yaklaşık beş dakika sürer ancak yalnızca ilk kez bir yapı aracısı'nı kullandığınızda gereklidir. Sonraki derlemeleri Aracısı'nı bu noktada çevrimiçi olduğundan çok daha hızlıdır.)

    ![Aracı oluşturulur ve çevrimiçi duruma kadar yapıyı sıraya alındı.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-pending.png)

    Yapı başladıktan sonra yapı çalışmadığını şeritli kutbu'na ilerleme çubuğu gösterir:

    ![Şeritli kutbu'na ilerleme çubuğu gördüğünüzde yapı çalışıyor.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-running.png)

1. Şeritli kutbu'na ilerleme çubuğu kaybolduğunda, yapı numarası yanındaki oka seçin. Bağlam menüsünden seçin **konsol çıkış**.

    ![Derleme bilgilerini görüntülemek için konsolu günlük menü öğesini tıklatın.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-menu.png)

1. Derleme işleminden yayılan konsol günlük bilgileri. (Azure Aracısı'nı uzaktan gerçekleştirilen yapı hakkındaki bilgiler, oluşturduğunuz dahil) tüm yapı bilgileri görüntülemek için seçin **tam günlük**.

    ![Daha ayrıntılı yapılandırma bilgilerini görüntülemek için tam günlük bağlantısına tıklayın.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log.png)

    Tam günlük daha ayrıntılı yapı bilgileri görüntüler:

    ![Tam günlük daha ayrıntılı yapılandırma bilgilerini görüntüler.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-full.png)
    
1. Yapı değerlendirme görüntülemek için günlük en altına gidin.

    ![Derleme bırakma derleme günlüğünde altında görüntüler.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-disposition.png)

## <a name="clean-up-azure-resources"></a>Azure kaynakları temizlemek

Bu öğreticide, iki Azure kaynak grubu içinde bulunan kaynaklar oluşturuldu: 
    - `JenkinsResourceGroup` -Jenkins server için Azure kaynaklarını içerir.
    - `JenkinsAciResourceGroup` -Jenkins aracı için Azure kaynaklarını içerir.
    
Artık herhangi bir kaynağa bir Azure kaynak grubu kullanmanız gerekiyorsa, kaynak grubunu kullanarak silebilirsiniz `az group delete` gibi komut (değiştirme &lt;resourceGroup > yer tutucu istediğiniz kaynak grubunun adı Sil):

```azurecli
az group delete -n <resourceGroup>
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Jenkins en son makaleleri ve örnekleri görmek için Azure hub'ına ziyaret edin](https://docs.microsoft.com/azure/jenkins/)