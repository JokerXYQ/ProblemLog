# bug问题记录

## 1. 增加xwork.xml后启动报错，报错信息大概意思是是没有/bzfy 的action和namespace
*问题原因*：action类所在包为bzfy,在xwork中配置的package name 和 namespace 均为 /bzfygl.  
修改为/bzfy 后成功启动
*************
## 2. 增加实体类后启动报错
*问题原因*：在po中新建的类名为Tpefyxx，而在mappings下新建的TpeBzfy.hbm.xml中class name 和 entity-name配置成了TpeFyxx ,table里少写了一个字母.  
规范po的类名为TpeFyxx,修正xml中的table后，问题解决。
******
		
## 3. 查询报错
```
java.lang.RuntimeException: org.hibernate.hql.ast.QuerySyntaxException: TPE_FYXX_XYQ is not mapped [ from TPE_FYXX_XYQ where yxbz = ? and fwzl like ? order by cjsj asc]
at com.eway.framework.basecomponent.dao.hibernate.BaseHibernateWithPageDAO.pagedQuery(Unknown Source)
at com.eway.framework.basecomponent.dao.hibernate.BaseHibernateWithPageDAO.pagedQuery(Unknown Source)
at com.eway.framework.basecomponent.services.BaseService.pagedQuery(Unknown Source)
at com.hzfc.soar.pgjg.bzfy.service.BzfyglServiceImpl.findBzfyByParams(BzfyglServiceImpl.java:54)
at sun.reflect.GeneratedMethodAccessor581.invoke(Unknown Source)
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:45005)
at java.lang.reflect.Method.invoke(Method.java:597)
at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:317)
at org.springframework.aop.framework.ReflectiveMethodInvocation.invokeJoinpoint(ReflectiveMethodInvocation.java:183)
at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:150)
at org.springframework.transaction.interceptor.TransactionInterceptor$1.proceedWithInvocation(TransactionInterceptor.java:96)
at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:260)
at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:94)
at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:172)
at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:204)
at com.sun.proxy.$Proxy133.findBzfyByParams(Unknown Source)
at com.hzfc.soar.pgjg.bzfy.action.BzfyglAction.findBzfyByParams(BzfyglAction.java:68)
at sun.reflect.GeneratedMethodAccessor579.invoke(Unknown Source)
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:45005)
at java.lang.reflect.Method.invoke(Method.java:597)
at com.opensymphony.xwork.DefaultActionInvocation.invokeAction(DefaultActionInvocation.java:358)
at com.opensymphony.xwork.DefaultActionInvocation.invokeActionOnly(DefaultActionInvocation.java:218)
at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:192)
at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
at com.opensymphony.xwork.interceptor.DefaultWorkflowInterceptor.doIntercept(DefaultWorkflowInterceptor.java:175)
```
错误原因：com/hzfc/soar/pgjg/bzfy/service/BzfyglServiceImpl.java中hql语句里 from 后面跟的是应该实体类而不是表名
*******
	
## 4.新增房源信息时，插入数据报错，
```
Error setting value
ognl.OgnlException: target is null for setProperty(null, "id", [Ljava.lang.String;@475908b5)
at ognl.OgnlRuntime.setProperty(OgnlRuntime.java:2219)
at ognl.ASTProperty.setValueBody(ASTProperty.java:127)
at ognl.SimpleNode.evaluateSetValueBody(SimpleNode.java:220)
at ognl.SimpleNode.setValue(SimpleNode.java:301)
at ognl.ASTChain.setValueBody(ASTChain.java:218)
at ognl.SimpleNode.evaluateSetValueBody(SimpleNode.java:220)
at ognl.SimpleNode.setValue(SimpleNode.java:301)
at ognl.Ognl.setValue(Ognl.java:737)
at com.opensymphony.xwork.util.OgnlUtil.setValue(OgnlUtil.java:188)
at com.opensymphony.xwork.util.OgnlValueStack.setValue(OgnlValueStack.java:162)
at com.opensymphony.xwork.util.OgnlValueStack.setValue(OgnlValueStack.java:145)
at com.opensymphony.xwork.interceptor.ParametersInterceptor.setParameters(ParametersInterceptor.java:144)
at com.opensymphony.xwork.interceptor.ParametersInterceptor.before(ParametersInterceptor.java:119)
at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:30)
at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
at com.eway.framework.basecomponent.webwork.interceptor.InvalidSessionIntercepter.intercept(Unknown Source)
at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
at com.eway.framework.basecomponent.webwork.interceptor.ExceptionInterceptor.intercept(Unknown Source)
at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
at com.opensymphony.xwork.DefaultActionProxy.execute(DefaultActionProxy.java:116)
```
	
