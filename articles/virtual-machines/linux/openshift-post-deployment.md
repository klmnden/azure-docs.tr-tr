---
title: Dağıtım sonrası görevleri Azure üzerinde OpenShift | Microsoft Docs
description: Bir OpenShift kümesi dağıtıldıktan sonra ek görevler için.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/19/2019
ms.author: haroldw
ms.openlocfilehash: fba29cd55f2d765faa107de3a8961032ef44deec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60771366"
---
# <a name="post-deployment-tasks"></a>Dağıtım sonrası görevler

OpenShift küme dağıtıldıktan sonra ek öğelerini yapılandırabilirsiniz. Bu makalede ele alınmıştır:

- Azure Active Directory (Azure AD) kullanarak çoklu oturum açmayı yapılandırma
- OpenShift izlemek için Azure İzleyici günlüklerine yapılandırma
- Ölçümler ve günlüğe kaydetme yapılandırma
- Açık hizmet Aracısı (OSBA) Azure için yükleme

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak çoklu oturum açmayı yapılandırın

Azure Active Directory kimlik doğrulaması için kullanmak için önce bir Azure AD uygulama kaydı oluşturmanız gerekir. Bu işlem iki adımdan oluşur: uygulama kaydı oluşturma ve izinlerini yapılandırma.

### <a name="create-an-app-registration"></a>Bir uygulama kaydı oluşturma

Bu adımlar, uygulama kaydı ve izinler ayarlamak üzere GUI (portal) oluşturmak için Azure CLI'yı kullanın. Uygulama kaydı oluşturmak için şu beş bilgilere ihtiyacınız vardır:

- Görünen ad: Uygulama kayıt adı (örneğin, OCPAzureAD)
- Giriş sayfası: (Örneğin, OpenShift Konsolu URL'si https://masterdns343khhde.westus.cloudapp.azure.com/console)
- Tanımlayıcı URI'si: (Örneğin, OpenShift Konsolu URL'si https://masterdns343khhde.westus.cloudapp.azure.com/console)
- Yanıt URL'si: Genel URL ve uygulama kayıt adı (örneğin, ana https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD)
- Parola: Güvenli parola (güçlü bir parola kullanın)

Aşağıdaki örnek, önceki bilgileri kullanarak bir uygulama kaydı oluşturur:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com/console --password {Strong Password}
```

Komut başarılı olursa, bir JSON çıkışı benzer alın:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/console"
  ],
  "objectId": "62cd74c9-42bb-4b9f-b2b5-b6ee88991c80",
  "objectType": "Application",
  "replyUrls": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD"
  ]
}
```

Sonraki adım için komuttan döndürülen AppID özelliğini not edin.

Azure portalında:

1. Seçin **Azure Active Directory** > **uygulama kaydı**.
2. Uygulama kaydınızı (örneğin, OCPAzureAD) arayın.
3. Sonuçlarda uygulama kaydını tıklayın.
4. Altında **ayarları**seçin **gerekli izinler**.
5. Altında **gerekli izinler**seçin **Ekle**.

   ![Uygulama kaydı](media/openshift-post-deployment/app-registration.png)

6. 1. Adım'a tıklayın: API seçin ve ardından **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)**. Tıklayın **seçin** altındaki.

   ![Uygulama kaydı API seçimi](media/openshift-post-deployment/app-registration-select-api.png)

7. Adım 2: İzinleri seçin, **oturum açın ve kullanıcı profilini okuma** altında **Temsilcili izinler**ve ardından **seçin**.

   ![Uygulama kaydı erişim](media/openshift-post-deployment/app-registration-access.png)

8. **Done** (Bitti) öğesini seçin.

### <a name="configure-openshift-for-azure-ad-authentication"></a>OpenShift Azure AD kimlik doğrulamasını yapılandırma

OpenShift bir kimlik doğrulama sağlayıcısı olarak Azure AD'yi kullanacak şekilde yapılandırmak için tüm ana düğümler üzerinde /etc/origin/master/master-config.yaml dosyasının düzenlenmesi gerekir.

Aşağıdaki CLI komutunu kullanarak Kiracı Kimliğini bulun:

```azurecli
az account show
```

Yaml dosyasında aşağıdaki satırları bulun:

```yaml
oauthConfig:
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com/console/
  grantConfig:
    method: auto
  identityProviders:
  - challenge: true
    login: true
    mappingMethod: claim
    name: htpasswd_auth
    provider:
      apiVersion: v1
      file: /etc/origin/master/htpasswd
      kind: HTPasswdPasswordIdentityProvider
```

Önceki satırları hemen sonra aşağıdaki satırları ekleyin:

```yaml
  - name: <App Registration Name>
    challenge: false
    login: true
    mappingMethod: claim
    provider:
      apiVersion: v1
      kind: OpenIDIdentityProvider
      clientID: <appId>
      clientSecret: <Strong Password>
      claims:
        id:
        - sub
        preferredUsername:
        - unique_name
        name:
        - name
        email:
        - email
      urls:
        authorize: https://login.microsoftonline.com/<tenant Id>/oauth2/authorize
        token: https://login.microsoftonline.com/<tenant Id>/oauth2/token
```

Metni doğru altında identityProviders hizalar emin olun. Aşağıdaki CLI komutunu kullanarak Kiracı Kimliğini bulun: ```az account show```

Tüm ana düğüm üzerinde OpenShift Yöneticisi hizmetlerini yeniden başlatın:

```bash
sudo /usr/local/bin/master-restart api
sudo /usr/local/bin/master-restart controllers
```

OpenShift konsolunda, artık kimlik doğrulaması için iki seçenek görürsünüz: htpasswd_auth ve [uygulama kaydı].

## <a name="monitor-openshift-with-azure-monitor-logs"></a>Azure İzleyici ile izleme OpenShift günlüğe kaydeder

OpenShift için Log Analytics aracısını eklemenin üç yolu vardır.
- Linux için Log Analytics aracısını doğrudan her OpenShift düğümüne yükleme
- Azure İzleyicisi VM uzantısı her OpenShift düğümde etkinleştir
- Log Analytics aracısını bir OpenShift arka plan programı kümesi olarak yükleme

Tam Okuma [yönergeleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-containers#configure-a-log-analytics-agent-for-red-hat-openshift) daha fazla ayrıntı için.

## <a name="configure-metrics-and-logging"></a>Ölçümler ve günlüğe kaydetme yapılandırın

Bir daldaki bağlı olarak, OpenShift kapsayıcı platformu ve OKD için Azure Resource Manager şablonları ölçümlerini etkinleştirme ve yüklemenin bir parçası günlüğe kaydetme için giriş parametrelerini sağlayabilir.

OpenShift kapsayıcı platformu Market teklifi, Ölçümler ve Küme yükleme sırasında günlük kaydını etkinleştirmek için bir seçenek de sağlar.

Varsa ölçümleri / küme yüklenmesi sırasında günlük kaydının etkin değildi, olaydan sonra kolayca etkinleştirilebilir.

### <a name="azure-cloud-provider-in-use"></a>Azure bulut sağlayıcısı kullanın

SSH savunma düğüm veya ilk ana düğüm (şablon ve dal kullanımda göre) için dağıtım sırasında sağlanan kimlik bilgilerini kullanarak. Aşağıdaki komutu yürütün:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True \
-e openshift_logging_es_pvc_dynamic=true
```

### <a name="azure-cloud-provider-not-in-use"></a>Azure bulut sağlayıcısı kullanımda

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True
```

## <a name="install-open-service-broker-for-azure-osba"></a>Açık hizmet Aracısı (OSBA) Azure'ı yükleme

Hizmet Aracısı'nı açmak için Azure veya OSBA, sağlar OpenShift doğrudan Azure bulut hizmetleri sağlama. OSBA Azure için açık hizmet Aracısı API uygulaması içinde. Açık hizmet Aracısı tanımlayan bir bulut yerel uygulamalarını bulut sağlayıcılarının kilit açma olmadan bulut hizmetlerini yönetmek için kullanabilirsiniz için ortak dil belirtimi API'dir.

OSBA üzerinde OpenShift yüklemek için buradaki yönergeleri izleyin: https://github.com/Azure/open-service-broker-azure#openshift-project-template. 
> [!NOTE]
> Yalnızca OpenShift proje şablonunu ve tüm yükleme bölümleri değil'ndaki adımları tamamlayın.

## <a name="next-steps"></a>Sonraki adımlar

- [OpenShift kapsayıcı platformu ile çalışmaya başlama](https://docs.openshift.com/container-platform)
