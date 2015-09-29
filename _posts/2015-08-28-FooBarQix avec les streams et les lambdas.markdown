---
layout: post
title:  "FooBarQix avec les streams et les lambdas"
date:   2015-08-28 14:00:00
categories: code
---


{% highlight java %}
import java.util.Collections;
import java.util.HashMap;
import java.util.Map;
import java.util.stream.IntStream;

import static java.lang.Character.getNumericValue;
import static java.util.stream.Collectors.*;

public class FunctionalFooBarQix {

    private final static Map<Integer,String> replacements = functionalFooBarQix();

    public static Map<Integer,String> functionalFooBarQix() {
        Map<Integer, String> map = new HashMap<>();
        map.put(3, "FOO");
        map.put(5, "BAR");
        map.put(7, "QIX");
        return Collections.unmodifiableMap(map);
    }

    public static String fooBarQix(Integer number) {
        String result = "";
        result += replacements.keySet().stream()
                .filter(replaceKey -> number % replaceKey == 0)
                .map(m -> replacements.get(m))
                .collect(joining());

        result += String.valueOf(number).chars()
                .mapToObj(integerAsChar -> replacements.getOrDefault(getNumericValue(integerAsChar), ""))
                .collect(joining());
        return result.isEmpty() ? String.valueOf(number) : result;
    }

    public static void main(String[] args) {
        IntStream.rangeClosed(1,100)
                .mapToObj(i -> fooBarQix(i))
                .forEach(System.out::println);
    }

}
{% endhighlight %}


