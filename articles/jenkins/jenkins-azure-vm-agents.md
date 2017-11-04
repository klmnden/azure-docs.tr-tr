---
title: "Azure VM Aracısı Jenkins dağıtımlarında ölçeklendirin."
description: "Azure sanal makineleri Jenkins Azure VM Aracısı ile eklentisini kullanarak Jenkins hatlarınızı ek kapasite ekleyin."
services: multiple
documentationcenter: 
author: rloutlaw
manager: justhe
ms.service: multiple
ms.workload: multiple
ms.topic: article
ms.date: 8/25/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: dbb30809ab68079666ecfa81a896c1d5101fb6fb
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="scale-your-jenkins-deployments-to-meet-demand-with-azure-vm-agents"></a>Azure VM aracılarla talebi karşılamak üzere Jenkins dağıtımlarınızı ölçeklendirme

Bu öğretici Jenkins kullanmayı gösterir [Azure VM Aracısı eklentisi](https://plugins.jenkins.io/azure-vm-agents) Azure'da çalışan Linux sanal makineleri ile isteğe bağlı kapasite eklemek için.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Azure VM Aracısı eklentisini yükleme
> * Azure aboneliğinizde kaynak oluşturmak için eklentiyi yapılandırmak
> * Kullanılabilir bilgi işlem kaynakları için her bir aracının ayarlayın
> * İşletim sistemi ve her bir aracının yüklü araçları ayarlayın
> * Yeni bir Jenkins Serbest stilde işi oluştur
> * Bir Azure VM Aracısı işini çalıştır

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents/player]

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure aboneliği
* Jenkins ana sunucu. Yoksa, görüntülemek [Hızlı Başlangıç](install-jenkins-solution-template.md) Azure birinde ayarlamak için.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-azure-vm-agents-plugin"></a>Azure VM Aracıları eklentisini yükleme

> [!TIP]
> Azure kullanarak Jenkins dağıttıysanız [çözüm şablonu](install-jenkins-solution-template.md), Azure VM Aracısı eklenti zaten yüklü.

1. Jenkins panodan seçin **yönetmek Jenkins**seçeneğini belirleyip **eklentileri yönetme**.
2. Seçin **kullanılabilir** sekmesini ve ardından arama **Azure VM Aracısı**. Seçin ve eklentisi için girişi yanındaki onay kutusunu işaretleyin **yeniden yükleme** panonun Alttan.

## <a name="configure-the-azure-vm-agents-plugin"></a>Azure VM Aracısı eklentisi yapılandırma

1. Jenkins panodan seçin **yönetmek Jenkins**, ardından **yapılandırma sistem**.
2. Sayfanın alt kısmına kaydırın ve bulma **bulut** ile bölümünde **yeni bulut eklemek** açılır ve **Microsoft Azure VM Aracısı**.
3. Gelen var olan bir hizmet sorumlusu seçin **Ekle** açılan **Azure kimlik bilgilerini** bölümü. Listeleniyorsa, için aşağıdaki adımları gerçekleştirin [bir hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager) için Azure hesabı ve Jenkins yapılandırmanıza ekleyin:   

    a. Seçin **Ekle** yanına **Azure kimlik bilgilerini** ve **Jenkins**.   
    b. İçinde **kimlik bilgilerini eklemek** iletişim kutusunda **Microsoft Azure hizmet sorumlusu** gelen **türü** açılır.   
    c. Azure CLI üzerinden bir Active Directory Hizmet sorumlusu oluşturmak veya [bulut Kabuk](/azure/cloud-shell/overview).
    
    ```azurecli-interactive
    az ad sp create-for-rbac --name jenkins_sp --password secure_password
    ```

    ```json
    {
        "appId": "BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBB",
        "displayName": "jenkins_sp",
        "name": "http://jenkins_sp",
        "password": "secure_password",
        "tenant": "CCCCCCCC-CCCC-CCCC-CCCCCCCCCCC"
    }
    ```
    d. Hizmet sorumlusu içine arasında kimlik bilgilerini girin **kimlik bilgileri ekleyin** iletişim. Azure abonelik Kimliğinizi bilmiyorsanız, CLI üzerinden sorgulama yapabilirsiniz:
     
     ```azurecli-interactive
     az account list
     ```

     ```json
        {
            "cloudName": "AzureCloud",
            "id": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA",
            "isDefault": true,
            "name": "Visual Studio Enterprise",
            "state": "Enabled",
            "tenantId": "CCCCCCCC-CCCC-CCCC-CCCC-CCCCCCCCCCC",
            "user": {
            "name": "raisa@fabrikam.com",
            "type": "user"
            }
     ```

    Tamamlanmış hizmet sorumlusu kullanması gereken `id` alanındaki **abonelik kimliği**, `appId` değerini **istemci kimliği**, `password` için **gizli**ve bir URL **OAuth 2.0 belirteç uç noktası** , `https://login.windows.net/<tenant_value>`. Seçin **Ekle** hizmet sorumlusu ekleyin ve ardından yeni oluşturulan kimlik bilgisi kullanmak için eklentiyi yapılandırmak için.

    ![Azure hizmet sorumlusu yapılandırın](./media/jenkins-azure-vm-agents/new-service-principal.png)

    

