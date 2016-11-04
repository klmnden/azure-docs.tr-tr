---
title: Azure App Service içinde Java API uygulaması derleme ve dağıtma
description: Java API uygulama paketini oluşturma ve Azure App Service’e dağıtma hakkında bilgi edinin.
services: app-service\api
documentationcenter: java
author: bradygaster
manager: mohisri
editor: tdykstra

ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: get-started-article
ms.date: 08/31/2016
ms.author: rachelap

---
# Azure App Service içinde Java API uygulaması derleme ve dağıtma
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Bu öğretici bir Java uygulamasının nasıl oluşturulacağını ve [Git] kullanılarak Azure App Service API Apps’e nasıl dağıtılacağını göstermektedir. Bu öğreticideki yönergeler Java çalıştırabilen tüm işletim sistemlerinde izlenebilir. Bu öğreticideki kod [Maven] kullanılarak derlenmiştir. [Jax-RS], RESTful Hizmetini oluşturmak için kullanılır ve [Swagger] meta veri belirtimine göre [Swagger Editor] kullanılarak oluşturulur.

## Ön koşullar
1. [Java Geliştirme Seti 8] \(veya üzeri)
2. Dağıtım makinenize yüklü [Maven]
3. Dağıtım makinenize yüklü [Git]
4. [Microsoft Azure] için ücretli veya [ücretsiz deneme] aboneliği
5. [Postman] gibi bir HTTP test uygulaması

## Swagger.IO kullanarak API iskelesi kurma
Swagger.io çevrimiçi düzenleyicisini kullanarak API’nizin yapısını temsil eden Swagger JSON veya YAML koduna giriş yapabilirsiniz. API yüzey alanı tasarlandıktan sonra kodu çeşitli platformlar ve çerçeveler için dışarı aktarabilirsiniz. Sonraki bölümde iskele kurulmuş kod, sahte işlevleri içerecek şekilde değiştirilecektir. 

Bu gösterim swagger.io düzenleyicisine yapıştıracağınız ve ardından bir REST API’si uç noktasına erişmek üzere JAX-RS kullanan kodu oluşturmak için kullanılacak bir Swagger JSON gövdesi ile başlar. Ardından, bir veri kalıcılığı mekanizmasının üzerinde oluşturulan REST API’sine benzeyen sahte veriler döndürmek için iskele kurulmuş kodu düzenlersiniz.  

1. Aşağıdaki Swagger JSON kodunu panonuza kopyalayın:
   
        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }
2. [Online Swagger Editor] uygulamasına gidin. Uygulamaya ulaştıktan sonra **Dosya -> JSON Yapıştır** menü öğesine tıklayın.
   
    ![JSON Yapıştır menü öğesi][paste-json]
3. Daha önce kopyaladığınız Kişi Listesi API’si Swagger JSON öğesini yapıştırın. 
   
    ![JSON kodunu Swagger’a yapıştırma][pasted-swagger]
4. Belge sayfalarını ve düzenleyicide işlenen API özetini görüntüleyin. 
   
    ![Swagger ile Oluşturulan Belgeleri Görüntüleme][view-swagger-generated-docs]
5. Sahte uygulama eklemek üzere daha sonra düzenleyeceğiniz sunucu tarafı kodunun iskelesini oluşturmak için **Sunucu Oluştur -> JAX-RS** menü seçeneğini belirleyin. 
   
    ![Kod Menü Öğesi Oluşturma][generate-code-menu-item]
   
    Kod oluşturulduktan sonra indirmeniz için ZIP dosyası sağlanır. Bu dosya Swagger kod oluşturucu tarafından iskelesi oluşturulmuş kodu ve tüm ilişkili derleme betiklerini içerir. Tüm kitaplığı, geliştirme iş istasyonunuzdaki bir dizine ayıklayın. 