解决方法：确认在action类里的接收对象get set方法，确定后，清缓存，刷新页面，重启项目素质三连。
*************
	
## 5.在action类里调用实体类服务接口TpeFyxxService报错
`org.springframework.orm.hibernate3.HibernateSystemException: Unknown entity:`
解决结果：映射文件hbm.xml里
`<class name="com.hzfc.soar.pgjg.common.po.TpeFyxx" table="TPE_FYXX_XYQ" schema="PGJG" entity-name="TpeFyxx">`
entity-name 和实体类服务接口实现类里return (TpeFyxx) this.merge("TpeFyxx", tpeFyxx);的第一个参数一定要对应！！！
*****************
	
## 6.在保障资格申请执行到bzzgsqServiceImpl中的保存家庭信息到工作库的方法时，报错，无法保存
```	
	2019-11-01 03:51:22 DEBUG com.opensymphony.xwork.util.XWorkConverter  - Class: com.hzfc.soar.pgjg.bzzgsq.action.BzzgsqAction
2019-11-01 03:51:22 DEBUG com.opensymphony.xwork.validator.ValidationInterceptor  - Validating /bzzgsq/saveSqxx with method saveSqxx.
2019-11-01 03:51:22 DEBUG com.opensymphony.xwork.interceptor.DefaultWorkflowInterceptor  - Invoking validate() on action com.hzfc.soar.pgjg.bzzgsq.action.BzzgsqAction@3c1e6a1d
2019-11-01 03:51:22 DEBUG com.opensymphony.xwork.interceptor.PrefixMethodInvocationUtil  - cannot find method [validateSaveSqxx] in action [com.hzfc.soar.pgjg.bzzgsq.action.BzzgsqAction@3c1e6a1d]
2019-11-01 03:51:22 DEBUG com.opensymphony.xwork.interceptor.PrefixMethodInvocationUtil  - cannot find method [validateDoSaveSqxx] in action [com.hzfc.soar.pgjg.bzzgsq.action.BzzgsqAction@3c1e6a1d]
2019-11-01 03:51:22 DEBUG com.opensymphony.xwork.DefaultActionInvocation  - Executing action method = saveSqxx
java.lang.NullPointerException
	at com.hzfc.soar.pgjg.bzzgsq.service.BzzgsqServiceImpl.saveJtxxGzk(BzzgsqServiceImpl.java:33)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:317)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.invokeJoinpoint(ReflectiveMethodInvocation.java:183)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:150)
	at org.springframework.transaction.interceptor.TransactionInterceptor$1.proceedWithInvocation(TransactionInterceptor.java:96)
	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:260)
	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:94)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:172)
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:204)
	at com.sun.proxy.$Proxy131.saveJtxxGzk(Unknown Source)
	at com.hzfc.soar.pgjg.bzzgsq.action.BzzgsqAction.saveSqxx(BzzgsqAction.java:43)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at com.opensymphony.xwork.DefaultActionInvocation.invokeAction(DefaultActionInvocation.java:358)
	at com.opensymphony.xwork.DefaultActionInvocation.invokeActionOnly(DefaultActionInvocation.java:218)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:192)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.DefaultWorkflowInterceptor.doIntercept(DefaultWorkflowInterceptor.java:175)
	at com.opensymphony.xwork.interceptor.MethodFilterInterceptor.intercept(MethodFilterInterceptor.java:86)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.validator.ValidationInterceptor.doIntercept(ValidationInterceptor.java:115)
	at com.opensymphony.xwork.interceptor.MethodFilterInterceptor.intercept(MethodFilterInterceptor.java:86)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.webwork.interceptor.FileUploadInterceptor.intercept(FileUploadInterceptor.java:174)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.I18nInterceptor.intercept(I18nInterceptor.java:151)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.webwork.interceptor.TokenInterceptor.handleValidToken(TokenInterceptor.java:161)
	at com.opensymphony.webwork.interceptor.SoarTokenInterceptor.doIntercept(SoarTokenInterceptor.java:33)
	at com.opensymphony.xwork.interceptor.MethodFilterInterceptor.intercept(MethodFilterInterceptor.java:86)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.interceptor.AroundInterceptor.intercept(AroundInterceptor.java:31)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.eway.framework.basecomponent.webwork.interceptor.InvalidSessionIntercepter.intercept(Unknown Source)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.eway.framework.basecomponent.webwork.interceptor.ExceptionInterceptor.intercept(Unknown Source)
	at com.opensymphony.xwork.DefaultActionInvocation.invoke(DefaultActionInvocation.java:190)
	at com.opensymphony.xwork.DefaultActionProxy.execute(DefaultActionProxy.java:116)
	at com.opensymphony.webwork.dispatcher.DispatcherUtils.serviceAction(DispatcherUtils.java:273)
	at com.opensymphony.webwork.dispatcher.FilterDispatcher.doFilter(FilterDispatcher.java:202)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
	at com.eway.framework.extcomponent.ui.formconfig.filter.PagePermissionFilter.doFilter(PagePermissionFilter.java:82)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
	at com.eway.framework.extcomponent.roleresolver.filter.RoleResolverFilter.doFilter(RoleResolverFilter.java:95)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
	at com.opensymphony.webwork.dispatcher.ActionContextCleanUp.doFilter(ActionContextCleanUp.java:88)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
	at com.eway.framework.extcomponent.security.filter.FunctionSecurityFilter.doFilter(FunctionSecurityFilter.java:57)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
	at com.eway.framework.basecomponent.web.filter.EncodingFilter.doFilter(Unknown Source)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
	at org.springframework.orm.hibernate3.support.OpenSessionInViewFilter.doFilterInternal(OpenSessionInViewFilter.java:233)
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
	at org.acegisecurity.util.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:265)
	at org.acegisecurity.intercept.web.FilterSecurityInterceptor.invoke(FilterSecurityInterceptor.java:107)
	at org.acegisecurity.intercept.web.FilterSecurityInterceptor.doFilter(FilterSecurityInterceptor.java:72)
	at org.acegisecurity.util.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:275)
	at org.acegisecurity.ui.ExceptionTranslationFilter.doFilter(ExceptionTranslationFilter.java:124)
	at org.acegisecurity.util.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:275)
	at org.acegisecurity.wrapper.SecurityContextHolderAwareRequestFilter.doFilter(SecurityContextHolderAwareRequestFilter.java:81)
	at org.acegisecurity.util.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:275)
	at org.acegisecurity.ui.basicauth.BasicProcessingFilter.doFilter(BasicProcessingFilter.java:174)
	at org.acegisecurity.util.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:275)
	at org.acegisecurity.ui.AbstractProcessingFilter.doFilter(AbstractProcessingFilter.java:271)
	at org.acegisecurity.util.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:275)
	at org.acegisecurity.ui.logout.LogoutFilter.doFilter(LogoutFilter.java:110)
	at org.acegisecurity.util.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:275)
	at org.acegisecurity.context.HttpSessionContextIntegrationFilter.doFilter(HttpSessionContextIntegrationFilter.java:249)
	at org.acegisecurity.util.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:275)
	at org.acegisecurity.util.FilterChainProxy.doFilter(FilterChainProxy.java:149)
	at org.acegisecurity.util.FilterToBeanProxy.doFilter(FilterToBeanProxy.java:98)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:233)
	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:191)
	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:127)
	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:103)
	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:109)
	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:293)
	at org.apache.coyote.http11.Http11AprProcessor.process(Http11AprProcessor.java:879)
	at org.apache.coyote.http11.Http11AprProtocol$Http11ConnectionHandler.process(Http11AprProtocol.java:617)
	at org.apache.tomcat.util.net.AprEndpoint$Worker.run(AprEndpoint.java:1774)
	at java.lang.Thread.run(Thread.java:662)
2019-11-01 03:51:27 WARN  com.opensymphony.xwork.DefaultActionInvocation  - No result defined for action com.hzfc.soar.pgjg.bzzgsq.action.BzzgsqAction and result null
2019-11-01 03:51:27 DEBUG com.opensymphony.xwork.interceptor.I18nInterceptor  - after Locale=zh_CN
2019-11-01 03:51:27 DEBUG com.opensymphony.xwork.interceptor.I18nInterceptor  - intercept } 
2019-11-01 03:51:27 DEBUG com.opensymphony.xwork.util.InstantiatingNullHandler  - Entering nullPropertyValue [target=[com.hzfc.soar.pgjg.bzzgsq.action.BzzgsqAction@3c1e6a1d, com.opensymphony.xwork.DefaultTextProvider@4cfdbb9f], property=DefaultComponentManager]
2019-11-01 03:51:27 DEBUG org.acegisecurity.ui.ExceptionTranslationFilter  - Chain processed normally
2019-11-01 03:51:27 DEBUG org.acegisecurity.context.HttpSessionContextIntegrationFilter  - SecurityContextHolder now cleared, as request processing completed
```
**原因**：service没有注入;  
**解决方法**：需要在base包下的PgjgBaseServiceImpl 里注入对应的实体类service
**************
	
