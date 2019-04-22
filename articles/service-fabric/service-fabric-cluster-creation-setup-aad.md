---
title: Service Fabric istemci kimlik doğrulaması için Azure Active Directory ayarlama | Microsoft Docs
description: Service Fabric kümeleri için istemcilerin kimliğini doğrulamak için Azure Active Directory'yi (Azure AD) ayarlama kurmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/15/2019
ms.author: aljo
ms.openlocfilehash: c02e38880fdf8e8f1a2229f009b343d6431af853
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699192"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>İstemci kimlik doğrulaması için Azure Active Directory ayarlayın

Azure üzerinde çalışan kümeler için Azure Active Directory (Azure AD), yönetim uç noktalarına erişimi güvenli hale getirmek için önerilir.  Nasıl bir Service Fabric kümesi için istemcilerin kimliğini doğrulamak için Azure AD kurulumu için önce yapılmalıdır bu makalede [kümeyi oluştururken](service-fabric-cluster-creation-via-arm.md).  Azure AD (kiracılar bilinir), kuruluşların uygulamalara kullanıcı erişimini yönetmenizi sağlar. Uygulamaları olan web tabanlı oturum açma kullanıcı Arabirimi hem de yerel istemci deneyimi ile ayrılır. 

Service Fabric kümesi birden çok giriş noktası için web tabanlı dahil olmak üzere Yönetim işlevselliğini sunar [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] ve [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulamaları oluşturduğunuz: bir web uygulaması ve bir yerel uygulama.  Uygulama oluşturulduktan sonra kullanıcıları salt okunur olarak atadığınız ve yönetici rolleri.

> [!NOTE]
> Kümeyi oluşturmadan önce aşağıdaki adımları tamamlamanız gerekir. Küme adları ve uç noktaları betikleri beklediğiniz çünkü değerleri planlanmalıdır ve, zaten oluşturduğunuz değerleri değil.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, zaten bir kiracı oluşturmuş varsayıyoruz. Tamamlamadıysanız, okuyarak başlamanız [bir Azure Active Directory kiracısı edinme][active-directory-howto-tenant].

Bazı yapılandırma Azure AD'de bir Service Fabric kümesi ile yer alan adımların basitleştirmek için Windows PowerShell komutları kümesi oluşturduk.

1. [Betiklerini indirme](https://github.com/robotechredmond/Azure-PowerShell-Snippets/tree/master/MicrosoftAzureServiceFabric-AADHelpers/AADTool) bilgisayarınıza.
2. Zip dosyasını sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır** onay kutusunu işaretleyin ve ardından **Uygula**.
3. Zip dosyasını ayıklayın.

## <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Azure AD uygulamaları oluşturmak ve kullanıcıları rollere atama
Küme erişimi denetlemek için iki Azure AD uygulaması oluştur: bir web uygulaması ve bir yerel uygulama. Kümenizi temsil etmek için uygulamaları oluşturduktan sonra kullanıcılarınıza atama [Service Fabric tarafından desteklenen roller](service-fabric-cluster-security-roles.md): salt okunur ve yönetici

Çalıştırma `SetupApplications.ps1`ve parametrelere Kiracı kimliği, küme adı ve web uygulamasının yanıt URL'si girin.  Ayrıca, kullanıcı adları ve kullanıcılar için parola belirtin.  Örneğin:

```powershell
$Configobj = .\SetupApplications.ps1 -TenantId '0e3d2646-78b3-4711-b8be-74a381d9890c' -ClusterName 'mysftestcluster' -WebApplicationReplyUrl 'https://mysftestcluster.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> (Örneğin Azure devlet kurumları, Azure Çin'de, Azure Almanya) Ulusal Bulutlar için de belirtmeniz `-Location` parametresi.

Bulabilirsiniz, *Tenantıd* PowerShell komutunu yürüterek `Get-AzureSubscription`. Bu komut yürütülürken, her abonelik için Tenantıd görüntüler.

*ClusterName* betiği tarafından oluşturulan Azure AD uygulamaları önek olarak eklemek için kullanılır. Gerçek bir küme adı tam olarak eşleşmesi gerekmez. Yalnızca bunlar ile kullanılan Service Fabric kümesine Azure AD'ye yapıtları eşlemek kolaylaştırmak için tasarlanmıştır.

*WebApplicationReplyUrl* varsayılan uç nokta oturum açma işlemini tamamladıktan sonra kullanıcılarınız için Azure AD'ye verir. Bu uç nokta olan varsayılan olarak, kümenizin Service Fabric Explorer uç nokta olarak ayarlayın:

https://&lt;cluster_domain&gt;: 19080/Explorer

Azure AD kiracısı için yönetici ayrıcalıklarına sahip bir hesap için oturum açmanız istenir. Oturum açtıktan sonra Service Fabric kümenizi temsil etmek için yerel uygulamalar ve web betik oluşturur. Kiracının uygulamaları bakarsanız [Azure portalında][azure-portal], iki yeni giriş görmeniz gerekir:

   * *ClusterName*\_küme
   * *ClusterName*\_istemci

Azure Resource Manager şablon tarafından gereken JSON betiği yazdırır olduğunda, [küme oluşturma](service-fabric-cluster-creation-create-template.md#add-azure-ad-configuration-to-use-azure-ad-for-client-access), PowerShell penceresi açık tutmak için iyi bir fikirdir.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Azure Active Directory'yi ayarlama konusunda Yardım sorunlarını giderme
Burada olduklarından bazı işaretçiler sorunla ilgili hataları ayıklamak için yapabilecekleriniz hakkında Azure AD'yi ayarlama ve kullanma, zor olabilir.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer bir sertifika seçmenizi ister.
#### <a name="problem"></a>Sorun
Başarılı bir şekilde Azure AD'ye Service Fabric Explorer'da oturum açtıktan sonra tarayıcı giriş sayfasına döndürür ancak iletiye bir sertifika seçmenizi ister.

![SFX sertifika iletişim kutusu][sfx-select-certificate-dialog]

#### <a name="reason"></a>Neden
Kullanıcı Azure AD küme uygulaması bir rol atanmamıştır. Bu nedenle, Service Fabric kümesinde Azure AD kimlik doğrulaması başarısız olur. Service Fabric Explorer, sertifika kimlik doğrulaması için geri döner.

#### <a name="solution"></a>Çözüm
Azure AD'yi ayarlama yönergelerini izleyin ve kullanıcı rolleri atayın. Ayrıca, "uygulamasına erişmek için kullanıcı ataması gerekli üzerinde" Kapat olarak öneririz `SetupApplications.ps1` yapar.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>PowerShell ile bağlantı bir hata ile başarısız olur: "Belirtilen kimlik bilgileri geçersiz"
#### <a name="problem"></a>Sorun
Azure AD'ye başarıyla oturum açtıktan sonra "AzureActiveDirectory" güvenlik modunu kullanarak kümeye bağlanmak için PowerShell kullanırken, bağlantı bir hata ile başarısız olur: "Belirtilen kimlik bilgileri geçersiz."

#### <a name="solution"></a>Çözüm
Bu çözüm, önceki bir ile aynıdır.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer, oturum açtığınızda bir hata döndürür: "AADSTS50011"
#### <a name="problem"></a>Sorun
Service Fabric Explorer'ın Azure AD'de oturum açmaya çalıştığınızda, sayfanın bir hata döndürür: "AADSTS50011: Yanıt adresi &lt;url&gt; uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor: &lt;GUID&gt;. "

![SFX yanıt adresi eşleşmiyor][sfx-reply-address-not-match]

#### <a name="reason"></a>Neden
Service Fabric Explorer'ı temsil eden bir küme (web) uygulaması, Azure AD'de bir kimlik doğrulama girişiminde ve isteğe bağlı olarak, isteğin bir parçası yeniden yönlendirme dönüş URL'si sağlar. Ancak Azure AD uygulama URL'sini listelenmeyen **yanıt URL'si** listesi.

#### <a name="solution"></a>Çözüm
AAD sayfasında "Uygulama kayıtları"'i seçin, küme uygulamanızı seçin ve ardından **yanıt URL'leri** düğmesi. "Yanıt URL'leri" sayfasında, Service Fabric Explorer URL'si listeye ekleyin veya listedeki öğelerden birini değiştirin. İşiniz bittiğinde değişikliğinizi kaydedin.

![Web uygulamasının yanıt URL'si][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>PowerShell aracılığıyla Azure AD kimlik doğrulamasını kullanarak kümeye bağlanın.
Service Fabric kümesine bağlanmak için aşağıdaki PowerShell komutu örneği kullanın:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Daha fazla bilgi için bkz. [Connect-ServiceFabricCluster cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Birden çok kümeleri aynı Azure AD kiracısında yeniden kullanabilir miyim?
Evet. Ancak, küme (web) uygulamanız için Service Fabric Explorer URL'si eklemeyi unutmayın. Aksi takdirde, Service Fabric Explorer çalışmaz.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Azure AD etkinken neden hala bir sunucu sertifikası ihtiyacım var?
FabricClient ve FabricGateway bir karşılıklı kimlik doğrulaması gerçekleştirin. Azure AD kimlik doğrulaması sırasında sunucuya bir istemci kimliği Azure AD tümleştirme sağlar ve sunucu sertifikası sunucu kimliğini doğrulamak için kullanılır. Service Fabric sertifikalar hakkında daha fazla bilgi için bkz. [X.509 sertifikaları ve Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Sonraki adımlar
Azure Active Directory uygulamaları ve kullanıcılar için ayar rolleri ayarlama sonra [yapılandırmak ve küme dağıtma](service-fabric-cluster-creation-via-arm.md).


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