## API uygulaması eklemek için kodu düzenleme
Bu bölümde Swagger ile oluşturulan kodun sunucu-tarafı uygulamasını özel kodunuzla değiştirirsiniz. Yeni kod, çağıran istemciye Kişi varlıkları için bir ArrayList döndürür. 

1. *src/gen/java/io/swagger/model* klasöründe bulunan *Contact.java* model dosyasını [Visual Studio Code] veya sık kullandığınız metin düzenleyiciyi kullanarak açın. 
   
    ![Kişi Model Dosyasını açma][open-contact-model-file]
2. **Kişi** sınıfına aşağıdaki oluşturucuyu ekleyin. 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. *src/main/java/io/swagger/api/impl* klasöründe bulunan *ContactsApiServiceImpl.java* hizmet uygulama dosyasını [Visual Studio Code] veya sık kullandığınız metin düzenleyiciyi kullanarak açın.
   
    ![Kişi Hizmeti Kod Dosyasını açma][open-contact-service-code-file]
4. Hizmet koduna sahte bir uygulama eklemek için dosyanın içindeki kodun üzerine bu yeni kodu yazın. 
   
        package io.swagger.api.impl;
   
        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;
   
        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
   
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
   
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
   
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
   
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }
5. Bir komut istemi açın ve dizini uygulamanızın kök klasörüyle değiştirin.
6. Kodu derlemek için aşağıdaki Maven komutunu yürütün ve yerel olarak Jetty uygulama sunucusu ile çalıştırın. 
   
        mvn package jetty:run
7. Komut penceresinde Jetty’nin bağlantı noktası 8080 üzerinde kodunuzu başlattığının gösterildiğini göreceksiniz. 
   
    ![Kişi Hizmeti Kod Dosyasını açma][run-jetty-war]
8. [Postman] kullanarak http://localhost:8080/api/contacts sayfasında "tüm kişileri al" API yöntemine bir istek gönderin.
   
    ![Kişiler API’sini çağırma][calling-contacts-api]
9. [Postman] kullanarak http://localhost:8080/api/contacts/2 sayfasında bulunan "belirli kişiyi al" API yöntemine istek gönderin.
   
    ![Kişiler API’sini çağırma][calling-specific-contact-api]
10. Son olarak, konsolunuzda aşağıdaki Maven komutunu yürüterek Java WAR (Web Arşivi) dosyasını derleyin. 
    
         mvn package war:war
11. WAR dosyası derlendikten sonra **hedef** klasörüne yerleştirilir. **Hedef** klasörüne gidin ve WAR dosyasını **ROOT.war** olarak yeniden adlandırın. (Büyük harflerin bu biçimle eşleştiğinden emin olun).
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. Son olarak, WAR dosyasını Azure’a dağıtmak için kullanılacak bir **dağıt** klasörü oluşturmak üzere uygulamanızın kök klasöründen aşağıdaki komutları yürütün. 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## Çıktıyı Azure App Service’de yayımlama
Bu bölümde Azure Portal’ı kullanarak yeni bir API oluşturma, bu API uygulamasını Java uygulamalarını barındıracak şekilde hazırlama ve yeni oluşturulan WAR dosyasını yeni API uygulamasını çalıştırmak üzere Azure App Service’e dağıtma hakkında bilgi edineceksiniz. 

1. [Azure portalında] yeni bir API uygulaması oluşturmak için **Yeni -> Web + Mobil -> API uygulaması** menü öğesine tıklayın, uygulama bilgilerinizi girin ve ardından **Oluştur**’a tıklayın.
   
    ![Yeni bir API uygulaması oluşturma][create-api-app]
2. API uygulamanız oluşturulduktan sonra uygulamanızın **Ayarlar** dikey penceresini açın ve ardından **Uygulama ayarları** menü öğesine tıklayın. Kullanılabilir seçenekler arasından en son Java sürümlerini seçin, ardından **Web kapsayıcısı** menüsünden en son Tomcat’i seçin ve ardından **Kaydet**’e tıklayın.
   
    ![API uygulaması dikey penceresinde Java ayarı][set-up-java]