## 7.修改后项目启动出错
```	
	2019-11-01 04:34:59 ERROR org.springframework.web.context.ContextLoader  - Context initialization failed
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jzgdService' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\dangan\dagl\dajl\Jzgd.bean.xml]: Cannot create inner bean 'com.hzfc.soar.dangan.dagl.jz.service.JzgdServiceImpl#79fb80c9' of type [com.hzfc.soar.dangan.dagl.jz.service.JzgdServiceImpl] while setting bean property 'target'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.hzfc.soar.dangan.dagl.jz.service.JzgdServiceImpl#79fb80c9' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\dangan\dagl\dajl\Jzgd.bean.xml]: Initialization of bean failed; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'tpeJtxxService' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\pgjg\common\TpeJtxx.bean.xml]: Cannot create inner bean 'com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl#7de4e3e4' of type [com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl] while setting bean property 'target'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl#7de4e3e4' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\pgjg\common\TpeJtxx.bean.xml]: Initialization of bean failed; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'tpeJtxxService': org.springframework.beans.factory.FactoryBeanNotInitializedException: FactoryBean is not fully initialized yet
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveInnerBean(BeanDefinitionValueResolver.java:282)
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveValueIfNecessary(BeanDefinitionValueResolver.java:121)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyPropertyValues(AbstractAutowireCapableBeanFactory.java:1393)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1134)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:522)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:461)
	at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:295)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:223)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:292)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:194)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:608)
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:932)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:479)
	at org.springframework.web.context.ContextLoader.configureAndRefreshWebApplicationContext(ContextLoader.java:389)
	at org.springframework.web.context.ContextLoader.initWebApplicationContext(ContextLoader.java:294)
	at org.springframework.web.context.ContextLoaderListener.contextInitialized(ContextLoaderListener.java:112)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4210)
	at org.apache.catalina.core.StandardContext.start(StandardContext.java:4709)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:799)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:779)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:583)
	at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1429)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
```
**原因**：在上一个问题里，service写在了实现类里，注错地方了
	
