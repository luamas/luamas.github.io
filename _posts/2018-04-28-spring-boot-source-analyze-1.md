---
title: springboot2.0源码分析（一）
layout: post
date: '2018-04-28 12:00:00'
categories: springboot
tags: springboot
author: luamas
original: true
---

* content
{:toc}

这里就直接进入分析主题了，就不说怎么使用了，首先我们来导入源码到ide中，我们一般创建一个springboot的应用需要一个main方法，
而入口点正是`SpringApplication.run(Application.class, args)`这个方法。下面我们一步步看看SpringApplication中的方法。








下面是静态run方法

```java
// 这里创建一个SpringApplication实例并执行实例的run方法
public static ConfigurableApplicationContext run(Class<?> primarySource,
			String[] args) {
		return new SpringApplication(primarySources).run(args);
	}
```

下面我们来看看构造方法


```java
// resourceLoader初始状态为null，primarySources为初始的时候main类
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
		this.resourceLoader = resourceLoader;
		Assert.notNull(primarySources, "PrimarySources must not be null");
		this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
		// 获取web应用程序类型。NONE：非web程序，SERVLET：servlet类型，REACTIVE：反应式编程（webflux）
		this.webApplicationType = deduceWebApplicationType();
		// 初始化实例变量initializers的变量值（ApplicationContextInitializer会在spring初始化的时候调用所有的initialize方法）
		// getSpringFactoriesInstances方法主要依赖SpringFactoriesLoader的loadFactoryNames静态
		// 方法去读取spring.factories文件内部所有的ApplicationContextInitializer实现类
		setInitializers((Collection) getSpringFactoriesInstances(
				ApplicationContextInitializer.class));
		// 初始化实例变量listeners，同理去spring.factories文件查找
		setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
		// 获取执行main方法的类
		this.mainApplicationClass = deduceMainApplicationClass();
	}
```

我们再来看看run方法

```java
public ConfigurableApplicationContext run(String... args) {
        // 这里采用StopWatch来实现计时器
		StopWatch stopWatch = new StopWatch();
		// 计时开始
		stopWatch.start();
		ConfigurableApplicationContext context = null;
		// 异常报告集合
		Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
		// 配置headless模式的系统环境 默认得到系统设置，如果没有设置则默认开启headless模式
		configureHeadlessProperty();
		// 获取所有的SpringApplicationRunListener类，同样在spring.factories里定义
		SpringApplicationRunListeners listeners = getRunListeners(args);
		// 调用所有SpringApplicationRunListener的starting()方法，当run方法启动时立即调用。用于最早的初始化
		listeners.starting();
		try {
		    // 将命令行参数转换为ApplicationArguments类
			ApplicationArguments applicationArguments = new DefaultApplicationArguments(
					args);
			// 加载环境配置，这里会调用所有SpringApplicationRunListener的environmentPrepared()方法来初始化环境
			ConfigurableEnvironment environment = prepareEnvironment(listeners,
					applicationArguments);
			// 配置忽略bean信息的系统环境，如果没有指定则默认为true
			configureIgnoreBeanInfo(environment);
			// 打印Banner，并返回 Banner对象，这里可以关闭打印，只要让实例变量bannerMode=false就可以了
			Banner printedBanner = printBanner(environment);
			// 根据webApplicationType类型获得具体ApplicationContext的类型。
			context = createApplicationContext();
			// 获取异常报告列表，同样在spring.factories里定义
			exceptionReporters = getSpringFactoriesInstances(
					SpringBootExceptionReporter.class,
					new Class[] { ConfigurableApplicationContext.class }, context);
			// 这里进行上下文的初始化1.设置上下文环境变量2.初始化bean名称生成器，初始化资源加载器
			// 3.执行所有ApplicationContextInitializer的initializer方法
			// 4.调用所有SpringApplicationRunListener的contextPrepared()方法来初始化上下文环境
			// 5.log启动信息
			// ... 一系列上下文的初始化操作
			// 最后调用所有SpringApplicationRunListener的contextLoaded()方法来加载上下文
			prepareContext(context, environment, listeners, applicationArguments,
					printedBanner);
			// 刷新上下文
			refreshContext(context);
			// 空
			afterRefresh(context, applicationArguments);
			// 停止计时
			stopWatch.stop();
			// 启动日志
			if (this.logStartupInfo) {
				new StartupInfoLogger(this.mainApplicationClass)
						.logStarted(getApplicationLog(), stopWatch);
			}
			// 调用所有SpringApplicationRunListener的started()方法来加载上下文，这里并没有真正的启动
			listeners.started(context);
			// 调用所有的ApplicationRunner和CommandLineRunner的run方法
			callRunners(context, applicationArguments);
		}
		catch (Throwable ex) {
		    // 失败抛出异常，这里会调用所有SpringApplicationRunListener的failed()方法
			handleRunFailure(context, ex, exceptionReporters, listeners);
			throw new IllegalStateException(ex);
		}

		try {
		    // 运行
			listeners.running(context);
		}
		catch (Throwable ex) {
		    // 失败抛出异常，不同于上面不会通知listeners
			handleRunFailure(context, ex, exceptionReporters, null);
			throw new IllegalStateException(ex);
		}
		// 最后返回上下文对象
		return context;
	}

```

# 来杯咖啡

AliPay

![Alipay](http://blog.luamas.com/images/aliPay.jpg)

WXPay

![WechatPay](http://blog.luamas.com/images/wechatPay.jpg)


