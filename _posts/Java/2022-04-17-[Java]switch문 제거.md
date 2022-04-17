---
layout: post
title: "[Java] switch문 제거"
subtitle: "java switch 리팩토링"
categories: study
tags: java
---

> if/else if , switch문 제거

## AS-IS
```java
public void processColor(String color) {
    if("Red".equalsIgnoreCase(color)) {
        processRed();
    } else if ("Yellow".equalsIgnoreCase(color)) {
        processYeollow();
    } else if ("Black".equalsIgnoreCase(color)) {
        processBlack();
    } else {
        // Do nothing
    }
}
```

## TO-BE
```java
public class ColorProcessor {
    private final Map<String, Supplier<String>> colorProcessorMap;

    ColorProcessor() {
        this.colorProcessorMap = new HashMap<>();
        colorProcessorMap.put("red", this::processRed);
        colorProcessorMap.put("yellow", this::processYellow);
        colorProcessorMap.put("black", this::processBlack);
    }

    public String processColor(String color) {
        if (colorProcessorMap.containsKey(color)) {
            return colorProcessorMap.get(color).get();
        } else {
            return "Invalid Color";
        }
    }

    String processRed() {
        return "Red is processed";
    }

    String processYellow() {
        return "Yellow is processed";
    }

    String processBlack() {
        return "Black is processed";
    }
}
```

## 출처
- https://medium.com/javarevisited/remove-the-if-else-hell-java-7927194bd2e