---
title: Bir küme - Azure HDInsight için Azure Active Directory Kullanıcıları eşitleme
description: Bir küme için kimliği doğrulanmış kullanıcılar Azure Active Directory'den eşitleyin.
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: mamccrea
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 2be67c604bebbe9b4c4356e241d1480ca0778d4a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64688557"
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Azure Active Directory kullanıcılarını HDInsight kümesine eşitleme

[HDInsight kümeleri Kurumsal güvenlik paketi (ESP)](hdinsight-domain-joined-introduction.md) güçlü kimlik doğrulaması ile Azure Active Directory (Azure AD) kullanıcılarını kullanın, aynı zamanda olarak kullanmak *rol tabanlı erişim denetimi* (RBAC) ilkeleri. Kullanıcıları ve grupları Azure AD'ye ekleme gibi kümenize erişmek isteyen kullanıcılar eşitleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Zaten bunu yapmadıysanız [Kurumsal güvenlik paketi ile bir HDInsight kümesi oluşturma](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Ekleme yeni Azure AD kullanıcıları

Konaklarınız görüntülemek için Ambari Web kullanıcı arabirimini açın. Her düğüm, yeni Katılımsız Yükseltme ayarları ile güncelleştirilecektir.

1. İçinde [Azure portalında](https://portal.azure.com), ESP kümenizle ilişkili Azure AD dizinine gidin.

2. Seçin **tüm kullanıcılar** sol taraftaki menüden, ardından **yeni kullanıcı**.

    ![Tüm kullanıcılar bölmesi](./media/hdinsight-sync-aad-users-to-cluster/aad-users.png)

3. Yeni kullanıcı formu doldurun. Küme tabanlı izinler atamak için oluşturduğunuz gruplar'ı seçin. Bu örnekte, yeni kullanıcılar atayabilirsiniz "HiveUsers" adlı bir grup oluşturun. [Örnek yönergeleri](hdinsight-domain-joined-configure.md) ESP kümeyi oluşturmak için iki gruba eklemeyi içeren `HiveUsers` ve `AAD DC Administrators`.

    ![Yeni kullanıcı bölmesi](./media/hdinsight-sync-aad-users-to-cluster/aad-new-user.png)

4. **Oluştur**’u seçin.

## <a name="use-the-apache-ambari-rest-api-to-synchronize-users"></a>Kullanıcılar eşitlemek için Apache Ambari REST API'sini kullanma

Küme oluşturma işlemi sırasında belirtilen kullanıcı grupları, o anda eşitlenir. Kullanıcı eşitleme otomatik olarak saatte bir gerçekleşir. Ambari REST API'yi hemen kullanıcıları eşitlemek veya küme oluşturma sırasında belirttiğiniz grupları dışındaki bir grubunu eşitlemek için kullanın.

Aşağıdaki yöntemi POST Ambari REST API ile kullanır. Daha fazla bilgi için [yönetme HDInsight kümeleri Apache Ambari REST API'yi kullanarak](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [Kümenizin SSH ile bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md). Azure portalında kümenizin genel bakış bölmesinden seçin **güvenli Kabuk (SSH)** düğmesi.

    ![Güvenli Kabuk (SSH)](./media/hdinsight-sync-aad-users-to-cluster/ssh.png)

2. Görüntülenen kopyalama `ssh` komut ve SSH istemciniz yapıştırın. Girin ssh istendiğinde kullanıcı parolası.

3. Kimlik doğrulandıktan sonra aşağıdaki komutu girin:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    Yanıt şöyle görünmelidir:

    ```json
    {
      "resources" : [
        {
          "href" : "http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1",
          "Event" : {
            "id" : 1
          }
        }
      ]
    }
    ```

4. Eşitleme durumu görmek için yeni bir yürütme `curl` komutu:

    ```bash
    curl -u admin:<YOUR PASSWORD> https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events/1
    ```
    
    Yanıt şöyle görünmelidir:
    
    ```json
    {
      "href" : "http://hn0-hadoop.YOURDOMAIN.com:8080/api/v1/ldap_sync_events/1",
      "Event" : {
        "id" : 1,
        "specs" : [
          {
            "sync_type" : "existing",
            "principal_type" : "groups"
          }
        ],
        "status" : "COMPLETE",
        "status_detail" : "Completed LDAP sync.",
        "summary" : {
          "groups" : {
            "created" : 0,
            "removed" : 0,
            "updated" : 0
          },
          "memberships" : {
            "created" : 1,
            "removed" : 0
          },
          "users" : {
            "created" : 1,
            "removed" : 0,
            "skipped" : 0,
            "updated" : 0
          }
        },
        "sync_time" : {
          "end" : 1497994072182,
          "start" : 1497994071100
        }
      }
    }
    ```

5. Bu sonucu durum olduğunu gösterir. **tam**, yeni bir kullanıcı oluşturuldu ve kullanıcının bir üyelik atandı. Bu örnekte, "eşitlenmiş HiveUsers için" kullanıcı atanmış kullanıcı için aynı grubu Azure AD'ye eklendikten sonra LDAP grup.

> [!NOTE]  
> Önceki yöntem yalnızca belirtilen Azure AD grupları eşitler **erişim kullanıcı grubu** küme oluşturma sırasında etki alanı ayarlarını özelliğidir. Daha fazla bilgi için [bir HDInsight kümesi oluşturma](domain-joined/apache-domain-joined-configure.md).

## <a name="verify-the-newly-added-azure-ad-user"></a>Yeni eklenen doğrulayın Azure AD kullanıcısı

Açık [Apache Ambari Web kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md) doğrulamak için yeni Azure AD kullanıcı eklendi. Ambari Web kullanıcı arabirimini göz atarak erişim **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`** . Küme Yöneticisi kullanıcı adı ve parolayı girin.

1. Ambari Panoda **yönetme Ambari** altında **yönetici** menüsü.

    ![Ambari ile yönetme](./media/hdinsight-sync-aad-users-to-cluster/manage-ambari.png)

2. Seçin **kullanıcılar** altında **kullanıcı + Grup Yönetimi** sayfanın sol tarafındaki menü grubu.

    ![Kullanıcı menü öğesi](./media/hdinsight-sync-aad-users-to-cluster/users-link.png)

3. Yeni kullanıcı kullanıcılar tabloda listelenmiş olmalıdır. Türü `LDAP` yerine `Local`.

    ![Kullanıcılar sayfası](./media/hdinsight-sync-aad-users-to-cluster/users.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Ambari için yeni bir kullanıcı olarak oturum açın

Yeni kullanıcı (veya herhangi bir etki alanı kullanıcı) için Ambari oturum açtığında, bunlar tam Azure AD kullanıcı adı ve etki alanı kimlik bilgilerini kullanır.  Ambari, Azure AD'de kullanıcının görünen adı olan bir kullanıcı diğer adı görüntüler. Yeni örnek kullanıcının kullanıcı adına sahip `hiveuser3@contoso.com`. Ambari bu yeni kullanıcı olarak görünür `hiveuser3` ancak Ambari kullanıcının oturum açtığı `hiveuser3@contoso.com`.

## <a name="see-also"></a>Ayrıca bkz.

* [ESP ile HDInsight, Apache Hive ilkelerini yapılandırma](hdinsight-domain-joined-run-hive.md)
* [ESP ile HDInsight kümelerini yönetme](hdinsight-domain-joined-manage.md)
* [Apache Ambari için kullanıcıları yetkilendirme](hdinsight-authorize-users-to-ambari.md)
