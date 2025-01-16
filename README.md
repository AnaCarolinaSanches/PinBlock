public java.lang.String executeAction() throws Exception {
        // BEGIN USER CODE

        // Codificar o Pinblock
        String iso9564format0 = format0Encode(pin, pan);
        System.out.println("Pinblock Encoded: " + iso9564format0);
        
        // Decodificar o Pinblock
        String pinDecoded = format0Decode(iso9564format0, pan);
        System.out.println("Pin Decoded: " + pinDecoded);

        // Ajustar o tamanho da chave para 16 ou 24 bytes
        if (key.length() > 24) {
            key = key.substring(0, 24); // Truncar para 24 bytes
        } else if (key.length() < 16) {
            key = String.format("%-16s", key).replace(' ', '0'); // Preencher com zeros até 16 bytes
        }

        // Converter a chave para bytes com a codificação UTF-8
        byte[] secretKey = key.getBytes(StandardCharsets.UTF_8);

        // Verificar o tamanho da chave
        if (secretKey.length != 16 && secretKey.length != 24) {
            throw new IllegalArgumentException("A chave para Triple DES deve ter 16 ou 24 bytes.");
        }

        // Criação do SecretKeySpec para Triple DES
        SecretKeySpec secretKeySpec = new SecretKeySpec(secretKey, "DESede");

        // Criação do Cipher para criptografia
        Cipher encryptCipher = Cipher.getInstance("DESede/ECB/PKCS5Padding");
        encryptCipher.init(Cipher.ENCRYPT_MODE, secretKeySpec);

        // Converter o Pinblock para bytes e realizar a criptografia
        byte[] secretMessagesBytes = iso9564format0.getBytes(StandardCharsets.UTF_8);
        byte[] encryptedMessageBytes = encryptCipher.doFinal(secretMessagesBytes);

        // Retornar o texto criptografado em maiúsculas
        return new String(encryptedMessageBytes).toUpperCase();

        // END USER CODE
    }
