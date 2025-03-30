

# **Proposed Architecture**

## **A. Datasets Utilized**
We created two datasets with different text corpus sizes to test the robustness of our encryption and decryption algorithm.  
- **Small-scale dataset:** 120 sentences  
- **Large-scale dataset:** 600 sentences  

This dual-dataset approach ensures feasibility testing across varying data volumes. The text is preprocessed to remove null values and ensure appropriate sentence lengths.

## **B. Proposed Algorithm**
Our encryption framework is based on **Kyber**, a lattice-based key encapsulation mechanism (KEM), combined with **AES-CBC** for symmetric encryption.  

### **Process Overview**
1. **Key Generation:**  
   - Kyber generates a public-private key pair.  
   - The receiver uses the **public key (pk)** for encryption and the **secret key (sk)** for decryption.  
   - Key generation time is recorded for performance evaluation.  

2. **Key Encapsulation & Decapsulation:**  
   - Sender wraps a shared secret using the public key:  
     **(ct, ss) ← KEM.EncapSecret(pk)**  
   - The ciphertext (**ct**) is sent to the receiver, who extracts the secret:  
     **ss′ ← KEM.DecapSecret(ct, sk)**  
   - The correctness is verified by checking **ss = ss′**.  
   - Encapsulation and decapsulation times are recorded.  

3. **AES Encryption & Decryption:**  
   - The first 16 bytes of the shared secret serve as the AES key:  
     **K = ss[:16]**  
   - A random **Initialization Vector (IV)** is generated for security.  
   - The plaintext message is padded and encrypted using AES-CBC:  
     **C_AES = AES_CBC_Encrypt(K, IV, M_padded)**  
   - Final ciphertext is constructed as:  
     **C = IV || C_AES**  
   - Decryption reverses the process to recover the original plaintext.  

### **Algorithm 1: Kyber+ Hybrid Encryption and Decryption**  

```
Input: Text Corpus (120 sentences / 600 sentences)  
Output: Encryption and Decryption statistics  
(pk, sk) ← KEM.GenerateKeypair()  
record key generation time  
(ct, ss) ← KEM.EncapSecret(pk)  
send ct to receiver  
ss' ← KEM.DecapSecret(ct, sk)  
record encapsulation/decapsulation time  
K ← ss[:16]  
IV ← Generate random initialization vector  
M_padded ← Pad(M) to match AES block size  
C_AES ← AES_CBC_Encrypt(K, IV, M_padded)  
C ← IV || C_AES  
extract IV from C  
extract C_AES from C  
M_padded' ← AES_CBC_Decrypt(K, IV, C_AES)  
M' ← Remove padding from M_padded'  
```

This **Kyber+ hybrid encryption** ensures quantum-resistant key exchange while leveraging AES for fast and efficient symmetric encryption.

---

## **C. Hybrid Encryption and Decryption Framework**
The **Kyber+ AES hybrid approach** enhances security by integrating Kyber’s quantum resistance for key exchange with AES for efficient encryption.  

### **Key Components:**
1. **Key Generation using Kyber:**  
   - Generates a public-private key pair using `kem.generate_keypair()`.  
   - The public key encapsulates the shared secret, while the private key handles decryption.  
   - Time-based performance evaluations assess computational efficiency.  

2. **Key Encapsulation & Decapsulation:**  
   - The public key is used for key exchange.  
   - A ciphertext is generated: `kem.encap_secret(public_key)`.  
   - The shared secret acts as the AES symmetric key.  
   - The receiver decrypts it using `kem.decap_secret(ciphertext)`.  

3. **AES Symmetric Encryption:**  
   - The first 16 bytes of the shared secret are used as the AES key.  
   - A **random IV** adds an additional layer of security.  
   - The message is padded and encrypted using AES-CBC mode.  

4. **AES Symmetric Decryption:**  
   - The IV is extracted from the ciphertext.  
   - The encrypted message is decrypted using the shared secret.  
   - Padding is removed to recover the original plaintext.  

This results in a **secure, quantum-resistant encryption framework**.

---

## **D. Kyber Variants with Multithreading for Parallel Execution**
To improve **efficiency and quantum attack resistance**, we implement three Kyber variants:  
- **Kyber-512:** Lightweight, moderate security, low computational overhead.  
- **Kyber-768:** Balanced security and efficiency.  
- **Kyber-1024:** High security but increased computational complexity.  

### **Parallel Processing with Multithreading**
We use **multithreading** to execute encryption operations concurrently, improving performance.  
Each Kyber variant runs in a **separate thread**, ensuring all processes complete execution before results are merged.

---

## **E. Metrics and Evaluation**
The security and efficiency of the proposed encryption method are assessed using the following metrics:

1. **Key Generation Time:**  
   - Measures the time taken to generate key pairs across different Kyber variants.  

2. **Encryption & Decryption Time:**  
   - Evaluates the time required to encrypt and decrypt messages.  

3. **Shannon Entropy of Ciphertext:**  
   - Indicates the **randomness** of the ciphertext.  
   - Formula:  
     \[
     H_{CT} = -\sum_{i=1}^{256} P(c_i) \log_2 P(c_i)
     \]
     Where **ci** is the i-th byte of the ciphertext, and **P(ci)** is its probability.  

4. **Shannon Entropy of AES Encrypted Text:**  
   - Measures randomness in AES-encrypted messages.  
   - Formula:  
     \[
     H_{EM} = -\sum_{i=1}^{256} P(e_i) \log_2 P(e_i)
     \]
     Where **ei** is the i-th unique byte in the encrypted message, and **P(ei)** is its probability.  

5. **Hamming Distance:**  
   - Evaluates bitwise differences between ciphertexts.  
   - Formula:  
     \[
     HD(C_1, C_2) = \sum_{i=1}^{n} \text{Weight}(C_1[i] \oplus C_2[i])
     \]
     Where **⊕** is the bitwise XOR operation and **Weight(X)** counts the number of **1s** in X.  

