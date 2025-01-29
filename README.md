String publicKeyPEM = PublicKey.replace("-----BEGIN PUBLIC KEY-----", "")
                                      .replace("-----END PUBLIC KEY-----", "");

        // Remover espaços e quebras de linha manualmente
        StringBuilder cleanedPublicKey = new StringBuilder();
        for (int i = 0; i < publicKeyPEM.length(); i++) {
            char c = publicKeyPEM.charAt(i);
            if (!Character.isWhitespace(c)) {
                cleanedPublicKey.append(c);
            }
        }
