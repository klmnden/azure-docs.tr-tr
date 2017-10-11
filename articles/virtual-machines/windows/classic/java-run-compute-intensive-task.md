---
title: "Bir VM üzerinde işlem yoğunluklu Java uygulaması | Microsoft Docs"
description: "Başka bir Java uygulaması tarafından izlenen bir işlem yoğunluklu Java uygulaması çalıştıran bir Azure sanal makinesi oluşturmayı öğrenin."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: ae6f2737-94c7-4569-9913-d871450c2827
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 8c51c0bb37e25ad61fe58a85dd641dabe0a1958c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Java’da bir sanal makine üzerinde yoğun işlem gücü kullanımlı görev çalıştırma
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Azure ile işlem yoğunluklu görevler işlemek için bir sanal makine kullanabilirsiniz. Örneğin, bir sanal makine görevleri ve sonuçları istemci makineleri veya mobil uygulamalara teslim etmek. Bu makaleyi okuduktan sonra başka bir Java uygulaması tarafından izlenen bir işlem yoğunluklu Java uygulaması çalıştıran bir sanal makine oluşturmak nasıl anlaşılması gerekir.

Bu öğretici Java konsol uygulamaları oluşturma biliyorsanız, kitaplıkları Java uygulamanız içe aktarabilir ve Java arşiv (JAR) oluşturabilir varsayar. Microsoft Azure olanağıyla varsayılır.

Şunları öğreneceksiniz:

* Bir Java Geliştirme Seti (zaten yüklü JDK ile) bir sanal makine oluşturma
* Sanal makinenize uzaktan oturum açmak nasıl.
* Hizmet veri yolu ad alanı oluşturma
* Bir işlem yoğunluklu görevi gerçekleştiren bir Java uygulaması oluşturma
* İşlem yoğunluklu görevin ilerlemesini izleyen bir Java uygulaması oluşturma
* Java uygulamalarını çalıştırmak nasıl.
* Java uygulamalarını durdurmak nasıl.

Bu öğretici gezici satış temsilcisi işlem yoğunluklu görev için kullanır. İşlem yoğunluklu görev çalışan Java uygulamasının bir örnek verilmiştir.

![Gezici satış temsilcisi Çözücü][solver_output]

İşlem yoğunluklu görev izleme Java uygulamasının bir örnek verilmiştir.

