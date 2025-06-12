# **Securing ICCHSMatter: A PQC-Secured HSM Server for Wallet API Functions**

ICCHSMatter is a **Post-Quantum Cryptography (PQC)**-secured Hardware Security Module (HSM) server designed to provide secure wallet API functions for key access and transaction signing. This article outlines the security architecture of ICCHSMatter, focusing on four key aspects: **dynamic SSL certificate loading**, **session-based PIN encryption**, **login and authentication**, and **additional essential security measures**. The implementation leverages PQC OpenSSL certificates and PQC KEM keys to ensure robust protection against quantum threats.


## **Communication Diagram: ICCHSMatter Server and PQC Wallet**

Below is the modified flow diagram emphasizing the secure communication between the **ICCHSMatter Server** and the **PQC Wallet**. The diagram highlights the security measures implemented during the interaction.

```plaintext
+-----------------------+                           +-----------------------+
|    PQC Wallet         |                           | ICCHSMatter Server    |
+-----------------------+                           +-----------------------+
        |                                                   |
        | 1. Initiate API Request                           |
        |-------------------------------------------------->|
        |                                                   |
        |                                                   |
        | 2. Nginx Validates PQC SSL Certificate            |
        |<--------------------------------------------------|
        |                                                   |
        |                                                   |
        | 3. Establish Session Key (PQC KEM)               |
        |-------------------------------------------------->|
        |                                                   |
        |                                                   |
        | 4. Encrypt PIN Using Session Key                 |
        |-------------------------------------------------->|
        |                                                   |
        |                                                   |
        | 5. Validate PIN                                   |
        |<--------------------------------------------------|
        |                                                   |
        |                                                   |
        | 6. Perform Wallet Operation                      |
        |   (Address Loading or Transaction Signing)        |
        |-------------------------------------------------->|
        |                                                   |
        |                                                   |
        | 7. HSM Executes Key Access or Signing            |
        |<--------------------------------------------------|
        |                                                   |
        |                                                   |
        | 8. Return Response to Wallet                     |
        |-------------------------------------------------->|
        |                                                   |
+-----------------------+                           +-----------------------+
```

---

### **Key Security Features in the Communication**

1. **PQC SSL Certificate Validation**:
   - Nginx dynamically loads a unique PQC SSL certificate for each wallet, ensuring secure HTTPS communication.

2. **Session Key Establishment**:
   - A session key is securely exchanged using PQC Key Encapsulation Mechanism (KEM), protecting the communication from quantum threats.

3. **PIN Encryption**:
   - The wallet encrypts the PIN using the session key before transmitting it to the server, ensuring the PIN is never exposed in plaintext.

4. **PIN Validation**:
   - The server decrypts and validates the PIN securely, ensuring only authorized wallets can access the HSM.

5. **HSM Operations**:
   - The ICCHSMatter server performs cryptographic operations (e.g., key access, transaction signing) securely within the HSM.

6. **Response Transmission**:
   - The server sends the results (e.g., wallet address or transaction signature) back to the wallet over the secure channel.

---

### **Security Emphasis**
- **End-to-End Encryption**: All communication between the wallet and server is encrypted using PQC algorithms.
- **Session-Based Security**: Each session is uniquely secured with a session key, preventing replay attacks.
- **HSM Isolation**: Sensitive cryptographic operations are performed within the HSM, ensuring key material is never exposed.

This communication diagram ensures robust security for wallet-server interactions. Let me know if you need further refinements!

---

## **1. Dynamic SSL Certificate Loading with PQC OpenSSL Certificates**

### **Overview**
To ensure secure communication between the client and the ICCHSMatter server, Nginx is configured to dynamically load PQC OpenSSL certificates. Each client is assigned a unique PQC SSL certificate, ensuring one-to-one secure communication.

### **Flow Diagram**
```plaintext
Client Request --> Nginx --> Dynamic SSL Cert Loading --> ICCHSMatter Server
```

### **Implementation Steps**
1. **Generate PQC SSL Certificates**:
   - Use OpenSSL with PQC algorithms (e.g., Kyber, Dilithium) to generate certificates for each client.
   ```bash
   openssl req -x509 -newkey rsa:2048 -keyout client-key.pem -out client-cert.pem -days 365
   ```

2. **Configure Nginx for Dynamic SSL Loading**:
   - Use the `ssl_certificate_by_lua` module to dynamically load certificates based on the client's domain.
   ```nginx
   server {
       listen 443 ssl;
       server_name _;

       ssl_certificate_by_lua_block {
           local domain = ngx.var.host
           local cert_path = "/certs/" .. domain .. ".pem"
           local key_path = "/certs/" .. domain .. ".key"

           ngx.ssl.set_der_cert(io.open(cert_path, "rb"):read("*a"))
           ngx.ssl.set_der_priv_key(io.open(key_path, "rb"):read("*a"))
       }

       location / {
           proxy_pass http://127.0.0.1:5000;
       }
   }
   ```

