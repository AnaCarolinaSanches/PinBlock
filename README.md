package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;

public class JA_GeneratePinblock extends CustomJavaAction<java.lang.String> {
    private java.lang.String key;
    private java.lang.String pin;
    private java.lang.String pan;

    public JA_GeneratePinblock(IContext context, java.lang.String key, java.lang.String pin, java.lang.String pan) {
        super(context);
        this.key = key;
        this.pin = pin;
        this.pan = pan;
    }

    @java.lang.Override
    public java.lang.String executeAction() throws Exception {
        // BEGIN USER CODE
        // Cria o objeto TripleDes e gera o Pinblock
        TripleDes d = new TripleDes(this.key, 4);
        String result = d.encrypt(pan, pin);
        return result;
        // END USER CODE
    }

    /**
     * Returns a string representation of this action
     * @return a string representation of this action
     */
    @java.lang.Override
    public java.lang.String toString() {
        return "JA_GeneratePinblock";
    }

    // BEGIN EXTRA CODE
    public class TripleDes {

        private final String ALGORITHM = "DESede";  // Triple DES
        private final String MODE = "ECB";
        private final String PADDING = "NoPadding";
        private final String TRANSFORMATION = ALGORITHM + "/" + MODE + "/" + PADDING;

        private final byte[] key;
        private int pinLength;

        public TripleDes(String keyHex, int pinLength) throws IllegalArgumentException {
            // Valida o comprimento da chave (64 caracteres hexadecimais == 32 bytes)
            if (keyHex.length() != 64) {
                throw new IllegalArgumentException("A chave deve ter exatamente 64 caracteres hexadecimais.");
            }
            this.key = h2b(keyHex);  // Converte a chave hexadecimal para bytes
            this.pinLength = pinLength;
        }

        public String encrypt(String pan, String pinClear) throws Exception {
            if (pinClear.length() != pinLength) {
                throw new IllegalArgumentException("Tamanho do PIN está incorreto. O PIN deve ter " + pinLength + " dígitos.");
            }

            String pinEncoded = encodePinBlockAsHex(pan, pinClear);
            byte[] cipherText = cipher(pinEncoded, Cipher.ENCRYPT_MODE);
            return b2h(cipherText);
        }

        private byte[] cipher(String data, int mode) throws Exception {
            Cipher cipher = Cipher.getInstance(TRANSFORMATION);
            cipher.init(mode, new SecretKeySpec(key, ALGORITHM));
            return cipher.doFinal(h2b(data));
        }

        private String encodePinBlockAsHex(String pan, String pin) throws Exception {
            pan = pan.substring(pan.length() - 12 - 1, pan.length() - 1);  // Retira o PAN
            String paddingPAN = "0000".concat(pan);  // Preenche o PAN
            String Fs = "FFFFFFFFFFFFFFFF";
            String paddingPIN = "0" + pin.length() + pin + Fs.substring(2 + pin.length(), Fs.length());

            byte[] pinBlock = xorBytes(h2b(paddingPAN), h2b(paddingPIN));
            return b2h(pinBlock);
        }

        private byte[] xorBytes(byte[] a, byte[] b) throws Exception {
            if (a.length != b.length) {
                throw new IllegalArgumentException("Arrays de dados precisam ter o mesmo tamanho para a operação XOR.");
            }
            byte[] result = new byte[a.length];
            for (int i = 0; i < a.length; i++) {
                result[i] = (byte) (a[i] ^ b[i]);
            }
            return result;
        }

        private byte[] h2b(String hex) {
            if ((hex.length() & 0x01) == 0x01)
                throw new IllegalArgumentException("O comprimento da string hexadecimal deve ser par.");
            byte[] bytes = new byte[hex.length() / 2];
            for (int idx = 0; idx < bytes.length; ++idx) {
                int hi = Character.digit(hex.charAt(idx * 2), 16);
                int lo = Character.digit(hex.charAt(idx * 2 + 1), 16);
                if (hi < 0 || lo < 0) {
                    throw new IllegalArgumentException("Caracteres inválidos na string hexadecimal.");
                }
                bytes[idx] = (byte) ((hi << 4) | lo);
            }
            return bytes;
        }

        private String b2h(byte[] bytes) {
            char[] hex = new char[bytes.length * 2];
            for (int idx = 0; idx < bytes.length; ++idx) {
                int hi = (bytes[idx] & 0xF0) >>> 4;
                int lo = (bytes[idx] & 0x0F);
                hex[idx * 2] = (char) (hi < 10 ? '0' + hi : 'A' - 10 + hi);
                hex[idx * 2 + 1] = (char) (lo < 10 ? '0' + lo : 'A' - 10 + lo);
            }
            return new String(hex);
        }
    }
    // END EXTRA CODE
}
