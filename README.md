	at Cryptography.Microflow (JavaAction : 'JA_Descriptografar')

Advanced stacktrace:
	at com.mendix.modules.microflowengine.MicroflowUtil.processException(MicroflowUtil.java:83)

Caused by: com.mendix.core.CoreRuntimeException: com.mendix.systemwideinterfaces.MendixRuntimeException: java.lang.IllegalArgumentException: Illegal base64 character 2d
	at com.mendix.basis.actionmanagement.ActionManager.executeSync(ActionManager.scala:110)

Caused by: com.mendix.systemwideinterfaces.MendixRuntimeException: java.lang.IllegalArgumentException: Illegal base64 character 2d
	at com.mendix.util.classloading.Runner$.withContextClassLoader(Runner.scala:23)

Caused by: java.lang.IllegalArgumentException: Illegal base64 character 2d
	at java.base/java.util.Base64$Decoder.decode0(Base64.java:746)
	at java.base/java.util.Base64$Decoder.decode(Base64.java:538)
	at java.base/java.util.Base64$Decoder.decode(Base64.java:561)
