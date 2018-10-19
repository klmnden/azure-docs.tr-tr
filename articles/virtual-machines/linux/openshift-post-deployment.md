---
title: Dağıtım sonrası görevleri Azure üzerinde OpenShift | Microsoft Docs
description: Bir OpenShift kümesi dağıtıldıktan sonra ek görevler için.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/09/2018
ms.author: haroldw
ms.openlocfilehash: 39febceff58127fb9777ace6e3063fbe41605b79
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426456"
---
# <a name="post-deployment-tasks"></a>Dağıtım sonrası görevler

OpenShift küme dağıtıldıktan sonra ek öğelerini yapılandırabilirsiniz. Bu makalede, aşağıdakileri içerir:

- Azure Active Directory (Azure AD) kullanarak çoklu oturum açmayı yapılandırma
- OpenShift izlemek için log Analytics'i yapılandırma
- Ölçümler ve günlüğe kaydetme yapılandırma

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak çoklu oturum açmayı yapılandırın

Azure Active Directory kimlik doğrulaması için kullanmak için önce bir Azure AD uygulama kaydı oluşturmanız gerekir. Bu işlem iki adımdan oluşur: uygulama kaydı oluşturma ve izinlerini yapılandırma.

### <a name="create-an-app-registration"></a>Bir uygulama kaydı oluşturma

Bu adımlar, uygulama kaydı ve izinler ayarlamak üzere GUI (portal) oluşturmak için Azure CLI'yı kullanın. Uygulama kaydı oluşturmak için şu beş bilgilere ihtiyacınız vardır:

- Görünen ad: uygulama kaydı adı (örneğin, OCPAzureAD)
- Giriş sayfası: OpenShift konsol URL'si (örneğin, https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- Tanımlayıcı URI'si: OpenShift Konsolu URL'si (örneğin, https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- Yanıt URL'si: genel URL ve uygulama kayıt adı (örneğin, ana https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD)
- Parola: Güvenli parola (güçlü bir parola kullanın)

Aşağıdaki örnek, önceki bilgileri kullanarak bir uygulama kaydı oluşturur:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --password {Strong Password}
```

Komut başarılı olursa, bir JSON çıkışı benzer alın:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console"
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

1.  Seçin **Azure Active Directory** > **uygulama kaydı**.
2.  Uygulama kaydınızı (örneğin, OCPAzureAD) arayın.
3.  Sonuçlarda uygulama kaydını tıklayın.
4.  Altında **ayarları**seçin **gerekli izinler**.
5.  Altında **gerekli izinler**seçin **Ekle**.

  ![Uygulama kaydı](media/openshift-post-deployment/app-registration.png)

6.  1. Adım'a tıklayın: API seçin ve ardından **Azure Active Directory (Microsoft.Azure.ActiveDirectory)**. Tıklayın **seçin** altındaki.

  ![Uygulama kaydı API seçimi](media/openshift-post-deployment/app-registration-select-api.png)

7.  Üzerinde Adım 2: İzinleri seçin, **oturum açın ve kullanıcı profilini okuma** altında **Temsilcili izinler**ve ardından **seçin**.

  ![Uygulama kaydı erişim](media/openshift-post-deployment/app-registration-access.png)

8.  **Done** (Bitti) öğesini seçin.

### <a name="configure-openshift-for-azure-ad-authentication"></a>OpenShift Azure AD kimlik doğrulamasını yapılandırma

OpenShift bir kimlik doğrulama sağlayıcısı olarak Azure AD'yi kullanacak şekilde yapılandırmak için tüm ana düğümler üzerinde /etc/origin/master/master-config.yaml dosyasının düzenlenmesi gerekir.

Aşağıdaki CLI komutunu kullanarak Kiracı Kimliğini bulun:

```azurecli
az account show
```

Yaml dosyasında aşağıdaki satırları bulun:

```yaml
oauthConfig:
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console/
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

Aşağıdaki CLI komutunu kullanarak Kiracı Kimliğini bulun: ```az account show```

Tüm ana düğüm üzerinde OpenShift Yöneticisi hizmetlerini yeniden başlatın:

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**Birden çok ana sunucu ile OpenShift kapsayıcı Platformu (OCP)**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**Tek bir ana şablon ile OpenShift kapsayıcı platformu**

```bash
sudo systemctl restart atomic-openshift-master
```

OpenShift konsolunda, artık kimlik doğrulaması için iki seçenek görürsünüz: htpasswd_auth ve [uygulama kaydı].

## <a name="monitor-openshift-with-log-analytics"></a>Log Analytics ile izleme OpenShift

Log Analytics ile OpenShift izlemek için iki seçenekten birini kullanabilirsiniz: Log Analytics VM konağında veya Log Analytics kapsayıcı Aracısı yükleme. Bu makale, Log Analytics kapsayıcıyı dağıtmak için yönergeler sağlar.

## <a name="create-an-openshift-project-for-log-analytics-and-set-user-access"></a>Log Analytics için OpenShift proje oluşturma ve kullanıcı erişimini ayarlama

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-a-daemon-set-yaml-file"></a>Arka plan programı kümesi yaml dosyası oluşturma