3. **Dağıtım kimlik bilgileri** ayarlar menü öğesine tıklayın ve dosyaları API uygulamanızda yayımlarken kullanmak istediğiniz kullanıcı adını ve parolayı belirtin. 
   
    ![Dağıtım kimlik bilgilerini ayarlama][deployment-credentials]
4. **Dağıtım kaynağı** ayarlar menü öğesine tıklayın. Menüye girdikten sonra **Kaynak seç** düğmesine tıklayın, **Yerel Git Deposu** seçeneğini belirleyin ve ardından **Tamam**’a tıklayın. Bunun yapılması Azure’da çalışan ve API uygulaması ile ilişkisi olan bir Git deposu oluşturur. Git deponuzun *ana* dalında her komut yürüttüğünüzde kodunuz çalışan canlı API uygulaması örneğinizde yayımlanır. 
   
    ![Yeni bir yerel Git deposu ayarlama][select-git-repo]
5. Yeni Git deposunun URL’sini panonuza kopyalayın. Birazdan önemli olacağı için bunu kaydedin. 
   
    ![Uygulamanız için yeni bir Git deposu ayarlama][copy-git-repo-url]
6. WAR dosyasını çevrimiçi depoya Git ile iletin. Bunu yapmak için daha önce oluşturduğunuz **dağıt** klasörüne gidin, böylece kodu App Service içinde çalışan depoya kolayca uygulayabilirsiniz. Konsol penceresine gelip web uygulamaları klasörünün bulunduğu klasöre gittiğinizde işlemi başlatmak ve dağıtımı devre dışı bırakmak için aşağıdaki Git komutlarını verin. 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    **Push** isteğini verdikten sonra daha önce dağıtım kimlik bilgisi için oluşturduğunuz parola istenir. Kimlik bilgilerinizi girdikten sonra güncelleştirmenin dağıtıldığı portal ekranını görürsünüz.
7. Azure App Service’de çalışan yeni dağıtılmış API uygulamasını bulmak için bir kez daha Postman kullanırsanız davranışın tutarlı olduğunu ve bu kez kişi verilerini beklenen şekilde döndürdüğünü ve iskelesi kurulmuş Swagger.io Java kodundaki basit kod değişikliklerini kullandığını görürsünüz. 
   
    ![Azure içinde Java Kişiler REST API'sini canlı kullanma][postman-calling-azure-contacts]

## Sonraki adımlar
Bu makalede, bir Swagger JSON dosyasını ve Swagger.io düzenleyicisinden elde edilen iskelesi kurulmuş bazı Java kodlarını kullanmaya başladınız. Burada, basit değişiklikleriniz ve Git dağıtım işlemi Java’da yazılmış işlevsel bir API uygulaması elde etmeyle sonuçlanmıştır. Sonraki öğreticide [JavaScript istemcilerinden API uygulamalarını CORS][App Service API CORS] ile kullanma işlemi gösterilmektedir. Serinin sonraki öğreticileri, kimlik doğrulama ve yetkilendirmenin nasıl uygulandığını göstermektedir.

Bu örneği geliştirmek için JSON blob’larını devam ettirmek üzere [Java için Depolama SDK’sı] hakkında daha fazla bilgi alabilirsiniz. Veya kişi verilerinizi Azure Belge DB’sine kaydetmek için [Belge DB Java SDK’sı] kullanabilirsiniz. 

Azure’da Java kullanma hakkında daha fazla bilgi için bkz. [Java Geliştirici Merkezi].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure portalında]: https://portal.azure.com/
[Belge DB Java SDK’sı]: ../documentdb/documentdb-java-application.md
[ücretsiz deneme]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Java Geliştirici Merkezi]: /develop/java/
[Java Geliştirme Seti 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger Editor]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Java için Depolama SDK’sı]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger Editor]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png



<!--HONumber=Oct16_HO1-->


