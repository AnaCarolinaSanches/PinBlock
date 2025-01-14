# PinBlockpackage <seu_namespace>.actions;

import org.apache.commons.codec.DecoderException;
import org.apache.commons.codec.binary.Hex;
import org.apache.commons.lang.StringUtils;
import com.mendix.core.Core;
import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.systemwideinterfaces.core.MendixObject;
import com.mendix.core.CoreException;

public class PinblockEncodeDecodeAction {

    /**
     * Codifica o PIN em um Pinblock no formato 0 (ISO 9564)
     * 
     * @param context Contexto do Mendix (IContext)
     * @param pin O PIN a ser codificado
     * @param pan O número da conta (PAN)
     * @return O Pinblock codificado em formato hexadecimal
     */
    public static String format0Encode(IContext context, String pin, String pan) {
        try {
            // Adiciona o comprimento do PIN ao começo do PIN (preenchido com '0' se necessário)
            final String pinLenHead = StringUtils.leftPad(Integer.toString(pin.length()), 2, '0') + pin;
            // Preenche o PIN com 'F' para garantir que tenha 16 caracteres
            final String pinData = StringUtils.rightPad(pinLenHead, 16, 'F');
            final byte[] bPin = Hex.decodeHex(pinData.toCharArray());

            // Extrai os últimos 12 ou 13 dígitos do PAN (dependendo do tamanho)
            final String panPart = extractPanAccountNumberPart(pan);
            final String panData = StringUtils.leftPad(panPart, 16, '0');
            final byte[] bPan = Hex.decodeHex(panData.toCharArray());

            // Cria o Pinblock com a operação XOR entre o PIN e o PAN
            final byte[] pinblock = new byte[8];
            for (int i = 0; i < 8; i++) {
                pinblock[i] = (byte) (bPin[i] ^ bPan[i]);
            }

            // Retorna o Pinblock em formato hexadecimal (maiúsculo)
            return Hex.encodeHexString(pinblock).toUpperCase();
        } catch (DecoderException e) {
            throw new RuntimeException("Erro ao decodificar o HEX!", e);
        }
    }

    /**
     * Decodifica um Pinblock no formato 0 (ISO 9564) para obter o PIN original.
     * 
     * @param context Contexto do Mendix (IContext)
     * @param pinblock O Pinblock codificado em formato hexadecimal
     * @param pan O número da conta (PAN)
     * @return O PIN original decodificado
     */
    public static String format0decode(IContext context, String pinblock, String pan) {
        try {
            // Extrai os últimos 12 ou 13 dígitos do PAN
            final String panPart = extractPanAccountNumberPart(pan);
            final String panData = StringUtils.leftPad(panPart, 16, '0');
            final byte[] bPan = Hex.decodeHex(panData.toCharArray());

            // Converte o Pinblock de hexadecimal para um array de bytes
            final byte[] bPinBlock = Hex.decodeHex(pinblock.toCharArray());

            // Cria um array de bytes para armazenar o PIN
            final byte[] bPin = new byte[8];
            for (int i = 0; i < 8; i++) {
                bPin[i] = (byte) (bPinBlock[i] ^ bPan[i]);
            }

            // Converte o PIN de volta para o formato hexadecimal
            final String pinData = Hex.encodeHexString(bPin);

            // Extrai o comprimento do PIN e retorna o PIN original
            final int pinLen = Integer.parseInt(pinData.substring(0, 2));
            return pinData.substring(2, 2 + pinLen);
        } catch (NumberFormatException e) {
            throw new RuntimeException("Formato do Pinblock inválido!", e);
        } catch (DecoderException e) {
            throw new RuntimeException("Erro ao decodificar o HEX!", e);
        }
    }

    /**
     * Extrai os 12 últimos dígitos do número da conta (PAN).
     * 
     * @param accountNumber O número da conta (PAN)
     * @return Os 12 últimos dígitos do número da conta
     */
    private static String extractPanAccountNumberPart(String accountNumber) {
        String accountNumberPart = null;
        if (accountNumber.length() > 12)
            accountNumberPart = accountNumber.substring(accountNumber.length() - 13, accountNumber.length() - 1);
        else
            accountNumberPart = accountNumber;
        return accountNumberPart;
    }
}