```	
2019-11-01 05:55:35 ERROR org.springframework.web.context.ContextLoader  - Context initialization failed
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jzgdService' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\dangan\dagl\dajl\Jzgd.bean.xml]: Cannot create inner bean 'com.hzfc.soar.dangan.dagl.jz.service.JzgdServiceImpl#311f56a3' of type [com.hzfc.soar.dangan.dagl.jz.service.JzgdServiceImpl] while setting bean property 'target'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.hzfc.soar.dangan.dagl.jz.service.JzgdServiceImpl#311f56a3' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\dangan\dagl\dajl\Jzgd.bean.xml]: Initialization of bean failed; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'tpeJtxxService' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\pgjg\common\TpeJtxx.bean.xml]: Cannot create inner bean 'com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl#3d882ea9' of type [com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl] while setting bean property 'target'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl#3d882ea9' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\pgjg\common\TpeJtxx.bean.xml]: Initialization of bean failed; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'tpeJtxxService': org.springframework.beans.factory.FactoryBeanNotInitializedException: FactoryBean is not fully initialized yet
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveInnerBean(BeanDefinitionValueResolver.java:282)
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveValueIfNecessary(BeanDefinitionValueResolver.java:121)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyPropertyValues(AbstractAutowireCapableBeanFactory.java:1393)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1134)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:522)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:461)
	at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:295)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:223)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:292)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:194)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:608)
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:932)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:479)
	at org.springframework.web.context.ContextLoader.configureAndRefreshWebApplicationContext(ContextLoader.java:389)
	at org.springframework.web.context.ContextLoader.initWebApplicationContext(ContextLoader.java:294)
	at org.springframework.web.context.ContextLoaderListener.contextInitialized(ContextLoaderListener.java:112)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4210)
	at org.apache.catalina.core.StandardContext.start(StandardContext.java:4709)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:799)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:779)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:583)
	at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1429)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:297)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:836)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:762)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:631)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:568)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:295)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:836)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:762)
	at javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1454)
	at javax.management.remote.rmi.RMIConnectionImpl.access$300(RMIConnectionImpl.java:74)
	at javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1295)
	at javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1387)
	at javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:818)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:303)
	at sun.rmi.transport.Transport$1.run(Transport.java:159)
	at java.security.AccessController.doPrivileged(Native Method)
	at sun.rmi.transport.Transport.serviceCall(Transport.java:155)
	at sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:535)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:790)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:649)
	at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:895)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:918)
	at java.lang.Thread.run(Thread.java:662)
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.hzfc.soar.dangan.dagl.jz.service.JzgdServiceImpl#311f56a3' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\dangan\dagl\dajl\Jzgd.bean.xml]: Initialization of bean failed; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'tpeJtxxService' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\pgjg\common\TpeJtxx.bean.xml]: Cannot create inner bean 'com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl#3d882ea9' of type [com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl] while setting bean property 'target'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl#3d882ea9' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\pgjg\common\TpeJtxx.bean.xml]: Initialization of bean failed; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'tpeJtxxService': org.springframework.beans.factory.FactoryBeanNotInitializedException: FactoryBean is not fully initialized yet
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:532)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:461)
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveInnerBean(BeanDefinitionValueResolver.java:271)
	... 56 more
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'tpeJtxxService' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\pgjg\common\TpeJtxx.bean.xml]: Cannot create inner bean 'com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl#3d882ea9' of type [com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl] while setting bean property 'target'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl#3d882ea9' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\pgjg\common\TpeJtxx.bean.xml]: Initialization of bean failed; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'tpeJtxxService': org.springframework.beans.factory.FactoryBeanNotInitializedException: FactoryBean is not fully initialized yet
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveInnerBean(BeanDefinitionValueResolver.java:282)
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveValueIfNecessary(BeanDefinitionValueResolver.java:121)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyPropertyValues(AbstractAutowireCapableBeanFactory.java:1393)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1134)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:522)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:461)
	at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:295)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:223)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:292)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:194)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireByName(AbstractAutowireCapableBeanFactory.java:1152)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1102)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:522)
	... 58 more
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.hzfc.soar.pgjg.common.service.TpeJtxxServiceImpl#3d882ea9' defined in file [D:\work\demo\WebRoot\WEB-INF\classes\spring\pgjg\common\TpeJtxx.bean.xml]: Initialization of bean failed; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'tpeJtxxService': org.springframework.beans.factory.FactoryBeanNotInitializedException: FactoryBean is not fully initialized yet
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:532)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:461)
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveInnerBean(BeanDefinitionValueResolver.java:271)
	... 70 more
```
**问题原因**：po的实体类对应的service和serviceImpl应该继承SoarBaseService和SoarBaseServiceImpl，继承错了，启动不了
*****************

