package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import java.security.*;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;
import javax.crypto.Cipher;
import javax.crypto.BadPaddingException;
import javax.crypto.IllegalBlockSizeException;

public class JA_Criptografar extends CustomJavaAction<java.lang.String>
{
    private java.lang.String PublicKey;
    private java.lang.String Value;

    public JA_Criptografar(IContext context, java.lang.String PublicKey, java.lang.String Value)
    {
        super(context);
        this.PublicKey = PublicKey;
        this.Value = Value;
    }

    @java.lang.Override
    public java.lang.String executeAction() throws Exception
    {
        try {
            // Valida e limpa a chave pública Base64
            RSAPublicKey rsaPublicKey = getPublicKeyFromBase64(PublicKey);

            // Valida o tamanho da chave pública (mínimo de 2048 bits)
            validateKeySize(rsaPublicKey);

            // Valida o valor a ser encriptado (não pode ser nulo ou vazio)
            validateInputData(Value);

            // Cria o objeto Cipher para encriptação
            Cipher cipher = Cipher.getInstance("RSA");
            cipher.init(Cipher.ENCRYPT_MODE, rsaPublicKey);

            // Encripta o valor
            byte[] encryptedValue = cipher.doFinal(Value.getBytes());

            // Codifica o valor encriptado em Base64 e retorna
            return Base64.getEncoder().encodeToString(encryptedValue);
        } catch (IllegalArgumentException e) {
            throw new IllegalArgumentException("Erro de validação: " + e.getMessage(), e);
        } catch (BadPaddingException | IllegalBlockSizeException e) {
            throw new Exception("Erro de encriptação: " + e.getMessage(), e);
        } catch (Exception e) {
            throw new Exception("Erro inesperado: " + e.getMessage(), e);
        }
    }

    // Método para validar e limpar a chave pública em Base64
    public static String validateAndCleanBase64PublicKey(String publicKey) throws IllegalArgumentException {
        // Remove cabeçalhos e rodapés do formato PEM
        publicKey = publicKey.replace("-----BEGIN PUBLIC KEY-----", "").replace("-----END PUBLIC KEY-----", "");

        // Remove qualquer espaço em branco extra (incluindo novas linhas) para garantir que seja uma chave Base64 válida
        publicKey = publicKey.replaceAll("\\s+", "");

        // Verifica se a chave tem o comprimento correto
        if (publicKey.length() % 4 != 0) {
            throw new IllegalArgumentException("A chave pública Base64 tem um comprimento inválido.");
        }

        // Verifica se a chave contém apenas caracteres válidos Base64
        if (!publicKey.matches("^[A-Za-z0-9+/=]*$")) {
            throw new IllegalArgumentException("A chave pública Base64 contém caracteres inválidos.");
        }

        // Retorna a chave limpa e validada
        return publicKey;
    }

    // Método para converter a chave pública Base64 para RSAPublicKey
    public static RSAPublicKey getPublicKeyFromBase64(String base64PublicKey) throws Exception {
        // Valida e limpa a chave pública
        base64PublicKey = validateAndCleanBase64PublicKey(base64PublicKey);

        // Decodifica a chave pública Base64
        byte[] decodedKey = Base64.getDecoder().decode(base64PublicKey);

        // Converte o array de bytes para uma chave pública RSA
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        X509EncodedKeySpec keySpec = new X509EncodedKeySpec(decodedKey);
        
        return (RSAPublicKey) keyFactory.generatePublic(keySpec);
    }

    // Método para validar o tamanho da chave pública (mínimo 2048 bits)
    public static void validateKeySize(RSAPublicKey publicKey) throws IllegalArgumentException {
        // Verifica se o tamanho da chave pública é de pelo menos 2048 bits
        if (publicKey.getModulus().bitLength() < 2048) {
            throw new IllegalArgumentException("A chave pública RSA deve ter pelo menos 2048 bits.");
        }
    }

    // Método para validar se o valor a ser encriptado não é nulo ou vazio
    public static void validateInputData(String value) throws IllegalArgumentException {
        if (value == null || value.isEmpty()) {
            throw new IllegalArgumentException("O valor a ser encriptado não pode ser nulo ou vazio.");
        }
    }

    // Retorna uma string representando a ação
    @java.lang.Override
    public java.lang.String toString() {
        return "JA_Criptografar";
    }
}
