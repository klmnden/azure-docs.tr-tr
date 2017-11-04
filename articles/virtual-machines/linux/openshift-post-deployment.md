---
title: "Azure Post dağıtım görevleri OpenShift | Microsoft Docs"
description: "OpenShift Post dağıtım görevleri"
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
ms.openlocfilehash: 12e6785358f5f412326418b0c64eeaeabdaa3b5f
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="post-deployment-tasks"></a>Dağıtım sonrası görevler

OpenShift küme dağıtıldıktan sonra yapılandırılabilir başka öğeler vardır. Bu makalede aşağıdakiler ele alınacaktır:

- Çoklu oturum Azure Active Directory (AAD) kullanarak özelliğini yapılandırın
- OpenShift izlemek için OMS yapılandırın
- Ölçümleri yapılandırmak ve günlüğe kaydetme

## <a name="single-sign-on-using-aad"></a>AAD kullanarak çoklu oturum açma

Kimlik doğrulaması için AAD kullanabilmek için öncelikle bir Azure AD uygulama kaydı oluşturulması gerekir. Bu işlem, iki adımlar - uygulama kaydı oluşturulmasını içerir ve izinlerini yapılandırın.

### <a name="create-app-registration"></a>Uygulama kaydı oluşturun

Uygulama kaydı ve GUI (Portal) izinlerini ayarlamak için oluşturmak için Azure CLI kullanacağız. Uygulama kaydı oluşturmak için beş parça bilgi gerekir.

- Görünen ad: Uygulama kayıt adı (örn: OCPAzureAD)
- Giriş sayfası: OpenShift Konsolu URL'si (örn: https://masterdns343khhde.westus.cloudapp.azure.com:8443/konsol)
- Tanımlayıcı URI: OpenShift Konsolu URL'si (örn: https://masterdns343khhde.westus.cloudapp.azure.com:8443/konsol)
- Yanıt URL'si: Ana genel URL ve uygulama kayıt adı (örn: oauth2callback/https://masterdns343khhde.westus.cloudapp.azure.com:8443/OCPAzureAD)
- Parola: Güvenli parola (güçlü bir parola kullanın)

Aşağıdaki örnek, yukarıda verilen bilgileri kullanarak bir uygulama kaydı oluşturur.

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --password {Strong Password}
```

Komut başarılı olursa, benzer JSON çıktısını alırsınız:

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

İçinde **Azure Portal**:

1.  Seçin **Azure Active Directory** --> **uygulama kaydı**
2.  Uygulama kaydınızı arayın (örn: OCPAzureAD)
3.  Sonuçlarda uygulama kayıt'a tıklayın.
4.  Ayarlar dikey penceresinde seçin **gerekli izinler**
5.  Gerekli izinler dikey penceresinde tıklayın **Ekle**

  ![Uygulama kaydı](media/openshift-post-deployment/app-registration.png)

6.  Adım 1'i tıklatın: API seçin ve ardından **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)** tıklatıp **seçin** altındaki

  ![Uygulama kayıt API seçimi](media/openshift-post-deployment/app-registration-select-api.png)

7.  Üzerinde Adım 2: İzinleri seçin, **oturum açın ve kullanıcı profilini okuma** altında **izinlere temsilci** tıklatıp **seçin**

  ![Uygulama kaydı erişim](media/openshift-post-deployment/app-registration-access.png)

8.  Tıklatın **bitti**

### <a name="configure-openshift-for-azure-ad-authentication"></a>OpenShift Azure AD kimlik doğrulaması için yapılandırma

Azure AD bir kimlik doğrulama sağlayıcısı olarak kullanmak üzere OpenShift yapılandırmak için **/etc/origin/master/master-config.yaml** tüm ana düğümlerinde dosya düzenlenemez.

Kiracı kimliği aşağıdaki CLI komut kullanılarak bulunabilir:

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

Yukarıdaki satırlar hemen sonra aşağıdaki satırları ekleyin:

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

Kiracı kimliği aşağıdaki CLI komut kullanılarak bulunabilir:```az account show```

Tüm ana düğümlerde OpenShift Yöneticisi hizmetlerini yeniden başlatın

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**Birden çok asıl OpenShift kapsayıcı platformu**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**Tek ana OpenShift kapsayıcı platformu**

```bash
sudo systemctl restart atomic-openshift-master
```

OpenShift konsolunda artık kimlik doğrulaması - htpasswd_auth için iki seçenek görürsünüz ve **[uygulama kaydı]**.

## <a name="monitor-openshift-with-oms"></a>İzleyici OpenShift OMS ile

OMS ile OpenShift izleme, iki seçenek - VM konağı veya OMS kapsayıcı OMS Aracısı yükleme biri kullanılarak sağlanabilir. Bu makalede, OMS kapsayıcı dağıtma hakkında yönergeler sağlar.

## <a name="create-an-openshift-project-for-oms-and-set-user-access"></a>OMS için bir OpenShift projesi oluşturun ve kullanıcı erişimini ayarlama

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-daemon-set-yaml-file"></a>Arka plan programı kümesi yaml dosyası oluşturma

Ocp-omsagent.yml adlı bir dosya oluşturun.

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

## <a name="create-secret-yaml-file"></a>Gizli yaml dosyası oluşturma

Gizli yaml dosyası oluşturmak için iki parça bilgi gerekecektir - OMS çalışma alanı kimliği ve paylaşılan OMS çalışma alanı anahtarı. 

Örnek ocp-secret.yml dosyası 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

OMS çalışma alanı kimliği Değiştir wsid_data Base64 ile kodlanmış ve OMS çalışma alanı paylaşılan anahtarı Değiştir key_data Base64 ile kodlanmış.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-secret-and-daemon-set"></a>Gizli anahtar ve arka plan programı kümesi oluşturma

Gizli dosya dağıtma

```bash
oc create -f ocp-secret.yml
```

OMS Aracısı arka plan programı kümesi dağıtma

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Ölçümleri ve günlük yapılandırın

Ölçümleri etkinleştirmek için giriş parametreleri OpenShift kapsayıcı Platform (OCP) Resource Manager şablonu sağlar ve günlüğe kaydetme. OpenShift kapsayıcı Platform Market sunar ve OpenShift kaynak Resource Manager şablonu desteklemez.

OCP Resource Manager şablonu kullanıldı ve yükleme sırasında ölçümleri ve günlük kaydı etkin doğru veya OCP Market teklifi kullanılan sahipse, bunlar kolayca olaydan sonra etkinleştirilebilir. OpenShift kaynak Resource Manager şablonu kullanarak, bazı ön iş gereklidir.

### <a name="openshift-origin-template-pre-work"></a>OpenShift kaynak şablonu öncesi çalışma

SSH için bağlantı noktası 2200 kullanarak ilk ana düğümü

Örnek

```bash
ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
```

Düzen **/etc/ansible/hosts dosya** ve kimlik sağlayıcısı bölümü (# etkinleştirmek HTPasswdPasswordIdentityProvider) sonra aşağıdaki satırları ekleyin

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

İçin kullanılan dize $ROUTING yerine **openshift_master_default_subdomain** aynı seçeneğinde **/etc/ansible/hosts** dosya.

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