![Gezici satış temsilcisi istemci][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Sanal makine oluşturmak için
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Tıklatın **yeni**, tıklatın **işlem**, tıklatın **sanal makine**ve ardından **Galeri'den**.
3. İçinde **sanal makine görüntüsü seçin** iletişim kutusunda **JDK 7 Windows Server 2012**.
   Unutmayın **JDK 6 Windows Server 2012** henüz JDK 7'de çalıştırmak hazır olmayan eski uygulamaları olduğunda kullanılabilir.
4. **İleri**’ye tıklayın.
5. İçinde **sanal makine yapılandırması** iletişim kutusunda:
   1. Sanal makine için bir ad belirtin.
   2. Sanal makine için kullanılacak boyutunu belirtin.
   3. Bir yönetici adı **kullanıcı adı** alan. Bu ad ve sonraki girer parola unutmayın, sanal makineye uzaktan oturum açtığınızda kullanır.
   4. Bir parolayı girin **yeni parola** alan ve yeniden girin içinde **Onayla** alan. Bu yönetici hesabının parolasını belirtir.
   5. **İleri**’ye tıklayın.
6. Sonraki **sanal makine yapılandırması** iletişim kutusunda:
   1. İçin **bulut hizmeti**, varsayılan kullanmak **yeni bir bulut hizmeti oluşturma**.
   2. Değeri **bulut hizmeti DNS adı** cloudapp.net arasında benzersiz olması gerekir. Bu Azure benzersiz olduğunu gösterir şekilde gerekirse, bu değeri değiştirin.
   3. Bir bölgeyi, benzeşim grubunu veya sanal ağ belirtin. Bu öğreticinin amaçları doğrultusunda, bir bölge gibi belirtin **Batı ABD**.
   4. İçin **depolama hesabı**seçin **otomatik olarak oluşturulan depolama hesabı kullan**.
   5. İçin **kullanılabilirlik kümesi**seçin **(hiçbiri)**.
   6. **İleri**’ye tıklayın.
7. Son olarak **sanal makine yapılandırması** iletişim kutusunda:
   1. Varsayılan uç noktası girişleri kabul eder.
   2. **Tamamla**’ya tıklayın.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Sanal makinenize uzaktan oturum açmak için
1. Oturum [Klasik Azure portalı](https://manage.windowsazure.com).
2. Tıklatın **sanal makineleri**.
3. Oturum açmak istediğiniz sanal makinenin adına tıklayın.
4. **Bağlan**'a tıklayın.
5. Sanal makineye bağlanmak için gereken şekilde yanıtlamasına. Yönetici adı ve parola istendiğinde, sanal makineyi oluşturduğunuzda belirttiğiniz değerleri kullanın.

Azure hizmet veri yolu işlevselliğini JRE'ın bir parçası olarak yüklenecek Baltimore CyberTrust kök sertifikasını gerektirir Not **cacerts** depolar. Bu sertifika otomatik olarak bu Öğreticisi tarafından kullanılan Java Çalışma zamanı ortamı (JRE) de dahil edilir. Bu sertifika, JRE sahip değilseniz **cacerts** depolamak için bkz: [Java CA sertifika deposu için bir sertifika ekleme] [ add_ca_cert] onu ekleme hakkında bilgi için (yanı sertifikaları cacerts deponuzda görüntüleme hakkında bilgi).

## <a name="how-to-create-a-service-bus-namespace"></a>Hizmet veri yolu ad alanı oluşturma
Azure'da Service Bus kuyruklarını kullanmaya başlamak için öncelikle bir hizmet ad alanı oluşturmanız gerekir. Hizmet ad alanı, uygulamanızın Service Bus kaynaklarını adreslemek için kapsam bir kapsayıcı sağlar.

Hizmet ad alanı oluşturmak için:

1. Oturum [Klasik Azure portalı](https://manage.windowsazure.com).
2. Klasik Azure portalında sol gezinti bölmesinde tıklayın **Service Bus, erişim denetimi ve önbelleğe alma**.
3. Klasik Azure portalında sol üst bölmesinde tıklatın **Service Bus** düğümünü ve ardından **yeni** düğmesi.  
   ![Hizmet veri yolu düğümü ekran görüntüsü][svc_bus_node]
4. İçinde **yeni bir hizmet Namespace oluşturma** iletişim kutusuna bir **Namespace**ve ardından benzersiz olduğundan emin olmak için **Kullanılabilirliği Denetle** düğmesi.  
   ![Yeni Namespace ekran oluşturma][create_namespace]
5. Ad alanı adı kullanılabilirliğinden emin olduktan sonra ülke veya bölge, ad alanınızın barındırılması ve ardından seçin **oluşturma Namespace** düğmesi.  
   
   Oluşturduğunuz ad alanı sonra Klasik Azure portalında görünür ve bir dakika sürer. Durum olana kadar bekleyin **etkin** sonraki adımla devam etmeden önce.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Varsayılan yönetim kimlik bilgilerini ad alanı için alma
Yeni ad alanında bir kuyruk oluşturma gibi yönetim işlemlerini gerçekleştirmek için ad alanı için yönetim kimlik bilgilerini edinmeniz gerekir.

1. Sol gezinti bölmesinde **Service Bus** kullanılabilir ad alanlarının listesini görüntülemek için düğümü.
   ![Kullanılabilir ad alanlarının ekran görüntüsü][avail_namespaces]
2. Görüntülenen listede az önce oluşturduğunuz ad alanını seçin.
   ![Namespace listesi ekran görüntüsü][namespace_list]
3. Sağ taraftaki **özellikleri** bölmesi, yeni ad alanı özellikleri listeler.
   ![Özellikler bölmesinde ekran görüntüsü][properties_pane]
4. **Varsayılan anahtar** gizlenir. Tıklatın **Görünüm** güvenlik kimlik bilgilerini görüntülemek için düğmeyi.
   ![Varsayılan anahtar ekran görüntüsü][default_key]
5. Not **varsayılan veren** ve **varsayılan anahtar** gibi ad alanıyla işlemleri gerçekleştirmek için bu bilgileri kullanın.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Bir işlem yoğunluklu görevi gerçekleştiren bir Java uygulaması oluşturma
1. (Bu, oluşturduğunuz sanal makine olması gerekmez) geliştirme makinenizdeki karşıdan [Java için Azure SDK](https://azure.microsoft.com/develop/java/).
2. Bu bölümün sonunda örnek kod kullanarak bir Java konsol uygulaması oluşturun. Bu öğreticide, kullanacağız **TSPSolver.java** Java dosya adı olarak. Değiştirme **,\_hizmet\_veri yolu\_ad alanı**, **,\_hizmet\_veri yolu\_sahibi**ve **,\_hizmet\_veri yolu\_anahtar** yer tutucuları, service bus kullanmak için **ad alanı**, **varsayılan veren** ve  **Varsayılan anahtar** değerler, sırasıyla.
3. Kodlama sonra runnable Java arşiv (JAR) uygulamaya dışa aktarmak ve gerekli kitaplıkları oluşturulan JAR paket. Bu öğreticide, kullanacağız **TSPSolver.jar** oluşturulan JAR adı olarak.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>İşlem yoğunluklu görevin ilerlemesini izleyen bir Java uygulaması oluşturma
1. Geliştirme makinenizde, bu bölümün sonunda örnek kodu kullanarak bir Java konsol uygulaması oluşturun. Bu öğreticide, kullanacağız **TSPClient.java** Java dosya adı olarak. Daha önce gösterildiği gibi değişiklik **,\_hizmet\_veri yolu\_ad alanı**, **,\_hizmet\_veri yolu\_sahibi**, ve **,\_hizmet\_veri yolu\_anahtar** yer tutucuları, service bus kullanmak için **ad alanı**, **varsayılan veren**ve **varsayılan anahtar** değerler, sırasıyla.
2. Runnable JAR uygulamaya dışa aktarmak ve gerekli kitaplıkları oluşturulan JAR paket. Bu öğreticide, kullanacağız **TSPClient.jar** oluşturulan JAR adı olarak.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Java uygulamalarını çalıştırma
Önce geçerli en iyi yolu service bus kuyruğuna ekleyeceksiniz seyahat Saleseman sorunu çözmek için kuyruk, ardından oluşturmak için işlem yoğunluklu uygulamayı çalıştırın. İşlem yoğunluklu uygulama çalışmıyor (veya daha sonra), olsa da service bus kuyruğundan sonuçları görüntülemek için istemciyi çalıştırın.

### <a name="to-run-the-compute-intensive-application"></a>İşlem yoğunluklu uygulamayı çalıştırmak için
1. Sanal makinenize oturum açın.
2. Uygulamanızın çalıştırdığı bir klasör oluşturun. Örneğin, **c:\TSP**.
3. Kopya **TSPSolver.jar** için **c:\TSP**,
4. Adlı bir dosya oluşturun **c:\TSP\cities.txt** aşağıdaki içeriğe sahip.
   
        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33
5. Bir komut isteminde dizinleri c:\TSP için değiştirin.
6. PATH ortam değişkeni JRE'ın bin klasörü olduğundan emin olun.
7. Hizmet veri yolu kuyruğu TSP Çözücü permütasyon çalıştırmadan önce oluşturmanız gerekir. Hizmet veri yolu kuyruğu oluşturmak için aşağıdaki komutu çalıştırın.
   
        java -jar TSPSolver.jar createqueue
8. Sıra oluşturulur, TSP Çözücü permütasyon çalıştırabilirsiniz. Örneğin, 8 Şehir Çözücü çalıştırmak için aşağıdaki komutu çalıştırın.
   
        java -jar TSPSolver.jar 8
   
   Bir sayı belirtmezseniz, bu için 10 Şehir çalışır. Çözücü geçerli kısa yollar buldukça, bunları sıraya ekleyeceksiniz.

> [!NOTE]
> Uzun belirttiğiniz sayıda Çözücü çalışacaktır. Örneğin, 14 Şehir birkaç dakika sürebilir ve çalıştırmak için 15 Şehir birkaç saat sürebilir çalışıyor. 16 veya daha fazla şehir artırma çalışma zamanı (sonunda hafta, ay ve yıl) gün içinde neden olabilir. Bu Çözücü tarafından Şehir artar sayı olarak değerlendirilen permütasyon sayısını hızla artması kaynaklanır.
> 
> 

### <a name="how-to-run-the-monitoring-client-application"></a>İzleme istemci uygulaması çalıştırma
1. İstemci uygulaması çalıştırdığı makinenize oturum açın. Bu çalışan aynı makinede olması gerekmez **TSPSolver** olabilir ancak uygulama.
2. Uygulamanızın çalıştırdığı bir klasör oluşturun. Örneğin, **c:\TSP**.
3. Kopya **TSPClient.jar** için **c:\TSP**,
4. PATH ortam değişkeni JRE'ın bin klasörü olduğundan emin olun.
5. Bir komut isteminde dizinleri c:\TSP için değiştirin.
6. Aşağıdaki komutu çalıştırın.
   
        java -jar TSPClient.jar
   
    İsteğe bağlı olarak, bir komut satırı bağımsız değişkeni geçirerek sıra denetimi Between uyku moduna dakika sayısını belirtin. Sıranın denetleme varsayılan uyku süresi 3 dakika için komut satırı bağımsız değişken aktarılırsa, kullanılır **TSPClient**. Örneğin, bir dakika uyku aralığı için farklı bir değer kullanmak istiyorsanız, aşağıdaki komutu çalıştırın.
   
        java -jar TSPClient.jar 1
   
    "Tam" bir kuyruk iletisi görür kadar istemci çalıştırın. İstemci çalıştırmadan Çözücü birden çok tekrarı çalıştırırsanız, istemci tamamen sıranın boşaltmak için birden çok kez çalıştırmak gerekebileceğini unutmayın. Alternatif olarak, sıra silin ve yeniden oluşturun. Sıra silmek için aşağıdaki komutu çalıştırın **TSPSolver** (değil **TSPClient**) komutu.
   
        java -jar TSPSolver.jar deletequeue
   
    Tüm yollar inceleniyor sonlanana kadar Çözücü çalıştırın.

## <a name="how-to-stop-the-java-applications"></a>Java uygulamalarını durdurma
Çözücü hem istemci uygulamaları için basabilirsiniz **Ctrl + C** normal tamamlanmadan önce sona erdirmek istediğiniz istiyorsanız.

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
