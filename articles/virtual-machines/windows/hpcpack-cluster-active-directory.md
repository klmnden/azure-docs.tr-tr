---
title: "Azure Active Directory ile HPC paketi küme | Microsoft Docs"
description: "Azure HPC Pack 2016 kümede Azure Active Directory ile tümleştirme öğrenin"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/14/2016
ms.author: danlep
ms.openlocfilehash: c5a06a9c810349b1bcce01c7f73563941a5af0ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak Azure HPC Pack kümede yönetme
[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) ile tümleştirmeyi destekleyen [Azure Active Directory](../../active-directory/index.md) (Azure AD) Azure HPC Pack kümede dağıtmak Yöneticiler için.



Aşağıdaki yüksek düzey görevler için bu makaledeki adımları izleyin: 
* HPC Pack kümenizi Azure AD kiracınıza el ile tümleştirme
* Yönetmek ve azure'da HPC Pack kümenizdeki işlerini zamanla 

HPC Pack küme çözümünü Azure AD ile tümleştirme, diğer uygulamalar ve hizmetler tümleştirmek için standart adımları izler. Bu makale, Azure AD'de temel kullanıcı yönetimi ile bildiğinizi varsayar. Daha fazla bilgi ve arka plan için bkz: [Azure Active Directory belgelerindeki](../../active-directory/index.md) ve aşağıdaki bölümü.

## <a name="benefits-of-integration"></a>Tümleştirme yararları


Azure Active Directory (Azure AD) olduğu bulut çözümleri çoklu oturum açma (SSO) erişimi sağlayan bir çok kiracılı bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti.

HPC Pack küme Azure AD ile tümleştirilmesi, aşağıdaki hedeflere ulaşmak yardımcı olabilir:

