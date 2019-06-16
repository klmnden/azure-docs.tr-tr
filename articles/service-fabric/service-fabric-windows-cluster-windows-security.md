---
title: Windows güvenliğini kullanarak Windows üzerinde çalışan bir küme güvenliğini sağlama | Microsoft Docs
description: Windows güvenliğini kullanarak Windows üzerinde çalışan tek başına küme üzerinde düğümden düğüme ve düğümden istemci güvenlik yapılandırmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: ccc726f54821d316c745f6af9c63d7ed13986d79
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65761941"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Windows güvenliğini kullanarak Windows üzerinde tek başına küme güvenliğini sağlama
Bir Service Fabric kümesine yetkisiz erişimi önlemek için küme güvenlik altına almanız gerekir. Üretim iş yükleri küme çalıştırdığında, güvenlik özellikle önemlidir. Bu makalede Windows güvenliği kullanarak düğümden düğüme ve düğümden istemci güvenlik yapılandırma *ClusterConfig.JSON* dosya.  İşleme için yapılandırma güvenlik adımı, karşılık gelen [Windows üzerinde çalışan tek başına küme oluşturma](service-fabric-cluster-creation-for-windows-server.md). Service Fabric Windows Güvenlik nasıl kullandığı hakkında daha fazla bilgi için bkz. [küme güvenliği senaryoları](service-fabric-cluster-security.md).

> [!NOTE]
> Başka bir küme yükseltme arasından bir güvenlik istediğinizi olduğundan düğümden düğüme güvenlik seçiminde dikkatli bir şekilde düşünmelisiniz. Güvenlik seçimini değiştirmek için tam küme yeniden derlemek zorunda.
>
>

## <a name="configure-windows-security-using-gmsa"></a>Windows güvenliğini kullanarak gmsa'yı yapılandırma  
Örnek *ClusterConfig.gMSA.Windows.MultiMachine.JSON* yapılandırma dosyası ile indirilen [Microsoft.Azure.ServiceFabric.WindowsServer.\< Sürüm > .zip](https://go.microsoft.com/fwlink/?LinkId=730690) tek başına küme pakette Windows güvenliğini kullanarak yapılandırmak için bir şablon [Grup yönetilen hizmet hesabı (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {  
        "ClustergMSAIdentity": "[gMSA Identity]",
        "ClusterSPN": "[Registered SPN for the gMSA account]",
        "ClientIdentities": [
            {
                "Identity": "domain\\username",
                "IsAdmin": true
            }
        ]
    }
}
```

| **Yapılandırma ayarı** | **Açıklama** |
| --- | --- |
| ClusterCredentialType |Kümesine *Windows* düğümden düğüme iletişim için Windows Güvenlik seçeneğini etkinleştirmek için.  | 
| ServerCredentialType |Kümesine *Windows* istemci-düğüm iletişimi için Windows Güvenlik seçeneğini etkinleştirmek için. |
| WindowsIdentities |Küme ve istemci kimliklerini içerir. |
| ClustergMSAIdentity |Düğümden düğüme güvenliğini yapılandırır. Bir grup yönetilen hizmet hesabı. |
| ClusterSPN |GMSA hesabı için kayıtlı SPN|
| ClientIdentities |İstemci düğümü güvenliğini yapılandırır. İstemci kullanıcı hesaplarını dizisi. |
| Kimlik |İstemci kimliği için etki alanı\kullanıcı adı bir etki alanı kullanıcısı ekleyin. |
| IsAdmin |Etki alanı kullanıcı istemci erişim yönetici veya kullanıcı istemci erişimi için yanlış olduğunu belirtmek için true olarak ayarlayın. |

> [!NOTE]
> ClustergMSAIdentity değer biçiminde olması "mysfgmsa@mydomain".

[Düğüm düğüm güvenlik](service-fabric-cluster-security.md#node-to-node-security) ayarlayarak yapılandırılmış **ClustergMSAIdentity** gerektiğinde service fabric gMSA altında çalıştırın. Düğümler arasındaki güven ilişkileri oluşturmak için bunlar birbirinden haberdar olmanız gerekir. Bu, iki farklı yollarla gerçekleştirilebilir: Grup yönetilen hizmet kümedeki tüm düğümleri içeren hesabı veya kümedeki tüm düğümleri içeren etki alanı makine grubu belirtin. Kullanmanızı öneririz [Grup yönetilen hizmet hesabı (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) yaklaşım, özellikle daha büyük kümeleri (10'dan fazla düğümler) veya büyük olasılıkla büyütmesine veya küçültmesine izin kümeleri.  
Bu yaklaşım, küme yöneticileri için ekleme ve kaldırma üyeleri için erişim hakkı verilmiş bir etki alanı grubu oluşturulmasını gerektirmez. Bu hesaplar otomatik parola yönetimi için de yararlıdır. Daha fazla bilgi için [Grup yönetilen hizmet hesapları ile çalışmaya başlama](https://technet.microsoft.com/library/jj128431.aspx).  
 
[İstemci düğümü güvenlik](service-fabric-cluster-security.md#client-to-node-security) kullanılarak yapılandırılan **ClientIdentities**. Bir istemci ve küme arasında güven oluşturmak için hangi istemci buna güvenmesi kimlikleri bilmek küme yapılandırmanız gerekir. Bu iki farklı yolla gerçekleştirebilirsiniz: Bağlanın veya bağlanabilen etki alanı düğümü kullanıcıları belirtin etki alanı grubu kullanıcıları belirtin. Service Fabric, bir Service Fabric kümesine bağlanan istemciler için iki farklı erişim denetim türlerini destekler: Yönetici ve kullanıcı. Erişim denetimi, belirli türdeki bir küme işlemleri farklı kümeye daha güvenli hale getirme, kullanıcı grupları için erişimi sınırlandırmak Küme Yöneticisi özelliği sunuyor.  Yöneticiler yönetim özellikleri (okuma/yazma özellikleri dahil olmak üzere) tam erişime sahiptir. Kullanıcılar, varsayılan olarak, yalnızca yönetim özelliklerine (örneğin, sorgu işlevleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümlenebilmesi sahiptir. Erişim denetimleri hakkında daha fazla bilgi için bkz. [Service Fabric istemciler için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).  
 
Aşağıdaki örnek **güvenlik** bölüm Windows güvenliğini kullanarak gmsa'yı yapılandırır ve belirten makinelerinizde *ServiceFabric.clusterA.contoso.com* gMSA kümenin ve o parçasıdır *CONTOSO\usera* yönetici istemci erişimi vardır:  
  
```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",
        "ClusterSPN" : "http/servicefabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
}
```
  
## <a name="configure-windows-security-using-a-machine-group"></a>Windows güvenliğini kullanarak bir makine grubu yapılandırma  
Bu model kullanımdan kaldırılıyor. Yukarıda açıklandığı biçimde gMSA kullanmak için önerilir. Örnek *ClusterConfig.Windows.MultiMachine.JSON* yapılandırma dosyası ile indirilen [Microsoft.Azure.ServiceFabric.WindowsServer.\< Sürüm > .zip](https://go.microsoft.com/fwlink/?LinkId=730690) tek başına küme paketi Windows güvenliği yapılandırmak için bir şablonu içerir.  Windows Güvenlik yapılandırılmıştır **özellikleri** bölümü: 

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
        "ClientIdentities": [{
            "Identity": "[domain\username]",
            "IsAdmin": true
        }]
    }
}
```

