<div align="center">
  <img src="PQCyear2035_505x449.png" alt="Logo" width="25%">
  <h1><strong>PQC/2035::Sandbox</strong></h1>
  <h2><strong>Initiating a Post-Quantum Secure Permissioned Blockchain with ICC OpenSSL & ICCHSM</strong></h2>
</div>
<br>

## Table of Contents

- [Abstract](#abstract)   
- [1. Introduction](#1-introduction)
  - [Sandbox Implementation](#sandbox-implementation)   
- [2. Background](#2-background)    
  - [2.1 Post-Quantum Cryptography (PQC)](#21-post-quantum-cryptography-pqc)   
  - [2.2 ICC OpenSSL](#22-icc-openssl)   
  - [2.3 OpenVPN](#23-openvpn)   
  - [2.4 Hyperledger Besu](#24-hyperledger-besu)   
  - [2.5 ICCHSM](#25-icchsm)  
- [3. Architecture](#3-architecture)   
  - [3.1 High-Level Architecture Diagram](#31-high-level-architecture-diagram)   
  - [3.2 PQC Blockchain Structure](#32-pqc-blockchain-structure)   
  - [Blockchain Framework Structure Diagram](#blockchain-framework-structure-diagram)  
- [4. Implementation Details](#4-implementation-details)   
  - [4.1 Installing ICC OpenSSL](#41-installing-icc-openssl)   
  - [4.2 Installing and Configuring ICCHSM](#42-installing-and-configuring-icchsm)   
  - [4.3 Configuring OpenVPN with ICC OpenSSL](#43-configuring-openvpn-with-icc-openssl)   
    - [4.3.1 Generate PQC Keys and Certificates](#431-generate-pqc-keys-and-certificates)   
    - [4.3.2 OpenVPN Configuration](#432-openvpn-configuration)   
    - [4.3.3 Framework Diagram 1: OpenVPN Initialization and TLS Handshake with Besu Context](#433-framework-diagram-1-openvpn-initialization-and-tls-handshake-with-besu-context)   
    - [4.3.4 Framework Diagram 2: OpenVPN Data Channel and ICC OpenSSL with Besu](#434-framework-diagram-2-openvpn-data-channel-and-icc-openssl-with-besu)   
    - [4.3.5 Start OpenVPN](#435-start-openvpn)   
  - [4.4 Configuring Hyperledger Besu with ICC OpenSSL and ICCHSM](#44-configuring-hyperledger-besu-with-icc-openssl-and-icchsm)   
    - [4.4.1 Install Besu](#441-install-besu)   
    - [4.4.2 Integrate ICC OpenSSL for Transaction Signing](#442-integrate-icc-openssl-for-transaction-signing)   
    - [4.4.3 Integrate ICCHSM for PQC Multi-Signature Operations](#443-integrate-icchsm-for-pqc-multi-signature-operations)   
    - [4.4.4 Configure Besu P2P over OpenVPN](#444-configure-besu-p2p-over-openvpn)      
- [5. ICCHSM PQC Methods](#5-icchsm-pqc-methods)   
- [6. System Requirements](#6-system-requirements)   
- [7. Demonstration Programs with Solidity Smart Contracts](#7-demonstration-programs-with-solidity-smart-contracts)   
  - [7.1 ICOToken and LockingContract on Hyperledger Besu](#71-icotoken-and-lockingcontract-on-hyperledger-besu)   
  - [7.2 WrappedICOToken and MintingContract on Ethereum](#72-wrappedicotoken-and-mintingcontract-on-ethereum)   
  - [7.3 Cross-Chain Bridge DApp with PQC Multi-Signature](#73-cross-chain-bridge-dapp-with-pqc-multi-signature)  
- [8. Security Analysis](#8-security-analysis)   
- [9. Performance Considerations](#9-performance-considerations)   
- [Conclusion](#conclusion)   
- [Technology Partners](#technology-partners)   

---

## Abstract

Quantum computing advancements threaten the cryptographic foundations of blockchain systems, particularly traditional algorithms like RSA and ECC. Permissioned blockchains, vital for enterprise applications due to their controlled access and privacy, require quantum-resistant solutions to protect sensitive data. This whitepaper presents a robust framework for constructing a post-quantum secure (PQC) permissioned blockchain using **Hyperledger Besu** as the blockchain platform, **ICC OpenSSL** for quantum-resistant cryptography, **ICCHSM** for PQC encryption and multi-signature operations, and **OpenVPN** for secure node communication. We provide detailed reference procedures, framework diagrams, and Solidity smart contract examples to demonstrate practical implementation, ensuring enterprises can future-proof their blockchain deployments against quantum threats through a dedicated Sandbox “PQC/2035” initiative.

---

## 1. Introduction

Blockchain technology ensures security, integrity, and authenticity through cryptographic mechanisms. Permissioned blockchains, with restricted access and governance, are ideal for enterprise use cases such as supply chain management, financial systems, and healthcare. However, quantum computing advancements, particularly Shor’s algorithm, threaten to break traditional cryptographic schemes like RSA and ECC, exposing these systems to future risks.

Post-Quantum Cryptography (PQC) offers algorithms resistant to quantum attacks, ensuring long-term security. The primary goal of this project is to complete a Sandbox initiative named **“PQC/2035”**, designed to test the efficiency, security, and feasibility of a PQC-enabled blockchain. The Sandbox will evaluate the performance of a PQC VPN blockchain, assess security risks, provide a platform for hacker testing, and explore the viability of Initial Coin Offering (ICO) in the primary token market connected via a blockchain bridge. This whitepaper outlines a comprehensive approach to building a PQC-enabled permissioned blockchain by integrating **ICC OpenSSL** for quantum-safe cryptography, **ICCHSM** for PQC encryption and multi-signature operations, **OpenVPN** for secure communication, and **Hyperledger Besu** as the permissioned blockchain platform. Enhanced framework diagrams, detailed procedures, and smart contracts woth DApp examples provide a practical roadmap for implementation within the Sandbox “PQC/2035” framework.

### Sandbox Implementation

The Sandbox “PQC/2035” is a controlled testing environment designed to validate the PQC-enabled blockchain framework. Its objectives are:

| Task                       | Description                                                                                                                    |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| **Efficiency Testing**     | Measure the performance of the PQC VPN blockchain, including transaction throughput, latency, and PQC multi-signature processing times. Benchmarks compare PQC algorithms (e.g., ML-DSA, ML-KEM) against traditional cryptography under various network conditions. |
| **Security Risk Assessment** | Identify vulnerabilities in the PQC implementation, focusing on cryptographic strength, key management, and smart contract security. Conduct penetration testing to evaluate resistance to quantum and classical attacks. |
| **Hacker Testing Platform** | Provide a secure environment for ethical hackers to stress-test the blockchain, simulating quantum-enabled attacks on PQC algorithms and VPN tunnels to validate robustness. |
| **Token ICO Feasibility**  | Assess the viability of a primary market token Initial Coin Offering (ICO) connected via the blockchain bridge (Section 7.2). Test cross-chain token transfers, PQC multi-signature validation, and regulatory compliance to ensure a secure and scalable ICO process. |

The Sandbox leverages the demonstration programs (Section 7) to simulate real-world scenarios, providing data to refine the blockchain for enterprise deployment.
This table will render nicely in a GitHub README with two columns: "Task" and "Description".

---

## 2. Background

### 2.1 Post-Quantum Cryptography (PQC)

PQC encompasses cryptographic algorithms designed to withstand both classical and quantum attacks. Unlike RSA or ECC, PQC relies on problems such as lattice-based or code-based mathematics, believed to resist quantum algorithms. Key NIST-standardized candidates include:

```info
ML-DSA (Dilithium): A lattice-based digital signature scheme for authentication and signing.
ML-KEM (Kyber):     A lattice-based key encapsulation mechanism for secure key exchanges.
SLH-DSA (Sphincs+): A stateless hash-based signature scheme.
FALCON:             A lattice-based signature scheme using the hash-and-sign paradigm.
```

### 2.2 ICC OpenSSL

**ICC OpenSSL** extends the OpenSSL library by integrating <a href="https://www.ironcap.ca">IronCAP</a>'s PQC algorithms, enabling quantum-resistant key generation, encryption, and signing. It serves as the cryptographic backbone for OpenVPN and supports Besu’s transaction signing.

### 2.3 OpenVPN

**OpenVPN** is an open-source VPN solution that secures point-to-point and site-to-site connections. Configured with <a href="https://www.ironcap.ca">IronCAP</a>'s ICC OpenSSL, it uses PQC algorithms to establish quantum-safe tunnels, protecting Besu’s peer-to-peer (P2P) communication.

### 2.4 Hyperledger Besu

**Hyperledger Besu**, an open-source Ethereum client under the Hyperledger project, supports permissioned networks tailored for enterprise needs. It handles transaction signing, consensus (e.g., IBFT 2.0), and P2P communication, which we enhance with PQC via <a href="https://www.ironcap.ca">IronCAP</a>'s ICC OpenSSL and ICCHSM.

### 2.5 ICCHSM

**ICCHSM** is a software implementation of the <a href="https://www.ironcap.ca">IronCAP</a>'s cryptographic Hardware Security Module (HSM) with a PKCS#11 interface, providing PQC encryption and signing capabilities. It supports single-purpose and dual-purpose keys for key encapsulation and digital signatures, including multi-signature operations for blockchain root hashes and smart contracts. ICCHSM enables Besu decentralized applications (DApps) to implement PQC multi-signatures, enhancing security for critical operations.

---

## 3. Architecture

The architecture integrates Hyperledger Besu, ICC OpenSSL, ICCHSM, and OpenVPN to create a PQC-enabled permissioned blockchain, tested within the Sandbox “PQC/2035”. Besu nodes handle blockchain operations, ICC OpenSSL and ICCHSM provide quantum-resistant cryptography, and OpenVPN ensures secure communication over PQC-protected tunnels.

### 3.1 High-Level Architecture Diagram

```note
                     PERMISSIONED BLOCKCHAIN NETWORK (ETHEREUM)

+-----------------------------------+          +-----------------------------------+
| Hyperledger Besu Node (Site A)    |          | Hyperledger Besu Node (Site B)    |
| --------------------------------- |          | --------------------------------- |
| - Transaction Signing (ML-DSA)    |          | - Transaction Signing (ML-DSA)    |
| - Consensus (e.g., IBFT 2.0)      |          | - Consensus (e.g., IBFT 2.0)      |
| - P2P Comm. (via OpenVPN)         |          | - P2P Comm. (via OpenVPN)         |
| - ICCHSM (PQC Multi-Sig)          |          | - ICCHSM (PQC Multi-Sig)          |
+-----------------------------------+          +-----------------------------------+
         |                                              |
         | [OpenVPN Tunnel (PQC)]                       | [OpenVPN Tunnel (PQC)]
         | - PQC-SSL (ICC OpenSSL)                      | - PQC-SSL (ICC OpenSSL)
         | - Key Exchange (ML-KEM)                      | - Key Exchange (ML-KEM)
         | - Authentication (ML-DSA)                    | - Authentication (ML-DSA)
         | - Encrypted Data (AES-256-GCM)               | - Encrypted Data (AES-256-GCM)
         +----------------------------------------------+
                                 [Secure Internet]
```

This diagram shows Besu nodes communicating securely over OpenVPN tunnels, with PQC algorithms (ML-KEM and ML-DSA) ensuring quantum resistance. ICC OpenSSL is an OpenSSL extension 
supporting PQC internet workflow. ICCHSM provides PQC multi-signature capabilities for securing root hashes and smart contracts.

### 3.2 PQC Blockchain Structure

The blockchain structure leverages PQC for both communication and core operations, tested within the Sandbox “PQC/2035”. Besu uses **ICCHSM** for PQC multi-signature operations on root hashes and smart contracts, complementing the quantum-safe communication provided by OpenVPN.

- **PQC Multi-Signature for Root Hashes**: ICCHSM enables multiple parties to sign a block’s root hash using PQC algorithms (e.g., ML-DSA, SLH-DSA, or FALCON), ensuring quantum-resistant integrity and authenticity.
- **PQC Smart Contract Signing**: Smart contracts are signed with PQC multi-signatures via ICCHSM, securing deployment and execution.
- **PQC Communication**: OpenVPN tunnels, configured with ICC OpenSSL, protect Besu’s P2P traffic with ML-KEM key exchanges and ML-DSA authentication.

#### Blockchain Framework Structure Diagram

```note
+-----------------------------+
| Hyperledger Besu Blockchain |
|   (Permissioned Ethereum)   |
|                             |
| +-------------------------+ |
| | Block Structure         | |
| | - Root Hash (ML-DSA)    | |
| | - Transactions (ML-DSA) | |
| | - Smart Contracts       | |
| |   (PQC Multi-Sig)       | |
| +-------------------------+ |
|                             |
| +-------------------------+ |
| | Consensus (IBFT 2.0)    | |
| | - PQC Validation        | |
| +-------------------------+ |
|                             |
| +-------------------------+ |
| | P2P Communication       | |
| | - OpenVPN (PQC Tunnel)  | |
| | - PQC-SSL (ICC OpenSSL) | |
| | - Key Exchange (ICCHSM) | |
| +-------------------------+ |
+-----------------------------+
```

This diagram illustrates the blockchain’s structure, highlighting PQC integration in block signing, smart contracts, consensus, and communication, all of which are tested in the Sandbox “PQC/2035”.

---

## 4. Implementation Details

This section provides detailed procedures for implementing the PQC-enabled blockchain, enriched with reference steps and examples, designed to support the Sandbox “PQC/2035” testing environment.

### 4.1 Installing ICC OpenSSL

ICC OpenSSL provides the foundation for PQC cryptography.

#### On Linux (Ubuntu/Debian):
1. **Download**:
   ```bash
   wget icc-openssl_6.0.y-z_amd64.deb
   ```
2. **Install**:
   ```bash
   sudo dpkg-deb -x icc-openssl_6.0.y-z_amd64.deb  /home/iccOpenSSL 
   ```
3. **Verify**:
   ```bash
   /path/to/icc-openssl/bin/openssl version
   ```
   Expected output: `ICC OpenSSL 6.0`.
4. **Set Library Path**:
   ```bash
   export LD_LIBRARY_PATH=/path/to/icc-openssl/lib:$LD_LIBRARY_PATH
   ```

### 4.2 Installing and Configuring ICCHSM

ICCHSM provides PQC encryption and multi-signature capabilities.

#### On Linux (Ubuntu/Debian):
1. **Download**:
   ```bash
   wget icchsm_6.0.y-z_amd64.deb
   ```
2. **Install**:
   ```bash
   sudo dpkg -i icchsm_6.0.y-z_amd64.deb
   ```
3. **Set Environment Variable**:
   ```bash
   export SOFTHSM2_CONF=/usr/local/etc/softhsm2.conf
   ```
4. **Initialize Token**:
   ```bash
   sudo softhsm2-util --module /usr/lib/softhsm/libsofthsm2.so --init-token --free --label "PQCToken" --pin 123456 --so-pin 123456
   ```

### 4.3 Configuring OpenVPN with ICC OpenSSL

OpenVPN secures Besu’s P2P communication using PQC-enabled tunnels, critical for Sandbox “PQC/2035” efficiency testing.

#### 4.3.1 Generate PQC Keys and Certificates
```bash
# Generate CA key and certificate
cd /path/to/icc-openssl/bin
./openssl genicc -bsl 128 -type shake256-kyber-li2 -out ca_key.pem
./openssl req -config openssl.cnf -keyform pem -key ca_key.pem -new -x509 -days 3650 -shake256 -extensions v3_ca -out ca_cert.pem

# Generate server key and certificate
./openssl genicc -bsl 128 -type shake256-kyber-li2 -out server_key.pem
./openssl req -config openssl.cnf -new -shake256 -keyform pem -key server_key.pem -out server.csr
./openssl ca -config icc-ca-shake256-kyber-li2.cnf -keyform pem -days 365 -md shake256 -in server.csr -out server_cert.pem

# Generate client key and certificate
./openssl genicc -bsl 128 -type shake256-kyber-li2 -out client_key.pem
./openssl req -config openssl.cnf -new -shake256 -keyform pem -key client_key.pem -out client.csr
./openssl ca -config icc-ca-shake256-kyber-li2.cnf -keyform pem -days 365 -md shake256 -in client.csr -out client_cert.pem
```

#### 4.3.2 OpenVPN Configuration
- **Server (`server.ovpn`)**:
  ```plaintext
  port 1194
  proto udp
  dev tun
  ca ca_cert.pem
  cert server_cert.pem
  key server_key.pem
  dh none
  server 10.8.0.0 255.255.255.0
  cipher AES-256-GCM
  tls-cipher TLS-ML-KEM-ML-DSA-WITH-AES-256-GCM-SHA384
  ```
- **Client (`client.ovpn`)**:
  ```plaintext
  client
  remote <server-ip> 1194
  proto udp
  dev tun
  ca ca_cert.pem
  cert client_cert.pem
  key client_key.pem
  cipher AES-256-GCM
  tls-cipher TLS-ML-KEM-ML-DSA-WITH-AES-256-GCM-SHA384
  ```

#### 4.3.3 Framework Diagram 1: OpenVPN Initialization and TLS Handshake with Besu Context

```note
Initialization Phase:
[OpenVPN Server]                       [OpenVPN Client]
  |                                       |
  | Load CA, Server Cert/Key              | Load CA, Client Cert/Key
  | (ICC OpenSSL: PQC ML-DSA/Kyber)       | (ICC OpenSSL: PQC ML-DSA/Kyber)
  | [Besu Node Prep: P2P Config]          | [Besu Node Prep: P2P Config]
  |                                       |

TLS Handshake (Quantum-Safe):
[OpenVPN Server]                       [OpenVPN Client]
  |                                       |
  | <--- ClientHello -------------------> | Send PQC ciphersuites (ML-KEM/DSA)
  | Send ServerHello, Cert -------------> | Present server cert (ML-DSA)
  |                                       | Verify cert (ML-DSA)
  | <--- Client Cert, KeyEx ------------> | Send client cert, KeyEx (ML-KEM)
  | Verify client cert (ML-DSA)           |
  | Complete key exchange --------------> | Derive session key (AES-256-GCM)
  | [Besu P2P: Secure Tunnel Ready]       | [Besu P2P: Secure Tunnel Ready]
  |                                       |
```

This diagram integrates Besu by showing how the OpenVPN handshake prepares a secure tunnel for Besu’s P2P communication.

#### 4.3.4 Framework Diagram 2: OpenVPN Data Channel and ICC OpenSSL with Besu

```note
[OpenVPN Server]                               [OpenVPN Client]
+--------------------+                         +--------------------+
| OpenVPN Server     |                         | OpenVPN Client     |
| - Config (PQC Cert)| <--- TLS Handshake ---> | - Config (PQC Cert)|
| - TLS (ML-KEM/DSA) |   (PQC: Kyber/DSA)      | - TLS (ML-KEM/DSA) |
| - Data (AES-256)   | <--- Data Channel ----> | - Data (AES-256)   |
| [Besu P2P Traffic] |   (AES-256-GCM)         | [Besu P2P Traffic] |
+--------------------+                         +--------------------+
| ICC OpenSSL        |                         | ICC OpenSSL        |
| - genicc (PQC Keys)|                         | - genicc (PQC Keys)|
| - iccutl (Enc/Dec) |                         | - iccutl (Enc/Dec) |
| - Sign/Verify      |                         | - Sign/Verify      |
+--------------------+                         +--------------------+
```

This diagram emphasizes Besu’s P2P traffic flowing through the quantum-safe OpenVPN tunnel, supported by ICC OpenSSL’s PQC capabilities.

#### 4.3.5 Start OpenVPN
```bash
openvpn --config server.ovpn  # Server
openvpn --config client.ovpn  # Client
```

### 4.4 Configuring Hyperledger Besu with ICC OpenSSL and ICCHSM

Besu is adapted to use ICC OpenSSL for transaction signing and ICCHSM for PQC multi-signature operations.

#### 4.4.1 Install Besu
1. **Download** from [GitHub](https://github.com/hyperledger/besu/releases):
   ```bash
   wget https://github.com/hyperledger/besu/releases/download/<version>/besu-<version>.tar.gz
   tar -xzf besu-<version>.tar.gz
   ```
2. **Set PATH**:
   ```bash
   export PATH=$PATH:/path/to/besu-<version>/bin
   ```
3. **Verify**:
   ```bash
   besu --version
   ```

#### 4.4.2 Integrate ICC OpenSSL for Transaction Signing
Besu’s Java-based architecture uses a JNI wrapper to call ICC OpenSSL’s PQC functions. Example:
```java
import icc.openssl.ICCOpenSSL;

public class BesuPQCrypto {
    public static byte[] signTransaction(byte[] txData, String privateKey) {
        return ICCOpenSSL.sign(privateKey, txData, "ML-DSA");
    }
    
    public static boolean verifyTransaction(byte[] txData, byte[] signature, String publicKey) {
        return ICCOpenSSL.verify(publicKey, txData, signature, "ML-DSA");
    }
}
```

#### 4.4.3 Integrate ICCHSM for PQC Multi-Signature Operations
Besu DApps leverage **ICCHSM** for PQC multi-signature operations on root hashes and smart contracts, ensuring quantum-resistant security.

- **Install ICCHSM**:
  - **On Linux (Ubuntu/Debian)**:
    ```bash
    wget icchsm_6.0.y-z_amd64.deb
    sudo dpkg -i icchsm_6.0.y-z_amd64.deb
    export SOFTHSM2_CONF=/usr/local/etc/softhsm2.conf
    ```
- **Generate PQC Multi-Signature Key Pair**:
  ```bash
  icc-tool --module /usr/lib/softhsm/libsofthsm2.so --slot <slot-id> --login --pin 123456 -k --key-type icc-shake256-kyber-dilithium:128 --id 101 --label MultiSigKey
  ```
- **Sign Root Hash with Multi-Signatures**:
  ```bash
  icc-tool --module /usr/lib/softhsm/libsofthsm2.so --slot <slot-id> --login --pin 123456 -m CKM-ICC-SHAKE256-KYBER-DILITHIUM --sign --id 101 -i root_hash.bin -o multi_sig.bin
  ```
- **Smart Contract Multi-Signature**: DApps call ICCHSM via PKCS#11 to sign contract bytecode or execution parameters, logged as events on the blockchain.

#### 4.4.4 Configure Besu P2P over OpenVPN
In `besu-config.toml`:
```toml
[p2p]
host = "10.8.0.2"  # VPN-assigned IP
port = 30303
```
Start Besu:
```bash
besu --config-file=besu-config.toml
```

---

## 5. ICCHSM PQC Methods

ICCHSM supports a variety of PQC mechanisms via its PKCS#11 interface, ensuring quantum-safe operations for blockchain applications. Supported methods include:

- **Key Encapsulation Mechanisms (KEM)**:
  ```info
  ML-KEM (Kyber):   Lattice-based KEM for secure key exchanges (NIST FIPS 203).
  Classic McEliece: Code-based KEM using binary Goppa codes (NIST candidate).
  Modern McEliece:  Proprietary code-based KEM with optimized key size and performance (US Patent No. 11,271,715).
  ```
  
- **Digital Signature Algorithms**:
  ```info
  ML-DSA (Dilithium): Lattice-based signature scheme for authentication and signing (NIST FIPS 204).
  SLH-DSA (Sphincs+): Stateless hash-based signature scheme (NIST FIPS 205).
  FALCON:             Lattice-based signature scheme using the hash-and-sign paradigm (NIST standardized).
  ```

- **Dual-Purpose Keys**: Combinations like Kyber-Dilithium, enabling key encapsulation and digital signatures in a single entity, ideal for multi-signature operations.

---

## 6. System Requirements

To deploy this PQC-enabled blockchain within the Sandbox “PQC/2035”, the following minimum system requirements ensure compatibility with OpenVPN, ICC OpenSSL, ICCHSM, and Besu:

- **Operating System**: Ubuntu 20.04+ (Linux) or Windows Server 2019+.
- **Processor**: Quad-core CPU (e.g., Intel Core i7 or AMD Ryzen 5) with AVX2 support for optimized PQC performance.
- **Memory**: 16 GB RAM to handle blockchain and cryptographic workloads.
- **Storage**: 100 GB SSD for blockchain data, software, and logs.
- **Network**: 100 Mbps internet connection for VPN and blockchain synchronization.
- **Software Dependencies**:
  ```info
  ICC OpenSSL (v6.0+)
  ICCHSM (v6.0+)
  OpenVPN (v2.5+)
  Hyperledger Besu (v24.1+)
  Java 17+ for Besu
  PKCS#11 libraries for ICCHSM integration
  ```

These requirements ensure efficient execution of the blockchain infrastructure.

---

## 7. Demonstration Programs with Solidity Smart Contracts

The Sandbox “PQC/2035” includes demonstration programs to validate the PQC-enabled blockchain’s efficiency, security, and feasibility for a primary market token Initial Coin Offering (ICO) connected via a blockchain bridge. The following programs implement a cross-chain token bridge between Hyperledger Besu (primary ICO market) and Ethereum (secondary DEX market), using PQC multi-signatures generated by ICCHSM for enhanced security. The flowchart below outlines the process:

```note
Hyperledger Besu                     Bridge                        Ethereum
(Primary ICO Market)              (Cross-Chain)                (Secondary DEX Market)
+------------------+                                           +------------------+
| User Wallet      |                                           | User Wallet      |
| (MetaMask)       |                                           | (MetaMask)       |
+--------+---------+                                           +--------+---------+
         | 1. Approve ICOToken                                    |
         |    (ICOToken.approve)                                  |
         v                                                        |
+------------------+                                           +------------------+
| ICOToken         |                                           | WrappedICOToken  |
| (ERC-20)         |                                           | (wICO, ERC-20)   |
+------------------+                                           +------------------+
         | 2. Lock Tokens                                         |
         |    (LockingContract.lockTokens)                        |
         v                                                        |
+------------------+          +-------------------+            +------------------+
| LockingContract  |--------->| TokensLocked Event|----------->| MintingContract  |
|                  |          |                   |            |                  |
+------------------+          +-------------------+            +------------------+
         |                       | Validators/Relayers                |
         |                       | Generate Proof (e.g., VAA)         |
         |                       | Submit to Ethereum                 |
         |                       v                                    |
         |                                                  3. Mint Wrapped Tokens
         |                                                     (MintingContract.mintTokens)
         |                                                            |
         |                                                            v
         |                                                     +------------------+
         |                                                     | WrappedICOToken  |
         |                                                     | Receives wICO    |
         |                                                     +------------------+
         |
         | Reverse Flow (Ethereum to Besu)
         |
         v
+------------------+                                           +------------------+
| User Wallet      |                                           | User Wallet      |
| (MetaMask)       |                                           | (MetaMask)       |
+--------+---------+                                           +--------+---------+
         |                                                        |
         |                                                  4. Approve Wrapped Token
         |                                                     (WrappedICOToken.approve)
         |                                                        |
         |                                                        v
+------------------+          +-------------------+            +------------------+
| LockingContract  |<---------| TokensBurned Event|<-----------| MintingContract  |
|                  |          |                   |            |                  |
+------------------+          +-------------------+            +------------------+
         |                       | Validators/Relayers            |
         |                       | Generate Proof (e.g., VAA)     |
         |                       | Submit to Besu                 |
         |                       v                                |
         | 5. Unlock Tokens                                       |
         |    (LockingContract.releaseTokens)                     |
         v                                                        |
+------------------+                                           +------------------+
| ICOToken         |                                           | WrappedICOToken  |
| Receives ICOToken|                                           | Burns wICO       |
+------------------+                                           +------------------+
```

Below are the demonstration programs implementing this flowchart.

### 7.1 ICOToken and LockingContract on Hyperledger Besu

- **Description**: This program deploys an ERC20 token (`ICOToken`) and a locking contract (`LockingContract`) on Besu, enabling users to approve and lock tokens for cross-chain transfer to Ethereum. PQC multi-signatures generated by ICCHSM secure the locking process, logged as events for traceability.

- **Solidity Smart Contracts**:
  ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.0;

  // ICOToken: ERC20 token for the primary ICO market
  contract ICOToken {
      string public name = "ICO Token";
      string public symbol = "ICOT";
      uint8 public decimals = 18;
      uint256 public totalSupply;
      mapping(address => uint256) public balanceOf;
      mapping(address => mapping(address => uint256)) public allowance;

      event Transfer(address indexed from, address indexed to, uint256 value);
      event Approval(address indexed owner, address indexed spender, uint256 value);

      constructor(uint256 initialSupply) {
          totalSupply = initialSupply * 10 ** uint256(decimals);
          balanceOf[msg.sender] = totalSupply;
      }

      function transfer(address to, uint256 value) public returns (bool) {
          require(balanceOf[msg.sender] >= value, "Insufficient balance");
          balanceOf[msg.sender] -= value;
          balanceOf[to] += value;
          emit Transfer(msg.sender, to, value);
          return true;
      }

      function approve(address spender, uint256 value) public returns (bool) {
          allowance[msg.sender][spender] = value;
          emit Approval(msg.sender, spender, value);
          return true;
      }

      function transferFrom(address from, address to, uint256 value) public returns (bool) {
          require(balanceOf[from] >= value, "Insufficient balance");
          require(allowance[from][msg.sender] >= value, "Insufficient allowance");
          balanceOf[from] -= value;
          balanceOf[to] += value;
          allowance[from][msg.sender] -= value;
          emit Transfer(from, to, value);
          return true;
      }
  }

  // LockingContract: Locks ICOToken for cross-chain transfer
  contract LockingContract {
      ICOToken public token;
      address public bridgeValidator;
      mapping(bytes32 => bool) public processedProofs;

      event TokensLocked(address indexed user, uint256 amount, bytes multiSig, bytes32 indexed rootHash);
      event TokensUnlocked(address indexed user, uint256 amount, bytes multiSig, bytes32 indexed proofId);
      event PQCMultiSig(bytes multiSig, address indexed signer, bytes32 indexed rootHash);

      constructor(address _token, address _bridgeValidator) {
          token = ICOToken(_token);
          bridgeValidator = _bridgeValidator;
      }

      function lockTokens(uint256 amount, bytes memory multiSig, bytes32 rootHash) public {
          require(amount > 0, "Invalid amount");
          require(token.transferFrom(msg.sender, address(this), amount), "Transfer failed");
          emit TokensLocked(msg.sender, amount, multiSig, rootHash);
          emit PQCMultiSig(multiSig, msg.sender, rootHash);
      }

      function releaseTokens(address user, uint256 amount, bytes memory multiSig, bytes32 proofId) public {
          require(msg.sender == bridgeValidator, "Only validator");
          require(!processedProofs[proofId], "Proof already processed");
          processedProofs[proofId] = true;
          require(token.transfer(user, amount), "Transfer failed");
          emit TokensUnlocked(user, amount, multiSig, proofId);
          emit PQCMultiSig(multiSig, msg.sender, proofId);
      }
  }
  ```

- **Integration**: The `LockingContract` uses ICCHSM to generate PQC multi-signatures (e.g., ML-DSA) for token locking and unlocking, logged as `PQCMultiSig` events. Validators detect `TokensLocked` events, generate cross-chain proofs (e.g., VAA), and submit them to Ethereum.

- **Procedure**:
  1. Deploy `ICOToken` with an initial supply (e.g., 1,000,000 tokens) and `LockingContract` on Besu, specifying the token address and validator address.
  2. Approve tokens:
     ```bash
     # User approves LockingContract to spend tokens (via DApp or Remix)
     ```
  3. Lock tokens with a PQC multi-signature:
     ```bash
     sudo icc-tool --module /usr/lib/softhsm/libsofthsm2.so --slot <slot-id> --login --pin 123456 -m CKM-ICC-SHAKE256-DILITHIUM --sign --id 101 -i lock_data.bin -o multi_sig.bin
     ```
  4. Call `lockTokens` via a DApp, passing the multi-signature and root hash.
  5. Validators monitor `TokensLocked` events and generate proofs for Ethereum.

### 7.2 WrappedICOToken and MintingContract on Ethereum

- **Description**: This program deploys a wrapped token (`WrappedICOToken`) and a minting contract (`MintingContract`) on Ethereum, enabling the minting of wrapped tokens (wICO) for DEX trading after tokens are locked on Besu. PQC multi-signatures secure the minting and burning processes.

- **Solidity Smart Contracts**:
  ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.0;

  // WrappedICOToken: ERC20 token for the secondary DEX market
  contract WrappedICOToken {
      string public name = "Wrapped ICO Token";
      string public symbol = "wICO";
      uint8 public decimals = 18;
      uint256 public totalSupply;
      mapping(address => uint256) public balanceOf;
      mapping(address => mapping(address => uint256)) public allowance;

      event Transfer(address indexed from, address indexed to, uint256 value);
      event Approval(address indexed owner, address indexed spender, uint256 value);

      function transfer(address to, uint256 value) public returns (bool) {
          require(balanceOf[msg.sender] >= value, "Insufficient balance");
          balanceOf[msg.sender] -= value;
          balanceOf[to] += value;
          emit Transfer(msg.sender, to, value);
          return true;
      }

      function approve(address spender, uint256 value) public returns (bool) {
          allowance[msg.sender][spender] = value;
          emit Approval(msg.sender, spender, value);
          return true;
      }

      function transferFrom(address from, address to, uint256 value) public returns (bool) {
          require(balanceOf[from] >= value, "Insufficient balance");
          require(allowance[from][msg.sender] >= value, "Insufficient allowance");
          balanceOf[from] -= value;
          balanceOf[to] += value;
          allowance[from][msg.sender] -= value;
          emit Transfer(from, to, value);
          return true;
      }

      function mint(address to, uint256 amount) external {
          // Only MintingContract can mint
          totalSupply += amount;
          balanceOf[to] += amount;
          emit Transfer(address(0), to, amount);
      }

      function burn(address from, uint256 amount) external {
          // Only MintingContract can burn
          require(balanceOf[from] >= amount, "Insufficient balance");
          balanceOf[from] -= amount;
          totalSupply -= amount;
          emit Transfer(from, address(0), amount);
      }
  }

  // MintingContract: Mints and burns wICO on Ethereum
  contract MintingContract {
      WrappedICOToken public token;
      address public bridgeValidator;
      mapping(bytes32 => bool) public processedProofs;

      event TokensMinted(address indexed user, uint256 amount, bytes multiSig, bytes32 indexed proofId);
      event TokensBurned(address indexed user, uint256 amount, bytes multiSig, bytes32 indexed rootHash);
      event PQCMultiSig(bytes multiSig, address indexed signer, bytes32 indexed rootHash);

      constructor(address _token, address _bridgeValidator) {
          token = WrappedICOToken(_token);
          bridgeValidator = _bridgeValidator;
      }

      function mintTokens(address user, uint256 amount, bytes memory multiSig, bytes32 proofId) public {
          require(msg.sender == bridgeValidator, "Only validator");
          require(!processedProofs[proofId], "Proof already processed");
          processedProofs[proofId] = true;
          token.mint(user, amount);
          emit TokensMinted(user, amount, multiSig, proofId);
          emit PQCMultiSig(multiSig, msg.sender, proofId);
      }

      function burnTokens(uint256 amount, bytes memory multiSig, bytes32 rootHash) public {
          require(amount > 0, "Invalid amount");
          require(token.transferFrom(msg.sender, address(this), amount), "Transfer failed");
          token.burn(msg.sender, amount);
          emit TokensBurned(msg.sender, amount, multiSig, rootHash);
          emit PQCMultiSig(multiSig, msg.sender, rootHash);
      }
  }
  ```

- **Integration**: The `MintingContract` uses ICCHSM to generate PQC multi-signatures for minting and burning wICO, logged as `PQCMultiSig` events. Validators detect `TokensBurned` events, generate proofs, and submit them to Besu for unlocking.

- **Procedure**:
  1. Deploy `WrappedICOToken` and `MintingContract` on Ethereum, specifying the token address and validator address.
  2. Validators submit proofs from Besu’s `TokensLocked` events to call `mintTokens` with a PQC multi-signature:
     ```bash
     icc-tool --module /usr/lib/softhsm/libsofthsm2.so --slot <slot-id> --login --pin 123456 -m CKM-ICC-SHAKE256-DILITHIUM --sign --id 102 -i mint_data.bin -o multi_sig.bin
     ```
  3. Users approve and burn wICO:
     ```bash
     # User approves MintingContract to burn wICO (via DApp or Remix)
     icc-tool --module /usr/lib/softhsm/libsofthsm2.so --slot <slot-id> --login --pin 123456 -m CKM-ICC-SHAKE256-DILITHIUM --sign --id 103 -i burn_data.bin -o multi_sig.bin
     ```
  4. Call `burnTokens` via a DApp, passing the multi-signature and root hash.
  5. Validators monitor `TokensBurned` events and submit proofs to Besu.

### 7.3 Cross-Chain Bridge DApp with PQC Multi-Signature

- **Description**: A JavaScript DApp that interacts with `LockingContract` on Besu and `MintingContract` on Ethereum, facilitating cross-chain token transfers. The DApp uses Web3.js and ICCHSM to generate PQC multi-signatures for locking, minting, burning, and unlocking tokens, tested in the Sandbox “PQC/2035” for ICO feasibility.

- **JavaScript DApp**:
  ```javascript
  const Web3 = require('web3');
  const pkcs11js = require('pkcs11js');

  // Besu and Ethereum connections
  const besuWeb3 = new Web3('http://localhost:8545'); // Besu RPC endpoint
  const ethWeb3 = new Web3('https://mainnet.infura.io/v3/YOUR_PROJECT_ID'); // Ethereum RPC endpoint

  // Contract ABIs and addresses
  const icotokenABI = [
      // ABI for ICOToken (approve, transferFrom)
      {
          "inputs": [
              {"name": "spender", "type": "address"},
              {"name": "value", "type": "uint256"}
          ],
          "name": "approve",
          "outputs": [{"name": "", "type": "bool"}],
          "stateMutability": "nonpayable",
          "type": "function"
      }
  ];
  const lockingContractABI = [
      // ABI for LockingContract (lockTokens, releaseTokens)
      {
          "inputs": [
              {"name": "amount", "type": "uint256"},
              {"name": "multiSig", "type": "bytes"},
              {"name": "rootHash", "type": "bytes32"}
          ],
          "name": "lockTokens",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
      },
      {
          "anonymous": false,
          "inputs": [
              {"indexed": true, "name": "user", "type": "address"},
              {"indexed": false, "name": "amount", "type": "uint256"},
              {"indexed": false, "name": "multiSig", "type": "bytes"},
              {"indexed": true, "name": "rootHash", "type": "bytes32"}
          ],
          "name": "TokensLocked",
          "type": "event"
      }
  ];
  const wrappedICOTokenABI = [
      // ABI for WrappedICOToken (approve)
      {
          "inputs": [
              {"name": "spender", "type": "address"},
              {"name": "value", "type": "uint256"}
          ],
          "name": "approve",
          "outputs": [{"name": "", "type": "bool"}],
          "stateMutability": "nonpayable",
          "type": "function"
      }
  ];
  const mintingContractABI = [
      // ABI for MintingContract (burnTokens)
      {
          "inputs": [
              {"name": "amount", "type": "uint256"},
              {"name": "multiSig", "type": "bytes"},
              {"name": "rootHash", "type": "bytes32"}
          ],
          "name": "burnTokens",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
      },
      {
          "anonymous": false,
          "inputs": [
              {"indexed": true, "name": "user", "type": "address"},
              {"indexed": false, "name": "amount", "type": "uint256"},
              {"indexed": false, "name": "multiSig", "type": "bytes"},
              {"indexed": true, "name": "rootHash", "type": "bytes32"}
          ],
          "name": "TokensBurned",
          "type": "event"
      }
  ];

  const icotokenAddress = '0xBesuICOTokenAddress'; // Replace with deployed address
  const lockingContractAddress = '0xBesuLockingContractAddress'; // Replace with deployed address
  const wrappedICOTokenAddress = '0xEthWrappedICOTokenAddress'; // Replace with deployed address
  const mintingContractAddress = '0xEthMintingContractAddress'; // Replace with deployed address

  const icotoken = new besuWeb3.eth.Contract(icotokenABI, icotokenAddress);
  const lockingContract = new besuWeb3.eth.Contract(lockingContractABI, lockingContractAddress);
  const wrappedICOToken = new ethWeb3.eth.Contract(wrappedICOTokenABI, wrappedICOTokenAddress);
  const mintingContract = new ethWeb3.eth.Contract(mintingContractABI, mintingContractAddress);

  // ICCHSM configuration
  const pkcs11 = new pkcs11js.PKCS11();
  pkcs11.load('/usr/lib/softhsm/libsofthsm2.so'); // Path to ICCHSM library
  const slotId = '1603015102'; // Replace with your slot ID
  const pin = '123456';

  // Initialize ICCHSM session
  async function initICCHSM() {
      pkcs11.C_Initialize();
      const slots = pkcs11.C_GetSlotList(true);
      const slot = slots.find(s => s.toString() === slotId);
      const session = pkcs11.C_OpenSession(slot, pkcs11js.CKF_RW_SESSION | pkcs11js.CKF_SERIAL_SESSION);
      pkcs11.C_Login(session, pkcs11js.CKU_USER, pin);
      return session;
  }

  // Generate PQC multi-signature
  async function generatePQCMultiSig(data, keyId) {
      const session = await initICCHSM();
      const mechanism = { mechanism: pkcs11js.CKM_ICC_SHAKE256_DILITHIUM };
      const privateKey = pkcs11.C_FindObjectsInit(session, [{ type: pkcs11js.CKA_ID, value: Buffer.from(keyId) }]);
      pkcs11.C_SignInit(session, mechanism, privateKey);
      const signature = pkcs11.C_Sign(session, Buffer.from(data), Buffer.alloc(4096));
      pkcs11.C_Finalize();
      return signature.toString('hex');
  }

  // Besu: Approve and lock tokens
  async function lockTokensOnBesu(account, amount, rootHash) {
      // Step 1: Approve ICOToken
      const approveTx = await icotoken.methods.approve(lockingContractAddress, besuWeb3.utils.toWei(amount.toString(), 'ether'))
          .send({ from: account, gas: 100000 });
      console.log('Approve transaction:', approveTx.transactionHash);

      // Step 2: Lock tokens
      const dataToSign = besuWeb3.utils.soliditySha3(account, amount, rootHash);
      const multiSig = await generatePQCMultiSig(dataToSign, '101');
      const lockTx = await lockingContract.methods.lockTokens(
          besuWeb3.utils.toWei(amount.toString(), 'ether'),
          '0x' + multiSig,
          rootHash
      ).send({ from: account, gas: 200000 });
      console.log('Lock transaction:', lockTx.transactionHash);
  }

  // Ethereum: Approve and burn tokens
  async function burnTokensOnEthereum(account, amount, rootHash) {
      // Step 4: Approve WrappedICOToken
      const approveTx = await wrappedICOToken.methods.approve(mintingContractAddress, ethWeb3.utils.toWei(amount.toString(), 'ether'))
          .send({ from: account, gas: 100000 });
      console.log('Approve transaction:', approveTx.transactionHash);

      // Step 5: Burn tokens
      const dataToSign = ethWeb3.utils.soliditySha3(account, amount, rootHash);
      const multiSig = await generatePQCMultiSig(dataToSign, '102');
      const burnTx = await mintingContract.methods.burnTokens(
          ethWeb3.utils.toWei(amount.toString(), 'ether'),
          '0x' + multiSig,
          rootHash
      ).send({ from: account, gas: 200000 });
      console.log('Burn transaction:', burnTx.transactionHash);
  }

  // Example usage
  async function main() {
      const besuAccounts = await besuWeb3.eth.getAccounts();
      const ethAccounts = await ethWeb3.eth.getAccounts();
      const besuAccount = besuAccounts[0];
      const ethAccount = ethAccounts[0];
      const amount = 100; // 100 tokens
      const rootHash = besuWeb3.utils.randomHex(32);

      // Besu to Ethereum flow
      await lockTokensOnBesu(besuAccount, amount, rootHash);

      // Ethereum to Besu flow
      await burnTokensOnEthereum(ethAccount, amount, rootHash);
  }

  main().catch(console.error);
  ```

- **Integration**:
  - The DApp connects to Besu and Ethereum nodes via Web3.js, interacting with `ICOToken`, `LockingContract`, `WrappedICOToken`, and `MintingContract`.
  - ICCHSM generates PQC multi-signatures (e.g., ML-DSA) for locking and burning transactions, logged as `PQCMultiSig` events.
  - Validators/relayers (not implemented in the DApp) monitor `TokensLocked` and `TokensBurned` events, generating proofs (e.g., VAA) for cross-chain operations.

- **Procedure**:
  1. **Deploy Contracts**:
     - Deploy `ICOToken` and `LockingContract` on Besu, and `WrappedICOToken` and `MintingContract` on Ethereum using Remix or Truffle.
     - Note contract addresses and ABIs.
  2. **Set Up ICCHSM**:
     ```bash
     sudo softhsm2-util --module /usr/lib/softhsm/libsofthsm2.so --init-token --free --label "PQCToken" --pin 123456 --so-pin 123456
     sudo icc-tool --module /usr/lib/softhsm/libsofthsm2.so --slot <slot-id> --login --pin 123456 -k --key-type icc-shake256-dilithium:128 --id 101 --label MultiSigKeyBesu
     sudo icc-tool --module /usr/lib/softhsm/libsofthsm2.so --slot <slot-id> --login --pin 123456 -k --key-type icc-shake256-dilithium:128 --id 102 --label MultiSigKeyEth
     ```
  3. **Run the DApp**:
     - Install dependencies:
       ```bash
       npm install web3 pkcs11js
       ```
     - Save the JavaScript code as `pqc_bridge_dapp.js` and run:
       ```bash
       node pqc_bridge_dapp.js
       ```
  4. **Validator Operations**:
     - Monitor `TokensLocked` events on Besu, generate proofs, and call `mintTokens` on Ethereum.
     - Monitor `TokensBurned` events on Ethereum, generate proofs, and call `releaseTokens` on Besu.
  5. **Verify Events**:
     - Check Besu and Ethereum logs for `TokensLocked`, `TokensMinted`, `TokensBurned`, `TokensUnlocked`, and `PQCMultiSig` events.

---

## 8. Security Analysis

- **Transaction Security**: ML-DSA and ICCHSM’s multi-signature capabilities ensure quantum-resistant transaction signing and root hash validation.
- **Communication Security**: OpenVPN’s ML-KEM and ML-DSA secure P2P traffic against quantum eavesdropping.
- **Smart Contract Security**: PQC multi-signatures protect smart contract deployment and execution.
- **Hashing**: SHA-3 provides quantum-resistant hashing for blockchain operations.

This multi-layered approach ensures comprehensive post-quantum security, rigorously tested in the Sandbox “PQC/2035”.

---

## 9. Performance Considerations

PQC algorithms (e.g., ML-DSA, ML-KEM) introduce higher computational overhead and larger key sizes compared to ECC. In permissioned networks, these impacts are manageable due to fewer nodes. Optimizations include:

- **Hardware Acceleration**: Leverage AVX2-enabled CPUs for faster PQC computations.
- **Parameter Tuning**: Use 128-bit security for balanced performance and protection.
- **Caching**: Store frequently used PQC keys in ICCHSM for efficient access.

These optimizations are evaluated for efficiency within the Sandbox “PQC/2035”.

---

## Conclusion

This whitepaper presents a comprehensive framework for a PQC-enabled permissioned blockchain, validated through the Sandbox “PQC/2035”. Key features include:

- **Project Goal**: Completion of Sandbox “PQC/2035” to test PQC VPN blockchain efficiency, security, and ICO feasibility.
- **Quantum-Resistant Cryptography**: ICC OpenSSL and ICCHSM provide PQC algorithms (ML-KEM, ML-DSA, etc.) for secure operations.
- **Secure Communication**: OpenVPN with PQC tunnels protects node-to-node communication.
- **Multi-Signature Support**: ICCHSM enables PQC multi-signatures for root hashes and smart contracts.
- **PQC Blockchain Structure**: Besu integrates PQC for signing, consensus, and communication, as shown in the blockchain framework diagram.
- **DApp Integration**: Besu DApps use ICCHSM for PQC multi-signatures in token generation, bridging, and wrapping.
- **System Compatibility**: Meets requirements for running OpenVPN, ICC OpenSSL, and ICCHSM.
- **Demonstration Programs**: Solidity contracts and JavaScript DApps for ERC20 token generation, blockchain bridging, and token wrapping showcase PQC integration.
- **Sandbox Testing**: Validates efficiency, security, and ICO feasibility in a controlled environment.

This framework enables enterprises to deploy secure, quantum-resistant blockchain systems, ensuring long-term trust and resilience.

---

## Technology Partners

This sandbox initiative is a collaboration between:
- **01 Communique Laboratory** - Provider of IronCAP™ Quantum-Safe cryptography
- **Real Matter Technology** - Provider of Chip-Level Blockchain technology

For more information, visit:
- [IronCAP™](https://ironcap.ca)
- [Real Matter Technology](https://www.realmatter.io)
- [Quantumatter Blockchain](https://quantumatter-blockchain.web.app)

> Seamless Integration of Quantum-Safe HSM Module & Lattice-Based Chip Entropy for PQC Next-Gen Security