* Geleneksel Active Directory etki alanı denetleyicisi HPC Pack kümeden kaldırın. Bu, bu, iş ve faturalamak dağıtım işlemi için gerekli değilse, küme koruma maliyetlerini azaltmaya yardımcı olabilir.
* Azure AD tarafından yapılmadan aşağıdaki yararları yararlanın:
    *   Çoklu oturum açma 
    *   Azure HPC paketi küme için bir yerel AD kimliği kullanma 

    ![Azure Active Directory ortamı](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a>Ön koşullar
* **Azure sanal makinelerinde dağıtılan HPC Pack 2016 küme** - adımları için bkz: [Azure HPC Pack 2016 kümede dağıtmak](hpcpack-2016-cluster.md). Baş düğüm DNS adını ve bu makaledeki adımları tamamlamak için bir Küme Yöneticisi kimlik bilgileri gerekir.

  > [!NOTE]
  > Azure Active Directory Tümleştirme HPC Pack HPC Pack 2016 önce sürümlerinde desteklenmez.



* **İstemci bilgisayar** -HPC Pack istemci yardımcı programları çalıştırmak için bir Windows veya Windows Server istemci bilgisayar gerekir. Yalnızca iş göndermek için HPC Pack web portalı veya REST API'yi kullanmak istiyorsanız, tercih ettiğiniz herhangi bir istemci bilgisayarı kullanabilirsiniz.

* **HPC Pack istemci yardımcı programları** -HPC Pack istemci yardımcı programları Microsoft Download Center'dan gelen kullanılabilir boş yükleme paketini kullanarak istemci bilgisayara yükleyin.


## <a name="step-1-register-the-hpc-cluster-server-with-your-azure-ad-tenant"></a>1. adım: Azure AD kiracınıza HPC küme sunucusu kaydetme
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Tıklatın **Active Directory** soldaki menüde ve istenen dizin aboneliğinizde'ı tıklatın. Dizindeki kaynaklara erişim izni olması gerekir.
3. Tıklatın **kullanıcılar**ve kullanıcı hesapları zaten oluşturuldu veya yapılandırılmış olmadığından emin olun.
4. Tıklatın **uygulamaları** > **Ekle**ve ardından **kuruluşumun geliştirmekte olduğu bir uygulama Ekle**. Sihirbazı'nda aşağıdaki bilgileri girin:
    * **Ad** -HPCPackClusterServer
    * **Tür** - seçin **Web uygulaması ve/veya Web API'si**
    * **URL oturum açma**-varsayılan örnek için temel URL`https://hpcserver`
    * **Uygulama Kimliği URI'si** - `https://<Directory_name>/<application_name>`. Değiştir `<Directory_name`> Azure AD kiracınız, örneğin, tam adıyla `hpclocal.onmicrosoft.com`ve yerine `<application_name>` daha önce seçtiğiniz ada sahip.

5. Uygulama eklendikten sonra tıklatın **yapılandırma**. Aşağıdaki özellikleri yapılandırın:
    * Seçin **Evet** için **uygulamasıdır çok kiracılı**
    * Seçin **Evet** için **uygulamaya erişmek için gerekli kullanıcı ataması**.

6. **Kaydet** düğmesine tıklayın. Kaydetme tamamlandığında tıklayın **yönetmek bildirim**. Bu eylem, uygulamanızın bildirim JavaScript nesne gösterimi (JSON) dosyası indirir. İndirilen bildirimi bularak Düzenle `appRoles` ayarlama ve aşağıdaki uygulama rolü ekleme:
    ```json
    "appRoles": [
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "displayName": "HpcAdminMirror",
        "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "description": "HpcAdminMirror",
        "value": "HpcAdminMirror"
        },
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "description": "HpcUsers",
        "displayName": "HpcUsers",
        "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "value": "HpcUsers"
        }
    ],
    ```
7. Dosyayı kaydedin. Portalda, ardından **yönetmek bildirim** > **karşıya bildirim**. Düzenlenen bildirimi ardından karşıya yükleyebilirsiniz.
8. Tıklatın **kullanıcılar**, bir kullanıcı seçin ve ardından **atamak**. Kullanılabilir roller (HpcUsers veya HpcAdminMirror) birini kullanıcıya atayın. Dizinde ek kullanıcılar ile bu adımı yineleyin. Küme kullanıcılar hakkında bilgi için bkz: [küme kullanıcıları yönetme](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).

   > [!NOTE] 
   > Kullanıcıları yönetmek için Azure Active Directory Önizleme dikey penceresinde kullanmanızı öneririz [Azure portal](https://portal.azure.com).
   >


## <a name="step-2-register-the-hpc-cluster-client-with-your-azure-ad-tenant"></a>2. adım: Azure AD kiracınıza HPC küme istemci Kaydet

1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Tıklatın **Active Directory** soldaki menüde ve istenen dizin aboneliğinizde'ı tıklatın. Dizindeki kaynaklara erişim izni olması gerekir.
3. Tıklatın **uygulamaları** > **Ekle**ve ardından **kuruluşumun geliştirmekte olduğu bir uygulama Ekle**. Sihirbazı'nda aşağıdaki bilgileri girin:

    * **Ad** -HPCPackClusterClient
    * **Tür** - seçin **yerel istemci uygulaması**
    * **Yeniden yönlendirme URI'si** - `http://hpcclient`

4. Uygulama eklendikten sonra tıklatın **yapılandırma**. Kopya **istemci kimliği** değer ve kaydedin. Bu daha sonra uygulamanızın yapılandırırken gerekir.

5. İçinde **diğer uygulamalara izinler**, tıklatın **uygulama Ekle**. Arayın ve (1. adımda oluşturduğunuz) HpcPackClusterServer uygulama ekleyin.

6. İçinde **izinlere temsilci** açılan listesinde, select **erişim HpcClusterServer**. Daha sonra **Kaydet**'e tıklayın.


## <a name="step-3-configure-the-hpc-cluster"></a>3. adım: HPC küme yapılandırma

1. Azure HPC Pack 2016 baş düğümünde bağlayın.

2. HPC PowerShell'i başlatın.

3. Şu komutu çalıştırın:

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    Burada

    * `AADTenant`Azure AD Kiracı adı gibi belirtir`hpclocal.onmicrosoft.com`
    * `AADClientAppId`2. adımda oluşturduğunuz uygulama istemci Kimliğini belirtir.

4. HpcSchedulerStateful hizmetini yeniden başlatın.

    Birden çok baş düğümü olan bir kümede baş düğüm birincil çoğaltma HpcSchedulerStateful hizmeti için geçiş yapmak için aşağıdaki PowerShell komutları çalıştırabilirsiniz:

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName “fabric:/HpcApplication/SchedulerStatefulService”

    ```

## <a name="step-4-manage-and-submit-jobs-from-the-client"></a>Adım 4: Yönetmek ve istemciden işleri gönderme

HPC Pack istemci yardımcı programları bilgisayarınıza yüklemek için Microsoft Download Center'dan gelen HPC Pack 2016 Kurulum dosyaları (tam yükleme) indirin. Yüklemeye başlamak için kurulum seçeneğini seçin **HPC Pack istemci yardımcı programları**.

Yükleme sırasında kullanılan sertifika istemci bilgisayarı hazırlamak için [HPC Küme kurulumu](hpcpack-2016-cluster.md) istemci bilgisayardaki. Ortak sertifikayı yüklemek için standart Windows sertifika yönetimi yordamları kullanın **Sertifikalar – Geçerli kullanıcı** > **güvenilen kök sertifika yetkilileri** depolar. 

Şimdi, HPC Pack komutları çalıştırmak veya HPC Pack İş Yöneticisi'ni GUI göndermek ve Azure AD hesabının kullanarak küme işlerini yönetmek için kullanın. İş gönderme seçenekleri için bkz: [bir HPC paketi için gönderme HPC işleri küme Azure'da](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).

> [!NOTE]
> İlk olarak Azure HPC Pack kümede bağlanmaya çalıştığında, açılır pencereleri görüntülenir. Oturum açmak için Azure AD kimlik bilgilerinizi girin. Belirteç sonra önbelleğe alınır. Kimlik doğrulaması değişiklikleri veya önbelleğe alınmış temizlenmediği sürece Azure kümedeki sonraki bağlantılar önbelleğe alınmış belirteci kullanın.
>
  
Örneğin, yukarıdaki adımları tamamladıktan sonra bir şirket içi istemci işlerden şu şekilde sorgulayabilirsiniz:

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a>Azure AD tümleştirmesiyle iş gönderme için kullanışlı cmdlet'leri 

### <a name="manage-the-local-token-cache"></a>Yerel belirteç önbelleği yönetme

HPC Pack 2016 yerel belirteç önbelleği yönetmek için iki yeni HPC PowerShell cmdlet'lerini sağlar. Bu cmdlet, işleri etkileşimsiz göndermek için faydalıdır. Aşağıdaki örneğe bakın:

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-the-credentials-for-submitting-jobs-using-the-azure-ad-account"></a>Azure AD hesabının kullanarak işleri göndermek için kimlik bilgilerini ayarlayın 

Bazı durumlarda, işi HPC küme kullanıcıdan (çalıştıran bir etki alanı kullanıcısı olarak; baş düğümünde bir yerel kullanıcı olarak çalıştırılan bir etki alanına katılmış HPC Kümesi için bir etki alanına katılmış HPC küme) altında çalıştırmak isteyebilirsiniz.

1. Kimlik bilgilerini ayarlamak için aşağıdaki komutları kullanın:

    ```powershell
    $localUser = “<username>”

    $localUserPassword=”<password>”

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. Ardından işi şu şekilde gönderin. İş/görevi $localUser altında işlem düğümlerinde çalışır.

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   Varsa `–Credential` ile belirtilmemiş `Submit-HpcJob`, iş veya görevi yerel eşlenen kullanıcı olarak Azure AD hesabının altında çalışır. (HPC küme görevi çalıştırmak için Azure AD hesabının aynı ada sahip yerel bir kullanıcı oluşturur.)
    
3. Azure AD hesabı için genişletilmiş veri ayarlayın. Bu, bir MPI işi Azure AD hesabının kullanarak Linux düğümleri üzerinde çalışırken yararlıdır.

   * Azure AD hesabı için genişletilmiş veri kümesi

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * Genişletilmiş veri kümesi ve HPC küme kullanıcı olarak çalıştırın
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