Ocp adlı bir dosya oluşturun-omsagent.yml:

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: oms
spec:
  selector:
    matchLabels:
      name: omsagent
  template:
    metadata:
      labels:
        name: omsagent
        agentVersion: 1.4.0-45
        dockerProviderVersion: 10.0.0-25
    spec:
      nodeSelector:
        zone: default
      serviceAccount: omsagent
      containers:
      - image: "microsoft/oms"
        imagePullPolicy: Always
        name: omsagent
        securityContext:
          privileged: true
        ports:
        - containerPort: 25225
          protocol: TCP
        - containerPort: 25224
          protocol: UDP
        volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        livenessProbe:
          exec:
            command:
              - /bin/bash
              - -c
              - ps -ef | grep omsagent | grep -v "grep"
          initialDelaySeconds: 60
          periodSeconds: 60
      volumes:
      - name: docker-sock
        hostPath:
          path: /var/run/docker.sock
      - name: omsagent-secret
        secret:
         secretName: omsagent-secret
````

## <a name="create-a-secret-yaml-file"></a>Gizli yaml dosyası oluşturma

Gizli yaml dosyası oluşturmak için iki parça bilgi gerekir: Log Analytics çalışma alanı Kimliğiniz ve Log Analytics çalışma alanı paylaşılan anahtarı. 

Örnek ocp-secret.yml dosyası aşağıdaki gibidir: 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

Log Analytics çalışma alanı kimliği Değiştir wsid_data Base64 ile kodlanmış Ardından key_data Base64 ile kodlanmış Log Analytics çalışma alanı paylaşılan anahtarı ile değiştirin.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-the-secret-and-daemon-set"></a>Gizli dizi ve arka plan programı kümesi oluşturma

Gizli dizi dosyasını dağıtın:

```bash
oc create -f ocp-secret.yml
```

Log Analytics aracısını arka plan programı kümesi dağıtın:

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Ölçümler ve günlüğe kaydetme yapılandırın

OpenShift kapsayıcı platformu için Azure Resource Manager şablonu ölçümlerini etkinleştirme ve günlüğe kaydetme için giriş parametrelerini sağlar. OpenShift kapsayıcı platformu Market teklifi ve OpenShift Origin Resource Manager şablonu yoktur.

OCP Market teklifi kullandıysanız, kolayca olabilir ve günlüğe kaydetme, yükleme sırasında etkin olmayan veya OCP Resource Manager şablonunu ve ölçümleri kullandıysanız bu çözümledikten sonra etkinleştirin. OpenShift Origin Resource Manager şablonu kullanıyorsanız, bazı öncesi iş gereklidir.

### <a name="openshift-origin-template-pre-work"></a>OpenShift Origin şablonu ön çalışma

1. SSH bağlantı noktası 2200'ı kullanarak ilk ana düğüme.

   Örnek:

   ```bash
   ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
   ```

2. /Etc/ansible/hosts dosyasını düzenleyin ve (# etkinleştirme HTPasswdPasswordIdentityProvider) kimlik sağlayıcısı bölümünden sonra aşağıdaki satırları ekleyin:

   ```yaml
   # Setup metrics
   openshift_hosted_metrics_deploy=false
   openshift_metrics_cassandra_storage_type=dynamic
   openshift_metrics_start_cluster=true
   openshift_metrics_hawkular_nodeselector={"type":"infra"}
   openshift_metrics_cassandra_nodeselector={"type":"infra"}
   openshift_metrics_heapster_nodeselector={"type":"infra"}
   openshift_hosted_metrics_public_url=https://metrics.$ROUTING/hawkular/metrics

   # Setup logging
   openshift_hosted_logging_deploy=false
   openshift_hosted_logging_storage_kind=dynamic
   openshift_logging_fluentd_nodeselector={"logging":"true"}
   openshift_logging_es_nodeselector={"type":"infra"}
   openshift_logging_kibana_nodeselector={"type":"infra"}
   openshift_logging_curator_nodeselector={"type":"infra"}
   openshift_master_logging_public_url=https://kibana.$ROUTING
   ```

3. $ROUTING aynı /etc/ansible/hosts dosyasındaki openshift_master_default_subdomain seçeneği kullanılacak dizeyle değiştirin.

### <a name="azure-cloud-provider-in-use"></a>Azure bulut sağlayıcısı kullanın

İlk ana düğüm (kaynak) veya savunma düğümü (OCP), dağıtım sırasında sağlanan kimlik bilgileri kullanarak SSH. Aşağıdaki komutu yürütün:

```bash
ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

### <a name="azure-cloud-provider-not-in-use"></a>Azure bulut sağlayıcısı kullanımda

İlk ana düğüm (kaynak) veya savunma düğümü (OCP), dağıtım sırasında sağlanan kimlik bilgileri kullanarak SSH. Aşağıdaki komutu yürütün:

```bash
ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True 

ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True 
```

## <a name="next-steps"></a>Sonraki adımlar

- [OpenShift kapsayıcı platformu ile çalışmaya başlama](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
- [OpenShift Origin kullanmaya başlama](https://docs.openshift.org/latest/getting_started/index.html)
