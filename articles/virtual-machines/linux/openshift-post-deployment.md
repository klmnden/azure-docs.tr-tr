---
title: "Azure dağıtım sonrası görevleri OpenShift | Microsoft Docs"
description: "Bir OpenShift küme dağıtıldıktan sonra ek görevleri için."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 77c4719b5cee7f5736d73ee10cf6abf12229ea11
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="post-deployment-tasks"></a>Dağıtım sonrası görevler

OpenShift küme dağıttıktan sonra ek öğelere yapılandırabilirsiniz. Bu makalede aşağıdakileri kapsar:

- Azure Active Directory (Azure AD) kullanarak çoklu oturum açmayı yapılandırma
- Operations Management Suite OpenShift izlemek için yapılandırma
- Ölçümleri ve günlük nasıl yapılandırılır?

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Azure Active Directory kullanarak çoklu oturum açmayı yapılandırın

Azure Active Directory kimlik doğrulaması için kullanmak için önce bir Azure AD uygulama kaydı oluşturmanız gerekir. Bu işlemi iki adımdan oluşur: uygulama kaydı oluşturma ve izinleri yapılandırma.

### <a name="create-an-app-registration"></a>Bir uygulama kaydı oluşturun

Uygulama kaydı ve izinleri ayarlamak için GUI (portal) oluşturmak için Azure CLI bu adımları kullanın. Uygulama kaydı oluşturmak için aşağıdaki beş parça bilgi gerekir:

- Görünen ad: uygulama kayıt adı (örneğin, OCPAzureAD)
- Giriş sayfası: OpenShift Konsolu URL'si (örneğin, https://masterdns343khhde.westus.cloudapp.azure.com:8443/Konsolu)
- Tanımlayıcı URI: OpenShift Konsolu URL'si (örneğin, https://masterdns343khhde.westus.cloudapp.azure.com:8443/Konsolu)
- Yanıt URL'si: Ana genel URL ve uygulama kayıt adı (örneğin, https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD)
- Parola: Güvenli parola (güçlü bir parola kullanın)

Aşağıdaki örnek, önceki bilgileri kullanarak bir uygulama kaydı oluşturur:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --password {Strong Password}
```

Komut başarılı olursa, JSON çıktısını benzer alın:

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
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD"
  ]
}
```

Komut için bir sonraki adımda döndürülen AppID özelliği not edin.

Azure portalında:

1.  Seçin **Azure Active Directory** > **uygulama kaydı**.
2.  Uygulama kaydınızı (örneğin, OCPAzureAD) arayın.
3.  Sonuçlarda uygulama kaydı'nı tıklatın.
4.  Altında **ayarları**seçin **gerekli izinleri**.
5.  Altında **gerekli izinler**seçin **Ekle**.

  ![Uygulama kaydı](media/openshift-post-deployment/app-registration.png)

6.  Adım 1'i tıklatın: API seçin ve ardından **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)**. Tıklatın **seçin** altındaki.

  ![Uygulama kayıt API seçimi](media/openshift-post-deployment/app-registration-select-api.png)

7.  Üzerinde Adım 2: İzinleri seçin, **oturum açın ve kullanıcı profilini okuma** altında **izinlere temsilci**ve ardından **seçin**.

  ![Uygulama kaydı erişim](media/openshift-post-deployment/app-registration-access.png)

8.  Seçin **Bitti**.

### <a name="configure-openshift-for-azure-ad-authentication"></a>OpenShift Azure AD kimlik doğrulaması için yapılandırma

Azure AD kimlik doğrulama sağlayıcısı olarak kullanmak üzere OpenShift yapılandırmak için tüm ana düğümlerinde /etc/origin/master/master-config.yaml dosya düzenlenmesi gerekir.

Aşağıdaki CLI komutu kullanarak Kiracı Kimliğini bulun:

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

Aşağıdaki CLI komutu kullanarak Kiracı Kimliğini bulun:```az account show```

