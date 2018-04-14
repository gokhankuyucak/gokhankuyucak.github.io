---
title: "JAVA 10"
excerpt: "Mart 2018 ile birlikte 23 yıllık Java tarihinde bir ilk yaşandı ve ilk defa yeni bir sürüm 6 ay gibi kısa bir zaman diliminde yayınlandı."
last_modified_at: 2018-04-13T22:45:06-05:00

tags: 
  - Java 10
  - Release
  - Java 9
  - JDK
toc: false
wide: true
---
Oracle, Java 9'un yayımlanmasından sonra  yeni sürümleri [6 aylık döngüler halinde yayımlayacağını açıklamıştı](https://mreinhold.org/blog/forward-faster) ve Mart 2018'de Java 10 yayımlandı. Java 9 ve 10 sürümleri short term support olarak duyurulmuştu, yani kullanıcıların bir sonraki sürüme(Java 11) hemen geçmeleri bekleniyor. Ek olarak Java-9 kullanıcılarının da artık Java-10'a geçmeleri gerekiyor. Eylül 2018'de yayınlanacak Java 11 (18.9) ise long term supporta sahip olacak(3 yıllık). 

Bu arada versiyon isimlendirmesi konusunda da bir değişikliğe gidilecek gibi görünüyor, Oracle 3 farklı versiyonlama şekli önerdi:

A: YYMM.Age(Yıl+Ay+Yaş(ilk releaseden sonra geçen ay sayısı)) formatında, Örnek: 1803, 1803.1

B: YY.M.Age(Yıl+Ay+Yaş(ilk releaseden sonra geçen ay sayısı)) formatında, Örnek: 18.3, 18.3.1

C: Yaş formatında, Örnek: 10, 10.1

|                         |   (A)  |   (B)  |  (C) |
|-------------------------|:------:|:------:|:----:|
| GA (March 2018)         | 1803   | 18.3   | 10   |
| First update (April)    | 1803.1 | 18.3.1 | 10.1 |
| Second update (July)    | 1803.4 | 18.3.4 | 10.4 |
| GA (September 2018)     | 1809   | 18.9   | 11   |
| First update (October)  | 1809.1 | 18.9.1 | 11.1 |
| Second update (January) | 1809.4 | 18.9.4 | 11.4 |

[Oracle](http://www.oracle.com/technetwork/java/eol-135779.html)'ın sitesinde B alternatifi kullanılmaya başlanmış.

6 ay gibi kısa bir sürede yayımlanmasından dolayı Java 10'da çok fazla yeni özellik yok, eklenen özelliklerin listesine [buradan](http://openjdk.java.net/projects/jdk/10/)  ulaşabilirsiniz. Daha önce her yeni sürüme ait  özellikleri anlatan kitaplar yayınlanırdı, bu kadar az yeni özellik için kitap yayınlanacağını zannetmiyorum, muhtemelen mevcut kitapların güncellenmiş sürümleri yayınlanacaktır. Dikkatimi çeken yenilikler ise:

**local variable type inference**

En büyük dikkati "local variable type inference" [JEP-286](http://openjdk.java.net/jeps/286)  çekiyor. Böylece artık java'da tür belirtmeksizin değişken tanımlaması yapılabilecek. var ile tanımlanan bir değişkene atanan ilk değer program derlendiği anda değişkenin veri türünü belirleyecektir. Bu yüzden var ile tanımladığınız değişkeni mutlaka initialize etmelisiniz.


```java
List myList = new ArrayList();
Map<String, String> map = new HashMap<>();
```
yerine 

```java
var myList = new ArrayList();
var map = new HashMap<String, String>();
```
gibi değişken tanımlamaları yapılabilir. 


**Container Awareness**

Bir diğer güzel gelişme ise container tarafında [oldu](http://openjdk.java.net/jeps/8182070), önceden container içerisinde JVM ile çalışan uygulamalarda cpu ve memory tarafında performans problemleri yaşanabiliyordu, her ne kadar containera cpu ve memory limiti koysanız da JVM container içerisinde çalıştığını farketmediğinden bu ayarları yok sayıyordu. Java 10 ile birlikte JVM, container içerisinde çalışıp çalışmadığını anlayabilecek özelliklere sahip oldu, böylece JVM, containera koyulan cpu ve ram limitlerini algılayıp, belirtilen sınırları aşamayacak. 

Örneğin, Java 8 ile containera memory limiti vermeyi deneyelim ve jvm in bu memory limitini kaale alıp almadığına bakalım:

İlk adım jdk-8 containerına 256MB bellek alanı kullanabileceğini söylemek
```bash
 docker container run -it -m256M openjdk:8-jdk
```
Bash açıldıktan sonra JVM in kullanabileceği max alana baktığımızda
```bash
 docker-java-home/bin/java -XX:+PrintFlagsFinal -version | grep MaxHeapSize
```
```output
     uintx MaxHeapSize                              := 520093696                          {product}
openjdk version "1.8.0_162"
OpenJDK Runtime Environment (build 1.8.0_162-8u162-b12-1~deb9u1-b12)
OpenJDK 64-Bit Server VM (build 25.162-b12, mixed mode)
```

Aynı komutu JDK-10 ile denediğimizde
```bash
 docker container run -it -m256M openjdk:10-jdk
```
JDK-10 un bashi otomatik olarak Java 9 ile birlikte gelen jshell e ayarlanmış durumda bu yüzden kullanabileceğimiz max alanı öğrenmek için

```bash
Runtime.getRuntime().maxMemory();
```
komutunu kullanabiliriz.

```output
$1 ==> 127729664
```

Görüldüğü gibi ilk örnekte JDK-8 containerına 256 MB limit koymamıza rağmen JVM max heap size olarak yaklaşık 520 MB kullandığını belirtmişti, ancak JDK-10 containerına aynı ayarla 256 MB limiti koyduğumuzda max heap size olarak yaklaşık  127 MB  kullandığını belirtti.



Özetle, 10 sürümü ile birlikte Java dünyası yeni bir yola girmiş durumda, 6 aylık döngüler ile daha dinamik bir yapıya kavuşuyor. Eylül ayında yayımlanması beklenen Java 11(18.9) da olması beklenen özelliklere [buradan](http://openjdk.java.net/projects/jdk/11/) ulaşabilirsiniz.