4. İçinde **kaynak grubu adı** bölümünde, bırakın **Yeni Oluştur** seçili ve girin `myJenkinsAgentGroup`.
5. Seçin **doğrulama yapılandırma** profil ayarlarını test etmek için Azure'a bağlanmak için.
6. Seçin **Uygula** eklentisi yapılandırmasını güncelleştirmek için.

## <a name="configure-agent-resources"></a>Aracı kaynaklarını yapılandırma

Bir Azure VM Aracısı tanımlamak kullanmak için bir şablon yapılandırın. Bu şablon her aracı olan işlem kaynakları tanımlayan oluşturuldu.

1. Seçin **Ekle** yanına **Azure sanal makine şablonu ekleme**.
2. Girin `defaulttemplate` için **adı**
3. Girin `ubuntu` için **etiketi**
4. İstenen seçin [Azure bölgesi](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) açılan kutusundan.
5. Seçin bir [VM boyutu](/azure/virtual-machines/linux/sizes) açılan gelen **sanal makine boyutu**. Bir genel amaçlı `Standard_DS1_v2` boyutudur ayrıntılı Bu öğretici için.   
6. Bırakın **bekletme süresini** adresindeki `60`. Bu ayar, boşta aracıları serbest önce Jenkins bekleyebilirsiniz dakika sayısını tanımlar. Boşta aracıları otomatik olarak kaldırılmasını istemiyorsanız, 0 değerini belirtin.

   ![Genel VM yapılandırması](./media/jenkins-azure-vm-agents/general-config.png)

## <a name="configure-agent-operating-system-and-tools"></a>Aracı işletim sistemi ve araçları yapılandırma

İçinde **görüntü yapılandırmasını** select eklentisi yapılandırma bölümünü **Ubuntu 16.04 LTS**. Yanındaki kutuları işaretleyin **Git yükleyin (son)**, **yükleme Maven (V3.5.0)**, ve **yükleme Docker** bu araçları yeni oluşturulan aracıları yüklemek için.

![VM işletim sistemi ve araçları yapılandırma](./media/jenkins-azure-vm-agents/jenkins-os-config.png)

Seçin **Ekle** yanına **yönetici kimlik bilgileri**seçeneğini belirleyip **Jenkins**. Bir kullanıcı adı ve bunlar karşılamak emin aracıları için oturum açmak için kullanılan parolayı girin [kullanıcı adı ve parola ilkesi](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm) Azure vm'lerinde yönetici hesapları için.

Seçin **doğrulayın şablonu** yapılandırmasını doğrulayın ve ardından **kaydetmek** yaptığınız değişiklikleri kaydetmek ve Jenkins panoya geri dönün.

## <a name="create-a-job-in-jenkins"></a>Jenkins içinde iş oluşturma

1. Jenkins panosunda **Yeni Öğe**’ye tıklayın. 
2. Girin `demoproject1` seçin ve ad için **Serbest stilde proje**seçeneğini belirleyip **Tamam**.
3. İçinde **genel** sekmesinde, seçin **burada proje çalıştırılabilir sınırla** ve türü `ubuntu` içinde **etiket ifadesi**. Etiket önceki adımda oluşturduğunuz Bulutu yapılandırması tarafından sunulan onaylayan bir ileti görür. 
   ![İş ayarlayın](./media/jenkins-azure-vm-agents/job-config.png)
4. İçinde **kaynak kodu Yönetimi** sekmesine **Git** ve aşağıdaki URL'yi içine ekleyin **depo URL'si** alan:`https://github.com/spring-projects/spring-petclinic.git`
5. İçinde **yapı** sekmesine **Ekle derleme adımı**, ardından **en üst düzey Maven hedefleri çağırma**. Girin `package` içinde **hedefleri** alan.
6. Seçin **kaydetmek** iş tanımı kaydetmek için.

## <a name="build-the-new-job-on-an-azure-vm-agent"></a>Bir Azure VM aracısını yeni proje oluşturma

1. Jenkins panosuna geri dönün.
2. Önceki adımda oluşturduğunuz işi seçin ve ardından **şimdi yapı**. Yeni bir yapı sıraya alındı, ancak bir aracı VM, Azure aboneliğinizde oluşturulana kadar başlatılmaz.
3. Derleme tamamlandıktan sonra **Konsol çıktısı**’na gidin. Yapı Azure aracıyı uzaktan gerçekleştirildiğini bakın.

![Konsol çıktısı](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [CI/CD'ye, Azure uygulama hizmeti](java-deploy-webapp-tutorial.md)