Tüm ana düğümlerde OpenShift Yöneticisi hizmetlerini yeniden başlatın:

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**Birden çok asıl ile OpenShift kapsayıcı Platform (OCP)**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**Tek bir ana OpenShift kapsayıcı platformu**

```bash
sudo systemctl restart atomic-openshift-master
```

OpenShift konsolunda, artık kimlik doğrulaması için iki seçenek görürsünüz: htpasswd_auth ve [uygulama kayıt].

## <a name="monitor-openshift-with-operations-management-suite"></a>Operations Management Suite ile İzleyici OpenShift

Operations Management Suite ile OpenShift izlemek için iki seçenekten birini kullanabilirsiniz: VM konağı veya OMS kapsayıcı OMS Aracısı yükleme. Bu makalede OMS kapsayıcıyı dağıtmak için yönergeler sağlar.

## <a name="create-an-openshift-project-for-operations-management-suite-and-set-user-access"></a>Operations Management Suite için bir OpenShift projesi oluşturun ve kullanıcı erişimini ayarlama

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-a-daemon-set-yaml-file"></a>Bir arka plan programı kümesi yaml dosyası oluşturma

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

Gizli yaml dosyası oluşturmak için iki parça bilgi gerekir: OMS çalışma alanı kimliği ve paylaşılan OMS çalışma alanı anahtarı. 

Bir örnek ocp-secret.yml dosyası aşağıdaki gibidir: 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

OMS çalışma alanı kimliği Değiştir wsid_data Base64 ile kodlanmış Ardından key_data Base64 ile kodlanmış OMS çalışma alanı paylaşılan anahtarı ile değiştirin.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-the-secret-and-daemon-set"></a>Gizli anahtar ve arka plan programı kümesi oluşturma

Gizli dosya dağıtın:

```bash
oc create -f ocp-secret.yml
```

OMS Aracısı arka plan programı kümesi dağıtın:

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Ölçümleri ve günlük yapılandırın

OpenShift kapsayıcı Platform için Azure Resource Manager şablonu ölçümlerini etkinleştirme ve günlüğe kaydetme için giriş parametreleri sağlar. OpenShift kapsayıcı Platform Market teklifi ve OpenShift kaynak Resource Manager şablonu yoktur.

OCP Resource Manager şablonu ve ölçümleri kullanılan ve yükleme sırasında günlük kaydı etkin doğru ya da OCP Market teklifi kullandıysanız, kolayca olabilir, bu olgu sonra etkinleştirin. OpenShift kaynak Resource Manager şablonunu kullanıyorsanız, bazı ön iş gereklidir.

### <a name="openshift-origin-template-pre-work"></a>OpenShift kaynak şablonu öncesi çalışma

1. SSH bağlantı noktası 2200 kullanarak ilk ana düğümü.

   Örnek:

   ```bash
   ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
   ```

2. /Etc/ansible/hosts dosyasını düzenleyin ve kimlik sağlayıcısı bölümü (# etkinleştirmek HTPasswdPasswordIdentityProvider) sonra aşağıdaki satırları ekleyin:

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

3. $ROUTING aynı /etc/ansible/hosts dosyasındaki openshift_master_default_subdomain seçeneği için kullanılan dizesiyle değiştirin.

### <a name="azure-cloud-provider-in-use"></a>Azure bulut sağlayıcısı kullanılıyor

İlk ana düğüm (kaynak) veya savunma düğümü (OCP), dağıtım sırasında sağladığınız kimlik bilgilerini kullanarak SSH. Aşağıdaki komutu yürütün:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

### <a name="azure-cloud-provider-not-in-use"></a>Azure bulut sağlayıcısı kullanılmadığı

İlk ana düğüm (kaynak) veya savunma düğümü (OCP), dağıtım sırasında sağladığınız kimlik bilgilerini kullanarak SSH. Aşağıdaki komutu yürütün:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True 

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True 
```

## <a name="next-steps"></a>Sonraki adımlar

- [OpenShift kapsayıcı Platform ile çalışmaya başlama](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
- [OpenShift Origin kullanmaya başlama](https://docs.openshift.org/latest/getting_started/index.html)