## 8.页面输入完信息后点击保存，会生成受理号和存入工作库，但是页面无法显示数据
**问题原因**：页面初始化方法viewSqxx()在action类里没有内容，直接返回SUCCESS；
**解决方法**：在viewSqxx的方法里写好表单初始化信息。
************

## 9.项目启动非常慢的原因可能是在debug中加了断点
***********
	
	
## 10.你的主机中的软件中止了一个已建立的连接。
```
	org.apache.catalina.connector.ClientAbortException: java.io.IOException: 你的主机中的软件中止了一个已建立的连接。
	at org.apache.catalina.connector.OutputBuffer.doFlush(OutputBuffer.java:321)
	at org.apache.catalina.connector.OutputBuffer.flush(OutputBuffer.java:284)
	at org.apache.catalina.connector.CoyoteOutputStream.flush(CoyoteOutputStream.java:118)
	at com.fasterxml.jackson.core.json.UTF8JsonGenerator.flush(UTF8JsonGenerator.java:1100)
	at com.fasterxml.jackson.databind.ObjectMapper.writeValue(ObjectMapper.java:2657)
	at org.springframework.web.servlet.view.json.AbstractJackson2View.writeContent(AbstractJackson2View.java:227)
	at org.springframework.web.servlet.view.json.AbstractJackson2View.renderMergedOutputModel(AbstractJackson2View.java:170)
	at org.springframework.web.servlet.view.AbstractView.render(AbstractView.java:314)
	at org.springframework.web.servlet.DispatcherServlet.render(DispatcherServlet.java:1325)
	at org.springframework.web.servlet.DispatcherServlet.processDispatchResult(DispatcherServlet.java:1069)
	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1008)
	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:925)
	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:974)
	at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:877)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:707)
	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:851)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:790)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
	at org.apache.catalina.core.ApplicationDispatcher.invoke(ApplicationDispatcher.java:728)
	at org.apache.catalina.core.ApplicationDispatcher.doInclude(ApplicationDispatcher.java:591)
	at org.apache.catalina.core.ApplicationDispatcher.include(ApplicationDispatcher.java:527)
	at org.apache.catalina.core.StandardHostValve.custom(StandardHostValve.java:389)
	at org.apache.catalina.core.StandardHostValve.status(StandardHostValve.java:254)
	at org.apache.catalina.core.StandardHostValve.throwable(StandardHostValve.java:349)
	at org.apache.catalina.core.AsyncContextImpl.setErrorState(AsyncContextImpl.java:424)
	at org.apache.catalina.connector.CoyoteAdapter.asyncDispatch(CoyoteAdapter.java:176)
	at org.apache.coyote.AbstractProcessor.dispatch(AbstractProcessor.java:236)
	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:53)
	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:800)
	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1471)
	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
	at java.lang.Thread.run(Thread.java:748)
Caused by: java.io.IOException: 你的主机中的软件中止了一个已建立的连接。
	at sun.nio.ch.SocketDispatcher.write0(Native Method)
	at sun.nio.ch.SocketDispatcher.write(SocketDispatcher.java:51)
	at sun.nio.ch.IOUtil.writeFromNativeBuffer(IOUtil.java:93)
	at sun.nio.ch.IOUtil.write(IOUtil.java:65)
	at sun.nio.ch.SocketChannelImpl.write(SocketChannelImpl.java:471)
	at org.apache.tomcat.util.net.NioChannel.write(NioChannel.java:134)
	at org.apache.tomcat.util.net.NioBlockingSelector.write(NioBlockingSelector.java:101)
	at org.apache.tomcat.util.net.NioSelectorPool.write(NioSelectorPool.java:157)
	at org.apache.tomcat.util.net.NioEndpoint$NioSocketWrapper.doWrite(NioEndpoint.java:1279)
	at org.apache.tomcat.util.net.SocketWrapperBase.doWrite(SocketWrapperBase.java:670)
```
**可能错误原因**：  
1. 表单重复提交，表单submit和ajax同时提交了相同的请求  
2. 在tomcat中出现这个错误是由于客户端在发送请求后，还没等服务器响应就断开了连接，有可能是因为网络原因，突然网断了，但是如果错误频繁出现的话，可能就是服务端的问题了。tomcat中配置了一个连接超时时间connectionTimeout，如果在这个时间之后客户端还未得到服务器端的响应的话，就会主动断开连接，这样就会出现上述异常了，tomcat中默认的连接超时时间是20秒，我们一般最好设置为60秒，从而避免后台程序处理时间长导致连接断开。  

