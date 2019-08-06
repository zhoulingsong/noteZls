# 《JAVA 工具类》

## 一、 jdk ##

### 1. Collator : 校对器 ###

	eg: Collator.getInstance(Locale.CHINA) 用于中文排序(常用字可用，生僻字任然有问题);

### 2. Arrays : 数组工具 ###

### 3. Collections : 集合工具 ###

	1. unmodifiableList() : 将列表设置为不可修改；













## 二、 commons-lang-2.1.jar 

### 1. HashCodeBuilder : hash 码生成工具

	org.apache.commons.lang.builder
	eg: new HashCodeBuilder().append("value").toHashCode();