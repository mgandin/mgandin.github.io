---
layout: post
title:  "Yet another reason to switch to Java 8"
date:   2015-08-21 14:34:25
categories: code
tags: featured
---
Beaucoup de site ont modifié leur support SSL pour utiliser TLS 1.2 à la place de SSL v3.
Java 6 ne fonctionne plus avec cette configuration et le code suivant ne marche qu'à partir de la version 7 du JRE :

{% highlight java %}
import java.net.HttpURLConnection;
import java.net.URL;

public class TestHttp {
    public static void main(String[] args) throws Exception {
        URL url = new URL("https://secure-test.be2bill.com/front/service/rest/process.php");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");
        System.out.println("Fetching " + url);
        try {
            int responseCode = conn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Site is up, content length = " + conn.getHeaderField("content-length"));
            } else {
                System.out.println("Site is up, but returns non-ok status = " + responseCode);
            }
        } catch (java.net.UnknownHostException e) {
            e.printStackTrace();
            System.out.println("Site is down");
        }
    }
}
{% endhighlight %}

L'utilisation de la librairie BouncyCastle - qui propose des compléments d'algorithmes de cryptographie - ne permet pas de contourner le problème.
L'ajout de cette ligne de code ne change pas le problème :
{% highlight java %}
Security.addProvider(new BouncyCastleProvider());
{% endhighlight %}

Et la configuration du fichier java.security en ajoutant un provider ne change rien :
{% highlight java %}
security.provider.N=org.bouncycastle.jce.provider.BouncyCastleProvider
{% endhighlight %}

Seule la mise à jour du JRE à la version 1.7, voire la 1.8, permet de contourner ce problème.