| **Yapılandırma ayarı** | **Açıklama** |
| --- | --- |
| ClusterCredentialType |Kümesine *Windows* düğümden düğüme iletişim için Windows Güvenlik seçeneğini etkinleştirmek için.  |
| ServerCredentialType |Kümesine *Windows* istemci-düğüm iletişimi için Windows Güvenlik seçeneğini etkinleştirmek için. |
| WindowsIdentities |Küme ve istemci kimliklerini içerir. |
| ClusterIdentity |Bir makine grubu adı domain\machinegroup, düğümden düğüme güvenliği yapılandırmak için kullanın. |
| ClientIdentities |İstemci düğümü güvenliğini yapılandırır. İstemci kullanıcı hesaplarını dizisi. |  
| Kimlik |İstemci kimliği için etki alanı\kullanıcı adı bir etki alanı kullanıcısı ekleyin. |  
| IsAdmin |Etki alanı kullanıcı istemci erişim yönetici veya kullanıcı istemci erişimi için yanlış olduğunu belirtmek için true olarak ayarlayın. |  

[Düğüm düğüm güvenlik](service-fabric-cluster-security.md#node-to-node-security) ayarı kullanılarak yapılandırılır **ClusterIdentity** bir Active Directory etki alanı içinde bir makine grubu kullanmak istiyorsanız. Daha fazla bilgi için [Active Directory'de bir makine grubu oluştur](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).

[İstemci düğümü güvenlik](service-fabric-cluster-security.md#client-to-node-security) kullanılarak yapılandırılan **ClientIdentities**. Bir istemci ve küme arasında güven oluşturmak için küme güvenebilir kimlikleri istemci bilmek küme yapılandırmanız gerekir. İki farklı yolla güven:

- Bağlanabilir etki alanı grubu kullanıcıları belirtin.
- Bağlanabilen etki alanı düğümü kullanıcıları belirtin.

Service Fabric, bir Service Fabric kümesine bağlanan istemciler için iki farklı erişim denetim türlerini destekler: Yönetici ve kullanıcı. Küme Yöneticisi, küme daha güvenli olmasını sağlayan belirli türlerdeki kullanıcılar, farklı kullanıcı grupları için küme işlemlerini erişimi sınırlandırmak erişim denetimi sağlar.  Yöneticiler yönetim özellikleri (okuma/yazma özellikleri dahil olmak üzere) tam erişime sahiptir. Kullanıcılar, varsayılan olarak, yalnızca yönetim özelliklerine (örneğin, sorgu işlevleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümlenebilmesi sahiptir.  

Aşağıdaki örnek **güvenlik** bölüm Windows güvenliğini yapılandırır, belirten makinelerinizde *ServiceFabric/clusterA.contoso.com* kümesinin parçası olan ve bu belirtir*CONTOSO\usera* yönetici istemci erişimi vardır:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric, bir etki alanı denetleyicisinde dağıtılmamalıdır. ClusterConfig.json etki alanı denetleyicisinin IP adresini bir makine grubu kullanırken içermez veya grup yönetilen hizmet hesabı (gMSA) olduğunu doğrulayın.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Windows Güvenlik yapılandırdıktan sonra *ClusterConfig.JSON* dosya, küme oluşturma işlemine devam [Windows üzerinde çalışan tek başına küme oluşturma](service-fabric-cluster-creation-for-windows-server.md).

Düğümden düğüme nasıl güvenlik, istemci düğümü güvenlik ve rol tabanlı erişim denetimi için bkz: hakkında daha fazla bilgi için [küme güvenliği senaryoları](service-fabric-cluster-security.md).

Bkz: [güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md) PowerShell veya FabricClient kullanarak bağlanma örnekler.
