String publicKeyPEM = PublicKey.replace("-----BEGIN PUBLIC KEY-----", "")
                                      .replace("-----END PUBLIC KEY-----", "")
                                      .replaceAll("\\s+", "");  // Remove todos os espaços e quebras de linha
