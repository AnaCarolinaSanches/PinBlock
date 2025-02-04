package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import java.security.KeyFactory;
import java.security.interfaces.RSAPrivateKey;
import java.security.spec.PKCS8EncodedKeySpec;
import javax.crypto.Cipher;
import java.nio.charset.StandardCharsets;
import java.util.Base64;

public class JA_Descriptografar extends CustomJavaAction<String>
{
    private String PrivateKey;
    private String PublicKey;
    private String EncryptedValue;

    public JA_Descriptografar(IContext context, String PrivateKey, String PublicKey, String EncryptedValue)
    {
        super(context);
        this.PrivateKey = PrivateKey;
        this.PublicKey = PublicKey;
        this.EncryptedValue = EncryptedValue;
    }

    @Override
    public String executeAction() throws Exception
    {
        // Validar se a chave privada corresponde à chave pública
        boolean keysMatch = RSAKeyValidator.doKeysMatch(PrivateKey, PublicKey);
        if (!keysMatch) {
            throw new Exception("As chaves fornecidas não correspondem.");
        }

        // Descriptografar o valor
        String privateKeyPEM = PrivateKey
                .replace("-----BEGIN PRIVATE KEY-----", "")
                .replace("-----END PRIVATE KEY-----", "");
        
        privateKeyPEM = privateKeyPEM.replaceAll("\\s+", ""); // Remover espaços extras
        
        byte[] decodedPrivateKey = Base64.getDecoder().decode(privateKeyPEM);
        PKCS8EncodedKeySpec keySpec = new PKCS8EncodedKeySpec(decodedPrivateKey);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(keySpec);

        // Inicializando o Cipher para a descriptografia RSA
        Cipher cipher = Cipher.getInstance("RSA/ECB/OAEPWithSHA-1AndMGF1Padding");
        cipher.init(Cipher.DECRYPT_MODE, privateKey);

        byte[] encryptedValueBytes = Base64.getDecoder().decode(EncryptedValue);
        byte[] decryptedValue = cipher.doFinal(encryptedValueBytes);

        return new String(decryptedValue, StandardCharsets.UTF_8);
    }

    @Override
    public String toString()
    {
        return "JA_Descriptografar";
    }
}
