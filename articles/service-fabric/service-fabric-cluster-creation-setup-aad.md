---
title: Service Fabric istemci kimlik doğrulaması için Azure Active Directory ayarlama | Microsoft Docs
description: Service Fabric kümeleri için istemcilerin kimliğini doğrulamak için Azure Active Directory'yi (Azure AD) ayarlama kurmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/15/2018
ms.author: aljo
ms.openlocfilehash: 75ba2ee378e9eddfeaeb2346b4d5bb584844afe2
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636685"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>İstemci kimlik doğrulaması için Azure Active Directory ayarlayın

Azure üzerinde çalışan kümeler için Azure Active Directory (Azure AD), yönetim uç noktalarına erişimi güvenli hale getirmek için önerilir.  Nasıl bir Service Fabric kümesi için istemcilerin kimliğini doğrulamak için Azure AD kurulumu için önce yapılmalıdır bu makalede [kümeyi oluştururken](service-fabric-cluster-creation-via-arm.md).  Azure AD (kiracılar bilinir), kuruluşların uygulamalara kullanıcı erişimini yönetmenizi sağlar. Uygulamaları olan web tabanlı oturum açma kullanıcı Arabirimi hem de yerel istemci deneyimi ile ayrılır. Bu makalede, zaten bir kiracı oluşturmuş varsayıyoruz. Tamamlamadıysanız, okuyarak başlamanız [bir Azure Active Directory kiracısı edinme][active-directory-howto-tenant].