3. **Benefits**:
   - Ensures secure communication using PQC algorithms.
   - Scales efficiently for large numbers of clients.

---

## **2. Session-Based PIN Encryption**

### **Overview**
The PIN used to unlock the HSM is encrypted using a session-based mechanism. A unique session key is established between the client and server using PQC KEM key exchange.

### **Flow Diagram**
```plaintext
Client Passkey (PIN) --> Session Key Exchange --> Encrypted PIN --> ICCHSMatter Server
```

### **Implementation Steps**
1. **Session Key Exchange**:
   - Use PQC KEM (e.g., Kyber) for secure key exchange.
   ```python
   from pqcrypto.kem.kyber import generate_keypair, encapsulate, decapsulate

   # Server generates keypair
   public_key, private_key = generate_keypair()

   # Client encapsulates session key
   session_key, ciphertext = encapsulate(public_key)

   # Server decapsulates session key
   session_key = decapsulate(private_key, ciphertext)
   ```

2. **Encrypt PIN**:
   - Use AES encryption with the session key to encrypt the PIN.
   ```python
   from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes

   def encrypt_pin(pin, session_key):
       iv = os.urandom(16)
       cipher = Cipher(algorithms.AES(session_key), modes.CFB(iv))
       encryptor = cipher.encryptor()
       encrypted_pin = encryptor.update(pin.encode()) + encryptor.finalize()
       return iv, encrypted_pin
   ```

3. **Decrypt PIN**:
   - The server decrypts the PIN using the session key.
   ```python
   def decrypt_pin(encrypted_pin, iv, session_key):
       cipher = Cipher(algorithms.AES(session_key), modes.CFB(iv))
       decryptor = cipher.decryptor()
       decrypted_pin = decryptor.update(encrypted_pin) + decryptor.finalize()
       return decrypted_pin.decode()
   ```

4. **Benefits**:
   - Prevents PIN exposure during transmission.
   - Ensures PIN is valid only for the session.

---

## **3. Login and Authentication**

### **Overview**
Clients must authenticate themselves before accessing the HSM server. Authentication is performed using a combination of client certificates and encrypted PIN validation.

### **Flow Diagram**
```plaintext
Client Login --> Nginx SSL Validation --> PIN Validation --> ICCHSMatter Server Access
```

### **Implementation Steps**
1. **Client Certificate Validation**:
   - Nginx validates the client's PQC SSL certificate during the handshake.

2. **PIN Validation**:
   - The server decrypts and validates the PIN against the stored value.
   ```python
   def validate_request(data, required_fields):
       if str(data.get('pin')) != WALLET_VALID_PIN:
           return False, "Invalid PIN"
       return True, "Valid"
   ```

3. **Session Management**:
   - Use Flask's session management to track authenticated sessions.
   ```python
   from flask import session

   session['authenticated'] = True
   session['expiry'] = time.time() + 3600  # Session expires in 1 hour
   ```

4. **Benefits**:
   - Ensures only authenticated clients can access the server.
   - Prevents unauthorized access to sensitive HSM functions.

---

## **4. Additional Essential Security Measures**

### **Overview**
To further enhance security, additional measures are implemented.

### **Flow Diagram**
```plaintext
Secure Communication --> Rate Limiting --> Logging --> Intrusion Detection
```

### **Implementation Steps**
1. **Secure Communication**:
   - Enforce HTTPS for all communication.
   - Use strong cipher suites and disable weak protocols (e.g., TLS 1.0).

2. **Rate Limiting**:
   - Limit the number of API requests per client to prevent brute-force attacks.
   ```nginx
   limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;

   server {
       location /wallet_sign_transaction {
           limit_req zone=api_limit burst=5 nodelay;
           proxy_pass http://127.0.0.1:5000;
       }
   }
   ```

3. **Logging**:
   - Log all access attempts and API calls for auditing.
   ```python
   import logging

   logging.basicConfig(filename='server.log', level=logging.INFO)
   logging.info(f"Client {client_id} accessed wallet_sign_transaction")
   ```

4. **Intrusion Detection**:
   - Monitor logs for suspicious activity and implement alerts for anomalies.

5. **Periodic Key Rotation**:
   - Rotate session keys and PINs periodically to reduce the risk of compromise.

---

## **Conclusion**

The security architecture of ICCHSMatter ensures robust protection for wallet API functions. By combining **dynamic SSL certificate loading**, **session-based PIN encryption**, **login and authentication**, and **additional security measures**, ICCHSMatter provides a secure and scalable solution for HSM-based operations. This approach leverages cutting-edge PQC algorithms to future-proof the system against quantum threats.

Let me know if you need further clarification or implementation details!
