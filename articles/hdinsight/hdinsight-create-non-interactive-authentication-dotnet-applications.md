---
title: Etkileşimli olmayan kimlik doğrulaması, Azure HDInsight .NET uygulamaları oluşturma
description: Etkileşimli olmayan kimlik doğrulaması, Azure HDInsight Microsoft .NET uygulamaları oluşturmayı öğrenin.
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: 8b96c38d5bb24a267ad0203083e485d1780f28c8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241474"
---
# <a name="create-a-non-interactive-authentication-net-hdinsight-application"></a>Etkileşimli olmayan kimlik doğrulaması .NET HDInsight uygulaması oluşturma
Microsoft .NET Azure HDInsight uygulamanızın (etkileşimli olmayan) bir uygulamanın kendi kimlik altında ya da uygulamanızın (etkileşimli) oturum açmış kullanıcının kimliği altında çalıştırabilirsiniz. Bu makalede, etkileşimli olmayan kimlik doğrulaması Azure'a bağlanmak ve HDInsight'ı yönetmek için .NET uygulaması oluşturma işlemini gösterir. Etkileşimli bir uygulama örneği için bkz. [Azure HDInsight Bağlan](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). 

Etkileşimli olmayan .NET uygulamanızdan şunlar gerekir:

* Azure abonelik Kiracı Kimliğinizi (olarak da adlandırılan bir *dizin kimliği*). Bkz: [Kiracı kimliği alma](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).
* Azure Active Directory (Azure AD) uygulama istemci kimliği. Bkz: [bir Azure Active Directory uygulaması oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) ve [bir uygulama kimliği alma](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).
* Azure AD uygulama gizli anahtarı. Bkz: [uygulama kimlik doğrulama anahtarını Al](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).

## <a name="prerequisites"></a>Önkoşullar
* Bir HDInsight kümesi. Bkz: [çalışmaya başlama Öğreticisi](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).

## <a name="assign-a-role-to-the-azure-ad-application"></a>Rol atamak için Azure AD uygulaması
Azure AD uygulamanız atama bir [rol](../role-based-access-control/built-in-roles.md)eylemleri gerçekleştirmek için izinler vermek için. Abonelik, kaynak grubu veya kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. (Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme uygulama kaynak grubunu ve tüm kaynakları da okuyabilirsiniz anlamına gelir.) Bu öğreticide kaynak grubu düzeyinde kapsamı ayarlayın. Daha fazla bilgi için [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../role-based-access-control/role-assignments-portal.md).

**Azure AD uygulamasına sahip rolü eklemek için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüden **Kaynak grupları**'nı seçin.
3. Daha sonra Bu öğreticide, Hive sorgu çalıştıracaksınız HDInsight kümesi içeren kaynak grubunu seçin. Çok sayıda kaynak grupları varsa, istediğinizi bulmak için filtre kullanabilirsiniz.
4. Kaynak grubu menüsünde **erişim denetimi (IAM)** .
5. Seçin **rol atamaları** geçerli rol atamaları görmek için sekmesinde.
6. Sayfanın üst kısmında seçin **rol ataması Ekle**.
7. Azure AD uygulamanız için sahip rolü eklemek için yönergeleri izleyin. Uygulama rolü başarıyla ekledikten sonra sahip rolünün altında listelenir. 

## <a name="develop-an-hdinsight-client-application"></a>HDInsight istemci uygulama geliştirme

1. Bir C# konsol uygulaması oluşturun.
2. Aşağıdaki [NuGet](https://www.nuget.org/) paketleri:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Aşağıdaki kodu çalıştırın:

    ```csharp
    using System;
    using System.Security;
    using Microsoft.Azure;
    using Microsoft.Azure.Common.Authentication;
    using Microsoft.Azure.Common.Authentication.Factories;
    using Microsoft.Azure.Common.Authentication.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.HDInsight;
    
    namespace CreateHDICluster
    {
        internal class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
    
            private static Guid SubscriptionId = new Guid("<Enter your Azure subscription ID>");
            private static string tenantID = "<Enter your tenant ID (also called directory ID)>";
            private static string applicationID = "<Enter your application ID>";
            private static string secretKey = "<Enter the application secret key>";
    
            private static void Main(string[] args)
            {
                var key = new SecureString();
                foreach (char c in secretKey) { key.AppendChar(c); }
    
                var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
    
                var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                resourceManagementClient.Providers.Register("Microsoft.HDInsight");
    
                _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
    
                var results = _hdiManagementClient.Clusters.List();
                foreach (var name in results.Clusters)
                {
                    Console.WriteLine("Cluster Name: " + name.Name);
                    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                    Console.WriteLine("\t Cluster location: " + name.Location);
                    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                }
                Console.WriteLine("Press Enter to continue");
                Console.ReadLine();
            }
    
            /// Get the access token for a service principal and provided key.          
            public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
            {
                var authFactory = new AuthenticationFactory();
                var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                var accessToken =
                    authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
    
                return new TokenCloudCredentials(accessToken);
            }
    
            public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
            {
                return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
            }
        }
    }
    ```


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Active Directory, Azure portalında uygulama ve hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).
* Bilgi edinmek için nasıl [Azure Resource Manager ile hizmet sorumlusu kimlik doğrulaması](../active-directory/develop/howto-authenticate-service-principal-powershell.md).
* Hakkında bilgi edinin [Azure rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md).