## <a name="create-azure-ad-applications"></a>Azure AD uygulamaları oluşturma
Service Fabric kümesi birden çok giriş noktası için web tabanlı dahil olmak üzere Yönetim işlevselliğini sunar [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] ve [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulamaları oluşturduğunuz: bir web uygulaması ve bir yerel uygulama.  Uygulama oluşturulduktan sonra kullanıcıları salt okunur olarak atadığınız ve yönetici rolleri.

Bazı yapılandırma Azure AD'de bir Service Fabric kümesi ile yer alan adımların basitleştirmek için Windows PowerShell komutları kümesi oluşturduk.

> [!NOTE]
> Kümeyi oluşturmadan önce aşağıdaki adımları tamamlamanız gerekir. Küme adları ve uç noktaları betikleri beklediğiniz çünkü değerleri planlanmalıdır ve, zaten oluşturduğunuz değerleri değil.

1. [Betiklerini indirme] [ sf-aad-ps-script-download] bilgisayarınıza.
2. Zip dosyasını sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır** onay kutusunu işaretleyin ve ardından **Uygula**.
3. Zip dosyasını ayıklayın.
4. Çalıştırma `SetupApplications.ps1`ve parametrelere Tenantıd, ClusterName ve WebApplicationReplyUrl girin. Örneğin:

```PowerShell
.\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
```

> [!NOTE]
> (Azure kamu, Azure Çin'de, Azure Almanya) Ulusal Bulutlar için de belirtmeniz `-Location` parametresi.

PowerShell komutunu yürüterek Tenantıd'nizi bulabilirsiniz `Get-AzureSubscription`. Bu komut yürütülürken, her abonelik için Tenantıd görüntüler.

ClusterName betiği tarafından oluşturulan Azure AD uygulamaları önek olarak eklemek için kullanılır. Gerçek bir küme adı tam olarak eşleşmesi gerekmez. Yalnızca bunlar ile kullanılan Service Fabric kümesine Azure AD'ye yapıtları eşlemek kolaylaştırmak için tasarlanmıştır.

WebApplicationReplyUrl oturum açma işlemini tamamladıktan sonra kullanıcılara Azure AD döndüren varsayılan uç noktadır. Bu uç nokta olan varsayılan olarak, kümenizin Service Fabric Explorer uç nokta olarak ayarlayın:

https://&lt;cluster_domain&gt;: 19080/Explorer

Azure AD kiracısı için yönetici ayrıcalıklarına sahip bir hesap için oturum açmanız istenir. Oturum açtıktan sonra Service Fabric kümenizi temsil etmek için yerel uygulamalar ve web betik oluşturur. Kiracının uygulamaları bakarsanız [Azure portalında][azure-portal], iki yeni giriş görmeniz gerekir:

   * *ClusterName*\_küme
   * *ClusterName*\_istemci

PowerShell penceresini açık tutmak için iyi bir fikirdir, bu nedenle sonraki bölümde, kümeyi oluşturduğunuzda Azure Resource Manager şablon tarafından gereken JSON betiği yazdırır.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Kullanıcı rollerine atama
Kümenizi temsil etmek için uygulamaları oluşturduktan sonra kullanıcılarınızın Service Fabric tarafından desteklenen roller atama: salt okunur ve yönetici Rolleri kullanarak atayabilirsiniz [Azure portalında][azure-portal].

1. Azure portalında, sağ üst köşedeki kiracınızı seçin.

    ![Kiracı düğmeyi seçin][select-tenant-button]
2. Seçin **Azure Active Directory** sol sekmesini ve ardından seçin "Kurumsal uygulamalar".
3. "Tüm uygulamalar" seçin ve ardından bulmak ve seçmek bir ada sahip web uygulaması gibi `myTestCluster_Cluster`.
4. Tıklayın **kullanıcılar ve gruplar** sekmesi.

    ![Kullanıcılar ve Gruplar sekmesinde][users-and-groups-tab]
5. Tıklayın **Kullanıcı Ekle** düğmesini yeni sayfada, bir kullanıcı ve rol atayın ve ardından seçin **seçin** sayfanın alt kısmındaki düğmesi.

    ![Kullanıcı rolleri sayfasına atama][assign-users-to-roles-page]
6. Tıklayın **atama** sayfanın alt kısmındaki düğmesi.

    ![Atama onayı Ekle][assign-users-to-roles-confirm]

> [!NOTE]
> Service fabric'te rolleri hakkında daha fazla bilgi için bkz. [Service Fabric istemciler için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).
>
>

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

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>PowerShell ile bağlantı bir hata ile başarısız oluyor: "belirtilen kimlik bilgileri geçersiz"
#### <a name="problem"></a>Sorun
Azure AD'ye başarıyla oturum açtıktan sonra "AzureActiveDirectory" güvenlik modunu kullanarak kümeye bağlanmak için PowerShell kullanırken, bağlantının bir hatayla başarısız oluyor: "belirtilen kimlik bilgileri geçersiz."

#### <a name="solution"></a>Çözüm
Bu çözüm, önceki bir ile aynıdır.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer, oturum açtığınızda bir hata döndürür: "AADSTS50011"
#### <a name="problem"></a>Sorun
Service Fabric Explorer'ın Azure AD'de oturum açmaya çalıştığınızda, sayfanın bir hata döndürür: "AADSTS50011: yanıt adresi &lt;url&gt; uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor: &lt;GUID&gt;."

![SFX yanıt adresi eşleşmiyor][sfx-reply-address-not-match]

#### <a name="reason"></a>Neden
Service Fabric Explorer'ı temsil eden bir küme (web) uygulaması, Azure AD'de bir kimlik doğrulama girişiminde ve isteğe bağlı olarak, isteğin bir parçası yeniden yönlendirme dönüş URL'si sağlar. Ancak Azure AD uygulama URL'sini listelenmeyen **yanıt URL'si** listesi.

#### <a name="solution"></a>Çözüm
AAD sayfasında "Uygulama kayıtları"'i seçin, küme uygulamanızı seçin ve ardından **yanıt URL'leri** düğmesi. "Yanıt URL'leri" sayfasında, Service Fabric Explorer URL'si listeye ekleyin veya listedeki öğelerden birini değiştirin. İşiniz bittiğinde değişikliğinizi kaydedin.

![Web uygulamasının yanıt URL'si][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>PowerShell aracılığıyla Azure AD kimlik doğrulamasını kullanarak kümeye bağlanın.
Service Fabric kümesine bağlanmak için aşağıdaki PowerShell komutu örneği kullanın:

```PowerShell
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
[select-tenant-button]: ./media/service-fabric-cluster-creation-setup-aad/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-setup-aad/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
