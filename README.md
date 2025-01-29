com.mendix.webui.WebUIException: Exception while executing runtime operation
	at com.mendix.webui.actions.client.RuntimeOperationAction.$anonfun$apply$1(RuntimeOperationAction.scala:62)

Caused by: com.mendix.modules.microflowengine.MicroflowException: com.mendix.systemwideinterfaces.MendixRuntimeException: java.lang.IllegalArgumentException: Illegal base64 character a
	at Cryptography.Microflow (JavaAction : 'JA_Criptografar')

Advanced stacktrace:
	at com.mendix.modules.microflowengine.MicroflowUtil.processException(MicroflowUtil.java:83)

Caused by: com.mendix.core.CoreRuntimeException: com.mendix.systemwideinterfaces.MendixRuntimeException: java.lang.IllegalArgumentException: Illegal base64 character a
	at com.mendix.basis.actionmanagement.ActionManager.executeSync(ActionManager.scala:110)

Caused by: com.mendix.systemwideinterfaces.MendixRuntimeException: java.lang.IllegalArgumentException: Illegal base64 character a
	at com.mendix.util.classloading.Runner$.withContextClassLoader(Runner.scala:23)
