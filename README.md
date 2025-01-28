C:\Dev\Mendix\appmaisv2-feature_APP-445-encriptacao-app-runtime\javasource\cryptography\actions\JA_Criptografar.java:41: error: cannot find symbol
            RSAPublicKey rsaPublicKey = getPublicKeyFromBase64(PublicKey);
                                        ^
  symbol:   method getPublicKeyFromBase64(String)
  location: class JA_Criptografar
C:\Dev\Mendix\appmaisv2-feature_APP-445-encriptacao-app-runtime\javasource\cryptography\actions\JA_Criptografar.java:44: error: cannot find symbol
            validateKeySize(rsaPublicKey);
            ^
  symbol:   method validateKeySize(RSAPublicKey)
  location: class JA_Criptografar
C:\Dev\Mendix\appmaisv2-feature_APP-445-encriptacao-app-runtime\javasource\cryptography\actions\JA_Criptografar.java:47: error: cannot find symbol
            validateInputData(Value);
            ^
  symbol:   method validateInputData(String)
  location: class JA_Criptografar
Note: Some input files use or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
3 errors

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':compile'.
> Compilation failed; see the compiler error output for details.

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --debug option to get more log output.
> Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 30s