**解决办法**：进入tomcat配置文件中server.xml  
`<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443"/>`
************************

## 11.报错：
```
2019-12-03 13:38:44.480 ERROR  org.springframework.boot.SpringApplication Line:845 - Application run failed
org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'nstjcxController': Unsatisfied dependency expressed through field 'nstjcxService'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'nstjcxServiceImpl': Injection of resource dependencies failed; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'nsxxRepository': Invocation of init method failed; nested exception is java.lang.IllegalArgumentException: Failed to create query for method public abstract org.springframework.data.domain.Page com.hzzfxx.jsmart.zfbz.tjcx.nstj.repository.NsxxRepository.findAll(java.util.Map,org.springframework.data.domain.Pageable)! No property findAll found for type NoModel!
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:586)
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:91)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:372)
```	
	
**错误原因**：在repository里的方法上漏掉了 @TemplateQuery  注解
*******
	
## 12.java.util.NoSuchElementException: No value present
在repository中引入param包时，引入错误，正确的是  
`importorg.springframework.data.repository.query.Param;`
*******************
	
## 13.用反射注入类注入不进来，确认注入的接口有没有加@Service接口。
**报错信息**：
```
org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'com.hzzfxx.jsmart.zfbz.swpz.common.report.service.SwpzReportService' available

			at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBean(DefaultListableBeanFactory.java:346)

			at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBean(DefaultListableBeanFactory.java:333)
```
******************
			
## 14.springboot外置tomcat启动时经常会遇到中文乱码的问题，通常是由于以下4种原因：
1. File->settings->Editor->file encoding将字符编码都修改为UTF-8。
2. 在IDEA的安装目录bin文件夹下找到idea64.exe.vmoptions（32位的选择idea.exe.vmoptions）文件,编辑此文件，插入：-Dfile.encoding=UTF-8。
3. IDEA的tomcat启动配置中加入-Dfile.encoding；打开tomcat/conf/logging.properties，找到java.util.logging.ConsoleHandler.encoding = UTF-8，修改为java.util.logging.ConsoleHandler.encoding = UTF-8。
******************

## 12.高层次人才房购房申请页面错误
**详细状况**：在流程页面vue中，各组件无法通过平台配置，在vue中，去除组件的 v-if:"data.showList['']"属性后，可以显示，顶部信息为空白.  
**问题原因**：使用的账号是超级管理员账号，没有配置权限。  
**解决方法**：使用有受理权限的zhs账号，从‘我的工作台’-->‘业务受理’入口进入流程，就会正常显示
***************************************

	